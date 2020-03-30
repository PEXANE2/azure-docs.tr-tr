---
title: Pazar yeri tekliflerini güncelleştir | Azure Marketi
description: Bulut İş Ortağı Portalını kullanarak Azure ve AppSource Pazar yerlerine ilişkin teklifleri güncelleştirin
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: dsindona
ms.openlocfilehash: 101369a050770be3acd9534cef6229037fe1c366
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288521"
---
# <a name="update-azure-marketplace-and-appsource-offers"></a>Azure Marketi ve AppSource tekliflerini güncelleştir

Yayımlandıktan sonra teklifinize uygulayabileceğiniz çeşitli güncelleştirmeler vardır.  [Bulut İş Ortağı Portalı,](https://cloudpartner.azure.com/) aşağıdakiler de dahil olmak üzere bir teklifin özniteliklerini düzgün bir şekilde değiştirmenize yardımcı olur:

-  Mevcut bir SKU'ya yeni sanal makine (VM) görüntü veya paket sürümü ekleme
-  Bir SKU'nun bulunduğu bölgeleri değiştirin
-  Yeni STU'lar ekleme
-  Teklifler veya SDO'lar için pazar meta verilerini güncelleme 
-  Çıktıkça öde teklifleri için fiyatlandırmayı güncelleme

Portal, özellikleri karşılaştırma ve bir teklifin geçmişlerini görüntüleme gibi değişiklikleri yönetmene yardımcı olan özelliklere de sahiptir.  Bir teklifi veya SKU'yu değiştirdikten sonra, değişiklikler "canlı" olarak gitmeden önce yeniden yayımlanmalıdır.  Bu makale, pazar teklifi güncelleme farklı yönleri ile size yol göstermektedir.

## <a name="unpermitted-changes-to-an-offersku"></a>Teklif/SKU'da izin verilmemiş değişiklikler

Pazarda yayımlandıktan sonra değiştirilemeyen bir teklifin veya SKU'nun bazı özellikleri vardır.  İlgili alanlar, örneğin portalın **Düzenleyici** sekmesinde devre dışı bırakılır:  

- Teklif kimliği ve Yayıncı Kimliği
- SKU KİmLİk 
- Varolan SK'lerin veri disk sayısı
- Mevcut SBI'lerin faturalandırma/lisans modeli değişiklikleri
- Sürüm etiketleri, örneğin:`1.0.1`


## <a name="common-update-operations"></a>Sık güncelleştirme işlemleri

Aşağıdaki bölümlerde en güncelleştirme işlemlerinden bazılarının nasıl gerçekleştirililmeyene açıklanmıştır.  Bu işlemler tüm teklif türleri için kullanılamaz.  Bu işlemlerden herhangi birini başlatmak için Bulut İş Ortağı Portalı'nda oturum açmanız gerekir.


### <a name="update-offer-contacts"></a>Teklif kişilerini güncelleştir

Teklifiniz için destek kişilerini güncelleştirmek için aşağıdaki adımları kullanın.
1. Tüm **Teklifler** sayfasında teklifi seçin.
2. **Kişiler** sekmesini seçin. Kişilerinizi güncelleştirin.
3. **Kaydet** düğmesini seçin.
4. Yayımlama işlemini başlatmak için **Yayımla'yı** seçin.


### <a name="change-regions-an-offer-or-sku-is-available-in"></a>Bir teklifin veya SKU'nun mevcut olduğu bölgeleri değiştirin

Zaman içinde, teklifinizi/SKU'nuzu daha fazla bölgede kullanılabilir hale getirmek isteyebilirsiniz.
Alternatif olarak, belirli bir bölgede teklifi/SKU'yu desteklemeyi durdurmak isteyebilirsiniz.
Bu değişiklikleri uygulamak için aşağıdaki adımları izleyin.

1. Tüm **teklifler** sayfasında, güncellemek istediğiniz teklifi bulun.

Azure Marketi teklifleri için:

1. **SK'ler** sekmesini seçin.  Değiştirmek için SKU'yu seçin.
1. **Ülke/Bölge kullanılabilirlik** alanının altındaki **Ülkeleri Seç** düğmesini tıklatın.
1. Bölge kullanılabilirliği iletişim kutusunda, bu SKU için bölgeleri ekleyin veya kaldırın.

AppSource teklifleri için:

1. **Storefront Ayrıntılar** sekmesini seçin.
1. **Desteklenen ülkeler/bölgeler** etiketinin yanında **Desteklenen ülkeler/bölgeler'i**tıklatın. 
1. Desteklenen ülkeler/bölgeler iletişim kutusunda, bu teklif için bölgeleri ekleyin veya kaldırın.

Her iki pazar için:

1. Yayımlama işlemini başlatmak için **Yayımla'yı** tıklatın. 

Yeni bir bölgede bir SKU kullanıma sunuluyorsa, **Dışa Aktarma Fiyatlandırma Verileri** işlevi aracılığıyla söz konusu bölgenin fiyatlandırmasını belirtme olanağınız vardır. Daha önce kullanılabilen bir bölgeyi geri ekliyorsanız, fiyatlandırma değişikliklerine izin verilmediği için fiyatlandırmasını güncelleştiremezsiniz.


### <a name="add-a-new-sku"></a>Yeni bir SKU ekle 

Mevcut bir teklif için yeni bir SKU kullanılabilir hale getirmek için aşağıdaki adımları kullanın:

1. Tüm **teklifler** sayfasında teklifi bulun.
3. **SKU** formunun altında **yeni SKU ekle'yi** tıklatın ve açılır pencerede bir **SKU kimliği** sağlayın.
4. [Sanal makine teklifini yayımla'da](../virtual-machine/cpp-publish-offer.md)ayrıntılı olarak belirtilen adımların geri kalanını izleyin.
5. Yayımlama işlemini başlatmak için **Yayımla'yı** tıklatın.


### <a name="update-offer-marketplace-assets"></a>Teklif pazar varlıklarını güncelleştirin

Şirket logolarınız, teklif açıklamanız vb. gibi pazar daki metin tabanlı ve görüntü varlıklarını güncellemeniz gereken senaryolarınız olabilir. Bu varlıkları güncelleştirmek için aşağıdaki adımları kullanın.

1. Tüm **teklifler** sayfasında, teklifinizi bulun. 
2. **Market** sekmesini seçin ve teklifinizin *Market sekmesi* ndeki yönergeleri izleyin.
3. Yayımlama işlemini başlatmak için **Yayımla'yı** tıklatın.


### <a name="update-pricing-on-published-offers"></a>Yayımlanmış tekliflerdeki fiyatlandırmayı güncelleştir

Her an öde teklifiniz yayınlandıktan sonra, mevcut bir SKU'nun fiyatını artıramazsınız.  Bunun yerine, aynı teklif altında bir SKU oluşturun, eski SKU'yu silin ve teklifinizi yeniden yayımlayın. Daha önce yayınlanmış tekliflerin fiyatını düşürebilirsiniz. Teklif fiyatınızı düşürmek için:

1. Fiyatlandırmayı düşürmek istediğiniz SKU'yu seçin.
2. Daha düşük fiyatı, ilk kullandığınız mekanizmaya göre ayarlamanız gerekir: doğrudan portal Kullanıcı Arabirimi'nde veya alma/dışa aktarma tablosunda.
3. **Kaydet**'e tıklayın.
4. Yayımlama işlemini başlatmak için **Yayımla'yı** tıklatın.

Fiyatlandırma, pazarda canlı olarak yayınlanınca yeni müşteriler tarafından görülebilir ve tüm yeni müşteriler yeni azalan fiyatı ödeyecektir.  Mevcut müşteriler için fiyat düşüşü, fiyat düşüşünün etkili olduğu faturalandırma döngüsünün başlangıcına geriye dönük olarak yansıtılır.  Fiyat düşüşlerinin meydana geldiği döngü için önceden faturalandırıldıysa, azalan fiyatı karşılamak için bir sonraki faturalandırma döngüsü sırasında geri ödeme alırlar.


## <a name="compare-feature"></a>Özelliği karşılaştırın

Yayımlanmış bir teklifte değişiklik yaptığınızda, değişiklikleri denetlemek için *Karşılaştır* özelliğini kullanabilirsiniz. Bu özelliği kullanmak için:

1. Düzenleme işleminin herhangi bir noktasında, teklifiniz için **Düzenleyici** sekmesindeki **Karşılaştır** düğmesini tıklatabilirsiniz.
2. Karşılaştırma penceresi, pazar teklifiyle karşılaştırıldığında bu teklifte kaydedilen değişikliklerin yan yana sürümlerini görüntüler. 

![Düzenleyici sekmesinde teklif düğmesini karşılaştır](./media/offer-compare-button.png)


## <a name="history-of-publishing-actions"></a>Yayımlama eylemlerinin tarihi

Geçmiş yayımlama etkinliğini görüntülemek için Bulut İş Ortağı Portalı'nın sol dikey menü çubuğundaki **Geçmiş** sekmesini seçin.  Geçmiş sayfası çeşitli özelliklere göre esnek filtreleme sağlar ve sütun sıralamayı destekler.  Her yayımlama olayı zaman damgası ile damgalanır.  Daha fazla bilgi için Denetim [geçmişi sayfasına](../portal-tour/cpp-history-page.md)bakın.


## <a name="next-steps"></a>Sonraki adımlar

[Ayrıca, yayınlanan bir SKU'yu veya teklifi silmek](./cpp-delete-offer.md)için Bulut İş Ortağı Portalını da kullanabilirsiniz.
