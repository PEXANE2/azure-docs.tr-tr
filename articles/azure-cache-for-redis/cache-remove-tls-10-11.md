---
title: TLS 1,0 ve 1,1 ' i Redsıs için Azure Cache ile birlikte kullanarak kaldırma
description: Redsıs için Azure önbelleğiyle iletişim kurarken uygulamanızın TLS 1,0 ve 1,1 ' i nasıl kaldıracağınızı öğrenin
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: yegu
ms.openlocfilehash: 69df5a65df99a7497099e71e9f41701458370c87
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84423930"
---
# <a name="remove-tls-10-and-11-from-use-with-azure-cache-for-redis"></a>TLS 1,0 ve 1,1 ' i Redsıs için Azure Cache ile birlikte kullanarak kaldırma

Aktarım Katmanı Güvenliği (TLS) sürüm 1,2 veya üzeri için özel kullanıma yönelik sektör genelinde bir gönderim vardır. TLS sürümleri 1,0 ve 1,1, BEAST ve POOTEKı gibi saldırılara açıktır ve diğer yaygın güvenlik açıkları ve Etkilenmeler (CVE) zayıflıklarına sahip olur. Ayrıca, ödeme kartı sektör (PCI) uyumluluk standartları tarafından önerilen modern şifreleme yöntemlerini ve şifre paketlerini desteklemezler. Bu [TLS güvenlik blogu](https://www.acunetix.com/blog/articles/tls-vulnerabilities-attacks-final-part/) , bu güvenlik açıklarından bazılarını daha ayrıntılı bir şekilde açıklamaktadır.

Bu çaba kapsamında, Redsıs için Azure önbelleğinde aşağıdaki değişiklikleri yapacağız:

* **1. Aşama:** Yeni oluşturulan önbellek örnekleri için varsayılan en düşük TLS sürümünü 1,2 olarak yapılandıracağız (daha önce TLS 1,0).  Mevcut önbellek örnekleri bu noktada güncelleştirilmeyecek. Gerekirse, geriye doğru uyumluluk için [En düşük TLS sürümünü](cache-configure.md#access-ports) yeniden 1,0 veya 1,1 olarak değiştirebilirsiniz. Bu değişiklik Azure portal veya diğer yönetim API 'Leri aracılığıyla yapılabilir.
* **2. Aşama:** 1,0 ve 1,1 TLS sürümlerini desteklemeye başlayacağız. Bu değişiklikten sonra, uygulamanız önbelleğiyle iletişim kurmak için TLS 1,2 veya sonraki bir sürümü kullanmanız gerekecektir.

Ayrıca, bu değişikliğin bir parçası olarak, eski, güvenli olmayan şifresi üzerinde anlaşılamadı paketleri desteğini kaldıracağız.  Önbellek en düşük TLS sürümü 1,2 ile yapılandırıldığında desteklenen şifresi üzerinde anlaşılamadı paketlerimiz aşağıdakiler ile kısıtlanır.

* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P384
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256

Bu makalede, bu önceki TLS sürümlerindeki bağımlılıkların nasıl algılanacağı ve uygulamanızdan kaldırılacağı hakkında genel yönergeler sunulmaktadır.

Bu değişikliklerin etkili olması için tarihler şunlardır:

| Bulut                | Aşama 1 başlangıç tarihi | 2. aşama başlangıç tarihi         |
|----------------------|--------------------|----------------------------|
| Azure (genel)       |  13 Ocak 2020  | COVıD 19 nedeniyle ertelendi  |
| Azure Kamu     |  13 Mart 2020    | COVıD 19 nedeniyle ertelendi  |
| Azure Almanya        |  13 Mart 2020    | COVıD 19 nedeniyle ertelendi  |
| Azure China 21Vianet |  13 Mart 2020    | COVıD 19 nedeniyle ertelendi  |

Not: 2. aşama için yeni tarih henüz belirlenmedi

## <a name="check-whether-your-application-is-already-compliant"></a>Uygulamanızın zaten uyumlu olup olmadığını denetleyin

Uygulamanızın TLS 1,2 ile çalışıp çalışmadığını bulmanın en kolay yolu, **En düşük TLS sürümü** değerini bir test veya HAZıRLAMA önbelleğinde TLS 1,2 olarak ayarlamak ve ardından testleri çalıştırmak için kullanılır. **En düşük TLS sürümü** ayarı, Azure Portal önbellek örneğinizin [Gelişmiş ayarlarından](cache-configure.md#advanced-settings) oluşur.  Bu değişiklikten sonra uygulama beklendiği gibi çalışmaya devam ederse büyük olasılıkla uyumludur. Redsıs için Azure önbelleğine bağlanmak üzere TLS 1,2 ' i etkinleştirmek için, uygulamanız tarafından kullanılan Redsıs istemci kitaplığını yapılandırmanız gerekebilir.

## <a name="configure-your-application-to-use-tls-12"></a>Uygulamanızı TLS 1,2 kullanacak şekilde yapılandırma

Çoğu uygulama, önbellekleri ile iletişimi işlemek için Redsıs istemci kitaplıklarını kullanır. Aşağıda, çeşitli programlama dillerinde ve çerçevelerinizdeki bazı popüler istemci kitaplıklarını, TLS 1,2 kullanacak şekilde yapılandırma yönergeleri verilmiştir.

### <a name="net-framework"></a>.NET Framework

Redsıs .NET istemcileri, en eski TLS sürümünü varsayılan olarak .NET Framework 4.5.2 veya önceki sürümlerde kullanır ve .NET Framework 4,6 veya sonraki sürümlerde en son TLS sürümünü kullanır. .NET Framework eski bir sürümünü kullanıyorsanız, TLS 1,2 'yi el ile etkinleştirebilirsiniz:

* **StackExchange. Reddir:** `ssl=true` `sslprotocols=tls12` Bağlantı dizesinde ve ayarlanır.
* **Servicestack. redsıs:** [Servicestack. redin](https://github.com/ServiceStack/ServiceStack.Redis#servicestackredis-ssl-support) talimatlarını izleyin ve en azından Servicestack. redsıs v 5.6 'yı gerektirir.

### <a name="net-core"></a>.NET Core

Redis .NET Core istemcileri varsayılan işletim sistemi varsayılan TLS sürümüne göre değişir. 

İşletim sistemi sürümüne ve uygulanan tüm düzeltme eklerine bağlı olarak, geçerli varsayılan TLS sürümü farklılık gösterebilir. Bu bilgiler hakkında bir bilgi kaynağı olmakla kalmaz, Windows için bir makale [aşağıda](https://docs.microsoft.com/dotnet/framework/network-programming/tls#support-for-tls-12) verilmiştir. 

Ancak, eski bir işletim sistemi kullanıyorsanız veya yalnızca emin olmak istiyorsanız, tercih edilen TLS sürümünü istemci aracılığıyla el ile yapılandırmanızı öneririz.


### <a name="java"></a>Java

Redsıs Java istemcileri Java sürüm 6 veya daha önceki bir sürümünde TLS 1,0 kullanır. TLS 1,0, önbellekte devre dışı bırakılmışsa, jedsıs, Lettuce ve Redisson, redin için Azure önbelleğine bağlanamaz. Yeni TLS sürümlerini kullanmak için Java çatısını yükseltin.

Java 7 için Redsıs istemcileri varsayılan olarak TLS 1,2 kullanmaz ancak bu için yapılandırılabilir. Jedsıs, aşağıdaki kod parçacığı ile temeldeki TLS ayarlarını belirtmenize olanak sağlar:

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

Letgce ve Redisson istemcileri henüz TLS sürümünün belirtilmesini desteklemez, bu nedenle önbellek yalnızca TLS 1,2 bağlantısı kabul ediyorsa kesilir. Bu istemcilere yönelik düzeltmeler incelenmektedir, bu nedenle bu desteğe sahip güncelleştirilmiş bir sürüm için bu paketlere danışın.

Java 8 ' de, TLS 1,2 varsayılan olarak kullanılır ve çoğu durumda istemci yapılandırmanızda güncelleştirmeler gerektirmemelidir. Güvenli olması için uygulamanızı test edin.

### <a name="nodejs"></a>Node.js

Redsıs ve ıoredin Node, varsayılan olarak TLS 1,2 kullanır.

### <a name="php"></a>PHP

#### <a name="predis"></a>Ön dış
 
* PHP 7 ' den önceki sürümler: Predıs yalnızca TLS 1,0 ' i destekler. Bu sürümler TLS 1,2 ile çalışmaz; TLS 1,2 ' i kullanmak için ' i yükseltmeniz gerekir.
 
* PHP 7,0 ile PHP 7.2.1: Predıs, varsayılan olarak yalnızca TLS 1,0 veya 1,1 kullanır. TLS 1,2 ' i kullanmak için aşağıdaki geçici çözümü kullanabilirsiniz. İstemci örneğini oluştururken TLS 1,2 belirtin:

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

* PHP 7,3 ve üzeri sürümler: Predıs, en son TLS sürümünü kullanır.

#### <a name="phpredis"></a>PhpRedis

Phpredıs, herhangi bir PHP sürümünde TLS 'yi desteklemez.

### <a name="python"></a>Python

Redin-Kopyala varsayılan olarak TLS 1,2 kullanır.

### <a name="go"></a>GO

Redigo varsayılan olarak TLS 1,2 kullanır.

## <a name="additional-information"></a>Ek bilgiler

- [Redsıs için Azure önbelleğini yapılandırma](cache-configure.md)
