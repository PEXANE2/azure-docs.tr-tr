---
title: Azure Otomasyonu’nu kullanarak Office 365 hizmetlerini yönetme
description: Bu makalede, Azure Otomasyonu 'nu kullanarak Office 365 abonelik hizmetlerini nasıl yöneteceğiniz açıklanmaktadır.
services: automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: cbd01f3868f44d975e0822a7812262d9e15ca299
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83745352"
---
# <a name="manage-office-365-services"></a>Office 365 hizmetlerini yönetme

Microsoft Word ve Microsoft Outlook gibi ürünler için Office 365 abonelik hizmetlerinin yönetimi için Azure Otomasyonu ' nu kullanabilirsiniz. Office 365 etkileşimleri [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)tarafından etkinleştirilir. Azure ['da kimlik doğrulaması yapmak için bkz. Azure Otomasyonu 'Nda Azure ad kullanma](automation-use-azure-ad.md).

## <a name="prerequisites"></a>Ön koşullar

Azure Otomasyonu 'nda Office 365 abonelik hizmetlerini yönetmek için aşağıdakiler gerekir.

* Azure aboneliği. Bkz. [abonelik karar Kılavuzu](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/subscriptions/).
* Kullanıcı hesabı kimlik bilgilerini ve Runbook 'ları tutmak için Azure 'da bir Otomasyon nesnesi. Bkz. [Azure Otomasyonu 'na giriş](https://docs.microsoft.com/azure/automation/automation-intro).
* Azure AD. Azure ['da kimlik doğrulaması yapmak için bkz. Azure Otomasyonu 'Nda Azure ad kullanma](automation-use-azure-ad.md).
* Bir hesabı olan Office 365 kiracısı. Bkz. [Office 365 kiracınızı ayarlama](https://docs.microsoft.com/sharepoint/dev/spfx/set-up-your-developer-tenant).

## <a name="install-the-msonline-and-msonlineext-modules"></a>MSOnline ve MSOnlineExt modüllerini yükler

Azure Otomasyonu 'nda Office 365 kullanımı, Windows PowerShell için Microsoft Azure Active Directory gerektirir ( `MSOnline` Modül). Ayrıca [`MSOnlineExt`](https://www.powershellgallery.com/packages/MSOnlineExt/1.0.35) , tek ve çok kiracılı ortamlarda Azure AD yönetimini kolaylaştıran modüle da ihtiyacınız olacaktır. Azure ['da kimlik doğrulamak için Azure Otomasyonu 'nda Azure ad kullanma](automation-use-azure-ad.md)bölümünde açıklandığı gibi modülleri de yüklersiniz.

>[!NOTE]
>MSOnline PowerShell 'i kullanmak için Azure AD 'nin bir üyesi olmanız gerekir. Konuk kullanıcılar modülünü kullanamaz.

## <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma

Bu makaledeki adımları tamamlayabilmeniz için, Azure Otomasyonu 'nda bir hesaba ihtiyacınız vardır. Bkz. [Azure Otomasyonu hesabı oluşturma](automation-quickstart-create-account.md).
 
## <a name="add-msonline-and-msonlineext-as-assets"></a>MSOnline ve MSOnlineExt as varlıkları ekleme

Şimdi, Office 365 işlevselliğini etkinleştirmek için yüklü MSOnline ve MSOnlineExt modüllerini ekleyin. [Azure Automation 'da modülleri yönetme](shared-resources/modules.md)bölümüne bakın.

1. Azure portal **Otomasyon hesapları**' nı seçin.
2. Otomasyon hesabınızı seçin.
3. **Paylaşılan kaynaklar**altında **modüller Galerisi** ' ni seçin.
4. MSOnline için arama yapın.
5. `MSOnline`PowerShell modülünü seçin ve **içeri aktar** ' a tıklayarak modülü bir varlık olarak içeri aktarın.
6. Modülün yerini bulmak ve içeri aktarmak için 4. ve 5. adımları tekrarlayın `MSOnlineExt` . 

## <a name="create-a-credential-asset-optional"></a>Kimlik bilgisi varlığı oluşturma (isteğe bağlı)

Komut dosyanızı çalıştırma iznine sahip olan Office 365 Yönetici kullanıcısı için bir kimlik bilgisi varlığı oluşturmak isteğe bağlıdır. Bu, ancak PowerShell betikleri içinde kullanıcı adlarını ve parolaları açığa tutmaya yardımcı olabilir. Yönergeler için bkz. [kimlik bilgisi varlığı oluşturma](automation-use-azure-ad.md#create-a-credential-asset).

## <a name="create-an-office-365-service-account"></a>Office 365 hizmet hesabı oluşturma

Office 365 abonelik hizmetlerini çalıştırmak için, istediğiniz şeyi yapmanız gereken izinlere sahip bir Office 365 hizmet hesabına sahip olmanız gerekir. Tek bir genel yönetici hesabı, hizmet başına bir hesap veya yürütülecek bir işlev ya da komut dosyası kullanabilirsiniz. Herhangi bir durumda, hizmet hesabı için karmaşık ve güvenli bir parola gerekir. Bkz. [iş Için Office 365 'Yi ayarlama](https://docs.microsoft.com/microsoft-365/admin/setup/setup?view=o365-worldwide). 

## <a name="connect-to-the-azure-ad-online-service"></a>Azure AD çevrimiçi hizmetine bağlanma

>[!NOTE]
>MSOnline modülü cmdlet 'lerini kullanmak için Windows PowerShell 'den çalıştırmanız gerekir. PowerShell Core Bu cmdlet 'leri desteklemez.

Office 365 aboneliğinden Azure AD 'ye bağlanmak için MSOnline modülünü kullanabilirsiniz. Bağlantı, Office 365 Kullanıcı adı ve parolası kullanır veya Multi-Factor Authentication (MFA) kullanır. Azure portal veya bir Windows PowerShell komut istemi (yükseltilmesi gerekmez) kullanarak bağlanabilirsiniz.

Bir PowerShell örneği aşağıda gösterilmiştir. [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7) cmdlet 'i kimlik bilgilerini ister ve `Msolcred` değişkende depolar. [Connect-MsolService](https://docs.microsoft.com/powershell/module/msonline/connect-msolservice?view=azureadps-1.0) cmdlet 'i Azure Directory Online hizmetine bağlanmak için kimlik bilgilerini kullanır. Belirli bir Azure ortamına bağlanmak istiyorsanız `AzureEnvironment` parametresini kullanın.

```powershell
$Msolcred = Get-Credential
Connect-MsolService -Credential $MsolCred -AzureEnvironment "AzureCloud"
```

Herhangi bir hata almazsanız, başarıyla bağlanırsınız. Hızlı bir test, örneğin, bir Office 365 cmdlet 'ini çalıştırmak `Get-MsolUser` ve sonuçları görmek içindir. Hata alırsanız, yaygın bir sorunun hatalı parola olduğunu unutmayın.

>[!NOTE]
>Ayrıca, Office 365 aboneliğinden Azure AD 'ye bağlanmak için Azurere modülünü veya az modülünü de kullanabilirsiniz. Ana bağlantı cmdlet 'i [Connect-AzureAD](https://docs.microsoft.com/powershell/module/azuread/connect-azuread?view=azureadps-2.0)' dir. Bu cmdlet, `AzureEnvironmentName` belirli Office 365 ortamları için parametreyi destekler.

## <a name="create-a-powershell-runbook-from-an-existing-script"></a>Mevcut bir betikten PowerShell runbook 'u oluşturma

Office 365 işlevselliğine bir PowerShell betiğiyle erişirsiniz. Kullanıcı adına adlı bir kimlik bilgisi için betiğe bir örnek aşağıda verilmiştir `Office-Credentials` `admin@TenantOne.com` . `Get-AutomationPSCredential`Office 365 kimlik bilgisini içeri aktarmak için kullanır.

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

## <a name="run-the-script-in-a-runbook"></a>Betiği runbook 'ta çalıştırma

Komut dosyanızı bir Azure Otomasyonu runbook 'unda kullanabilirsiniz. Örneğin, PowerShell runbook türünü kullanacağız.

1. Yeni bir PowerShell runbook 'u oluşturun. [Azure Otomasyonu runbook 'U oluşturma](https://docs.microsoft.com/azure/automation/automation-quickstart-create-runbook)bölümüne bakın.
2. Otomasyon hesabınızdan, **Işlem Otomasyonu**altında **runbook 'lar** ' ı seçin.
3. Yeni runbook 'u seçin ve **Düzenle**' ye tıklayın.
4. Betiğinizi kopyalayıp runbook için metin düzenleyicisine yapıştırın.
5. **Varlıklar**' ı seçin, ardından **kimlik bilgileri** ' ni genişletin ve Office 365 kimlik bilgisinin orada olduğunu doğrulayın.
6. **Kaydet**’e tıklayın.
7. **Test bölmesi**' ni seçin ve ardından runbook 'unuzu sınamaya başlamak için **Başlat** ' a tıklayın. Bkz. [Azure Otomasyonu 'nda runbook 'Ları yönetme](https://docs.microsoft.com/azure/automation/manage-runbooks).
8. Sınama tamamlandığında, test bölmesinden çıkın.

## <a name="publish-and-schedule-the-runbook"></a>Runbook 'u yayımlama ve zamanlama

Runbook 'unuzu yayımlamak ve zamanlamak için bkz. [Azure Otomasyonu 'nda runbook 'Ları yönetme](https://docs.microsoft.com/azure/automation/manage-runbooks).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Otomasyonu 'nda runbook 'ları yönetme](https://docs.microsoft.com/azure/automation/manage-runbooks)
* [Azure Otomasyonu’nda modülleri yönetme](shared-resources/modules.md)
* [Azure Otomasyonu 'nda kimlik bilgilerini yönetme](shared-resources/credentials.md)
* [PowerShell belgeleri](https://docs.microsoft.com/powershell/scripting/overview)