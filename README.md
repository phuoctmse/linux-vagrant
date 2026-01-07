# Vagrant VMs Project

Dự án này chứa các cấu hình Vagrant để tạo và quản lý máy ảo (Virtual Machines) trên Windows sử dụng Oracle VirtualBox.

## 📦 Các VM có sẵn

| Thư mục | Hệ điều hành | Box | Version |
|---------|--------------|-----|---------|
| `ubuntu/` | Ubuntu 14.04 LTS (Trusty) | ubuntu/trusty64 | 20191107.0.0 |
| `centos/` | CentOS Stream 9 | eurolinux-vagrant/centos-stream-9 | 9.0.48 |

---

## 🛠️ Yêu cầu hệ thống

- **Hệ điều hành**: Windows 10/11
- **RAM**: Tối thiểu 8GB (khuyến nghị 16GB+)
- **Disk**: Tối thiểu 20GB trống
- **CPU**: Hỗ trợ ảo hóa (VT-x/AMD-V) đã bật trong BIOS

---

## 🚀 Cài đặt

### Bước 1: Cài đặt Chocolatey

Chocolatey là trình quản lý gói cho Windows, giúp cài đặt phần mềm dễ dàng qua command line.

1. Mở **PowerShell với quyền Administrator**:
   - Nhấn `Windows + X`, chọn **Windows Terminal (Admin)** hoặc **PowerShell (Admin)**

2. Chạy lệnh sau để cài đặt Chocolatey:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

3. Đóng và mở lại PowerShell (Admin), kiểm tra cài đặt:

```powershell
choco --version
```

---

### Bước 2: Cài đặt Oracle VirtualBox

Chạy lệnh sau trong PowerShell (Admin):

```powershell
choco install virtualbox -y
```

> **Lưu ý**: Sau khi cài đặt, có thể cần khởi động lại máy tính để hoàn tất cài đặt drivers.

Kiểm tra cài đặt:

```powershell
VBoxManage --version
```

---

### Bước 3: Cài đặt Vagrant

Chạy lệnh sau trong PowerShell (Admin):

```powershell
choco install vagrant -y
```

Khởi động lại terminal và kiểm tra cài đặt:

```powershell
vagrant --version
```

---

### Cài đặt tất cả cùng lúc (Tùy chọn)

Bạn có thể cài đặt cả VirtualBox và Vagrant cùng lúc:

```powershell
choco install virtualbox vagrant -y
```

---

## 📖 Hướng dẫn sử dụng

### Khởi động VM

1. Mở terminal và di chuyển đến thư mục VM muốn sử dụng:

```powershell
# Cho Ubuntu
cd d:\vagrant-vms\ubuntu

# Hoặc cho CentOS
cd d:\vagrant-vms\centos
```

2. Khởi động VM:

```powershell
vagrant up
```

> Lần đầu khởi động sẽ tải box image (có thể mất vài phút tùy tốc độ mạng).

---

### Các lệnh Vagrant thường dùng

| Lệnh | Mô tả |
|------|-------|
| `vagrant up` | Khởi động VM |
| `vagrant ssh` | SSH vào VM |
| `vagrant halt` | Tắt VM |
| `vagrant reload` | Khởi động lại VM |
| `vagrant suspend` | Tạm dừng VM (tiết kiệm RAM) |
| `vagrant resume` | Tiếp tục VM đã tạm dừng |
| `vagrant destroy` | Xóa VM hoàn toàn |
| `vagrant status` | Kiểm tra trạng thái VM |
| `vagrant box list` | Liệt kê các box đã tải |

---

### SSH vào VM

Sau khi VM đã khởi động:

```powershell
vagrant ssh
```

Để thoát khỏi SSH session:

```bash
exit
```

---

## ⚙️ Tùy chỉnh VM

Mở file `Vagrantfile` trong thư mục VM để tùy chỉnh:

### Thay đổi RAM và CPU

Bỏ comment và chỉnh sửa phần sau trong `Vagrantfile`:

```ruby
config.vm.provider "virtualbox" do |vb|
  vb.memory = "2048"  # RAM (MB)
  vb.cpus = 2         # Số CPU cores
end
```

### Cấu hình mạng

```ruby
# Private network với IP tĩnh
config.vm.network "private_network", ip: "192.168.33.10"

# Forward port (ví dụ: port 80 của VM -> port 8080 của host)
config.vm.network "forwarded_port", guest: 80, host: 8080
```

### Chia sẻ thư mục

```ruby
# Mount thư mục host vào VM
config.vm.synced_folder "../data", "/vagrant_data"
```

---

## 🔧 Xử lý lỗi thường gặp

### Lỗi: VT-x/AMD-V chưa bật

**Triệu chứng**: VM không khởi động được, báo lỗi virtualization.

**Giải pháp**:
1. Khởi động lại máy tính
2. Vào BIOS/UEFI (thường nhấn F2, F10, DEL khi boot)
3. Tìm và bật tùy chọn:
   - Intel: `Intel Virtualization Technology (VT-x)`
   - AMD: `AMD-V` hoặc `SVM Mode`

### Lỗi: Hyper-V conflict

**Triệu chứng**: VirtualBox không chạy được do Hyper-V đang bật.

**Giải pháp** (chạy với quyền Admin):

```powershell
# Tắt Hyper-V
bcdedit /set hypervisorlaunchtype off

# Khởi động lại máy
shutdown /r /t 0
```

Để bật lại Hyper-V:

```powershell
bcdedit /set hypervisorlaunchtype auto
```

### Lỗi: Box download chậm/timeout

**Giải pháp**: Tải box thủ công:

```powershell
vagrant box add ubuntu/trusty64 --provider virtualbox
```

---

## 📚 Tài liệu tham khảo

- [Vagrant Documentation](https://developer.hashicorp.com/vagrant/docs)
- [VirtualBox Manual](https://www.virtualbox.org/manual/)
- [Chocolatey Packages](https://community.chocolatey.org/packages)
- [Vagrant Cloud Boxes](https://app.vagrantup.com/boxes/search)

---

## 📝 License

MIT License
