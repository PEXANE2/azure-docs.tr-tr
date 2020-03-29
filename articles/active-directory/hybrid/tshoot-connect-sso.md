---
title: 'Azure Active Directory Connect: Sorunsuz Tek Oturum Açma | Microsoft Dokümanlar'
description: Bu konu, Azure Active Directory Seamless Tek Oturum Açma sorunu nasıl giderilen açıklar
services: active-directory
author: billmath
ms.reviewer: swkrish
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 10/07/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 759748124893a8f906a4bc336f835546202b0b62
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80049487"
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Sorun Giderme Azure Active Directory Sorunsuz Tek Oturum Açma

Bu makale, Azure Etkin Dizini (Azure AD) Sorunsuz Tek Oturum Açma (Sorunsuz SSO) ile ilgili sık karşılaşılan sorunlarla ilgili sorun giderme bilgilerini bulmanıza yardımcı olur.

## <a name="known-issues"></a>Bilinen sorunlar

- Birkaç durumda, Sorunsuz SSO etkinleştirme 30 dakika kadar sürebilir.
- Kiracınızda Sorunsuz SSO'yu devre dışı bırakıp yeniden etkinleştirirseniz, kullanıcılar genellikle 10 saat geçerli olan önbelleğe alınmış Kerberos biletleri nin süresi dolmadan tek bir oturum açma deneyimi elde etmez.
- Sorunsuz SSO başarılı olursa, kullanıcı beni oturum **açmış tut'u**seçme fırsatına sahip değildir. Bu davranış [nedeniyle, SharePoint ve OneDrive eşleme senaryoları](https://support.microsoft.com/help/2616712/how-to-configure-and-to-troubleshoot-mapped-network-drives-that-connec) çalışmıyor.
- 16.0.8730.xxxx ve üzeri sürümlere sahip Office 365 Win32 istemcileri (Outlook, Word, Excel ve diğerleri) etkileşimli olmayan bir akış kullanılarak desteklenir. Diğer sürümler desteklenmez; bu sürümlerde, kullanıcılar oturum açmaları için parolalarını değil, kullanıcı adlarını girerler. OneDrive için sessiz bir oturum açma deneyimi için [OneDrive sessiz config özelliğini](https://techcommunity.microsoft.com/t5/Microsoft-OneDrive-Blog/Previews-for-Silent-Sync-Account-Configuration-and-Bandwidth/ba-p/120894) etkinleştirmeniz gerekir.
- Sorunsuz SSO Firefox'ta özel tarama modunda çalışmıyor.
- Gelişmiş Korumalı mod açık olduğunda Sorunsuz SSO Internet Explorer'da çalışmaz.
- Sorunsuz SSO iOS ve Android'deki mobil tarayıcılarda çalışmaz.
- Bir kullanıcı Active Directory'de çok fazla grubun parçasıysa, kullanıcının Kerberos bileti büyük olasılıkla işlenmemesi için çok büyük olacaktır ve bu da Sorunsuz SSO'nun başarısız lığa neden olur. Azure AD HTTPS isteklerinde en fazla 50 KB boyutuna sahip üstbilgi olabilir; Kerberos biletlerinin çerezler gibi diğer Azure REKLAM yapılarını (genellikle 2 - 5 KB) barındırmak için bu sınırdan daha küçük olması gerekir. Tavsiyemiz, kullanıcının grup üyeliklerini azaltmak ve yeniden denemektir.
- 30 veya daha fazla Active Directory ormanLarını eşitlıyorsanız, Azure AD Connect aracılığıyla Kesintisiz SSO'ya olanak sayılamaz. Geçici çözüm olarak, kiracınızdaki özelliği [el ile etkinleştirebilirsiniz.](#manual-reset-of-the-feature)
- Azure AD hizmet URL'sinin Yerel`https://autologon.microsoftazuread-sso.com`intranet bölgesi yerine Güvenilen siteler bölgesine eklenmesi kullanıcıların oturum *açmasını engeller.*
- Sorunsuz SSO, Kerberos için AES256_HMAC_SHA1, AES128_HMAC_SHA1 ve RC4_HMAC_MD5 şifreleme türlerini destekler. AzureADSSOAcc$ hesabının şifreleme türünün AES256_HMAC_SHA1 veya ek güvenlik için AES ve RC4 türlerinden birine ayarlanması önerilir. Şifreleme türü, Active Dizininizdeki hesabın msDS Destekli Şifreleme Türleri özniteliği üzerinde depolanır.  AzureADSSOAcc$ hesap şifreleme türü RC4_HMAC_MD5 olarak ayarlanmışsa ve bunu AES şifreleme türlerinden biriyle değiştirmek istiyorsanız, lütfen azureADSSOAcc$ hesabının Kerberos şifre çözme anahtarını ilgili soru altında [SSS belgesinde](how-to-connect-sso-faq.md) açıklandığı gibi devraldığınızdan emin olun, aksi takdirde Sorunsuz SSO gerçekleşmez.

## <a name="check-status-of-feature"></a>Özelliğin durumunu denetleme

Sorunsuz SSO özelliğinin kiracınızda hala **etkin** olduğundan emin olun. [Azure Active Directory yönetici merkezindeki](https://aad.portal.azure.com/)Azure AD **Bağlantı** bölmesine giderek durumu kontrol edebilirsiniz.

![Azure Active Directory yönetici merkezi: Azure AD Connect bölmesi](./media/tshoot-connect-sso/sso10.png)

Sorunsuz SSO için etkinleştirilen tüm AD ormanlarını görmek için tıklayın.

![Azure Active Directory yönetici merkezi: Kesintisiz SSO bölmesi](./media/tshoot-connect-sso/sso13.png)

## <a name="sign-in-failure-reasons-in-the-azure-active-directory-admin-center-needs-a-premium-license"></a>Azure Active Directory yönetici merkezinde oturum açma hatası nedenleri (Premium lisansa ihtiyaç duyar)

Kiracınızda bununla ilişkili bir Azure AD Premium lisansı varsa, [Azure Etkin Dizin yönetici merkezindeki](https://aad.portal.azure.com/)oturum açma faaliyet [raporuna](../reports-monitoring/concept-sign-ins.md) da bakabilirsiniz.

![Azure Active Directory yönetici merkezi: Oturum açma raporu](./media/tshoot-connect-sso/sso9.png)

[Azure Active Directory yönetici merkezinde](https://aad.portal.azure.com/)Azure Active **Directory** > **Oturum** Açma'ya göz atın ve ardından belirli bir kullanıcının oturum açma etkinliğini seçin. **OTURUM AÇMA HATA KODU** alanına bakın. Aşağıdaki tabloyu kullanarak bu alanın değerini bir hata nedeni ve çözümle eşleştirin:

|Oturum açma hata kodu|Oturum açma hatası nedeni|Çözüm
| --- | --- | ---
| 81001 | Kullanıcının Kerberos anahtarı fazla büyük. | Kullanıcının grup üyeliklerini azaltın ve yeniden deneyin.
| 81002 | Kullanıcının Kerberos biletini doğrulayamıyor. | Sorun [giderme denetim listesine](#troubleshooting-checklist)bakın.
| 81003 | Kullanıcının Kerberos biletini doğrulayamıyor. | Sorun [giderme denetim listesine](#troubleshooting-checklist)bakın.
| 81004 | Kerberos kimlik doğrulaması girişimi başarısız oldu. | Sorun [giderme denetim listesine](#troubleshooting-checklist)bakın.
| 81008 | Kullanıcının Kerberos biletini doğrulayamıyor. | Sorun [giderme denetim listesine](#troubleshooting-checklist)bakın.
| 81009 | Kullanıcının Kerberos biletini doğrulayamıyor. | Sorun [giderme denetim listesine](#troubleshooting-checklist)bakın.
| 81010 | Kullanıcının Kerberos anahtarının süresi dolduğu veya anahtar geçersiz olduğu için sorunsuz SSO başarısız oldu. | Kullanıcının şirket ağınızdaki etki alanına katılan bir aygıttan oturum açması gerekir.
| 81011 | Kullanıcının Kerberos biletindeki bilgilere dayanarak kullanıcı nesnesini bulamıyor. | Kullanıcıbilgilerini Azure AD ile senkronize etmek için Azure AD Connect'i kullanın.
| 81012 | Azure AD'de oturum açmaya çalışan kullanıcı, aygıtta oturum açan kullanıcıdan farklıdır. | Kullanıcının farklı bir aygıttan oturum açması gerekir.
| 81013 | Kullanıcının Kerberos biletindeki bilgilere dayanarak kullanıcı nesnesini bulamıyor. |Kullanıcıbilgilerini Azure AD ile senkronize etmek için Azure AD Connect'i kullanın. 

## <a name="troubleshooting-checklist"></a>Sorun giderme denetim listesi

Sorunsuz SSO sorunlarını gidermek için aşağıdaki denetim listesini kullanın:

- Azure AD Connect'te Kesintisiz SSO özelliğinin etkin olduğundan emin olun. Özelliği etkinleştiremezseniz (örneğin, engellenen bir bağlantı noktası nedeniyle), tüm [ön koşulların](how-to-connect-sso-quick-start.md#step-1-check-the-prerequisites) yerinde olduğundan emin olun.
- Kiracınızda hem [Azure AD Join'i](../active-directory-azureadjoin-overview.md) hem de Kesintisiz SSO'yu etkinleştirdiyseniz, sorunun Azure AD Join'te olmadığından emin olun. Azure AD Join'in SSO'su, aygıtın hem Azure AD'ye kayıtlı hem de etki alanına dahil olması durumunda Sorunsuz SSO'dan önce gelir. Azure AD'den SSO ile Join kullanıcı üzerinde "Windows'a Bağlı" yazan bir oturum açma döşemesi görür.
- Azure REKLAM URL'sinin`https://autologon.microsoftazuread-sso.com`kullanıcının Intranet bölge ayarlarının bir parçası olduğundan emin olun.
- Kurumsal aygıtın Active Directory etki alanına katıldığından emin olun. Sorunsuz _SSO'nun_ çalışması için aygıtın [Azure AD Joined](../active-directory-azureadjoin-overview.md) olması gerekmez.
- Kullanıcının Etkin Dizin etki alanı hesabı üzerinden aygıtta oturum açtığından emin olun.
- Kullanıcının hesabının Kesintisiz SSO'nun kurulduğu bir Active Directory ormanından olduğundan emin olun.
- Aygıtın şirket ağına bağlı olduğundan emin olun.
- Aygıtın zamanının hem Active Directory'deki hem de etki alanı denetleyicilerindeki süreyle eşitlendirildiğından ve bunların birbirinden beş dakika içinde olduğundan emin olun.
- Bilgisayar hesabının `AZUREADSSOACC` her AD ormanında, Kesintisiz SSO'nun etkin olmasını istediğiniz şekilde etkin olduğundan emin olun. Bilgisayar hesabı silinmişse veya eksikse, bunları yeniden oluşturmak için [PowerShell cmdlets'i](#manual-reset-of-the-feature) kullanabilirsiniz.
- Bir komut istemi komutunu kullanarak `klist` cihazda mevcut Kerberos biletliste. Bilgisayar hesabı için verilen `AZUREADSSOACC` biletlerin mevcut olduğundan emin olun. Kullanıcıların Kerberos biletleri genellikle 10 saat için geçerlidir. Etkin Dizin'de farklı ayarlarınız olabilir.
- Kiracınızda Sorunsuz SSO'yu devre dışı bıraktıysanız ve yeniden etkinleştirdiyseniz, önbelleğe alınmış Kerberos biletleri nin süresi dolana kadar kullanıcılar tek bir oturum açma deneyimi elde etmez.
- Mevcut Kerberos biletlerini `klist purge` komutu kullanarak cihazdan temizle ve yeniden deneyin.
- JavaScript ile ilgili sorunlar olup olmadığını belirlemek için tarayıcının konsol günlüklerini inceleyin **(Geliştirici Araçları**altında).
- Etki [alanı denetleyici günlüklerini](#domain-controller-logs)gözden geçirin.

### <a name="domain-controller-logs"></a>Etki alanı denetleyici günlükleri

Etki alanı denetleyicinizde başarı denetimine olanak tanırsanız, bir kullanıcı Sorunsuz SSO aracılığıyla her oturum açsa, olay günlüğüne bir güvenlik girişi kaydedilir. Aşağıdaki sorguyu kullanarak bu güvenlik olaylarını bulabilirsiniz. **(AzureADSSOAcc$** bilgisayar hesabıyla ilişkili **4769** olayını arayın.)

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-the-feature"></a>Özelliğin manuel olarak sıfırla

Sorun giderme yardımcı olmadıysa, kiracınızdaki özelliği el ile sıfırlayabilirsiniz. Azure AD Connect'i çalıştırdığınız şirket içi sunucuda aşağıdaki adımları izleyin.

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>Adım 1: Dikişsiz SSO PowerShell modüllerini alma

1. İlk olarak Azure [AD PowerShell'i](https://docs.microsoft.com/powershell/azure/active-directory/overview)indirin ve yükleyin.
2. Klasöre `%programfiles%\Microsoft Azure Active Directory Connect` göz atın.
3. Bu komutu kullanarak Dikişsiz SSO `Import-Module .\AzureADSSO.psd1`PowerShell modüllerini içeri aktarın: .

### <a name="step-2-get-the-list-of-active-directory-forests-on-which-seamless-sso-has-been-enabled"></a>Adım 2: Kesintisiz SSO'nun etkinleştirildiği Active Directory ormanlarının listesini alın

1. PowerShell'i yönetici olarak çalıştırın. PowerShell'de, `New-AzureADSSOAuthenticationContext`arayın. İstendiğinde, kiracınızın genel yönetici kimlik bilgilerini girin.
2. Arayın. `Get-AzureADSSOStatus` Bu komut, bu özelliğin etkinleştirildiği Etkin Dizin ormanları ("Etki Alanları" listesine bakın) listesini sağlar.

### <a name="step-3-disable-seamless-sso-for-each-active-directory-forest-where-youve-set-up-the-feature"></a>Adım 3: Özelliği kurduğunuz her Active Directory ormanı için Sorunsuz SSO'yu devre dışı bırak

1. Arayın. `$creds = Get-Credential` İstendiğinde, amaçlanan Etkin Dizin ormanı için etki alanı yöneticisi kimlik bilgilerini girin.

   > [!NOTE]
   >Etki alanı yöneticisi kimlik bilgileri kullanıcı adı SAM hesap adı biçiminde girilmelidir (contoso\johndoe veya contoso.com\johndoe). DNS kullanarak Etki Alanı Yöneticisi'nin Etki Alanı Denetleyicisini bulmak için kullanıcı adının etki alanı bölümünü kullanırız.

   >[!NOTE]
   >Kullanılan etki alanı yöneticisi hesabı Korumalı Kullanıcılar grubunun bir üyesi olmamalıdır. Bu nedenle, işlem başarısız olur.

2. Arayın. `Disable-AzureADSSOForest -OnPremCredentials $creds` Bu komut, `AZUREADSSOACC` bu özel Active Directory ormanı için bilgisayar hesabını şirket içi etki alanı denetleyicisinden kaldırır.
3. Özelliği kurduğunuz her Active Directory ormanı için önceki adımları yineleyin.

### <a name="step-4-enable-seamless-sso-for-each-active-directory-forest"></a>Adım 4: Her Active Directory ormanı için Kesintisiz SSO'ya olanak sağlama

1. Arayın. `Enable-AzureADSSOForest` İstendiğinde, amaçlanan Etkin Dizin ormanı için etki alanı yöneticisi kimlik bilgilerini girin.

   > [!NOTE]
   >Etki alanı yöneticisi kimlik bilgileri kullanıcı adı SAM hesap adı biçiminde girilmelidir (contoso\johndoe veya contoso.com\johndoe). DNS kullanarak Etki Alanı Yöneticisi'nin Etki Alanı Denetleyicisini bulmak için kullanıcı adının etki alanı bölümünü kullanırız.

   >[!NOTE]
   >Kullanılan etki alanı yöneticisi hesabı Korumalı Kullanıcılar grubunun bir üyesi olmamalıdır. Bu nedenle, işlem başarısız olur.

2. Özelliği ayarlamak istediğiniz her Etkin Dizin ormanı için önceki adımı yineleyin.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>5. Adım. Kiracınızdaki özelliği etkinleştirme

Kiracınızdaki özelliği açmak için . `Enable-AzureADSSO -Enable $true`
