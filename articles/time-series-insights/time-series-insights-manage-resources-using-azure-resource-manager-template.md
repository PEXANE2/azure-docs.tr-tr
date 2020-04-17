---
title: Azure Kaynak Yöneticisi şablonlarını kullanarak ortamınızı yönetin - Azure Zaman Serisi Öngörüleri | Microsoft Dokümanlar
description: Azure Kaynak Yöneticisi'ni kullanarak Azure Time Series Öngörüleri ortamınızı programlı olarak nasıl yöneteceklerinizi öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/16/2020
ms.custom: seodec18
ms.openlocfilehash: a670e32058794daeaa233464ba7d054f45ef25e3
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536327"
---
# <a name="create-time-series-insights-resources-using-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarını kullanarak Zaman Serisi Öngörüleri kaynakları oluşturun

Bu makalede, [Azure Kaynak Yöneticisi şablonları,](https://docs.microsoft.com/azure/azure-resource-manager/)PowerShell ve Time Series Insights kaynak sağlayıcısını kullanarak Time Series Insights kaynaklarının nasıl oluşturulup dağıtılanınca açıklanmaktadır.

Time Series Insights aşağıdaki kaynakları destekler:

   | Kaynak | Açıklama |
   | --- | --- |
   | Ortam | Zaman Serisi Öngörüleri ortamı, olay aracılarından okunan, depolanan ve sorgu için kullanılabilir hale getirilen olayların mantıksal bir gruplandırmasIdır. Daha fazla bilgi için [Azure Zaman Serisi Öngörüleri ortamınızı planlayın'ı](time-series-insights-environment-planning.md) okuyun |
   | Olay Kaynağı | Etkinlik kaynağı, Time Series Insights'ın olayları okuduğu ve ortama yutturdığı bir etkinlik aracısına bağlantıdır. Şu anda desteklenen olay kaynakları IoT Hub ve Olay Hub'ıdır. |
   | Referans Veri Seti | Başvuru veri kümeleri, ortamdaki olaylar hakkında meta veriler sağlar. Başvuru veri kümelerinde meta veriler giriş sırasında olaylarla birleştirilir. Başvuru veri kümeleri, olay anahtar özelliklerine göre kaynak olarak tanımlanır. Başvuru veri kümesini oluşturan gerçek meta veriler veri düzlemi API'leri aracılığıyla yüklenir veya değiştirilir. |
   | Erişim Politikası | Erişim ilkeleri, veri sorguları verme, ortamdaki başvuru verilerini işleme ve kaydedilen sorguları ve ortamla ilişkili perspektifleri paylaşma izni verir. Daha fazla bilgi için [Azure portalLarını kullanarak Time Series Öngörüleri ortamına veri erişimi ver'i](time-series-insights-data-access.md) okuyun |

Kaynak Yöneticisi şablonu, kaynak grubundaki kaynakların altyapısını ve yapılandırmasını tanımlayan bir JSON dosyasıdır. Aşağıdaki belgeler şablon dosyalarını daha ayrıntılı olarak açıklar:

- [Azure Kaynak Yöneticisi şablon dağıtımı](../azure-resource-manager/templates/overview.md)
- [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/templates/deploy-powershell.md)
- [Microsoft.TimeSeriesInsights kaynak türleri](/azure/templates/microsoft.timeseriesinsights/allversions)

[201-timeseriesinsights-environment-with-eventhub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-timeseriesinsights-environment-with-eventhub) quickstart şablonu GitHub'da yayınlanır. Bu şablon, Bir Zaman Serisi Öngörüleri ortamı, olay hub'ından olayları tüketecek şekilde yapılandırılan bir alt olay kaynağı ve ortamın verilerine erişim sağlayan ilkelere erişim oluşturur. Varolan bir Olay Hub'ı belirtilmemişse, dağıtımla birlikte bir tane oluşturulur.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="specify-deployment-template-and-parameters"></a>Dağıtım şablonu ve parametrelerini belirtin

Aşağıdaki yordam, Zaman Serisi Öngörüleri ortamını oluşturan bir Azure Kaynak Yöneticisi şablonu, olay merkezindeki olayları tüketecek şekilde yapılandırılan bir alt olay kaynağı ve ortamın verilerine erişim sağlayan ilkelere erişmek için PowerShell'in nasıl kullanılacağıaçıklanmaktadır. Varolan bir Olay Hub'ı belirtilmemişse, dağıtımla birlikte bir tane oluşturulur.

1. Azure PowerShell ile başlarken yönergeleri izleyerek [Azure PowerShell'i yükleyin.](https://docs.microsoft.com/powershell/azure/get-started-azureps)

1. GitHub'dan [201-timeseriesinsights-environment-with-eventhub](https://github.com/Azure/azure-quickstart-templates/blob/master/201-timeseriesinsights-environment-with-eventhub/azuredeploy.json) şablonunu klonlayın veya kopyalayın.

   * Parametre dosyası oluşturma

     Bir parametre dosyası oluşturmak [için, 201-timeseriesinsights-environment-with-eventhub](https://github.com/Azure/azure-quickstart-templates/blob/master/201-timeseriesinsights-environment-with-eventhub/azuredeploy.parameters.json) dosyasını kopyalayın.

      [!code-json[deployment-parameters](~/quickstart-templates/201-timeseriesinsights-environment-with-eventhub/azuredeploy.parameters.json)]

    <div id="required-parameters"></div>

   * Gerekli Parametreler

     | Parametre | Açıklama |
     | --- | --- |
     | eventHubNamespaceName | Kaynak olay hub'ının ad alanı. |
     | eventHubName | Kaynak olay hub'ının adı. |
     | consumerGroupName | Zaman Serisi Öngörüleri hizmetinin olay merkezindeki verileri okumak için kullanacağı tüketici grubunun adı. **NOT:** Kaynak çekişmesini önlemek için, bu tüketici grubunun Time Series Insights hizmetine adanması ve diğer okuyucularla paylaşılmaması gerekir. |
     | environmentName | Çevrenin adı. Ad içeremez: `<` `>`, `%` `&`, `:` `\\`, `?` `/`, , , , , ve herhangi bir kontrol karakterleri. Diğer tüm karakterlere izin verilir.|
     | eventSourceName | Olay kaynağı alt kaynağının adı. Ad içeremez: `<` `>`, `%` `&`, `:` `\\`, `?` `/`, , , , , ve herhangi bir kontrol karakterleri. Diğer tüm karakterlere izin verilir. |

    <div id="optional-parameters"></div>

   * İsteğe Bağlı Parametreler

     | Parametre | Açıklama |
     | --- | --- |
     | mevcutEventHubResourceId | Etkinlik kaynağı üzerinden Time Series Öngörüleri ortamına bağlanacak olan varolan bir Olay Hub'ının isteğe bağlı kaynak kimliği. **NOT:** Şablonu dağıtan kullanıcının Olay Hub'ındaki listkeys işlemini gerçekleştirmek için ayrıcalıkları olması gerekir. Değer geçirilirse, şablon tarafından yeni bir olay hub'ı oluşturulur. |
     | environmentDisplayName | Ortam adı yerine araç veya kullanıcı arabirimlerinde gösterilen isteğe bağlı bir dostu ad. |
     | çevreSkuName | Sku'nun adı. Daha fazla bilgi için [Time Series Öngörüler Fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/time-series-insights/)okuyun.  |
     | çevreSkuCapacity | Sku'nun birim kapasitesi. Daha fazla bilgi için [Time Series Öngörüler Fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/time-series-insights/)okuyun.|
     | çevreDataRetentionTime | Ortamın olaylarının en az zaman açıklığı sorgu için kullanılabilir. Değer, örneğin `P30D` 30 günlük bir bekletme ilkesi için ISO 8601 biçiminde belirtilmelidir. |
     | eventSourceDisplayName | Olay kaynağı adı yerine araç veya kullanıcı arabirimlerinde gösterilen isteğe bağlı bir dostu ad. |
     | olaySourceTimestampPropertyName | Olay kaynağının zaman damgası olarak kullanılacak olay özelliği. TimestampPropertyName için bir değer belirtilmemişse veya null veya boş dize belirtilmişse, olay oluşturma süresi kullanılır. |
     | olaySourceKeyName | Zaman Serisi Öngörüleri hizmetinin etkinlik merkezine bağlanmak için kullanacağı paylaşılan erişim anahtarının adı. |
     | accessPolicyReaderObjectIds | Azure AD'deki kullanıcıların veya uygulamaların, Reader'ın ortama erişmesi gereken nesne iD'lerinin listesi. Hizmet temel objectId **Get-AzADUser** veya **Get-AzADServicePrincipal** cmdlets arayarak elde edilebilir. Azure REKLAM grupları için bir erişim ilkesi oluşturma henüz desteklenmemektedir. |
     | accessPolicyContributorObjectIds | Azure AD'deki kullanıcıların veya uygulamaların, Katılımcı'nın ortama erişmesi gereken nesne iD'lerinin listesi. Hizmet temel objectId **Get-AzADUser** veya **Get-AzADServicePrincipal** cmdlets arayarak elde edilebilir. Azure REKLAM grupları için bir erişim ilkesi oluşturma henüz desteklenmemektedir. |

   * Örnek olarak, aşağıdaki parametreler dosyası bir ortam ve varolan bir olay hub'ından olayları okuyan bir olay kaynağı oluşturmak için kullanılır. Ayrıca, Katılımcıya ortama erişim izni veren iki erişim ilkeleri de oluşturur.

     ```JSON
     {
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
         "contentVersion": "1.0.0.0",
         "parameters": {
             "eventHubNamespaceName": {
                 "value": "tsiTemplateTestNamespace"
             },
             "eventHubName": {
                 "value": "tsiTemplateTestEventHub"
             },
             "consumerGroupName": {
                 "value": "tsiTemplateTestConsumerGroup"
             },
             "environmentName": {
                 "value": "tsiTemplateTestEnvironment"
             },
             "eventSourceName": {
                 "value": "tsiTemplateTestEventSource"
             },
             "existingEventHubResourceId": {
                 "value": "/subscriptions/{yourSubscription}/resourceGroups/MyDemoRG/providers/Microsoft.EventHub/namespaces/tsiTemplateTestNamespace/eventhubs/tsiTemplateTestEventHub"
             },
             "accessPolicyContributorObjectIds": {
                 "value": [
                     "AGUID001-0000-0000-0000-000000000000",
                     "AGUID002-0000-0000-0000-000000000000"
                 ]
             }    
         }
     }
     ```

    * Daha fazla bilgi için [Parametreler](../azure-resource-manager/templates/parameter-files.md) makalesini okuyun.

## <a name="deploy-the-quickstart-template-locally-using-powershell"></a>PowerShell'i kullanarak hızlı başlangıç şablonuna yerel olarak dağıtın

> [!IMPORTANT]
> Aşağıda görüntülenen komut satırı işlemleri [Az PowerShell modülunu](https://docs.microsoft.com/powershell/azure/overview)açıklar.

1. PowerShell'de Azure hesabınızda oturum açın.

    * PowerShell komut isteminden aşağıdaki komutu çalıştırın:

      ```powershell
      Connect-AzAccount
      ```

    * Azure hesabınızda oturum açmanız istenir. Oturum açtıktan sonra, kullanılabilir aboneliklerinizi görüntülemek için aşağıdaki komutu çalıştırın:

      ```powershell
      Get-AzSubscription
      ```

    * Bu komut, kullanılabilir Azure aboneliklerinin bir listesini döndürür. Aşağıdaki komutu çalıştırarak geçerli oturum için bir abonelik seçin. Kullanmak `<YourSubscriptionId>` istediğiniz Azure aboneliği için GUID ile değiştirin:

      ```powershell
      Set-AzContext -SubscriptionID <YourSubscriptionId>
      ```

1. Yoksa yeni bir kaynak grubu oluşturun.

   * Varolan bir kaynak grubunuz yoksa, Yeni Kaynak **Grubu** komutunu içeren yeni bir kaynak grubu oluşturun. Kullanmak istediğiniz kaynak grubunun adını ve konumunu sağlayın. Örneğin:

     ```powershell
     New-AzResourceGroup -Name MyDemoRG -Location "West US"
     ```

   * Başarılı olursa, yeni kaynak grubunun bir özeti görüntülenir.

     ```powershell
     ResourceGroupName : MyDemoRG
     Location          : westus
     ProvisioningState : Succeeded
     Tags              :
     ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
     ```

1. Dağıtımı test edin.

   * `Test-AzResourceGroupDeployment` Cmdlet çalıştırarak dağıtım doğrulayın. Dağıtımı sınarken, dağıtımı yürürken tam olarak yaptığınız parametreleri sağlayın.

     ```powershell
     Test-AzResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
     ```

1. Dağıtımı oluşturma

    * Yeni dağıtım oluşturmak için `New-AzResourceGroupDeployment` cmdlet'i çalıştırın ve istendiğinde gerekli parametreleri sağlayın. Parametreler, dağıtımınız için bir ad, kaynak grubunuzun adını ve şablon dosyasının yolunu veya URL'sini içerir. **Mod** parametresi belirtilmemişse, **Artımlı'nın** varsayılan değeri kullanılır. Daha fazla bilgi için [Artımlı ve tam dağıtımları](../azure-resource-manager/templates/deployment-modes.md)okuyun.

    * Aşağıdaki komut, PowerShell penceresinde gerekli beş parametre için sizi ister:

      ```powershell
      New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
      ```

    * Bunun yerine bir parametre dosyası belirtmek için aşağıdaki komutu kullanın:

      ```powershell
      New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
      ```

    * Dağıtım cmdlet'ini çalıştırdığınızda satır içinde parametrelerde de kullanabilirsiniz. Komut aşağıdaki gibidir:

      ```powershell
      New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
      ```

    * [Tam](../azure-resource-manager/templates/deployment-modes.md) bir dağıtım çalıştırmak için **Mod** parametresini **Tamamla**olarak ayarlayın:

      ```powershell
      New-AzResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
      ```

1. Dağıtımı doğrulama

    * Kaynaklar başarıyla dağıtılırsa, dağıtımın bir özeti PowerShell penceresinde görüntülenir:

      ```powershell
       DeploymentName          : MyDemoDeployment
       ResourceGroupName       : MyDemoRG
       ProvisioningState       : Succeeded
       Timestamp               : 10/11/2019 3:20:37 AM
       Mode                    : Incremental
       TemplateLink            :
       Parameters              :
                                 Name                                Type                       Value
                                 ==================================  =========================  ==========
                                 eventHubNewOrExisting               String                     new
                                 eventHubResourceGroup               String                     MyDemoRG
                                 eventHubNamespaceName               String                     tsiquickstartns
                                 eventHubName                        String                     tsiquickstarteh
                                 consumerGroupName                   String                     tsiquickstart
                                 environmentName                     String                     tsiquickstart
                                 environmentDisplayName              String                     tsiquickstart
                                 environmentSkuName                  String                     S1
                                 environmentSkuCapacity              Int                        1
                                 environmentDataRetentionTime        String                     P30D
                                 eventSourceName                     String                     tsiquickstart
                                 eventSourceDisplayName              String                     tsiquickstart
                                 eventSourceTimestampPropertyName    String
                                 eventSourceKeyName                  String                     manage
                                 accessPolicyReaderObjectIds         Array                      []
                                 accessPolicyContributorObjectIds    Array                      []
                                 location                            String                     westus

       Outputs                 :
                                  Name              Type                       Value
                                  ================  =========================  ==========
                                  dataAccessFQDN    String
                                  11aa1aa1-a1aa-1a1a-a11a-aa111a111a11.env.timeseries.azure.com

       DeploymentDebugLogLevel :
      ```

1. Azure portalı üzerinden hızlı başlangıç şablonu dağıtma

   * Hızlı başlangıç şablonunun GitHub'daki ana sayfasında **Azure'a Dağıt** düğmesi de yer alıyor. Tıklatıldığında, Azure portalında özel dağıtım sayfası açılır. Bu sayfadan, [gerekli parametrelerden](#required-parameters) veya [isteğe bağlı parametreler](#optional-parameters) tablolarından parametrelerin her biri için değerler girebilir veya seçebilirsiniz. Ayarları doldurduktan sonra Satın **Alma** düğmesini tıklattığınızda şablon dağıtımı başlatılır.
    </br>
    </br>
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-timeseriesinsights-environment-with-eventhub%2Fazuredeploy.json" target="_blank">
       <img src="https://azuredeploy.net/deploybutton.png"/>
    </a>

## <a name="next-steps"></a>Sonraki adımlar

- REST API'lerini kullanarak Time Series Insights kaynaklarını programlı olarak yönetme hakkında daha fazla bilgi için [Time Series Insights Management](https://docs.microsoft.com/rest/api/time-series-insights-management/)'ı okuyun.
