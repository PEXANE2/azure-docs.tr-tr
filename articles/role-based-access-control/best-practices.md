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
ms.openlocfilehash: 053e86f3493c7a11a3cbbaad0871e45345697878
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2020
ms.locfileid: "82735343"
---
# <a name="best-practices-for-azure-rbac"></a>Azure RBAC için en iyi uygulamalar

Bu makalede, Azure rol tabanlı erişim denetimi (Azure RBAC) kullanmaya yönelik bazı en iyi yöntemler açıklanmaktadır. Bu en iyi uygulamalar, Azure RBAC ile deneyimimizin yanı sıra sizin gibi müşterilerin deneyimlerini elde edilmiştir.

## <a name="only-grant-the-access-users-need"></a>Yalnızca kullanıcılara ihtiyacı olan erişime izin ver

Azure RBAC kullanarak ekibiniz içinde görevleri ayırabilir, bu işlere gerek duyan kişilere sadece erişim miktarını verebilirsiniz. Herkese Azure aboneliğiniz veya kaynaklarınızda sınırsız izin vermek yerine belirli eylemlere yalnızca belirli kapsamlarda izin verebilirsiniz.

Erişim denetimi stratejinizi planlarken en iyi yöntem, kullanıcılara yalnızca işlerini yapmak için gerekli olan ayrıcalığı tanımaktır. Aşağıdaki diyagramda Azure RBAC kullanımı için önerilen bir model gösterilmektedir.

![Azure RBAC ve en düşük ayrıcalık](./media/best-practices/rbac-least-privilege.png)

Rol atamaları ekleme hakkında daha fazla bilgi için, bkz. [Azure Portal kullanarak Azure rol atamaları ekleme veya kaldırma](role-assignments-portal.md).

## <a name="limit-the-number-of-subscription-owners"></a>Abonelik sahiplerinin sayısını sınırlayın

Güvenliği aşılmış bir sahibe göre ihlal olasılığını azaltmak için en fazla 3 abonelik sahibi olmanız gerekir. Bu öneri, Azure Güvenlik Merkezi ' nde izlenebilir. Güvenlik Merkezi 'ndeki diğer kimlik ve erişim önerileri için bkz. [güvenlik önerileri-bir başvuru kılavuzu](../security-center/recommendations-reference.md).

## <a name="use-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management kullan

Ayrıcalıklı hesapları kötü niyetli Cyber saldırılarına karşı korumak için Azure Active Directory Privileged Identity Management (PıM) kullanarak ayrıcalıkların etkilenme süresini azaltır ve raporlar ve uyarılar aracılığıyla kullanım için görünürlüğünüzü artırabilirsiniz. PıM, Azure AD ve Azure kaynaklarına tam zamanında ayrıcalıklı erişim sağlayarak ayrıcalıklı hesapların korunmasına yardımcı olur. Erişim, otomatik olarak iptal edilen ayrıcalıkların zaman sınırı olabilir. 

Daha fazla bilgi için bkz. [Azure AD Privileged Identity Management nedir?](../active-directory/privileged-identity-management/pim-configure.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC sorunlarını giderme](troubleshooting.md)