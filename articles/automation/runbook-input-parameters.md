---
title: Runbook giriş parametreleri
description: Runbook giriş parametreleri, başlatıldığında runbook 'lara veri geçirmenize izin vererek runbook 'ların esnekliğini artırır. Bu makalede, runbook 'larda giriş parametrelerinin kullanıldığı farklı senaryolar açıklanır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 02/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: be7d244f5aa422b2083d35fc56a52318a4379b79
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850236"
---
# <a name="runbook-input-parameters"></a>Runbook giriş parametreleri

Runbook giriş parametreleri, başlatıldığında veri geçirmenize izin vererek runbook 'ların esnekliğini artırır. Parametreler, runbook eylemlerinin belirli senaryolar ve ortamlar için hedeflenmiş olmasını sağlar. Bu makalede, runbook 'larda giriş parametrelerinin kullanıldığı farklı senaryolara yol göstereceğiz.

## <a name="configure-input-parameters"></a>Giriş parametrelerini Yapılandır

Giriş parametreleri PowerShell, PowerShell Iş akışı, Python ve grafik runbook 'larda yapılandırılabilir. Bir runbook, farklı veri türlerine sahip birden fazla parametreye sahip olabilir veya hiç parametre içermez. Giriş parametreleri zorunlu veya isteğe bağlı olabilir ve isteğe bağlı parametreler için varsayılan bir değere sahip olabilirsiniz. Bir runbook için giriş parametrelerine değerleri, kullanılabilir metotlardan biri aracılığıyla başlattığınızda atayabilirsiniz. Bu yöntemler Azure portal, bir Web hizmeti veya PowerShell 'den bir runbook başlatmayı içerir. Ayrıca, başka bir runbook 'ta satır içi olarak adlandırılan bir alt runbook olarak da başlatabilirsiniz.

## <a name="configure-input-parameters-in-powershell-runbooks"></a>PowerShell runbook 'larında giriş parametrelerini yapılandırma

Azure Otomasyonu 'ndaki PowerShell ve PowerShell Iş akışı runbook 'ları, aşağıdaki öznitelikler aracılığıyla tanımlanan giriş parametrelerini destekler:  

| **Özellik** | **Açıklama** |
|:--- |:--- |
| `Type` |Gereklidir. Parametre değeri için beklenen veri türü. Herhangi bir .NET türü geçerlidir. |
| `Name` |Gereklidir. Parametrenin adı. Bu, runbook içinde benzersiz olmalıdır ve yalnızca harf, sayı veya alt çizgi karakterlerini içerebilir. Bir harfle başlamalıdır. |
| `Mandatory` |İsteğe bağlı. Parametresi için bir değer sağlanması gerekip gerekmediğini belirtir. Bunu **true\$** olarak ayarlarsanız, runbook başlatıldığında bir değer sağlanmalıdır. Bunu **yanlış\$** olarak ayarlarsanız bir değer isteğe bağlıdır. |
| `Default value` |İsteğe bağlı. Runbook başlatıldığında bir değer geçirilmemişse, parametresi için kullanılan bir değeri belirtir. Herhangi bir parametre için varsayılan bir değer ayarlanabilir ve zorunlu ayarından bağımsız olarak parametreyi otomatik olarak isteğe bağlı hale getirir. |

Windows PowerShell, giriş parametrelerinin, doğrulama, diğer adlar ve parametre kümeleri gibi burada listelenenlerden daha fazla özniteliğini destekler. Ancak, Azure Otomasyonu Şu anda yalnızca önceki giriş parametrelerini desteklemektedir.

PowerShell Iş akışı runbook 'larında bir parametre tanımı, birden fazla parametrenin virgülle ayrıldığı aşağıdaki genel biçime sahiptir.

```powershell
Param
(
  [Parameter (Mandatory= $true/$false)]
  [Type] $Name1 = <Default value>,

  [Parameter (Mandatory= $true/$false)]
  [Type] $Name2 = <Default value>
)
```

> [!NOTE]
> Parametreleri tanımlarken, **zorunlu** özniteliği belirtmezseniz, varsayılan olarak parametre isteğe bağlı olarak kabul edilir. Ayrıca, PowerShell Iş akışı runbook 'larında bir parametre için varsayılan bir değer ayarlarsanız, **zorunlu** öznitelik değerinden bağımsız olarak PowerShell tarafından isteğe bağlı bir parametre olarak değerlendirilir.

Örnek olarak, bir PowerShell Iş akışı runbook 'unun, tek bir VM veya bir kaynak grubundaki tüm VM 'Ler hakkındaki ayrıntıları çıkaran bir PowerShell Iş akışı runbook 'u için giriş parametrelerini yapılandıralim. Bu runbook 'un aşağıdaki ekran görüntüsünde gösterildiği gibi iki parametresi vardır: sanal makinenin adı ve kaynak grubunun adı.

![Otomasyon PowerShell Iş akışı](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

Bu parametre tanımında, **\$VMName** ve **\$resourcegroupname** parametreleri dize türünde basit parametrelerdir. Ancak, PowerShell ve PowerShell Iş akışı runbook 'ları, giriş parametreleri için **nesne** veya **PSCredential** gibi tüm basit türleri ve karmaşık türleri destekler.

Runbook 'da bir nesne türü giriş parametresi varsa, bir değeri geçirmek için (ad, değer) çiftleriyle bir PowerShell Hashtable kullanın. Örneğin, bir runbook 'ta aşağıdaki parametreye sahipseniz:

```powershell
[Parameter (Mandatory = $true)]
[object] $FullName
```

Ardından aşağıdaki değeri parametresine geçirebilirsiniz:

```powershell
@{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}
```

> [!NOTE]
> _Varsayılan değeri_ `\$null`olan isteğe bağlı `[String]` türü parametreye değer geçirmezseniz, parametrenin değeri `\$null`**değil** _boş bir dize_olur.

## <a name="configure-input-parameters-in-graphical-runbooks"></a>Grafik runbook 'larda giriş parametrelerini yapılandırma

Giriş parametreleriyle [bir grafik runbook 'u yapılandırmak](automation-first-runbook-graphical.md) için, sanal makineler hakkındaki ayrıntıları, tek bir VM veya bir kaynak grubu Içindeki tüm VM 'ler hakkında çıkaran bir grafik runbook oluşturalım. Bir runbook 'u yapılandırmak, aşağıda açıklandığı gibi iki önemli etkinlikten oluşur.

Azure ile kimlik doğrulamak için [**Azure farklı çalıştır hesabı Ile runbook 'Ların kimliğini doğrulayın**](automation-sec-configure-azure-runas-account.md) .

Bir sanal makinenin özelliklerini almak için [**Get-AzureRmVm**](/powershell/module/azurerm.compute/get-azurermvm) .

Sanal makinelerin adlarını çıkarmak için [**yazma çıkışı**](/powershell/module/microsoft.powershell.utility/write-output) etkinliğini kullanabilirsiniz. **Get-AzureRmVm** etkinliği iki parametreyi, **sanal makine adını** ve **kaynak grubu adını**kabul eder. Bu parametreler runbook 'u her başlattığınızda farklı değerler gerektirebilen için, runbook uygulamanıza giriş parametreleri ekleyebilirsiniz. Giriş parametrelerini ekleme adımları aşağıda verilmiştir:

1. **Runbook 'lar** dikey penceresinden grafik runbook 'u seçin ve ardından [**Düzenle**](automation-graphical-authoring-intro.md) ' ye tıklayın.
2. Runbook düzenleyicisinden giriş ve **çıkış dikey penceresini açmak için giriş ve** **Çıkış** ' a tıklayın.

   ![Otomasyon grafik runbook 'u](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)

3. **Giriş ve çıkış** dikey penceresi, runbook için tanımlanan giriş parametrelerinin bir listesini görüntüler. Bu dikey pencerede, yeni bir giriş parametresi ekleyebilir veya var olan bir giriş parametresinin yapılandırmasını düzenleyebilirsiniz. Runbook için yeni bir parametre eklemek için **giriş Ekle** ' ye tıklayarak **runbook giriş parametresi** dikey penceresini açın. Burada, aşağıdaki parametreleri yapılandırabilirsiniz:

   | **Özellik** | **Açıklama** |
   |:--- |:--- |
   | `Name` |Gereklidir. Parametrenin adı. Bu, runbook içinde benzersiz olmalıdır ve yalnızca harf, sayı veya alt çizgi karakterlerini içerebilir. Bir harfle başlamalıdır. |
   | `Description` |İsteğe bağlı. Giriş parametresinin amacı hakkında açıklama. |
   | `Type` |İsteğe bağlı. Parametre değeri için beklenen veri türü. Desteklenen parametre türleri **String**, **Int32**, **Int64**, **Decimal**, **Boolean**, **DateTime**ve **Object**. Bir veri türü seçilmezse, varsayılan olarak **dize**olur. |
   | `Mandatory` |İsteğe bağlı. Parametresi için bir değer sağlanması gerekip gerekmediğini belirtir. **Evet**' i seçerseniz, runbook başlatıldığında bir değer sağlanmalıdır. **Hayır**' ı seçerseniz, runbook başlatıldığında bir değer gerekli değildir ve varsayılan bir değer ayarlanabilir. |
   | `Default Value` |İsteğe bağlı. Runbook başlatıldığında bir değer geçirilmemişse, parametresi için kullanılan bir değeri belirtir. Zorunlu olmayan bir parametre için varsayılan değer ayarlanabilir. Varsayılan bir değer ayarlamak için **özel**' i seçin. Bu değer, runbook başlatıldığında başka bir değer sağlanmadığı takdirde kullanılır. Herhangi bir varsayılan değer sağlamak istemiyorsanız **hiçbiri** ' ni seçin. |

    ![Yeni giriş Ekle](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. **Get-AzureRmVm** etkinliği tarafından kullanılan aşağıdaki özelliklerle iki parametre oluşturun:

   * **Parametre1**
     * Ad-VMName
     * Tür dizesi
     * Zorunlu-Hayır
   * **Parametre2**
     * Ad-resourceGroupName
     * Tür dizesi
     * Zorunlu-Hayır
     * Varsayılan değer-özel
     * Özel varsayılan değer-sanal makineleri içeren kaynak grubunun \<adı\>

5. Parametreleri ekledikten sonra, **Tamam**' a tıklayın. Artık bunları **giriş ve çıkış sayfasında**görüntüleyebilirsiniz. Yeniden **Tamam** ' a tıklayın ve ardından **Kaydet** ' e tıklayın ve Runbook 'unuzu **yayımlayın** .

## <a name="configure-input-parameters-in-python-runbooks"></a>Python runbook 'larda giriş parametrelerini yapılandırma

PowerShell, PowerShell Iş akışı ve grafik runbook 'larının aksine, Python runbook 'ları adlandırılmış parametreler almaz.
Tüm giriş parametreleri bağımsız değişken değerleri dizisi olarak ayrıştırılır.
`sys` modülünü Python betiğe içeri aktararak ve sonra `sys.argv` dizisini kullanarak diziye erişirsiniz.
`sys.argv[0]`, dizinin ilk öğesinin betiğin adı olduğunu ve bu nedenle ilk gerçek giriş parametresinin `sys.argv[1]`olduğunu unutmayın.

Bir Python runbook 'unda giriş parametrelerinin nasıl kullanılacağına ilişkin bir örnek için bkz. [Azure Automation 'da Ilk Python runbook 'Um](automation-first-runbook-textual-python2.md).

## <a name="assign-values-to-input-parameters-in-runbooks"></a>Runbook 'larda giriş parametrelerine değerler atama

Aşağıdaki senaryolarda runbook 'larda girdi parametrelerine değerler geçirebilirsiniz:

### <a name="start-a-runbook-and-assign-parameters"></a>Bir runbook 'u başlatma ve parametreleri atama

Bir runbook birçok yolla başlatılabilir: Azure portal, bir Web kancası ile, PowerShell cmdlet 'leri, REST API veya SDK ile birlikte kullanılabilir. Aşağıda, bir runbook 'u başlatmak ve parametre atamak için farklı yöntemler tartışıyoruz.

#### <a name="start-a-published-runbook-by-using-the-azure-portal-and-assign-parameters"></a>Azure portal ve parametreleri ata ' yı kullanarak yayımlanmış bir runbook başlatma

Runbook 'u [başlattığınızda](start-runbooks.md#start-a-runbook-with-the-azure-portal) **runbook 'u Başlat** dikey penceresi açılır ve oluşturduğunuz parametrelere ilişkin değerleri girebilirsiniz.

![Portalı kullanmaya başlayın](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

Giriş kutusunun altındaki etikette parametresi için ayarlanmış olan öznitelikleri görebilirsiniz. Öznitelikler zorunlu veya isteğe bağlı, tür ve varsayılan değer içerir. Parametre adının yanındaki yardım balonunda parametre giriş değerleriyle ilgili kararlar almak için ihtiyacınız olan tüm anahtar bilgilerini görebilirsiniz. Bu bilgiler, bir parametrenin zorunlu veya isteğe bağlı olup olmadığını içerir. Ayrıca, türü ve varsayılan değeri (varsa) ve diğer yararlı notları da içerir.

> [!NOTE]
> Dize türü parametreleri **boş** dize değerlerini destekler.  Giriş parametresi kutusuna **[emptyString]** girilmesi, parametreye boş bir dize geçirir. Ayrıca, dize tür parametreleri geçilen **null** değerleri desteklemez. Dize parametresine herhangi bir değer geçirmezseniz, PowerShell onu null olarak yorumlar.

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a>PowerShell cmdlet 'lerini kullanarak yayımlanmış bir runbook 'u başlatma ve parametreleri atama

* **Azure Resource Manager cmdlet 'leri:** [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook)kullanarak bir kaynak grubunda oluşturulan bir Otomasyon Runbook 'unu başlatabilirsiniz.
  
  **Örnek:**

  ```powershell
  $params = @{"VMName"="WSVMClassic";"resourceGroupeName"="WSVMClassicSG"}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName "TestAutomation" -Name "Get-AzureVMGraphical" –ResourceGroupName $resourceGroupName -Parameters $params
  ```

* **Azure klasik dağıtım modeli cmdlet 'leri:** [Start-AzureAutomationRunbook](/powershell/module/servicemanagement/azure/start-azureautomationrunbook)kullanarak, varsayılan bir kaynak grubunda oluşturulmuş bir Otomasyon Runbook 'unu başlatabilirsiniz.
  
  **Örnek:**

  ```powershell
  $params = @{"VMName"="WSVMClassic"; "ServiceName"="WSVMClassicSG"}
  
  Start-AzureAutomationRunbook -AutomationAccountName "TestAutomation" -Name "Get-AzureVMGraphical" -Parameters $params
  ```

> [!NOTE]
> PowerShell cmdlet 'lerini kullanarak bir runbook 'u başlattığınızda, varsayılan bir parametre olan **Microsoftapplicationmanagementstartedby** , **PowerShell**değeriyle oluşturulur. Bu parametreyi **iş ayrıntıları** sayfasında görüntüleyebilirsiniz.  

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a>SDK kullanarak bir runbook 'u başlatma ve parametreleri atama

* **Azure Resource Manager yöntemi:** Bir programlama dilinin SDK 'sını kullanarak bir runbook 'u başlatabilirsiniz. Otomasyon hesabınızda runbook C# başlatmaya yönelik bir kod parçacığı aşağıda verilmiştir. [GitHub deponuzdaki](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs)tüm kodu görüntüleyebilirsiniz.  

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

* **Azure klasik dağıtım modeli yöntemi:** Bir programlama dilinin SDK 'sını kullanarak bir runbook 'u başlatabilirsiniz. Otomasyon hesabınızda runbook C# başlatmaya yönelik bir kod parçacığı aşağıda verilmiştir. [GitHub deponuzdaki](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs)tüm kodu görüntüleyebilirsiniz.

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

  Bu yöntemi başlatmak için Runbook parametreleri, **VMName** ve **resourcegroupname**ve bunların değerlerini depolamak üzere bir sözlük oluşturun. Ardından runbook 'u başlatın. Yukarıda tanımlanan yöntemi C# çağırmak için kod parçacığı aşağıda verilmiştir.

  ```csharp
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters to the dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call the StartRunbook method with parameters
  StartRunbook("Get-AzureVMGraphical", RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-the-rest-api-and-assign-parameters"></a>REST API ve parametre ata parametrelerini kullanarak bir runbook 'u başlatma

Aşağıdaki istek URI 'SI ile **PUT** yöntemi kullanılarak Azure Otomasyonu REST API birlikte bir runbook işi oluşturulup başlatılabilir: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Automation/automationAccounts/{automationAccountName}/jobs/{jobName}?api-version=2017-05-15-preview`


İstek URI 'sinde, aşağıdaki parametreleri değiştirin:

* **abonelik kimliği:** Azure abonelik KIMLIĞINIZ.  
* **Resourcegroupname:** Otomasyon hesabı için kaynak grubunun adı.
* **Automationaccountname:** Belirtilen bulut hizmeti içinde barındırılan Otomasyon hesabınızın adı.  
* **JobName:** İş için GUID. PowerShell 'deki GUID 'Ler **[GUID]:: NewGuid () kullanılarak oluşturulabilir. ToString ()** komutu.

Parametreleri runbook işine geçirmek için istek gövdesini kullanın. JSON biçiminde belirtilen aşağıdaki iki özelliği alır:

* **Runbook adı:** Gerekli. Başlatılacak iş için Runbook 'un adı.  
* **Runbook parametreleri:** Seçim. Adın dize türünde olması gereken (ad, değer) biçimdeki parametre listesinin bir sözlüğü ve değer geçerli bir JSON değeri olabilir.

Daha önce **VMName** ve **resourcegroupname** Ile oluşturulmuş **Get-AzureVMTextual** runbook 'unu parametre olarak başlatmak ISTIYORSANıZ, istek gövdesi için aşağıdaki JSON biçimini kullanın.

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

İş başarıyla oluşturulduysa 201 HTTP durum kodu döndürülür. Yanıt üstbilgileri ve yanıt gövdesi hakkında daha fazla bilgi için, [REST API kullanarak bir runbook işi oluşturma](/rest/api/automation/job/create) hakkında makalesine bakın.

### <a name="test-a-runbook-and-assign-parameters"></a>Runbook 'u test etme ve parametreleri atama

Test seçeneğini kullanarak [runbook 'un taslak sürümünü test](automation-testing-runbook.md) ettiğinizde, **Test** sayfası açılır ve oluşturduğunuz parametrelerin değerlerini yapılandırabilirsiniz.

![Test ve atama parametreleri](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-to-a-runbook-and-assign-parameters"></a>Bir runbook 'a zamanlama bağlama ve parametreleri atama

Runbook 'un belirli bir zamanda başlaması için [bir zamanlamayı](automation-schedules.md) runbook 'una bağlayabilirsiniz. Zamanlamayı oluştururken giriş parametrelerini atarsınız ve runbook, zamanlama tarafından başlatıldığında bu değerleri kullanır. Tüm zorunlu parametre değerleri sağlanana kadar zamanlamayı kaydedemezsiniz.

![Parametreleri zamanlayın ve atayın](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a>Runbook için Web kancası oluşturma ve parametreleri atama

Runbook 'larınız için bir [Web kancası](automation-webhooks.md) oluşturabilir ve Runbook giriş parametrelerini yapılandırabilirsiniz. Tüm zorunlu parametre değerleri sağlanana kadar Web kancasını kaydedemezsiniz.

![Web kancası oluşturma ve parametreleri atama](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

Bir Web kancasını kullanarak bir runbook yürüttüğünüzde, önceden tanımlanmış giriş parametresi **[Web](automation-webhooks.md#details-of-a-webhook)** kancasına gönderilir ve tanımladığınız giriş parametreleri birlikte gönderilir. Daha fazla ayrıntı için **Web Kancaverisi** parametresini tıklayabilirsiniz.

![Web kancası verileri parametresi](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="pass-a-json-object-to-a-runbook"></a>JSON nesnesini runbook 'a geçirme

Bir JSON dosyasındaki runbook 'a geçirmek istediğiniz verileri depolamak yararlı olabilir.
Örneğin, bir runbook 'a geçirmek istediğiniz tüm parametreleri içeren bir JSON dosyası oluşturabilirsiniz. Bunu yapmak için, JSON 'ı bir dizeye dönüştürmeniz ve sonra, runbook 'a geçirilmeden önce dizeyi bir PowerShell nesnesine dönüştürmeniz gerekir.

Bu örnekte, bir PowerShell runbook 'u başlatmak için [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) ' u çağıran bir PowerShell betiğiniz var ve JSON içeriğini runbook 'a geçirerek.
PowerShell runbook 'u, geçirilen JSON 'dan VM 'nin parametrelerini alarak bir Azure VM 'yi başlatır.

### <a name="create-the-json-file"></a>JSON dosyası oluşturma

Aşağıdaki testi bir metin dosyasına yazın ve yerel bilgisayarınızda bir yere `test.json` olarak kaydedin.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

### <a name="create-the-runbook"></a>Runbook 'u oluşturma

Azure Otomasyonu 'nda "test-JSON" adlı yeni bir PowerShell runbook 'u oluşturun.
Yeni bir PowerShell runbook 'u oluşturmayı öğrenmek için bkz. [Ilk PowerShell runbook 'Um](automation-first-runbook-textual-powershell.md).

JSON verilerini kabul etmek için, runbook bir nesneyi giriş parametresi olarak almalıdır.

Runbook daha sonra JSON içinde tanımlanan özellikleri kullanabilir.

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect to Azure account
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object to actual JSON
$json = $json | ConvertFrom-Json

# Use the values from the JSON object as the parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
```

Bu runbook 'u Otomasyon hesabınızda kaydedin ve yayımlayın.

### <a name="call-the-runbook-from-powershell"></a>Runbook 'u PowerShell 'den çağırma

Artık Azure PowerShell kullanarak runbook 'u yerel makinenizden çağırabilirsiniz.
Aşağıdaki PowerShell komutlarını çalıştırın:

1. Azure 'da oturum açın:

   ```powershell
   Connect-AzureRmAccount
   ```

   Azure kimlik bilgilerinizi girmeniz istenir.

   > [!IMPORTANT]
   > **Add-azurermaccount** artık **Connect-azurermaccount**için bir diğer addır. Kitaplık öğelerinizi ararken, **Connect-AzureRMAccount**' u görmüyorsanız **Add-azurermaccount**komutunu kullanabilir veya bir Otomasyon hesabınızda modüllerinizi güncelleştirebilirsiniz.

1. JSON dosyasının içeriğini alın ve bir dizeye dönüştürün:

   ```powershell
   $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
   ```

   `JsonPath` JSON dosyasını kaydettiğiniz yoldur.

1. `$json` dize içeriğini bir PowerShell nesnesine Dönüştür:

   ```powershell
   $JsonParams = @{"json"=$json}
   ```

1. `Start-AzureRmAutomationRunbook`parametreler için bir Hashtable oluşturun:

   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```

   `Parameters` değerini, JSON dosyasındaki değerleri içeren PowerShell nesnesine ayarlamadığınızı unutmayın.
1. Runbook’u başlatma

   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

## <a name="next-steps"></a>Sonraki adımlar

* Runbook 'u başlatmak için farklı yollar hakkında daha fazla bilgi için bkz. [runbook 'U başlatma](automation-starting-a-runbook.md).
* Bir metinsel runbook 'u düzenlemek için, [metin runbook 'Larını düzenleme](automation-edit-textual-runbook.md)bölümüne bakın.
* Grafik bir runbook 'u düzenlemek için bkz. [Azure Otomasyonu 'Nda grafik yazma](automation-graphical-authoring-intro.md).
