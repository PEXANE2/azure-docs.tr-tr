---
title: Raporlama için Azure AD PowerShell cmdlet 'leri | Microsoft Docs
description: Raporlama için Azure AD PowerShell cmdlet 'lerinin başvurusu.
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 07/12/2019
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 372041dc24db4ed66977364e2a2997c7195cca9c
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68988027"
---
# <a name="azure-ad-powershell-cmdlets-for-reporting"></a>Raporlama için Azure AD PowerShell cmdlet’leri

Azure Active Directory (Azure AD) raporlarında, yönlerinizde (denetim günlükleri) ve kimlik doğrulama verilerinde (oturum açma günlükleri) tüm yazma işlemlerinde bulunan etkinliklerle ilgili ayrıntılı bilgi edinebilirsiniz. Bilgiler MS Graph API kullanılarak kullanılabilir olsa da, artık raporlama için Azure AD PowerShell cmdlet 'lerini kullanarak aynı verileri alabilirsiniz.

Bu makale, denetim günlükleri ve oturum açma günlükleri için kullanılacak PowerShell cmdlet 'lerine genel bir bakış sunar.

## <a name="audit-logs"></a>Denetim günlükleri

[Denetim günlükleri](concept-audit-logs.md) , Azure AD içindeki çeşitli özelliklerle gerçekleştirilen tüm değişiklikler için Günlükler aracılığıyla izlenebilirlik sağlar. Denetim günlüklerine örnek olarak, Azure AD 'de Kullanıcı, uygulama, Grup, rol ve ilke ekleme veya kaldırma gibi herhangi bir kaynak üzerinde yapılan değişiklikler bulunur.

' Get-AzureADAuditDirectoryLogs cmdlet 'ini kullanarak denetim günlüklerine erişebilirsiniz.


| Senaryo                      | PowerShell komutu |
| :--                           | :--                |
| Uygulama görünen adı      | Get-AzureADAuditDirectoryLogs-filtre "/App/displayName EQ ' Azure AD bulut eşitlemesi '" |
| Category                      | Get-AzureADAuditDirectoryLogs-filtre "category EQ ' uygulama yönetimi '" |
| Etkinlik tarihi saati            | Get-AzureADAuditDirectoryLogs-filtre "activityDateTime gt 2019-04-18" |
| Yukarıdakilerin tümü              | Get-AzureADAuditDirectoryLogs-filtre "/uygulama/displayName EQ ' Azure AD bulut eşitleme ' ve kategori EQ ' uygulama yönetimi ' ve activityDateTime gt 2019-04-18"|


Aşağıdaki görüntüde bu komut için bir örnek gösterilmektedir. 

!["Veri özeti" düğmesi](./media/reference-powershell-reporting/get-azureadauditdirectorylogs.png)



## <a name="sign-in-logs"></a>Oturum açma günlükleri

[Oturum açma](concept-sign-ins.md) günlükleri, yönetilen uygulamaların kullanımı ve Kullanıcı oturum açma etkinliklerinin kullanımı hakkında bilgi sağlar.

' Get-Azureadauditsignınlogs cmdlet 'ini kullanarak oturum açma günlüklerine erişim edinirsiniz.


| Senaryo                      | PowerShell komutu |
| :--                           | :--                |
| Kullanıcı Görünen Adı             | Get-Azureadauditsignınlogs-"userDisplayName EQ ' Timoingperkins '" filtresini filtrele |
| Tarih saat oluştur              | Get-Azureadauditsignınlogs-Filter "createdDateTime gt 2019-04-18T17:30:00.0 Z" (4/18 tarihinde 5:30 ile bu yana her şey) |
| Durum                        | Get-Azureadauditsignınlogs-filtre "Status/errorCode EQ 50105" |
| Uygulama görünen adı      | Get-Azureadauditsignınlogs-Filter "appDisplayName EQ ' StoreFrontStudio [wsbessenabled] '" |
| Yukarıdakilerin tümü              | Get-Azureadauditsignınlogs-"userDisplayName EQ ' Timoilperkins ' ve Status/errorCode ne 0 ve appDisplayName EQ ' StoreFrontStudio [wsbessenabled] '" filtrelemesine filtre uygulayın |


Aşağıdaki görüntüde bu komut için bir örnek gösterilmektedir. 

!["Veri özeti" düğmesi](./media/reference-powershell-reporting/get-azureadauditsigninlogs.png)



## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD raporlarına genel bakış](overview-reports.md).
- [Denetim günlükleri raporu](concept-audit-logs.md). 
- [Azure AD raporlarına programlı erişim](concept-reporting-api.md)
