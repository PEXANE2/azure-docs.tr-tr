---
title: Yeni bir Azure Izleyici Application Insights çalışma alanı tabanlı kaynak oluştur | Microsoft Docs
description: Yeni Azure Izleyici Application Insights çalışma alanı tabanlı kaynakları etkinleştirmek için gereken adımlar hakkında bilgi edinin.
author: mrbullwinkle
ms.author: mbullwin
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: e66ae6aa0b9b7e309fbd6fcc3699cb873a266bbe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83647902"
---
# <a name="workspace-based-application-insights-resources-preview"></a>Çalışma alanı tabanlı Application Insights kaynakları (Önizleme)

Çalışma alanı tabanlı kaynaklar Application Insights ve Log Analytics arasında tam tümleştirmeyi destekler. Artık Application Insights telemetrinizi ortak bir Log Analytics çalışma alanına göndermenizi seçebilirsiniz. Bu, uygulama, altyapı ve platform günlüklerinin tek bir birleştirilmiş konumda tutulması sırasında tüm Log Analytics özelliklerine tam erişim sağlar.

Bu Ayrıca, kaynaklarınız genelinde ortak rol tabanlı Access Control (RBAC) sağlar ve uygulamalar arası/çalışma alanı sorgularının gereksinimini ortadan kaldırır.

> [!NOTE]
> Çalışma alanı tabanlı Application Insights kaynakları için veri alımı ve saklama, verilerin bulunduğu Log Analytics çalışma alanı üzerinden faturalandırılır. Çalışma alanı tabanlı Application Insights kaynakları için faturalandırma hakkında [daha fazla bilgi edinin]( https://docs.microsoft.com/azure/azure-monitor/app/pricing#workspace-based-application-insights) .

Yeni deneyimi test etmek için [Azure Portal](https://portal.azure.com)oturum açın ve bir Application Insights kaynağı oluşturun:

![Çalışma alanı tabanlı Application Insights kaynağı](./media/create-workspace-resource/create-workspace-based.png)

Zaten mevcut bir Log Analytics çalışma alanınız yoksa, [Log Analytics çalışma alanı oluşturma belgelerine başvurun](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

Genel Önizleme **çalışma alanı tabanlı kaynaklar şu anda Batı ABD 2, Doğu ABD ve Orta Güney ABD ile sınırlıdır.**

Kaynağınız oluşturulduktan sonra **genel bakış** bölmesinde ilgili çalışma alanı bilgilerini görürsünüz:

![Çalışma Alanı Adı](./media/create-workspace-resource/workspace-name.png)

Mavi bağlantı metnine tıkladığınızda, yeni Birleşik çalışma alanı sorgu ortamından yararlanabilmeniz için ilgili Log Analytics çalışma alanına gidecektir.

> [!NOTE]
> Application Insights deneyiminde Application Insights klasik kaynak sorguları, çalışma kitapları ve günlük tabanlı uyarılarınız için de tam geriye doğru uyumluluk sağlıyoruz. [Yeni çalışma alanı tabanlı tablo yapısına/şemasına](apm-tables.md) karşı sorgu/görünüm için öncelikle Log Analytics çalışma alanınıza gitmeniz gerekir. Önizleme sırasında, Application Insights bölmeleri içinden **günlüklerin** seçilmesi, klasik Application Insights sorgu deneyimine erişmenizi sağlayacaktır.

## <a name="copy-the-connection-string"></a>Bağlantı dizesini kopyalayın

[Bağlantı dizesi](https://docs.microsoft.com/azure/azure-monitor/app/sdk-connection-string?tabs=net) , telemetri verilerinizi ilişkilendirmek istediğiniz kaynağı tanımlar. Ayrıca, kaynağınızın telemetri için hedef olarak kullanacağı uç noktaları değiştirmenize de olanak tanır. Bağlantı dizesini kopyalamanız ve uygulamanızın koduna veya bir ortam değişkenine eklemeniz gerekir.

## <a name="monitoring-configuration"></a>İzleme yapılandırması

Çalışma alanı tabanlı Application Insights kaynak oluşturulduktan sonra izlemeyi yapılandırmak nispeten basittir.

### <a name="code-based-application-monitoring"></a>Kod tabanlı uygulama izleme

Kod tabanlı uygulama izleme için, yalnızca uygun Application Insights SDK 'sını yüklemeli ve yeni oluşturulan kaynağınız için izleme anahtarına veya bağlantı dizesine işaret edersiniz.  

Kod tabanlı izleme için bir Application Insights SDK 'Sı ayarlamaya yönelik ayrıntılı belgeler için dile/çerçeveye özgü belgelere başvurun:

- [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net)
- [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)
- [Modern konsol uygulamaları & arka plan görevleri (.NET/.NET Core)](https://docs.microsoft.com/azure/azure-monitor/app/worker-service)
- [Klasik konsol uygulamaları (.NET)](https://docs.microsoft.com/azure/azure-monitor/app/console) 
- [Java](https://docs.microsoft.com/azure/azure-monitor/app/java-get-started?tabs=maven)
- [JavaScript](https://docs.microsoft.com/azure/azure-monitor/app/javascript)
- [Node.js](https://docs.microsoft.com/azure/azure-monitor/app/nodejs)
- [Python](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python)

### <a name="codeless-monitoring-and-visual-studio-resource-creation"></a>Codeless izleme ve Visual Studio kaynak oluşturma

Azure Işlevleri ve Azure uygulama hizmetleri gibi Hizmetleri birlikte izlemek için, önce çalışma alanı tabanlı Application Insights kaynağınızı oluşturmanız ve ardından izleme yapılandırma aşamasında bu kaynağı işaret etmeniz gerekir.

Bu hizmetler, kendi kaynak oluşturma sürecinde yeni bir Application Insights kaynağı oluşturma seçeneği sunurken, bu kullanıcı arabirimi seçenekleri aracılığıyla oluşturulan kaynaklar şu anda klasik Application Insights deneyimiyle kısıtlıdır.

Aynı durum, ASP.NET ve ASP.NET Core için Visual Studio 'daki Application Insights kaynak oluşturma deneyimi için geçerlidir. Visual Studio izleme etkinleştirme Kullanıcı arabirimi ile içinden var olan bir çalışma alanı tabanlı kaynağı seçmeniz gerekir. Visual Studio içinden yeni kaynak oluştur ' a seçilirse, bu, klasik Application Insights kaynağı oluşturma ile sınırlandıracaktır.

## <a name="creating-a-resource-automatically"></a>Otomatik olarak kaynak oluşturma

### <a name="azure-cli"></a>Azure CLI

Azure CLı komutlarına önizleme Application Insights erişmek için öncelikle şunu çalıştırmanız gerekir:

```azurecli
 az extension add -n application-insights
```

Komutu çalıştırmazsanız `az extension add` , şu durumlarda bir hata iletisi görürsünüz:`az : ERROR: az monitor: 'app-insights' is not in the 'az monitor' command group. See 'az monitor --help'.`

Artık Application Insights kaynağınız oluşturmak için aşağıdakileri çalıştırabilirsiniz:

```azurecli
az monitor app-insights component create --app
                                         --location
                                         --resource-group
                                         [--application-type]
                                         [--ingestion-access {Disabled, Enabled}]
                                         [--kind]
                                         [--only-show-errors]
                                         [--query-access {Disabled, Enabled}]
                                         [--tags]
                                         [--workspace]
```

#### <a name="example"></a>Örnek

```azurecli
az monitor app-insights component create --app demoApp --location eastus --kind web -g my_resource_group --workspace "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/test1234/providers/microsoft.operationalinsights/workspaces/test1234555"
```

Bu komutla ilgili tam Azure CLı belgeleri için [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/ext/application-insights/monitor/app-insights/component?view=azure-cli-latest#ext-application-insights-az-monitor-app-insights-component-create)başvurun.

### <a name="azure-powershell"></a>Azure PowerShell

`New-AzApplicationInsights`PowerShell komutu şu anda çalışma alanı tabanlı Application Insights kaynağı oluşturmayı desteklememektedir. PowerShell ile çalışma alanı tabanlı bir kaynak oluşturmak için aşağıdaki Azure Resource Manager şablonları kullanabilir ve PowerShell ile dağıtabilirsiniz.

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

#### <a name="template-file"></a>Şablon dosyası

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "string"
        },
        "type": {
            "type": "string"
        },
        "regionId": {
            "type": "string"
        },
        "tagsArray": {
            "type": "object"
        },
        "requestSource": {
            "type": "string"
        },
        "workspaceResourceId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('name')]",
            "type": "microsoft.insights/components",
            "location": "[parameters('regionId')]",
            "tags": "[parameters('tagsArray')]",
            "apiVersion": "2020-02-02-preview",
            "properties": {
                "ApplicationId": "[parameters('name')]",
                "Application_Type": "[parameters('type')]",
                "Flow_Type": "Redfield",
                "Request_Source": "[parameters('requestSource')]",
                "WorkspaceResourceId": "[parameters('workspaceResourceId')]"
            }
        }
    ]
}
```

#### <a name="parameters-file"></a>Parametre dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "type": {
            "value": "web"
        },
        "name": {
            "value": "customresourcename"
        },
        "regionId": {
            "value": "eastus"
        },
        "tagsArray": {
            "value": {}
        },
        "requestSource": {
            "value": "Custom"
        },
        "workspaceResourceId": {
            "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/my_resource_group/providers/microsoft.operationalinsights/workspaces/myworkspacename"
        }
    }
}

```

## <a name="modifying-the-associated-workspace"></a>İlişkili çalışma alanını değiştirme

Çalışma alanı tabanlı bir Application Insights kaynağı oluşturulduktan sonra, ilişkili Log Analytics çalışma alanını değiştirebilirsiniz.

Application Insights kaynak bölmesi içinden **Özellikler**  >  **çalışma alanını Değiştir**  >  **Log Analytics çalışma alanlarını** seçin

## <a name="export-telemetry"></a>Telemetriyi dışarı aktarma

Eski sürekli dışa aktarma işlevselliği, çalışma alanı tabanlı kaynaklar için desteklenmez. Bunun yerine, Application Insights kaynağınız içinden **Tanılama ayarları**  >  **Tanılama ayarını Ekle** ' yi seçin. Bir depolama hesabına arşivlemek veya bir Azure Olay Hub 'ına akış yapmak için tüm tabloları veya tabloların bir alt kümesini seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Ölçümleri keşfetme](../../azure-monitor/platform/metrics-charts.md)
* [Analytics sorguları yazma](../../azure-monitor/app/analytics.md)

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[metrics]: ../../azure-monitor/platform/metrics-charts.md
[start]: ../../azure-monitor/app/app-insights-overview.md
