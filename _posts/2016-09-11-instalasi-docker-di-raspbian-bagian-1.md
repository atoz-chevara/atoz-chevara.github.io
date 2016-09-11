---
layout: post
title: "Instalasi Docker di Raspbian - Bagian 1"
description: "Pemasangan Docker di Raspbian Jessie - Bagian 1"
modified: 2016-09-11
tags: [rasberry pi 3, raspberry pi, raspbian, jessie, docker]
comments: true
---

## Pemasangan Docker di Raspbian Jessie

### Apa Itu Docker ?

Docker adalah sebuah project open source yang ditujukan untuk developer atau sysadmin untuk membangun, 
mengemas dan menjalankan aplikasi dimana pun di dalam sebuah container.

### Pengaturan GPU Raspberry Pi
 
 Sebelum melakukan pemasangan Docker, kita atur memori GPU terlebih dahulu:

{% highlight bash %}
$ sudo raspi-config
{% endhighlight %}

 Pilih `9 Advanced Options > A3 Memory Split` saat muncul pertanyaan `How much memory should the GPU have?` 
 ubah nilai dari `64` ke `16` pilih `<OK>` dan `<Finish>`, jalankan ulang Raspberry Anda.
 
 Anda juga dapat melakukan cara berikut:
 
{% highlight bash %}
$ sudo nano /boot/config.txt
{% endhighlight %}

 Ubah nilai `gpu_mem` menjadi `gpu_mem=16`, jalankan ulang Raspberry Anda.
 
### Pemasangan Docker

 Selanjutnya kita pasang Docker, 
 
{% highlight bash %}
$ curl -sSL get.docker.com | sh
+ sudo -E sh -c mkdir -p /etc/systemd/system/docker.service.d
+ sudo -E sh -c echo '[Service]\nExecStart=\nExecStart=/usr/bin/dockerd --storage-driver overlay -H fd://' > /etc/systemd/system/docker.service.d/overlay.conf
+ sudo -E sh -c sleep 3; apt-get update
...
{% endhighlight %}

 Jalankan service docker:
 
{% highlight bash %}
$ sudo systemctl enable docker
$ sudo systemctl start docker
{% endhighlight %}

 Tambahkan user Anda ke dalam group docker sehingga tidak perlu menggunakan sudo saat menjalankan docker:
 
{% highlight bash %}
$ sudo usermod -aG docker useranda
{% endhighlight %}
 
 Untuk memastikan docker telah berjalan, jalankan perintah berikut:
 
{% highlight bash %}
$ docker version
Client:
 Version:      1.12.1
 API version:  1.24
 Go version:   go1.6.3
 Git commit:   23cf638
 Built:        Thu Aug 18 05:31:15 2016
 OS/Arch:      linux/arm

Server:
 Version:      1.12.1
 API version:  1.24
 Go version:   go1.6.3
 Git commit:   23cf638
 Built:        Thu Aug 18 05:31:15 2016
 OS/Arch:      linux/arm
{% endhighlight %}

{% highlight bash %}
$ docker info
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 1.12.1
Storage Driver: overlay
 Backing Filesystem: extfs
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
 Volume: local
 Network: null overlay host bridge
Swarm: inactive
Runtimes: runc
Default Runtime: runc
Security Options:
Kernel Version: 4.4.11-v7+
Operating System: Raspbian GNU/Linux 8 (jessie)
OSType: linux
Architecture: armv7l
CPUs: 4
Total Memory: 973.1 MiB
Name: collab
ID: 6OKE:4S5L:MCLQ:Y2W4:MPJR:K3O2:ZY3D:OYYA:V6GP:H2LT:FVDH:DYCJ
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/v1/
WARNING: No swap limit support
WARNING: No kernel memory limit support
WARNING: No cpu cfs quota support
WARNING: No cpu cfs period support
WARNING: No cpuset support
Insecure Registries:
 127.0.0.0/8
{% endhighlight %}

 Selamat, docker sudah berjalan di mesin raspi Anda.
 
### Memasang `docker-compose` dan `docker-machine`

 Untuk memasang `docker-compose` dan `docker-machine` kita memerlukan repositori Hypriot, jalankan perintah berikut:

{% highlight bash %}
$ curl -s https://packagecloud.io/install/repositories/Hypriot/Schatzkiste/script.deb.sh | sudo bash
Detected operating system as raspbian/jessie.
Checking for curl...
Detected curl...
Running apt-get update... done.
Installing apt-transport-https... done.
Installing /etc/apt/sources.list.d/Hypriot_Schatzkiste.list...done.
Importing packagecloud gpg key... done.
Running apt-get update... done.

The repository is setup! You can now install packages.
{% endhighlight %}

 Selanjutnya pasang paket `docker-compose` dan `docker-machine`:
 
{% highlight bash %}
$ sudo apt-get install git docker-compose docker-machine
{% endhighlight %}

 Periksa paket yang sudah dipasang:
 
{% highlight bash %}
$ docker-compose -v
docker-compose version 1.8.0, build 94f7016
{% endhighlight %}

{% highlight bash %}
$ docker-machine -v
docker-machine version 0.8.0, build b85aac1
{% endhighlight %}

 Selesai, pada artikel selanjutnya kita akan lanjutkan dengan membuat `image` dan push ke [Docker Hub](https://hub.docker.com), silahkan mendaftar terlebih dahulu.
