---
description: Docker 이미지 폴더의 저장소를 변경 합니다.
---

# Docker Folder 변경

### Docker 에러 원인

Docker 는 기본적으로 /var/lib/docker 에 모든 이미지를 저장하나, 작업 중 이미지 폴더가 늘어나게 됩니다. 해당 이미지가 늘어나면 시스템 에러 및 다운을 야기하며\(/ 폴더가 꽉 차게 됨\) 여러 문제점이 발생할 소지가 있기 때문에 폴더를 대용량 디스크로 옮겨주시는게 좋습니다.

[https://www.digitalocean.com/community/questions/how-to-move-the-default-var-lib-docker-to-another-directory-for-docker-on-linux](https://www.digitalocean.com/community/questions/how-to-move-the-default-var-lib-docker-to-another-directory-for-docker-on-linux)

{% embed url="https://www.digitalocean.com/community/questions/how-to-move-the-default-var-lib-docker-to-another-directory-for-docker-on-linux" %}

**Before you get started, make sure to backup your Droplet so that in case that anything goes wrong, you could revert back to a backup!**

Once you have your backup in place, follow these steps here:

* [SSH to your server](https://www.digitalocean.com/docs/droplets/how-to/connect-with-ssh/)
* Make sure that Docker is stopped:

```text
sudo systemctl stop docker
```

 Copy Copy

After that, make sure that Docker is not running with the following commands:

```text
sudo systemctl status docker
```

 Copy Copy

If you see that Docker is not running, then you could proceed. Another way of checking if there are any Docker processes is by using the `ps` command:

```text
ps faux | grep -i docker
```

 Copy Copy

* After that, copy the `/var/lib/docker/` Docker directory to the new location. Let’s say that we want to put the files in a folder called `/home/docker`. To do so, first create the folder:

```text
mkdir /home/docker
```

 Copy Copy

Then using the `rsync` command transfer the files over:

```text
rsync -avxP /var/lib/docker/ /home/docker
```

 Copy Copy

> Note: this might take a while depending on the size of your images. If your folder is too large you might want to run the rsync command in a [screen session](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-screen-on-an-ubuntu-cloud-server) to avoid your connection being dropped and interrupting the transfer.

* Next, you need to update the Docker unit file. To do that, using your favorite text editor, edit the following file:

```text
sudo nano /lib/systemd/system/docker.service
```

 Copy Copy

Find the following line:

```text
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```

And change it to:

```text
ExecStart=/usr/bin/dockerd -g /home/docker -H fd:// --containerd=/run/containerd/containerd.sock
```

Then reload the systemd daemons:

```text
sudo systemctl daemon-reload
```

 Copy Copy

And finally, start Docker:

```text
systemctl start docker
```

 Copy Copy

Finally, to confirm if your images are being loaded from the new path, you can inspect one of your images:

Find an image id:

```text
docker images
```

Inspect the image and look for the WorkDir:

```text
docker inspect image_id | grep WorkDir
```

I hope that this helps!  
Regards,  
Bobby



