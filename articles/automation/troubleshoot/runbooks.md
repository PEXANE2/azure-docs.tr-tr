---
title: Azure Otomasyonu runbook 'Ları sorunlarını giderme
description: Azure Otomasyonu runbook 'ları ile karşılaşabileceğiniz sorunları nasıl giderebileceğinizi ve çözeceğinizi öğrenin.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/24/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 8fd2f808169a0615e1ad5ca338823e3a8e3212ff
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79297858"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Runbook 'larda hatalarda sorun giderme

Azure Otomasyonu 'nda runbook yürütmekte olan hatalar olduğunda, sorunların tanılanmasına yardımcı olması için aşağıdaki adımları kullanabilirsiniz.

1. **Runbook betiğinizin yerel makinenizde başarıyla yürütüldüğünden emin olun.** 

    Dil başvurusu ve öğrenme modülleri için [PowerShell belgeleri](/powershell/scripting/overview) veya [Python belgeleri](https://docs.python.org/3/) bölümüne bakın. Komut dosyanızı yerel olarak yürütmek, şu gibi yaygın hataları bulabilir ve çözümleyebilir:

      * Eksik modüller
      * Sözdizimi hataları
      * Mantık hataları

2. **Runbook [hata akışlarını](https://docs.microsoft.com/azure/automation/automation-runbook-output-and-messages#runbook-output)araştırın.**

    Bu akışlara belirli iletiler için bakın ve bunları bu makalede belgelenen hatalarla karşılaştırın.

3. **Düğümlerinizin ve Otomasyon çalışma alanınızın gerekli modüllere sahip olduğundan emin olun.** 

    Runbook 'ünüz herhangi bir modülü içeri aktarırsanız, [Içeri aktarma modülleri](../shared-resources/modules.md#import-modules)' nde listelenen adımları kullanarak Otomasyon hesabınız için kullanılabilir olduğunu doğrulayın. [Azure Otomasyonu 'Nda Azure modüllerini güncelleştir bölümündeki](..//automation-update-azure-modules.md)yönergeleri izleyerek modüllerinizi en son sürüme güncelleştirin. Daha fazla sorun giderme bilgisi için bkz. [Modül sorunlarını giderme](shared-resources.md#modules).

4. **Runbook 'un askıya alınması veya beklenmedik şekilde başarısız olması durumunda yapın.**

    * [İş durumlarını denetleme](https://docs.microsoft.com/azure/automation/automation-runbook-execution#job-statuses) runbook durumlarını ve olası nedenleri tanımlar.
    * Runbook 'un askıya alınmadan önce ne olacağını belirlemek için Runbook 'a [ek çıkış ekleyin](https://docs.microsoft.com/azure/automation/automation-runbook-output-and-messages#message-streams) .
    * İşiniz tarafından oluşturulan [tüm özel durumları işleyin](https://docs.microsoft.com/azure/automation/automation-runbook-execution#handling-exceptions) .

## <a name="login-azurerm"></a>Senaryo: oturum açmak için login-AzureRMAccount komutunu çalıştırın

### <a name="issue"></a>Sorun

Bir runbook yürütürken aşağıdaki hatayı alıyorsunuz:

```error
Run Login-AzureRMAccount to login.
```

### <a name="cause"></a>Nedeni

Bu hata, farklı çalıştır hesabı kullanmadığınız veya farklı çalıştır hesabının süresi dolduğunda oluşabilir. Bkz. [Azure Otomasyonu farklı çalıştır hesaplarını yönetme](https://docs.microsoft.com/azure/automation/manage-runas-account).

Bu hatanın iki birincil nedeni vardır:

* Azurerd veya az modülünün farklı sürümleri vardır.
* Kaynaklara ayrı bir abonelikte erişmeye çalışıyorsunuz.

### <a name="resolution"></a>Çözüm

Bir Azurerd veya az Module güncelleştirildikten sonra bu hatayı alırsanız, tüm modüllerinizi aynı sürüme güncelleştirmeniz gerekir.

Başka bir abonelikteki kaynaklara erişmeye çalışıyorsanız, izinleri yapılandırmak için aşağıdaki adımları izleyebilirsiniz.

1. Otomasyon Farklı Çalıştır hesabına gidin ve uygulama KIMLIĞINI ve parmak izini kopyalayın.
  ![kopya uygulama KIMLIĞI ve Parmak Izi](../media/troubleshoot-runbooks/collect-app-id.png)
1. Otomasyon hesabının barındırılmadığını ve yeni bir rol ataması eklemek için abonelik Access Control gidin.
  ![Erişim denetimi](../media/troubleshoot-runbooks/access-control.png)
1. Daha önce toplanan uygulama KIMLIĞINI ekleyin. Katkıda bulunan izinleri seçin.
   rol ataması eklemek ![](../media/troubleshoot-runbooks/add-role-assignment.png)
1. Aboneliğin adını kopyalayın.
1. Artık Otomasyon hesabınızdaki izinleri diğer aboneliğe test etmek için aşağıdaki runbook kodunu kullanabilirsiniz. `"\<CertificateThumbprint\>"`, 1. adımda kopyaladığınız değerle değiştirin. `"\<SubscriptionName\>"`, 4. adımda kopyaladığınız değerle değiştirin.

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint "<CertificateThumbprint>"
    #Select the subscription you want to work with
    Select-AzureRmSubscription -SubscriptionName '<YourSubscriptionNameGoesHere>'

    #Test and get outputs of the subscriptions you granted access.
    $subscriptions = Get-AzureRmSubscription
    foreach($subscription in $subscriptions)
    {
        Set-AzureRmContext $subscription
        Write-Output $subscription.Name
    }
    ```

## <a name="unable-to-find-subscription"></a>Senaryo: Azure aboneliği bulunamıyor

### <a name="issue"></a>Sorun

`Select-AzureSubscription` veya `Select-AzureRmSubscription` cmdlet 'i ile çalışırken aşağıdaki hatayı alırsınız:

```error
The subscription named <subscription name> cannot be found.
```

### <a name="error"></a>Hata

Bu hata şu durumlarda oluşabilir:

* Abonelik adı geçerli değil.
* Abonelik ayrıntılarını almaya çalışan Azure Active Directory Kullanıcı, aboneliğin Yöneticisi olarak yapılandırılmamıştır.

### <a name="resolution"></a>Çözüm

Azure 'da kimlik doğrulaması yapılıp kalmadığını ve seçmek istediğiniz aboneliğe erişimi olup olmadığını belirlemek için aşağıdaki adımları izleyin.

1. Betiğinizin tek başına çalıştığından emin olmak için, Azure Otomasyonu dışında test edin.
2. `Select-AzureSubscription` cmdlet 'ini çalıştırmadan önce betiğinizin `Add-AzureAccount` cmdlet 'ini çalıştırdığından emin olun.
3. Runbook 'un başlangıcına `Disable-AzureRmContextAutosave –Scope Process` ekleyin. Bu cmdlet çağrısı, tüm kimlik bilgilerinin yalnızca geçerli runbook 'un yürütülmesine uygulanmasını sağlar.
4. Hala bu hata iletisini görüyorsanız, `Add-AzureAccount` cmdlet 'inin `AzureRmContext` parametresini ekleyerek kodunuzu değiştirin ve kodu yürütün.

   ```powershell
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $context = Get-AzureRmContext

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $context
    ```

## <a name="auth-failed-mfa"></a>Senaryo: Multi-Factor Authentication etkinleştirildiğinden Azure kimlik doğrulaması başarısız oldu

### <a name="issue"></a>Sorun

Azure Kullanıcı adı ve parolanızla Azure 'da kimlik doğrulanırken aşağıdaki hatayı alırsınız:

```error
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

### <a name="cause"></a>Nedeni

Azure hesabınızda Multi-Factor Authentication varsa, Azure 'da kimlik doğrulaması yapmak için bir Azure Active Directory Kullanıcı kullanamazsınız. Bunun yerine, kimlik doğrulaması için bir sertifika veya hizmet sorumlusu kullanmanız gerekir.

### <a name="resolution"></a>Çözüm

Azure klasik dağıtım modeli cmdlet 'leriyle bir sertifika kullanmak için [Azure hizmetlerini yönetmek üzere bir sertifika oluşturma ve ekleme](https://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx)bölümüne bakın. Azure Resource Manager cmdlet 'leriyle hizmet sorumlusu kullanmak için bkz. [Azure Portal kullanarak hizmet sorumlusu oluşturma](../../active-directory/develop/howto-create-service-principal-portal.md) ve [Azure Resource Manager Ile hizmet sorumlusu kimlik doğrulama](../../active-directory/develop/howto-authenticate-service-principal-powershell.md).

## <a name="get-serializationsettings"></a>Senaryo: iş akışlarında get_SerializationSettings yöntemiyle ilgili bir hata görürsünüz

### <a name="issue"></a>Sorun

Runbook için iş akışlarınız içinde aşağıdaki hatayı görürsünüz:

```error
Connect-AzureRMAccount : Method 'get_SerializationSettings' in type
'Microsoft.Azure.Management.Internal.Resources.ResourceManagementClient' from assembly
'Microsoft.Azure.Commands.ResourceManager.Common, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'
does not have an implementation.
At line:16 char:1
+ Connect-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Connect-AzureRmAccount], TypeLoadException
    + FullyQualifiedErrorId : System.TypeLoadException,Microsoft.Azure.Commands.Profile.ConnectAzureRmAccountCommand
```

### <a name="cause"></a>Nedeni

Bu hata, bir runbook 'ta hem Azurerd hem de az Module cmdlet 'leri kullanılarak oluşur. Azurerd modülünü içeri aktarmadan önce az Module içeri aktardığınızda gerçekleşir.

### <a name="resolution"></a>Çözüm

Az ve Azurerd cmdlet 'leri aynı runbook 'ta içeri aktarılamaz ve kullanılamaz. Azure Otomasyonu 'nda az cmdlet 'ler hakkında daha fazla bilgi için bkz. [Azure Otomasyonu 'Nda az Module support](../az-modules.md).

## <a name="task-was-cancelled"></a>Senaryo: runbook hata vererek başarısız oldu: bir görev iptal edildi

### <a name="issue"></a>Sorun

Runbook 'larınız aşağıdaki örneğe benzer bir hata ile başarısız olur:

```error
Exception: A task was canceled.
```

### <a name="cause"></a>Nedeni

Bu hata, eski Azure modülleri kullanılarak kaynaklanıyor olabilir.

### <a name="resolution"></a>Çözüm

Azure modüllerinizi en son sürüme güncelleştirerek bu hatayı çözebilirsiniz. 

1. Otomasyon hesabınızda **modüller**' e tıklayın ve ardından **Azure modüllerini güncelleştirin**. 
2. Güncelleştirme yaklaşık 15 dakika sürer. Tamamlandıktan sonra, başarısız olan runbook 'u yeniden çalıştırın.

Modüllerinizi güncelleştirme hakkında daha fazla bilgi için bkz. Azure [Otomasyonu 'Nda Azure modüllerini güncelleştirme](../automation-update-azure-modules.md).

## <a name="runbook-auth-failure"></a>Senaryo: birden çok abonelikle ilgilenirken runbook 'Lar başarısız olur

### <a name="issue"></a>Sorun

Runbook 'lar çalıştırılırken, runbook Azure kaynaklarını yönetemez.

### <a name="cause"></a>Nedeni

Runbook çalışırken doğru bağlamı kullanmıyor.

### <a name="resolution"></a>Çözüm

Runbook birden çok runbook 'u çağırdığında abonelik bağlamı kaybolabilir. Abonelik bağlamının runbook 'lara geçirildiğinden emin olmak için, istemci runbook 'unun bağlamı `AzureRmContext` parametresindeki `Start-AzureRmAutomationRunbook` cmdlet 'ine geçirdiğinden emin olun. Belirtilen kimlik bilgilerinin yalnızca geçerli runbook için kullanıldığından emin olmak için, `Scope` parametresi `Process` olarak ayarlanan `Disable-AzureRmContextAutosave` cmdlet 'ini kullanın. Daha fazla bilgi için bkz. [birden çok abonelikle çalışma](../automation-runbook-execution.md#working-with-multiple-subscriptions).

```azurepowershell-interactive
# Ensures that any credentials apply only to the execution of this runbook
Disable-AzureRmContextAutosave –Scope Process

# Connect to Azure with Run As account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzureRmContext $AzureContext `
    –Parameters $params –wait
```

## <a name="not-recognized-as-cmdlet"></a>Senaryo: bir cmdlet, işlev, komut dosyası adı olarak terim tanınmıyor

### <a name="issue"></a>Sorun

Runbook 'larınız aşağıdaki örneğe benzer bir hata ile başarısız olur:

```error
The term 'Connect-AzureRmAccount' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if the path was included verify that the path is correct and try again.
```

### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenlerden kaynaklanabilir:

* Cmdlet 'ini içeren modül Otomasyon hesabına içeri aktarılmaz.
* Cmdlet 'i içeren modül içeri aktarıldı, ancak güncel değil.

### <a name="resolution"></a>Çözüm

Bu hatayı çözmek için aşağıdaki görevlerden birini yapın. 

* Bir Azure modülü için bkz. [Azure Automation 'da Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md) , Otomasyon hesabınızda modüllerinizi güncelleştirme hakkında bilgi edinin.

* Azure olmayan bir modül için, modülün Otomasyon hesabınıza içeri aktarıldığından emin olun.

## <a name="job-attempted-3-times"></a>Senaryo: runbook işinin başlaması üç kez denendi, ancak her seferinde başlatılamadı

### <a name="issue"></a>Sorun

Runbook 'larınız şu hata ile başarısız oluyor:

```error
The job was tried three times but it failed
```

### <a name="cause"></a>Nedeni

Bu hata, aşağıdaki sorunlardan biri nedeniyle oluşur:

* Bellek sınırı. Bir iş 400 MB 'tan fazla bellek kullanıyorsa başarısız olabilir. Bir korumalı alana ayrılan bellek üzerinde belgelenen sınırlar [Automation hizmeti sınırları](../../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)' nde bulunur. 

* Ağ yuvaları. Azure korumalı alanlar 1000 eşzamanlı ağ yuvası ile sınırlıdır. [Otomasyon Hizmeti sınırlarına](../../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)bakın.

* Modül uyumsuz. Modül bağımlılıkları doğru olmayabilir. Bu durumda, runbook genellikle bir `Command not found` veya `Cannot bind parameter` iletisi döndürür.

* Korumalı alan için Active Directory kimlik doğrulaması yok. Runbook 'iniz, bir Azure korumalı alanında çalışan bir yürütülebilir dosyayı veya alt işlemi çağırmayı denedi. Azure Active Directory kimlik doğrulaması kitaplığı (ADAL) kullanarak Azure AD ile kimlik doğrulaması yapmak için Runbook 'ların yapılandırılması desteklenmez.

* Çok fazla özel durum verisi. Runbook, çıkış akışına çok fazla özel durum verisi yazmayı denedi.

### <a name="resolution"></a>Çözüm

* Bellek sınırı, ağ yuvaları. Bellek sınırları içinde çalışma için önerilen yollar, iş yükünü birden çok runbook arasında bölmek, bellekte daha az veri işlemek, runbook 'lerinizden gereksiz çıkış yazmaktan kaçınmak ve PowerShell iş akışınıza kaç denetim noktası yazılacağını göz önünde bulundurmaktır 'ları. Değişkenleri temizlemek için `$myVar.clear`gibi Clear yöntemini kullanın ve çöp toplamayı hemen çalıştırmak için `[GC]::Collect` kullanın. Bu eylemler çalışma zamanı sırasında runbook 'larınızın bellek parmak izini azaltır.

* Modül uyumsuz. Azure [Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](../automation-update-azure-modules.md)bölümündeki adımları izleyerek Azure modüllerinizi güncelleştirin.

* Korumalı alan için Active Directory kimlik doğrulaması yok. Azure AD 'de bir runbook ile kimlik doğrulanırken, Azure AD modülünün Otomasyon hesabınızda kullanılabilir olduğundan emin olun. Farklı Çalıştır hesabına runbook 'un otomatikleştiren görevleri gerçekleştirmek için gerekli izinleri verdiğinizden emin olun.

  Runbook 'iniz bir Azure korumalı alanında çalışan bir yürütülebilir dosyayı veya alt işlemi çağıramıyorum, bir [karma runbook çalışanında](../automation-hrw-run-runbooks.md)runbook 'u kullanın. Hibrit çalışanlar, Azure sanal ağı 'nın sahip olduğu bellek ve ağ limitleriyle sınırlı değildir.

* Çok fazla özel durum verisi. İş çıktı akışında bir 1 MB sınırı vardır. Runbook 'larınızın `try` ve `catch` blokları kullanarak bir çalıştırılabilir veya alt işleme çağrıları barındıraldığından emin olun. İşlemler bir özel durum oluştura, kodun özel durumdan bir Otomasyon değişkenine ileti yazmasını sağlayabilirsiniz. Bu teknik, iletinin iş çıkış akışına yazılmasını engeller.

## <a name="sign-in-failed"></a>Senaryo: Azure hesabında oturum açma başarısız oldu

### <a name="issue"></a>Sorun

`Add-AzureAccount` veya `Connect-AzureRmAccount` cmdlet 'i ile çalışırken aşağıdaki hatalardan birini alırsınız:

```error
Unknown_user_type: Unknown User Type
```

```error
No certificate was found in the certificate store with thumbprint
```

### <a name="cause"></a>Nedeni

Kimlik bilgisi varlık adı geçerli değilse bu hatalar oluşur. Ayrıca, Otomasyon kimlik bilgisi varlığını ayarlamak için kullandığınız Kullanıcı adı ve parola geçerli değilse da meydana gelebilir.

### <a name="resolution"></a>Çözüm

Neyin yanlış olduğunu belirlemek için aşağıdaki adımları uygulayın:

1. Özel karakter olmadığından emin olun. Bu karakterler, Azure 'a bağlanmak için kullandığınız Otomasyon kimlik bilgileri varlık adının `\@` karakterini içerir.
2. Yerel PowerShell ıSE düzenleyicinizdeki Azure Otomasyonu kimlik bilgilerinde depolanan kullanıcı adını ve parolayı kullanıp kullanamadıysanız emin olun. PowerShell ıSE 'de aşağıdaki cmdlet 'leri çalıştırın.

   ```powershell
   $Cred = Get-Credential
   #Using Azure Service Management
   Add-AzureAccount –Credential $Cred
   #Using Azure Resource Manager
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Kimlik doğrulama işlemi yerel olarak başarısız olursa, Azure Active Directory kimlik bilgilerinizi düzgün bir şekilde kurmadınız. Azure Active Directory hesabını doğru bir şekilde ayarlamak için Azure Active Directory blog gönderisine [Azure 'Da kimlik](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) doğrulamaya bakın.

4. Hata geçici gibi görünüyorsa, kimlik doğrulamasının daha sağlam olması için kimlik doğrulama yordamınızın yeniden deneme mantığını eklemeyi deneyin.

   ```powershell
   # Get the connection "AzureRunAsConnection"
   $connectionName = "AzureRunAsConnection"
   $servicePrincipalConnection = Get-AutomationConnection -Name $connectionName

   $logonAttempt = 0
   $logonResult = $False

   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
       $LogonAttempt++
       #Logging in to Azure...
       $connectionResult = Connect-AzureRmAccount `
                              -ServicePrincipal `
                              -TenantId $servicePrincipalConnection.TenantId `
                              -ApplicationId $servicePrincipalConnection.ApplicationId `
                              -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint

       Start-Sleep -Seconds 30
   }
   ```

## <a name="child-runbook-object"></a>Senaryo: nesne başvurusu bir nesnenin örneğine ayarlanmadı

### <a name="issue"></a>Sorun

Bir alt runbook 'u `Wait` parametresiyle çağırırken ve çıkış akışı bir nesne içeriyorsa aşağıdaki hatayı alırsınız:

```error
Object reference not set to an instance of an object
```

### <a name="cause"></a>Nedeni

akış nesneler içeriyorsa `Start-AzureRmAutomationRunbook` çıktı akışını doğru işlemez.

### <a name="resolution"></a>Çözüm

Bir yoklama mantığı uygulamak ve çıktıyı almak için [Get-Azurermautomationjoi put](/powershell/module/azurerm.automation/get-azurermautomationjoboutput) cmdlet 'ini kullanmanız önerilir. Bu mantığın bir örneği aşağıda tanımlanmıştır.

```powershell
$automationAccountName = "ContosoAutomationAccount"
$runbookName = "ChildRunbookExample"
$resourceGroupName = "ContosoRG"

function IsJobTerminalState([string] $status) {
    return $status -eq "Completed" -or $status -eq "Failed" -or $status -eq "Stopped" -or $status -eq "Suspended"
}

$job = Start-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName -Name $runbookName -ResourceGroupName $resourceGroupName
$pollingSeconds = 5
$maxTimeout = 10800
$waitTime = 0
while((IsJobTerminalState $job.Status) -eq $false -and $waitTime -lt $maxTimeout) {
   Start-Sleep -Seconds $pollingSeconds
   $waitTime += $pollingSeconds
   $job = $job | Get-AzureRmAutomationJob
}

$jobResults | Get-AzureRmAutomationJobOutput | Get-AzureRmAutomationJobOutputRecord | Select-Object -ExpandProperty Value
```

## <a name="fails-deserialized-object"></a>Senaryo: Serisi kaldırılan nesne nedeniyle runbook başarısız oluyor

### <a name="issue"></a>Sorun

Runbook 'larınız şu hata ile başarısız oluyor:

```error
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

### <a name="cause"></a>Nedeni

Runbook 'ağınız bir PowerShell Iş akışıdır, iş akışı askıya alınmışsa runbook 'unuzu kalıcı hale getirmek için karmaşık nesneleri seri durumdan çıkarılmış biçimde depolar.

### <a name="resolution"></a>Çözüm

Bu sorunu gidermek için aşağıdaki çözümlerden herhangi birini kullanın.

* Karmaşık nesneleri bir cmdlet 'ten diğerine çevreleyorsanız, bu cmdlet 'leri bir `InlineScript` etkinliğinde sarın.
* Tüm nesneyi geçirmek yerine, ihtiyacınız olan adı veya değeri karmaşık nesneden geçirin.
* PowerShell Iş akışı runbook 'u yerine bir PowerShell runbook 'u kullanın.

## <a name="quota-exceeded"></a>Senaryo: ayrılan kota aşıldığı için Runbook işi başarısız oldu

### <a name="issue"></a>Sorun

Runbook işiniz şu hata ile başarısız oluyor:

```error
The quota for the monthly total job run time has been reached for this subscription
```

### <a name="cause"></a>Nedeni

Bu hata, iş yürütmesi hesabınız için 500 dakikalık boş kotayı aştığında oluşur. Bu kota, tüm iş yürütme görevi türleri için geçerlidir. Bu görevlerden bazıları, bir işi test ediyor, portaldan bir işi başlatıyor, Web kancalarını kullanarak bir işi yürütüyor veya Azure portal ya da veri merkeziniz kullanılarak yürütülecek bir iş zamanlıyor. Otomasyon fiyatlandırması hakkında daha fazla bilgi edinmek için bkz. [Automation fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).

### <a name="resolution"></a>Çözüm

Ayda 500 dakikadan fazla işlem kullanmak istiyorsanız aboneliğinizi ücretsiz katmandan temel katmana değiştirin.

1. Azure aboneliğinizde oturum açın.
2. Yükseltilecek Otomasyon hesabını seçin.
3. **Ayarlar**' a ve ardından **fiyatlandırma**' ya tıklayın.
4. Hesabınızı temel katmana yükseltmek için en alttaki sayfada **Etkinleştir** ' e tıklayın.

## <a name="cmdlet-not-recognized"></a>Senaryo: bir runbook çalıştırılırken cmdlet tanınmıyor

### <a name="issue"></a>Sorun

Runbook işiniz şu hata ile başarısız oluyor:

```error
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

### <a name="cause"></a>Nedeni

Bu hata, PowerShell motorunun runbook 'inizdeki kullanmakta olduğunuz cmdlet 'i bulamamasının nedeni olur. Bu cmdlet 'i içeren modülün hesapta eksik olması, runbook adıyla bir ad çakışması olması veya cmdlet 'inin başka bir modülde de mevcut olması ve otomasyon 'un adı çözememem mümkündür.

### <a name="resolution"></a>Çözüm

Sorunu gidermek için aşağıdaki çözümlerden herhangi birini kullanın.

* Cmdlet adını doğru girdiğinizden emin olun.
* Cmdlet 'inin Otomasyon hesabınızda mevcut olduğundan ve çakışma olmadığından emin olun. Cmdlet 'in mevcut olup olmadığını doğrulamak için, düzenleme modunda bir runbook açın ve kitaplıkta bulmak istediğiniz cmdlet 'i arayın veya `Get-Command <CommandName>`çalıştırın. Cmdlet 'in hesap için kullanılabilir olduğunu ve diğer cmdlet 'ler veya Runbook 'larla ad çakışması olmadığını doğrulandıktan sonra, cmdlet 'ini tuvale ekleyin ve Runbook 'larınızda geçerli bir parametre kümesini kullandığınızdan emin olun.
* Ad çakışması varsa ve cmdlet iki farklı modülde kullanılabiliyorsa, cmdlet için tam adı kullanarak sorunu çözün. Örneğin, `ModuleName\CmdletName`kullanabilirsiniz.
* Bir karma çalışan grubunda runbook 'u şirket içinde yürütüyorsanız, modülün ve cmdlet 'inin karma çalışanı barındıran makinede yüklü olduğundan emin olun.

## <a name="long-running-runbook"></a>Senaryo: uzun süre çalışan bir runbook tamamlanamazsa

### <a name="issue"></a>Sorun

Runbook 'larınız 3 saat boyunca çalıştıktan sonra durdurulmuş durumda görüntülenir. Bu hatayı da alabilirsiniz:

```error
The job was evicted and subsequently reached a Stopped state. The job cannot continue running.
```

Bu davranış, Azure Otomasyonu 'ndaki işlemlerin dengeli bir şekilde izlenmesi nedeniyle Azure korumalı alan tasarımı ile [tasarlanmalıdır](../automation-runbook-execution.md#fair-share) . Bir işlem üç saatten uzun bir süre yürütülüyorsa, dengeli bir şekilde bir runbook otomatik olarak durduruluyor. Dengeli bir şekilde geçen bir runbook 'un durumu, runbook türüne göre farklılık gösterir. PowerShell ve Python runbook 'lar durdurulmuş bir duruma ayarlanır. PowerShell Iş akışı runbook 'ları başarısız olarak ayarlanır.

### <a name="cause"></a>Nedeni

Runbook, bir Azure korumalı alanında dengeli bir paylaşımın izin verdiği 3 saatlik sınırın üzerinde çalışır.

### <a name="resolution"></a>Çözüm

Önerilen bir çözüm, runbook 'u [karma Runbook Worker](../automation-hrw-run-runbooks.md)üzerinde çalıştıralım. Hibrit çalışanlar, Azure korumalı alan olan 3 saatlik dengeli bir paylaşılan runbook sınırı ile sınırlı değildir. Karma runbook çalışanları üzerinde çalışan runbook 'lar, beklenmeyen yerel altyapı sorunları varsa yeniden başlatma davranışlarını destekleyecek şekilde geliştirilmiştir.

Diğer bir çözüm, runbook 'u [alt runbook 'lar](../automation-child-runbooks.md)oluşturarak en iyileştirmenize olanak sağlar. Runbook 'iniz birkaç kaynak üzerinde aynı işlevle döngüye geçtiğinde (örneğin, birkaç veritabanında bir veritabanı işleminde), işlevi bir alt runbook 'a taşıyabilirsiniz. Her alt runbook ayrı bir işlemde paralel olarak yürütülür. Bu davranış, üst runbook 'un tamamlanacağı toplam süreyi düşürür.

Alt runbook senaryosunu etkinleştiren PowerShell cmdlet 'leri şunlardır:

* [Start-AzureRMAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook). Bu cmdlet bir runbook başlatmanızı ve bu runbook'a parametre iletmenizi sağlar.

* [Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/get-azurermautomationjob). Alt runbook tamamlandıktan sonra gerçekleştirilmesi gereken işlemler varsa, bu cmdlet her çocuk için iş durumunu denetlemenizi sağlar.

## <a name="expired webhook"></a>Senaryo: durum: 400 Web kancasını çağırırken hatalı Istek

### <a name="issue"></a>Sorun

Bir Azure Otomasyonu runbook 'u için bir Web kancası çağırmaya çalıştığınızda, şu hatayı alırsınız:

```error
400 Bad Request : This webhook has expired or is disabled
```

### <a name="cause"></a>Nedeni

Çağırmayı denediğiniz Web kancası devre dışı veya zaman aşımına uğradı.

### <a name="resolution"></a>Çözüm

Web kancası devre dışıysa, Azure portal aracılığıyla Web kancasını yeniden etkinleştirebilirsiniz. Web kancası süresi dolmuşsa, onu silip yeniden oluşturmanız gerekir. [Web kancasını](../automation-webhooks.md#renew-webhook) yalnızca henüz dolmamışsa yenileyebilirsiniz.

## <a name="429"></a>Senaryo: 429: istek oranı şu anda çok büyük...

### <a name="issue"></a>Sorun

`Get-AzureRmAutomationJobOutput` cmdlet 'ini çalıştırırken aşağıdaki hata iletisini alırsınız:

```error
429: The request rate is currently too large. Please try again
```

### <a name="cause"></a>Nedeni

Bu hata, çok sayıda [ayrıntılı akışı](../automation-runbook-output-and-messages.md#verbose-stream)olan bir runbook 'tan iş çıktısı alınırken oluşabilir.

### <a name="resolution"></a>Çözüm

Bu hatayı çözmek için aşağıdakilerden birini yapın.

* Runbook 'u düzenleyin ve yaydığı iş akışlarının sayısını azaltın.

* Cmdlet 'i çalıştırırken alınacak akış sayısını azaltın. Bunu yapmak için, `Get-AzureRmAutomationJobOutput` cmdlet 'inin `Stream` parametresinin değerini yalnızca çıkış akışlarını almak üzere ayarlayabilirsiniz. 

## <a name="cannot-invoke-method"></a>Senaryo: PowerShell işi hata vererek başarısız oldu: Yöntem çağrılamıyor

### <a name="issue"></a>Sorun

Azure 'da çalışan bir runbook 'ta bir PowerShell işi başlatırken aşağıdaki hata iletisini alırsınız:

```error
Exception was thrown - Cannot invoke method. Method invocation is supported only on core types in this language mode.
```

### <a name="cause"></a>Nedeni

Bu hata, bir Azure korumalı alanında yürütülen runbook 'ların [tam dil modunda](/powershell/module/microsoft.powershell.core/about/about_language_modes)çalıştıramayacağı anlamına gelebilir.

### <a name="resolution"></a>Çözüm

Bu hatayı çözmek için iki yol vardır.

* `Start-Job`kullanmak yerine, runbook 'u başlatmak için `Start-AzureRmAutomationRunbook` kullanın.
* Bir karma runbook çalışanında runbook 'u çalıştırmayı deneyin.

Bu davranış ve Azure Otomasyonu runbook 'larının diğer davranışları hakkında daha fazla bilgi edinmek için bkz. [runbook davranışı](../automation-runbook-execution.md#runbook-behavior).

## <a name="scenario-linux-hybrid-runbook-worker-receives-a-prompt-for-a-password-when-signing-a-runbook"></a>Senaryo: Linux hibrit Runbook Worker, bir runbook imzalanırken parola istemi alır

### <a name="issue"></a>Sorun

Linux karma Runbook Worker için `sudo` komutunun çalıştırılması, parola için beklenmeyen bir istem alır.

### <a name="cause"></a>Nedeni

Linux için Log Analytics aracısına ait **nxautomationuser** hesabı, **sudoers** dosyasında doğru şekilde yapılandırılmamış. Karma Runbook Worker 'ın, Linux Runbook Worker 'daki runbook 'ları imzalayabilmesi için hesap izinlerinin ve diğer verilerin uygun yapılandırmasına ihtiyacı vardır.

### <a name="resolution"></a>Çözüm

* Karma runbook çalışanının makinede GnuPG (GPG) yürütülebilirine sahip olduğundan emin olun.

* **Sudoers** dosyasındaki **nxautomationuser** hesabının yapılandırmasını doğrulayın. Bkz. [karma Runbook Worker üzerinde runbook 'Ları çalıştırma](../automation-hrw-run-runbooks.md)

## <a name="scenario-cmdlet-failing-in-pnp-powershell-runbook-on-azure-automation"></a>Senaryo: Azure Otomasyonu 'nda PnP PowerShell runbook 'unda başarısız olan cmdlet

### <a name="issue"></a>Sorun

Bir runbook, Azure Otomasyonu çıkışına doğrudan bir PnP PowerShell tarafından oluşturulan nesne yazdığında, cmdlet çıktısı Automation 'a geri akış yapamıyor.

### <a name="cause"></a>Nedeni

Bu sorun en yaygın olarak, Azure Otomasyonu, PnP PowerShell cmdlet 'lerini çağıran runbook 'ları işlediğinde (örneğin, `add-pnplistitem`, dönüş nesnelerini yakalama olmadan) oluşur.

### <a name="resolution"></a>Çözüm

Cmdlet 'lerinizi, tüm nesneleri standart çıktıya yazmayı denememesi için, değişkenlere bir dönüş değeri atamak üzere betikleri düzenleyin. Bir komut dosyası çıktı akışını aşağıda gösterildiği gibi bir cmdlet 'e yönlendirebilir.

```azurecli
  $null = add-pnplistitem
```

Komut dosyası cmdlet çıkışını ayrıştırdığında, komut dosyası çıktıyı bir değişkende depolamalıdır ve yalnızca çıktıyı akışa alma yerine değişkeni işleyebilir.

```azurecli
$SomeVariable = add-pnplistitem ....
if ($SomeVariable.someproperty -eq ....
```

## <a name="other"></a>Sorun yukarıda listelenmiyor

Aşağıdaki bölümler, diğer yaygın hataları listeler ve sorununuzu çözmenize yardımcı olacak destekleyici belgeler sağlar.

### <a name="hybrid-runbook-worker-doesnt-run-jobs-or-isnt-responding"></a>Karma runbook çalışanı işleri çalıştırmıyor veya yanıt vermiyor

Azure Otomasyonu yerine karma Runbook Worker üzerinde iş çalıştırıyorsanız, [karma çalışanın üzerinde sorun gidermeniz](https://docs.microsoft.com/azure/automation/troubleshoot/hybrid-runbook-worker)gerekebilir.

### <a name="runbook-fails-with-no-permission-or-some-variation"></a>Runbook, hiçbir izin veya bir çeşitleme olmadan başarısız oluyor

Farklı Çalıştır hesapları, geçerli hesabınız olarak Azure kaynaklarında aynı izinlere sahip olmayabilir. Farklı Çalıştır hesabınızın betikte kullanılan [kaynaklara erişmek için izinlere](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) sahip olduğundan emin olun.

### <a name="issues-passing-parameters-into-webhooks"></a>Web kancalarına parametre geçirme sorunları

Web kancalarına parametre geçirme konusunda yardım için bkz. [Web kancasından runbook başlatma](../automation-webhooks.md#parameters-used-when-the-webhook-starts-a-runbook).

### <a name="issues-using-az-modules"></a>Az modüller kullanan sorunlar

Aynı Otomasyon hesabında az modules ve Azurerd modüllerinin kullanılması desteklenmez. Daha fazla ayrıntı için bkz. [runbook 'Larda az modüller](https://docs.microsoft.com/azure/automation/az-modules) .

### <a name="inconsistent-behavior-in-runbooks"></a>Runbook'larda tutarsız davranış

Eşzamanlı işlerle ilgili sorunları, birden çok kez oluşturulan kaynakları veya Runbook 'larda diğer zamanlamaya duyarlı mantığı önlemek için [runbook yürütme](https://docs.microsoft.com/azure/automation/automation-runbook-execution#runbook-behavior) içindeki yönergeleri izleyin.

### <a name="runbook-fails-with-the-error-no-permission-forbidden-403-or-some-variation"></a>Runbook, izin yok, yasak (403) veya bazı çeşitlerle başarısız oluyor

Farklı Çalıştır hesapları, geçerli hesabınız olarak Azure kaynaklarında aynı izinlere sahip olmayabilir. Farklı Çalıştır hesabınızın betikte kullanılan [kaynaklara erişmek için izinlere](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) sahip olduğundan emin olun.

### <a name="runbooks-were-working-but-suddenly-stopped"></a>Runbook'lar çalışıyordu ama aniden durduruldu

* Farklı çalıştır hesabının süresi dolmadığından emin olun. Bkz. [sertifika yenileme](https://docs.microsoft.com/azure/automation/manage-runas-account#cert-renewal).
* Bir runbook 'u başlatmak için [Web kancası](https://docs.microsoft.com/azure/automation/automation-webhooks#renew-webhook) kullanıyorsanız, Web kancasının süresi dolmadığından emin olun.

### <a name="passing-parameters-into-webhooks"></a>Web kancalarına parametre geçirme

Web kancalarına parametre geçirme konusunda yardım için bkz. [Web kancasından runbook başlatma](https://docs.microsoft.com/azure/automation/automation-webhooks#parameters-used-when-the-webhook-starts-a-runbook).

### <a name="using-az-modules"></a>Az modüllerini kullanma

Aynı Otomasyon hesabında az modules ve Azurerd modüllerinin kullanılması desteklenmez. Bkz. [runbook 'Larda az modüller](https://docs.microsoft.com/azure/automation/az-modules).

### <a name="using-self-signed-certificates"></a>Otomatik olarak imzalanan sertifikaları kullanma

Otomatik olarak imzalanan sertifikaları kullanmak için bkz. [Yeni sertifika oluşturma](https://docs.microsoft.com/azure/automation/shared-resources/certificates#creating-a-new-certificate).

### <a name="access-denied-when-using-azure-sandbox-for-a-runbook"></a>Bir runbook için Azure korumalı alanı kullanılırken erişim reddedildi

Azure korumalı alanı tüm işlem dışı COM sunucularına erişimi engeller. Örneğin, korumalı bir uygulama veya Runbook Windows Yönetim Araçları (WMI) veya Windows Installer hizmetine (msiserver. exe) çağrı yapamıyor. Korumalı alanın kullanımı hakkındaki ayrıntılar için bkz. [Azure Otomasyonu 'Nda runbook yürütme](https://docs.microsoft.com/azure/automation/automation-runbook-execution.md).

## <a name="recommended-documents"></a>Önerilen Belgeler

* [Azure Otomasyonu 'nda runbook başlatma](https://docs.microsoft.com/azure/automation/automation-starting-a-runbook)
* [Azure Otomasyonu 'nda runbook yürütmesi](https://docs.microsoft.com/azure/automation/automation-runbook-execution)

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıtlar alın.
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyası gönderebilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
