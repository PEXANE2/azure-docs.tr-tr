---
title: Redsıs için Azure Cache ile TLS 1,0 ve 1,1 kullanımını kaldırın | Microsoft Docs
description: Redsıs için Azure önbelleğiyle iletişim kurarken uygulamanızın TLS 1,0 ve 1,1 ' i nasıl kaldıracağınızı öğrenin
services: cache
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 10/22/2019
ms.author: yegu
ms.openlocfilehash: d1d44467d310b87d306ea7401e66784d684a1bf3
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72901904"
---
# <a name="remove-use-of-tls-10-and-11-with-azure-cache-for-redis"></a>Redsıs için Azure Cache ile TLS 1,0 ve 1,1 kullanımını kaldırma

Yalnızca TLS 1,2 veya üzeri bir sürümü kullanarak sektör genelinde bir gönderim vardır. 1,0 ve 1,1 arasındaki TLS sürümleri, BEAST ve POOıDA gibi saldırılara açıktır ve diğer yaygın güvenlik açıkları ve Etkilenmeler (CVE) zayıf yönleri vardır. Ayrıca, PCI uyumluluk standartları tarafından önerilen modern şifreleme yöntemlerini ve şifre paketlerini desteklemezler. Bu [TLS güvenlik blogu](https://www.acunetix.com/blog/articles/tls-vulnerabilities-attacks-final-part/) , daha fazla ayrıntı için bu güvenlik açıklarından bazılarını açıklamaktadır.

Bu sorun ortaya çıkan sorunlardan hiçbiri sırasında, TLS 1,0 ve 1,1 ' yi mümkün olduğunca erken kullanmaya dikkat etmelisiniz. Redsıs için Azure önbelleği, 31 Mart 2020 ' den itibaren bu TLS sürümlerinin desteklenmesini durdurur. Bu tarihten sonra önbelleğiniz ile iletişim kurmak için uygulamanızın en az TLS 1,2 kullanması gerekecektir.

Bu makalede, bu bağımlılıkları nasıl tespit etmek ve uygulamanızdan kaldırmak için genel yönergeler sağlanmaktadır.

## <a name="check-if-your-application-is-already-compliant"></a>Uygulamanızın zaten uyumlu olup olmadığını denetleyin

Uygulamanızın TLS 1,2 ile çalışacağından en kolay yolu, kullandığı bir test veya hazırlama önbelleğinde en düşük TLS sürümünü ayarlamak ve TLS 1,2 ' dir. En düşük TLS sürümü ayarını, Azure portal önbellek örneğinizin [Gelişmiş ayarlarında](cache-configure.md#advanced-settings) bulabilirsiniz. Uygulama bu değişiklikten sonra beklendiği gibi çalışmaya devam ediyorsa, büyük olasılıkla uyumlu olabilir. Uygulamamız tarafından kullanılan Redsıs istemci kitaplıklarının, bu güvenlik protokolünün üzerinden Red. için Azure önbelleğine bağlanmak üzere TLS 1,2 ' i etkinleştirmek için özellikle yapılandırılması gerekebilir.

## <a name="configure-your-application-to-use-tls-12"></a>Uygulamanızı TLS 1,2 kullanacak şekilde yapılandırma

Çoğu uygulama, önbellekleri ile iletişimi işlemek için Redsıs istemci kitaplıklarını kullanır. Aşağıda, çeşitli programlama dillerinde ve çerçevelerindeki bazı popüler istemci kitaplıklarının, TLS 1,2 kullanması için nasıl yapılandırılacağı hakkında yönergeler verilmiştir.

### <a name="net-framework"></a>.NET Framework

Redsıs .NET istemcileri en düşük TLS sürümünü varsayılan olarak .NET Framework 4.5.2 veya altında ve 4,6 veya üzeri sürümlerde en yüksek TLS sürümünü kullanır. .NET Framework eski bir sürümünü kullanıyorsanız, TLS 1,2 'yi el ile etkinleştirebilirsiniz:

* StackExchange. Redsıs: bağlantı dizesinde `ssl=true` ve `sslprotocls=tls12` ayarla.
* ServiceStack. Redsıs: [Bu yönergeleri](https://github.com/ServiceStack/ServiceStack.Redis/pull/247)izleyin.

### <a name="net-core"></a>.NET Core

Redsıs .NET Core istemcileri varsayılan olarak en yüksek TLS sürümünü kullanır.

### <a name="java"></a>Java

Redsıs Java istemcileri Java sürüm 6 veya altında TLS 1,0 kullanır. Jedsıs, Lettuce ve Sedisson, TLS 1,0 önbellekte devre dışı bırakılmışsa redin için Azure önbelleğine bağlanamayacak. Şu anda bilinen bir geçici çözüm yoktur.

Java 7 veya üzeri sürümlerde, Redsıs istemcileri varsayılan olarak TLS 1,2 kullanmaz ancak bu için yapılandırılmış olabilir. Aşağıdaki anda Lettuce ve Oydisson desteklenmez. Önbellek yalnızca TLS 1,2 bağlantılarını kabul ediyorsa, bunlar kesilir. Jedsıs, aşağıdaki kod parçacığı ile temeldeki TLS ayarlarını belirtmenize olanak sağlar:

``` Java
SSLSocketFactory sslSocketFactory = (SSLSocketFactory) SSLSocketFactory.getDefault();
SSLParameters sslParameters = new SSLParameters();
sslParameters.setEndpointIdentificationAlgorithm("HTTPS");
sslParameters.setProtocols(new String[]{"TLSv1", "TLSv1.1", "TLSv1.2"});
 
URI uri = URI.create("rediss://host:port");
JedisShardInfo shardInfo = new JedisShardInfo(uri, sslSocketFactory, sslParameters, null);
 
shardInfo.setPassword("cachePassword");
 
Jedis jedis = new Jedis(shardInfo);
```

### <a name="nodejs"></a>Node.js

Redsıs ve ıoredin Node, varsayılan olarak TLS 1,2 kullanır.

### <a name="php"></a>PHP

İkincisi yalnızca TLS 1,0 ' i desteklediğinden PHP 7 ' deki predıs çalışmaz. PHP 7.2.1 veya altında, Predıs varsayılan olarak TLS 1,0 veya 1,1 kullanır. İstemciyi örnekledikten sonra TLS 1,2 ' i belirtebilirsiniz:

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

PHP 7,3 veya üzeri sürümlerde, Predıs en son TLS sürümünü kullanır.

Phpredıs, herhangi bir PHP sürümünde TLS 'yi desteklemez.

### <a name="python"></a>Python

Redin-Kopyala varsayılan olarak TLS 1,2 kullanır.

### <a name="go"></a>GO

Redigo varsayılan olarak TLS 1,2 kullanır.

## <a name="additional-information"></a>Ek Bilgi

- [Redsıs için Azure önbelleğini yapılandırma](cache-configure.md)
