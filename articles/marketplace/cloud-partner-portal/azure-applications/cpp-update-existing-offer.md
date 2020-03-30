---
title: Varolan bir Azure uygulama teklifini güncelleştirme | Azure Marketi
description: Azure Marketi'ndeki mevcut bir Azure uygulama teklifini güncelleştirme.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: dsindona
ms.openlocfilehash: 152fd24fbc5d2762d381ffce2a937bc448858b0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275960"
---
# <a name="update-an-existing-azure-application-offer"></a>Varolan bir Azure uygulama teklifini güncelleştirme

Yayımlandıktan ve yayında yayınlandıktan sonra teklifinize yapmak isteyebileceğin çeşitli güncelleştirmeler vardır. Teklifinizin yeni sürümünde yaptığınız herhangi bir değişiklik, pazar yerinde yansıtılması için kaydedilmeli ve yeniden yayımlanmalıdır. Bu makalede, [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)yönetilen uygulama teklifinizi güncelleştirmenin farklı yönleri ile adım atılmıştır.

Teklifinizi güncelleştirmek istemenizin birkaç nedeni vardır:

- Mevcut STU'lara yeni bir resim sürümü ekleme.
- Yeni STU'lar ekleme.
- Teklif veya tek tek SNU'lar için pazar meta verilerini güncelleme.

Bu değişikliklerde size yardımcı olmak için, portal **Karşılaştırma** ve **Geçmiş** özelliklerini sağlar.

## <a name="unpermitted-changes-to-an-azure-application-offer-or-sku"></a>Azure uygulama teklifinde veya SKU'da izin verilmemiş değişiklikler

Azure Marketi'nde canlı olarak kullanılabilir olduktan sonra değiştirilemeyecek bir kapsayıcı teklifinin veya SKU'nun öznitelikleri vardır. Aşağıdaki ayarları değiştiremezsiniz:

- Teklifin kimliği ve Yayıncı Kimliği
- Mevcut SKU'ların SKU Kimliği
- Sürüm etiketleri, örneğin:`1.0.1`
- Mevcut SBI'lerde faturalama/lisans modeli değişiklikleri

## <a name="common-update-operations"></a>Sık güncelleştirme işlemleri

Aşağıdaki güncelleştirme işlemleri yaygındır.

### <a name="update-image-version-for-a-sku"></a>SKU için görüntü sürümünü güncelleştir

Bir görüntünün güvenlik yamaları, ek özellikler ve benzeri yollarla düzenli olarak güncellenmesi yaygındır. Bu senaryoda, Aşağıdaki adımları kullanarak SKU'nuzun başvurur olduğu görüntüyü güncelleştirmek istiyorsunuz:

1. [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.
2. **Tüm teklifler**altında, güncellemek istediğiniz teklifi bulun.
3. **SKU** sekmesinde, görüntüyle ilişkili SKU'yu seçin.
4. Yeni bir resim eklemek için **+ Yeni Resim Sürümü'nü** seçin.
5. Yeni resim sürümlerini sağlayın. Resim sürümünün önceki sürümler ile aynı etiketler yönergelerine uyması gerekir. Sürüm etiketleri X, Y ve Z'nin tümseger olduğu X.Y.Z formunda olmalıdır. Sağladığınız yeni sürümün önceki tüm sürümlerden daha büyük olduğunu doğrulayın.
6. Yeni kapsayıcı resim sürümünüzü Azure Marketi'nde yayınlamak için iş akışını başlatmak için **Yayımla'yı** seçin.

### <a name="add-a-new-sku"></a>Yeni bir SKU ekle

Yeni bir SKU'yu teklifiniz için kullanılabilir hale getirmek için aşağıdaki adımları kullanın:

1. [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.
2. **Tüm teklifler**altında, güncellemek istediğiniz teklifi bulun.
3. **SKU** sekmesinin altında **yeni SKU ekle'yi** seçin ve açılır pencerede bir **SKU kimliği** sağlayın.
4. [Azure'u Yayımla uygulama teklifinde](./cpp-publish-offer.md)açıklanan adımları kullanarak teklifi yeniden yayımlayın.
5. Yeni SKU'nuzu yayınlamak için iş akışını başlatmak için **Yayımla'yı** seçin.

### <a name="update-offer-marketplace-metadata"></a>Teklif pazar meta verilerini güncelleştirin

Teklifinizle ilişkili pazar meta verilerini güncelleştirmek için aşağıdaki adımları kullanın. (Örneğin: şirket adı, logolar, vb.)

1. [Bulut İş Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.
2. **Tüm teklifleraltında,** güncellemek istediğiniz teklifi bulun.
3. **Market** sekmesine gidin. Meta veri değişiklikleri yapmak için [Azure uygulama teklifini Yayımla'daki](./cpp-publish-offer.md) yönergeleri kullanın.
4. Değişikliklerinizi yayınlamak için iş akışını başlatmak için **Yayımla'yı** seçin.
 
>[!Note]
>Bulut Çözüm Sağlayıcıları (CSP) iş ortağı kanal tercihi artık kullanılabilir.  Microsoft CSP iş ortağı kanalları aracılığıyla teklifinizi pazarlama hakkında daha fazla bilgi için lütfen [Bulut Çözüm Sağlayıcıları'na](../../cloud-solution-providers.md) bakın.

## <a name="deleting-an-existing-offer"></a>Varolan bir teklifi silme

Teklifinizi Market'ten kaldırmaya karar verebilirsiniz. Bir teklifin silmesi, teklifin geçerli satın alımlarını etkilemez. Bu müşteri alımları eskisi gibi çalışmaya devam edecektir. Ancak, teklif silme işlemi tamamlandıktan sonra yeni satın almaişlemleri için kullanılamaz.

Teklif Sonlandırma, siz ve mevcut müşterileriniz arasındaki hizmet ve/veya lisans sözleşmesini sona erdirme işlemidir.
Teklif kaldırma ve sonlandırmayla ilgili kılavuz ve ilkeler Microsoft Marketplace Publisher Agreement (bkz. Bölüm 7) ve Katılım İlkeleri 'ne tabidir (bkz. Bölüm 6.2).

Daha fazla bilgi için Azure [Marketi'nden Sil ve Teklif/SKU'ya](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-managed-app-offer-delete)bakın.

## <a name="compare-feature"></a>Özelliği karşılaştırın

Yayımlanmış bir teklifte değişiklik yaptığınızda, yaptığınız değişiklikleri denetlemek için Karşılaştır özelliğini kullanabilirsiniz.

Karşılaştır özelliğini kullanmak için:

1. Düzenleme işleminin herhangi bir noktasında teklifiniz için Karşılaştır'ı seçin.
2. Pazarlama varlıklarının ve meta verilerin yan yana sürümlerine bakın.

## <a name="history-of-publishing-actions"></a>Yayımlama eylemlerinin tarihi

Geçmiş yayımlama etkinliğini görmek için Bulut İş Ortağı Portalı'nın sol daki gezinti menüsü çubuğundaki **Geçmiş** sekmesini seçin. Azure Marketi tekliflerinizin ömrü boyunca zaman damgalı eylemleri görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure uygulama teklifi](./cpp-azure-app-offer.md)
