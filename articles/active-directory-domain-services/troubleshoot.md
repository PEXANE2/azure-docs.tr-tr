---
title: 'Azure Active Directory Domain Services: Sorun giderme kılavuzu | Microsoft Docs'
description: Azure AD Domain Services için sorun giderme kılavuzu
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 4bc8c604-f57c-4f28-9dac-8b9164a0cf0b
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: iainfou
ms.openlocfilehash: acb001417b85b8ff45b2617e148e8b1961f3cbfa
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2019
ms.locfileid: "68772984"
---
# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure AD Domain Services-sorun giderme kılavuzu
Bu makalede, Azure Active Directory (AD) etki alanı hizmetlerini ayarlarken veya yönetirken karşılaşabileceğiniz sorunlar için sorun giderme ipuçları sunulmaktadır.

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Azure AD dizininiz için Azure AD Domain Services etkinleştiremezsiniz
Bu bölüm, dizininiz için Azure AD Domain Services etkinleştirmeye çalıştığınızda hata gidermenize yardımcı olur.

Karşılaştığınız hata iletisine karşılık gelen sorun giderme adımlarını seçin.

| **Hata Iletisi** | **Çözümleme** |
| --- |:--- |
| *Contoso100.com adı bu ağda zaten kullanımda. Kullanımda olmayan bir ad belirtin.* |[Sanal ağda etki alanı adı çakışması](troubleshoot.md#domain-name-conflict) |
| *Domain Services bu Azure AD kiracısında etkinleştirilemedi. Hizmetin, 'Azure AD Domain Services Sync' adlı uygulama üzerinde yeterli izinleri yok. 'Azure AD Domain Services Sync' adlı uygulamayı silin ve ardından Azure AD kiracınız için Domain Services’ı etkinleştirmeyi deneyin.* |[Etki alanı Hizmetleri Azure AD Domain Services eşitleme uygulaması için yeterli izinlere sahip değil](troubleshoot.md#inadequate-permissions) |
| *Domain Services bu Azure AD kiracısında etkinleştirilemedi. Azure AD kiracınızdaki Domain Services uygulamasının, Etki Alanı Hizmetlerini etkinleştirmek için gereken izinleri yok. Uygulama tanımlayıcısı d87dcbc6-a371-462e-88e3-28ad15ec4e64 olan uygulamayı silin ve Azure AD kiracınızda Domain Services’ı etkinleştirmeyi deneyin.* |[Etki alanı Hizmetleri uygulaması kiracınızda düzgün yapılandırılmamış](troubleshoot.md#invalid-configuration) |
| *Domain Services bu Azure AD kiracısında etkinleştirilemedi. Azure AD kiracınızda Microsoft Azure AD uygulaması devre dışı bırakıldı. Uygulama tanımlayıcısı 00000002-0000-0000-c000-000000000000 olan uygulamayı etkinleştirin ve Azure AD kiracınızda Domain Services’ı etkinleştirmeyi deneyin.* |[Microsoft Graph uygulama Azure AD kiracınızda devre dışı bırakıldı](troubleshoot.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a>Etki alanı adı çakışması
**Hata iletisi:**

*Contoso100.com adı bu ağda zaten kullanımda. Kullanımda olmayan bir ad belirtin.*

**Düzeltmesi**

Bu sanal ağda aynı etki alanı adına sahip mevcut bir etki alanınız olmadığından emin olun. Örneğin, seçilen sanal ağ üzerinde zaten "contoso.com" adında bir etki alanınız olduğunu varsayın. Daha sonra, bu sanal ağ üzerinde aynı etki alanı adına (' contoso.com ') sahip Azure AD Domain Services yönetilen bir etki alanını etkinleştirmeye çalışırsınız. Azure AD Domain Services etkinleştirmeye çalışırken bir hata ile karşılaşırsınız.

Bu hata, bu sanal ağdaki etki alanı adı için ad çakışmalarından kaynaklanır. Bu durumda, Azure AD Etki Alanı Hizmetleri tarafından yönetilen etki alanınızı ayarlamak için farklı bir ad kullanmanız gerekir. Alternatif olarak, var olan etki alanının sağlanmasını kaldırıp Azure AD Etki Alanı Hizmetleri'ni etkinleştirme işlemiyle devam edebilirsiniz.

### <a name="inadequate-permissions"></a>Yetersiz izinler
**Hata iletisi:**

*Domain Services bu Azure AD kiracısında etkinleştirilemedi. Hizmetin, 'Azure AD Domain Services Sync' adlı uygulama üzerinde yeterli izinleri yok. 'Azure AD Domain Services Sync' adlı uygulamayı silin ve ardından Azure AD kiracınız için Domain Services’ı etkinleştirmeyi deneyin.*

**Düzeltmesi**

Azure AD dizininizde ' Azure AD Domain Services Sync ' adlı bir uygulama olup olmadığını denetleyin. Bu uygulama varsa, silin ve Azure AD Domain Services yeniden etkinleştirin.

Uygulamanın varolup olmadığını denetlemek ve uygulamayı silmek için aşağıdaki adımları gerçekleştirin:

1. [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/)Azure AD dizininizin **uygulamalar** bölümüne gidin.
2. **Göster** açılan menüsünde **tüm uygulamalar** ' ı seçin. **Uygulamalar durum** açılan menüsünde **herhangi birini** seçin. **Uygulama görünürlüğü** açılan menüsünde **herhangi birini** seçin.
3. Arama kutusuna **Azure AD Domain Services eşitleme** yazın. Uygulama varsa, üzerine tıklayın ve silmek için araç çubuğundaki **Sil** düğmesine tıklayın.
4. Uygulamayı sildikten sonra Azure AD Domain Services yeniden etkinleştirmeyi deneyin.

### <a name="invalid-configuration"></a>Geçersiz yapılandırma
**Hata iletisi:**

*Domain Services bu Azure AD kiracısında etkinleştirilemedi. Azure AD kiracınızdaki Domain Services uygulamasının, Etki Alanı Hizmetlerini etkinleştirmek için gereken izinleri yok. Uygulama tanımlayıcısı d87dcbc6-a371-462e-88e3-28ad15ec4e64 olan uygulamayı silin ve Azure AD kiracınızda Domain Services’ı etkinleştirmeyi deneyin.*

**Düzeltmesi**

Azure AD dizininizde adı ' AzureActiveDirectoryDomainControllerServices ' olan (uygulama tanımlayıcısı d87dcbc6-a371-462e-88e3-28ad15ec4e64 olan) bir uygulama olup olmadığını denetleyin. Bu uygulama varsa, bunu silip Azure AD Domain Services yeniden etkinleştirmeniz gerekir.

Uygulamayı bulmak ve silmek için aşağıdaki PowerShell betiğini kullanın.

> [!NOTE]
> Bu betik, **Azure AD PowerShell sürüm 2** cmdlet 'lerini kullanır. Tüm kullanılabilir cmdlet 'lerin tam listesi için ve modülünü indirmek için [Azuread PowerShell başvuru belgelerini](https://msdn.microsoft.com/library/azure/mt757189.aspx)okuyun.
>
>

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
<br>

### <a name="microsoft-graph-disabled"></a>Microsoft Graph devre dışı
**Hata iletisi:**

Bu Azure AD kiracısında etki alanı Hizmetleri etkinleştirilemedi. Azure AD kiracınızda Microsoft Azure AD uygulaması devre dışı bırakıldı. Uygulamayı 00000002-0000-0000-C000-000000000000 uygulama tanımlayıcısıyla etkinleştirin ve sonra Azure AD kiracınız için etki alanı hizmetlerini etkinleştirmeyi deneyin.

**Düzeltmesi**

00000002-0000-0000-C000-000000000000 tanımlayıcısına sahip bir uygulamayı devre dışı bırakılıp bırakılmadığını kontrol edin. Bu uygulama Microsoft Azure AD uygulamasıdır ve Azure AD kiracınıza Graph API erişim sağlar. Azure AD Domain Services, Azure AD kiracınızı yönetilen etki alanınız ile eşitleyecek şekilde bu uygulamanın etkinleştirilmesini gerektirir.

Bu hatayı çözmek için bu uygulamayı etkinleştirin ve sonra Azure AD kiracınız için etki alanı hizmetlerini etkinleştirmeyi deneyin.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Kullanıcılar Azure AD Domain Services yönetilen etki alanında oturum açamıyor
Azure AD kiracınızdaki bir veya daha fazla Kullanıcı yeni oluşturulan yönetilen etki alanında oturum açamıyor ise, aşağıdaki sorun giderme adımlarını gerçekleştirin:

* **UPN biçimini kullanarak oturum açın:** SAMAccountName biçimi ('CONTOSO\joeuser') yerine UPN biçimini kullanarak oturum açmayı deneyin (örneğin, 'joeuser@contoso.com'). SAMAccountName, UPN ön eki aşırı uzun olan veya yönetilen etki alanındaki başka bir kullanıcıyla aynı olan kullanıcılar için otomatik olarak oluşturulabilir. UPN biçiminin bir Azure AD kiracısı içinde benzersiz olması garanti edilir.

> [!NOTE]
> Azure AD Domain Services yönetilen etki alanında oturum açmak için UPN biçimini kullanmanızı öneririz.
>
>

* Başlarken kılavuzunda açıklanan adımlara uygun olarak [parola eşitlemesini etkinleştirdiğinizden](active-directory-ds-getting-started-password-sync.md) emin olun.
* **Dış hesaplar:** Etkilenen kullanıcı hesabının, Azure AD kiracısında bir dış hesap olmadığından emin olun. Dış hesap örnekleri, Microsoft hesapları (örneğin, 'joe@live.com') veya dış bir Azure AD dizininden Kullanıcı hesapları içerir. Azure AD Domain Services bu kullanıcı hesapları için kimlik bilgileri olmadığından, bu kullanıcılar yönetilen etki alanında oturum açabilirler.
* **Eşitlenmiş hesaplar:** Etkilenen Kullanıcı hesapları şirket içi dizinden eşitleniyorsa şunları doğrulayın:

  * [En son önerilen Azure AD Connect sürümüne](https://www.microsoft.com/download/details.aspx?id=47594)dağıttıysanız veya güncellenmiştir.
  * [Tam eşitleme gerçekleştirmek](active-directory-ds-getting-started-password-sync.md)için Azure AD Connect yapılandırdınız.
  * Dizininizin boyutuna bağlı olarak, Kullanıcı hesapları ve kimlik bilgisi karmalarının Azure AD Domain Services ' de kullanılabilir olması biraz zaman alabilir. Kimlik doğrulamasını yeniden denemeden önce yeterince uzun süre beklemediğinizden emin olun.
  * Yukarıdaki adımları doğruladıktan sonra sorun devam ederse, Microsoft Azure AD eşitleme hizmetini yeniden başlatmayı deneyin. Eşitleme makinenizden bir komut istemi başlatın ve aşağıdaki komutları yürütün:

    1. net stop ' Microsoft Azure AD Sync '
    2. net start ' Microsoft Azure AD Sync '
* **Yalnızca bulut hesapları**: Etkilenen Kullanıcı hesabı yalnızca bulutta yer alan bir kullanıcı hesabı ise, Azure AD Domain Services etkinleştirildikten sonra kullanıcının parolasını değiştirdiğinden emin olun. Bu adım, Azure AD Domain Services için gereken kimlik bilgisi karmalarının oluşturulmasına neden olur.
* **Kullanıcı hesabının etkin olduğunu doğrulayın**: Bir kullanıcının hesabı kilitliyse, hesabı yeniden etkin olana kadar oturum açamaz. Yönetilen etki alanında 2 dakika içinde 5 geçersiz parola denemesi, bir kullanıcı hesabının 30 dakika boyunca kilitlenmesine neden olur. 30 dakika sonra, Kullanıcı hesabı otomatik olarak kilidi açılır.
  * Yönetilen etki alanında geçersiz parola denemeleri, Azure AD 'de Kullanıcı hesabını kilitlemez. Kullanıcı hesabı yalnızca Azure AD Domain Services yönetilen etki alanı içinde kilitlenir. Azure AD 'de değil, Azure AD DS yönetilen etki alanı için Active Directory Yönetim Konsolu 'nu (ADAC) kullanarak Kullanıcı hesabı durumunu kontrol edin.
  * Ayrıca [, varsayılan kilitleme eşiğini ve süresini değiştiren hassas parola ilkeleri de yapılandırabilirsiniz](https://docs.microsoft.com/azure/active-directory-domain-services/password-policy).

## <a name="there-are-one-or-more-alerts-on-your-managed-domain"></a>Yönetilen etki alanında bir veya daha fazla uyarı var

[Sorun giderme uyarıları](troubleshoot-alerts.md) makalesini ziyaret ederek, yönetilen etki alanındaki uyarıları nasıl giderecağınızı öğrenin.

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Azure AD kiracınızdan kaldırılan kullanıcılar yönetilen etki alanından kaldırılmaz
Azure AD kullanıcı nesnelerinin yanlışlıkla silinmesine karşı sizi korur. Azure AD kiracınızdan bir kullanıcı hesabı sildiğinizde, buna karşılık gelen kullanıcı nesnesi Geri Dönüşüm Kutusu’na taşınır. Bu silme işlemi yönetilen etki alanınız ile eşitlendiğinde, ilgili Kullanıcı hesabının devre dışı olarak işaretlenmesine neden olur. Bu özellik, daha sonra Kullanıcı hesabını kurtarmanıza veya geri almanıza yardımcı olur.

Azure AD dizininizde aynı UPN ile bir kullanıcı hesabını yeniden oluştursanız bile Kullanıcı hesabı, yönetilen etki alanındaki devre dışı durumda kalır. Kullanıcı hesabını yönetilen etki alanından kaldırmak için Azure AD kiracınızdan zorla silmeniz gerekir.

Kullanıcı hesabını yönetilen etki alanından tamamen kaldırmak için kullanıcıyı Azure AD kiracınızdan kalıcı olarak silin. Bu MSDN [makalesinde](/previous-versions/azure/dn194132(v=azure.100))açıklandığı gibi `-RemoveFromRecycleBin` seçeneğiyle PowerShellcmdlet'inikullanın.`Remove-MsolUser`


## <a name="contact-us"></a>Bizimle iletişim kurun
[Geri bildirim paylaşmak veya destek için](contact-us.md)Azure Active Directory Domain Services ürün ekibine başvurun.
