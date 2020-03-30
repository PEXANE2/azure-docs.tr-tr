---
title: dsregcmd komutunu kullanarak sorun giderme - Azure Active Directory
description: Azure AD'deki cihazların durumunu anlamak için dsregcmd'den çıktı kullanma
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2cd782cdab625934fe60617142e5ac0baf756398
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128753"
---
# <a name="troubleshooting-devices-using-the-dsregcmd-command"></a>dsregcmd komutunu kullanan sorun giderme cihazları

dsregcmd /status yardımcı programı bir etki alanı kullanıcı hesabı olarak çalıştırılmalıdır.

## <a name="device-state"></a>Aygıt durumu

Bu bölümde aygıt birleştirme durum parametreleri listelenir. Aşağıdaki tabloda, aygıtın çeşitli birleştirme durumlarında olması için ölçütler listelenir.

| AzureAdJoined | EnterpriseJoined | DomainJoined | Aygıt durumu |
| ---   | ---   | ---   | ---   |
| EVET | NO | NO | Azure AD Katıldı |
| NO | NO | EVET | Etki Alanı Katıldı |
| EVET | NO | EVET | Hibrit AD Katıldı |
| NO | EVET | EVET | Şirket Içi DRS Katıldı |

> [!NOTE]
> İşyeri Birleştirme (Azure AD kayıtlı) durumu "Kullanıcı Durumu" bölümünde görüntülenir

- **AzureAdJoined:** - Aygıt Azure AD'ye katıldıysa "EVET" olarak ayarlayın. "HAYDI" aksi takdirde.
- **EnterpriseJoined:** - Cihaz şirket içi DRS'ye katıldıysa "EVET" olarak ayarlayın. Bir aygıt hem EnterpriseJoined hem de AzureAdJoined olamaz.
- **DomainJoined:** - Aygıt bir etki alanına (AD) katıldıysa "EVET" olarak ayarlayın.
- **DomainName:** - Aygıt bir etki alanına katıldıysa etki alanının adına ayarlayın.

### <a name="sample-device-state-output"></a>Örnek aygıt durumu çıktısı

```
+----------------------------------------------------------------------+
| Device State                                                         |
+----------------------------------------------------------------------+
             AzureAdJoined : YES
          EnterpriseJoined : NO
              DomainJoined : YES
                DomainName : HYBRIDADFS
+----------------------------------------------------------------------+
```

## <a name="device-details"></a>Cihaz ayrıntıları

Yalnızca aygıt Azure AD'ye katıldığında veya azure AD'ye katıldıklarında görüntülenir (Azure AD kayıtlı değildir). Bu bölümde, bulutta depolanan aygıt tanımlayıcı ayrıntıları listelenir.

- **DeviceId:** - Azure AD kiracısındaki aygıtın benzersiz kimliği
- **Parmak izi:** - Cihaz sertifikasının parmak izi 
- **DeviceCertificateValidity:** - Cihaz sertifikasının geçerliliği
- **KeyContainerId:** - Device sertifikası ile ilişkili cihazın özel anahtarıcontainerid
- **KeyProvider:** - KeyProvider (Donanım / Yazılım) cihazı özel anahtar depolamak için kullanılır.
- **TpmProtected:** - "EVET" aygıt özel anahtar donanım TPM saklanır.

### <a name="sample-device-details-output"></a>Örnek cihaz ayrıntıları çıktısı

```
+----------------------------------------------------------------------+
| Device Details                                                       |
+----------------------------------------------------------------------+

                  DeviceId : e92325d0-xxxx-xxxx-xxxx-94ae875dxxxx
                Thumbprint : D293213EF327483560EED8410CAE36BB67208179
 DeviceCertificateValidity : [ 2019-01-11 21:02:50.000 UTC -- 2029-01-11 21:32:50.000 UTC ]
            KeyContainerId : 13e68a58-xxxx-xxxx-xxxx-a20a2411xxxx
               KeyProvider : Microsoft Software Key Storage Provider
              TpmProtected : NO
+----------------------------------------------------------------------+
```

## <a name="tenant-details"></a>Kiracı ayrıntıları

Yalnızca aygıt Azure AD'ye katıldığında veya azure AD'ye katıldıklarında görüntülenir (Azure AD kayıtlı değildir). Bu bölümde, bir aygıt Azure AD'ye katıldığında ortak kiracı ayrıntıları listelenir.

> [!NOTE]
> Bu bölümdeki MDM URL'leri boşsa, MDM'nin yapılandırılmadığını veya geçerli kullanıcının MDM kaydı kapsamında olmadığını gösterir. MDM yapılandırmanızı incelemek için Azure AD'deki Mobilite ayarlarını kontrol edin.

> [!NOTE]
> MDM URL'lerini görseniz bile bu, aygıtın bir MDM tarafından yönetildiği anlamına gelmez. Kiracıda otomatik kayıt için MDM yapılandırması varsa, aygıtın kendisi yönetilmese bile bilgiler görüntülenir. 

### <a name="sample-tenant-details-output"></a>Örnek kiracı ayrıntıları çıktısı

```
+----------------------------------------------------------------------+
| Tenant Details                                                       |
+----------------------------------------------------------------------+

                TenantName : HybridADFS
                  TenantId : 96fa76d0-xxxx-xxxx-xxxx-eb60cc22xxxx
                       Idp : login.windows.net
               AuthCodeUrl : https://login.microsoftonline.com/96fa76d0-xxxx-xxxx-xxxx-eb60cc22xxxx/oauth2/authorize
            AccessTokenUrl : https://login.microsoftonline.com/96fa76d0-xxxx-xxxx-xxxx-eb60cc22xxxx/oauth2/token
                    MdmUrl : https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc
                 MdmTouUrl : https://portal.manage-beta.microsoft.com/TermsOfUse.aspx
          MdmComplianceUrl : https://portal.manage-beta.microsoft.com/?portalAction=Compliance
               SettingsUrl : eyJVxxxxIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyxxxx==
            JoinSrvVersion : 1.0
                JoinSrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/device/
                 JoinSrvId : urn:ms-drs:enterpriseregistration.windows.net
             KeySrvVersion : 1.0
                 KeySrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/key/
                  KeySrvId : urn:ms-drs:enterpriseregistration.windows.net
        WebAuthNSrvVersion : 1.0
            WebAuthNSrvUrl : https://enterpriseregistration.windows.net/webauthn/96fa76d0-xxxx-xxxx-xxxx-eb60cc22xxxx/
             WebAuthNSrvId : urn:ms-drs:enterpriseregistration.windows.net
    DeviceManagementSrvVer : 1.0
    DeviceManagementSrvUrl : https://enterpriseregistration.windows.net/manage/96fa76d0-xxxx-xxxx-xxxx-eb60cc22xxxx/
     DeviceManagementSrvId : urn:ms-drs:enterpriseregistration.windows.net
+----------------------------------------------------------------------+
```

## <a name="user-state"></a>Kullanıcı durumu

Bu bölümde, aygıtta oturum açmış olan kullanıcı için çeşitli özniteliklerin durumu listelenir.

> [!NOTE]
> Komut, geçerli durumu almak için kullanıcı bağlamında çalıştırılmalıdır.

- **NgcSet:** - Kullanıcıda oturum açan geçerli anahtar için bir Windows Hello tuşu ayarlanmışsa "EVET" olarak ayarlayın.
- **NgcKeyId:** - Kullanıcıda geçerli oturum açan kişi için ayarlanmışsa Windows Hello tuşunun kimliği.
- **CanReset:** - Windows Hello tuşu kullanıcı tarafından sıfırlanabiliyorsa gösterir. 
- **Olası değerler:** - DestructiveOnly, NonDestructiveOnly, DestructiveAndNonDestructive veya Unknown if hatası. 
- **WorkplaceJoined:** - Geçerli NTUSER bağlamında aygıta Azure AD kayıtlı hesapları eklenmişse "EVET" olarak ayarlayın.
- **WamDefaultSet:** - Oturum açmış kullanıcı için wam varsayılan Web Hesabı oluşturulursa "EVET" olarak ayarlayın. DSreg /status yükseltilmiş bir komut istemiçalıştırılırsa bu alan bir hata görüntüleyebilir. 
- **WamDefaultAuthority:** - Azure AD için "kuruluşlar" olarak ayarlayın.
- **WamDefaultId:** -https://login.microsoft.comHer zaman " " Azure AD için.
- **WamDefaultGUID:** - Varsayılan WAM Web Hesabı için WAM sağlayıcısının (Azure AD/Microsoft hesabı) GUID'i. 

### <a name="sample-user-state-output"></a>Örnek kullanıcı durumu çıktısı

```
+----------------------------------------------------------------------+
| User State                                                           |
+----------------------------------------------------------------------+

                    NgcSet : YES
                  NgcKeyId : {FA0DB076-A5D7-4844-82D8-50A2FB42EC7B}
                  CanReset : DestructiveAndNonDestructive
           WorkplaceJoined : NO
             WamDefaultSet : YES
       WamDefaultAuthority : organizations
              WamDefaultId : https://login.microsoft.com
            WamDefaultGUID : { B16898C6-A148-4967-9171-64D755DA8520 } (AzureAd)

+----------------------------------------------------------------------+
```

## <a name="sso-state"></a>SSO durumu

Bu bölüm Azure AD kayıtlı aygıtlar için yoksayılabilir.

> [!NOTE]
> Komut, bu kullanıcı için geçerli durumu almak için bir kullanıcı bağlamında çalışması gerekir.

- **AzureAdPrt:** - Oturum açmış kullanıcıiçin cihazda bir PRT varsa "EVET" olarak ayarlayın.
- **AzureAdPrtUpdateTime:** - İİT'nin en son güncelleştirildiğinde UTC'de saate göre ayarlayın.
- **AzureAdPrtExpiryTime:** - YENIlenmezse PRT'nin süresinin dolacağı UTC'de zamana ayarlayın.
- **AzureAdPrtAuthority:** - Azure AD yetki URL'si
- **EnterprisePrt:** - Cihaz da ADFS'den PRT varsa "EVET" olarak ayarlayın. Karma Azure AD'ye katılan aygıtlar için aygıt, hem Azure AD'den hem de şirket içi AD'den aynı anda PRT'ye sahip olabilir. Şirket içi birleştirilmiş aygıtlarda yalnızca Enterprise PRT bulunur.
- **EnterprisePrtUpdateTime:** - Enterprise PRT'nin en son güncelleştirildiğinde UTC'deki zamana ayarlayın.
- **EnterprisePrtExpiryTime:** - YENIlenmezse İİT'nin süresinin dolacağı UTC'deki zamana ayarlayın.
- **EnterprisePrtAuthority:** - ADFS yetki URL'si

### <a name="sample-sso-state-output"></a>Örnek SSO durum çıktısı

```
+----------------------------------------------------------------------+
| SSO State                                                            |
+----------------------------------------------------------------------+

                AzureAdPrt : YES
      AzureAdPrtUpdateTime : 2019-01-24 19:15:26.000 UTC
      AzureAdPrtExpiryTime : 2019-02-07 19:15:26.000 UTC
       AzureAdPrtAuthority : https://login.microsoftonline.com/96fa76d0-xxxx-xxxx-xxxx-eb60cc22xxxx
             EnterprisePrt : YES
   EnterprisePrtUpdateTime : 2019-01-24 19:15:33.000 UTC
   EnterprisePrtExpiryTime : 2019-02-07 19:15:33.000 UTC
    EnterprisePrtAuthority : https://fs.hybridadfs.nttest.microsoft.com:443/adfs

+----------------------------------------------------------------------+
```

## <a name="diagnostic-data"></a>Tanısal veriler

### <a name="pre-join-diagnostics"></a>Katılım öncesi tanılama

Bu bölüm yalnızca aygıt etki alanı birleştirilmişse ve Azure AD birleştirmesini hibrit olarak kullanamıyorsa görüntülenir.

Bu bölümde, birleştirme hatalarını tanılamaya yardımcı olmak için çeşitli testler gerçekleştirir. Bu bölümde ayrıca önceki (?) ayrıntılarını içerir. Bu bilgiler hata aşaması, hata kodu, sunucu istek kimliği, sunucu yanıtı http durumu, sunucu yanıt hatası iletisi içerir.

- **Kullanıcı Bağlamı:** - Tanılamanın çalıştırıldığı bağlam. Olası değerler: SİsteM, UN-ELEVATED Kullanıcı, YÜKSELMİş Kullanıcı. 

   > [!NOTE]
   > Gerçek birleştirme SİsteM bağlamında gerçekleştirildiğinden, tanılamayı System bağlamında çalıştırmak gerçek birleştirme senaryosuna en yakın dır. SİsteM bağlamında tanılamayı çalıştırmak için dsregcmd /durum komutu yükseltilmiş bir komut isteminden çalıştırılmalıdır.

- **İstemci Saati:** - UTC'deki sistem zamanı.
- **AD Bağlantı Testi:** - Test etki alanı denetleyicisine bir bağlantı testi gerçekleştirir. Bu testteki hata büyük olasılıkla ön kontrol aşamasında Birleştirme hatalarına neden olur.
- **AD Yapılandırma Testi:** - Test, SCP nesnesinin şirket içi AD ormanında düzgün bir şekilde yapılandırılıp yapılandırılmadığını okur ve doğrular. Bu testteki hatalar büyük olasılıkla 0x801c001d hata koduyla keşfetme aşamasında Birleştirme hatalarına neden olur.
- **DRS Discovery Test:** - Test, drs uç noktalarını meta veri bitiş noktasından alır ve kullanıcı alanı isteğini gerçekleştirir. Bu testteki hatalar büyük olasılıkla bulma aşamasında Birleştirme hatalarına neden olur.
- **DRS Bağlantı Testi:** - Test, DRS bitiş noktasına temel bağlantı testi gerçekleştirir.
- **Belirteç edinme Testi:** - Test, kullanıcı kiracısı federe ise Bir Azure AD kimlik doğrulama belirteci almaya çalışır. Bu testteki hatalar büyük olasılıkla auth aşamasında Birleştirme hatalarına neden olur. Auth başarısız olursa eşitleme birleştirme, aşağıdaki kayıt defteri anahtarı ayarlarıyla açıkça devre dışı bırakılmadığı sürece geri dönüş olarak denenir.
```
    Keyname: Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\CDJ
    Value: FallbackToSyncJoin
    Type:  REG_DWORD
    Value: 0x0 -> Disabled
    Value: 0x1 -> Enabled
    Default (No Key): Enabled
 ```
- **Fallback to Sync-Join:** - Yukarıdaki kayıt defteri anahtarı, auth hataları ile birleştirmek için geri dönüş önlemek için, "Etkin" olarak ayarlayın, mevcut DeğİlD. Bu seçenek, Windows 10 1803 ve sonraki nedenlerden edinilebilir.
- **Önceki Kayıt:** - Önceki Birleştirme girişiminin gerçekleştiği saat. Yalnızca başarısız Birleştirme denemeleri günlüğe kaydedilir.
- **Hata Aşaması:** - İptal edildiği birleştirme aşaması. Olası değerler ön kontrol, keşfetmek, auth, join vardır.
- **İstemci Hata Kodu:** - İstemci hata kodu döndürülür (HRESULT).
- **Server ErrorCode:** - Sunucuya bir istek gönderilmişse sunucu hata kodu ve sunucu bir hata kodu ile yanıt verdi. 
- **Sunucu Mesajı:** - Sunucu iletisi hata kodu ile birlikte döndürülür.
- **Https Durumu:** - Sunucu tarafından döndürülen http durumu.
- **İstek Kimliği:** - İstenen İstenen Id sunucuya gönderilir. Sunucu tarafı günlükleriyle ilişkilendirmek için yararlıdır.

### <a name="sample-pre-join-diagnostics-output"></a>Örnek ön birleştirme tanıçıktısı

Aşağıdaki örnek, bir bulma hatası ile hata tanılama testi gösterir.

```
+----------------------------------------------------------------------+
| Diagnostic Data                                                      |
+----------------------------------------------------------------------+

     Diagnostics Reference : www.microsoft.com/aadjerrors
              User Context : SYSTEM
               Client Time : 2019-01-31 09:25:31.000 UTC
      AD Connectivity Test : PASS
     AD Configuration Test : PASS
        DRS Discovery Test : FAIL [0x801c0021/0x801c000c]
     DRS Connectivity Test : SKIPPED
    Token acquisition Test : SKIPPED
     Fallback to Sync-Join : ENABLED

     Previous Registration : 2019-01-31 09:23:30.000 UTC
               Error Phase : discover
          Client ErrorCode : 0x801c0021

+----------------------------------------------------------------------+
```

Aşağıdaki örnek, tanılama testlerinin geçtiğini, ancak kayıt denemesinin eşitleme birleştirme için beklenen bir dizin hatasıyla başarısız olduğunu gösterir. Azure AD Connect eşitleme işi tamamlandığında, aygıt katılabilir.

```
+----------------------------------------------------------------------+
| Diagnostic Data                                                      |
+----------------------------------------------------------------------+

     Diagnostics Reference : www.microsoft.com/aadjerrors
              User Context : SYSTEM
               Client Time : 2019-01-31 09:16:50.000 UTC
      AD Connectivity Test : PASS
     AD Configuration Test : PASS
        DRS Discovery Test : PASS
     DRS Connectivity Test : PASS
    Token acquisition Test : PASS
     Fallback to Sync-Join : ENABLED

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

### <a name="post-join-diagnostics"></a>Birleştirme sonrası tanılama

Bu bölümde, buluta katılan bir aygıtta gerçekleştirilen akıl sağlığı denetimlerinin çıktısı görüntülenir.

- **AadRecoveryEnabled:** - "EVET" ise, aygıtta depolanan anahtarlar kullanılabilir değildir ve aygıt kurtarma için işaretlenir. Bir sonraki oturum açma kurtarma akışını tetikler ve aygıtı yeniden kaydeder.
- **KeySignTest:** - "GEÇTI" cihaz tuşları iyi durumda. KeySignTest başarısız olursa, aygıt genellikle kurtarma için işaretlenir. Bir sonraki oturum açma kurtarma akışını tetikler ve aygıtı yeniden kaydeder. Karma Azure AD'ye katılan aygıtlar için kurtarma sessizdir. Azure AD'ye katılırken veya Azure AD kayıtlıyken, aygıtlar kullanıcı kimlik doğrulamasının gerekirse aygıtı kurtarmasını ve yeniden kaydetmesini ister. **KeySignTest yüksek ayrıcalıklar gerektirir.**

#### <a name="sample-post-join-diagnostics-output"></a>Örnek birleştirme sonrası tanı çıktısı

```
+----------------------------------------------------------------------+
| Diagnostic Data                                                      |
+----------------------------------------------------------------------+

         AadRecoveryEnabled: NO
               KeySignTest : PASSED
+----------------------------------------------------------------------+
```

## <a name="ngc-prerequisite-check"></a>NGC ön koşul kontrolü

Bu bölümde, Windows Hello for Business (WHFB) sağlanması için gerekli denetimleri gerçekleştirir. 

> [!NOTE]
> Kullanıcı WHFB'yi başarıyla yapılandırmışsa, NGC ön koşul kontrol ayrıntılarını dsregcmd /status'da göremeyebilirsiniz.

- **IsDeviceJoined:** - Aygıt Azure AD'ye katıldıysa "EVET" olarak ayarlayın.
- **IsUserAzureAD:** - Oturum açmış kullanıcı Azure AD'de mevcutsa "EVET" olarak ayarlayın.
- **PolicyEnabled:** - Cihazda WHFB ilkesi etkinse "EVET" olarak ayarlayın.
- **PostLogonEnabled:** - WHFB kaydı platform tarafından yerel olarak tetiklenirse "EVET" olarak ayarlayın. "HAYIR" olarak ayarlanmışsa, Windows Hello for Business kaydının özel bir mekanizma tarafından tetiklenir olduğunu gösterir
- **DeviceEligible:** - Aygıt WHFB'ye kaydolmak için donanım gereksinimini karşılıyorsa "EVET" olarak ayarlayın.
- **SessionIsNotRemote:** - Geçerli kullanıcı doğrudan aygıta oturum açmışsa ve uzaktan değil,"EVET" olarak ayarlayın.
- **CertEnrollment:** - WHFB için sertifika kayıt yetkilisini gösteren WHFB Sertifika Güven dağıtımına özeldir. WHFB ilkesinin kaynağı Grup Politikası ise "kayıt yetkilisi" olarak ayarlayın, kaynak MDM ise "mobil cihaz yönetimi". "yok" aksi takdirde
- **AdfsRefreshToken:** - WHFB Sertifika Güven dağıtımına özeldir. Yalnızca CertEnrollment "kayıt yetkilisi" ise mevcut. Cihazın kullanıcı için kurumsal bir PRT'si olup olmadığını gösterir.
- **AdfsRaIsReady:** - WHFB Sertifika Güven dağıtımına özeldir.  Yalnızca CertEnrollment "kayıt yetkilisi" ise mevcut. ADFS, WHFB'yi desteklediğini bulma meta verilerinde belirtilmişse *ve* oturum açma sertifikası şablonu varsa "EVET" olarak ayarlayın.
- **LogonCertTemplateReady:** - WHFB Sertifika Güven dağıtımına özeldir. Yalnızca CertEnrollment "kayıt yetkilisi" ise mevcut. Oturum açma sertifikası şablonunun durumu geçerliyse ve ADFS RA sorun gidermeye yardımcı oluyorsa "EVET" olarak ayarlayın.
- **PreReqResult:** - Tüm WHFB ön koşul değerlendirmesinin sonucunu sağlar. WHFB kaydının bir sonraki sefere kullanıcı oturum açtırdığında oturum açma sonrası görev olarak başlatılırsa "Will Provision" olarak ayarlayın.

### <a name="sample-ngc-prerequisite-check-output"></a>Örnek NGC ön koşul kontrol çıktısı

```
+----------------------------------------------------------------------+
| Ngc Prerequisite Check                                               |
+----------------------------------------------------------------------+

            IsDeviceJoined : YES
             IsUserAzureAD : YES
             PolicyEnabled : YES
          PostLogonEnabled : YES
            DeviceEligible : YES
        SessionIsNotRemote : YES
            CertEnrollment : enrollment authority
          AdfsRefreshToken : YES
             AdfsRaIsReady : YES
    LogonCertTemplateReady : YES ( StateReady )
              PreReqResult : WillProvision
+----------------------------------------------------------------------+
```

## <a name="next-steps"></a>Sonraki adımlar

Sorularınız için [cihaz yönetimi SSS'ye](faq.md) bakın
