---
title: Windows Azure Tanılama ile Olay Toplama
description: Azure Hizmet Kumaşı kümelerinin izlenmesi ve teşhisi için WAD kullanarak olayları toplama ve toplama hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 04/03/2018
ms.author: srrengar
ms.openlocfilehash: b9a448ff41c66fa3a38c124f7acde062bacbe9ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282503"
---
# <a name="event-aggregation-and-collection-using-windows-azure-diagnostics"></a>Windows Azure Tanılama'yı kullanarak olay toplama ve toplama
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Bir Azure Hizmet Kumaşı kümesini çalıştırırken, tüm düğümlerin günlüklerini merkezi bir konumda toplamak iyi bir fikirdir. Günlüklerin merkezi bir konumda olması, kümenizdeki sorunları veya bu kümede çalışan uygulama ve hizmetlerdeki sorunları çözümlemenize ve sorun gidermenize yardımcı olur.

Günlükleri yüklemenin ve toplamanın bir yolu, günlükleri Azure Depolama'ya yükleyen ve günlükleri Azure Uygulama Bilgileri'ne veya Etkinlik Hub'larına gönderme seçeneğine sahip olan Windows Azure Tanılama (WAD) uzantısını kullanmaktır. Ayrıca, olayları depolama alanından okumak ve [Azure Monitor günlükleri](../log-analytics/log-analytics-service-fabric.md) veya başka bir günlük ayrıştma çözümü gibi bir analiz platformu ürününe yerleştirmek için harici bir işlem de kullanabilirsiniz.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu makalede aşağıdaki araçlar kullanılmıştır:

* [Azure Resource Manager](../azure-resource-manager/management/overview.md)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Resource Manager şablonu](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric-platform-events"></a>Servis Kumaş platformu etkinlikleri
Service Fabric, izleme ve tanılama verilerini bir depolama tablosuna veya başka bir yere göndermek için uzantıyla birlikte aşağıdaki kanalların önceden yapılandırılan birkaç [hazır günlük kanalıyla](service-fabric-diagnostics-event-generation-infra.md)sizi ayarlar:
  * [Operasyonel olaylar:](service-fabric-diagnostics-event-generation-operational.md)Service Fabric platformunun gerçekleştirdiği üst düzey işlemler. Örnekler arasında uygulama ve hizmetlerin oluşturulması, düğüm durumu değişiklikleri ve yükseltme bilgileri yer almaktadır. Bunlar Windows (ETW) günlükleri için Olay İzleme olarak yayılır
  * [Güvenilir Aktörler programlama modeli olaylar](service-fabric-reliable-actors-diagnostics.md)
  * [Güvenilir Hizmetler programlama modeli olaylar](service-fabric-reliable-services-diagnostics.md)

## <a name="deploy-the-diagnostics-extension-through-the-portal"></a>Tanılama uzantısını portal üzerinden dağıtın
Günlükleri toplamanın ilk adımı, Hizmet Kumaşı kümesindeki sanal makine ölçeği kümesi düğümlerine Tanılama uzantısını dağıtmaktır. Tanılama uzantısı, her VM'deki günlükleri toplar ve bunları belirttiğiniz depolama hesabına yükler. Aşağıdaki adımlar, Azure portalı ve Azure Kaynak Yöneticisi şablonları aracılığıyla yeni ve varolan kümeler için bunu nasıl başaracaklarını açıklar.

### <a name="deploy-the-diagnostics-extension-as-part-of-cluster-creation-through-azure-portal"></a>Azure portalı üzerinden küme oluşturmanın bir parçası olarak Tanılama uzantısını dağıtma
Kümenizi oluştururken, küme yapılandırma adımında, isteğe bağlı ayarları genişletin ve Tanılama'nın **Açık** (varsayılan ayar) olarak ayarlandığından emin olun.

![Küme oluşturma portalındaki Azure Tanılama ayarları](media/service-fabric-diagnostics-event-aggregation-wad/azure-enable-diagnostics-new.png)

Son adımda **Oluştur'u tıklatmadan önce** şablonu indirmenizi öneririz. Ayrıntılar için, [Azure Kaynak Yöneticisi şablonu kullanarak Hizmet Dokusu küme oluşturma'ya](service-fabric-cluster-creation-via-arm.md)bakın. Veri toplamak için hangi kanallarda (yukarıda listelenen) değişiklik yapmak için şablona ihtiyacınız vardır.

![Küme Şablonu](media/service-fabric-diagnostics-event-aggregation-wad/download-cluster-template.png)

Azure Depolama'da olayları bir araya getirerek, azure [monitörgünlükleri](service-fabric-diagnostics-oms-setup.md) oluşturmak ve Bunları Azure Monitor günlükleri portalında sorgulamak için Azure Monitor günlüklerini ayarlayın

>[!NOTE]
>Şu anda tablolara gönderilen olayları filtrelemenin veya damadın bir yolu yok. Olayları tablodan kaldırmak için bir işlem uygulamazsanız, tablo büyümeye devam eder (varsayılan kapak 50 GB'dır). Bu nasıl değiştirilen talimatlar [bu makalede daha aşağıdadır.](service-fabric-diagnostics-event-aggregation-wad.md#update-storage-quota) Ayrıca, [Watchdog örnek](https://github.com/Azure-Samples/service-fabric-watchdog-service)çalışan bir veri damat hizmeti bir örnek vardır , ve 30 veya 90 günlük zaman dilimi ötesinde günlükleri depolamak için iyi bir neden olmadığı sürece, kendiniz için de bir yazmanızı tavsiye edilir.



## <a name="deploy-the-diagnostics-extension-through-azure-resource-manager"></a>Azure Kaynak Yöneticisi aracılığıyla Tanılama uzantısını dağıtma

### <a name="create-a-cluster-with-the-diagnostics-extension"></a>Tanılama uzantısı ile bir küme oluşturma
Kaynak Yöneticisi'ni kullanarak bir küme oluşturmak için, tam Kaynak Yöneticisi şablonuna Tanılama yapılandırması JSON eklemeniz gerekir. Kaynak Yöneticisi şablon örneklerimizin bir parçası olarak eklenen Tanılama yapılandırması ile birlikte örnek beş VM küme Kaynak Yöneticisi şablonu sağlarız. Bu konumda Azure Örnekleri galerisinde görebilirsiniz: [Tanılama Kaynak Yöneticisi şablon örneği ile beş düğümlü küme.](https://azure.microsoft.com/resources/templates/service-fabric-secure-cluster-5-node-1-nodetype/)

Kaynak Yöneticisi şablonunda Tanılama ayarını görmek için azuredeploy.json dosyasını açın ve **IaaSDiagnostics'i**arayın. Bu şablonu kullanarak bir küme oluşturmak için, önceki bağlantıda bulunan **Azure'a Dağıt** düğmesini seçin.

Alternatif olarak, Kaynak Yöneticisi örneğini indirebilir, değişiklik yapabilir ve Azure PowerShell `New-AzResourceGroupDeployment` penceresindeki komutu kullanarak değiştirilmiş şablona sahip bir küme oluşturabilirsiniz. Komuta geçtiğiniz parametreler için aşağıdaki koda bakın. PowerShell'i kullanarak bir kaynak grubunu nasıl dağıtılacak hakkında ayrıntılı bilgi için, [Azure Kaynak Yöneticisi şablonuna sahip bir kaynak grubu dağıt'a](../azure-resource-manager/templates/deploy-powershell.md)bakın.

### <a name="add-the-diagnostics-extension-to-an-existing-cluster"></a>Varolan bir kümeye tanılama uzantısı ekleme
Tanılama dağıtılmamasa varolan bir kümeniz varsa, küme şablonu aracılığıyla ekleyebilir veya güncelleştirebilirsiniz. Varolan kümeyi oluşturmak veya şablonu daha önce açıklandığı şekilde portaldan indirmek için kullanılan Kaynak Yöneticisi şablonuna değiştirin. Aşağıdaki görevleri gerçekleştirerek template.json dosyasını değiştirin:

Kaynaklar bölümüne ekleyerek şablona yeni bir depolama kaynağı ekleyin.

```json
{
    "apiVersion": "2018-07-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('applicationDiagnosticsStorageAccountName')]",
    "location": "[parameters('computeLocation')]",
    "sku": {
    "name": "[parameters('applicationDiagnosticsStorageAccountType')]"
    "tier": "standard"
  },
    "tags": {
    "resourceType": "Service Fabric",
    "clusterName": "[parameters('clusterName')]"
  }
},
```

 Ardından, depolama hesabı tanımlarından hemen sonra parametreler `supportLogStorageAccountName`bölümüne ekleyin. Yer tutucu metin *depolama hesabı adını istediğiniz* depolama hesabının adı ile değiştirin.

```json
    "applicationDiagnosticsStorageAccountType": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "Replication option for the application diagnostics storage account"
      }
    },
    "applicationDiagnosticsStorageAccountName": {
      "type": "string",
      "defaultValue": "**STORAGE ACCOUNT NAME GOES HERE**",
      "metadata": {
        "description": "Name for the storage account that contains application diagnostics data from the cluster"
      }
    },
```
Ardından, uzantılar dizisine aşağıdaki kodu ekleyerek template.json dosyasının `VirtualMachineProfile` bölümünü güncelleştirin. Nereye eklendiğine bağlı olarak, başlangıçta veya sonunda virgül eklediğinizden emin olun.

```json
{
    "name": "[concat(parameters('vmNodeType0Name'),'_Microsoft.Insights.VMDiagnosticsSettings')]",
    "properties": {
        "type": "IaaSDiagnostics",
        "autoUpgradeMinorVersion": true,
        "protectedSettings": {
        "storageAccountName": "[parameters('applicationDiagnosticsStorageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
        "storageAccountEndPoint": "https://core.windows.net/"
        },
        "publisher": "Microsoft.Azure.Diagnostics",
        "settings": {
        "WadCfg": {
            "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": "50000",
            "EtwProviders": {
                "EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
                {
                    "provider": "Microsoft-ServiceFabric-Services",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableServiceEventTable"
                    }
                }
                ],
                "EtwManifestProviderConfiguration": [
                {
                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                },
                {
                    "provider": "02d06793-efeb-48c8-8f7f-09713309a810",
                    "scheduledTransferLogLevelFilter": "Information",
                    "scheduledTransferKeywordFilter": "4611686018427387904",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricSystemEventTable"
                    }
                }
                ]
            }
            }
        },
        "StorageAccount": "[parameters('applicationDiagnosticsStorageAccountName')]"
        },
        "typeHandlerVersion": "1.5"
    }
}
```

template.json dosyasını açıklandığı gibi değiştirdikten sonra Kaynak Yöneticisi şablonu yeniden yayımlayın. Şablon dışa aktarıldıysa, deploy.ps1 dosyasını çalıştıran şablonu yeniden yayımlar. Dağıttıktan sonra **ProvisioningState'in** **Başarılı**olduğundan emin olun.

> [!TIP]
> Kümenize kapsayıcılar dağıtacaksanız, **WAD'ın bunu WadCfg > DiagnosticMonitorConfiguration** bölümünüze ekleyerek docker istatistiklerini almasını sağlayın.
>
>```json
>"DockerSources": {
>    "Stats": {
>        "enabled": true,
>        "sampleRate": "PT1M"
>    }
>},
>```

### <a name="update-storage-quota"></a>Depolama kotası güncelleştir

Uzantınma tarafından doldurulan tablolar kota vuruluna kadar büyüdüğünden, kota boyutunu küçültmeyi düşünebilirsiniz. Varsayılan değer 50 GB'dır ve `overallQuotaInMB` aşağıdaki alanın altındaki şablonda yapılandırılabilir`DiagnosticMonitorConfiguration`

```json
"overallQuotaInMB": "50000",
```

## <a name="log-collection-configurations"></a>Günlük toplama yapılandırmaları
Ek kanallardan günlükler de koleksiyon için kullanılabilir, burada Azure'da çalışan kümeler için şablonda yapabileceğiniz en yaygın yapılandırmalardan bazıları aşağıda verilmiştir.

* Operasyonel Kanal - Taban: Varsayılan olarak etkinleştirilen, Hizmet Kumaşı ve küme tarafından gerçekleştirilen üst düzey işlemler, bir düğüm için olaylar da dahil olmak üzere, yeni bir uygulama nın dağıtılması veya yükseltme geri alma, vb. Olayların listesi için Operasyonel [Kanal Etkinlikleri'ne](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-operational)bakın.
  
```json
      scheduledTransferKeywordFilter: "4611686018427387904"
  ```
* Operasyonel Kanal - Ayrıntılı: Bu sağlık raporları ve yük dengeleme kararları, artı temel operasyonel kanal her şeyi içerir. Bu olaylar, [reportPartitionHealth](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportpartitionhealth.aspx) veya [ReportLoad](https://msdn.microsoft.com/library/azure/system.fabric.iservicepartition.reportload.aspx)gibi sistem veya kod tarafından sistem veya kod tarafından oluşturulur. Visual Studio'nun Tanılama Etkinliği Görüntüleyici'sinde bu olayları görüntülemek için ETW sağlayıcıları listesine "Microsoft-ServiceFabric:4:0x40000000000000" ekleyin.

```json
      scheduledTransferKeywordFilter: "4611686018427387912"
  ```

* Veri ve Mesajlaşma Kanalı - Temel: Ayrıntılı operasyonel kanal günlüklerine ek olarak, iletide (şu anda yalnızca Ters Proxy) ve veri yolunda oluşturulan kritik günlükler ve olaylar. Bu olaylar, Ters Proxy'deki istek işleme hataları ve diğer kritik sorunların yanı sıra işlenen isteklerdir. **Bu kapsamlı günlük için tavsiyemizdir.** Visual Studio'nun Tanılama Etkinliği Görüntüleyici'sinde bu olayları görüntülemek için ETW sağlayıcıları listesine "Microsoft-ServiceFabric:4:0x400000000000000" ekleyin.

```json
      scheduledTransferKeywordFilter: "4611686018427387928"
  ```

* Data & Messaging Channel - Ayrıntılı: Kümedeki veri ve mesajlaşmadan ve ayrıntılı işletim kanalından kritik olmayan tüm günlükleri içeren verbose kanalı. Tüm ters proxy olaylarının ayrıntılı sorun giderme için [ters proxy tanılama kılavuzuna](service-fabric-reverse-proxy-diagnostics.md)bakın.  Bu olayları Visual Studio'nun Tanılama Etkinliği görüntüleyicisinde görüntülemek için ETW sağlayıcıları listesine "Microsoft-ServiceFabric:4:0x4000000000000020" ekleyin.

```json
      scheduledTransferKeywordFilter: "4611686018427387944"
  ```

>[!NOTE]
>Bu kanal, bu ayrıntılı kanaldan olay toplamanın hızlı bir şekilde oluşturulan bir çok izlemeyle sonuçlanmasını sağlayan çok yüksek bir olay hacmine sahiptir ve depolama kapasitesini tüketebilir. Sadece gerekirse bunu açın.


**Base Operational Channel'ı** en az gürültüyle kapsamlı günlüğe `EtwManifestProviderConfiguration` kaydetme `WadCfg` önerimizi etkinleştirmek için şablonunuzun içinde aşağıdakiler gibi görünür:

```json
  "WadCfg": {
        "DiagnosticMonitorConfiguration": {
          "overallQuotaInMB": "50000",
          "EtwProviders": {
            "EtwEventSourceProviderConfiguration": [
              {
                "provider": "Microsoft-ServiceFabric-Actors",
                "scheduledTransferKeywordFilter": "1",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                  "eventDestination": "ServiceFabricReliableActorEventTable"
                }
              },
              {
                "provider": "Microsoft-ServiceFabric-Services",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                  "eventDestination": "ServiceFabricReliableServiceEventTable"
                }
              }
            ],
            "EtwManifestProviderConfiguration": [
              {
                "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                "scheduledTransferLogLevelFilter": "Information",
                "scheduledTransferKeywordFilter": "4611686018427387904",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                  "eventDestination": "ServiceFabricSystemEventTable"
                }
              },
              {
                "provider": "02d06793-efeb-48c8-8f7f-09713309a810",
                "scheduledTransferLogLevelFilter": "Information",
                "scheduledTransferKeywordFilter": "4611686018427387904",
                "scheduledTransferPeriod": "PT5M",
                "DefaultEvents": {
                "eventDestination": "ServiceFabricSystemEventTable"
                }
              }
            ]
          }
        }
      },
```

## <a name="collect-from-new-eventsource-channels"></a>Yeni EventSource kanallarından toplama

Dağıtmak üzere olduğunuz yeni bir uygulamayı temsil eden yeni EventSource kanallarından günlükleri toplamak için Tanılama'yı güncelleştirmek için, varolan bir küme için Tanılama kurulumu için daha önce açıklandığı gibi aynı adımları gerçekleştirin.

PowerShell `EtwEventSourceProviderConfiguration` komutunu kullanarak yapılandırma güncelleştirmesini uygulamadan önce yeni EventSource kanallarıiçin giriş eklemek `New-AzResourceGroupDeployment` için template.json dosyasındaki bölümü güncelleştirin. Olay kaynağının adı Visual Studio tarafından oluşturulan ServiceEventSource.cs dosyasında kodunuzu bir parçası olarak tanımlanır.

Örneğin, olay kaynağınızın adı My-Eventsource ise, My-Eventsource'daki olayları MyDestinationTableName adlı bir tabloya yerleştirmek için aşağıdaki kodu ekleyin.

```json
        {
            "provider": "My-Eventsource",
            "scheduledTransferPeriod": "PT5M",
            "DefaultEvents": {
            "eventDestination": "MyDestinationTableName"
            }
        }
```

Performans sayaçları veya olay günlükleri toplamak için, Bir Azure [Kaynak Yöneticisi şablonu kullanarak izleme ve tanılama ile windows sanal makine oluştur'da](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)sağlanan örnekleri kullanarak Kaynak Yöneticisi şablonu değiştirin. Ardından, Kaynak Yöneticisi şablonu yeniden yayımlayın.

## <a name="collect-performance-counters"></a>Performans Sayaçlarını Topla

Kümenizden performans ölçümleri toplamak için, kümenizin Kaynak Yöneticisi şablonundaki "WadCfg > DiagnosticMonitorConfiguration"ınıza performans sayaçlarını ekleyin. Belirli [Performance monitoring with WAD](service-fabric-diagnostics-perf-wad.md) performans sayaçları toplamak için değiştirme `WadCfg` adımlarını görmek için WAD ile Performans izleme konusuna bakın. Toplamanızı önerdiğimiz performans sayaçlarının listesi için Referans [Servis Kumaş Performans Sayaçları.](service-fabric-diagnostics-event-generation-perf.md)
  
Aşağıdaki bölümde açıklandığı gibi bir Uygulama Öngörüleri lavabosu kullanıyorsanız ve bu ölçümlerin Application Insights'ta görünmesini istiyorsanız, lavabo adını yukarıda gösterildiği gibi "lavabolar" bölümüne eklediğinizden emin olun. Bu, tek tek yapılandırılan performans sayaçlarını Application Insights kaynağınıza otomatik olarak gönderir.


## <a name="send-logs-to-application-insights"></a>Günlükleri Uygulama Öngörülerine gönderme

### <a name="configuring-application-insights-with-wad"></a>WAD ile Uygulama Öngörülerini Yapılandırma

>[!NOTE]
>Bu, şu anda yalnızca Windows kümeleri için geçerlidir.

WAD'dan Azure Uygulama Öngörüleri'ne veri göndermenin iki temel yolu vardır ve bu da Wad yapılandırmasına, Azure portalı üzerinden veya Azure Kaynak Yöneticisi şablonu aracılığıyla uygulama öngörüleri ekleyerek elde edilir.

#### <a name="add-an-application-insights-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Azure portalında küme oluştururken Uygulama Öngörüleri Enstrümantasyon Anahtarı ekleme

![AIKey Ekleme](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

Bir küme oluştururken, Tanılama "Açık" olarak açıksa, Uygulama Öngörüleri Enstrümantasyon anahtarı girmek için isteğe bağlı bir alan görüntülenir. Uygulama Öngörüleri Anahtarınızı buraya yapıştırArsanız, Uygulama Öngörüleri batması, kümenizi dağıtmak için kullanılan Kaynak Yöneticisi şablonunda sizin için otomatik olarak yapılandırılır.

#### <a name="add-the-application-insights-sink-to-the-resource-manager-template"></a>Kaynak Yöneticisi şablonuna Uygulama Öngörüleri Biçin Ekleme

Kaynak Yöneticisi şablonunun "WadCfg"ine aşağıdaki iki değişikliği ekleyerek bir "Lavabo" ekleyin:

1. Lavabo yapılandırmasını, beyan tamamlandıktan `DiagnosticMonitorConfiguration` hemen sonra ekleyin:

    ```json
    "SinksConfig": {
        "Sink": [
            {
                "name": "applicationInsights",
                "ApplicationInsights": "***ADD INSTRUMENTATION KEY HERE***"
            }
        ]
    }

    ```

2. Lavabo'yu `DiagnosticMonitorConfiguration` aşağıdaki satırı `DiagnosticMonitorConfiguration` ekleyerek ekleyin `WadCfg` (beyan `EtwProviders` edilmeden hemen önce):

    ```json
    "sinks": "applicationInsights"
    ```

Her iki önceki kod parçacıklarında, lavaboyu tanımlamak için "applicationInsights" adı kullanılmıştır. Bu bir gereklilik değildir ve lavabonun adı "lavabolar"a dahil olduğu sürece, adı herhangi bir dize olarak ayarlayabilirsiniz.

Şu anda, kümedeki günlükler Application Insights'ın günlük görüntüleyicisinde **iz** olarak gösteriş verir. Platformdan gelen izlemelerin çoğu "Bilgilendirme" düzeyinde olduğundan, lavabo yapılandırmasını yalnızca "Uyarı" veya "Hata" türü günlükleri göndermek için değiştirmeyi de düşünebilirsiniz. Bu, [bu makalede](../azure-monitor/platform/diagnostics-extension-to-application-insights.md)gösterildiği gibi, lavaboiçin "Kanallar" ekleyerek yapılabilir.

>[!NOTE]
>Portalda veya Kaynak Yöneticisi şablonunuzda yanlış bir Uygulama Öngörüleri Anahtarı kullanıyorsanız, anahtarı el ile değiştirmeniz ve kümeyi güncelleştirmeniz / yeniden dağıtmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure tanılamalarını doğru şekilde yapılandırdıktan sonra, ETW ve EventSource günlüklerinden Depolama tablolarınızdaki verileri görürsünüz. Kaynak Yöneticisi şablonunda doğrudan yapılandırılan Azure Monitor günlüklerini, Kibana'yı veya diğer veri analizlerini ve görselleştirme platformlarını kullanmayı seçerseniz, bu depolama tablolarından verileri okumak için seçtiğiniz platformu ayarladığınızdan emin olun. Bunu Azure Monitor günlükleri için yapmak nispeten önemsizdir ve [Olay ve günlük çözümlemesi](service-fabric-diagnostics-event-analysis-oms.md)ile açıklanır. Uygulama Öngörüleri bu anlamda biraz özel bir durumdur, çünkü Tanılama uzantısı yapılandırmasının bir parçası olarak yapılandırılabilir, bu nedenle AI kullanmayı seçerseniz [uygun makaleye](service-fabric-diagnostics-event-analysis-appinsights.md) bakın.

>[!NOTE]
>Şu anda tabloya gönderilen olayları filtrelemenin veya damadın bir yolu yok. Olayları tablodan kaldırmak için bir işlem uygulamazsanız, tablo büyümeye devam edecektir. Şu anda, [Watchdog örnek](https://github.com/Azure-Samples/service-fabric-watchdog-service)çalışan bir veri damat hizmeti bir örnek vardır , ve 30 veya 90 günlük zaman dilimi ötesinde günlükleri depolamak için iyi bir neden olmadığı sürece, kendiniz için de bir yazmanızı tavsiye edilir.

* [Tanılama uzantısını kullanarak performans sayaçlarını veya günlükleri nasıl toplayacaklarını öğrenin](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Uygulama Öngörüleri ile Etkinlik Analizi ve Görselleştirme](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Azure Monitör günlükleri ile Olay Analizi ve Görselleştirme](service-fabric-diagnostics-event-analysis-oms.md)
* [Uygulama Öngörüleri ile Etkinlik Analizi ve Görselleştirme](service-fabric-diagnostics-event-analysis-appinsights.md)
* [Azure Monitör günlükleri ile Olay Analizi ve Görselleştirme](service-fabric-diagnostics-event-analysis-oms.md)
