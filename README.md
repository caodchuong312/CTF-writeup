

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



