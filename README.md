# prusa-cam


This is a Fork of Flacks Prusa-cam Webcam Daemon to work with Raspbian OS

![Snapshot of `prusa-cam`](snapshot.png)

I set up a Logitech C920 webcam [inside my Prusa enclosure](https://www.printables.com/model/433908-logitech-c920-original-prusa-enclosure-mount) to be able to monitor my prints using Prusa Connect. `prusa-cam` is a Bash script that POSTs snapshots from your webcam to the Prusa Connect API in 10 second intervals, which I believe is the minimum time generally allowed by Prusa. It only sends snapshots when your printer is online and actively printing. It pings your printer locally and runs as a systemd service, and logs if things are successful or not, resetting states if failures occur.

![Camera inside printer enclosure](camera.jpg)

![View from camera](cheese.jpg)

## Dependencies

- jq
- ffmpeg

## Setup

- Flash Raspbian OS 64Bit Lite on your SD Card eg using Raspberry Pi Imager
- SSH into your Pi
- install git: ```sudo apt-get install git```
- install jq: ```sudo apt-get install jq```
- install ffmpeg: ```sudo apt-get install ffmpeg```
- Git Clone Prusa-Cam to your home directory (/home/pi/): ```git clone https://github.com/its-Blackpoint/prusa-cam.git```
- Copy the service to /lib/systemd/system/ ```sudo cp /home/pi/prusa-cam/prusa-cam.service /lib/systemd/system```
- Add "new other camera" in Prusa Connect
- Edit `env.example` file to fit your Setup. Youll Find all info in the env.example File. ```sudo nano prusa-cam/env.example``` 
- Rename `env.example` to `env.`  ```mv prusa-cam/env.example prusa-cam/env```
- Run `prusa-cam`, ensure you're getting successes: ```./prusa-cam/prusa-cam```
  - Note: `prusa-cam` will only start POSTing snapshots if your printer is actively printing
  - `prusa-cam` will let you know if one of the critical dependencies is missing, or if your config is missing required fields
- Start prusa-cam Service: ```sudo systemctl start prusa-cam.service```
- Check that prusa-cam Service is working: ```sudo systemctl status prusa-cam.service```
