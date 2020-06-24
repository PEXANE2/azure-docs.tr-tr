---
title: Azure Lab Services Hakkında | Microsoft Docs
description: Lab Services’in geliştiriciler, test uzmanları, eğitimciler, öğrenciler ve diğerleri tarafından kullanılabilen sanal makinelerle laboratuvar oluşturma, yönetme ve güvenli hale getirmeyi nasıl kolaylaştırabildiğini öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 06/16/2020
ms.author: spelluru
ms.openlocfilehash: 1c4fb6660f8eaebe2a65e3dba3f9adbb07f1eb22
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84882714"
---
# <a name="an-introduction-to-azure-lab-services"></a>Azure Lab Services’a giriş
**Azure Lab Services** , altyapısı Azure tarafından yönetilen laboratuvarlar oluşturmanızı sağlar. Şu anda, sınıf Laboratuvarı Azure Lab Services tarafından desteklenen tek bir yönetilen laboratuvar türüdür. Hizmet, yönetilen bir laboratuvar türü için tüm altyapı yönetimini işler ve VM 'Leri, hataları işlemeye ve altyapıyı ölçeklendirmeye yönelik olarak alır. BT Yöneticisi Azure Lab Services bir laboratuvar hesabı oluşturduktan sonra, bir eğitmen, sınıf için hızlıca bir laboratuvar ayarlayabilir, sınıfta alıştırmalarda gereken VM 'lerin sayısını ve türünü belirtebilir ve sınıfa Kullanıcı eklemektir. Kullanıcı sınıfa kaydolduktan sonra, Kullanıcı, sınıf için alıştırmaları yapmak üzere VM 'ye erişebilir.  

## <a name="key-capabilities"></a>Temel işlevler
Azure Lab Services aşağıdaki temel özellikleri destekler:

- **Hızlı ve esnek bir laboratuvar kurulumu**. Laboratuvar sahipleri Azure Lab Services’i kullanarak gereksinimlerine uygun bir laboratuvarı hızlıca ayarlayabilir. Hizmet, yönetilen laboratuvar türleri için tüm Azure altyapı çalışmalarını ele alma seçeneği sunar. Hizmet, sizin yerinize yönettiği laboratuvarlar için yerleşik ölçeklendirme ve esneklik özelliği sağlar.
- **Laboratuvar kullanıcıları için basitleştirilmiş deneyim**. Laboratuvar kullanıcıları, kayıt kodu olan bir laboratuvara kaydedebilir ve laboratuvar kaynaklarını kullanmak için her zaman laboratuvara erişebilir. 
- **Maliyet iyileştirme ve analizi**. Laboratuvar sahibi, sanal makineleri otomatik olarak kapatmak ve başlatmak için laboratuvar zamanlamaları ayarlayabilir. Laboratuvar sahibi, laboratuvarın sanal makinelerini kullanıcılara erişilebilen zaman yuvalarını belirlemek için bir zamanlama ayarlayabilir ve maliyeti iyileştirmek için Kullanıcı başına veya laboratuvar başına kullanım ilkeleri ayarlayabilir. 

Yalnızca bir laboratuvarda ihtiyacınız olanları girmek ve hizmetin laboratuvar için gereken altyapıyı ayarlayıp yönetmesine izin vermek istiyorsanız, **yönetilen laboratuvar türlerinden**birini seçin. Şu anda, **sınıf laboratuvarı** Azure Lab Services oluşturabileceğiniz tek yönetilen laboratuvar türüdür.

Aşağıdaki bölümlerde bu laboratuvarlar hakkında daha ayrıntılı bilgi verilmektedir. 

## <a name="managed-lab-types"></a>Yönetilen laboratuvar türleri
Azure Lab Services, altyapısı Azure tarafından yönetilen laboratuvarlar oluşturmanızı sağlar. Bu makale, bunlara yönetilen laboratuvar türleri olarak başvurur. Yönetilen laboratuvar türleri, özel gereksinimlerinize uyan farklı türde laboratuvarlar sunar. Şu anda desteklenen tek yönetilen laboratuvar türü, **sınıf laboratuvarıdır**. 

Yönetilen laboratuvar türleri, en az kurulum ile hemen çalışmaya başlamanızı sağlar. Hizmetin kendisi, laboratuvar için altyapının tüm yönetimini işler ve VM 'Leri hataları işlemeye ve altyapıyı ölçeklendirmeye yönelik olarak çalışır.Sınıf Laboratuvarı gibi bir yönetilen laboratuvar türü oluşturmak için önce kuruluşunuz için bir laboratuar hesabı oluşturmanız gerekir. Laboratuvar hesabı, kuruluştaki tüm laboratuvarların yönetildiği merkezi hesap olarak görev yapar. 

Azure kaynaklarını oluşturduğunuz ve bu yönetilen laboratuvar türlerinde kullandığınızda hizmet, iç Microsoft aboneliklerinde kaynakları oluşturur ve yönetir. Bunlar sizin Azure aboneliğinizde oluşturulmaz. Hizmet bu kaynakların dahili Microsoft aboneliklerindeki kullanımını takip eder. Bu kullanım, laboratuvar hesabını içeren Azure aboneliğinize faturalanır.   

**Yönetilen laboratuvar türleri için kullanım örneklerinin**bazıları şunlardır: 

- Öğrencilere tam olarak bir sınıfın gereksinimleriyle yapılandırılmış sanal makinelerden oluşan bir laboratuvar sağlayın. Her öğrenciye, ev ödevi veya kişisel projeleri için VM’leri kullanabilecekleri sınırlı sayıda saat verin.
- İşlem yoğunluklu veya grafik yoğunluklu araştırmalar gerçekleştirmek üzere yüksek performanslı işlem VM’leri içeren bir havuz oluşturun. Gerektiğinde VM’leri çalıştırın ve işiniz bittikten sonra makineleri temizleyin. 
- Okulunuzun fiziksel bilgisayar laboratuvarını buluta taşıyın. VM sayısını yalnızca laboratuvarda ayarladığınız üst kullanım sınırı ve maliyet eşiğine göre otomatik olarak ölçeklendirin.  
- Bir hackathon barındırmak için hızlıca bir sanal makine laboratuvarı sağlayın. İşiniz bittiğinde tek bir tıklama ile laboratuvarı silin. 

## <a name="next-steps"></a>Sonraki adımlar
Laboratuvar hesabı oluşturmaya ve bir sınıf Laboratuvarı oluşturmaya yönelik adım adım yönergeler için aşağıdaki öğreticilere bakın.

- [Öğretici: Laboratuvar hesabı ayarlama](tutorial-setup-lab-account.md)
- [Öğretici: sınıf Laboratuvarı oluşturma](tutorial-setup-classroom-lab.md)
