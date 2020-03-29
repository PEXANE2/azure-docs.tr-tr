---
title: Otomatik ölçeklendirme ortak ölçümleri
description: Bulut Hizmetleri, Sanal Makineler ve Web Uygulamalarınızı otomatikleştirmek için hangi ölçümlerin yaygın olarak kullanıldığını öğrenin.
ms.topic: conceptual
ms.date: 12/6/2016
ms.subservice: autoscale
ms.openlocfilehash: 2c335168683212337876c963a7cfdb441d0ac69a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76845571"
---
# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure Monitörü ortak ölçümleri otomatik olarak ölçekleme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitor otomatik ölçeklendirme, telemetri verilerine (ölçümler) dayalı olarak çalışan örnek sayısını yukarı veya aşağı ölçeklendirmenize olanak tanır. Bu belge, kullanmak isteyebileceğiniz yaygın ölçümleri açıklar. Azure portalında, ölçeklendirilecek kaynağın metrik lerini seçebilirsiniz. Ancak, farklı bir kaynaktan ölçeklendirilecek herhangi bir metrik de seçebilirsiniz.

Azure Monitör otomatik ölçeklendirmesi yalnızca [Sanal Makine Ölçeği Kümeleri,](https://azure.microsoft.com/services/virtual-machine-scale-sets/) [Bulut Hizmetleri,](https://azure.microsoft.com/services/cloud-services/) [Uygulama Hizmeti - Web Uygulamaları](https://azure.microsoft.com/services/app-service/web/)ve [API Yönetimi hizmetleri](https://docs.microsoft.com/azure/api-management/api-management-key-concepts)için geçerlidir. Diğer Azure hizmetleri farklı ölçekleme yöntemleri kullanır.

## <a name="compute-metrics-for-resource-manager-based-vms"></a>Kaynak Yöneticisi tabanlı VM'ler için bilgi işlem ölçümleri
Varsayılan olarak, Kaynak Yöneticisi tabanlı Sanal Makineler ve Sanal Makine Ölçeği Kümeleri temel (ana bilgisayar düzeyinde) ölçümler imal eder. Ayrıca, bir Azure VM ve VMSS için tanılama veri koleksiyonunu yapılandırdığınızda, Azure tanılama uzantısı konuk işletim sistemi performans sayaçları da yakar (genellikle "konuk-İş'ler ölçümleri" olarak bilinir).  Tüm bu ölçümleri otomatik ölçek kurallarında kullanırsınız.

VMSS `Get MetricDefinitions` kaynağınız için kullanılabilir ölçümleri görüntülemek için API/PoSH/CLI'yi kullanabilirsiniz.

VM ölçek kümeleri kullanıyorsanız ve listelenen belirli bir metrik görmüyorsanız, tanılama uzantısıdevre *dışı* bırakılır.

Belirli bir metrik örnekleme yapılmazsa veya istediğiniz sıklıkta aktarılamazsa, tanılama yapılandırmasını güncelleştirebilirsiniz.

Önceki durumlardan biri doğruysa, Azure VM Diagnostics uzantınızı yapılandırmak ve metnini etkinleştirmek için güncelleştirmek için [Windows çalıştıran bir sanal makinede Azure Diagnostics'i](../../virtual-machines/extensions/diagnostics-windows.md) etkinleştirmek için PowerShell'i kullan'ı gözden geçirin. Bu makale, örnek tanılama yapılandırma dosyası da içerir.

### <a name="host-metrics-for-resource-manager-based-windows-and-linux-vms"></a>Kaynak Yöneticisi tabanlı Windows ve Linux VM'leri için ana bilgisayar ölçümleri
Aşağıdaki ana bilgisayar düzeyindeölçümler, hem Windows hem de Linux örneklerinde Azure VM ve VMSS için varsayılan olarak yayımlanır. Bu ölçümler Azure VM'nizi açıklar, ancak konuk VM'ye yüklenen aracı yerine Azure VM ana bilgisayardan toplanır. Bu ölçümleri otomatik ölçekleme kurallarında kullanabilirsiniz.

- [Kaynak Yöneticisi tabanlı Windows ve Linux VM'leri için ana bilgisayar ölçümleri](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines)
- [Kaynak Yöneticisi tabanlı Windows ve Linux VM Ölçek Setleri için ana bilgisayar ölçümleri](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachinescalesets)

### <a name="guest-os-metrics-for-resource-manager-based-windows-vms"></a>Kaynak Yöneticisi tabanlı Windows VM'leri için Konuk İşletim Sistemi ölçümleri
Azure'da bir VM oluşturduğunuzda, Tanılama uzantısı kullanılarak tanılama etkinleştirilir. Tanılama uzantısı VM içinden alınan ölçümler bir dizi yakar. Bu, varsayılan olarak yayımlanamayan ölçümleri otomatik olarak ölçeklendirebileceğiniz anlamına gelir.

PowerShell'de aşağıdaki komutu kullanarak ölçümlerin bir listesini oluşturabilirsiniz.

```
Get-AzMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Aşağıdaki ölçümler için bir uyarı oluşturabilirsiniz:

| Metrik Adı | Birim |
| --- | --- |
| \Processor(_Total)\% Processor Time |Yüzde |
| \Processor(_Total)\% Ayrıcalıklı Zaman |Yüzde |
| \Processor(_Total)\% Kullanıcı Süresi |Yüzde |
| \İşlemci Bilgileri(_Total)\İşlemci Frekansı |Sayı |
| \Sistem\İşlemler |Sayı |
| \İşlem(_Total)\İş Parçacığı Sayısı |Sayı |
| \İşlem(_Total)\Tutamak Sayısı |Sayı |
| \Kullanımda Kullanılan Bellek\% Taahhüt Baytlar |Yüzde |
| \Memory\Available Bytes |Bayt |
| \Bellek\Taahhüt Bayt |Bayt |
| \Bellek\Commit Limit |Bayt |
| \Bellek\Havuz Sayfalı Baytlar |Bayt |
| \Memory\Pool Nonpaged Bayt |Bayt |
| \PhysicalDisk(_Total)\% Disk Süresi |Yüzde |
| \PhysicalDisk(_Total)\% Disk Okuma Süresi |Yüzde |
| \PhysicalDisk(_Total)\% Disk Yazma Süresi |Yüzde |
| \PhysicalDisk(_Total)\Disk Aktarımları/sn |CountPerSecond |
| \PhysicalDisk(_Total)\Disk Okuma/sn |CountPerSecond |
| \PhysicalDisk(_Total)\Disk Yazma/sn |CountPerSecond |
| \PhysicalDisk(_Total)\Disk Bayt/sn |BaytPerİkinci |
| \PhysicalDisk(_Total)\Disk Okuma Bayt/sn |BaytPerİkinci |
| \PhysicalDisk(_Total)\Disk Bayt/sn Yazma |BaytPerİkinci |
| \PhysicalDisk(_Total)\Avg. Disk Sıra Uzunluğu |Sayı |
| \PhysicalDisk(_Total)\Avg. Disk Okuma Sırası Uzunluğu |Sayı |
| \PhysicalDisk(_Total)\Avg. Disk Yazma Sıra Uzunluğu |Sayı |
| \LogicalDisk(_Total)\% Boş Alan |Yüzde |
| \LogicalDisk(_Total)\Serbest Megabayt |Sayı |

### <a name="guest-os-metrics-linux-vms"></a>Konuk İşletim Sistemi ölçümleri Linux VM'ler
Azure'da bir VM oluşturduğunuzda, Tanılama uzantısı kullanılarak varsayılan olarak tanılama etkinleştirilir.

PowerShell'de aşağıdaki komutu kullanarak ölçümlerin bir listesini oluşturabilirsiniz.

```
Get-AzMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Aşağıdaki ölçümler için bir uyarı oluşturabilirsiniz:

| Metrik Adı | Birim |
| --- | --- |
| \Bellek\Kullanılabilir Bellek |Bayt |
| \Bellek\PercentKullanılabilir Bellek |Yüzde |
| \Bellek\Kullanılmış Bellek |Bayt |
| \Bellek\YüzdeKullanılan Bellek |Yüzde |
| \Bellek\YüzdeKullanılmış ByCache |Yüzde |
| \Bellek\PagesPersec |CountPerSecond |
| \Bellek\PagesReadPersec |CountPerSecond |
| \Bellek\PagesWrittenPersec |CountPerSecond |
| \Bellek\KullanılabilirTakas |Bayt |
| \Bellek\PercentAvailableSwap |Yüzde |
| \Bellek\UsedSwap |Bayt |
| \Bellek\YüzdeKullanılmış Takas |Yüzde |
| \İşlemci\PercentIdleTime |Yüzde |
| \İşlemci\YüzdeKullanıcı Zamanı |Yüzde |
| \İşlemci\YüzdeNiceTime |Yüzde |
| \İşlemci\YüzdeAyrıcalıklı Zaman |Yüzde |
| \İşlemci\YüzdeKesme Süresi |Yüzde |
| \İşlemci\YüzdeDPCTime |Yüzde |
| \İşlemci\YüzdeİşlemciZaman |Yüzde |
| \İşlemci\YüzdeioBekleme Süresi |Yüzde |
| \PhysicalDisk\BaytPerİkinci |BaytPerİkinci |
| \PhysicalDisk\ReadBytesPerSecond |BaytPerİkinci |
| \PhysicalDisk\WriteBytesPerSecond |BaytPerİkinci |
| \PhysicalDisk\TransfersPerSecond |CountPerSecond |
| \PhysicalDisk\ReadsPerSecond |CountPerSecond |
| \PhysicalDisk\WritesPerSecond |CountPerSecond |
| \PhysicalDisk\AverageReadTime |Saniye |
| \PhysicalDisk\AverageYazma Süresi |Saniye |
| \PhysicalDisk\AverageTransferTime |Saniye |
| \PhysicalDisk\AverageDiskQueueLength |Sayı |
| \NetworkInterface\BytesTransmitted |Bayt |
| \NetworkInterface\BaytAlınan |Bayt |
| \NetworkInterface\Paketlerİletilens |Sayı |
| \NetworkInterface\PaketlerAlındı |Sayı |
| \AğAraSı\BaytToplam |Bayt |
| \AğArası\TotalRxHatalar |Sayı |
| \NetworkInterface\TotalTxHatalar |Sayı |
| \NetworkInterface\TotalCollisions |Sayı |

## <a name="commonly-used-app-service-server-farm-metrics"></a>Yaygın olarak kullanılan App Service (Server Farm) ölçümleri
Ayrıca, Http sıra uzunluğu gibi yaygın web sunucusu ölçümlerine dayalı otomatik ölçeklendirme de gerçekleştirebilirsiniz. Metrik adı **HttpQueueLength'tir.**  Aşağıdaki bölümde kullanılabilir sunucu çiftliği (App Service) ölçümleri listelenmektedir.

### <a name="web-apps-metrics"></a>Web Apps ölçümleri
PowerShell'de aşağıdaki komutu kullanarak Web Uygulamaları ölçümlerinin bir listesini oluşturabilirsiniz.

```
Get-AzMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Bu ölçümlere göre uyarabilir veya ölçeklendirebilirsiniz.

| Metrik Adı | Birim |
| --- | --- |
| CpuYüzdesi |Yüzde |
| Bellek Yüzdesi |Yüzde |
| DiskQueueLength |Sayı |
| httpQueueLength |Sayı |
| BaytAlınan |Bayt |
| Baytgönderildi |Bayt |

## <a name="commonly-used-storage-metrics"></a>Yaygın olarak kullanılan Depolama ölçümleri
Depolama sıra uzunluğuna göre ölçeklendirebilirsiniz, bu da depolama kuyruğundaki ileti sayısıdır. Depolama sıra uzunluğu özel bir ölçüdür ve eşik örnek başına ileti sayısıdır. Örneğin, iki örnek varsa ve eşik 100 olarak ayarlanmışsa, kuyruktaki toplam ileti sayısı 200 olduğunda ölçekleme oluşur. Bu, örnek başına 100 ileti, 120 ve 80 veya 200 veya daha fazla ekleyen başka bir kombinasyon olabilir.

Bu ayarı **Ayarlar** bıtır'daki Azure portalında yapılandırın. VM ölçek kümeleri için, *MetricName'i* *Yaklaşıkİleti Sayısı* olarak kullanmak ve depolama kuyruğunun kimliğini *metricResourceUri*olarak geçirmek için Kaynak Yöneticisi şablonundaki Otomatik Ölçek ayarını güncelleştirebilirsiniz.

Örneğin, Klasik Depolama Hesabı ile otomatik ölçek ayarı metricTrigger şunları içerir:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ClassicStorage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
 ```

(Klasik olmayan) bir depolama hesabı için metricTrigger şunları içerir:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.Storage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
```

## <a name="commonly-used-service-bus-metrics"></a>Yaygın olarak kullanılan Servis Veri Günü ölçümleri
Servis Veri Yolu kuyruğundaki ileti sayısı olan Servis Veri Yolu sıra uzunluğuna göre ölçeklendirebilirsiniz. Servis Veri Yolu sıra uzunluğu özel bir ölçüdür ve eşik örnek başına ileti sayısıdır. Örneğin, iki örnek varsa ve eşik 100 olarak ayarlanmışsa, kuyruktaki toplam ileti sayısı 200 olduğunda ölçekleme oluşur. Bu, örnek başına 100 ileti, 120 ve 80 veya 200 veya daha fazla ekleyen başka bir kombinasyon olabilir.

VM ölçek kümeleri için, *MetricName'i* *Yaklaşıkİleti Sayısı* olarak kullanmak ve depolama kuyruğunun kimliğini *metricResourceUri*olarak geçirmek için Kaynak Yöneticisi şablonundaki Otomatik Ölçek ayarını güncelleştirebilirsiniz.

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ServiceBus/namespaces/SB_NAMESPACE/queues/QUEUE_NAME"
```

> [!NOTE]
> Hizmet Veri Mes'i için kaynak grubu kavramı yok, ancak Azure Kaynak Yöneticisi bölge başına varsayılan bir kaynak grubu oluşturur. Kaynak grubu genellikle 'Varsayılan-ServiceBus-[region]' biçimindedir. Örneğin, 'Default-ServiceBus-EastUS', 'Default-ServiceBus-WestUS', 'Default-ServiceBus-AustraliaEast' vb.
>
>

