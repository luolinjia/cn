---
layout: post
title: Java Excel 导入到 Oracle
categories:
- Study
- Data
tags:
- Excel
- Java
- Oracle
---

> Excel文件导入到Oracle数据库  

首先这里的情况是针对一个excel文件里面的一个sheet导入到数据库的多张表。  

设计一个配置(ExcelImportCon.java)类，能保证我传入的配置能够起作用：  

{% highlight java %}

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.HashMap;
import java.util.Map;

public class ExcelImportCon {
	
	//要插入的表名（TABLEi ,表名）
	private Map tableMap = new HashMap();
	//要插入的栏位名（COLUMNi ,{栏位名,exl第几列数据 从0开始}）
	private Map columnMap = new HashMap();
	//其他插入的隐藏栏位名（HIDDENi ,数组｛栏位名,类型，值｝）/1 date ,2 String ,3 double,4 int,5 变量，6 SEQ 
	private Map hiddenColMap = new HashMap();
	
	private Map sequence = new HashMap();
/***********************************DBconfig*****************************************/
	private String classString="oracle.jdbc.driver.OracleDriver";
	private String username="ec1016";
	private String password="password";
	private String url="java:oracle:thin:@192.168.1.171:1521:htfdbweb";
	private Connection conn=null;
	private PreparedStatement ps = null;
	private ResultSet rs = null;
/****************************************************************************/
	public Connection getConnection() {
		try {
			Class.forName(classString);
			conn = DriverManager.getConnection(url, username, password);
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		return conn;
	}
	
	public Map getTableMap() {
		return tableMap;
	}
	public Map getColumnMap() {
		return columnMap;
	}
	public Map getHiddenColMap() {
		return hiddenColMap;
	}
	
	//拿到sequence，如果处在同一行，并且有，就拿以前的，如果不是以前的dataLine，就另取值
	public String getSequence(String seqName,int dataLine) {
		return (String)sequence.get((seqName+"-"+dataLine))==null?getSeq(seqName,dataLine):(String)sequence.get((seqName+"-"+dataLine));
	}
	
	public Map getSequenceMap() {
		return sequence;
	}
	
	
	public String getSeq(String seqName){
		conn = getConnection();
		String sql = "";
		String resultSeq = "";
		try {
			sql = "SELECT " + seqName + ".NEXTVAL FROM DUAL";
			ps = conn.prepareStatement(sql);
			rs = ps.executeQuery();
			while(rs.next()){
				resultSeq = rs.getString(1);				
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if(conn != null){
				try {
					conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
			if(ps != null){
				try {
					ps.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
			if(rs != null){
				try {
					rs.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
		}
		return resultSeq;
	}
	
	public String getSeq(String seqName,int dataLine){
		conn = getConnection();
		String sql = "";
		String resultSeq = "";
		try {
			sql = "SELECT " + seqName + ".NEXTVAL FROM DUAL";
			ps = conn.prepareStatement(sql);
			rs = ps.executeQuery();
			while(rs.next()){
				resultSeq = rs.getString(1);
				this.sequence.put((seqName+"-"+dataLine), resultSeq);
				if(this.sequence.get((seqName+"-"+(dataLine-1)))!=null){
					this.sequence.remove((seqName+"-"+(dataLine-1)));
				}
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if(conn != null){
				try {
					conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
			if(ps != null){
				try {
					ps.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
			if(rs != null){
				try {
					rs.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
		}
		return resultSeq;
	}
	
	/**
	 * 拼装表列数
	 * @author Karl.luo at 2013-07-24
	 * @param i
	 * @return
	 */
	public String buildCol(int i){
		StringBuffer sqlBuffer = new StringBuffer();
		sqlBuffer.append(" ");
		String[] columinfo = (String[]) this.getColumnMap().get(("COLUMN"+i));
		for( int w=0, y=columinfo.length;w<y;w++){
			sqlBuffer.append(columinfo[w]);
			if(w<y-1){
			  sqlBuffer.append(",");
			}
		}
		return sqlBuffer.toString();
	}
	
	/**
	 * 拼装表隐藏列
	 * @param i
	 * @return
	 */
	public String buildHiddenCol(int i){
		StringBuffer sqlBuffer = new StringBuffer();
		sqlBuffer.append(" ");
		String[][] hiddenInfo = (String[][])this.getHiddenColMap().get(("HIDDEN"+i));
		if(hiddenInfo != null){
			if(hiddenInfo.length>0){
				sqlBuffer.append(",");
			}
			for( int w=0, y=hiddenInfo.length;w<y;w++){
				sqlBuffer.append(hiddenInfo[0][0]);
				if(w<y-1){
					  sqlBuffer.append(",");
			    }
			}
		}
		sqlBuffer.append(") VALUES ( ");
		return sqlBuffer.toString();
	}
	
}

{% endhighlight %}

具体的拼装，导入类（ImportExcelFile.java）  

{% highlight java %}

import java.io.FileInputStream;
import java.io.IOException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.sql.Timestamp;
import java.util.HashMap;
import java.util.Map;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.apache.poi.hssf.usermodel.HSSFCell;
import org.apache.poi.hssf.usermodel.HSSFRow;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;


/**
 * <p>Title:导入Excel到数据库</p>
 * <p>Description:读取Excel文件  </p>
 * <p>Copyright: Copyright (c) 2013</p>
 * <p>Company:  </p>
 * @author Karl.luo
 * @version 1.0
 * @CreateDate: 2013-07-23
 * @UpdateDate:
 */
public class ImportExcelFile {
    private static final Log log = LogFactory.getLog(ImportExcelFile.class);

    public ImportExcelFile() {
    }

	private String classString="oracle.jdbc.driver.OracleDriver";
	private String username="ec1016";
	private String password="password";
	private String url="java:oracle:thin:@192.168.1.171:1521:htfdbweb";
	private Connection conn=null;
	private PreparedStatement ps = null;

	public Connection getConnection(){
	   try {
	    Class.forName(classString);
	    conn=DriverManager.getConnection(url,username,password);
	   } catch (ClassNotFoundException e) {
	    e.printStackTrace();
	   } catch (SQLException e) {
	    e.printStackTrace();
	   }
	   return conn;
	}
	
	/**
	 * 得到存入数据库的结果
	 * @author Karl.luo at 2013-07-23
	 * @param filePath
	 * @param tableStr
	 * @param fieldStr
	 * @return flag
	 */
	public String getSuccessFlag(ExcelImportCon test, String filePath){
		//打印开始
		log.debug("=======导入文件 "+ filePath +" 开始======");
		System.out.println("=======导入文件 "+ filePath +" 开始======");
		String result = "";
		HSSFSheet sheet = null;
		Map sqlMap = new HashMap();
		
		try {
			//
			
			//拿到拼装SQL Map集合
			sqlMap = buildSql(test);
			if(sqlMap == null){
				log.error("SQL拼装出错，为null");
				System.out.println("SQL拼装出错，为null");
				return "导入出错，配置excel文件错误";
			}
			// 拿到第一个sheet
			sheet = getRowNo(filePath);
			if(sheet == null){
				log.error("Excel文件没有可导入的数据");
				return  "Excel文件没有可导入的数据";
			}
			
			conn = getConnection();
			conn.setAutoCommit(false);
			//把sheet里的值塞进表，并commit
			result = setTableValue(sqlMap, sheet, ps, conn, test);
			
			conn.setAutoCommit(true);
			
			
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if (conn != null) {
				try {
					conn.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
			if (ps != null) {
				try {
					ps.close();
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
		}
		return result;
	}
	
	/**
	 * 返回拼装好的SQL Map
	 * @param test
	 * @param columinfo
	 * @param hiddenInfo
	 * @return
	 */
	public Map buildSql(ExcelImportCon test){
		Map sqlMap = new HashMap();
		StringBuffer sqlBuffer;
		String[] columinfo;
		String[][] hiddenInfo;
		if (test != null && test.getTableMap() != null) {
			// 打印开始拼装SQL
			log.debug("========SQL拼装开始:========");
			System.out.println("========SQL拼装开始:========");
			// 拼装需要执行的sql语句
			for (int i = 1, k = test.getTableMap().size(); i <= k; i++) {
				sqlBuffer = new StringBuffer();
				sqlBuffer.append(" INSERT INTO ");
				sqlBuffer.append(test.getTableMap().get(("TABLE" + i)));
				sqlBuffer.append(" ( ");
				columinfo = (String[]) test.getColumnMap().get(("COLUMN" + i));
				hiddenInfo = (String[][]) test.getHiddenColMap().get(("HIDDEN" + i));
				// 拼装表列
				sqlBuffer.append(test.buildCol(i));
				// 拼装隐藏栏位
				sqlBuffer.append(test.buildHiddenCol(i));
				//处理空存在问题
				int totalLength = 0;
				if(columinfo != null){
					totalLength+= columinfo.length;
				}
				if(hiddenInfo != null){
					totalLength+= hiddenInfo.length;
				}				
				for (int w = 0, y = totalLength; w < y; w++) {
					sqlBuffer.append("?");
					if (w < y - 1) {
						sqlBuffer.append(",");
					}
				}
				sqlBuffer.append(")");
				//打印SQL i : = 
				log.debug("SQL 第"+i+"条: " + sqlBuffer);
				System.out.println("SQL 第"+i+"条: " + sqlBuffer);
				sqlMap.put("SQL" + i, sqlBuffer.toString());
			}
		}
		return sqlMap;
	}
	
	
	/**
	 * 读取文件,拿到数据sheet
	 * @author Karl.luo at 2013-07-25
	 * @param filePath
	 * @return HSSFSheet
	 */
	private HSSFSheet getRowNo(String filePath){
		// 打印开始拿到第一个sheet操作
		FileInputStream fis = null;
		HSSFWorkbook workbook = null;
		HSSFSheet sheet = null;
		try {
			// 传入文件地址
			fis = new FileInputStream(filePath);
			// 根据传入的文件流创建工作薄
			workbook = new HSSFWorkbook(fis);
			// 拿到第一个sheet
			sheet = workbook.getSheetAt(0);
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if(fis != null){
				try {
					fis.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
		return sheet;
	}
	
	/**
	 * 传入sheet表值，并insert表和commit
	 * @author Karl.luo at 2013-07-25
	 * @param totalRow
	 * @param sqlMap
	 * @param sheet
	 * @param ps
	 * @param conn
	 * @param test
	 * @throws SQLException
	 */
	private String setTableValue(Map sqlMap, HSSFSheet sheet,
			PreparedStatement ps, Connection conn, ExcelImportCon test) throws SQLException{
		//开始导入数据
		log.debug("=======开始导入数据========");
		System.out.println("=======开始导入数据========");
		HSSFRow row = null;
		HSSFCell cell = null;
		int totalRow = sheet.getLastRowNum();
		String[] columinfo;
		//定义所需读的excel栏位
		int noline = 0;
		int success = 0;
		int fail = 0;
		log.debug("=======正在导入数据中，请等待。。。========");
		for (int j = 1; j <= totalRow; j++) {			
			try { 
				noline = 0;
				for (int i = 1, k = sqlMap.size(); i <= k; i++) {
					//System.out.println("sql:" + (String) sqlMap.get("SQL" + i));
					ps = conn.prepareStatement((String) sqlMap.get("SQL" + i));
					columinfo = (String[]) test.getColumnMap().get(("COLUMN" + i));
					row = sheet.getRow(j);
					for (int w = 0, y = columinfo.length; w < y; w++) {
						cell = row.getCell((short) (w + noline));
						switch (cell.getCellType()) {
						case HSSFCell.CELL_TYPE_NUMERIC:
							//System.out.println("number：" + cell.getNumericCellValue());
							ps.setString(w + 1, cell.getNumericCellValue() + "");
							break;
						case HSSFCell.CELL_TYPE_STRING:
							//System.out.println("string：" + cell.getStringCellValue());
							ps.setString(w + 1, cell.getStringCellValue() + "");
							break;
						case HSSFCell.CELL_TYPE_BOOLEAN:
							//System.out.println("boolean：" + cell.getBooleanCellValue());
							ps.setString(w + 1, cell.getBooleanCellValue() + "");
							break;
						case HSSFCell.CELL_TYPE_FORMULA:
							//System.out.println("formula：" + cell.getCellFormula());
							ps.setString(w + 1, cell.getCellFormula() + "");
							break;
						case HSSFCell.CELL_TYPE_BLANK:
							ps.setString(w + 1, "");
							break;
						default:
							//System.out.println("unsuported sell type");
							break;
						}
					}
					//隐藏列set值
					getClassType(ps, i, test, columinfo, j);
					ps.addBatch();
					ps.executeBatch();
					//每生成一条insert语句，就commit一次（大数据）
					conn.commit();
					noline += columinfo.length;
				}
				success += 1;
			} catch (Exception e) {
				e.printStackTrace();
				log.error("=======导入第"+j+"条失败========");
				System.out.println("=======导入第"+j+"条失败========");
				fail += 1;
				continue;
			}
		}
		log.debug("===========导入数据结束===========");
		System.out.println("===========导入数据结束===========");
		String result = "总共导入数据"+totalRow+"条，其中成功"+success+"条，失败"+fail+"条";
		//打印 
		log.debug(result);
		System.out.println(result);
		return result;
	}
	
	/**
	 * 根据传值类型做判断，分别进行set值
	 * @author Karl.luo at 2013-07-25
	 * @param ps
	 * @param mapNo
	 * @param test
	 * @param columinfo
	 * @param dataLine
	 */
	public void getClassType(PreparedStatement ps, int mapNo, ExcelImportCon test,
			String[] columinfo,int dataLine) {
		String[][] hiddenInfo = (String[][]) test.getHiddenColMap().get(
				("HIDDEN" + mapNo));	
		for (int j = 0; j < hiddenInfo.length; j++) {
			switch (Integer.parseInt(hiddenInfo[0][1])) {
			case 1:
				//System.out.println("this is date!");
				try {
					ps.setTimestamp(columinfo.length + j + 1, Timestamp.valueOf(hiddenInfo[0][2]));
				} catch (SQLException e1) {
					e1.printStackTrace();
				}
				break;
			case 2:
				//System.out.println("this is string!");
				try {
					ps.setString(columinfo.length + j + 1, hiddenInfo[0][2]);
				} catch (SQLException e1) {
					e1.printStackTrace();
				}
				break;
			case 3:
				//System.out.println("this is double!");
				try {
					ps.setDouble(columinfo.length + j + 1, Double.parseDouble(hiddenInfo[0][2]));
				} catch (SQLException e1) {
					e1.printStackTrace();
				}
				break;
			case 4:
				//System.out.println("this is int!");
				try {
					ps.setInt(columinfo.length + j + 1, Integer.parseInt(hiddenInfo[0][2]));
				} catch (SQLException e1) {
					e1.printStackTrace();
				}
				break;
			case 5:
				//System.out.println("this is bianliang!");
				try {
					//System.out.println("-------------"+dataLine);
					String seqVal = test.getSequence(hiddenInfo[0][2],dataLine);
					ps.setString(columinfo.length + j + 1, seqVal);
				} catch (SQLException e) {
					e.printStackTrace();
				}
				break;
			case 6:
				//System.out.println("this is sequence!");
				try {
					String seqVal = test.getSeq(hiddenInfo[0][2]);
					ps.setString(columinfo.length + j + 1, seqVal);
				} catch (SQLException e) {
					e.printStackTrace();
				}
				break;
			default:
				System.out.println("没有相关类型!");
				break;
			}
		}

	}
	
	
	public static void main(String[] args) {
		ImportExcelFile x = new ImportExcelFile();
		String filePath = "D:/testDemo.xls";
		ExcelImportCon test = new ExcelImportCon();
		String[] colArr;
		test.getTableMap().put("TABLE1", "aaa");
		colArr = new String[]{"test1","test2"};
		test.getColumnMap().put("COLUMN1", colArr);
		String[][] hiddenArr = new String[1][3];
		hiddenArr[0] = new String[]{"no","5","SEQ_AAANO"};
		test.getHiddenColMap().put("HIDDEN1", hiddenArr);
		test.getTableMap().put("TABLE2", "bbb");
		colArr = new String[]{"test3","test4","test5"};
		test.getColumnMap().put("COLUMN2", colArr);
		hiddenArr[0] = new String[]{"no","5","SEQ_AAANO"};
		//hiddenArr[0] = new String[]{"in","2","11111"};
		//hiddenArr[0] = new String[]{"in","3","201.121};
		//hiddenArr[0] = new String[]{"in","3","admin};
		//hiddenArr[0] = new String[]{"in","1","20130717};
		test.getHiddenColMap().put("HIDDEN2", hiddenArr);
		try {
			System.out.println(x.getSuccessFlag( test, filePath));		
		} catch (Exception e) {		
			e.printStackTrace();
		}
		
	}
}


{% endhighlight %}

> Ps：通过Excel导入到多张表，excel里面的字段要按照在main函数里面的配置顺序来，这样存入数据库表的时候才不会出现问题。

