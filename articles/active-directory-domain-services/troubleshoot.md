---
title: Azure Active Directory Domain Services sorunlarını giderme | Microsoft Docs '
description: Azure Active Directory Domain Services oluştururken veya yönetirken sık karşılaşılan hataların nasıl giderileceği hakkında bilgi edinin
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
ms.openlocfilehash: c4b2dea05b459d6e9ae4eb086fa127d88a84a768
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78249203"
---
# <a name="common-errors-and-troubleshooting-steps-for-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services için sık karşılaşılan hatalar ve sorun giderme adımları

Azure Active Directory Domain Services (Azure AD DS) uygulamalar için kimlik ve kimlik doğrulamanın merkezi bir parçası olarak bazı sorunlar oluşur. Sorunlarla karşılaşırsanız, yeniden çalışan işlemleri almanıza yardımcı olmak için bazı yaygın hata iletileri ve ilgili sorun giderme adımları vardır. İstediğiniz zaman, ek sorun giderme yardımı için [bir Azure destek isteği de açabilirsiniz][azure-support] .

Bu makalede, Azure AD DS 'daki yaygın sorunlar için sorun giderme adımları sunulmaktadır.

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Azure AD dizininiz için Azure AD Domain Services etkinleştiremezsiniz

Azure AD DS 'yi etkinleştirme sorunlarıyla karşılaşırsanız, bu sorunları çözmek için aşağıdaki yaygın hataları ve adımları gözden geçirin:

| **Örnek hata Iletisi** | **Çözünürlük** |
| --- |:--- |
| *Addscontoso.com adı bu ağda zaten kullanılıyor. Kullanımda olmayan bir ad belirtin.* |[Sanal ağda etki alanı adı çakışması](troubleshoot.md#domain-name-conflict) |
| *Bu Azure AD kiracısında etki alanı Hizmetleri etkinleştirilemedi. Hizmet ' Azure AD Domain Services Sync ' adlı uygulama için yeterli izinlere sahip değil. ' Azure AD Domain Services Sync ' adlı uygulamayı silin ve ardından Azure AD kiracınız için etki alanı hizmetlerini etkinleştirmeyi deneyin.* |[Etki alanı Hizmetleri Azure AD Domain Services eşitleme uygulaması için yeterli izinlere sahip değil](troubleshoot.md#inadequate-permissions) |
| *Bu Azure AD kiracısında etki alanı Hizmetleri etkinleştirilemedi. Azure AD kiracınızdaki etki alanı Hizmetleri uygulaması, etki alanı hizmetlerini etkinleştirmek için gerekli izinlere sahip değil. Uygulamayı d87dcbc6-a371-462e-88e3-28ad15ec4e64 uygulama tanımlayıcısı ile silin ve ardından Azure AD kiracınız için etki alanı hizmetlerini etkinleştirmeyi deneyin.* |[Etki alanı Hizmetleri uygulaması, Azure AD kiracınızda düzgün yapılandırılmamış](troubleshoot.md#invalid-configuration) |
| *Bu Azure AD kiracısında etki alanı Hizmetleri etkinleştirilemedi. Microsoft Azure AD uygulama Azure AD kiracınızda devre dışı bırakıldı. Uygulamayı 00000002-0000-0000-C000-000000000000 uygulama tanımlayıcısıyla etkinleştirin ve sonra Azure AD kiracınız için etki alanı hizmetlerini etkinleştirmeyi deneyin.* |[Microsoft Graph uygulama Azure AD kiracınızda devre dışı bırakıldı](troubleshoot.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a>Etki alanı adı çakışması

**Hata iletisi**

*Aaddscontoso.com adı bu ağda zaten kullanılıyor. Kullanımda olmayan bir ad belirtin.*

**Çözünürlük**

Aynı etki alanı adına sahip mevcut bir AD DS ortamınız veya eşlenmiş bir sanal ağ olup olmadığına bakın. Örneğin, Azure VM 'lerde çalışan *aaddscontoso.com* adlı bir AD DS etki alanınız olabilir. Sanal ağdaki *aaddscontoso.com* aynı etki alanı adıyla bir Azure AD DS yönetilen etki alanını etkinleştirmeye çalıştığınızda, istenen işlem başarısız olur.

Bu hata, sanal ağ üzerindeki etki alanı adı için ad çakışmalarının kaynaklanmaktadır. DNS araması, mevcut bir AD DS ortamının istenen etki alanı adında yanıt verip vermediğini denetler. Bu hatayı çözmek için, Azure AD DS yönetilen etki alanınızı ayarlamak için farklı bir ad kullanın veya var olan AD DS etki alanının sağlamasını kaldırın ve ardından Azure AD DS etkinleştirmeyi yeniden deneyin.

### <a name="inadequate-permissions"></a>Yetersiz izinler

**Hata iletisi**

*Bu Azure AD kiracısında etki alanı Hizmetleri etkinleştirilemedi. Hizmet ' Azure AD Domain Services Sync ' adlı uygulama için yeterli izinlere sahip değil. ' Azure AD Domain Services Sync ' adlı uygulamayı silin ve ardından Azure AD kiracınız için etki alanı hizmetlerini etkinleştirmeyi deneyin.*

**Çözünürlük**

Azure AD dizininizde *Azure AD Domain Services eşitleme* adlı bir uygulama olup olmadığını denetleyin. Bu uygulama varsa, silin ve sonra Azure AD DS 'yi etkinleştirmeyi yeniden deneyin. Mevcut bir uygulamayı denetlemek ve gerekirse silmek için aşağıdaki adımları izleyin:

1. Azure portal sol taraftaki gezinti menüsünden **Azure Active Directory** ' i seçin.
1. **Kurumsal uygulamalar**' ı seçin. **Uygulama türü** açılan menüsünden *tüm uygulamalar* ' ı seçin ve ardından **Uygula**' yı seçin.
1. Arama kutusuna *Azure AD Domain Services eşitleme*yazın. Uygulama varsa, seçin ve **Sil**' i seçin.
1. Uygulamayı sildikten sonra Azure AD DS 'yi yeniden etkinleştirmeyi deneyin.

### <a name="invalid-configuration"></a>Geçersiz yapılandırma

**Hata iletisi**

*Bu Azure AD kiracısında etki alanı Hizmetleri etkinleştirilemedi. Azure AD kiracınızdaki etki alanı Hizmetleri uygulaması, etki alanı hizmetlerini etkinleştirmek için gerekli izinlere sahip değil. Uygulamayı d87dcbc6-a371-462e-88e3-28ad15ec4e64 uygulama tanımlayıcısı ile silin ve ardından Azure AD kiracınız için etki alanı hizmetlerini etkinleştirmeyi deneyin.*

**Çözünürlük**

Azure AD dizininizde *d87dcbc6-a371-462e-88e3-28ad15ec4e64* uygulama tanımlayıcısına sahip *AzureActiveDirectoryDomainControllerServices* adlı mevcut bir uygulamanız olup olmadığını denetleyin. Bu uygulama varsa, silin ve sonra Azure AD DS 'yi etkinleştirmeyi yeniden deneyin.

Mevcut bir uygulama örneğini aramak ve gerekirse silmek için aşağıdaki PowerShell betiğini kullanın:

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

*Bu Azure AD kiracısında etki alanı Hizmetleri etkinleştirilemedi. Microsoft Azure AD uygulama Azure AD kiracınızda devre dışı bırakıldı. Uygulamayı 00000002-0000-0000-C000-000000000000 uygulama tanımlayıcısıyla etkinleştirin ve sonra Azure AD kiracınız için etki alanı hizmetlerini etkinleştirmeyi deneyin.*

**Çözünürlük**

*00000002-0000-0000-C000-000000000000*tanımlayıcısına sahip bir uygulamayı devre dışı bırakılıp bırakılmadığını kontrol edin. Bu uygulama Microsoft Azure AD uygulamasıdır ve Azure AD kiracınıza Graph API erişim sağlar. Azure AD kiracınızı eşitlemeniz için bu uygulamanın etkinleştirilmesi gerekir.

Bu uygulamanın durumunu denetlemek ve gerekirse etkinleştirmek için aşağıdaki adımları izleyin:

1. Azure portal sol taraftaki gezinti menüsünden **Azure Active Directory** ' i seçin.
1. **Kurumsal uygulamalar**' ı seçin. **Uygulama türü** açılan menüsünden *tüm uygulamalar* ' ı seçin ve ardından **Uygula**' yı seçin.
1. Arama kutusuna *00000002-0000-0000-C000-00000000000*yazın. Uygulamayı seçin ve ardından **Özellikler**' i seçin.
1. **Kullanıcıların oturum açması Için etkinleştirilirse** , *Hayır*olarak ayarlanırsa değeri *Evet*olarak ayarlayın, sonra **Kaydet**' i seçin.
1. Uygulamayı etkinleştirdikten sonra Azure AD DS yeniden etkinleştirmeyi deneyin.

## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Kullanıcılar Azure AD Domain Services yönetilen etki alanında oturum açamıyor

Azure AD kiracınızdaki bir veya daha fazla Kullanıcı Azure AD DS yönetilen etki alanında oturum açamaz, aşağıdaki sorun giderme adımlarını izleyin:

* **Kimlik bilgileri biçimi** -`dee@aaddscontoso.onmicrosoft.com`gibi kimlik bilgilerini BELIRTMEK için UPN biçimini kullanmayı deneyin. UPN biçimi, Azure AD DS kimlik bilgilerini belirtmenin önerilen yoludur. Azure AD 'de bu UPN 'nin doğru yapılandırıldığından emin olun.

    Kiracınızda aynı UPN ön ekine sahip birden fazla kullanıcı varsa veya UPN önekiniz aşırı uzunsa, hesabınız için *sAMAccountName* *öğesi (Aaddscontoso\driley* gibi) otomatik olarak oluşturulabilir. Bu nedenle, hesabınız için *sAMAccountName* biçimi, şirket içi etki alanında beklediğiniz veya kullandığınız verilerden farklı olabilir.

* **Parola eşitleme** - [yalnızca bulutta bulunan kullanıcılar][cloud-only-passwords] veya [Azure AD Connect kullanarak karma ortamlar][hybrid-phs]için parola eşitlemesini etkinleştirdiğinizden emin olun.
    * **Karma eşitlenmiş hesaplar:** Etkilenen Kullanıcı hesapları şirket içi bir dizinden eşitleniyorsa aşağıdaki alanların doğrulanması gerekir:
    
      * [En son önerilen Azure AD Connect sürümünü](https://www.microsoft.com/download/details.aspx?id=47594)dağıttıysanız veya sürümüne güncelleştirdiniz.
      * [Tam eşitleme gerçekleştirmek][hybrid-phs]için Azure AD Connect yapılandırdınız.
      * Dizininizin boyutuna bağlı olarak, Kullanıcı hesapları ve kimlik bilgisi karmalarının Azure AD DS 'de kullanılabilir olması biraz zaman alabilir. Yönetilen etki alanında kimlik doğrulaması yapmayı denemeden önce yeterince uzun süre beklemeniz gerekir.
      * Önceki adımları doğruladıktan sonra sorun devam ederse, *Microsoft Azure AD eşitleme hizmetini*yeniden başlatmayı deneyin. Azure AD Connect sunucunuzda bir komut istemi açın ve aşağıdaki komutları çalıştırın:
    
        ```console
        net stop 'Microsoft Azure AD Sync'
        net start 'Microsoft Azure AD Sync'
        ```

    * **Yalnızca bulut hesapları**: etkilenen Kullanıcı hesabı yalnızca bulutta yer alan bir kullanıcı hesabı Ise, [Azure AD DS etkinleştirildikten sonra kullanıcının parolasını değiştirdiğine][cloud-only-passwords]emin olun. Bu parola sıfırlama Azure AD Domain Services için gereken kimlik bilgisi karmalarının oluşturulmasına neden olur.

* **Kullanıcı hesabının etkin olduğunu doğrulayın**: varsayılan olarak, yönetilen etki alanında 2 dakika içinde beş geçersiz parola denemesi, bir kullanıcı hesabının 30 dakika boyunca kilitlenmesine neden olur. Hesap kilitliyken Kullanıcı oturum açamaz. 30 dakika sonra, Kullanıcı hesabı otomatik olarak kilidi açılır.
  * Azure AD DS yönetilen etki alanında geçersiz parola girişimleri, Azure AD 'de Kullanıcı hesabını kilitlemez. Kullanıcı hesabı yalnızca yönetilen etki alanı içinde kilitlenir. Azure AD 'de değil, [YÖNETIM sanal][management-vm]makinesini kullanarak *Active Directory Yönetim Konsolu 'nda (ADAC)* Kullanıcı hesabı durumunu kontrol edin.
  * Ayrıca, varsayılan kilitleme eşiğini ve süresini değiştirmek için [hassas parola ilkeleri yapılandırabilirsiniz][password-policy] .

* **Dış hesaplar** -etkilenen Kullanıcı HESABıNıN Azure AD kiracısında bir dış hesap olmadığını kontrol edin. Dış hesap örnekleri, dış bir Azure AD dizininden `dee@live.com` veya Kullanıcı hesapları gibi Microsoft hesaplarını içerir. Azure AD DS, dış Kullanıcı hesapları için kimlik bilgilerini depolamaz, bu nedenle yönetilen etki alanında oturum açamaz.

## <a name="there-are-one-or-more-alerts-on-your-managed-domain"></a>Yönetilen etki alanında bir veya daha fazla uyarı var

Azure AD DS yönetilen etki alanında etkin uyarılar varsa, kimlik doğrulama işleminin düzgün çalışmasını engelleyebilir.

Etkin bir uyarı olup olmadığını görmek için [Azure AD DS yönetilen bir etki alanının sistem durumunu kontrol][check-health]edin. Herhangi bir uyarı gösteriliyorsa, [sorunlarını giderin ve çözümleyin][troubleshoot-alerts].

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Azure AD kiracınızdan kaldırılan kullanıcılar yönetilen etki alanından kaldırılmaz

Azure AD, Kullanıcı nesnelerinin yanlışlıkla silinmesine karşı koruma sağlar. Bir Azure AD kiracısından bir kullanıcı hesabı sildiğinizde, karşılık gelen Kullanıcı nesnesi geri dönüşüm kutusu 'na taşınır. Bu silme işlemi Azure AD DS yönetilen etki alanınız ile eşitlendiğinde, karşılık gelen Kullanıcı hesabı devre dışı olarak işaretlenir. Bu özellik kullanıcı hesabını kurtarmanıza veya geri almanıza yardımcı olur.

Azure AD dizininde aynı UPN ile bir kullanıcı hesabını yeniden oluştursanız bile Kullanıcı hesabı, Azure AD DS yönetilen etki alanındaki devre dışı durumda kalır. Kullanıcı hesabını Azure AD DS yönetilen etki alanından kaldırmak için Azure AD kiracısından zorla silmeniz gerekir.

Bir kullanıcı hesabını Azure AD DS yönetilen bir etki alanından tamamen kaldırmak için, [Remove-MsolUser][Remove-MsolUser] PowerShell cmdlet 'ini `-RemoveFromRecycleBin` parametresiyle kullanarak Azure AD kiracınızdan kullanıcıyı kalıcı olarak silin.

## <a name="next-steps"></a>Sonraki adımlar

Sorun yaşamaya devam ederseniz, ek sorun giderme yardımı için [bir Azure destek isteği açın][azure-support] .

<!-- INTERNAL LINKS -->
[cloud-only-passwords]: tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds
[hybrid-phs]: tutorial-configure-password-hash-sync.md
[management-vm]: tutorial-create-management-vm.md
[password-policy]: password-policy.md
[check-health]: check-health.md
[troubleshoot-alerts]: troubleshoot-alerts.md
[Remove-MsolUser]: /powershell/module/MSOnline/Remove-MsolUser
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
