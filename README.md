# nginx-letsencrypt-compose

Automate Letencrypt SSL lifecycle with Nginx and docker compose (ansible driven deployment )

I needed multi-domains certificates so i borrowed and tweaked some really nice scripts here : 

 - [For the idea](https://www.programonaut.com/setup-ssl-with-docker-nginx-and-lets-encrypt/)
 - [For the separation create/renew ](https://blog.jarrousse.org/2022/04/09/an-elegant-way-to-use-docker-compose-to-obtain-and-renew-a-lets-encrypt-ssl-certificate-with-certbot-and-configure-the-nginx-service-to-use-it/)
 - [For the smart all-in-one docker compose ](https://github.com/seliverstov-maxim/docker-nginx-certbot)


# Configuration

0. Install ansible if you don't already have it

Check [latest ansible documentation](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

1.  Inventory 

Copy and rename `inventory.yaml.dist` to `inventory.yaml` and fill replace placeholders with your actual data.

You're set !

2. Customization 

You can tailor Nginx config by editing `templates/proxy.conf.j2` to fit your particular needs

# Deployment on host

Be sure you can SSH into your targer host and launch the ansible playbook.

__Use dry-run first to check that certbot is OK__

`ansible-playbook -i inventory.yaml deploy.yaml -b -e 'dryrun=true'`

__Launch the stack__

`ansible-playbook -i inventory.yaml deploy.yaml -b`

__Stop the stack__

`ansible-playbook -i inventory.yaml deploy.yaml -b -e 'state=stopped'`

# Dev / Troubleshooting 

Check containers logs 

`docker logs [-f]  nginx-ssl-certbot-init-1`

Inspect usefull volumes 

`docker run --rm -it --volumes-from nginx-ssl-nginx-1 alpine sh`

