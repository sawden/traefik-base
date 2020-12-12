<div align="center">
    <a href="https://traefik.folibon.de">
        <img height="70" src="./logo.png" />
    </a>
    <br />
    <br />
    <strong>Traefik dockerized Reverse-Proxy and Let’s Encrypt Provider Setup</strong>
    <br />
    <br />
</div>

## Install Dependencies

#### 📦 Git
```bash
apt-get update
apt-get install git
```

#### 📦 Docker & Docker Compose
Instructions [here](https://docs.docker.com/compose/install/)

#### 📦 htpasswd
```bash
apt-get update
apt-get install apache2-utils
```

## Clone this Project
```bash
git clone https://github.com/sawden/traefik-base.git /opt/containers/traefik
```

## Config and start traefik
```bash
# Create the log file
touch /var/log/traefik.log
# Create a htpasswd user (named traefik) and enter a password of your choice
htpasswd -c /opt/containers/traefik/data/.htpasswd traefik
# Replace 'example@email.com' with your email
nano /opt/containers/traefik/data/traefik.yml
# Make sure traefik has the correct access rights on 'acme.json'
chmod 600 /opt/containers/traefik/data/acme.json
# Copy the sample env
cp /opt/containers/traefik/.env.dist /opt/containers/traefik/.env
# Edit the env
nano /opt/containers/traefik/.env
# Create the Docker network
docker network create traefik
# Start 🚀 the container
docker-compose up -d
```

## Documentation and Inspiration
<strong>See the documentation [here](https://doc.traefik.io/traefik/) and a 
helpful source for this setup [here](https://goneuland.de/traefik-v2-reverse-proxy-fuer-docker-unter-debian-10-einrichten/).</strong>