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

## 2. Docker:
### 2.1. Overview:
- Docker là nền tảng phát triển, đóng gói vận chuyển và vận hành ứng dụng.
- Docker cho phép tách rời cấu trúc ứng dụng để có thể dễ dàng vận chuyển
- Tận dụng được lợi thế docker cho phép việc vận chuyển, kiểm tra, phát triển ứng dụng một cách nhanh chóng. Rút ngắn thời gian giữa việc phát triển và đưa lên chạy thực tế.
### 2.2. Docker platform:
- Docker cung cấp khả năng chạy package được gọi là container.
- Có khả năng chạy nhiều containers trên một host.
- Container có đặc điểm là nhẹ, chứa tất cả các dependency để có thể chạy được ứng dụng.
- Các containers có thể được share dễ dàng.
### 2.3. Khả năng của docker:
- Vận chuyển nhanh các ứng dụng.
- Khả năng scaling, chạy được trên nhiều platforms.
- Chạy được nhiều workloads trên cùng một host.
### 2.4. Docker architecture:
- Docker sử dụng kiến trúc client-server. Docker client giao tiếp với Docker daemon để thực hiện build, run, distribute docker container. Docker client và daemon có thể cùng trên một host hoặc có thể remote Docker client để điều khiển docker daemon. Docker client và daemon giao tiếp với nhau sử dụng REST-API, qua UNIX socket hoặc network interface. Một biển thể khác của docker client đó là docker compose giúp ứng dụng chạy trên một tập containers.
- Docker daemon: thực hiện lắng nghe yêu cầu docker API và quản lý các docker objects như images, containers, networks và volumes. Các docker daemon có thể giao tiếp với nhau để quản lý docker services.
- Docker client: là cách giao tiếp chính để người dùng tương tác với Docker. Khi thực hiện các command sử dụng prefix `docker` là cách giao tiếp sử dụng docker API tới daemon.
- Docker desktop: là GUI giúp build và chia sẻ ứng dụng và microservices. Docker desktop đã bao gồm docker daemon, docker client, docker compose,...
- Docker registries: dùng để lưu trữ docker images. Là một docker hub mà tất cả mọi người có thể tìm các images. Khi sử dụng `docker pull` hay `docker run` command, docker sẽ pull images cần thiết về. Khi sử dụng `docker push` command, docker sẽ push image tới configured registry.
- Docker objects: images, containers, networks, volumes, plugins,... được coi là các docker object.
  * Images: là read-only template với các instruction để tạo Docker container. Bình thường thì một images sẽ dựa trên một image khác và có thêm sửa đổi. Ta có thể tự tạo image của riêng mình hoặc sử dụng chúng trên public registry. Để tạo một images, ta sử dụng docker file để định nghĩa các bước cần thực hiện.
  * Containers: là một chương trình chạy của image, ta có thể create, start, stop, move, delete container sử dụng Docker API hoặc CLI. Ta có thể mount volume tới container hoặc network. Mặc định, container sẽ độc lập với các container khác, ta có thể config các tùy chỉnh.
### 2.5. Docker concept:
- Container:
  * Một ứng dụng thường được chạy dựa trên nhiều components
  * Để quản lý version các component cũng như các thiết lập, khái niệm container ra đời
  * Mỗi component tương ứng với một container chạy độc lập
  * Các container chạy độc lập mà không có phụ thuộc vào host.
  * Container có tác động rất nhỏ tới các container và host machine.
  * Việc xóa một container không làm ảnh hưởng tới các container khác.
  * Các containers có thể chạy bất kì platform nào từ local machine, remote cloud,....
- Images:
  * Nhu cầu chia sẻ môi trường dev sinh ra image.
  * Một container image chứa tất cả các file, thư viện, binaries, các dependency.
  * Các images không thể thay đổi một khi đã tạo.
  * Container image là tập hợp các layer. Mỗi layer đại diện cho một tập file system mà add, remove hoặc thay đổi file.
- Registry:
  * Nhu cầu lưu trữ các images, chia sẻ các images tới các images khác.
  * Image registry là một nơi tập trung lưu trữ và chia sẻ container image.
  * Cần phân biệt rõ registry và repository: registry là nơi tập trung lưu trữ và quản lý container images, trong khi repository là tập hợp các container images có liên quan tới nhau (thường là các version khác nhau).

## 3. Docker engine:
### 3.1. Storage:
- Mặc định, các file được sinh ra trong container sẽ được lưu ở vùng ghi được, làm xảy ra những vấn đề sau:
  * Nếu container không còn tồn tại thì dữ liệu có thể không được bảo toàn.
  * Vùng ghi được có mối liên kết chặt chẽ với với máy local nên không thể di chuyển dễ dàng được.
  * Khi vào vùng ghi được yêu cầu bộ storage driver để lưu trữ trong khi sử dụng data volumes sẽ được ghi thẳng vào host filesystem.
- Docker có 2 lựa chọn khác cho việc lưu trữ file ở local machine: volume và bind-mounts.
- Docker hỗ trợ việc lưu file in-memory, trong trường hợp các file đấy không cần lưu trữ lâu dài là tmpfs.
- Sự khác biệt giữa 3 cách lưu trữ:
  * Volume: được lưu như 1 phần của host system và được quản lý với Docker với path: `/var/lib/docker/volumes/`. Những tiến trình không từ Docker sẽ không thể sửa đổi dữ liệu này.
  * Bind-mount: có thể được lưu trữ trên vị trí bất kì trong host machine. Những tiến trình không từ Docker sẽ có thể sửa đổi những file này.
  * Tmpfs: chỉ được lưu ở host system memory và không được ghi vào filesystem của host.
- Nên sử dụng flag `--mount` cho tất cả các kiểu lưu trữ khác nhau.
### 3.2 Volumes:
- Volume được tạo và quản lý bởi docker.
- Có thể tạo volume thông qua commnad: `docker volume create`.
- Khi tạo volume, nó được lưu trực tiếp trong docker host. Khi mount volume vào container, thư mục được mount trực tiếp vào container.
- Một volume có thể được mount bởi nhiều contaienrs khác nhau. Khi không có container nào mount với volume, volume cũng không được tự động xóa, hoặc được xóa thủ công qua command `docker volume prune`.
- Usecase phổ biến:
  * Chia sẻ dữ liệu giữa các containers với nhau.
  * Docker host không đảm bảo có đường dẫn hay cấu trúc file sẵn dùng.
  * Lưu trữ dữ liệu trên cloud, remote thay vì local.
  * Khi cần backup, migrate, restore dữ liệu từ một Docker host sang docker host khác.
  * Cần có hiệu năng cao ở I/O.
  * Cần native filesystem behaivor.
### 3.3. Bind mounts:
- Hạn chế về tính năng hơn so với volume.
- Mount trực tiếp file hoặc đường dẫn trong local host.
- Bind mount chạy nhanh nhưng cần có host system có đường dẫn cấu trúc cụ thể.
- Usecase phổ biến:
  * Chia sẻ config file từ host machine tới container.
  * Chia sẻ source code hay build sẵn của code.
  -> overall, nên sử dụng volume nếu được.
### 3.4 tmpfs:
- Thường được dùng để lưu trữ file trong life-time của container.
- Được sử dụng trong docker swarm.
- Usecase phổ biến:
  * Khi không muốn lưu trữ dữ liệu ở localhost hay trong container.
### 3.5. Lưu ý:
- Nếu mount empty volume vào một đường dẫn có các file hoặc thư mục sẵn có, các dữ liệu ấy được copy vào volume.
- Nếu start container mà volume chưa được tạo, volume đó sẽ được tạo tự động.
- Nếu mount/bind non-empty vào đường dẫn mà ở đó thư mục hoặc file có sẵn, các dữ liệu đó sẽ tạm thời không được hiển thị, sử dụng cho đến khi unmount.
### 3.6. Cách sử dung:
- Sử dụng flag --v/--volume: có 3 trường theo thứ tự: name volume(có thể bỏ qua), path cần mount, optional.
- Sử dụng flag --mount: là các cặp key:value bao gồm: type(loại mount), scr: tên volume, destination: path, readonly options,....
