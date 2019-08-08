#FROM ubuntu:16.04
FROM tensorflow/tensorflow:1.8.0-gpu-py3


RUN sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/' /etc/apt/sources.list

#ENV http_proxy=http://10.41.249.28:8080 https_proxy=http://10.41.249.28:8080

RUN apt-get update -yq && apt-get install -yq build-essential cmake git pkg-config wget zip && \
apt-get install -yq libjpeg8-dev libtiff5-dev libjasper-dev libpng12-dev && \
apt-get install -yq libavcodec-dev libavformat-dev libswscale-dev libv4l-dev && \
apt-get install -yq libgtk2.0-dev && \
apt-get install -yq libatlas-base-dev gfortran && \
apt-get install -yq python3 python3-dev python3-pip python3-setuptools python3-tk git swig && \
apt-get remove -yq python-pip python3-pip && wget https://bootstrap.pypa.io/get-pip.py && python3 get-pip.py && \
pip3 install Cython && \
#pip3 install numpy Cython && \
cd ~ && git clone https://github.com/Itseez/opencv.git && \
cd opencv && mkdir build && cd build && \
cmake -D CMAKE_BUILD_TYPE=RELEASE \
	-D CMAKE_INSTALL_PREFIX=/usr/local \
	-D INSTALL_PYTHON_EXAMPLES=ON \
	-D BUILD_opencv_python3=yes -D PYTHON_EXECUTABLE=/usr/bin/python3 .. && \
make -j8 && make install && rm -rf /root/opencv/ && \
mkdir -p /root/tf-openpose && \
rm -rf /tmp/*.tar.gz && \
apt-get clean && rm -rf /tmp/* /var/tmp* /var/lib/apt/lists/* && \
rm -f /etc/ssh/ssh_host_* && rm -rf /usr/share/man?? /usr/share/man/??_*

COPY . /root/tf-openpose/
COPY ./requirements.txt /root/tf-openpose/
WORKDIR /root/tf-openpose/

RUN cd /root/tf-openpose/ && pip3 install -U setuptools && \
#pip3 install -r /root/tf-openpose/requirements.txt
#pip3 install tensorflow && pip3 install -r requirements.txt
pip3 install argparse dill fire matplotlib numba psutil pycocotools requests scikit-image scipy slidingwindow tqdm pandas git+https://github.com/ppwwyyxx/tensorpack.git

RUN cd /root && git clone https://github.com/cocodataset/cocoapi && \
pip3 install cython && \
cd cocoapi/PythonAPI && python3 setup.py build_ext --inplace && python3 setup.py build_ext install && \
mkdir /coco && cd /coco && wget http://images.cocodataset.org/annotations/annotations_trainval2017.zip && \
unzip annotations_trainval2017.zip && rm -rf annotations_trainval2017.zip

RUN pip3 install -U numpy

#RUN cd /root/tf-openpose/tf_pose/pafprocess && swig -python -c++ pafprocess.i && python3 setup.py build_ext --inplace

#ENTRYPOINT ["python3", "pose_dataworker.py"]

#ENV http_proxy= https_proxy=
