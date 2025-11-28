<h1>Thuật toán KNN</h1>
<h2>Nguồn gốc:</h2>
<ul>
<li>Ý tưởng “lấy điểm gần nhất để dự đoán” là 
<b>instance-based / nonparametric</b> — có gốc sớm trong thống kê phi tham số.
</li>
<li>
Một trong những công trình nền tảng thường được trích dẫn là <b>Fix & Hodges (1951)</b> — đề cập tới phương pháp phân loại không tham số.
</li>
<li>
Bài báo quan trọng khác là <b>Cover & Hart (1967)</b>: họ phân tích lý thuyết của <b>nearest neighbor rule</b> (quy tắc 1-NN) và cho kết quả thú vị về sai số (1-NN error ≤ 2 × Bayes error as n → ∞). Vì vậy khi nói “ai tạo ra” thường nhắc tới Fix & Hodges (sớm) và Cover & Hart (lý thuyết hiện đại).
</li>
</ul>

<h2>Ứng dụng thực tế</h2>
KNN rất phổ biến vì đơn giản, dễ triển khai:

<ul>
<li>
<b>Phân loại (classification)</b>: nhận diện chữ viết, phân loại email spam vs not-spam (đơn giản).
</li>

<li>
<b>Hệ gợi ý / Recommendation</b>: tìm user tương tự; item-based collaborative filtering.
</li>


<li>
<b>Tìm kiếm gần nhất (spatial)</b>: tìm địa điểm/quán ăn gần nhất (k = 1 hoặc nhiều).
</li>

<li>
<b>Xử lý ảnh / retrieval</b>: tìm hình ảnh có vector đặc trưng gần giống.
</li>

<li>
<b>Regression (hồi quy k-NN)</b>: dự đoán giá trung bình của k láng giềng.
</li>

<li>
<b>Anomaly detection</b>: điểm có khoảng cách trung bình tới k láng giềng lớn → anomaly.
</li>
</ul>

<h2>Nguyên lý hoạt động (một câu)</h2>
Với một điểm truy vấn, tính khoảng cách đến mọi điểm trong dataset, chọn k điểm nhỏ nhất rồi dùng voting (classification) hoặc trung bình/weighted average (regression) để đưa ra dự đoán.

<h2>
Các thành phần cần biết
</h2>
<ul>
<li>
<b>Khoảng cách</b>: Euclidean, Manhattan, Cosine similarity (đặc biệt với embedding), Mahalanobis...
</li>

<li>
<b>Normalization</b>: chuẩn hóa features trước khi tính khoảng cách (standardization hoặc min-max).
</li>

<li>
<b>Chọn k</b>: nhỏ → nhiễu; lớn → mờ nhãn. Thường dùng cross-validation để chọn.
</li>

<li>
<b>Tie-break</b>: nếu vote hòa, có thể lấy nhãn của điểm gần nhất hoặc giảm/increase k.
</li>

<li>
<b>Weighted KNN</b>: cho trọng số theo khoảng cách (ví dụ weight = 1 / (d + ε)).
</li>

<li>
<b>Curse of dimensionality</b>: khi số chiều lớn, khoảng cách trở nên kém phân biệt — cần PCA/embedding/approx methods.
</li>

<li>
<b>Tối ưu</b>: KD-Tree, Ball Tree, VP-Tree, LSH, ANN libraries để giảm thời gian tìm kNN.
</li>
</ul>

<h2>Mã giả (pseudocode)</h2>
<p>KNN (classification) — pseudocode</p>

```c#
    function KNN_Classify(dataset, target, k, distance):
    // dataset: list of (features, label)
    // target: features to classify
    // distance: function to compute distance between two feature vectors 

        // 1. Compute distances
        distances = []
        for each (x_i, label_i) in dataset:
            d = distance(x_i, target)
            append (d, label_i) to distances

        // 2. Sort by distance ascending
        sort distances by d ascending

        // 3. Take k nearest
        neighbors = first k elements of distances

        // 4. Voting
        votes = map from label to count (or weight)
        for each (d, label) in neighbors:
            weight = 1            // for unweighted
            // or weight = 1 / (d + epsilon) for weighted
            votes[label] += weight

        // 5. Return label with highest votes
    return label with max votes
```
<p>KNN (regression)</p>

```c++
    function KNN_Regression(dataset, target, k, distance):
        distances = []
        for each (x_i, y_i) in dataset:
            d = distance(x_i, target)
            append (d, y_i) to distances

        // sort distances ascending
        neighbors = first k elements

        // unweighted mean
        y_pred = average(y_i for (d, y_i) in neighbors)

        // or weighted mean by 1/(d+eps)
        return y_pred
```

<h2>Ví dụ minh họa từng bước (2D, k = 3)</h2>
<p>Dataset (features + label):</p>
<ul>
<li>
A: (1,1), label = Red
</li>

<li>
B: (2,2), label = Red
</li>
<li>
C: (4,4), label = Blue
</li>
<li>
D: (5,5), label = Blue
</li>
</ul>
&nbsp;&nbsp;&nbsp;Target T = (3,3), k = 3
