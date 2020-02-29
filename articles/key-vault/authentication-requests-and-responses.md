---
title: Kimlik doğrulaması, istekler ve yanıtlar
description: Key Vault kullanmak için AD ile kimlik doğrulama
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: b84084dafe79a8caa9cdb70bc201f68834079845
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78196085"
---
# <a name="authentication-requests-and-responses"></a>Kimlik doğrulaması, istekler ve yanıtlar

Azure Key Vault JSON biçimli istekleri ve yanıtları destekler. Azure Key Vault istekleri, URL parametreleri ve JSON kodlamalı istek ve yanıt gövdeleriyle HTTPS kullanılarak geçerli bir Azure Key Vault URL 'sine yönlendirilir.

Bu konu, Azure Key Vault hizmetinin özelliklerini içerir. Kimlik doğrulama/yetkilendirme ve erişim belirteci alma gibi Azure REST arabirimlerini kullanma hakkında genel bilgi için bkz. [azure REST API başvurusu](https://docs.microsoft.com/rest/api/azure).

## <a name="request-url"></a>İstek URL'si  
 Anahtar yönetim işlemleri, mevcut anahtar nesnelerinde http DELETE, GET, PATCH, PUT ve HTTP POST ve şifreleme işlemlerini kullanır. Belirli HTTP fiillerini destekleyemediği istemciler, amaçlanan fiili belirtmek için X-HTTP-REQUEST üst bilgisini kullanarak da HTTP POST kullanabilir. Normalde bir gövde gerektirmeyen istekler HTTP POST kullanılırken boş bir gövde içermelidir, örneğin DELETE yerine POST kullanılıyor.  

 Azure Key Vault nesneleriyle çalışmak için aşağıdaki örnek URL 'Ler verilmiştir:  

- Key Vault Use-`PUT /keys/TESTKEY?api-version=<api_version> HTTP/1.1` TESTKEY adlı bir anahtar oluşturmak için  

- IMPORTEDKEY adlı bir anahtarı Key Vault kullanım `POST /keys/IMPORTEDKEY/import?api-version=<api_version> HTTP/1.1` içeri aktarmak için  

- Key Vault kullanımı ile MYSECRET adlı bir gizli dizi almak için `GET /secrets/MYSECRET?api-version=<api_version> HTTP/1.1`  

- Key Vault Use-`POST /keys/TESTKEY/sign?api-version=<api_version> HTTP/1.1` TESTKEY adlı bir anahtarı kullanarak bir özeti IMZALAMAK için  

  Bir Key Vault isteği için olan yetki her zaman aşağıdaki gibidir `https://{keyvault-name}.vault.azure.net/`  

  Anahtarlar her zaman/Keys yolu altında depolanır, gizlilikler her zaman/gizlilikler yolu altında depolanır.  

## <a name="api-version"></a>API sürümü  
 Azure Key Vault hizmeti, alt düzey istemcilerle uyumluluk sağlamak için protokol sürümü oluşturmayı destekler, ancak tüm yetenekler bu istemciler için kullanılabilir olmaz. İstemcilerin, desteklediği protokol sürümünü belirtmek için `api-version` sorgu dizesi parametresini kullanması gerekir, çünkü varsayılan yoktur.  

 Azure Key Vault protokol sürümleri, {YYYY} kullanarak bir tarih numaralandırma şemasını izler. {MM}. {DD} biçimi.  

## <a name="request-body"></a>İstek Gövdesi  
 HTTP belirtimine göre, GET işlemlerinde bir istek gövdesi olmaması ve POST ve PUT işlemlerinin bir istek gövdesine sahip olması gerekir. DELETE işlemlerinde gövde, HTTP 'de isteğe bağlıdır.  

 İşlem açıklamasında aksi belirtilmedikçe, istek gövdesi içerik türü Application/JSON olmalı ve içerik türüne uyumlu serileştirilmiş JSON nesnesi içermelidir.  

 İşlem açıklamasında aksi belirtilmedikçe, Accept istek üst bilgisi Application/JSON medya türünü içermelidir.  

## <a name="response-body"></a>Yanıt Gövdesi  
 İşlem açıklamasında aksi belirtilmedikçe, hem başarılı hem de başarısız olan işlemlerin yanıt gövdesi içerik türü Application/JSON olur ve ayrıntılı hata bilgilerini içerir.  

## <a name="using-http-post"></a>HTTP POST kullanma  
 Bazı istemciler, düzeltme eki veya SILME gibi bazı HTTP fiillerini kullanmayabilir. Azure Key Vault, istemcinin özgün HTTP fiiline özgü "X-HTTP-METHOD" üst bilgisini de içermesi kaydıyla, bu istemcilere alternatif olarak HTTP POST 'un kullanılmasını destekler. Bu belgede tanımlanan her API için HTTP POST için destek belirtilmiştir.  

## <a name="error-responses"></a>Hata yanıtları  
 Hata işleme HTTP durum kodlarını kullanacaktır. Tipik sonuçlar şunlardır:  

- 2xx – başarılı: normal işlem için kullanılır. Yanıt gövdesinde beklenen sonuç yer alacak  

- 3xx – yeniden yönlendirme: koşullu bir GET yerine getirmek için 304 "değiştirilmedi" geri döndürülebilir. Diğer 3xx kodları gelecekte DNS ve yol değişikliklerini göstermek için kullanılabilir.  

- 4xx – Istemci hatası: Hatalı istekler, eksik anahtarlar, sözdizimi hataları, geçersiz parametreler, kimlik doğrulama hataları vb. için kullanılır. Yanıt gövdesinde ayrıntılı hata açıklaması yer alacak.  

- 5xx – sunucu hatası: iç sunucu hataları için kullanılır. Yanıt gövdesi özetlenen hata bilgilerini içerir.  

  Sistem bir proxy veya güvenlik duvarının arkasında çalışacak şekilde tasarlanmıştır. Bu nedenle, bir istemci diğer hata kodlarını alabilir.  

  Azure Key Vault, bir sorun oluştuğunda yanıt gövdesinde hata bilgilerini de döndürür. Yanıt gövdesi JSON olarak biçimlendirilir ve şu biçimde olur:  

```  

{  
  "error":  
  {  
    "code": "BadArgument",  
    "message":  

      "’Foo’ is not a valid argument for ‘type’."  
    }  
  }  
}  

```  

## <a name="authentication"></a>Kimlik Doğrulaması  
 Azure Key Vault tüm isteklerin kimliğinin doğrulanması gerekır. Azure Key Vault, OAuth2 [[RFC6749](https://tools.ietf.org/html/rfc6749)] kullanılarak elde edilebilir Azure Active Directory erişim belirteçlerini destekler. 
 
 Uygulamanızı kaydetme ve Azure Key Vault kullanmak için kimlik doğrulama hakkında daha fazla bilgi için bkz. [istemci uygulamanızı Azure AD 'ye kaydetme](https://docs.microsoft.com/rest/api/azure/index#register-your-client-application-with-azure-ad).
 
 Erişim belirteçlerinin HTTP yetkilendirme üst bilgisi kullanılarak hizmete gönderilmesi gerekir:  

```  
PUT /keys/MYKEY?api-version=<api_version>  HTTP/1.1  
Authorization: Bearer <access_token>  

```  

 Erişim belirteci sağlanmamışsa veya bir belirteç hizmet tarafından kabul edilmediğinde, istemciye bir HTTP 401 hatası döndürülür ve WWW-Authenticate üst bilgisini dahil eder; örneğin:  

```  
401 Not Authorized  
WWW-Authenticate: Bearer authorization="…", resource="…"  

```  

 WWW-Authenticate üstbilgisindeki parametreler şunlardır:  

-   Yetkilendirme: istek için bir erişim belirteci almak üzere kullanılabilecek OAuth2 yetkilendirme hizmetinin adresi.  

-   Kaynak: yetkilendirme isteğinde kullanılacak kaynağın adı (https://vault.azure.net).  

## <a name="see-also"></a>Ayrıca Bkz.  
 [Anahtarlar, gizli diziler ve sertifikalar hakkında](about-keys-secrets-and-certificates.md)
