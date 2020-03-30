---
title: Windows Azure Diagnostics uzantısı (WAD) yapılandırma şeması sürüm geçmişi
description: Azure Sanal Makineler, VM Ölçek Setleri, Hizmet Kumaşı ve Bulut Hizmetleri'nde perf sayaçlarının toplanmasıyla ilgili.
ms.subservice: diagnostic-extension
ms.topic: reference
author: bwren
ms.author: bwren
ms.date: 01/29/2020
ms.openlocfilehash: 4dd91363cdebf18e6303238816e8269065a6a317
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77672251"
---
# <a name="windows-azure-diagnostics-extension-wad-configuration-schema-versions-and-history"></a>Windows Azure Diagnostics uzantısı (WAD) yapılandırma şeması sürümleri ve geçmişi
Bu makalede, Microsoft Azure SDK'nın bir parçası olarak gönderilen [Windows (WAD)](diagnostics-extension-overview.md) şema sürümleri için Azure Tanılama uzantısı sürüm geçmişi verilmektedir.  


## <a name="azure-sdk-and-diagnostics-versions-shipping-chart"></a>Azure SDK ve tanılama sürümleri gönderi grafiği  

|Azure SDK sürümü | Tanılama uzantısı sürümü | Model|  
|------------------|-------------------------------|------|  
|1.x               |1.0                            |Eklenti|  
|2.0 - 2.4         |1.0                            |Eklenti|  
|2,5               |1.2                            |uzantı|  
|2,6               |1.3                            |"|  
|2.7               |1.4                            |"|  
|2.8               |1,5                            |"|  
|2.9               |1.6                            |"|
|2.96              |1.7                            |"|
|2.96              |1,8                            |"|
|2.96              |1.8.1                          |"|
|2.96              |1.9                            |"|
|2.96              |1.11                           |"|


 Azure Diagnostics sürüm 1.0 ilk olarak bir eklenti modelinde gönderilir , yani Azure SDK'yı yüklediğinizde Azure tanılama sürümünü de onunla birlikte göndermiş olursunuz.  

 Azure tanılama, SDK 2.5 (tanılama sürümü 1.2) ile başlayarak bir uzantı modeline gitti. Yeni özellikleri kullanmak için araçlar yalnızca yeni Azure SDK'larında kullanılabilir, ancak Azure tanılama özelliğini kullanan herhangi bir hizmet en son gönderi sürümünü doğrudan Azure'dan alır. Örneğin, SDK 2.5 kullanmaya devam eden herkes, yeni özellikleri kullanıyor olsalar da önceki tabloda gösterilen en son sürümü yükler.  

## <a name="schemas-index"></a>Şema indeksi  
Azure tanılamanın farklı sürümlerinde farklı yapılandırma şemaları kullanılır. Şema 1.0 ve 1.2 amortismana uğradı. Sürüm 1.3 ve sonraki sürüm hakkında daha fazla bilgi için [Tanılama 1.3 ve daha sonra Configuration Schema'ya](diagnostics-extension-schema-windows.md) bakın  

## <a name="version-history"></a>Sürüm geçmişi

### <a name="diagnostics-extension-111"></a>Tanılama uzantısı 1.11
Azure Monitor lavabosu için destek eklendi. Bu lavabo yalnızca performans sayaçları için geçerlidir. VM, VMSS veya bulut hizmetinizde toplanan performans sayaçlarının Azure Monitor'a özel ölçümler olarak gönderilmesini sağlar. Azure Monitor lavabosu şunları destekler:
* [Azure Monitor ölçüm API'leri](https://docs.microsoft.com/rest/api/monitor/metrics/list) aracılığıyla Azure Monitor'a gönderilen tüm performans sayaçlarını alma.
* Azure Monitor'daki yeni birleşik uyarı deneyimi aracılığıyla Azure Monitor'a gönderilen tüm performans [sayaçları](../../azure-monitor/platform/alerts-overview.md) hakkında uyarı
* Performans sayaçlarında joker karakter operatöre metriküzerinde "Örnek" boyutu olarak davranın. Örneğin, "LogicalDisk()/DiskWrites/sn"\*sayacını topladıysanız, her Mantıksal Disk için Disk Yazmaları/sn'yi çizmek veya uyarmak için "Örnek" boyutunu filtreleyebilir ve bölebilirsiniz (C:, D:, vb.)

Tanılama uzantısı yapılandırmanızda Azure Monitor'u yeni bir lavabo olarak tanımlayın
```json
"SinksConfig": {
    "Sink": [
        {
            "name": "AzureMonitorSink",
            "AzureMonitor": {}
        },
    ]
}
```

```XML
<SinksConfig>  
  <Sink name="AzureMonitorSink">
      <AzureMonitor/>
  </Sink>
</SinksConfig>
```
> [!NOTE]
> Azure Monitor lavabosu Klasik VM'ler ve Klasik CLoud Hizmeti için yapılandırıldığında, Tanılama uzantısıözel config'inde daha fazla parametrenin tanımlanması gerekir.
>
> Daha fazla bilgi için lütfen [ayrıntılı tanılama uzantısı şema belgelerine başvurun.](diagnostics-extension-schema-windows.md)

Ardından, performans sayaçlarınızı Azure Monitör Lavabosu'na yönlendirilecek şekilde yapılandırabilirsiniz.
```json
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "sinks": "AzureMonitorSink",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT1M",
            "unit": "percent"
        }
    ]
},
```
```XML
<PerformanceCounters scheduledTransferPeriod="PT1M", sinks="AzureMonitorSink">  
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />  
</PerformanceCounters>
```

### <a name="diagnostics-extension-19"></a>Tanılama uzantısı 1.9
Docker desteği eklendi.


### <a name="diagnostics-extension-181"></a>Tanılama uzantısı 1.8.1
Özel config'de depolama hesabı anahtarı yerine bir SAS belirteği belirtebilirsiniz. Bir SAS belirteci sağlanırsa, depolama hesabı anahtarı yoksayılır.


```json
{
    "storageAccountName": "diagstorageaccount",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    }
}
```

```xml
<PrivateConfig>
    <StorageAccount name="diagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    <SecondaryStorageAccounts>
        <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>
</PrivateConfig>
```


### <a name="diagnostics-extension-18"></a>Tanılama uzantısı 1.8
PublicConfig'e Depolama Türü eklendi. StorageType *Tablo*, *Blob*, *TableAndBlob*olabilir . *Tablo* varsayılandır.


```json
{
    "WadCfg": {
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

```xml
<PublicConfig>
    <WadCfg />
    <StorageAccount>diagstorageaccount</StorageAccount>
    <StorageType>TableAndBlob</StorageType>
</PublicConfig>
```


### <a name="diagnostics-extension-17"></a>Tanılama uzantısı 1.7
EventHub'a yönlendirme özelliği eklendi.

### <a name="diagnostics-extension-15"></a>Tanılama uzantısı 1.5
Lavaboöğesi ve uygulamanız genelinde sorunları tanılamayı kolaylaştıran [Uygulama Öngörüleri'ne](../../azure-monitor/app/cloudservices.md) tanılama verileri gönderme özelliği eklendi ve sistem ve altyapı düzeyinde sorunlar teşhis edildi.

### <a name="azure-sdk-26-and-diagnostics-extension-13"></a>Azure SDK 2.6 ve tanılama uzantısı 1.3
Visual Studio'daki Bulut Hizmeti projeleri için aşağıdaki değişiklikler yapılmıştır. (Bu değişiklikler Azure SDK'nın sonraki sürümleri için de geçerlidir.)

* Yerel emülatör artık tanılamayı destekliyor. Bu değişiklik, Visual Studio'da geliştirme ve test ederken tanılama verilerini toplayabileceğiniz ve uygulamanızın doğru izleri oluşturduğundan emin olabileceğiniz anlamına gelir. Bağlantı dizesi, `UseDevelopmentStorage=true` Azure depolama emülatörü kullanarak Visual Studio'da bulut hizmeti projenizi çalıştırırken tanılama veri toplamayı sağlar. Tüm tanılama verileri (Geliştirme Depolama) depolama hesabında toplanır.
* Tanılama depolama hesabı bağlantı dizesi (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) bir kez daha hizmet yapılandırması (.cscfg) dosyasında depolanır. Azure SDK 2.5'te tanılama depolama hesabı diagnostics.wadcfgx dosyasında belirtilmiştir.

Bağlantı dizesinin Azure SDK 2.4 ve daha önceki nde nasıl çalıştığı ile Azure SDK 2.6 ve sonraki yıllarda nasıl çalıştığı arasında bazı önemli farklar vardır.

* Azure SDK 2.4 ve daha önce, bağlantı dizesi tanılama günlükleri aktarmak için depolama hesabı bilgilerini almak için tanılama eklentisi tarafından çalışma zamanında kullanılmıştır.
* Azure SDK 2.6 ve sonraki dönemlerde Visual Studio, tanılama uzantısını yayımlama sırasında uygun depolama hesabı bilgileriyle yapılandırmak için tanılama bağlantı dizesini kullanır. Bağlantı dizesi, Visual Studio'nun yayımlama da kullanacağı farklı hizmet yapılandırmaları için farklı depolama hesapları tanımlamanıza olanak tanır. Ancak, tanılama eklentisi artık kullanılamadığından (Azure SDK 2.5'ten sonra), .cscfg dosyası tek başına Tanılama Uzantısı'nı etkinleştiremez. Uzantısı Visual Studio veya PowerShell gibi araçlar aracılığıyla ayrı ayrı etkinleştirmeniz gerekir.
* PowerShell ile tanılama uzantısı yapılandırma işlemini kolaylaştırmak için, Visual Studio'dan gelen paket çıktısı, her rol için tanılama uzantısı için ortak yapılandırma XML'i de içerir. Visual Studio, ortak yapılandırmada bulunan depolama hesabı bilgilerini doldurmak için tanılama bağlantı dizesini kullanır. Ortak config dosyaları Uzanlar klasöründe oluşturulur ve `PaaSDiagnostics.<RoleName>.PubConfig.xml`deseni izleyin. PowerShell tabanlı dağıtımlar, her yapılandırmayı bir Role eşlemek için bu deseni kullanabilir.
* .cscfg dosyasındaki bağlantı dizesi, **Izleme** sekmesinde görünebilmek için tanılama verilerine erişmek için Azure portalı tarafından da kullanılır. Bağlantı dizesi, hizmetin portaldaki ayrıntılı izleme verilerini göstermek için yapılandırması gerekir.

#### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Projeleri Azure SDK 2.6 ve sonrası
Azure SDK 2.5'ten Azure SDK 2.6 veya daha sonrasına geçerken,..wadcfgx dosyasında belirtilen bir tanılama depolama hesabınız varsa, bu hesap orada kalır. Farklı depolama yapılandırmaları için farklı depolama hesapları kullanma esnekliğinden yararlanmak için, bağlantı dizesini projenize el ile eklemeniz gerekir. Bir projeyi Azure SDK 2.4 veya daha eski sinden Azure SDK 2.6'ya geçirtiyorsanız, tanılama bağlantı dizeleri korunur. Ancak, bağlantı dizelerinin önceki bölümde belirtildiği gibi Azure SDK 2.6'da bağlantı dizelerinin nasıl işlenirşekilde işlenirindeki değişikliklere dikkat edin.

#### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Visual Studio tanıdepolama hesabını nasıl belirler?
* .cscfg dosyasında bir tanılama bağlantı dizesi belirtilirse, Visual Studio yayımlama sırasında tanılama uzantısını yapılandırmak ve paketleme sırasında ortak yapılandırma xml dosyalarını oluştururken kullanır.
* .cscfg dosyasında tanılama bağlantı dizesi belirtilmemişse, Visual Studio yayımlarken tanılama uzantısını yapılandırmak ve ortak yapılandırma xml dosyalarını oluşturmak için .wadcfgx dosyasında belirtilen depolama hesabını kullanmaya geri döner. paketleme yaparken.
* .cscfg dosyasındaki tanılama bağlantı dizesi .wadcfgx dosyasındaki depolama hesabından önceliklidir. .cscfg dosyasında bir tanılama bağlantı dizesi belirtilirse, Visual Studio bunu kullanır ve .wadcfgx'teki depolama hesabını yok sayar.

#### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>"Geliştirme depolama bağlantı dizelerini güncelleştirin..." ne işe yarar? onay kutusu yapmak?
**Microsoft Azure'da yayımlarken Microsoft Azure depolama hesabı kimlik bilgileriyle Tanılama ve Önbelleğe Alma için Geliştirme depolama bağlantı dizelerini güncelleştirme** onay kutusu, yayımlama sırasında belirtilen Azure depolama hesabıyla herhangi bir geliştirme depolama hesabı bağlantı dizesini güncelleştirmeniz için kullanışlı bir yol sağlar.

Örneğin, bu onay kutusunu seçtiğinizi ve tanılama bağlantı `UseDevelopmentStorage=true`dizesi belirtir. Projeyi Azure'da yayımladığınızda, Visual Studio tanılama bağlantı dizesini Yayımlama sihirbazında belirttiğiniz depolama hesabıyla otomatik olarak günceller. Ancak, tanılama bağlantı dizesi olarak gerçek bir depolama hesabı belirtilmişse, bunun yerine bu hesap kullanılır.

### <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Azure SDK 2.4 ve daha önceki arasındaki tanılama işlevselliği farkları ile Azure SDK 2.5 ve sonrası
Projenizi Azure SDK 2.4'ten Azure SDK 2.5 veya sonraki ne rekatlarına yükseltiyorsanız, aşağıdaki tanılama işlevselliği farklılıklarını göz önünde bulundurmalısınız.

* **Yapılandırma API'leri amortismana tabidir** – Tanılamanın programlı yapılandırması Azure SDK 2.4 veya önceki sürümlerinde kullanılabilir, ancak Azure SDK 2.5 ve sonraki sürümlerde amortismana tabidir. Tanılama yapılandırmanız şu anda kodda tanımlanmışsa, tanılamanın çalışmaya devam edebilmesi için bu ayarları geçirilen projede sıfırdan yeniden yapılandırmanız gerekir. Azure SDK 2.4 için tanılama yapılandırma dosyası, Azure SDK 2.5 ve sonrası için diagnostics.wadcfg ve diagnostics.wadcfgx'tir.
* **Bulut hizmeti uygulamaları için tanılama, örnek düzeyinde değil, yalnızca rol düzeyinde yapılandırılabilir.**
* **Uygulamanızı her dağıttığınızda tanılama yapılandırması güncelleştirilir** – Tanılama yapılandırmanızı Server Explorer'dan değiştirip uygulamanızı yeniden dağıtırsanız bu durum eşlik sorunlarına neden olabilir.
* **Azure SDK 2.5 ve sonraki durumlarda, kilitlenme dökümleri kodda değil tanılama yapılandırma dosyasında yapılandırılır** – Kodda yapılandırılan kilitlenme dökümleri varsa, kilitlenme dökümleri Azure SDK 2.6'ya geçiş sırasında aktarılmadığından yapılandırmayı koddan yapılandırma dosyasına el ile aktarmanız gerekir.

