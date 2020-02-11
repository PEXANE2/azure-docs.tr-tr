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
ms.date: 02/26/2019
ms.author: spelluru
ms.openlocfilehash: 348340516f9332f5492c7ce60c3d164da44a008c
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77120887"
---
# <a name="classroom-labs-concepts"></a>Sınıf Laboratuvarları kavramları

Aşağıdaki liste, Temel Laboratuvar Hizmetleri kavramlarını ve tanımlarını içerir:

## <a name="quota"></a>Kota

Kota, bir öğrencinin bir laboratuvar VM 'si kullanabilmesi için bir öğretmen tarafından ayarlanabildiğinden zaman sınırı (saat cinsinden). 0 veya belirli bir saat kadar ayarlanabilir. Kota 0 olarak ayarlandıysa, bir öğrenci yalnızca bir zamanlama çalışırken veya bir öğretmen, öğrenci için sanal makineyi el ile açtığında sanal makineyi kullanabilir.  

Öğrenci laboratuvar VM 'sini başlattığında kota saatleri sayılır.  Bir öğretmen, bir öğrenci için laboratuvar VM 'sini el ile başlattığında, bu öğrenci için kota saatleri kullanılmaz.

## <a name="schedules"></a>Zamanlamalar

Zamanlamalar, bir öğretmenin sınıf için oluşturabileceğiniz zaman yuvaları (bir kerelik veya yinelenen). Laboratuvardaki tüm sanal makineler, zamanlamanın başlangıcında otomatik olarak başlatılır ve zamanlamanın sonunda durdurulur. Bir zamanlama çalışırken kota saatleri kullanılmaz.

## <a name="template-virtual-machine"></a>Şablon sanal makinesi

Laboratuvardaki bir şablon sanal makinesi, tüm kullanıcıların sanal makinelerinin oluşturulduğu temel bir sanal makine görüntüsüdür. Gelişme/laboratuvar oluşturucuları, şablon sanal makinesini ayarlayıp BT laboratuvarlarına eğitim katılımcılarına sağlamak istedikleri yazılımla yapılandırır. Bir şablon VM yayımladığınızda Azure Lab Services, şablon VM 'yi temel alarak laboratuvar VM 'Leri oluşturur veya güncelleştirir.

## <a name="user-profiles"></a>Kullanıcı profilleri

Bu makalede Azure Lab Services içindeki farklı kullanıcı profilleri açıklanmaktadır.

### <a name="lab-account-owner"></a>Laboratuvar hesap sahibi

Genellikle, bir kuruluşun bulut kaynaklarının, aynı zamanda Azure aboneliğinin sahibi olan BT yöneticisi, laboratuvar hesap sahibi olarak hareket eder ve aşağıdaki görevleri yerine getirir:

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
