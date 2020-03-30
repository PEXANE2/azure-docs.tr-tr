---
title: Ayrıcalıklı Kimlik Yönetimi - Azure Active Directory | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure kaynak rolleri için güvenlik uyarılarını nasıl yapılandırılabilirsiniz öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: f0b938dc808d9b02ad4105d85a5b3125135c51d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74023080"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rolleri için güvenlik uyarılarını yapılandırma

Ayrıcalıklı Kimlik Yönetimi (PIM), Azure Etkin Dizin (Azure AD) kuruluşunuzda şüpheli veya güvenli olmayan etkinlik olduğunda uyarılar oluşturur. Bir uyarı tetiklendiğinde, Uyarılar sayfasında gösterir.

![Azure kaynakları - Uyarı sayfası listeleme uyarısı, risk düzeyi ve sayım](media/pim-resource-roles-configure-alerts/rbac-alerts-page.png)

## <a name="review-alerts"></a>Uyarıları gözden geçirin

Düzeltme kılavuzunun yanı sıra, uyarıyı tetikleyen kullanıcıları veya rolleri listeleyen bir rapor görmek için bir uyarı seçin.

![Son tetkik süresini, açıklamayı, azaltma adımlarını, türü, önem derecesini, güvenlik etkisini ve bir sonraki sefere nasıl önlenebileceğinizi gösteren uyarı raporu](media/pim-resource-roles-configure-alerts/rbac-alert-info.png)

## <a name="alerts"></a>Uyarılar

| Uyarı | Severity | Tetikleyici | Öneri |
| --- | --- | --- | --- |
| **Kaynağa atanan çok fazla sahip** |Orta |Çok fazla kullanıcı sahibi rolü var. |Listedeki kullanıcıları gözden geçirin ve bazılarını daha az ayrıcalıklı rollere yeniden atayın. |
| **Kaynağa atanan çok fazla kalıcı sahip** |Orta |Çok fazla kullanıcı kalıcı olarak bir role atanır. |Listedeki kullanıcıları gözden geçirin ve rol kullanımı için etkinleştirme gerektirecek şekilde bazılarını yeniden atayın. |
| **Yinelenen rol oluşturuldu** |Orta |Birden çok rol aynı ölçüte sahiptir. |Bu rollerden yalnızca birini kullanın. |

### <a name="severity"></a>Severity

- **Yüksek**: Bir ilke ihlali nedeniyle derhal harekete geçilmesini gerektirir. 
- **Orta**: Hemen harekete geçilmesini gerektirmez, ancak olası bir politika ihlalinin sinyallerini vermektedir.
- **Düşük**: Hemen harekete geçmeyi gerektirmez, ancak tercih edilen bir ilke değişikliği önerir.

## <a name="configure-security-alert-settings"></a>Güvenlik uyarı ayarlarını yapılandırma

Uyarılar sayfasından **Ayarlar**sayfasına gidin.

![Ayarlar vurgulanmış uyarılar sayfası](media/pim-resource-roles-configure-alerts/rbac-navigate-settings.png)

Ortamınız ve güvenlik hedefleriniz ile çalışmak için farklı uyarılardaki ayarları özelleştirin.

![Ayarları etkinleştirmek ve yapılandırmak için uyarı için sayfa ayarlama](media/pim-resource-roles-configure-alerts/rbac-alert-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
