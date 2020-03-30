---
title: Azure Active Directory Etki Alanı Hizmetleri sorun giderme | Microsoft Dokümanlar'
description: Azure Etkin Dizin Etki Alanı Hizmetleri oluştururken veya yönetirken sık karşılaşılan hataları nasıl giderdiğinizi öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 4bc8c604-f57c-4f28-9dac-8b9164a0cf0b
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: iainfou
ms.openlocfilehash: e17112cbe2a494a585cd5a09c36cfe449d3d433c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79365824"
---
# <a name="common-errors-and-troubleshooting-steps-for-azure-active-directory-domain-services"></a>Azure Etkin Dizin Etki Alanı Hizmetleri için sık karşılaşılan hatalar ve sorun giderme adımları

Uygulamalar için kimlik ve kimlik doğrulamanın merkezi bir parçası olarak, Azure Active Directory Domain Services (Azure AD DS) bazen sorunlarla karşı laşMaktadır. Sorunlarla karşılaştıysanız, bazı yaygın hata iletileri ve bir şeylerin yeniden çalışmasını yardımcı olmak için ilişkili sorun giderme adımları vardır. İstediğiniz zaman, ek sorun giderme yardımı için [bir Azure destek isteği][azure-support] de açabilirsiniz.

Bu makalede, Azure AD DS'de sık karşılaşılan sorunlar için sorun giderme adımları sağlanmaktadır.

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Azure REKLAM dizininiz için Azure AD Etki Alanı Hizmetlerini etkinleştiremezsiniz

Azure AD DS'yi etkinleştirmede sorun yaşıyorsanız, bunları çözmek için aşağıdaki yaygın hataları ve adımları gözden geçirin:

| **Örnek hata Iletisi** | **Çözünürlük** |
| --- |:--- |
| *aaddscontoso.com adı bu ağda zaten kullanılıyor. Kullanılmayan bir ad belirtin.* |[Sanal ağda alan adı çakışması](troubleshoot.md#domain-name-conflict) |
| *Etki Alanı Hizmetleri bu Azure AD kiracısında etkinleştirilemedi. Hizmetin 'Azure AD Etki Alanı Hizmetleri Eşitlemesi' adlı uygulama için yeterli izinleri yoktur. 'Azure AD Etki Alanı Hizmetleri Eşitleme' adlı uygulamayı silin ve ardından Azure AD kiracınız için Etki Alanı Hizmetlerini etkinleştirmeye çalışın.* |[Etki Alanı Hizmetleri, Azure AD Etki Alanı Hizmetleri Eşitleme uygulaması için yeterli izine sahip değildir](troubleshoot.md#inadequate-permissions) |
| *Etki Alanı Hizmetleri bu Azure AD kiracısında etkinleştirilemedi. Azure AD kiracınızdaki Etki Alanı Hizmetleri uygulaması, Etki Alanı Hizmetlerini etkinleştirmek için gerekli izinlere sahip değildir. D87dcbc6-a371-462e-88e3-28ad15ec4e64 uygulama tanımlayıcısıyla uygulamayı silin ve ardından Azure AD kiracınız için Etki Alanı Hizmetlerini etkinleştirmeye çalışın.* |[Etki Alanı Hizmetleri uygulaması Azure AD kiracınızda düzgün şekilde yapılandırılmamıştır](troubleshoot.md#invalid-configuration) |
| *Etki Alanı Hizmetleri bu Azure AD kiracısında etkinleştirilemedi. Microsoft Azure AD uygulaması Azure AD kiracınızda devre dışı bırakılır. Uygulama tanımlayıcısı 00000000-0000-c000-0000000000000000 00000000 ile uygulamayı etkinleştirin ve ardından Azure AD kiracınız için Etki Alanı Hizmetlerini etkinleştirmeye çalışın.* |[Microsoft Graph uygulaması Azure AD kiracınızda devre dışı bırakılır](troubleshoot.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a>Etki Alanı Adı çakışması

**Hata iletisi**

*aaddscontoso.com adı bu ağda zaten kullanılıyor. Kullanılmayan bir ad belirtin.*

**Çözünürlük**

Aynı alan adı üzerinde aynı etki alanı adı veya eşlenmiş, sanal ağ bulunan varolan bir AD DS ortamına sahip olmadığınızı denetleyin. Örneğin, Azure VM'lerinde çalışan *aaddscontoso.com* adında bir AD DS etki alanınız olabilir. Sanal ağdaki *aaddscontoso.com* aynı etki alanı adı taşıyan bir Azure AD DS yönetilen etki alanını etkinleştirmeye çalıştığınızda, istenen işlem başarısız olur.

Bu hata, sanal ağdaki etki alanı adı için ad çakışmaları nedeniyledir. DNS araması, varolan bir AD DS ortamının istenen etki alanı adına yanıt verilip verilelmeyiş sayılsa denetler. Bu hatayı gidermek için, Azure AD DS yönetilen etki alanınızı ayarlamak için farklı bir ad kullanın veya varolan AD DS etki alanını sağlamayı kaldırın ve ardından Azure AD DS'yi etkinleştirmek için yeniden deneyin.

### <a name="inadequate-permissions"></a>Yetersiz izinler

**Hata iletisi**

*Etki Alanı Hizmetleri bu Azure AD kiracısında etkinleştirilemedi. Hizmetin 'Azure AD Etki Alanı Hizmetleri Eşitlemesi' adlı uygulama için yeterli izinleri yoktur. 'Azure AD Etki Alanı Hizmetleri Eşitleme' adlı uygulamayı silin ve ardından Azure AD kiracınız için Etki Alanı Hizmetlerini etkinleştirmeye çalışın.*

**Çözünürlük**

Azure REKLAM dizininizde *Azure AD Etki Alanı Hizmetleri Eşitlemi* adlı bir uygulama olup olmadığını denetleyin. Bu uygulama varsa, silin ve Azure AD DS'yi etkinleştirmek için yeniden deneyin. Varolan bir uygulamayı denetlemek ve gerekirse silmek için aşağıdaki adımları tamamlayın:

1. Azure portalında, sol daki gezinme menüsünden **Azure Active Directory'yi** seçin.
1. **Kurumsal uygulamaları**seçin. Uygulama Türü açılır menüsünden *tüm uygulamaları* seçin ve ardından **Uygula'yı**seçin. **Application Type**
1. Arama kutusuna *Azure AD Etki Alanı Hizmetleri Eşitle'yi*girin. Uygulama varsa, seçin ve **Sil'i**seçin.
1. Uygulamayı sildikten sonra Azure AD DS'yi yeniden etkinleştirmeye çalışın.

### <a name="invalid-configuration"></a>Geçersiz yapılandırma

**Hata iletisi**

*Etki Alanı Hizmetleri bu Azure AD kiracısında etkinleştirilemedi. Azure AD kiracınızdaki Etki Alanı Hizmetleri uygulaması, Etki Alanı Hizmetlerini etkinleştirmek için gerekli izinlere sahip değildir. D87dcbc6-a371-462e-88e3-28ad15ec4e64 uygulama tanımlayıcısıyla uygulamayı silin ve ardından Azure AD kiracınız için Etki Alanı Hizmetlerini etkinleştirmeye çalışın.*

**Çözünürlük**

Azure AD dizininizde *d87dcbc6-a371-462e-88e3-28ad15ec4e64* uygulama tanımlayıcısı bulunan *AzureActiveDirectoryDomainControllerServices* adında mevcut bir uygulamanız olup olmadığını kontrol edin. Bu uygulama varsa, silin ve Azure AD DS'yi etkinleştirmek için yeniden deneyin.

Varolan bir uygulama örneğini aramak ve gerekirse silmek için aşağıdaki PowerShell komut dosyasını kullanın:

```powershell
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```

### <a name="microsoft-graph-disabled"></a>Microsoft Graph devre dışı

**Hata iletisi**

*Etki Alanı Hizmetleri bu Azure AD kiracısında etkinleştirilemedi. Microsoft Azure AD uygulaması Azure AD kiracınızda devre dışı bırakılır. Uygulama tanımlayıcısı 00000000-0000-c000-0000000000000000 00000000 ile uygulamayı etkinleştirin ve ardından Azure AD kiracınız için Etki Alanı Hizmetlerini etkinleştirmeye çalışın.*

**Çözünürlük**

Tanımlayıcı *000000002-0000-0000-c000-0000000000000000000000*00000000000 00000000 0000000 0 0000000000 00000000 000000000 000000000000 00000000000000000000 000000000. Bu uygulama Microsoft Azure AD uygulamasıdır ve Azure AD kiracınıza Grafik API erişimi sağlar. Azure AD kiracınızı eşitlemek için bu uygulamanın etkinleştirilmesi gerekir.

Bu uygulamanın durumunu denetlemek ve gerekirse etkinleştirmek için aşağıdaki adımları tamamlayın:

1. Azure portalında, sol daki gezinme menüsünden **Azure Active Directory'yi** seçin.
1. **Kurumsal uygulamaları**seçin. Uygulama Türü açılır menüsünden *tüm uygulamaları* seçin ve ardından **Uygula'yı**seçin. **Application Type**
1. Arama kutusuna *00000002-0000-0000-c000-00000000000000000000000000000.* Uygulamayı seçin, ardından **Özellikler'i**seçin.
1. **Kullanıcıların oturum açmaları için Etkinleştirilen** ler *Hayır*olarak ayarlanmışsa, değeri *Evet*olarak ayarlayın, ardından **Kaydet'i**seçin.
1. Uygulamayı etkinleştirdikten sonra Azure AD DS'yi yeniden etkinleştirmeye çalışın.

## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Kullanıcılar Azure AD Domain Services yönetilen etki alanında oturum açamıyor

Azure AD kiracınızdaki bir veya daha fazla kullanıcı Azure AD DS yönetilen etki alanında oturum açamazsa, aşağıdaki sorun giderme adımlarını tamamlayın:

* **Kimlik bilgileri biçimi** - Upn biçimini kullanarak `dee@aaddscontoso.onmicrosoft.com`kimlik bilgilerini belirtmeyi deneyin. UPN biçimi, Azure AD DS'de kimlik bilgilerini belirtmenin önerilen yoludur. Bu UPN'nin Azure AD'de doğru şekilde yapılandırıldığından emin olun.

    *AADDSCONTOSO\driley* gibi hesabınıziçin *SAMAccountName,* kiracınızda aynı UPN önekine sahip birden fazla kullanıcı varsa veya UPN önekiniz aşırı uzunsa otomatik olarak oluşturulabilir. Bu nedenle, hesabınızdaki *SAMAccountName* biçimi, şirket içi etki alanınızda beklediğiniz veya kullandığınızdan farklı olabilir.

* **Parola eşitleme** - Azure AD Connect'i kullanarak [yalnızca bulut kullanıcıları][cloud-only-passwords] veya [karma ortamlar][hybrid-phs]için parola eşitlemesini etkinleştirdiğinizden emin olun.
    * **Karma senkronize hesaplar:** Etkilenen kullanıcı hesapları şirket içi dizinden eşitlenirse, aşağıdaki alanları doğrulayın:
    
      * [Azure AD Connect'in en son önerilen sürümüne](https://www.microsoft.com/download/details.aspx?id=47594)dağıttınız veya güncellediniz.
      * Azure AD Connect'i [tam bir eşitleme gerçekleştirecek][hybrid-phs]şekilde yapılandırıldınız.
      * Dizininizin boyutuna bağlı olarak, kullanıcı hesaplarının ve kimlik bilgisi açıklarının Azure AD DS'de kullanılabilir olması biraz zaman alabilir. Yönetilen etki alanına karşı kimlik doğrulamayı denemeden önce yeterince beklemeyi unutmayın.
      * Önceki adımları doğruladıktan sonra sorun devam ederse, *Microsoft Azure AD Eşitleme Hizmeti'ni*yeniden başlatmayı deneyin. Azure AD Connect sunucunuzdan bir komut istemi açın ve aşağıdaki komutları çalıştırın:
    
        ```console
        net stop 'Microsoft Azure AD Sync'
        net start 'Microsoft Azure AD Sync'
        ```

    * **Yalnızca bulut hesapları**: Etkilenen kullanıcı hesabı yalnızca buluta özel bir kullanıcı hesabıysa, [Azure AD DS'yi etkinleştirdikten sonra kullanıcının parolasını değiştirdiğinden][cloud-only-passwords]emin olun. Bu parola sıfırlama, Azure AD Etki Alanı Hizmetleri için gerekli kimlik bilgisi işbirlerinin oluşturulmasına neden olur.

* **Kullanıcı hesabının etkin olduğunu doğrulayın**: Varsayılan olarak, yönetilen etki alanında 2 dakika içinde beş geçersiz parola denemesi, bir kullanıcı hesabının 30 dakika boyunca kilitlenmiş olmasınedeniyle. Hesap kilitlenirken kullanıcı oturum açamıyor. 30 dakika sonra, kullanıcı hesabı otomatik olarak açılır.
  * Azure AD DS yönetilen etki alanında geçersiz parola denemeleri, Azure AD'deki kullanıcı hesabını kilitlemez. Kullanıcı hesabı yalnızca yönetilen etki alanı içinde kilitlenir. Azure AD'de değil, [VM yönetimini][management-vm]kullanarak *Active Directory Administrative Console'daki (ADAC)* kullanıcı hesabı durumunu denetleyin.
  * Ayrıca, varsayılan kilitleme eşiğini ve süresini değiştirmek için [ince taneli parola ilkelerini yapılandırabilirsiniz.][password-policy]

* **Dış hesaplar** - Etkilenen kullanıcı hesabının Azure AD kiracısında harici bir hesap olmadığından kontrol edin. Dış hesaplara örnek olarak, harici bir Azure AD dizinindeki Microsoft hesapları `dee@live.com` veya kullanıcı hesapları verilebilir. Azure AD DS, yönetilen etki alanında oturum açamamak için harici kullanıcı hesapları için kimlik bilgileri depolamaz.

## <a name="there-are-one-or-more-alerts-on-your-managed-domain"></a>Yönetilen etki alanınızda bir veya daha fazla uyarı var

Azure AD DS yönetilen etki alanında etkin uyarılar varsa, kimlik doğrulama işleminin doğru çalışmasını engelleyebilir.

Etkin uyarılar olup olmadığını görmek [için, Azure AD DS yönetilen etki alanının sistem durumu durumunu kontrol edin.][check-health] Herhangi bir uyarı gösterilirse, [sorun giderme ve bunları çöz.][troubleshoot-alerts]

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Azure AD kiracınızdan kaldırılan kullanıcılar yönetilen etki alanınızdan kaldırılmaz

Azure AD, kullanıcı nesnelerinin yanlışlıkla silinmesine karşı koruma sağlar. Bir Azure AD kiracısından bir kullanıcı hesabını sildiğinizde, ilgili kullanıcı nesnesi geri dönüşüm kutusuna taşınır. Bu silme işlemi Azure AD DS yönetilen etki alanınızla eşitlendiğinde, ilgili kullanıcı hesabı devre dışı bırakılmış olarak işaretlenir. Bu özellik, kullanıcı hesabını kurtarmanıza veya silmenize yardımcı olur.

Kullanıcı hesabı, Azure AD dizininde aynı UPN'ye sahip bir kullanıcı hesabını yeniden oluştursanız bile, Azure AD DS yönetilen etki alanında devre dışı bırakılmış durumda kalır. Kullanıcı hesabını Azure AD DS yönetilen etki alanından kaldırmak için, hesabı Azure AD kiracısından zorla silmeniz gerekir.

Bir kullanıcı hesabını Azure AD DS yönetilen etki alanından tamamen kaldırmak için, `-RemoveFromRecycleBin` parametreli [Remove-MsolUser][Remove-MsolUser] PowerShell cmdlet'i kullanarak kullanıcıyı Azure AD kiracınızdan kalıcı olarak silin.

## <a name="next-steps"></a>Sonraki adımlar

Sorun yaşamaya devam ederseniz, ek sorun giderme yardımı için [bir Azure destek isteği açın.][azure-support]

<!-- INTERNAL LINKS -->
[cloud-only-passwords]: tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds
[hybrid-phs]: tutorial-configure-password-hash-sync.md
[management-vm]: tutorial-create-management-vm.md
[password-policy]: password-policy.md
[check-health]: check-health.md
[troubleshoot-alerts]: troubleshoot-alerts.md
[Remove-MsolUser]: /powershell/module/MSOnline/Remove-MsolUser
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
