# [Trang chủ](https://ppap-1264589.github.io/interesting-solution)

## Bài Toán
Cho n điểm trên mặt phẳng tọa độ. Tìm khoảng cách ngắn nhất giữa 2 điểm bất kỳ.

**Input**

Dòng đầu tiên chứa số nguyên dương n (n ≤ 10^5) là số điểm.

n dòng tiếp, dòng thứ i chứa 2 số nguyên xi, yi (abs(xi), abs(yi) ≤ 10^9) xác định tọa độ điểm thứ i.

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

-> Tối ưu quá trình xét: nếu distance(điểm a, điểm b) > best_distance, ta break vòng lặp ngay lập tức

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

Tuy nhiên ta chỉ cần xét những bộ điểm có tọa độ x nằm trong khoảng best_distance gần *đường chia mặt phẳng*. Bởi những điểm có tọa độ x xa hơn best_distance về hai phía *đường chia mặt phẳng* chắc chắn không thể là cặp điểm nhỏ nhất.

![jkljkl](https://user-images.githubusercontent.com/88699088/135323511-f238f5f1-d1c5-4f89-9d33-f8cae77a9cb3.PNG)

Tới đây, sắp xếp các điểm nằm trong dải hình chữ nhật theo tọa độ y: tạo độ y nào lớn hơn sẽ được duyệt trước. 

Sau đó, xét từng cặp điểm một trong dài hình chữ nhật. Vẫn lưu ý là khi khoảng cách giữa hai điểm lớn hơn best_distance thì break ngay lập tức. Việc sort và duyệt như vậy đảm bảo sẽ không bỏ sót trường hợp cặp điểm tốt hơn best_distance.

![34](https://user-images.githubusercontent.com/88699088/135738025-8a6a95cc-c1a3-48a3-8bc2-e05d647a79ac.png)

Thực tế, việc xét các điểm trong dải Ruy-băng như vậy chính là việc xét một điểm với các điểm nằm trong hình chữ nhật 2h*h (h là best_distance)

Do cả bên trái và bên phải của mặt phẳng, không thể có cặp điểm nào có khoảng cách nhỏ hơn best_distance, nên chúng ta chỉ có thể đặt tối đa 4 điểm vào trong một hình vuông của HCN đang xét:
![closest](https://user-images.githubusercontent.com/88699088/135738314-2c5a1907-efd4-4162-90ab-4a51db815012.PNG)

Như vậy: trong trường hợp xấu nhất, với mỗi điểm, ta chỉ cần duyệt với 3 điểm theo thứ tự đã sort để tìm ra cặp điểm nhỏ nhất. Độ phức tạp O(1)

> Tổng độ phức tạp thuật toán : O(NlogN)

# Code
```c++
#include <bits/stdc++.h>
#define up(i,a,b) for (int i = (int)a; i <= (int)b; i++)
#define pii pair<int, int>
#define x first
#define y second
using namespace std;

const int maxn = 200001;
pii a[maxn];
pii temp[maxn];
pii strip[maxn];
double best = 1e18;

double dist(pii a, pii b){
    double hx = a.x - b.x;
    double hy = a.y - b.y;
    return sqrt(hx*hx + hy*hy);
}

double Basecase(pii P[], int n){
    up(i, 0, n-1) up(j, i+1, n-1){
        double T = dist(P[i], P[j]);
        best = min(best, T);
    }
    return best;
}

bool compy(pii p, pii q) { return (p.y < q.y); }
bool compx(pii p, pii q) { return (p.x < q.x); }

void MergeStrip(pii P[], int n, double d){
    up(i, 0, n-1) for (int j = 1; j <= 3 && i + j < n; j++){
        double T = dist(strip[i], strip[i+j]);
        if (best > T) best = T;
        else break;
    }
}

double ClosestPair(pii P[], int n){
    if (n <= 3){
        sort(P, P+n, compy);
        return Basecase(P, n);
    }

    int mid = n/2;
    pii midP = P[mid];
    double dl = ClosestPair(P, mid);
    double dr = ClosestPair(P+mid, n-mid);
    double d = min(dl, dr);

    merge(P, P+mid, P+mid, P+n, temp, compy);

    int cnt = 0;
    up(i, 0, n-1){
        P[i] = temp[i];
        if (abs(midP.x - P[i].x) < d){
            strip[cnt++] = P[i];
        }
    }
    MergeStrip(strip, cnt, d);
    return best;
}

signed main(){
    int n;
    cin >> n;
    up(i, 0, n-1) cin >> a[i].x >> a[i].y;
    sort(a, a+n, compx);
    double result = ClosestPair(a, n);

    cout << fixed << setprecision(3);
    cout << result;
}
```

## Một số chú ý :

- Khi giải trường hợp gốc, ta sort luôn các điểm theo tọa độ y

    -> Khi gộp các điểm trong hai mặt phẳng về một, dễ dàng dùng thuật sắp xếp trộn hơn (với ĐPT O(n))
