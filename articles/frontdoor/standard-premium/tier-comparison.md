---
title: Azure ön kapısının Standart/Premium SKU karşılaştırması
description: Bu makalede, aralarında Azure ön kapı standardı ve Premium SKU ve özellik farklılıkları hakkında bir genel bakış sunulmaktadır.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: amsriva
ms.openlocfilehash: 1753f2bb649e73d7a5fe6c1cc32361a418ea7f63
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102181814"
---
# <a name="overview-of-azure-front-door-standardpremium-sku-preview"></a>Azure ön kapısı Standart/Premium SKU 'ya genel bakış (Önizleme)

> [!Note]
> Bu belge, Azure ön kapısının Standart/Premium (Önizleme) içindir. Azure ön kapısı hakkında bilgi mi arıyorsunuz? [Burada](../front-door-overview.md)görüntüleyin.

Azure ön kapısının 3 farklı SKU 'Lar, [Azure ön kapısı](../front-door-overview.md), Azure ön kapı standardı (Önizleme) ve Azure ön kapısı Premium (Önizleme) için sunulması önerilir. Azure ön kapısının Standart/Premium SKU 'Ları Azure ön kapısının yeteneklerini, Microsoft 'tan Azure CDN standardını, Azure WAF ' i akıllı tehdit koruması olan tek bir güvenli bulut CDN platformunda birleştirir.

> [!IMPORTANT]
> Azure ön kapı Standart/Premium (Önizleme) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [**Microsoft Azure önizlemeleri Için ek kullanım koşulları**](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

* **Azure ön kapısının standart SKU 'su** :

    * İçerik teslimi iyileştirilmiş
    * Hem statik hem de dinamik içerik hızlandırma sunma
    * Küresel Yük Dengeleme
    * SSL boşaltması
    * Etki alanı ve sertifika yönetimi
    * Gelişmiş Trafik Analizi 
    * Temel güvenlik özellikleri

* **Azure ön kapısı Premium SKU 'Su** standart SKU 'nun özelliklerine göre oluşturulur ve şunları ekler:

    * WAF genelinde kapsamlı güvenlik özellikleri
    * BOT koruması
    * Özel bağlantı desteği
    * Microsoft Threat Intelligence ve Security Analytics ile tümleştirme. 

![Ön kapı SKU 'Ları arasındaki karşılaştırmayı gösteren diyagram.](../media/tier-comparison/tier-comparison.png)

## <a name="feature-comparison"></a>Özellik karşılaştırması

| Özellik |      Standart      |  Premium |
|----------|:-------------:|------:|
| Özel etki alanları | Yes | Yes |
| SSL boşaltması | Yes | Yes |
| Önbelleğe Alma |  Yes  | Yes |
| Sıkıştırma | Yes | Yes   |
| Küresel Yük Dengeleme | Yes  | Yes |
| Katman 7 yönlendirme | Yes | Yes |
| URL yeniden yazma | Yes | Yes |
| Kural Altyapısı | Yes | Yes |
| Özel kaynak (özel bağlantı) | Hayır | Yes |
| WAF | Yalnızca özel kurallar | Yes |
| Bot koruması | Hayır | Yes |
| Gelişmiş ölçümler ve Tanılamalar | Yes | Yes |
| Trafik raporu | Yes | Yes |
| Güvenlik raporu | Hayır | Evet | 

## <a name="next-steps"></a>Sonraki adımlar

[Ön kapı oluşturmayı](create-front-door-portal.md) öğrenin
