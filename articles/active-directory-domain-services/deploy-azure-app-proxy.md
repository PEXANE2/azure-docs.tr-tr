---
title: 'Azure Active Directory Domain Services: Azure AD Uygulama Ara Sunucusu Dağıtma | Microsoft Docs'
description: Azure Active Directory Domain Services yönetilen etki alanlarında Azure AD Uygulama Ara Sunucusu kullanma
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
ms.date: 05/14/2019
ms.author: iainfou
ms.openlocfilehash: b3fb5d665380bc15b61d3b4b7913a992915f9afb
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68853837"
---
# <a name="deploy-azure-ad-application-proxy-on-an-azure-ad-domain-services-managed-domain"></a>Azure AD Uygulama Ara Sunucusu Azure AD Domain Services yönetilen bir etki alanında dağıtma
Azure Active Directory (AD) uygulama proxy 'Si, internet üzerinden erişilecek şirket içi uygulamaları yayımlayarak uzak çalışanları desteklemenize yardımcı olur. Azure AD Domain Services ile, artık şirket içinde çalışan eski uygulamaları Azure altyapı hizmetleri 'ne açabilir ve taşıyabilirsiniz. Böylece, kuruluşunuzdaki kullanıcılara güvenli uzaktan erişim sağlamak için Azure AD Uygulama Ara Sunucusu kullanarak bu uygulamaları yayımlayabilirsiniz.

Azure AD Uygulama Ara Sunucusu yeni başladıysanız, bu özellik hakkında daha fazla bilgi edinmek için aşağıdaki makaleye gidin: Şirket [içi uygulamalara güvenli uzaktan erişim sağlama](../active-directory/manage-apps/application-proxy.md).

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için şunlar gerekir:

1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure ad dizini** -şirket içi bir dizinle veya yalnızca bulut diziniyle eşitlenir.
3. Azure AD Uygulama Ara Sunucusu kullanmak için bir **Azure AD temel veya Premium lisans** gerekir.
4. Azure AD dizini için **Azure AD Domain Services** etkinleştirilmelidir. Bunu yapmadıysanız, [Başlarken kılavuzunda](create-instance.md)özetlenen tüm görevleri izleyin.

<br>

## <a name="task-1---enable-azure-ad-application-proxy-for-your-azure-ad-directory"></a>Görev 1-Azure AD dizininiz için Azure AD Uygulama Ara Sunucusu Etkinleştirme
Azure AD dizininiz için Azure AD Uygulama Ara Sunucusu etkinleştirmek üzere aşağıdaki adımları gerçekleştirin.

1. [Azure Portal](https://portal.azure.com)yönetici olarak oturum açın.

2. Dizine genel bakışı getirmek için **Azure Active Directory** ' a tıklayın. **Kurumsal uygulamalar**' a tıklayın.

3. **Uygulama proxy 'si**' ne tıklayın.

4. Bağlayıcıyı indirmek için **bağlayıcı** düğmesine tıklayın.

5. İndirme sayfasında, lisans koşullarını ve gizlilik sözleşmesi 'ni kabul edin ve **İndir** düğmesine tıklayın.

    ![İndirmeyi Onayla](./media/app-proxy/app-proxy-enabled-confirm-download.png)


## <a name="task-2---provision-domain-joined-windows-servers-to-deploy-the-azure-ad-application-proxy-connector"></a>Görev 2-Azure AD Uygulama Ara Sunucusu bağlayıcısını dağıtmak için etki alanına katılmış Windows Server sağlama
Azure AD Uygulama Ara Sunucusu bağlayıcısını yükleyebileceğiniz, etki alanına katılmış Windows Server sanal makinelerine ihtiyacınız vardır. Bazı uygulamalarda, bağlayıcının yüklendiği birden çok sunucu sağlamayı tercih edebilirsiniz. Bu dağıtım seçeneği, daha fazla kullanılabilirlik sağlar ve daha ağır kimlik doğrulama yüklerini işlemeye yardımcı olur.

Azure AD Domain Services yönetilen etki alanınızı etkinleştirdiğiniz aynı sanal ağa (veya bağlı/eşlenmiş bir sanal ağ) bağlayıcı sunucuları sağlayın. Benzer şekilde, uygulama proxy 'Si aracılığıyla yayımladığınız uygulamaları barındıran sunucuların aynı Azure sanal ağına yüklenmesi gerekir.

Bağlayıcı sunucuları sağlamak için, [Windows sanal makinesini yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md)başlıklı makalede özetlenen görevleri izleyin.


## <a name="task-3---install-and-register-the-azure-ad-application-proxy-connector"></a>Görev 3-Azure AD Uygulama Ara Sunucusu bağlayıcısını yükleyip kaydetme
Daha önce, bir Windows Server sanal makinesi sağladınız ve yönetilen etki alanına katıldınız. Bu görevde Azure AD Uygulama Ara Sunucusu bağlayıcısını bu sanal makineye yüklersiniz.

1. Bağlayıcı yükleme paketini Azure AD Web uygulaması proxy bağlayıcısını yüklediğiniz VM 'ye kopyalayın.

2. Sanal makinede **Aadapplicationproxyconnectorınstaller. exe** ' yi çalıştırın. Yazılım lisans koşulları 'nı kabul edin.

    ![Yüklenmek üzere koşulları kabul et](./media/app-proxy/app-proxy-install-connector-terms.png)
3. Yükleme sırasında, bağlayıcıyı Azure AD dizininizin uygulama proxy 'Si ile kaydetmeniz istenir.
   * **Azure AD uygulama Yöneticisi kimlik bilgilerinizi**sağlayın. Uygulama Yöneticisi kiracınız Microsoft Azure kimlik bilgilerinizle farklı olabilir.
   * Bağlayıcıyı kaydetmek için kullanılan yönetici hesabı, uygulama ara sunucusu hizmetini etkinleştirdiğiniz dizine ait olmalıdır. Örneğin, kiracı etki alanı contoso.com ise, yönetici admin@contoso.com bu etki alanında veya diğer geçerli bir diğer ad olmalıdır.
   * Bağlayıcıyı yüklemekte olduğunuz sunucu için IE artırılmış güvenlik yapılandırması açıksa, kayıt ekranı engellenebilir. Erişime izin vermek için hata iletisindeki yönergeleri uygulayın. Internet Explorer Artırılmış Güvenlik seçeneğinin devre dışı olduğundan emin olun.
   * Bağlayıcı kaydı başarısız olursa bkz. [Uygulama Proxy’si Sorunlarını Giderme](../active-directory/manage-apps/application-proxy-troubleshoot.md).

     ![Bağlayıcı yüklendi](./media/app-proxy/app-proxy-connector-installed.png)
4. Bağlayıcının düzgün çalıştığından emin olmak için Azure AD Uygulama Ara Sunucusu Bağlayıcısı sorun gidericisini çalıştırın. Sorun gidericiyi çalıştırdıktan sonra başarılı bir rapor görmeniz gerekir.

    ![Sorun giderici başarısı](./media/app-proxy/app-proxy-connector-troubleshooter.png)
5. Yeni yüklenen bağlayıcıyı Azure AD dizininizin uygulama proxy 'si sayfasında görmeniz gerekir.

    ![Azure portal, yüklü bağlayıcı kullanılabilir olarak gösterilir](./media/app-proxy/app-proxy-connector-page.png)

> [!NOTE]
> Azure AD Uygulama Ara Sunucusu aracılığıyla yayımlanan uygulamaların kimlik doğrulaması için yüksek kullanılabilirlik sağlamak üzere bağlayıcıları birden çok sunucuya yüklemeyi tercih edebilirsiniz. Bağlayıcıyı, yönetilen etki alanına katılmış diğer sunuculara yüklemek için yukarıda listelenen adımların aynısını gerçekleştirin.
>
>

## <a name="next-steps"></a>Sonraki Adımlar
Azure AD Uygulama Ara Sunucusu ayarlamış ve Azure AD Domain Services yönetilen etki alanınız ile tümleştirmiş olabilirsiniz.

* **Uygulamalarınızı Azure sanal makinelerine geçirin:** Şirket içi sunuculardan uygulamalarınızı, yönetilen etki alanına katılmış Azure sanal makinelerine açabilir ve taşıyabilirsiniz. Bunun yapılması, şirket içi sunucuları çalıştırmanın altyapı maliyetlerinden kurtuya yardımcı olur.

* **Azure AD Uygulama Ara Sunucusu kullanarak uygulama yayımlama:** Azure AD Uygulama Ara Sunucusu kullanarak Azure sanal makinelerinizde çalışan uygulamalar yayımlayın. Daha fazla bilgi için bkz. [Azure kullanarak uygulama yayımlama ad uygulama ara sunucusu](../active-directory/manage-apps/application-proxy-publish-azure-portal.md)


## <a name="deployment-note---publish-iwa-integrated-windows-authentication-applications-using-azure-ad-application-proxy"></a>Dağıtım notunun-Azure AD Uygulama Ara Sunucusu kullanarak ıWA (tümleşik Windows kimlik doğrulaması) uygulamalarını yayımlayın
Uygulama ara sunucusu bağlayıcılarını kullanıcılara taklit etmek için izin vererek ve kendi adına belirteçler alıp alarak tümleşik Windows kimlik doğrulaması (ıWA) kullanarak uygulamalarınızda çoklu oturum açmayı etkinleştirin. Yönetilen etki alanındaki kaynaklara erişmek için gerekli izinleri vermek üzere bağlayıcı için Kerberos kısıtlanmış temsilcisini (KCD) yapılandırın. Daha yüksek güvenlik için yönetilen etki alanlarında kaynak tabanlı KCD mekanizmasını kullanın.


### <a name="enable-resource-based-kerberos-constrained-delegation-for-the-azure-ad-application-proxy-connector"></a>Azure AD Uygulama Ara Sunucusu Bağlayıcısı için kaynak tabanlı Kerberos kısıtlı temsilcisini etkinleştirme
Azure uygulama proxy Bağlayıcısı, Kerberos kısıtlanmış temsil (KCD) için yapılandırılmalıdır, bu nedenle yönetilen etki alanındaki kullanıcıların kimliğine bürünebilir. Azure AD Domain Services yönetilen bir etki alanında etki alanı yöneticisi ayrıcalıklarına sahip değilsiniz. Bu nedenle, **yönetilen bir etki alanında geleneksel hesap düzeyi KCD yapılandırılamaz**.

Bu [makalede](deploy-kcd.md)açıklandığı gibi kaynak tabanlı KCD 'yi kullanın.

> [!NOTE]
> AD PowerShell cmdlet 'lerini kullanarak yönetilen etki alanını yönetmek için ' AAD DC Administrators ' grubunun bir üyesi olmanız gerekir.
>
>

Azure AD Uygulama Ara Sunucusu bağlayıcısının yüklü olduğu bilgisayarın ayarlarını almak için Get-ADComputer PowerShell cmdlet 'ini kullanın.
```powershell
$ConnectorComputerAccount = Get-ADComputer -Identity contoso100-proxy.contoso100.com
```

Bundan sonra, Resource Server için kaynak tabanlı KCD 'yi ayarlamak için set-ADComputer cmdlet 'ini kullanın.
```powershell
Set-ADComputer contoso100-resource.contoso100.com -PrincipalsAllowedToDelegateToAccount $ConnectorComputerAccount
```

Yönetilen etki alanında birden çok uygulama proxy Bağlayıcısı dağıttıysanız, bu tür bağlayıcı örnekleri için kaynak tabanlı KCD 'yi yapılandırmanız gerekir.


## <a name="related-content"></a>İlgili İçerik
* [Azure AD Domain Services-Başlarken Kılavuzu](create-instance.md)
* [Yönetilen bir etki alanında Kerberos kısıtlanmış temsilciyi yapılandırma](deploy-kcd.md)
* [Kerberos kısıtlı temsilciye genel bakış](https://technet.microsoft.com/library/jj553400.aspx)
