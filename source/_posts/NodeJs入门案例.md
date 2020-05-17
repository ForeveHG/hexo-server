---
title: NodeJs入门案例
date: 2017-01-01 16:00:03
tags: [Node]
categories: 技术
---
刚开始接触nodejs，看了nodejs入门指南，根据上面的例子实现了一个特别简单的图片上传功能.
文件目录：
nodeTest
--index.js
<!--more-->
--server.js
--route.js
--requestHandlers.js

下面是代码：
```
<!--index.js-->
var server = require('./server');
var route = require('./route');
var requestHandlers = require('./requestHandlers');
var handle = {}
handle["/"] = requestHandlers.start;
handle["/start"] = requestHandlers.start;
handle["/upload"] = requestHandlers.upload;
handle["/show"] = requestHandlers.show;
server.start(route.route,handle);
```

```
<!--server.js-->
var http = require("http");
var url = require("url");

function start(route,handle){
    http.createServer(function(request,response){
        var pathname = url.parse(request.url).pathname;
        console.log("Request for "+pathname+" received.");
        route(pathname,handle,response,request);   
    }).listen(8888);
    console.log("Server has started.");
}

exports.start=start;
```

```
<!--route.js-->
function route(pathname,handle,response,request){
    console.log(pathname,handle);
    if(typeof handle[pathname] === 'function'){
        return handle[pathname](response,request);
    }else{
        console.log("No reuest handle found for "+pathname)
        response.writeHead(404,{"Content-Type":"text/plain; charset='utf-8'"});
        response.write("404 Not Found");
    }
}
exports.route = route;
```

```
<!--srequestHandlers-->
var exec = require("child_process").exec;
var querystring = require("querystring");
var formidable = require("formidable");
var fs = require("fs");
var util = require("util");

function start(response,request){
    console.log("Request handler 'start' was called");
    var body = '<html>'+
    '<head>'+
    '<meta http-equiv="Content-Type" '+
    'content="text/html; charset=UTF-8" />'+
    '</head>'+
    '<body>'+
    '<form action="/upload" enctype="multipart/form-data" '+
    'method="post">'+
    '<input type="file" name="upload">'+
    '<input type="submit" value="Upload file" />'+
    '</form>'+
    '</body>'+
    '</html>';
    response.writeHead(200,{"Content-Type":"text/html"});
    response.write(body);
    response.end();

}

function upload(response,request){
    console.log("Request handler 'upload' was called");
    var form = new formidable.IncomingForm();
    form.uploadDir = "G:\\tmp";
    console.log("abliut to parse");
    form.parse(request,function(error,fields,files){
        console.log("parsing done");
        fs.renameSync(files.upload.path,"G:\\tmp\\test.png");
        response.writeHead(200,{"Content-Type":"text/html"});
        response.write("received image:<br/>");
        response.write("<img src='/show' />");
        response.end();    
    });
}

function show(response){
    console.log("Request handler 'show' was called");
    fs.readFile("G:\\tmp\\test.png","binary",function(error,file){
        if(error){
            response.writeHead(500,{"Content-Type":"text/plain"});
            response.write(error+"\n");
            response.end();
        }else{
            response.writeHead(200,{"Content-Type":"image/png"});
            response.write(file,"binary");
            response.end();
        }
    })
}
exports.start = start;
exports.upload = upload;
exports.show = show;
```

踩坑：
1.
