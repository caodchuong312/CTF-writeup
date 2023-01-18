Cao Dang Chuong_AT18C

# BinggChillinggg - PROG/MISC


Description: Bingg.zip


![image](https://user-images.githubusercontent.com/92881216/212955105-592c72b0-5448-4660-9454-8a8b298d66ee.png)

Mình giải nén file và thấy được rất nhiều bức ảnh nhỏ có cùng kích thước(10x10) và đoán được nó được cắt ra từ 1 bức ảnh lớn (520x780) dựa vào tên các ảnh mình biết được quy luật để xếp chúng lại.
Ý tưởng của mình là sẽ ghép tường ảnh theo ngang rồi tổng hợp các ảnh đó ghép theo chiều dọc sẽ được ảnh hoàn chỉnh. 
Mình tìm hiểu và sử dụng thư viện OpenCV của python:
```python
import cv2
list_result=[]  #list nãy mình sẽ giải thích phía dưới
for j in range(0,520,10):
    list=[]     #list này sẽ chứa tên tất cả ảnh mà bài cho
    for i in range(0,780,10):
        list.append(f'img{j}_{i}.png')
    image_h = []    #list này sẽ chứa dữ liệu các ảnh để ghép theo chiều ngang
    for image_name in list:
        image = cv2.imread(image_name)  #Hàm này được sử dụng để đọc một hình ảnh từ một tệp và trả về nó dưới dạng một mảng NumPy đa chiều
        image_h.append(image)        #Thêm dữ liệu vừa đọc ở trên vào list
    result = cv2.hconcat(image_h)    #Hàm hconcat() ghép các ảnh theo chiều ngang từ list
    cv2.imwrite(f'result{j}.png', result)   #kết quả sẽ ra đc các bức ảnh đã ghép theo chiều ngang (10x780)
    list_result.append(f'result{j}.png')    #list này sẽ chứa tên các ảnh ở trên
list_v=[]   #list này sẽ chứa dữ liệu các ảnh đã ghép ở trên để ghép theo chiều dọc
for image_name in list_result:  #tượng tự trên
        image = cv2.imread(image_name)
        list_v.append(image)
result_e = cv2.vconcat(list_v)   #Hàm vconcat() ghép các ảnh theo chiều dọc từ list
# Lưu ảnh cuối cùng :
cv2.imwrite(f'result.png', result_e)
```
kết quả :

![image](https://user-images.githubusercontent.com/92881216/212963793-3634e976-765d-44a4-9e65-f2ef96fd5201.png)

> Flag: `KCSC{Zao_shang_hao_zhong_gou!_Xian_zai_wo_you_bing_chilling!!}`  




# Phpri Phprai - WEB
Description: http://47.254.251.2:8889

![image](https://user-images.githubusercontent.com/92881216/212965042-1d0d2fbb-e8ee-4304-8182-9e14c597bec6.png)

Vào link mình thấy source code đã được show ra : 
```php
<?php
    include 'config.php';
    show_source("index.php");
    error_reporting(0);
    if(isset($_GET["1"]) && isset($_GET["2"])) {
        $str1 = $_GET["1"];
        $str2 = $_GET["2"];
        if(($str1 !== $str2) && (md5($str1) == md5($str2))) {
            echo $flag1;
        }
    }
    if(isset($_GET["3"])) {
        if( strcmp( $_GET['3'], $$flag ) == 0) {
            echo $flag2;
        }
    }
    if(isset($_GET["4"])) {
        $str4 = $_GET["4"];
        $str4=trim($str4);
        if($str4 == '1.4e5' && $str4 !== '1.4e5') {
            echo $flag3;
        }
    }
    if(isset($_GET["5"])) {
        $str5 = $_GET["5"];
        if($str5 == 69 && $str5 !== '69' && $str5 !== 69 && strlen(trim($str5)) == 2) {
            echo $flag4;
        }
    }
    if(isset($_GET["6"])) {
        $str6 = $_GET["6"];
        $var1 = 'KaCeEtCe';
        $var2 = preg_replace("/$var1/", '', $str6);
        if($var1 === $var2) {
            echo $flag5;
        }
    }
?>
```

Nhìn tổng quan để giải bài này mình cần truyền vào url (phương thức GET) các biến có tên '1', '2', '3', '4', '5', '6' được gán các giá trị tương ứng thỏa mãn bài toán sẽ show ra được biến flag1 -> flag5.
Bắt đầu thôi :

-flag1:

Để flag1 show ra thì 2 chuỗi giá trị truyền vào khác nhau nhưng giá trị băm md5 của chúng phải giống nhau, mình bypass bằng cách biến chúng thành mảng chúng thành mảng và truyền 2 giá trị khác nhau: 1[]=1,2[]=2 và thành công 

payload: ```?1[]=1&2[]=2```

-flag2:

Thực hiện hàm strcmp() so sánh giá trị của biến 3 với giá trị của $$flag, mình không biết giá trị nó là gì nên mình thử không truyền gì ai ngờ nó đúng luôn @@ hoặc có thể biến nó thành mảng 3[] như ở flag1 

Payload: ```&3```

-flag3: 

Giá trị biến 4 sẽ so sánh với '1.4e5' và mình nghĩ nó sẽ so sánh giá trị chuỗi là sai và so sánh giá trị số học là đúng thì nó sẽ bypass, mà 1.4e5=1,4.10^5 nên mình sẽ truyền giá trị bằng nó là 14.e4 và thành công 

Payload: `&4=14.e4`

-flag4:

Để flag4 show ra thì: giá trị biến 5 bằng 69 và khác chuỗi '69' và chiều dài biến 5 sau khi qua hàm trim() (xóa khoảng trắng) bằng 2 nên ta dễ dàng biết được bằng truyền giá trị khoảng trắng vào trước hoặc sau 69

(Để dễ hình dung nên mình biểu diễn khoảng trắng trên url là %20)

Payload: ```&&5=69%20```

-flag5:

Để flag5 show ra thì: giá trị biến $var1 bằng với $var2 với $var2= preg_replace("/$var1/", '', $str6);

$var2 sẽ có giá trị mới sau khi xóa giá trị $var1 ('KaCeEtCe') có trong $str6 (biến 6).

Bypass bằng cách ta sẽ chèn 'KaCeEtCe' vào giữa 'KaCeEtCe' thành 'KaCKaCeEtCeeEtCe' vì hàm preg_replace() sẽ không thể kiểm tra kết quả để thực hiện tiếp

Payload: ```&6=KaCKaCeEtCeeEtCe```

Tổng hợp lại :

Payload: ```?1[]=1&2[]=2&3&4=14.e4&5=69%20&6=KaCKaCeEtCeeEtCe```

> Flag: `KCSC{B0_u_Bu_S4C_8usssssss_https://www.youtube.com/watch?v=xQtC3F8fH6g}`  


# ezenc - CRYPTO

Description: file chal.txt

![image](https://user-images.githubusercontent.com/92881216/212980732-2f2e634a-13b3-41be-a949-323890ee071d.png)

File chal.txt :

`4e544d7a4d44526c4e5451314d544d7a4e7a51304e6a59794e6d51305a5463324e5745304e7a5a6a4e7a553159544d784d7a6b305954597a4d7a457a4f5451304e6a497a4d6a4d354e7a4d304f54557a4e4455324f4459324e54457a5a444e6b`

Mình đoán đó là mã hex nên nên giải mã ra được :

`NTMzMDRlNTQ1MTMzNzQ0NjYyNmQ0ZTc2NWE0NzZjNzU1YTMxMzk0YTYzMzEzOTQ0NjIzMjM5NzM0OTUzNDU2ODY2NTEzZDNk`

Tiếp tục mình thử decode Base64 lại được mã hex, và cứ vậy mình được flag ! 

Mình sử dụng https://gchq.github.io/CyberChef/ :

![image](https://user-images.githubusercontent.com/92881216/212982722-5cfb91f5-3026-44e1-ae44-664562650fb5.png)

> Flag : `KCSC{Encoding_Is_Cool!!!}`

# Chuyến tàu vô tận - CRYPTO

Description: messenger.txt

![image](https://user-images.githubusercontent.com/92881216/212983235-a66e954c-d198-4fba-bccf-9f8cce69d4fc.png)

File messenger.txt chứa nhị phân : 
`01010011 00110001 01000010 01010100 01010110 01101011 00110101 01010110 01010011 01101011 01001010 01010011 01010101 00110000 01000110 01001111 01010001 00110000 01001110 01001101 01010001 01010101 01010110 01000010 01010010 01010101 01010110 01001000 01010011 00110000 01110100 01010000 01010110 01010101 00111001 01000110 01010100 00110000 01010110 01000010 01010110 01000110 01010010 01010101 01010100 00110001 01001110 01000110 01010101 00110001 01001010 01010000 01010111 01010110 01001010 01000111 01010100 01000110 01001110 01001010`

Decode sang text được `S1BTVk5VSkJSU0FOQ0NMQUVBRUVHS0tPVU9FT0VBVFRUT1NFU1JPWVJGTFNJ` sau đó mình decode Base64 được : `KPSVNUJBRSANCCLAEAEEGKKOUOEOEATTTOSESROYRFLSI`

Đến đây mình thấy phần description của bài là 'Hashibira Inosuke có nhặt được một bức thư được mã hóa trên đường ray' và mình nghĩ đến 
```Rail Fence``` và mình lên https://www.dcode.fr/redefence-cipher để bruteforce và được flag :

![image](https://user-images.githubusercontent.com/92881216/213079664-3c51788c-55b3-4de9-a662-b40d325375a8.png)

> Flag : `KCSCPLEASESAVERENGOKUKYOJUROBEFORELASTSTATION`


# cat -PWNABLE


Description: nc 146.190.115.228 9994

![image](https://user-images.githubusercontent.com/92881216/213080208-f65b87dd-884b-4284-8da1-08fa8d4f5368.png)

Mình thử chạy dòng lệnh trên máy ảo kali của mình : 

![image](https://user-images.githubusercontent.com/92881216/213080437-39ec2966-1e9f-4185-80b0-87239489e2cf.png)

Như vậy chương trình sẽ cho nhập Username và Password để tiếp tục và bây giờ mình sẽ xem xét tập tin cat được đính kèm để xem mã nguồn.

Mình sử dụng ghidra và truy cập vào hàm main và biết được username là "KCSC_4dm1n1str4t0r" .

![image](https://user-images.githubusercontent.com/92881216/213081176-5cc12e9f-fd46-4223-9850-73fa2f34d93b.png)

Đến đây mình cũng không biết làm sao nữa :v  nên mình chạy thử câu lệnh `strings cat` đển xem file có gì và mình nghĩ password là : 'wh3r3_1s_th3_fl4g' mình check user và pass thì nó đúng và bây giờ chương trình yêu cầu mình nhập secret :


![image](https://user-images.githubusercontent.com/92881216/213083487-de5799fb-fefe-4e98-b874-83f07c779508.png)
![image](https://user-images.githubusercontent.com/92881216/213083809-fdf9130e-1361-4917-971a-9bfcdc3b2eee.png)

Mình không có thông tin gì về secret nên mình đã nghĩ đến buffer overflow và mình đã thành công khi nhập 512 ký tự 'a' :

![image](https://user-images.githubusercontent.com/92881216/213084516-32c9d6a2-7a74-4319-93d0-e32370b8f6ba.png)

> Flag : `KCSC{w3ll_d0n3_y0u_g0t_my_s3cr3t_n0w_d04942f299}`


















