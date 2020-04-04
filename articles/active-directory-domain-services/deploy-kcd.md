---
title: Azure AD Etki Alanı Hizmetleri için Kerberos kısıtlı delegasyon | Microsoft Dokümanlar
description: Azure Active Directory Domain Services yönetilen etki alanında kaynak tabanlı Kerberos kısıtlı temsilciliği (KCD) nasıl etkinleştireceğinizi öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: 07aa9ade25d1d986833b6da2f3907d07b752b662
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80655434"
---
# <a name="configure-kerberos-constrained-delegation-kcd-in-azure-active-directory-domain-services"></a>Azure Active Directory Etki Alanı Hizmetlerinde Kerberos kısıtlı delegasyonu (KCD) yapılandırın

Uygulamaları çalıştırırken, bu uygulamaların farklı bir kullanıcı bağlamında kaynaklara erişmeleri gerekebilir. Active Directory Domain Services (AD DS), *kerberos delegasyonu* adı verilen ve bu kullanım örneğini etkinleştiren bir mekanizmayı destekler. Kerberos *kısıtlı* delegasyon (KCD) sonra kullanıcı bağlamında erişilebilir belirli kaynakları tanımlamak için bu mekanizma üzerine inşa eder. Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS) yönetilen etki alanları, geleneksel şirket içi AD DS ortamlarına göre daha güvenli bir şekilde kilitlenir, bu nedenle daha güvenli bir *kaynak tabanlı* KCD kullanın.

Bu makalede, azure AD DS yönetilen bir etki alanında kaynak tabanlı Kerberos kısıtlı delegasyonu nasıl yapılandırılabildiğiniz gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için aşağıdaki kaynaklara ihtiyacınız var:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, [bir Azure Etkin Dizin Etki Alanı Hizmetleri örneği oluşturun ve yapılandırın.][create-azure-ad-ds-instance]
* Azure AD DS yönetilen etki alanına katılan bir Windows Server yönetimi VM'si.
    * Gerekirse, bir Windows [Server VM oluşturmak için][create-join-windows-vm] öğretici yi tamamlayın ve yönetilen bir etki alanına katılmak sonra AD [DS yönetim araçlarını yükleyin.][tutorial-create-management-vm]
* Azure AD kiracınızda *Azure AD DC yöneticileri* grubunun üyesi olan bir kullanıcı hesabı.

## <a name="kerberos-constrained-delegation-overview"></a>Kerberos kısıtlı delegasyon genel bakış

Kerberos delegasyonu, bir hesabın kaynaklara erişmek için başka bir hesabın kimliğine bürünmelerine izin verir. Örneğin, arka uç web bileşenine erişen bir web uygulaması, arka uç bağlantısı yaptığında kendisini farklı bir kullanıcı hesabı olarak taklit edebilir. Kerberos delegasyonu, taklit eden hesabın erişebileceği kaynakları sınırlamadığından güvensizdir.

Kerberos kısıtlı delegasyonu (KCD), belirli bir sunucunun veya uygulamanın başka bir kimliği taklit ederken bağlanabileceği hizmetleri veya kaynakları kısıtlar. Geleneksel KCD, bir hizmet için etki alanı hesabını yapılandırmak için etki alanı yöneticisi ayrıcalıkları gerektirir ve hesabın tek bir etki alanında çalışmasını kısıtlar.

Geleneksel KCD de birkaç sorunları vardır. Örneğin, önceki işletim sistemlerinde, hizmet yöneticisinin sahip oldukları kaynak hizmetlerine hangi ön uç hizmetlerinin devredildiğini bilmenin yararlı bir yolu yoktu. Kaynak hizmetine temsilci verebilecek herhangi bir ön uç hizmeti olası bir saldırı noktasıydı. Kaynak hizmetlerine devretmek üzere yapılandırılmış bir ön uç hizmetini barındıran bir sunucu tehlikeye girerse, kaynak hizmetleri de tehlikeye atılabilir.

Azure AD DS yönetilen bir etki alanında etki alanı yöneticisi ayrıcalıklarınız yoktur. Sonuç olarak, geleneksel hesap tabanlı KCD, yönetilen bir etki alanında Azure AD DS'de yapılandırılamaz. Kaynak tabanlı KCD bunun yerine daha güvenli de kullanılabilir.

### <a name="resource-based-kcd"></a>Kaynak tabanlı KCD

Windows Server 2012 ve daha sonra hizmet yöneticilerine hizmetleri için kısıtlı delegasyonu yapılandırma olanağı sağlar. Bu model kaynak tabanlı KCD olarak bilinir. Bu yaklaşımla, arka uç hizmet yöneticisi belirli ön uç hizmetlerinin KCD kullanmasına izin verebilir veya reddedebilir.

Kaynak tabanlı KCD PowerShell kullanılarak yapılandırılır. Kimlik taklidi yapılan hesabın bir bilgisayar hesabı mı yoksa kullanıcı hesabı / hizmet hesabı mı olduğuna bağlı olarak [Set-ADComputer][Set-ADComputer] veya [Set-ADUser][Set-ADUser] cmdlets'i kullanırsınız.

## <a name="configure-resource-based-kcd-for-a-computer-account"></a>Bir bilgisayar hesabı için kaynak tabanlı KCD yapılandırma

Bu senaryoda, *contoso-webapp.aaddscontoso.com*adlı bilgisayarda çalışan bir web uygulamanız olduğunu varsayalım. Web uygulamasının etki alanı kullanıcıları bağlamında *contoso-api.aaddscontoso.com* adlı bilgisayarda çalışan bir web API'sine erişmesi gerekir. Bu senaryoyu yapılandırmak için aşağıdaki adımları tamamlayın:

1. [Özel bir OU oluşturun.](create-ou.md) Bu özel OU'yu yönetmek için izinleri Azure AD DS yönetilen etki alanındaki kullanıcılara devredebilirsiniz.
1. Hem web uygulamasını çalıştıran hem de web API'sini çalıştıran [sanal makinelere][create-join-windows-vm]Azure AD DS yönetilen etki alanına katılın. Bu bilgisayar hesaplarını önceki adımdaki özel OU'da oluşturun.

    > [!NOTE]
    > Web uygulaması nın bilgisayar hesapları ve web API'si, kaynak tabanlı KCD'yi yapılandırma izinlerine sahip olduğunuz özel bir OU'da olmalıdır. Yerleşik *AAD DC Bilgisayar* kapsayıcısındaki bir bilgisayar hesabı için kaynak tabanlı KCD yapılandıramazsınız.

1. Son olarak, [Set-ADComputer][Set-ADComputer] PowerShell cmdlet kullanarak kaynak tabanlı KCD yapılandırın. Etki alanına katılan yönetim VM'nizden ve *Azure AD DC yöneticileri* grubunun üyesi olan kullanıcı hesabı olarak oturum açın, aşağıdaki cmdlets'i çalıştırın. Gerektiğinde kendi bilgisayar adlarınızı sağlayın:
    
    ```powershell
    $ImpersonatingAccount = Get-ADComputer -Identity contoso-webapp.aaddscontoso.com
    Set-ADComputer contoso-api.aaddscontoso.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
    ```

## <a name="configure-resource-based-kcd-for-a-user-account"></a>Bir kullanıcı hesabı için kaynak tabanlı KCD yapılandırma

Bu senaryoda, *appsvc*adlı bir hizmet hesabı olarak çalışan bir web uygulamanız olduğunu varsayalım. Web uygulamasının etki alanı kullanıcıları bağlamında *backendsvc* adlı bir hizmet hesabı olarak çalışan bir web API'sine erişmesi gerekir. Bu senaryoyu yapılandırmak için aşağıdaki adımları tamamlayın:

1. [Özel bir OU oluşturun.](create-ou.md) Bu özel OU'yu yönetmek için izinleri Azure AD DS yönetilen etki alanındaki kullanıcılara devredebilirsiniz.
1. Arka uç web API'sını/kaynağını Azure AD DS yönetilen etki alanına çalıştıran [sanal makinelere katılın.][create-join-windows-vm] Özel OU içinde bilgisayar hesabını oluşturun.
1. Web uygulamasını özel OU içinde çalıştırmak için kullanılan hizmet hesabını (örneğin, 'appsvc') oluşturun.

    > [!NOTE]
    > Yine, web API VM için bilgisayar hesabı ve web uygulaması için hizmet hesabı, kaynak tabanlı KCD yapılandırmak için izinleri var özel bir OU olmalıdır. Yerleşik *AAD DC Bilgisayarlardaki* veya *AAD DC Users* kapsayıcılarında bulunan hesaplar için kaynak tabanlı KCD yapılandıramazsınız. Bu, kaynak tabanlı KCD ayarlamak için Azure AD'den eşitlenen kullanıcı hesaplarını kullanamadığınız anlamına da gelir. Azure AD DS'de özel olarak oluşturulmuş hizmet hesapları oluşturmalı ve kullanmalısınız.

1. Son olarak, [Set-ADUser][Set-ADUser] PowerShell cmdlet kullanarak kaynak tabanlı KCD yapılandırın. Etki alanına katılan yönetim VM'nizden ve *Azure AD DC yöneticileri* grubunun üyesi olan kullanıcı hesabı olarak oturum açın, aşağıdaki cmdlets'i çalıştırın. Gerektiğinde kendi hizmet adlarınızı sağlayın:

    ```powershell
    $ImpersonatingAccount = Get-ADUser -Identity appsvc
    Set-ADUser backendsvc -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
    ```

## <a name="next-steps"></a>Sonraki adımlar

Etkin Dizin Etki Alanı Hizmetlerinde temsilcinin nasıl çalıştığı hakkında daha fazla bilgi edinmek için [Kerberos Kısıtlı Delegasyona Genel Bakış'a][kcd-technet]bakın.

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[create-join-windows-vm]: join-windows-vm.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[Set-ADComputer]: /powershell/module/addsadministration/set-adcomputer
[Set-ADUser]: /powershell/module/addsadministration/set-aduser

<!-- EXTERNAL LINKS -->
[kcd-technet]: https://technet.microsoft.com/library/jj553400.aspx
