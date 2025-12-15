## ĐỒ ÁN CUỐI KỲ - CSC17104: LẬP TRÌNH KHOA HỌC DỮ LIỆU
### TRƯỜNG ĐẠI HỌC KHOA HỌC TỰ NHIÊN - ĐHQG-HCM
#### Khoa Công nghệ Thông tin

**Tên đề tài:** Phân tích Xu hướng Âm nhạc Toàn cầu trên Spotify (2009-2025)  
**Bộ dữ liệu:** Spotify Global Music Dataset 2009-2025

**Danh sách thành viên nhóm:**
1. 23120038 - Lê Hoàng Mỹ Hạ
2. 23120084 - Nguyễn Mạnh Thắng 
---

---

## 1. Project Overview
* Dự án này tập trung phân tích bộ dữ liệu âm nhạc từ Spotify nhằm xây dựng quy trình xử lý dữ liệu và khám phá các yếu tố ảnh hưởng đến xu hướng âm nhạc toàn cầu.

* Mục tiêu chính là giải quyết các thách thức thực tế trong dữ liệu âm nhạc như: dữ liệu thể loại đa nhãn (multi-label genres), dữ liệu bị thiếu (missing values), và sự phân phối lệch chuẩn (skewed distribution) của các chỉ số tương tác cũng như thiết lập mô hình dự đoán độ phổ biến và xu hướng âm nhạc trong tương lai.
## 2. Dataset source and description
* **Nền tảng:** Kaggle
* **URL:** [https://www.kaggle.com/datasets/wardabilal/spotify-global-music-dataset-20092025](https://www.kaggle.com/datasets/wardabilal/spotify-global-music-dataset-20092025)
* **Tác giả:** Warda Bilal
* **Mô tả:** Bộ dữ liệu này bao gồm những bài hát nổi tiếng và kinh điển trên Spotify từ năm 2009 đến 2023 của các ca sĩ nổi tiếng như Taylor Swift, Billie Eilish, Rihanna và nhiều người khác. Nó giúp phân tích những thay đổi dài hạn về phong cách âm nhạc, độ dài bài hát và danh tiếng của nghệ sĩ.
## 3.Research question list
* **Câu hỏi 1:** Nghệ sĩ có mức độ phổ biến cao (`artist_popularity`) hoặc nhiều người theo dõi (`artist_followers`) hơn có xu hướng phát hành các bài hát có độ phổ biến (`track_popularity`) cao hơn hay không?
* **câu hỏi 2:**  Xu hướng sáng tác các thể loại âm nhạc (`artist_genres`) thay đổi như thế nào theo thời gian trên Spotify?
* **Câu hỏi 3:** Chiến lược phát hành nào hiệu quả hơn: Ra mắt bài hát dưới dạng 'Single' hay phát hành trong một 'Album' đầy đủ? Loại hình nào thường mang lại độ phổ biến (Popularity) cao hơn?
* **Câu hỏi 4:** Dự đoán `track_popularity` bằng mô hình học máy
## 4. Methodology & Approach

### 4.1. Data Exploration 
Nhóm đã thực hiện phân tích sâu để hiểu cấu trúc dữ liệu:
* **Missing Value Analysis:** Sử dụng Heatmap để phát hiện sự thiếu hụt nghiêm trọng trong cột `artist_genres`.
* **Distribution Check:** Phát hiện phân phối "Long-tail" (lệch phải) của biến `artist_followers` thông qua Histogram.
* **Correlation Analysis:** Sử dụng ma trận tương quan Pearson để đánh giá mối liên hệ giữa các biến số.

### 4.2. Data Preprocessing Strategy
Đây là phần trọng tâm của dự án với các kỹ thuật xử lý dữ liệu:
1.  **Handling Multi-label Genres:**
    * Vấn đề: Dữ liệu genre dạng chuỗi ký tự hỗn loạn (`"['pop', 'rock']"`).
    * Giải pháp: Xây dựng hàm parser sử dụng `ast.literal_eval`. Áp dụng chiến thuật **Top-N Frequency (N=20)** để giữ lại các thể loại chính, tách biệt nhóm **'Unknown'** (dữ liệu thiếu) và gộp các nhãn hiếm vào nhóm **'Other'**.
2.  **Feature Transformation:**
    * Vấn đề: Lượng người theo dõi (`artist_followers`) dao động quá lớn (từ 0 đến 145 triệu).
    * Giải pháp: Áp dụng **Log Transformation (`np.log1p`)** để chuẩn hóa phân phối, giúp giảm ảnh hưởng của các giá trị ngoại lai (Outliers).
3.  **Encoding:** Chuyển đổi các biến phân loại đã xử lý sang dạng số học (One-Hot Encoding).

---

## 4.3 Key Findings

Dựa trên quá trình phân tích, nhóm đã rút ra một vài insight quan trọng nhất:

* **Insight 1:**
    Dữ liệu `artist_followers` phân phối cực kỳ lệch. Chỉ 1% nghệ sĩ hàng đầu chiếm phần lớn lượng người theo dõi (lên tới 145 triệu), trong khi đa số nghệ sĩ có lượng fan rất thấp. Điều này khẳng định sự cần thiết bắt buộc của việc xử lý Log Transform trước khi đưa vào mô hình dự đoán.

* **Insight 2:**
    Việc phân loại nhạc không đơn giản là 1-1. Một bài hát thường gắn với nhiều thể loại (Multi-label). Việc chỉ giữ lại **Top 20 genres** phổ biến nhất kết hợp với nhóm **'Unknown'** và **'Other'** là chiến lược tối ưu để cân bằng giữa việc giảm chiều dữ liệu (Dimensionality Reduction) và giữ lại thông tin ngữ nghĩa.

* **Insight 3:**
    Có mối tương quan tuyến tính mạnh ($r=0.64$) giữa độ nổi tiếng của ca sĩ (`artist_popularity`) và lượng fan (`artist_followers`). Tuy nhiên, các đặc trưng kỹ thuật của bài hát (như thời lượng, số bài trong album) lại có tương quan rất yếu với độ phổ biến của bài hát (`track_popularity`).
* **Insight 4:**
    Trong tập dữ liệu này, loại hình phát hành là Album mang lại độ phổ biến (Popularity) cho bài hát tốt hơn so với phát hành dạng Single.
* **Insight 5:**
Từ sau năm 2010, số lượng bài hát trên Spotify tăng vọt theo cấp số nhân, phản ánh sự thống trị của nhạc số.Từ sau năm 2010, số lượng bài hát trên Spotify tăng vọt theo cấp số nhân, phản ánh sự thống trị của nhạc số.

 * **Insight thú vị nhất:** 
 Điều bất ngờ nhất là biến mục tiêu `track_popularity` không phụ thuộc mạnh mẽ vào bất kỳ biến đơn lẻ nào. Điều này gợi ý rằng độ hot của một bài hát là kết quả của mô hình phi tuyến tính phức tạp (Non-linear), có thể phụ thuộc vào các yếu tố ngoại cảnh (xu hướng, marketing) mà dataset này chưa bao phủ hết.

---

## 4.4 Khó khăn và thách thức 

* **Dataset Limitations:**
    * **Sampling Bias:** Dữ liệu có dấu hiệu lấy mẫu không trọn vẹn (ví dụ: `max(track_number) = 102` trong khi `max(album_total_tracks) = 181`).
    * **Missing Context:** Thiếu các dữ liệu quan trọng về hành vi người dùng (số lượt stream, skip rate) và dữ liệu âm thanh chi tiết (Audio Features như tempo, key).

* **Analysis Limitations:**
    * Phương pháp gộp nhóm Genre vào 'Other' dù giúp giảm chiều dữ liệu nhưng làm mất đi tính đặc thù của các dòng nhạc ngách (Niche genres).
    * Feature Engineering cho cột `artist_genres`**. Dữ liệu thô rất "bẩn" (dirty data) với nhiều định dạng sai lệch và giá trị rỗng.
* **Solution:** Nhóm đã viết hàm xử lý chuỗi tùy chỉnh (Custom Parser) và thử nghiệm nhiều ngưỡng (Threshold) khác nhau cho việc gộp nhóm 'Other' để tìm ra tỷ lệ cân bằng nhất.
---

## 4.5 Hướng đi trong tương lai

Nếu có thêm thời gian, dự án sẽ được mở rộng theo hướng:

* **Deeper Analysis:**
    * Sử dụng **NLP (Natural Language Processing)** để phân tích tên bài hát và lời bài hát (Lyrics Analysis) để tìm ra mối liên hệ giữa cảm xúc ngôn từ và độ phổ biến.
    * Áp dụng **Clustering (K-Means)** để gom nhóm Genre tự động dựa trên sự tương đồng thay vì tần suất xuất hiện.

* **Advanced Modeling:**
    * Thử nghiệm các mô hình Ensemble (Random Forest, XGBoost, Gradient Boosting) để bắt được các mối quan hệ phi tuyến tính mà mô hình hiện tại có thể bỏ sót.

---
## 7. How to Run
1.  Cài đặt thư viện: `pip install -r requirement.txt`
2.  Đặt file `spotify_dataset.csv` cùng tầng với các file khác.
3.  Chạy Notebook: `Spotify_project.ipynb`
