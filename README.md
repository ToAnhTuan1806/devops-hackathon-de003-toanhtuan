# DevOps Hackathon – Đề 003: Quản lý công việc

## 1. Thông tin sinh viên

| Họ và tên   | Mã sinh viên | Lớp           | Tài khoản Linux     | GitHub        | Cổng Nginx |
| ----------- | ------------ | ------------- | ------------------- | ------------- | ---------- |
| Tô Anh Tuấn | PTIT-HN-100  | HN-KS24-CNTT2 | toanhtuan-ks24cntt2 | ToAnhTuan1806 | 8080       |

## 2. Môi trường triển khai

* Hệ điều hành: Ubuntu 24.04.5 LTS
* Web Server: Nginx
* Quản lý mã nguồn: Git
* Tường lửa: UFW
* Máy chủ: VPS
* Cổng Nginx: 8080
* Địa chỉ IP VPS: 221.121.4.54

## 3. Cấu trúc dự án

```text
devops-hackathon-de003-toanhtuan/
├── src/
│   └── index.html
├── nginx/
│   └── toanhtuan-ks24cntt2.conf
├── screenshots/
├── .gitignore
└── README.md
```

## 4. Thông tin triển khai

### 4.1. Thư mục triển khai

```text
/var/www/devops-hackathon-de003-toanhtuan
```

### 4.2. Thư mục Web Root

```text
/var/www/devops-hackathon-de003-toanhtuan/src
```

### 4.3. Địa chỉ truy cập website

```text
http://221.121.4.54:8080
```

### 4.4. Cấu hình Nginx

File cấu hình:

```text
/etc/nginx/sites-available/toanhtuan-ks24cntt2.conf
```

Nội dung chính:

```nginx
server {
    listen 8080;
    listen [::]:8080;

    server_name 221.121.4.54;

    root /var/www/devops-hackathon-de003-toanhtuan/src;
    index index.html;

    access_log /var/log/nginx/toanhtuan-ks24cntt2.access.log;
    error_log /var/log/nginx/toanhtuan-ks24cntt2.error.log;

    location / {
        allow all;
        try_files $uri $uri/ =404;
    }
}
```

Nginx được kích hoạt thông qua symbolic link:

```text
/etc/nginx/sites-enabled/toanhtuan-hn-ks24-cntt2.conf
```

Đồng thời đã loại bỏ cấu hình Nginx mặc định để tránh xung đột.

## 5. Cấu hình UFW

UFW được sử dụng để kiểm soát kết nối đến VPS.

Các cổng cần thiết:

```text
22/tcp      SSH
8080/tcp    Nginx Web Server
```

Kiểm tra trạng thái UFW:

```bash
sudo ufw status verbose
```

Kiểm tra danh sách rule:

```bash
sudo ufw status numbered
```

## 6. Phân quyền thư mục

Thư mục được thiết lập quyền:

```text
Directory: 755
File:      644
```

Lệnh sử dụng:

```bash
sudo find /var/www/devops-hackathon-de003-toanhtuan -type d -exec chmod 755 {} \;
```

```bash
sudo find /var/www/devops-hackathon-de003-toanhtuan -type f -exec chmod 644 {} \;
```toanhtuan-ks24cntt2

Thiết lập chủ sở hữu:

```bash
sudo chown -R toanhtuan-ks24cntt2:toanhtuan-ks24cntt2 /var/www/devops-hackathon-de003-toanhtuan
```

Không sử dụng quyền `777`.

## 7. Các lệnh triển khai

### 7.1. Clone project từ GitHub

```bash
cd /var/www
sudo git clone https://github.com/ToAnhTuan1806/devops-hackathon-de003-toanhtuan.git
```

### 7.2. Phân quyền project

```bash
sudo chown -R toanhtuan-ks24cntt2:toanhtuan-ks24cntt2 /var/www/devops-hackathon-de003-toanhtuan
```

### 7.3. Kiểm tra cấu hình Nginx

```bash
sudo nginx -t
```

### 7.4. Reload Nginx

```bash
sudo systemctl reload nginx
```

### 7.5. Kiểm tra Nginx

```bash
sudo systemctl status nginx
```

### 7.6. Kiểm tra cổng 8080

```bash
sudo ss -lntp | grep 8080
```

### 7.7. Kiểm tra website trên VPS

```bash
curl -I http://127.0.0.1:8080
```

Kiểm tra bằng địa chỉ IP:

```bash
curl -I http://221.121.4.54:8080
```

## 8. Quy trình Git

Khởi tạo repository:

```bash
git init
```

Đổi tên branch chính:

```bash
git branch -M main
```

Kiểm tra trạng thái:

```bash
git status
```

Thêm file:

```bash
git add .
```

Commit:

```bash
git commit -m "noi dung commit"
```

Đẩy code lên GitHub:

```bash
git push -u origin main
```

Kiểm tra lịch sử commit:

```bash
git log --oneline
```

## 9. Quy trình cập nhật website

Khi cần cập nhật website:

### Bước 1: Sửa file

```text
src/index.html
```

Thêm nội dung cập nhật, ví dụ:

```text
Cập nhật lần 2 - [ngày giờ]
```

### Bước 2: Kiểm tra thay đổi

```bash
git status
```

### Bước 3: Commit thay đổi

```bash
git add src/index.html
```

```bash
git commit -m "feat: cap nhat website lan 2"
```

### Bước 4: Push lên GitHub

```bash
git push
```

### Bước 5: Cập nhật trên VPS

```bash
cd /var/www/devops-hackathon-de003-toanhtuan
```

```bash
git pull
```

Không cần reload Nginx khi chỉ thay đổi file `index.html`.

## 10. Kiểm tra sau khi triển khai

### Kiểm tra Nginx

```bash
sudo nginx -t
```

Kết quả mong đợi:

```text
syntax is ok
test is successful
```

### Kiểm tra trạng thái Nginx

```bash
sudo systemctl status nginx
```

Trạng thái mong đợi:

```text
active (running)
```

### Kiểm tra UFW

```bash
sudo ufw status verbose
```

Cần có rule cho:

```text
22/tcp
8080/tcp
```

### Kiểm tra cổng Nginx

```bash
sudo ss -lntp | grep 8080
```

### Kiểm tra website

```text
http://221.121.4.54:8080
```

## 11. Danh sách ảnh minh chứng

Các ảnh minh chứng được lưu trong thư mục:

```text
screenshots/
```

Danh sách ảnh:

```text
01-user.png
02-nginx.png
03-ufw.png
04-website.png
05-git-log.png
06-update.png
```

### 01-user.png

Minh chứng tài khoản Linux:

```bash
whoami
```

```bash
id
```

### 02-nginx.png

Minh chứng cấu hình và trạng thái Nginx:

```bash
sudo nginx -t
```

```bash
sudo systemctl status nginx
```

### 03-ufw.png

Minh chứng cấu hình UFW:

```bash
sudo ufw status verbose
```

### 04-website.png

Ảnh trình duyệt truy cập:

```text
http://221.121.4.54:8080
```

### 05-git-log.png

Minh chứng lịch sử commit:

```bash
git log --oneline
```

### 06-update.png

Minh chứng website sau khi thực hiện cập nhật lần 2 bằng:

```bash
git pull
```

## 12. Kết quả

Website tĩnh được triển khai thành công trên VPS Ubuntu bằng Nginx.

Các thành phần đã thực hiện:

* Tạo và cấu hình tài khoản Linux.
* Sử dụng Git và GitHub để quản lý mã nguồn.
* Clone project từ GitHub lên VPS.
* Cấu hình Nginx phục vụ website tĩnh.
* Sử dụng cổng 8080 cho Nginx.
* Cấu hình UFW cho SSH và Web Server.
* Thiết lập quyền thư mục và file theo yêu cầu.
* Kiểm tra trạng thái Nginx và kết nối HTTP.
* Thực hiện quy trình cập nhật website bằng Git.
