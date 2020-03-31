---
title: Sınıf Laboratuvarları kavramları - Azure Lab Hizmetleri | Microsoft Dokümanlar
description: Laboratuvar Hizmetleri'nin temel kavramlarını ve laboratuvar oluşturmayı ve yönetmeyi nasıl kolaylaştırabileceğini öğrenin.
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
ms.openlocfilehash: 15fd3c18d059466c2b2bd5e2431013f393092b4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77526238"
---
# <a name="classroom-labs-concepts"></a>Sınıf Laboratuvarları kavramları

Aşağıdaki liste önemli Laboratuvar Hizmetleri kavramlarını ve tanımlarını içerir:

## <a name="quota"></a>Kota

Kontenjan, bir öğretmenin bir öğrencinin laboratuvar VM'sini kullanması için ayarlayabileceği zaman sınırıdır (saatlerde). 0 veya belirli bir saat sayısı olarak ayarlanabilir. Kontenjan 0 olarak ayarlanmışsa, öğrenci sanal makineyi yalnızca bir program çalışırken veya öğretmen öğrenci için sanal makineyi el ile döndüğünde kullanabilir.  

Öğrenci laboratuvar VM'yi başlattığında kontenjan saatleri sayılır.  Bir öğretmen bir öğrenci için laboratuvar VM'yi el ile başlatırsa, kontenjan saatleri o öğrenci için kullanılmaz.

## <a name="schedules"></a>Zamanlamalar

Program, öğrenci VM'lerinin sınıf saati için kullanılabilmesi için öğretmenin sınıf için oluşturabileceği zaman dilimleridir.  Zamanlamalar tek seferlik veya yinelenen olabilir.  Bir zamanlama çalışırken kota saatleri kullanılmaz.

Üç tür zamanlama vardır: Standart, yalnızca Başlat ve Yalnızca Durdur.

- **Standart**.  Bu zamanlama, tüm öğrenci VM'lerini belirtilen başlangıç saatinde başlatacak ve belirtilen duraklama zamanındaki tüm öğrenci VM'lerini kapatacaktır.
- **Yalnızca başlatın.**   Bu zamanlama, tüm öğrenci VM'lerini belirtilen zamanda başlatır.  Öğrenci VM'leri, bir öğrenci Azure Lab Hizmetleri portalı üzerinden VM'lerini durdurana veya yalnızca bir durak çizelgesi gerçekleşene kadar durdurulmaz.
- **Sadece dur.**  Bu zamanlama, belirtilen zamanda tüm öğrenci VM'lerini durdurur.  

## <a name="template-virtual-machine"></a>Şablon sanal makine

Laboratuvardaki şablon sanal makine, tüm kullanıcıların sanal makinelerinin oluşturulduğu temel sanal makine görüntüsüdür. Eğitmenler/laboratuvar oluşturucuları şablonu sanal makineyi ayarlar ve katılımcılara laboratuvar yapmak için eğitmek istedikleri yazılımla yapılandırır. Bir şablon VM yayımladığınızda, Azure Lab Hizmetleri şablon VM'yi temel alan laboratuvar VM'leri oluşturur veya güncelleştirir.

## <a name="user-profiles"></a>Kullanıcı profilleri

Bu makalede Azure Lab Services içindeki farklı kullanıcı profilleri açıklanmaktadır.

### <a name="lab-account-owner"></a>Laboratuvar hesap sahibi

Genellikle, bir kuruluşun bulut kaynaklarının, aynı zamanda Azure aboneliğinin sahibi olan BT yöneticisi, laboratuvar hesap sahibi olarak hareket eder ve aşağıdaki görevleri yerine getirir:

- Kuruluşunuz için bir laboratuvar hesabı ayarlar.
- Tüm laboratuvarlardaki ilkeleri yönetir ve yapılandırır.
- Kuruluştaki kişilere laboratuvar hesabı altında laboratuvar oluşturma izinleri verir.

### <a name="professor"></a>Profesör

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
