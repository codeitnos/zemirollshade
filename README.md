# Zemismart Roller Shade Integration
This is a Python script to connect a Raspberry PI to a Zemismart Roller Shade. It listens to an MQTT topic and executes a close or open command based on that topic.

# Requirements

- Python 3+
- pip (to automatically install python dependencies)

# Dependencies
- paho-mqtt
- bluepy
- [Zemismart](https://github.com/GylleTanken/python-zemismart-roller-shade)
- libglib2.0-dev ```sudo apt-get install libglib2.0-dev```

# Install

1. Download or clone the repository:
2. In the directory run ```pip install -r requirements.txt```
3. Configure details in ```rollshade.py``` and then run with ```python rollshade.py```

# Home Assistant Config

Just add a MQTT cover:

```yaml
...
cover:
  - platform: mqtt
    name: "Blinds Bedroom"
    state_topic: "blinds/00:00:00:00:00:00/status"
    command_topic: "blinds/00:00:00:00:00:00"
    qos: 0
    state_open: "on"
    state_closed: "off"
    payload_open: "open"
    payload_close: "close"
    retain: false
    optimistic: false
```

# Запускать при загрузке системы

Создайте файл
```sh
sudo nano /lib/systemd/system/rollershade.service
```


Добавьте в этот файл следующие строки: 
```service
[Unit] 
Description=Rollshade 
After=multi-user.target
 
[Service] 
Type=simple 
ExecStart=python3 /home/alex/blinds/zemirollshade/rollshade.py 
Restart=on-failure

StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=roll
 
[Install]
WantedBy=multi-user.target
```
Теперь нам нужно активировать сервис:

```sh
sudo chmod 644 /lib/systemd/system/rollershade.service
chmod +x /home/pi/zemirollshade/rollshade.py
sudo systemctl daemon-reload
sudo systemctl enable rollershade.service
sudo systemctl start rollershade.service
```
