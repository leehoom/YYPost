# Linux下Wkhtmltopdf的安装和使用


> Wiki：wkhtmltopdf are open source (LGPLv3) command line tools to render HTML into PDF and various image formats using the Qt WebKit rendering engine. These run entirely "headless" and do not require a display or display service.

![wkhtmltopdf](http://ou0bp2mrg.bkt.clouddn.com/2017-08-14-15026995528394.jpg)

* 直接使用

```
sudo apt-get install wkhtmltopdf
```

* 使用：

```
wkhtmltopdf src dst
```

* 如果使用出现：*wkhtmltopdf: cannot connect to X server*，是因为install的版本比较旧。不过官网最新的版本已无需x-server支持。你可以使用下面的处理方法：



1.下载官网预编译版本：

```
wget http://download.gna.org/wkhtmltopdf/0.12/0.12.3/wkhtmltox-0.12.3_linux-generic-amd64.tar.xz
```

2.解压：

```
tar xvfJ wkhtmltox-0.12.3_linux-generic-amd64.tar.xz
```

3.使用其中的可执行文件替换旧版本文件：

```
sudo mv ./wkhtmltox/bin/wkhtmltopdf /usr/bin/wkhtmltopdf
```

4.赋予执行权限：

```
sudo chmod a+x /usr/bin/wkhtmltopdf
```


**其它:**

如果生成的pdf的字符都是方块，则需要安装standard PostScript fonts：

* Centos：

```
sudo yum install urw-fonts libXext openssl-devel
```
* Ubuntu：

```
拷贝中文字体到/usr/share/fonts/msfonts/下，文件名不能为中文，执行完之后：fc-list看一下有没有导入字体。
```

