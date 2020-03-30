---
title: TLS 1.0 ve 1.1'i Redis için Azure Önbelleği ile kullanımdan kaldırın
description: Redis için Azure Önbelleği ile iletişim kurarken TLS 1.0 ve 1.1'i uygulamanızdan nasıl kaldıracağınız hakkında bilgi edinin
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: yegu
ms.openlocfilehash: 0d28469820f63f63089d9b91d57ccd7fe75c8b95
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80348650"
---
# <a name="remove-tls-10-and-11-from-use-with-azure-cache-for-redis"></a>TLS 1.0 ve 1.1'i Redis için Azure Önbelleği ile kullanımdan kaldırın

Transport Layer Security (TLS) sürüm 1.2 veya daha sonralarının özel kullanımına yönelik sektör çapında bir itme vardır. TLS 1.0 ve 1.1 sürümlerinin BEAST ve POODLE gibi saldırılara karşı hassas olduğu ve diğer Ortak Güvenlik Açıkları ve Pozlama (CVE) zayıflıklarına sahip olduğu bilinmektedir. Ayrıca, Ödeme Kartı Endüstrisi (PCI) uyumluluk standartları tarafından önerilen modern şifreleme yöntemlerini ve şifre paketlerini de desteklemezler. Bu [TLS güvenlik blogu](https://www.acunetix.com/blog/articles/tls-vulnerabilities-attacks-final-part/) bu güvenlik açıklarından bazılarını daha ayrıntılı olarak açıklar.

Bu çabanın bir parçası olarak, Redis için Azure Önbelleğinde aşağıdaki değişiklikleri yapacağız:

* **Aşama 1:** Yeni oluşturulan önbellek örnekleri için varsayılan minimum TLS sürümünü 1,2 olarak yapılandırırız. (Bu tls 1.0 için kullanılır.) Varolan önbellek örnekleri bu noktada güncelleştirilmez. Gerekirse geriye dönük uyumluluk için [minimum TLS sürümünü](cache-configure.md#access-ports) 1,0 veya 1,1 olarak değiştirebilirsiniz. Bu değişiklik Azure portalı veya diğer yönetim API'leri aracılığıyla yapılabilir.
* **Aşama 2:** TLS 1.0 ve 1.1 sürümlerini desteklemeyi bırakacağız. Bu değişiklikten sonra, uygulamanızın önbelleğinizle iletişim kurmak için TLS 1.2 veya daha sonra kullanması gerekir.

Ayrıca, bu değişikliğin bir parçası olarak, eski, güvensiz cypher paketleri için desteği kaldıracağız.  Desteklenen cypher paketlerimiz, önbellek en az 1,2 TLS sürümüyle yapılandırıldığında aşağıdakilerle sınırlandırılacaktır.

* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P384
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256

Bu makalede, bu önceki TLS sürümlerindeki bağımlılıkların nasıl algılanış edilebildiğini ve bunları uygulamanızdan nasıl kaldırılacak hakkında genel kılavuz lar verilmektedir.

Bu değişikliklerin yürürlüğe girdiği tarihler şunlardır:

| Bulut               | Faz 1 Başlangıç Tarihi | Aşama 2 Başlangıç Tarihi      |
|---------------------|--------------------|-------------------------|
| Azure (genel)      |  13 Ocak 2020  | 11 Mayıs 2020 (genişletilmiş) |
| Azure Kamu    |  Mart 13, 2020    | Mayıs 11, 2020            |
| Azure Almanya       |  Mart 13, 2020    | Mayıs 11, 2020            |
| Azure Çin         |  Mart 13, 2020    | Mayıs 11, 2020            |

## <a name="check-whether-your-application-is-already-compliant"></a>Uygulamanızın zaten uyumlu olup olmadığını kontrol edin

Uygulamanızın TLS 1.2 ile çalışıp çalışmayacağını öğrenmenin en kolay yolu, kullandığı test veya evreleme önbelleğinde **Minimum TLS sürüm** değerini TLS 1.2 olarak ayarlamaktır. **Minimum TLS sürüm** ayarı, Azure portalındaki önbellek örneğinizin Gelişmiş [ayarlarındadır.](cache-configure.md#advanced-settings) Uygulama bu değişiklikten sonra beklendiği gibi çalışmaya devam ederse, büyük olasılıkla uyumludur. Uygulamanız tarafından özellikle TLS 1.2'yi etkinleştirmek için kullanılan bazı Redis istemci kitaplıklarını yapılandırmanız gerekebilir, böylece bu güvenlik protokolü üzerinden Redis için Azure Önbelleği'ne bağlanabilirler.

## <a name="configure-your-application-to-use-tls-12"></a>UYGULAMANIZI TLS 1.2 kullanacak şekilde yapılandırın

Çoğu uygulama, önbellekleriyle iletişimi işlemek için Redis istemci kitaplıklarını kullanır. TlS 1.2'yi kullanmak üzere çeşitli programlama dillerinde ve çerçevelerinde popüler istemci kitaplıklarından bazılarını yapılandırma yönergeleri aşağıda veda edilmiştir.

### <a name="net-framework"></a>.NET Framework

Redis .NET istemcileri ,NET Framework 4.5.2 veya daha önce varsayılan olarak en erken TLS sürümünü kullanır ve .NET Framework 4.6 veya sonraki sürümdeki en son TLS sürümünü kullanır. .NET Framework'ün eski bir sürümünü kullanıyorsanız, TLS 1.2'yi el ile etkinleştirebilirsiniz:

* **StackExchange.Redis:** Ayarlayın `ssl=true` `sslprotocols=tls12` ve bağlantı dizesinde.
* **ServiceStack.Redis:** [ServiceStack.Redis yönergeleriizleyin.](https://github.com/ServiceStack/ServiceStack.Redis/pull/247)

### <a name="net-core"></a>.NET Core

Redis .NET Core istemcileri varsayılan olarak en son TLS sürümünü kullanır.

### <a name="java"></a>Java

Redis Java istemcileri, Java sürüm 6 veya daha önce TLS 1.0 kullanır. TLS 1.0 önbellekte devre dışı bırakılırsa Jedi'lar, Marul ve Redisson Redis için Azure Önbelleğine bağdaştırılamaz. Yeni TLS sürümlerini kullanmak için Java çerçevenizi yükseltin.

Java 7 için Redis istemcileri TLS 1.2'yi varsayılan olarak kullanmaz, ancak bunun için yapılandırılabilir. Jedi'lar aşağıdaki kod snippet ile temel TLS ayarlarını belirtmenizi sağlar:

``` Java
SSLSocketFactory sslSocketFactory = (SSLSocketFactory) SSLSocketFactory.getDefault();
SSLParameters sslParameters = new SSLParameters();
sslParameters.setEndpointIdentificationAlgorithm("HTTPS");
sslParameters.setProtocols(new String[]{"TLSv1.2"});
 
URI uri = URI.create("rediss://host:port");
JedisShardInfo shardInfo = new JedisShardInfo(uri, sslSocketFactory, sslParameters, null);
 
shardInfo.setPassword("cachePassword");
 
Jedis jedis = new Jedis(shardInfo);
```

Marul ve Redisson istemcileri TLS sürümünü belirtmeyi henüz desteklemez, bu nedenle önbellek yalnızca TLS 1.2 bağlantılarını kabul ederse kırılırlar. Bu istemcilerin düzeltmeleri gözden geçiriliyor, bu nedenle bu destekle güncelleştirilmiş bir sürüm için bu paketleri kontrol edin.

Java 8'de TLS 1.2 varsayılan olarak kullanılır ve çoğu durumda istemci yapılandırmanızda güncelleştirmeler gerektirmemelidir. Güvende olmak için, uygulamanızı test edin.

### <a name="nodejs"></a>Node.js

Düğüm Redis ve IORedis varsayılan olarak TLS 1.2 kullanır.

### <a name="php"></a>PHP

#### <a name="predis"></a>Predis
 
* PHP 7'den önceki sürümler: Predis sadece TLS 1.0'ı destekler. Bu sürümler TLS 1.2 ile çalışmaz; TLS 1.2 kullanmak için yükseltme niz gerekir.
 
* PHP 7.0 - PHP 7.2.1: Predis varsayılan olarak sadece TLS 1.0 veya 1.1 kullanır. TLS 1.2'yi kullanmak için aşağıdaki geçici çözüm İstemci örneğini oluştururken TLS 1.2 belirtin:

  ``` PHP
  $redis=newPredis\Client([
      'scheme'=>'tls',
      'host'=>'host',
      'port'=>6380,
      'password'=>'password',
      'ssl'=>[
          'crypto_type'=>STREAM_CRYPTO_METHOD_TLSv1_2_CLIENT,
      ],
  ]);
  ```

* PHP 7.3 ve sonraki sürümler: Predis en son TLS sürümünü kullanır.

#### <a name="phpredis"></a>PhpRedis

PhpRedis herhangi bir PHP sürümünde TLS desteklemiyor.

### <a name="python"></a>Python

Redis-py varsayılan olarak TLS 1.2 kullanır.

### <a name="go"></a>GO

Redigo varsayılan olarak TLS 1.2 kullanır.

## <a name="additional-information"></a>Ek bilgiler

- [Redis için Azure Önbelleği nasıl yapılandırılır?](cache-configure.md)
