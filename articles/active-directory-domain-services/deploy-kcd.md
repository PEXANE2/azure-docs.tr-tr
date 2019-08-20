---
title: 'Azure Active Directory Domain Services: Kerberos kısıtlanmış temsilciyi etkinleştirme | Microsoft Docs'
description: Azure Active Directory Domain Services yönetilen etki alanlarında Kerberos kısıtlanmış temsilciyi etkinleştir
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: iainfou
ms.openlocfilehash: f234eaea0d4df3859ef9458ea334f1b7616add34
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69612937"
---
# <a name="configure-kerberos-constrained-delegation-kcd-on-a-managed-domain"></a>Yönetilen bir etki alanında Kerberos kısıtlanmış temsilcisini (KCD) yapılandırma
Birçok uygulamanın, kullanıcı bağlamında kaynaklara erişmesi gerekir. Active Directory, bu kullanım örneğini sağlayan Kerberos temsili adlı bir mekanizmayı destekler. Ayrıca, kullanıcı bağlamında yalnızca belirli kaynaklara erişilebilmesi için temsilciyi kısıtlayabilirsiniz. Azure AD Domain Services yönetilen etki alanları, daha güvenli bir şekilde kilitlendiğinden geleneksel Active Directory etki alanlarından farklıdır.

Bu makalede, Azure AD Domain Services yönetilen bir etki alanında Kerberos kısıtlanmış temsilciyi nasıl yapılandıracağınız gösterilir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="kerberos-constrained-delegation-kcd"></a>Kerberos kısıtlanmış temsili (KCD)
Kerberos temsili, bir hesabın kaynaklara erişmek için başka bir güvenlik sorumlusunun (Kullanıcı gibi) taklit etmesine olanak sağlar. Bir kullanıcı bağlamında arka uç Web API 'sine erişen bir Web uygulaması düşünün. Bu örnekte, (bir hizmet hesabı veya bilgisayar/makine hesabı bağlamında çalışan) Web uygulaması, kaynağa erişirken (arka uç Web API 'SI) kullanıcıyı taklit eder. Kimliğe bürünme hesabının kullanıcı bağlamında erişebileceği kaynakları kısıtlayamadığından, Kerberos temsili güvenli değildir.

Kerberos kısıtlanmış temsili (KCD), belirtilen sunucunun bir kullanıcı adına davranabileceği Hizmetleri/kaynakları kısıtlar. Geleneksel KCD, bir hizmet için etki alanı hesabı yapılandırmak için etki alanı yöneticisi ayrıcalıkları gerektirir ve hesabı tek bir etki alanına kısıtlar.

Geleneksel KCD ile ilişkili bazı sorunlar da vardır. Önceki işletim sistemlerinde, etki alanı Yöneticisi hizmet için hesap tabanlı KCD 'yi yapılandırdıysa, hizmet yöneticisi, sahip oldukları kaynak hizmetleri için hangi ön uç hizmetlerin temsilci olarak olduğunu bilmemiz için kullanışlı bir yönteme sahip değildir. Ve bir kaynak hizmetine temsilci olabilecek bir ön uç hizmeti olası bir saldırı noktasını temsil eder. Ön uç hizmeti barındırılan bir sunucunun güvenliği tehlikeye girerse ve kaynak hizmetleri 'ne temsilci olarak yapılandırıldıysa, kaynak hizmetleri de tehlikeye girebilir.

> [!NOTE]
> Azure AD Domain Services yönetilen bir etki alanında etki alanı yöneticisi ayrıcalıklarına sahip değilsiniz. Bu nedenle, **yönetilen bir etki alanında geleneksel hesap tabanlı KCD yapılandırılamaz**. Bu makalede açıklandığı gibi kaynak tabanlı KCD 'YI kullanın. Bu mekanizma da daha güvenlidir.
>
>

## <a name="resource-based-kcd"></a>Kaynak tabanlı KCD
Windows Server 2012 ve sonraki sürümlerde, hizmet yöneticileri, Hizmetleri için kısıtlanmış temsilciyi yapılandırma olanağı elde edebilir. Bu modelde, arka uç hizmet yöneticisi, belirli ön uç hizmetlerinin KCD kullanmasını sağlayabilir veya reddedebilir. Bu model **kaynak tabanlı KCD**olarak bilinir.

Kaynak tabanlı KCD, PowerShell kullanılarak yapılandırılır. Kimliğe bürünme hesabının `Set-ADComputer` bir `Set-ADUser` bilgisayar hesabı mı yoksa bir kullanıcı hesabı/hizmet hesabı mı olduğuna bağlı olarak, veya cmdlet 'lerini kullanabilirsiniz.

### <a name="configure-resource-based-kcd-for-a-computer-account-on-a-managed-domain"></a>Yönetilen bir etki alanındaki bilgisayar hesabı için kaynak tabanlı KCD 'yi yapılandırma
' Contoso-webapp.contoso.com ' bilgisayarında çalışan bir Web uygulamanız olduğunu varsayalım. Etki alanı kullanıcıları bağlamında kaynağa (' contoso-api.contoso.com ' üzerinde çalışan bir Web API 'SI) erişmesi gerekir. Bu senaryo için kaynak tabanlı KCD 'YI nasıl ayarlayabileceğinizi aşağıda bulabilirsiniz:

1. [Özel BIR OU oluşturun](create-ou.md). Bu özel OU 'yu yönetilen etki alanındaki kullanıcılara yönetmek için izinler atayabilirsiniz.
2. Hem sanal makineleri (Web uygulamasını çalıştıran bir Web uygulamasını ve Web API 'sini çalıştıran) yönetilen etki alanına ekleyin. Bu bilgisayar hesaplarını özel OU içinde oluşturun.
3. Şimdi, aşağıdaki PowerShell komutunu kullanarak kaynak tabanlı KCD 'yi yapılandırın:

```powershell
$ImpersonatingAccount = Get-ADComputer -Identity contoso-webapp.contoso.com
Set-ADComputer contoso-api.contoso.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

> [!NOTE]
> Web uygulaması ve Web API 'sinin bilgisayar hesaplarının kaynak tabanlı KCD 'yi yapılandırma izninizin olduğu özel bir OU 'da olması gerekir. Yerleşik ' AAD DC Computers ' kapsayıcısında bir bilgisayar hesabı için kaynak tabanlı KCD 'yi yapılandıramazsınız.
>

### <a name="configure-resource-based-kcd-for-a-user-account-on-a-managed-domain"></a>Yönetilen bir etki alanında bir kullanıcı hesabı için kaynak tabanlı KCD 'yi yapılandırma
' Appsvc ' hizmet hesabı olarak çalışan bir Web uygulamanız olduğunu ve etki alanı kullanıcıları bağlamında kaynağa (hizmet hesabı olarak çalışan bir Web API 'SI ' backendsvc ') erişmesi gerektiğini varsayın. Bu senaryo için kaynak tabanlı KCD 'yi nasıl ayarlayabileceğinizi aşağıda bulabilirsiniz.

1. [Özel BIR OU oluşturun](create-ou.md). Bu özel OU 'yu yönetilen etki alanındaki kullanıcılara yönetmek için izinler atayabilirsiniz.
2. Arka uç Web API 'SI/kaynağını çalıştıran sanal makineyi yönetilen etki alanına ekleyin. Kendi bilgisayar hesabını özel OU içinde oluşturun.
3. Web uygulamasını özel OU içinde çalıştırmak için kullanılan hizmet hesabını (örneğin, ' appsvc ') oluşturun.
4. Şimdi, aşağıdaki PowerShell komutunu kullanarak kaynak tabanlı KCD 'yi yapılandırın:

```powershell
$ImpersonatingAccount = Get-ADUser -Identity appsvc
Set-ADUser backendsvc -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

> [!NOTE]
> Arka uç Web API 'SI ve hizmet hesabının bilgisayar hesabının her ikisi de kaynak tabanlı KCD 'yi yapılandırma izninizin olduğu özel bir OU 'da olmalıdır. Yerleşik ' AAD DC Computers ' kapsayıcısında veya yerleşik ' AAD DC kullanıcıları ' kapsayıcısındaki kullanıcı hesaplarında, bir bilgisayar hesabı için kaynak tabanlı KCD 'yi yapılandıramazsınız. Bu nedenle, kaynak tabanlı KCD 'yi ayarlamak için Azure AD 'den eşitlenen Kullanıcı hesaplarını kullanamazsınız.
>

## <a name="related-content"></a>İlgili İçerik
* [Azure AD Domain Services-Başlarken Kılavuzu](tutorial-create-instance.md)
* [Kerberos kısıtlı temsilciye genel bakış](https://technet.microsoft.com/library/jj553400.aspx)
