# 👟 Adidas Sales Analysis — Phân tích hiệu quả kinh doanh

Phân tích dữ liệu bán hàng của Adidas tại thị trường Mỹ giai đoạn 2020–2021,
xác định động lực tăng trưởng doanh thu/lợi nhuận theo khu vực, nhà bán lẻ,
sản phẩm và kênh bán hàng, từ đó đề xuất chiến lược kinh doanh cụ thể theo
từng phân khúc.

---

## ❓ Vấn đề kinh doanh

Trong bối cảnh bán lẻ cạnh tranh, Adidas cần xác định:
1. Khu vực, nhà bán lẻ, sản phẩm và kênh bán hàng nào đang mang lại hiệu quả tốt nhất
2. Doanh thu cao có thực sự đi kèm biên lợi nhuận tốt hay không
3. Chiến lược nào nên áp dụng riêng cho từng khu vực/kênh để tối ưu lợi nhuận

---

## 📊 Dữ liệu

- **Nguồn**: [Kaggle – Adidas Sales Dataset](https://www.kaggle.com/datasets/heemalichaudhari/adidas-sales-dataset)
- **Kích thước gốc**: 9,652 dòng × 14 cột
- **Giai đoạn**: 2020–2021
- **Biến chính**: Retailer, Region, State/City, Product, Price per Unit, Units Sold, Total Sales, Operating Profit, Operating Margin, Sales Method

---

## 🛠️ Quy trình thực hiện

### 1. Làm sạch & xử lý dữ liệu (Python)
- Loại cột rỗng phát sinh khi đọc file Excel, reset lại index
- Kiểm tra outliers bằng boxplot + IQR trên cột `Units Sold` → phát hiện nhóm lệch phải rõ rệt, **không loại bỏ** vì đây có thể là đơn hàng sỉ/khuyến mãi, thay vào đó tạo cột `Outlier_Type` (Normal/High/Low) để giữ lại tín hiệu phân tích
- Tách cột `Product` thành 3 cột riêng: `Gender`, `Category`, `Style` để phân tích đa chiều
- Xuất dữ liệu đã xử lý ra CSV để nạp vào Power BI

### 2. Khám phá dữ liệu & kiểm định thống kê (Python EDA)
- **Random Forest Regressor**: xác định mức ảnh hưởng của các biến (Retailer, Region, Product, Price, Sales Method) đến `Operating Profit` thông qua Feature Importance — loại `Units Sold` khỏi phân tích vì biến này tác động trực tiếp/hiển nhiên đến lợi nhuận, mục tiêu là tìm yếu tố gián tiếp
- **ANOVA (One-way)**: kiểm định sự khác biệt về Operating Margin giữa 3 kênh bán hàng (In-store/Online/Outlet) — kết quả có ý nghĩa thống kê (p-value < 0.05)
- **Correlation Matrix**: kiểm tra mối quan hệ giữa Price, Units Sold, Total Sales, Operating Profit, Operating Margin

### 3. Trực quan hóa & phân tích chuyên sâu (Power BI)
Dashboard gồm **5 trang phân tích**:

| Trang | Nội dung |
|---|---|
| Tổng quan | Xu hướng doanh thu/lợi nhuận theo năm & tháng, top khu vực/nhà bán lẻ/sản phẩm |
| Phân tích Sản phẩm | Cơ cấu doanh thu theo ngành hàng, mối quan hệ giá bán – sản lượng – biên lợi nhuận |
| Phân tích Kênh bán hàng | Hiệu quả In-store / Online / Outlet theo từng nhóm sản phẩm |
| Phân tích Khu vực | Đặc điểm và chiến lược riêng cho 5 khu vực (West, Northeast, Southeast, South, Midwest) |
| Phân tích Nhà bán lẻ | Vai trò chiến lược của từng nhà bán lẻ (Value driver / Volume driver / Clearance) |

---

## 📈 Kết quả & Insight chính

### Tổng quan
- Doanh thu 2021 chiếm **79,77%** cơ cấu 2 năm (2020 chỉ 20,23% do ảnh hưởng Covid-19)
- Doanh thu cao nhất vào mùa hè (tháng 7, 8) nhưng **doanh thu cao không đồng nghĩa biên lợi nhuận cao** — tháng 7 đạt doanh thu kỷ lục nhưng biên lợi nhuận (35,67%) chỉ tương đương tháng 3, tháng bán thấp nhất (35,98%)
- Top khu vực: **West, Northeast, Southeast** | Top nhà bán lẻ: **Foot Locker, Sports Direct, West Gear** | Top sản phẩm: **giày nam, quần áo nữ**

### Phân tích sản phẩm
- Giày dép chiếm **66%** doanh thu, quần áo **33%**
- Phân loại 4 nhóm chiến lược theo ma trận sản lượng × biên lợi nhuận:
  - **Đẩy mạnh quy mô**: Men's Street Footwear, Women's Apparel (bán nhiều + lãi cao)
  - **Duy trì + upsell**: Women's Street Footwear, Men's Apparel
  - **Cần tối ưu giá/chi phí**: Men's & Women's Athletic Footwear (bán nhiều nhưng biên lợi nhuận thấp nhất)
- Tương quan giá–sản lượng dương (~0.25–0.35) ở các sản phẩm bán chạy → **còn dư địa tăng giá** (price testing) mà không lo mất khách

### Phân tích kênh bán hàng
- **In-store/Outlet** vẫn chiếm ưu thế doanh thu nhưng biên lợi nhuận thấp do chi phí vận hành cao
- **Online** đóng góp doanh thu chưa cao nhất nhưng có **biên lợi nhuận cao nhất** → kênh cần đẩy mạnh để tiết kiệm chi phí vận hành

### Phân tích khu vực (insight nổi bật)

| Khu vực | Đặc điểm | Chiến lược đề xuất |
|---|---|---|
| **West** | Doanh thu cao nhất (30%), phụ thuộc lớn vào West Gear | Đa dạng hóa nhà bán lẻ, đẩy mạnh online |
| **Northeast** | Hành vi mua truyền thống, cao điểm Q3–Q4 | Instore flagship, bundle sản phẩm |
| **Southeast** | Dân số trẻ, online phát triển tốt, cao điểm Q2–Q3 | Online-first, flash sale có kiểm soát |
| **South** | Phụ thuộc Outlet, biên lợi nhuận thấp | Dịch chuyển Outlet → Online |
| **Midwest** | Phân bổ nhà bán lẻ đều, còn dư địa online | Test pricing & product mix trước khi scale |

### Phân tích nhà bán lẻ
- **West Gear** (27% doanh thu) và **Foot Locker** (24%) là *Value driver* — giữ giá tốt, lợi nhuận cao
- **Sports Direct** là *Volume driver* — đẩy mạnh số lượng, nhạy cảm giá
- **Amazon, Walmart** đóng vai trò *Distribution/Clearance* — xả hàng tồn, mở rộng độ phủ

---

## 💡 Đề xuất kinh doanh tổng thể

1. **Đẩy mạnh kênh Online** cho các sản phẩm trọng điểm (Men's Street Footwear, Women's Apparel) để tối đa hóa lợi nhuận; dùng Outlet chỉ để xử lý tồn kho
2. **Tận dụng yếu tố mùa vụ theo khu vực**: phân bổ hàng chủ lực và chiến dịch marketing đúng thời điểm cao điểm riêng của từng khu vực (Q1 cho West, Q3–Q4 cho Northeast/South...)
3. **Thử nghiệm tăng giá** (price testing) ở các sản phẩm bán chạy nhất — dữ liệu cho thấy nhu cầu chưa nhạy cảm với giá ở phân khúc này
4. **Giảm rủi ro phụ thuộc nhà bán lẻ**: nhiều khu vực đang phụ thuộc 1 nhà phân phối duy nhất (ví dụ West phụ thuộc West Gear) → cần đa dạng hóa kênh phân phối

---

## 🧰 Công nghệ sử dụng

`Python` `Pandas` `Scikit-learn (Random Forest)` `SciPy (ANOVA)` `Matplotlib/Seaborn` `Power BI (DAX Measures)`

---

## 📁 Cấu trúc Repo

```
├── Adidas.ipynb                    # Notebook: làm sạch dữ liệu, EDA, Random Forest, kiểm định thống kê
├── ADIDAS.pbix                     # File Power BI: dashboard 5 trang phân tích
├── Adidas_Sales_Analysis.pptx      # Slide báo cáo trình bày đầy đủ
└── README.md
```

---

## 🚀 Cách chạy lại project

```bash
git clone https://github.com/Hoibeopro/Adidas_Sales_Analysis.git
cd Adidas_Sales_Analysis
pip install pandas numpy scikit-learn scipy matplotlib seaborn openpyxl
jupyter notebook Adidas.ipynb
```
Mở file `ADIDAS.pbix` bằng Power BI Desktop để xem dashboard tương tác.

---

## 👤 Tác giả

**Mã Thị Hồi** — Data Analyst Trainee
Mentor: Nguyễn Minh Sơn
