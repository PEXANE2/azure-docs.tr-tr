---
title: Privileged Identity Management kullanılacak lisans gereksinimleri-Azure Active Directory | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) kullanmak için lisans gereksinimlerini açıklar.
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
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75932330"
---
# <a name="license-requirements-to-use-privileged-identity-management"></a>Privileged Identity Management kullanılacak lisans gereksinimleri

Azure Active Directory (Azure AD) Privileged Identity Management (PıM) kullanmak için, bir dizinin geçerli bir lisansı olması gerekir. Ayrıca, lisansların Yöneticiler ve ilgili kullanıcılara atanması gerekir. Bu makalede Privileged Identity Management kullanmak için lisans gereksinimleri açıklanmaktadır.

## <a name="valid-licenses"></a>Geçerli lisanslar

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

## <a name="how-many-licenses-must-you-have"></a>Kaç lisansa sahip olmanız gerekir?

Aşağıdaki görevleri yerine getirmek istediğiniz çalışanlara sahip olduğunuz için, dizininizde en az sayıda Azure AD Premium P2 lisansa sahip olduğundan emin olun:

- PıM kullanılarak yönetilen Azure AD rollerine uygun olarak atanan kullanıcılar
- PıM 'de etkinleştirme isteklerini onaylayabilecek veya reddedebilen kullanıcılar
- Tam zamanında veya doğrudan (zaman tabanlı) atamalarla bir Azure Kaynak rolüne atanan kullanıcılar  
- Erişim gözden geçirmesi için atanan kullanıcılar
- Erişim gözden geçirmeleri gerçekleştiren kullanıcılar

Aşağıdaki görevler için Azure AD Premium P2 lisansları gerekli **değildir** :

- PıM 'yi ayarlama, uyarıları alma ve erişim incelemelerini ayarlama gibi genel yönetici veya ayrıcalıklı rol yöneticisi rollerine sahip kullanıcılar için lisans gerekmez.

Lisanslar hakkında daha fazla bilgi için bkz. [Azure Active Directory portalını kullanarak lisans atama veya kaldırma](../fundamentals/license-users-groups.md).

## <a name="example-license-scenarios"></a>Örnek lisans senaryoları

Sahip olmanız gereken lisans sayısını belirlemenize yardımcı olacak bazı örnek lisans senaryoları aşağıda verilmiştir.

| Senaryo | Hesaplama | Lisans sayısı |
| --- | --- | --- |
| Woodgrove Bank, farklı departmanlar için 10 yönetici ve PıM 'yi yapılandıran ve yöneten 2 genel yönetici vardır. Bunlar beş yönetici için uygun hale getirir. | Uygun yöneticiler için beş lisans | 5 |
| Grafik tasarımı Enstitüsü, 14 ' ün PıM aracılığıyla yönettiği 25 yöneticileridir. Rol etkinleştirme onay gerektirir ve kuruluşta etkinleştirmeleri onaylayabilecek üç farklı kullanıcı vardır. | uygun roller ve üç onaylayan için 14 lisans | 17 |
| Contoso, 50 'e 42 ait, PıM aracılığıyla yönetilmekte olan yöneticilerine sahiptir. Rol etkinleştirme onay gerektirir ve kuruluşta etkinleştirmeleri onaylayabilecek beş farklı kullanıcı vardır. Contoso Ayrıca, yönetici rollerine atanan kullanıcılara yönelik aylık incelemeler ve gözden geçirenler, kullanıcının PıM tarafından yönetilen yönetici rollerde bulunmayan Kullanıcı yöneticilerinde de bulunur. | uygun rollere yönelik 42 lisans + beş onaylayan + altı gözden geçiren | 53 |

## <a name="what-happens-when-a-license-expires"></a>Lisansın süresi dolarsa ne olur?

Bir Azure AD Premium P2, EMS E5 veya deneme lisansının süresi dolarsa Privileged Identity Management özellikler artık dizininizde kullanılamaz:

- Azure AD rollerine yönelik kalıcı rol atamaları bundan etkilenmeyecektir.
- Azure portal Privileged Identity Management hizmetinin yanı sıra Privileged Identity Management Graph API cmdlet 'leri ve PowerShell arabirimleri artık kullanıcıların ayrıcalıklı rolleri etkinleştirmesine, ayrıcalıklı erişimi yönetmesine veya gerçekleştirmesine izin vermez ayrıcalıklı rollerin erişim gözden geçirmeleri.
- Azure AD rollerinin uygun rol atamaları kaldırılacak, çünkü kullanıcılar artık ayrıcalıklı rolleri etkinleştiremeyecektir.
- Azure AD rollerinin devam eden tüm erişim İncelemeleri sona acaktır ve Privileged Identity Management yapılandırma ayarları kaldırılır.
- Privileged Identity Management, artık rol atama değişikliklerinde e-posta göndermeyecektir.

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management dağıt](pim-deployment-plan.md)
- [Privileged Identity Management kullanmaya başlayın](pim-getting-started.md)
- [Privileged Identity Management içinde yönetemezsiniz roller](pim-roles.md)
