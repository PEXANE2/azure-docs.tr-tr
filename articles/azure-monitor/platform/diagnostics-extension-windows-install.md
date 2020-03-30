---
title: Windows Azure tanılama uzantısını yükleme ve yapılandırma (WAD)
description: Azure tanılama verilerini bir Azure Depolama hesabında nasıl topladığınızı öğrenin, böylece azure verilerini kullanılabilir birkaç araçtan biriyle görüntüleyebilirsiniz.
services: azure-monitor
author: bwren
ms.subservice: diagnostic-extension
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: bwren
ms.openlocfilehash: 929ab4109eb8d0e90b6c561a2135c0b7dd4205bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77672268"
---
# <a name="install-and-configure-windows-azure-diagnostics-extension-wad"></a>Windows Azure tanılama uzantısını yükleme ve yapılandırma (WAD)
Azure tanılama uzantısı, Azure Monitor'da konuk işletim sisteminden izleme verileri ve Azure sanal makineleri ve diğer bilgi işlem kaynaklarının iş yüklerini toplayan bir aracıdır. Bu makalede, Windows tanılama uzantısının yüklenmesi ve yapılandırılması ve verilerin ve Azure Depolama hesabında nasıl depolandırıldığına ilişkin açıklama ayrıntıları verilmektedir.

Tanılama uzantısı Azure'da [sanal makine uzantısı](../../virtual-machines/extensions/overview.md) olarak uygulandığından, Kaynak Yöneticisi şablonları, PowerShell ve CLI'yi kullanarak aynı yükleme seçeneklerini destekler. Sanal [makine uzantılarının](../../virtual-machines/extensions/features-windows.md) yüklenmesi ve bakımı yla ilgili ayrıntılar için Windows için Sanal makine uzantıları ve özelliklerine bakın.

## <a name="install-with-azure-portal"></a>Azure portalı ile yükleme
Azure portalında, doğrudan yapılandırmayla çalışmak yerine bir arayüz sağlayan tek bir sanal makinede tanılama uzantısını yükleyebilir ve yapılandırabilirsiniz. Tanılama uzantısını etkinleştirdiğinizde, en yaygın performans sayaçları ve olayları içeren varsayılan yapılandırmayı otomatik olarak kullanır. Bu varsayılan yapılandırmayı özel gereksinimlerinize göre değiştirebilirsiniz.

> [!NOTE]
> Azure Etkinlik Hub'larına veri göndermek de dahil olmak üzere Azure portalını kullanarak yapılandıramadığınız tanılama uzantısı ayarları vardır. Bu ayarlar için diğer yapılandırma yöntemlerinden birini kullanmanız gerekir.

1. Azure portalında sanal bir makinenin menüsünü açın.
2. VM menüsünün **İzleme** bölümündeki **Tanılama ayarlarına** tıklayın.
3. Tanılama uzantısı zaten etkinleştirilmemişse **konuk düzeyinde izlemeyi etkinleştir'i** tıklatın.
4. VM için yeni bir Azure Depolama hesabı oluşturulacak ve bu ad VM'nin kaynak grubunun adını temel alır. **Aracı** sekmesini seçerek VM'yi başka bir depolama hesabına ekleyebilirsiniz.

![Tanılama ayarları](media/diagnostics-extension-windows-install/diagnostic-settings.png)


Tanılama uzantısı etkinleştirildikten sonra varsayılan yapılandırmayı değiştirebilirsiniz. Aşağıdaki tabloda, farklı sekmelerde değiştirebileceğiniz seçenekler açıklanmaktadır. Bazı seçenekler, daha ayrıntılı yapılandırma belirtmenize olanak tanıyan **özel** bir komuta sahiptir; farklı ayarlar la ilgili ayrıntılar için [Windows tanılama uzantısı şemasına](diagnostics-extension-schema-windows.md) bakın.

| Tab | Açıklama |
|:---|:---|
| Genel Bakış | Geçerli yapılandırmayı diğer sekmelere bağlantılarla görüntüler. |
| Performans sayaçları | Toplamak için performans sayaçlarını ve her biri için örnek oranını seçin.  |
| Günlükler | Toplamak için günlük verilerini seçin. Buna Windows Event günlükleri, IIS günlükleri, .NET uygulama günlükleri ve ETW olayları dahildir.  |
| Kilitlenme bilgi dökümleri | Farklı işlemler için kilitlenme dökümü etkinleştirin. |
| Lavabo | Azure Depolama'ya ek olarak hedeflere veri göndermek için veri lavabolarının etkinolmasını etkinleştirin.<br>Azure Monitörü - Performans verilerini Azure Monitör Ölçümleri'ne gönderir.<br>Uygulama Öngörüleri - Bir Application Insights uygulamasına veri gönderin. |
| Aracı | Aracı için aşağıdaki yapılandırmayı değiştirin:<br>- Depolama hesabını değiştirin.<br>- Aracı için kullanılan maksimum yerel diski belirtin.<br>- Aracının kendisi için günlükleri yapılandırın.|


> [!NOTE]
> Tanılama uzantısı yapılandırması JSON veya XML olarak biçimlendirilebilir, ancak Azure portalında yapılan tüm yapılandırmalar her zaman JSON olarak depolanır. XML'i başka bir yapılandırma yöntemiyle kullanır ve azure portalıyla yapılandırmanızı değiştirirseniz, ayarlar JSON olarak değiştirilir.

## <a name="resource-manager-template"></a>Resource Manager şablonu
Bkz. Tanılama uzantısını Azure Kaynak Yöneticisi şablonlarıyla dağıtma konusunda [Windows VM ve Azure Kaynak Yöneticisi şablonlarıyla izleme ve tanılama](../../virtual-machines/extensions/diagnostics-template.md) yı kullan. 

## <a name="azure-cli-deployment"></a>Azure CLI dağıtımı
Azure CLI, Aşağıdaki örnekte olduğu gibi [az vm uzantı kümesini](https://docs.microsoft.com/cli/azure/vm/extension?view=azure-cli-latest#az-vm-extension-set) kullanarak Azure Tanılama uzantısını varolan bir sanal makineye dağıtmak için kullanılabilir. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name IaaSDiagnostics \
  --publisher Microsoft.Azure.Diagnostics \
  --protected-settings protected-settings.json \
  --settings public-settings.json 
```

Korumalı ayarlar, yapılandırma şemasının [PrivateConfig öğesinde](diagnostics-extension-schema-windows.md#privateconfig-element) tanımlanır. Aşağıda, depolama hesabını tanımlayan korumalı ayarlar dosyasının en az bir örneği verilmiştir. Özel ayarların tüm ayrıntıları için [Örnek yapılandırmaya](diagnostics-extension-schema-windows.md#privateconfig-element) bakın.

```JSON
{
    "storageAccountName": "mystorageaccount",
    "storageAccountKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "storageAccountEndPoint": "https://mystorageaccount.blob.core.windows.net"
}
```
Ortak ayarlar, yapılandırma şemasının [Ortak öğesinde](diagnostics-extension-schema-windows.md#publicconfig-element) tanımlanır. Tanılama altyapısı günlüklerinin, tek bir performans sayacının ve tek bir olay günlüğünün toplanmasını sağlayan genel ayarlar dosyasının en az bir örneği aşağıda verilmiştir. Genel ayarların tüm ayrıntıları için [Örnek yapılandırmaya](diagnostics-extension-schema-windows.md#publicconfig-element) bakın.

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
PowerShell, Aşağıdaki örnekte olduğu gibi [Set-AzVMDiagnosticsExtension'i](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurevmdiagnosticsextension) kullanarak Azure Tanılama uzantısını varolan bir sanal makineye dağıtmak için kullanılabilir. 

```powershell
Set-AzVMDiagnosticsExtension -ResourceGroupName "myvmresourcegroup" `
  -VMName "myvm" `
  -DiagnosticsConfigurationPath "DiagnosticsConfiguration.json"
```

Özel ayarlar [PrivateConfig öğesinde](diagnostics-extension-schema-windows.md#privateconfig-element)tanımlanırken, ortak ayarlar yapılandırma şemasının [Ortak öğesinde](diagnostics-extension-schema-windows.md#publicconfig-element) tanımlanır. Ayrıca, depolama hesabının ayrıntılarını özel ayarlara dahil etmek yerine Set-AzVMDiagnosticsExtension cmdlet'in parametreleri olarak belirtmeyi de seçebilirsiniz.

Tanılama altyapısı günlüklerinin, tek bir performans sayacının ve tek bir olay günlüğünün toplanmasını sağlayan yapılandırma dosyasının en az bir örneği aşağıda verilmiştir. Özel ve genel ayarların tüm ayrıntıları için [Örnek yapılandırmaya](diagnostics-extension-schema-windows.md#publicconfig-element) bakın. 

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

Ayrıca [Windows çalıştıran sanal bir makinede Azure Tanılama'yı etkinleştirmek için PowerShell'i kullanın.](../../virtual-machines/extensions/diagnostics-windows.md)

## <a name="data-storage"></a>Veri depolama
Aşağıdaki tablo, tanılama uzantısından toplanan farklı veri türlerini ve bunların tablo veya leke olarak depolanıp depolanmadığını listeler. Tablolarda depolanan veriler, genel yapılandırmanızdaki [StorageType ayarına](diagnostics-extension-schema-windows.md#publicconfig-element) bağlı olarak blob'larda da depolanabilir.


| Veri | Depolama türü | Açıklama |
|:---|:---|:---|
| WADDiagnosticInfrastructureLogsTable | Tablo | Tanımonitörü ve yapılandırma değişiklikleri. |
| WADDirectoriesTablo | Tablo | Tanı monitörün izlediği dizinler.  Buna IIS günlükleri, IIS başarısız istek günlükleri ve özel dizinler dahildir.  Blob günlük dosyasının konumu Kapsayıcı alanında belirtilir ve blob adı RelativePath alanındadır.  AbsolutePath alanı, Azure sanal makinesinde var olduğu gibi dosyanın konumunu ve adını gösterir. |
| WadLogsTable | Tablo | İzleme dinleyicisi kullanılarak kodla yazılmış günlükler. |
| WADPerformanceCountersTable | Tablo | Performans sayaçları. |
| WADWindowsEventLogsTable | Tablo | Windows Olay günlükleri. |
| wad-iis-failedreqlogfiles | Blob | IIS Başarısız İstek günlüklerinden bilgiler içerir. |
| wad-iis-logfiles | Blob | IIS günlükleri hakkında bilgi içerir. |
| "özel" | Blob | Tanılama monitörü tarafından izlenen dizinleri yapılandırmaya dayalı özel bir kapsayıcı.  Bu blob kapsayıcının adı WADDirectoriesTable'da belirtilir. |

## <a name="tools-to-view-diagnostic-data"></a>Tanılama verilerini görüntülemek için araçlar
Depolama alanına aktarıldıktan sonra verileri görüntülemek için çeşitli araçlar kullanılabilir. Örnek:

* Visual Studio'da Sunucu Gezgini - Microsoft Visual Studio için Azure Araçlarını yüklediyseniz, Azure depolama hesaplarınızdaki salt okunur blob ve tablo verilerini görüntülemek için Server Explorer'daki Azure Depolama düğümunu kullanabilirsiniz. Verileri yerel depolama emülatör hesabınızdan ve Azure için oluşturduğunuz depolama hesaplarından görüntüleyebilirsiniz. Daha fazla bilgi için Bkz. [Sunucu Gezgini ile Depolama Kaynaklarını Tarama ve Yönetme.](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage)
* [Microsoft Azure Depolama Gezgini,](../../vs-azure-tools-storage-manage-with-storage-explorer.md) Windows, OSX ve Linux'taki Azure Depolama verileriyle kolayca çalışmanızı sağlayan bağımsız bir uygulamadır.
* [Azure Yönetim Stüdyosu,](https://www.cerebrata.com/products/azure-management-studio/introduction) Azure'da çalışan uygulamalar tarafından toplanan tanılama verilerini görüntülemenize, indirmenize ve yönetmenize olanak tanıyan Azure Diagnostics Manager'ı içerir.

## <a name="next-steps"></a>Sonraki Adımlar
- İzleme verilerini Azure Etkinlik Hub'larına iletme yle ilgili ayrıntılar için [Windows Azure tanılama uzantısından Olay Hub'larına veri gönderme](diagnostics-extension-stream-event-hubs.md) konusuna bakın.
