---
title: Ayrıcalıklı Kimlik Yönetimi ile ilgili bir sorunu giderme - Azure Active Directory | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi (PIM) 'deki rollerle sistem hatalarını nasıl giderdiğinizi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 10/18/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: 474f2634e6f7ddc1840548c39ae86cb54c3bf08e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299695"
---
# <a name="troubleshoot-a-problem-with-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi ile ilgili bir sorunu giderme

Azure Etkin Dizin (Azure AD)'de Ayrıcalıklı Kimlik Yönetimi (PIM) ile ilgili bir sorun mu yaşıyorsunuz? Aşağıdaki bilgiler, işleri yeniden çalıştırmanıza yardımcı olabilir.

## <a name="access-to-azure-resources-denied"></a>Azure kaynaklarına erişim reddedildi

### <a name="problem"></a>Sorun

Azure kaynağının etkin sahibi veya kullanıcı erişim yöneticisi olarak, kaynağınızı Ayrıcalıklı Kimlik Yönetimi içinde görebilirsiniz, ancak uygun bir atama yapmak veya kaynaktan rol atamaları listesini görüntülemek gibi herhangi bir eylem gerçekleştiremezsiniz genel bakış sayfası. Bu eylemlerden herhangi biri yetkilendirme hatasıyla sonuçlanır.

### <a name="cause"></a>Nedeni

Bu sorun, PIM hizmet sorumlusunun Kullanıcı Erişim Yöneticisi rolü yanlışlıkla abonelikten kaldırıldığında olabilir. Ayrıcalıklı Kimlik Yönetimi hizmetinin Azure kaynaklarına erişebilmesi için, MS-PIM hizmet sorumlusunun Azure aboneliği yerine Her zaman [Kullanıcı Erişim Yöneticisi rolüne](../../role-based-access-control/built-in-roles.md#user-access-administrator) atanmış olması gerekir.

### <a name="resolution"></a>Çözüm

Kullanıcı Erişim Yöneticisi rolünü abonelik düzeyinde Ayrıcalıklı kimlik yönetimi hizmeti ana adına (MS-PIM) atayın. Bu atama, Ayrıcalıklı kimlik yönetimi hizmetinin Azure kaynaklarına erişmesine izin vermelidir. Rol, gereksinimlerinize bağlı olarak yönetim grubu düzeyinde veya abonelik düzeyinde atanabilir. Daha fazla bilgi hizmeti ilkeleri için bir [role uygulama atama'ya](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-a-role-to-the-application)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'ni kullanmak için lisans gereksinimleri](subscription-requirements.md)
- [Azure AD'de karma ve bulut dağıtımları için ayrıcalıklı erişim güvenliğini sağlama](../users-groups-roles/directory-admin-roles-secure.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json)
- [Privileged Identity Management'ı dağıtma](pim-deployment-plan.md)
