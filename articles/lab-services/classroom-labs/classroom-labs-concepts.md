---
title: Derslik Labs kavramları-Azure Lab Services | Microsoft Docs
description: Laboratuvar hizmetlerinin temel kavramlarını ve laboratuvarların oluşturulmasını ve yönetilmesini nasıl kolaylaştırabileceğinizi öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2020
ms.author: spelluru
ms.openlocfilehash: a34af2ef779a41c19d4bfb831360948af1a66724
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83115056"
---
# <a name="classroom-labs-concepts"></a>Sınıf Laboratuvarları kavramları

Aşağıdaki liste, Temel Laboratuvar Hizmetleri kavramlarını ve tanımlarını içerir:

## <a name="quota"></a>Kota

Kota, bir öğrencinin bir laboratuvar VM 'si kullanabilmesi için bir öğretmen tarafından ayarlanabildiğinden zaman sınırı (saat cinsinden). 0 veya belirli bir saat kadar ayarlanabilir. Kota 0 olarak ayarlandıysa, bir öğrenci yalnızca bir zamanlama çalışırken veya bir öğretmen, öğrenci için sanal makineyi el ile açtığında sanal makineyi kullanabilir.  

Öğrenci laboratuvar VM 'sini başlattığında kota saatleri sayılır.  Bir öğretmen, bir öğrenci için laboratuvar VM 'sini el ile başlattığında, bu öğrenci için kota saatleri kullanılmaz.

## <a name="schedules"></a>Zamanlamalar

Zamanlamalar, bir öğretmenin sınıf için oluşturabileceğiniz zaman yuvaları, bu nedenle öğrenci VM 'lerinin sınıf zamanı için kullanılabilir olmasını sağlayabilir.  Zamanlamalar bir kerelik veya yinelenen olabilir.  Bir zamanlama çalışırken kota saatleri kullanılmaz.

Üç tür zamanlama vardır: Standart, yalnızca Başlat ve yalnızca durdur.

- **Standart**.  Bu zamanlama, belirtilen başlangıç zamanında tüm öğrenci sanal makinelerini başlatacak ve belirtilen durdurma tarihinde tüm öğrenci sanal makinelerini kapatacaktır.
- **Yalnızca Başlat**.   Bu zamanlama, tüm öğrenci sanal makinelerini belirtilen zamanda başlatacak.  Öğrenci VM 'Leri Azure Lab Services portalından veya yalnızca durdur zamanlaması oluştuktan sonra durduruluncaya kadar öğrenci VM 'Leri durdurulmaz.
- **Yalnızca durdur**.  Bu zamanlama, belirtilen zamanda tüm öğrenci sanal makinelerini durdurur.  

## <a name="template-virtual-machine"></a>Şablon sanal makinesi

Laboratuvardaki bir şablon sanal makinesi, tüm kullanıcıların sanal makinelerinin oluşturulduğu temel bir sanal makine görüntüsüdür. Gelişme/laboratuvar oluşturucuları, şablon sanal makinesini ayarlayıp BT laboratuvarlarına eğitim katılımcılarına sağlamak istedikleri yazılımla yapılandırır. Bir şablon VM yayımladığınızda Azure Lab Services, şablon VM 'yi temel alarak laboratuvar VM 'Leri oluşturur veya güncelleştirir.

## <a name="user-profiles"></a>Kullanıcı profilleri

Bu makalede Azure Lab Services içindeki farklı kullanıcı profilleri açıklanmaktadır.

### <a name="lab-account-owner"></a>Laboratuvar hesap sahibi

Genellikle, kuruluşun Azure aboneliğinin sahibi olan bulut kaynakları için BT Yöneticisi, bir laboratuar hesabı sahibi görevi görür ve aşağıdaki görevleri yapar:

- Kuruluşunuz için bir laboratuvar hesabı ayarlar.
- Tüm laboratuvarlardaki ilkeleri yönetir ve yapılandırır.
- Kuruluştaki kişilere laboratuvar hesabı altında laboratuvar oluşturma izinleri verir.

### <a name="professor"></a>Öğretim

Normalde, öğretmen veya çevrimiçi eğitimci gibi kullanıcılar sınıf laboratuvarlarını bir laboratuvar hesabı altında oluşturur. Eğitimci aşağıdaki görevleri yerine getirir:

- Sınıf laboratuvarı oluşturur.
- Laboratuvarda sanal makineler oluşturur.
- Sanal makinelere uygun yazılımları yükler.
- Laboratuvara kimlerin erişebileceğini belirtir.
- Öğrencilere laboratuvarın kayıt bağlantısını sağlar.

### <a name="student"></a>Öğrenci

Öğrenci aşağıdaki görevleri yerine getirir:

- Laboratuvar kullanıcısının laboratuvara kaydolmak için laboratuvar oluşturucusundan aldığı kayıt bağlantısını kullanır.
- Laboratuvardaki bir sanal makineye bağlanır ve sınıf çalışmalarını, ödevleri ve projeleri yapmak için onu kullanır.

## <a name="next-steps"></a>Sonraki adımlar

Azure Lab Services kullanarak sınıf laboratuvarı oluşturmak için gereken laboratuvar hesabını ayarlamaya başlayın:

- [Bir laboratuvar hesabı ayarlama](tutorial-setup-lab-account.md)
