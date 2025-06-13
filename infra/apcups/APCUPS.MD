#
# APCUPS
#

mkdir /infra/apcups/config
mkdir /infra/apcups/data

run -d --privileged   --name=apcupsd    -e TZ=Europe/London   --device=/dev/usb/hiddev0   --restart unless-stopped   -p=3551:3551   -v /var/run/dbus/system_bus_socket:/var/run/dbus/system_bus_socket   -v /infra/apcups/config:/etc/apcupsd -v /infra/apcups/data:/var gregewing/apcupsd:latest


#
# APC Metrics
#

https://github.com/56quarters/apcmetrics
cd apcmetrics

#
# Install GO
#

apt -y install wget git build-essential

mkdir -p /root/.local/share

cat > install-go.sh << EOF
#!/bin/bash
VERSION=1.22.1 # pick the latest version from https://golang.org/dl/
ARCH=arm64 # arm64 for 64-bit OS, armv6l for 32-bit OS
## Download the latest version of Golang
echo "Downloading Go $VERSION"
wget https://dl.google.com/go/go$VERSION.linux-$ARCH.tar.gz
echo "Downloading Go $VERSION completed"
## Extract the archive
echo "Extracting..."
tar -C ~/.local/share -xzf go$VERSION.linux-$ARCH.tar.gz
echo "Extraction complete"
## Detect the user's shell and add the appropriate path variables
SHELL_TYPE=$(basename "$SHELL")
if [ "$SHELL_TYPE" = "zsh" ]; then     echo "Found ZSH shell";     SHELL_RC="$HOME/.zshrc"; elif [ "$SHELL_TYPE" = "bash" ]; then     echo "Found Bash shell";     SHELL_RC="$HOME/.bashrc"; elif [ "$SHELL_TYPE" = "fish" ]; then     echo "Found Fish shell";     SHELL_RC="$HOME/.config/fish/config.fish"; else     echo "Unsupported shell: $SHELL_TYPE";     exit 1; fi
echo 'export GOPATH=$HOME/.local/share/go' >> "$SHELL_RC"
echo 'export PATH=$HOME/.local/share/go/bin:$PATH' >> "$SHELL_RC"
## Verify the installation
if [ -x "$(command -v go)" ]; then     INSTALLED_VERSION=$(go version | awk '{print $3}');     if [ "$INSTALLED_VERSION" == "go$VERSION" ]; then         echo "Go $VERSION is installed successfully.";     else         echo "Installed Go version ($INSTALLED_VERSION) doesn't match the expected version (go$VERSION).";     fi; else     echo "Go is not found in the PATH. Make sure to add Go's bin directory to your PATH."; fi
## Clean up
rm go$VERSION.linux-$ARCH.tar.gz
EOF

chmod 700 install-go.sh
./install-go.sh

make

docker login -u moki38
docker build -t moki38/apcmetrics:0.1.0 -t moki38/apcmetrics:latest .
docker image push moki38/apcmetrics:0.1.0 moki38/apcmetrics:latest
docker image push moki38/apcmetrics:0.1.0

 docker run --restart always -p 9780:9780 -d --name apcmetrics moki38/apcmetrics:latest --ups.address=192.168.50.45:3551 metrics

#
# Prometheus
#

scrape_configs:
  - job_name: apcmetrics
    static_configs:
      - targets: [ '10.0.70.1:9780' ]

