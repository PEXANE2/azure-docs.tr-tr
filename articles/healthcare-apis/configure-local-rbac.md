---
title: FHıR için Azure API 'SI için yerel rol tabanlı Access Control (RBAC) yapılandırma
description: Bu makalede, FHıR için Azure API 'sinin veri düzlemi için dış bir Azure AD kiracısı kullanacak şekilde nasıl yapılandırılacağı açıklanır.
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 03/15/2020
ms.author: mihansen
ms.openlocfilehash: a8c1b36d6a439297dfb0bbcb34efe059349fc5a2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871913"
---
# <a name="configure-local-rbac-for-fhir"></a>FHıR için yerel RBAC 'yi yapılandırma 

Bu makalede, FHıR için Azure API 'sinin, veri düzlemi erişimini yönetmek için dış, ikincil Azure Active Directory kiracı kullanacak şekilde nasıl yapılandırılacağı açıklanmaktadır. Bu modu yalnızca aboneliğinizle ilişkili Azure Active Directory kiracı kullanmanız mümkün değilse kullanın.

> [!NOTE]
> FHıR hizmeti veri düzlemi, aboneliğinizle ilişkili birincil Azure Active Directory kiracınızı kullanacak şekilde yapılandırıldıysa, [veri düzlemi rolleri atamak Için Azure RBAC kullanın](configure-azure-rbac.md).

## <a name="add-service-principal"></a>Hizmet sorumlusu ekleme

Yerel RBAC, FHıR sunucunuza bir dış Azure Active Directory kiracı kullanmanıza olanak sağlar. RBAC sisteminin bu Kiracıdaki grup üyeliklerini denetlemesine izin vermek için, FHıR için Azure API 'sinin kiracıda bir hizmet sorumlusu olmalıdır. Bu hizmet sorumlusu, FHıR için Azure API 'SI dağıtmış olan aboneliklerde bulunan kiracılarda otomatik olarak oluşturulur, ancak kiracınızın kendisine bağlı bir aboneliği yoksa, bir kiracı yöneticisinin aşağıdaki komutlardan biriyle bu hizmet sorumlusunu oluşturması gerekir:

`Az`PowerShell modülünü kullanma:

```azurepowershell-interactive
New-AzADServicePrincipal -ApplicationId 3274406e-4e0a-4852-ba4f-d7226630abb7
```

isterseniz PowerShell modülünü de kullanabilirsiniz `AzureAd` :

```azurepowershell-interactive
New-AzureADServicePrincipal -AppId 3274406e-4e0a-4852-ba4f-d7226630abb7
```

isterseniz Azure CLı 'yi de kullanabilirsiniz:

```azurecli-interactive
az ad sp create --id 3274406e-4e0a-4852-ba4f-d7226630abb7
```

## <a name="configure-local-rbac"></a>Yerel RBAC 'yi yapılandırma

FHıR için Azure API 'sini, **kimlik doğrulama** dikey penceresinde dış veya ikincil Azure Active Directory kiracı kullanacak şekilde yapılandırabilirsiniz:

![Yerel RBAC atamaları](media/rbac/local-rbac-guids.png).

Yetkili kutusuna geçerli bir Azure Active Directory kiracı girin. Kiracı doğrulandıktan sonra **Izin verilen nesne kimlikleri** kutusu etkinleştirilmelidir ve kimlik nesnesi kimliklerinin bir listesini girebilirsiniz. Bu kimlikler, öğesinin kimlik nesne kimlikleri olabilir:

* Azure Active Directory Kullanıcı.
* Bir Azure Active Directory hizmet sorumlusu.
* Bir Azure Active Directory güvenlik grubu.

Daha fazla ayrıntı için [kimlik nesne kimliklerini bulma](find-identity-object-ids.md) makalesini okuyabilirsiniz.

Gerekli nesne kimliklerini girdikten sonra **Kaydet** ' e tıklayın ve atanan kullanıcılar, hizmet sorumluları veya grupları kullanarak veri düzlemine erişmeyi denemeden önce değişikliklerin kaydedilmesini bekleyin.

## <a name="caching-behavior"></a>Önbelleğe alma davranışı

FHıR için Azure API 'SI, kararları 5 dakikaya kadar önbelleğe alacak. İzin verilen nesne kimlikleri listesine ekleyerek FHıR sunucusuna bir kullanıcı erişimi verirseniz veya onları listeden kaldırırsanız, bu, en fazla beş dakika sonra yayarak yayma izinleridir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir dış (ikincil) Azure Active Directory kiracısı kullanarak FHıR veri düzlemi erişimi atamayı öğrendiniz. Daha sonra FHıR için Azure API 'SI için ek ayarlar hakkında bilgi edinin:
 
>[!div class="nextstepaction"]
>[Ek ayarlar FHıR için Azure API](azure-api-for-fhir-additional-settings.md)

