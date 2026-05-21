# FIT4012 Lab 7 - Báo cáo 1 trang: SHA-256

## 1. Mục tiêu / Objective

Mục tiêu của bài thực hành là tìm hiểu cơ chế hoạt động của hàm băm mật mã SHA-256, bao gồm cấu trúc đệm dữ liệu Merkle–Damgård và quá trình nén dữ liệu qua 64 vòng xử lý. Ngoài ra, bài lab còn áp dụng SHA-256 vào các tình huống thực tế như kiểm tra tính toàn vẹn của tệp, xác thực mật khẩu và tăng cường bảo mật bằng kỹ thuật Salted Hashing.
## 2. Cách làm / Approach

Quá trình thực hiện bài thực hành cá nhân bao gồm:
  Phân tích mã nguồn trong sha256_lib.h và structure.h để hiểu các phép toán bit và cơ chế nén dữ liệu của SHA-256.

  Kiểm tra chương trình sha_procedure.cpp để băm chuỗi ký tự và tệp dữ liệu thông qua chế độ --self-test.

  Hoàn thiện file_integrity.cpp để so sánh hash thực tế của tệp với hash mong đợi nhằm phát hiện thay đổi dữ liệu.
  
  Xây dựng password_hash.cpp cho chức năng đăng ký và đăng nhập bằng cơ chế lưu hash SHA-256. 
  
  Phát triển salted_password_hash.cpp bằng cách sinh salt ngẫu nhiên 16 byte hex và lưu theo định dạng salt:hash.
  
  Chuẩn hóa toàn bộ kết quả đầu ra thành [PASS] và [FAIL] để tương thích với hệ thống Autograder.a của các ứng dụng ngoại vi về hai trạng thái `[PASS]` hoặc `[FAIL]` nhằm đáp ứng tuyệt đối điều kiện kiểm thử của hệ thống Autograder.

## 3. Kết quả / Result

Các minh chứng và kết quả thu được từ hệ thống kiểm thử:
Giá trị hash của chuỗi "abc":

SHA-256(abc)=ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad

  File mẫu ban đầu tạo ra hash trùng khớp với giá trị mong đợi.

  Sau khi chỉnh sửa nội dung file, hệ thống trả về [FAIL], chứng tỏ dữ liệu đã bị thay đổi.

  Đăng nhập với mật khẩu đúng trả về [PASS].

  Đăng nhập với mật khẩu sai trả về [FAIL].

  Hai tài khoản sử dụng cùng một mật khẩu nhưng tạo ra hai chuỗi salt:hash khác nhau nhờ salt ngẫu nhiên.

## 4. Kết luận / Conclusion
SHA-256 có khả năng phát hiện thay đổi dữ liệu rất hiệu quả nhờ đặc tính “Avalanche Effect”, tức chỉ cần thay đổi rất nhỏ ở dữ liệu đầu vào thì kết quả hash đầu ra sẽ thay đổi hoàn toàn.

Việc sử dụng salt giúp chống lại các cuộc tấn công Rainbow Table và tránh trường hợp hai người dùng có cùng mật khẩu sẽ có cùng hash trên cơ sở dữ liệu.

Mặc dù SHA-256 phù hợp để kiểm tra tính toàn vẹn dữ liệu, nhưng không nên dùng trực tiếp để lưu mật khẩu trong hệ thống thực tế vì tốc độ tính toán quá nhanh, dễ bị brute-force bằng GPU hoặc ASIC. Các hệ thống hiện đại thường sử dụng các thuật toán chuyên dụng như Argon2id, bcrypt hoặc scrypt để tăng mức độ bảo mật.