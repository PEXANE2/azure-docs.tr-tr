---
title: Azure AD Etki Alanı Hizmetleri için grup yönetilen hizmet hesapları | Microsoft Dokümanlar
description: Azure Active Directory Domain Services yönetilen etki alanlarında kullanılmak üzere grup yönetilen hizmet hesabı (gMSA) oluşturmayı öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: e6faeddd-ef9e-4e23-84d6-c9b3f7d16567
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: 5955f52cda73630f371a46f83ac0fb9a252b80e3
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80655479"
---
# <a name="create-a-group-managed-service-account-gmsa-in-azure-ad-domain-services"></a>Azure AD Etki Alanı Hizmetlerinde grup yönetilen hizmet hesabı (gMSA) oluşturma

Uygulamalar ve hizmetler genellikle diğer kaynaklarla kendilerini doğrulamak için bir kimlik gerekir. Örneğin, bir web hizmetinin bir veritabanı hizmetiyle kimlik doğrulaması gerekebilir. Bir uygulama veya hizmetin web sunucusu çiftliği gibi birden çok örneği varsa, bu kaynakların kimliklerini el ile oluşturma ve yapılandırma zaman alır.

Bunun yerine, Azure Etkin Dizin Etki Alanı Hizmetleri (Azure AD DS) yönetilen etki alanında grup yönetilen hizmet hesabı (gMSA) oluşturulabilir. Windows İşletim Sistemi, büyük kaynak gruplarının yönetimini kolaylaştıran bir gMSA'nın kimlik bilgilerini otomatik olarak yönetir.

Bu makalede, Azure PowerShell kullanarak Azure AD DS yönetilen etki alanında gMSA nasıl oluşturulabileceğiniz gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, bir Azure [Active Directory Etki Alanı Hizmetleri örneği oluşturmak ve yapılandırmak][create-azure-ad-ds-instance]için öğreticiyi tamamlayın.
* Azure AD DS yönetilen etki alanına katılan bir Windows Server yönetimi VM'si.
    * Gerekirse, bir yönetim [VM oluşturmak][tutorial-create-management-vm]için öğretici tamamlayın.

## <a name="managed-service-accounts-overview"></a>Yönetilen hizmet hesaplarına genel bakış

Bağımsız yönetilen hizmet hesabı (sMSA), parolası otomatik olarak yönetilen bir etki alanı hesabıdır. Bu yaklaşım, hizmet ana adı (SPN) yönetimini basitleştirir ve diğer yöneticilere devredilen yönetimi sağlar. Hesap için kimlik bilgilerini el ile oluşturmanız ve döndürmeniz gerekmez.

Grup yönetilen hizmet hesabı (gMSA), etki alanında birden çok sunucu için aynı yönetim basitleştirmesini sağlar. gMSA, sunucu çiftliğinde barındırılan bir hizmetin tüm örneklerinin, karşılıklı kimlik doğrulama protokollerinin çalışması için aynı hizmet ilkesini kullanmasına olanak tanır. Bir gMSA hizmet sorumlusu olarak kullanıldığında, Windows işletim sistemi yöneticiye güvenmek yerine hesabın parolasını yeniden yönetir.

Daha fazla bilgi için [grup yönetilen hizmet hesaplarına (gMSA) genel bakış][gmsa-overview]abakın.

## <a name="using-service-accounts-in-azure-ad-ds"></a>Azure AD DS'de hizmet hesaplarını kullanma

Azure AD DS yönetilen etki alanları Microsoft tarafından kilitlendiğinden ve microsoft tarafından yönetildiğinden, hizmet hesaplarını kullanırken bazı noktalar vardır:

* Yönetilen etki alanında özel kuruluş birimlerinde (OU) hizmet hesapları oluşturun.
    * Yerleşik *AADDC Kullanıcıları* veya *AADDC Computers* OS'lerde bir hizmet hesabı oluşturamaz.
    * Bunun yerine, Azure AD DS yönetilen etki alanında [özel bir OU oluşturun][create-custom-ou] ve ardından bu özel OU'da hizmet hesapları oluşturun.
* Anahtar Dağıtım Hizmetleri (KDS) kök anahtarı önceden oluşturulmuştur.
    * KDS kök anahtarı, gMSA'lar için parola oluşturmak ve almak için kullanılır. Azure AD DS'de KDS kökü sizin için oluşturulur.
    * Başka bir kds kök anahtarı oluşturmak veya varsayılan KDS kök anahtarını görüntülemek için ayrıcalıklarınız yoktur.

## <a name="create-a-gmsa"></a>gMSA oluşturma

İlk olarak, [Yeni-ADOrganizationalUnit][New-AdOrganizationalUnit] cmdlet kullanarak özel bir OU oluşturun. Özel OSB oluşturma ve yönetme hakkında daha fazla bilgi için [Azure AD DS'deki Özel OS'lere][create-custom-ou]bakın.

> [!TIP]
> Bir gMSA oluşturmak için bu adımları tamamlamak [için, yönetim VM kullanın.][tutorial-create-management-vm] Bu yönetim VM zaten gerekli AD PowerShell cmdlets ve yönetilen etki alanına bağlantı olmalıdır.

Aşağıdaki örnek, *aaddscontoso.com*adlı Azure AD DS yönetilen etki alanında *myNewOU* adlı özel bir OU oluşturur. Kendi OU ve yönetilen etki alanı adınızı kullanın:

```powershell
New-ADOrganizationalUnit -Name "myNewOU" -Path "DC=aaddscontoso,DC=COM"
```

Şimdi [New-ADServiceAccount][New-ADServiceAccount] cmdlet kullanarak bir gMSA oluşturun. Aşağıdaki örnek parametreler tanımlanır:

* **-Adı** *WebFarmSvc* olarak ayarlanır
* **-Yol** parametresi, önceki adımda oluşturulan gMSA için özel OU'yu belirtir.
* DNS girişleri ve hizmet temel adları *WebFarmSvc.aaddscontoso.com* için ayarlanır
* *AADDSCONTOSO-SERVER$* adresindeki müdürlerin kimliği kullanarak parolayı almasına izin verilir.

Kendi adlarınızı ve etki alanı adlarınızı belirtin.

```powershell
New-ADServiceAccount -Name WebFarmSvc `
    -DNSHostName WebFarmSvc.aaddscontoso.com `
    -Path "OU=MYNEWOU,DC=aaddscontoso,DC=com" `
    -KerberosEncryptionType AES128, AES256 `
    -ManagedPasswordIntervalInDays 30 `
    -ServicePrincipalNames http/WebFarmSvc.aaddscontoso.com/aaddscontoso.com, `
        http/WebFarmSvc.aaddscontoso.com/aaddscontoso, `
        http/WebFarmSvc/aaddscontoso.com, `
        http/WebFarmSvc/aaddscontoso `
    -PrincipalsAllowedToRetrieveManagedPassword AADDSCONTOSO-SERVER$
```

Uygulamalar ve hizmetler artık gerektiğinde gMSA'yı kullanacak şekilde yapılandırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

GMSA'lar hakkında daha fazla bilgi için [bkz.][gmsa-start]

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[create-custom-ou]: create-ou.md

<!-- EXTERNAL LINKS -->
[New-ADOrganizationalUnit]: /powershell/module/addsadministration/New-AdOrganizationalUnit
[New-ADServiceAccount]: /powershell/module/addsadministration/New-AdServiceAccount
[gmsa-overview]: /windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview
[gmsa-start]: /windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts
