Bootstrap:docker
From:nvidia/cuda:11.2.2-cudnn8-devel-ubuntu20.04

%labels
MAINTAINER Thomas Green

%environment

export DLClight=False

%runscript
exec /bin/bash /bin/echo "Not supported"

%post
mkdir /scratch
mkdir /software
mkdir /apps
mkdir /app

DEBIAN_FRONTEND=noninteractive apt-get update -yy
DEBIAN_FRONTEND=noninteractive apt-get install -yy --no-install-recommends python3 python3-venv python3-pip python3-dev ffmpeg libsm6 libxext6
DEBIAN_FRONTEND=noninteractive apt-get install -yy --no-install-recommends build-essential make cmake gcc g++
DEBIAN_FRONTEND=noninteractive apt-get update -yy
DEBIAN_FRONTEND=noninteractive apt-get install -yy --no-install-recommends \
         locales \
         libxkbcommon-x11-0 libxcb-icccm4 libxcb-image0 libxcb-keysyms1 libxcb-randr0 libxcb-render-util0 libxcb-shape0

apt-get clean
rm -rf /var/lib/apt/lists/*
locale-gen en_US.UTF-8 en_GB.UTF-8

mkdir -p /opt/deeplabcut/venv
python3 -m venv /opt/deeplabcut/venv
. /opt/deeplabcut/venv/bin/activate

pip3 install --no-cache-dir --upgrade pip
pip3 install --no-cache-dir --upgrade "deeplabcut[gui,tf,modelzoo]==2.3.6" tensorflow==2.9.3
pip3 list


# TODO required to fix permission errors when running the container with limited permission.
#chmod a+rwx -R /usr/local/lib/python3.8/dist-packages/deeplabcut/pose_estimation_tensorflow/models/pretrained
strip --remove-section=.note.ABI-tag /opt/deeplabcut/venv/lib/python3.8/site-packages/PySide6/Qt/lib/libQt6Core.so.6
pip3 uninstall --yes opencv-python opencv-python-headless
pip3 install --no-cache-dir --upgrade "opencv-python-headless<3.8"

#pip3 uninstall --yes tensorboard
#pip3 install --no-cache-dir --upgrade protobuf==3.20.1

pip3 install --no-cache-dir jupyterlab

deactivate
