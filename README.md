# Rentix Meter OCR

Hệ thống nhận diện chỉ số công tơ điện bằng công nghệ OCR, được phát triển phục vụ cho dự án Rentix.

## Cấu trúc dự án

```text
Rentix_meter_ocr/
│
├── dataset/
│   ├── raw/                 # Ảnh công tơ điện gốc
│   ├── processed/           # Ảnh sau khi tiền xử lý
│   ├── train/               # Tập dữ liệu huấn luyện
│   ├── val/                 # Tập dữ liệu kiểm định
│   └── test/                # Tập dữ liệu kiểm thử
│
├── labels/
│   ├── train.csv            # Nhãn của tập huấn luyện
│   ├── val.csv              # Nhãn của tập kiểm định
│   └── test.csv             # Nhãn của tập kiểm thử
│
├── notebooks/
│   ├── 01_explore_dataset.ipynb
│   └── 02_test_ocr.ipynb
│
├── src/
│   ├── dataset/
│   │   ├── __init__.py
│   │   ├── loader.py        # Đọc và nạp dataset
│   │   └── preprocessing.py # Tiền xử lý ảnh
│   │
│   ├── models/
│   │   ├── __init__.py
│   │   └── ocr_model.py     # Định nghĩa mô hình OCR
│   │
│   ├── training/
│   │   ├── __init__.py
│   │   └── train.py         # Huấn luyện mô hình
│   │
│   ├── evaluation/
│   │   ├── __init__.py
│   │   └── evaluate.py      # Đánh giá mô hình
│   │
│   └── inference/
│       ├── __init__.py
│       └── predict.py       # Chạy nhận diện OCR
│
├── checkpoints/
│   └── .gitkeep             # Lưu các checkpoint của mô hình
│
├── outputs/
│   ├── predictions/         # Kết quả dự đoán
│   └── metrics/             # Các chỉ số đánh giá
│
├── requirements.txt
├── .gitignore
└── README.md
```

## Quy trình

Hệ thống được xây dựng theo quy trình tổng quát:

```text
Ảnh công tơ điện
       ↓
Tiền xử lý ảnh
       ↓
    Mô hình OCR
       ↓
 Chỉ số điện nhận diện
       ↓
    Đánh giá
```

Mô hình sẽ được huấn luyện và đánh giá trên tập ảnh công tơ điện được thu thập từ nhiều loại công tơ và nhiều điều kiện thực tế khác nhau.

## Dataset

Dataset bao gồm ảnh công tơ điện cùng với thông tin ground truth tương ứng với chỉ số điện hiển thị trên công tơ.

Ví dụ:

```csv
filename,meter_type,reading
meter_0001.jpg,digital_a,012384
meter_0002.jpg,digital_b,001928
meter_0003.jpg,mechanical_a,12384
```

Trong đó:

* `filename`: Tên file ảnh.
* `meter_type`: Loại công tơ điện.
* `reading`: Chỉ số điện thực tế được hiển thị trên công tơ.

Dataset được chia thành ba tập chính:

* **Train**: Sử dụng để huấn luyện mô hình.
* **Validation**: Sử dụng để theo dõi và điều chỉnh mô hình trong quá trình huấn luyện.
* **Test**: Sử dụng để đánh giá hiệu năng cuối cùng của mô hình.

Ảnh gốc được lưu tại `dataset/raw/` và không được chỉnh sửa trực tiếp.

## Các giai đoạn phát triển

### 1. Thu thập dữ liệu

Thu thập ảnh công tơ điện từ nhiều loại và điều kiện khác nhau.

Các yếu tố cần xem xét:

* Loại công tơ.
* Model công tơ.
* Góc chụp.
* Khoảng cách chụp.
* Điều kiện ánh sáng.
* Độ phản sáng trên mặt kính.
* Độ mờ của ảnh.
* Bụi bẩn hoặc tình trạng mặt công tơ.
* Các trường hợp khó đọc.

Mỗi ảnh cần có chỉ số điện thực tế tương ứng để làm ground truth khi có thể.

### 2. Khảo sát dataset

Phân tích dataset sau khi thu thập nhằm xác định:

* Có bao nhiêu loại công tơ.
* Số lượng ảnh của từng loại.
* Định dạng chỉ số điện.
* Độ phân giải ảnh.
* Chất lượng ảnh.
* Mức độ mất cân bằng giữa các loại dữ liệu.
* Các trường hợp khó nhận diện.

Kết quả khảo sát được sử dụng để quyết định phương pháp tiền xử lý và kiến trúc mô hình phù hợp.

### 3. Tiền xử lý dữ liệu

Xây dựng các phương pháp tiền xử lý nhằm cải thiện chất lượng đầu vào cho mô hình OCR.

Một số phương pháp có thể được sử dụng:

* Cắt vùng cần nhận diện.
* Thay đổi kích thước ảnh.
* Chuyển ảnh sang grayscale.
* Điều chỉnh độ tương phản.
* Giảm nhiễu.
* Chuẩn hóa ảnh.

Các phương pháp cụ thể sẽ được xác định dựa trên đặc điểm thực tế của dataset.

### 4. Huấn luyện mô hình

Huấn luyện mô hình OCR trên dataset đã được chuẩn bị.

Kiến trúc mô hình sẽ được lựa chọn sau khi hoàn thành quá trình khảo sát dataset và xác định yêu cầu của bài toán.

### 5. Đánh giá mô hình

Đánh giá khả năng nhận diện chỉ số điện của mô hình bằng các chỉ số phù hợp, dự kiến bao gồm:

* **Character Accuracy**: Độ chính xác ở cấp độ ký tự.
* **Character Error Rate (CER)**: Tỷ lệ lỗi ký tự.
* **Exact Reading Accuracy**: Tỷ lệ nhận diện chính xác toàn bộ chỉ số điện.

Trong đó, **Exact Reading Accuracy** đặc biệt quan trọng đối với việc ứng dụng mô hình vào hệ thống Rentix.

### 6. Inference

Sau khi huấn luyện, mô hình được sử dụng để nhận diện chỉ số điện từ ảnh công tơ.

Quy trình inference:

```text
Ảnh công tơ
    ↓
Tiền xử lý
    ↓
Mô hình OCR
    ↓
Chỉ số điện dự đoán
```

## Tích hợp với Rentix

Mô hình OCR được định hướng tích hợp vào ứng dụng Rentix Mobile để hỗ trợ người thuê tự động đọc chỉ số công tơ điện.

Quy trình dự kiến:

```text
Camera
   ↓
Vùng quét cố định
   ↓
Chụp ảnh tự động
   ↓
Tiền xử lý
   ↓
OCR
   ↓
Kiểm tra kết quả
   ↓
Hiển thị xác nhận
   ↓
Người dùng xác nhận
   ↓
Gửi chỉ số về Rentix Backend
```

Ứng dụng có thể tự động thực hiện nhiều lần nhận diện nếu kết quả OCR chưa đạt yêu cầu.

Chi tiết về
