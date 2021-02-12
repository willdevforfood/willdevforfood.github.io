---
title: Median Of Two Sorted Arrays
author: Vu Dang
# avatar: /img/author.jpg
authorlink: https://github.com/giavudangle
# cover: /img/cover.jpg
categories:
  - Tutorial
tags:
  - Algo
  - leetcode
  - optimize
draft: false
---

Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.

Cho 2 mảng đã được sắp xếp lần lượt là nums1 (kích thước m) và nums2 (kích thước n).Trả về kết quả là trung vị của 2 mảng đã được sắp xếp

<!--more-->

![" Median Of Two Sorted Arrays"](/assets/median.jpg "Median Of Two Sorted Arrays")

**Điều kiện :**
Độ phức tạp toàn bộ quá trình runtime phải là phải là **O(log (m+n)).** (Khó ở phần optimize được **O(log (m+n))** như đề yêu cầu ).

**Ràng buộc của bài toán :**
- nums1.length == m
- nums2.length == n
- 0 <= m <= 1000
- 0 <= n <= 1000
- 1 <= m + n <= 2000
- -10<sup>6</sup> <= nums1[i], nums2[i] <= 10<sup>6</sup>


## Ví dụ
Ví dụ 1 :
```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

Ví dụ 2 :
```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

## Phân tích bài toán
Mới nhìn vào ,đọc lướt qua đề bài ta thấy vấn đề vô cùng dơn giản :v . Ta sẽ nghĩ kiểu : Merge 2 mảng lại thì độ dài của mảng sau khi merge là `m + n` .

Ok thôi, như vậy sẽ có hai trường hợp để tính toán trung vị (median).Tạm gọi kết quả sau khi merge 2 mảng lại là `res`
Vậy thì ta sẽ tính được :
- TH1 : Nếu như (m + n) là số lẻ, thì median của chúng ta là `res[(m + n + 1) / 2]`.(Lẻ thì chỉ cần lấy chính xác tại vị trí này)
- TH2 : Nếu như (m + n) là số chẵn, thì median của chúng ta là `(result[(m + n) / 2] + result[(m + n) / 2 + 1]) / 2`.(Nhưng với chẵn ta phải lấy cả 2 vị trí sau đó lấy trung bình cộng)

Mảng đã được sắp xếp, nên chúng ta không cần quan tâm về vấn đề đã sort hay chưa.Vậy đơn giản chúng ta chỉ cần MERGE 2 mảng lại <a href='https://en.wikipedia.org/wiki/Merge_sort'>Merge Sort</a> xong tính median như cách trên là xong rồi =))).

😹 Ề ế ê :v ? Nếu như vấn đề đơn giản vậy thì tại sao nó lại được đánh là tag <b><span style="color:red">HARD</span></b>

🤔 Đôi lúc chúng ta hay ngáo bài chỗ đó =))) . OK. Quay lại đọc kĩ Time Complexity nào.
=> **O(log (m+n)).**
Ta phải giải nó sao cho chỉ cắn trong phạm vi **O(log (m+n))** thôi =)))).Vấn đề ở đây đó.Nếu dùng hàm merge thì độ phức tạp (time complexity) đã là **O(m+n)** rồi.
=> Còn gì nữa đâu mà khóc với sầu khi suy nghĩ theo hướng merge 2 thằng lại =))). (Bỏ đi nha)

## Hướng tiếp cận
Đầu tiên phải đọc kĩ đề đã,đọc nhiều lần lên mới thông não được. Và leetcode cũng có phần `hint` để mình mò đó =)))

Cùng nhìn xíu nào 😤 .
- Mảng đã được sắp xếp
- Độ phức tạp runtime **O(log (m+n))**

Sau một hồi suy diễn thì chắc chắn phải dùng 😜 Binary Search (Tìm kiếm nhị phân rồi ) =)))

Nảy sinh tiếp một vấn đề.Áp dụng Binary Search vào trường hợp này như thế nào đây ? Vấn đề hiện tại của chúng ta, là tìm vị trí của cả 2 mảng mà `phần tử bên trái` `<=` `phần tử bên phải` . Và điều này phải đúng với mảng đã được merge.

Nếu chưa hiểu thì cũng không sao :v . Đọc lại đề và ví dụ lần nữa nào, khi ta giải theo ví dụ ta sẽ hiểu được vấn đề thôi :D . Okay cùng xem ví dụ bên dưới nhé.
**Ví dụ**
```
    A = [3, 5, 16, 17, 19, 24] => m = 6
    B = [4, 6, 8, 10, 13, 17, 20] => n = 7
    Khi ta merge lại được mảng Merged
    Merged = [3, 4, 5, 6, 8, 10, 13, 16, 17, 17, 19, 20, 24] => m + n  = 6 + 7 = 13
    Do đó, trung vị của ta sẽ là (i(13) + i(1))/2 = i(7) có giá trị là 13.
    (Với i là chỉ số của mảng)
```
Điều này chỉ thực hiện được khi ta chia 2 mảng A(i(2)) và B(i(3)) như sau :
| Trái        | Phải |
| ----------- | ----------- |
| A = 3, 5, 16     | A =  17, 19, 24       |
| B = 4, 6, 8, 10  | B =  13, 17, 20       |

Như vậy ta sẽ áp dụng tìm kiếm nhị phân để tìm các chỉ số này,
nếu như ta chia mảng và tìm được ngay được trung vị.
Vậy ta phải làm sao ? Các bước như sau :
***
1. Kiểm tra xem mảng nào có độ dài là nhỏ nhất trong 2 mảng.Ta sẽ áp dụng Binary Search trên mảng nhỏ hơn.
2. <a href='https://www.geeksforgeeks.org/binary-search/'>Binary Search</a> của chúng ta sẽ có 2 pointers. Là ``start = 0`` và ``end = m`` (với m lầ độ dài của mảng nhỏ nhất trong 2 mảng).
3. Ta loop khi nào ``left <= right`` hay ``start <= end`` vẫn làm.
4. Tiến hành tính chỉ số mid của A (partA) là điểm ở giữa của nó :
``(start + end) / 2``
5. Tiến hành tính chỉ số mid của B (parB) là điểm ở giữa của nó :
``(m + n + 1) / 2 - partitionA``
6. Tính maxLeftA,minRightA,maxLeftB,minRightB
    - maxLeftA : phần tử lớn nhất bên trái của A
    - minRightA : phần tử nhỏ nhất bên phải của A
    - maxLeftB : phần tử lớn nhất bên trái của B
    - minRightB : phần tử nhỏ nhất bên phải của B
7. Okay giờ ta sẽ rơi vào 3 trường hợp như sau :
    * If (maxLeftA <= minRightB && maxLeftB <= minRightA)
    => Ta đã tìm đúng vị trí của median. Ta trả về median dựa vào ``m + n`` là chẵn hay lẻ như phân tích trên .
    * Else If (maxLeftA > minRightB)
    => Vị trí median của ta đang ở xa phía bên phải và ta cần thu hẹp khoảng cách tìm kiếm cho nó gần về phía trái, do đó ``end = partA - 1``
    * Else
    =>  Vị trí median của ta đang ở xa phía bên trái và ta cần thu hẹp khoảng cách tìm kiếm cho nó gần về phía phải, do đó ``end = partA + 1``

**=> Đó chính là cách chúng ta áp dụng Binary Search để giải bài Median này ^^.**

## Code

### Javascript
```js
var findMedianSortedArrays = function (nums1, nums2) {
    // Kiểm tra nums1.length có lớn hơn nums2.length không ?
    // Nếu lớn hơn thì ta đệ quy lại hàm mới đồng thời thay đổi tham số :D
    if (nums1.length > nums2.length) {
        return findMedianSortedArrays(nums2, nums1);
    }

    // Lấy length tiện cho việc tính toán
    const m = nums1.length;
    const n = nums2.length;
    // 2 con trỏ start và end.
    let start = 0;
    let end = m;

    // Tiến hành BS
    while (start <= end) {
        // Chia mảng thành 2 part
        let partA = Math.floor((start + end) / 2);
        let partB = Math.floor((m + n + 1) / 2) - partA;

        // Tiến hành validate một số case
        let maxLeftA = partA == 0 ? Number.MIN_SAFE_INTEGER : nums1[partA - 1];
        let minRightA = partA == m ? Number.MAX_SAFE_INTEGER : nums1[partA];
        let maxLeftB = partB == 0 ? Number.MIN_SAFE_INTEGER : nums2[partB - 1];
        let minRightB = partB == n ? Number.MAX_SAFE_INTEGER : nums2[partB];

        // 3 trường hợp đã đề cập ở trong bài
        if (maxLeftA <= minRightB && maxLeftB <= minRightA) {
            // Kiểm tra chẵn lẻ độ dài tổng để tính median
            if ((m + n) % 2 == 0) {
                return (Math.max(maxLeftA, maxLeftB) + Math.min(minRightA, minRightB)) / 2.0;
            } else {
                return Math.max(maxLeftA, maxLeftB);
            }
        }
        // Nằm về phải quá thì thu hẹp khoảng cách về trái
        else if (maxLeftA > minRightB) {
            end = partA - 1;
        }
        // Nằm về trái quá thì thu hẹp khoảng cách về phải
        else {
            start = partA + 1;
        }
    }
};
```

### C++

```c++
class Solution {
public:
    double findMedianSortedArrays(std::vector<int>& A, std::vector<int>& B) {

    // Lấy length tiện cho việc tính toán
    int m = A.size();
    int n = B.size();

    const int INF = (int)1e9;

    // Kiểm tra nums1.length có lớn hơn nums2.length không ?
    if (m > n) {
        int temp = m;
        m = n;
        n = temp;

        std::vector<int> tempNums = A;
        A = B;
        B = tempNums;
    }

    // 2 con trỏ start và end.
    int start = 0; int end = m;

    // Tiến hành BS
    while (start <= end) {
        int partA = (start + end) / 2;
        int partB = ((m + n + 1) / 2) - partA;


        int maxLeftA = partA == 0 ? -INF : A[partA - 1];
        int minRightA = partA == m ? INF : A[partA];
        int maxLeftB = partB == 0 ? -INF : B[partB - 1];
        int minRightB = partB == n ? INF : B[partB];

       // 3 trường hợp đã đề cập ở trong bài
        if (maxLeftA <= minRightB && maxLeftB <= minRightA) {
            // Kiểm tra chẵn lẻ độ dài tổng để tính median
            if ((m + n) % 2 == 0) {
                return (max(maxLeftA, maxLeftB) + min(minRightA, minRightB)) / 2.0;
            } else {
                return max(maxLeftA, maxLeftB);
            }
        }
        // Nằm về phải quá thì thu hẹp khoảng cách về trái
        else if (maxLeftA > minRightB) {
            end = partA - 1;
        }
        // Nằm về trái quá thì thu hẹp khoảng cách về phải
        else {
            start = partA + 1;
        }
    }

    return 0.0;
    }
};
```

### C#
```csharp
public class Solution {
    public double FindMedianSortedArrays(int[] nums1, int[] nums2) {
         // Kiểm tra nums1.length có lớn hơn nums2.length không ?
        // Nếu lớn hơn thì ta đệ quy lại hàm mới đồng thời thay đổi tham số :D
        if (nums1.Length > nums2.Length) {
            return FindMedianSortedArrays(nums2, nums1);
        }

        // Lấy length tiện cho việc tính toán
        int m = nums1.Length;
        int n = nums2.Length;

        // 2 con trỏ start và end.
        int start = 0;
        int end = m;

        // Tiến hành BS
        while (start <= end) {
            // Chia mảng thành 2 part
            int partA = (start + end) / 2;
            int partB = ((m + n + 1) / 2) - partA;

            // Tiến hành validate một số case
            int maxLeftA = partA == 0 ? Int32.MinValue : nums1[partA - 1];
            int minRightA = partA == m ? Int32.MaxValue : nums1[partA];
            int maxLeftB = partB == 0 ? Int32.MinValue : nums2[partB - 1];
            int minRightB = partB == n ? Int32.MaxValue : nums2[partB];

            // 3 trường hợp đã đề cập ở trong bài
            if (maxLeftA <= minRightB && maxLeftB <= minRightA) {
                // Kiểm tra chẵn lẻ độ dài tổng để tính median
                if ((m + n) % 2 == 0) {
                    return (Math.Max(maxLeftA, maxLeftB) + Math.Min(minRightA, minRightB)) / 2.0;
                } else {
                    return Math.Max(maxLeftA, maxLeftB);
                }
            }
            // Nằm về phải quá thì thu hẹp khoảng cách về trái
            else if (maxLeftA > minRightB) {
                end = partA - 1;
            }
            // Nằm về trái quá thì thu hẹp khoảng cách về phải
            else {
                start = partA + 1;
            }
        }
        return 0.0;
    }
}
```

## Runtime : Faster than 99.97% JS Submissions
![" Median Of Two Sorted Arrays"](/assets/runtime.png "Median Of Two Sorted Arrays")

## Kết luận
Otoke :v Vậy là chúng ta đã giải được một bài tag <b><span style="color:red">HARD</span></b>.
Qua bài này, chúng ta đã hình dung cách tiếp cận bài toán cũng như áp dụng Binary Search để tìm Median.
Nếu bạn thấy bài viết có ít,đừng ngần ngại chia sẽ blog của tụi mình nhé.
Cảm ơn đã đọc bài của mình . 😄
Happy coding <3
