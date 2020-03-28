---
title: Öğretici - Şirket içi uygulama ekleme - Azure AD'de Uygulama Proxy'si
description: Azure Active Directory (Azure AD), kullanıcıların Azure AD hesaplarıyla oturum açarak şirket içi uygulamalara erişmesini sağlayan bir Uygulama Proxy hizmetine sahiptir. Bu öğretici, Uygulama Proxy ile kullanmak için ortamınızı nasıl hazırlayacağınızı gösterir. Ardından, Azure AD kiracınıza şirket içi uygulama eklemek için Azure portalını kullanır.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: tutorial
ms.date: 10/24/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 73aa01ea08c8bab1395516c31bb46dbfd88045db
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79481424"
---
# <a name="tutorial-add-an-on-premises-application-for-remote-access-through-application-proxy-in-azure-active-directory"></a>Öğretici: Azure Active Directory'de Application Proxy aracılığıyla uzaktan erişim için şirket içi uygulama ekleme

Azure Active Directory (Azure AD), kullanıcıların Azure AD hesaplarıyla oturum açarak şirket içi uygulamalara erişmesini sağlayan bir Uygulama Proxy hizmetine sahiptir. Bu öğretici, Uygulama Proxy ile kullanım için ortamınızı hazırlar. Ortamınız hazır olduğunda, Azure AD kiracınıza şirket içi uygulama eklemek için Azure portalını kullanırsınız.

Bu öğreticide:

> [!div class="checklist"]
> * Giden trafik için bağlantı noktalarını açar ve belirli URL'lere erişim sağlar
> * Bağlayıcıyı Windows sunucunuza yükler ve Uygulama Proxy'ye kaydeder
> * Konektörü doğru yükleyip doğru olarak kaydettiğini doğrular
> * Azure AD kiracınıza şirket içi uygulama ekler
> * Bir test kullanıcısının Azure AD hesabı kullanarak uygulamada oturum açabileceğini doğrular

## <a name="before-you-begin"></a>Başlamadan önce

Azure AD'ye şirket içi bir uygulama eklemek için şunları yapmanız gerekir:

* [Microsoft Azure AD premium aboneliği](https://azure.microsoft.com/pricing/details/active-directory)
* Bir uygulama yöneticisi hesabı
* Kullanıcı kimlikleri şirket içi bir dizinden eşitlenmeli veya doğrudan Azure AD kiracılarınızın içinde oluşturulmalıdır. Kimlik eşitleme, Azure AD'nin kullanıcılara App Proxy tarafından yayınlanan uygulamalara erişim hakkı vermeden önce kullanıcıların kimlik doğrulaması için ön kimlik doğrulamasına ve tek oturum açma (SSO) gerçekleştirmek için gerekli kullanıcı tanımlayıcı bilgilerine sahip olmasını sağlar.

### <a name="windows-server"></a>Windows sunucusu

Application Proxy'yi kullanmak için Windows Server 2012 R2 veya daha yeni bir Windows sunucusu çalıştırmanız gerekir. Uygulama Proxy bağlayıcısını sunucuya yüklersiniz. Bu bağlayıcı sunucunun Azure'daki Uygulama Proxy hizmetlerine ve yayımlamayı planladığınız şirket içi uygulamalara bağlanması gerekir.

Üretim ortamınızda yüksek kullanılabilirlik için birden fazla Windows sunucusuna sahip olmanızı öneririz. Bu öğretici için, bir Windows sunucusu yeterlidir.

> [!IMPORTANT]
> Ekneyi Windows Server 2019'a yüklüyorsanız, bir HTTP2 sınırlaması vardır. Bu sürümde bağlayıcıyı kullanmak için bir geçici çözüm aşağıdaki kayıt defteri anahtarı nı ekleyerek sunucuyu yeniden başlatın. Not, bu bir makine kayıt defteri geniş anahtardır. 
    ```
    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\WinHttp\EnableDefaultHttp2 (DWORD) Value: 0 
    ```

#### <a name="recommendations-for-the-connector-server"></a>Bağlayıcı sunucusu için öneriler

1. Bağlayıcı ve uygulama arasındaki performansı en iyi duruma getirmek için bağlayıcı sunucusunu uygulama sunucularına yakın fiziksel olarak bulun. Daha fazla bilgi için [Ağ topolojisi konularına](application-proxy-network-topology.md)bakın.
1. Bağlayıcı sunucu ve web uygulamaları sunucuları aynı Active Directory etki alanına ait olmalı veya güvenilen etki alanlarını kapsamalıdır. Sunucuların aynı etki alanında olması veya etki alanlarını güvenerek olması, Tümleşik Windows Kimlik Doğrulama (IWA) ve Kerberos Kısıtlı Delegasyonu (KCD) ile tek oturum açma (SSO) kullanmak için bir gerekliliktir. Bağlayıcı sunucu ve web uygulama sunucuları farklı Active Directory etki alanlarındaysa, tek oturum açma için kaynak tabanlı delegasyon kullanmanız gerekir. Daha fazla bilgi [için, Uygulama Proxy ile tek oturum açma için KCD'ye](application-proxy-configure-single-sign-on-with-kcd.md)bakın.

> [!WARNING]
> Azure AD Parola Koruma Proxy'sini dağıttıysanız, Azure AD Application Proxy ve Azure AD Parola Koruma Proxy'sini aynı makineye birlikte yüklemeyin. Azure AD Uygulama Proxy'si ve Azure AD Parola Koruma Proxy' si, Azure AD Connect Agent Updater hizmetinin farklı sürümlerini yükler. Bu farklı sürümler, aynı makineye birlikte yüklendiğinde uyumsuzdur.

#### <a name="tls-requirements"></a>TLS gereksinimleri

Uygulama Proxy bağlayıcısını yüklemeden önce Windows bağlayıcı sunucusunun TLS 1.2'yi etkinleştirilmiş olması gerekir.

TLS 1.2'yi etkinleştirmek için:

1. Aşağıdaki kayıt defteri anahtarlarını ayarlayın:
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

1. Sunucuyu yeniden başlatın.

> [!IMPORTANT]
> Uygulama Proxy hizmeti, müşterilerimize sınıfının en iyisi şifrelemesağlamak için yalnızca TLS 1.2 protokollerine erişimi sınırlandırmaktadır. Bu değişiklikler 31 Ağustos 2019'dan itibaren kademeli olarak kullanıma sunuldu. Uygulama Proxy hizmetine bağlantıyı korumak için tls 1.2 kullanmak üzere tüm istemci-sunucu ve tarayıcı-sunucu kombinasyonlarının güncelleştirdiğinden emin olun. Bunlar, kullanıcılarınızın Application Proxy aracılığıyla yayınlanan uygulamalara erişmek için kullandığı istemcileri içerir. Yararlı referanslar ve kaynaklar için [Office 365'te TLS 1.2'ye](https://support.microsoft.com/help/4057306/preparing-for-tls-1-2-in-office-365) hazırlanma ya da kaynaklara bakın.

## <a name="prepare-your-on-premises-environment"></a>Şirket içi ortamınızı hazırlayın

Ortamınızı Azure AD Application Proxy'ye hazırlamak için Azure veri merkezlerine iletişimi etkinleştirerek başlayın. Yolda bir güvenlik duvarı varsa, açık olduğundan emin olun. Açık güvenlik duvarı, bağlayıcının Uygulama Proxy'sine HTTPS (TCP) isteklerini yapmasına olanak tanır.

### <a name="open-ports"></a>Bağlantı noktalarını açma

Aşağıdaki bağlantı noktalarını **giden** trafiğe açın.

   | Bağlantı noktası numarası | Nasıl kullanılır? |
   | --- | --- |
   | 80 | TLS/SSL sertifikasını doğrularken sertifika iptal listelerini (CRLs) indirme |
   | 443 | Application Proxy hizmeti ile tüm giden iletişim |

Güvenlik duvarınız trafiği gelen kullanıcılara göre zorlarsa, Ağ Hizmeti olarak çalışan Windows hizmetlerinden gelen trafik için 80 ve 443 bağlantı noktalarını da açın.

### <a name="allow-access-to-urls"></a>URL'lere erişime izin verme

Aşağıdaki URL'lere erişime izin verin:

| URL'si | Nasıl kullanılır? |
| --- | --- |
| \*.msappproxy.net<br>\*.servicebus.windows.net | Bağlayıcı ve Uygulama Proxy bulut hizmeti arasındaki iletişim |
| mscrl.microsoft.com:80<br>crl.microsoft.com:80<br>ocsp.msocsp.com:80<br>www.microsoft.com:80 | Bağlayıcı, sertifikaları doğrulamak için bu URL'leri kullanır. |
| login.windows.net<br>secure.aadcdn.microsoftonline-p.com<br>\*.microsoftonline.com<br>\*.microsoftonline-p.com<br>\*.msauth.net<br>\*.msauthimages.net<br>\*.msecnd.net<br>\*.msftauth.net<br>\*.msftauthimages.net<br>\*.phonefactor.net<br>enterpriseregistration.windows.net<br>management.azure.com<br>policykeyservice.dc.ad.msft.net<br>ctdl.windowsupdate.com:80 | Bağlayıcı, kayıt işlemi sırasında bu URL'leri kullanır. |

Güvenlik duvarınız veya \*proxy'niz DNS izin listelerini yapılandırmanıza izin veriyorsa,.msappproxy.net ve \*.servicebus.windows.net bağlantılarına izin verebilirsiniz. Değilse, [Azure IP aralıklarına ve Hizmet Etiketleri - Genel Bulut'a](https://www.microsoft.com/download/details.aspx?id=56519)erişime izin verebilirsiniz. IP aralıkları her hafta güncellenir.

## <a name="install-and-register-a-connector"></a>Konektör yükleme ve kaydetme

Application Proxy'yi kullanmak için, Uygulama Proxy hizmetiyle kullandığınız her Windows sunucusuna bir bağlayıcı yükleyin. Bağlayıcı, şirket içi uygulama sunucularından Azure AD'deki Application Proxy'ye giden bağlantıyı yöneten bir aracıdır. Azure AD Connect gibi diğer kimlik doğrulama aracıları da yüklü olan sunucularda bir bağlayıcı yükleyebilirsiniz.

Konektörü yüklemek için:

1. Uygulama Proxy'sini kullanan dizinin uygulama yöneticisi olarak [Azure portalında](https://portal.azure.com/) oturum açın. Örneğin, kiracı etki alanı contoso.com, yönetici admin@contoso.com veya bu etki alanında başka bir yönetici takma olmalıdır.
1. Sağ üst köşede kullanıcı adınızı seçin. Uygulama Proxy'sini kullanan bir dizinde oturum unuzu imzaladığınızı doğrulayın. Dizin değiştirmeniz gerekiyorsa, **Dizin Değiştir'i** seçin ve Uygulama Proxy'sini kullanan bir dizin seçin.
1. Sol gezinti panelinde **Azure Etkin Dizin'i**seçin.
1. **Yönet**altında, **Uygulama proxy'si**seçin.
1. **İndir bağlayıcı hizmetini**seçin.

    ![Hizmet Koşullarını görmek için konektör hizmetini indirin](./media/application-proxy-add-on-premises-application/application-proxy-download-connector-service.png)

1. Hizmet Koşullarını okuyun. Hazır olduğunuzda, Kabul **koşullarını seçin & İndir.**
1. Pencerenin alt kısmında, konektörü yüklemek için **Çalıştır'ı** seçin. Yükleme sihirbazı açılır.
1. Hizmeti yüklemek için sihirbazdaki yönergeleri izleyin. Azure AD kiracınız için bağlayıcıyı Uygulama Proxy'sine kaydetmeniz istendiğinde, uygulama yöneticisi kimlik bilgilerinizi girin.
    - Internet Explorer (IE) için, **IE Gelişmiş Güvenlik Yapılandırması** **A)'ya**ayarlanmışsa, kayıt ekranını göremeyebilirsiniz. Erişim için hata iletisindeki yönergeleri izleyin. **Internet Explorer Gelişmiş Güvenlik Yapılandırması'nın** **Kapalı**olarak ayarlandıklarına emin olun.

### <a name="general-remarks"></a>Genel açıklamalar

Daha önce bir bağlayıcı yüklediyseniz, en son sürümü almak için yeniden yükleyin. Daha önce yayımlanan sürümler ve hangi değişiklikleri içerdiği hakkında bilgi görmek için [Bkz. Uygulama Proxy: Sürüm Yayın Geçmişi](application-proxy-release-version-history.md).

Şirket içi uygulamalarınız için birden fazla Windows sunucusuna sahip olmayı seçerseniz, bağlayıcıyı her sunucuya yüklemeniz ve kaydetmeniz gerekir. Bağlayıcıları bağlayıcı gruplar halinde düzenleyebilirsiniz. Daha fazla bilgi için [Bağlayıcı gruplarına](application-proxy-connector-groups.md)bakın.

Kuruluşunuz internete bağlanmak için proxy sunucuları kullanıyorsa, bunları Uygulama Proxy'si için yapılandırmanız gerekir.  Daha fazla bilgi için, [mevcut şirket içi proxy sunucularıyla çalışma'ya](application-proxy-configure-connectors-with-proxy-servers.md)bakın. 

Bağlayıcılar, kapasite planlaması ve bunların nasıl güncel kaldığı hakkında bilgi [için](application-proxy-connectors.md)bkz.

## <a name="verify-the-connector-installed-and-registered-correctly"></a>Konektörüdoğru yüklü ve kaydedilmiş doğrulayın

Yeni bir bağlayıcının doğru yüklendiğinden onaylamak için Azure portalını veya Windows sunucunuzu kullanabilirsiniz.

### <a name="verify-the-installation-through-azure-portal"></a>Yüklemeyi Azure portalı üzerinden doğrulama

Konektörün doğru şekilde yüklenmesini ve kaydedildiğini doğrulamak için:

1. [Azure portalında](https://portal.azure.com)kiracı dizininizde oturum açın.
1. Soldaki gezinti panelinde **Azure Etkin Dizini'ni**seçin ve ardından **Yönet** bölümü altında **Uygulama Proxy'sini** seçin. Tüm bağlayıcılarınız ve bağlayıcı gruplarınız bu sayfada görünür.
1. Ayrıntılarını doğrulamak için bir bağlayıcıyı görüntüleyin. Bağlayıcılar varsayılan olarak genişletilmelidir. Görüntülemek istediğiniz bağlayıcı genişletilmezse, ayrıntıları görüntülemek için bağlayıcıyı genişletin. Etkin bir yeşil etiket, bağlayıcınızın hizmete bağlanabileceğini gösterir. Ancak, etiket yeşil olsa da, bir ağ sorunu yine de bağlayıcının ileti almasını engelleyebilir.

    ![Azure AD Uygulama Proxy Bağlayıcıları](./media/application-proxy-add-on-premises-application/app-proxy-connectors.png)

Bağlayıcı yükleme konusunda daha fazla yardım için Uygulama [Proxy Bağlayıcısı'nı yükleme sorunu'na](application-proxy-connector-installation-problem.md)bakın.

### <a name="verify-the-installation-through-your-windows-server"></a>Yüklemeyi Windows sunucunuz aracılığıyla doğrulama

Konektörün doğru şekilde yüklenmesini ve kaydedildiğini doğrulamak için:

1. **Windows** anahtarına tıklayıp *services.msc'ye*girerek Windows Hizmetleri Yöneticisi'ni açın.
1. Aşağıdaki iki hizmetin durumunun **çalışıyor**olup olmadığını denetleyin.
   - **Microsoft AAD Application Proxy Bağlayıcısı** bağlantı sağlar.
   - **Microsoft AAD Application Proxy Bağlayıcısı Güncelleyici** otomatik bir güncelleştirme hizmetidir. Updateer bağlayıcının yeni sürümlerini denetler ve gerektiğinde bağlayıcıyı güncelleştirir.

     ![Uygulama Ara Sunucusu Bağlayıcısı hizmetleri - ekran görüntüsü](./media/application-proxy-add-on-premises-application/app_proxy_services.png)

1. Hizmetlerin durumu Çalışmıyorsa, **Running**her hizmeti seçmek için sağ tıklatın ve **Başlat'ı**seçin.

## <a name="add-an-on-premises-app-to-azure-ad"></a>Azure AD'ye şirket içi uygulama ekleme

Artık ortamınızı hazırladığınıza ve bir bağlayıcı yüklediğinize göre, Azure AD'ye şirket içi uygulamalar eklemeye hazırsınız.  

1. [Azure portalında](https://portal.azure.com/)yönetici olarak oturum açın.
2. Sol daki gezinti panelinde **Azure Etkin Dizin'i**seçin.
3. **Kurumsal uygulamaları**seçin ve ardından **Yeni uygulamayı**seçin.
4. Şirket **içi uygulamalar** bölümünde, **şirket içi uygulama ekle'yi**seçin.
5. Kendi **şirket içi uygulama ekle** bölümünde, başvurunuz hakkında aşağıdaki bilgileri sağlayın:

    | Alan | Açıklama |
    | :---- | :---------- |
    | **Adı** | Erişim panelinde ve Azure portalında görünecek uygulamanın adı. |
    | **Dahili URL** | Uygulamaya özel ağınızın içinden erişme URL'si. Arka uç sunucusundaki belirli bir yolun yayımlanmasını sağlayabilirsiniz. Sunucunun geri kalanı yayımlanmaz. Bu şekilde, farklı uygulamalarla aynı sunucuda farklı siteler yayımlayabilir ve her birine kendi adını ve erişim kurallarını verebilirsiniz.<br><br>Bir yol yayımlarsanız uygulamanıza ilişkin tüm gerekli görüntüleri, betikleri ve stil sayfalarını içerdiğinden emin olun. Örneğin, uygulamanız https:\//yourapp/app adresindeyse ve https:\//yourapp/media adresinde bulunan görüntüleri\/kullanıyorsa, https: /yourapp/ yol olarak yayınlamalısınız. Bu dahili URL, kullanıcılarınızın gördüğü açılış sayfası olmak zorunda değildir. Daha fazla bilgi için [bkz.](application-proxy-configure-custom-home-page.md) |
    | **Dış URL** | Kullanıcıların uygulamaya anızın dışından erişme adresi. Varsayılan Uygulama Proxy etki alanını kullanmak istemiyorsanız, [Azure AD Application Proxy'deki özel etki alanları](application-proxy-configure-custom-domain.md)hakkında bilgi edinin.|
    | **Ön Kimlik Doğrulama** | Uygulama Proxy'si, uygulamanız için erişim vermeden önce kullanıcıları nasıl doğrular.<br><br>**Azure Etkin Dizin** - Uygulama Proxy, kullanıcıları, dizin ve uygulama için izinlerini doğrulayan Azure AD ile oturum açmaya yönlendirir. Koşullu Erişim ve Çok Faktörlü Kimlik Doğrulama gibi Azure REKLAM güvenlik özelliklerinden yararlanabilmeniz için bu seçeneği varsayılan olarak tutmanızı öneririz. Uygulamayı Microsoft Cloud Application Security ile izlemek için **Azure Active Directory** gereklidir.<br><br>**Geçiş** - Kullanıcıların uygulamaya erişmek için Azure AD'ye karşı kimlik doğrulaması gerekmez. Arka uçta kimlik doğrulama gereksinimlerini ayarlamaya devam edebilirsiniz. |
    | **Konektör Grubu** | Bağlayıcılar uygulamanıza uzaktan erişimi işler ve bağlayıcı grupları bağlayıcıları ve uygulamaları bölgeye, ağa veya amaca göre düzenlemenize yardımcı olur. Henüz oluşturulmuş herhangi bir bağlayıcı grubunyoksa, uygulamanız **Varsayılan**olarak atanır.<br><br>Uygulamanız bağlanmak için WebSockets kullanıyorsa, gruptaki tüm bağlayıcılar sürüm 1.5.612.0 veya sonraki olmalıdır.|

6. Gerekirse, Ek **ayarları**yapılandırın. Çoğu uygulama için, bu ayarları varsayılan durumlarında tutmanız gerekir. 

    | Alan | Açıklama |
    | :---- | :---------- |
    | **Backend Uygulama Zaman Sonu** | Bu değeri yalnızca uygulamanızın kimlik doğrulaması ve bağlanması yavaşsa **Uzun** olarak ayarlayın. Varsayılan olarak, arka uç uygulama zaman sonu 85 saniye uzunluğundadır. Uzun akadar ayarlandığında, arka uç zaman sonu 180 saniyeye çıkarılır. |
    | **Yalnızca HTTP Çerezi Kullanma** | Uygulama Proxy tanımlama bilgilerinin HTTP yanıt üstbilgisinde HTTPOnly bayrağını içermesi için bu değeri **Evet** olarak ayarlayın. Uzak Masaüstü Hizmetlerini kullanıyorsanız, bu değeri **Hayır**olarak ayarlayın.|
    | **Güvenli Çerez Kullanın**| Çerezleri şifreli BIR HTTPS isteği gibi güvenli bir kanal üzerinden iletmek için bu değeri **Evet** olarak ayarlayın.
    | **Kalıcı Çerez Kullanın**| Bu değeri **No**olarak ayarlayın. Bu ayarı yalnızca işlemler arasında tanımlama bilgileri paylaşılabilen uygulamalar için kullanın. Çerez ayarları hakkında daha fazla bilgi için [Azure Etkin Dizini'ndeki şirket içi uygulamalara erişmek için Çerez ayarlarına](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings)bakın.
    | **URL'leri Üstbilgide Çevir** | Uygulamanız kimlik doğrulama isteğinde özgün ana bilgisayar üstbilgisini gerektirmedikçe bu değeri **Evet** olarak tutun. |
    | **URL'leri Uygulama Gövdesinde Çevir** | Diğer şirket içi uygulamalara uygun HTML bağlantıları yoksa ve özel etki alanları kullanmadığınız sürece bu değeri **Hayır** olarak tutun. Daha fazla bilgi için Uygulama [Proxy ile Bağlantı çevirisine](application-proxy-configure-hard-coded-link-translation.md)bakın.<br><br>Bu uygulamayı Microsoft Cloud App Security (MCAS) ile izlemeyi planlıyorsanız, bu değeri **Evet** olarak ayarlayın. Daha fazla bilgi için bkz: [Microsoft Cloud App Security ve Azure Active Directory ile gerçek zamanlı uygulama erişim izlemeyi yapılandırma.](application-proxy-integrate-with-microsoft-cloud-application-security.md) |

7. **Ekle'yi**seçin.

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulamanın doğru eklenmesini test etmeye hazırsınız. Aşağıdaki adımlarda, uygulamaya bir kullanıcı hesabı ekler ve oturum açmayı denersiniz.

### <a name="add-a-user-for-testing"></a>Test için kullanıcı ekleme

Uygulamaya bir kullanıcı eklemeden önce, kullanıcı hesabının uygulamaya şirket ağı içinden erişmek için zaten izinleri olduğunu doğrulayın.

Test kullanıcısı eklemek için:

1. **Kurumsal uygulamaları**seçin ve ardından test etmek istediğiniz uygulamayı seçin.
2. **Başlarken'i**seçin ve ardından **test için kullanıcı atay'ı**seçin.
3. **Kullanıcılar ve gruplar**altında, kullanıcı **ekle'yi**seçin.
4. **Atama Ekle** **altında, Kullanıcıları ve grupları**seçin. **Kullanıcı ve gruplar** bölümü görüntülenir.
5. Eklemek istediğiniz hesabı seçin.
6. **Seç'i**seçin ve sonra **Atla'yı**seçin.

### <a name="test-the-sign-on"></a>Oturum açmayı test edin

Uygulamanın oturum açmasını test etmek için:

1. Test etmek istediğiniz uygulamadan **Uygulama Proxy'sini**seçin.
2. Sayfanın üst kısmında, uygulama üzerinde bir test çalıştırmak ve yapılandırma sorunlarını denetlemek için **Uygulama** Test'i seçin.
3. Uygulamada oturum açmatest etmek için uygulamayı ilk olarak başlattığından, ardından algılanan sorunlar için çözüm kılavuzunu gözden geçirmek için tanılama raporunu indirdiğinden emin olun.

Sorun giderme için, [Sorun Giderme Uygulaması Proxy sorunları ve hata iletileri](application-proxy-troubleshoot.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Uygulama Proxy ile çalışmak için şirket içi ortamınızı hazırladınız ve ardından Application Proxy bağlayıcısını yüklediniz ve kaydettiniz. Ardından, Azure AD kiracınıza bir uygulama eklediniz. Bir kullanıcının azure REKLAM hesabı kullanarak uygulamada oturum açabileceğini doğruladınız.

Şu işlemleri yaptınız:
> [!div class="checklist"]
> * Giden trafik için açılan bağlantı noktaları ve belirli URL'lere erişime izin
> * Bağlayıcıyı Windows sunucunuza yükledi ve Uygulama Proxy'ye kaydettirdi
> * Konektörüdoğrulanmış ve doğru olarak kaydetmiş
> * Azure AD kiracınıza şirket içi uygulama eklendi
> * Bir test kullanıcısının Azure AD hesabı kullanarak uygulamada oturum açabileceği doğrulandı

Uygulamayı tek oturum açma için yapılandırmaya hazırsınız. Tek bir oturum açma yöntemi seçmek ve tek oturum açma öğreticilerini bulmak için aşağıdaki bağlantıyı kullanın.

> [!div class="nextstepaction"]
> [Çoklu oturum açmayı yapılandırma](what-is-single-sign-on.md#choosing-a-single-sign-on-method)
