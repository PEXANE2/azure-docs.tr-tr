---
title: .NET için Medya Hizmetleri SDK'da mantığı yeniden deneyin | Microsoft Dokümanlar
description: Konu, .NET için Medya Hizmetleri SDK'daki yeniden deneme mantığına genel bir bakış sağlar.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 527b61a6-c862-4bd8-bcbc-b9aea1ffdee3
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 63715f668438519131eba5bfff7aa38fc73267d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61094666"
---
# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>.NET için Medya Hizmetleri SDK'da mantığı yeniden deneyin  

Microsoft Azure hizmetleriyle çalışırken geçici hatalar oluşabilir. Geçici bir hata oluşursa, çoğu durumda, birkaç yeniden denemeden sonra işlem başarılı olur. .NET için Medya Hizmetleri SDK, web isteklerinin neden olduğu özel durumlar ve hatalarla ilişkili geçici hataları işlemek için yeniden deneme mantığını uygular, sorguları yürütme, değişiklikleri kaydetme ve depolama işlemleri.  Varsayılan olarak, .NET için Medya Hizmetleri SDK uygulamanızın özel durum yeniden atmadan önce dört yeniden çalışır. Uygulamanızdaki kodun bu özel durumu düzgün bir şekilde işlemesi gerekir.  

 Aşağıda Web İsteği, Depolama, Sorgu ve SaveChanges ilkelerinin kısa bir kılavuzu verilmiştir:  

* Depolama ilkesi blob depolama işlemleri (yüklemeler veya varlık dosyalarının karşıdan yüklenmesi) için kullanılır.  
* Web İstek ilkesi genel web istekleri (örneğin, kimlik doğrulama belirteci almak ve kullanıcı kümesi bitiş noktasını çözmek için) için kullanılır.  
* Sorgu ilkesi, REST'teki varlıkları sorgulamak için kullanılır (örneğin, mediaContext.Assets.Where(...).  
* SaveChanges ilkesi, hizmet içindeki verileri değiştiren her şeyi yapmak için kullanılır (örneğin, bir varlığı güncelleyen, bir işlem için hizmet işlevini çağıran bir varlık oluşturmak).  
  
  Bu konu, .NET yeniden deneme mantığı için Medya Hizmetleri SDK tarafından işlenen özel durum türlerini ve hata kodlarını listeler.  

## <a name="exception-types"></a>Özel durum türleri
Aşağıdaki tabloda, .NET için Medya Hizmetleri SDK'sının işlediği veya geçici hatalara neden olabilecek bazı işlemler için işlemediği özel durumları açıklanmaktadır.  

| Özel durum | Web İsteği | Depolama | Sorgu | Savechanges |
| --- | --- | --- | --- | --- |
| Webexception<br/>Daha fazla bilgi için [WebException durum kodları](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) bölümüne bakın. |Evet |Evet |Evet |Evet |
| Dataserviceclientexception<br/> Daha fazla bilgi için [HTTP hata durum kodlarına](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode)bakın. |Hayır |Evet |Evet |Evet |
| Dataservicequeryexception<br/> Daha fazla bilgi için [HTTP hata durum kodlarına](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode)bakın. |Hayır |Evet |Evet |Evet |
| Dataservicerequestexception<br/> Daha fazla bilgi için [HTTP hata durum kodlarına](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode)bakın. |Hayır |Evet |Evet |Evet |
| DataServiceTransportException |Hayır |Hayır |Evet |Evet |
| Timeoutexception |Evet |Evet |Evet |Hayır |
| Socketexception |Evet |Evet |Evet |Evet |
| Depolama Özel Durumu |Hayır |Evet |Hayır |Hayır |
| ıoexception |Hayır |Evet |Hayır |Hayır |

### <a name="webexception-status-codes"></a><a name="WebExceptionStatus"></a>WebException durum kodları
Aşağıdaki tablo, yeniden deneme mantığının hangi WebException hata kodlarını kodlar olduğunu gösterir. [WebExceptionStatus](https://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) numaralandırma durum kodlarını tanımlar.  

| Durum | Web İsteği | Depolama | Sorgu | Savechanges |
| --- | --- | --- | --- | --- |
| ConnectFailure |Evet |Evet |Evet |Evet |
| Ad Çözümleme Hatası |Evet |Evet |Evet |Evet |
| ProxyNameResolutionFailure |Evet |Evet |Evet |Evet |
| SendFailure |Evet |Evet |Evet |Evet |
| Boru Hattı Arıza |Evet |Evet |Evet |Hayır |
| Bağlantı Kapalı |Evet |Evet |Evet |Hayır |
| KeepAliveFailure |Evet |Evet |Evet |Hayır |
| UnknownError |Evet |Evet |Evet |Hayır |
| Alma Hatası |Evet |Evet |Evet |Hayır |
| İstekİptal edildi |Evet |Evet |Evet |Hayır |
| Zaman aşımı |Evet |Evet |Evet |Hayır |
| ProtokolHatası <br/>ProtocolError'ın yeniden denemesi HTTP durum kodu işleme tarafından denetlenir. Daha fazla bilgi için [HTTP hata durum kodlarına](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode)bakın. |Evet |Evet |Evet |Evet |

### <a name="http-error-status-codes"></a><a name="HTTPStatusCode"></a>HTTP hata durum kodları
Sorgu ve SaveChanges işlemleri DataServiceClientException, DataServiceQueryException veya DataServiceQueryException'ı attığında, HTTP hata durum kodu StatusCode özelliğinde döndürülür.  Aşağıdaki tablo, yeniden deneme mantığının hangi hata kodlarıiçin uygulandığını gösterir.  

| Durum | Web İsteği | Depolama | Sorgu | Savechanges |
| --- | --- | --- | --- | --- |
| 401 |Hayır |Evet |Hayır |Hayır |
| 403 |Hayır |Evet<br/>Daha uzun beklemelerle yeniden denemeleri işleme. |Hayır |Hayır |
| 408 |Evet |Evet |Evet |Evet |
| 429 |Evet |Evet |Evet |Evet |
| 500 |Evet |Evet |Evet |Hayır |
| 502 |Evet |Evet |Evet |Hayır |
| 503 |Evet |Evet |Evet |Evet |
| 504 |Evet |Evet |Evet |Hayır |

.NET yeniden deneme mantığı için Medya Hizmetleri SDK'nın gerçek uygulamasına bakmak istiyorsanız, [bkz.](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling)

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

