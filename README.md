# video-stream

```sh
#subscription-manager repos --enable "rhel-*-optional-rpms" --enable "rhel-*-extras-rpms" # Only needed for RHEL
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm -y
sudo yum install https://download1.rpmfusion.org/free/el/rpmfusion-free-release-7.noarch.rpm -y


sudo rpm --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
sudo rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-1.el7.nux.noarch.rpm
sudo yum -y install ffmpeg ffmpeg-devel




sudo yum install vlc -y
sudo yum install vlc-core -y   #(for minimal headless/server install)
sudo yum install python-vlc npapi-vlc -y #(optionals)

```

