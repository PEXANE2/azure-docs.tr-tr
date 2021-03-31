---
title: Azure ön kapısı Standart/Premium 'da önbellek temizleme (Önizleme)
description: Bu makale, bir Azure ön kapısı Standart/Premium üzerinde önbelleğin nasıl temizyükleneceğini anlamanıza yardımcı olur.
services: frontdoor
author: duongau
manager: KumudD
ms.service: frontdoor
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: duau
ms.openlocfilehash: aacbf2ceab8580727b1885bf6533cd74a7c4e60a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101100435"
---
# <a name="cache-purging-in-azure-front-door-standardpremium-preview"></a>Azure ön kapısı Standart/Premium 'da önbellek temizleme (Önizleme)

> [!Note]
> Bu belge, Azure ön kapısının Standart/Premium (Önizleme) içindir. Azure ön kapısı hakkında bilgi mi arıyorsunuz? [Burada](../front-door-overview.md)görüntüleyin.

Varlığın yaşam süresi (TTL) sona erene kadar Azure ön kapısının Standart/Premium önbelleğe alma varlıkları. İstemci, süresi dolma süresi geçen bir varlık istediğinde, Azure ön kapı ortamı, isteği karşılamak için varlığın yeni bir güncelleştirilmiş kopyasını alır ve ardından yenilenen önbelleği depolar.

En iyi uygulama, kullanıcılarınızın varlıklarınızın en son kopyasını her zaman elde ettiğinizden emin olmak. Bunu yapmanın yolu, her bir güncelleştirme için varlıklarınızı sürüm ve yeni URL 'Ler olarak yayımlama yöntemidir. Azure ön kapısı Standart/Premium, yeni varlıkları bir sonraki istemci istekleri için hemen alır. Bazen, tüm kenar düğümlerinden önbelleğe alınmış içerikleri temizlemek ve bunları yeni güncelleştirilmiş varlıkları almaya zorlamak isteyebilirsiniz. Önbelleğe alınmış içerikleri temizlemek istediğiniz neden, uygulamanızda yeni güncelleştirmeler yaptığınız veya hatalı bilgiler içeren varlıkları güncelleştirmek istediğidir.

> [!IMPORTANT]
> Azure ön kapı Standart/Premium (Önizleme) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Önbelleğe almanın nasıl çalıştığını anlamak için [Azure ön kapı önbelleği](concept-caching.md) ' ni gözden geçirin.

## <a name="configure-cache-purge"></a>Önbellek temizlemeyi yapılandırma

1. Temizlemek istediğiniz varlıkların bulunduğu Azure ön kapısının genel bakış sayfasına gidin ve **önbelleği temizle**' yi seçin.

   :::image type="content" source="../media/how-to-cache-purge/front-door-cache-purge-1.png" alt-text="Genel Bakış sayfasında önbellek temizleme ekran görüntüsü.":::

1. Kenar düğümlerinden temizlemek istediğiniz uç noktayı ve etki alanını seçin. *(Birden fazla etki alanı seçebilirsiniz)*

   :::image type="content" source="../media/how-to-cache-purge/front-door-cache-purge-2.png" alt-text="Önbellek Temizleme sayfasının ekran görüntüsü.":::

1. Tüm varlıkları temizlemek için **Seçili etki alanları için tüm varlıkları temizle**' yi seçin. Aksi takdirde, **yollar**' da, temizlemek istediğiniz her varlığın yolunu girin.

Bu biçimler, temizlenecek yolların listelerinde desteklenir:

* **Tek yol temizleme**: varlığın tam yolunu (protokol ve etki alanı olmadan) dosya uzantısıyla (örneğin,/resim/strasbourg.png) belirterek tek tek varlıkları temizleyin.
* **Kök etki alanı temizleme**: yoldaki "/*" ile uç noktanın kökünü temizle.

Azure ön kapısı standart/öncesi önbellek temizler, büyük/küçük harfe duyarlıdır. Bunlara ek olarak, sorgu dizesi belirsiz olur, yani bir URL 'nin temizlenmesi, tüm sorgu dizesi çeşitlemelerini temizler. 

## <a name="next-steps"></a>Sonraki adımlar

[Ön kapı Standart/Premium oluşturmayı](create-front-door-portal.md)öğrenin.
