---
title: Azure RBAC için en iyi uygulamalar
description: Azure rol tabanlı erişim denetimi (Azure RBAC) kullanmak için en iyi uygulamalar.
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
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726780"
---
# <a name="best-practices-for-azure-rbac"></a>Azure RBAC için en iyi uygulamalar

Bu makalede, Azure rol tabanlı erişim denetimi (Azure RBAC) kullanmak için bazı en iyi uygulamalar açıklanmaktadır. Bu en iyi uygulamalar Azure RBAC ile olan deneyimlerimizden ve sizin gibi müşterilerin deneyimlerinden elde edilir.

## <a name="only-grant-the-access-users-need"></a>Yalnızca kullanıcıların gereksinim duyduğu erişimi verme

Azure RBAC'ı kullanarak, ekibinizin görevlerini ayırabilir ve yalnızca işlerini gerçekleştirmek için gereken kullanıcılara erişim miktarı verebilirsiniz. Herkese Azure aboneliğiniz veya kaynaklarınızda sınırsız izin vermek yerine belirli eylemlere yalnızca belirli kapsamlarda izin verebilirsiniz.

Erişim denetimi stratejinizi planlarken en iyi yöntem, kullanıcılara yalnızca işlerini yapmak için gerekli olan ayrıcalığı tanımaktır. Aşağıdaki diyagramda RBAC kullanımı için önerilen model gösterilmiştir.

![RBAC ve en düşük öncelik](./media/best-practices/rbac-least-privilege.png)

Rol atamalarının nasıl eklenilen hakkında bilgi için [bkz.](role-assignments-portal.md)

## <a name="limit-the-number-of-subscription-owners"></a>Abonelik sahiplerinin sayısını sınırlama

Tehlikeye atılmış bir sahibin ihlal potansiyelini azaltmak için en fazla 3 abonelik sahibine sahip olmalısınız. Bu öneri Azure Güvenlik Merkezi'nde izlenebilir. Güvenlik Merkezi'ndeki diğer kimlik ve erişim önerileri için, [bir başvuru kılavuzu olan Güvenlik önerilerine](../security-center/recommendations-reference.md)bakın.

## <a name="use-azure-ad-privileged-identity-management"></a>Azure AD Ayrıcalıklı Kimlik Yönetimi'ni kullanma

Ayrıcalıklı hesapları kötü amaçlı siber saldırılara karşı korumak için, ayrıcalıkların maruz kalma süresini azaltmak ve raporlar ve uyarılar aracılığıyla görünürlüğünüzü artırmak için Azure Active Directory Ayrıcalıklı Kimlik Yönetimi'ni (PIM) kullanabilirsiniz. PIM, Azure AD ve Azure kaynaklarına tam zamanında ayrıcalıklı erişim sağlayarak ayrıcalıklı hesapların korunmasına yardımcı olur. Erişim, ayrıcalıkların otomatik olarak iptal edilmesinden sonra zamana bağlı olabilir. 

Daha fazla bilgi için Azure [AD Ayrıcalıklı Kimlik Yönetimi nedir?](../active-directory/privileged-identity-management/pim-configure.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Sorun Giderme Azure RBAC](troubleshooting.md)