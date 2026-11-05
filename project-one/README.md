# School Website — Nginx

Cloned static site, containerized with Nginx. Not developed here.

**Source:** https://github.com/DolfinED-HoLabs/Docker-Capstone-Project1

## Run

docker build -t school-website .
docker run -d --name school-website -p 80:80 school-website

Open: http://localhost:80

On a remote VM, use the VM's public IP and allow port 80 in the firewall/NSG.

## Files

- school-website/ — site files
- Dockerfile — FROM nginx, COPY site to /usr/share/nginx/html, EXPOSE 80

## Commands

| Action           | Command                     |
|------------------|-----------------------------|
| Stop             | docker stop school-website  |
| Start            | docker start school-website |
| Logs             | docker logs school-website  |
| Remove container | docker rm -f school-website |
| Remove image     | docker rmi school-website   |
