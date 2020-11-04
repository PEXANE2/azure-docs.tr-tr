---
title: Azure Güvenlik Duvarı günlüklerini ve ölçümlerini izleme
description: Bu makalede, Azure Güvenlik Duvarı günlüklerinin ve ölçümlerinin nasıl etkinleştirileceğini ve yönetileceğini öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 11/04/2020
ms.author: victorh
ms.openlocfilehash: 2899121db4b6a3f202be4860e2e4f43027cdef7c
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93348778"
---
# <a name="monitor-azure-firewall-logs-and-metrics"></a>Azure Güvenlik Duvarı günlüklerini ve ölçümlerini izleme

Güvenlik duvarı günlüklerini kullanarak Azure Güvenlik Duvarı'nı izleyebilirsiniz. Ayrıca etkinlik günlüklerini kullanarak Azure Güvenlik Duvarı kaynaklarıyla ilgili işlemleri denetleyebilirsiniz. Ölçümleri kullanarak portalda performans sayaçlarını görüntüleyebilirsiniz.

Bu günlüklerden bazılarına portaldan erişebilirsiniz. Günlükler [Azure izleyici](../azure-monitor/insights/azure-networking-analytics.md)günlüklerine, depolamaya ve Event Hubs gönderilebilir ve Azure izleyici günlüklerinde veya Excel ve Power BI gibi farklı araçlarla analiz edilebilir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce Azure Güvenlik Duvarı için kullanılabilen tanılama günlüklerine ve ölçümlere genel bir bakış için [Azure Güvenlik Duvarı günlüklerini ve ölçümlerini](logs-and-metrics.md) okumalısınız.

## <a name="enable-diagnostic-logging-through-the-azure-portal"></a>Azure portaldan tanılama günlüğüne kaydetmeyi etkinleştirme

Tanılama günlüğüne kaydetme işlemi etkinleştirildikten sonra verilerin günlükte görünmesi birkaç dakika sürebilir. İlk seferde görünen veri olmazsa birkaç dakika sonra tekrar deneyin.

1. Azure portal, güvenlik duvarı kaynak grubunuzu açın ve güvenlik duvarını seçin.
2. **İzleme** bölümünde **Tanılama ayarları** 'nı seçin.

   Azure Güvenlik Duvarı için hizmete özgü dört günlük kullanılabilir:

   * AzureFirewallApplicationRule
   * AzureFirewallNetworkRule
   * AzureFirewallThreatIntelLog
   * AzureFirewallDnsProxy


3. **Tanılama ayarı Ekle** ' yi seçin. **Tanılama ayarları** sayfasında tanılama günlükleriyle ilgili ayarlar bulunur.
5. Bu örnekte, Azure Izleyici günlükleri günlükleri depolar, bu nedenle ad için **güvenlik duvarı Log Analytics** yazın.
6. **Günlük** altında, günlükleri toplamak için **AzureFirewallApplicationRule** , **AzureFirewallNetworkRule** , **AzureFirewallThreatIntelLog** ve **AzureFirewallDnsProxy** öğesini seçin.
7. Çalışma alanınızı yapılandırmak için **Log Analytics gönder** ' i seçin.
8. Aboneliğinizi seçin.
9. **Kaydet** ’i seçin.

## <a name="enable-logging-with-powershell"></a>PowerShell ile günlüğe kaydetmeyi etkinleştirme

Etkinlik günlüğü tüm Kaynak Yöneticisi kaynakları için otomatik olarak etkinleştirilir. Bu günlükler aracılığıyla sunulan verileri toplamaya başlamak için tanılama günlüğüne kaydetme işlevinin etkinleştirilmesi gerekir.

Tanılama günlüğüne kaydetmeyi etkinleştirmek için aşağıdaki adımları izleyin:

1. Günlük verilerinin depolandığı depolama hesabınızın kaynak kimliğini not edin. Bu değer şu biçimdedir: */Subscriptions/ \<subscriptionId\> /ResourceGroups/ \<resource group name\> /providers/Microsoft.Storage/storageAccounts/ \<storage account name\>*.

   Aboneliğinizdeki herhangi bir depolama hesabını kullanabilirsiniz. Bu bilgileri Azure portalda bulabilirsiniz. Bu bilgiler kaynağın **Özellik** sayfasında yer alır.

2. Günlüğe kaydetmenin etkinleştirildiği güvenlik duvarının kaynak kimliğini not edin. Bu değer şu biçimdedir: */Subscriptions/ \<subscriptionId\> /ResourceGroups/ \<resource group name\> /providers/Microsoft.Network/azureFirewalls/ \<Firewall name\>*.

   Bu bilgileri portalda bulabilirsiniz.

3. Tanılama günlüğüne kaydetmeyi etkinleştirmek için aşağıdaki PowerShell cmdlet'ini kullanın:

    ```powershell
    Set-AzDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/azureFirewalls/<Firewall name> `
   -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> `
   -Enabled $true     
    ```

> [!TIP]
>Tanılama günlükleri için ayrı bir depolama hesabına gerek yoktur. Depolama alanının erişim ve performans günlüğü kaydı için kullanılması durumunda hizmet ücreti tahsil edilir.

## <a name="enable-diagnostic-logging-by-using-azure-cli"></a>Azure CLı kullanarak tanılama günlüğünü etkinleştirme

Etkinlik günlüğü tüm Kaynak Yöneticisi kaynakları için otomatik olarak etkinleştirilir. Bu günlükler aracılığıyla sunulan verileri toplamaya başlamak için tanılama günlüğüne kaydetme işlevinin etkinleştirilmesi gerekir.

[!INCLUDE [azure-cli-prepare-your-environment-h3.md](../../includes/azure-cli-prepare-your-environment-h3.md)]

### <a name="enable-diagnostic-logging"></a>Tanılama günlüğünü etkinleştirme

Tanılama günlük kaydını etkinleştirmek için aşağıdaki komutları kullanın.

1. Tanılama günlüğünü etkinleştirmek için [az Monitor Diagnostic-Settings Create](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create) komutunu çalıştırın:

   ```azurecli
   az monitor diagnostic-settings create –name AzureFirewallApplicationRule \
     --resource Firewall07 --storage-account MyStorageAccount
   ```

   Bir kaynağın tanılama ayarlarını görmek için [az Monitor Diagnostic-Settings List](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_list) komutunu çalıştırın:

   ```azurecli
   az monitor diagnostic-settings list --resource Firewall07
   ```

   Bir kaynağın etkin tanılama ayarlarını görmek için [az Monitor Diagnostic-Settings Show göster](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_show) ' i kullanın:

   ```azurecli
   az monitor diagnostic-settings show --name AzureFirewallApplicationRule --resource Firewall07
   ```

1. Ayarları güncelleştirmek için [az Monitor Diagnostic-Settings Update](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_update) komutunu çalıştırın.

   ```azurecli
   az monitor diagnostic-settings update --name AzureFirewallApplicationRule --resource Firewall07 --set retentionPolicy.days=365
   ```

   Bir tanılama ayarını silmek için [az Monitor Diagnostic-Settings Delete](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_delete) komutunu kullanın.

   ```azurecli
   az monitor diagnostic-settings delete --name AzureFirewallApplicationRule --resource Firewall07
   ```

> [!TIP]
>Tanılama günlükleri için ayrı bir depolama hesabına gerek yoktur. Depolama alanının erişim ve performans günlüğü kaydı için kullanılması durumunda hizmet ücreti tahsil edilir.

## <a name="view-and-analyze-the-activity-log"></a>Etkinlik günlüğünü görüntüleme ve analiz etme

Aşağıdaki yöntemlerden birini kullanarak etkinlik günlüğü verilerini görüntüleyebilir ve analiz edebilirsiniz:

* **Azure araçları** : Etkinlik günlüğü verilerini Azure PowerShell, Azure CLI, Azure REST API veya Azure portal üzerinden alabilirsiniz. Her yöntemle ilgili ayrıntılı adımlar [Kaynak Yöneticisi etkinlik işlemleri](../azure-resource-manager/management/view-activity-logs.md) makalesinde ayrıntılı bir şekilde anlatılmıştır.
* **Power BI** : [Power BI](https://powerbi.microsoft.com/pricing) hesabınız yoksa ücretsiz oluşturabilirsiniz. [Power BI için Azure Activity Logs içerik paketi](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) ile verilerinizi önceden yapılandırılmış panoları olduğu gibi veya değiştirerek kullanarak analiz edebilirsiniz.
* **Azure Sentinel** : Azure Güvenlik Duvarı günlüklerini Azure Sentinel 'e bağlanarak, çalışma kitaplarında günlük verilerini görüntülemenize, özel uyarılar oluşturmak için bu uygulamayı kullanmanıza ve araştırmanızı iyileştirebilecek şekilde katabilirsiniz. Azure Sentinel 'deki Azure Güvenlik Duvarı veri Bağlayıcısı Şu anda genel önizlemededir. Daha fazla bilgi için bkz. [Azure güvenlik duvarından verileri bağlama](../sentinel/connect-azure-firewall.md).

## <a name="view-and-analyze-the-network-and-application-rule-logs"></a>Ağ ve uygulama kuralı günlüklerini görüntüleme ve analiz etme

[Azure izleyici günlükleri](../azure-monitor/insights/azure-networking-analytics.md) sayaç ve olay günlüğü dosyalarını toplar. Günlüklerinizi analiz etmek için görselleştirmelere ve güçlü arama özelliklerine sahiptir.

Azure Güvenlik Duvarı günlük Analizi örnek sorguları için bkz. [Azure Güvenlik Duvarı günlük Analizi örnekleri](log-analytics-samples.md).

Dilerseniz depolama hesabınıza bağlanabilir ve JSON erişim günlüklerini ve performans günlüklerini alabilirsiniz. İndirdiğiniz JSON dosyalarını CSV biçimine dönüştürebilir ve Excel, Power BI veya diğer veri görselleştirme araçlarında görüntüleyebilirsiniz.

> [!TIP]
> Visual Studio ve C# ile sabit ve değişken değerlerini değiştirme konusunda temel kavramlara hakimseniz GitHub'daki [günlük dönüştürücü araçlarını](https://github.com/Azure-Samples/networking-dotnet-log-converter) kullanabilirsiniz.

## <a name="view-metrics"></a>Ölçümleri görüntüle
Azure Güvenlik Duvarı ' na giderek **Izleme** **ölçüm** ' i seçin. Kullanılabilir değerleri görüntülemek için **ÖLÇÜM** açılan listesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Güvenlik duvarınızı günlükleri toplayacak şekilde yapılandırdığınıza göre, verilerinizi görüntülemek için Azure Izleyici günlüklerini keşfedebilirsiniz.

[Azure Izleyici günlüklerinde ağ izleme çözümleri](../azure-monitor/insights/azure-networking-analytics.md)
