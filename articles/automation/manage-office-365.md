---
title: Azure Otomasyonu'ni kullanarak Office 365 hizmetlerini yönetme
description: Office 365 abonelik hizmetlerini yönetmek için Azure Otomasyonu'nun nasıl kullanılacağını söyler.
services: automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 9cb505ced907b143fbd6a5f4f30c818092005bb8
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80550428"
---
# <a name="manage-office-365-services-using-azure-automation"></a>Azure Otomasyonu'ni kullanarak Office 365 hizmetlerini yönetme

Microsoft Word ve Microsoft Outlook gibi ürünler için Office 365 abonelik hizmetlerinin yönetimi için Azure Otomasyonu'ni kullanabilirsiniz. Office 365 ile etkileşimler [Azure Etkin Dizin (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)tarafından etkinleştirilir. Azure'a [kimlik doğrulamak için Azure Reklam'ı kullanın'a](automation-use-azure-ad.md)bakın.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Azure Otomasyonu'nda Office 365 abonelik hizmetlerini yönetmek için aşağıdakilere ihtiyacınız var.

* Azure aboneliği. Bkz. [Abonelik karar kılavuzu.](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/subscriptions/)
* Kullanıcı hesabı kimlik bilgilerini ve runbook'ları tutmak için Azure'daki bir Otomasyon nesnesi. Azure [Otomasyonuna giriş e-postasını](https://docs.microsoft.com/azure/automation/automation-intro)görün.
* Azure AD. Azure'a [kimlik doğrulamak için Azure Reklam'ı kullanın'a](automation-use-azure-ad.md)bakın.
* Hesabı olan bir Office 365 kiracısı. Bkz. [Office 365 kiracınızı ayarlama.](https://docs.microsoft.com/sharepoint/dev/spfx/set-up-your-developer-tenant)

## <a name="installing-the-msonline-and-msonlineext-modules"></a>MSOnline ve MSOnlineExt modüllerinin yüklenmesi

Office 365'in Azure Otomasyonu içinde kullanılması için Windows`MSOnline` PowerShell için Microsoft Azure Active Directory (modül) gerekir. Ayrıca, tek ve [`MSOnlineExt`](https://www.powershellgallery.com/packages/MSOnlineExt/1.0.35)çok kiracılı ortamlarda Azure AD yönetimini kolaylaştıran modüle de ihtiyacınız vardır. Azure'a kimlik doğrulaması yapmak için Azure [Otomasyonu'nda Azure AD'ı kullanın'da](automation-use-azure-ad.md)açıklandığı gibi modülleri yükleyin.

>[!NOTE]
>MSOnline PowerShell'i kullanmak için Azure AD üyesi olmalısınız. Konuk kullanıcılar modülü kullanamaz.

## <a name="creating-an-azure-automation-account"></a>Azure Otomasyon hesabı oluşturma

Bu makaledeki adımları tamamlamak için Azure Otomasyonu'nda bir hesaba ihtiyacınız var. Bkz. [Azure Otomasyon hesabı oluştur.](automation-quickstart-create-account.md)
 
## <a name="adding-msonline-and-msonlineext-as-assets"></a>Varlık olarak MSOnline ve MSOnlineExt ekleme

Şimdi Office 365 işlevselliğini etkinleştirmek için yüklü MSOnline ve MSOnlineExt modüllerini ekleyin. Azure [Otomasyonundaki Modülleri Yönet'e](shared-resources/modules.md)bakın.

1. Azure portalında **Otomasyon Hesapları'nı**seçin.
2. Otomasyon hesabınızı seçin.
3. **Paylaşılan Kaynaklar**altında **Modüller Galerisi'ni** seçin.
4. MSOnline'ı arayın.
5. PowerShell `MSOnline` modülünü seçin ve modülü varlık olarak almak için **İçe Aktar'ı** tıklatın.
6. Modülü bulmak ve almak için `MSOnlineExt` 4 ve 5 adımlarını tekrarlayın. 

## <a name="creating-a-credential-asset-optional"></a>Kimlik bilgisi kıymeti oluşturma (isteğe bağlı)

Komut dosyanızı çalıştırma izniolan Office 365 yönetim kullanıcısı için bir kimlik bilgisi varlığı oluşturmak isteğe bağlıdır. PowerShell komut dosyalarının içinde kullanıcı adlarını ve parolaları açığa çıkarmaktan alıkoymak için yardımcı olabilir. Yönergeler için [bkz.](automation-use-azure-ad.md#creating-a-credential-asset)

## <a name="creating-an-office-365-service-account"></a>Office 365 hizmet hesabı oluşturma

Office 365 abonelik hizmetlerini çalıştırmak için, istediğinizi yapmak için izinlere sahip bir Office 365 hizmet hesabına ihtiyacınız vardır. Bir genel yönetici hesabı, hizmet başına bir hesap veya yürütmek için bir işlev veya komut dosyası kullanabilirsiniz. Her durumda, hizmet hesabı karmaşık ve güvenli bir parola gerektirir. Bkz. [İşletmeler için Office 365'i ayarla.](https://docs.microsoft.com/microsoft-365/admin/setup/setup?view=o365-worldwide) 

## <a name="connecting-to-the-azure-ad-online-service"></a>Azure AD çevrimiçi hizmetine bağlanma

>[!NOTE]
>MSOnline modül cmdlets kullanmak için, Windows PowerShell bunları çalıştırmak gerekir. PowerShell Core bu cmdlets desteklemez.

Office 365 aboneliğinden Azure AD'ye bağlanmak için MSOnline modülünü kullanabilirsiniz. Bağlantı, Office 365 kullanıcı adı ve parolasını kullanır veya çok faktörlü kimlik doğrulaması (MFA) kullanır. Azure portalını veya Windows PowerShell komut istemini kullanarak bağlanabilirsiniz (yükseltilmesi gerekmez).

PowerShell örneği aşağıda gösterilmiştir. Kimlik bilgileri için [Get-Credentials cmdlet](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7) ister ve `Msolcred` bunları değişkende saklar. Ardından [Connect-MsolService](https://docs.microsoft.com/powershell/module/msonline/connect-msolservice?view=azureadps-1.0) cmdlet, Azure dizini çevrimiçi hizmetine bağlanmak için kimlik bilgilerini kullanır. Belirli bir Azure ortamına bağlanmak istiyorsanız, `AzureEnvironment` parametreyi kullanın.

```powershell
$Msolcred = Get-Credential
Connect-MsolService -Credential $MsolCred -AzureEnvironment "AzureCloud"
```

Herhangi bir hata almazsanız, başarılı bir şekilde bağlandınız. Hızlı bir test, örneğin `Get-MsolUser`bir Office 365 cmdlet çalıştırmak ve sonuçları görmektir. Hata alırsanız, sık karşılaşılan bir sorunun yanlış parola olduğunu unutmayın.

>[!NOTE]
>Azure AD'ye Office 365 aboneliğinden bağlanmak için AzureRM modülünü veya Az modülünü de kullanabilirsiniz. Ana bağlantı cmdlet [Connect-AzureAD](https://docs.microsoft.com/powershell/module/azuread/connect-azuread?view=azureadps-2.0)olduğunu. Bu cmdlet, `AzureEnvironmentName` belirli Office 365 ortamları için parametreyi destekler.

## <a name="creating-a-powershell-runbook-from-an-existing-script"></a>Varolan bir komut dosyasından PowerShell runbook oluşturma

PowerShell komut dosyasından Office 365 işlevine erişebilirsiniz. Burada kullanıcı adı `Office-Credentials` `admin@TenantOne.com`ile adlı bir kimlik bilgisi için bir komut dosyası örneği. Office `Get-AutomationPSCredential` 365 kimlik bilgisini almak için kullanır.

```powershell
$emailFromAddress = "admin@TenantOne.com" 
$emailToAddress = "servicedesk@TenantOne.com" 
$emailSMTPServer = "outlook.office365.com" 
$emailSubject = "Office 365 License Report" 

$credObject = Get-AutomationPSCredential -Name "Office-Credentials" 
Connect-MsolService -Credential $credObject

$O365Licenses = Get-MsolAccountSku | Out-String 
Send-MailMessage -Credential $credObject -From $emailFromAddress -To $emailToAddress -Subject $emailSubject -Body 

$O365Licenses -SmtpServer $emailSMTPServer -UseSSL
```

## <a name="running-the-script-in-a-runbook"></a>Komut dosyasını runbook'ta çalıştırma

Komut dosyanızı Azure Otomasyonu çalışma kitabında kullanabilirsiniz. Örneğin, PowerShell runbook türünü kullanırız.

1. Yeni bir PowerShell runbook oluşturun. Azure [Otomasyonu runbook'u Oluştur'a](https://docs.microsoft.com/azure/automation/automation-quickstart-create-runbook)bakın.
2. Otomasyon **hesabınızdan, Proses Otomasyonu**altında **Runbook'ları** seçin.
3. Yeni runbook'u seçin ve **Edit'i**tıklatın.
4. Komut dosyanızı kopyalayın ve runbook için metin düzenleyicisine yapıştırın.
5. **VARLIKLAR'ı**seçin, ardından **Kimlik Bilgilerini** genişletin ve Office 365 kimlik bilgisinin orada olduğunu doğrulayın.
6. **Kaydet**'e tıklayın.
7. **Test bölmesini**seçin, ardından çalışma kitabınızı test etmeye başlamak için **Başlat'ı** tıklatın. Bkz. [Azure Otomasyonunda Runbook'ları Yönet.](https://docs.microsoft.com/azure/automation/manage-runbooks)
8. Test tamamlandığında, Test bölmesinden çıkın.

## <a name="publishing-and-scheduling-the-runbook"></a>Runbook'u yayımlama ve zamanlama

Runbook'unuzu yayımlamak ve sonra zamanlamak için [Azure Otomasyonu'nda runbook'ları yönet'e](https://docs.microsoft.com/azure/automation/manage-runbooks)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonu'nda [Kimlik Bilgileri varlıklarında](shared-resources/credentials.md)Otomasyon kimlik bilgileri hakkında bilgi bulabilirsiniz.
* Otomasyon modülleriyle nasıl çalışacağımızı öğrenmek için [Azure Otomasyonu'ndaki modülleri yönet'e](shared-resources/modules.md) bakın.
* Runbook yönetimine genel bir bakış için azure [otomasyonunda runbook'ları yönet'e](https://docs.microsoft.com/azure/automation/manage-runbooks)bakın.
* Azure Otomasyonu'nda bir runbook başlatmak için kullanılabilecek yöntemler hakkında daha fazla bilgi edinmek için azure [otomasyonunda runbook başlatma](automation-starting-a-runbook.md)'ya bakın.
* Dil referansı ve öğrenme modülleri de dahil olmak üzere PowerShell hakkında daha fazla bilgi için [PowerShell Dokümanları'na](https://docs.microsoft.com/powershell/scripting/overview)bakın.