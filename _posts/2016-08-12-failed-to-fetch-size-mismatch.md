---
layout: post
title: "E: Failed to fetch - Size mismatch"
description: "Mengatasi Galat apt-get/aptitude"
modified: 2016-08-12
tags: [debian, apt, aptitude, galat, error]
comments: true
---

## Mengatasi Galat apt-get/aptitude "E: Failed to fetch - Size mismatch"

 Terkadang saat memasang aplikasi kita menemukan pesan galat yang beraneka ragam,
 salah satunya seperti ini:

{% highlight bash %}
$ sudo aptitude install tree
The following NEW packages will be installed:
  tree 
0 packages upgraded, 1 newly installed, 0 to remove and 63 not upgraded.
Need to get 47,9 kB of archives. After unpacking 137 kB will be used.
Get: 1 http://kambing.ui.ac.id/debian/ jessie/main tree i386 1.7.0-3 [47,9 kB]
Fetched 449 B in 0s (4.333 B/s)
E: Failed to fetch http://kambing.ui.ac.id/debian/pool/main/t/tree/tree_1.7.0-3_i386.deb: Size mismatch
{% endhighlight %}

 Hal ini terjadi karena beberapa hal, seperti koneksi yang buruk, pembatalan pemasangan secara tiba-tiba dll.
 Untuk mengatasi hal ini, Anda cukup menghapus paket yang gagal terpasang pada direktori arsip apt:

{% highlight bash %}
$ sudo rm /var/cache/apt/archives/partial/tree_1.7.0-3_i386.deb.FAILED
{% endhighlight %}

 Silahkan mencoba kembali:

{% highlight bash %}
$ sudo aptitude install tree
{% endhighlight %}

 Pada kasus yang saya alami, berhubung menggunakan `apt-cacher-ng` sebagai proxy apt agar perangkat
 lainnya dapat memasang aplikasi melalui proxy apt yang telah saya buat, maka saya perlu menghapus
 paket yang gagal pada arsip `apt-cacher-ng`:

{% highlight bash %}
$ sudo rm /var/cache/apt-cacher-ng/debrep/pool/main/t/tree/tree_1.7.0-3*
{% endhighlight %}

<div class="alert alert-note"><strong>Catatan:</strong>
<ul>
Dilain kesempatan saya akan menulis tentang <em>apt-cacher-ng</em> ini.
</ul>
</div>

