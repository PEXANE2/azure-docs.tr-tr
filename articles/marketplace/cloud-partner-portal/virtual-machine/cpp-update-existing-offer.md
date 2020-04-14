---
title: Azure Marketi'ndeki mevcut bir VM teklifini güncelleştirme
description: Azure Marketi'nde varolan bir VM teklifini nasıl güncelleştireceğimi açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: dsindona
ms.openlocfilehash: a15ccb1de2a9ce0072d032e624ead3b4d730763b
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273061"
---
# <a name="update-an-existing-vm-offer-on-azure-marketplace"></a>Azure Marketi'nde varolan bir VM teklifini güncelleştirme

> [!IMPORTANT]
> 13 Nisan 2020'den itibaren Azure Sanal Makine tekliflerinizin yönetimini İş Ortağı Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure Sanal Makine Oluştur teklifindeki](https://aka.ms/CreateAzureVMoffer) yönergeleri izleyin.

Bu makale, [Bulut İş Ortağı Portalı'ndaki](https://cloudpartner.azure.com/) sanal makine (VM) teklifinizi güncelleştirmenin ve teklifi yeniden yayımlamanın farklı yönlerini size iletir. 

Teklifinizi güncellemeniz için sık karşılaşılan nedenler şunlardır:

-  Mevcut STU'lara yeni bir VM resim sürümü ekleme
-  SKU'nun kullanılabilen bölgeleri değiştirme
-  Yeni STU'lar ekle
-  Teklif veya tek tek SNU'lar için pazar meta verilerini güncelleştirme
-  Gittikçe öde teklifleri nin fiyatlandırmasını güncelleştir

Bu değişikliklerde size yardımcı olmak için, portal **Karşılaştırma** ve **Geçmiş** özelliklerini sunar.  

>[!Note]
>Bulut Çözüm Sağlayıcıları (CSP) iş ortağı kanal tercihi artık kullanılabilir.  Microsoft CSP iş ortağı kanalları aracılığıyla teklifinizi pazarlama hakkında daha fazla bilgi için lütfen [Bulut Çözüm Sağlayıcıları'na](../../cloud-solution-providers.md) bakın.

## <a name="unpermitted-changes-to-vm-offer-or-sku"></a>VM teklifinde veya SKU'da izin verilmemiş değişiklikler

Teklif Azure Marketi'nde canlı olarak yayınlanınca değiştirileemeyen bir VM teklifinin veya SKU'nun bazı öznitelikleri vardır:

-  **Teklifin kimliği** ve **Yayıncı Kimliği**
-  Mevcut SKU'ların **SKU Kimliği**
-  Varolan SK'lerin veri disk sayısı
-  Mevcut SBI'lerde faturalama/lisans modeli değişiklikleri
-  Yayınlanan SKU'ya fiyat artışları


## <a name="common-update-operations"></a>Sık güncelleştirme işlemleri

VM teklifinde değiştirebileceğiniz çok çeşitli özellikler olmasına rağmen, aşağıdaki işlemler yaygındır.

### <a name="update-the-vm-image-version-for-a-sku"></a>Bir SKU için VM görüntü sürümünü güncelleştirme

VM görüntüsünün güvenlik yamaları, ek özellikler ve benzeri yollarla düzenli olarak güncellenmesi yaygındır.  Bu tür senaryolar altında, Aşağıdaki adımları kullanarak SKU başvurur VM görüntü güncelleştirmek istiyorum:

1.  [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.

2.  **Tüm teklifler**altında, güncelleme teklifi bulabilirsiniz.

3.  **SKU** sekmesinde, güncelleştirmek için VM resmiyle ilişkili SKU'ya tıklayın.

4.  **Disk sürümü**altında, yeni bir VM resmi eklemek için **+Yeni Disk Sürümü'ne** tıklayın.

5.  Yeni VM Images **Disk sürümünü**sağlayın. Disk sürümünün [anlamsal sürüm](https://semver.org/) biçimini izlemesi gerekir. Sürümler X, Y ve Z'nin tümseger olduğu X.Y.Z formunda olmalıdır. Sağladığınız yeni sürümün önceki tüm sürümlerden daha büyük olduğunu doğrulayın; aksi takdirde yeni sürümü yeniden yayımladıktan sonra portalda veya Azure Marketi'nde görüntülenmeyecek.

6.  **OS VHD URL için,** işletim sistemi VHD için oluşturulan [paylaşılan erişim imzası (SAS) URI'yi](./cpp-get-sas-uri.md) girin. 

    > [!WARNING] 
    > Veri diski sayısı SKU'nun farklı sürümleri arasında değiştirilemez. Önceki sürümlerde yapılandırılmış veri diskleri varsa, bu yeni sürüm de aynı sayıda veri diski olmalıdır.

7.  Yeni VM sürümünüzü Azure Marketi'nde yayınlamak için iş akışını başlatmak için **Yayımla'yı** tıklatın.


### <a name="change-region-availability-of-a-sku"></a>Bir SKU'nun bölge kullanılabilirliğini değiştirme

Zaman içinde, teklifinizi/SKU'nuzu daha fazla bölgede kullanılabilir hale getirmek isteyebilirsiniz.  Alternatif olarak, belirli bir bölgede teklifi/SKU'yu desteklemeyi durdurmak isteyebilirsiniz.
Kullanılabilirliği değiştirmek için aşağıdaki adımları kullanın:

1.  [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.

2.  **Tüm teklifler** altında güncellemek istediğiniz teklifi bulabilirsiniz.

3.  **SKU** sekmesinde, kullanılabilirliğini değiştirmek istediğiniz SKU'yu tıklatın.

4.  **Ülke/Bölge kullanılabilirlik** alanı altındaki **Ülkeleri Seç** düğmesine tıklayın.

5.  Bölgede kullanılabilirlik açılır pencere, ekleyin veya bu SKU için bölgeleri kaldırın.

6.  SK'ler bölge kullanılabilirliğinizi güncelleştirmek için yayımlama iş akışını başlatmak için **Yayımla'yı** tıklatın.

Yeni bir bölgede bir SKU kullanıma sunuluyorsa, **Dışa Aktarma Fiyatlandırma Verileri** işlevi aracılığıyla söz konusu bölge için fiyatlandırma belirtme olanağınız olacaktır. Daha önce kullanılabilir olan bir bölgeyi geri ekliyorsanız, fiyatlandırma değişikliklerine izin verilmediği için fiyatlandırmasını güncelleştiremesiniz.


### <a name="add-a-new-sku"></a>Yeni bir SKU ekle

Mevcut teklifiniz için yeni bir SKU kullanılabilir hale getirmek için aşağıdaki adımları kullanın: 

1.  [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.

2.  **Tüm teklifler** altında güncellemek istediğiniz teklifi bulabilirsiniz.

3.  **SKU** sekmesinin altında, **yeni SKU ekle'ye** tıklayın ve açılır pencerede bir **SKU kimliği** sağlayın.

4.  Makalede ayrıntılı olarak belirtildiği gibi VM'yi yeniden yayımlayın [Sanal makineyi Azure Marketi'nde yayımlayın.](./cpp-publish-offer.md)

5.  Yeni SKU'nuzu yayınlamak için iş akışını başlatmak için **Yayımla'yı** tıklatın.


### <a name="update-offer-marketplace-metadata"></a>Teklif pazar meta verilerini güncelleştirin

Teklifinizle ilişkili pazar meta verilerini (şirket adı, logolar, vb.) güncellemek için aşağıdaki adımları kullanın: 

1.  [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.

2.  **Tüm teklifler** altında güncellemek istediğiniz teklifi bulabilirsiniz.

3.  **Market** sekmesine gidin ve meta veri değişiklikleri yapmak [için sanal bir makineyi Azure Marketi'ne yayımlama](./cpp-publish-offer.md) makaledeki yönergeleri izleyin.

4.  Değişikliklerinizi yayınlamak için iş akışını başlatmak için **Yayımla'yı** tıklatın.


### <a name="update-pricing-on-published-offers"></a>Yayınlanan Tekliflerdeki Fiyatlandırmayı Güncelleştir

Gittikçe öde teklifiniz yayınlandıktan sonra, SKU fiyatlandırmasını doğrudan artıramazsınız.  (Ancak, aynı teklif altında yeni bir SKU oluşturabilir, eski SKU'yu silebilir ve ardından yeni müşteriler için teklifinizi yeniden yayımlayabilirsiniz.)  Buna karşılık, aşağıdaki adımları kullanarak yayımlanmış bir teklifin fiyatını düşürebilirsiniz:

1.  [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.

2.  **Tüm teklifler**altında, güncelleme teklifi bulabilirsiniz.

3.  Fiyatlandırmayı düşürmek istediğiniz SKU'ya tıklayın.

4.  Fiyatlandırmayı 1x1 GUI'de ayarladıysanız, fiyatı doğrudan UI'de değiştirebilirsiniz. Fiyatlandırmayı alma/dışa aktarma tablosu yla ayarlarsanız, fiyatları yalnızca alma/dışa aktarma özelliği yle düşürebilirsiniz.

3.  **Kaydet**'e tıklayın.

4.  Değişikliklerinizi yayınlamak için iş akışını başlatmak için **Yayımla'yı** tıklatın.

Yeni azalan fiyatlandırma, web sitesinde canlı olarak yeni müşteriler tarafından görülebilir.  Bu yeni fiyat müşterilerinizi aşağıdaki şekillerde etkileyecektir:

- Yeni müşterilerden bu yeni fiyat tahsil edilecektir. 
- Mevcut müşteriler için fiyat düşüşü, fiyat düşüşünün etkili olduğu faturalandırma döngüsünün başlangıcına geriye dönük olarak yansıtılacaktır.
Fiyat düşüşlerinin meydana geldiği döngü için önceden faturalandırıldıysa, azalan fiyatı karşılamak için bir sonraki faturalandırma döngüsü sırasında geri ödeme alırlar.


<!-- TD: This has been implemented, need to change the SKU Tab topic to reflect and move this section there. -->
### <a name="simplified-currency-pricing"></a>Basitleştirilmiş Döviz Fiyatlandırması

1 Eylül 2018 tarihinden itibaren **basitleştirilmiş döviz fiyatlandırması** adlı yeni bir bölüm portala eklenecektir. Microsoft, dünyanın dört bir yanındaki müşterilerinizden daha öngörülebilir fiyatlandırma ve koleksiyonlar sağlayarak Azure Marketi işini kolaylaştırıyor. Bu düzene, müşterilerinize fatura verdiğimiz para birimi sayısını azaltmak da dahildir.

Yeni bölüm bu yeni para birimlerinde fiyatlandırma alacak.Tüm müşteriler bu yeni kapatma para birimlerine geçirildikten sonra, orijinal fiyatlandırma bölümü kullanımdan kaldırılacak ve yalnızca Basitleştirilmiş Para Birimi Fiyatlandırması bölümü kalır.

Uzlaşma para biriminin değiştiği bölgeler için yeni bir fiyat belirlemek için 1 Kasım 2018 tarihine kadar süreniz olacak. Uzlaşma para biriminin değişmediği bölgelerin fiyatını artıramazsınız.

> [!NOTE] 
> Teklifinizi yayınlamak için API'leri kullanırsanız, Teklif JSON'da yeni bir bölüm görebilirsiniz. Bu, teklifin türüne `virtualMachinePricingV2` `monthlyPricingV2`bağlı olarak veya , olarak açıklamalı olacaktır. 

Bu değişiklikle ilgili herhangi bir sorunuz varsa Azure [Marketi Desteği'ne](../../support-azure-marketplace.md)başvurun.


## <a name="compare-feature"></a>Özelliği Karşılaştır

Zaten yayımlanmış bir teklifte değişiklik yaptığınızda, yapılan değişiklikleri denetlemek için **Karşılaştırma** özelliğinden yararlanabilirsiniz. Bu özelliği kullanmak için:

1.  Düzenleme işleminin herhangi bir noktasında, teklifiniz için **Karşılaştır** düğmesini tıklatın.

    ![Özellik düğmesini karşılaştır](./media/publishvm_037.png)


2.  Pazarlama varlıklarının ve meta verilerin yan yana sürümlerini görüntüleyin.


## <a name="history-of-publishing-actions"></a>Yayın Eylemlerinin Tarihi

Herhangi bir geçmiş yayımlama etkinliğini görüntülemek için Bulut İş Ortağı Portalı'nın sol navigasyon menü çubuğundaki **Geçmiş** öğesini tıklatın. Burada Azure Marketi tekliflerinizin ömrü boyunca gerçekleştirilen zaman damgalı eylemleri görüntüleyebilirsiniz.  
<!-- TD: Add after section authored: For more information, see [History page](../portal-tour/cpp-history-page.md). -->

