---
title: Azure CDN ile HTTP davranışını geçersiz kılma - Verizon Premium kuralları motoru
description: Kurallar altyapısı, belirli içerik türlerinin teslimini engellemek, önbelleğe alma ilkesi tanımlamak ve HTTP üstbilgilerini değiştirmek gibi HTTP isteklerinin Verizon Premium'dan Azure CDN tarafından nasıl işlendiğini özelleştirmenize olanak tanır.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: aa0606eafb8fe4c517b0c18e0137058a120115ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74082951"
---
# <a name="override-http-behavior-using-the-azure-cdn-from-verizon-premium-rules-engine"></a>Verizon Premium kuralları altyapısından Azure CDN'yi kullanarak HTTP davranışını geçersiz kılma

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış

Azure CDN kuralları altyapısı, HTTP isteklerinin nasıl işleneceğini özelleştirmenize olanak tanır. Örneğin, belirli içerik türlerinin teslimini engellemek, önbelleğe alma ilkesi tanımlamak veya bir HTTP üstbilgisini değiştirmek. Bu öğretici, CDN varlıklarının önbelleğe alma davranışını değiştiren bir kuralın nasıl oluşturulacağımı gösterir. Kurallar altyapısı sözdizimi hakkında daha fazla bilgi için Azure [CDN kuralları altyapısı başvurusuna](cdn-verizon-premium-rules-engine-reference.md)bakın.

## <a name="access"></a>Erişim

Kurallar altyapısına erişmek için, Azure CDN yönetim sayfasına erişmek için önce **CDN profil** sayfasının üst kısmından **Yönet'i** seçmeniz gerekir. Bitiş noktanızın dinamik site hızlandırma (DSA) için optimize edilip edilmediğine bağlı olarak, son nokta türünüze uygun kurallar kümesiyle kurallar motoruna erişebilirsiniz:

- Genel web teslimi veya DSA dışındaki diğer optimizasyon için optimize edilmiş uç noktaları:
    
    HTTP **Büyük** sekmesini seçin, ardından **Kurallar Motoru'nu**seçin.

    ![HTTP için kurallar motoru](./media/cdn-rules-engine/cdn-http-rules-engine.png)

- DSA için optimize edilmiş uç noktalar:
    
    **ADN** sekmesini seçin, ardından **Kurallar Motoru'nu**seçin.
    
    ADN, Verizon tarafından DSA içeriğini belirtmek için kullanılan bir terimdir. Burada oluşturduğunuz kurallar, profilinizde DSA için optimize edilmiş olmayan uç noktalar tarafından yoksayılır.

    ![DSA için kurallar motoru](./media/cdn-rules-engine/cdn-dsa-rules-engine.png)

## <a name="tutorial"></a>Öğretici

1. **CDN profil** sayfasından **Yönet'i**seçin.
   
    ![CDN profili Yönet düğmesi](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    CDN yönetim portalı açılır.

2. HTTP **Büyük** sekmesini seçin, ardından **Kurallar Motoru'nu**seçin.
   
    Yeni bir kural için seçenekler görüntülenir.
   
    ![CDN yeni kural seçenekleri](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > Birden çok kuralın listelenme sırası, bunların nasıl işleneceğini etkiler. Sonraki bir kural önceki kural tarafından belirtilen eylemleri geçersiz kılabilir.
   >

3. **Ad / Açıklama** metin kutusuna bir ad girin.

4. Kuralın geçerli olduğu istek türünü tanımlayın. Varsayılan eşleşme koşulunu kullanın, **Her zaman**.
   
   ![CDN kural maç durumu](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!NOTE]
   > Açılan listede birden çok eşleşme koşulu mevcuttur. Şu anda seçili eşleşme koşulu hakkında bilgi için, solundaki mavi bilgi simgesini seçin.
   >
   >  Koşullu ifadelerin ayrıntılı bir listesi [için, Kurallar altyapısı koşullu ifadelere](cdn-verizon-premium-rules-engine-reference-match-conditions.md)bakın.
   >  
   > Maç koşullarının ayrıntılı bir listesi için [Kurallar motoru eşleşme koşullarına](cdn-verizon-premium-rules-engine-reference-match-conditions.md)bakın.
   >
   >

5. Yeni bir özellik eklemek **+** için **Özellikler'in**yanındaki düğmeyi seçin.  Soldaki açılır durumda Force **Internal Max-Age'i**seçin.  Görünen metin kutusuna **300**girin. Kalan varsayılan değerleri değiştirmeyin.
   
   ![CDN kural özelliği](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > Açılır listede birden çok özellik mevcuttur. Şu anda seçili özellik hakkında bilgi için, solundaki mavi bilgi simgesini seçin.
   >
   > **Force Internal Max-Age**için, `Cache-Control` CDN kenar düğümü varlığı nakıştan yenilediğinde varlığın ve `Expires` üstbilginin denetimi geçersiz kılınır. Bu örnekte, CDN kenar düğümü kıymeti kaynağından yenilemeden önce kıymeti 300 saniye veya 5 dakika olarak önbelleğe alır.
   >
   > Özelliklerin ayrıntılı bir listesi için [Kurallar motoru özelliklerine](cdn-verizon-premium-rules-engine-reference-features.md)bakın.
   >
   >

6. Yeni kuralı kaydetmek için **Ekle'yi** seçin.  Yeni kural şimdi onay bekliyor. Onaylandıktan sonra, durum Bekleyen **XML'den Etkin XML'e** dönüşür. **Active XML**
   
   > [!IMPORTANT]
   > Kural değişikliklerinin Azure CDN'de yayılması 10 dakika kadar sürebilir.
   >
   >

## <a name="see-also"></a>Ayrıca bkz.

- [Azure CDN'ye genel bakış](cdn-overview.md)
- [Kural altyapısı başvurusu](cdn-verizon-premium-rules-engine-reference.md)
- [Kural altyapısı eşleştirme koşulları](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Kural altyapısı koşullu ifadeleri](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Kural altyapısı özellikleri](cdn-verizon-premium-rules-engine-reference-features.md)
- [Azure Cumaları: Azure CDN'nin güçlü yeni premium özellikleri](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)