---
title: Azure Otomasyonu runbook'unu web kancasıyla başlatma
description: Bir istemcinin Azure Otomasyonu'nda bir runbook'u http çağrısından başlatmasına olanak tanıyan bir webhook.  Bu makalede, bir webhook oluşturmak ve nasıl bir runbook başlatmak için çağrı açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 01/16/2020
ms.topic: conceptual
ms.openlocfilehash: 8cb641f95e7327e80f42df86a56eba8c34e7e598
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367032"
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a>Azure Otomasyonu runbook'unu web kancasıyla başlatma

Webhook, harici bir hizmetin azure otomasyonunda belirli bir runbook'u tek bir HTTP isteği yle başlatmasına olanak tanır. Dış hizmetler arasında Azure DevOps Hizmetleri, GitHub, Azure Monitor günlükleri ve özel uygulamalar yer almaktadır. Böyle bir hizmet, Azure Otomasyon API'sini kullanarak tam bir çözüm uygulamadan bir runbook başlatmak için bir webhook kullanabilir. Azure Otomasyonu'nda [runbook Başlatma'da](automation-starting-a-runbook.md)web hooks'ları runbook başlatmanın diğer yöntemleriyle karşılaştırabilirsiniz.

> [!NOTE]
> Python runbook başlatmak için bir webhook kullanma desteklenmez.

![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="webhook-properties"></a>Webhook özellikleri

Aşağıdaki tabloda, bir webhook için yapılandırmanız gereken özellikler açıklanmaktadır.

| Özellik | Açıklama |
|:--- |:--- |
| Adı |Webhook'un adı. İstemciye açık olmadığından, istediğiniz adı sağlayabilirsiniz. Yalnızca Azure Otomasyonu'ndaki runbook'u tanımlamak için kullanılır. En iyi uygulama olarak, webhook kullanan istemci ile ilgili bir ad vermelidir. |
| URL'si |Webhook URL'si. Bu, bir istemcinin webhook'a bağlı runbook'u başlatmak için bir HTTP POST ile aradığı benzersiz adrestir. Webhook'u oluşturduğunuzda otomatik olarak oluşturulur. Özel bir URL belirtemezsiniz. <br> <br> URL, üçüncü taraf bir sistemin daha fazla kimlik doğrulaması olmadan runbook'u çağırmasına olanak tanıyan bir güvenlik belirteci içerir. Bu nedenle, URL'ye parola gibi davranmalısınız. Güvenlik nedeniyle, URL'yi yalnızca web hook oluştururken Azure portalında görüntüleyebilirsiniz. URL'yi ileride kullanmak üzere güvenli bir konuma not edin. |
| Son kullanma tarihi | Webhook'un son kullanma tarihi, bundan sonra artık kullanılamaz. Webhook'un süresi dolmamış sayılsa da, webhook oluşturulduktan sonra son kullanma tarihini değiştirebilirsiniz. |
| Etkin | Web hook oluşturulduğunda varsayılan olarak etkin olup olmadığını belirten ayarı. Bu özelliği Devre Dışı Olarak ayarlarsanız, hiçbir istemci webhook'u kullanamaz. Bu özelliği, webhook'u oluşturduğunuzda veya oluşturulduktan sonra başka bir zamanda ayarlayabilirsiniz. |

## <a name="parameters-used-when-the-webhook-starts-a-runbook"></a>Webhook bir runbook başlatTığında kullanılan parametreler

Webhook, runbook başlatıldığında kullanılan runbook parametreleri için değerleri tanımlayabilir. Webhook herhangi bir zorunlu runbook parametreleri için değerleri içermelidir ve isteğe bağlı parametreler için değerleri içerebilir. Webhook'a yapılandırılan bir parametre değeri, webhook oluşturulduktan sonra bile değiştirilebilir. Tek bir runbook'a bağlı birden çok web hook'un her biri farklı runbook parametresi değerlerini kullanabilir. İstemci bir webhook kullanarak bir runbook başlattığında, webhook'ta tanımlanan parametre değerlerini geçersiz kılabilir.

İstemciden veri almak için runbook , `WebhookData`. Bu parametre, istemcinin POST isteğinde içerdiği verileri içeren bir nesneyi tanımlar.

![WebhookData özellikleri](media/automation-webhooks/webhook-data-properties.png)

`WebhookData` Parametre aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama |
|:--- |:--- |
| `WebhookName` | Webhook'un adı. |
| `RequestHeader` | Gelen POST isteğinin üstbilgilerini içeren karma tablo. |
| `RequestBody` | Gelen POST isteğinin gövdesi. Bu gövde dize, JSON, XML veya form kodlanmış gibi herhangi bir veri biçimlendirmesini korur. Runbook beklenen veri biçimi ile çalışmak için yazılmalıdır. |

`WebhookData` Parametreyi desteklemek için webhook'un yapılandırması gerekmez ve runbook'un bunu kabul etmesi gerekmez. Runbook parametreyi tanımlamazsa, istemciden gönderilen isteğin tüm ayrıntıları yoksayılır.

> [!NOTE]
> Bir webhook'u ararken, istemci her zaman aramanın başarısız olması durumunda parametre değerlerini depolamalıdır. Ağ kesintisi veya bağlantı sorunu varsa, uygulama başarısız webhook çağrılarını alamaz.

Webhook `WebhookData` oluşturmada bir değer belirtirseniz, webhook runbook'u istemci POST isteğindeki verilerle başlattığında geçersiz kılınz. Uygulama istek gövdesinde herhangi bir veri içermese bile bu durum gerçekleşir. 

Webhook dışında bir mekanizma `WebhookData` kullanarak tanımlayan bir runbook başlatıyorsanız, runbook'un tanıdığı bir değer `WebhookData` sağlayabilirsiniz. Bu değer, çalışma kitabının [properties](#webhook-properties) bir `WebhookData` webhook tarafından geçirilen gerçek `WebhookData` nesnelerle çalıştığı gibi onunla çalışabilmesi için parametreyle aynı özelliklere sahip bir nesne olmalıdır.

Örneğin, Azure portalından aşağıdaki runbook'u başlatıyorsanız ve test için bazı örnek webhook verilerini geçirmek istiyorsanız, kullanıcı arabirimindeki JSON'daki verileri geçirmeniz gerekir.

![UI'den WebhookData parametresi](media/automation-webhooks/WebhookData-parameter-from-UI.png)

Bir sonraki runbook örneği için `WebhookData`aşağıdaki özellikleri tanımlayalım:

* **WebhookName**: MyWebhook
* **İstek Gövdesi**:`*[{'ResourceGroup': 'myResourceGroup','Name': 'vm01'},{'ResourceGroup': 'myResourceGroup','Name': 'vm02'}]*`

Şimdi parametre için UI aşağıdaki JSON `WebhookData` nesne geçmek. Bu örnek, satır başları ve yeni satır karakterleri ile, bir webhook geçirilen biçimeşleştirilir.

```json
{"WebhookName":"mywebhook","RequestBody":"[\r\n {\r\n \"ResourceGroup\": \"vm01\",\r\n \"Name\": \"vm01\"\r\n },\r\n {\r\n \"ResourceGroup\": \"vm02\",\r\n \"Name\": \"vm02\"\r\n }\r\n]"}
```

![WebhookData parametreyi UI'dan başlatın](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> Azure Otomasyonu, runbook işiyle tüm giriş parametrelerinin değerlerini günlüğe kaydeder. Böylece webhook isteği istemci tarafından sağlanan herhangi bir giriş günlüğe kaydedilir ve otomasyon iş erişimi olan herkes için kullanılabilir. Bu nedenle, webhook aramalarında hassas bilgileri dahil etme konusunda dikkatli olmalısınız.

## <a name="webhook-security"></a>Webhook güvenliği

Bir webhook'un güvenliği, webhook'un çağrılmasını sağlayan bir güvenlik belirteci içeren URL'sinin gizliliğine dayanır. Azure Otomasyonu, doğru URL'ye yapıldığı sürece istek üzerinde herhangi bir kimlik doğrulaması gerçekleştirmez. Bu nedenle, müşterileriniz isteği doğrulamak için alternatif bir araç kullanmadan son derece hassas işlemler gerçekleştiren runbook'lar için webhooks kullanmamalıdır.

Bir webhook tarafından çağrılıp çağrılmasını belirlemek için bir runbook içinde mantık ekleyebilirsiniz. Runbook'un parametrenin `WebhookName` `WebhookData` özelliğini kontrol edin. Runbook `RequestHeader` ve `RequestBody` özellikleri belirli bilgileri arayarak daha fazla doğrulama gerçekleştirebilirsiniz.

Başka bir strateji, runbook bir webhook isteği aldığında harici bir durumun bazı doğrulama gerçekleştirmek için. Örneğin, GitHub deposuna yeni bir taahhüt olduğunda GitHub tarafından çağrılan bir runbook düşünün. Runbook, devam etmeden önce yeni bir işlemenin oluştuğunu doğrulamak için GitHub'a bağlanabilir.

## <a name="creating-a-webhook"></a>Webhook oluşturma

Azure portalındaki bir runbook'a bağlı yeni bir webhook oluşturmak için aşağıdaki yordamı kullanın.

1. Azure portalındaki RunBook sayfasından, webhook'un runbook ayrıntılarını görüntülemeye başladığı runbook'u tıklatın. Runbook **Durum** alanının **Yayımlanmış**olarak ayarlandığından emin olun.
2. Webhook Ekle sayfasını açmak için sayfanın üst kısmındaki **Webhook'u** tıklatın.
3. Webhook oluştur sayfasını açmak için **yeni webhook Oluştur'u** tıklatın.
4. Webhook için **Ad** ve **Son Kullanma Tarihi** alanlarını doldurun ve etkinleştirilmesi gerektirebilip etkinleştirilmemesi gerektiğini belirtin. Bu özellikler hakkında daha fazla bilgi için [Webhook özelliklerine](#webhook-properties) bakın.
5. Kopya simgesine tıklayın ve webhook'un URL'sini kopyalamak için Ctrl+C tuşuna basın. O zaman güvenli bir yere kaydet. 

    > [!NOTE]
    > Webhook'u oluşturduktan sonra URL'yi yeniden alamazsınız.

   ![Webhook URL](media/automation-webhooks/copy-webhook-url.png)

1. Runbook parametreleri için değerler sağlamak için **Parametreler'i** tıklatın. Runbook'un zorunlu parametreleri varsa, değerler sağlamadığınız sürece webhook'u oluşturamazsınız.
1. Web kancasını oluşturmak için **Oluştur**’a tıklayın.

## <a name="using-a-webhook"></a>Webhook kullanma

Oluşturulduktan sonra bir webhook kullanmak için istemciniz `POST` webhook için URL ile bir HTTP isteği yayımlamak gerekir. Söz dizimi aşağıdaki gibidir:

```http
http://<Webhook Server>/token?=<Token Value>
```

İstemci `POST` istekten aşağıdaki iade kodlarından birini alır.

| Kod | Metin | Açıklama |
|:--- |:--- |:--- |
| 202 |Accepted |İstek kabul edildi ve runbook başarıyla sıraya alındı. |
| 400 |Hatalı İstek |İstek aşağıdaki nedenlerden biri için kabul edilmedi: <ul> <li>Webhook'un süresi doldu.</li> <li>Webhook devre dışı bırakılır.</li> <li>URL'deki belirteç geçersizdir.</li>  </ul> |
| 404 |Bulunamadı |İstek aşağıdaki nedenlerden biri için kabul edilmedi: <ul> <li>Webhook bulunamadı.</li> <li>Runbook bulunamadı.</li> <li>Hesap bulunamadı.</li>  </ul> |
| 500 |Dahili Sunucu Hatası |URL geçerliydi, ancak bir hata oluştu. Lütfen isteği yeniden gönderin. |

İsteğin başarılı olduğunu varsayarsak, webhook yanıtı aşağıda gösterildiği gibi JSON formatında iş kimliği içerir. Tek bir iş kimliği içerir, ancak JSON biçimi gelecekteki olası geliştirmelere izin verir.

```json
{"JobIds":["<JobId>"]}
```

İstemci, runbook işinin ne zaman tamamlaşabileceğini veya webhook'tan tamamlanma durumunu belirleyemez. Bu bilgileri iş kimliğini [kullanarak Windows PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationjob) veya Azure Otomasyon [API](/rest/api/automation/job)gibi başka bir mekanizmayla bulabilir.

## <a name="renewing-a-webhook"></a><a name="renew-webhook"></a>Web hook yenileme

Bir webhook oluşturulduğunda, on yıllık bir geçerlilik süresi vardır ve bu süre otomatik olarak sona erer. Bir webhook'un süresi dolduktan sonra, yeniden etkinleştiremezsiniz. Yalnızca kaldırAbilir ve sonra yeniden oluşturabilirsiniz. 

Son kullanma süresine ulaşmamış bir webhook'u uzatabilirsiniz. Bir webhook genişletmek için:

1. Webhook içeren runbook gidin. 
2. **Kaynaklar**altında **Webhooks'u** seçin. 
3. Uzatmak istediğiniz webhook'u tıklatın. 
4. Webhook sayfasında, yeni bir son kullanma tarihi ve saati seçin ve **Kaydet'i**tıklatın.

## <a name="sample-runbook"></a>Örnek runbook

Aşağıdaki örnek runbook webhook verilerini kabul eder ve istek gövdesinde belirtilen sanal makineleri başlatır. Runbook'lar altındaki Otomasyon hesabınızda **Runbooks**bu runbook'u test etmek için **runbook oluştur'u**tıklatın. Runbook oluşturmayı bilmiyorsanız, [runbook oluşturma'ya](automation-quickstart-create-runbook.md)bakın.

> [!NOTE]
> Grafiksel olmayan PowerShell runbook'ları için `Add-AzAccount` ve `Add-AzureRMAccount` [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0)için diğer adlardır. Bu cmdlets'i kullanabilir veya Otomasyon hesabınızdaki [modüllerinizi](automation-update-azure-modules.md) en son sürümlere güncelleyebilirsiniz. Yeni bir Otomasyon hesabı oluşturmuş olsanız bile modüllerinizi güncellemeniz gerekebilir.

```powershell
param
(
    [Parameter (Mandatory = $false)]
    [object] $WebhookData
)

# If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData) {

    # Check header for message to validate request
    if ($WebhookData.RequestHeader.message -eq 'StartedbyContoso')
    {
        Write-Output "Header has required information"}
    else
    {
        Write-Output "Header missing required information";
        exit;
    }

    # Retrieve VMs from Webhook request body
    $vms = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Authenticate to Azure by using the service principal and certificate. Then, set the subscription.

    Write-Output "Authenticating to Azure with service principal and certificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    Write-Output "Get connection asset: $ConnectionAssetName"

    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null)
            {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Write-Output "Authenticating to Azure with service principal."
            Add-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Output

        # Start each virtual machine
        foreach ($vm in $vms)
        {
            $vmName = $vm.Name
            Write-Output "Starting $vmName"
            Start-AzVM -Name $vm.Name -ResourceGroup $vm.ResourceGroup
        }
}
else {
    # Error
    write-Error "This runbook is meant to be started from an Azure alert webhook only."
}
```

## <a name="testing-the-sample"></a>Numuneyi test etme

Aşağıdaki örnekte, web hook ile bir runbook başlatmak için Windows PowerShell kullanır. HTTP isteği nde bulunabilecek herhangi bir dil webhook kullanabilir. Windows PowerShell burada örnek olarak kullanılır.

Runbook istek gövdesinde JSON biçimlendirilmiş sanal makinelerin bir listesini bekliyor. Runbook, üstbilginin webhook arayan'ın geçerli olduğunu doğrulamak için tanımlanmış bir ileti içerdiğini de doğrular.

```azurepowershell-interactive
$uri = "<webHook Uri>"

$vms  = @(
            @{ Name="vm01";ResourceGroup="vm01"},
            @{ Name="vm02";ResourceGroup="vm02"}
        )
$body = ConvertTo-Json -InputObject $vms
$header = @{ message="StartedbyContoso"}
$response = Invoke-WebRequest -Method Post -Uri $uri -Body $body -Headers $header
$jobid = (ConvertFrom-Json ($response.Content)).jobids[0]
```

Aşağıdaki örnek, runbook'un özelliğinde kullanılabilen isteğin `RequestBody` gövdesini `WebhookData`gösterir. Bu değer, isteğin gövdesinde yer alan biçimle uyumlu olacak şekilde JSON olarak biçimlendirilir.

```json
[
    {
        "Name":  "vm01",
        "ResourceGroup":  "myResourceGroup"
    },
    {
        "Name":  "vm02",
        "ResourceGroup":  "myResourceGroup"
    }
]
```

Aşağıdaki resim, Windows PowerShell'den gönderilen isteği ve ortaya çıkan yanıtı gösterir. İş kimliği yanıttan çıkarılır ve bir dize dönüştürülür.

![Webhooks düğmesi](media/automation-webhooks/webhook-request-response.png)

## <a name="next-steps"></a>Sonraki adımlar

* Azure uyarılarında işlem yapmak için Azure Otomasyonu'nu nasıl kullanacağınızı öğrenmek için [bkz.](automation-create-alert-triggered-runbook.md)
