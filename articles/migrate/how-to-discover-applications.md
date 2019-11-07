---
title: Azure geçişi sunucu değerlendirmesini kullanarak şirket içi sunucularda yüklü olan uygulamaları, rolleri ve özellikleri bulma
description: Azure geçişi sunucu değerlendirmesi kullanılarak şirket içi sunucularda uygulamalar, roller ve özelliklerin nasıl keşfedileceğini açıklar.
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: article
ms.date: 10/16/2019
ms.author: snehaa
ms.openlocfilehash: 40cb73848bab97ef9030cb0c4f3f17c984a1e0ec
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73715792"
---
# <a name="discover-machine-apps-roles-and-features"></a>Makine uygulamalarını, rolleri ve özellikleri bul 

> [!NOTE]
> Azure geçişi portalında bu özelliği henüz görmüyorsanız, ' ı kapatın. Sonraki hafta içinde görünür.

Bu makalede, Azure geçişi: Sunucu değerlendirmesi kullanılarak şirket içi sunuculardaki uygulamaların, rollerin ve özelliklerin nasıl keşfedileceğini açıklanmaktadır.

Uygulamaların envanterini bulma ve şirket içi makinelerinizde çalışan roller/özellikler, iş yükleriniz için uyarlanmış bir Azure 'a geçiş yolu tanımlamanızı ve planlamanız sağlar. 

Azure geçişi kullanarak uygulama bulma: Sunucu değerlendirmesi aracısız. Makinelere ve VM 'Lere hiçbir şeyin yüklü olması gerekir. Sunucu değerlendirmesi, makine konuk kimlik bilgileriyle birlikte bulma işlemi gerçekleştirmek için Azure geçişi gereci kullanır. Gereç, WMI ve SSH çağrılarını kullanarak makinelere uzaktan erişir. 

> [!NOTE]
> Uygulama bulma Şu anda yalnızca VMware VM 'Leri için desteklenmektedir ve yalnızca bulma ile sınırlıdır. Henüz uygulama tabanlı değerlendirme sunmuyoruz.  Şimdi sunucu değerlendirmesi Şu anda şirket içi VMware VM 'leri, Hyper-V VM 'Leri ve makine düzeyindeki fiziksel sunucuları, yükseltme ve kaydırma geçişleri için değerlendirir.


## <a name="before-you-start"></a>Başlamadan önce

1. Uygulama düzeyinde bulma için [destek sınırlamalarını](migrate-support-matrix-vmware.md#application-discovery) gözden geçirin.
2. Bir Azure geçişi projesi [oluşturduğunuzdan](how-to-add-tool-first-time.md) emin olun.
3. Zaten bir proje oluşturduysanız Azure geçişi: Sunucu değerlendirmesi [aracını eklediğinizden emin](how-to-assess.md) olun.
4. Azure geçişi gereci ile VMware VM 'lerini bulmak ve değerlendirmek için [VMware gereksinimlerini](migrate-support-matrix-vmware.md#assessment-vcenter-server-requirements) denetleyin.
4. Azure geçişi gerecini dağıtmaya yönelik [gereksinimleri](migrate-support-matrix-vmware.md#assessment-appliance-requirements) kontrol edin.

## <a name="prepare-for-app-discovery"></a>Uygulama bulma için hazırlanma

1. [Gereç dağıtımına hazırlanın](https://docs.microsoft.com/azure/migrate/tutorial-prepare-vmware). Hazırlık, Gereç ayarlarını doğrulamayı ve gerecin vCenter Server erişmek için kullanacağı bir hesap ayarlamayı içerir.
2. Uygulamaları, rolleri ve özellikleri öğrenmek istediğiniz makineler için yönetici izinlerine sahip bir kullanıcı hesabına (Windows ve Linux sunucuları için birer tane) sahip olduğunuzdan emin olun.
3. Bulmayı başlatmak için VMware gerecini [dağıtın](how-to-set-up-appliance-vmware.md) . Gereci dağıtmak için bir OVA şablonunu indirip bir VMware VM 'si olarak oluşturmak üzere VMware 'ye içeri aktarabilirsiniz. Gereci yapılandırıp Azure geçişi ile kaydedersiniz.
2. Gereci dağıtırken, sürekli bulmayı başlatmak için aşağıdakileri belirtirsiniz:
    - Bağlanmak istediğiniz vCenter Server adı.
    - VCenter Server Bağlanılacak gereç için oluşturduğunuz kimlik bilgileri.
    - Windows/Linux VM 'lerine bağlanmak için gereç için oluşturduğunuz hesap kimlik bilgileri.

Gereç dağıtıldıktan ve kimlik bilgilerini sağladıysanız, Gereç VM meta verilerinin ve performans verilerinin sürekli olarak keşfedilmesi ve uygulamaların, özelliklerin ve rollerin keşfedilmesi ile birlikte başlatılır.  Uygulama bulmanın süresi, sahip olduğunuz VM sayısına bağlıdır. Genellikle uygulama keşfi 500 VM 'Ler için bir saat sürer.

## <a name="review-and-export-the-inventory"></a>Envanteri gözden geçirin ve dışa aktarın

Bulma tamamlandıktan sonra, uygulama bulma için kimlik bilgileri sağladıysanız, Azure portal uygulama envanterini gözden geçirebilir ve dışarı aktarabilirsiniz. 

1. Azure **geçişi-sunucuları** > **Azure geçişi: Sunucu değerlendirmesi**' nde, **bulunan sunucular** sayfasını açmak için, görünen sayıya tıklayın.

    > [!NOTE]
    > Bu aşamada, değerlendirmek istediğiniz makineler arasında bağımlılıkları görselleştirmeniz için isteğe bağlı olarak bulunan makineler için bağımlılık eşlemesini da ayarlayabilirsiniz. [Daha fazla bilgi edinin](how-to-create-group-machine-dependencies.md).

2. **Bulunan uygulamalarda**, görünen sayıya tıklayın.
3. **Uygulama envanteri**' nde, bulunan uygulamaları, rolleri ve özellikleri inceleyebilirsiniz.
4. Envanteri dışarı aktarmak için **bulunan sunucularda**, **uygulama envanterini dışarı aktar**' a tıklayın.

Uygulama envanteri, Excel biçiminde verilir ve indirilir. **Uygulama envanteri** sayfasında tüm makinelerde bulunan tüm uygulamalar görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

- Bulunan sunucuların geçiş ve kaydırma için [bir değerlendirme oluşturun](how-to-create-assessment.md) .
- [Azure geçişi: veritabanı değerlendirmesi](https://docs.microsoft.com/sql/dma/dma-assess-sql-data-estate-to-sqldb?view=sql-server-2017)kullanarak SQL Server veritabanlarını değerlendirin.
