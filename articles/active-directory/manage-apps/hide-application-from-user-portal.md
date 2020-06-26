---
title: Azure AD 'de bir kurumsal uygulamayı kullanıcının deneyiminden gizleme
description: Azure Active Directory erişim panellerinde veya Office 365 launchers sürümünde kullanıcının deneyiminden bir kurumsal uygulamayı gizleme.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 03/25/2020
ms.author: kenwith
ms.reviewer: kasimpso
ms.collection: M365-identity-device-management
ms.openlocfilehash: d21ba14fba24c9b8e0b460e56b93d0e5212bfb27
ms.sourcegitcommit: bf8c447dada2b4c8af017ba7ca8bfd80f943d508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85367708"
---
# <a name="hide-enterprise-applications-from-end-users-in-azure-active-directory"></a>Azure Active Directory içindeki son kullanıcılardan kurumsal uygulamaları gizleyin

Son kullanıcıların Uygulamaps panelinden veya Office 365 başlatıcısı 'ndan uygulama gizleme yönergeleri. Bir uygulama gizli olduğunda, kullanıcıların uygulama izinleri hala vardır. 

## <a name="prerequisites"></a>Ön koşullar

Uygulama Yöneticisi ayrıcalıkları, uygulamayı Uygps panelinden ve Office 365 başlatıcısı 'ndan gizlemek için gereklidir.

Tüm Office 365 uygulamalarını gizlemek için genel yönetici ayrıcalıkları gerekir.


## <a name="hide-an-application-from-the-end-user"></a>Bir uygulamayı son kullanıcıdan gizleme
Uygulamaps panelinden ve Office 365 uygulama başlatıcısı 'ndan bir uygulamayı gizlemek için aşağıdaki adımları kullanın.

1.  [Azure Portal](https://portal.azure.com) , dizininiz için genel yönetici olarak oturum açın.
2.  **Azure Active Directory**seçin.
3.  **Kurumsal uygulamalar**' ı seçin. **Kurumsal uygulamalar-tüm uygulamalar** dikey penceresi açılır.
4.  **Uygulama türü**' nün altında, henüz seçili değilse **Kurumsal uygulamalar**' ı seçin.
5.  Gizlemek istediğiniz uygulamayı arayın ve uygulamayı tıklatın.  Uygulamanın genel görünümü açılır.
6.  **Özellikler**'e tıklayın. 
7.  **Kullanıcılar Için görünebilir mi?** sorusu için **Hayır**'a tıklayın.
8.  **Kaydet**’e tıklayın.

> [!NOTE]
> Bu yönergeler yalnızca kurumsal uygulamalar için geçerlidir.

## <a name="use-azure-ad-powershell-to-hide-an-application"></a>Azure AD PowerShell kullanarak bir uygulamayı gizleme

Uygulamaps panelinden bir uygulamayı gizlemek için, HideApp etiketini uygulamanın hizmet sorumlusuna el ile ekleyebilirsiniz. Uygulamanın **Users olarak görünür mü?** özelliğini **Hayır**olarak ayarlamak Için aşağıdaki [azuread PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#service_principals) komutlarını çalıştırın. 

```PowerShell
Connect-AzureAD

$objectId = "<objectId>"
$servicePrincipal = Get-AzureADServicePrincipal -ObjectId $objectId
$tags = $servicePrincipal.tags
$tags += "HideApp"
Set-AzureADServicePrincipal -ObjectId $objectId -Tags $tags
```

## <a name="hide-office-365-applications-from-the-myapps-panel"></a>Uygulamaps panelinden Office 365 uygulamalarını gizleme

Uygulamaps panelinden tüm Office 365 uygulamalarını gizlemek için aşağıdaki adımları kullanın. Uygulamalar hala Office 365 portalında görünür.

1.  [Azure Portal](https://portal.azure.com) , dizininiz için genel yönetici olarak oturum açın.
2.  **Azure Active Directory**seçin.
3.  **Kullanıcıları**seçin.
4.  **Kullanıcı ayarları**'nı seçin.
5.  **Kurumsal uygulamalar**altında **son kullanıcıların uygulamalarını nasıl başlatıp görüntüleyebileceğini Yönet** ' e tıklayın.
6.  **Kullanıcılar yalnızca office 365 portalında office 365 uygulamalarını görebilir**, **Evet**' e tıklayın.
7.  **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Tüm gruplarımı gör](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Kurumsal uygulamaya Kullanıcı veya Grup atama](assign-user-or-group-access-portal.md)
* [Bir kurumsal uygulamadan Kullanıcı veya grup atamasını kaldırma](remove-user-or-group-access-portal.md)
* [Kurumsal uygulamanın adını veya logosunu değiştirme](change-name-or-logo-portal.md)

