---
title: PıM-Azure Active Directory kullanmak için lisans gereksinimleri | Microsoft Docs
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
ms.date: 01/16/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 83c31c2731a8e872dfd2750fced8b91d283d0892
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804064"
---
# <a name="license-requirements-to-use-pim"></a>PıM 'yi kullanmak için lisans gereksinimleri

Azure Active Directory (Azure AD) Privileged Identity Management (PıM) kullanmak için, bir dizinin geçerli bir lisansı olması gerekir. Ayrıca, lisansların Yöneticiler ve ilgili kullanıcılara atanması gerekir. Bu makalede, PıM 'yi kullanmak için lisans gereksinimleri açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

PıM 'yi kullanmak için dizininiz aşağıdaki ücretli veya deneme lisanslarından birine sahip olmalıdır:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5
- Microsoft 365 M5

Daha fazla bilgi için bkz. [Azure Active Directory nedir?](../fundamentals/active-directory-whatis.md).

## <a name="which-users-must-have-licenses"></a>Hangi kullanıcıların Lisansı olmalıdır?

PıM 'den faydalanabilir veya bu avantajlardan yararlanan her yönetici veya Kullanıcı bir lisansa sahip olmalıdır. Örneklere şunlar dahildir:

- Azure AD rollerine yönelik PıM kullanılarak yönetilen Yöneticiler
- Azure Kaynak rolleri ile PıM kullanılarak yönetilen Yöneticiler
- Ayrıcalıklı rol yöneticisi rolüne atanan yöneticiler
- PıM kullanılarak yönetilen Azure AD rollerine uygun olarak atanan kullanıcılar
- Kullanıcı PıM 'de istekleri onaylayabilir/reddedebiliyor
- Tam zamanında veya doğrudan (zaman tabanlı) atamalarla bir Azure Kaynak rolüne atanan kullanıcılar  
- Erişim gözden geçirmesi için atanan kullanıcılar
- Erişim gözden geçirmeleri gerçekleştiren kullanıcılar

Kullanım için lisansları atama hakkında daha fazla bilgi için, bkz. [Azure Active Directory portalını kullanarak lisans atama veya kaldırma](../fundamentals/license-users-groups.md).

## <a name="what-happens-when-a-license-expires"></a>Lisansın süresi dolarsa ne olur?

Bir Azure AD Premium P2, EMS E5 veya deneme lisansının süresi dolarsa, artık dizininizde PıM özellikleri kullanılamaz:

- Azure AD rollerine yönelik kalıcı rol atamaları bundan etkilenmeyecektir.
- Azure portal Graph API cmdlet 'leri ve PıM 'nin PowerShell arabirimleri, kullanıcıların ayrıcalıklı rolleri etkinleştirmesine, ayrıcalıklı erişimi yönetmesine veya ayrıcalıklı rollere yönelik erişim gözden geçirmeleri gerçekleştirmesine izin vermez.
- Azure AD rollerinin uygun rol atamaları kaldırılacak, çünkü kullanıcılar artık ayrıcalıklı rolleri etkinleştiremeyecektir.
- Azure AD rollerinin devam eden tüm erişim İncelemeleri sona acaktır ve PıM yapılandırma ayarları kaldırılır.
- PıM, rol atama değişikliklerinde artık e-posta göndermeyecektir.

## <a name="next-steps"></a>Sonraki adımlar

- [PıM dağıtma](pim-deployment-plan.md)
- [PIM kullanmaya başlama](pim-getting-started.md)
- [PıM 'de yöneteceğiniz roller](pim-roles.md)
