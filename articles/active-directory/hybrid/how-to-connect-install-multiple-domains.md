---
title: Birden çok etki alanı Azure AD Connect
description: Bu belgede O365 ve Azure AD ile birden çok üst düzey etki alanı ayarlama ve yapılandırma açıklanmaktadır.
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
ms.topic: how-to
ms.date: 05/31/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7a49abdea9d5b80687c53fbaa3d41480825ed504
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85849942"
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Azure AD ile Federasyon için Çoklu Etki Alanı Desteği
Aşağıdaki belgelerde, Office 365 veya Azure AD etki alanlarıyla federasyona göre birden çok üst düzey etki alanı ve alt etki alanı kullanma hakkında rehberlik verilmektedir.

## <a name="multiple-top-level-domain-support"></a>Birden çok üst düzey etki alanı desteği
Azure AD ile birden çok üst düzey etki alanını federasyona eklemek için, bir üst düzey etki alanı ile federasyona göre gerekli olmayan bazı ek yapılandırmalar gerekir.

Bir etki alanı Azure AD ile federe olduğunda, Azure 'daki etki alanında birkaç özellik ayarlanır.  Önemli biri ıssueruri ' dir.  Bu özellik, Azure AD tarafından belirtecin ilişkilendirildiği etki alanını tanımlamak için kullanılan bir URI 'dir.  URI 'nin her şeye çözülmesi gerekmez, ancak geçerli bir URI olmalıdır.  Varsayılan olarak, Azure AD, URI 'yi şirket içi AD FS yapılandırmanızda Federasyon Hizmeti tanımlayıcısının değerine ayarlar.

> [!NOTE]
> Federasyon Hizmeti tanımlayıcısı, bir Federasyon hizmetini benzersiz bir şekilde tanımlayan bir URI 'dir.  Federasyon Hizmeti, güvenlik belirteci hizmeti olarak işlev gören bir AD FS örneğidir.
>
>

Issueruri ' i PowerShell komutunu kullanarak görüntüleyebilirsiniz `Get-MsolDomainFederationSettings -DomainName <your domain>` .

![Get-MsolDomainFederationSettings](./media/how-to-connect-install-multiple-domains/MsolDomainFederationSettings.png)

Birden fazla en üst düzey etki alanı eklediğinizde bir sorun ortaya çıkar.  Örneğin, Azure AD ile şirket içi ortamınız arasında federasyon ayarınızın olduğunu varsayalım.  Bu belge için bmcontoso.com etki alanı kullanılır.  Şimdi ikinci bir üst düzey etki alanı, bmfabrikam.com eklenmiştir.

![Etki Alanları](./media/how-to-connect-install-multiple-domains/domains.png)

Bmfabrikam.com etki alanını federasyona dönüştürmeye çalıştığınızda bir hata oluşur.  Bunun nedeni, Azure AD 'nin birden fazla etki alanı için aynı değere sahip olduğundan ıssueruri özelliğinin aynı değere sahip olmasını engelleyen bir kısıtlaması vardır.  

![Federasyon hatası](./media/how-to-connect-install-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>SupportMultipleDomain parametresi
Bu kısıtlamayı geçici olarak çözmek için parametresi kullanılarak yapılabilen farklı bir ıssueruri eklemeniz gerekir `-SupportMultipleDomain` .  Bu parametre aşağıdaki cmdlet 'lerle kullanılır:

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

Bu parametre, Azure AD 'nin ıssueruri 'yi etki alanının adını temel alarak yapılandırmasını sağlar.  Issueruri, Azure AD 'de dizinler arasında benzersiz olacaktır.  Parametresinin kullanılması PowerShell komutunun başarıyla tamamlanmasını sağlar.

![Federasyon hatası](./media/how-to-connect-install-multiple-domains/convert.png)

Bmfabrikam.com etki alanının ayarlarına bakarak aşağıdakileri görebilirsiniz:

![Federasyon hatası](./media/how-to-connect-install-multiple-domains/settings.png)

`-SupportMultipleDomain`Hala adfs.bmcontoso.com üzerinde Federasyon Hizmeti 'ne işaret edecek şekilde yapılandırılmış olan diğer uç noktaları değiştirmez.

Diğer bir deyişle, `-SupportMultipleDomain` AD FS sisteminin Azure AD için verilen belirteçlerde uygun veren değerini içermesi güvence altına alınır. Bu değer, kullanıcılar UPN 'sinin etki alanı kısmı alınarak ve ıssueruri içinde etki alanı olarak ayarlanarak ayarlanır, yani https://{UPN soneki}/ADFS/Services/Trust.

Bu nedenle, Azure AD veya Office 365 kimlik doğrulaması sırasında kullanıcının belirtecindeki ıssueruri öğesi, Azure AD 'de etki alanını bulmak için kullanılır.  Bir eşleşme bulunamazsa, kimlik doğrulaması başarısız olur.

Örneğin, bir kullanıcının UPN 'si ise, bsimon@bmcontoso.com belirteçteki ıssueruri öğesi AD FS sorunları olarak ayarlanır `http://bmcontoso.com/adfs/services/trust` . Bu öğe Azure AD yapılandırmasıyla eşleşecek ve kimlik doğrulaması başarılı olur.

Aşağıda, bu mantığı uygulayan özelleştirilmiş talep kuralı verilmiştir:

```
c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));
```


> [!IMPORTANT]
> Zaten var olan etki alanlarını eklemeye veya dönüştürmeye çalışırken,-SupportMultipleDomain anahtarını kullanmak için, Federal güvenin bunları destekleyecek şekilde zaten ayarlanmış olması gerekir.
>
>

## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>AD FS ile Azure AD arasındaki güveni güncelleştirme
AD FS ile Azure AD arasında federasyon güveni ayarlanmamışsa, bu güveni yeniden oluşturmanız gerekebilir.  Nedeni, ilk olarak parametre olmadan ayarlandığında `-SupportMultipleDomain` , ıssueruri varsayılan değerle ayarlanır.  Aşağıdaki ekran görüntüsünde, ıssueruri ' nin olarak ayarlandığını görebilirsiniz `https://adfs.bmcontoso.com/adfs/services/trust` .

Azure AD portalında başarıyla yeni bir etki alanı eklediyseniz ve sonra bunu kullanarak dönüştürmeyi denerseniz `Convert-MsolDomaintoFederated -DomainName <your domain>` , aşağıdaki hatayı alırsınız.

![Federasyon hatası](./media/how-to-connect-install-multiple-domains/trust1.png)

Anahtarı eklemeye çalışırsanız `-SupportMultipleDomain` aşağıdaki hatayı alırsınız:

![Federasyon hatası](./media/how-to-connect-install-multiple-domains/trust2.png)

Yalnızca `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` özgün etki alanı üzerinde çalışmayı denemek bir hataya neden olur.

![Federasyon hatası](./media/how-to-connect-install-multiple-domains/trust3.png)

Ek bir üst düzey etki alanı eklemek için aşağıdaki adımları kullanın.  Zaten bir etki alanı eklediyseniz ve `-SupportMultipleDomain` parametresini kullanmadıysanız, özgün etki alanınızı kaldırma ve güncelleştirme adımlarını kullanmaya başlayın.  Henüz en üst düzey bir etki alanı eklemediyseniz Azure AD Connect PowerShell kullanarak etki alanı ekleme adımlarıyla başlayabilirsiniz.

Microsoft Online güvenini kaldırmak ve özgün etki alanınızı güncelleştirmek için aşağıdaki adımları kullanın.

1. AD FS Federasyon sunucunuzda **AD FS Yönetimi** 'ni açın.
2. Sol tarafta **güven ilişkileri** ve **bağlı olan taraf güvenleri** ' ni genişletin.
3. Sağ tarafta **Microsoft Office 365 kimlik Platform** girişini silin.
   ![Microsoft Online 'ı kaldır](./media/how-to-connect-install-multiple-domains/trust4.png)
4. [Windows PowerShell için Azure Active Directory modülü](https://msdn.microsoft.com/library/azure/jj151815.aspx) yüklü bir makinede aşağıdakileri çalıştırın: `$cred=Get-Credential` .  
5. Federasyona sahip olduğunuz Azure AD etki alanı için genel bir yöneticinin kullanıcı adını ve parolasını girin.
6. PowerShell 'de, şunu girin`Connect-MsolService -Credential $cred`
7. PowerShell 'de, girin `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain` .  Bu güncelleştirme, özgün etki alanı içindir.  Bu nedenle, yukarıdaki etki alanlarını kullanarak şöyle olacaktır:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`

PowerShell kullanarak yeni en üst düzey etki alanını eklemek için aşağıdaki adımları kullanın

1. [Windows PowerShell için Azure Active Directory modülü](https://msdn.microsoft.com/library/azure/jj151815.aspx) yüklü bir makinede aşağıdakileri çalıştırın: `$cred=Get-Credential` .  
2. Federasyona eklemek istediğiniz Azure AD etki alanı için genel bir yöneticinin kullanıcı adını ve parolasını girin
3. PowerShell 'de, şunu girin`Connect-MsolService -Credential $cred`
4. PowerShell 'de, şunu girin`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Azure AD Connect kullanarak yeni en üst düzey etki alanını eklemek için aşağıdaki adımları kullanın.

1. Masaüstünden veya Başlat menüsünden Azure AD Connect başlatın
2. "Ek Azure AD etki alanı Ekle" seçeneğini belirleyin. ![ ek bir Azure AD etki alanı ekleyin](./media/how-to-connect-install-multiple-domains/add1.png)
3. Azure AD ve Active Directory kimlik bilgilerinizi girin
4. Federasyon için yapılandırmak istediğiniz ikinci etki alanını seçin.
   ![Ek bir Azure AD etki alanı ekleme](./media/how-to-connect-install-multiple-domains/add2.png)
5. Yükle'ye tıklayın

### <a name="verify-the-new-top-level-domain"></a>Yeni en üst düzey etki alanını doğrulama
PowerShell komutunu kullanarak `Get-MsolDomainFederationSettings -DomainName <your domain>` güncelleştirilmiş ıssueruri 'yi görüntüleyebilirsiniz.  Aşağıdaki ekran görüntüsünde, Federasyon ayarlarının orijinal etki alanında güncelleştirildiği gösterilmektedir`http://bmcontoso.com/adfs/services/trust`

![Get-MsolDomainFederationSettings](./media/how-to-connect-install-multiple-domains/MsolDomainFederationSettings.png)

Ve yeni etki alanındaki ıssueruri şu şekilde ayarlandı`https://bmfabrikam.com/adfs/services/trust`

![Get-MsolDomainFederationSettings](./media/how-to-connect-install-multiple-domains/settings2.png)

## <a name="support-for-subdomains"></a>Alt etki alanları için destek
Bir alt etki alanı eklediğinizde, Azure AD tarafından işlenen etki alanlarının yolu nedeniyle üst öğenin ayarları devralınır.  Bu nedenle, ıssuerurı 'nin üst öğe ile eşleşmesi gerekir.

Bu nedenle, örneğin, bmcontoso.com ve ardından corp.bmcontoso.com ' i eklemem gibi bir şekilde izin verir.  Corp.bmcontoso.com 'ten bir kullanıcının ıssueruri 'Si olması gerekir **`http://bmcontoso.com/adfs/services/trust`** .  Ancak, Azure AD için yukarıda uygulanan standart kural, veren olarak bir belirteç üretir **`http://corp.bmcontoso.com/adfs/services/trust`** . Bu, etki alanının gerekli değeriyle eşleşmeyecek ve kimlik doğrulaması başarısız olacak.

### <a name="how-to-enable-support-for-subdomains"></a>Alt etki alanları desteğini etkinleştirme
Bu davranışa geçici bir çözüm için, Microsoft Online için AD FS bağlı olan taraf güveninin güncelleştirilmesi gerekir.  Bunu yapmak için özel bir talep kuralı yapılandırmanız gerekir, böylece özel veren değerini oluştururken kullanıcının UPN sonekine ait tüm alt etki alanlarını kaldırır.

Aşağıdaki talep bunu kullanacaktır:

```    
c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));
```

[!NOTE]
Normal ifade kümesindeki son sayı, kök etki alanlarınızda kaç tane üst etki alanı olduğunu gösteren bir sayıdır. Burada bmcontoso.com kullanılır, bu nedenle iki üst etki alanı gereklidir. Üç üst etki alanı tutuluyorsa (ör.: corp.bmcontoso.com), sayı üç olur. Sonuç olarak bir Aralık belirtilebilir, eşleşme her zaman en fazla etki alanı ile eşleşecek şekilde yapılır. " {2,3} " iki ile üç etki alanı ile eşleşir (örn: bmfabrikam.com ve Corp.bmcontoso.com).

Alt etki alanlarını desteklemek için özel bir talep eklemek üzere aşağıdaki adımları kullanın.

1. AD FS Yönetimi 'ni açın
2. Microsoft Online RP güvenine sağ tıklayın ve talep kurallarını Düzenle ' yi seçin.
3. Üçüncü talep kuralını seçin ve ![ düzenleme talebini değiştirin](./media/how-to-connect-install-multiple-domains/sub1.png)
4. Geçerli talebi Değiştir:

   ```
   c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
   ```
    örneklerini şununla değiştirin:

   ```
   c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));
   ```

    ![Talebi Değiştir](./media/how-to-connect-install-multiple-domains/sub2.png)

5. Tamam ' a tıklayın.  Uygula'ya tıklayın.  Tamam ' a tıklayın.  AD FS Yönetimi'ni kapatın.

## <a name="next-steps"></a>Sonraki adımlar
Azure AD Connect'i yüklediniz, artık [yüklemeyi doğrulayabilir ve lisans atayabilirsiniz](how-to-connect-post-installation.md).

Yüklemeyle etkinleştirilen özellikler hakkında daha fazla bilgi edinin: [Otomatik yükseltme](how-to-connect-install-automatic-upgrade.md), [Yanlışlıkla silmeleri engelleme](how-to-connect-sync-feature-prevent-accidental-deletes.md) ve [Azure AD Connect Health](how-to-connect-health-sync.md).

Şu genel konu başlıkları hakkında daha fazla bilgi edinin: [Zamanlayıcı ve eşitleme tetikleme](how-to-connect-sync-feature-scheduler.md).

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
