---
title: Yönetilmeyen bir dizinin yönetici tarafından devralınması - Azure AD | Microsoft Dokümanlar
description: Yönetilmeyen bir Azure REKLAM kuruluşunda (gölge kiracı) bir DNS etki alanı adı devralma.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 09012d93a1f9fd24427cb8b3937b3a36cf75d9e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75834180"
---
# <a name="take-over-an-unmanaged-directory-as-administrator-in-azure-active-directory"></a>Azure Etkin Dizin'de yönetici olarak yönetilmeyen bir dizinin devrin

Bu makalede, Azure Etkin Dizin (Azure AD)'de yönetilmeyen bir dizinde bir DNS etki alanı adını devralmanın iki yolu açıklanmaktadır. Bir self servis kullanıcısı, Azure AD kullanan bir bulut hizmetine kaydolduğunda bu kullanıcı, e-posta etki alanına göre yönetilmeyen bir Azure AD dizinine eklenir. Self servis veya bir hizmet için "viral" kayıt hakkında daha fazla şey için Azure [Active Directory için self servis kaydolma nedir?](directory-self-service-signup.md)

## <a name="decide-how-you-want-to-take-over-an-unmanaged-directory"></a>Yönetilmeyen bir dizinin devralmasını nasıl istediğinize karar verin
Yönetici devralma işlemi sırasında, [Azure AD’ye özel etki alanı adı ekleme](../fundamentals/add-custom-domain.md) bölümünde açıklandığı gibi sahipliği kanıtlayabilirsiniz. Sonraki bölümlerde, yönetici deneyimi daha ayrıntılı şekilde açıklanmaktadır, ancak bir özeti aşağıda verilmiştir:

* Yönetilmeyen bir Azure dizininin ["iç" yönetici devralma işlemini](#internal-admin-takeover) gerçekleştirdiğinizde, yönetilmeyen dizinin genel yöneticisi olarak eklendiniz. Herhangi bir kullanıcı, etki alanı veya hizmet planı, yönettiğiniz diğer dizine geçirilmiyor.

* Yönetilmeyen bir Azure dizininin ["dış" yönetici devralma işlemini](#external-admin-takeover) gerçekleştirdiğinizde, yönetilmeyen dizininizin DNS etki alanı adını, yönetilen Azure dizininize eklersiniz. Etki alanı adını eklediğinizde, kullanıcıların kesinti olmadan hizmetlere erişmeye devam edebilmesi için yönetilen Azure dizininizde, kullanıcıların kaynaklara bir eşlemesi oluşturulur. 

## <a name="internal-admin-takeover"></a>Dahili yönetici devralma

Office 365 gibi SharePoint ve OneDrive'ı içeren bazı ürünler harici devralmayı desteklemez. Bu sizin senaryonuzsa veya yöneticiyseniz ve self servis kaydolan kullanıcılar tarafından oluşturulan yönetilmeyen veya "gölge" bir kiracıyı devralmak istiyorsanız, bunu bir dahili yönetici devralma ile yapabilirsiniz.

1. Power BI'ye kaydolarak yönetilmeyen kiracıda bir kullanıcı bağlamı oluşturun. Örneğin kolaylık sağlamak için, bu adımlar bu yolu varsayar.

2. Power [BI sitesini](https://powerbi.com) açın ve **Ücretsiz Başlat'ı**seçin. Kuruluşun etki alanı adını kullanan bir kullanıcı hesabı girin; örneğin, `admin@fourthcoffee.xyz`. Doğrulama kodunu girdikten sonra, onay kodu için e-postanızı kontrol edin.

3. Power BI'nin onay e-postasında **Evet'i seçin, bu benim.**

4. Power BI kullanıcı hesabıyla [Microsoft 365 yönetici merkezinde](https://portal.office.com/admintakeover) oturum açın. Yönetilmeyen kiracıda zaten doğrulanmış olan etki alanı adının **Yöneticisi olmanızı** emreden bir ileti alırsınız. **Evet seçin, ben yönetici olmak istiyorum**.
  
   ![Admin ol için ilk ekran görüntüsü](./media/domains-admin-takeover/become-admin-first.png)
  
5. Etki alanı adı kayıt şirketinizde **dördüncücoffee.xyz** alan adının sahibi olduğunuzu kanıtlamak için TXT kaydını ekleyin. Bu örnekte, GoDaddy.com.
  
   ![Etki alanı adı için txt kaydı ekleme](./media/domains-admin-takeover/become-admin-txt-record.png)

DNS TXT kayıtları alan adı kayıt şirketinizde doğrulandığında, Azure AD kiracısını yönetebilirsiniz.

Önceki adımları tamamladığınızda, artık Office 365'teki Dördüncü Kahve kiracısının genel yöneticisi oluyorsunuz. Alan adını diğer Azure hizmetlerinizle tümleştirmek için, alan adını Office 365'ten kaldırabilir ve Azure'da yönetilen farklı bir kiracıya ekleyebilirsiniz.

### <a name="adding-the-domain-name-to-a-managed-tenant-in-azure-ad"></a>Azure AD'de yönetilen bir kiracıya etki alanı adı ekleme

1. Microsoft [365 yönetici merkezini](https://admin.microsoft.com)açın.
2. **Kullanıcılar** sekmesini seçin ve özel alan adı kullanmayan *kullanıcı\@fourthcoffeexyz.onmicrosoft.com* gibi bir ada sahip yeni bir kullanıcı hesabı oluşturun. 
3. Yeni kullanıcı hesabının Azure AD kiracısı için genel yönetici ayrıcalıklarına sahip olduğundan emin olun.
4. Microsoft 365 yönetici merkezinde **Etki Alanları** sekmesini açın, etki alanı adını seçin ve **Kaldır'ı**seçin. 
  
   ![alan adını Office 365'ten kaldırma](./media/domains-admin-takeover/remove-domain-from-o365.png)
  
5. Office 365'te kaldırılan etki alanı adına başvuran kullanıcı veya gruplarınız varsa, bunların .onmicrosoft.com etki alanı olarak yeniden adlandırılması gerekir. Etki alanı adını silmeye zorlarsanız, bu örnekte *\@* tüm kullanıcılar otomatik olarak fourthcoffeexyz.onmicrosoft.com.
  
6. Azure AD yöneticisinin genel yöneticisi olan bir hesapla [Azure AD yönetici merkezinde](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) oturum açın.
  
7. **Özel alan adları**seçin, ardından etki alanı adını ekleyin. Etki alanı adının sahipliğini doğrulamak için DNS TXT kayıtlarını girmeniz gerekir. 
  
   ![Azure AD'ye eklendi olarak doğrulanan etki alanı](./media/domains-admin-takeover/add-domain-to-azure-ad.png)
  
> [!NOTE]
> Office 365 kiracısında lisansları olan Power BI veya Azure Hakları Yönetimi hizmetikullanıcılarının, alan adı kaldırılırsa panolarını kaydetmeleri gerekir. Onlar *kullanıcı\@fourthcoffee.xyz*yerine *\@kullanıcı fourthcoffeexyz.onmicrosoft.com* gibi bir kullanıcı adı ile oturum açmalı.

## <a name="external-admin-takeover"></a>Harici yönetici devralma

Azure hizmetleri veya Office 365 ile bir kiracıyı zaten yönetiyorsanız, başka bir Azure AD kiracısında zaten doğrulandıysa özel bir etki alanı adı ekleyemezsiniz. Ancak, Azure AD'deki yönetilen kiracınızdan yönetilmeyen bir kiracıyı harici yönetici devralma olarak devralabilirsiniz. Genel yordam, [Azure AD'ye özel bir etki alanı ekleme](../fundamentals/add-custom-domain.md)makalesinden sonra gelir.

Etki alanı adının sahipliğini doğruladığınızda, Azure AD etki alanı adını yönetilmeyen kiracıdan kaldırır ve varolan kiracınıza taşır. Yönetilmeyen bir dizinin harici yönetici devralması, dahili yönetici devralma yla aynı DNS TXT doğrulama işlemini gerektirir. Fark, aşağıdaki de etki alanı adı ile taşınır olmasıdır:

- Kullanıcılar
- Abonelikler
- Lisans atamaları

### <a name="support-for-external-admin-takeover"></a>Harici yönetici devralma desteği
Harici yönetici devralma aşağıdaki çevrimiçi hizmetler tarafından desteklenir:

- Azure Rights Management
- Exchange Online

Desteklenen hizmet planları şunlardır:

- PowerApps Ücretsiz
- PowerFlow Ücretsiz
- Kişiler için RMS
- Microsoft Stream
- Dynamics 365 ücretsiz deneme

Harici yönetici devralma, SharePoint, OneDrive veya Skype For Business dahil olmak üzere hizmet planları olan herhangi bir hizmet için desteklenmez; örneğin, Office ücretsiz abonelik aracılığıyla. 

İsteğe bağlı olarak, etki alanı adını yönetilmeyen kiracıdan kaldırmak ve istenen kiracıda doğrulamak için [ **ForceTakeover** seçeneğini](#azure-ad-powershell-cmdlets-for-the-forcetakeover-option) kullanabilirsiniz. 

#### <a name="more-information-about-rms-for-individuals"></a>Bireyler için RMS hakkında daha fazla bilgi

[Bireyler için RMS](/azure/information-protection/rms-for-individuals)için , yönetilmeyen kiracı sahip olduğunuz kiracıyla aynı bölgede olduğunda, otomatik olarak oluşturulan [Azure Bilgi Koruması kiracı anahtarı](/azure/information-protection/plan-implement-tenant-key) ve varsayılan [koruma şablonları](/azure/information-protection/configure-usage-rights#rights-included-in-the-default-templates) ayrıca etki alanı adı ile birlikte taşınır.

Yönetilmeyen kiracı farklı bir bölgedeyken anahtar ve şablonlar taşınmıyor. Örneğin, yönetilmeyen kiracı Avrupa'daysa ve sahip olduğunuz kuruluş Kuzey Amerika'daysa.

Bireyler için RMS, korumalı içeriği açmak için Azure AD kimlik doğrulamasını desteklemek üzere tasarlanmış olsa da, kullanıcıların içeriği korumasını da engellemez. Kullanıcılar bireyler aboneliği için RMS ile içeriği koruduysa ve anahtar ve şablonlar taşınmışsa, bu içeriğe etki alanı devralındıktan sonra erişilemez.

### <a name="azure-ad-powershell-cmdlets-for-the-forcetakeover-option"></a>ForceTakeover seçeneği için Azure AD PowerShell cmdlets
[PowerShell örneğinde](#powershell-example)kullanılan bu cmdlets görebilirsiniz.

cmdlet | Kullanım
------- | -------
`connect-msolservice` | İstendiğinde, yönetilen kiracınızda oturum açın.
`get-msoldomain` | Geçerli kiracıyla ilişkili alan adlarınızı gösterir.
`new-msoldomain –name <domainname>` | Alan adını doğrulanmamış olarak kiracıya ekler (henüz DNS doğrulaması gerçekleştirilmedi).
`get-msoldomain` | Alan adı artık yönetilen kiracınızla ilişkili alan adları listesine dahil edilir, ancak **Doğrulanmamış**olarak listelenir.
`get-msoldomainverificationdns –Domainname <domainname> –Mode DnsTxtRecord` | Etki alanı (MS=xxxxx) için yeni DNS TXT kaydına koymak için bilgi sağlar. Doğrulama, TXT kaydının yayılması biraz zaman aldığından hemen gerçekleşmeyebilir, bu nedenle **-ForceTakeover** seçeneğini düşünmeden önce birkaç dakika bekleyin. 
`confirm-msoldomain –Domainname <domainname> –ForceTakeover Force` | <li>Alan adınız hala doğrulanmadıysa, **-ForceTakeover** seçeneğine devam edebilirsiniz. TXT kaydının oluşturulduğunu doğrular ve devralma işlemini başlatır.<li>**-ForceTakeover** seçeneği, yalnızca yönetilmeyen kiracının devralmayı engelleyen Office 365 hizmetlerine sahip olması gibi harici bir yönetici devralmayı zorlarken cmdlet'e eklenmelidir.
`get-msoldomain` | Etki alanı listesi artık etki alanı adını **Doğrulandı**olarak gösterir.

> [!NOTE]
> Yönetilmeyen Azure REKLAM kuruluşu, harici devralma gücü seçeneğini uyguladıktan 10 gün sonra silinir.

### <a name="powershell-example"></a>PowerShell örneği

1. Self servis teklifine yanıt vermek için kullanılan kimlik bilgilerini kullanarak Azure AD'ye bağlanın:
   ```powershell
   Install-Module -Name MSOnline
   $msolcred = get-credential
    
   connect-msolservice -credential $msolcred
   ```
2. Etki alanlarının listesini alın:
  
   ```powershell
   Get-MsolDomain
   ```
3. Bir meydan okuma oluşturmak için Get-MsolDomainVerificationDns cmdlet çalıştırın:
   ```powershell
   Get-MsolDomainVerificationDns –DomainName *your_domain_name* –Mode DnsTxtRecord
   ```
    Örnek:
   ```
   Get-MsolDomainVerificationDns –DomainName contoso.com –Mode DnsTxtRecord
   ```

4. Bu komuttan döndürülen değeri (meydan okumayı) kopyalayın. Örnek:
   ```powershell
   MS=32DD01B82C05D27151EA9AE93C5890787F0E65D9
   ```
5. Ortak DNS ad alanınızda, önceki adımda kopyaladığınız değeri içeren bir DNS txt kaydı oluşturun. Bu kaydın adı üst etki alanının adıdır, bu nedenle Windows Server'daki DNS rolünü kullanarak bu kaynak kaydını oluşturursanız, Kayıt adını boş bırakın ve değeri Metin kutusuna yapıştırın.
6. Zorluğu doğrulamak için Confirm-MsolDomain cmdlet'i çalıştırın:
  
   ```powershell
   Confirm-MsolDomain –DomainName *your_domain_name* –ForceTakeover Force
   ```
  
   Örnek:
  
   ```powershell
   Confirm-MsolDomain –DomainName contoso.com –ForceTakeover Force
   ```

Başarılı bir meydan okuma hatasız komut istemi ne döndürür.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD'ye özel bir etki alanı adı ekleme](../fundamentals/add-custom-domain.md)
* [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Cmdlet Başvurusu](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
