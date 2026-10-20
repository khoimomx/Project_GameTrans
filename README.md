# Hệ thống quản lý giao dịch trong trò chơi

Ứng dụng web quản lý giao dịch tiền tệ ảo và vật phẩm trong trò chơi. Dự án hướng đến việc hỗ trợ người chơi tra cứu tài khoản, đồng thời cung cấp cho nhân viên các chức năng xử lý giao dịch, đối soát tài chính, quản lý vật phẩm và phân tích dữ liệu.

> **Tình trạng dự án:** Hiện tại đây mới là bộ khung backend ban đầu. Các mô-đun trong mã nguồn chưa được triển khai đầy đủ; dự án chưa có điểm khởi chạy FastAPI, API hoạt động, mô hình cơ sở dữ liệu hay quy trình nghiệp vụ hoàn chỉnh. Các chức năng bên dưới là phạm vi dự kiến, không có nghĩa là đã dùng được.

## Mục lục

- [Mục tiêu dự án](#mục-tiêu-dự-án)
- [Vai trò người dùng](#vai-trò-người-dùng)
- [Chức năng dự kiến](#chức-năng-dự-kiến)
- [Công nghệ sử dụng](#công-nghệ-sử-dụng)
- [Cấu trúc thư mục](#cấu-trúc-thư-mục)
- [Yêu cầu môi trường](#yêu-cầu-môi-trường)
- [Cài đặt trên máy cá nhân](#cài-đặt-trên-máy-cá-nhân)
- [Cấu hình](#cấu-hình)
- [Chạy ứng dụng](#chạy-ứng-dụng)
- [Tài liệu API](#tài-liệu-api)
- [Cơ sở dữ liệu](#cơ-sở-dữ-liệu)
- [Bảo mật và tính toàn vẹn dữ liệu](#bảo-mật-và-tính-toàn-vẹn-dữ-liệu)
- [Tình trạng phát triển](#tình-trạng-phát-triển)
- [Đóng góp](#đóng-góp)

## Mục tiêu dự án

Xây dựng một hệ thống ghi nhận và quản lý giao dịch tiền tệ ảo trong trò chơi. Người chơi có thể tra cứu số dư và lịch sử giao dịch của mình. Nhân viên được phân quyền có thể kiểm tra kết quả giao dịch, xử lý giao dịch lỗi, theo dõi doanh thu và quản lý các dữ liệu liên quan.

## Vai trò người dùng

| Vai trò | Quyền sử dụng dự kiến |
| --- | --- |
| Người chơi | Xem số dư tiền ảo, lịch sử giao dịch và chi tiết giao dịch của tài khoản mình. |
| Nhân viên quản lý giao dịch | Tra cứu, kiểm tra và xử lý giao dịch; xử lý giao dịch thất bại và hoàn tiền đủ điều kiện. |
| Nhân viên kế toán/tài chính | Đối soát dữ liệu, theo dõi doanh thu và xuất báo cáo. |
| Nhân viên quản lý vật phẩm | Quản lý thông tin vật phẩm và giá bán bằng tiền ảo. |
| Bộ phận phân tích dữ liệu | Phân tích giao dịch, hành vi sử dụng tiền ảo và xem xét các cảnh báo bất thường. |

Các chức năng nội bộ dự kiến được giới hạn theo vai trò và quyền được cấp.

## Chức năng dự kiến

### Tài khoản và lịch sử giao dịch của người chơi

- Xem số dư tiền ảo hiện tại.
- Xem lịch sử giao dịch.
- Xem chi tiết giao dịch thuộc tài khoản đang đăng nhập.

### Quản lý giao dịch

- Tra cứu giao dịch theo các tiêu chí được hỗ trợ.
- Kiểm tra thông tin và trạng thái giao dịch.
- Cập nhật trạng thái khi thỏa mãn quy tắc chuyển trạng thái.
- Ghi nhận và xử lý giao dịch thất bại.
- Hoàn tiền cho giao dịch đủ điều kiện và ghi lại kết quả xử lý.

### Tài chính và báo cáo

- Đối soát giao dịch nội bộ với dữ liệu thanh toán hoặc nguồn dữ liệu liên quan.
- Tổng hợp doanh thu theo khoảng thời gian hoặc phạm vi được chọn.
- Tạo báo cáo giao dịch phục vụ kiểm tra và lưu trữ.

### Quản lý vật phẩm

- Thêm, sửa, ngừng cung cấp và tra cứu vật phẩm trong trò chơi.
- Thiết lập và cập nhật giá vật phẩm bằng tiền ảo.

### Phân tích dữ liệu

- Phân tích số lượng, giá trị, tần suất và xu hướng giao dịch.
- Phân tích xu hướng sử dụng tiền ảo ở mức phù hợp với quyền truy cập.
- Đánh dấu giao dịch có dấu hiệu bất thường để nhân viên kiểm tra.

Cảnh báo bất thường chỉ là dấu hiệu cần xem xét; tự nó không chứng minh gian lận và không được dùng để tự động hủy giao dịch hoặc đổi trạng thái giao dịch.

### Tiếp nhận dữ liệu từ hệ thống bên ngoài

- Nhận giao dịch hoặc biến động tiền ảo từ hệ thống game.
- Nhận kết quả thanh toán từ cổng thanh toán.
- Kiểm tra dữ liệu nhận được và chỉ cập nhật giao dịch khi dữ liệu hợp lệ, phù hợp với trạng thái hiện tại.

## Công nghệ sử dụng

| Công nghệ | Mục đích |
| --- | --- |
| Python | Ngôn ngữ phát triển backend. |
| FastAPI | Xây dựng API HTTP. |
| Pydantic | Kiểm tra dữ liệu đầu vào và định nghĩa cấu trúc phản hồi. |
| SQLAlchemy | ORM và truy cập dữ liệu dự kiến; hiện chưa có trong `Requirements.txt`. |
| pyodbc | Kết nối SQL Server qua ODBC. |
| PyJWT | Hỗ trợ xác thực bằng JWT. |
| cryptography | Các chức năng mật mã khi cần thiết. |
| python-dotenv | Đọc biến môi trường từ tệp `.env`. |
| Uvicorn | Máy chủ ASGI dùng để chạy ứng dụng FastAPI. |

Các gói Python hiện được khai báo trong [`Requirements.txt`](Requirements.txt). Vì dự án vẫn đang xây dựng, một số công nghệ trên mới là định hướng và chưa được triển khai trong mã nguồn.

## Cấu trúc thư mục

```text
Project_MIS/
├── App/
│   ├── Models.py       # Định nghĩa mô hình cơ sở dữ liệu (dự kiến)
│   ├── Repository.py   # Truy vấn và lưu trữ dữ liệu (dự kiến)
│   ├── Router.py       # Khai báo các tuyến API FastAPI (dự kiến)
│   ├── Schema.py       # Cấu trúc và kiểm tra dữ liệu vào/ra (dự kiến)
│   └── Service.py      # Quy tắc và luồng nghiệp vụ (dự kiến)
├── Core/
│   ├── Config.py       # Cấu hình ứng dụng và cơ sở dữ liệu (dự kiến)
│   └── Dependencies.py # Các phụ thuộc dùng chung và phiên DB (dự kiến)
├── Static/             # CSS và JavaScript giao diện (dự kiến)
├── Templates/          # Mẫu HTML (dự kiến)
├── _env.example        # Biến môi trường mẫu cho SQL Server
├── Requirements.txt    # Danh sách thư viện Python
└── Readme.md
```

## Yêu cầu môi trường

- Python 3.10 trở lên được khuyến nghị; phiên bản tối thiểu chính thức cần được xác nhận khi dự án hoàn thiện.
- Microsoft SQL Server.
- Microsoft ODBC Driver 18 for SQL Server, tương ứng với giá trị `MSSQL_DRIVER` trong tệp môi trường mẫu.
- Git nếu bạn lấy mã nguồn bằng cách clone repository.

## Cài đặt trên máy cá nhân

Thực hiện các lệnh sau tại thư mục gốc của dự án.

### 1. Tạo và kích hoạt môi trường ảo

Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

macOS hoặc Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 2. Cài đặt thư viện

```bash
python -m pip install --upgrade pip
python -m pip install -r Requirements.txt
```

### 3. Tạo cấu hình môi trường

Sao chép `_env.example` thành `.env` tại thư mục gốc của dự án, sau đó thay các giá trị mẫu bằng thông tin môi trường local. Xem phần [Cấu hình](#cấu-hình).

## Cấu hình

Tệp `_env.example` hiện khai báo các biến SQL Server sau:

| Biến | Ý nghĩa | Giá trị mẫu trong tệp |
| --- | --- | --- |
| `MSSQL_DRIVER` | Tên ODBC driver đã cài đặt. | `ODBC Driver 18 for SQL Server` |
| `MSSQL_SERVER` | Máy chủ SQL Server và cổng tùy chọn. | `localhost,1433` |
| `MSSQL_DATABASE` | Tên cơ sở dữ liệu. | `FGameDB` |
| `MSSQL_USER` | Tên đăng nhập cơ sở dữ liệu. | `sa` |
| `MSSQL_PASSWORD` | Mật khẩu cơ sở dữ liệu. | `123` |
| `MSSQL_ENCRYPT` | Bật hoặc tắt mã hóa kết nối ODBC. | `yes` |
| `MSSQL_TRUST_SERVER_CERTIFICATE` | Có tin cậy chứng chỉ máy chủ hay không. | `yes` |

Các giá trị trong `_env.example` chỉ để minh họa, không dùng làm thông tin xác thực production. Không đưa `.env` lên Git. Với môi trường triển khai, hãy cấu hình secrets qua biến môi trường hoặc hệ thống quản lý bí mật. Hiện trình nạp cấu hình chưa được triển khai, nên chỉ tạo `.env` chưa đủ để cấu hình một ứng dụng đang chạy.

## Chạy ứng dụng

Repository hiện chưa có điểm khởi chạy (ví dụ `main.py` hoặc `App/main.py`) và chưa khai báo đối tượng ASGI `app`. Vì vậy chưa thể chạy ứng dụng web bằng Uvicorn. Sau khi điểm khởi chạy được triển khai, cập nhật lệnh thực tế tại đây. Lệnh mẫu dự kiến có dạng:

```bash
python -m uvicorn <module>:app --reload
```

Thay `<module>` bằng đường dẫn module thực tế chứa đối tượng FastAPI. Lệnh trên chưa chạy được cho đến khi module đó được tạo.

## Tài liệu API

Sau khi ứng dụng và các tuyến API được triển khai, FastAPI có thể cung cấp giao diện tài liệu tương tác tại `/docs` và `/redoc`. Hiện chưa có endpoint API hoạt động trong bộ khung dự án.

## Cơ sở dữ liệu

Tệp môi trường mẫu hướng đến Microsoft SQL Server. Schema cơ sở dữ liệu, migration, mô hình SQLAlchemy, quản lý kết nối/phiên và dữ liệu khởi tạo chưa được triển khai. Cập nhật hướng dẫn tạo schema và chạy migration khi các thành phần này được bổ sung.

## Bảo mật và tính toàn vẹn dữ liệu

Hệ thống dự kiến cần:

- Xác thực người dùng và kiểm tra quyền của từng thao tác nội bộ theo vai trò.
- Chỉ cho phép người chơi truy cập dữ liệu thuộc tài khoản của họ.
- Kiểm tra yêu cầu và callback từ hệ thống ngoài trước khi cập nhật dữ liệu.
- Chỉ cho phép chuyển trạng thái giao dịch hợp lệ và ngăn xử lý trùng lặp.
- Lưu lịch sử có thể kiểm tra cho các thao tác xử lý giao dịch và hoàn tiền.
- Không xác nhận thanh toán hoặc hoàn tiền thành công nếu chưa có kết quả hợp lệ từ nhà cung cấp.
- Giữ trạng thái giao dịch nhất quán khi ghi cơ sở dữ liệu hoặc thao tác với hệ thống ngoài gặp lỗi.
- Không ghi mật khẩu, token, khóa thanh toán hoặc dữ liệu nhạy cảm vào log.

Đây là yêu cầu sản phẩm, chưa phải cam kết về các biện pháp đã có trong bộ khung hiện tại.

## Tình trạng phát triển

Các tệp hiện có mới là khung ban đầu. Những phần cần triển khai trước khi có thể sử dụng hệ thống gồm:

- Thiết kế schema cơ sở dữ liệu và bổ sung SQLAlchemy vào danh sách thư viện nếu tiếp tục chọn ORM này.
- Xây dựng bộ nạp cấu hình, kết nối cơ sở dữ liệu và migration.
- Tạo điểm khởi chạy FastAPI, router, schema đầu vào/đầu ra, service và repository.
- Triển khai đăng nhập, phân quyền theo vai trò và kiểm tra quyền sở hữu tài khoản.
- Xây dựng các luồng người chơi, giao dịch, tài chính, vật phẩm, phân tích và tiếp nhận dữ liệu ngoài.
- Tạo giao diện nếu ứng dụng sẽ phục vụ trang web từ `Templates/` và `Static/`.
- Bổ sung, chạy và xác minh hướng dẫn cài đặt, hành vi API và triển khai khi các thành phần trên đã có.

## Đóng góp

Khi gửi thay đổi, hãy mô tả mục đích và cập nhật README nếu thay đổi cách cài đặt, biến cấu hình, tuyến API hoặc quy trình được hỗ trợ. Không commit thông tin xác thực hoặc tệp `.env` cá nhân.
