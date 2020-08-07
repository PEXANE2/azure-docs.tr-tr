---
title: AWS VM 'lerini Azure geçişi ile Azure 'a geçirme
description: Bu makalede AWS örneklerini Azure 'a geçirme seçenekleri açıklanmakta ve Azure geçişi önerilmektedir.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/27/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 83d8106b9f206da9095c37b6179bdda8001b439c
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87852778"
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>Amazon Web Services (AWS) sanal makinelerini Azure’a geçirme

Bu makalede Amazon Web Services (AWS) örneklerinin Azure 'a geçirilmesi için seçenekler açıklanmaktadır.

## <a name="migrate-with-azure-migrate"></a>Azure geçişi ile geçiş

AWS EC2 örneklerini [Azure geçişi](../migrate/migrate-services-overview.md) hizmetini kullanarak Azure 'a geçirmeniz önerilir. Azure geçişi, sunucu geçişi için oluşturulmuştur. Azure geçişi, şirket içi makinelerin Azure 'a bulunması, değerlendirilmesi ve geçirilmesi için merkezi bir merkez sağlar.

AWS örneklerini Azure geçişi ile geçirmeyi [öğrenin](../migrate/tutorial-migrate-aws-virtual-machines.md) . 


## <a name="migrate-with-site-recovery"></a>Site Recovery ile geçir

Site Recovery, yalnızca olağanüstü durum kurtarma için kullanılmalıdır, geçiş değildir.

Zaten Azure Site Recovery kullanıyorsanız ve AWS geçişi için kullanmaya devam etmek istiyorsanız, [fiziksel makinelerin olağanüstü durum kurtarma](physical-azure-disaster-recovery.md)'yı ayarlamak için kullandığınız adımların aynısını izleyin.


> [!NOTE]
> Olağanüstü durum kurtarma için bir yük devretme işlemi çalıştırdığınızda, son bir adım olarak yük devretmeyi yürütmeniz gerekir. AWS örneklerini geçirdiğinizde, **COMMIT** seçeneği ilgili değildir. Bunun yerine, **geçişi Tamam** seçeneğini belirleyin. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> Azure geçişi ile ilgili [sık sorulan soruları gözden geçirin](../migrate/resources-faq.md) .
