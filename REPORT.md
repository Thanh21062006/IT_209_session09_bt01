# Báo cáo Bài 1: Kiểm tra cài đặt Docker Engine và chạy Container Hello-World

## 1. Mục tiêu
- Kiểm tra trạng thái hoạt động của Docker Engine và Docker CLI trên hệ thống.
- Cấu hình phân quyền truy cập Docker daemon mà không cần dùng tiền tố `sudo`.
- Chạy các lệnh kiểm tra phiên bản, cấu hình và khởi chạy container đầu tiên (`hello-world`).

## 2. Các bước thực hiện và lệnh đã sử dụng

### Bước 1: Cấu hình phân quyền cho Docker Daemon (Không dùng `sudo`)
Nếu gặp lỗi `permission denied while trying to connect to the docker API at unix:///var/run/docker.sock`, thực hiện thêm user hiện tại vào group `docker`:
```bash
sudo usermod -aG docker $USER
newgrp docker
```
*(Trên hệ thống Ubuntu Snap: đảm bảo nhóm `docker` được tạo và socket `/var/run/docker.sock` được phân quyền cho nhóm `docker`).*

### Bước 2: Kiểm tra phiên bản Docker Client và Server
```bash
docker version
```

### Bước 3: Xem thông tin chi tiết hệ thống Docker
```bash
docker info
```

### Bước 4: Khởi chạy container kiểm tra Hello-World
```bash
docker run hello-world
```

---

## 3. Nhật ký kết quả đầu ra thực tế từ Terminal

### 3.1. Kết quả lệnh `docker version`
```text
Client:
 Version:           29.8.0
 API version:       1.56
 Go version:        go1.26.7
 Git commit:        88096ef
 Built:             Fri Sep 11 09:11:15 2026
 OS/Arch:           linux/amd64
 Context:           default

Server:
 Engine:
  Version:          29.8.0
  API version:      1.56 (minimum version 1.40)
  Go version:       go1.26.7
  Git commit:       3ce5872
  Built:            Fri Sep 11 09:11:43 2026
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v2.3.4
  GitCommit:        db8809540e1a7a9da5d518876894933ff55692ab
 runc:
  Version:          1.5.1
  GitCommit:        
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

### 3.2. Kết quả lệnh `docker info`
```text
Client:
 Version:    29.8.0
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.37.0
    Path:     /usr/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v5.5.1
    Path:     /usr/libexec/docker/cli-plugins/docker-compose

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 2
 Server Version: 29.8.0
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: json-file
 Cgroup Driver: systemd
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: db8809540e1a7a9da5d518876894933ff55692ab
 runc version: 
 init version: de40ad0
 Security Options:
  apparmor
   Profile: default
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.8.0-146-generic
 Operating System: Ubuntu Core 24
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 3.777GiB
 Name: 1037257148523321
 ID: 485b3784-f0c0-4cdc-a391-be26e105d60b
 Docker Root Dir: /var/snap/docker/common/var-lib-docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
 Firewall Backend: iptables
  EnableUserlandProxy: true
  UserlandProxyPath: /snap/docker/3613/bin/docker-proxy
```

### 3.3. Kết quả lệnh `docker run hello-world`
```text
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pulling fs layer
4f55086f7dd0: Download complete
4f55086f7dd0: Pull complete
d5e71e642bf5: Download complete
Digest: sha256:5e23090353324d887c48ad5e5c56d294eab81588df9605b07d1afe895f9cc8f8
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

---

## 4. Phân tích nguyên lý hoạt động
Khi chạy lệnh `docker run hello-world`:
1. **Kiểm tra Local Cache:** Docker CLI gửi yêu cầu tới Docker Daemon kiểm tra xem image `hello-world:latest` đã có ở local hay chưa.
2. **Kéo Image từ Docker Hub Registry:** Do image chưa tồn tại trên máy (`Unable to find image ... locally`), daemon tự động kết nối tới Docker Hub Registry và kéo các layer của image về.
3. **Tạo và khởi chạy Container:** Daemon tạo một container mới từ image vừa kéo và thực thi binary hiển thị thông báo.
4. **Trả kết quả:** Luồng output được daemon chuyển tiếp về stdout của terminal người dùng và container kết thúc tiến trình thực thi.

---

## 5. Hướng dẫn chụp ảnh minh chứng & Placeholder
Để chụp ảnh minh chứng cho bài nộp:
1. Mở cửa sổ Terminal.
2. Chạy lệnh:
   ```bash
   docker run hello-world
   ```
3. Chụp lại toàn bộ màn hình Terminal chứa lệnh và dòng chữ *"Hello from Docker! This message shows that your installation appears to be working correctly."*.
4. Dán ảnh chụp màn hình vào vị trí placeholder bên dưới:

`ImgEx`
