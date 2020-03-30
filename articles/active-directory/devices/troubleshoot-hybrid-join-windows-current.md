---
title: Karma Azure Etkin Dizin'ine katılan aygıtları sorun giderme
description: Sorun giderme hibrit Azure Active Directory, Windows 10 ve Windows Server 2016 aygıtlarına katıldı.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 26e52930211611673b6fe2309e2dca067a91ebc8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80331782"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-devices"></a>Karma Azure Etkin Dizin'ine katılan aygıtları sorun giderme 

Bu makalenin içeriği, Windows 10 veya Windows Server 2016 çalıştıran aygıtlar için geçerlidir.

Diğer Windows istemcileri için, [alt düzey aygıtları birleştiren karma Azure Etkin Dizini'ni](troubleshoot-hybrid-join-windows-legacy.md)gideren makaleye bakın.

Bu makalede, aşağıdaki senaryoları desteklemek için [karma Azure Etkin Dizin birleştirilmiş aygıtlar yapılandırdığınız](hybrid-azuread-join-plan.md) varsayar:

- Aygıt Tabanlı Koşullu Erişim
- [Ayarların kurumsal dolaşımı](../active-directory-windows-enterprise-state-roaming-overview.md)
- [İş İçin Windows Hello](../active-directory-azureadjoin-passport-deployment.md)

Bu belge, olası sorunları gidermek için sorun giderme kılavuzu sağlar. 

Windows 10 ve Windows Server 2016 için karma Azure Active Directory join, Windows 10 Kasım 2015 Güncelleştirmesi ve üzerini destekler.

## <a name="troubleshoot-join-failures"></a>Sorun giderme birleştirme hataları

### <a name="step-1-retrieve-the-join-status"></a>Adım 1: Birleştirme durumunu alma 

**Birleştirme durumunu almak için:**

1. Yönetici olarak komut istemi açma
2. `dsregcmd /status` yazın

```
+----------------------------------------------------------------------+
| Device State                                                         |
+----------------------------------------------------------------------+

    AzureAdJoined: YES
 EnterpriseJoined: NO
         DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7
       Thumbprint: B753A6679CE720451921302CA873794D94C6204A
   KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9
      KeyProvider: Microsoft Platform Crypto Provider
     TpmProtected: YES
     KeySignTest: : MUST Run elevated to test.
              Idp: login.windows.net
         TenantId: 72b988bf-xxxx-xxxx-xxxx-2d7cd011xxxx
       TenantName: Contoso
      AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize
   AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token
           MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc
        MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx
  dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance
      SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ==
   JoinSrvVersion: 1.0
       JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/
        JoinSrvId: urn:ms-drs:enterpriseregistration.windows.net
    KeySrvVersion: 1.0
        KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/
         KeySrvId: urn:ms-drs:enterpriseregistration.windows.net
     DomainJoined: YES
       DomainName: CONTOSO

+----------------------------------------------------------------------+
| User State                                                           |
+----------------------------------------------------------------------+

             NgcSet: YES
           NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
    WorkplaceJoined: NO
      WamDefaultSet: YES
WamDefaultAuthority: organizations
       WamDefaultId: https://login.microsoft.com
     WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)
         AzureAdPrt: YES
```

### <a name="step-2-evaluate-the-join-status"></a>Adım 2: Birleştirme durumunu değerlendirme 

Aşağıdaki alanları gözden geçirin ve beklenen değerlere sahip olduğundan emin olun:

#### <a name="domainjoined--yes"></a>DomainJoined : EVET  

Bu alan, aygıtın şirket içi Active Directory'ye katılıp katılmadığını gösterir. Değer **HAYRsi**ise, aygıt karma Azure AD birleştirme gerçekleştiremez.  

#### <a name="workplacejoined--no"></a>İşyeri Katıldı : HAYDI  

Bu alan, aygıtın Azure AD'ye kişisel bir aygıt olarak kaydedilip kaydedildiğini gösterir *(İşyeri Birleştirilmiş olarak*işaretlenir). Bu değer, karma Azure AD'si de birleştirilmiş etki alanı birleştirilmiş bir bilgisayar için **HAYR** olmalıdır. Değer **EVET**ise, karma Azure AD join'in tamamlanmasından önce bir çalışma veya okul hesabı eklendi. Bu durumda, Windows 10'un Yıldönümü Güncelleştirmesi (1607) sürümü kullanırken hesap yoksayılır.

#### <a name="azureadjoined--yes"></a>AzureAdJoined : EVET  

Bu alan, aygıtın birleşip birleştirilemeyeceğini gösterir. Aygıt Azure AD'ye bağlı bir aygıt veya karma bir Azure AD birleştirilmiş aygıtsa, değer **EVET** olacaktır.
Değer **HAYRsi**ise, Azure AD'ye katılma henüz tamamlanmadı. 

Daha fazla sorun giderme için sonraki adımlara devam edin.

### <a name="step-3-find-the-phase-in-which-join-failed-and-the-errorcode"></a>Adım 3: Birleştirme nin başarısız olduğu aşamayı ve hata kodunu bulma

#### <a name="windows-10-1803-and-above"></a>Windows 10 1803 ve üzeri

Birleştirme durumu çıktısının 'Tanılama Verileri' bölümündeki 'Önceki Kayıt' alt bölümüne bakın. Bu bölüm yalnızca aygıt etki alanı birleştirilmişse ve Azure AD birleştirmesini hibrit olarak kullanamıyorsa görüntülenir.
'Hata Aşaması' alanı birleştirme hatası aşamasını gösterirken ,'Client ErrorCode' Join işleminin hata kodunu gösterir.

```
+----------------------------------------------------------------------+
     Previous Registration : 2019-01-31 09:16:43.000 UTC
         Registration Type : sync
               Error Phase : join
          Client ErrorCode : 0x801c03f2
          Server ErrorCode : DirectoryError
            Server Message : The device object by the given id (e92325d0-xxxx-xxxx-xxxx-94ae875d5245) is not found.
              Https Status : 400
                Request Id : 6bff0bd9-820b-484b-ab20-2a4f7b76c58e
+----------------------------------------------------------------------+
```

#### <a name="older-windows-10-versions"></a>Eski Windows 10 sürümleri

Birleştirme hataları için aşama ve hata kodunu bulmak için Olay Görüntüleyici günlüğünü kullanın.

1. Olay görüntüleyicide **Kullanıcı Aygıtı Kaydı** olay günlüğünü açın. **Uygulamalar ve Hizmetler Günlüğü** > **Microsoft** > **Windows** > **Kullanıcı Cihazı Kaydı** altında bulunan
2. Aşağıdaki eventIDs 304, 305, 307 ile olayları arayın.

![Hata Günlüğü Olayı](./media/troubleshoot-hybrid-join-windows-current/1.png)

![Hata Günlüğü Olayı](./media/troubleshoot-hybrid-join-windows-current/2.png)

### <a name="step-4-check-for-possible-causes-and-resolutions-from-the-lists-below"></a>Adım 4: Aşağıdaki listelerden olası nedenleri ve çözümleri kontrol edin

#### <a name="pre-check-phase"></a>Ön kontrol aşaması

Başarısızlığın olası nedenleri:

- Aygıtın Etki Alanı denetleyicisine görüş alanı yoktur.
   - Aygıt, kuruluşun dahili ağında veya VPN'de, şirket içi Active Directory (AD) etki alanı denetleyicisine ağ çizgisiyle birlikte olmalıdır.

#### <a name="discover-phase"></a>Aşamayı keşfedin

Başarısızlığın olası nedenleri:

- Hizmet Bağlantı Noktası (SCP) nesnesi DC'den SCP nesnesini yanlış yapılandırMış/okuyamaz.
   - Aygıtın ait olduğu AD ormanında, Azure AD'de doğrulanmış bir etki alanı adına işaret eden geçerli bir SCP nesnesi gereklidir.
   - Ayrıntılar bir [Hizmet Bağlantı Noktası Yapılandırma](hybrid-azuread-join-federated-domains.md#configure-hybrid-azure-ad-join)bölümünde bulunabilir.
- Bulma meta verilerinin bulma bitiş noktasından bağlanmaması ve getirilememesi.
   - Cihaz, kayıt ve `https://enterpriseregistration.windows.net`yetkilendirme uç noktalarını bulmak için SİsteM bağlamında erişebilmeli. 
   - Şirket içi ortam giden bir proxy gerektiriyorsa, BT yöneticisi aygıtın bilgisayar hesabının giden proxy'yi keşfedip sessizce doğrulayabilmesini sağlamalıdır.
- Kullanıcı alemi bitiş noktasına bağlanma ve bölge keşfini gerçekleştirememe. (Windows 10 sürüm 1809 ve daha sonra sadece)
   - Aygıt, doğrulanmış etki `https://login.microsoftonline.com`alanı için alan keşfi gerçekleştirmek ve etki alanı türünü (yönetilen/federe) belirlemek için SİsteM bağlamında erişebilmeli.
   - Şirket içi ortam giden bir proxy gerektiriyorsa, BT yöneticisi aygıttaki SYSTEM bağlamını keşfedebilmesini ve giden proxy'ye sessizce doğrulayabilmesini sağlamalıdır.

**Yaygın hata kodları:**

- **DSREG_AUTOJOIN_ADCONFIG_READ_FAILED** (0x801c001d/-2145648611)
   - Neden: SCP nesnesini okuyamıyor ve Azure AD kiracı bilgilerini alamıyor.
   - Çözünürlük: [Hizmet Bağlantı Noktasını Yapılandır'a](hybrid-azuread-join-federated-domains.md#configure-hybrid-azure-ad-join)bakın.
- **DSREG_AUTOJOIN_DISC_FAILED** (0x801c0021/-2145648607)
   - Sebep: Genel Bulma hatası. DRS'den bulma meta verilerini alamadım.
   - Çözüm: Daha fazla araştırmak için aşağıdaki alt hatayı bulun.
- **DSREG_AUTOJOIN_DISC_WAIT_TIMEOUT** (0x801c001f/-2145648609)
   - Sebep: İşlem, Discovery gerçekleştirirken zaman doldu.
   - Çözüm: SİsteM bağlamında erişilebilir `https://enterpriseregistration.windows.net` olduğundan emin olun. Daha fazla bilgi için, bölüm [Ağ bağlantı gereksinimleri](hybrid-azuread-join-managed-domains.md#prerequisites)bakın.
- **DSREG_AUTOJOIN_USERREALM_DISCOVERY_FAILED** (0x801c0021/-2145648611)
   - Sebep: Genel Diyar Bulma hatası. STS'den etki alanı türünü (yönetilen/federe) belirleyemedi. 
   - Çözüm: Daha fazla araştırmak için aşağıdaki alt hatayı bulun.

**Ortak alt hata kodları:**

Bulma hata kodunun alt hata kodunu bulmak için aşağıdaki yöntemlerden birini kullanın.

##### <a name="windows-10-1803-and-above"></a>Windows 10 1803 ve üzeri

Birleştirme durumu çıktısının 'Tanılama Verileri' bölümünde 'DRS Bulma Testi'ni arayın. Bu bölüm yalnızca aygıt etki alanı birleştirilmişse ve Azure AD birleştirmesini hibrit olarak kullanamıyorsa görüntülenir.

```
+----------------------------------------------------------------------+
| Diagnostic Data                                                      |
+----------------------------------------------------------------------+

     Diagnostics Reference : www.microsoft.com/aadjerrors
              User Context : UN-ELEVATED User
               Client Time : 2019-06-05 08:25:29.000 UTC
      AD Connectivity Test : PASS
     AD Configuration Test : PASS
        DRS Discovery Test : FAIL [0x801c0021/0x80072ee2]
     DRS Connectivity Test : SKIPPED
    Token acquisition Test : SKIPPED
     Fallback to Sync-Join : ENABLED

+----------------------------------------------------------------------+
```

##### <a name="older-windows-10-versions"></a>Eski Windows 10 sürümleri

Birleştirme hataları için aşama ve hata kodunu bulmak için Olay Görüntüleyici günlüğünü kullanın.

1. Olay görüntüleyicide **Kullanıcı Aygıtı Kaydı** olay günlüğünü açın. **Uygulamalar ve Hizmetler Günlüğü** > **Microsoft** > **Windows** > **Kullanıcı Cihazı Kaydı** altında bulunan
2. Aşağıdaki eventIDs 201 ile olayları arayın

![Hata Günlüğü Olayı](./media/troubleshoot-hybrid-join-windows-current/5.png)

###### <a name="network-errors"></a>Ağ hataları

- **WININET_E_CANNOT_CONNECT** (0x80072efd/-2147012867)
   - Sebep: Sunucu ile bağlantı kurulamadı
   - Çözüm: Gerekli Microsoft kaynaklarına ağ bağlantısı sağlayın. Daha fazla bilgi için [Ağ bağlantı gereksinimlerine](hybrid-azuread-join-managed-domains.md#prerequisites)bakın.
- **WININET_E_TIMEOUT** (0x80072e2/-2147012894)
   - Sebep: Genel ağ zaman.
   - Çözüm: Gerekli Microsoft kaynaklarına ağ bağlantısı sağlayın. Daha fazla bilgi için [Ağ bağlantı gereksinimlerine](hybrid-azuread-join-managed-domains.md#prerequisites)bakın.
- **WININET_E_DECODING_FAILED** (0x80072f8f/-2147012721)
   - Sebep: Ağ yığını sunucudan gelen yanıtı çözemedi.
   - Çözüm: Ağ proxy'sinin sunucu yanıtını engellemediğinden ve değiştirmediğinden emin olun.

###### <a name="http-errors"></a>HTTP hataları

- **DSREG_DISCOVERY_TENANT_NOT_FOUND** (0x801c003a/-2145648582)
   - Sebep: SCP nesnesi yanlış kiracı kimliğiyle yapılandırıldı. Veya kiracıda etkin abonelik bulunamadı.
   - Çözüm: SCP nesnesinin doğru Azure AD kiracı kimliği ve etkin aboneliklerle yapılandırıldığından veya kiracıda bulunduğundan emin olun.
- **DSREG_SERVER_BUSY** (0x801c0025/-2145648603)
   - Sebep: DRS sunucusundan HTTP 503.
   - Çözünürlük: Sunucu şu anda kullanılamıyor. sunucu yeniden çevrimiçi olduğunda gelecekteki birleştirme girişimleri büyük olasılıkla başarılı olacaktır.

###### <a name="other-errors"></a>Diğer hatalar

- **E_INVALIDDATA** (0x8007000d/-2147024883)
   - Sebep: Sunucu yanıtı JSON ayrıştı olamazdı. Büyük olasılıkla bir HTML auth sayfası ile HTTP 200 dönen proxy nedeniyle.
   - Çözüm: Şirket içi ortam giden bir proxy gerektiriyorsa, BT yöneticisi aygıttaki SYSTEM bağlamını keşfedebilmesini ve giden proxy'ye sessizce doğrulayabilmesini sağlamalıdır.

#### <a name="authentication-phase"></a>Kimlik doğrulama aşaması

Yalnızca federe etki alanı hesapları için geçerlidir.

Başarısızlık nedenleri:

- DRS kaynağı için sessizce bir Erişim belirteci alınamıyor.
   - Windows 10 aygıtları, Tümleşik Windows Kimlik Doğrulaması'nı kullanarak federasyon hizmetinden etkin bir WS-Trust bitiş noktasına auth belirteci elde etti. Ayrıntılar: [Federasyon Hizmet Yapılandırması](hybrid-azuread-join-manual.md#set-up-issuance-of-claims)

**Yaygın hata kodları:**

Hata kodunu, alt hata kodunu, sunucu hata kodunu ve sunucu hata iletisini bulmak için Olay Görüntüleyici günlüğünü kullanın.

1. Olay görüntüleyicide **Kullanıcı Aygıtı Kaydı** olay günlüğünü açın. **Uygulamalar ve Hizmetler Günlüğü** > **Microsoft** > **Windows** > **Kullanıcı Cihazı Kaydı** altında bulunan
2. Aşağıdaki eventID 305 ile olayları arayın

![Hata Günlüğü Olayı](./media/troubleshoot-hybrid-join-windows-current/3.png)

##### <a name="configuration-errors"></a>Yapılandırma hataları

- **ERROR_ADAL_PROTOCOL_NOT_SUPPORTED** (0xcaa90017/-894894057)
   - Sebep: Kimlik doğrulama protokolü WS-Trust değildir.
   - Çözüm: Şirket içi kimlik sağlayıcısı WS-Trust'ı desteklemelidir 
- **ERROR_ADAL_FAILED_TO_PARSE_XML** (0xcaa9002c/-894894036)
   - Sebep: Şirket içi federasyon hizmeti XML yanıtını döndürmedi.
   - Çözünürlük: MEX uç noktasının geçerli bir XML döndürdiğinden emin olun. Proxy'nin xml olmayan yanıtları engellemediğinden ve döndürmediğinden emin olun.
- **ERROR_ADAL_COULDNOT_DISCOVER_USERNAME_PASSWORD_ENDPOINT** (0xcaa90023/-894894045)
   - Sebep: Kullanıcı adı/parola kimlik doğrulaması için bitiş noktası bulunamadı.
   - Çözüm: Şirket içi kimlik sağlayıcı ayarlarını kontrol edin. WS-Trust uç noktalarının etkin olduğundan emin olun ve MEX yanıtının bu doğru uç noktaları içerdiğinden emin olun.

##### <a name="network-errors"></a>Ağ hataları

- **ERROR_ADAL_INTERNET_TIMEOUT** (0xcaa82ee2/-894947614)
   - Sebep: Genel ağ zaman.
   - Çözüm: SİsteM bağlamında erişilebilir `https://login.microsoftonline.com` olduğundan emin olun. Şirket içi kimlik sağlayıcısına SİsteM bağlamında erişilebilir olduğundan emin olun. Daha fazla bilgi için [Ağ bağlantı gereksinimlerine](hybrid-azuread-join-managed-domains.md#prerequisites)bakın.
- **ERROR_ADAL_INTERNET_CONNECTION_ABORTED** (0xcaa82efe/-894947586)
   - Sebep: Auth uç noktası ile bağlantı iptal edildi.
   - Çözüm: Bir süre sonra yeniden deneyin veya alternatif bir kararlı ağ konumundan katılmayı deneyin.
- **ERROR_ADAL_INTERNET_SECURE_FAILURE** (0xcaa82f8f/-894947441)
   - Sebep: Daha önce Güvenli Soketkatmanı (SSL) olarak bilinen Aktarım Katmanı Güvenliği (TLS), sunucu tarafından gönderilen sertifika doğrulanamadı.
   - Çözüm: İstemcinin zaman eğriliğini kontrol edin. Bir süre sonra yeniden deneyin veya alternatif bir kararlı ağ konumundan katılmayı deneyin. 
- **ERROR_ADAL_INTERNET_CANNOT_CONNECT** (0xcaa82efd/-894947587)
   - Sebep: `https://login.microsoftonline.com` Başarısız bağlanma girişimi.
   - Çözünürlük: Ağ bağlantısını `https://login.microsoftonline.com`kontrol edin.

##### <a name="other-errors"></a>Diğer hatalar

- **ERROR_ADAL_SERVER_ERROR_INVALID_GRANT** (0xcaa20003/-895352829)
   - Sebep: Şirket içi kimlik sağlayıcısından SAML belirteci Azure AD tarafından kabul edilmedi.
   - Çözünürlük: Federasyon sunucu ayarlarını kontrol edin. Kimlik doğrulama günlüklerinde sunucu hata kodunu arayın.
- **ERROR_ADAL_WSTRUST_REQUEST_SECURITYTOKEN_FAILED** (0xcaa90014/-894894060)
   - Sebep: Sunucu WS-Trust yanıtı hata özel durum bildirdi ve iddia almak için başarısız oldu
   - Çözünürlük: Federasyon sunucu ayarlarını kontrol edin. Kimlik doğrulama günlüklerinde sunucu hata kodunu arayın.
- **ERROR_ADAL_WSTRUST_TOKEN_REQUEST_FAIL** (0xcaa90006/-894894074)
   - Sebep: Belirteç bitiş noktasından erişim belirteci almaya çalışırken bir hata aldı.
   - Çözüm: ADAL günlüğündeki altta yatan hatayı arayın. 
- **ERROR_ADAL_OPERATION_PENDING** (0xcaa1002d/-895418323)
   - Sebep: Genel ADAL hatası
   - Çözüm: Kimlik doğrulama günlüklerinden alt hata kodunu veya sunucu hata kodunu arayın.
    
#### <a name="join-phase"></a>Birleştirme Aşaması

Başarısızlık nedenleri:

Kayıt türünü bulun ve aşağıdaki listeden hata kodunu arayın.

#### <a name="windows-10-1803-and-above"></a>Windows 10 1803 ve üzeri

Birleştirme durumu çıktısının 'Tanılama Verileri' bölümündeki 'Önceki Kayıt' alt bölümüne bakın. Bu bölüm yalnızca aygıt etki alanı birleştirilmişse ve Azure AD birleştirmesini hibrit olarak kullanamıyorsa görüntülenir.
'Kayıt Türü' alanı gerçekleştirilen birleştirme türünü gösterir.

```
+----------------------------------------------------------------------+
     Previous Registration : 2019-01-31 09:16:43.000 UTC
         Registration Type : sync
               Error Phase : join
          Client ErrorCode : 0x801c03f2
          Server ErrorCode : DirectoryError
            Server Message : The device object by the given id (e92325d0-7ac4-4714-88a1-94ae875d5245) is not found.
              Https Status : 400
                Request Id : 6bff0bd9-820b-484b-ab20-2a4f7b76c58e
+----------------------------------------------------------------------+
```

#### <a name="older-windows-10-versions"></a>Eski Windows 10 sürümleri

Birleştirme hataları için aşama ve hata kodunu bulmak için Olay Görüntüleyici günlüğünü kullanın.

1. Olay görüntüleyicide **Kullanıcı Aygıtı Kaydı** olay günlüğünü açın. **Uygulamalar ve Hizmetler Günlüğü** > **Microsoft** > **Windows** > **Kullanıcı Cihazı Kaydı** altında bulunan
2. Aşağıdaki eventIDs 204 ile olayları arayın

![Hata Günlüğü Olayı](./media/troubleshoot-hybrid-join-windows-current/4.png)

##### <a name="http-errors-returned-from-drs-server"></a>DRS sunucusundan http hataları döndürülür

- **DSREG_E_DIRECTORY_FAILURE** (0x801c03f2/-2145647630)
   - Sebep: ErrorCode ile DRS'den hata yanıtı aldı: "DirectoryError"
   - Çözüm: Olası nedenler ve çözümler için sunucu hata koduna bakın.
- **DSREG_E_DEVICE_AUTHENTICATION_ERROR** (0x801c0002/-2145648638)
   - Sebep: ErrorCode ile DRS bir hata yanıtı aldı: "AuthenticationError" ve ErrorSubCode Değİl "DeviceNotFound". 
   - Çözüm: Olası nedenler ve çözümler için sunucu hata koduna bakın.
- **DSREG_E_DEVICE_INTERNALSERVICE_ERROR** (0x801c0006/-2145648634)
   - Sebep: ErrorCode ile DRS'den hata yanıtı aldı: "DirectoryError"
   - Çözüm: Olası nedenler ve çözümler için sunucu hata koduna bakın.

##### <a name="tpm-errors"></a>TPM hataları

- **NTE_BAD_KEYSET** (0x80090016/-2146893802)
   - Sebep: TPM işlemi başarısız oldu veya geçersiz oldu
   - Çözünürlük: Büyük olasılıkla kötü bir sysprep görüntü nedeniyle. Sysprep görüntüsünün oluşturulduğu makinenin Azure AD'ye katılmadığından, karma Azure AD'si birleştirilmiş veya Azure AD kayıtlı olmadığından emin olun.
- **TPM_E_PCP_INTERNAL_ERROR** (0x80290407/-2144795641)
   - Sebep: Genel TPM hatası. 
   - Çözünürlük: Bu hataya sahip cihazlarda TPM'yi devre dışı kılabilir. Windows 10 sürüm 1809 ve üzeri otomatik olarak TPM hatalarını algılar ve TPM'yi kullanmadan karma Azure AD birleştirmesini tamamlar.
- **TPM_E_NOTFIPS** (0x80280036/-2144862154)
   - Sebep: FIPS modunda TPM şu anda desteklenmez.
   - Çözünürlük: Bu hataya sahip cihazlarda TPM'yi devre dışı kılabilir. Windows 1809, TPM hatalarını otomatik olarak algılar ve TPM'yi kullanmadan karma Azure AD birleştirmesini tamamlar.
- **NTE_AUTHENTICATION_IGNORED** (0x80090031/-2146893775)
   - Sebep: TPM kilitlendi.
   - Çözünürlük: Geçici hata. Bekleme süresini bekleyin. Bir süre sonra girişimi katılın başarılı olmalıdır. Daha fazla bilgi makale [TPM temelleri](/windows/security/information-protection/tpm/tpm-fundamentals#anti-hammering) bulunabilir

##### <a name="network-errors"></a>Ağ Hataları

- **WININET_E_TIMEOUT** (0x80072e2/-2147012894)
   - Sebep: Cihazı DRS'de kaydetmeye çalışırken genel ağ süresi
   - Çözünürlük: Ağ bağlantısını `https://enterpriseregistration.windows.net`kontrol edin.
- **WININET_E_NAME_NOT_RESOLVED** (0x80072ee7/-2147012889)
   - Sebep: Sunucu adı veya adresi çözülemedi.
   - Çözünürlük: Ağ bağlantısını `https://enterpriseregistration.windows.net`kontrol edin. Ana bilgisayar adı için DNS çözünürlüğünün n/w'de ve cihazda doğru olduğundan emin olun.
- **WININET_E_CONNECTION_ABORTED** (0x80072efe/-2147012866)
   - Sebep: Sunucuyla bağlantı anormal olarak sonlandırıldı.
   - Çözüm: Bir süre sonra yeniden deneyin veya alternatif bir kararlı ağ konumundan katılmayı deneyin.

##### <a name="federated-join-server-errors"></a>Federe birleştirme sunucusu Hataları

| Sunucu hata kodu | Sunucu hata iletisi | Olası nedenler | Çözüm |
| --- | --- | --- | --- |
| DizinHatası | İsteğiniz geçici olarak azaltıldı. Lütfen 300 saniye sonra deneyin. | Beklenen hata. Muhtemelen hızlı bir şekilde birden fazla kayıt isteğinde bulunmak nedeniyle. | Bekleme süresinden sonra yeniden birleştirme |

##### <a name="sync-join-server-errors"></a>Eşitleme birleştirme sunucusu Hataları

| Sunucu hata kodu | Sunucu hata iletisi | Olası nedenler | Çözüm |
| --- | --- | --- | --- |
| DizinHatası | AADSTS90002: <UUID> Kiracı bulunamadı. Kiracı için etkin abonelik yoksa bu hata oluşabilir. Abonelik yöneticinize danışın. | SCP nesnesindeki kiracı kimliği yanlış | SCP nesnesinin doğru Azure AD kiracı kimliği ve etkin aboneliklerle yapılandırıldığından ve kiracıda bulunduğundan emin olun. |
| DizinHatası | Verilen kimlikteki aygıt nesnesi bulunamadı. | Eşitleme birleştirme için beklenen hata. Aygıt nesnesi AD'den Azure AD'ye eşitlenmedi | Azure AD Connect eşitlemesini bekleyin ve eşitleme tamamlandıktan sonraki birleştirme denemesi sorunu çözecektir |
| Kimlik Doğrulama Hatası | Hedef bilgisayarın SID'sinin doğrulanması | Azure AD aygıtındaki sertifika, eşitleme birleştirme sırasında blob'u imzalamak için kullanılan sertifikayla eşleşmiyor. Bu hata genellikle eşitlemenin henüz tamamlanmadığı anlamına gelir. |  Azure AD Connect eşitlemesini bekleyin ve eşitleme tamamlandıktan sonraki birleştirme denemesi sorunu çözecektir |

### <a name="step-5-collect-logs-and-contact-microsoft-support"></a>Adım 5: Günlükleri toplayın ve Microsoft Desteği'ne başvurun

Burada kamu scriptler alın: [ https://1drv.ms/u/s! AkyTjQ17vtfagYkZ6VJzPg78e3o7PQ]( https://1drv.ms/u/s!AkyTjQ17vtfagYkZ6VJzPg78e3o7PQ)

1. Yönetici komut istemini `start_ngc_tracing_public.cmd`açın ve çalıştırın.
2. Sorunu çoğaltmak için adımları gerçekleştirin.
3. Yürüterek günlük komut dosyasını çalıştırmayı durdurun. `stop_ngc_tracing_public.cmd`
4. Zip ve analiz için `%SYSTEMDRIVE%\TraceDJPP\*` altında günlükleri gönderin.

## <a name="troubleshoot-post-join-issues"></a>Birleştirme Sonrası sorunları giderme

### <a name="retrieve-the-join-status"></a>Birleştirme durumunu alma 

#### <a name="wamdefaultset-yes-and-azureadprt-yes"></a>WamDefaultSet: EVET ve AzureADPrt: EVET
  
Bu alanlar, kullanıcının aygıtta oturum açken Azure AD'de başarıyla kimlik doğrulaması yapıp olmadığını gösterir. Değerler **HAYRsi**ise, vadesi geçmiş olabilir:

- Kayıt sırasında cihazla ilişkili TPM'deki bozuk depolama anahtarı (yüksek çalışırken KeySignTest'i kontrol edin).
- Alternatif Giriş Kimliği
- HTTP Proxy bulunamadı

## <a name="known-issues"></a>Bilinen sorunlar
- Ayarlar -> Hesapları -> Access Work veya Okul altında, Karma Azure AD birleştirilmiş aygıtlar, biri Azure AD diğeri mobil etkin noktalara veya harici Wi-Fi ağlarına bağlandığında şirket içi AD için olmak üzere iki farklı hesap gösterebilir. Bu yalnızca bir UI sorunudur ve işlevsellik üzerinde herhangi bir etkisi yoktur. 
 
## <a name="next-steps"></a>Sonraki adımlar

[dsregcmd komutunu kullanarak aygıtları sorun giderme ye](troubleshoot-device-dsregcmd.md) devam etme

Sorularınız için [cihaz yönetimi SSS'ye](faq.md) bakın
