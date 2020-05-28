---
layout: post
title: Java Oracle 导入到 Excel
categories:
- Study
- Data
tags:
- Excel
- Java
- Oracle
---

> Oracle数据库导入到Excel文件  

这里的概况是讲Oracle的数据导入到Excel文件的一个公共类（运用的是POI，并且只支持较老的Excel版本）  


附件：ExportExcelInst.java  
{% highlight java %}

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Types;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.apache.poi.hssf.usermodel.HSSFCell;
import org.apache.poi.hssf.usermodel.HSSFRow;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.poifs.filesystem.POIFSFileSystem;

import com.fund.etrading.eccapp.dto.ExportExcelDto;

/**
 * 导出EXCEL公共类
 * @author Karl.luo at 2013-08-19
 *
 */
public class ExportExcelInst {
	protected static final Log log = LogFactory.getLog(ExportExcelInst.class.getName());
	/*
     * Excel文件后缀名
     */
    private static final String EXCEL_POSTFIX = ".xls";
	/*
	 * 所查询sequence id
	 */
	private String seq_id;
	/*
	 * 传值map数据
	 */
	private Map map = new HashMap();
	/*
	 * procedure 名称
	 */
	private String pro_name;
	
	/***********************************DB config*****************************************/
	private String classString="oracle.jdbc.driver.OracleDriver";
	private String username="ec1016";
	private String password="password";
	private String url="java:oracle:thin:@192.168.1.171:1521:htfdbweb";
	private Connection conn=null;
	private PreparedStatement ps = null;
	private CallableStatement cStmt = null;
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
	
	/**
	 * 拿SEQ_REPORT_ID 最新的下一个ID  NEXTVAL
	 */
	public void getSeq(){
		conn = getConnection();
		String sql = "";
		try {
			sql = "SELECT SEQ_REPORT_ID.NEXTVAL FROM DUAL";
			ps = conn.prepareStatement(sql);
			rs = ps.executeQuery();
			while(rs.next()){
				seq_id = rs.getString(1);
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try{
				freeConnection(conn, ps, rs);
			}catch(java.sql.SQLException ex){
				ex.printStackTrace();
			}	
		}
	}
	
	/**
	 * 删除table数据
	 * @return
	 */
	public String delReportDateData(){
		String result = "9999";
		String sql = null;
		try {
			sql = "DELETE FROM REPORT_DATE WHERE REPORT_ID = " + seq_id;
			conn = getConnection();
			ps = conn.prepareStatement(sql);
			ps.execute();
			result = "0000";
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try{
				freeConnection(conn, ps, rs);
			}catch(java.sql.SQLException ex){
				ex.printStackTrace();
			}	
		}
		return result;
	}
	
	private void freeConnection(Connection conn,PreparedStatement pStmt, ResultSet rs ) throws SQLException {
        try {
        
            if(rs != null){
                rs.close();
                rs = null;
            }
            if(pStmt != null){
                pStmt.close();
                pStmt = null;
            }
            if((conn != null) && (!conn.isClosed())){
                conn.close();
                conn = null;
            }
        } catch (java.sql.SQLException ex) {
        	log.error("Exception : ",ex);
        	throw ex;
        }
    }
	
	/**
     * 判断指定文件的路径是否是Excel文件
     * @param filePath  文件的路径 
     * @return   是否是Excel文件
     *          true:是Excel文件
     *          false:不是Excel文件
     */
    public boolean isExcel(String filePath){
        //获取文件后缀名
        String postfix = getPostfix(filePath);
        if(!postfix.equals(ExportExcelInst.EXCEL_POSTFIX)){
            return false;
        }
        return true;
    }
	
    /**
	 * 返回一个文件的后缀 如.txt,.doc
	 *
	 * @param filename
	 * @return
	 */
	public static String getPostfix(String filename) {
		int pos = filename.lastIndexOf('.');
		if (pos == -1)
			return "";
		return filename.substring(pos);
	}
	
	/**
	 * 传入文件地址，copy到相应的新Excel文件
	 * @param filePath
	 * @return
	 */
	public String createExcel(String filePath){
		String newTotalPath = null;
		String cpre = "1";
		//判断是否为Excel文件
		if(!isExcel(filePath)){
            System.out.println("文件必须是Excel文件,必须是xls格式的文件");
            cpre = "0";
            return "";
        }
		//如果是，copy一个新的Excel文件，并用新的名字：fileName+yyyyMMdd
		if("1".equals(cpre)){
			// 获取文件的存放文件夹
			// 得到最后一个“/”位置
			int filePos = filePath.lastIndexOf("/");
			String newFilePath = filePath.substring(0, filePos + 1);
			// 拿到时间yyyyMMdd
			SimpleDateFormat df = new SimpleDateFormat("yyyyMMdd");//设置日期格式
		    String dfDate = df.format(new Date());
			// 新的excel 文件名
		    int fileNamePosBg = filePath.lastIndexOf("/");
		    int fileNamePosEnd = filePath.lastIndexOf(".");
	        String excelName = filePath.substring(fileNamePosBg + 1,fileNamePosEnd);
			newTotalPath = newFilePath + excelName + dfDate + ".xls";
			//判断文件是否已存在
			if(new File(newTotalPath).exists()){
				System.out.println("文件已存在");
				return newTotalPath;
			}
			FileInputStream fis;
			FileOutputStream fos;
			try {
				fis = new FileInputStream(new File(filePath));
				fos = new FileOutputStream(new File(newTotalPath));
				int bytesRead; 
		        byte[] buf = new byte[4 * 1024];  // 4K 
		        while ((bytesRead = fis.read(buf)) != -1) { 
		        	fos.write(buf, 0, bytesRead); 
		        } 
		        fos.flush(); 
		        fos.close(); 
		        fis.close(); 
			} catch (FileNotFoundException e) {
				e.printStackTrace();
			} catch (IOException e) {
				e.printStackTrace();
			} 
		}
		//返回新的文件地址
		return newTotalPath;
	}
	
	/**
	 * 查询table数据结果
	 * @return
	 */
	public List getTableData(){
		List list = new ArrayList();
		String sql = null;
		try {
			sql = "SELECT REPORT_ID, X_NUMBER, Y_NUMBER, VALUE_DATE FROM REPORT_DATE WHERE REPORT_ID = " + Integer.parseInt(seq_id);
			System.out.println(sql);
			conn = getConnection();
			ps = conn.prepareStatement(sql);
			rs = ps.executeQuery(sql);
			ExportExcelDto dto;
			while(rs.next()){
				dto = new ExportExcelDto();
				dto.setExportId(rs.getInt("REPORT_ID"));
				dto.setX(rs.getInt("X_NUMBER"));
				dto.setY(rs.getInt("Y_NUMBER"));
				dto.setValueDate(rs.getString("VALUE_DATE"));
				list.add(dto);
			}
			
			return list;
		} catch (Exception ex) {
			log.error("(EC-Exception)异常：", ex);
		} finally {
			try{
				freeConnection(conn, cStmt, rs);
			}catch(java.sql.SQLException ex){
				ex.printStackTrace();
			}	
		}
		return null;
	}
	
	/**
	 * 把list数据导入到excel文件
	 * @param path
	 * @param list
	 * @return
	 */
	public String exportToExcel(String path, List list){
		POIFSFileSystem fs;
		HSSFWorkbook wb = null;
		HSSFSheet sheet = null;
		FileOutputStream fos = null;
		HSSFRow row = null;
		HSSFCell cell = null;
		String result = "9999";
		try {
			fs = new POIFSFileSystem(new FileInputStream(path));
			wb = new HSSFWorkbook(fs);
			sheet = wb.getSheetAt(0);
			for (int i = 0; i < list.size(); i++) {
				ExportExcelDto dto = (ExportExcelDto)list.get(i);
				row = sheet.createRow((short)dto.getY());
				cell = row.createCell((short)dto.getX());
				cell.setCellValue(dto.getValueDate());
			}
			
			fos = new FileOutputStream(path);
			wb.write(fos);
			fos.close();
			// 导出excel文件成功后，删除数据库数据
			delReportDateData();
			result = "0000";
		} catch (Exception e) {
			e.printStackTrace();
		}
		return result;
	}
	
	/**
	 * 将相关数据插入表report_date作导出记录
	 * @return
	 */
	public String insertIntoTable(){
		getSeq();
		try {
			conn = getConnection();
			String procedure = "{call " + pro_name + "(?,?,?,?,?)}"; 
			cStmt = conn.prepareCall(procedure);
			log.debug("存储过程:" + procedure);
			log.debug(" opid :" +  (String) map.get("opid"));
			log.debug(" reportname :" + (String) map.get("reportname"));
			log.debug(" reportid :" + (String) map.get("reportid"));
			cStmt.setString(1, (String) map.get("opid"));
			cStmt.setString(2, (String) map.get("reportname"));
			cStmt.setString(3, seq_id);
			cStmt.registerOutParameter(4, Types.VARCHAR);
			cStmt.registerOutParameter(5, Types.VARCHAR);
			cStmt.execute();
			String errCod = cStmt.getString(4);
			String errMsg = cStmt.getString(5);
			log.debug("errCod: " + errCod);
			log.debug("errMsg: " + errMsg);
			
			return errCod + "," + errMsg;
		} catch (Exception ex) {
			log.error("(EC-Exception)异常：", ex);
		} finally {
			try{
				freeConnection(conn, cStmt, rs);
			}catch(java.sql.SQLException ex){
				ex.printStackTrace();
			}	
		}
		return null;
	}
	
	
	public Map getMap() {
		return map;
	}

	public void setMap(Map map) {
		this.map = map;
	}

	public String getPro_name() {
		return pro_name;
	}

	public void setPro_name(String pro_name) {
		this.pro_name = pro_name;
	}

	public static void main(String[] args) {
		String proName = "REOPRT_DSPAY_EXPORT";
		ExportExcelInst x = new ExportExcelInst();
		Map maps = new HashMap();
		maps.put("opid", "9999");
		maps.put("reportname", "demo");
		x.setMap(maps);
		x.setPro_name(proName);
		String result = x.insertIntoTable();
		if("0000".equals(result.split(",")[0])){
			String filePath = x.createExcel("D:/HTF_space/newECCJSP2.0/WEB-INF/config/Excel/demo.xls");
			System.out.println("新的文件地址：" + filePath);
			List list = x.getTableData();
			String res = x.exportToExcel(filePath, list);
			System.out.print(res);
		}
	}
	
}


{% endhighlight %}
