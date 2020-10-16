---
title: .NET için Media Services SDK 'da yeniden deneme mantığı | Microsoft Docs
description: Bu konu, .NET için Media Services SDK 'da yeniden deneme mantığına genel bir bakış sunar.
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
ms.openlocfilehash: 144db6a5ceaf56a35d3ce11dd54e1dfb4c97d7e3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89264122"
---
# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>.NET için Media Services SDK 'da yeniden deneme mantığı

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Microsoft Azure hizmetleriyle çalışırken geçici hatalar meydana gelebilir. Geçici bir hata oluşursa, çoğu durumda işlem başarılı olur. .NET için Media Services SDK, Web istekleri, sorgu yürütme, değişiklikleri kaydetme ve depolama işlemleri gibi özel durumlar ve hatalarla ilişkili geçici hataları işlemek için yeniden deneme mantığını uygular.  Varsayılan olarak, .NET için Media Services SDK, uygulamanıza özel durumu yeniden oluşturmadan önce dört yeniden deneme yürütür. Uygulamanızdaki kod daha sonra bu özel durumu doğru bir şekilde işlemelidir.  

 Web Isteği, depolama, sorgu ve SaveChanges ilkelerinin kısa bir Kılavuzu aşağıda verilmiştir:  

* Depolama ilkesi, BLOB depolama işlemleri için kullanılır (varlık dosyalarını karşıya yükler veya indirir).  
* Web Isteği ilkesi, genel Web istekleri için kullanılır (örneğin, kimlik doğrulama belirteci almak ve kullanıcı kümesi uç noktasını çözümlemek için).  
* Sorgu İlkesi, varlıkları REST 'ten sorgulamak için kullanılır (örneğin, mediaContext. Entities. WHERE (...)).  
* SaveChanges ilkesi, hizmet içindeki verileri değiştiren her şeyi yapmak için kullanılır (örneğin, bir varlığı güncelleştiren bir varlık oluşturmak ve bir işlem için bir hizmet işlevi çağırmak).  
  
  Bu konu, .NET yeniden deneme mantığı için Media Services SDK tarafından işlenen özel durum türlerini ve hata kodlarını listeler.  

## <a name="exception-types"></a>Özel durum türleri
Aşağıdaki tabloda, .NET için Media Services SDK 'nın, geçici hatalara neden olabilecek bazı işlemler için işlediği veya işlemeyen özel durumlar açıklanmaktadır.  

| Özel durum | Web Isteği | Depolama | Sorgu | Sonrasında |
| --- | --- | --- | --- | --- |
| Gönderdi<br/>Daha fazla bilgi için, [WebException durum kodları](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) bölümüne bakın. |Evet |Evet |Evet |Evet |
| DataServiceClientException<br/> Daha fazla bilgi için bkz. [http hata durumu kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Hayır |Evet |Evet |Evet |
| DataServiceQueryException<br/> Daha fazla bilgi için bkz. [http hata durumu kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Hayır |Evet |Evet |Evet |
| DataServiceRequestException<br/> Daha fazla bilgi için bkz. [http hata durumu kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Hayır |Evet |Evet |Evet |
| DataServiceTransportException |Hayır |Hayır |Evet |Evet |
| TimeoutException |Evet |Evet |Evet |Hayır |
| SocketException |Evet |Evet |Evet |Evet |
| StorageException |Hayır |Evet |Hayır |Hayır |
| IOException |Hayır |Evet |Hayır |Hayır |

### <a name="webexception-status-codes"></a><a name="WebExceptionStatus"></a> WebException durum kodları
Aşağıdaki tabloda, yeniden deneme mantığının uygulandığı WebException hata kodlarının gösterildiği gösterilmektedir. [WebExceptionStatus](/dotnet/api/system.net.webexceptionstatus?view=netcore-3.1) numaralandırması durum kodlarını tanımlar.  

| Durum | Web Isteği | Depolama | Sorgu | Sonrasında |
| --- | --- | --- | --- | --- |
| ConnectFailure |Evet |Evet |Evet |Evet |
| NameResolutionFailure |Evet |Evet |Evet |Evet |
| ProxyNameResolutionFailure |Evet |Evet |Evet |Evet |
| SendFailure |Evet |Evet |Evet |Evet |
| PipelineFailure |Evet |Evet |Evet |Hayır |
| ConnectionClosed |Evet |Evet |Evet |Hayır |
| KeepAliveFailure |Evet |Evet |Evet |Hayır |
| UnknownError |Evet |Evet |Evet |Hayır |
| ReceiveFailure |Evet |Evet |Evet |Hayır |
| Requestiptal edildi |Evet |Evet |Evet |Hayır |
| Zaman aşımı |Evet |Evet |Evet |Hayır |
| ProtocolError <br/>Protocolon 'da yeniden dene hatası, HTTP durum kodu işleme tarafından denetlenir. Daha fazla bilgi için bkz. [http hata durumu kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Evet |Evet |Evet |Evet |

### <a name="http-error-status-codes"></a><a name="HTTPStatusCode"></a> HTTP hatası durum kodları
Sorgu ve SaveChanges işlemleri DataServiceClientException, DataServiceQueryException veya DataServiceQueryException oluştururken, StatusCode özelliğinde HTTP hatası durum kodu döndürülür.  Aşağıdaki tabloda, yeniden deneme mantığının uygulandığı hata kodları gösterilmektedir.  

| Durum | Web Isteği | Depolama | Sorgu | Sonrasında |
| --- | --- | --- | --- | --- |
| 401 |Hayır |Evet |Hayır |Hayır |
| 403 |Hayır |Evet<br/>Daha uzun bir süre sonra yeniden denemeleri işleme. |Hayır |Hayır |
| 408 |Evet |Evet |Evet |Evet |
| 429 |Evet |Evet |Evet |Evet |
| 500 |Evet |Evet |Evet |Hayır |
| 502 |Evet |Evet |Evet |Hayır |
| 503 |Evet |Evet |Evet |Evet |
| 504 |Evet |Evet |Evet |Hayır |

.NET yeniden deneme mantığı için Media Services SDK 'nın gerçek uygulamasına göz atmak istiyorsanız bkz. [Azure-SDK-for-Media-Services](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
