# Bài 2: Cấu hình phân quyền Nhóm và sudoers bằng visudo


## 1. Đăng nhập VPS với tài khoản root
```bash
ssh root@<IP>
```

## 2. Tạo nhóm và người dùng mới
```bash
sudo groupadd devops-admin

sudo adduser deployer

sudo usermod -aG devops-admin deployer
```

## 3. Mở file cấu hình sudoers an toàn bằng visudo:
```bash
sudo visudo
```

## 4. Thêm quy tắc phân quyền cho nhóm devops-admin vào cuối file
- Kéo xuống dưới cùng/phím mũi tên xuống cuối cùng:
- Thêm quy tắc cấu hình:
```bash
%devops-admin ALL=(ALL) NOPASSWD: /usr/bin/systemctl start *, /usr/bin/systemctl stop *, /usr/bin/systemctl restart *, /usr/bin/systemctl status *
```

## 5. Kiểm tra
- Chuyển sang tài khoản deployer:
```bash
su - deployer
```

- Kiểm tra các lệnh đặc quyền sudo được phép thực thi:
```bash
sudo -l 
```

- Chạy thử lệnh khởi động lại dịch vụ hệ thống:
```bash
sudo systemctl restart cron 
```


## Kết quả:
```bash
ubuntu@ip-172-31-42-200:~$ sudo groupadd devops-admin
ubuntu@ip-172-31-42-200:~$ sudo adduser deployer
New password:
Retype new password:
Sorry, passwords do not match.
passwd: Authentication token manipulation error
passwd: password unchanged
warn: `/bin/passwd deployer' failed with status 10. Continuing.
warn: wrong password given or password retyped incorrectly
Try again? [y/N] y
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for deployer
Enter the new value, or press ENTER for the default
        Full Name []:
        Room Number []:
        Work Phone []:
        Home Phone []:
        Other []:
Is the information correct? [Y/n] y
ubuntu@ip-172-31-42-200:~$ sudo usermod -aG devops-admin deployer
ubuntu@ip-172-31-42-200:~$ sudo visudo
ubuntu@ip-172-31-42-200:~$ su - deployer
Password:
deployer@ip-172-31-42-200:~$ sudo -l
User deployer may run the following commands on ip-172-31-42-200:
    (ALL) NOPASSWD: /usr/bin/systemctl start *, /usr/bin/systemctl stop *, /usr/bin/systemctl restart *, /usr/bin/systemctl status *
deployer@ip-172-31-42-200:~$ sudo systemctl restart cron
```
