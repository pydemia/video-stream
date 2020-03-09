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
sudo mkdir -p sudo mkdir -p /share/video_stream
sudo cp ~/aws_tutorial_video.mp4  /share/video_stream/
```


```sh
sudo vim /share/video_stream/produce_video_stream.sh

```

```vim
#!/bin/bash

#cvlc --random --loop /share/video_stream/aws_tutorial_video.mp4 :sout=\#gather:rtp{sdp=rtsp://:8554/} :network-caching=1500 :sout-all :sout-keep

while true
do
  cvlc --random --loop /share/video_stream/aws_tutorial_video.mp4 :sout=\#gather:rtp{sdp=rtsp://:8554/} :network-caching=1500 :sout-all :sout-keep
done

```

#cvlc --daemon --random --loop /share/video_stream/aws_tutorial_video.mp4 :sout=\#gather:rtp{sdp=rtsp://:8554/} :network-caching=1500 :sout-all :sout-keep


```sh
sudo chmod 755 -R /share

```

```sh
sudo vim /etc/systemd/system/video_stream_producer.service

```


```vim
[Unit]
Description=Test Video Stream Producer Daemon

[Service]
Type=simple
PIDFile=/run/video_stream_producer.pid

ExecStart=/bin/bash /share/video_stream/produce_video_stream.sh

User=ec2-user
Group=ec2-user

Restart=on-failure
RestartSec=10


[Install]
WantedBy=multi-user.target

```


```sh
sudo systemctl daemon-reload && \
sudo systemctl enable video_stream_producer && \
sudo systemctl restart restart video_stream_producer && \
sudo systemctl status video_stream_producer

```


URL:
> rtsp://[IP_ADDR]:8554
