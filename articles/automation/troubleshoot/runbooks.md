---
title: Azure Otomasyon Runbook'ları ile hataları giderme
description: Azure Otomasyon runbook'ları ile karşılaşabileceğiniz sorunları nasıl gidereceğinizi ve çözeceğinizi öğrenin.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/24/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 26c5c5b31d5f3f9e1a642c0bafb947190e479055
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632620"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Runbook sorunlarını giderme

Azure Otomasyonu'nda runbook'ları çalıştırırken hatalar olduğunda, sorunları tanılamaya yardımcı olmak için aşağıdaki adımları kullanabilirsiniz.

1. **Runbook komut dosyanızın yerel makinenizde başarılı bir şekilde yürütülmesini sağlayın.** 

    Dil referansı ve öğrenme modülleri için [PowerShell Dokümanları](/powershell/scripting/overview) veya [Python Dokümanları'na](https://docs.python.org/3/) bakın. Komut dosyanızı yerel olarak yürütmek, şu gibi yaygın hataları keşfedebilir ve çözebilir:

      * Eksik modüller
      * Sözdizimi hataları
      * Mantık hataları

2. **Runbook [hata akışlarını](https://docs.microsoft.com/azure/automation/automation-runbook-output-and-messages#runbook-output)araştırın.**

    Belirli iletiler için bu akışlara bakın ve bunları bu makalede belgelenen hatalarla karşılaştırın.

3. **Düğümlerinizin ve Otomasyon çalışma alanınızın gerekli modüllere sahip olduğundan emin olun.** 

    Runbook'unuz herhangi bir modül alıyorsa, [İçe Aktarma modüllerinde](../shared-resources/modules.md#importing-modules)listelenen adımları kullanarak bunların Otomasyon hesabınızda kullanılabildiğini doğrulayın. Azure Otomasyonu'ndaki [Azure modüllerini güncelleştir'deki](..//automation-update-azure-modules.md)yönergeleri izleyerek modüllerinizi en son sürüme güncelleyin. Daha fazla sorun giderme bilgisi için [Sorun Giderme modüllerine](shared-resources.md#modules)bakın.

4. **Runbook'unuzun askıya alınması veya beklenmedik bir şekilde başarısız olması durumunda yapın.**

    * [İş durumlarını denetle,](https://docs.microsoft.com/azure/automation/automation-runbook-execution#job-statuses) runbook durumlarını ve bazı olası nedenleri tanımlar.
    * Runbook askıya alınmadan önce ne olacağını belirlemek için runbook'a [ek çıktı ekleyin.](https://docs.microsoft.com/azure/automation/automation-runbook-output-and-messages#message-streams)
    * İşiniz tarafından atılan [tüm özel durumları ele alın.](https://docs.microsoft.com/azure/automation/automation-runbook-execution#handling-exceptions)

## <a name="scenario-run-login-azurermaccount-to-log-in"></a><a name="login-azurerm"></a>Senaryo: Oturum açmak için Giriş-AzureRMAccount'u çalıştırın

### <a name="issue"></a>Sorun

Bir runbook çalıştırırken aşağıdaki hatayı alırsınız:

```error
Run Login-AzureRMAccount to login.
```

### <a name="cause"></a>Nedeni

Bu hata, Bir Run As hesabı kullanmadığınızda veya Çalıştır Çalıştır hesabının süresi dolduğunda oluşabilir. Bkz. [Azure Otomasyonu Hesabı Olarak Çalıştır'](https://docs.microsoft.com/azure/automation/manage-runas-account)ı yönetin.

Bu hatanın iki birincil nedeni vardır:

* AzureRM veya Az modülünün farklı sürümleri vardır.
* Ayrı bir abonelikteki kaynaklara erişmeye çalışıyorsunuz.

### <a name="resolution"></a>Çözüm

Bir AzureRM veya Az modülünüzü güncelledikten sonra bu hatayı alırsanız, tüm modüllerinizi aynı sürümde güncelleştirmeniz gerekir.

Başka bir abonelikteki kaynaklara erişmeye çalışıyorsanız, izinleri yapılandırmak için aşağıdaki adımları izleyebilirsiniz.

1. Otomasyon Çalıştır Hesabına gidin ve uygulama kimliğini ve parmak izini kopyalayın.
  ![Uygulama Kimliğini ve Parmak İzininI Kopyala](../media/troubleshoot-runbooks/collect-app-id.png)
1. Otomasyon hesabının barındırılan olmadığı aboneliğin Erişim Denetimi'ne gidin ve yeni bir rol ataması ekleyin.
  ![Erişim denetimi](../media/troubleshoot-runbooks/access-control.png)
1. Daha önce toplanan uygulama kimliğini ekleyin. Katılımcı izinlerini seçin.
   ![Rol ataması ekle](../media/troubleshoot-runbooks/add-role-assignment.png)
1. Aboneliğin adını kopyalayın.
1. Artık Otomasyon hesabınızdan diğer aboneye kadar izinleri test etmek için aşağıdaki runbook kodunu kullanabilirsiniz. Adım `"\<CertificateThumbprint\>"` 1'de kopyaladığınız değerle değiştirin. Adım `"\<SubscriptionName\>"` 4'te kopyaladığınız değerle değiştirin.

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

## <a name="scenario-unable-to-find-the-azure-subscription"></a><a name="unable-to-find-subscription"></a>Senaryo: Azure aboneliğini bulamıyor

### <a name="issue"></a>Sorun

Cmdlet `Select-AzureSubscription` ile `Select-AzureRmSubscription` çalışırken aşağıdaki hatayı alırsınız:

```error
The subscription named <subscription name> cannot be found.
```

### <a name="error"></a>Hata

Bu hata oluşabilir:

* Abonelik adı geçerli değil.
* Abonelik ayrıntılarını almaya çalışan Azure Etkin Dizin kullanıcısı, aboneliğin yöneticisi olarak yapılandırılmamıştır.

### <a name="resolution"></a>Çözüm

Azure'da kimlik doğrulaması yapıp yaptığınızı ve seçmeye çalıştığınız aboneye erişip erişmediğinizi belirlemek için aşağıdaki adımları izleyin.

1. Komut dosyanızın tek başına çalıştığından emin olmak için, komut dosyanızı Azure Otomasyonu dışında test edin.
2. Cmdlet'i çalıştırmadan önce komut `Select-AzureSubscription` dosyanızın cmdlet çalıştığından `Add-AzureAccount` emin olun.
3. Runbook'unuzun başına ekleyin. `Disable-AzureRmContextAutosave –Scope Process` Bu cmdlet çağrısı, herhangi bir kimlik bilgilerinin yalnızca geçerli runbook'un yürütülmesi için geçerli olmasını sağlar.
4. Bu hata iletisini hala görüyorsanız, `AzureRmContext` `Add-AzureAccount` cmdlet için parametre ekleyerek kodunuzu değiştirin ve ardından kodu çalıştırın.

   ```powershell
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $context = Get-AzureRmContext

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $context
    ```

## <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a><a name="auth-failed-mfa"></a>Senaryo: Çok faktörlü kimlik doğrulama etkinleştirildiğinden Azure'da kimlik doğrulama başarısız oldu

### <a name="issue"></a>Sorun

Azure kullanıcı adınız ve parolanızla Azure'a kimlik doğrularken aşağıdaki hatayı alırsınız:

```error
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

### <a name="cause"></a>Nedeni

Azure hesabınızda çok faktörlü kimlik doğrulamanız varsa, Azure'a kimlik doğrulamak için bir Azure Etkin Dizin kullanıcısını kullanamazsınız. Bunun yerine, kimlik doğrulaması için bir sertifika veya hizmet sorumlusu kullanmanız gerekir.

### <a name="resolution"></a>Çözüm

Azure klasik dağıtım modeli cmdlets ile bir sertifika kullanmak [için, Azure hizmetlerini yönetmek için sertifika oluşturma ve ekleme'ye](https://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx)bakın. Azure Kaynak Yöneticisi cmdlets ile bir hizmet yöneticisi kullanmak için bkz: [Azure portalını kullanarak hizmet sorumlusu oluşturma](../../active-directory/develop/howto-create-service-principal-portal.md) ve Azure Kaynak Yöneticisi ile bir hizmet [yöneticisinin kimlik doğrulaması.](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="scenario-you-see-an-error-in-your-job-streams-about-the-get_serializationsettings-method"></a><a name="get-serializationsettings"></a>Senaryo: İş akışlarınızda get_SerializationSettings yöntemiyle ilgili bir hata görürsünüz

### <a name="issue"></a>Sorun

Bir runbook için iş akışlarınızda aşağıdaki hatayı görürsünüz:

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

Bu hata, bir runbook'ta hem AzureRM hem de Az modülü cmdlet'leri kullanılmasından kaynaklanır. AzureRM modüllerini almadan önce Az modüllerini içe aktardığınızda oluşur.

### <a name="resolution"></a>Çözüm

Az ve AzureRM cmdlets aynı runbook içe aktarılamaz ve kullanılamaz. Azure Otomasyonu'nda Az cmdlets hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda Az modülü desteğine](../az-modules.md)bakın.

## <a name="scenario-the-runbook-fails-with-the-error-a-task-was-canceled"></a><a name="task-was-cancelled"></a>Senaryo: Runbook hata ile başarısız olur: Bir görev iptal edildi

### <a name="issue"></a>Sorun

Runbook aşağıdaki örneğe benzer bir hata ile başarısız olur:

```error
Exception: A task was canceled.
```

### <a name="cause"></a>Nedeni

Bu hata, eski Azure modülleri kullanılarak neden olabilir.

### <a name="resolution"></a>Çözüm

Bu hatayı, Azure modüllerinizi en son sürüme güncelleyerek çözebilirsiniz. 

1. Otomasyon hesabınızda **Modüller'i**tıklatın ve ardından **Azure modüllerini güncelleştirin.** 
2. Güncelleştirme yaklaşık 15 dakika sürer. Tamamlandıktan sonra, başarısız olan runbook'u yeniden çalıştırın.

Modüllerinizi güncelleme hakkında daha fazla bilgi edinmek için [Azure modüllerini Azure Otomasyonu'nda güncelleştir'e](../automation-update-azure-modules.md)bakın.

## <a name="scenario-runbooks-fail-when-dealing-with-multiple-subscriptions"></a><a name="runbook-auth-failure"></a>Senaryo: Runbook'lar birden çok abonelikle uğraşırken başarısız

### <a name="issue"></a>Sorun

Runbook'lar yürütüldüğünde, runbook Azure kaynaklarını yönetemezse.

### <a name="cause"></a>Nedeni

Runbook çalışırken doğru bağlamı kullanmıyor.

### <a name="resolution"></a>Çözüm

Bir runbook birden çok runbook çağırdığında abonelik bağlamı kaybolabilir. Abonelik bağlamını runbook'lara geçirinsağlamak için istemci runbook'un `Start-AzureRmAutomationRunbook` bağlamı parametredeki cmdlet'e geçirmesini `AzureRmContext` sağlayın. Belirtilen `Disable-AzureRmContextAutosave` kimlik bilgilerinin `Scope` yalnızca geçerli `Process` runbook için kullanıldığından emin olmak için parametre ayarlı cmdlet'i kullanın. Daha fazla bilgi için bkz: [Birden çok abonelikle çalışma.](../automation-runbook-execution.md#working-with-multiple-subscriptions)

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

## <a name="scenario-term-not-recognized-as-the-name-of-a-cmdlet-function-script"></a><a name="not-recognized-as-cmdlet"></a>Senaryo: Bir cmdlet, fonksiyon, komut dosyası adı olarak tanınmıyor terim

### <a name="issue"></a>Sorun

Runbook aşağıdaki örneğe benzer bir hata ile başarısız olur:

```error
The term 'Connect-AzureRmAccount' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if the path was included verify that the path is correct and try again.
```

### <a name="cause"></a>Nedeni

Bu hata aşağıdaki nedenlerle olabilir:

* Cmdlet içeren modül Otomasyon hesabına aktarılmaz.
* Cmdlet içeren modül içe aktarılır ancak güncel değildir.

### <a name="resolution"></a>Çözüm

Bu hatayı gidermek için aşağıdaki görevlerden birini yapın. 

* Azure modülü için, Modüllerinizi Otomasyon hesabınızda nasıl güncelleştireceklerini öğrenmek için [Azure Otomasyonu'ndaki Azure PowerShell modüllerini nasıl](../automation-update-azure-modules.md) güncelleştirebilirsiniz'e bakın.

* Azure olmayan bir modül için, otomasyon hesabınıza aktarılan modülün olduğundan emin olun.

## <a name="scenario-the-runbook-job-start-was-attempted-three-times-but-failed-to-start-each-time"></a><a name="job-attempted-3-times"></a>Senaryo: Runbook iş başlangıcı üç kez denendi, ancak her seferinde başlatılamamış

### <a name="issue"></a>Sorun

Runbook aşağıdaki hata ile başarısız olur:

```error
The job was tried three times but it failed
```

### <a name="cause"></a>Nedeni

Bu hata, aşağıdaki sorunlardan biri nedeniyle oluşur:

* Bellek Sınırı. Bir iş, 400 MB'dan fazla bellek kullanıyorsa başarısız olabilir. Bir kum havuzuna ayrılan bellekte belgelenmiş sınırlar [Otomasyon hizmet sınırlarında](../../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)bulunur. 

* Ağ Soketleri. Azure sandbox'ları 1000 eşzamanlı ağ soketiile sınırlıdır. Bkz. [Otomasyon hizmet limitleri.](../../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)

* Modül Uyumsuz. Modül bağımlılıkları doğru olmayabilir. Bu durumda, runbook'unuzun `Command not found` genellikle `Cannot bind parameter` bir veya ileti döndürür.

* Sandbox için Active Directory ile Kimlik Doğrulama yok. Runbook'unuzun azure kum alanında çalışan bir yürütülebilir veya alt işlemi çağırmaya çalışması. Azure Active Directory Authentication Library (ADAL) kullanarak runbook'ları Azure AD ile kimlik doğrulaması yapacak şekilde yapılandırmak desteklenmez.

* Çok fazla özel durum verisi. Runbook çıktı akışıiçin çok fazla özel durum verileri yazmaya çalıştı.

### <a name="resolution"></a>Çözüm

* Bellek Sınırı, Ağ Soketleri. Bellek sınırları içinde çalışmanın önerilen yolları, iş yükünü birden çok runbook arasında bölmek, bellekte daha az veri işlemek, runbook'larınızdan gereksiz çıktı yazmaktan kaçınmak ve PowerShell iş akışı çalışma kitaplarınızda kaç denetim noktası yazıldığını göz önünde bulundurmaktır. Değişkenleri temizlemek ve `$myVar.clear`çöp toplamayı hemen çalıştırmak `[GC]::Collect` için kullanmak gibi açık yöntemi kullanın. Bu eylemler, çalışma sırasında runbook'unuzun bellek ayak izini azaltır.

* Modül Uyumsuz. [Azure PowerShell modüllerini Azure](../automation-update-azure-modules.md)Otomasyonu'nda güncelleştirme adımlarını izleyerek Azure modüllerinizi güncelleştirin.

* Sandbox için Active Directory ile Kimlik Doğrulama yok. Azure AD'ye bir runbook ile kimlik doğrulaması yaparken, Azure AD modülünün Otomasyon hesabınızda kullanılabilir olduğundan emin olun. Run As hesabına runbook'un otomatikleştiren görevleri gerçekleştirmek için gerekli izinleri verdiğinden emin olun.

  Runbook'unuzun Azure kum havuzunda çalışan bir yürütülebilir veya alt işlemi çağıramazsa, [Karma Runbook Çalışması'nda](../automation-hrw-run-runbooks.md)runbook'u kullanın. Karma çalışanlar, Azure kum kutularının sahip olduğu bellek ve ağ sınırlarıyla sınırlı değildir.

* Çok fazla özel durum verisi. İş çıktısı akışında 1MB sınırı vardır. Runbook'unuzun aramaları çalıştırılabilir veya alt işlem `try` le `catch` kapladığını ve engellediklerinden emin olun. İşlemler bir özel durum atarsa, kodun özel durumdaki iletiyi bir Otomasyon değişkenine yazsın. Bu teknik, iletinin iş çıktısı akışına yazılmasını engeller.

## <a name="scenario-sign-in-to-azure-account-failed"></a><a name="sign-in-failed"></a>Senaryo: Azure hesabında oturum açma başarısız oldu

### <a name="issue"></a>Sorun

Cmdlet `Add-AzureAccount` ile `Connect-AzureRmAccount` çalışırken aşağıdaki hatalardan birini alırsınız:

```error
Unknown_user_type: Unknown User Type
```

```error
No certificate was found in the certificate store with thumbprint
```

### <a name="cause"></a>Nedeni

Kimlik bilgisi varlık adı geçerli değilse, bu hatalar oluşur. Otomasyon kimlik bilgilerini ayarlamak için kullandığınız kullanıcı adı ve parola geçerli değilse de oluşabilir.

### <a name="resolution"></a>Çözüm

Sorunun ne olduğunu belirlemek için aşağıdaki adımları izleyin:

1. Özel karakteriniz olmadığından emin olun. Bu karakterler, `\@` Azure'a bağlanmak için kullandığınız Otomasyon kimlik bilgisi varlık adındaki karakteri içerir.
2. Yerel PowerShell ISE düzenleyicinizde Azure Otomasyon kimlik bilgisinde depolanan kullanıcı adı ve parolayı kullanıp kullanamayacağınızı kontrol edin. PowerShell ISE'de aşağıdaki cmdletleri çalıştırın.

   ```powershell
   $Cred = Get-Credential
   #Using Azure Service Management
   Add-AzureAccount –Credential $Cred
   #Using Azure Resource Manager
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Kimlik doğrulamanız yerel olarak başarısız olursa, Azure Etkin Dizin kimlik bilgilerinizi düzgün şekilde ayarlamamışsınız. Azure Active Directory hesabının doğru ayarlanması için Azure Active Directory blog gönderisini [kullanarak Azure'a Kimlik Doğrulaması'na](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) bakın.

4. Hata geçici görünüyorsa, kimlik doğrulama yı daha sağlam hale getirmek için kimlik doğrulama yordamınıza yeniden deneme mantığı eklemeyi deneyin.

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

## <a name="scenario-object-reference-not-set-to-an-instance-of-an-object"></a><a name="child-runbook-object"></a>Senaryo: Nesne başvurusu bir nesneörneğine ayarlanmaz

### <a name="issue"></a>Sorun

`Wait` Parametreile bir alt runbook çağırArak aşağıdaki hatayı alırsınız ve Çıktı akışı bir nesne içerir:

```error
Object reference not set to an instance of an object
```

### <a name="cause"></a>Nedeni

`Start-AzureRmAutomationRunbook`akış nesneleri içeriyorsa Çıktı akışını doğru şekilde işlemez.

### <a name="resolution"></a>Çözüm

Bir yoklama mantığı uygulamak ve çıktıalmak için [Get-AzureRmAutomationJobOutput](/powershell/module/azurerm.automation/get-azurermautomationjoboutput) cmdlet kullanılması önerilir. Bu mantığın bir örneği aşağıda tanımlanmıştır.

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

## <a name="scenario-runbook-fails-because-of-deserialized-object"></a><a name="fails-deserialized-object"></a>Senaryo: Runbook deserialized nesne nedeniyle başarısız olur

### <a name="issue"></a>Sorun

Runbook hata ile başarısız olur:

```error
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

### <a name="cause"></a>Nedeni

Runbook'unuzun bir PowerShell İş Akışı olması durumunda, iş akışı askıya alınmışsa runbook durumunuzu devam etmek için karmaşık nesneleri deserialized biçiminde depolar.

### <a name="resolution"></a>Çözüm

Bu sorunu gidermek için aşağıdaki çözümlerden herhangi birini kullanın.

* Karmaşık nesneleri bir cmdletten diğerine borulıyorsanız, bu cmdletleri `InlineScript` bir aktiviteye sarın.
* Tüm nesneyi geçmek yerine karmaşık nesneden gereksinim duyduğunuz adı veya değeri geçirin.
* PowerShell İş Akışı çalışma kitabı yerine PowerShell runbook kullanın.

## <a name="scenario-runbook-job-fails-because-allocated-quota-exceeded"></a><a name="quota-exceeded"></a>Senaryo: Ayrılan kota aşıldığından runbook işi başarısız oldu

### <a name="issue"></a>Sorun

Runbook iş hata ile başarısız olur:

```error
The quota for the monthly total job run time has been reached for this subscription
```

### <a name="cause"></a>Nedeni

Bu hata, iş yürütme hesabınız için 500 dakikalık ücretsiz kotayı aştığında oluşur. Bu kota, tüm iş yürütme görevleri türleri için geçerlidir. Bu görevlerden bazıları bir işi sınamak, portaldan bir iş başlatmak, web hooks kullanarak bir işi yürütmek veya Azure portalınızı veya veri merkezinizi kullanarak bir işi yürütmek üzere zamanlamakdır. Otomasyon fiyatlandırması hakkında daha [Automation pricing](https://azure.microsoft.com/pricing/details/automation/)fazla bilgi edinmek için Otomasyon fiyatlandırması'na bakın.

### <a name="resolution"></a>Çözüm

Ayda 500 dakikadan fazla işlem yapmak istiyorsanız, aboneliğinizi Ücretsiz katmandan Temel katmana değiştirin.

1. Azure aboneliğinizde oturum açın.
2. Yükseltmek için Otomasyon hesabını seçin.
3. **Ayarlar'ı**tıklatın, ardından **Fiyatlandırma.**
4. Hesabınızı Temel katmana yükseltmek için sayfanın altındaki **Etkinleştir'i** tıklatın.

## <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a><a name="cmdlet-not-recognized"></a>Senaryo: Cmdlet bir runbook çalıştırırken tanınmıyor

### <a name="issue"></a>Sorun

Runbook iş hata ile başarısız olur:

```error
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

### <a name="cause"></a>Nedeni

Bu hata, PowerShell motoru runbook'unuzda kullandığınız cmdlet'i bulamıyorsa neden olur. Cmdlet içeren modülün hesapta eksik olması, runbook adı ile bir ad çakışması olması veya cmdlet'in başka bir modülde bulunması ve Otomasyon'un adı çözemeolasılığı mümkündür.

### <a name="resolution"></a>Çözüm

Sorunu gidermek için aşağıdaki çözümlerden herhangi birini kullanın.

* Cmdlet adını doğru girdiğinize emin olun.
* Cmdlet'in Otomasyon hesabınızda olduğundan ve çakışma olmadığından emin olun. Cmdlet'in mevcut olup olmadığını doğrulamak için, bir runbook'u edit modunda açın ve kitaplıkta bulmak istediğiniz cmdlet'i arayın veya çalıştırın. `Get-Command <CommandName>` Cmdlet'in hesapta kullanılabildiğini ve diğer cmdletveya runbook'larla hiçbir ad çakışması olmadığını doğruladıktan sonra, cmdlet'i tuvale ekleyin ve runbook'unuzda geçerli bir parametre kümesi kullandığınızdan emin olun.
* Bir ad çakışması nız varsa ve cmdlet iki farklı modülde mevcutsa, cmdlet için tam nitelikli adı kullanarak sorunu çözün. Örneğin `ModuleName\CmdletName` kullanabilirsiniz.
* Çalışma kitabını karma bir işçi grubunda şirket içinde çalıştırıyorsanız, modülün ve cmdlet'in hibrit işçiyi barındıran makineye yüklendiğinden emin olun.

## <a name="scenario-a-long-running-runbook-fails-to-complete"></a><a name="long-running-runbook"></a>Senaryo: Uzun soluklu bir runbook tamamlayamadı

### <a name="issue"></a>Sorun

Runbook'unuzun 3 saat çalıştırdıktan sonra Durduruldu durumunda gösterir. Bu hatayı da alabilirsiniz:

```error
The job was evicted and subsequently reached a Stopped state. The job cannot continue running.
```

Bu davranış, Azure Otomasyonu içindeki işlemlerin [adil paylaşım](../automation-runbook-execution.md#fair-share) izlemesi nedeniyle Azure ve Azure kum kutularında tasarım gereğidir. Bir işlem üç saatten uzun yürütürse, adil paylaşım bir runbook'u otomatik olarak durdurur. Adil paylaşım süresi sınırını geçen bir runbook'un durumu runbook türüne göre değişir. PowerShell ve Python runbook'ları Durduruldu durumuna ayarlanır. PowerShell İş Akışı runbook'ları Başarısız olarak ayarlanır.

### <a name="cause"></a>Nedeni

Runbook, Azure kum havuzunda adil paylaşımın izin verdiği 3 saatlik sınırı aştı.

### <a name="resolution"></a>Çözüm

Önerilen bir [çözüm, bir Karma Runbook Worker](../automation-hrw-run-runbooks.md)runbook çalıştırmaktır. Karma Çalışanlar, Azure kum kutularının sahip olduğu 3 saatlik adil paylaşım çalışma kitabı sınırıyla sınırlı değildir. Karma Runbook Çalışanları üzerinde çalıştırılan runbook'lar, beklenmeyen yerel altyapı sorunları varsa yeniden başlatma davranışlarını desteklemek için geliştirilmelidir.

Başka bir çözüm [alt runbook'lar](../automation-child-runbooks.md)oluşturarak runbook optimize etmektir. Runbook'unuz, örneğin, çeşitli veritabanlarındaki bir veritabanı işleminde, birden çok kaynakta aynı işlev üzerinden döngü ler yapıyorsa, işlevi bir alt çalışma kitabına taşıyabilirsiniz. Her alt çalışma kitabı ayrı bir işlemde paralel olarak yürütülür. Bu davranış, üst çalışma kitabının tamamlanması için gereken toplam süreyi azaltır.

Çocuk runbook senaryosunu etkinleştiren PowerShell cmdlets şunlardır:

* [Başlangıç-AzureRMAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook). Bu cmdlet bir runbook başlatmanızı ve bu runbook'a parametre iletmenizi sağlar.

* [Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/get-azurermautomationjob). Alt çalışma defteri tamamlandıktan sonra yapılması gereken işlemler varsa, bu cmdlet her çocuğun iş durumunu kontrol etmenizi sağlar.

## <a name="scenario-status-400-bad-request-when-calling-a-webhook"></a><a name="expired webhook"></a>Senaryo: Durum: Webhook'u ararken 400 Kötü İstek

### <a name="issue"></a>Sorun

Azure Otomasyon runbook'u için bir webhook çağırmaya çalıştığınızda aşağıdaki hatayı alırsınız:

```error
400 Bad Request : This webhook has expired or is disabled
```

### <a name="cause"></a>Nedeni

Aramaya çalıştığınız web hook devre dışı veya süresi dolmuş.

### <a name="resolution"></a>Çözüm

Webhook devre dışı bırakılırsa, Azure portalı üzerinden webhook'u yeniden etkinleştirebilirsiniz. Webhook süresi dolduysa, silmek ve sonra yeniden gerekir. [Bir web kancası](../automation-webhooks.md#renew-webhook) yalnızca süresi dolmamışsa yenileyebilirsiniz.

## <a name="scenario-429-the-request-rate-is-currently-too-large"></a><a name="429"></a>Senaryo: 429: İstek oranı şu anda çok büyük...

### <a name="issue"></a>Sorun

Cmdlet çalıştırırken aşağıdaki `Get-AzureRmAutomationJobOutput` hata iletisini alırsınız:

```error
429: The request rate is currently too large. Please try again
```

### <a name="cause"></a>Nedeni

Bu hata, birçok [Verbose akışı](../automation-runbook-output-and-messages.md#verbose-stream)olan bir runbook iş çıktısı alırken oluşabilir.

### <a name="resolution"></a>Çözüm

Bu hatayı gidermek için aşağıdakilerden birini yapın.

* Runbook'u düzenle ve yaydığı iş akışlarının sayısını azaltın.

* Cmdlet çalıştırırken alınacak akış sayısını azaltın. Bunu yapmak için, `Stream` `Get-AzureRmAutomationJobOutput` yalnızca Çıkış akışları almak için cmdlet için parametre değerini ayarlayabilirsiniz. 

## <a name="scenario-powershell-job-fails-with-error-cannot-invoke-method"></a><a name="cannot-invoke-method"></a>Senaryo: PowerShell iş hatası ile başarısız olur: Yöntem çağıramaz

### <a name="issue"></a>Sorun

Azure'da çalışan bir runbook'ta PowerShell işini başlatırken aşağıdaki hata iletisini alırsınız:

```error
Exception was thrown - Cannot invoke method. Method invocation is supported only on core types in this language mode.
```

### <a name="cause"></a>Nedeni

Bu hata, Azure kum havuzunda çalıştırılabilen runbook'ların [Tam dil modunda](/powershell/module/microsoft.powershell.core/about/about_language_modes)çalıştırılamayabileceğini gösterebilir.

### <a name="resolution"></a>Çözüm

Bu hatayı gidermenin iki yolu vardır.

* Kullanmak yerine `Start-Job`runbook'u başlatmak için kullanın. `Start-AzureRmAutomationRunbook`
* Bir Karma Runbook Worker üzerinde runbook çalıştırmayı deneyin.

Azure Otomasyon runbook'larının bu davranışı ve diğer davranışları hakkında daha fazla bilgi edinmek için [Runbook davranışına](../automation-runbook-execution.md#runbook-behavior)bakın.

## <a name="scenario-linux-hybrid-runbook-worker-receives-a-prompt-for-a-password-when-signing-a-runbook"></a>Senaryo: Linux Hybrid Runbook Worker, runbook imzalarken parola için bir komut istemi alır

### <a name="issue"></a>Sorun

Bir `sudo` Linux Hybrid Runbook Worker komutunu çalıştırmak, parola için beklenmeyen bir komut istemi alır.

### <a name="cause"></a>Nedeni

Linux için Log Analytics aracısının **nxautomationuser** hesabı **sudoers** dosyasında doğru şekilde yapılandırılmamıştır. Karma Runbook Worker, Linux Runbook Worker'da runbook'ları imzalayabilmek için hesap izinlerinin ve diğer verilerin uygun yapılandırmasını sağlar.

### <a name="resolution"></a>Çözüm

* Hibrit Runbook Worker'ın makinede çalıştırılabilir GnuPG (GPG) olduğundan emin olun.

* **Sudoers** dosyasındaki **nxautomationuser** hesabının yapılandırmasını doğrulayın. Bkz. [Karma Runbook Çalışanı üzerinde runbook'lar çalıştırma](../automation-hrw-run-runbooks.md)

## <a name="scenario-cmdlet-failing-in-pnp-powershell-runbook-on-azure-automation"></a>Senaryo: Azure Otomasyonu'nda PnP PowerShell runbook'ta cmdlet başarısız oluyor

### <a name="issue"></a>Sorun

Bir runbook, PnP PowerShell tarafından oluşturulan bir nesneyi doğrudan Azure Otomasyon çıkışına yazdığında, cmdlet çıktısı Otomasyon'a geri aktaramaz.

### <a name="cause"></a>Nedeni

Bu sorun en sık, örneğin, geri dönen nesneleri yakalamadan PnP PowerShell `add-pnplistitem`cmdlets'i çağıran runbook'ları azure otomasyonu işlediğinde oluşur.

### <a name="resolution"></a>Çözüm

Cmdlets standart çıktıtüm nesneleri yazmaya çalışmayın böylece değişkenlere herhangi bir dönüş değerleri atamak için komut dosyalarınızı edin. Komut dosyası, çıkış akışını aşağıda gösterildiği gibi bir cmdlet'e yönlendirebilir.

```azurecli
  $null = add-pnplistitem
```

Komut dosyanız cmdlet çıktısını ayrıştırırsa, komut dosyasıçıktıyı bir değişkende depolamalı ve çıktıyı akışla değiştirmek yerine değişkeni manipüle etmelidir.

```azurecli
$SomeVariable = add-pnplistitem ....
if ($SomeVariable.someproperty -eq ....
```

## <a name="my-problem-isnt-listed-above"></a><a name="other"></a>Sorunum yukarıda listelenmez

Aşağıdaki bölümlerde diğer yaygın hataları listeler ve sorununuzu çözmenize yardımcı olacak destekleyici belgeler sağlar.

### <a name="hybrid-runbook-worker-doesnt-run-jobs-or-isnt-responding"></a>Karma runbook çalışanı işleri çalıştırmıyor veya yanıt vermiyor

Azure Otomasyonu yerine Karma Runbook İşçisi'nde iş çalıştırıyorsanız, [karma çalışanın kendisini sorun gidermeniz](https://docs.microsoft.com/azure/automation/troubleshoot/hybrid-runbook-worker)gerekebilir.

### <a name="runbook-fails-with-no-permission-or-some-variation"></a>Runbook izni veya bazı varyasyon ile başarısız olur

Hesapları Çalıştır, azure kaynaklarına karşı geçerli hesabınızla aynı izinlere sahip olmayabilir. Run As hesabınızın komut dosyanızda kullanılan [kaynaklara erişmek için izinleri](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) olduğundan emin olun.

### <a name="issues-passing-parameters-into-webhooks"></a>Parametreleri webhooks'a aktaran sorunlar

Parametreleri webhook'lara geçirme konusunda yardım için [bkz.](../automation-webhooks.md#parameters-used-when-the-webhook-starts-a-runbook)

### <a name="issues-using-az-modules"></a>Az modüllerini kullanan sorunlar

Az modüllerinin ve AzureRM modüllerinin aynı Otomasyon hesabında kullanılması desteklenmez. Daha fazla bilgi için [runbook'larda Az modüllerine](https://docs.microsoft.com/azure/automation/az-modules) bakın.

### <a name="inconsistent-behavior-in-runbooks"></a>Runbook'larda tutarsız davranış

Eşzamanlı işler, birden çok kez oluşturulan kaynaklar veya runbook'larda zamanlamaya duyarlı diğer mantıkla ilgili sorunları önlemek için [Runbook yürütmedeki](https://docs.microsoft.com/azure/automation/automation-runbook-execution#runbook-behavior) kılavuzu izleyin.

### <a name="runbook-fails-with-the-error-no-permission-forbidden-403-or-some-variation"></a>Runbook hata no izni, Yasak (403) veya bazı varyasyon ile başarısız

Hesapları Çalıştır, azure kaynaklarına karşı geçerli hesabınızla aynı izinlere sahip olmayabilir. Run As hesabınızın komut dosyanızda kullanılan [kaynaklara erişmek için izinleri](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) olduğundan emin olun.

### <a name="runbooks-were-working-but-suddenly-stopped"></a>Runbook'lar çalışıyordu ama aniden durduruldu

* Çalıştır Hesabının süresinin dolmadığından emin olun. Bkz. [sertifika yenileme.](https://docs.microsoft.com/azure/automation/manage-runas-account#cert-renewal)
* Runbook'u başlatmak için [bir webhook](https://docs.microsoft.com/azure/automation/automation-webhooks#renew-webhook) kullanıyorsanız, webhook'un süresinin dolmadığından emin olun.

### <a name="passing-parameters-into-webhooks"></a>Web kancalarına parametre geçirme

Parametreleri webhook'lara geçirme konusunda yardım için [bkz.](https://docs.microsoft.com/azure/automation/automation-webhooks#parameters-used-when-the-webhook-starts-a-runbook)

### <a name="using-az-modules"></a>Az modüllerini kullanma

Az modüllerinin ve AzureRM modüllerinin aynı Otomasyon hesabında kullanılması desteklenmez. [Runbook'larda Az modüllerine](https://docs.microsoft.com/azure/automation/az-modules)bakın.

### <a name="using-self-signed-certificates"></a>Kendi imzalı sertifikaları kullanma

Kendi imzalı sertifikaları kullanmak için [bkz.](https://docs.microsoft.com/azure/automation/shared-resources/certificates#creating-a-new-certificate)

### <a name="access-denied-when-using-azure-sandbox-for-a-runbook"></a>Bir runbook için Azure sandbox kullanırken erişim reddedildi

Azure sandbox, tüm işlem dışı COM sunucularına erişimi engeller. Örneğin, kumhavuzuyla dolu bir uygulama veya runbook, Windows Management Instrumentation'a (WMI) veya Windows Installer hizmetine (msiserver.exe) çağrı yapamaz. Kum havuzunun kullanımı yla ilgili ayrıntılar için [Azure Otomasyonu'nda Runbook yürütmebölümüne](https://docs.microsoft.com/azure/automation/automation-runbook-execution)bakın.

## <a name="recommended-documents"></a>Önerilen Belgeler

* [Azure Otomasyonu'nda runbook başlatma](https://docs.microsoft.com/azure/automation/automation-starting-a-runbook)
* [Azure Otomasyon'da runbook yürütme](https://docs.microsoft.com/azure/automation/automation-runbook-execution)

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
