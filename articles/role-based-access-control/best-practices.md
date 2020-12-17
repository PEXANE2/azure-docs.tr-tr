---
title: Azure RBAC için en iyi uygulamalar
description: Azure rol tabanlı erişim denetimi (Azure RBAC) kullanmaya yönelik en iyi uygulamalar.
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: conceptual
ms.workload: identity
ms.date: 12/16/2020
ms.author: rolyon
ms.openlocfilehash: 00fd00112dad9efc31cea83f69bb458a8e1ca935
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97617383"
---
# <a name="best-practices-for-azure-rbac"></a>Azure RBAC için en iyi uygulamalar

Bu makalede, Azure rol tabanlı erişim denetimi (Azure RBAC) kullanmaya yönelik bazı en iyi yöntemler açıklanmaktadır. Bu en iyi uygulamalar, Azure RBAC ile deneyimimizin yanı sıra sizin gibi müşterilerin deneyimlerini elde edilmiştir.

## <a name="only-grant-the-access-users-need"></a>Yalnızca kullanıcılara ihtiyacı olan erişime izin ver

Azure RBAC kullanarak ekibiniz içinde görevleri ayırabilir, bu işlere gerek duyan kişilere sadece erişim miktarını verebilirsiniz. Herkese Azure aboneliğiniz veya kaynaklarınızda sınırsız izin vermek yerine belirli eylemlere yalnızca belirli kapsamlarda izin verebilirsiniz.

Erişim denetimi stratejinizi planlarken en iyi yöntem, kullanıcılara yalnızca işlerini yapmak için gerekli olan ayrıcalığı tanımaktır. Daha geniş bir rolün daha kolay bir şekilde daha kolay görünse bile daha geniş kapsamlar atamaktan kaçının. Özel roller oluştururken, yalnızca kullanıcıların ihtiyacı olan izinleri ekleyin. Rolleri ve kapsamları sınırlayarak, güvenlik sorumlusu tehlikeye girerse, hangi kaynakların risk altında olduğunu sınırlayabilirsiniz.

Aşağıdaki diyagramda Azure RBAC kullanımı için önerilen bir model gösterilmektedir.

![Azure RBAC ve en düşük ayrıcalık](./media/best-practices/rbac-least-privilege.png)

Rol atamaları ekleme hakkında daha fazla bilgi için, bkz. [Azure Portal kullanarak Azure rol atamaları ekleme veya kaldırma](role-assignments-portal.md).

## <a name="limit-the-number-of-subscription-owners"></a>Abonelik sahiplerinin sayısını sınırlayın

Güvenliği aşılmış bir sahibe göre ihlal olasılığını azaltmak için en fazla 3 abonelik sahibi olmanız gerekir. Bu öneri, Azure Güvenlik Merkezi ' nde izlenebilir. Güvenlik Merkezi 'ndeki diğer kimlik ve erişim önerileri için bkz. [güvenlik önerileri-bir başvuru kılavuzu](../security-center/recommendations-reference.md).

## <a name="use-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management kullan

Ayrıcalıklı hesapları kötü niyetli Cyber saldırılarına karşı korumak için Azure Active Directory Privileged Identity Management (PıM) kullanarak ayrıcalıkların etkilenme süresini azaltır ve raporlar ve uyarılar aracılığıyla kullanım için görünürlüğünüzü artırabilirsiniz. PıM, Azure AD ve Azure kaynaklarına tam zamanında ayrıcalıklı erişim sağlayarak ayrıcalıklı hesapların korunmasına yardımcı olur. Erişim, otomatik olarak iptal edilen ayrıcalıkların zaman sınırı olabilir. 

Daha fazla bilgi için bkz. [Azure AD Privileged Identity Management nedir?](../active-directory/privileged-identity-management/pim-configure.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC sorunlarını giderme](troubleshooting.md)