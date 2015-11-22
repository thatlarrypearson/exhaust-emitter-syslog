# Exhaust Emitters - Syslog

Syslog logging is one type of exhaust emitter, a component of the [Exhaust Proof-of-Concept](https://github.com/ThatLarryPearson/exhaust-PoC).

Syslog logging takes advantage of a syslog package called [syslog-ng](https://www.balabit.com/network-security/syslog-ng).  You will want to chose the community edition of syslog-ng.

## Installation

Use `apt-get` to fetch the syslog-ng software packages.
```
sudo apt-get -y install syslog-ng-core syslog-ng-mod-amqp
```

## Configuration

Make sure that the software can find the RabbitMQ server.  As a precondition, you should already have installed the [collector](https://github.com/ThatLarryPearson/exhaust-collector).  One of the collector install steps had you get the IP address of the server by issuing the `hostname -I` command.  You need the IP address from that command to use when modifying the the `/etc/hosts` file.
```
sudo su - root
echo '<IP Address from RabbitMQ Host>' rabbitmq >> /etc/hosts
exit
```

Get the alternative configuration files and move them into place.
```
sudo apt-get install git-core
git clone https://github.com/ThatLarryPearson/exhaust-emitter-syslog.git
cd exhaust-emitter-syslog
sudo mkdir --parents /etc/syslog-ng/conf.d
sudo cp etc/syslog-ng/syslog-ng.conf /etc/syslog-ng
sudo cp etc/syslog-ng/conf.d/amqp.conf /etc/syslog-ng/conf.d
sudo chown root:root /etc/syslog-ng/*.conf
sudo chown -R root:root /etc/syslog-ng/conf.d
sudo chmod 0644 /etc/syslog-ng/syslog-ng.conf /etc/syslog-ng/conf.d/amqp.conf
sudo chmod 0755 /etc/syslog-ng/conf.d
```

Restart the syslog-ng service to load the new configuration.
```
sudo service syslog-ng restart
```

## Testing

To test, follow the testing outlined in the [collector testing section](https://github.com/ThatLarryPearson/exhaust-collector#Testing).  Additionally, you can "inject" messages into the logging system by using the `logger` command.
```
logger "My Test Message"
```

## Licensing

The MIT License (MIT)

Copyright &copy; 2015 Lawrence Bennett Pearson

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.


