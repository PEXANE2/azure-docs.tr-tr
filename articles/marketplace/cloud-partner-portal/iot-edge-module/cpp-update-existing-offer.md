---
title: Mevcut azure IoT Edge modül teklifini güncelleştirme | Azure Marketi
description: Azure Marketi'nde varolan bir IoT Edge modül teklifini güncelleştirme.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: 821007e40eaa91c98f157ee0b28fe01f37e3cbc9
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80743951"
---
# <a name="update-an-existing-iot-edge-module-offer"></a>Mevcut bir IoT Edge modül teklifini güncelleştirme

>[!Important]
>30 Mart 2020'den itibaren, IoT Edge modül tekliflerinizi İş Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Bir IoT Edge modülü teklifi oluştur'daki](https://aka.ms/AzureCreateIoT) yönergeleri izleyin.

Bu makalede, [Bulut İş Ortağı Portalı'ndaki](https://cloudpartner.azure.com/) IoT Edge modül teklifinizi güncelleştirmenin ve teklifi yeniden yayımlamanın farklı yönlerini temel almaktadır.

Teklifinizi güncelleştirmek istemenizin birkaç nedeni vardır:

-  Mevcut STU'lara yeni bir IoT Edge modül görüntü sürümü ekleme.
-  Yeni STU'lar ekleme.
-  Teklif veya tek tek SNU'lar için pazar meta verilerini güncelleme.

Bu değişikliklerde size yardımcı olmak için, portal **Karşılaştırma** ve **Geçmiş** özelliklerini sunar.  


## <a name="unpermitted-changes-to-iot-edge-module-offer-or-sku"></a>IoT Edge modül teklifinde veya SKU'da izin verilmemiş değişiklikler

Teklif Azure Marketi'nde canlı olarak yayınlanındıktan sonra değiştirilemeyecek bir IoT Edge modülü teklifinin veya SKU'nun öznitelikleri vardır. Aşağıdaki ayarları değiştiremezsiniz:

-  **Teklifin kimliği** ve **Yayıncı Kimliği**
-  Mevcut SKU'ların **SKU Kimliği**
-  Sürüm etiketleri, örneğin:`1.0.1`
-  Mevcut SBI'lerde faturalama/lisans modeli değişiklikleri

## <a name="common-update-operations"></a>Sık güncelleştirme işlemleri

Aşağıdaki güncelleştirme işlemleri yaygındır.

### <a name="update-the-iot-edge-module-image-version-for-a-sku"></a>Bir SKU için IoT Edge modül görüntü sürümünü güncelleştirin

Bir IoT Edge modül görüntüsünün güvenlik yamaları, ek özellikler ve benzeri özelliklerle düzenli olarak güncellenmesi yaygındır. Bu senaryoda, SKU'nuzun başvurulması gereken IoT Edge modül görüntüsünü aşağıdaki adımları kullanarak güncelleştirmek istiyorsunuz:

1.  [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.

2.  **Tüm teklifler**altında, güncellemek istediğiniz teklifi bulun.

3.  **SKU** sekmesinde, güncelleştirmek için IoT Edge modül görüntüsüyle ilişkili SKU'yu seçin.

4.  **Edge modül görüntüsü**altında , yeni bir IoT Edge modül görüntüsü eklemek için + Yeni Resim **Sürümü** seçin.

5.  Yeni IoT Edge modül **görüntü sürümlerini**sağlayın. Resim sürümünün önceki sürümler ile aynı etiketler yönergelerine uyması gerekir. Sürüm etiketleri X, Y ve Z'nin tümseger olduğu X.Y.Z formunda olmalıdır. Sağladığınız yeni sürümün önceki tüm sürümlerden daha büyük olduğunu doğrulayın.

6.  Yeni IoT Edge modül sürümünüzü Azure Marketi'nde yayınlamak için iş akışını başlatmak için **Yayımla'yı** seçin.

### <a name="add-a-new-sku"></a>Yeni bir SKU ekle

Yeni bir SKU'yu teklifiniz için kullanılabilir hale getirmek için aşağıdaki adımları kullanın: 

1.  [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.

2.  **Tüm teklifler**altında, güncellemek istediğiniz teklifi bulun.

3.  **SKU** sekmesinin altında **yeni SKU ekle'yi** seçin ve açılır pencerede bir **SKU kimliği** sağlayın.

4.  IoT Edge modülünde açıklanan adımları kullanarak [IoT Edge modüllerini Azure Marketi'ne yayımla'yı](./cpp-publish-offer.md)yeniden yayımlayın.

5.  Yeni SKU'nuzu yayınlamak için iş akışını başlatmak için **Yayımla'yı** seçin.


### <a name="update-offer-marketplace-metadata"></a>Teklif pazar meta verilerini güncelleştirin

Teklifinizle ilişkili pazar meta verilerini güncelleştirmek için aşağıdaki adımları kullanın. (Örneğin: şirket adı, logolar, vb.)

1.  [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.

2.  **Tüm teklifler**altında, güncellemek istediğiniz teklifi bulun.

3.  **Market** sekmesine gidin. Meta veri değişiklikleri yapmak [için IoT Edge modülündeki yönergeleri Azure Marketi makalesine](./cpp-publish-offer.md) yayımla'yı kullanın.

4.  Değişikliklerinizi yayınlamak için iş akışını başlatmak için **Yayımla'yı** seçin.

## <a name="compare-feature"></a>Özelliği Karşılaştır

Yayımlanmış bir teklifte değişiklik yaptığınızda, yaptığınız değişiklikleri denetlemek için **Karşılaştır** özelliğini kullanabilirsiniz. 

**Karşılaştır özelliğini kullanmak için:**

1.  Düzenleme işleminin herhangi bir noktasında teklifiniz için **Karşılaştır'ı** seçin.

    ![Özellik düğmesini karşılaştır](./media/iot-edge-module-compare.png)


2.  Pazarlama varlıklarının ve meta verilerin yan yana sürümlerine bakın.


## <a name="history-of-publishing-actions"></a>Yayın Eylemlerinin Tarihi

Geçmiş yayımlama etkinliğini görmek için Bulut İş Ortağı Portalı'nın sol daki gezinti menüsü çubuğundaki **Geçmiş** sekmesini seçin. Azure Marketi tekliflerinizin ömrü boyunca zaman damgalı eylemleri görebilirsiniz.  <!-- Need to find correct link here:  legal time windowsFor more information, see [History page](cpp-history-page.md) -->
