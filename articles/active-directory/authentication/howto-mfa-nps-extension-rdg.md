---
title: RDG 'YI Azure MFA NPS Uzantısı ile tümleştirme-Azure Active Directory
description: Uzak Masaüstü Ağ Geçidi altyapınızı Azure mfa'yı Microsoft Azure için ağ ilkesi sunucusu uzantısı kullanarak tümleştirin
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6ec402cf2c741d88d230e5734485bf9eb0dd1b03
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74381825"
---
# <a name="integrate-your-remote-desktop-gateway-infrastructure-using-the-network-policy-server-nps-extension-and-azure-ad"></a>Ağ İlkesi Sunucusu (NPS) uzantısı ve Azure AD kullanarak Uzak Masaüstü Ağ Geçidi altyapınızı tümleştirin

Bu makalede ayrıntıları Uzak Masaüstü Ağ Geçidi altyapınızı Azure multi-Factor Authentication (MFA) ile tümleştirmek için Microsoft Azure için ağ ilkesi sunucusu (NPS) uzantısını kullanarak sağlar.

Azure için ağ Ilkesi sunucusu (NPS) uzantısı, müşterilerin Azure 'un bulut tabanlı [Multi-Factor Authentication (MFA)](multi-factor-authentication.md)kullanarak arayan kullanıcının uzaktan KIMLIĞINI doğrulama HIZMETI (RADIUS) istemci kimlik doğrulamasını korumanıza olanak tanır. Bu çözüm, kullanıcı oturum açmaları ve işlemleri için ikinci bir güvenlik katmanı eklemek için iki aşamalı doğrulama sağlar.

Bu makalede, Azure için NPS uzantısı kullanarak NPS altyapı ile Azure mfa'yı tümleştirmek için adım adım yönergeler sağlar. Bu, bir Uzak Masaüstü Ağ Geçidi oturum açmaya çalışan kullanıcılar için güvenli doğrulama sağlar.

> [!NOTE]
> Bu makale MFA sunucu dağıtımlarıyla kullanılmamalıdır ve yalnızca Azure MFA (bulut tabanlı) dağıtımlarıyla birlikte kullanılmalıdır.

Ağ İlkesi ve erişim Hizmetleri'ni (NPS) kuruluşlar yeteneği aşağıdakileri sağlar:

* Yönetim Merkezi konumlarını ve denetim kimin bağlanabilir belirterek ağ isteklerinin gün bağlantıların ne zaman izin verilir, bağlantıları süresini ve istemcilerin bağlanın ve benzeri güvenlik düzeyini tanımlar. Bu ilkelerin her VPN ya da Uzak Masaüstü (RD) Ağ Geçidi sunucusuna belirtmek yerine, bu ilkeler, bir kez merkezi bir konumda belirtilebilir. RADIUS protokolü, merkezi kimlik doğrulaması, yetkilendirme ve hesap işlemleri (AAA) sağlar.
* Kurmak ve cihazları Kısıtlanmamış veya kısıtlanmış ağ kaynaklarına erişim izni olup olmadığını belirleyen Ağ Erişim Koruması (NAP) istemci sistem durumu ilkeleri uygular.
* Kimlik doğrulama ve yetkilendirme erişim için 802.1 zorlamak için sağladıkları x özellikli kablosuz erişim noktaları ve Ethernet anahtarları.

Genellikle kuruluşlar VPN ilkelerinin yönetimini basitleştirmek ve merkezileştirmek için NPS (RADIUS) kullanır. Ancak, çoğu kuruluş ayrıca NPS RD Masaüstü Bağlantısı Yetkilendirme İlkeleri (RD Cap'leri) yönetimini merkezden gerçekleştirin ve kolaylaştırmak için kullanır.

Kuruluşlar, NPS güvenliğini ve yüksek düzeyde uyumluluk sağlamak için Azure MFA ile de tümleştirebilirsiniz. Bu kullanıcılar oturum açmak için Uzak Masaüstü Ağ geçidi için iki aşamalı doğrulama oluşturmanızı sağlar. Kullanıcıların erişim verilmesi için kendi denetimde kullanıcının sahip olduğu bilgilerle birlikte, kullanıcı adı/parola bileşimini sağlamanız gerekir. Bu bilgileri güvenilir ve kolayca yinelenen, bir cep telefonu numarası, telefona numarası, uygulamayı bir mobil cihaz ve benzeri gibi. RDG Şu anda 2FA için Microsoft Authenticator uygulama yöntemlerinden telefon aramasını ve anında iletme bildirimlerini desteklemektedir. Desteklenen kimlik doğrulama yöntemleri hakkında daha fazla bilgi için [kullanıcılarınızın hangi kimlik doğrulama yöntemlerini kullanabileceğini belirleme](howto-mfa-nps-extension.md#determine-which-authentication-methods-your-users-can-use)bölümüne bakın.

Azure için NPS uzantısının kullanılabilirliğine başlamadan önce, tümleşik NPS ve Azure MFA ortamları için iki aşamalı doğrulama uygulamayı kullanan müşterilerin, [Uzak Masaüstü Ağ Geçidi ve azure MULTI-Factor AUTHENTICATION sunucusu RADIUS kullanarak,](howto-mfaserver-nps-rdg.md)şirket içi ortamda ayrı bir MFA sunucusu yapılandırması ve bakımını gerçekleştirmiş olması gerekiyordu.

Azure için NPS uzantısı kullanılabilirliğini artık kuruluşların güvenli RADIUS istemci kimlik doğrulaması için şirket içi tabanlı MFA çözümünü veya bir bulut tabanlı MFA çözümünü dağıtmak için seçmenizi sağlar.

## <a name="authentication-flow"></a>Kimlik doğrulama akışı

Kullanıcıların Uzak Masaüstü Ağ Geçidi aracılığıyla ağ kaynaklarına erişim verilmesi için bir RD Bağlantı Yetkilendirme İlkesi (RD CAP) ve bir RD kaynak yetkilendirme ilkesi (RD RAP) belirtilen koşulları karşılaması gerekir. RD CAP kimin RD ağ geçitlerine bağlanmak için yetkili belirtin. Uzak Masaüstü veya kullanıcı RD Ağ geçidi üzerinden bağlanmasına izin verilen uzaktan uygulamaları gibi ağ kaynaklarına RD RAP belirtin.

RD Ağ geçidi için RD Cap'leri bir merkezi ilke deposunu kullanmak üzere yapılandırılabilir. RD Ağ Geçidi üzerinde işlendikçe RD RAP bir merkezi ilke kullanamazsınız. Merkezi ilke deposu olarak hizmet veren başka bir NPS sunucusunun RADIUS istemcisi için RD Cap'leri merkezi ilke deposu kullanmak üzere yapılandırılmış bir RD Ağ Geçidi örneğidir.

Azure için NPS uzantısı NPS ve Uzak Masaüstü Ağ geçidi ile tümleştirildiğinde, başarılı kimlik doğrulaması akışı aşağıdaki gibidir:

1. Uzak Masaüstü Ağ Geçidi sunucusu, Uzak Masaüstü oturumu gibi bir kaynağa bağlanmak için bir Uzak Masaüstü kullanıcı kimlik doğrulama isteği alır. Bir RADIUS istemcisi işlevi gören, Uzak Masaüstü Ağ Geçidi sunucusu, RADIUS erişim isteğini ileti isteği dönüştürür ve NPS uzantısı, yüklü olduğu, RADIUS (NPS'yi) sunucusuna ileti gönderir.
1. Kullanıcı adı ve parola birleşimini Active Directory'de doğrulanır ve kullanıcının kimliği doğrulanır.
1. NPS bağlantı isteği ve ağ ilkelerinde belirtilen tüm koşullar karşılanıyorsa (örneğin, zaman gün veya grubun üyelik kısıtlamaları), bir istek için ikincil kimlik doğrulaması ile Azure MFA NPS uzantısı tetikler.
1. Azure MFA, Azure AD ile iletişim kurar, kullanıcının ayrıntılarını alır ve desteklenen bir yöntemle ikincil kimlik doğrulaması gerçekleştirir.
1. MFA testini başarılı olduktan sonra Azure MFA NPS uzantısı sonucu iletişim kurar.
1. Uzantının yüklü, NPS sunucusunun bir RADIUS Erişim Kabul iletisi RD CAP ilkesi için Uzak Masaüstü Ağ Geçidi sunucusuna gönderir.
1. Kullanıcı, RD Ağ Geçidi aracılığıyla istenen ağ kaynağına erişim izni verilir.

## <a name="prerequisites"></a>Önkoşullar

Bu bölümde, Uzak Masaüstü Ağ geçidi ile Azure mfa'yı tümleştirme önce gerekli önkoşulları açıklanmaktadır. Başlamadan önce aşağıdaki önkoşulların yerinde olması gerekir.  

* Uzak Masaüstü Hizmetleri (RDS) altyapısı
* Azure MFA lisans
* Windows Server yazılımı
* Ağ İlkesi ve erişim Hizmetleri'ni (NPS) rol
* Azure Active Directory, şirket içi Active Directory ile eşitlenmiş
* Azure Active Directory GUID kimliği

### <a name="remote-desktop-services-rds-infrastructure"></a>Uzak Masaüstü Hizmetleri (RDS) altyapısı

Çalışan bir Uzak Masaüstü Hizmetleri (RDS) altyapısının yerinde olması gerekir. Bunu yapmazsanız, aşağıdaki hızlı başlangıç şablonunu kullanarak bu altyapıyı Azure 'da hızlıca oluşturabilirsiniz: [Uzak Masaüstü oturumu koleksiyonu dağıtımı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/ad20c78b36d8e1246f96bb0e7a8741db481f957f/rds-deployment).

El ile test etmek için hızlı bir şekilde şirket içi RDS altyapı oluşturmak istiyorsanız, bir dağıtmak için adımları izleyin.
**Daha fazla bilgi edinin**: [RDS 'yi Azure hızlı başlangıç](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-in-azure) ve [temel RDS altyapı dağıtımı](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure)ile dağıtın.

### <a name="azure-mfa-license"></a>Azure MFA lisans

Gerekli bir lisans Azure MFA için hangi Azure AD Premium ya da dahil diğer grupları kullanılabilir. Tüketim tabanlı lisans gibi kullanıcı başına veya kimlik doğrulaması lisans başına Azure mfa NPS uzantısı ile uyumlu değildir. Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication alma](concept-mfa-licensing.md). Sınama amacıyla bir deneme aboneliğini kullanabilirsiniz.

### <a name="windows-server-software"></a>Windows Server yazılımı

NPS uzantısı, Windows Server 2008 R2 SP1 gerektirir veya üstü yüklü NPS rol hizmetine sahip. Bu bölümdeki tüm adımlar, Windows Server 2016'yı kullanarak gerçekleştirildi.

### <a name="network-policy-and-access-services-nps-role"></a>Ağ İlkesi ve erişim Hizmetleri'ni (NPS) rol

NPS rol hizmetinin işlevselliğinin yanı sıra ağ erişim ilkesi sistem sağlığı hizmeti RADIUS sunucusu ve istemci sağlar. Bu rol altyapınızda en az iki bilgisayara yüklenmesi gerekir: Uzak Masaüstü Ağ geçidi ve başka bir üye sunucu veya etki alanı denetleyicisi. Varsayılan olarak, zaten Uzak Masaüstü Ağ geçidi olarak yapılandırılmış bilgisayarda rolüdür.  Ayrıca NPS rolü üzerinde en az bir etki alanı denetleyicisi veya üye sunucu gibi başka bir bilgisayar üzerinde yüklemeniz gerekir.

Windows Server 2012 veya daha eski bir NPS rol hizmetini yükleme hakkında bilgi için bkz. [NAP sistem durumu Ilkesi sunucusu yükleme](https://technet.microsoft.com/library/dd296890.aspx). NPS 'nin bir etki alanı denetleyicisine yüklenmesi önerisi dahil olmak üzere en iyi NPS yöntemlerinin açıklaması için bkz. [NPS Için En Iyi uygulamalar](https://technet.microsoft.com/library/cc771746).

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Azure Active Directory, şirket içi Active Directory ile eşitlenmiş

NPS uzantısını kullanmak için şirket içi kullanıcıları Azure AD ile eşitlenen ve MFA için etkinleştirilmiş olmalıdır. Bu bölümde, şirket içi kullanıcıların AD Connect kullanarak Azure AD ile eşitlenir varsayılır. Azure AD Connect hakkında daha fazla bilgi için bkz. Şirket [içi dizinlerinizi Azure Active Directory tümleştirme](../hybrid/whatis-hybrid-identity.md).

### <a name="azure-active-directory-guid-id"></a>Azure Active Directory GUID kimliği

NPS uzantısını yüklemek için Azure AD'nin GUID bilmeniz gerekir. Azure AD GUID'i bulmak için yönergeler aşağıda verilmiştir.

## <a name="configure-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamasını yapılandırma

Bu bölümde, Azure mfa'yı Uzak Masaüstü Ağ geçidi ile tümleştirmeye yönelik yönergeler sağlar. Yönetici olarak, kullanıcılar şirket içinde çok faktörlü cihazlar ya da uygulamaları kaydedebilmek için önce Azure MFA hizmetini yapılandırmanız gerekir.

Azure AD kullanıcılarınız için MFA 'yı etkinleştirmek üzere [bulutta azure Multi-Factor Authentication kullanmaya](howto-mfa-getstarted.md) başlama adımlarını izleyin.

### <a name="configure-accounts-for-two-step-verification"></a>İki aşamalı doğrulama için hesaplarını yapılandırma

Bir hesap için mfa'yı etkinleştirildikten sonra ikinci kimlik doğrulama faktörü için kullanın ve iki aşamalı doğrulama kullanarak kimlik doğrulaması güvenilir bir cihaz başarılı bir şekilde yapılandırmadığınız sürece MFA İlkesi tarafından yönetilen kaynaklara oturum açamazsınız.

[Azure Multi-Factor Authentication benim Için ne anlama geldiğini](../user-help/multi-factor-authentication-end-user.md) öğrenmek için bu adımları izleyin.

## <a name="install-and-configure-nps-extension"></a>Yükleme ve NPS uzantısı yapılandırma

Bu bölümde, Uzak Masaüstü Ağ Geçidi istemci kimlik doğrulaması için Azure mfa'yı kullanmak için RDS altyapı yapılandırma için yönergeler sağlar.

### <a name="acquire-azure-active-directory-guid-id"></a>Azure Active Directory GUID kimliği Al

NPS uzantısı yapılandırma işleminin bir parçası olarak, Azure AD kiracınız için yönetici kimlik bilgileri ve Azure AD Kimliğini sağlamanız gerekir. Aşağıdaki adımlar Kiracı kimliğini almak nasıl gösterir

1. Azure kiracının genel yöneticisi olarak [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** simgesini seçin.
1. **Özellikler**’i seçin.
1. Özellikler dikey penceresinde, dizin KIMLIĞI ' nin yanında, KIMLIĞI panoya kopyalamak için aşağıda gösterildiği gibi **Kopyala** simgesine tıklayın.

   ![Azure portal dizin KIMLIĞI alınıyor](./media/howto-mfa-nps-extension-rdg/image1.png)

### <a name="install-the-nps-extension"></a>NPS uzantısını yükleme

NPS uzantısı, Ağ İlkesi ve erişim Hizmetleri'ni (NPS) rolü yüklü bir sunucuya yükleyin. Tasarımınızı için RADIUS sunucusu olarak işlev görür.

> [!Important]
> Uzak Masaüstü Ağ Geçidi sunucunuzda NPS uzantısı yüklemeyin emin olun.
>

1. [NPS uzantısını](https://aka.ms/npsmfa)indirin.
1. Yürütülebilir kurulum dosyası (NpsExtnForAzureMfaInstaller.exe) NPS sunucusuna kopyalayın.
1. NPS sunucusunda **NpsExtnForAzureMfaInstaller. exe**' ye çift tıklayın. İstenirse, **Çalıştır**' a tıklayın.
1. Azure MFA Için NPS uzantısı kurulum iletişim kutusunda, yazılım lisans koşullarını gözden geçirin, **Lisans hüküm ve koşullarını kabul**ediyorum ' u Işaretleyin ve **yükleme**' ye tıklayın.
1. Azure MFA Için NPS uzantısı kurulum iletişim kutusunda **Kapat**' a tıklayın.

### <a name="configure-certificates-for-use-with-the-nps-extension-using-a-powershell-script"></a>Bir PowerShell betiğini kullanarak NPS uzantısı ile kullanım için sertifikaları yapılandırma

Ardından, iletişimlerin güvenliğini sağlamak ve güvencesi sağlamak için NPS uzantısı tarafından sertifikalar kullanmak için yapılandırmanız gerekir. NPS bileşenleri otomatik olarak imzalanan bir sertifika kullanmak için NPS ile yapılandıran bir Windows PowerShell Betiği içerir.

Komut aşağıdaki eylemleri gerçekleştirir:

* Otomatik olarak imzalanan bir sertifika oluşturur
* Hizmet sorumlusu Azure AD için sertifikanın ortak anahtarını ilişkilendirir
* Sertifika yerel makine deposu
* Sertifikanın özel anahtarı ağ kullanıcı erişimi verir
* Ağ İlkesi Sunucusu hizmetini yeniden başlatır

Kendi sertifikalarını kullanmak istiyorsanız, Azure AD'de hizmet sorumlusu sertifikanıza ortak anahtarı ilişkilendirmek vb. gerekir.

Betiği kullanmak için Azure AD yönetici kimlik bilgilerinizi ve daha önce kopyaladığınız Azure AD Kiracı Kimliğinizi uzantısı sağlar. Betik, NPS uzantısı yüklü olduğu her NPS sunucusunda çalıştırın. Ardından şunları yapın:

1. Bir yönetici Windows PowerShell istemi açın.
1. PowerShell komut isteminde `cd ‘c:\Program Files\Microsoft\AzureMfa\Config’`yazın ve **ENTER**tuşuna basın.
1. `.\AzureMfaNpsExtnConfigSetup.ps1`yazın ve **ENTER**tuşuna basın. Betik, Azure Active Directory PowerShell Modülü yüklü olup olmadığını denetler. Yüklü değilse, komut sizin için modülünü yükler.

   ![Azure AD PowerShell 'de AzureMfaNpsExtnConfigSetup. ps1 çalıştırma](./media/howto-mfa-nps-extension-rdg/image4.png)
  
1. PowerShell modülünün yükleme betiği doğruladıktan sonra Azure Active Directory PowerShell modülü iletişim kutusunu görüntüler. İletişim kutusunda Azure AD yönetici kimlik bilgilerinizi ve parolanızı girip **oturum aç**' a tıklayın.

   ![PowerShell 'de Azure AD kimlik doğrulaması](./media/howto-mfa-nps-extension-rdg/image5.png)

1. İstendiğinde, pano 'ya kopyaladığınız Dizin KIMLIĞINI daha önce yapıştırın ve **ENTER**tuşuna basın.

   ![PowerShell 'de dizin KIMLIĞI yerleştirme](./media/howto-mfa-nps-extension-rdg/image6.png)

1. Betik, otomatik olarak imzalanan bir sertifika oluşturur ve başka yapılandırma değişiklikleri gerçekleştirir. Çıktı, görüntüyü aşağıda gösterildiği gibi olması gerekir.

   ![Otomatik olarak imzalanan sertifikayı gösteren PowerShell çıkışı](./media/howto-mfa-nps-extension-rdg/image7.png)

## <a name="configure-nps-components-on-remote-desktop-gateway"></a>Uzak Masaüstü Ağ Geçidi üzerinde NPS bileşenlerini yapılandırma

Bu bölümde, Uzak Masaüstü Ağ Geçidi bağlantısı Yetkilendirme İlkeleri ve diğer RADIUS ayarlarını yapılandırın.

Kimlik doğrulama akışı, Uzak Masaüstü Ağ Geçidi ile NPS uzantısının yüklendiği NPS sunucusu arasında RADIUS iletilerinin değiştirilmesini gerektirir. Başka bir deyişle, Uzak Masaüstü Ağ Geçidi hem NPS uzantısı, yüklü olduğu bir NPS sunucusunun RADIUS istemci ayarları yapılandırmalısınız.

### <a name="configure-remote-desktop-gateway-connection-authorization-policies-to-use-central-store"></a>Uzak Masaüstü Ağ Geçidi bağlantısı yetkilendirme ilkeleri merkezi deposunu kullanmak üzere yapılandırma

Uzak Masaüstü Bağlantısı Yetkilendirme İlkeleri (RD Cap'leri) bir Uzak Masaüstü Ağ Geçidi sunucusuna bağlanmak için koşulları belirtin. RD CAP yerel olarak depolanabilir (varsayılan) ya da depolanabilir NPS çalıştıran merkezi bir RD CAP Deposu içinde. RDS ile Azure mfa'yı tümleştirmesini yapılandırmak için merkezi bir depo kullanımını belirtmeniz gerekir.

1. RD Ağ Geçidi sunucusunda, **Sunucu Yöneticisi**açın.
1. Menüsünde **Araçlar**' a tıklayın, **Uzak Masaüstü Hizmetleri**' nin üzerine gelin ve ardından **Uzak Masaüstü Ağ Geçidi Yöneticisi**' e tıklayın.
1. RD Ağ Geçidi Yöneticisi **\[sunucu adı\] (yerel)** öğesine sağ tıklayın ve **Özellikler**' e tıklayın.
1. Özellikler iletişim kutusunda **RD CAP deposu** sekmesini seçin.
1. RD CAP Deposu sekmesinde **NPS çalıştıran merkezi sunucu**' yı seçin. 
1. **NPS çalıştıran sunucu için bir ad veya IP adresi girin** alanına NPS uzantısını YÜKLEDIĞINIZ sunucunun IP adresini veya sunucu adını yazın.

   ![NPS sunucunuzun adını veya IP adresini girin](./media/howto-mfa-nps-extension-rdg/image10.png)
  
1. **Ekle**'ye tıklayın.
1. **Paylaşılan gizlilik** iletişim kutusunda paylaşılan bir gizlilik girin ve ardından **Tamam**' a tıklayın. Bu paylaşılan gizli dizinin kaydedin ve kaydı güvenli bir şekilde saklayın emin olun.

   >[!NOTE]
   >Paylaşılan gizliliğin, RADIUS sunucuları ve istemciler arasında güven oluşturmak için kullanılır. Uzun ve karmaşık bir parola oluşturun.
   >

   ![Güven sağlamak için paylaşılan gizlilik oluşturma](./media/howto-mfa-nps-extension-rdg/image11.png)

1. **Tamam**’a tıklayarak iletişim kutusunu kapatın.

### <a name="configure-radius-timeout-value-on-remote-desktop-gateway-nps"></a>Uzak Masaüstü Ağ Geçidi NPS'ye RADIUS zaman aşımı değerini yapılandırma

Kullanıcıların kimlik bilgilerini doğrulamak için zaman olduğundan emin olmak için iki aşamalı doğrulamanın, yanıtlar almasına ve yanıt RADIUS iletiler için RADIUS zaman aşımı değeri ayarlamak gereklidir.

1. RD Ağ Geçidi sunucusunda, Sunucu Yöneticisi'ni açın. Menüsünde **Araçlar**' a ve ardından **ağ ilkesi sunucusu**' na tıklayın.
1. **NPS (yerel)** konsolunda, **RADIUS istemcileri ve sunucuları**' nı genişletin ve **uzak RADIUS sunucusu**' nu seçin.

   ![Uzak RADIUS sunucusunu gösteren ağ Ilkesi sunucusu yönetim konsolu](./media/howto-mfa-nps-extension-rdg/image12.png)

1. Ayrıntılar bölmesinde **TS Ağ GEÇIDI sunucu grubu**' na çift tıklayın.

   >[!NOTE]
   >NPS ilkelerinin merkezi sunucu yapılandırıldığında bu RADIUS sunucu grubu oluşturuldu. RD Ağ Geçidi grubunda birden daha fazla olması durumunda bu sunucuya veya sunucu grubu RADIUS iletileri iletir.
   >

1. **TS Ağ GEÇIDI sunucu grubu özellikleri** iletişim kutusunda, RD CAP 'leri depolamak IÇIN yapılandırdığınız NPS sunucusunun IP adresini veya adını seçin ve ardından **Düzenle**' ye tıklayın.

   ![Daha önce yapılandırılan NPS sunucusunun IP veya adını seçin](./media/howto-mfa-nps-extension-rdg/image13.png)

1. **RADIUS sunucusunu Düzenle** Iletişim kutusunda **Yük Dengeleme** sekmesini seçin.
1. **Yük Dengeleme** sekmesinde, **istek bırakılmış olarak kabul edilmeden önce yanıt olmadan saniye sayısı** alanına, varsayılan değeri 3 ' ten 30 ile 60 saniye arasında bir değere değiştirin.
1. **Sunucu kullanılamaz olarak tanımlandığında istekler arasındaki saniye sayısı** alanında, 30 saniyelik varsayılan değeri, önceki adımda belirttiğiniz değere eşit veya ondan daha büyük bir değere değiştirin.

   ![Yük Dengeleme sekmesinde RADIUS sunucusu zaman aşımı ayarlarını düzenleyin](./media/howto-mfa-nps-extension-rdg/image14.png)

1. İletişim kutularını kapatmak için iki kez **Tamam** ' a tıklayın.

### <a name="verify-connection-request-policies"></a>Bağlantı isteği ilkeleri doğrulayın

RD Ağ Geçidi bağlantısı Yetkilendirme İlkeleri için bir merkezi ilke deposunu kullanmak üzere yapılandırdığınızda, varsayılan olarak, RD Ağ Geçidi UÇ istekleri NPS sunucusuna iletmek için yapılandırılır. NPS sunucusu ile Azure mfa'yı uzantısı yüklü, RADIUS erişim isteğini işler. Aşağıdaki adımlar varsayılan bağlantı isteği ilkesi nasıl gösterir.

1. RD Ağ Geçidi, NPS (yerel) konsolunda, **ilkeler**' i genişletin ve **bağlantı isteği ilkeleri**' ni seçin.
1. **TS Ağ GEÇIDI YETKILENDIRME ilkesi**' ne çift tıklayın.
1. **TS Ağ GEÇIDI YETKILENDIRME ilkesi özellikleri** Iletişim kutusunda **Ayarlar** sekmesine tıklayın.
1. **Ayarlar** sekmesinde, bağlantı isteği Iletme altında **kimlik doğrulama**' ya tıklayın. RADIUS istemcisi, kimlik doğrulama istekleri iletmek için yapılandırılır.

   ![Sunucu grubunu belirten kimlik doğrulama ayarlarını yapılandırın](./media/howto-mfa-nps-extension-rdg/image15.png)

1. **Iptal 'e**tıklayın.

## <a name="configure-nps-on-the-server-where-the-nps-extension-is-installed"></a>NPS uzantısı, yüklü olduğu sunucuda NPS yapılandırma

NPS uzantısı, yüklü olduğu bir NPS sunucusu ile NPS sunucu Uzak Masaüstü Ağ Geçidi üzerinde RADIUS mesaj alışverişi gerekir. Bu ileti değişim etkinleştirmek için NPS uzantısı Hizmeti'nin yüklendiği sunucuda NPS bileşenleri yapılandırmanız gerekir.

### <a name="register-server-in-active-directory"></a>Sunucusu Active Directory'de Kaydettir

Bu senaryoda düzgün çalışması için NPS sunucusunun Active Directory'de kayıtlı olması gerekir.

1. NPS sunucusunda **Sunucu Yöneticisi**açın.
1. Sunucu Yöneticisi ' de **Araçlar**' a ve ardından **ağ ilkesi sunucusu**' na tıklayın.
1. Ağ Ilkesi sunucu konsolunda **NPS (yerel)** öğesine sağ tıklayın ve ardından **Active Directory sunucuyu kaydet**' e tıklayın.
1. **Tamam** ' ı iki kez tıklatın.

   ![NPS sunucusunu Active Directory kaydetme](./media/howto-mfa-nps-extension-rdg/image16.png)

1. Konsolunu sonraki yordam için açık bırakın.

### <a name="create-and-configure-radius-client"></a>Oluşturma ve RADIUS istemci yapılandırma

Uzak Masaüstü Ağ Geçidi NPS sunucusunun RADIUS istemcisi olarak yapılandırılması gerekir.

1. NPS uzantısının yüklendiği NPS sunucusunda, **NPS (yerel)** konsolunda, **RADIUS istemcileri** ' ne sağ tıklayın ve **Yeni**' ye tıklayın.

   ![NPS konsolunda yeni bir RADIUS Istemcisi oluşturma](./media/howto-mfa-nps-extension-rdg/image17.png)

1. **Yenı RADIUS istemcisi** iletişim kutusunda, _ağ geçidi_gibi kolay bir ad ve Uzak Masaüstü Ağ Geçidi sunucusunun IP adresini veya DNS adını belirtin.
1. **Paylaşılan gizlilik** ve **paylaşılan gizli anahtarı Onayla** alanlarında, daha önce kullandığınız gizli anahtarı girin.

   ![Kolay bir ad ve IP veya DNS adresi yapılandırın](./media/howto-mfa-nps-extension-rdg/image18.png)

1. Yeni RADIUS Istemcisi iletişim kutusunu kapatmak için **Tamam** ' ı tıklatın.

### <a name="configure-network-policy"></a>Ağ İlkesi yapılandırma

NPS sunucusu ile Azure mfa'yı uzantısı atanmış merkezi ilke deposu bağlantı yetkilendirme ilkesi (CAP) için olduğunu hatırlayın. Bu nedenle, bir büyük harf geçerli bağlantı isteklerini yetkilendirmek için NPS sunucusuna uygulamak gerekir.  

1. NPS sunucusunda NPS (yerel) konsolunu açın, **ilkeler**' i genişletin ve **ağ ilkeleri**' ne tıklayın.
1. **Diğer erişim sunucularının bağlantıları**' na sağ tıklayın ve **yinelenen ilke**' ye tıklayın.

   ![Diğer erişim sunucuları ilkesiyle bağlantıyı çoğaltın](./media/howto-mfa-nps-extension-rdg/image19.png)

1. **Diğer erişim sunucularındaki bağlantıların kopyasını**sağ tıklatın ve **Özellikler**' e tıklayın.
1. **Diğer erişim sunucularına bağlantı kopyalama** iletişim kutusunda, **ilke adı**' nda, _RDG_CAP_gibi uygun bir ad girin. **İlkeyi etkin**olarak işaretleyin ve **erişim ver**' i seçin. İsteğe bağlı olarak, **ağ erişim sunucusu türü**' nde **Uzak Masaüstü Ağ Geçidi**' yi seçin veya **belirtilmemiş**olarak bırakabilirsiniz.

   ![İlkeyi adlandırın, etkinleştirin ve erişim izni verin](./media/howto-mfa-nps-extension-rdg/image21.png)

1. **Kısıtlamalar** sekmesine tıklayın ve **istemcilerin kimlik doğrulama yöntemi üzerinde anlaşmadan bağlanmasına izin ver**' i işaretleyin.

   ![İstemcilerin bağlanmasına izin vermek için kimlik doğrulama yöntemlerini değiştirme](./media/howto-mfa-nps-extension-rdg/image22.png)

1. İsteğe bağlı olarak, **koşullar** sekmesine tıklayın ve bağlantının yetkilendirildiği, örneğin belirli bir Windows grubuna üyelik için karşılanması gereken koşulları ekleyin.

   ![İsteğe bağlı olarak bağlantı koşullarını belirtin](./media/howto-mfa-nps-extension-rdg/image23.png)

1. **OK (Tamam)** düğmesine tıklayın. İlgili yardım konusunu görüntülemeniz istendiğinde **Hayır**' a tıklayın.
1. Erişim verir ve yeni ilkeniz ilkenin etkinleştirildiğini listenin başında olduğundan emin olun.

   ![İlkenizi listenin en üstüne taşıyın](./media/howto-mfa-nps-extension-rdg/image24.png)

## <a name="verify-configuration"></a>Yapılandırmayı doğrulama

Yapılandırmayı doğrulamak için uygun bir RDP istemcisi ile Uzak Masaüstü Ağ geçidi için oturum açmanız gerekir. Bağlantı Yetkilendirme ilkelerinizi tarafından izin verilen ve Azure MFA için etkinleştirilmiş bir hesap kullandığınızdan emin olun.

Aşağıdaki görüntüde gösterildiği gibi, **uzak masaüstü web erişimi** sayfasını kullanabilirsiniz.

![Uzak Masaüstü Web Erişimi test etme](./media/howto-mfa-nps-extension-rdg/image25.png)

Kimlik bilgilerinizi birincil kimlik doğrulaması için başarıyla girildikten sonra Uzak Masaüstü Bağlantısı iletişim kutusu başlatma uzak bağlantı durumunu aşağıda gösterildiği gibi gösterir. 

Başarılı bir şekilde Azure mfa'yı daha önce yapılandırdığınız ikincil kimlik doğrulama yöntemi ile kimlik doğrulaması, kaynağa bağlıdır. Ancak, ikincil kimlik doğrulaması başarılı olmazsa, kaynağa erişimi reddedilir. 

![Uzak bağlantı başlatma Uzak Masaüstü Bağlantısı](./media/howto-mfa-nps-extension-rdg/image26.png)

Aşağıdaki örnekte, kimlik doğrulayıcı uygulamasını bir Windows phone'da ikincil kimlik doğrulaması sağlamak için kullanılır.

![Doğrulamayı gösteren Windows Phone Authenticator uygulaması örneği](./media/howto-mfa-nps-extension-rdg/image27.png)

İkincil kimlik doğrulama yöntemi başarıyla doğruladıktan sonra Uzak Masaüstü Ağ Geçidi normal şekilde oturum açtığınız. Ancak, güvenilir bir cihazda bir mobil uygulama kullanarak ikincil bir kimlik doğrulama yöntemi kullanmanız gerektiğinden oturum açma işlemi, aksi takdirde daha güvenlidir.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Başarılı oturum açma olayları için Olay Görüntüleyicisi günlüklerini görüntüle

Başarılı oturum açma olaylarını Windows Olay Görüntüleyicisi günlükleri görüntülemek için Windows Terminal Hizmetleri ve Windows güvenlik günlükleri sorgulamak için aşağıdaki Windows PowerShell komutunu verebilir.

Ağ Geçidi işlem günlüklerinde _(olay Viewer\uygulamalar ve hizmetler Logs\microsoft\windows\terminalservices-gateway\işletimsel)_ başarılı oturum açma olaylarını sorgulamak Için aşağıdaki PowerShell komutlarını kullanın:

* `Get-WinEvent -Logname Microsoft-Windows-TerminalServices-Gateway/Operational | where {$_.ID -eq '300'} | FL`
* Bu komut, kullanıcının kaynak yetkilendirme ilkesi gereksinimlerini (RD RAP) sağlandığından ve erişim izni verildi gösteren Windows olayları görüntüler.

![PowerShell kullanarak olayları görüntüleme](./media/howto-mfa-nps-extension-rdg/image28.png)

* `Get-WinEvent -Logname Microsoft-Windows-TerminalServices-Gateway/Operational | where {$_.ID -eq '200'} | FL`
* Bu komut, kullanıcı bağlantısı yetkilendirme ilkesi gereksinimleri sağlandığında gösteren olayları görüntüler.

![PowerShell kullanarak bağlantı yetkilendirme ilkesini görüntüleme](./media/howto-mfa-nps-extension-rdg/image29.png)

Bu günlük ve filtre olay kimlikleri, 300 ve 200 üzerinde görüntüleyebilirsiniz. Başarılı oturum açma olaylarını Güvenlik Olay Görüntüleyicisi günlüklerinde sorgulamak için aşağıdaki komutu kullanın:

* `Get-WinEvent -Logname Security | where {$_.ID -eq '6272'} | FL`
* Bu komut, merkezi NPS veya RD Ağ Geçidi sunucusu üzerinde çalıştırılabilir.

![Başarılı oturum açma olayları örneği](./media/howto-mfa-nps-extension-rdg/image30.png)

Aşağıda gösterildiği gibi güvenlik günlüğü ya da Ağ İlkesi ve Erişim Hizmetleri özel görünümü da görüntüleyebilirsiniz:

![Ağ Ilkesi ve erişim Hizmetleri Olay Görüntüleyicisi](./media/howto-mfa-nps-extension-rdg/image31.png)

Azure MFA için NPS uzantısını yüklediğiniz sunucuda, _uygulama ve hizmetler Logs\Microsoft\AzureMfa_' de uzantıya özgü Olay Görüntüleyicisi uygulama günlüklerini bulabilirsiniz.

![Olay Görüntüleyicisi AuthZ uygulama günlükleri](./media/howto-mfa-nps-extension-rdg/image32.png)

## <a name="troubleshoot-guide"></a>Sorun giderme kılavuzu

Yapılandırma beklendiği gibi çalışmıyorsa, sorun giderme başlamak için ilk kullanıcının Azure mfa'yı kullanmak için yapılandırıldığını doğrulamak için yerdir. Kullanıcının [Azure Portal](https://portal.azure.com)bağlanmasını sağlar. Kullanıcıların ikincil doğrulama için istenir ve başarıyla kimlik doğrulaması, Azure MFA'ın hatalı bir yapılandırma ortadan kaldırabilir.

Azure MFA için kullanıcı olarak çalışıyorsa, ilgili olay günlüklerini gözden geçirmelisiniz. Bunlar, güvenlik olayı, ağ geçidi işletimsel ve önceki bölümde açıklanan Azure mfa'yı günlükleri içerir.

Aşağıda güvenlik günlüğü başarısız oturum açma olayı (olay kimliği 6273) gösteren bir örnek çıktı verilmiştir.

![Başarısız oturum açma olayının örneği](./media/howto-mfa-nps-extension-rdg/image33.png)

İlgili bir olay AzureMFA günlüklerinden aşağıdadır:

![Örnek Azure MFA oturum açma Olay Görüntüleyicisi](./media/howto-mfa-nps-extension-rdg/image34.png)

Gerçekleştirmek için Gelişmiş Seçenekleri sorun giderin, NPS hizmetinin yüklendiği NPS veritabanı biçimi günlük dosyalarına başvurun. Bu günlük dosyaları, _%systemroot%\System32\Logs_ klasöründe virgülle ayrılmış metin dosyaları olarak oluşturulur.

Bu günlük dosyalarının açıklaması için bkz. [NPS veritabanı biçimi günlük dosyalarını yorumlama](https://technet.microsoft.com/library/cc771748.aspx). Bu günlük dosyaları girişleri bir elektronik tablo veya bir veritabanı aktarmadan yorumlamak zor olabilir. Günlük dosyaları yorumlama içinde yardımcı olmak için birkaç IAS Çözümleyicileri çevrimiçi bulabilirsiniz.

Aşağıdaki görüntüde, bu tür indirilebilir bir [paylaşılan yazılım uygulamasının](https://www.deepsoftware.com/iasviewer)çıkışı gösterilmektedir.

![Örnek paylaşılan yazılım uygulaması IAS ayrıştırıcısı](./media/howto-mfa-nps-extension-rdg/image35.png)

Son olarak, ek sorun giderme seçenekleri için, [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx)gibi bir protokol Çözümleyicisi kullanabilirsiniz.

Microsoft Message Analyzer ' dan aşağıdaki görüntüde, **contoso alicec**Kullanıcı adını içeren RADIUS protokolünde filtrelenmiş ağ trafiği gösterilmektedir.

![Filtrelenmiş trafiği gösteren Microsoft Ileti Çözümleyicisi](./media/howto-mfa-nps-extension-rdg/image36.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Multi-Factor Authentication’ı edinme](concept-mfa-licensing.md)

[RADIUS kullanan Uzak Masaüstü Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu](howto-mfaserver-nps-rdg.md)

[Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)
