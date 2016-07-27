---
layout: post
title: "Menambahkan Notes di tema Jekyll HPSTR"
description: "Modifikasi tema Jekyll HPSTR."
modified: 2016-07-12
tags: [theme, jekyll, hpstr]
image:
  background: triangular.png
comments: true
---

## Modifikasi tema Jekyll HPSTR

 Sebelum menambahkan class notes pada css di tema HPSTR yang saya gunakan, saya sebelumnya menggunakan
 sintak markdown `blockquotes` untuk menampilkan keterangan ataupun catatan, setelah saya ngulik sedikit dokumentasi GitHub,
 akhirnya saya menambahkan berkas baru untuk pada direktori `_sass/` dan menamakan berkas tersebut
 dengan `_alert.scss`.
 
 isinya seperti berikut:
 
 <script src="https://gist.github.com/atoz-chevara/0b1ff1ae181763868e3b1b93aecaf39a.js"></script>
 
 Setelah berkas dibuat, maka kita perlu menyunting berkas `assets/css/main.scss` untuk memanggil berkas yang
 telah dibuat sebelumnya dengan menambahkan `@import "alert";` pada akhir baris.

{% highlight bash %}
$ echo "@import "alert";" >> assets/css/main.scss
{% endhighlight %}
 
 Hasilnya akan seperti ini:

{% highlight bash %}
$ cat assets/css/main.scss
{% endhighlight %}

{% highlight scss %}
---
sitemap: false
---

/*
 *
 *  HPSTR
 *
 *  Designer: Michael Rose
 *  Twitter: http://twitter.com/mmistakes
 *
*/

// Partials
@import "variables";
@import "mixins";
@import "reset";
@import "vendor/font-awesome/font-awesome";
@import "vendor/magnific-popup/magnific-popup";
@import "site";
@import "typography";
@import "syntax";
@import "coderay";
@import "gist";
@import "grid";
@import "elements";
@import "alert";
@import "animations";
@import "dl-menu";
@import "page";
{% endhighlight %}

Mari kita lihat hasil dari penambahan `css` yang sudah dibuat:

{% highlight html %}
<div class="alert alert-note"><strong>Catatan:</strong>
<ul>
<li>Item 1.</li>
<li>Item 2.</li>
<li>Item 3.</li>
<li>Item 4.</li>
</ul>
</div>
{% endhighlight %}

Output:

<div class="alert alert-note"><strong>Catatan:</strong>
<ul>
<li>Item 1.</li>
<li>Item 2.</li>
<li>Item 3.</li>
<li>Item 4.</li>
</ul>
</div>

Selesai, semoga bisa lebih bersemangat berbagi pengetahuan.

