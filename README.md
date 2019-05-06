# Sensu Go Minimal

Greetings traveler! This repo contains a Docker compose setup for a minimal Sensu deployment. It consists of the following:

* A Sensu Backend
* A Sensu Agent
* An asset server

This idea here is that you can quickly spin up a Docker-based environment that will give you the bare minimum for doing any troubleshooting. Let's go over a few things quick before you decide to get off to the races.

## Know Before You Deploy

We'll go over some things here. I invite you to take a gander over `docker-compose.yaml` so you can see what will be happening in this particular setup.

### Sensu Backend

* All of the standard backend ports are exposed on `localhost`
* `/tmp/sensu` will be mounted under `/var/lib/sensu` (this allows for things like proxy entities to persist, and for assets to be persist once downloaded)

### Sensu Agent

* Comes with some pre-defined subscriptions (dev, poller, system, linux, docker)
* The client cache dir _does not persist_ (read below to see why)

### Asset Server

Why use this instead of Bonsai? The local asset server does the following:

* Circumvents s****y wifi (ty DockerCon 2019 & Caleb for the inspiration)
* Download your asset tarballs into `./assets`
* Make sure when you create your asset definitions to us `http://localhost/assets/$NAMEOFYOURASSET` for maximum effectiosity

## Click Here for Deploy

Alright. All read up? Good! Time to run:

`docker-compose up -d`

That's going to give you some output that looks like this:

```
docker-compose up -d                                                                                                 ~/s/c/s/d/sensu-go-minimal
Starting sensu-go-minimal_sensu-backend_1 ... done
Starting sensu-go-minimal_sensu-asset-server_1 ... done
Starting sensu-go-minimal_sensu-agent_1        ... done
```

Niceeeeee! Now we're cooking with gas. So what can you do with this setup?

Again, it's a simple setup. This is great for trying to replicate some basic issues. Keep in mind that this is an _Alpine_ environment. It's minimal and not really that useful if you're trying to debug a potential os-level issue. 

## BONUS LEVEL

Wanna do something cool? 

`docker-compose up --scale sensu-agent=10`

^ this will allow you to scale up your agents quickly. Go on, give it a shot! 