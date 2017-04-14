# docker
my docker tutorial

## Quick Reference


```
Remove dangling images
List :
docker images -f dangling=true
Remove :
docker rmi $(docker images -f dangling=true -q)


Remove all images :
docker rmi $(docker images -a -q)

Remove Issue :
[root@devel ~/GIT/docker]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              16.04               0ef2e08ed3fa        6 weeks ago         130 MB
ubuntu              latest              0ef2e08ed3fa        6 weeks ago         130 MB

[root@devel ~/GIT/docker]# docker rmi $(docker images -a -q)
Error response from daemon: conflict: unable to delete 0ef2e08ed3fa (must be forced) - image is referenced in multiple repositories
Error response from daemon: conflict: unable to delete 0ef2e08ed3fa (must be forced) - image is referenced in multiple repositories

Fixes :
[root@devel ~/GIT/docker]# docker rmi -f $(docker images -a -q)
Untagged: ubuntu:16.04
Untagged: ubuntu:latest
Untagged: ubuntu@sha256:dd7808d8792c9841d0b460122f1acf0a2dd1f56404f8d1e56298048885e45535
Deleted: sha256:0ef2e08ed3fabfc44002ccb846c4f2416a2135affc3ce39538834059606f32dd
Deleted: sha256:0d58a35162057295d273c5fb8b7e26124a31588cdadad125f4bce63b638dddb5
Deleted: sha256:cb7f997e049c07cdd872b8354052c808499937645f6164912c4126015df036cc
Deleted: sha256:fcb4581c4f016b2e9761f8f69239433e1e123d6f5234ca9c30c33eba698487cc
Deleted: sha256:b53cd3273b78f7f9e7059231fe0a7ed52e0f8e3657363eb015c61b2a6942af87
Deleted: sha256:745f5be9952c1a22dd4225ed6c8d7b760fe0d3583efd52f91992463b53f7aea3

```



