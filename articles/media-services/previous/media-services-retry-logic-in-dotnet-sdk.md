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
ms.openlocfilehash: 120b7e044452dc47126923449a3e1a6e55cfd6a8
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87000032"
---
# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>.NET için Media Services SDK 'da yeniden deneme mantığı  

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
| Gönderdi<br/>Daha fazla bilgi için, [WebException durum kodları](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) bölümüne bakın. |Yes |Yes |Yes |Yes |
| DataServiceClientException<br/> Daha fazla bilgi için bkz. [http hata durumu kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Hayır |Yes |Yes |Yes |
| DataServiceQueryException<br/> Daha fazla bilgi için bkz. [http hata durumu kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Hayır |Yes |Yes |Yes |
| DataServiceRequestException<br/> Daha fazla bilgi için bkz. [http hata durumu kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Hayır |Yes |Yes |Yes |
| DataServiceTransportException |Hayır |Hayır |Yes |Yes |
| TimeoutException |Yes |Yes |Yes |Hayır |
| SocketException |Yes |Yes |Yes |Yes |
| StorageException |Hayır |Evet |Hayır |Hayır |
| IOException |Hayır |Evet |Hayır |Hayır |

### <a name="webexception-status-codes"></a><a name="WebExceptionStatus"></a>WebException durum kodları
Aşağıdaki tabloda, yeniden deneme mantığının uygulandığı WebException hata kodlarının gösterildiği gösterilmektedir. [WebExceptionStatus](/dotnet/api/system.net.webexceptionstatus?view=netcore-3.1) numaralandırması durum kodlarını tanımlar.  

| Durum | Web Isteği | Depolama | Sorgu | Sonrasında |
| --- | --- | --- | --- | --- |
| ConnectFailure |Yes |Yes |Yes |Yes |
| NameResolutionFailure |Yes |Yes |Yes |Yes |
| ProxyNameResolutionFailure |Yes |Yes |Yes |Yes |
| SendFailure |Yes |Yes |Yes |Yes |
| PipelineFailure |Yes |Yes |Yes |Hayır |
| ConnectionClosed |Yes |Yes |Yes |Hayır |
| KeepAliveFailure |Yes |Yes |Yes |Hayır |
| UnknownError |Yes |Yes |Yes |Hayır |
| ReceiveFailure |Yes |Yes |Yes |Hayır |
| Requestiptal edildi |Yes |Yes |Yes |Hayır |
| Zaman aşımı |Yes |Yes |Yes |Hayır |
| ProtocolError <br/>Protocolon 'da yeniden dene hatası, HTTP durum kodu işleme tarafından denetlenir. Daha fazla bilgi için bkz. [http hata durumu kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Yes |Yes |Yes |Yes |

### <a name="http-error-status-codes"></a><a name="HTTPStatusCode"></a>HTTP hatası durum kodları
Sorgu ve SaveChanges işlemleri DataServiceClientException, DataServiceQueryException veya DataServiceQueryException oluştururken, StatusCode özelliğinde HTTP hatası durum kodu döndürülür.  Aşağıdaki tabloda, yeniden deneme mantığının uygulandığı hata kodları gösterilmektedir.  

| Durum | Web Isteği | Depolama | Sorgu | Sonrasında |
| --- | --- | --- | --- | --- |
| 401 |Hayır |Evet |Hayır |Hayır |
| 403 |Hayır |Evet<br/>Daha uzun bir süre sonra yeniden denemeleri işleme. |Hayır |Hayır |
| 408 |Yes |Yes |Yes |Yes |
| 429 |Yes |Yes |Yes |Yes |
| 500 |Yes |Yes |Yes |Hayır |
| 502 |Yes |Yes |Yes |Hayır |
| 503 |Yes |Yes |Yes |Yes |
| 504 |Yes |Yes |Yes |Hayır |

.NET yeniden deneme mantığı için Media Services SDK 'nın gerçek uygulamasına göz atmak istiyorsanız bkz. [Azure-SDK-for-Media-Services](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geribildirim gönderme
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
