# Hướng dẫn workspace kho Sileo

Đây là workspace chỉ để duy trì kho gói APT/Sileo của `Datkep92/gateway-packages`. Nội dung repo được giữ theo nguồn upstream; không tự thay đổi sản phẩm hoặc tài liệu kho ngoài yêu cầu cụ thể của người dùng.

## Cấu trúc và quy trình

- `debs/`: gói phát hành, mỗi phiên bản một tệp `<Package>_<Version>_<Architecture>.deb`. Không đổi tên để ghi đè phiên bản đã phát hành; rootless dùng `iphoneos-arm64`, roothide dùng `iphoneos-arm64e`.
- Mỗi gói cần trường `Package`, `Name`, `Version`, `Architecture`, `Description`, `Maintainer`, `Author` và `Section` trong control.
- `depictions/<package id>/`: trang và tài nguyên minh họa tuỳ chọn; `sileo-featured.json` là danh sách banner; `index.html` và `CydiaIcon.png` là trang chủ và biểu tượng nguồn.
- Khi có yêu cầu thêm gói, chỉ dùng gói phát hành do quy trình build của Apelin ghi nhận và quy trình publish được người dùng yêu cầu/xác nhận. Sau khi đẩy thay đổi lên `main`, GitHub Actions chạy `.github/workflows/update-repository.yml`, gọi `scripts/update-repository.sh` để tạo chỉ mục APT và triển khai GitHub Pages.
- Để kiểm tra cú pháp script mà không tạo bản triển khai: `bash -n scripts/update-repository.sh`. Script đầy đủ cần `apt-ftparchive` và `xz`; workflow GitHub cài các công cụ này.
- Không commit các chỉ mục tạo ra (`Packages`, `Packages.xz`, `Release`) hoặc `_site/`.

## An toàn phát hành

- Không tự sao chép, tạo hoặc thay thế `.deb` trong `debs/`; đặc biệt không dùng `dpkg-deb` từ shell để tạo gói cho kho. Chỉ publish gói đã build qua quy trình Apelin, khi người dùng yêu cầu rõ ràng.
- Không tự publish bằng `git_push` hoặc công cụ publish. Push thay đổi kho sẽ làm GitHub Actions triển khai công khai; chỉ thực hiện sau yêu cầu rõ ràng của người dùng.
- Không đăng gói mới, dọn phiên bản cũ hay sửa depiction/banner nếu chưa được yêu cầu.
- Trước khi commit/push thay đổi kho, kiểm tra `git_status` và diff; giữ lịch sử hiện có, không force-push.
