---
title: Ayrıcalıklı Kimlik Yönetimi kullanmak için lisans gereksinimleri - Azure Active Directory | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi 'ni (PIM) kullanmak için lisans gereksinimlerini açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 01/10/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70696cdb95fffc1e5faa46ca1b5f2180633ed63a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75932330"
---
# <a name="license-requirements-to-use-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'ni kullanmak için lisans gereksinimleri

Azure Etkin Dizin (Azure AD) Ayrıcalıklı Kimlik Yönetimi 'ni (PIM) kullanmak için bir dizinin geçerli bir lisansı olması gerekir. Ayrıca, lisanslar yöneticilere ve ilgili kullanıcılara atanmalıdır. Bu makalede, Ayrıcalıklı Kimlik Yönetimi kullanmak için lisans gereksinimleri açıklanmaktadır.

## <a name="valid-licenses"></a>Geçerli lisanslar

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

## <a name="how-many-licenses-must-you-have"></a>Kaç lisansın olmalı?

Dizininizin, aşağıdaki görevleri yerine getirecek çalışanlarınız kadar en az sayıda Azure AD Premium P2 lisansına sahip olduğundan emin olun:

- Azure REKLAM rollerine uygun olarak atanan kullanıcılar PIM kullanılarak yönetildi
- PIM'de etkinleştirme isteklerini onaylayan veya reddedebilen kullanıcılar
- Tam zamanında veya doğrudan (zaman tabanlı) atamalarla Azure kaynak rolüne atanan kullanıcılar  
- Erişim incelemesine atanan kullanıcılar
- Erişim incelemeleri gerçekleştiren kullanıcılar

Azure AD Premium P2 lisansları aşağıdaki görevler için gerekli **değildir:**

- PIM'yi kuran, ilkeleri yapılandıran, uyarı alan ve erişim incelemeleri ayarlayan Global Administrator veya Ayrıcalıklı Rol Yöneticisi rollerine sahip kullanıcılar için lisans gerekmez.

Lisanslar hakkında daha fazla bilgi için Azure [Etkin Dizin portalını kullanarak lisans atayın veya kaldırın'a](../fundamentals/license-users-groups.md)bakın.

## <a name="example-license-scenarios"></a>Örnek lisans senaryoları

Burada, sahip olduğunuz lisans sayısını belirlemenize yardımcı olacak bazı örnek lisans senaryoları verilmiştir.

| Senaryo | Hesaplama | Lisans sayısı |
| --- | --- | --- |
| Woodgrove Bank'ın farklı departmanlar için 10 yöneticisi ve PIM'i yapılandıran ve yöneten 2 Global Yöneticisi vardır. Beş yöneticiyi uygun hale getirin. | Uygun yöneticiler için beş lisans | 5 |
| Grafik Tasarım Enstitüsü'nde 14'ü PIM üzerinden yönetilen 25 yönetici bulunmaktadır. Rol etkinleştirme onayı gerektirir ve kuruluşta etkinleştirmeleri onaylayabilecek üç farklı kullanıcı vardır. | Uygun roller için 14 lisans + üç onaylayıcı | 17 |
| Contoso'nun 42'si PIM üzerinden yönetilen 50 yöneticisi vardır. Rol etkinleştirme onayı gerektirir ve kuruluşta etkinleştirmeleri onaylayabilecek beş farklı kullanıcı vardır. Contoso ayrıca yönetici rollerine atanan kullanıcıların aylık incelemelerini yapar ve gözden geçirenler, altısı PIM tarafından yönetilen yönetici rollerinde olmayan kullanıcı yöneticileridir. | Uygun roller için 42 lisans + beş onaylayıcı + altı gözden geçiren | 53 |

## <a name="what-happens-when-a-license-expires"></a>Lisansın süresi dolduğunda ne olur?

Azure AD Premium P2, EMS E5 veya deneme lisansının süresi dolduğunda, Ayrıcalıklı Kimlik Yönetimi özellikleri artık dizininizde kullanılamaz:

- Azure REKLAM rolleriiçin kalıcı rol atamaları etkilenmez.
- Azure portalındaki Ayrıcalıklı Kimlik Yönetimi hizmetinin yanı sıra, Ayrıcalıklı Kimlik Yönetimi'nin Grafik API cmdlets'leri ve PowerShell arayüzleri artık kullanıcıların ayrıcalıklı rolleri etkinleştirmeleri, ayrıcalıklı erişimi yönetmeleri veya gerçekleştiremeleri için kullanılamaz ayrıcalıklı rollerin değerlendirmelerini erişim.
- Kullanıcılar artık ayrıcalıklı rolleri etkinleştiremeyeceğinden, Azure AD rollerinin uygun rol atamaları kaldırılır.
- Azure AD rolleriyle ilgili devam eden erişim incelemeleri sona erer ve Ayrıcalıklı Kimlik Yönetimi yapılandırma ayarları kaldırılır.
- Ayrıcalıklı Kimlik Yönetimi artık rol atama değişiklikleri yle ilgili e-postalar göndermez.

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management'ı dağıtma](pim-deployment-plan.md)
- [Privileged Identity Management'ı kullanmaya başlama](pim-getting-started.md)
- [Ayrıcalıklı Kimlik Yönetiminde Yönetemeyeceğiniz Roller](pim-roles.md)
