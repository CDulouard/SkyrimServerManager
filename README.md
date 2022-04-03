# SkyrimServerManager

## Step 1
Create an Amazon EC2 instance with a reasonable size to host the server.
Make sure to have a valid SSH key to connect to the instance.
You can attach an Elastic IP to the instance if you want to keep the public ip after the instance stops.
You must open port 10578 for inbound UDP in the EC2 security group.

## Step 2
Connect to the EC2 instance using SSH.

## Step 3
Make sure Docker is installed on the EC2. 
If it is not installed run `sudo apt-get update -y` the `sudo apt-get install docker.io -y` if you use an ubuntu like AMI.

## Step 4
Creat a file called `/etc/systemd/system/skyrim-together.service` and paste the following content:
```
[Unit]
Description=Skyrim Together container
After=network-online.target

[Service]
Type=simple
User=root
Group=root
UMask=007
ExecStart=docker run -p 10578:10578/udp tiltedphoques/st-game-server -token Crunching-Knowledge -premium # You can change the token value
Restart=on-failure
TimeoutStopSec=300

[Install]
WantedBy=multi-user.target
```

## Step 5
Run `sudo systemctl enable skyrim-together.service` then `sudo systemctl start skyrim-together.service` to allow the server to restart when the instance starts.
You can check the service status with `sudo systemctl status skyrim-together.service` and the container status with `sudo docker ps.`

## Step 6
Get the instance ID and paste it in both `.github/workflows/stop.yml` and `.github/workflows/start.yml` (INSTANCE_ID env variable).

## Step 7
Make sure `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` secrets are set in the GitHub repo (admin only).

## Step 8
You can now start and stop the server by running manually the start and stop workflow.
