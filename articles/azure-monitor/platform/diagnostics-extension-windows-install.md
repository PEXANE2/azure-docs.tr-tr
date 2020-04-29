---
title: Windows Azure tanılama uzantısı 'nı (WAD) yükleyip yapılandırma
description: Azure tanılama verilerini bir Azure depolama hesabında nasıl toplayacağınızı öğrenmek için, mevcut araçlardan birini kullanarak görüntüleyebilirsiniz.
services: azure-monitor
author: bwren
ms.subservice: diagnostic-extension
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: bwren
ms.openlocfilehash: 929ab4109eb8d0e90b6c561a2135c0b7dd4205bb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77672268"
---
# <a name="install-and-configure-windows-azure-diagnostics-extension-wad"></a>Windows Azure tanılama uzantısı 'nı (WAD) yükleyip yapılandırma
Azure tanılama uzantısı, Azure Izleyici 'de Konuk işletim sisteminden ve Azure sanal makinelerinin ve diğer işlem kaynaklarının iş yüklerinden izleme verilerini toplayan bir aracıdır. Bu makalede, Windows Tanılama uzantısı 'nı yükleme ve yapılandırma hakkında ayrıntılar ve verilerin ve Azure depolama hesabında nasıl depolandığı hakkında bir açıklama sağlanmaktadır.

Tanılama uzantısı, Azure 'da bir [sanal makine uzantısı](../../virtual-machines/extensions/overview.md) olarak uygulanır, bu nedenle Kaynak Yöneticisi şablonları, POWERSHELL ve CLI kullanarak aynı yükleme seçeneklerini destekler. Sanal makine uzantılarının yüklenmesi ve bakımında ilgili ayrıntılar için bkz. [Windows Için sanal makine uzantıları ve özellikleri](../../virtual-machines/extensions/features-windows.md) .

## <a name="install-with-azure-portal"></a>Azure portal ile yüklensin
Tanılama uzantısını, yapılandırma ile doğrudan çalışmaktan farklı bir arabirim sağlayan Azure portal tek bir sanal makineye yükleyebilir ve yapılandırabilirsiniz. Tanılama uzantısını etkinleştirdiğinizde, en yaygın performans sayaçları ve olayları ile otomatik olarak varsayılan bir yapılandırma kullanılır. Bu varsayılan yapılandırmayı, özel gereksinimlerinize göre değiştirebilirsiniz.

> [!NOTE]
> Azure Event Hubs 'a veri gönderme dahil Azure portal kullanarak yapılandırayükleyemezsiniz tanılama uzantısı ayarları vardır. Bu ayarlar için diğer yapılandırma yöntemlerinden birini kullanmalısınız.

1. Azure portal bir sanal makinenin menüsünü açın.
2. VM menüsünün **izleme** bölümünde **Tanılama ayarları** ' na tıklayın.
3. Tanılama uzantısı zaten etkinleştirilmemişse **Konuk düzeyinde Izlemeyi etkinleştir** ' e tıklayın.
4. Adlı VM için yeni bir Azure depolama hesabı oluşturulacak ve sanal makine için kaynak grubunun adını temel alır. **Aracı** SEKMESINI seçerek VM 'yi başka bir depolama hesabına ekleyebilirsiniz.

![Tanılama ayarları](media/diagnostics-extension-windows-install/diagnostic-settings.png)


Tanılama uzantısı etkinleştirildikten sonra varsayılan yapılandırmayı değiştirebilirsiniz. Aşağıdaki tabloda, farklı sekmelerde değiştirebileceğiniz seçenekler açıklanmaktadır. Bazı seçeneklerde, daha ayrıntılı yapılandırma belirtmenize olanak tanıyan **özel** bir komut vardır. farklı ayarlarla ilgili ayrıntılar için bkz. [Windows Tanılama uzantısı şeması](diagnostics-extension-schema-windows.md) .

| Tab | Açıklama |
|:---|:---|
| Genel Bakış | Geçerli yapılandırmayı diğer sekmelerin bağlantılarıyla birlikte görüntüler. |
| Performans sayaçları | Toplanacak performans sayaçlarını ve her birinin örnek hızını seçin.  |
| Günlükler | Toplanacak günlük verilerini seçin. Buna Windows olay günlükleri, IIS günlükleri, .NET uygulama günlükleri ve ETW olayları dahildir.  |
| Kilitlenme bilgi dökümleri | Farklı süreçler için kilitlenme dökümünü etkinleştirin. |
| Yapma | Azure depolama 'ya ek olarak hedefe veri göndermek için veri havuzlarını etkinleştirin.<br>Azure Izleyici-Azure Izleyici ölçümlerine performans verileri gönderir.<br>Application Insights-verileri bir Application Insights uygulamasına gönderin. |
| Aracı | Aracı için aşağıdaki yapılandırmayı değiştirin:<br>-Depolama hesabını değiştirin.<br>-Aracı için kullanılan en yüksek yerel diski belirtin.<br>-Aracının kendisinin sistem durumu için günlükleri yapılandırın.|


> [!NOTE]
> Tanılama uzantısının yapılandırması JSON veya XML içinde biçimlendirilese de, Azure portal yapılan tüm yapılandırmalar her zaman JSON olarak depolanır. Başka bir yapılandırma yöntemiyle XML kullanırsanız ve sonra Azure portal yapılandırmanızı değiştirirseniz, ayarlar JSON olarak değiştirilir.

## <a name="resource-manager-template"></a>Resource Manager şablonu
Tanılama uzantısını Azure Resource Manager şablonlarıyla dağıtma hakkında [bir Windows sanal makinesi ve Azure Resource Manager şablonları ile izleme ve tanılamayı kullanma](../../virtual-machines/extensions/diagnostics-template.md) konusuna bakın. 

## <a name="azure-cli-deployment"></a>Azure CLı dağıtımı
Azure CLı, aşağıdaki örnekte olduğu gibi [az VM Extension set](https://docs.microsoft.com/cli/azure/vm/extension?view=azure-cli-latest#az-vm-extension-set) kullanılarak mevcut bir sanal makineye Azure tanılama uzantısını dağıtmak için kullanılabilir. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name IaaSDiagnostics \
  --publisher Microsoft.Azure.Diagnostics \
  --protected-settings protected-settings.json \
  --settings public-settings.json 
```

Korunan ayarlar, yapılandırma şemasının [Privateconfig öğesinde](diagnostics-extension-schema-windows.md#privateconfig-element) tanımlanmıştır. Aşağıda, depolama hesabını tanımlayan korumalı ayarlar dosyasının en az bir örneği verilmiştir. Özel ayarların tüm ayrıntıları için bkz. [örnek yapılandırma](diagnostics-extension-schema-windows.md#privateconfig-element) .

```JSON
{
    "storageAccountName": "mystorageaccount",
    "storageAccountKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "storageAccountEndPoint": "https://mystorageaccount.blob.core.windows.net"
}
```
Ortak ayarlar, yapılandırma şemasının [ortak öğesinde](diagnostics-extension-schema-windows.md#publicconfig-element) tanımlanmıştır. Aşağıda, tanılama altyapı günlüklerinin toplanması, tek bir performans sayacı ve tek bir olay günlüğü sağlayan bir genel ayarlar dosyası örneği verilmiştir. Genel ayarların tüm ayrıntıları için bkz. [örnek yapılandırma](diagnostics-extension-schema-windows.md#publicconfig-element) .

```JSON
{
  "StorageAccount": "mystorageaccount",
  "WadCfg": {
    "DiagnosticMonitorConfiguration": {
      "overallQuotaInMB": 5120,
      "PerformanceCounters": {
        "scheduledTransferPeriod": "PT1M",
        "PerformanceCounterConfiguration": [
          {
            "counterSpecifier": "\\Processor Information(_Total)\\% Processor Time",
            "unit": "Percent",
            "sampleRate": "PT60S"
          }
        ]
      },
      "WindowsEventLog": {
        "scheduledTransferPeriod": "PT1M",
        "DataSource": [
          {
            "name": "Application!*[System[(Level=1 or Level=2 or Level=3)]]"
          }
        ]
      }
    }
  }
}
```



## <a name="powershell-deployment"></a>PowerShell dağıtımı
PowerShell, aşağıdaki örnekte olduğu gibi [set-Azvmdiagnosticsextenma](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurevmdiagnosticsextension) kullanılarak mevcut bir sanal makineye Azure tanılama uzantısını dağıtmak için kullanılabilir. 

```powershell
Set-AzVMDiagnosticsExtension -ResourceGroupName "myvmresourcegroup" `
  -VMName "myvm" `
  -DiagnosticsConfigurationPath "DiagnosticsConfiguration.json"
```

Özel ayarlar [Privateconfig öğesinde](diagnostics-extension-schema-windows.md#privateconfig-element)tanımlanır, ancak genel ayarlar yapılandırma şemasının [ortak öğesinde](diagnostics-extension-schema-windows.md#publicconfig-element) tanımlanmıştır. Depolama hesabının ayrıntılarını özel ayarlara dahil etmek yerine set-Azvmdiagnosticsextenma cmdlet 'inin parametreleri olarak belirtmeyi de tercih edebilirsiniz.

Aşağıda, tanılama altyapısı günlüklerinin, tek bir performans sayacının ve tek bir olay günlüğünün toplanmasını sağlayan bir yapılandırma dosyasının en az bir örneği verilmiştir. Özel ve genel ayarların tüm ayrıntıları için bkz. [örnek yapılandırma](diagnostics-extension-schema-windows.md#publicconfig-element) . 

```JSON
{
    "PublicConfig": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
                "overallQuotaInMB": 10000
            },
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT3M",
                        "unit": "percent"
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT1M",
                    "DataSource": [
                    {
                        "name": "Application!*[System[(Level=1 or Level=2 or Level=3)]]"
                    }
                ]
            }
        },
        "StorageAccount": "mystorageaccount",
        "StorageType": "TableAndBlob"
    },
    "PrivateConfig": {
        "storageAccountName": "mystorageaccount",
        "storageAccountKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
        "storageAccountEndPoint": "https://mystorageaccount.blob.core.windows.net"
    }
}
```

Ayrıca bkz. [Windows çalıştıran bir sanal makinede Azure tanılama etkinleştirmek Için PowerShell 'ı kullanma](../../virtual-machines/extensions/diagnostics-windows.md).

## <a name="data-storage"></a>Veri depolama
Aşağıdaki tabloda, tanılama uzantısından toplanan farklı veri türleri ve bunların bir tablo ya da blob olarak saklanıp saklanmayacağı listelenmiştir. Tablolarda depolanan veriler ayrıca, ortak yapılandırmanızda [StorageType ayarına](diagnostics-extension-schema-windows.md#publicconfig-element) bağlı olarak bloblarda depolanabilir.


| Veriler | Depolama türü | Açıklama |
|:---|:---|:---|
| WADDiagnosticInfrastructureLogsTable | Tablo | Tanılama İzleyicisi ve yapılandırma değişiklikleri. |
| WADDirectoriesTable | Tablo | Tanılama izleyicisinin izlediği dizinler.  Buna IIS günlükleri, IIS başarısız istek günlükleri ve özel dizinler dahildir.  Blob günlük dosyasının konumu kapsayıcı alanında belirtilir ve BLOB adı RelativePath alanında bulunur.  AbsolutePath alanı, Azure sanal makinesinde var olan dosyanın konumunu ve adını gösterir. |
| WadLogsTable | Tablo | İzleme dinleyicisi kullanılarak kodda yazılan günlükler. |
| WADPerformanceCountersTable | Tablo | Performans sayaçları. |
| WADWindowsEventLogsTable | Tablo | Windows olay günlükleri. |
| WAD-IIS-failedreqlogfiles | Blob | IIS başarısız Istek günlüklerinden bilgiler içerir. |
| WAD-IIS-LogFiles | Blob | IIS günlükleriyle ilgili bilgiler içerir. |
| Özel | Blob | Tanılama İzleyicisi tarafından izlenen dizinleri yapılandırmaya dayalı özel bir kapsayıcı.  Bu blob kapsayıcısının adı WADDirectoriesTable içinde belirtilecektir. |

## <a name="tools-to-view-diagnostic-data"></a>Tanılama verilerini görüntülemek için Araçlar
Verileri depolama alanına aktarıldıktan sonra görüntülemek için kullanabileceğiniz çeşitli araçlar vardır. Örneğin:

* Visual Studio 'da Sunucu Gezgini-Azure araçlarını Microsoft Visual Studio yüklediyseniz, Azure depolama hesaplarınızdan salt okunurdur blob ve tablo verilerini görüntülemek için Sunucu Gezgini Azure Storage düğümünü kullanabilirsiniz. Yerel depolama öykünücü hesabınızdan ve ayrıca Azure için oluşturduğunuz depolama hesaplarından verileri görüntüleyebilirsiniz. Daha fazla bilgi için bkz. [Sunucu Gezgini Ile depolama kaynaklarına göz atma ve yönetme](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage).
* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md) , Windows, OSX ve Linux 'Ta Azure Depolama verileriyle kolayca çalışabilmenizi sağlayan tek başına bir uygulamadır.
* [Azure Management Studio](https://www.cerebrata.com/products/azure-management-studio/introduction) , Azure üzerinde çalışan uygulamalar tarafından toplanan tanılama verilerini görüntülemenize, yüklemenize ve yönetmenize olanak tanıyan Azure tanılama yöneticisini içerir.

## <a name="next-steps"></a>Sonraki Adımlar
- İzleme verilerini Azure Event Hubs 'a iletme hakkında daha fazla bilgi için bkz. [Windows Azure tanılama uzantısından veri gönderme Event Hubs](diagnostics-extension-stream-event-hubs.md) .
