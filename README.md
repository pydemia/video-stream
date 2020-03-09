# video-stream

## Set up

```sh
sudo dnf update -y
sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo dnf install -y https://download1.rpmfusion.org/free/el/rpmfusion-free-release-8.noarch.rpm
sudo dnf install -y vlc

sudo dnf install -y unzip vim
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

aws s3 cp s3://yjkim-dataset/video/aws_tutorial_video.mp4 ./


```

## Launch

```sh
#cvlc --random --loop /var/www/html/files/test.xspf :sout=#gather:rtp{sdp=rtsp://:8554/} :network-caching=1500 :sout-all :sout-keep
#vlc -vvv ./Desktop/VideoprototypFinal.wmv --sout '#standard{access=udp, mux=ogg, dst=0.0.0.0:1234}'

cvlc --random --loop ~/aws_tutorial_video.mp4 :sout=#gather:rtp{sdp=rtsp://:8554/} :network-caching=1500 :sout-all :sout-keep
#vlc -vvv ~/aws_tutorial_video.mp4 --sout '#standard{access=udp, mux=ogg, dst=0.0.0.0:1234}'


```


## Make it as a service daemon

```sh
vim ~/produce_video_stream.sh

```

```vim
#!/bin/bash

cvlc --random --loop ~/aws_tutorial_video.mp4 :sout=#gather:rtp{sdp=rtsp://:8554/} :network-caching=1500 :sout-all :sout-keep 

```


```sh
sudo vim /etc/systemd/system/video_stream_producer.service
```

```vim
[Unit]
Description=Test Video Stream Producer Daemon

[Service]
Type=simple
ExecStart=/home/ec2-user/produce_video_stream.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target

```

```sh
sudo systemctl enable video_stream_producer
sudo systemctl start video_stream_producer
sudo systemctl status video_stream_producer

```


URL:
> rtsp://[IP_ADDR]:8554
