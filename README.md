# FFMPEG-Pi-Camera-Raspberry-Pi

1. Install libraries 
A.
X264 library [necessary] :

cd /usr/src
git clone git://git.videolan.org/x264
cd x264
./configure --host=arm-unknown-linux-gnueabi --enable-static --disable-opencl
make
sudo make install

B. library aac plus
cd /usr/src
wget http://tipok.org.ua/downloads/media/aacplus/libaacplus/libaacplus-2.0.2.tar.gz
tar -xzf libaacplus-2.0.2.tar.gz
cd libaacplus-2.0.2
./autogen.sh --with-parameter-expansion-string-replace-capable-shell=/bin/bash --host=arm-unknown-linux-gnueabi --enable-static
make
sudo make install


C. fdk aac
cd usr/src
wget -O fdk-aac.tar.gz https://github.com/mstorsjo/fdk-aac/tarball/master
tar xzvf fdk-aac.tar.gz
cd mstorsjo-fdk-aac*
autoreconf -fiv
./configure --enable-shared
make -j2
sudo make install


2. Install ffmpeg
cd /usr/src
git clone git://source.ffmpeg.org/ffmpeg.git
cd ffmpeg/
sudo ./configure --arch=armel --target-os=linux --enable-gpl --enable-libx264 --enable-nonfree
make
make install

LIVE STREAM FROM COMMAND LINE:

raspivid -o - -t 0 -vf -hf -w 960 -h 540 -fps 10 -b 500000 | ffmpeg -re -ar 44100 -ac 2 -acodec pcm_s16le -f s16le -ac 2 -i /dev/zero -f h264 -i - -vcodec copy -acodec aac -ab 128k -g 50 -strict experimental -f flv rtmp://ip.address.of.videoserver/live1/stream

Or
raspivid -o - -t 0 -vf -hf -n -w 1280 -h 720 -fps 25 -b 500000 | ffmpeg -i - -vcodec copy -f flv rtmp://ip.address.of.videoderver/live1/stream



TO PULL OUT STREAM :
ffplay "rtmp://ip.address.of.videoserver/live1/stream?user=user&pass=password"


 
