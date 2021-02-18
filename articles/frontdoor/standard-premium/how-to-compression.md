---
title: Azure ön kapı Standart/Premium 'daki dosyaları sıkıştırarak performansı geliştirme (Önizleme)
description: Azure ön kapısındaki dosyalarınızı sıkıştırarak dosya aktarım hızını geliştirmeyi ve sayfa yükleme performansını artırmayı öğrenin.
services: front-door
author: duongau
ms.service: frontdoor
ms.topic: article
ms.date: 02/18/2021
ms.author: yuajia
ms.openlocfilehash: 4b526d82465862b1c0d27aed6443c6d7199bfb5b
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101100412"
---
# <a name="improve-performance-by-compressing-files-in-azure-front-door-standardpremium-preview"></a>Azure ön kapı Standart/Premium 'daki dosyaları sıkıştırarak performansı geliştirme (Önizleme)

> [!Note]
> Bu belge, Azure ön kapısının Standart/Premium (Önizleme) içindir. Azure ön kapısı hakkında bilgi mi arıyorsunuz? [Burada](../front-door-overview.md)görüntüleyin.

Dosya sıkıştırma, dosya aktarım hızını artırmak ve sayfa yükleme performansını artırmak için etkili bir yöntemdir. Sıkıştırma, sunucu tarafından gönderilmeden önce dosyanın boyutunu azaltır. Dosya sıkıştırma, bant genişliği maliyetlerini azaltabilir ve kullanıcılarınız için daha iyi bir deneyim sağlayabilir.

> [!IMPORTANT]
> Azure ön kapı Standart/Premium (Önizleme) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
Dosya sıkıştırmasını etkinleştirmenin iki yolu vardır:

- Kaynak sunucunuzda sıkıştırmayı etkinleştirme. Azure ön kapısı, sıkıştırılmış dosyalar üzerinde geçiş yapar ve bunları isteyen istemcilere gönderir.
- Doğrudan Azure ön kapısı POP sunucularında sıkıştırmayı etkinleştirme (*anında sıkıştırma*). Bu durumda, Azure ön kapısı dosyaları sıkıştırır ve son kullanıcılara gönderir.

> [!IMPORTANT]
> Azure ön kapı yapılandırma değişikliklerinin, ağ genelinde yayılması 10 dakika kadar sürer. CDN uç noktanız için ilk kez sıkıştırma ayarlıyorsanız, sıkıştırma ayarlarının tüm pop 'Lara yayıldığından emin olmak için sorun gidermeye başlamadan önce 1-2 saat beklemeyi göz önünde bulundurun.

## <a name="enabling-compression"></a>Sıkıştırmayı etkinleştirme

> [!Note]
> Azure ön kapıda sıkıştırma, rotada **önbelleğe almayı etkinleştirme** parçasıdır. Yalnızca **önbelleğe almayı etkinleştirdiğinizde**, Azure ön kapısının sıkıştırmasından faydalanabilirsiniz.

Sıkıştırmayı aşağıdaki yollarla etkinleştirebilirsiniz:
* Hızlı oluşturma sırasında-önbelleğe almayı etkinleştirdiğinizde sıkıştırmayı etkinleştirebilirsiniz.
* Özel Oluştur-bir rota eklerken önbelleğe almayı ve sıkıştırmayı etkinleştirin. 
* Endpoint Manager rotasında.
* En Iyi duruma getirme sayfasında.

### <a name="enable-compression-in-endpoint-manager"></a>Endpoint Manager 'da sıkıştırmayı etkinleştir

1. Azure ön kapısı Standart/Premium profili sayfasından **Endpoint Manager** ' a gidin ve sıkıştırmayı etkinleştirmek istediğiniz uç noktayı seçin.

1. **Uç noktayı Düzenle**' yi seçin ve ardından sıkıştırmayı etkinleştirmek istediğiniz **yolu** seçin. 

   :::image type="content" source="../media/how-to-compression/front-door-compression-endpoint-manager-1.png" alt-text="Endpoint Manager giriş sayfasının ekran görüntüsü." lightbox="../media/how-to-compression/front-door-compression-endpoint-manager-1-expanded.png":::   

1. **Önbelleği etkinleştir** ' in işaretli olduğundan emin olun, sonra **sıkıştırmayı etkinleştir** onay kutusunu seçin.

   :::image type="content" source="../media/how-to-compression/front-door-compression-endpoint-manager-2.png" alt-text="Endpoint Manager 'da sıkıştırmayı etkinleştirin.":::   

1. Yapılandırmayı kaydetmek için **Güncelleştir** ' i seçin.

### <a name="enable-compression-in-optimization"></a>Iyileştirmede sıkıştırmayı etkinleştir

1. Azure ön kapısı Standart/Premium profili sayfasından ayarlar ' ın altında **iyileştirmeler** ' a gidin. Yolların listesini görmek için uç noktayı genişletin. 

1. Sıkıştırmanın *devre dışı* olduğu **yolun** yanındaki üç noktayı seçin. Ardından **rotayı Yapılandır**' ı seçin.

   :::image type="content" source="../media/how-to-compression/front-door-compression-optimization-1.png" alt-text="İyileştirme sayfasında sıkıştırmayı etkinleştir ekranı." lightbox="../media/how-to-compression/front-door-compression-optimization-1-expanded.png"::: 

1. **Önbelleği etkinleştir** ' in işaretli olduğundan emin olun, sonra **sıkıştırmayı etkinleştir** onay kutusunu seçin.

     :::image type="content" source="../media/how-to-compression/front-door-compression-endpoint-manager-2.png" alt-text="Endpoint Manager 'da sıkıştırmayı etkinleştirme ekran görüntüsü."::: 

1. **Güncelleştir**’e tıklayın.

## <a name="modify-compression-content-type"></a>Sıkıştırma içerik türünü değiştir

En Iyileştirmeleri sayfasında MIME türleri varsayılan listesini değiştirebilirsiniz.

1. Azure ön kapısı Standart/Premium profili sayfasından ayarlar ' ın altında **iyileştirmeler** ' a gidin. Ardından, sıkıştırma *etkin* olan **yolu** seçin.

1. Sıkıştırmanın *etkinleştirildiği* **yolun** yanındaki üç noktayı seçin. Ardından **Sıkıştırılmış dosya türlerini görüntüle**' yi seçin.

   :::image type="content" source="../media/how-to-compression/front-door-compression-edit-content-type.png" alt-text="İyileştirme sayfasının ekran görüntüsü." lightbox="../media/how-to-compression/front-door-compression-edit-content-type-expanded.png"::: 

1. Varsayılan biçimleri silin veya yeni içerik türleri eklemek için **Ekle** ' yi seçin.

   :::image type="content" source="../media/how-to-compression/front-door-compression-edit-content-type-2.png" alt-text="Dosya sıkıştırmayı özelleştirme sayfasının ekran görüntüsü."::: 

1. Sıkıştırma yapılandırmasını güncelleştirmek için **Kaydet**' i seçin.

## <a name="disabling-compression"></a>Sıkıştırmayı devre dışı bırakma

Sıkıştırmayı aşağıdaki yollarla devre dışı bırakabilirsiniz:
* Endpoint Manager rotasında sıkıştırmayı devre dışı bırakın.
* Iyileştirme sayfasında sıkıştırmayı devre dışı bırakın.

### <a name="disable-compression-in-endpoint-manager"></a>Endpoint Manager 'da sıkıştırmayı devre dışı bırak

1. Azure ön kapısı Standart/Premium profili sayfasından ayarlar ' ın altında **Endpoint Manager** ' a gidin. Sıkıştırmayı devre dışı bırakmak istediğiniz uç noktayı seçin.

1. **Uç noktayı Düzenle** ' yi seçin ve ardından sıkıştırmayı devre dışı bırakmak istediğiniz **yolu** seçin. **Sıkıştırmayı etkinleştir** kutusunun işaretini kaldırın.

1. Yapılandırmayı kaydetmek için **Güncelleştir** ' i seçin.

### <a name="disable-compression-in-optimizations"></a>Iyileştirmelerin sıkıştırmasını devre dışı bırak

1. Azure ön kapısı Standart/Premium profili sayfasından ayarlar ' ın altında **iyileştirmeler** ' a gidin. Ardından, sıkıştırma *etkin* olan **yolu** seçin.

1. Sıkıştırmanın *etkinleştirildiği* **yolun** yanındaki üç noktayı seçin ve ardından *rotayı Yapılandır*' ı seçin.

    :::image type="content" source="../media/how-to-compression/front-door-disable-compression-optimization.png" alt-text="İyileştirme sayfasında sıkıştırmayı devre dışı bırakma ekranının ekran görüntüsü."::: 

1. **Sıkıştırmayı etkinleştir** kutusunun işaretini kaldırın.

    :::image type="content" source="../media/how-to-compression/front-door-disable-compression-optimization-2.png" alt-text="Sıkıştırmayı devre dışı bırakmak için güncelleştirme yolu sayfasının ekran görüntüsü."::: 

1. Yapılandırmayı kaydetmek için **Güncelleştir** ' i seçin.

## <a name="compression-rules"></a>Sıkıştırma kuralları

Azure ön kapıda yalnızca uygun dosyalar sıkıştırılır. Sıkıştırmaya uygun olması için bir dosya şunları içermelidir:
* MIME türünde olmalıdır 
* 1 KB 'den büyük olmalıdır
* 8 MB 'tan küçük olmalıdır

Bu profiller aşağıdaki sıkıştırma kodlamalarını destekler:
* gzip (GNU zip)
* brotli 

İstek birden fazla sıkıştırma türünü destekliyorsa, brotli sıkıştırması önceliklidir.

Bir varlık için bir istek gzip sıkıştırmasını belirttiğinde ve istek bir önbellek isabetsizliği ile sonuçlanırsa, Azure ön kapısının doğrudan POP sunucusunda, varlığın gzip sıkıştırması olur. Daha sonra, sıkıştırılan dosya önbellekten sunulur.

Kaynak, Azure ön kapısına sıkıştırılmış veri göndermek için öbekli aktarım kodlaması (CTE) kullanıyorsa, 8 MB 'tan büyük yanıt boyutları desteklenmez. 

## <a name="next-steps"></a>Sonraki adımlar

- İlk [kural kümesini](how-to-configure-rule-set.md) nasıl yapılandıracağınızı öğrenin
- [Kural kümesi eşleşme koşulları](concept-rule-set-match-conditions.md) hakkında daha fazla bilgi edinin
- [Azure ön kapı kural kümesi](concept-rule-set.md) hakkında daha fazla bilgi edinin
