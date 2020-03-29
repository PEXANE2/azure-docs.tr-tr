---
title: RDG'yi Azure MFA NPS uzantısı ile tümleştir - Azure Etkin Dizini
description: Microsoft Azure için Ağ Politikası Sunucusu uzantısını kullanarak Uzak Masaüstü Ağ Geçidi altyapınızı Azure MFA ile tümleştirin
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71d9b2332d6d78e7bde63d0fa3f5b64b588e576b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75425469"
---
# <a name="integrate-your-remote-desktop-gateway-infrastructure-using-the-network-policy-server-nps-extension-and-azure-ad"></a>Ağ İlkesi Sunucusu (NPS) uzantısı ve Azure AD'yi kullanarak Uzak Masaüstü Ağ Geçidi altyapınızı tümleştirin

Bu makalede, Microsoft Azure için Ağ İlkesi Sunucusu (NPS) uzantısını kullanarak Uzak Masaüstü Ağ Geçidi altyapınızı Azure Çok Faktörlü Kimlik Doğrulaması (MFA) ile tümleştirmek için ayrıntılar verilmektedir.

Azure için Ağ İlkesi Sunucusu (NPS) uzantısı, müşterilerin Azure'un bulut tabanlı [Çok Faktörlü Kimlik Doğrulaması (MFA)](multi-factor-authentication.md)kullanarak Uzaktan Kimlik Doğrulama Çevirmeli Kullanıcı Hizmeti (RADIUS) istemci kimlik doğrulamasını korumasına olanak tanır. Bu çözüm, kullanıcı oturum açma ve hareketlere ikinci bir güvenlik katmanı eklemek için iki aşamalı doğrulama sağlar.

Bu makalede, Azure için NPS uzantısını kullanarak NPS altyapısını Azure MFA ile tümleştirmek için adım adım yönergeler sağlanmaktadır. Bu, Uzak Masaüstü Ağ Geçidi'nde oturum açmaya çalışan kullanıcılar için güvenli doğrulama sağlar.

> [!NOTE]
> Bu makale MFA Server dağıtımlarında kullanılmamalı ve yalnızca Azure MFA (Bulut tabanlı) dağıtımlarıyla kullanılmalıdır.

Ağ İlkesi ve Erişim Hizmetleri (NPS), kuruluşlara aşağıdakileri yapma olanağı sağlar:

* Kimlerin bağlanabileceğini, günlük bağlantılara hangi saatlerde izin verilebileni, bağlantıların süresini ve istemcilerin bağlanmak için kullanması gereken güvenlik düzeyini ve benzeri bilgileri belirterek ağ isteklerinin yönetimi ve denetimi için merkezi konumları tanımlayın. Bu ilkeleri her VPN veya Uzak Masaüstü (RD) Ağ Geçidi sunucusunda belirtmek yerine, bu ilkeler merkezi bir konumda bir kez belirtilebilir. RADIUS protokolü merkezi Kimlik Doğrulama, Yetkilendirme ve Muhasebe (AAA) sağlar.
* Aygıtlara ağ kaynaklarına sınırsız veya sınırlı erişim verilip verilmeyeceğini belirleyen Ağ Erişim Koruması (NAP) istemci sistem durumu ilkelerini belirleyin ve uygulayın.
* 802.1x özellikli kablosuz erişim noktalarına ve Ethernet anahtarlarına erişmek için kimlik doğrulama ve yetkilendirmeyi zorlamak için bir araç sağlayın.

Genellikle, kuruluşlar VPN ilkelerinin yönetimini basitleştirmek ve merkezileştirmek için NPS (RADIUS) kullanır. Ancak, birçok kuruluş RD Masaüstü Bağlantı Yetkilendirme İlkeleri'nin (RD CAP'leri) yönetimini basitleştirmek ve merkezileştirmek için NPS'yi de kullanır.

Kuruluşlar, güvenliği artırmak ve yüksek düzeyde uyumluluk sağlamak için NPS'yi Azure MFA ile tümleştirebilir. Bu, kullanıcıların Uzak Masaüstü Ağ Geçidi'nde oturum açmak için iki aşamalı doğrulama oluşturmalarını sağlamaya yardımcı olur. Kullanıcılara erişim hakkı verebilmesi için, kullanıcı adı/parola birleşimini kullanıcının kontrolündeki bilgilerle birlikte sağlamaları gerekir. Bu bilgilere güvenilmeli ve cep telefonu numarası, sabit hat numarası, mobil cihazdaki uygulama gibi kolayca çoğaltılamaz. RDG şu anda 2FA için Microsoft kimlik doğrulama uygulaması yöntemlerinden gelen telefon görüşmesi ve anında iletme bildirimlerini destekler. Desteklenen kimlik doğrulama yöntemleri hakkında daha fazla bilgi için, [kullanıcılarınızın kullanabileceği kimlik doğrulama yöntemlerini belirleyin](howto-mfa-nps-extension.md#determine-which-authentication-methods-your-users-can-use)bölümüne bakın.

Azure için NPS uzantısı kullanılabilirliğinden önce, tümleşik NPS ve Azure MFA ortamları için iki aşamalı doğrulama uygulamak isteyen [müşterilerin, RADIUS'u kullanarak Uzak Masaüstü Ağ Geçidi ve Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'nda](howto-mfaserver-nps-rdg.md)belgelenen şirket içi ortamda ayrı bir MFA Server'ı yapılandırmaları ve bakımını yapmak zorunda kaldılar.

Azure için NPS uzantısı kullanılabilirliği artık kuruluşlara RADIUS istemci kimlik doğrulamasını güvence altına almak için şirket içi tabanlı bir MFA çözümü veya bulut tabanlı MFA çözümünün dağıtılması seçeneği sunuyor.

## <a name="authentication-flow"></a>Kimlik Doğrulama Akışı

Kullanıcılara Uzak Masaüstü Ağ Geçidi aracılığıyla ağ kaynaklarına erişim hakkı verilebilmek için, bir RD Bağlantı Yetkilendirme İlkesi (RD CAP) ve bir RD Kaynak Yetkilendirme İlkesi 'nde (RD RAP) belirtilen koşulları karşılamaları gerekir. RD AP'leri, RD Ağ Geçitleri'ne bağlanmayetkisinin kim olduğunu belirtir. RD RAP'leri, kullanıcının RD Ağ Geçidi üzerinden bağlanmasına izin verilen uzak masaüstü veya uzak uygulamalar gibi ağ kaynaklarını belirtir.

RD Ağ Geçidi, RD AP'leri için merkezi bir ilke deposu kullanacak şekilde yapılandırılabilir. RD RAP'leri, RD Ağ Geçidi'nde işlendiği için merkezi bir ilke kullanamaz. RD AP'leri için merkezi bir ilke deposu kullanmak üzere yapılandırılan bir RD Ağ Geçidi örneği, merkezi ilke deposu olarak hizmet veren başka bir NPS sunucusunun RADIUS istemcisidir.

Azure için NPS uzantısı NPS ve Uzak Masaüstü Ağ Geçidi ile tümleşik olduğunda, başarılı kimlik doğrulama akışı aşağıdaki gibidir:

1. Uzak Masaüstü Ağ Geçidi sunucusu, uzak masaüstü oturumu gibi bir kaynağa bağlanmak için uzak bir masaüstü kullanıcısından kimlik doğrulama isteği alır. RADIUS istemcisi olarak hareket eden Uzak Masaüstü Ağ Geçidi sunucusu isteği RADIUS Erişim İsteği iletisine dönüştürür ve iletiyi NPS uzantısının yüklü olduğu RADIUS (NPS) sunucusuna gönderir.
1. Kullanıcı adı ve parola birleşimi Active Directory'de doğrulanır ve kullanıcının kimliği doğrulanır.
1. NPS Bağlantı İsteği ve Ağ İlkeleri'nde belirtilen tüm koşullar karşılanırsa (örneğin, günün saati veya grup üyeliği kısıtlamaları), NPS uzantısı Azure MFA ile ikincil kimlik doğrulaması isteğini tetikler.
1. Azure MFA, Azure AD ile iletişim kurar, kullanıcının ayrıntılarını alır ve desteklenen yöntemleri kullanarak ikincil kimlik doğrulamasını gerçekleştirir.
1. MFA mücadelesinin başarısı üzerine, Azure MFA sonucu NPS uzantısına iletir.
1. Uzantın yüklü olduğu NPS sunucusu, UZAK Masaüstü Ağ Geçidi sunucusuna RD CAP ilkesi için RADIUS Erişim Kabul iletisi gönderir.
1. Kullanıcıya RD Ağ Geçidi aracılığıyla istenen ağ kaynağına erişim izni verilir.

## <a name="prerequisites"></a>Ön koşullar

Bu bölümde, Azure MFA'yı Uzak Masaüstü Ağ Geçidi ile tümleştirmeden önce gereken ön koşullar ayrıntılı olarak açıklanmaktadır. Başlamadan önce, aşağıdaki ön koşullara sahip olmalısınız.  

* Uzak Masaüstü Hizmetleri (RDS) altyapısı
* Azure MFA Lisansı
* Windows Server yazılımı
* Ağ Politikası ve Erişim Hizmetleri (NPS) rolü
* Azure Etkin Dizini şirket içi Active Directory ile senkronize edildi
* Azure Etkin Dizin GUID Kimliği

### <a name="remote-desktop-services-rds-infrastructure"></a>Uzak Masaüstü Hizmetleri (RDS) altyapısı

Çalışan bir Uzak Masaüstü Hizmetleri (RDS) altyapınız olmalıdır. Bunu yapmazsanız, aşağıdaki hızlı başlangıç şablonunu kullanarak azure'da bu altyapıyı hızla oluşturabilirsiniz: [Uzak Masaüstü Oturum Koleksiyonu dağıtımı oluşturun.](https://github.com/Azure/azure-quickstart-templates/tree/ad20c78b36d8e1246f96bb0e7a8741db481f957f/rds-deployment)

Test amacıyla şirket içi RDS altyapısını el ile hızlı bir şekilde oluşturmak istiyorsanız, bir tanesini dağıtmak için aşağıdaki adımları izleyin.
**Daha fazla bilgi :** Azure hızlı başlatma ve Temel RDS altyapı dağıtımı ile [RDS'yi](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure) [dağıtın.](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-in-azure)

### <a name="azure-mfa-license"></a>Azure MFA Lisansı

Gerekli olan, Azure AD Premium veya onu içeren diğer paketler aracılığıyla kullanılabilen Azure MFA lisansıdır. Azure MFA'nın kullanıcı başına veya kimlik doğrulama lisansları gibi tüketim etütleri NPS uzantısı ile uyumlu değildir. Daha fazla bilgi için Azure [Çok Faktörlü Kimlik Doğrulama'yı nasıl edinirebilirsiniz.](concept-mfa-licensing.md) Sınama amacıyla deneme aboneliği kullanabilirsiniz.

### <a name="windows-server-software"></a>Windows Server yazılımı

NPS uzantısı, NPS rol hizmeti yüklü Windows Server 2008 R2 SP1 veya üzeri gerektirir. Bu bölümdeki tüm adımlar Windows Server 2016 kullanılarak gerçekleştirilmiştir.

### <a name="network-policy-and-access-services-nps-role"></a>Ağ Politikası ve Erişim Hizmetleri (NPS) rolü

NPS rol hizmeti RADIUS sunucu ve istemci işlevselliğinin yanı sıra Ağ Erişim Politikası sağlık hizmeti sağlar. Bu rol altyapınızdaki en az iki bilgisayara yüklenmelidir: Uzak Masaüstü Ağ Geçidi ve başka bir üye sunucu veya etki alanı denetleyicisi. Varsayılan olarak, rol zaten Uzak Masaüstü Ağ Geçidi olarak yapılandırılan bilgisayarda mevcuttur.  NPS rolünü en azından etki alanı denetleyicisi veya üye sunucu gibi başka bir bilgisayara yüklemeniz gerekir.

NPS rol hizmeti Windows Server 2012 veya daha büyük yükleme hakkında bilgi [için](https://technet.microsoft.com/library/dd296890.aspx)bkz. NPS'yi bir etki alanı denetleyicisine yükleme önerisi de dahil olmak üzere NPS için en iyi uygulamaların açıklaması [için](https://technet.microsoft.com/library/cc771746)Bkz.

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Azure Etkin Dizini şirket içi Active Directory ile senkronize edildi

NPS uzantısını kullanmak için şirket içi kullanıcıların Azure AD ile senkronize edilmesi ve MFA için etkinleştirilmesi gerekir. Bu bölümde, şirket içi kullanıcıların AD Connect kullanılarak Azure AD ile senkronize edildigini varsayar. Azure AD bağlantısı hakkında bilgi için bkz. Şirket [içi dizinlerinizi Azure Etkin Dizini ile tümleştirin.](../hybrid/whatis-hybrid-identity.md)

### <a name="azure-active-directory-guid-id"></a>Azure Etkin Dizin GUID Kimliği

NPS uzantısını yüklemek için Azure AD'nin GUID'ini bilmeniz gerekir. Azure REKLAM'ın GUID'ini bulma yönergeleri aşağıda verilmiştir.

## <a name="configure-multi-factor-authentication"></a>Çok Faktörlü Kimlik Doğrulamayı Yapılandırma

Bu bölümde, Azure MFA'nın Uzak Masaüstü Ağ Geçidi ile tümleştirilmesi için yönergeler sağlanmaktadır. Yönetici olarak, kullanıcıların çok faktörlü aygıtlarını veya uygulamalarını kendi kendilerine kaydedebilmeleri için Azure MFA hizmetini yapılandırmanız gerekir.

Azure AD kullanıcılarınız için MFA'yı etkinleştirmek için [buluttaki Azure Çok Faktörlü Kimlik Doğrulaması ile başlarken](howto-mfa-getstarted.md) ki adımları izleyin.

### <a name="configure-accounts-for-two-step-verification"></a>İki aşamalı doğrulama için hesapları yapılandırma

MFA için bir hesap etkinleştirildikten sonra, ikinci kimlik doğrulama faktörü için kullanılacak güvenilir bir aygıtı başarıyla yapılandırıp iki aşamalı doğrulama kullanarak kimlik doğrulaması yapılandırana kadar MFA ilkesi tarafından yönetilen kaynaklarda oturum açamazsınız.

Azure Çok [Faktörlü Kimlik Doğrulama'daki](../user-help/multi-factor-authentication-end-user.md) adımları izleyin?

## <a name="install-and-configure-nps-extension"></a>NPS uzantısını yükleme ve yapılandırma

Bu bölümde, UZAK Masaüstü Ağ Geçidi ile istemci kimlik doğrulaması için Azure MFA'yı kullanacak RDS altyapısını yapılandırmak için yönergeler sağlanmaktadır.

### <a name="acquire-azure-active-directory-guid-id"></a>Azure Active Directory GUID Kimliğini Edinin

NPS uzantısı yapılandırmasının bir parçası olarak, Azure AD kiracınız için yönetici kimlik bilgileri ve Azure AD Kimliği sağlamanız gerekir. Aşağıdaki adımlar, kiracı kimliğini nasıl alacağınızı gösterir.

1. Azure kiracısının genel yöneticisi olarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Azure portalı menüsünde **Azure Active Directory'yi**seçin veya herhangi bir sayfadan **Azure Active Directory'yi** arayın ve seçin.
1. **Özellikleri**seçin.
1. Özellikler bıyıkta, Dizin Kimliğinin yanında, kimliği panoya kopyalamak için aşağıda gösterildiği gibi **Kopyala** simgesini tıklatın.

   ![Azure portalından Dizin Kimliği alma](./media/howto-mfa-nps-extension-rdg/azure-active-directory-id-in-azure-portal.png)

### <a name="install-the-nps-extension"></a>NPS uzantısını yükleme

Ağ İlkesi ve Erişim Hizmetleri (NPS) rolünün yüklü olduğu bir sunucuya NPS uzantısını yükleyin. Bu, tasarımınız için RADIUS sunucusu olarak işlev görür.

> [!Important]
> NPS uzantısını Uzak Masaüstü Ağ Geçidi sunucunuza yüklemediğinizden emin olun.
>

1. [NPS uzantısını](https://aka.ms/npsmfa)indirin.
1. Kurulum yürütülebilir dosyasını (NpsExtnForAzureMfaInstaller.exe) NPS sunucusuna kopyalayın.
1. NPS sunucusunda, **NpsExtnForAzureMfaInstaller.exe'yi**çift tıklatın. İstenirse, **Çalıştır'ı**tıklatın.
1. Azure MFA Kurulumu için NPS Uzantısı iletişim kutusunda, yazılım lisans koşullarını gözden geçirin, **lisans hüküm ve koşullarını kabul ediyorum**seçeneğini işaretleyin ve **Yükle'yi**tıklatın.
1. Azure MFA Kurulumu iletişim kutusu için NPS Uzantısı'nda **Kapat'ı**tıklatın.

### <a name="configure-certificates-for-use-with-the-nps-extension-using-a-powershell-script"></a>PowerShell komut dosyası kullanarak NPS uzantısı ile kullanılacak sertifikaları yapılandırma

Ardından, güvenli iletişim ve güvence sağlamak için Sertifikaları NPS uzantısı tarafından kullanılmak üzere yapılandırmanız gerekir. NPS bileşenleri, NPS ile kullanılmak üzere kendi imzalı bir sertifikayı yapılandıran bir Windows PowerShell komut dosyası içerir.

Komut dosyası aşağıdaki eylemleri gerçekleştirir:

* Kendi imzalı sertifika oluşturur
* Azure AD'de ortak sertifika anahtarını hizmet sorumlusuna ilişkilendirin
* Sertifikayı yerel makine mağazasında saklar
* Sertifikanın ağ kullanıcısı için özel anahtarına erişim sağlar
* Ağ Politikası Sunucusu hizmetini yeniden başlatır

Kendi sertifikalarınızı kullanmak istiyorsanız, sertifikanızın ortak anahtarını Azure AD'deki hizmet ilkesiyle ilişkilendirmeniz gerekir.

Komut dosyasını kullanmak için uzantıyı Azure AD Yöneticisi kimlik bilgileriniz ve daha önce kopyaladığınız Azure AD kiracı kimliğiyle birlikte sağlayın. Komut dosyasını, NPS uzantısını yüklediğiniz her NPS sunucusunda çalıştırın. Ardından şunları yapın:

1. Yönetimwindows PowerShell istemini açın.
1. PowerShell komut isteminde `cd ‘c:\Program Files\Microsoft\AzureMfa\Config’` **ENTER**yazın ve enter tuşuna basın.
1. Yazın `.\AzureMfaNpsExtnConfigSetup.ps1`ve **ENTER**tuşuna basın. Komut dosyası, Azure Active Directory PowerShell modülünün yüklü olup olmadığını denetler. Yüklenmezse, komut dosyası modülü sizin için yükler.

   ![Azure'da AzureMfaNpsExtnConfigSetup.ps1'i Azure AD PowerShell'de çalıştırma](./media/howto-mfa-nps-extension-rdg/image4.png)
  
1. Komut dosyası PowerShell modülünün yüklenmesini doğruladıktan sonra Azure Active Directory PowerShell modülü iletişim kutusunu görüntüler. İletişim kutusuna Azure AD yönetici kimlik bilgilerinizi ve parolanızı girin ve **Oturum Aç'ı**tıklatın.

   ![PowerShell'de Azure AD'ye kimlik doğrulama](./media/howto-mfa-nps-extension-rdg/image5.png)

1. İstendiğinde, daha önce kopyaladığınız Dizin Kimliğini panoya yapıştırın ve **ENTER**tuşuna basın.

   ![PowerShell'e Dizin Kimliği gir](./media/howto-mfa-nps-extension-rdg/image6.png)

1. Komut dosyası kendi imzalı bir sertifika oluşturur ve diğer yapılandırma değişikliklerini gerçekleştirir. Çıktı aşağıda gösterilen resim gibi olmalıdır.

   ![PowerShell'in kendi imzalı sertifikasını gösteren çıktısı](./media/howto-mfa-nps-extension-rdg/image7.png)

## <a name="configure-nps-components-on-remote-desktop-gateway"></a>Uzak Masaüstü Ağ Geçidi'nde NPS bileşenlerini yapılandırma

Bu bölümde, Uzak Masaüstü Ağ Geçidi bağlantı yetkilendirme ilkelerini ve diğer RADIUS ayarlarını yapılandırırsınız.

Kimlik doğrulama akışı, RADIUS iletilerinin Uzak Masaüstü Ağ Geçidi ile NPS uzantısının yüklü olduğu NPS sunucusu arasında değiş tokuş edilmesini gerektirir. Bu, RADIUS istemci ayarlarını hem Uzak Masaüstü Ağ Geçidi'nde hem de NPS uzantısının yüklü olduğu NPS sunucusunda yapılandırmanız gerektiği anlamına gelir.

### <a name="configure-remote-desktop-gateway-connection-authorization-policies-to-use-central-store"></a>Merkezi depokullanmak için Uzak Masaüstü Ağ Geçidi bağlantı yetkilendirme ilkelerini yapılandırma

Uzak Masaüstü bağlantı yetkilendirme ilkeleri (RD AP'leri), Uzak Masaüstü Ağ Geçidi sunucusuna bağlanma gereksinimlerini belirtir. RD AP'leri yerel olarak (varsayılan) depolanabilir veya NPS çalıştıran merkezi bir RD CAP deposunda depolanabilir. Azure MFA'nın RDS ile tümleştirilmesini yapılandırmak için merkezi bir mağazanın kullanımını belirtmeniz gerekir.

1. RD Ağ Geçidi sunucusunda **Server Manager'ı**açın.
1. Menüde **Araçlar'ı**tıklatın, **Uzak Masaüstü Hizmetleri'ni**işaret edin ve ardından Uzak Masaüstü Ağ **Geçidi Yöneticisi'ni**tıklatın.
1. RD Ağ Geçidi Yöneticisi'nde ** \[Sunucu\] Adı (Yerel)** seçeneğini sağ tıklatın ve **Özellikler'i**tıklatın.
1. Özellikler iletişim kutusunda **RD CAP Store** sekmesini seçin.
1. RD CAP Store sekmesinde, **NPS çalıştıran Merkezi sunucuyu**seçin. 
1. NPS alanını **çalıştıran sunucunun ad veya IP adresi girin'de, NPS** uzantısını yüklediğiniz sunucunun IP adresini veya sunucu adını yazın.

   ![NPS Sunucunuzun adını veya IP Adresini girin](./media/howto-mfa-nps-extension-rdg/image10.png)
  
1. **Ekle**’ye tıklayın.
1. Paylaşılan **Gizli** iletişim kutusuna paylaşılan bir gizli girin ve ardından **Tamam'ı**tıklatın. Bu paylaşılan sırrı kaydettiğinizi ve kaydı güvenli bir şekilde sakladığınızı sağlayın.

   >[!NOTE]
   >Paylaşılan gizli RADIUS sunucuları ve istemcileri arasında güven kurmak için kullanılır. Uzun ve karmaşık bir sır oluşturun.
   >

   ![Güven oluşturmak için paylaşılan bir sır oluşturma](./media/howto-mfa-nps-extension-rdg/image11.png)

1. **Tamam**’a tıklayarak iletişim kutusunu kapatın.

### <a name="configure-radius-timeout-value-on-remote-desktop-gateway-nps"></a>Uzak Masaüstü Ağ Geçidi NPS'de RADIUS zaman aşım değerini yapılandırma

Kullanıcıların kimlik bilgilerini doğrulamak, iki aşamalı doğrulama gerçekleştirmek, yanıt almak ve RADIUS iletilerine yanıt vermek için zaman olduğundan emin olmak için RADIUS zaman aşma değerini ayarlamak gerekir.

1. RD Ağ Geçidi sunucusunda Server Manager'ı açın. Menüde **Araçlar'ı**tıklatın ve ardından **Ağ İlkesi Sunucusu'nu**tıklatın.
1. **NPS (Yerel)** konsolunda RADIUS **İstemcileri ve Sunucularını**genişletin ve **Uzak RADIUS Sunucusu'nu**seçin.

   ![Remote RADIUS Server'ı gösteren Ağ İlkesi Sunucusu yönetim konsolu](./media/howto-mfa-nps-extension-rdg/image12.png)

1. Ayrıntılar bölmesinde, **TS GATEWAY SERVER GROUP'u**çift tıklatın.

   >[!NOTE]
   >Bu RADIUS Server Group, NPS ilkeleri için merkezi sunucuyu yapılandırdığınızda oluşturulmuştur. RD Ağ Geçidi, RADIUS iletilerini, grupta birden fazla ysa, bu sunucuya veya sunucu grubuna ileter.
   >

1. **TS GATEWAY SERVER GROUP Properties** iletişim kutusunda, RD AP'lerini depolamak için yapılandırdığınız NPS sunucusunun IP adresini veya adını seçin ve ardından **Edit'i**tıklatın.

   ![Daha önce yapılandırılan NPS Sunucusunun IP veya adını seçin](./media/howto-mfa-nps-extension-rdg/image13.png)

1. RADIUS **Server'ı Düzenleme** iletişim kutusunda **Yük Dengeleme** sekmesini seçin.
1. Yük **Dengeleme** sekmesinde, **istek düşürülmeden önce yanıtsız saniye sayısı** olarak kabul edilir, varsayılan değeri 3'ten 30 ile 60 saniye arasında bir değere değiştirin.
1. Sunucu **nun kullanılamayan alan olarak tanımlandığında istekler arasındaki saniye sayısında,** varsayılan değeri 30 saniyeyi önceki adımda belirttiğiniz değere eşit veya daha büyük bir değere değiştirin.

   ![Yük dengeleme sekmesinde Radius Server zaman sonu ayarlarını düzenleme](./media/howto-mfa-nps-extension-rdg/image14.png)

1. İletişim kutularını kapatmak için Iki kez **Tamam'ı** tıklatın.

### <a name="verify-connection-request-policies"></a>Bağlantı İstek İlkelerini Doğrula

Varsayılan olarak, bağlantı yetkilendirme ilkeleri için merkezi bir ilke deposu kullanacak şekilde RD Ağ Geçidi'ni yapılandırdığınızda, RD Ağ Geçidi CAP isteklerini NPS sunucusuna iletecek şekilde yapılandırılır. Azure MFA uzantısı yüklü NPS sunucusu, RADIUS erişim isteğini işler. Aşağıdaki adımlar, varsayılan bağlantı isteği ilkesini nasıl doğrulayabileceğinizi gösterir.  

1. RD Ağ Geçidi'nde, NPS (Yerel) konsolunda, **İlkeler'i**genişletin ve **Bağlantı İstek İlkeleri'ni**seçin.
1. **TS GATEWAY YETKILENDIRME POLİtİkASI'nı**çift tıklatın.
1. **TS GATEWAY YETKILENDIRME POLİtİkASI özellikleri** iletişim kutusunda **Ayarlar** sekmesini tıklatın.
1. **Ayarlar** sekmesinde, Bağlantı İsteğini İleten'in altında **Kimlik Doğrulama'yı**tıklatın. RADIUS istemcisi kimlik doğrulama isteklerini iletmek için yapılandırılır.

   ![Sunucu grubunu belirten Kimlik Doğrulama Ayarlarını Yapılandırma](./media/howto-mfa-nps-extension-rdg/image15.png)

1. **İptal'i**tıklatın.

>[!NOTE]
> Bir bağlantı isteği ilkesi oluşturma hakkında daha fazla bilgi için makaleye bakın, aynı için [bağlantı isteği ilkeleri](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure#add-a-connection-request-policy) belgeleri yapılandırmak. 

## <a name="configure-nps-on-the-server-where-the-nps-extension-is-installed"></a>NPS uzantısının yüklendiği sunucuda NPS'yi yapılandırma

NPS uzantısıyüklü NPS sunucusu uzak masaüstü ağ geçidinde NPS sunucusu ile RADIUS iletileri alışverişi gerekir. Bu ileti alışverişini etkinleştirmek için, NPS uzantı hizmetinin yüklendiği sunucudaki NPS bileşenlerini yapılandırmanız gerekir.

### <a name="register-server-in-active-directory"></a>Aktif Dizine Sunucu Kaydet

Bu senaryoda düzgün çalışabilmek için NPS sunucusunun Active Directory'de kaydedilmesi gerekir.

1. NPS sunucusunda **Server Manager'ı**açın.
1. Server Manager'da **Araçlar'ı**tıklatın ve ardından **Ağ İlkesi Sunucusu'na**tıklayın.
1. Network Policy Server konsolunda **NPS (Yerel) düğmesine**sağ tıklayın ve ardından **Active Directory'de Sunucuyu Kaydet'e**tıklayın.
1. **Tamam'ı** iki kez tıklatın.

   ![NPS sunucusunu Active Directory'de kaydedin](./media/howto-mfa-nps-extension-rdg/image16.png)

1. Bir sonraki yordam için konsolu açık bırakın.

### <a name="create-and-configure-radius-client"></a>RADIUS istemcisi oluşturma ve yapılandırma

Uzak Masaüstü Ağ Geçidi'nin NPS sunucusuna RADIUS istemcisi olarak yapılandırılması gerekir.

1. NPS uzantısının yüklü olduğu NPS sunucusunda, **NPS (Yerel)** konsolunda **RADIUS İstemleri'ni** sağ tıklatın ve **Yeni'yi**tıklatın.

   ![NPS konsolunda Yeni RADIUS İstemci oluşturma](./media/howto-mfa-nps-extension-rdg/image17.png)

1. Yeni **RADIUS İstemci** iletişim kutusunda, _Ağ Geçidi_ve Uzak Masaüstü Ağ Geçidi sunucusunun IP adresi veya DNS adı gibi kolay bir ad sağlayın.
1. Paylaşılan **gizli** ve Paylaşılan gizli alanları **onayla,** daha önce kullandığınız aynı sırrı girin.

   ![Dostu bir adı ve IP veya DNS adresini yapılandırma](./media/howto-mfa-nps-extension-rdg/image18.png)

1. Yeni RADIUS İstemci iletişim kutusunu kapatmak için **Tamam'ı** tıklatın.

### <a name="configure-network-policy"></a>Ağ İlkesi'ni Yapılandırma

Azure MFA uzantılı NPS sunucusunun Bağlantı Yetkilendirme İlkesi (CAP) için belirlenmiş merkezi ilke deposu olduğunu hatırlayın. Bu nedenle, geçerli bağlantı isteklerini yetkilendirmek için NPS sunucusunda bir CAP uygulamanız gerekir.  

1. NPS Sunucusu'nda NPS (Yerel) konsolu açın, **İlkeler'i**genişletin ve **Ağ İlkeleri'ni**tıklatın.
1. **Diğer erişim sunucularına bağlantılara**sağ tıklayın ve **Yinelenen İlke'yi**tıklatın.

   ![Diğer erişim sunucuları ilkesine bağlantı yıkın](./media/howto-mfa-nps-extension-rdg/image19.png)

1. **Diğer erişim sunucularına Bağlantı Kopyala'yı**sağ tıklatın ve **Özellikler'i**tıklatın.
1. Diğer **erişim sunucularına Bağlantı Kopyala** iletişim kutusunda, **İlke adına,** _RDG_CAP_gibi uygun bir ad girin. **İlke etkin**denetleyin ve **Erişim Eve Ver'i**seçin. İsteğe bağlı olarak, **Ağ erişim sunucusu türünde,** Uzak Masaüstü **Ağ Geçidi'ni**seçin veya **belirtilmemiş**olarak bırakabilirsiniz.

   ![İlkeyi adlandırın, etkinleştirin ve erişim izni](./media/howto-mfa-nps-extension-rdg/image21.png)

1. Kısıtlamalar sekmesini tıklatın ve kimlik doğrulama yöntemi üzerinde pazarlık yapmadan **istemcilerin** **bağlanmasına izin ver'i**işaretleyin.

   ![İstemcilerin bağlanmasına izin vermek için kimlik doğrulama yöntemlerini değiştirme](./media/howto-mfa-nps-extension-rdg/image22.png)

1. İsteğe bağlı olarak, **Koşullar** sekmesini tıklatın ve bağlantının yetkilendirilen olması için karşılanması gereken koşulları ekleyin (örneğin, belirli bir Windows grubuna üyelik).

   ![İsteğe bağlı olarak bağlantı koşullarını belirtin](./media/howto-mfa-nps-extension-rdg/image23.png)

1. **Tamam**'a tıklayın. Karşılık gelen Yardım konusunu görüntülemek istendiğinde, **Hayır'ı**tıklatın.
1. Yeni ilkenizin listenin en üstünde olduğundan, ilkenin etkin olduğundan ve erişim sağladığından emin olun.

   ![İlkenizi listenin en üstüne taşıyın](./media/howto-mfa-nps-extension-rdg/image24.png)

## <a name="verify-configuration"></a>Yapılandırmayı doğrulama

Yapılandırmayı doğrulamak için, uygun bir RDP istemcisi ile Uzak Masaüstü Ağ Geçidi'nde oturum açmanız gerekir. Bağlantı Yetkilendirme İlkeleriniz tarafından izin verilen ve Azure MFA için etkinleştirilen bir hesap kullandığınızdan emin olun.

Aşağıdaki resimde de belirtildiği gibi, **Uzak Masaüstü Web Erişimi** sayfasını kullanabilirsiniz.

![Uzak Masaüstü Web Erişiminde Test](./media/howto-mfa-nps-extension-rdg/image25.png)

Birincil kimlik doğrulama için kimlik bilgilerinizi başarıyla girdikten sonra, Uzak Masaüstü Bağlantısı iletişim kutusu aşağıda gösterildiği gibi uzaktan bağlantı başlatma durumunu gösterir. 

Azure MFA'da daha önce yapılandırdığınız ikincil kimlik doğrulama yöntemiyle başarılı bir şekilde kimlik doğrulaması yaptıysanız, kaynağa bağlısınız. Ancak, ikincil kimlik doğrulaması başarılı olmazsa, kaynağa erişiminiz engellenir. 

![Uzak Masaüstü Bağlantısı uzaktan bağlantı başlatma](./media/howto-mfa-nps-extension-rdg/image26.png)

Aşağıdaki örnekte, windows phone'daki Authenticator uygulaması ikincil kimlik doğrulamasını sağlamak için kullanılır.

![Doğrulamayı gösteren örnek Windows Phone Authenticator uygulaması](./media/howto-mfa-nps-extension-rdg/image27.png)

İkincil kimlik doğrulama yöntemini kullanarak başarılı bir şekilde kimlik doğrulaması yaptıktan sonra, Uzak Masaüstü Ağ Geçidi'ne normal olarak giriş yapmış olursunuz. Ancak, güvenilir bir cihazda bir mobil uygulamayı kullanarak ikincil bir kimlik doğrulama yöntemi kullanmanız gerektiğinden, oturum açma işlemi aksi takdirde olduğundan daha güvenlidir.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Başarılı oturum açma etkinlikleri için Olay Görüntüleyicisi günlüğünü görüntüleme

Windows Event Viewer günlüklerinde başarılı oturum açma olaylarını görüntülemek için, Windows Terminal Hizmetleri ve Windows Güvenlik günlüklerini sorgulamak için aşağıdaki Windows PowerShell komutunu düzenleyebilirsiniz.

Ağ Geçidi operasyonel günlüklerinde _(Olay Görüntüleyicisi\Uygulamalar ve Hizmetler Günlükleri\Microsoft\Windows\TerminalServices-Gateway\Operational)_ başarılı oturum açma olaylarını sorgulamak için aşağıdaki PowerShell komutlarını kullanın:

* `Get-WinEvent -Logname Microsoft-Windows-TerminalServices-Gateway/Operational | where {$_.ID -eq '300'} | FL`
* Bu komut, kullanıcının kaynak yetkilendirme ilkesi gereksinimlerini (RD RAP) karşıladığını ve erişim izni verildiğini gösteren Windows olaylarını görüntüler.

![PowerShell kullanarak olayları görüntüleme](./media/howto-mfa-nps-extension-rdg/image28.png)

* `Get-WinEvent -Logname Microsoft-Windows-TerminalServices-Gateway/Operational | where {$_.ID -eq '200'} | FL`
* Bu komut, kullanıcının bağlantı yetkilendirme ilkesi gereksinimlerini karşıladığını gösteren olayları görüntüler.

![PowerShell kullanarak bağlantı yetkilendirme ilkesini görüntüleme](./media/howto-mfa-nps-extension-rdg/image29.png)

Ayrıca bu günlüğü görüntüleyebilir ve olay lı 300 ve 200'de filtreuygulayabilirsiniz. Güvenlik olay görüntüleyicisi günlüklerinde başarılı oturum açma olaylarını sorgulamak için aşağıdaki komutu kullanın:

* `Get-WinEvent -Logname Security | where {$_.ID -eq '6272'} | FL`
* Bu komut, merkezi NPS veya RD Ağ Geçidi Sunucusu'nda çalıştırılabilir.

![Başarılı oturum açma etkinliklerini örnekle](./media/howto-mfa-nps-extension-rdg/image30.png)

Güvenlik günlüğünü veya Ağ İlkesi ve Erişim Hizmetleri özel görünümünü aşağıda gösterildiği gibi görüntüleyebilirsiniz:

![Ağ İlkesi ve Erişim Hizmetleri Etkinlik Görüntüleyici](./media/howto-mfa-nps-extension-rdg/image31.png)

Azure MFA için NPS uzantısını yüklediğiniz sunucuda, _Uygulama ve Hizmetler Günlükleri\Microsoft\AzureMfa'da_uzantıya özgü Olay Görüntüleyen uygulama günlüklerini bulabilirsiniz.

![Olay Görüntüleyici AuthZ uygulama günlükleri](./media/howto-mfa-nps-extension-rdg/image32.png)

## <a name="troubleshoot-guide"></a>Sorun Giderme Kılavuzu

Yapılandırma beklendiği gibi çalışmıyorsa, sorun gidermeye başlamak için ilk sırada kullanıcının Azure MFA kullanacak şekilde yapılandırıldığı doğrulanır. Kullanıcının [Azure portalına](https://portal.azure.com)bağlanmasını sağlar. Kullanıcılardan ikincil doğrulama istenirse ve başarılı bir şekilde kimlik doğrulaması yapılabiliyorsa, Azure MFA'nın yanlış yapılandırması kaldırabilirsiniz.

Azure MFA kullanıcı(lar) için çalışıyorsa, ilgili Etkinlik günlüklerini gözden geçirmelisiniz. Bunlar arasında önceki bölümde tartışılan Güvenlik Olayı, Ağ Geçidi işletim ihdası ve Azure MFA günlükleri yer almaktadır.

Aşağıda, başarısız bir oturum açma olayını (Olay Kimliği 6273) gösteren Güvenlik günlüğünün örnek bir çıktısı verilmiştir.

![Başarısız oturum açma olayı örneği](./media/howto-mfa-nps-extension-rdg/image33.png)

AzureMFA günlüklerinden ilgili bir olay aşağıda verilmiştir:

![Olay Görüntüleyicisi'nde Azure MFA girişine örnek](./media/howto-mfa-nps-extension-rdg/image34.png)

Gelişmiş sorun giderme seçenekleri gerçekleştirmek için, NPS hizmetinin yüklü olduğu NPS veritabanı biçimi günlük dosyalarına başvurun. Bu günlük dosyaları _%SystemRoot%\System32\Logs_ klasöründe virgülle sınırlandırılmış metin dosyaları olarak oluşturulur.

Bu günlük dosyalarının açıklaması için [NPS Veritabanı Biçim Günlüğü Dosyalarını Yorumla'ya](https://technet.microsoft.com/library/cc771748.aspx)bakın. Bu günlük dosyalarındaki girişleri bir elektronik tabloya veya veritabanına aktarmadan yorumlamak zor olabilir. Günlük dosyalarını yorumlamada size yardımcı olmak için çevrimiçi birkaç IAS ayrıştıcı sıyrık bulabilirsiniz.

Aşağıdaki resimde böyle bir indirilebilir [shareware uygulama](https://www.deepsoftware.com/iasviewer)çıktısı gösterir.

![Örnek Shareware uygulaması IAS parser](./media/howto-mfa-nps-extension-rdg/image35.png)

Son olarak, ek sorun giderme seçenekleri için, bir protokol çözümleyicisi kullanabilirsiniz, bu tür [Microsoft İleti Çözümleyicisi](https://technet.microsoft.com/library/jj649776.aspx).

Microsoft İleti Çözümleyicisi'nden aşağıdaki resim, KULLANıCı adı **CONTOSO\AliceC'i**içeren RADIUS protokolünde filtreuygulanmış ağ trafiğini gösterir.

![Filtreuygulanmış trafiği gösteren Microsoft İleti Çözümleyicisi](./media/howto-mfa-nps-extension-rdg/image36.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Multi-Factor Authentication’ı edinme](concept-mfa-licensing.md)

[RADIUS kullanan Uzak Masaüstü Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu](howto-mfaserver-nps-rdg.md)

[Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)
