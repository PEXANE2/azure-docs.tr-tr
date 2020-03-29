---
title: Azure AD Birden Çok Etki Alanını Bağla
description: Bu belge, O365 ve Azure AD ile birden çok üst düzey etki alanı kurmayı ve yapılandırmayı açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/31/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 18b5f19e3e994aa05fa99caf360d0c1be69ec7a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80049780"
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Azure AD ile Federasyon için Çoklu Etki Alanı Desteği
Aşağıdaki belgeler, Office 365 veya Azure AD etki alanlarıyla beslenirken birden çok üst düzey etki alanı nın ve alt etki alanının nasıl kullanılacağı nailişkin kılavuz sağlar.

## <a name="multiple-top-level-domain-support"></a>Birden çok üst düzey etki alanı desteği
Azure AD ile birden çok, üst düzey etki alanını feserating bir üst düzey etki alanı ile fetoderecelendirme gerekli olmayan bazı ek yapılandırma gerektirir.

Bir etki alanı Azure AD ile federe olduğunda, Azure'daki etki alanında çeşitli özellikler ayarlanır.  Önemli olanlardan biri IssuerUri' dir.  Bu özellik, belirteçle ilişkili etki alanını tanımlamak için Azure AD tarafından kullanılan bir URI'dir.  URI bir şey çözmek gerekmez ama geçerli bir URI olmalıdır.  Varsayılan olarak, Azure AD URI'yi şirket içi AD FS yapılandırmanızda federasyon hizmet tanımlayıcısının değerine ayarlar.

> [!NOTE]
> Federasyon hizmet tanımlayıcısı, bir federasyon hizmetini benzersiz olarak tanımlayan bir URI'dir.  Federasyon hizmeti, güvenlik belirteci hizmeti olarak işlev gösteren AD FS örneğidir.
>
>

PowerShell komutunu `Get-MsolDomainFederationSettings -DomainName <your domain>`kullanarak IssuerUri'yi görüntüleyebilirsiniz.

![Get-MsolDomainFederationSettings](./media/how-to-connect-install-multiple-domains/MsolDomainFederationSettings.png)

Birden fazla üst düzey etki alanı eklediğinizde bir sorun ortaya çıkar.  Örneğin, Azure AD ile şirket ortamınız arasında federasyon kurduğunuzu varsayalım.  Bu belge için etki alanı, bmcontoso.com kullanılıyor.  Şimdi ikinci, üst düzey etki alanı, bmfabrikam.com eklendi.

![Etki Alanları](./media/how-to-connect-install-multiple-domains/domains.png)

bmfabrikam.com etki alanını federe olarak dönüştürmeye çalıştığınızda bir hata oluşur.  Bunun nedeni, Azure AD'nin, VerenUri özelliğinin birden fazla etki alanı için aynı değere sahip olmasına izin vermeyen bir kısıtlamaya sahip olmasıdır.  

![Federasyon hatası](./media/how-to-connect-install-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>DestekMultipleDomain Parametresi
Bu kısıtlamayı çözmek için, `-SupportMultipleDomain` parametre kullanılarak yapılabilir farklı bir IssuerUri eklemeniz gerekir.  Bu parametre aşağıdaki cmdlets ile kullanılır:

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

Bu parametre, Azure AD'nin VerenUri'yi etki alanının adını temel alabilmesi için yapılandırmasını sağlar.  IssuerUri, Azure AD'deki dizinler arasında benzersiz olacaktır.  Parametrenin kullanılması PowerShell komutunu başarıyla tamamlamasını sağlar.

![Federasyon hatası](./media/how-to-connect-install-multiple-domains/convert.png)

bmfabrikam.com etki alanı ayarlarına baktığınızda aşağıdakileri görebilirsiniz:

![Federasyon hatası](./media/how-to-connect-install-multiple-domains/settings.png)

`-SupportMultipleDomain`yine de adfs.bmcontoso.com federasyon hizmetine işaret etmek üzere yapılandırılan diğer uç noktaları değiştirmez.

Bunu yapan `-SupportMultipleDomain` bir diğer şey de, AD FS sisteminin Azure AD için verilen belirteçlerde uygun Veren değerini içmesini sağlamasıdır. Bu değer, kullanıcıların etki alanı bölümünü UPN alarak ve IssuerUri etki alanı olarak ayarlayarak ayarlanır, yani https://{upn soneki}/adfs/services/trust.

Bu nedenle, Azure AD veya Office 365 kimlik doğrulaması sırasında, kullanıcının belirtecindeki VerenUri öğesi, etki alanını Azure AD'de bulmak için kullanılır.  Eşleşme bulunamazsa, kimlik doğrulama başarısız olur.

Örneğin, bir kullanıcının UPN'si bsimon@bmcontoso.com, belirteçteki VerenUri öğesi ise, AD `http://bmcontoso.com/adfs/services/trust`FS sorunları. Bu öğe Azure AD yapılandırması ile eşleşir ve kimlik doğrulama başarılı olur.

Bu mantığı uygulayan özelleştirilmiş talep kuralı aşağıda veda eder:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> Yeni veya zaten varolan etki alanlarını dönüştürmeye çalışırken -SupportMultipleDomain anahtarını kullanabilmek için, federe güveninizin bunları destekleyecek şekilde ayarlanmış olması gerekir.
>
>

## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>AD FS ve Azure AD arasındaki güven nasıl güncellenir?
AD FS ile Azure AD örneğiniz arasında federe güveni ayarlamadıysanız, bu güveni yeniden oluşturmanız gerekebilir.  Bunun nedeni, başlangıçta `-SupportMultipleDomain` parametre olmadan ayarlandığında, VerenUri varsayılan değer ile ayarlanır.  Aşağıdaki ekran görüntüsünde, IssuerUri'nin .' `https://adfs.bmcontoso.com/adfs/services/trust`

Azure AD portalına başarıyla yeni bir etki alanı eklediyseniz `Convert-MsolDomaintoFederated -DomainName <your domain>`ve sonra kullanarak dönüştürmeye çalıştıysanız, aşağıdaki hatayı alırsınız.

![Federasyon hatası](./media/how-to-connect-install-multiple-domains/trust1.png)

`-SupportMultipleDomain` Anahtarı eklemeye çalışırsanız, aşağıdaki hatayı alırsınız:

![Federasyon hatası](./media/how-to-connect-install-multiple-domains/trust2.png)

Yalnızca özgün `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` etki alanında çalıştırmaya çalışmak da bir hataya neden olur.

![Federasyon hatası](./media/how-to-connect-install-multiple-domains/trust3.png)

Ek bir üst düzey etki alanı eklemek için aşağıdaki adımları kullanın.  Zaten bir etki alanı eklediyseniz ve `-SupportMultipleDomain` parametreyi kullanmadıysanız, özgün etki alanınızı kaldırmak ve güncelleştirmek için adımlarla başlayın.  Henüz üst düzey bir etki alanı eklemediyseniz, Azure AD Connect'in PowerShell'ini kullanarak bir etki alanı ekleme adımlarıyla başlayabilirsiniz.

Microsoft Online güvenini kaldırmak ve özgün etki alanınızı güncelleştirmek için aşağıdaki adımları kullanın.

1. AD FS federasyon sunucunuzda **AD FS Management'ı açın.**
2. Solda, Güven **İlişkilerini** ve **Güvenerek Parti Güvenlerini genişletin**
3. Sağdaki Microsoft **Office 365 Kimlik Platformu** girişini silin.
   ![Microsoft Çevrimiçi'ni Kaldırma](./media/how-to-connect-install-multiple-domains/trust4.png)
4. [Windows PowerShell için Azure Active Directory Modülü](https://msdn.microsoft.com/library/azure/jj151815.aspx) yüklü olan bir `$cred=Get-Credential`makinede aşağıdakileri çalıştırın: .  
5. Federating olan Azure AD etki alanı için global bir yöneticinin kullanıcı adını ve parolasını girin.
6. PowerShell'de, girin`Connect-MsolService -Credential $cred`
7. PowerShell'e `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`girin.  Bu güncelleştirme orijinal etki alanı içindir.  Yani yukarıdaki etki alanlarını kullanarak olacaktır:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`

PowerShell'i kullanarak yeni üst düzey etki alanını eklemek için aşağıdaki adımları kullanın

1. [Windows PowerShell için Azure Active Directory Modülü](https://msdn.microsoft.com/library/azure/jj151815.aspx) yüklü olan bir `$cred=Get-Credential`makinede aşağıdakileri çalıştırın: .  
2. Federating olan Azure AD etki alanı için global bir yöneticinin kullanıcı adını ve parolasını girin
3. PowerShell'de, girin`Connect-MsolService -Credential $cred`
4. PowerShell'de, girin`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Azure AD Connect kullanarak yeni üst düzey etki alanını eklemek için aşağıdaki adımları kullanın.

1. Masaüstünden Azure AD Connect'i başlatın veya başlat Menüsünden
2. "Ek Bir Azure REKLAM ![Etki Alanı Ekle" seçeneğini seçin Ek bir Azure REKLAM etki alanı ekleme](./media/how-to-connect-install-multiple-domains/add1.png)
3. Azure AD ve Etkin Dizin kimlik bilgilerinizi girin
4. Federasyon için yapılandırmak istediğiniz ikinci etki alanını seçin.
   ![Ek bir Azure AD etki alanı ekleme](./media/how-to-connect-install-multiple-domains/add2.png)
5. Yükle'ye tıklayın

### <a name="verify-the-new-top-level-domain"></a>Yeni üst düzey etki alanını doğrulama
PowerShell komutunu `Get-MsolDomainFederationSettings -DomainName <your domain>`kullanarak güncelleştirilmiş VerenUri'yi görüntüleyebilirsiniz.  Aşağıdaki ekran görüntüsü, federasyon ayarlarının orijinal etki alanında güncelleştirilen`http://bmcontoso.com/adfs/services/trust`

![Get-MsolDomainFederationSettings](./media/how-to-connect-install-multiple-domains/MsolDomainFederationSettings.png)

Ve yeni etki alanında IssuerUri için ayarlanmıştir`https://bmfabrikam.com/adfs/services/trust`

![Get-MsolDomainFederationSettings](./media/how-to-connect-install-multiple-domains/settings2.png)

## <a name="support-for-subdomains"></a>Alt etki alanları için destek
Azure AD etki alanlarını ele alma biçimi nedeniyle bir alt etki alanı eklediğinizde, üst alan daki ayarları devralır.  Yani, IssuerUri, veliler maç gerekiyor.

Yani diyelim ki, örneğin, ben bmcontoso.com ve sonra corp.bmcontoso.com ekleyin.  corp.bmcontoso.com bir kullanıcı için IssuerUri olması ** http://bmcontoso.com/adfs/services/trustgerekir.**  Ancak Azure AD için yukarıda uygulanan standart kural, bir veren ile bir belirteç ** http://corp.bmcontoso.com/adfs/services/trustoluşturacaktır .** etki alanının gerekli değeriyle eşleşmez ve kimlik doğrulaması başarısız olur.

### <a name="how-to-enable-support-for-subdomains"></a>Alt etki alanları için desteği etkinleştirme
Bu davranışın çözümü için, Microsoft Online için AD FS güvenveren parti güveninin güncellenmesi gerekir.  Bunu yapmak için, özel Veren değerini yaparken kullanıcının UPN sonekinden alt etki alanlarını sökecek şekilde özel bir talep kuralı nı yapılandırmanız gerekir.

Aşağıdaki iddia bunu yapacaktır:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
Normal ifade kümesindeki son sayı, kök etki alanınızda kaç üst etki alanı olduğudur. Burada bmcontoso.com kullanılır, bu nedenle iki üst etki alanı gereklidir. Üç üst etki alanı tutulacak olsaydı (yani: corp.bmcontoso.com), sayı üç olurdu. Sonunda bir aralık belirtilebilir, maç her zaman etki alanlarının maksimum maç için yapılacaktır. "{2,3}" " iki ila üç etki alanıyla (yani: bmfabrikam.com ve corp.bmcontoso.com) eşleşir.

Alt etki alanlarını desteklemek için özel bir talep eklemek için aşağıdaki adımları kullanın.

1. Açık AD FS Yönetimi
2. Microsoft Online RP güvenini sağ tıklatın ve Talep Edin kurallarını seçin
3. Üçüncü talep kuralını seçin ![ve Reddi Edit talebini değiştirin](./media/how-to-connect-install-multiple-domains/sub1.png)
4. Geçerli talebi değiştirin:

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));

       with

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Talebi değiştirme](./media/how-to-connect-install-multiple-domains/sub2.png)

5. Tamam'ı tıklatın.  Uygula'yı tıklatın.  Tamam'ı tıklatın.  AD FS Yönetimi'ni kapatın.

## <a name="next-steps"></a>Sonraki adımlar
Azure AD Connect'i yüklediniz, artık [yüklemeyi doğrulayabilir ve lisans atayabilirsiniz](how-to-connect-post-installation.md).

Yüklemeyle etkinleştirilen özellikler hakkında daha fazla bilgi edinin: [Otomatik yükseltme](how-to-connect-install-automatic-upgrade.md), [Yanlışlıkla silmeleri engelleme](how-to-connect-sync-feature-prevent-accidental-deletes.md) ve [Azure AD Connect Health](how-to-connect-health-sync.md).

Şu genel konu başlıkları hakkında daha fazla bilgi edinin: [Zamanlayıcı ve eşitleme tetikleme](how-to-connect-sync-feature-scheduler.md).

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
