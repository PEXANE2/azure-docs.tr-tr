---
title: Azure Geçişi ile Azure'a geçiş için çok sayıda VMware VM'yi değerlendirin
description: Azure Geçiş hizmetini kullanarak Azure'a geçiş için çok sayıda VMware VM'nin nasıl değerlendirildiğini açıklar.e
ms.topic: how-to
ms.date: 03/23/2020
ms.openlocfilehash: d404583b1bad474a5e24e8c7cf060aeb80d610bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336866"
---
# <a name="assess-large-numbers-of-vmware-vms-for-migration-to-azure"></a>Azure'a geçiş için çok sayıda VMware VM'yi değerlendirin


Bu makalede, Azure Geçir Sunucu Değerlendirmesi aracını kullanarak Azure'a geçiş için şirket içi VMware VM'lerin çok sayıda (1000-35.000) nasıl değerlendirileceğimi açıklanmaktadır.

[Azure Geçiş,](migrate-services-overview.md) uygulamaları, altyapıyı ve iş yüklerini Microsoft Azure'a keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olan bir araç merkezi sağlar. Hub, Azure Geçiş araçlarını ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir. 

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
> * Ölçekte değerlendirme planı.
> * Azure izinlerini yapılandırın ve VMware'i değerlendirmeye hazırlayın.
> * Bir Azure Geçiş projesi oluşturun ve bir değerlendirme oluşturun.
> * Geçiş planı planlarken değerlendirmeyi gözden geçirin.


> [!NOTE]
> Ölçekte değerlendirmeden önce birkaç VM'yi değerlendirmek için bir kavram kanıtı denemek istiyorsanız, [öğretici serimizi](tutorial-prepare-vmware.md) takip edin

## <a name="plan-for-assessment"></a>Değerlendirme planı

Çok sayıda VMware VM'nin değerlendirilmesi için planlama yaparken, düşünmen gereken birkaç şey vardır:

- **Azure Geçiş projelerini planlayın**: Azure Geçiş projelerini nasıl dağıtılayacağından öğrenin. Örneğin, veri merkezleriniz farklı coğrafyalardaysa veya bulma, değerlendirme veya geçişle ilgili meta verileri farklı bir coğrafyada depolamanız gerekiyorsa, birden çok projeye ihtiyacınız olabilir. 
- **Plan cihazları**: Azure Geçir, VM'leri sürekli olarak keşfetmek için VMware VM olarak dağıtılan şirket içi Azure Geçiş cihazını kullanır. Cihaz, VM, disk veya ağ bağdaştırıcıları ekleme gibi ortam değişikliklerini izler. Ayrıca, bunlarla ilgili meta verileri ve performans verilerini Azure'a da gönderir. Kaç tane cihaz dağıtmanız gerektiğini bulmanız gerekiyor.
- **Keşif için hesap planı**: Azure Geçir cihazı, değerlendirme ve geçiş için VM'leri keşfetmek için vCenter Server erişimiolan bir hesap kullanır. 10.000'den fazla VM keşfediyorsanız, birden çok hesap ayarlayın.


## <a name="planning-limits"></a>Planlama sınırları
 
Planlama için bu tabloda özetlenen sınırları kullanın.

**Planlama** | **Limitler**
--- | --- 
**Azure Geçiş projeleri** | Bir projede en fazla 35.000 VM'yi değerlendirin.
**Azure Geçişi gereci** | Bir cihaz vCenter Server'da en fazla 10.000 VM keşfedebilir.<br/> Bir cihaz yalnızca tek bir vCenter Server'a bağlanabilir.<br/> Bir cihaz yalnızca tek bir Azure Geçiş projesiyle ilişkilendirilebilir.<br/>  Herhangi bir sayıda cihaz tek bir Azure Geçiş projesiyle ilişkilendirilebilir. <br/><br/> 
**Grup** | Tek bir grupta en fazla 35.000 VM ekleyebilirsiniz.
**Azure Geçir değerlendirmesi** | Tek bir değerlendirmede en fazla 35.000 VM'yi değerlendirebilirsiniz.

Bu sınırlamaları göz önünde bulundurarak, bazı örnek dağıtımlar şunlardır:


**vCenter server** | **Sunucudaki VM'ler** | **Öneri** | **Eylem**
---|---|---
Bir | < 10.000 | Bir Azure Geçiş projesi.<br/> Bir cihaz.<br/> Keşif için bir vCenter hesabı. | Cihazı kurun, bir hesapla vCenter Server'a bağlanın.
Bir | > 10.000 | Bir Azure Geçiş projesi.<br/> Birden fazla alet.<br/> Birden çok vCenter hesabı. | Her 10.000 VM için cihaz ayarlayın.<br/><br/> vCenter hesaplarını ayarlayın ve bir hesabın erişimini 10.000 VM'den az ile sınırlamak için envanteri bölün.<br/> Her cihazı bir hesapla vCenter sunucusuna bağlayın.<br/> Farklı cihazlarla keşfedilen makinelerdeki bağımlılıkları analiz edebilirsiniz.
Birden çok | < 10.000 |  Bir Azure Geçiş projesi.<br/> Birden fazla alet.<br/> Keşif için bir vCenter hesabı. | Cihazları ayarlayın, bir hesapla vCenter Server'a bağlanın.<br/> Farklı cihazlarla keşfedilen makinelerdeki bağımlılıkları analiz edebilirsiniz.
Birden çok | > 10.000 | Bir Azure Geçiş projesi.<br/> Birden fazla alet.<br/> Birden çok vCenter hesabı. | vCenter Server keşfi 10.000 VM <, her vCenter Server için bir cihaz ayarlayın.<br/><br/> vCenter Server 10.000 VM> keşfederse, her 10.000 VM için bir cihaz ayarlayın.<br/> vCenter hesaplarını ayarlayın ve bir hesabın erişimini 10.000 VM'den az ile sınırlamak için envanteri bölün.<br/> Her cihazı bir hesapla vCenter sunucusuna bağlayın.<br/> Farklı cihazlarla keşfedilen makinelerdeki bağımlılıkları analiz edebilirsiniz.


## <a name="plan-discovery-in-a-multi-tenant-environment"></a>Çok kiracılı bir ortamda keşif planı

Çok kiracılı bir ortam planlıyorsanız, keşfi vCenter Server'da kapsama alabilirsiniz.

- Cihaz bulma kapsamını vCenter Server veri merkezleri, kümeler veya kümeler klasörü, ana bilgisayarlar veya ana bilgisayarlar klasörüne veya tek tek VM'lere ayarlayabilirsiniz.
- Ortamınız kiracılar arasında paylaşılırsa ve her kiracıyı ayrı ayrı keşfetmek istiyorsanız, cihazın keşif için kullandığı vCenter hesabına erişimi kapsam dışı kullanabilirsiniz. 
    - Kiracılar ana bilgisayarları paylaşıyorsa VM klasörlerine göre kapsama almak isteyebilirsiniz. VCenter hesabı vCenter VM klasör ü düzeyinde verilen erişime sahipse Azure Geçir VM'leri keşfedemez. Keşifinizi VM klasörlerine göre kapsamda görmek istiyorsanız, bunu vCenter hesabının VM düzeyinde atanan salt okunur erişime sahip olmasını sağlayarak yapabilirsiniz. [Daha fazla bilgi edinin](set-discovery-scope.md).

## <a name="prepare-for-assessment"></a>Değerlendirmeye hazırlanın

Azure ve VMware'i sunucu değerlendirmesi için hazırlayın. 

1. [VMware destek gereksinimlerini ve sınırlamalarını](migrate-support-matrix-vmware.md)doğrulayın.
2. Azure hesabınıziçin Azure Geçiş ile etkileşim kurma izinleri ayarlayın.
3. VMware'i değerlendirmeye hazırlayın.

Bu ayarları yapılandırmak için [bu öğreticideki](tutorial-prepare-vmware.md) yönergeleri izleyin.


## <a name="create-a-project"></a>Proje oluşturma

Planlama gereksinimlerinize uygun olarak aşağıdakileri yapın:

1. Bir Azure Geçiş projeleri oluşturun.
2. Projelere Azure Geçir Sunucusu Değerlendirmesi aracını ekleyin.

[Daha fazlasını öğrenin](how-to-add-tool-first-time.md)

## <a name="create-and-review-an-assessment"></a>Bir değerlendirme oluşturma ve gözden geçirme

1. VMware VM'ler için değerlendirmeler oluşturun.
1. Göç planlamasına hazırlık değerlendirmelerini gözden geçirin.


Bu ayarları yapılandırmak için [bu öğreticideki](tutorial-assess-vmware.md) yönergeleri izleyin.
    

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede şunları yapacaksınız:
 
> [!div class="checklist"] 
> * VMware VM'ler için Azure Geçiş değerlendirmelerini ölçeklendirmesi planlanıyor
> * Değerlendirme için Hazırlanmış Azure ve VMware
> * Bir Azure Geçiş projesi oluşturdu ve değerlendirmeler igüncelleştirme
> * Göçe hazırlık değerlendirmelerini gözden geçirdi.

Şimdi, değerlendirmelerin nasıl hesaplandığı ve [değerlendirmelerin](how-to-modify-assessment.md)nasıl değiştirilebildiğini [öğrenin.](concepts-assessment-calculation.md)
