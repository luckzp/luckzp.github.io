---
title: ajax学习
urlname: gu3u8s
date: 2018-07-03 11:51:00 +0800
author: Zhang Peng
tags: [前端]
categories: []
---

Ajax 是一种异步请求数据的 web 开发技术，对于改善用户的体验和页面性能很有帮助。简单地说，在不需要重新刷新页面的情况下，Ajax 通过异步请求加载后台数据，并在网页上呈现出来。常见运用场景有表单验证是否登入成功。
需求：点击登陆按钮时，提交账号和密码给后端，异步刷新显示返回的数据。

**Demo 代码：**

```javascript
<script>
    $(function() {
        $("#submitBtn").click(function() {
            if(0 == $("#username").val().length || 0==$("#password").val().length){
                $("#errorMessage").text("输入账号密码为空");
                $("#loginErrorMessageDiv").css("visibility","visible");
                return false;
            }else{
            var username = $("#username").val(),
                password = $("#password").val();

                $.ajax({
                    type: "POST",
                    data: {username: username, password: password}, //传输的数据
                    dataType:"json", //传输的数据类型
                    url: "/loginsuccess", //提交目的地址
                    success: function (data) { //返回的数据为data对象，该对象有msg和code两个属性
                        console.log(data);
                        if (data.code == 1) {
                            $("#errorMessage").text(data.msg);
                            $("#loginErrorMessageDiv").css("visibility","visible");
                            return false;
                        } else {
                            window.location.href=data.msg;
                            return true;
                        }
                    },
                    error: function (data) {
                        alert("认证失败");
                    }
                });
            }
        });

        $("input").keyup(function(){
            $("#loginErrorMessageDiv").css("visibility","hidden");
        });
    });
</script>
```

**效果图：**

![](http://ww1.sinaimg.cn/large/aacc02d8ly1fxv1c35oiij20pu0a3gr3.jpg#alt=)
