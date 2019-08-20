---
title: 'Azure Active Directory Domain Services: Grup tarafından yönetilen hizmet hesabı oluştur | Microsoft Docs'
description: Azure Active Directory Domain Services yönetilen etki alanları ile kullanmak üzere bir grup yönetilen hizmet hesabı (gMSA) oluşturmayı öğrenin
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: e6faeddd-ef9e-4e23-84d6-c9b3f7d16567
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: iainfou
ms.openlocfilehash: 3742aed7ff39e0a2f6bdf353fb9f261176027422
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69612951"
---
# <a name="create-a-group-managed-service-account-gmsa-on-an-azure-ad-domain-services-managed-domain"></a>Azure AD Domain Services yönetilen bir etki alanında Grup yönetilen hizmet hesabı (gMSA) oluşturma
Bu makalede, yönetilen hizmet hesaplarının Azure AD Domain Services yönetilen bir etki alanında nasıl oluşturulacağı gösterilmektedir.

## <a name="managed-service-accounts"></a>Yönetilen hizmet hesapları
Tek başına yönetilen hizmet hesabı (sMSA), parolası otomatik olarak yönetilen bir yönetilen etki alanı hesabıdır. Hizmet asıl adı (SPN) yönetimini basitleştirir ve diğer yöneticilere Temsilcili yönetim sağlar. Bu tür yönetilen hizmet hesabı (MSA) Windows Server 2008 R2 ve Windows 7 ' de tanıtılmıştır.

Grup tarafından yönetilen hizmet hesabı (gMSA), etki alanındaki birçok sunucu için aynı avantajları sağlar. Bir sunucu grubunda barındırılan bir hizmetin tüm örneklerinin, karşılıklı kimlik doğrulama protokollerinin çalışması için aynı hizmet sorumlusunu kullanması gerekir. Bir gMSA hizmet sorumlusu olarak kullanıldığında, Windows işletim sistemi, hesabın parolasını yöneticiye güvenmek yerine yönetir.

**Daha fazla bilgi:**
- [Grup yönetilen hizmet hesaplarına genel bakış](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Grup tarafından yönetilen hizmet hesapları ile çalışmaya başlama](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)


## <a name="using-service-accounts-in-azure-ad-domain-services"></a>Azure AD etki alanı Hizmetleri 'nde hizmet hesaplarını kullanma
Azure AD Domain Services yönetilen etki alanları Microsoft tarafından kilitlidir ve yönetilir. Azure AD Domain Services ile hizmet hesapları kullanırken birkaç temel dikkat edilecek önemli noktalar vardır.

### <a name="create-service-accounts-within-custom-organizational-units-ou-on-the-managed-domain"></a>Yönetilen etki alanında özel kuruluş birimleri (OU) içinde hizmet hesapları oluşturma
Yerleşik ' AADDC Users ' veya ' AADDC Computers ' Kurumsal birimlerinde bir hizmet hesabı oluşturamazsınız. Yönetilen etki alanında [Özel BIR OU oluşturun](create-ou.md) ve ardından bu özel OU içinde hizmet hesapları oluşturun.

### <a name="the-key-distribution-services-kds-root-key-is-already-pre-created"></a>Anahtar Dağıtım Hizmetleri (KDS) kök anahtarı önceden oluşturulmuş
Anahtar Dağıtım Hizmetleri (KDS) kök anahtarı Azure AD Domain Services yönetilen bir etki alanında önceden oluşturulur. KDS kök anahtarı oluşturmanız gerekmez ve bunu yapmak için ayrıcalıklarınız yoktur. Yönetilen etki alanında KDS kök anahtarını görüntüleyemezsiniz.

## <a name="sample---create-a-gmsa-using-powershell"></a>Örnek-PowerShell kullanarak gMSA oluşturma
Aşağıdaki örnek, PowerShell kullanarak özel bir OU oluşturmayı gösterir. Daha sonra bu OU içinde, OU 'yu belirtmek için ```-Path``` parametresini kullanarak bir gMSA oluşturabilirsiniz.

```powershell
# Create a new custom OU on the managed domain
New-ADOrganizationalUnit -Name "MyNewOU" -Path "DC=contoso,DC=COM"

# Create a service account 'WebFarmSvc' within the custom OU.
New-ADServiceAccount -Name WebFarmSvc  `
-DNSHostName ` WebFarmSvc.contoso.com  `
-Path "OU=MYNEWOU,DC=contoso,DC=com"  `
-KerberosEncryptionType AES128, AES256  ` -ManagedPasswordIntervalInDays 30  `
-ServicePrincipalNames http/WebFarmSvc.contoso.com/contoso.com, `
http/WebFarmSvc.contoso.com/contoso,  `
http/WebFarmSvc/contoso.com, http/WebFarmSvc/contoso  `
-PrincipalsAllowedToRetrieveManagedPassword CONTOSO-SERVER$
```

**PowerShell cmdlet belgeleri:**
- [New-ADOrganizationalUnit cmdlet 'i](https://docs.microsoft.com/powershell/module/addsadministration/new-adorganizationalunit)
- [New-ADServiceAccount cmdlet 'i](https://docs.microsoft.com/powershell/module/addsadministration/New-ADServiceAccount)


## <a name="next-steps"></a>Sonraki adımlar
- [Yönetilen bir etki alanında özel bir OU oluşturma](create-ou.md)
- [Grup yönetilen hizmet hesaplarına genel bakış](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Grup tarafından yönetilen hizmet hesapları ile çalışmaya başlama](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)
