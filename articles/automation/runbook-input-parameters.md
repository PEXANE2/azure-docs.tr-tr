---
title: Runbook giriş parametreleri
description: Runbook giriş parametreleri, başlatıldığında verileri runbook'a aktarmanıza izin vererek runbook'ların esnekliğini artırır. Bu makalede, giriş parametrelerinin runbook'larda kullanıldığı farklı senaryolar açıklanmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 02/14/2019
ms.topic: conceptual
ms.openlocfilehash: 17be351d4af3d277242af70ea96e8735a5f68bc9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78329094"
---
# <a name="runbook-input-parameters"></a>Runbook giriş parametreleri

Runbook giriş parametreleri, başlatıldığında verilerin ona aktarılmasına izin vererek runbook'un esnekliğini artırır. Bu parametreler, runbook eylemlerinin belirli senaryolar ve ortamlar için hedefalınmasına olanak tanır. Bu makalede, runbook'larınızdaki giriş parametrelerinin yapılandırması ve kullanımı açıklanmaktadır.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="configuring-input-parameters"></a>Giriş parametrelerini yapılandırma

PowerShell, PowerShell İş Akışı, grafik ve Python runbook'ları için giriş parametrelerini yapılandırabilirsiniz. Runbook'un farklı veri türlerine sahip birden çok parametresi olabilir veya hiç parametre yok. Giriş parametreleri zorunlu veya isteğe bağlı olabilir ve isteğe bağlı parametreler için varsayılan değerleri kullanabilirsiniz.

Başlattığınızda bir runbook'un giriş parametrelerine değerler atarsınız. Azure portalından, web hizmetinden veya PowerShell'den bir runbook başlatabilirsiniz. Ayrıca, başka bir runbook'ta satır adı verilen bir alt çalışma kitabı olarak da başlayabilirsiniz.

### <a name="configure-input-parameters-in-powershell-runbooks"></a>PowerShell runbook'larında giriş parametrelerini yapılandırma

Azure Otomasyonu'ndaki PowerShell ve PowerShell İş Akışı çalışma kitapları, aşağıdaki özellikler aracılığıyla tanımlanan giriş parametrelerini destekler. 

| **Özellik** | **Açıklama** |
|:--- |:--- |
| Tür |Gereklidir. Parametre değeri için beklenen veri türü. Herhangi bir .NET türü geçerlidir. |
| Adı |Gereklidir. Parametrenin adı. Bu ad runbook içinde benzersiz olmalıdır, bir harf ile başlamalı ve yalnızca harfler, sayılar veya alt çizgi karakterleri içerebilir. |
| Zorunlu |İsteğe bağlı. Parametrenin bir değer gerektirip gerektirmeyini belirten Boolean değeri. Bunu **doğru**ayarlarsanız, runbook başlatıldığında bir değer sağlanmalıdır. Bunu **false**olarak ayarlarsanız, bir değer isteğe bağlıdır. **Zorunlu** özellik için bir değer belirtmezseniz, PowerShell varsayılan olarak isteğe bağlı giriş parametresini dikkate alır. |
| Varsayılan değer |İsteğe bağlı. Runbook başlatıldığında giriş değeri geçirilirse parametre için kullanılan değer. Runbook herhangi bir parametre için varsayılan bir değer ayarlayabilir. |

Windows PowerShell, doğrulama, diğer ad ve parametre kümeleri gibi yukarıda listelenenlerden daha fazla giriş parametreleri özniteliklerini destekler. Ancak Azure Otomasyonu şu anda yalnızca listelenen giriş parametresi özelliklerini destekler.

Örnek olarak, PowerShell İş Akışı çalışma kitabındaki parametre tanımına bakalım. Bu tanım, birden çok parametrenin virgülle ayrıldığı aşağıdaki genel forma sahiptir.

```powershell
Param
(
  [Parameter (Mandatory= $true/$false)]
  [Type] $Name1 = <Default value>,

  [Parameter (Mandatory= $true/$false)]
  [Type] $Name2 = <Default value>
)
```

Şimdi, tek bir VM veya kaynak grubundaki tüm VM'ler gibi sanal makinelerle ilgili ayrıntıları ortaya koyan powershell iş akışı çalışma kitabının giriş parametrelerini yapılandıralım. Bu runbook aşağıdaki ekran görüntüsünde gösterildiği gibi iki parametre vardır: sanal makinenin adı (*VMName*) ve kaynak grubunun adı (*resourceGroupName*).

![Otomasyon PowerShell İş Akışı](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

Bu parametre tanımında, giriş parametreleri tip dizesinin basit parametreleridir.

PowerShell ve PowerShell İş Akışı runbook'ların giriş parametreleri için **nesne** veya **PSCredential** gibi tüm basit türleri ve karmaşık türleri desteklediğini unutmayın. Runbook'unuzda nesne giriş parametresi varsa, bir değerde geçmek için ad değeri çiftleri içeren bir PowerShell karma tablosu kullanmanız gerekir. Örneğin, bir runbook'ta aşağıdaki parametreye sahipsiniz.

```powershell
[Parameter (Mandatory = $true)]
[object] $FullName
```

Bu durumda, aşağıdaki değeri parametreye geçirebilirsiniz.

```powershell
@{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}
```

> [!NOTE]
> Bir değeri null varsayılan değeri olan isteğe bağlı String parametresine geçirmediğinizde, parametrenin değeri **Null**yerine boş bir dizedir.

### <a name="configure-input-parameters-in-graphical-runbooks"></a>Grafik çalışma kitaplarında giriş parametrelerini yapılandırma

Grafik çalışma kitabı giriş parametrelerinin yapılandırmasını göstermek için, tek bir VM veya kaynak grubundaki tüm VM'ler gibi sanal makinelerle ilgili ayrıntıları çıktılayan bir runbook oluşturalım. Ayrıntılar için [ilk grafik çalışma kitabıma](automation-first-runbook-graphical.md)bakın.

Grafik çalışma kitabı şu büyük runbook etkinliklerini kullanır:

* Azure Ile kimlik doğrulaması için Azure Çalıştır Hesabı'nın yapılandırması. 
* VM özelliklerini almak için [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm?view=azps-3.5.0) cmdlet tanımı.
* VM adlarını çıktılamak için [Yazma-Çıktı](/powershell/module/microsoft.powershell.utility/write-output) etkinliğinin kullanımı. 

**Get-AzVM** etkinliği iki girişi tanımlar: VM adı ve kaynak grubu adı. Runbook her başlatılında bu adlar farklı olabileceğinden, bu girişleri kabul etmek için runbook'unuza giriş parametreleri eklemeniz gerekir. Azure [Otomasyonunda Grafiksel yazarlık](automation-graphical-authoring-intro.md)kitabına bakın.

Giriş parametrelerini yapılandırmak için aşağıdaki adımları izleyin.

1. **Runbook** sayfasından grafik runbook'u seçin ve ardından **Edit'i**tıklatın.
2. Grafik düzenleyicisinde, Giriş **ve çıktı** düğmesini tıklatın ve runbook Giriş Parametresi bölmesini açmak için **giriş ekleyin.**

   ![Otomasyon grafik runbook](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)

3. Giriş ve Çıktı denetimi, runbook için tanımlanan giriş parametrelerinin listesini görüntüler. Burada yeni bir giriş parametresi ekleyebilir veya varolan bir giriş parametresinin yapılandırmasını düzenlemeyapabilirsiniz. Runbook için yeni bir parametre eklemek için, [Azure Otomasyonunda Grafik yazmada](automation-graphical-authoring-intro.md)tanımlanan özellikleri kullanarak parametreleri yapılandırabileceğiniz **Runbook giriş parametre** bıçaklarını açmak için **giriş** ekle'yi tıklatın.

    ![Yeni giriş ekleme](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. **Get-AzVM** etkinliği tarafından kullanılacak aşağıdaki özelliklere sahip iki parametre oluşturun ve ardından **Tamam'ı**tıklatın.

   * Parametre 1:
        * **Adı** -- **VMName**
        * **Tür** -- String
        * **Zorunlu** -- **No**

   * Parametre 2:
        * **Ad** -- **kaynakGroupName**
        * **Tür** -- String
        * **Zorunlu** -- **No**
        * **Varsayılan değer** -- **Özel**
        * Özel varsayılan değer -- VM'leri içeren kaynak grubunun adı

5. Giriş ve Çıkış denetimindeki parametreleri görüntüleyin. 
6. **Yeniden Tamam'ı** tıklatın ve sonra **Kaydet'i**tıklatın.
7. Runbook'unuzu yayınlamak için **Yayımla'yı** tıklatın.

### <a name="configure-input-parameters-in-python-runbooks"></a>Python çalışma kitaplarında giriş parametrelerini yapılandırma

PowerShell, PowerShell İş Akışı ve grafik çalışma kitaplarının aksine, Python runbook'ları adlandırılmış parametreleri almaz. Runbook düzenleyicisi tüm giriş parametrelerini bağımsız değişken değerleri dizisi olarak ayrıştırır. **Sys** modüllerini Python komut dosyanıza aktarıp **sys.argv** dizisini kullanarak diziye erişebilirsiniz. Dizinin ilk öğesinin `sys.argv[0]`komut dosyasının adı olduğunu belirtmek önemlidir. Bu nedenle ilk gerçek giriş parametresi *sys.argv[1]* olduğunu.

Python runbook'ta giriş parametrelerinin nasıl kullanılacağına bir örnek için Azure [Otomasyonundaki Ilk Python çalışma kitabım](automation-first-runbook-textual-python2.md)bölümüne bakın.

## <a name="assigning-values-to-input-parameters-in-runbooks"></a>Runbook'larda giriş parametrelerine değer atama

Bu bölümde, çalışma kitaplarındaki giriş parametrelerine değerleri geçirmenin çeşitli yolları açıklanmaktadır. Parametre değerlerini şu anda atayabilirsiniz:

* [Runbook başlatma](#start-a-runbook-and-assign-parameters)
* [Bir runbook'u test etme](#test-a-runbook-and-assign-parameters)
* [Runbook için bir zamanlama bağlama](#link-a-schedule-to-a-runbook-and-assign-parameters)
* [Runbook için web kancası oluşturma](#create-a-webhook-for-a-runbook-and-assign-parameters)

### <a name="start-a-runbook-and-assign-parameters"></a>Runbook başlatın ve parametreleri atayın

Bir runbook birçok şekilde başlatılabilir: Azure portalı üzerinden, bir webhook ile, PowerShell cmdlets ile, REST API ile, veya SDK ile. 

#### <a name="start-a-published-runbook-using-the-azure-portal-and-assign-parameters"></a>Azure portalını kullanarak yayımlanmış bir runbook başlatın ve parametreleri atama

Azure portalında [runbook'u başlattığınızda,](start-runbooks.md#start-a-runbook-with-the-azure-portal) **Başlangıç Runbook** bıçağı açılır ve oluşturduğunuz parametreler için değerler girebilirsiniz.

![Portalı kullanmaya başlayın](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

Giriş kutusunun altındaki etikette, parametre özniteliklerini (örneğin zorunlu veya isteğe bağlı, tür, varsayılan değer) tanımlamak üzere ayarlanmış özellikleri görebilirsiniz. Parametre adının yanındaki yardım balonu, parametre giriş değerleri hakkında karar vermek için gereken temel bilgileri de tanımlar. 

> [!NOTE]
> String parametreleri String tipiboş değerleri destekler. Giriş parametresi kutusuna **[EmptyString]** girilmesi boş bir dizeyi parametreye geçirir. Ayrıca, dize parametreleri Null'u desteklemez. Herhangi bir değeri dize parametresine geçirmezseniz, PowerShell bunu Null olarak yorumlar.

#### <a name="start-a-published-runbook-using-powershell-cmdlets-and-assign-parameters"></a>PowerShell cmdlets kullanarak yayınlanmış bir runbook başlatın ve parametreleri atama

* **Azure Kaynak Yöneticisi cmdlets:** [Start-AzAutomationRunbook'u](https://docs.microsoft.com/powershell/module/Az.Automation/Start-AzAutomationRunbook?view=azps-3.5.0
)kullanarak kaynak grubunda oluşturulmuş bir Otomasyon runbook'u başlatabilirsiniz.

   ```powershell
     $params = @{"VMName"="WSVMClassic";"resourceGroupeName"="WSVMClassicSG"}
  
     Start-AzAutomationRunbook -AutomationAccountName "TestAutomation" -Name "Get-AzureVMGraphical" –ResourceGroupName $resourceGroupName -Parameters $params
   ```

* **Azure klasik dağıtım modeli cmdlets:** [Başlangıç-AzureAutomationRunbook'u](/powershell/module/servicemanagement/azure/start-azureautomationrunbook)kullanarak varsayılan kaynak grubunda oluşturulan bir otomasyon runbook'u başlatabilirsiniz.
  
   ```powershell
     $params = @{"VMName"="WSVMClassic"; "ServiceName"="WSVMClassicSG"}
  
     Start-AzureAutomationRunbook -AutomationAccountName "TestAutomation" -Name "Get-AzureVMGraphical" -Parameters $params
   ```

> [!NOTE]
> PowerShell cmdlets kullanarak bir runbook başlattığınızda, varsayılan bir parametre, *MicrosoftApplicationManagementStartedBy*, **değeri PowerShell**ile oluşturulur. Bu parametreyi İş ayrıntıları bölmesinde görüntüleyebilirsiniz.  

#### <a name="start-a-runbook-using-an-sdk-and-assign-parameters"></a>SDK kullanarak runbook başlatın ve parametreleri atama

* **Azure Kaynak Yöneticisi yöntemi:** Bir programlama dilinin SDK'sını kullanarak bir runbook başlatabilirsiniz. Aşağıda, Otomasyon hesabınızda bir runbook başlatmak için bir C# kodu snippet'i yer almaktadır. Tüm kodu [GitHub depomuzda](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs)görüntüleyebilirsiniz.  

   ```csharp
   public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
      {
        var response = AutomationClient.Jobs.Create(resourceGroupName, automationAccount, new JobCreateParameters
         {
            Properties = new JobCreateProperties
             {
                Runbook = new RunbookAssociationProperty
                 {
                   Name = runbookName
                 },
                   Parameters = parameters
             }
         });
      return response.Job;
      }
   ```

* **Azure klasik dağıtım modeli yöntemi:** Bir programlama dilinin SDK'sını kullanarak bir runbook başlatabilirsiniz. Aşağıda, Otomasyon hesabınızda bir runbook başlatmak için bir C# kodu snippet'i yer almaktadır. Tüm kodu [GitHub depomuzda](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs)görüntüleyebilirsiniz.

   ```csharp
  public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
    {
      var response = AutomationClient.Jobs.Create(automationAccount, new JobCreateParameters
    {
      Properties = new JobCreateProperties
         {
           Runbook = new RunbookAssociationProperty
         {
           Name = runbookName
              },
                Parameters = parameters
              }
       });
      return response.Job;
    }
   ```

   Bu yöntemi başlatmak için, runbook parametrelerini *VMName* ve *resourceGroupName* ve değerlerini depolamak için bir sözlük oluşturun. O zaman runbook'u başlat. Aşağıda yukarıda tanımlanan yöntemi aramak için C# kodu snippet olduğunu.

   ```csharp
   IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
   // Add parameters to the dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
   RunbookParameters.Add("resourceGroupName", "WSSC1");
  
   //Call the StartRunbook method with parameters
   StartRunbook("Get-AzureVMGraphical", RunbookParameters);
   ```

#### <a name="start-a-runbook-using-the-rest-api-and-assign-parameters"></a>REST API'sini kullanarak bir runbook başlatın ve parametreleri atayın

Aşağıdaki istek URI ile **PUT** yöntemini kullanarak Azure Automation REST API ile bir runbook işi oluşturabilir ve başlatabilirsiniz:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Automation/automationAccounts/{automationAccountName}/jobs/{jobName}?api-version=2017-05-15-preview`

Uri isteğinde aşağıdaki parametreleri değiştirin:

* *subscriptionId*: Azure abonelik kimliğiniz.  
* *resourceGroupName*: Otomasyon hesabıiçin kaynak grubunun adı.
* *automationAccountName*: Belirtilen bulut hizmeti içinde barındırılan Otomasyon hesabının adıdır.  
* *jobName*: İş için GUID. PowerShell'deki GUID'ler kullanılarak `[GUID]::NewGuid().ToString()*`oluşturulabilir.

Parametreleri runbook işine geçirmek için istek gövdesini kullanın. JSON formatında sağlanan aşağıdaki bilgileri alır:

* Runbook adı: Gerekli. İş başlatmak için runbook adı.  
* Runbook parametreleri: İsteğe bağlı. Ad Dize türü ve değeri herhangi bir geçerli JSON değeri olabilir (ad, değer) biçiminde parametre listesi sözlüğü.

*VMName* ve *resourceGroupName* ile daha önce oluşturulan **Get-AzureVMTextual** runbook'u parametre olarak başlatmak istiyorsanız, istek gövdesi için aşağıdaki JSON biçimini kullanın.

```json
    {
      "properties":{
        "runbook":{
        "name":"Get-AzureVMTextual"},
      "parameters":{
         "VMName":"WindowsVM",
         "resourceGroupName":"ContosoSales"}
        }
    }
```

İş başarıyla oluşturulursa, http durum kodu 201 döndürülür. Yanıt üstbilgileri ve yanıt gövdesi hakkında daha fazla bilgi için, [REST API'yi kullanarak bir runbook işi oluşturmaya](/rest/api/automation/job/create)bakın.

### <a name="test-a-runbook-and-assign-parameters"></a>Çalışma kitabını test edin ve parametreleri atayın

Test seçeneğini kullanarak [runbook'unuzun taslak sürümünü test](automation-testing-runbook.md) **ettiğinizde, Test** sayfası açılır. Oluşturduğunuz parametrelerin değerlerini yapılandırmak için bu sayfayı kullanın.

![Parametreleri test edin ve atayın](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-to-a-runbook-and-assign-parameters"></a>Zamanlamayı runbook'a bağlama ve parametreleri atama

Çalışma kitabının belirli bir zamanda başlaması için [bir zamanlamayı](automation-schedules.md) runbook'unuza bağlayabilirsiniz. Zamanlamayı oluştururken giriş parametrelerini atarsınız ve runbook zamanlama tarafından başlatıldığında bu değerleri kullanır. Tüm zorunlu parametre değerleri sağlanana kadar zamanlamayı kaydedemezsiniz.

![Parametreleri zamanlama ve atama](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a>Runbook için bir webhook oluşturma ve parametreler atama

Runbook'unuz için bir [webhook](automation-webhooks.md) oluşturabilir ve runbook giriş parametrelerini yapılandırabilirsiniz. Tüm zorunlu parametre değerleri sağlanana kadar webhook kaydedemez.

![Webhook oluşturma ve parametreleri atama](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

Bir runbook'u bir webhook kullanarak çalıştırdığınızda, önceden tanımlanmış giriş parametresi *[WebhookData,](automation-webhooks.md)* tanımladığınız giriş parametreleriyle birlikte gönderilir. 

![WebhookData parametresi](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="passing-a-json-object-to-a-runbook"></a>JSON nesnesini runbook'a geçirme

Bir JSON dosyasında bir runbook'a geçirmek istediğiniz verileri depolamak yararlı olabilir. Örneğin, bir runbook'a geçirmek istediğiniz tüm parametreleri içeren bir JSON dosyası oluşturabilirsiniz. Bunu yapmak için, JSON kodunu bir dize dönüştürmeniz ve ardından dizeyi runbook'a geçirmeden önce bir PowerShell nesnesine dönüştürmeniz gerekir.

Bu bölümde, PowerShell komut dosyasının Bir PowerShell runbook'u başlatmak için [Start-AzAutomationRunbook'u](https://docs.microsoft.com/powershell/module/az.automation/start-azautomationrunbook?view=azps-3.5.0) çağırdığı ve JSON dosyasının içeriğini runbook'a aktardığı bir örnek kullanır. PowerShell runbook, VM parametrelerini JSON nesnesinden alarak bir Azure VM başlatır.

### <a name="create-the-json-file"></a>JSON dosyasını oluşturma

Bir metin dosyasına aşağıdaki kodu yazın `test.json` ve yerel bilgisayarınızda bir yere kaydedin.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

### <a name="create-the-runbook"></a>Runbook'u oluşturma

Azure Otomasyonunda **Test-Json** adında yeni bir PowerShell runbook oluşturun. [Benim ilk PowerShell runbook](automation-first-runbook-textual-powershell.md)bakın.

JSON verilerini kabul etmek için runbook'un bir nesneyi giriş parametresi olarak alması gerekir. Runbook daha sonra JSON dosyasında tanımlanan özellikleri kullanabilirsiniz.

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect to Azure account
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object to actual JSON
$json = $json | ConvertFrom-Json

# Use the values from the JSON object as the parameters for your command
Start-AzVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
```

Bu runbook'u Otomasyon hesabınıza kaydedin ve yayınlayın.

### <a name="call-the-runbook-from-powershell"></a>PowerShell'den runbook'u arayın

Artık Azure PowerShell'i kullanarak yerel makinenizden runbook'u arayabilirsiniz. 

1. Gösterildiği gibi Azure'da oturum açın. Daha sonra Azure kimlik bilgilerinizi girmeniz istenir.

   ```powershell
   Connect-AzAccount
   ```

    >[!NOTE]
    >PowerShell runbook'ları için **Add-AzAccount** ve **Add-AzureRMAccount,** **Connect-AzAccount**takma adlarıdır. Bu diğer adların grafik çalışma kitapları için kullanılamadığını unutmayın. Grafik çalışma kitabı yalnızca **Connect-AzAccount'ı** kullanabilir.

1. Kaydedilen JSON dosyasının içeriğini alın ve bir dize dönüştürün. `JsonPath`JSON dosyasını kaydettiğiniz yoldur.

   ```powershell
   $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
   ```

1. String içeriğini powershell nesnesine `$json` dönüştürün.

   ```powershell
   $JsonParams = @{"json"=$json}
   ```

1. **Start-AzAutomationRunbook**parametreleri için bir karma tablo oluşturun. 

   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```

   *Parametrelerin* değerini JSON dosyasındaki değerleri içeren PowerShell nesnesine ayarladığınıza dikkat edin.
1. Runbook'u başlatın.

   ```powershell
   $job = Start-AzAutomationRunbook @RBParams
   ```

## <a name="next-steps"></a>Sonraki adımlar

* Runbook'u başlatmanın farklı yolları hakkında ayrıntılı bilgi [için](automation-starting-a-runbook.md)bkz.
* Metinsel bir runbook düzenlemek için metin [çalışma defterlerini düzenleme'ye](automation-edit-textual-runbook.md)bakın.
* Grafik çalışma kitabını yönetmek için [Azure Otomasyonu'nda Grafiksel yazarlık kitabına](automation-graphical-authoring-intro.md)bakın.
