---
title: Azure Media Services kodlama hatası kodları | Microsoft Docs
description: Bu konuda, kodlama görevi yürütülürken bir hatayla karşılaşılması durumunda döndürülebilecek hata kodları listelenmektedir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 6e56dbe1d1236a567ed6f59acfcca325a6c9ee7e
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89269038"
---
# <a name="encoding-error-codes"></a>Kodlama hata kodları

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Aşağıdaki tabloda, kodlama görevi yürütülürken bir hatayla karşılaşılması durumunda döndürülebilecek hata kodları listelenmektedir.  .NET kodunuzda hata ayrıntılarını almak için [ErrorDetails](/previous-versions/azure/jj126075(v=azure.100)) sınıfını kullanın. REST kodunuzda hata ayrıntılarını almak için [ErrorDetail](/rest/api/media/operations/errordetail) REST API kullanın.

| ErrorDetail. Code | Hatanın olası nedenleri |
| --- | --- |
| Bilinmiyor |Görev yürütülürken bilinmeyen hata oluştu |
| Errordownloadingınputassetmalformedcontent |Hatalı dosya adları, sıfır uzunluklu dosyalar, yanlış biçimler vb. gibi giriş varlığı indirmede oluşan hataları ele alan hata kategorisi. |
| Errordownloadingınputassetservicefailure |Hizmet tarafında sorunları ele alan hata kategorisi; Örneğin, indirme sırasında ağ veya depolama hataları. |
| ErrorParsingConfiguration |Görevin \<see cref="MediaTask.PrivateData"/> (yapılandırma) geçerli olmadığı bir hata kategorisi; Örneğin, yapılandırma geçerli bir sistem önayarı değil veya GEÇERSIZ XML içeriyor. |
| ErrorExecutingTaskMalformedContent |Görevin, giriş medya dosyaları içindeki sorunların başarısız olmasına neden olan hata kategorisi. |
| ErrorExecutingTaskUnsupportedFormat |Medya işlemcisinin belirtilen dosyaları işleyemediğinden oluşan hata kategorisi-medya biçimi desteklenmiyor veya yapılandırmayla eşleşmiyor. Örneğin, yalnızca video içeren bir varlıktan yalnızca ses çıkışı üretme girişimi |
| ErrorProcessingTask |Medya işlemcisinin içerikle ilgisi olmayan görevin işlenmesi sırasında karşılaştığı diğer hataların kategorisi. |
| ErrorUploadingOutputAsset |Çıkış varlığı karşıya yüklenirken hata kategorisi |
| ErrorCancelingTask |Görevi iptal edilmeye çalışılırken oluşan hataları kapsayan hata kategorisi |
| Geçişli Enterror |Geçici sorunları kapsayan hata kategorisi (örn. Azure depolama ile geçici ağ sorunları) |

**Media Services** takımdan yardım almak için bir [destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade)açın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geribildirim gönderme
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>İlgili makaleler:
* [Media Encoder Standard ön ayarlarını özelleştirerek gelişmiş kodlama görevleri gerçekleştirin](media-services-custom-mes-presets-with-dotnet.md)
* [Kotalar ve sınırlamalar](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/
