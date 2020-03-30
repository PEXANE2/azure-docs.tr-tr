---
title: Raporlama için Azure AD PowerShell cmdlets | Microsoft Dokümanlar
description: Raporlama için Azure AD PowerShell cmdlets başvurusu.
services: active-directory
documentationcenter: ''
author: MarkusVi
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
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2192c472e00d123780ec6bc5574e7b9fe326258b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75495315"
---
# <a name="azure-ad-powershell-cmdlets-for-reporting"></a>Raporlama için Azure AD PowerShell cmdlet’leri

> [!NOTE] 
> Bu Powershell cmdlets şu anda yalnızca [Azure AD Önizleme](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#directory_auditing) Modülü ile çalışır. Önizleme modülünün üretim kullanımı için önerilemediğini lütfen unutmayın. 

Genel önizleme yayınını yüklemek için aşağıdakileri kullanın. 

```powershell
Install-module AzureADPreview
```
Powershell kullanarak Azure AD'ye nasıl bağlanılacak hakkında daha fazla bilgi için lütfen [Graph için Azure AD Powershell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0)makalesine bakın.  

Azure Active Directory (Azure AD) raporlarıyla, yönünüzüzdeki tüm yazma işlemleri (denetim günlükleri) ve kimlik doğrulama verileri (oturum açma günlükleri) etrafındaki etkinlikler hakkında ayrıntılı bilgi alabilirsiniz. Bilgiler MS Graph API'sini kullanarak kullanılabilir olsa da, artık raporlama için Azure AD PowerShell cmdlets'i kullanarak aynı verileri alabilirsiniz.

Bu makalede, denetim günlükleri ve oturum açma günlükleri için kullanılacak PowerShell cmdlets genel bir bakış sağlar.

## <a name="audit-logs"></a>Denetim günlükleri

[Denetim günlükleri,](concept-audit-logs.md) Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. Denetim günlüklerine örnek olarak, Azure AD'deki kullanıcılar, uygulamalar, gruplar, roller ve ilkeler ekleme veya kaldırma gibi kaynaklarda yapılan değişiklikler verilebilir.

'Get-AzureADAuditDirectoryLogs cmdlet'i kullanarak denetim günlüklerine erişebilirsiniz.


| Senaryo                      | PowerShell komutu |
| :--                           | :--                |
| Uygulama Görüntü Adı      | AzureADAuditDirectoryLogs -Filtre "başlatılanBy/app/displayName eq 'Azure AD Cloud Sync'" |
| Kategori                      | Get-AzureADAuditDirectoryLogs -Filtre "kategori eq 'Uygulama Yönetimi'" |
| Etkinlik Tarihi Saati            | Al-AzureADAuditDirectoryLogs -Filtre "activityDateTime gt 2019-04-18" |
| Yukarıdakilerin tümü              | Al-AzureADAuditDirectoryLogs -Filtre "başlatılanBy/app/displayName eq 'Azure AD Cloud Sync' ve kategori eq 'Uygulama Yönetimi' ve etkinlikDateTime gt 2019-04-18"|


Aşağıdaki resimde bu komut için bir örnek gösterilmektedir. 

!["Veri Özeti" düğmesi](./media/reference-powershell-reporting/get-azureadauditdirectorylogs.png)



## <a name="sign-in-logs"></a>Oturum açma günlükleri

[Oturum açma günlükleri,](concept-sign-ins.md) yönetilen uygulamaların kullanımı ve kullanıcı oturum açma etkinlikleri hakkında bilgi sağlar.

'Get-AzureADAuditSignInLogs cmdlet' kullanarak oturum açma günlüklerine erişebilirsiniz.


| Senaryo                      | PowerShell komutu |
| :--                           | :--                |
| Kullanıcı Ekran Adı             | Al-AzureADAuditSignInLogs -Filtre "userDisplayName eq 'Timothy Perkins'" |
| Tarih Saati Oluşturma              | Get-AzureADAuditSignInLogs -Filtre "createdDateTime gt 2019-04-18T17:30:00.0Z" (17:30'dan beri her şey 18:00'de) |
| Durum                        | Al-AzureADAuditSignInLogs -Filtre "durum/hataKodu eq 50105" |
| Uygulama Görüntü Adı      | Al-AzureADAuditSignInLogs -Filtre "appDisplayName eq 'StoreFrontStudio [wsfed etkin]'" |
| Yukarıdakilerin tümü              | Get-AzureADAuditSignInLogs -Filtre "userDisplayName eq 'Timothy Perkins' ve durum / errorCode ne 0 ve appDisplayName eq 'StoreFrontStudio [wsfed etkin]'" |


Aşağıdaki resimde bu komut için bir örnek gösterilmektedir. 

!["Veri Özeti" düğmesi](./media/reference-powershell-reporting/get-azureadauditsigninlogs.png)



## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD raporlarına genel bakış.](overview-reports.md)
- [Denetim günlükleri raporu.](concept-audit-logs.md) 
- [Azure AD raporlarına programlı erişim](concept-reporting-api.md)
