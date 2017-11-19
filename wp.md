###  2017-9-23

## Geek2017

# Unserialize

#### 0x00 [原理]
     php反序列化
#### 0x01 [目的]
     了解反序列化函数调用的情况和php对象注入漏洞
#### 0x02 [环境]
     windows
#### 0x03 [工具]
     chrome，notepad++
#### 0x04 [步骤]

1.首先打开address，提示为?code=

2.在url栏输入//192.168.19.60:13002/?code=1，出现提示 hint

![](/files_for_wp/fanxulie1.png)

3.根据提示，查看目标文件flag.php http://192.168.19.60:13002/flag.php，出现第二条hint

![](/files_for_wp/fanxulie2.png)

4.查看目标文件help.php http://192.168.19.60:13002/help.php

![](/files_for_wp/fanxulie3.png)

5.百度了一下，发现是php对象注入漏洞：_to string当对象被当做一个字符串的时候被调用

![](/files_for_wp/fanxulie4.png)

6.运行php文件，得到反序列化字符串
            O:9:"FileClass":1:{s:8:"filename";s:8:"flag.php";}

![](/files_for_wp/fanxulie5.png)

7.tags：php反序列化

   所以?code=O:9:"FileClass":1:%7Bs:8:"filename";s:8:"flag.php";%7D

   ![](/files_for_wp/fanxulie6.png)

8.flag：SJU{UUNser1AL1Z3_SJU__0)(0}SJU{UUNser1AL1Z3_SJU__0)(0}

#### 0x05 [总结]
    php对象注入 ，对文件的调用



# Variacover

#### 0x00 [原理]
     php弱类型
#### 0x01 [目的]
     了解md5弱类型比较，变量覆盖，代码审计
#### 0x02 [环境]
     windows
#### 0x03 [工具]
     chrome
#### 0x04 [步骤]

1.首先打开address，源代码为

![](/files_for_wp/ruoleixing1.png)

2.观察代码，发现问题在以下两行：</br>
 @parse_str($b);</br>
 if ($a[0] != 'QNKCDZO' && md5($a[0]) == md5('QNKCDZO'))</br>
 ①parse_str( )函数导致变量覆盖，要重新覆盖变量$a</br>
 ②tags：php弱类型，这里是md5弱类型比较</br>

![](/files_for_wp/ruoleixing2.png)

3.问题解决：</br>
  ①b=a[0]<br>
  ②可用的值有 240610708、QNKCDZO、aabg7XSs、aabC9RqS</br>
   依次尝试：

 ![](/files_for_wp/ruoleixing3.png)  

 ![](/files_for_wp/ruoleixing4.png)

 ![](/files_for_wp/ruoleixing5.png)  

 ![](/files_for_wp/ruoleixing6.png)  

4.flag：SJU{A_sTr_c0vcd3rd_t3st_y0u_0W?}


#### 0x05 [总结]
    变量覆盖，md5弱类型比较