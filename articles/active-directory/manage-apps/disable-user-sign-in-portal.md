---
title: Azure AD'de kurumsal bir uygulama için kullanıcı oturum açma larını devre dışı
description: Azure Etkin Dizini'nde hiçbir kullanıcının oturum açamayabilmesi için kurumsal bir uygulamayı devre dışı etme
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10553898376c4b9236ee62718fffccd45b12d70b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74274085"
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Azure Active Directory'deki bir kurumsal uygulama için kullanıcı oturum açma oturumlarını devre dışı

Azure Etkin Dizini'nde (Azure AD) hiçbir kullanıcının oturum açaması için kurumsal bir uygulamayı devre dışı kullanabilirsiniz. Kurumsal uygulamayı yönetmek için uygun izinlere ihtiyacınız vardır. Ve, dizin için küresel yönetici olmalıdır.

## <a name="how-do-i-disable-user-sign-ins"></a>Kullanıcı oturum açma oturumlarını nasıl devre dışı alarım?

1. Dizin için genel yönetici olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açın.
1. **Tüm hizmetleri**seçin, metin kutusuna Azure **Etkin Dizini** girin ve sonra **Enter'u**seçin.
1. Azure **Etkin Dizin** -  ***dizini bölmesi*** (diğer bir deyişle, yöneticiliğiniz için Azure AD bölmesi) üzerinde **Kurumsal uygulamaları**seçin.
1. Kurumsal **uygulamalarda - Tüm uygulamalar** bölmesi, yönetebileceğiniz uygulamaların bir listesini görürsünüz. Bir uygulama seçin.
1. Ad ***bölmesi*** (diğer bir deyişle, başlıkta seçilen uygulamanın adını içeren bölme), **Özellikler'i**seçin.
1. **Özellikler** ***ekbnde,*** - kullanıcıların oturum **açmaması için Etkin emediye** **Hayır'ı** seçin.
1. **Kaydet** komutunu seçin.

## <a name="use-azure-ad-powershell-to-disable-an-unlisted-app"></a>Listelenmemiş bir uygulamayı devre dışı etmek için Azure AD PowerShell'i kullanma

Kurumsal uygulamalar listesinde görünmeyen bir uygulamanın AppId'ini biliyorsanız (örneğin, uygulamayı veya hizmet sorumlusu nun Microsoft tarafından ön izin li olması nedeniyle uygulamayı veya hizmet sorumlusu henüz oluşturulmadığıiçin), uygulama için hizmet ilkesini el ile oluşturabilir ve [AzureAD PowerShell cmdlet'i](https://docs.microsoft.com/powershell/module/azuread/New-AzureADServicePrincipal?view=azureadps-2.0)kullanarak uygulamayı devre dışı bırakabilirsiniz.

```PowerShell
# The AppId of the app to be disabled
$appId = "{AppId}"

# Check if a service principal already exists for the app
$servicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$appId'"
if ($servicePrincipal) {
    # Service principal exists already, disable it
    Set-AzureADServicePrincipal -ObjectId $servicePrincipal.ObjectId -AccountEnabled $false
} else {
    # Service principal does not yet exist, create it and disable it at the same time
    $servicePrincipal = New-AzureADServicePrincipal -AppId $appId -AccountEnabled $false
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tüm gruplarımı görün](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](assign-user-or-group-access-portal.md)
* [Bir kurumsal uygulamadan kullanıcı veya grup ataması kaldırma](remove-user-or-group-access-portal.md)
* [Kurumsal bir uygulamanın adını veya logosunu değiştirme](change-name-or-logo-portal.md)
