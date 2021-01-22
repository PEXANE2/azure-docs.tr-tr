---
title: Azure Media Services V2 ve v3 arasındaki özellik boşlukları | Microsoft Docs
description: Bu makalede, Azure Media Services V2 ile v3 arasındaki özellik boşlukları açıklanmaktadır.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.devlang: multiple
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 1/14/2020
ms.author: inhenkel
ms.openlocfilehash: 0f15c2bcd921c431dba1d1cce0454a6b2e752141
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98690653"
---
# <a name="feature-gaps-between-azure-media-services-v2-and-v3"></a>Azure Media Services V2 ve v3 arasındaki özellik boşlukları

![Geçiş Kılavuzu logosu](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![geçiş adımları 2](./media/migration-guide/steps-2.svg)

Geçiş kılavuzunun bu bölümü, v2 ve v3 API 'Leri arasındaki farklılıklar hakkında ayrıntılı bilgi sağlar.

## <a name="feature-gaps-between-v2-and-v3-apis"></a>V2 ve v3 API 'Ler arasındaki özellik boşlukları

V3 API, v2 API 'SI ile aşağıdaki özellik boşluklarını içerir. V2 API 'Lerinde Media Encoder Standard gelişmiş özellikler Şu anda v3 'de kullanılamaz:

- Artık Azure Media Player için gerekli olmadığı için, girişte ses olmadığında sessiz ses izi ekleme.

- Girişte video olmadığında video izlemesi ekleme.

- Transkodlamaya sahip canlı olaylar şu anda, API çağrısı aracılığıyla kurşun ekleme orta akış ve ad işaretçisi ekleme işlemini desteklemez.

- Azure Media Premium Kodlayıcısı artık v2 sürümünde desteklenmeyecektir. Bunu 8 bit HEVC kodlaması için kullanıyorsanız, standart Kodlayıcıdaki yeni HEVC desteğini kullanın. 
    - Standart kodlayıcıya ses kanalı eşleme desteği ekledik.  [Media Services kodlaması Swagger belgelerindeki ses '](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2020-05-01/Encoding.json)i inceleyin.
    - MXF veya ProRes gibi üçüncü taraf lisanslı ürünün gelişmiş özelliklerini veya çıkış biçimlerini kullanıyorsanız, Telestream 'den Azure Iş ortağı çözümünü kullanarak v2 emekliliğin zamanına göre işlem yapılır. Alternatif olarak, Imagine Communications veya [Bitmovin](http://bitmovin.com)kullanabilirsiniz.

- V2 'deki akış uç noktasındaki "kullanılabilirlik kümesi" özelliği artık desteklenmiyor. V3 API 'sindeki [yüksek kullanılabilirliğe sahip VOD](https://docs.microsoft.com/azure/media-services/latest/media-services-high-availability-encoding) teslimi için örnek projeye ve kılavuza bakın.

- Media Services v3 'de, FairPlay IV belirtilemez. Hem paketleme hem de lisans teslimi için Media Services kullanan müşterileri etkilemediğinden, FairPlay lisanslarını (karma mod) sunmak için bir üçüncü taraf DRM sistemi kullanılırken bir sorun olabilir.

- Rest 'teki varlıkların korunması için istemci tarafı depolama şifrelemesi v3 API 'sinde kaldırılmıştır ve bekleyen veriler için depolama hizmeti şifrelemesi ile değiştirilmiştir. V3 API 'Leri mevcut depolama şifrelenmiş varlıklarla çalışmaya devam eder, ancak yenilerini oluşturulmasına izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [migration guide next steps](./includes/migration-guide-next-steps.md)]
