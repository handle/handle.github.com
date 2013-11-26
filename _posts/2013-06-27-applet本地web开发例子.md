---
layout: post
title: "applet本地web开发例子"
description: ""
category: Java
tags: [沧海拾遗]
---
{% include JB/setup %}
最近想做一个应用，需要用到本地的tcp服务，因此就想通过applet来实现。骤然发现，虽然用java开发web这么多年，用applet估计还是在上学期间。记录下一个简单的开发例子：

1. 写一个applet类   

       
        import java.applet.Applet;  
        import java.awt.Button;   
        import java.awt.event.ActionEvent;   
        import java.awt.event.ActionListener;    
        import java.io.DataOutputStream;    
        import java.net.Socket;   

        public class TestApplet extends Applet {
            private static final long serialVersionUID = 1L;

	    public void init() {
		    Button send = new Button("send");
		    this.add(send);
		    send.addActionListener(new ActionListener() {
			    @Override
			    public void actionPerformed(ActionEvent e) {
				    try {
					    Socket clientSocket = new Socket("127.0.0.1", 9998);
					    DataOutputStream outToServer = new DataOutputStream(
							    clientSocket.getOutputStream());
					    outToServer.writeBytes("hello tcp" + '\n');
					    clientSocket.close();
				    } catch (Exception ex) {
				    }
			    }
		    });

	    }
        }

2. 将所有的class文件打到jar包里：

        jar -cvf TestApplet.jar \*\.class

3. 将applet放到html代码中：   

        <?xml version = "1.0" encoding="utf-8" ?>
        <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN""DTD/xhtml1-strict.dtd">
        <html xmlns = "http://www.w3.org/1999/xhtml">
        <head>
	    <title>JavaSocketBridge Example</title>
        </head>
        <body>
	    <div>  
	        <applet id="TestApplet" archive="TestApplet.jar" code="TestApplet.class" codebase="." width="100" height="100"></applet>   
	    </div>
        </body>
        </html>

这时候如果你将jar包和上面的html代码一起运行时，会报applet被本地安全设置阻止，因此需要对你的jar包进行签名，如下：

    keytool -genkey -keystore test.store -alias test

这个命令用来生成密钥库。

    keytool -export -keystore test.store -alias test -file test.cert

生成证书。

    jarsigner -keystore test.store TestApplet.jar test

签名完成。

然后再运行页面就可以打开了。
