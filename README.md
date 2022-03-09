<div align="center">
    <img height="70" src="./logo.png" />
    <br />
    <br />
    <strong>Traefik dockerized Reverse-Proxy and Let’s Encrypt Provider Setup</strong>
    <br />
    <br />
</div>

---
**IMPORTANT**

This guide assumes that docker uses the user and group 'traefik' through [userns-remap](https://docs.docker.com/engine/security/userns-remap/#enable-userns-remap-on-the-daemon).

---

## Install Dependencies

#### 📦 Git
```bash
apt-get update
apt-get install git
```

#### 📦 Docker & Docker Compose
Instructions [here](https://docs.docker.com/compose/install/)

## Clone this Project
```bash
git clone https://github.com/sawden/traefik-base.git /opt/containers/traefik
```

## Config and start traefik
```bash
# Create a 'secrets' folder.
mkdir /opt/containers/traefik/secrets/
# Create all required secrets.
echo "YOUR_CLOUDFLARE_API_EMAIL" > "/opt/containers/traefik/secrets/cloudflare_api_email"
echo "YOUR_CLOUDFLARE_API_KEY" > "/opt/containers/traefik/secrets/cloudflare_api_key"
echo "YOUR_NETCUP_CUSTOMER_NUMBER" > "/opt/containers/traefik/secrets/netcup_customer_number"
echo "YOUR_NETCUP_API_KEY" > "/opt/containers/traefik/secrets/netcup_api_key"
echo "YOUR_NETCUP_API_PASSWORD" > "/opt/containers/traefik/secrets/netcup_api_password"
# Change the 'secrets' owner and permissions
sudo chmod -R 600 /opt/containers/traefik/secrets/
sudo chown -R traefik:traefik /opt/containers/traefik/secrets/
# Create the log files
touch /var/log/traefik.log
touch /var/log/access.log
# Replace 'example@email.com' with your email
nano /opt/containers/traefik/data/traefik.yml
# Make sure traefik has the correct access rights on 'acme.json'
echo "{}" > "/opt/containers/traefik/data/acme.json"
chmod 600 /opt/containers/traefik/data/acme.json
# Replace 'auth.example.com' with your authelia domain
nano /opt/containers/traefik/data/middlewares/authelia.yml
# Copy the sample env
cp /opt/containers/traefik/.env.dist /opt/containers/traefik/.env
# Edit the env
nano /opt/containers/traefik/.env
# Change the permissions of the 'data' folder to the docker remap user
sudo chown -R traefik:traefik /opt/containers/authelia/data
# Start 🚀 the container
docker-compose up -d
```

By default, [authelia](https://github.com/sawden/traefik-authelia) is enabled to protect the dashboard.
To disable Authelia simply replace the `chain-auth@file` middleware with `chain-no-auth@file` in the docker-compose.yml.

## Documentation and Inspiration
<strong>See the documentation [here](https://doc.traefik.io/traefik/) and a 
helpful source for this setup [here](https://goneuland.de/traefik-v2-reverse-proxy-fuer-docker-unter-debian-10-einrichten/).</strong>