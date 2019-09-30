# nginx-letsencrypt-template
Project to spin up a quick nginx reverse proxy from template using docker image from here: [https://hub.docker.com/r/linuxserver/letsencrypt/]

## How to

Clone this repository.

If you are on Ubuntu 18 you can install docker and docker-compose by running the `docker-install.sh` file:
```
sudo bash ./docker-install.sh
```

Inside the directory you can pull the image and get all the files templated to your directory by executing from inside the repository. The image will start with errors but now we can update the Cloudflare config files. You can copy the docker-compose file to make the appropriate changes for your domain using
```
cp docker-compose.yml.sample docker-compose.yml
```

Now execute the following to get the container started:
```
sudo docker-compose up
```

Getting Cloudflare setup with your DNS entry since we are doing a wildcard in this we want to create an entry for the domain/sub-domain we want to use.

If you want to use the subdomains on the root of your domain we need to create an entry like: `*.example.com`. With this you could do sonarr.example.com or radarr.example.com.

If for example you want to use a subdomain you can still use a wildcard but we wildcard that subdomain directly such as `*.home.example.com`. With this everything would be nested under the home.example.com subdomain.

Once you have your public IP assigned to your wildcard entry go to your profile on Cloudflare and get your Global API key. To get this you can follow the guide here: [https://support.cloudflare.com/hc/en-us/articles/200167836-Managing-API-Tokens-and-Keys]

With the API key you can put this in `cloudflare.ini` located in `./dockerconfig/letsencrypt/dns-conf/` along with the email assigned to your Cloudflare account.

Ensure that you have your network configured correctly for the image to start correctly as well. Make sure you port forward 80 and 443 to the IP of your nginx/letsencrypt-template instance/machine.

Once you have it started with portforwarding, Cloudflare credentials properly updated, your container should start without errors.

You can find proxy templates inside `./dockerconfig/letsencrypt/nginx/proxy-confs/` path. There are tons of templates for subdomain and subfolder for various applications.