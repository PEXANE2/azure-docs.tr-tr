---
title: Kimlik doğrulaması, istekler ve yanıtlar
description: Key Vault'u kullanmak için AD'ye kimlik doğrulaması
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 33e3bc13e67e268b82bf517033b4b1c7c51c361f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81430896"
---
# <a name="authentication-requests-and-responses"></a>Kimlik doğrulaması, istekler ve yanıtlar

Azure Key Vault, JSON biçimlendirilmiş istek ve yanıtları destekler. Azure Anahtar Kasası'na yapılan istekler, bazı URL parametreleri ve JSON kodlanmış istek ve yanıt gövdeleriyle HTTPS kullanılarak geçerli bir Azure Key Vault URL'sine yönlendirilir.

Bu konu, Azure Anahtar Kasası hizmetinin ayrıntılarını kapsar. Kimlik doğrulama/yetkilendirme ve erişim jetonunun nasıl elde edilen dahil olmak üzere Azure REST arabirimlerinin kullanımı hakkında genel bilgi için Azure [REST API Başvurusu'na](https://docs.microsoft.com/rest/api/azure)bakın.

## <a name="request-url"></a>İstek URL'si  
 Anahtar yönetimi işlemleri, http delete, GET, PATCH, PUT ve HTTP POST ve mevcut anahtar nesnelere karşı şifreleme işlemleri http post kullanın kullanın. Belirli HTTP fiillerini destekleyemeyen istemciler, amaçlanan fiili belirtmek için X-HTTP-REQUEST üstbilgisini kullanarak HTTP POST'u da kullanabilir; normalde bir gövde gerektirmeyen istekler, http post kullanırken boş bir gövde içermelidir( örneğin DELETE yerine POST kullanırken).  

 Azure Anahtar Kasası'ndaki nesnelerle çalışmak için aşağıdaki örnek URL'ler şunlardır:  

- Key Vault kullanımında TESTKEY adlı bir anahtar oluşturmak için -`PUT /keys/TESTKEY?api-version=<api_version> HTTP/1.1`  

- ImportKEY adlı bir anahtarı Key Vault kullanımına almak için -`POST /keys/IMPORTEDKEY/import?api-version=<api_version> HTTP/1.1`  

- Bir Key Vault kullanımı MYSECRET adlı bir sır almak için -`GET /secrets/MYSECRET?api-version=<api_version> HTTP/1.1`  

- Key Vault kullanımında TESTKEY adlı bir anahtar kullanarak özet imzalamak için -`POST /keys/TESTKEY/sign?api-version=<api_version> HTTP/1.1`  

  Key Vault'a istek yapma yetkisi her zaman aşağıdaki gibidir,`https://{keyvault-name}.vault.azure.net/`  

  Anahtarlar her zaman /keys yolu altında saklanır, Sırlar her zaman /secrets yolu altında saklanır.  

## <a name="api-version"></a>API Sürümü  
 Azure Anahtar Kasası Hizmeti, tüm özellikler bu istemciler tarafından kullanılamasa da, alt düzey istemcilerle uyumluluk sağlamak için protokol sürümünü destekler. İstemciler, `api-version` varsayılan olmadığı için destekledikleri protokolün sürümünü belirtmek için sorgu dize parametresini kullanmalıdır.  

 Azure Key Vault protokolü sürümleri {YYYY} kullanarak tarih numaralandırma düzenini izler. {MM}. {DD} biçimi.  

## <a name="request-body"></a>İstek Gövdesi  
 HTTP belirtimi uyarınca, GET işlemleri bir istek gövdesi ne de POST ve PUT operasyonlarının bir istek gövdesine sahip olması gerekir. DELETE işlemlerindeki gövde HTTP'de isteğe bağlıdır.  

 Operasyon açıklamasında aksi belirtilmedikçe, istek gövdesi içerik türü uygulama/json olmalıdır ve içerik türüne uygun serileştirilmiş bir JSON nesnesi içermelidir.  

 İşlem açıklamasında aksi belirtilmedikçe, Kabul istek üstbilgisinin uygulama/json ortam türünü içermesi gerekir.  

## <a name="response-body"></a>Yanıt Gövdesi  
 Operasyon açıklamasında aksi belirtilmedikçe, hem başarılı hem de başarısız operasyonların yanıt gövdesi içerik türü uygulama/json olacaktır ve ayrıntılı hata bilgileri içerir.  

## <a name="using-http-post"></a>HTTP POST'u kullanma  
 Bazı istemciler PATCH veya DELETE gibi belirli HTTP fiillerini kullanamayabilir. Azure Key Vault, istemcinin orijinal HTTP fiiline özgü "X-HTTP-METHOD" üstbilgisini de içerdiği koşuluyla, bu istemciler için alternatif olarak HTTP POST'u destekler. HTTP POST desteği, bu belgede tanımlanan API'lerin her biri için not edilir.  

## <a name="error-responses"></a>Hata Yanıtları  
 Hata işleme HTTP durum kodları kullanır. Tipik sonuçlar şunlardır:  

- 2xx – Başarı: Normal çalışma için kullanılır. Yanıt gövdesi beklenen sonucu içerecektir  

- 3xx – Yeniden Yönlendirme: 304 "Değiştirilmemiş" koşullu GET yerine getirmek için iade edilebilir. Diğer 3xx kodları gelecekte DNS ve yol değişikliklerini belirtmek için kullanılabilir.  

- 4xx – İstemci Hatası: Hatalı istekler, eksik anahtarlar, sözdizimi hataları, geçersiz parametreler, kimlik doğrulama hataları vb. için kullanılır. Yanıt gövdesi ayrıntılı hata açıklaması içerecektir.  

- 5xx – Sunucu Hatası: Dahili sunucu hataları için kullanılır. Yanıt gövdesi özetlenmiş hata bilgileri içerir.  

  Sistem bir proxy veya güvenlik duvarı arkasında çalışmak üzere tasarlanmıştır. Bu nedenle, istemci diğer hata kodları alabilir.  

  Azure Key Vault, bir sorun oluştuğunda yanıt gövdesinde hata bilgilerini de döndürür. Yanıt gövdesi JSON biçimlendirilmiş ve form alır:  

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
 Azure Key Vault'a yönelik tüm isteklerin kimlik doğrulaması yapılmalıdır. Azure Key Vault, OAuth2 [[RFC6749](https://tools.ietf.org/html/rfc6749)] kullanılarak elde edilebilecek Azure Active Directory erişim belirteçlerini destekler. 
 
 Azure Key Vault'u kullanmak için uygulamanızı kaydetme ve kimlik doğrulaması hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/azure/index#register-your-client-application-with-azure-ad)
 
 Erişim belirteçleri HTTP Yetkilendirme üstbilgisini kullanarak hizmete gönderilmelidir:  

```  
PUT /keys/MYKEY?api-version=<api_version>  HTTP/1.1  
Authorization: Bearer <access_token>  

```  

 Bir erişim belirteci sağlanmadığında veya bir belirteç hizmet tarafından kabul edilmezse, istemciye bir HTTP 401 hatası döndürülür ve örneğin WWW-Authenticate üstbilgisini içerir:  

```  
401 Not Authorized  
WWW-Authenticate: Bearer authorization="…", resource="…"  

```  

 WWW-Authenticate üstbilgisindeki parametreler şunlardır:  

-   yetkilendirme: İstek için bir erişim jetonu almak için kullanılabilecek OAuth2 yetkilendirme hizmetinin adresi.  

-   kaynak: Yetkilendirme isteğinde`https://vault.azure.net`kullanılacak kaynağın adı ( )  

