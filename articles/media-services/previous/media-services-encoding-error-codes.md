---
title: Azure Medya Hizmetleri kodlama hata kodları | Microsoft Dokümanlar
description: Bu konu, kodlama görevi yürütme sırasında bir hata yla karşılaşılması durumunda döndürülebilecek hata kodlarını listeler...
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
ms.openlocfilehash: 5c038f0be31acea52c2ef07d43f0dbaf3434a371
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64709517"
---
# <a name="encoding-error-codes"></a>Kodlama hata kodları

Aşağıdaki tabloda, kodlama görevi yürütme sırasında bir hata yla karşılaşılması durumunda döndürülebilecek hata kodları listelenir.  .NET kodunuzda hata ayrıntıları almak için [Hata Ayrıntıları](https://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) sınıfını kullanın. REST kodunuzda hata ayrıntıları almak için [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API'sını kullanın.

| HataDetay.Kod | Olası hata nedenleri |
| --- | --- |
| Bilinmiyor |Görevi yürüterken bilinmeyen hata |
| ErrorDownloadingInputAssetMalformedContent |Hatalı dosya adları, sıfır uzunluktaki dosyalar, yanlış biçimler ve benzeri giriş varlık larını indirmedeki hataları kapsayan hatalar kategorisi. |
| ErrorDownloadingInputAssetServiceFailure |Hizmet tarafındaki sorunları kapsayan hatalar kategorisi (örneğin, indirme sırasında ağ veya depolama hataları). |
| ErrorParsingConfiguration |Görevin \<cref="MediaTask.PrivateData"/> (yapılandırma) geçerli olmadığı, örneğin yapılandırmanın geçerli bir sistem önceden ayarlanmadığı veya geçersiz XML içerdiği hatalar kategorisi. |
| ErrorExecutingTaskMalformedContent |Giriş ortamı dosyalarının içindeki sorunların hataya neden olduğu görevin yürütülmesi sırasındaki hatalar kategorisi. |
| ErrorExecutingTaskUnsupportedFormat |Ortam işlemcisinin sağlanan dosyaları işleyemediği hatalar kategorisi - ortam biçimi desteklenmez veya Yapılandırma ile eşleşmez. Örneğin, yalnızca video su olan bir varlıktan yalnızca ses çıktısı oluşturmaya çalışmak |
| Hata İşleme Görevi |Ortam işlemcisinin içerikle ilgisi olmayan görevin işlenmesi sırasında karşılaştığı diğer hatalar kategorisi. |
| ErrorUploadingOutputAsset |Çıktı kıymetini yüklerken hata kategorisi |
| HataİptalGörevi |Görevi iptal etmeye çalışırken hataları kapsayacak hatalar kategorisi |
| Geçici Hata |Geçici sorunları kapsayacak hata kategorisi (örn. Azure Depolama ile geçici ağ sorunları) |

**Medya Hizmetleri** ekibinden yardım almak için bir [destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade)açın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>İlgili makaleler:
* [Media Encoder Standart ön ayarlarını özelleştirerek gelişmiş kodlama görevleri gerçekleştirin](media-services-custom-mes-presets-with-dotnet.md)
* [Kotalar ve Sınırlamalar](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/
