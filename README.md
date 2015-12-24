# k8s-codis
codis on k8s

## namespace
* default 
    * zookeeper

* user
    * codis-dashboard
    * codis-master
    * codis-slave
    * codis-proxy

* game
    * codis-dashboard
    * codis-master
    * codis-slave
    * codis-proxy

* kube-system
    * dns 
    * elasticsearch
    * ui
    * mornitor

## build images
    * Build  [https://github.com/kevinz/codis](https://github.com/kevinz/codis).
    * Copy all files under `$GOPATH/src/github.com/wandoulabs/codis/bin/` to `codis_image/conf/codis/bin`.
    * Build proxy image, run `docker build  -f ./Dockerfile.proxy .`
    * Tag it with `docker tag -f fe3bc14caeb6 reg.local:5000/codis-proxy:your_tag`, do not use the *latest* tag cause need to keep it stable.
    * Push it to local registry with `docker push reg.local:5000/codis-proxy`.

## known issue
    * The codis-dashboard gets killed when delete pod, it doesn't clean itself in zk which prevent next running of codis dashboard.

