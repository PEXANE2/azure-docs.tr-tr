---
title: Azure Geçişi ile Azure'a geçiş için çok sayıda Hyper-V VM'yi değerlendirin | Microsoft Dokümanlar
description: Azure Geçiş hizmetini kullanarak Azure'a geçiş için çok sayıda Hyper-V VM'nin nasıl değerlendirildiğini açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: raynew
ms.openlocfilehash: c1ae3a9ed8a775161aaf85ab2c91b1e43113d2e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70279448"
---
# <a name="assess-large-numbers-of-hyper-v-vms-for-migration-to-azure"></a>Azure'a geçiş için çok sayıda Hyper-V VM'yi değerlendirin

Bu makalede, Azure Geçir Sunucusu Değerlendirmesi aracını kullanarak Azure'a geçiş için çok sayıda şirket içi Hyper-V V VM'nin nasıl değerlendirildiği açıklanmaktadır.

[Azure Geçiş,](migrate-services-overview.md) uygulamaları, altyapıyı ve iş yüklerini Microsoft Azure'a keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olan bir araç merkezi sağlar. Hub, Azure Geçiş araçlarını ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir. 


Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
> * Ölçekte değerlendirme planı.
> * Azure izinlerini yapılandırın ve Hyper-V'yi değerlendirmeye hazırlayın.
> * Bir Azure Geçiş projesi oluşturun ve bir değerlendirme oluşturun.
> * Geçiş planı planlarken değerlendirmeyi gözden geçirin.


> [!NOTE]
> Ölçekte değerlendirmeden önce birkaç VM'yi değerlendirmek için bir kavram kanıtı denemek istiyorsanız, [öğretici serimizi](tutorial-prepare-hyper-v.md) takip edin

## <a name="plan-for-assessment"></a>Değerlendirme planı

Çok sayıda Hyper-V VM'nin değerlendirilmesi için planlama yaparken, üzerinde düşünülecek birkaç şey vardır:

- **Azure Geçiş projelerini planlayın**: Azure Geçiş projelerini nasıl dağıtılayacağından öğrenin. Örneğin, veri merkezleriniz farklı coğrafyalardaysa veya bulma, değerlendirme veya geçişle ilgili meta verileri farklı bir coğrafyada depolamanız gerekiyorsa, birden çok projeye ihtiyacınız olabilir.
- **Plan cihazları**: Azure Geçir, değerlendirme ve geçiş için VM'leri sürekli olarak keşfetmek için Hyper-V VM olarak dağıtılan şirket içi Azure Geçiş cihazını kullanır. Cihaz, VM, disk veya ağ bağdaştırıcıları ekleme gibi ortam değişikliklerini izler. Ayrıca, bunlarla ilgili meta verileri ve performans verilerini Azure'a da gönderir. Kaç tane aleti dağıtabileceğinizi bulmanız gerekiyor.


## <a name="planning-limits"></a>Planlama sınırları
 
Planlama için bu tabloda özetlenen sınırları kullanın.

**Planlama** | **Limitler**
--- | --- 
**Azure Geçiş projeleri** | Bir projede en fazla 35.000 VM'yi değerlendirin.
**Azure Geçişi gereci** | Bir cihaz 5000 VM'ye kadar keşfedebilir.<br/> Bir cihaz en fazla 300 Hyper-V ana bilgisayara bağlanabilir.<br/> Bir cihaz yalnızca tek bir Azure Geçiş projesiyle ilişkilendirilebilir.<br/> Herhangi bir sayıda cihaz tek bir Azure Geçiş projesiyle ilişkilendirilebilir. <br/><br/> 
**Grup** | Tek bir grupta en fazla 35.000 VM ekleyebilirsiniz.
**Azure Geçir değerlendirmesi** | Tek bir değerlendirmede en fazla 35.000 VM'yi değerlendirebilirsiniz.



## <a name="other-planning-considerations"></a>Diğer planlama konuları

- Cihazdan keşfe başlamak için her Hyper-V ana bilgisayarını seçmeniz gerekir. 
- Çok kiracılı bir ortam çalıştırıyorsanız, şu anda yalnızca belirli bir kiracıya ait VM'leri keşfedemezsiniz. 

## <a name="prepare-for-assessment"></a>Değerlendirmeye hazırlanın

Sunucu değerlendirmesi için Azure ve Hyper-V'yi hazırlayın. 

1. [Hyper-V destek gereksinimlerini ve sınırlamalarını](migrate-support-matrix-hyper-v.md)doğrulayın.
2. Azure hesabınızın Azure Geçiş ile etkileşimkurması için izinler ayarlama
3. Hyper-V ana bilgisayarları ve VM'leri hazırlayın

Bu ayarları yapılandırmak için [bu öğreticideki](tutorial-prepare-hyper-v.md) yönergeleri izleyin.

## <a name="create-a-project"></a>Proje oluşturma

Planlama gereksinimlerinize uygun olarak aşağıdakileri yapın:

1. Bir Azure Geçiş projeleri oluşturun.
2. Projelere Azure Geçir Sunucusu Değerlendirmesi aracını ekleyin.

[Daha fazlasını öğrenin](how-to-add-tool-first-time.md)

## <a name="create-and-review-an-assessment"></a>Bir değerlendirme oluşturma ve gözden geçirme

1. Hyper-V VM'ler için değerlendirmeler oluşturun.
1. Göç planlamasına hazırlık değerlendirmelerini gözden geçirin.

Değerlendirme oluşturma ve gözden geçirme hakkında [daha fazla bilgi edinin.](tutorial-assess-hyper-v.md)
    

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede şunları yapacaksınız:
 
> [!div class="checklist"] 
> * Hyper-V V VM'ler için Azure Geçiş değerlendirmelerini ölçeklendirmesi planlanıyor
> * Değerlendirme için Hazırlanmış Azure ve Hyper-V
> * Bir Azure Geçiş projesi oluşturdu ve değerlendirmeler igüncelleştirme
> * Göçe hazırlık değerlendirmelerini gözden geçirdi.

Şimdi, değerlendirmelerin nasıl hesaplandığı ve [değerlendirmelerin](how-to-modify-assessment.md) nasıl değiştirilebildiğini [öğrenin](concepts-assessment-calculation.md)
