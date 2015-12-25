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

## steps

### start zookeeper svc
`kubectl create -f k8s-zookeeper.yaml`

### start codis user cluster

#### start all dashboard/master/slave/proxy
`kubectl create -f k8s-codis-user.yaml`

#### remove fence
`kubectl exec -it your_dashboard_pod_name --namespace=user bash`
`codis-config -C $CODIS_CONF --product=user --zk=$ZK --dashboard-addr=codis-dashboard:18087 action remove-fence`

#### init slot
`kubectl exec your_dashboard_pod_name --namespace=user bash codis-start initslot start`

#### add group master
`kubectl exec your_master_pod_name --namespace=user bash codis-start group_master start`

#### add group slave
`kubectl exec your_slave_pod_name --namespace=user bash codis-start group_slave start`

#### UI operation
1. set slot range of the group on codis UI
2. mark proxy online on codis UI

#### scale up the codis-proxy rc for fun (optional)
`kubectl scale --replicas=2 --namespace=user rc codis-proxy`

### start codis game cluster
same steps for "game" user space.
