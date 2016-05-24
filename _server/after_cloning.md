---
title: After Cloning
---

After cloning for each development virtual machine from qa.dac.dartmouth.edu...

## Install VM specific Configuration

``` shell
cd /usr/local/hydra-config
sudo ./install_config.rb
```

## Delete deployment directory

``` shell
rm -rf /opt/deploy
```

## Application
You'll probably want to chown everything in any application you'll be using. Replace {username} with your username.

``` shell 
sudo chown -R {username}:dac {application root dir}
```

For more specific information about each application's development environment, please visit application specific documentation:

- [Linked Name Authority](/lna/techdocs)
