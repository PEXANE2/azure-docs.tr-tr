---
title: Azure AD Etki Alanı Hizmetleri için Azure AD Uygulama Proxy'yi dağıtma | Microsoft Dokümanlar
description: Azure Active Directory Application Services yönetilen etki alanında Azure Active Directory Application Proxy'yi dağıtarak ve yapılandırarak uzak çalışanlar için dahili uygulamalara nasıl güvenli erişim sağlayacağınızı öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/31/2020
ms.author: iainfou
ms.openlocfilehash: c1dc5216f758c2dda263e2f61b043dbde5f76604
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80655498"
---
# <a name="deploy-azure-ad-application-proxy-for-secure-access-to-internal-applications-in-an-azure-ad-domain-services-managed-domain"></a>Azure AD Etki Alanı Hizmetleri yönetilen etki alanında dahili uygulamalara güvenli erişim için Azure AD Uygulama Proxy'sini dağıtma

Azure AD Etki Alanı Hizmetleri (Azure AD DS) ile, şirket içinde çalışan eski uygulamaları Azure'a taşıyıp kaydırabilirsiniz. Azure Active Directory (AD) Application Proxy, daha sonra bu dahili uygulamaları Azure AD DS yönetilen etki alanının bir kısmını güvenli bir şekilde yayımlayarak uzak çalışanları desteklemenize yardımcı olur, böylece bunlara internet üzerinden erişilebilmektedir.

Azure AD Uygulama Proxy'de yeniyseniz ve daha fazla bilgi edinmek istiyorsanız, [dahili uygulamalara nasıl güvenli uzaktan erişim sağlayacağınız bilgisini](../active-directory/manage-apps/application-proxy.md)edinin.

Bu makalede, Azure AD DS yönetilen etki alanında uygulamalara güvenli erişim sağlamak için bir Azure AD Application Proxy bağlayıcısı nasıl oluşturabileceğinizi ve yapılandırabileceğiniz gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
    * Azure AD Uygulama Proxy'sini kullanmak için **Azure AD Premium lisansı** gereklidir.
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, [bir Azure Etkin Dizin Etki Alanı Hizmetleri örneği oluşturun ve yapılandırın.][create-azure-ad-ds-instance]

## <a name="create-a-domain-joined-windows-vm"></a>Etki alanına birleştirilmiş Windows VM oluşturma

Trafiği ortamınızda çalışan uygulamalara yönlendirmek için Azure AD Application Proxy bağlayıcısı bileşenini yüklersiniz. Bu Azure AD Application Proxy bağlayıcısı, Azure AD DS yönetilen etki alanına katılan Windows Server sanal makinelerine (VM) yüklenmiş olmalıdır. Bazı uygulamalar için, her bağlayıcı yüklü olan birden çok sunucu dağıtabilirsiniz. Bu dağıtım seçeneği size daha fazla kullanılabilirlik sağlar ve daha ağır kimlik doğrulama yüklerini işlemeye yardımcı olur.

Azure AD Application Proxy bağlayıcısını çalıştıran VM, Azure AD DS'yi etkinleştirdiğiniz aynı veya eşlenmiş sanal ağda olmalıdır. Uygulama Proxy'sini kullanarak yayımladığınız uygulamaları barındıran SANAL M'lerin de aynı Azure sanal ağında dağıtılması gerekir.

Azure AD Application Proxy bağlayıcısı için bir VM oluşturmak için aşağıdaki adımları tamamlayın:

1. [Özel bir OU oluşturun.](create-ou.md) Bu özel OU'yu yönetmek için izinleri Azure AD DS yönetilen etki alanındaki kullanıcılara devredebilirsiniz. Azure AD Uygulama Proxy'si için ve uygulamalarınızı çalıştıran VM'ler varsayılan *AAD DC Computers* OU'nun değil, özel OU'nun bir parçası olmalıdır.
1. Hem Azure AD Application Proxy bağlayıcısını çalıştıran [sanal makinelere][create-join-windows-vm]hem de uygulamalarınızı çalıştıranlara Azure AD DS yönetilen etki alanına katılın. Bu bilgisayar hesaplarını önceki adımdaki özel OU'da oluşturun.

## <a name="download-the-azure-ad-application-proxy-connector"></a>Azure AD Application Proxy bağlayıcısını indirin

Azure AD Application Proxy bağlayıcısını indirmek için aşağıdaki adımları gerçekleştirin. İndirdiğiniz kurulum dosyası sonraki bölümde App Proxy VM'nize kopyalanır.

1. Azure AD'de *Kurumsal yönetici* izinlerine sahip bir kullanıcı hesabıyla [Azure portalında](https://portal.azure.com) oturum açın.
1. Portalın üst kısmında **Azure Active Directory'yi** arayın ve seçin ve ardından **Kurumsal uygulamaları**seçin.
1. Sol taraftaki menüden **Uygulama proxy'sini** seçin. İlk bağlayıcınızı oluşturmak ve App Proxy'yi etkinleştirmek için **bir bağlayıcı indirmek**için bağlantıyı seçin.
1. İndirme sayfasında, lisans koşullarını ve gizlilik anlaşmasını kabul edin, ardından **İndirme koşulları & kabul et'i**seçin.

    ![Azure AD App Proxy bağlayıcısını indirin](./media/app-proxy/download-app-proxy-connector.png)

## <a name="install-and-register-the-azure-ad-application-proxy-connector"></a>Azure AD Application Proxy bağlayıcısını yükleyin ve kaydettirin

Azure AD Application Proxy bağlayıcısı olarak kullanılmaya hazır bir VM ile Azure portalından indirilen kurulum dosyasını kopyalayın ve çalıştırın.

1. Azure AD Application Proxy bağlayıcısı kurulum dosyasını VM'nize kopyalayın.
1. *AADApplicationProxyConnectorInstaller.exe*gibi kurulum dosyasını çalıştırın. Yazılım lisans koşullarını kabul edin.
1. Yükleme sırasında, bağlayıcıyı Azure AD dizininizde Uygulama Proxy'sine kaydetmeniz istenir.
   * Azure AD dizininizde küresel bir yöneticinin kimlik bilgilerini sağlayın. Azure AD genel yönetici kimlik bilgileri, portaldaki Azure kimlik bilgilerinizden farklı olabilir

        > [!NOTE]
        > Bağlayıcıyı kaydetmek için kullanılan genel yönetici hesabı, Uygulama Proxy hizmetini etkinleştirdiğiniz aynı dizine ait olmalıdır.
        >
        > Örneğin, Azure AD etki alanı *aaddscontoso.com*ise, `admin@aaddscontoso.com` genel yönetici nin bu etki alanında geçerli bir diğer ad olması gerekir.

   * Bağlayıcıyı yüklediğiniz VM için Internet Explorer Gelişmiş Güvenlik Yapılandırması açıksa, kayıt ekranı engellenebilir. Erişime izin vermek için, hata iletisindeki yönergeleri izleyin veya yükleme işlemi sırasında Internet Explorer Gelişmiş Güvenliği kapatın.
   * Bağlayıcı kaydı başarısız olursa, [Sorun Giderme Uygulama](../active-directory/manage-apps/application-proxy-troubleshoot.md)Proxy'si'ne bakın.
1. Kurulumun sonunda, giden proxy'si olan ortamlar için bir not gösterilir. Azure AD Application Proxy bağlayıcısını giden proxy üzerinden çalışacak şekilde yapılandırmak için `C:\Program Files\Microsoft AAD App Proxy connector\ConfigureOutBoundProxy.ps1`sağlanan komut dosyasını çalıştırın.
1. Azure portalındaki Uygulama proxy sayfasında, yeni bağlayıcı aşağıdaki örnekte gösterildiği gibi *Etkin*durumuyla listelenir:

    ![Azure portalında etkin olarak gösterilen yeni Azure AD Uygulama Proxy bağlayıcısı](./media/app-proxy/connected-app-proxy.png)

> [!NOTE]
> Azure AD Application Proxy üzerinden kimlik doğrulaması yapılan uygulamalar için yüksek kullanılabilirlik sağlamak için, birden çok VM'ye bağlayıcı yükleyebilirsiniz. Azure AD DS yönetilen etki alanına katılan diğer sunuculara konektör yüklemek için önceki bölümde listelenen aynı adımları yineleyin.

## <a name="enable-resource-based-kerberos-constrained-delegation"></a>Kaynak tabanlı Kerberos kısıtlı delegasyonu etkinleştirme

Tümleşik Windows Kimlik Doğrulama (IWA) kullanarak uygulamalarınızda tek oturum açma kullanmak istiyorsanız, Azure AD Application Proxy bağlayıcılarına kullanıcıların kimliğine bürünme ve onların adına jeton gönderip alma izni ver. Bu izinleri vermek için, bağlayıcının Azure AD DS yönetilen etki alanındaki kaynaklara erişmeleri için Kerberos kısıtlanmış delegasyonu (KCD) yapılandırırsınız. Azure AD DS yönetilen etki alanında etki alanı yöneticisi ayrıcalıklarınız olmadığı için, geleneksel hesap düzeyi KCD yönetilen bir etki alanında yapılandırılamaz. Bunun yerine, kaynak tabanlı KCD kullanın.

Daha fazla bilgi için Azure [Active Directory Domain Services'da Kerberos sınırlandırılan temsilciliği (KCD) yapılandırın.](deploy-kcd.md)

> [!NOTE]
> Aşağıdaki PowerShell cmdlets'i çalıştırmak için *Azure AD DC yöneticileri* grubuna üye olan bir kullanıcı hesabında oturum açmış olmalısınız.
>
> App Proxy bağlayıcınız VM'nizin bilgisayar hesapları ve uygulama VM'leri, kaynak tabanlı KCD yapılandırma izinlerinizin olduğu özel bir OU'da olmalıdır. Yerleşik *AAD DC Bilgisayar* kapsayıcısındaki bir bilgisayar hesabı için kaynak tabanlı KCD yapılandıramazsınız.

Azure AD Application Proxy bağlayıcısının yüklü olduğu bilgisayarın ayarlarını almak için [Get-ADComputer'ı][Get-ADComputer] kullanın. Etki alanına katılan yönetim VM'nizden ve *Azure AD DC yöneticileri* grubunun üyesi olan kullanıcı hesabı olarak oturum açın, aşağıdaki cmdlets'i çalıştırın.

Aşağıdaki örnek, *appproxy.aaddscontoso.com*adlı bilgisayar hesabı hakkında bilgi alır. Önceki adımlarda yapılandırılan Azure AD Application Proxy VM için kendi bilgisayar adınızı sağlayın.

```powershell
$ImpersonatingAccount = Get-ADComputer -Identity appproxy.aaddscontoso.com
```

Azure AD Application Proxy'nin arkasındaki uygulamaları çalıştıran her uygulama sunucusu için, kaynak tabanlı KCD'yi yapılandırmak için [Set-ADComputer][Set-ADComputer] PowerShell cmdlet'i kullanın. Aşağıdaki örnekte, Azure AD Uygulama Proxy bağlayıcısına *appserver.aaddscontoso.com* bilgisayarı kullanma izni verilir:

```powershell
Set-ADComputer appserver.aaddscontoso.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

Birden çok Azure AD Application Proxy bağlayıcısı dağıtıyorsanız, her bağlayıcı örneği için kaynak tabanlı KCD yapılandırmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD DS ile tümleşik Azure AD Uygulama Proxy ile kullanıcıların erişebilmek için uygulamaları yayımlayın. Daha fazla bilgi için Azure [AD Application Proxy kullanarak uygulama yayımlama'ya](../active-directory/manage-apps/application-proxy-publish-azure-portal.md)bakın.

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[create-join-windows-vm]: join-windows-vm.md
[azure-bastion]: ../bastion/bastion-create-host-portal.md
[Get-ADComputer]: /powershell/module/addsadministration/get-adcomputer
[Set-ADComputer]: /powershell/module/addsadministration/set-adcomputer
