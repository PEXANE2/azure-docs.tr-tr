---
title: Bir uygulamayı Azure AD'deki kullanıcı deneyiminden gizleme
description: Azure Active Directory erişim panellerinde veya Office 365 başlatıcılarında bir uygulamayı kullanıcının deneyiminden gizleme.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: mimart
ms.reviewer: kasimpso
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5718adf4fd76e2fbd0ff793dd2fa33ee08f7c0fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295043"
---
# <a name="hide-applications-from-end-users-in-azure-active-directory"></a>Azure Etkin Dizini'nde son kullanıcılardan uygulamaları gizleme

Son kullanıcıların MyApps panelinden veya Office 365 başlatıcısından uygulamaların nasıl gizlenine ilgili talimatlar. Bir uygulama gizlendiğinde, kullanıcıların yine de uygulama için izinleri vardır. 

## <a name="prerequisites"></a>Ön koşullar

Uygulama yöneticisi ayrıcalıkları MyApps paneli ve Office 365 başlatıcısı bir uygulama gizlemek için gereklidir.

Tüm Office 365 uygulamalarını gizlemek için genel yönetici ayrıcalıkları gereklidir.


## <a name="hide-an-application-from-the-end-user"></a>Uygulamayı son kullanıcıdan gizleme
MyApps panelinden ve Office 365 uygulama başlatıcısından bir uygulamayı gizlemek için aşağıdaki adımları kullanın.

1.  Dizininizin genel yöneticisi olarak [Azure portalında](https://portal.azure.com) oturum açın.
2.  **Azure Etkin Dizini'ni**seçin.
3.  **Kurumsal uygulamaları**seçin. **Kurumsal uygulamalar - Tüm uygulamalar** bıçak açılır.
4.  **Uygulama Türü**altında, zaten seçilmemişse **Kurumsal Uygulamalar'ı**seçin.
5.  Gizlemek istediğiniz uygulamayı arayın ve uygulamayı tıklatın.  Uygulamanın genel bakışı açılır.
6.  **Özellikler**'e tıklayın. 
7.  **Kullanıcılara Görünür?** sorusu için **Hayır'ı**tıklatın.
8.  **Kaydet**'e tıklayın.

## <a name="use-azure-ad-powershell-to-hide-an-application"></a>Bir uygulamayı gizlemek için Azure AD PowerShell'i kullanma

Bir uygulamayı MyApps panelinden gizlemek için, uygulama için HideApp etiketini el ile servis sorumlusuna ekleyebilirsiniz. Uygulamanın **Kullanıcılara Görünür özelliğini** **No**olarak ayarlamak için aşağıdaki [AzureAD PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#service_principals) komutlarını çalıştırın. 

```PowerShell
Connect-AzureAD

$objectId = "<objectId>"
$servicePrincipal = Get-AzureADServicePrincipal -ObjectId $objectId
$tags = $servicePrincipal.tags
$tags += "HideApp"
Set-AzureADServicePrincipal -ObjectId $objectId -Tags $tags
```

## <a name="hide-office-365-applications-from-the-myapps-panel"></a>Office 365 uygulamalarını MyApps panelinden gizleme

Tüm Office 365 uygulamalarını MyApps panelinden gizlemek için aşağıdaki adımları kullanın. Uygulamalar Office 365 portalında hala görülebilir.

1.  Dizininiz için genel bir yönetici olarak [Azure portalında](https://portal.azure.com) oturum açın.
2.  **Azure Etkin Dizini'ni**seçin.
3.  **Kullanıcı ayarları**'nı seçin.
4.  **Kurumsal uygulamalar**altında, son kullanıcıların uygulamalarını **nasıl başlatıp görüntülelerini yönet'i tıklatın.**
5.  Kullanıcılar için **Office 365 portalında office 365 uygulamalarını sadece görebilir**, **Evet'i**tıklatın.
6.  **Kaydet**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Tüm gruplarımı görün](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](assign-user-or-group-access-portal.md)
* [Bir kurumsal uygulamadan kullanıcı veya grup ataması kaldırma](remove-user-or-group-access-portal.md)
* [Kurumsal bir uygulamanın adını veya logosunu değiştirme](change-name-or-logo-portal.md)

