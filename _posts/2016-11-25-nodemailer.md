---
layout: post
title: NodeMailer 批量发送邮件
categories:
- Tech
- Code
tags:
- NodeMailer
---

> GitHub link: <https://github.com/luolinjia/batchMail>  

我们自己在测试nodemailer的时候，只需要如下简单的配置即可：  

{% highlight javascript %}
var nodemailer = require('nodemailer');
var smtpConfig = {
    host: 'mail.xxx.com',
    port: 25,
    secure: false,
    ignoreTLS: true,
    auth: {
        user: '',
        pass: ''
    },
    pool: true,
    maxConnections: 15000
};

// setup e-mail data with unicode symbols
var mailOptions = {
    from: 'xxx@xxx.com', // sender address
    to: '360512239@qq.com'
    // bcc: 'xiao_luove@126.com',
    subject: '', // Subject line
    text: 'This is a demo email', // plaintext body
    html: '<div></div>' // html body
};
var transporter = nodemailer.createTransport(smtpConfig);

transporter.sendMail(mailOptions, function(err, info){
    console.log('Message sent: ' + info.response);
    if(err){
        console.log('one email send failed with address: ' + mailAddr);
    }else{
        console.log('successfully!');
    }
    callback(null);
});
{% endhighlight %}  

但是很多**实际的场景**却跟简单的有很大的差别，这里我们需要达到如下需求：  

- 批量发送，控制批量的具体数量
- 发送的地址通过excel来获取
- 发送成功后，记录发送成功邮件的地址

{% highlight javascript %}
var nodemailer = require('nodemailer');
var Excel = require("exceljs");
var async = require("async");
var _ = require("underscore");
var fs = require("fs");
var smtpConfig = {
    host: 'mail.xxx.com',
    port: 25,
    secure: false,
    ignoreTLS: true,
    auth: {
        user: '',
        pass: ''
    },
    pool: true,
    maxConnections: 15000
};

// setup e-mail data with unicode symbols
var mailOptions = {
    from: 'xxx@xxx.com', // sender address
    to: '360512239@qq.com'
    // bcc: 'xiao_luove@126.com',
    subject: '', // Subject line
    text: 'This is a demo email', // plaintext body
    html: '<div></div>' // html body
};
var transporter = nodemailer.createTransport(smtpConfig);

async.parallel(
    {
        all: function (callback) {
            var allMembersWorkbook = new Excel.Workbook();
            var emailArr = [];
            allMembersWorkbook.xlsx.readFile('E:\\files\\work\\onenet_members.xlsx')
                .then(function () {
                    var worksheet = allMembersWorkbook.getWorksheet(1);
                    var emailsColumn = worksheet.getColumn(1);
                    emailsColumn.eachCell(function(cell, rowNumber) {
                        cell.value.text ? emailArr.push(cell.value.text) : emailArr.push(cell.value);
                    });
                    emailArr = _.uniq(_.compact(emailArr));
                    callback(null, {arr:emailArr, worksheet: worksheet});
                });
        },
        success:function(callback){
            var successMembersWorkbook = new Excel.Workbook();
            var emailArr = [];
            successMembersWorkbook.xlsx.readFile('E:\\files\\work\\onenet_success.xlsx')
                .then(function () {
                    var worksheet = successMembersWorkbook.getWorksheet(1);
                    var emailsColumn = worksheet.getColumn(1);
                    emailsColumn.eachCell(function(cell, rowNumber) {
                        cell.value.text ? emailArr.push(cell.value.text) : emailArr.push(cell.value);
                    });
                    callback(null, {arr:emailArr, worksheet: worksheet, workbook: successMembersWorkbook});
                });
        }
    },
    function(err1, results){
        if (err1){
            console.log(err1);
            return;
        }
        var resultsArr = _.reject(results.all.arr, function (email) {
            return _.contains(results.success.arr, email);
        });
        // send the maximum 4 at the same time.
        async.eachLimit(resultsArr, 4, function(mailAddr, callback){
            var newMailOptions = _.extend({to: mailAddr}, mailOptions);
            // send mail with defined transport object
            transporter.sendMail(newMailOptions, function(err2, info){
                console.log('Message sent: ' + info.response);
                if(err2){
                    console.log('one email send failed with address: ' + mailAddr);
                }else{
                    // mark the success recording into the excel file!
                    results.success.worksheet.addRow([mailAddr]).commit();
                }
                callback(null);
            });
        }, function(){
            // delete the relevant file
            fs.unlink('E:\\files\\work\\onenet_success.xlsx', function (err3) {
                if(err3){
                    console.log('delete file error:', err3);
                    return;
                }
                // write the relevant file
                results.success.workbook.xlsx.writeFile('E:\\files\\work\\onenet_success.xlsx')
                    .then(function () {
                        console.log('task finished');
                    });
            });
        });
    }
);
{% endhighlight %}   