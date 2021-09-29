# [Trang chủ](https://ppap-1264589.github.io/interesting-solution)

## Bài Toán
Cho n điểm trên mặt phẳng tọa độ. Tìm khoảng cách ngắn nhất giữa 2 điểm bất kỳ.

**Input**

Dòng đầu tiên chứa số nguyên dương n (n ≤ 10^5) là số điểm.

n dòng tiếp, dòng thứ i chứa 2 số nguyên xi, yi (|xi|,|yi|≤10^9) xác định tọa độ điểm thứ i.

**Output**

Một số thực duy nhất là khoảng cách nhỏ nhất tìm được. Ghi chính xác 3 chữ số sau dấu phảy.

**Example**

*Input*
```c++
3
1 1
2 2
2 1
```

*Output*
```c++
1.000
```

## Thuật toán BruteForce

Ta xét từng cặp điểm một, tìm cặp điểm nào có khoảng cách gần nhất

-> Độ Phức Tạp : O(n^2)

Với n = 2*100000, thuật toán trên tỏ ra rất không hiệu quả về mặt thời gian

## Quan sát

Thực tế chúng ta có thể quan sát một cách thông minh hơn:

Sort các điểm theo thứ tự tăng dần của x, nếu hai điểm có tọa độ x bằng nhau, điểm nào có tọa độ y nhỏ hơn sẽ được chuyển lên trước

-> Tối ưu quá trình xét: nếu distance(điểm a, điểm b) > best_distance, ta break vòng xe ngay lập tức

Trường hợp xấu nhất: 2*100000 điểm thẳng hàng -> thuật toán O(nlogn) + O(n^2/2) vẫn không hiệu quả

# Các phương pháp tiếp cận tốt hơn:

## Thuật toán Chia để trị

Nếu ta chia mặt phẳng ta đang xét thành hai phần có số điểm tương đương nhau, ta thấy cặp điểm gần nhất chỉ có thể nằm ở 1 trong 3 trường hợp:

A. Cặp điểm gần nhất nằm ở bên trái mặt phẳng

B. Cặp điểm gần nhất nằm ở bên phải mặt phẳng

C. Cặp điểm gần nhất nằm ở giữa hai mặt phẳng

![pic1](https://user-images.githubusercontent.com/88699088/135320100-19408b21-4ce0-4fb6-a7f0-8d5cd5c5509c.PNG)

Hình dung nguyên lý trên cũng đúng với mọi phía mặt phẳng: bên trái, bên phải. Ta tiếp tục chia mặt phẳng thành những phần nhỏ hơn, nhỏ đến khi ta đạt đến bài toán gốc: một mặt phẳng chỉ gồm 2 hoặc 3 điểm

> Giải bài toán gốc : Closest Pair trong mặt phẳng có số điểm bằng 2 hoặc 3 có best_distance là bao nhiêu ?
> 
> (Quá dễ. Kiểm tra trong O(1))

![qưeqwe](https://user-images.githubusercontent.com/88699088/135321989-b3b725f4-14f7-46bd-81dd-60f0e8aea6fc.PNG)

Cuối cùng, ta xét trường hợp Closest Pair nằm giữa hai mặt phẳng

Ta chỉ cần xét những bộ điểm có tọa độ x nằm trong khoảng best_distance gần đường thẳng chia hai mặt phẳng

![jkljkl](https://user-images.githubusercontent.com/88699088/135323511-f238f5f1-d1c5-4f89-9d33-f8cae77a9cb3.PNG)
