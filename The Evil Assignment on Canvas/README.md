# The Evil Assignment on Canvas 

### link challenge : [The Evil Assignment on Canvas](https://battle.cookiearena.org/challenges/web/the-evil-assignment-on-canvas)


## Vào đề 

nhìn đề có giao diện như sau thì chắc là up shell code rồi  : >

![Alt text](image.png)

đề cho mình up ảnh jpg/jpeg 

ban đầu mình up một file tiêu chuẩn lên ! 

 khi up lên thì nó sẽ move file mình qua : ``` /uploads/your_file```

 ![Alt text](image-1.png)

 ở client nó chỉ chấp nhận type : jpg/jpeg 

 -> mình sẽ sửa request ở burp ! 
 
 ![Alt text](image-2.png)
 Mình đổi thử thành php và đã bị nó filter không cho up lên ! 
mình sẽ thử các file type khác nhau : như php2,phtml,pgif = > đều không được vì nó chỉ cho jpg và jpeg 
thử đổi thành  

![Alt text](image-3.png)
và thành công => mình đã bypass được check extension 

giờ thì sửa nội dung file =>  nên sửa bằng exiftool , hoặc do mình lười thì sửa vào giữa file ( vì sợ nó check byte đầu ) 


![Alt text](image-5.png)

mình đã up được shell code giờ thì cần lên và gọi flag ra thôi ! 

Update: khi mình lấy được src của nó !  cùng phân tích xem nó filter kiểu gì 
vì nó chặn curl nên mình dùng wget để gửi file sang webhook ! 

![Alt text](image-6.png)

Ban đầu nó lấy content của file return ra string rồi đưa vào biến *$magic_num_handler* , rồi check 3 char đầu đưa sang hex rồi check xem có bằng *ffd8ff* không  ? cái đó là đánh dấu bắt đầu tập tin của file ảnh jpg/jpeg 
Bước tiếp theo là nó check extension = >  nó kiểm ta xem vị trí của .jpg có trong chuỗi path của nó không ? 
nếu ko có jpg và jpeg  => nó log ra lỗi và exit 
và payload của mình : ```shell.jpg.php``` => PATH của mình là .jpg.php  nó có trong chuỗi nên nó bypass được ! 

# FLAG 
``` CHH{l3v3L_4_c0d3_0f_c0ndUcT_27c620c439cdbcf7c3a08fba55c843b4} ```