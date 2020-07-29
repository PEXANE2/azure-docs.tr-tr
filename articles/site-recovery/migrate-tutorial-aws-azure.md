---
title: Azure Site Recovery hizmetiyle AWS VM'lerini Azure'a geçirme | Microsoft Docs
description: Bu makalede, Azure Site Recovery kullanılarak, Amazon Web Services’te (AWS) çalıştırılan Windows sanal makinelerinin Azure’a nasıl geçirileceği açıklanmaktadır.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/27/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: dd91e99b45405cca10b9ddc2982674e72ad6bf86
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87281302"
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>Amazon Web Services (AWS) sanal makinelerini Azure’a geçirme

Bu makalede Amazon Web Services (AWS) örneklerinin Azure 'a geçirilmesi için seçenekler açıklanmaktadır.

## <a name="migrate-with-azure-migrate"></a>Azure geçişi ile geçiş

[Azure geçişi](../migrate/migrate-services-overview.md) hizmetini kullanarak AWS örneklerini Azure 'a geçirmeniz önerilir. Azure geçişi, Azure geçişi, diğer Azure hizmetleri ve üçüncü taraf araçları kullanarak şirket içi makinelerin Azure 'a değerlendirilmesi ve geçirilmesi için merkezi bir merkez sağlar.

AWS örneklerini Azure geçişi ile geçirmeyi [öğrenin](../migrate/tutorial-migrate-aws-virtual-machines.md) . 


## <a name="migrate-with-site-recovery"></a>Site Recovery ile geçir

Site Recovery, yalnızca olağanüstü durum kurtarma için kullanılmalıdır, geçiş değildir.

Zaten Azure Site Recovery kullanıyorsanız ve AWS geçişi için kullanmaya devam etmek istiyorsanız, [fiziksel makinelerin olağanüstü durum kurtarma](physical-azure-disaster-recovery.md)'yı ayarlamak için kullandığınız adımların aynısını izleyin.


> [!NOTE]
> Olağanüstü durum kurtarma için bir yük devretme işlemi çalıştırdığınızda, son bir adım olarak yük devretmeyi yürütmeniz gerekir. AWS örneklerini geçirdiğinizde, **COMMIT** seçeneği ilgili değildir. Bunun yerine, **geçişi Tamam** seçeneğini belirleyin. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> Azure geçişi ile ilgili [sık sorulan soruları gözden geçirin](../migrate/resources-faq.md) .
