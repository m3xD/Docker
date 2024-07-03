# Docker

## 1. Image và container:
- Image là một gói phần mềm bao gồm các file cấu hình, biến môi trường,... để chạy chương trình.
- Container là chương trình chạy của image.
- Các command liên quan:
  * Kiểm tra các images hiện tại: `docker images -a`
  * Tải một image: `docker pull name:tag`
  * Kiểm tra các containers đang chạy: `docker ps`
  * Liệt kê tất cả các containers (bao gồm các containers đang offline): `docker container ls -all`
  * Để tạo và chạy một container: `docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`
    * Options thường gặp: -it -rm
  * Để vào terminal của container: `docker container attach containerid`
  * Chạy một container đang dừng: `docker container start -i containerid`
  * Xoá bỏ hẳn một container: `docker container rm containerid`

## 2. Cập nhật, lưu, nạp image từ file trong Docker:
### 2.1 Lưu container thành image:
- Khi sử dụng container, ta thường thực hiện các cấu hình khác nhau.
  -> Nhu cầu muốn giữ thiết lập này
- Khi đó ta sẽ lưu container thành image với câu lệnh: `docker commit mycontainer myimage:version`
### 2.2 Lưu, load image:
- Khi muốn xuất image ra file ta sử dụng với câu lệnh: `docker save --output myimage.tar myimage`
- Để nạp một image qua file ta sử dụng câu lệnh: `docker load -i myimage.tar`
- Để thay đổi tên image ta sử dụng: `docker tag image_id imagename:version`

## 3. Chia sẽ dữ liệu với host hoặc với các containers:
### 3.1 Chia sẻ dữ liệu với host:
- Sử dụng câu lệnh: `docker run -it -v path_host:path_container name_container`
- Khi container được chạy thì đường dẫn `path_host` sẽ được mount vào đường dẫn `path_container`.
### 3.2 Chia sẻ dữ liệu giữa các containers:
- Sử dụng câu lệnh: `docker run -it --volumes-from container_first name_container`
- Với điều kiện `container_first` đã được mount với host từ trước.
### 3.3 Quản lý các ổ đĩa với docker volume:
- Liệt kê các ổ đĩa: `docker volume ls`
- Tạo một ổ đĩa: `docker volume create name_volume`
- Xem thông tin chi tiết của đĩa: `docker volume inspect name_volume`
- Xoá một ổ đĩa: `docker volume inspect name_volume`
- Mount ổ đĩa vào container: `docker run -it --mount source=name_volume,target=path_container name_container`
- Gán ổ đĩa được bind ở path host vào container khi tạo container: `docker volume create --opt device=path_in_host --opt type=none --opt o=bind  volumename` và `docker run -it -v mydisk:/home/sites name_container`
- Xoá các ổ đĩa không được sử dụng bởi containers: `docker volume prune`
- 
