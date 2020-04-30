---
title: Azure RBAC için en iyi uygulamalar
description: Azure rol tabanlı erişim denetimi (Azure RBAC) kullanmaya yönelik en iyi uygulamalar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/17/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: dc86dd488ff9e8649ae80f4768941791dd37fce6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81726780"
---
# <a name="best-practices-for-azure-rbac"></a>Azure RBAC için en iyi uygulamalar

Bu makalede, Azure rol tabanlı erişim denetimi (Azure RBAC) kullanmaya yönelik bazı en iyi yöntemler açıklanmaktadır. Bu en iyi uygulamalar, Azure RBAC ile deneyimimizin yanı sıra sizin gibi müşterilerin deneyimlerini elde edilmiştir.

## <a name="only-grant-the-access-users-need"></a>Yalnızca kullanıcılara ihtiyacı olan erişime izin ver

Azure RBAC kullanarak, ekiplerinizi takımınızın içinde ayırabilirsiniz ve yalnızca işlerini gerçekleştirmesi için ihtiyaç duydukları kullanıcılara erişim miktarını verebilirsiniz. Herkese Azure aboneliğiniz veya kaynaklarınızda sınırsız izin vermek yerine belirli eylemlere yalnızca belirli kapsamlarda izin verebilirsiniz.

Erişim denetimi stratejinizi planlarken en iyi yöntem, kullanıcılara yalnızca işlerini yapmak için gerekli olan ayrıcalığı tanımaktır. Aşağıdaki diyagramda RBAC kullanımı için önerilen model gösterilmiştir.

![RBAC ve en düşük öncelik](./media/best-practices/rbac-least-privilege.png)

Rol atamaları ekleme hakkında daha fazla bilgi için bkz. [rol atamaları ekleme veya kaldırma](role-assignments-portal.md).

## <a name="limit-the-number-of-subscription-owners"></a>Abonelik sahiplerinin sayısını sınırlayın

Güvenliği aşılmış bir sahibe göre ihlal olasılığını azaltmak için en fazla 3 abonelik sahibi olmanız gerekir. Bu öneri, Azure Güvenlik Merkezi ' nde izlenebilir. Güvenlik Merkezi 'ndeki diğer kimlik ve erişim önerileri için bkz. [güvenlik önerileri-bir başvuru kılavuzu](../security-center/recommendations-reference.md).

## <a name="use-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management kullan

Ayrıcalıklı hesapları kötü niyetli Cyber saldırılarına karşı korumak için Azure Active Directory Privileged Identity Management (PıM) kullanarak ayrıcalıkların etkilenme süresini azaltır ve raporlar ve uyarılar aracılığıyla kullanım için görünürlüğünüzü artırabilirsiniz. PıM, Azure AD ve Azure kaynaklarına tam zamanında ayrıcalıklı erişim sağlayarak ayrıcalıklı hesapların korunmasına yardımcı olur. Erişim, otomatik olarak iptal edilen ayrıcalıkların zaman sınırı olabilir. 

Daha fazla bilgi için bkz. [Azure AD Privileged Identity Management nedir?](../active-directory/privileged-identity-management/pim-configure.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC sorunlarını giderme](troubleshooting.md)