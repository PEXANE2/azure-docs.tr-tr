---
title: Azure Geçişi ile Azure'a geçiş için çok sayıda fiziksel sunucuyu değerlendirin | Microsoft Dokümanlar
description: Azure Geçiş hizmetini kullanarak Azure'a geçiş için çok sayıda fiziksel sunucunun nasıl değerlendirildiğini açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 01/19/2020
ms.author: hamusa
ms.openlocfilehash: a19a1b6e7416667079ab07fc5440ee8828c26bf4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76294376"
---
# <a name="assess-large-numbers-of-physical-servers-for-migration-to-azure"></a>Azure'a geçiş için çok sayıda fiziksel sunucuyu değerlendirin

Bu makalede, Azure Geçir Sunucusu Değerlendirmesi aracını kullanarak Azure'a geçiş için çok sayıda şirket içi fiziksel sunucunun nasıl değerlendirildiği açıklanmaktadır.

[Azure Geçiş,](migrate-services-overview.md) uygulamaları, altyapıyı ve iş yüklerini Microsoft Azure'a keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olan bir araç merkezi sağlar. Hub, Azure Geçiş araçlarını ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir. 


Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
> * Ölçekte değerlendirme planı.
> * Azure izinlerini yapılandırın ve fiziksel sunucuları değerlendirmeye hazırlayın.
> * Bir Azure Geçiş projesi oluşturun ve bir değerlendirme oluşturun.
> * Geçiş planı planlarken değerlendirmeyi gözden geçirin.


> [!NOTE]
> Eğer ölçekte değerlendirmeden önce sunucuların bir çift değerlendirmek için bir kanıtı-of-concept denemek istiyorsanız, bizim [öğretici serisi](tutorial-prepare-physical.md)izleyin.

## <a name="plan-for-assessment"></a>Değerlendirme planı

Çok sayıda fiziksel sunucunun değerlendirilmesi için planlama yaparken, üzerinde düşünülecek birkaç şey vardır:

- **Azure Geçiş projelerini planlayın**: Azure Geçiş projelerini nasıl dağıtılayacağından öğrenin. Örneğin, veri merkezleriniz farklı coğrafyalardaysa veya bulma, değerlendirme veya geçişle ilgili meta verileri farklı bir coğrafyada depolamanız gerekiyorsa, birden çok projeye ihtiyacınız olabilir.
- **Plan cihazları**: Azure Geçir, değerlendirme ve geçiş için sunucuları sürekli olarak keşfetmek için windows makinesinde dağıtılan şirket içi Azure Geçir cihazını kullanır. Cihaz, VM, disk veya ağ bağdaştırıcıları ekleme gibi ortam değişikliklerini izler. Ayrıca, bunlarla ilgili meta verileri ve performans verilerini Azure'a da gönderir. Kaç tane aleti dağıtabileceğinizi bulmanız gerekiyor.


## <a name="planning-limits"></a>Planlama sınırları
 
Planlama için bu tabloda özetlenen sınırları kullanın.

**Planlama** | **Limitler**
--- | --- 
**Azure Geçiş projeleri** | Bir projede en fazla 35.000 sunucuyu değerlendirin.
**Azure Geçişi gereci** | Bir cihaz en fazla 250 sunucu keşfedebilir.<br/> Bir cihaz yalnızca tek bir Azure Geçiş projesiyle ilişkilendirilebilir.<br/> Herhangi bir sayıda cihaz tek bir Azure Geçiş projesiyle ilişkilendirilebilir. <br/><br/> 
**Grup** | Tek bir grupta en fazla 35.000 sunucu ekleyebilirsiniz.
**Azure Geçir değerlendirmesi** | Tek bir değerlendirmede en fazla 35.000 sunucuyu değerlendirebilirsiniz.


## <a name="other-planning-considerations"></a>Diğer planlama konuları

- Cihazdan keşif başlatmak için, her fiziksel sunucu seçmeniz gerekir. 

## <a name="prepare-for-assessment"></a>Değerlendirmeye hazırlanın

Sunucu değerlendirmesi için Azure ve fiziksel sunucular hazırlayın. 

1. [Fiziksel sunucu destek gereksinimlerini ve sınırlamalarını](migrate-support-matrix-physical.md)doğrulayın.
2. Azure hesabınıziçin Azure Geçiş ile etkileşim kurma izinleri ayarlayın.
3. Fiziksel sunucuları hazırlayın.

Bu ayarları yapılandırmak için [bu öğreticideki](tutorial-prepare-physical.md) yönergeleri izleyin.

## <a name="create-a-project"></a>Proje oluşturma

Planlama gereksinimlerinize uygun olarak aşağıdakileri yapın:

1. Bir Azure Geçişi projesi oluşturun.
2. Projelere Azure Geçir Sunucusu Değerlendirmesi aracını ekleyin.

[Daha fazlasını öğrenin](how-to-add-tool-first-time.md)

## <a name="create-and-review-an-assessment"></a>Bir değerlendirme oluşturma ve gözden geçirme

1. Fiziksel sunucular için değerlendirmeler oluşturun.
1. Göç planlamasına hazırlık değerlendirmelerini gözden geçirin.

Değerlendirme oluşturma ve gözden geçirme hakkında [daha fazla bilgi edinin.](tutorial-assess-physical.md)
    

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede şunları yapacaksınız:
 
> [!div class="checklist"] 
> * Fiziksel sunucular için Azure Geçir değerlendirmelerini ölçeklendirmesi planlandı.
> * Değerlendirme için Azure ve fiziksel sunucular hazırladı.
> * Bir Azure Geçiş projesi oluşturdu ve değerlendirmeler çalıştırdın.
> * Göçe hazırlık değerlendirmelerini gözden geçirdi.

Şimdi, değerlendirmelerin nasıl hesaplandığı ve [değerlendirmelerin](how-to-modify-assessment.md)nasıl değiştirilebildiğini [öğrenin.](concepts-assessment-calculation.md)
