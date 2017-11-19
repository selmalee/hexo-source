---
title: php学习笔记之实战小demo
date: 2017-01-17 23:58:32
tags: PHP
categories:
 - 实践
---

## 前言
都快两个月没写博客啦！我最近在学后端的东西，借此文章做记录，这也是督促自己学习、积累的一种方式吧。
<!-- more -->

## 利用GD库制作验证码
以下是体验地址：
 - 普通验证码：[http://119.29.142.213/captcha/form.php](http://119.29.142.213/captcha/form.php)
 - 图片验证码：[http://119.29.142.213/captcha/imgCaptcha/form.php](http://119.29.142.213/captcha/imgCaptcha/form.php)
 - 中文验证码：[http://119.29.142.213/captcha/cnCaptcha/form.php](http://119.29.142.213/captcha/cnCaptcha/form.php)

主要思路：
 1. 利用GD库绘出验证码、干扰点和干扰线，主要用到imagecreatetruecolor、imagecolorallocate、imagefill、imagestring、imagesetpixel、imageline等API。
 2. 将验证码存在$_SESSION['authcode']中，利用imagepng()以 PNG 格式将所绘的图像输出到浏览器。
 3. 在form.php中写好前端界面，将captcha.php的路径写到显示验证码图片的`<img>`的src属性中，实现输入验证码后点击提交跳转到form_ok.php中。
 4. 在form_ok.php中对比$_SESSION['authcode']和$_POST['authcode']，显示提示信息。
 5. 图片验证码只是把随机数据写成图片与图片名字的数组，返回图片文件；中文验证码则是把随机数据改为由汉字组成的字符串。

[代码github地址](https://github.com/seminelee/phpdemo/tree/master/captcha)

## 写一个cgi
cgi(Common Gateway Interface)，公共网关接口，是HTTP服务器与你的或其他机器上的程序进行“交谈”的一种工具。工作原理是这样的：浏览器通过HTML表单或其他方式请求指向一个CGI应用程序的URL，服务器收发请求后执行CGI，把结果格式化为网络服务器和浏览器能够理解的文档如HTML、json，最后返回到浏览器。cgi可以用很多种语言编写，这里用php编写并用到了thinkphp框架。

主要思路：
1. 新建一个MySQL数据表，有两个字段，分别是name和favor，用来存储数据。
![tb_fruit](/static/2017/03/tb_fruit.jpeg)
2. 在Lib/Action/IndexAction.class.php中写两个方法，分别是create和submit。create返回下拉列表的数据和下拉列表默认选中的value；submit接收浏览器传送的数据，写入表，返回成功的消息。
``` php
class IndexAction extends Action {

    public function index(){
        $origin=isset($_SERVER['HTTP_ORIGIN'])?$_SERVER['HTTP_ORIGIN']:'';
        /************** 获取客户端的Origin域名 **************/
        header('Access-Control-Allow-Origin:'.$origin);
        header('content-type:application/json;charset=UTF-8');

        $name = $_POST["name"];
        $favor = $_POST["fruit"];
        $data = array(
          'name' => $name,
          'favor' => $favor
        );
        $tb = M('fruit');
        if($tb->data($data)->add()){
          echo 'success';
        } else {
          echo $_POST['name'];
        }
    }

    public function create(){
        $jsonp = $_GET["callback"];
        $default = "banana";
        $data = array(
            "opts" => array("apple"  => "苹果","banana" => "香蕉","watermelon" => "西瓜"),
            "default" => $default
        );

        echo $jsonp.'('.json_encode($data).')';
    }
}
```
3. 在本地新建一个html，写一个表单，利用ajax发送get请求（来获取初始化下拉列表的数据）和post请求（提交表单）。

``` html
<form id="form">
    <h1>表格标题</h1>
    <label>姓名：</label>
    <input type="text" name="name">
    <br/>
    <br/>
    <label>最喜爱的水果：</label>
    <select id="favorSelect" name="fruit">
    </select>
    <input id="submit" type="button" style="display: block; margin: 40px 0 0;" value="提交">
</form>
```
``` js
$(document).ready(function(){
    $.getJSON('http://119.29.142.213/thinkphpdemo/index.php?m=Index&a=create&callback=?', function(data){
        var opts = data['opts'];
        for(val in opts){
            $('#favorSelect').append('<option value="'+val+'">'+opts[val]+'</option>');
        }
        $('#favorSelect').val(data['default']);
    })
});

$('#submit').on('click', function(){
    var data = {};
    var temp = $('#form').serialize().split('&');
    var key, val;
    for (var i=0; i<temp.length; i++){
        key = temp[i].split('=')[0];
        val = temp[i].split('=')[1];
        data[key] = val;
    }
    console.log(data);
    $.post('http://119.29.142.213/thinkphpdemo/', data, function(res){
        console.log(res);
    })
})
```
P.S.没有写样式有点丑请不要介意= =

