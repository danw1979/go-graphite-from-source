# go-carbon from source

## About
You can run this file from the current directory using - 

`eval "$(sed -n '/^```/,/^```/ p' < ./install.md | sed '/^```/ d')"`

## Build
Build go-carbon in the current user's home directory - 
```
CWD=`pwd`
go get -d github.com/lomik/go-carbon
cd ~/go/src/github.com/lomik/go-carbon
make 
```

## Install
Install executable to somewhere sensible, create a `carbon` user and create config, logging and data directories - 
``` 
sudo install go-carbon /usr/local/sbin/
sudo mkdir -p /etc/go-carbon /var/log/go-carbon /var/lib/graphite/whisper
sudo useradd -r carbon
sudo chown -R carbon: /var/log/go-carbon /var/lib/graphite/whisper
```

Copy the provided systemd unit into place - 
```
cd $CWD
sudo cp -a go-carbon.service /lib/systemd/system/
sudo chmod 644 /lib/systemd/system/go-carbon.service
```

## Config
Install a default go-carbon config file and the provided storage schema config - 
```
cd $CWD
/usr/local/sbin/go-carbon --config-print-default | sudo tee /etc/go-carbon.conf > /dev/null
sudo cp -a storage-schemas.conf /etc/go-carbon/
```

Enable Service
```
sudo systemctl daemon-reload
sudo systemctl enable go-carbon
```

Start Service
```
sudo systemctl start go-carbon
```
