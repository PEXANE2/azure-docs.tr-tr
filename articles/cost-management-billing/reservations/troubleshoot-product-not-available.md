---
title: Azure rezervasyon türünün kullanılamaması ile ilgili sorunları giderme
description: Bu makale, Azure portalda kullanılabilir olarak görünmeyen ayrılmış örnekleri anlamanıza ve bunlarla ilgili sorunları gidermenize yardımcı olur.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: reservations
ms.author: banders
ms.reviewer: yashar
ms.topic: troubleshooting
ms.date: 10/27/2020
ms.openlocfilehash: 8575d9d86d8e720122a295cf92fa571ef33d5b4c
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92798235"
---
# <a name="troubleshoot-reservation-type-not-available"></a>Rezervasyon türünün kullanılamaması ile ilgili sorunları giderme

Bu makale, Azure portalda kullanılabilir olarak görünmeyen ayrılmış örnekleri anlamanıza ve bunlarla ilgili sorunları gidermenize yardımcı olur.

## <a name="symptoms"></a>Belirtiler

1. [Azure portalda](https://portal.azure.com/) oturum açın ve **Rezervasyonlar** ’a gidin.
2. **+ Ekle** seçeneğini belirleyin ve sonra bir ürün seçin.
3. **Tüm Ürünler** sekmesini seçin.
4. Ürün listesindeki bir ürünü seçin. Aşağıdaki iletilerden birini görebilirsiniz:
    - `Product unavailable for the selected subscription or region. Contact support.`  
        :::image type="content" source="./media/troubleshoot-product-not-available/product-unavailable-message.png" alt-text="Ürün seçili abonelik veya bölgede kullanılamıyor iletisinin gösterildiği örnek" lightbox="./media/troubleshoot-product-not-available/product-unavailable-message.png" :::
    - `The selected subscription does not have enough core quota remaining to purchase this product. Request quota increase`  
        :::image type="content" source="./media/troubleshoot-product-not-available/not-enough-core-quota-message.png" alt-text="Ürün seçili abonelik veya bölgede kullanılamıyor iletisinin gösterildiği örnek" lightbox="./media/troubleshoot-product-not-available/not-enough-core-quota-message.png" :::

## <a name="cause"></a>Nedeni

Bazı rezervasyonlar, aşağıdaki nedenlerden dolayı satın alınamaz:

- Ayrılmış örnek ürünlerinin hepsi her bölgede satışa sunulmayabilir
- Aboneliğinizde kota sınırlaması vardır

### <a name="cause-1"></a>1\. Neden

Tüm ayrılmış örnek ürünleri satışa sunulmayabilir. Azure, müşterilere parasal indirim sağlamayla ilişkili maliyetleri dikkate alarak bazı ürünlerin satışına izin verip vermeyeceğine karar verir. Diğer durumlarda Azure, belirli veri merkezlerindeki kapasite koşulları nedeniyle bazı ürünleri sunmaz. Ayrıca, bazı Azure ürün geliştirme grupları eski bir ürünü kullanımdan kaldırmak istediğinden belirli ürünlerin satışına izin verebilir.

Diğer durumlarda Azure, bazı bölgelerdeki kaynaklara yönelik talebi göz önünde bulundurarak bazı ürünlere kapasite sınırlaması uygular. Örneğin, belirli bir sanal makine boyutu bir veri merkezinde hiç talep görmediğinde böyle bir kısıtlama uygulanabilir. Bu örnekte Azure, bu bölgedeki söz konusu boyut için rezervasyon satın alan müşterilere kapasite garantisi veremez. Son olarak, çeşitli nedenlerden dolayı benzersiz olan bazı ürünler vardır. Bu ürünler yalnızca küçük, seçili bir müşteri grubuna sunulur.

Bazı rezervasyonlar satın alınamıyorsa neden Azure portalda gösteriliyor? Çünkü kullanıcılar, istedikleri ürünü bulamadıklarını belirttikleri destek istekleri oluşturuyor. Azure Rezervasyon geliştirme ekibi, `Product unavailable` ileti sonuçlarıyla birlikte tüm ürünleri göstermenin, göstermemeye kıyasla daha az destek isteğine neden olduğunu belirledi.

### <a name="cause-2"></a>2\. Neden

Aboneliğinizde kota sınırlaması vardır. Aboneliklerde, tüketilebilecek CPU çekirdeği sayısıyla ilgili sınırlar vardır. Bu sınır, en çok sanal makinelere olmak üzere bazı ayrılmış örnek ürünlerine uygulanır. Azure, ayrılmış örnek satın alan kişilerin bunlardan en iyi şekilde yararlanmasını sağlamak ister. Azure, aboneliğinizde sanal makineleri dağıtmak ve rezervasyon satın alımınızın avantajlarından yararlanmak üzere yeterli sayıda boşta çekirdek olduğundan emin olmak için Azure portalda basit ve yüzeysel bir denetim yapar.

Sepetinize belirli bir ürün eklemenizi ve rezervasyon satın almanızı sağlayan bu denetim basittir. Azure, aboneliğinizde kullanılabilen toplam CPU çekirdeği sayısını değerlendirir ve bu sayının, seçili öğeye ait çekirdek sayısından daha yüksek olup olmadığını denetler.

Azure, **Paylaşılan** kapsama ait ayrılmış örnekler için kotayı denetlemez. Paylaşılan kapsama ait ayrılmış örnek avantajı, kayıttaki tüm abonelikler için geçerlidir. Azure, tüm aboneliklerinizde kaynağı dağıtmak için yeterli sayıda boşta çekirdeğe sahip olup olmadığınızı belirleyemez. Azure, paylaşılan kapsam seçildiğinde kotadan bağımsız olarak her zaman bir VM boyutu seçmenizi sağlar.

Ayrıca Azure, **Önerilen** satın alımlar için kota denetimi yapmaz. Öneriler etkin kullanımı temel alır. Öneriyi oluşturmak için gereken kullanımı zaten oluşturduğunuzdan, Azure belirli bir VM boyutunu çalıştıracak sayıda çekirdeğe sahip olduğunuzu varsayar.

## <a name="solution"></a>Çözüm

Aldığınız hata iletisine bağlı olarak, sorununuzu çözmek için aşağıdaki çözümlerden birini uygulayın.

### <a name="solution-1"></a>1\. Çözüm

_Ürün kullanılamıyor_ iletisi görüyorsanız aboneliğinize özel durum eklenmesini talep etmek için hata iletisindeki **Desteğe ulaşın** bağlantısını seçin. Özel durumlar her zaman verilmez.

### <a name="solution-2"></a>2\. Çözüm

_Yeterli çekirdek kotası yok_ iletisini görürseniz kapsamı **Paylaşılan** olarak değiştirebilirsiniz. Rezervasyonu satın aldıktan sonra, rezervasyon kapsamını **Paylaşılan** yerine **Tek** olarak değiştirebilirsiniz.

Dilerseniz aboneliğiniz için ek CPU çekirdeği kotası talep etmek amacıyla hata iletisindeki **Kota artışı talep et** bağlantısını seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyon kapsamı seçenekleri hakkında daha fazla bilgi için bkz. [Kapsam rezervasyonları](prepare-buy-reservation.md#scope-reservations).