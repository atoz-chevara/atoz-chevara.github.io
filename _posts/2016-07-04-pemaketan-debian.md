---
layout: post
title: Pemaketan Debian
description: "Pedoman pemaketan dasar Debian."
tags: [debian, blankon]
image:
  background: triangular.png
comments: true
---

## Pedoman pemaketan dasar Debian.
 
### Persiapan.

#### Pemasangan paket untuk membuat paket Debian.

{% highlight bash %}
$ sudo apt-get install -y devscripts build-essential fakeroot debhelper \
gnupg pbuilder dh-make dpkg-dev ubuntu-dev-tools \
autotools-dev lintian haveged
{% endhighlight %}

#### Konfigurasi awal informasi tentang pemaket.

Mengatur variabel $DEBFULLNAME dan $DEBMAIL pada lingkungan shell sehingga berbagai alat-alat pemaketan Debian mengenali **Nama** dan **Alamat Email** Anda untuk memaketkan paket.

{% highlight bash %}
$ echo 'export DEBFULLNAME="Nama Anda"' >> ~/.bashrc
$ echo 'export DEBMAIL="email@anda.com"' >> ~/.bashrc

$ source ~/.bashrc
{% endhighlight %}

 atau

{% highlight bash %}
$ echo 'export DEBFULLNAME="Nama Anda"' >> ~/.profile
$ echo 'export DEBMAIL="email@anda.com"' >> ~/.profile

$ source ~/.profile
{% endhighlight %}

 atau	

{% highlight bash %}
$ . ~/.bashrc
{% endhighlight %}

 atau

{% highlight bash %}
$ . ~/.profile
{% endhighlight %}

atau bisa juga seperti ini:

{% highlight bash %}
$ cat >> ~/.bashrc <<EOF
DEBFULLNAME="Nama Anda"
DEBMAIL="email@anda.com"
export DEBFULLNAME DEBMAIL
EOF
$ . ~/.bashrc
{% endhighlight %}

 Periksa hasilnya

{% highlight bash %}
$ echo $DEBFULLNAME ; echo $DEBMAIL
$ export | grep DEB
$ grep DEB* ~/.bashrc
$ grep DEB* ~/.profile
{% endhighlight %}

 Hasilnya akan menampilkan informasi tentang pemaket, pilih salah satu antara merubah berkas 
 *.bashrc* atau *.profile* karena Banyak jalan menuju Roma :)
 
#### Pembuatan kunci GnuPG (GnuPrivacyGuard).

{% highlight bash %}
$ gpg --gen-key
{% endhighlight %}		

 Pilih: *(1) RSA and RSA (default) -> 4096 -> (0) -> y -> (O)kay*
 
#### Unduh paket sumber.

 Anda bisa memilih paket apa yang mau di buatkan versi Debiannya, disini saya 
 memilih paket **[GNU ed](http://ftp.gnu.org/gnu/ed/)**. Saat artikel ini ditulis 
 versi terakhir adalah 1.9.
 
 Mulai mengunduh paket sumber

{% highlight bash %}
$ wget http://ftp.gnu.org/gnu/ed/ed-1.9.tar.gz
{% endhighlight %}

 Ekstrak paket sumber dan masuk ke direktori hasil ekstrak

{% highlight bash %}
$ tar zxvf ed-1.9.tar.gz
$ cd ed-1.9
{% endhighlight %}

 Kita dapat melihat struktur direktorinya dengan perintah *tree*

{% highlight bash %}
$ tree -d ../
../
`-- ed-1.9
	|-- doc
	`-- testsuite
{% endhighlight %}

 Bila perintah tidak ditemukan silahkan pasang paketnya

{% highlight bash %}
$ sudo apt-get install -y tree
{% endhighlight %}

### Memulai membangun paket.

 Mari kita mulai membangun paket yang telah kita pilih sebelumnya

{% highlight bash %}
$ dh_make -e $DEBMAIL -c gpl3 -s -f ../ed-1.9.tar.gz
{% endhighlight %}

 Kita bisa melihat perubahan yang terjadi pada struktur direktorinya

{% highlight bash %}
$ tree -d -D -t ../
../
`-- [Jul  3  3:41]  ed-1.9
	|-- [Jul  3  3:38]  doc
	|-- [Jul  3  3:38]  testsuite
	`-- [Jul  3  3:41]  debian
		`-- [Jul  3  3:41]  source


$ tree -D -t debian/
debian/
|-- [Jul  3  3:41]  README.Debian
|-- [Jul  3  3:41]  README.source
|-- [Jul  3  3:41]  changelog
|-- [Jul  3  3:41]  compat
|-- [Jul  3  3:41]  control
|-- [Jul  3  3:41]  copyright
|-- [Jul  3  3:41]  docs
|-- [Jul  3  3:41]  ed.cron.d.ex
|-- [Jul  3  3:41]  ed.default.ex
|-- [Jul  3  3:41]  ed.doc-base.EX
|-- [Jul  3  3:41]  info
|-- [Jul  3  3:41]  init.d.ex
|-- [Jul  3  3:41]  manpage.1.ex
|-- [Jul  3  3:41]  manpage.sgml.ex
|-- [Jul  3  3:41]  manpage.xml.ex
|-- [Jul  3  3:41]  menu.ex
|-- [Jul  3  3:41]  postinst.ex
|-- [Jul  3  3:41]  postrm.ex
|-- [Jul  3  3:41]  preinst.ex
|-- [Jul  3  3:41]  prerm.ex
|-- [Jul  3  3:41]  rules
|-- [Jul  3  3:41]  source
|   `-- [Jul  3  3:41]  format
`-- [Jul  3  3:41]  watch.ex


$ ls ../
ed-1.9  ed-1.9.tar.gz  ed_1.9.orig.tar.gz
{% endhighlight %}

 Dari hasil perintah *tree* kita bisa melihat ada penambahan direktori *debian* dan *debian/source* 
 dan juga salinan dari paket sumber yaitu *ed_1.9.orig.tar.gz*.

 Mari kita hapus beberapa berkas yang tidak diperlukan pada direktori *debian*.

{% highlight bash %}
$ cd debian
$ rm *.ex *.EX README.* docs info
{% endhighlight %}

 Hasilnya akan seperti ini

{% highlight bash %}
$ tree -D ../debian/
../debian/
|-- [Jul  3  3:41]  changelog
|-- [Jul  3  3:41]  compat
|-- [Jul  3  3:41]  control
|-- [Jul  3  3:41]  copyright
|-- [Jul  3  3:41]  rules
`-- [Jul  3  3:41]  source
	`-- [Jul  3  3:41]  format
{% endhighlight %}

 Pada tahap ini kita akan merubah beberapa berkas yang ada di direktori *debian* diawali 
 dengan berkas *changelog*, sebagai catatan...berkas-berkas ini disunting sesuai dengan 
 ketentuan pemaketan Debian, sehingga kita tidak bisa seenaknya menyunting berkas yang ada.
 
 Mari kita mulai menyunting berkas *changelog* ala Debian :)

{% highlight bash %}
$ dch -e

ed (1.9-1) unstable; urgency=low

  * Initial release (Closes: #nnnn)  <nnnn is the bug number of your ITP>

 -- Nama Anda <email@anda.com>  Sun, 03 Jul 2016 03:41:14 +0700
{% endhighlight %}

 Menjadi

{% highlight bash %}
ed (1.9-0blankon1) tambora; urgency=low

  * Initial release for Blankon Tambora

 -- Nama Anda <email@anda.com>  Sun, 03 Jul 2016 03:41:14 +0700
{% endhighlight %}

 Catatan:
 
1. Perintah *dch -e* digunakan untuk penyuntingan awal.
2. Perintah *dch -i* digunakan untuk penyuntingan selanjutnya ataupun pemaket lainnya.
 
 Sunting berkas *control* dengan text editor favorit.

{% highlight bash %}
$ cd ..
$ nano debian/control
		
Source: ed
Section: editors
Priority: optional
Maintainer: Nama Anda <email@anda.com>
Build-Depends: debhelper (>= 9), autotools-dev
Standards-Version: 3.9.5
Homepage: http://www.gnu.org/software/ed/
Vcs-Git: cvs.savannah.gnu.org:/web/ed
Vcs-Browser: http://web.cvs.savannah.gnu.org/viewvc/?root=ed

Package: ed
Architecture: any
Depends: ${shlibs:Depends}, ${misc:Depends}
Description: GNU ed is a line-oriented text editor.
 GNU ed is a line-oriented text editor. It is used to create, display,
 modify and otherwise manipulate text files, both interactively and via
 shell scripts. A restricted version of ed, red, can only edit files in
 the current directory and cannot execute shell commands.
 .
 Ed is the "standard" text editor in the sense that it is the original
 editor for Unix, and thus widely available. For most purposes, however,
 it is superseded by full-screen editors such as GNU Emacs or GNU Moe.
 .
 Extensions to and deviations from the POSIX standard are described below.
{% endhighlight %}

 Pada berkas ini saya hanya melakukan penyuntingan pada bagian *Section*, *Homepage*, 
 *Vcs-Git*, *Vcs-Browser* dan *Description*. silahkan lihat informasi paket **GNU ed** 
 pada berkas README, untuk informasi lengkap paket ini bisa merujuk pada websitenya.

 Langkah berikutnya kita akan menyunting berkas *copyright*, berikut caranya

{% highlight bash %}
$ nano debian/copyright

Format: http://www.debian.org/doc/packaging-manuals/copyright-format/1.0/
Upstream-Name: ed
Source: http://www.gnu.org/software/ed/

Files: *
Copyright: 2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, Free Software Foundation, Inc.
	   2006, 2007, 2008, 2009, 2010, 2012, 2013, Antonio Diaz Diaz <ant_diaz@teleline.es>
	   1993, Francois Pinard <pinard@icule>
	   1993-1994, Andrew Moore <alm@woops.talke.org>
License: GPL-3.0+

Files: debian/*
Copyright: 2016 Nama Anda <email@anda.com>
License: GPL-3.0+

License: GPL-3.0+
 This program is free software: you can redistribute it and/or modify
 it under the terms of the GNU General Public License as published by
 the Free Software Foundation, either version 3 of the License, or
 (at your option) any later version.
 .
 This package is distributed in the hope that it will be useful,
 but WITHOUT ANY WARRANTY; without even the implied warranty of
 MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 GNU General Public License for more details.
 .
 You should have received a copy of the GNU General Public License
 along with this program. If not, see <http://www.gnu.org/licenses/>.
 .
 On Debian systems, the complete text of the GNU General
 Public License version 3 can be found in "/usr/share/common-licenses/GPL-3".
{% endhighlight %}

 Informasi lisensi bisa dilihat di berkas *AUTHORS* dan *ChangeLog*. Saya hanya 
 menyunting bagian *Upstream-Name* dan *Copyright*.

 Lakukan pengecekan dengan perintah *cme*, bila tidak tersedia silahkan pasang 
 terlebih dahulu

{% highlight bash %}
$ sudo apt-get install -y libconfig-model-dpkg-perl
$ cme check dpkg-copyright debian/copyright
loading data
checking data
check done
{% endhighlight %}

 Apabila hasilnya tidak seperti diatas maka lakukan penyuntingan kembali sesuai 
 dengan output dari perintah *cme*. Langkah selanjutnya kita akan mulai membuat paket Debian.

{% highlight bash %}
$ dpkg-buildpackage -rfakeroot
{% endhighlight %}

 Lihat hasilnya

{% highlight bash %}
$ ls ../ | grep ed
ed-1.9
ed-1.9.tar.gz
ed_1.9-0blankon1.debian.tar.xz
ed_1.9-0blankon1.dsc
ed_1.9-0blankon1_armhf.changes
ed_1.9-0blankon1_armhf.deb
ed_1.9.orig.tar.gz
{% endhighlight %}

 Periksa paket dengan menggunakan *lintian*	

{% highlight bash %}
$ lintian -iIEv --pedantic ../*.changes
{% endhighlight %}

 Selesai.

