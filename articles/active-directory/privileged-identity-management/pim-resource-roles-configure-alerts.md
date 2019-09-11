---
title: PıM-Azure Active Directory 'de Azure Kaynak rolleri için güvenlik uyarılarını yapılandırma | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içindeki Azure Kaynak rolleri için güvenlik uyarılarını yapılandırma hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/02/2018
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 177f61392c3e441c891ba1b531301b3dae8c0db2
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804244"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-pim"></a>PıM 'de Azure Kaynak rolleri için güvenlik uyarılarını yapılandırma
Azure Active Directory (Azure AD) Privileged Identity Management (PıM), ortamınızda şüpheli veya güvenli olmayan bir etkinlik olduğunda uyarılar oluşturur. Bir uyarı tetiklendiğinde Uyarılar sayfasında görüntülenir. 

![Azure kaynakları-uyarılar sayfası listeleme uyarısı, risk düzeyi ve sayı](media/pim-resource-roles-configure-alerts/rbac-alerts-page.png)

## <a name="review-alerts"></a>Uyarıları gözden geçirin
Uyarıyı tetikleyen Kullanıcı veya rolleri listeleyen bir rapor görmek için bir uyarı seçin ve düzeltme önerisi ile birlikte.

![Son tarama zamanını, açıklamayı, risk azaltma adımlarını, türü, önem derecesini, güvenlik etkisini ve bir sonraki sefer nasıl önlemeyi gösteren uyarı raporu](media/pim-resource-roles-configure-alerts/rbac-alert-info.png)

## <a name="alerts"></a>Uyarılar
| Uyarı | severity | Tetikleyici | Öneri |
| --- | --- | --- | --- |
| **Bir kaynağa çok fazla sahip atandı** |Orta |Çok fazla kullanıcı sahip rolüne sahip. |Listedeki kullanıcıları gözden geçirin ve bazılarını daha az ayrıcalıklı rollere yeniden atayın. |
| **Bir kaynağa çok fazla sayıda kalıcı sahip atandı** |Orta |Rol için çok fazla sayıda kullanıcı kalıcı olarak atandı. |Listedeki kullanıcıları gözden geçirin ve rol kullanımı için etkinleştirmeyi gerektirmek üzere bazılarını yeniden atayın. |
| **Yinelenen rol oluşturuldu** |Orta |Birden çok rol aynı ölçütlere sahiptir. |Bu rollerden yalnızca birini kullanın. |


### <a name="severity"></a>severity
* **Yüksek**: İlke ihlali nedeniyle hemen eyleme gerek duyar. 
* **Orta**: Acil eylem gerektirmez, ancak olası bir ilke ihlaline işaret eder.
* **Düşük**: Acil eylem gerektirmez, ancak tercih edilen bir ilke değişikliği önerir.

## <a name="configure-security-alert-settings"></a>Güvenlik Uyarısı ayarlarını yapılandırma
Uyarılar sayfasından **Ayarlar**' a gidin.

![Ayarlar vurgulanmış olarak uyarılar sayfası](media/pim-resource-roles-configure-alerts/rbac-navigate-settings.png)

Farklı uyarılardaki ayarları, ortamınız ve güvenlik hedeflerinizle çalışacak şekilde özelleştirin.

![Ayarları etkinleştirmek ve yapılandırmak için bir uyarının sayfası ayarlanıyor](media/pim-resource-roles-configure-alerts/rbac-alert-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure Kaynak rolü ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
