![](https://git.elektrollart.org/Elektroll/SimpleCliMonitoring/raw/branch/master/sclimon.png)

# SCLIMON
The name sclimon stands for \[**S**\]imple \[**C**\]ommand \[**L**\]ine \[**I**\]nterface \[**Mon**\]itoring.
Sclimon is a shell monitoring tool for websites, hosts and SSL certificates written in bash. 
You just need to drop a config file in the sclimon directory and run the script file in **list** mode or **alarm** mode.
It's possible to configure the treshold how often the checks may fail until you get a notification mail. 

## Example
![](https://git.elektrollart.org/Elektroll/SimpleCliMonitoring/raw/branch/master/demo.gif)

## Installation

Just clone this repository or download the script file and make it executable<br>

```
$ chmod +x sclimon
```
and run the script file
```
./sclimon
```

If you trust this repo and the script file, you can drop it into in your binary directory.
```
sudo curl https://git.elektrollart.org/Elektroll/SimpleCliMonitoring/raw/branch/master/simpleclimonitoring -o /usr/bin/sclimon
sudo chmod +x /usr/bin/sclimon
```

and run it from enywhere with `$ sclimon`


## How to configure
The default configuration of sclimon is ```$HOME/.config/sclimon/config```. Sclimon will create a empty file, if it doesn't exists, but you must fill it on your own.

```
FROMMAIL="foobar@example.com"
MAILSERVER="example.com"
PASSWORD="<my secret password>"
TOMAIL="monitoring@example.com"
DELAY="60"
```

This file will be include by the script and set the mailserver (MAILSERVER) from where the mails should be send (FROMMAIL) to the receiver (TOMAIL). The delay option set in which intervals the checks should be run. 

After the basic configuration you can create under ```$HOME/.config/sclimon/config/``` a text file with a ```.conf``` ending like ```$HOME/.config/sclimon/conf/example.conf```. Every `.conf` file will be included by sclimon and read in a loop. The `.conf` files need the following informations:

```
TITLE="localhost"
DOMAIN="example.com"
HOST="127.0.0.1"
PORT="8000"
PROTOCOL="http|https"
PATTERN="example"
TYPE="Web|Ping|SSL"
TRESHOLD="10"
```

**TITLE** set the name in sclimon which will be shown in the interface.<br>
**DOMAIN** is needed for the SSL and web check. <br>
**HOST** set the IP adress for the ping check.<br>
**PORT** The Port is needed for the SSL and web check. The default value is 443. If your Application runs on another port, please set it here.<br>
**PROTOCOL** may be http or https. This option must be set for the webcheck and will be combine `PRTOCOL` and `DOMAIN` <br>
**PATTERN** for the web check, sclimon will search on `DOMAIN` for a word pattern. If `PATTERN` could not be found, sclimon will mark the check as failed.<br>
**TYPE** here you can set which kind of check should be run by sclimon. `WEB` checks on the webpage for a word or sentence from **PATTERN**. `PING` will test if `HOST` is reachable and `SSL` checks `DOMAIN`:`PORT` how long the SSL cert is valid and alarm it expires in less then 8 days or is already expired.<br>
**TRESHOLD** set how often the ping and web check may fail, until you get a notification mail of the downtime.<br>

If the `DELAY` in ```$HOME/.config/sclimon/config``` is set to 60 and the `TRESHOLD` will be set to 5, it will take a check every minute until 5 fails are reached, so you get a notification of a downtime after 5 minutes. The `TRESHOLD` will be reset by a postive test. Please note, if you have a lot configurations this may increase the delay between the checks.

## run
You can run sclimon with one of these options:

```
Usage: check-if-online.sh [options]
Options:
  -l    List mode. Show all stats.
  -a    Alarm mode. Show only alarms.
  -h    Show this help page
```

The `List mode` will show all configured checks in ```$HOME/.config/sclimon/conf/```<br>
If you run sclimon with `-a` in `Alarm mode`, sclimon will only show failed checks <br>