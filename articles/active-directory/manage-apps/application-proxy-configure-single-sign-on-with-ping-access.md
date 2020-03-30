---
title: Azure AD Application Proxy için PingAccess ile üstbilgi tabanlı kimlik doğrulama | Microsoft Dokümanlar
description: Üstbilgi tabanlı kimlik doğrulamasını desteklemek için PingAccess ve App Proxy ile uygulamaları yayımlayın.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: celested
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: f3fb94629262519f8cfa5da72ee343726aa7d1c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77367990"
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a>Application Proxy ve PingAccess ile tek oturum açma için üstbilgi tabanlı kimlik doğrulama

Azure AD müşterilerinizin daha fazla uygulamanıza erişebilmeleri için Azure Active Directory (Azure AD) Application Proxy, PingAccess ile ortaklık kurmuştur. [PingAccess, varolan Uygulama Proxy tekliflerini](application-proxy.md) kimlik doğrulama için üstbilgi kullanan uygulamalara tek oturum açma erişimi içerecek şekilde genişletir.

## <a name="whats-pingaccess-for-azure-ad"></a>Azure AD için PingAccess nedir?

Azure AD için PingAccess ile kullanıcılara kimlik doğrulama için üstbilgi kullanan uygulamalara erişim ve tek oturum açma (SSO) verebilirsiniz. Application Proxy, erişimin kimliğini doğrulamak için Azure AD'yi kullanarak ve ardından bağlayıcı hizmetinden trafiği geçerek bu uygulamaları diğerleri gibi ele verir. PingAccess uygulamaların önünde yer eder ve Azure AD'den gelen erişimi bir üstbilgiye çevirir. Uygulama daha sonra kimlik doğrulamasını okuyabileceği biçimde alır.

Kullanıcılarınız kurumsal uygulamalarınızı kullanmak için oturum açken farklı bir şey fark etmez. Herhangi bir cihazüzerinde herhangi bir yerden çalışabilirler. Application Proxy bağlayıcıları, kimlik doğrulama türüne bakılmaksızın tüm uygulamalara uzak trafiği yönlendirir, böylece yükleri otomatik olarak dengeler.

## <a name="how-do-i-get-access"></a>Nasıl erişebilirim?

Bu senaryo Azure Active Directory ve PingAccess arasındaki bir ortaklıktan geldiği için, her iki hizmet için de lisans gerekir. Ancak Azure Active Directory Premium abonelikleri, 20'ye kadar uygulamayı kapsayan temel bir PingAccess lisansIçerir. 20'den fazla üstbilgi tabanlı uygulama yayımlamanız gerekiyorsa, PingAccess'ten ek bir lisans satın alabilirsiniz.

Daha fazla bilgi için [Azure Etkin Dizin sürümlerine](../fundamentals/active-directory-whatis.md)bakın.

## <a name="publish-your-application-in-azure"></a>Uygulamanızı Azure'da yayımlayın

Bu makale, insanların bu senaryoya sahip bir uygulamayı ilk kez yayımlamaları içindir. Yayımlama adımlarını detaylandırmanın yanı sıra, hem Application Proxy hem de PingAccess ile başlamanızda size yol gösteriyor. Her iki hizmeti de zaten yapılandırmış ancak yayımlama adımlarında yenileyici istiyorsanız, Uygulama Proxy'si ile [uygulamanızı Azure AD'ye ekle bölümüne](#add-your-application-to-azure-ad-with-application-proxy) atlayın.

> [!NOTE]
> Bu senaryo Azure AD ve PingAccess arasında bir ortaklık olduğundan, bazı yönergeler Ping Identity sitesinde bulunmaktadır.

### <a name="install-an-application-proxy-connector"></a>Uygulama Proxy bağlayıcısı yükleme

Uygulama Proxy'yi etkinleştirdiyseniz ve zaten bir bağlayıcı yüklediyseniz, bu bölümü atlayabilir ve [Uygulama Proxy ile uygulamanızı Azure AD'ye ekle'ye](#add-your-application-to-azure-ad-with-application-proxy)gidebilirsiniz.

Application Proxy bağlayıcısı, uzak çalışanlarınızdan gelen trafiği yayınlanmış uygulamalarınıza yönlendiren bir Windows Server hizmetidir. Daha ayrıntılı yükleme yönergeleri için [Bkz. Öğretici: Azure Active Directory'de Application Proxy aracılığıyla uzaktan erişim için şirket içi uygulama ekleyin.](application-proxy-add-on-premises-application.md)

1. Uygulama yöneticisi olarak [Azure Active Directory portalında](https://aad.portal.azure.com/) oturum açın. **Azure Etkin Dizin yöneticisi merkezi** sayfası görüntülenir.
1. **Azure Active Directory** > **Application proxy** > **İndir bağlayıcı hizmetini**seçin. **Uygulama Proxy Bağlayıcı İndirme** sayfası görüntülenir.

   ![Uygulama proxy bağlayıcı indir](./media/application-proxy-configure-single-sign-on-with-ping-access/application-proxy-connector-download.png)

1. Yükleme yönergelerini izleyin.

Bağlayıcıyı indirmek dizininiz için Uygulama Proxy'sini otomatik olarak etkinleştirmeli, ancak indirmiyorsa **Uygulama Proxy'sini etkinleştir'i**seçebilirsiniz.

### <a name="add-your-application-to-azure-ad-with-application-proxy"></a>Application Proxy ile uygulamanızı Azure AD'ye ekleyin

Azure portalında yapmanız gereken iki eylem vardır. İlk olarak, Uygulama Proxy ile uygulamanızı yayımlamanız gerekir. Ardından, PingAccess adımları sırasında kullanabileceğiniz uygulama hakkında bazı bilgiler toplamanız gerekir.

#### <a name="publish-your-application"></a>Uygulamanızı yayımlama

Önce başvurunuzu yayınlamanız gerekir. Bu eylem şunları içerir:

- Şirket içi uygulamanızı Azure AD'ye ekleme
- Uygulamayı test etmek için bir kullanıcı atama ve üstbilgi tabanlı SSO'yu seçme
- Uygulamanın yönlendirme URL'sini ayarlama
- Şirket içi uygulamanızı kullanma ları için kullanıcılara ve diğer uygulamalara izin verme

Kendi şirket içi başvurunuzu yayınlamak için:

1. Son bölümde yapmadıysanız, uygulama yöneticisi olarak [Azure Active Directory portalında](https://aad.portal.azure.com/) oturum açın.
1. **Kurumsal uygulamaları** > seçin**Yeni uygulama** > **Şirket içi uygulama ekleyin.** **Kendi şirket içi uygulama sayfanızı ekle** görüntülenir.

   ![Kendi şirket içi uygulamanızı ekleyin](./media/application-proxy-configure-single-sign-on-with-ping-access/add-your-own-on-premises-application.png)
1. Yeni uygulamanız hakkında bilgi içeren gerekli alanları doldurun. Ayarlar için aşağıdaki kılavuzu kullanın.

   > [!NOTE]
   > Bu adımın daha ayrıntılı bir şekilde gözden geçirincesi için Azure [AD'ye şirket içi uygulama ekleme bölümüne](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad)bakın.

   1. **Dahili URL**: Normalde, şirket ağındayken sizi uygulamanın oturum açma sayfasına götüren URL'yi sağlarsınız. Bu senaryo için bağlayıcının PingAccess proxy'sini uygulamanın ön sayfası olarak ele alması gerekir. Bu biçimi `https://<host name of your PingAccess server>:<port>`kullanın: . Bağlantı noktası varsayılan olarak 3000'dir, ancak PingAccess'te yapılandırabilirsiniz.

      > [!WARNING]
      > Bu tür tek oturum açma için, dahili URL'nin kullanması `https` ve kullanamayacağı. `http`

   1. **Ön kimlik doğrulama yöntemi**: **Azure Etkin Dizini'ni**seçin.
   1. **URL'yi Üstbilgide Çevir**: **Hayır'ı**seçin.

   > [!NOTE]
   > Bu ilk uygulamanızsa, PingAccess yapılandırmanızı değiştirirseniz bu ayarı güncelleştirmek için başlangıç ve geri gelmek için 3000 bağlantı noktasını kullanın. Sonraki uygulamalar için bağlantı noktasının PingAccess'te yapılandırdığınız Dinleyiciyle eşleşmesi gerekir. [PingAccess'teki dinleyiciler](https://support.pingidentity.com/s/document-item?bundleId=pingaccess-52&topicId=reference/ui/pa_c_Listeners.html)hakkında daha fazla bilgi edinin.

1. **Ekle'yi**seçin. Yeni uygulama için genel bakış sayfası görüntülenir.

Şimdi uygulama testi için bir kullanıcı atayın ve üstbilgi tabanlı tek oturum açma seçin:

1. Uygulama kenar çubuğundan, **Kullanıcıları ve grupları** > seçin**Kullanıcı** > **Kullanıcılarını ve gruplarını ekle (Seçili\<Sayı>)**. Aralarından seçim yapabileceğiniz kullanıcıların ve grupların listesi görüntülenir.

   ![Kullanıcı ve grupların listesini gösterir](./media/application-proxy-configure-single-sign-on-with-ping-access/users-and-groups.png)

1. Uygulama testi için bir kullanıcı seçin ve **Seç'i**seçin. Bu test hesabının şirket içi uygulamaya erişimi olduğundan emin olun.
1. **Ata**'yı seçin.
1. Uygulama kenar çubuğundan, **Tek oturum açma** > **Üstbilgi tabanlı'nı**seçin.

   > [!TIP]
   > Üstbilgi tabanlı tek oturum açma yı ilk kez kullanıyorsanız PingAccess'i yüklemeniz gerekir. Azure aboneliğinizin PingAccess kurulumunuzla otomatik olarak ilişkilendirildiğinden emin olmak için PingAccess'i indirmek için bu tek oturum açma sayfasındaki bağlantıyı kullanın. İndirme sitesini şimdi açabilir veya daha sonra bu sayfaya geri dönebilirsiniz.

   ![Üstbilgi tabanlı oturum açma ekranı ve PingAccess'i gösterir](./media/application-proxy-configure-single-sign-on-with-ping-access/sso-header.png)

1. **Kaydet'i**seçin.

Ardından yönlendirme URL'nizin harici URL'nize ayarlandıklarına emin olun:

1. Azure **Active Directory yöneticisi merkezi** kenar çubuğundan **Azure Active Directory** > **App kayıtlarını**seçin. Uygulamaların listesi görüntülenir.
1. Uygulamanızı seçin.
1. Web ve ortak istemciler için ayarlanan yeniden yönlendirme **URI'lerinin**sayısını gösteren Yönlendirme URL'lerinin yanındaki bağlantıyı seçin. ** \<Uygulama adı> - Kimlik doğrulama** sayfası görüntülenir.
1. Uygulamanıza daha önce atadığınız harici URL'nin **Yönlendirme URI'leri** listesinde olup olmadığını denetleyin. Değilse, **web**yeniden yönlendirmek URI türünü kullanarak şimdi harici URL ekleyin ve **Kaydet'i**seçin.

Son olarak, kullanıcıların erişimi okuyup diğer uygulamaların okuma/yazma erişimine sahip olması için şirket içi uygulamanızı ayarlayın:

1. Uygulamanız için **Uygulama kayıtları** kenar çubuğundan, **API izinlerini** > seçin**Microsoft API'leri** > **Microsoft Graph**izni > **ekleyin.** **Microsoft Graph** için **İstek API izinleri** sayfası, Windows Azure Etkin Dizin apilerini içeren görünür.

   ![İstek API izinleri sayfasını gösterir](./media/application-proxy-configure-single-sign-on-with-ping-access/required-permissions.png)

1. **Temsilcileştirilmiş izinleri** > **seçin Kullanıcı** > **Kullanıcı.Oku**.
1. **Uygulama İzinleri** > **Uygulama.ReadWrite.All****Application** > seçin .
1. **İzin Ekle'yi**seçin.
1. **API izinleri** sayfasında, **dizin \<adınız için yönetici onayı>. **

#### <a name="collect-information-for-the-pingaccess-steps"></a>PingAccess adımları için bilgi toplama

Uygulamanızı PingAccess ile ayarlamak için bu üç bilgi parçasını (tüm GUID'ler) toplamanız gerekir:

| Azure AD alanının adı | PingAccess alanının adı | Veri biçimi |
| --- | --- | --- |
| **Uygulama (istemci) kimliği** | **İstemci Kimliği** | GUID |
| **Dizin (kiracı) kimliği** | **Veren** | GUID |
| `PingAccess key` | **Müşteri Sırrı** | Rasgele dize |

Bu bilgileri toplamak için:

1. Azure **Active Directory yöneticisi merkezi** kenar çubuğundan **Azure Active Directory** > **App kayıtlarını**seçin. Uygulamaların listesi görüntülenir.
1. Uygulamanızı seçin. Uygulamanızın **Uygulama kayıtları** sayfası görüntülenir.

   ![Bir uygulama için kayıt genel bakışı](./media/application-proxy-configure-single-sign-on-with-ping-access/registration-overview-for-an-application.png)

1. **Uygulama (istemci) kimlik** değerinin yanında, **pano simgesine** Kopyala'yı seçin, ardından kopyalayın ve kaydedin. Bu değeri daha sonra PingAccess'in istemci kimliği olarak belirtirsiniz.
1. Ardından **Dizin (kiracı) kimlik** değeri, ayrıca **panoya**Kopyala'yı seçin, ardından kopyalayıp kaydedin. Bu değeri daha sonra PingAccess'in vereni olarak belirtirsiniz.
1. Uygulamanız için **Uygulama kayıtlarının** kenar çubuğundan **Sertifikalar ve sırlar** > Yeni**istemci sırrı**seçin. **İstemci gizli** sayfası ekle görüntülenir.

   ![İstemci gizli sayfa ekle'yi gösterir](./media/application-proxy-configure-single-sign-on-with-ping-access/add-a-client-secret.png)

1. **Tanımolarak**, `PingAccess key`türü .
1. **Expires**altında , PingAccess anahtarını ayarlamak için nasıl seçin: **1 yıl içinde**, 2 yıl **içinde**, veya **asla**.
1. **Ekle'yi**seçin. PingAccess tuşu, **DEĞER** alanında otomatik olarak dolduran rasgele bir dizeyle istemci sırları tablosunda görünür.
1. PingAccess anahtarının **VALUE** alanının yanında, **pano simgesine** Kopyala'yı seçin, ardından kopyalayıp kaydedin. Bu değeri daha sonra PingAccess'in istemci sırrı olarak belirtirsiniz.

**Alanı `acceptMappedClaims` güncelleyin:**

1. Uygulama yöneticisi olarak [Azure Active Directory portalında](https://aad.portal.azure.com/) oturum açın.
1. **Azure Active Directory** > **App kayıtlarını**seçin. Uygulamaların listesi görüntülenir.
1. Uygulamanızı seçin.
1. Uygulamanız için **Uygulama kayıtları** sayfasının kenar çubuğundan **Manifest'i**seçin. Başvurunuzun kaydı için bildirim JSON kodu görüntülenir.
1. `acceptMappedClaims` Alanı arayın ve değeri ' `True`ye değdirin.
1. **Kaydet'i**seçin.

### <a name="use-of-optional-claims-optional"></a>İsteğe bağlı taleplerin kullanımı (isteğe bağlı)

İsteğe bağlı talepler, her kullanıcının ve kiracının sahip olduğu standart ancak dahil olmayan varsayılan talepler eklemenize olanak tanır. Uygulama bildirimini değiştirerek uygulamanız için isteğe bağlı talepleri yapılandırabilirsiniz. Daha fazla bilgi için Azure [REKLAM uygulama bildirimini anlama makalesine](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest/) bakın

PingAccess'in tüketeceği access_token e-posta adresini eklemek için örnek:
```
    "optionalClaims": {
        "idToken": [],
        "accessToken": [
            {
                "name": "email",
                "source": null,
                "essential": false,
                "additionalProperties": []
            }
        ],
        "saml2Token": []
    },
```

### <a name="use-of-claims-mapping-policy-optional"></a>Talep eşleme ilkesinin kullanımı (isteğe bağlı)

AzureAD'da bulunmayan öznitelikler için [Talepler Eşleme Politikası (önizleme).](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping#claims-mapping-policy-properties) Talep eşleme, ADFS veya kullanıcı nesneleriniz tarafından desteklenen ek özel talepler ekleyerek eski prem uygulamaları buluta geçirmenize olanak tanır

Uygulamanızın özel bir talep kullanmasını ve ek alanlar eklemesini sağlamak [için, özel bir talep eşleme ilkesi oluşturduğunuzdan ve uygulamaya atadığınızdan](../develop/active-directory-claims-mapping.md#claims-mapping-policy-assignment)emin olun.

> [!NOTE]
> Özel bir talep kullanmak için, özel bir ilke tanımlı ve uygulamaya atanmış olması gerekir. Bu ilke, gerekli tüm özel öznitelikleri içermelidir.
>
> PowerShell veya Microsoft Graph aracılığıyla ilke tanımı ve atama yapabilirsiniz. PowerShell'de yapıyorsanız, önce kullanmanız `New-AzureADPolicy` ve daha sonra uygulamaya atamanız `Add-AzureADServicePrincipalPolicy`gerekebilir. Daha fazla bilgi için [bkz.](../develop/active-directory-claims-mapping.md#claims-mapping-policy-assignment)

Örnek:
```powershell
$pol = New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema": [{"Source":"user","ID":"employeeid","JwtClaimType":"employeeid"}]}}') -DisplayName "AdditionalClaims" -Type "ClaimsMappingPolicy"

Add-AzureADServicePrincipalPolicy -Id "<<The object Id of the Enterprise Application you published in the previous step, which requires this claim>>" -RefObjectId $pol.Id
```

### <a name="enable-pingaccess-to-use-custom-claims"></a>Özel talepleri kullanmak için PingAccess'i etkinleştirin

PingAccess'in özel talepler kullanmasını etkinleştirmek isteğe bağlıdır, ancak uygulamanın ek talepler de kullanmasını bekliyorsanız gereklidir.

PingAccess'i aşağıdaki adımda yapılandıracağınız da, oluşturacağınız Web Oturumu (Ayarlar >Access->Web Oturumları) **İstek Profilinin** seçilmemiş olması ve **Kullanıcı Özniteliklerini** **Yenile'nin Hayır** olarak ayarlamalıdır

## <a name="download-pingaccess-and-configure-your-application"></a>PingAccess'i indirin ve uygulamanızı yapılandırın

Azure Active Directory kurulum adımlarını tamamladığınıziçin PingAccess'i yapılandırmaya geçebilirsiniz.

Bu senaryonun PingAccess bölümü için ayrıntılı adımlar Ping Identity belgelerinde devam eder. Ping Identity web sitesinde [Microsoft Azure AD Application Proxy kullanılarak yayınlanan uygulamaları korumak için Azure AD için PingAccess'i Yapılandır'daki](https://support.pingidentity.com/s/document-item?bundleId=pingaccess-52&topicId=agents/azure/pa_c_PAAzureSolutionOverview.html) yönergeleri izleyin.

Bu adımlar PingAccess'i yüklemenize ve bir PingAccess hesabınız olmasına yardımcı olur (zaten hesabınız yoksa). Ardından, bir Azure AD OpenID Bağlantısı (OIDC) bağlantısı oluşturmak için, Azure AD portalından kopyaladığınız **Dizin (kiracı) kimlik** değerine sahip bir belirteç sağlayıcısı ayarlarsınız. Ardından, PingAccess'te bir web oturumu oluşturmak için Uygulama `PingAccess key` **(istemci) kimliğini** ve değerlerini kullanırsınız. Bundan sonra, kimlik eşleme ayarlayabilir ve sanal bir ana bilgisayar, site ve uygulama oluşturabilirsiniz.

### <a name="test-your-application"></a>Başvurunuzu test edin

Tüm bu adımları tamamladığınızda, uygulamanız çalışır durumda olmalıdır. Test etmek için bir tarayıcı açın ve uygulamayı Azure'da yayınladığınızda oluşturduğunuz harici URL'ye gidin. Uygulamaya atadığınız test hesabıyla oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Azure AD Application Proxy kullanılarak yayınlanan uygulamaları korumak için Azure AD için PingAccess'i yapılandırın](https://support.pingidentity.com/s/document-item?bundleId=pingaccess-52&topicId=agents/azure/pa_c_PAAzureSolutionOverview.html)
- [Azure Active Directory'deki uygulamalarda tek oturum açma](what-is-single-sign-on.md)
- [Sorun Giderme Uygulama Proxy sorunları ve hata iletileri](application-proxy-troubleshoot.md)
