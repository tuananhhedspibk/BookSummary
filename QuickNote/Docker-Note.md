# Sơ lược về Docker :rocket: :green_book: :memo:

## 0. Docker Architecture
  + Docker daemon (server) -> RESTAPI -> Client Docker CLI

## 1. Docker images
  + Image là 1 stack các layers, lớp dưới cùng có thời gian bắt đầu sớm nhất
    - Các lớp này là read-only
    - Lớp trên cùng: writable layer, do container tạo ra
    - Mọi thêm sửa, xóa data sẽ tác động lên writable layer
    - 1 image có thể tạo ra nhiều containers
      * Các containers này chung nhau lớp dưới, khác nhau ở lớp writable layer
      * Khi container bị xóa, lớp writable layer cũng bị xóa luôn
      * container = read-only layers in image + layer of changes
      * Layers tầng trên sẽ reference xuống layer phía dưới
	  
    ``` docker
      $ docker run image_name [─it] [/bin/bash]: chạy image -> container
      [-it: run mode - chạy ở chế độ tương tác]
		  [/bin/bash: chạy bash shell khi image chạy]
    ```
		
    - Sẽ download image nếu nó chưa tồn tại
	+ $ docker images [-q]: list toàn bộ images
		- Kết quả đầu ra gồm các attributes:
			* TAG: tag logic cho images
			* Image ID
			* Virtual size: kích cỡ của image
			* -q: chỉ cho ra imageID
	+ $ docker rmi [imageID / imageName]
	+ $ docker inspect image/container_name: xem chi tiết image/ container

## 2. Docker container
	
  + Là thể hiện của image khi chạy
	+ Sử dụng câu lệnh: "$docker run" để chạy
	+ $ docker ps [-a]: liệt kê mọi containers trong máy
		* -a: liệt kê mọi containers trong hệ thống (kể cả những containers đang không chạy)
	+ $ docker history imageId/imageName: list toàn bộ câu lệnh đã dùng với image này

## 3. Working with Containers
	
  + $ docker top containerID: xem top các processes trong container
	+ $ docker stop containerID: dừng container đang chạy
	+ $ docker rm containerID: xóa container
	+ Container sau khi dừng sẽ có trạng thái "stop", nó vẫn tồn tại nhưng không hiển thị khi dùng
		câu lệnh "$ docker ps"
	+ $ docker stats containerID: thống kê liên quan tới container - CPU, memory
	+ $ docker attach containerID: nhảy vào container
	+ $ docker pause containerID: dừng các processes trong 1 container đang chạy
	+ $ docker kill containerID: kill container
	+ $ docker run chỉ chuyển các container từ stop -> running (không có pause)
	+ Docker – Container Lifecycle

## 4. Docker Architecture
	
  + Truyền thống: Hypervisor sẽ dùng để host các máy ảo - GuestOS (nhiều cái)
	+ Hiện đại: Docker engine: sẽ đảm đương Hypervisor và GuestOS -> có thể dùng toàn bộ tài nguyên của hệ thống
		- Các apps chạy phía trên docker engine sẽ tương đương như các docker containers

## 5. Container and Hosts

## 6. Configuring Docker
	
  + service docker [start | stop]

## 8. Expose port
	
  + Cổng của ứng dụng
	+ Bên trong container có thể là cổng A, nhưng cho ra ngoài có thể là 1 cổng B khác

## 9. Một số lệnh cơ bản làm việc với docker
	
  + $ docker run --port B:A image
	+ $ docker run --name containerName image: đặt tên riêng cho container
		khi nó được tạo ra
	+ $ docker run --volume pathOnMyComputer:intheContainer image
		Mount giữa folder làm việc bên ngoài với thành phần trong container tương ứng

## 10. Dockerfile
	
  + Đơn thuần là 1 file config gồm 1 chuỗi các câu lệnh trong đó
	+ Các command cơ bản
		- `FROM`: base image để build 1 image mới, phải đặt trên cùng của Dockerfile
		- `MAINTAINER`: option command: thông tin người tiến hành xây dựng images
		- `RUN`: sử dụng khi muốn thực thi 1 command trong quá trình build image
		- `COPY`: Copy file đến image, có thể dùng URL, khi đó docker sẽ tải file đó đến thư mục đích
		- `ENV`: biến môi trường
		- `CMD`: sử dụng khi muốn thực thi 1 cmd trong quá trình build container từ image
		- `ENTRYPOINT`: định nghĩa những command sẽ được chạy khi container running
		- `WORKDIR`: định nghĩa directory cho CMD
		- `USER`: đặt user, UID cho container được tạo bởi image
		- `VOLUME`: liên kết thư mục/ truy cập các container và máy chủ
		- `ADD curDIr imageDir`: thêm curDIr vào imageDir trong image

## 11. Docker compose
	
  + Là công cụ build, start nhiều docker containers 's application
	+ Ta sẽ sử dụng compose file: docker-compose.yml
	+ 3 bước sử dụng cơ bản
		B1: Khai báo app's environment với Dockerfile
		B2: Khai báo các services cần thiết để chạy app trong docker-compose.yml
		B3: Chạy $docker-compose start
	+ Với docker thì localhost sẽ là chính nó
	+ Trong docker compose, ta có thể truy cập URL của service theo tên của service được định nghĩa trong file
		docker-compose.yml
	+ Với docker-compose ta có thể build container theo 2 cách:
		- C1: Dựa theo images có sẵn trong máy hoặc từ docker hub
		- C2: Build images dựa theo thư mục trong máy, rồi build container
	+ Tùy chọn [-d] sẽ giúp cho container chạy ở detach mode - chạy ngầm
		- Muốn dừng thì dùng lệnh: docker-compose stop
	+ Việc sử dụng volumes trong docker-compose file cũng cho phép ta chỉnh sửa code mà không cần rebuild image

## 12. Volumes
  + Là cách chia sẻ vùng nhớ giữa máy chủ vs container
	+ $ docker run -v <local dir>:<container dir>
	+ Volumes trong docker là cách chia sẻ dữ liệu giữa các containers với nhau và với host
	
### 12.1. Mount 1 thư mục nào đó trong host vào container
	
  + docker run -v pathOnMyComputer image

### 12.2. Sử dụng volume để chia sẻ dữ liệu giữa host và container
	
  + Mount 1 thư mục của host với 1 thư mục của container
	+ Dữ liệu sinh ra trên thư mục được mount của container sẽ xuất hiện ở thư mục của host
	+ Có 2 chế độ chia sẻ volume trong docker đó là rw - ro (read-write(default), read-only)
	`$ docker run --volume pathOnMyComputer:intheContainer:[mode: rw, ro] image`

### 12.3. Sử dụng volume để chia sẽ dữ liệu giữa các container
	
  + Tạo container chứa volume 
    -  $ docker create -v /mountDir --name containerName imageName
	+ Tạo 1 container mới sử dụng container vừa tạo làm volume, mọi sự thay đổi trong container mới sẽ được cập nhật vào trong
		container ban đầu này
    ``` Dockerfile
		  $ docker run -t -i --volumes-from volumecontainer ubuntu /bin/bash
		  [--volumes-from] sẽ chỉ ra tên của container được map volume
    ```

## 13. Docker network
### 13.1. Tổng quan
	
  + $ docker network ls: xem các network
	+ Có 3 loại network mặc định:
		- bridge (default)
		- host
		- none
### 13.2. None
  + Các container thiết lập network này sẽ không được cấu hình mạng

### 13.3. Bridge
  + Docker sẽ tạo ra 1 switch ảo, container được tạo ra thì interface của nó sẽ được gắn với switch ảo này
		và kết nối tới interface của host

### 13.4. Host
  + Container sẽ dùng mạng trực tiếp của máy host, network config bên trong container đồng nhất với host

### 13.5. Tương tác giữa các container với nhau
  + Trong cùng 1 host, các containers có thể tương tác với nhau thông qua bridge-network
	+ Tuy nhiên các containers được cấp IP động - liên lạc giữa các containers sẽ khó khăn => dùng name để tương tác
	+ Sử dụng default bridge network khai báo thêm [--link=name_container]

### 13.6. User-defined network
  + $ docker network create --driver [tên network: bridge, host, ...] --subnet 192.168.2.1 bridexyz
    - --subnet: địa chỉ mạng
		- bridexyz: tên của mạng
	+ $ docker run --network=name_network: --network chỉ định ra tên dải mạng ta muốn dùng 