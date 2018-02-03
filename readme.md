
# What is this
Over the next couple of months, i will be putting together ultra scale kubernetes cluster (target 5000 nodes). This will require some work on:
1. upstream cloud provider perf (such as fixing https://github.com/kubernetes/kubernetes/issues/58770 by https://github.com/kubernetes/kubernetes/pull/59253)
2. acs-engine specifically etcd disks/topology

Eventually findings/lesson learned in this repo will find thier way into both repos (kubernetes/acs-engine)

> This is an experimental repo, so don't try this at home (unless you know what you are doing).
