---
title: Azure AD Etki Alanı Hizmetlerinde güvenli uzaktan VM erişimi | Microsoft Dokümanlar
description: Bir Azure Active Directory Etki Alanı Hizmetleri yönetilen etki alanında Uzak Masaüstü Hizmetleri dağıtımıyla Ağ İlkesi Sunucusu (NPS) ve Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanarak VM'lere uzaktan erişimi nasıl sağlayacağınızı öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: f2a222c6a382fa2c316211e293547780a8778177
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80423151"
---
# <a name="secure-remote-access-to-virtual-machines-in-azure-active-directory-domain-services"></a>Azure Active Directory Etki Alanı Hizmetlerinde sanal makinelere uzaktan erişimi güvenli hale

Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS) yönetilen etki alanında çalışan sanal makinelere (VM) uzaktan erişimi sağlamak için Uzak Masaüstü Hizmetleri (RDS) ve Ağ İlkesi Sunucusu'nu (NPS) kullanabilirsiniz. Azure AD DS, kullanıcıların RDS ortamı üzerinden erişim istediklerinde kimlik doğrulamasını sağlar. Gelişmiş güvenlik için, oturum açma etkinlikleri sırasında ek bir kimlik doğrulama istemi sağlamak için Azure Çok Faktörlü Kimlik Doğrulaması'nı tümleştirebilirsiniz. Azure Çok Faktörlü Kimlik Doğrulama, bu özelliği sağlamak için NPS için bir uzantı kullanır.

> [!IMPORTANT]
> Azure AD DS yönetilen bir etki alanında Sanal M'lerinize güvenli bir şekilde bağlanmanın önerilen yolu, sanal ağınızda sağladığınız tam platform tarafından yönetilen bir PaaS hizmeti olan Azure Bastion'u kullanmaktır. Bir burç ana bilgisayar, Doğrudan SSL üzerinden Azure portalında VM'lerinize güvenli ve sorunsuz Uzak Masaüstü Protokolü (RDP) bağlantısı sağlar. Bir burç ana bilgisayarı üzerinden bağlandığınızda, VM'lerinizin ortak bir IP adresine ihtiyacı yoktur ve TCP bağlantı noktası 3389'da RDP'ye erişimi ortaya çıkarmak için ağ güvenlik gruplarını kullanmanız gerekmez.
>
> Azure Kalesi'ni desteklendiği tüm bölgelerde kullanmanızı şiddetle öneririz. Azure Bastion kullanılabilirliği olmayan bölgelerde, Azure Bastion kullanılabilir olana kadar bu makalede ayrıntılı olarak belirtilen adımları izleyin. Tüm gelen RDP trafiğine izin verilen Azure AD DS'ye katılan VM'lere genel IP adresleri atamaya özen kaydedin.
>
> Daha fazla bilgi için [bkz: Azure Kalesi nedir?][bastion-overview]

Bu makalede, Azure AD DS'de RDS'yi nasıl yapılandırabileceğiniz ve isteğe bağlı olarak Azure Çok Faktörlü Kimlik Doğrulama NPS uzantısını nasıl kullanacağınızı gösterilmektedir.

![Uzak Masaüstü Hizmetleri (RDS) genel bakış](./media/enable-network-policy-server/remote-desktop-services-overview.png)

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için aşağıdaki kaynaklara ihtiyacınız var:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, [bir Azure Etkin Dizin Etki Alanı Hizmetleri örneği oluşturun ve yapılandırın.][create-azure-ad-ds-instance]
* Azure Active Directory Etki Alanı Hizmetleri sanal ağınızda oluşturulan *iş yükleri* alt ağı.
    * Gerekirse, [Azure Etkin Dizin Etki Alanı Hizmetleri yönetilen etki alanı için sanal ağ yapılandırma.][configure-azureadds-vnet]
* Azure AD kiracınızda *Azure AD DC yöneticileri* grubunun üyesi olan bir kullanıcı hesabı.

## <a name="deploy-and-configure-the-remote-desktop-environment"></a>Uzak Masaüstü ortamını dağıtma ve yapılandırma

Başlamak için, Windows Server 2016 veya Windows Server 2019 çalıştıran en az iki Azure VM'si oluşturun. Uzak Masaüstü (RD) ortamınızın fazlalığı ve yüksek kullanılabilirliği için, daha sonra ek ana bilgisayarlar ekleyebilir ve yükleyebilirsiniz.

Önerilen BIR RDS dağıtımı aşağıdaki iki VM'yi içerir:

* *RDGVM01* - RD Connection Broker sunucusu, RD Web Access sunucusu ve RD Ağ Geçidi sunucusu çalışır.
* *RDSHVM01* - RD Session Host sunucusunu çalıştırın.

Sanal M'lerin Azure AD DS sanal ağınızın *iş yükü* alt ağına dağıtıldığından emin olun ve ardından Azure AD DS yönetilen etki alanına VM'lere katılın. Daha fazla bilgi için, [Bir Windows Server VM'yi Azure AD DS yönetilen etki alanına nasıl oluşturup katılacağınıza][tutorial-create-join-vm]bakın.

RD ortamı dağıtımı birkaç adım içerir. Mevcut RD dağıtım kılavuzu, Azure AD DS yönetilen bir etki alanında kullanılacak belirli bir değişiklik olmadan kullanılabilir:

1. *Contosoadmin*gibi *Azure AD DC Yöneticileri* grubunun bir parçası olan bir hesapla RD ortamı için oluşturulan VM'lerde oturum açın.
1. RDS oluşturmak ve yapılandırmak için varolan [Uzak Masaüstü ortamı dağıtım kılavuzunu][deploy-remote-desktop]kullanın. RD sunucu bileşenlerini istediğiniz gibi Azure VM'lerinize dağıtın.
1. Bir web tarayıcısı kullanarak erişim sağlamak istiyorsanız, [kullanıcılarınız için Uzak Masaüstü web istemcisini ayarlayın.][rd-web-client]

RD'nin Azure AD DS yönetilen etki alanına dağıtılmasıyla, hizmeti şirket içinde bir AD DS etki alanında olduğu gibi yönetebilir ve kullanabilirsiniz.

## <a name="deploy-and-configure-nps-and-the-azure-mfa-nps-extension"></a>NPS ve Azure MFA NPS uzantısını dağıtma ve yapılandırma

Kullanıcı oturum açma deneyiminin güvenliğini artırmak istiyorsanız, AR-Ge ortamını isteğe bağlı olarak Azure Çok Faktörlü Kimlik Doğrulaması ile tümleştirebilirsiniz. Bu yapılandırmaile kullanıcılar, oturum açma sırasında kimliklerini doğrulamak için ek bir istem alır.

Bu özelliği sağlamak için, Azure Çok Faktörlü Kimlik Doğrulama NPS uzantısı ile birlikte ortamınıza ek bir Ağ İlkesi Sunucusu (NPS) yüklenir. Bu uzantı, çok faktörlü kimlik doğrulama istemlerinin durumunu istemek ve döndürmek için Azure AD ile tümleşir.

Kullanıcıların ek Azure AD lisansları gerektirebilecek [Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmak için kaydolması][user-mfa-registration]gerekir.

Azure Çok Faktörlü Kimlik Doğrulaması'nı Azure AD DS Uzak Masaüstü ortamınıza entegre etmek için bir NPS Sunucusu oluşturun ve uzantıyı yükleyin:

1. Azure AD DS sanal ağınızdaki *iş yükü* alt ağına bağlı *NPSVM01*gibi ek bir Windows Server 2016 veya 2019 VM oluşturun. Azure AD DS yönetilen etki alanına VM'ye katılın.
1. *Contosoadmin*gibi *Azure AD DC Yöneticileri* grubunun bir parçası olan hesap olarak NPS VM'de oturum açın.
1. **Server**Manager'dan, Rol **ve Özellikler Ekle'yi**seçin, ardından *Ağ İlkesi ve Erişim Hizmetleri* rolünü yükleyin.
1. [Azure MFA NPS uzantısını yüklemek ve yapılandırmak][nps-extension]için varolan nasıl yapılandırılır makalesini kullanın.

NPS sunucusu ve Azure Çok Faktörlü Kimlik Doğrulama NPS uzantısı yüklü olduğundan, RD ortamında kullanılmak üzere yapılandırmak için bir sonraki bölümü tamamlayın.

## <a name="integrate-remote-desktop-gateway-and-azure-multi-factor-authentication"></a>Uzak Masaüstü Ağ Geçidi ni ve Azure Çok Faktörlü Kimlik Doğrulamayı Tümleştir

Azure Çok Faktörlü Kimlik Doğrulama NPS uzantısını tümleştirmek için, [Ağ İlkesi Sunucusu (NPS) uzantısı ve Azure AD'yi kullanarak Uzak Masaüstü Ağ Geçidi altyapınızı tümleştirmek][azure-mfa-nps-integration]için varolan nasıl yapılır makalesini kullanın.

Azure AD DS yönetilen etki alanıyla tümleştirmek için aşağıdaki ek yapılandırma seçenekleri gereklidir:

1. [NPS sunucusunu Active Directory'ye kaydettirme.][register-nps-ad] Bu adım, Azure AD DS yönetilen etki alanında başarısız olur.
1. [Ağ ilkesini yapılandırmak için 4 numaralı adımda,][create-nps-policy] **kullanıcı hesabı arama özelliklerini yoksaymak**için kutuyu da işaretleyin.
1. NPS sunucusu ve Azure Çok Faktörlü Kimlik Doğrulama NPS uzantısı için Windows Server 2019 kullanıyorsanız, NPS sunucusunun doğru iletişim kurabilmesi için güvenli kanalı güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```powershell
    sc sidtype IAS unrestricted
    ```

Kullanıcılardan artık oturum açtıklarında Microsoft Authenticator uygulamasındakısa mesaj veya istem gibi ek bir kimlik doğrulama faktörü istenir.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtımınızın esnekliğini artırma hakkında daha fazla bilgi için [Uzak Masaüstü Hizmetleri - Yüksek kullanılabilirlik bölümüne][rds-high-availability]bakın.

Kullanıcı oturum açma güvenliğini sağlama hakkında daha fazla bilgi için [bkz: Azure Çok Faktörlü Kimlik Doğrulama.][concepts-mfa]

<!-- INTERNAL LINKS -->
[bastion-overview]: ../bastion/bastion-overview.md
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[configure-azureadds-vnet]: tutorial-configure-networking.md
[tutorial-create-join-vm]: join-windows-vm.md
[user-mfa-registration]: ../active-directory/authentication/howto-mfa-nps-extension.md#register-users-for-mfa
[nps-extension]: ../active-directory/authentication/howto-mfa-nps-extension.md
[azure-mfa-nps-integration]: ../active-directory/authentication/howto-mfa-nps-extension-rdg.md
[register-nps-ad]:../active-directory/authentication/howto-mfa-nps-extension-rdg.md#register-server-in-active-directory
[create-nps-policy]: ../active-directory/authentication/howto-mfa-nps-extension-rdg.md#configure-network-policy
[concepts-mfa]: ../active-directory/authentication/concept-mfa-howitworks.md

<!-- EXTERNAL LINKS -->
[deploy-remote-desktop]: https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure
[rd-web-client]: https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-web-client-admin
[rds-high-availability]: https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-plan-high-availability
