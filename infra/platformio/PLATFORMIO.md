#
# PlatformIO
#

#
# The board i use is this one:
#

![Alt text](https://www.wemos.cc/en/latest/_images/d1_mini_v4.0.0_1_16x16.png "Wemos D1 Mini")

https://www.wemos.cc/en/latest/d1/d1_mini.html


#
# Run our docker container
#

## If only we got this far...

WARNING: The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested
exec /usr/local/bin/platformio: exec format error


#
# https://github.com/sglahn/docker-platformio-core.git
#

#
# Lucky threy left us a dockerfile
#

```
git clone https://github.com/sglahn/docker-platformio-core.git
cd docker-platformio-core
build build .

docker login
docker build -t cwier/docker-platformio-core:v6.1.7 .
docker image push cwier/docker-platformio-core:v6.1.7

```
docker run --rm \
    --name platformio \
    -v /infra/platformio/config/:/.platformio \
    -u `id -u $USER`:`id -g $USER` \
    -v /infra/platformio/data:/workspace \
    --device=/dev/ttyS0 \
    cwier/docker-platformio-core:v6.1.7 \
    init --board d1_mini

```

#
#
#

![image](Isolated.png "Title") https://belni.atlassian.net/wiki/download/attachments/1378086/2021-12-16%2018_15_27-wemos-d1-mini-pinout.jpg%20(WEBP%20Image,%20700%C2%A0%C3%97%C2%A0394%20pixels)%20%E2%80%94%20Mozilla%20Firefox.png?version=1&modificationDate=1639674961451&cacheVersion=1&api=v2

```
curl -fsSL https://raw.githubusercontent.com/platformio/platformio-core/develop/platformio/assets/system/99-platformio-udev.rules | sudo tee /etc/udev/rules.d/99-platformio-udev.rules
```

```
platformio init -b teensy31 (for Teensy 3.2)
platformio run

#
# Library install
#
```
pkg install -l SerialTransfer
pkg install -l Wire -p espressif8266

```

#
# platformio run --target upload
#
```
docker run --rm     --name platformio     -v /infra/platformio/config:/.platformio     -v /infra/platformio/data:/workspace   -u `id -u $USER`:`id -g $USER`   --device=/dev/ttyS0     cwier/docker-platformio-core:v6.1.7  run -t upload

##
## platformio device monitor -p /dev/ttyACM0
## 
```
docker run --rm     --name platformio     -v /infra/platformio/config:/.platformio     -v /infra/platformio/data:/workspace   -u `id -u $USER`:`id -g $USER`   --device=/dev/ttyS0     cwier/docker-platformio-core:v6.1.7  device monitor -p /dev/ttyACM0
```
