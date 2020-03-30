---
title: Azure Geçiş ile şirket içi sunucularda uygulamaları, rolleri ve özellikleri keşfedin
description: Azure Geçiş Sunucusu Değerlendirmesi ile şirket içi sunucularda uygulamaları, rolleri ve özellikleri nasıl keşfederiz öğrenin.
ms.topic: article
ms.date: 03/12/2020
ms.openlocfilehash: e8ce279afc845ebf37ad4ab8b2ce7236cb18137a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79453591"
---
# <a name="discover-machine-apps-roles-and-features"></a>Makine uygulamalarını, rollerini ve özelliklerini keşfedin

Bu makalede, Azure Geçiş: Sunucu Değerlendirmesi'ni kullanarak şirket içi sunucularda uygulamaların, rollerin ve özelliklerin nasıl keşfedilebildiğini açıklanmaktadır.

Şirket içi makinelerinizde çalışan uygulamaların ve rollerin/özelliklerin envanterini keşfetmek, iş yüklerinize göre uyarlanmış bir Azure geçiş yolunu belirlemenize ve planlamanıza yardımcı olur.

> [!NOTE]
> Uygulama bulma şu anda yalnızca VMware VM'ler için önizlemededir ve yalnızca keşifle sınırlıdır. Henüz uygulama tabanlı değerlendirme sunmuyoruz. Şirket içi VMware VM'ler, Hyper-V VM'ler ve fiziksel sunucular için makine tabanlı değerlendirme.

Azure Geçiş: Sunucu Değerlendirmesi aracısız kullanarak uygulama bulma. Makinelere ve VM'lere hiçbir şey yüklenmez. Sunucu Değerlendirmesi, makine konuk kimlik bilgileriyle birlikte keşif gerçekleştirmek için Azure Geçir cihazını kullanır. Cihaz VMware API'lerini kullanarak VMware makinelerine uzaktan eriştir.


## <a name="before-you-start"></a>Başlamadan önce

1. Bir Azure Geçiş projesi [oluşturduğunuzdan](how-to-add-tool-first-time.md) emin olun.
2. Projeye Azure Geçiş: Sunucu Değerlendirmesi aracını [eklediğinizden](how-to-assess.md) emin olun.
4. Azure Geçiş cihazıyla VMware VM'leri keşfetmek ve değerlendirmek için [VMware gereksinimlerini](migrate-support-matrix-vmware.md#vmware-requirements) kontrol edin.
5. Azure Geçiş cihazını dağıtma [gereksinimlerini](migrate-appliance.md) kontrol edin.
6. Uygulama bulma için [desteği ve gereksinimleri doğrulayın.](migrate-support-matrix-vmware.md#application-discovery)

## <a name="prepare-for-app-discovery"></a>Uygulama keşfine hazırlanın

1. [Cihaz dağıtımına hazırlanın.](tutorial-prepare-vmware.md) Hazırlık, cihaz ayarlarını doğrulamayı ve cihazın vCenter Server'a erişmek için kullanacağı bir hesap oluşturmayı içerir.
2. Uygulamaları, rolleri ve özellikleri keşfetmek istediğiniz makineler için yönetici izinlerine sahip bir kullanıcı hesabınız olduğundan (Windows ve Linux sunucuları için birer tane) emin olun.
3. Keşfetmeye başlamak için [Azure Geçir cihazını dağıtın.](how-to-set-up-appliance-vmware.md) Cihazı dağıtmak için, cihazı VMware VM olarak oluşturmak için VMware'e bir OVA şablonu indirip içe aktarabilirsiniz. Cihazı yapılandırır ve Azure Geçiş'e kaydedersiniz.
2. Cihazı dağıtırken, sürekli keşif başlatmak için aşağıdakileri belirtin:
    - Bağlanmak istediğiniz vCenter Server'ın adı.
    - Cihazın vCenter Server'a bağlanması için oluşturduğunuz kimlik bilgileri.
    - Cihazın Windows/Linux VM'lerine bağlanması için oluşturduğunuz hesap kimlik bilgileri.

Cihaz dağıtıldıktan ve kimlik bilgileri sağladıktan sonra, cihaz, uygulamaların, özelliklerin ve rollerin yanı sıra VM meta verilerinin ve performans verilerinin sürekli olarak keşfedilmesine başlar.  Uygulama bulma süresi kaç VM'ine sahip olduğunuza bağlıdır. Genellikle 500 VM'nin app-discovery için bir saat sürer.

## <a name="review-and-export-the-inventory"></a>Envanteri gözden geçirme ve dışa aktarma

Keşif sona erdikten sonra, uygulama bulma için kimlik bilgileri sağladıysanız, uygulama envanterini Azure portalında inceleyebilir ve dışa aktarabilirsiniz.

1. **Azure Geçir - Sunucular** > Azure**Geçir: Sunucu Değerlendirmesi,** **Keşfedilen sunucular** sayfasını açmak için görüntülenen sayıyı tıklatın.

    > [!NOTE]
    > Bu aşamada, değerlendirmek istediğiniz makineler deki bağımlılıkları görselleştirebilmeniz için, keşfedilen makineler için bağımlılık eşlemi de isteğe bağlı olarak ayarlayabilirsiniz. [Daha fazla bilgi edinin](how-to-create-group-machine-dependencies.md).

2. **Keşfedilen Uygulamalar'da**görüntülenen sayıyı tıklatın.
3. **Uygulama**envanterinde, keşfedilen uygulamaları, rolleri ve özellikleri inceleyebilirsiniz.
4. Envanteri dışa aktarmak **için, Keşfedilen Sunucular'da,** **uygulama envanterini dışa aktar'ı**tıklatın.

Uygulama envanteri dışa aktarılır ve Excel formatında indirilir. **Uygulama Envanteri** sayfası, tüm makinelerde keşfedilen tüm uygulamaları görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

- Keşfedilen sunucuların kaldırma ve kaydırma geçişi için [bir değerlendirme oluşturun.](how-to-create-assessment.md)
- [Azure Geçiş: Veritabanı Değerlendirmesi'ni](https://docs.microsoft.com/sql/dma/dma-assess-sql-data-estate-to-sqldb?view=sql-server-2017)kullanarak SQL Server veritabanlarını değerlendirin.
