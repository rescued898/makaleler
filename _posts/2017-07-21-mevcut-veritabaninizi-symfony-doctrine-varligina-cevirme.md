---
ID: 9325
post_title: >
  Mevcut Veritabanınızı Symfony
  Doctrine Varlığına Çevirme
author: Oral ÜNAL
post_excerpt: ""
layout: post
permalink: >
  https://www.trkodlama.com/makaleler/mevcut-veritabaninizi-symfony-doctrine-varligina-cevirme-9325.html
published: true
post_date: 2017-07-21 03:42:09
---
Başlık doğru mu oldu bilmiyorum fakat işin özü şu: Şahsen ben veritabanı tablolarımı Entity kullanarak hazırlamaya üşendim. Onun yerine phpMyAdmin kullanarak veritabanımı oluşturdum. Yani elimde yeni bir veritabanı var. Fakat bu sizin halihazırda kullandığınız veritabanı da olabilir. Problem değil. İkisi de aynı mantık.

Şimdi elimizdeki veritabanını inceleyerek Symfony bizim için Entity'leri otomatik olarak oluşturacak. Bunun için aşağıdaki adımları takip edin:
<h4>Adım 1 - parameters.yml Dosyanızı Güncelleyin</h4>
<em>app/config/parameters.yml</em> dosyanızda veritabanı bilgilerinizi güncelleyin.
<h4>Adım 2 - Terminali açın</h4>
Terminali açın ve proje klasörünüzün içine gelin. Daha sonra aşağıdaki komutu çalıştırın:
<pre class="prettyprint lang-sh" data-start-line="1" data-visibility="visible" data-highlight="" data-caption="">php bin/console doctrine:mapping:import --force AppBundle yml</pre>
Bu işlem <em>src/AppBundle/Resources/config/</em> dizini içerisine .yml uzantılı olarak veritabanınızı dönüştürmüş olacak. Mevcut tablolarınızın hepsini burada görebilirsiniz.
<h4>Adım 3 - YML dosyalarını Annotation olarak güncelleyelim</h4>
Bu işlem ile <em>src/AppBundle/Entity/</em> klasörü içerisinde bütün varlıklarınız oluşturulacak
<pre class="prettyprint lang-sh" data-start-line="1" data-visibility="visible" data-highlight="" data-caption="">php bin/console doctrine:mapping:convert annotation ./src</pre>
Eveett, varlıklarımız oluşturuldu.
<h4>Adım 4 - Oluşturduğumuz YML dosyalarını silelim</h4>
<em>src/AppBundle/Resources/config/</em> klasörü içerisinde az önce oluşturduğumuz <em><strong>.yml</strong></em> uzantılı dosyaları kaldıralım. Bu dosyalar çakışmaya sebep oluyor.

İşlem bu kadar basit arkadaşlar. Temsili bir varlık dosyası paylaşmak istiyorum:
<pre class="lang:php decode:true prettyprint lang-php">&lt;?php
// src/AppBundle/Entity/BlogComment.php
namespace AppBundleEntity;

use DoctrineORMMapping as ORM;

/**
 * @ORMTable(name="blog_comment")
 * @ORMEntity
 */
class BlogComment
{
    /**
     * @var integer $id
     *
     * @ORMColumn(name="id", type="bigint")
     * @ORMId
     * @ORMGeneratedValue(strategy="IDENTITY")
     */
    private $id;

    /**
     * @var string $author
     *
     * @ORMColumn(name="author", type="string", length=100, nullable=false)
     */
    private $author;

    /**
     * @var text $content
     *
     * @ORMColumn(name="content", type="text", nullable=false)
     */
    private $content;

    /**
     * @var datetime $createdAt
     *
     * @ORMColumn(name="created_at", type="datetime", nullable=false)
     */
    private $createdAt;

    /**
     * @var BlogPost
     *
     * @ORMManyToOne(targetEntity="BlogPost")
     * @ORMJoinColumn(name="post_id", referencedColumnName="id")
     */
    private $post;
}</pre>
<h4>Sonuç</h4>
Yani her tablo için Annotation'ı anlayıp yazarak uğraşmamıza gerek yok :) Symfony bizim için herşeyi halletmiş. Aslında Doctrine halletmiş desek daha doğru olur.

Kaynak: https://symfony.com/doc/current/doctrine/reverse_engineering.html