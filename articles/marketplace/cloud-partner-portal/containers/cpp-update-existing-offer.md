---
title: Varolan Azure Kapsayıcıları teklifini güncelleştirme | Azure Marketi
description: Azure Marketi'nde varolan bir kapsayıcı teklifini güncelleştirme.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: dsindona
ms.openlocfilehash: 74f97b082c07e17a59a1615c4b1245434c497ab5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80279955"
---
# <a name="update-an-existing-container-offer"></a>Varolan bir kapsayıcı teklifini güncelleştirme

Bu makale, [Bulut İş Ortağı Portalı'ndaki](https://cloudpartner.azure.com/)konteyner teklifinizi güncelleştirmenin farklı yönlerini uygular.

Teklifinizi güncelleştirmek istemenizin birkaç nedeni vardır:

-  Mevcut STU'lara yeni bir kapsayıcı görüntüsü sürümü ekleme.
-  Yeni STU'lar ekleme.
-  Teklif veya tek tek SNU'lar için pazar meta verilerini güncelleme.

Bu değişikliklerde size yardımcı olmak için, portal **Karşılaştırma** ve **Geçmiş** özelliklerini sağlar.  


## <a name="unpermitted-changes-to-a-container-offer-or-sku"></a>Konteyner teklifinde veya SKU'da izin verilmemiş değişiklikler

Azure Marketi'nde canlı olarak kullanılabilir olduktan sonra değiştirilemeyecek bir kapsayıcı teklifinin veya SKU'nun öznitelikleri vardır. Aşağıdaki ayarları değiştiremezsiniz:

-  **Teklifin kimliği** ve **Yayıncı Kimliği**
-  Mevcut SKU'ların **SKU Kimliği**
-  Sürüm etiketleri, örneğin:`1.0.1`
-  Mevcut SBI'lerde faturalama/lisans modeli değişiklikleri

## <a name="common-update-operations"></a>Sık güncelleştirme işlemleri

Aşağıdaki güncelleştirme işlemleri yaygındır.

### <a name="update-container-image-version-for-a-sku"></a>Bir SKU için kapsayıcı resim sürümünü güncelleştirme

Bir kapsayıcı görüntüsünün güvenlik yamaları, ek özellikler ve benzeri yollarla düzenli olarak güncellenmesi yaygındır. Bu senaryoda, Aşağıdaki adımları kullanarak SKU'nuzun başvurulmaya vurduğu kapsayıcı görüntüsünü güncelleştirmek istiyorsunuz:

1. [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.
2. **Tüm teklifler**altında, güncellemek istediğiniz teklifi bulun.
3. **SKU** sekmesinde, güncelleştirmek için kapsayıcı görüntüsüyle ilişkili SKU'yu seçin.
4. **Kapsayıcı resminin**altında, yeni bir kapsayıcı resmi eklemek için **+ Yeni Resim Sürümü'nü** seçin.
5. Yeni kapsayıcı **resim sürümlerini**sağlayın. Resim sürümünün önceki sürümler ile aynı etiketler yönergelerine uyması gerekir. Sürüm etiketleri X, Y ve Z'nin tümseger olduğu X.Y.Z formunda olmalıdır. Sağladığınız yeni sürümün önceki tüm sürümlerden daha büyük olduğunu doğrulayın.
6. Yeni kapsayıcı resim sürümünüzü Azure Marketi'nde yayınlamak için iş akışını başlatmak için **Yayımla'yı** seçin.

### <a name="add-a-new-sku"></a>Yeni bir SKU ekle

Yeni bir SKU'yu teklifiniz için kullanılabilir hale getirmek için aşağıdaki adımları kullanın:

1. [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.
2. **Tüm teklifler**altında, güncellemek istediğiniz teklifi bulun.
3. **SKU** sekmesinin altında **yeni SKU ekle'yi** seçin ve açılır pencerede bir **SKU kimliği** sağlayın.
4. [Kapsayıcı teklifini Yayımla'da](./cpp-publish-offer.md)açıklanan adımları kullanarak kapsayıcıyı yeniden yayımlayın.
5. Yeni SKU'nuzu yayınlamak için iş akışını başlatmak için **Yayımla'yı** seçin.

### <a name="update-offer-marketplace-metadata"></a>Teklif pazar meta verilerini güncelleştirin

Teklifinizle ilişkili pazar meta verilerini güncelleştirmek için aşağıdaki adımları kullanın. (Örneğin: şirket adı, logolar ve vb.)

1. [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.
2. **Tüm teklifleraltında,** güncellemek istediğiniz teklifi bulun.
3. **Market** sekmesine gidin. Meta veri değişiklikleri yapmak için [Yayımlama kapsayıcı teklifi](./cpp-publish-offer.md) makalesindeki yönergeleri kullanın.
4. Değişikliklerinizi yayınlamak için iş akışını başlatmak için **Yayımla'yı** seçin.

## <a name="compare-feature"></a>Özelliği karşılaştırın

Yayımlanmış bir teklifte değişiklik yaptığınızda, yaptığınız değişiklikleri denetlemek için **Karşılaştır** özelliğini kullanabilirsiniz.

### <a name="to-use-the-compare-feature"></a>Karşılaştır özelliğini kullanmak için:

1. Düzenleme işleminin herhangi bir noktasında teklifiniz için Karşılaştır'ı seçin.
2. Pazarlama varlıklarının ve meta verilerin yan yana sürümlerine bakın.


## <a name="history-of-publishing-actions"></a>Yayımlama eylemlerinin tarihi

Geçmiş yayımlama etkinliğini görmek için Bulut İş Ortağı Portalı'nın sol daki gezinti menüsü çubuğundaki **Geçmiş** sekmesini seçin. Azure Marketi tekliflerinizin ömrü boyunca zaman damgalı eylemleri görebilirsiniz.
