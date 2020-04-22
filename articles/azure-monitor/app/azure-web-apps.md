---
title: Azure uygulama hizmetleri performansını izleyin | Microsoft Dokümanlar
description: Azure uygulama hizmetleri için uygulama performansı izleme. Yükleme ve yanıt süresini, bağımlılık bilgilerini grafik ve performans uyarıları ayarlayın.
ms.topic: conceptual
ms.date: 12/11/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: dd0d3be6ed7e5185183618cc2bdeff5ee8d749f3
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81729801"
---
# <a name="monitor-azure-app-service-performance"></a>Azure App Service performansını izleme

[Azure Uygulama Hizmetleri'nde](https://docs.microsoft.com/azure/app-service/) çalışan ASP.NET ve ASP.NET Core tabanlı web uygulamalarınızda izlemeyi etkinleştirmek artık her zamankinden daha kolay. Daha önce bir site uzantısını el ile yüklemeniz gerekirken, en son uzantı/aracı artık varsayılan olarak uygulama hizmeti görüntüsüne yerleşiktir. Bu makale, Uygulama Öngörüleri izleme yi etkinleştirmenin yanı sıra büyük ölçekli dağıtımlar için işlemi otomatikleştirmek için ön yönerge ler sağlayacaktır.

> [!NOTE]
> **Geliştirme Araçları** > **Uzantıları** aracılığıyla Uygulama Öngörüleri site uzantısının el ile eklenmesi amortismana tabi dir. Bu uzantı yükleme yöntemi, her yeni sürüm için el ile güncelleştirmeye bağlıydı. Uzantının en son kararlı sürümü, Uygulama Hizmeti görüntüsünün bir parçası olarak [önceden yüklenmiş](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions) oldu. Dosyalar bulunur `d:\Program Files (x86)\SiteExtensions\ApplicationInsightsAgent` ve her kararlı sürümle otomatik olarak güncelleştirilir. Aşağıda izlemeyi etkinleştirmek için aracı tabanlı yönergeleri izlerseniz, sizin için amortismana dayalı uzantıyı otomatik olarak kaldırır.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

Azure Uygulama Hizmetleri barındırılan uygulamalar için uygulama izlemeyi etkinleştirmenin iki yolu vardır:

* **Aracı tabanlı uygulama izleme** (ApplicationInsightsAgent).  
    * Bu yöntem etkinleştirilmesi en kolay yöntemdir ve gelişmiş yapılandırma gerekmez. Genellikle "çalışma zamanı" izleme olarak adlandırılır. Azure Uygulama Hizmetleri için en azından bu izleme düzeyini etkinleştirmenizi öneririz ve ardından özel senaryonuza göre manuel enstrümantasyon yoluyla daha gelişmiş izleme gerekip gerekmediğini değerlendirebilirsiniz.

* Application Insights SDK'yı yükleyerek **uygulamayı kod üzerinden elle kullanma.**

    * Bu yaklaşım çok daha özelleştirilebilir, ancak [Uygulama Öngörüleri SDK NuGet paketleri bir bağımlılık eklemeyi](https://docs.microsoft.com/azure/azure-monitor/app/asp-net)gerektirir. Bu yöntem, aynı zamanda paketlerin en son sürümüne güncellemeleri kendiniz yönetmek zorunda anlamına gelir.

    * Aracı tabanlı izleme yle varsayılan olarak yakalanan olayları/bağımlılıkları izlemek için özel API çağrıları yapmanız gerekiyorsa, bu yöntemi kullanmanız gerekir. Daha fazla bilgi [edinmek için özel olaylar ve ölçümler makalesi için API'ye](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics) göz atın. Bu da şu anda Linux tabanlı iş yükleri için desteklenen tek seçenektir.

> [!NOTE]
> Hem aracı tabanlı izleme hem de manuel SDK tabanlı enstrümantasyon algılanırsa, yalnızca manuel enstrümantasyon ayarlarına uyulacaktır. Bu, yinelenen verilerin gönderilmesini önlemek içindir. Bu konuda daha fazla bilgi edinmek için aşağıdaki [sorun giderme bölümüne](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#troubleshooting) göz atın.

## <a name="enable-agent-based-monitoring"></a>Aracı tabanlı izlemeyi etkinleştirme

# <a name="net"></a>[.NET](#tab/net)

> [!NOTE]
> APPINSIGHTS_JAVASCRIPT_ENABLED ve urlCompression kombinasyonu desteklenmez. Daha fazla bilgi için [sorun giderme bölümündeki](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#troubleshooting)açıklamaya bakın.


1. Uygulama hizmetiniz için Azure denetim panelinde **Uygulama Öngörüleri'ni seçin.**

    ![Ayarlar altında, Uygulama Öngörüleri'ni seçin](./media/azure-web-apps/settings-app-insights-01.png)

   * Bu uygulama için zaten bir Application Insights kaynağı ayarlamadığınız sürece yeni bir kaynak oluşturmayı seçin. 

     > [!NOTE]
     > Yeni kaynağı oluşturmak için **Tamam'ı** tıklattığınızda **izleme ayarlarını uygulamanız**istenir. **Continue'yi** seçmek yeni Uygulama Öngörüleri kaynağınızı uygulama hizmetinize bağlar, bunu yapmak da **uygulama hizmetinizin yeniden başlatılmasını tetikler.** 

     ![Web uygulamanızı izleme](./media/azure-web-apps/create-resource-01.png)

2. Hangi kaynağın kullanılacağını belirttikten sonra, uygulamanızın platform başına veri toplamasını nasıl istediğinizi seçebilirsiniz. ASP.NET uygulama izleme, iki farklı toplama düzeyiyle varsayılan olarak yapılır.

    ![Platform başına seçenekleri seçin](./media/azure-web-apps/choose-options-new.png)
 
 Aşağıda her rota için toplanan verilerin bir özeti verilmiştir:
        
|  | .NET Temel Koleksiyonu | .NET Önerilen koleksiyon |
| --- | --- | --- |
| CPU, bellek ve G/Ç kullanım eğilimlerini ekler |Evet |Evet |
| Kullanım eğilimlerini toplar ve kullanılabilirlik sonuçlarıyla işlemler arasında bağıntı sağlar | Evet |Evet |
| Ana işlem tarafından işlenmeyen özel durumları toplar | Evet |Evet |
| Örnekleme kullanıldığında yük altındaki APM ölçümü doğruluğunu geliştirir | Evet |Evet |
| Mikro hizmetler ile istek/bağımlılık sınırları arasında bağıntı sağlar | Hayır (yalnızca tek örnekLi APM özellikleri) |Evet |

3. Örnekleme gibi ayarları yapılandırmak için, daha önce applicationinsights.config dosyası üzerinden kontrol edebilirsiniz şimdi ilgili bir önek ile Uygulama ayarları üzerinden aynı ayarları ile etkileşim edersiniz. 

    * Örneğin, ilk örnekleme yüzdesini değiştirmek için, bir Uygulama `MicrosoftAppInsights_AdaptiveSamplingTelemetryProcessor_InitialSamplingPercentage` ayarı `100`oluşturabilirsiniz: ve bir değer .

    * Desteklenen uyarlanabilir örnekleme telemetri işlemci ayarları listesi [için, kod](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/master/src/ServerTelemetryChannel/AdaptiveSamplingTelemetryProcessor.cs) ve [ilişkili belgelere](https://docs.microsoft.com/azure/azure-monitor/app/sampling)danışabilirsiniz.

# <a name="net-core"></a>[.NET Core](#tab/netcore)

.NET Core'un aşağıdaki sürümleri desteklenir: ASP.NET Core 2.0, ASP.NET Core 2.1, ASP.NET Core 2.2, ASP.NET Core 3.0

.NET Core,bağımsız dağıtım ve Linux tabanlı uygulamalardan tam çerçeveyi hedeflemek şu anda aracı/uzantı tabanlı izleme ile **desteklenmemektedir.** ( Kod üzerinden[manuel enstrümantasyon](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) önceki tüm senaryolarda çalışacaktır.)

1. Uygulama hizmetiniz için Azure denetim panelinde **Uygulama Öngörüleri'ni seçin.**

    ![Ayarlar altında, Uygulama Öngörüleri'ni seçin](./media/azure-web-apps/settings-app-insights-01.png)

   * Bu uygulama için zaten bir Application Insights kaynağı ayarlamadığınız sürece yeni bir kaynak oluşturmayı seçin. 

     > [!NOTE]
     > Yeni kaynağı oluşturmak için **Tamam'ı** tıklattığınızda **izleme ayarlarını uygulamanız**istenir. **Continue'yi** seçmek yeni Uygulama Öngörüleri kaynağınızı uygulama hizmetinize bağlar, bunu yapmak da **uygulama hizmetinizin yeniden başlatılmasını tetikler.** 

     ![Web uygulamanızı izleme](./media/azure-web-apps/create-resource-01.png)

2. Hangi kaynağın kullanılacağını belirttikten sonra, Uygulama Öngörüleri'nin uygulamanız için platform başına veri toplamasını nasıl istediğinizi seçebilirsiniz. .NET Core ,NET Core 2.0, 2.1, 2.2 ve 3.0 için **Önerilen toplama** veya Devre **Dışı sunar.**

    ![Platform başına seçenekleri seçin](./media/azure-web-apps/choose-options-new-net-core.png)

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

**Ayarlar** > altındaki Uygulama Hizmeti web uygulamanızın içinden**Uygulama Öngörülerini** > **Etkinleştir'i**seçin. Node.js aracı tabanlı izleme şu anda önizlemede.

# <a name="java"></a>[Java](#tab/java)

Java App Service tabanlı web uygulamaları şu anda otomatik ajan/uzantı tabanlı izlemeyi desteklemez. Java uygulamanızın izlenmesini etkinleştirmek [için, uygulamanızı el ile kullanmanız](https://docs.microsoft.com/azure/azure-monitor/app/java-get-started)gerekir.

# <a name="python"></a>[Python](#tab/python)

Python App Service tabanlı web uygulamaları şu anda otomatik aracı/uzantı tabanlı izlemeyi desteklemez. Python uygulamanızın izlenmesini etkinleştirmek [için, uygulamanızı el ile kullanmanız](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python)gerekir.

---

## <a name="enable-client-side-monitoring"></a>İstemci tarafı izlemeyi etkinleştirme

# <a name="net"></a>[.NET](#tab/net)

İstemci tarafı izleme ASP.NET için tercih edilir. İstemci tarafı izlemeyi etkinleştirmek için:

* **Ayarlar** >** **Uygulama ayarları**** seçeneğini belirleyin
   * Uygulama ayarları altında, yeni bir **uygulama ayar adı** ve **değeri**ekleyin:

     Adı:`APPINSIGHTS_JAVASCRIPT_ENABLED`

     Değer:`true`

   * Ayarları **Kaydedin** ve uygulamanızı **Yeniden başlatın**.

![Uygulama ayarları kullanıcı tarafından görüntülenmiştir Kullanıcı Bira](./media/azure-web-apps/appinsights-javascript-enabled.png)

İstemci tarafı izlemeyi devre dışı düşürmek için, ilişkili anahtar değer çiftini Uygulama ayarlarından kaldırın veya değeri false olarak ayarlayın.

# <a name="net-core"></a>[.NET Core](#tab/netcore)

'APPINSIGHTS_JAVASCRIPT_ENABLED' uygulama ayarının mevcut olup olmadığına bakılmaksızın, **Önerilen koleksiyona**sahip .NET Core uygulamaları için varsayılan olarak istemci tarafı izleme **etkinleştirilir.**

Bazı nedenden dolayı istemci tarafı izlemeyi devre dışı kalmak istiyorsanız:

* **Ayarlar** > **Uygulama ayarlarını** seçin
   * Uygulama ayarları altında, yeni bir **uygulama ayar adı** ve **değeri**ekleyin:

     Adı:`APPINSIGHTS_JAVASCRIPT_ENABLED`

     Değer:`false`

   * Ayarları **Kaydedin** ve uygulamanızı **Yeniden başlatın**.

![Uygulama ayarları kullanıcı tarafından görüntülenmiştir Kullanıcı Bira](./media/azure-web-apps/appinsights-javascript-disabled.png)

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Node.js uygulamanız için istemci tarafı izlemeyi etkinleştirmek için, [istemci tarafındaki JavaScript SDK'yı uygulamanıza el ile eklemeniz](https://docs.microsoft.com/azure/azure-monitor/app/javascript)gerekir.

# <a name="java"></a>[Java](#tab/java)

Java uygulamanız için istemci tarafı izlemeyi etkinleştirmek için, [istemci tarafındaki JavaScript SDK'yı uygulamanıza el ile eklemeniz](https://docs.microsoft.com/azure/azure-monitor/app/javascript)gerekir.

# <a name="python"></a>[Python](#tab/python)

Python uygulamanız için istemci tarafı izlemeyi etkinleştirmek için, [istemci tarafındaki JavaScript SDK'yı uygulamanıza el ile eklemeniz](https://docs.microsoft.com/azure/azure-monitor/app/javascript)gerekir.

---

## <a name="automate-monitoring"></a>İzlemeyi otomatikleştirin

Uygulama Öngörüleri ile telemetri koleksiyonunu etkinleştirmek için yalnızca Uygulama ayarlarının ayarlanabilmesi gerekir:

   ![Kullanılabilir Uygulama Öngörüleri ayarlarıyla Uygulama Hizmeti Uygulama Ayarları](./media/azure-web-apps/application-settings.png)

### <a name="application-settings-definitions"></a>Uygulama ayarları tanımları

|Uygulama ayar adı |  Tanım | Değer |
|-----------------|:------------|-------------:|
|ApplicationInsightsAgent_EXTENSION_VERSION | Çalışma zamanı izlemeyi kontrol eden ana uzantı. | `~2` |
|XDT_MicrosoftApplicationInsights_Mode |  Yalnızca varsayılan modda, en iyi performansı sağlamak için temel özellikler etkinleştirilir. | `default` veya `recommended`. |
|InstrumentationEngine_EXTENSION_VERSION | İkili yeniden yazma motorunun `InstrumentationEngine` açık olup olmayacağını kontrol eder. Bu ayarın performans etkileri vardır ve soğuk başlangıç/başlangıç süresini etkiler. | `~1` |
|XDT_MicrosoftApplicationInsights_BaseExtensions | SQL & Azure tablo metni bağımlılık çağrılarıyla birlikte yakalanacaksa denetimler. Performans uyarısı: uygulama soğuk başlatma süresi etkilenir. Bu ayar, `InstrumentationEngine`. | `~1` |

### <a name="app-service-application-settings-with-azure-resource-manager"></a>Azure Kaynak Yöneticisi ile Uygulama Hizmeti Uygulaması ayarları

Uygulama Hizmetleri için uygulama ayarları Azure [Kaynak Yöneticisi şablonları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)ile yönetilebilir ve yapılandırılabilir. Bu yöntem, Azure Kaynak Yöneticisi otomasyonu yla yeni Uygulama Hizmeti kaynaklarını dağıtırken veya varolan kaynakların ayarlarını değiştirmek için kullanılabilir.

Bir uygulama hizmeti için JSON uygulama ayarlarının temel yapısı aşağıdadır:

```JSON
      "resources": [
        {
          "name": "appsettings",
          "type": "config",
          "apiVersion": "2015-08-01",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites', variables('webSiteName'))]"
          ],
          "tags": {
            "displayName": "Application Insights Settings"
          },
          "properties": {
            "key1": "value1",
            "key2": "value2"
          }
        }
      ]
```

Uygulama Öngörüleri için yapılandırılan Uygulama ayarlarına sahip bir Azure Kaynak Yöneticisi şablonu örneği için, bu [şablon,](https://github.com/Andrew-MSFT/BasicImageGallery) özellikle [238 satırından](https://github.com/Andrew-MSFT/BasicImageGallery/blob/c55ada54519e13ce2559823c16ca4f97ddc5c7a4/CoreImageGallery/Deploy/CoreImageGalleryARM/azuredeploy.json#L238)başlayan bölüm olmak üzere yararlı olabilir.

### <a name="automate-the-creation-of-an-application-insights-resource-and-link-to-your-newly-created-app-service"></a>Bir Application Insights kaynağının oluşturulmasını otomatikleştirin ve yeni oluşturduğunuz Uygulama Hizmetine bağlantı kurun.

Tüm varsayılan Uygulama Öngörüleri ayarlarını yapılandırılan bir Azure Kaynak Yöneticisi şablonu oluşturmak için, uygulama istatistikleri etkinleştirilmiş yeni bir Web Uygulaması oluşturacakmış gibi işlemi başlatın.

**Otomasyon seçeneklerini** seçin

   ![App Service web uygulaması oluşturma menüsü](./media/azure-web-apps/create-web-app.png)

Bu seçenek, gerekli tüm ayarları yapılandırılan en son Azure Kaynak Yöneticisi şablonu oluşturur.

  ![App Service web uygulaması şablonu](./media/azure-web-apps/arm-template.png)

Aşağıda bir örnek, site adınız `AppMonitoredSite` ile tüm örnekleri değiştirin:

```json
{
    "resources": [
        {
            "name": "[parameters('name')]",
            "type": "Microsoft.Web/sites",
            "properties": {
                "siteConfig": {
                    "appSettings": [
                        {
                            "name": "APPINSIGHTS_INSTRUMENTATIONKEY",
                            "value": "[reference('microsoft.insights/components/AppMonitoredSite', '2015-05-01').InstrumentationKey]"
                        },
                        {
                            "name": "APPLICATIONINSIGHTS_CONNECTION_STRING",
                            "value": "[reference('microsoft.insights/components/AppMonitoredSite', '2015-05-01').ConnectionString]"
                        },
                        {
                            "name": "ApplicationInsightsAgent_EXTENSION_VERSION",
                            "value": "~2"
                        }
                    ]
                },
                "name": "[parameters('name')]",
                "serverFarmId": "[concat('/subscriptions/', parameters('subscriptionId'),'/resourcegroups/', parameters('serverFarmResourceGroup'), '/providers/Microsoft.Web/serverfarms/', parameters('hostingPlanName'))]",
                "hostingEnvironment": "[parameters('hostingEnvironment')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Web/serverfarms/', parameters('hostingPlanName'))]",
                "microsoft.insights/components/AppMonitoredSite"
            ],
            "apiVersion": "2016-03-01",
            "location": "[parameters('location')]"
        },
        {
            "apiVersion": "2016-09-01",
            "name": "[parameters('hostingPlanName')]",
            "type": "Microsoft.Web/serverfarms",
            "location": "[parameters('location')]",
            "properties": {
                "name": "[parameters('hostingPlanName')]",
                "workerSizeId": "[parameters('workerSize')]",
                "numberOfWorkers": "1",
                "hostingEnvironment": "[parameters('hostingEnvironment')]"
            },
            "sku": {
                "Tier": "[parameters('sku')]",
                "Name": "[parameters('skuCode')]"
            }
        },
        {
            "apiVersion": "2015-05-01",
            "name": "AppMonitoredSite",
            "type": "microsoft.insights/components",
            "location": "West US 2",
            "properties": {
                "ApplicationId": "[parameters('name')]",
                "Request_Source": "IbizaWebAppExtensionCreate"
            }
        }
    ],
    "parameters": {
        "name": {
            "type": "string"
        },
        "hostingPlanName": {
            "type": "string"
        },
        "hostingEnvironment": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "sku": {
            "type": "string"
        },
        "skuCode": {
            "type": "string"
        },
        "workerSize": {
            "type": "string"
        },
        "serverFarmResourceGroup": {
            "type": "string"
        },
        "subscriptionId": {
            "type": "string"
        }
    },
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0"
}
```

### <a name="enabling-through-powershell"></a>PowerShell ile etkinleştirme

PowerShell üzerinden uygulama izlemeyi etkinleştirmek için yalnızca temel uygulama ayarlarının değiştirilmesi gerekir. Aşağıda kaynak grubu "AppMonitoredRG" "AppMonitoredSite" adlı bir web sitesi için uygulama izleme sağlayan bir örnek, ve "012345678-abcd-ef01-2345-6789abcd" enstrümantasyon anahtarı gönderilecek verileri yapılandırır.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```powershell
$app = Get-AzWebApp -ResourceGroupName "AppMonitoredRG" -Name "AppMonitoredSite" -ErrorAction Stop
$newAppSettings = @{} # case-insensitive hash map
$app.SiteConfig.AppSettings | %{$newAppSettings[$_.Name] = $_.Value} # preserve non Application Insights application settings.
$newAppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"] = "012345678-abcd-ef01-2345-6789abcd"; # set the Application Insights instrumentation key
$newAppSettings["APPLICATIONINSIGHTS_CONNECTION_STRING"] = "InstrumentationKey=012345678-abcd-ef01-2345-6789abcd"; # set the Application Insights connection string
$newAppSettings["ApplicationInsightsAgent_EXTENSION_VERSION"] = "~2"; # enable the ApplicationInsightsAgent
$app = Set-AzWebApp -AppSettings $newAppSettings -ResourceGroupName $app.ResourceGroup -Name $app.Name -ErrorAction Stop
```

## <a name="upgrade-monitoring-extensionagent"></a>Yükseltme izleme uzantısı/aracısı

### <a name="upgrading-from-versions-289-and-up"></a>2.8.9 ve yukarı sürümlerinden yükseltme

Sürüm 2.8.9'dan yükseltme, herhangi bir ek eylem olmadan otomatik olarak gerçekleşir. Yeni izleme bitleri arka planda hedef uygulama hizmetine teslim edilir ve uygulama yeniden başlatılır.

Çalıştırdığınız uzantının hangi sürümünü ziyaret ettiğinizi denetlemek için`http://yoursitename.scm.azurewebsites.net/ApplicationInsights`

![url yolunun ekran görüntüsühttp://yoursitename.scm.azurewebsites.net/ApplicationInsights](./media/azure-web-apps/extension-version.png)

### <a name="upgrade-from-versions-100---265"></a>1.0.0 - 2.6.5 sürümlerinden yükseltme

Sürüm 2.8.9 ile başlayarak önceden yüklenmiş site uzantısı kullanılır. Daha önceki bir sürümseniz, iki şekilde güncelleştirebilirsiniz:

* [Portal üzerinden etkinleştirerek yükseltme.](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enable-application-insights) (Azure Uygulama Hizmeti için Uygulama Öngörüleri uzantısı yüklü olsa bile, Kullanıcı Arabirimi yalnızca **Etkinleştir** düğmesini gösterir. Arka planda, eski özel site uzantısı kaldırılacak.)

* [PowerShell ile yükseltme:](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enabling-through-powershell)

    1. Önceden yüklenmiş site uzantısı ApplicationInsightsAgent'ı etkinleştirmek için uygulama ayarlarını ayarlayın. Bkz. [Powershell ile etkinleştirme.](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enabling-through-powershell)
    2. Azure Uygulama Hizmeti için Application Insights uzantısı adlı özel site uzantısını el ile kaldırın.

Yükseltme 2.5.1'den önceki bir sürümden yapılırsa, ApplicationInsigths dlls'nin uygulama kutusu klasöründen kaldırılıp kaldırılmaz sorun [giderme adımlarını görün.](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#troubleshooting)

## <a name="troubleshooting"></a>Sorun giderme

Aşağıda, Azure Uygulama Hizmetleri'nde çalışan .NET ve .NET Core tabanlı uygulamalar için uzantı/aracı tabanlı izleme için adım adım sorun giderme kılavuzumuz yer almaktadır.

> [!NOTE]
> Java uygulamaları yalnızca El Ile SDK tabanlı araçlar la Azure Uygulama Hizmetleri'nde desteklenir ve bu nedenle aşağıdaki adımlar bu senaryolar için geçerli değildir.

1. Uygulamanın `ApplicationInsightsAgent`.
    * Uygulama `ApplicationInsightsAgent_EXTENSION_VERSION` ayarının "~2" değerine ayarlı olup olmadığını kontrol edin.
2. Uygulamanın izlenecek gereksinimleri karşıladığından emin olun.
    * Göz atın`https://yoursitename.scm.azurewebsites.net/ApplicationInsights`

    ![Sonuçlar https://yoursitename.scm.azurewebsites/applicationinsights sayfasının ekran görüntüsü](./media/azure-web-apps/app-insights-sdk-status.png)

    * Bu `Application Insights Extension Status` olduğunu onaylamak`Pre-Installed Site Extension, version 2.8.12.1527, is running.`
        * Çalışmıyorsa, [Uygulama Öngörülerini etkinleştirme yönergelerini](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps#enable-application-insights) izleyin

    * Durum kaynağının var olduğunu ve aşağıdaki gibi göründüğünü doğrulayın:`Status source D:\home\LogFiles\ApplicationInsights\status\status_RD0003FF0317B6_4248_1.json`
        * Benzer bir değer yoksa, uygulama şu anda çalışmıyor veya desteklenmiyor demektir. Uygulamanın çalıştığından emin olmak için, çalışma zamanı bilgilerinin kullanılabilir olmasını sağlayacak olan uygulama url'sini/uygulama bitiş noktalarını el ile ziyaret etmeyi deneyin.

    * Bunun `IKeyExists` doğrula`true`
        * Eğer, `false`eklemek `APPINSIGHTS_INSTRUMENTATIONKEY` ve `APPLICATIONINSIGHTS_CONNECTION_STRING` uygulama ayarları için ikey guid ile.

    * Için giriş olmadığını doğrulayın ve `AppContainsAspNetTelemetryCorrelationAssembly`. `AppContainsDiagnosticSourceAssembly` `AppAlreadyInstrumented`
        * Bu girişlerden herhangi biri varsa, aşağıdaki paketleri `Microsoft.ApplicationInsights` `System.Diagnostics.DiagnosticSource`uygulamanızdan `Microsoft.AspNet.TelemetryCorrelation`kaldırın: , , ve .

Aşağıdaki tablo, bu değerlerin ne anlama geldiğini, bunların altında yatan nedenleri ve önerilen düzeltmeleri hakkında daha ayrıntılı bir açıklama sağlar:

|Sorun Değeri|Açıklama|Düzeltme
|---- |----|---|
| `AppAlreadyInstrumented:true` | Bu değer, uzantının SDK'nın bazı yönünün Uygulamada zaten mevcut olduğunu algıladığını ve geri çekeceğini gösterir. Bu bir referans nedeniyle `System.Diagnostics.DiagnosticSource`olabilir `Microsoft.AspNet.TelemetryCorrelation`, veya`Microsoft.ApplicationInsights`  | Başvuruları kaldırın. Bu başvuruların bazıları varsayılan olarak belirli Visual Studio şablonlarından eklenir ve Visual `Microsoft.ApplicationInsights`Studio'nun eski sürümlerine referanslar eklenebilir.
|`AppAlreadyInstrumented:true` | Uygulama .NET Core 2.1 veya 2.2'yi hedefliyorsa ve [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) meta paketine başvuruyorsa, Uygulama Öngörüleri getirir ve uzantısı geri çekülür. | .NET Core 2.1,2.2'deki müşterilerin Microsoft.AspNetCore.App meta paketini kullanmaları [önerilir.](https://github.com/aspnet/Announcements/issues/287)|
|`AppAlreadyInstrumented:true` | Bu değer, önceki bir dağıtımdan uygulama klasöründe yukarıdaki dlls varlığı ndan da kaynaklanabilir. | Bu dll'lerin kaldırıldığından emin olmak için uygulama klasörünü temizleyin. Hem yerel uygulamanızın bin dizinini hem de Uygulama Hizmeti'ndeki wwwroot dizinini kontrol edin. (App Service web uygulamanızın wwwroot dizinini kontrol etmek için: Gelişmiş Araçlar (Kudu) > Hata Ayıklama konsolu > > CMD > ev\site\wwwroot).
|`AppContainsAspNetTelemetryCorrelationAssembly: true` | Bu değer, uzantı `Microsoft.AspNet.TelemetryCorrelation` uygulamada başvurular algılanan gösterir ve geri olacaktır. | Başvuruyu kaldırın.
|`AppContainsDiagnosticSourceAssembly**:true`|Bu değer, uzantı `System.Diagnostics.DiagnosticSource` uygulamada başvurular algılanan gösterir ve geri olacaktır.| Başvuruyu kaldırın.
|`IKeyExists:false`|Bu değer, enstrümantasyon anahtarının AppSetting'te `APPINSIGHTS_INSTRUMENTATIONKEY`bulunmadığını gösterir. Olası nedenler: Değerler yanlışlıkla kaldırılmış olabilir, otomasyon komut dosyasındaki değerleri ayarlamayı unutmuş olabilir, vb. | Ayarın Uygulama Hizmeti uygulama ayarlarında olduğundan emin olun.

### <a name="appinsights_javascript_enabled-and-urlcompression-is-not-supported"></a>APPINSIGHTS_JAVASCRIPT_ENABLED ve urlSıkıştırma desteklenmiyor

İçeriğin kodlandığı durumlarda APPINSIGHTS_JAVASCRIPT_ENABLED=True kullanıyorsanız, şu gibi hatalar alabilirsiniz: 

- 500 URL yeniden yazma hatası
- 500.53 URL yeniden yazma modülü hatası ile ileti Giden yeniden yazma kuralları http yanıtı içeriği kodlandığında uygulanamaz ('gzip'). 

Bunun nedeni, APPINSIGHTS_JAVASCRIPT_ENABLED uygulama ayarı gerçek ve içerik kodlama aynı anda mevcut olarak ayarlanmaktadır kaynaklanmaktadır. Bu senaryo henüz desteklenmedi. Geçici çözüm, uygulama ayarlarınızdan APPINSIGHTS_JAVASCRIPT_ENABLED kaldırmaktır. Ne yazık ki bu, istemci/tarayıcı tarafı JavaScript enstrümantasyonu hala gerekliyse, web sayfalarınız için manuel SDK başvuruları gerektiği anlamına gelir. Lütfen JavaScript SDK ile manuel enstrümantasyon [talimatlarını](https://github.com/Microsoft/ApplicationInsights-JS#snippet-setup-ignore-if-using-npm-setup) uygulayın.

Application Insights aracısı/uzantısı hakkında en son bilgiler için [sürüm notlarına](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/app-insights-web-app-extensions-releasenotes.md)göz atın.

### <a name="php-and-wordpress-are-not-supported"></a>PHP ve WordPress desteklenmiyor

PHP ve WordPress siteleri desteklenmez. Şu anda bu iş yüklerinin sunucu tarafından izlenmesi için resmi olarak desteklenen Bir SDK/aracı bulunmamaktadır. Ancak, web sayfalarınıza istemci tarafı javascript ekleyerek bir PHP veya WordPress sitesinde istemci tarafı işlemleri el ile enstrümaning [JavaScript SDK](https://docs.microsoft.com/azure/azure-monitor/app/javascript)kullanılarak gerçekleştirilebilir. 

## <a name="next-steps"></a>Sonraki adımlar
* [Canlı uygulamanızda profil oluşturucuyu çalıştırın](../app/profiler.md).
* [Azure İşlevleri](https://github.com/christopheranderson/azure-functions-app-insights-sample) - Application Insights ile Azure İşlevlerini izleyin
* Application Insights’a gönderilmek üzere [Azure tanılamayı etkinleştirin](../platform/diagnostics-extension-to-application-insights.md).
* Hizmetinizin kullanılabilir ve yanıt verir durumda oluğundan emin olmak için [hizmet durumu ölçümlerini izleyin](../platform/data-platform.md).
* İşletimsel olaylar gerçekleştiğinde ya da ölçümler bir eşiği aştığında [uyarı bildirimleri alın](../platform/alerts-overview.md).
* Bir web sayfasını ziyaret eden tarayıcılardan istemci telemetrisi toplamak istiyorsanız [JavaScript uygulamaları ve web sayfaları için Application Insights](javascript.md)’ı kullanın.
* Sitenizin kapalı olması durumunda uyarı almak istiyorsanız [Kullanılabilirlik web testleri](monitor-web-app-availability.md) ayarlayın.
