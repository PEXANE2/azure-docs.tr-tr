---
title: Azure Bulut Hizmetini İzle | Microsoft Dokümanlar
description: Azure Bulut Hizmeti'ni izlemenin neler içerdiğini ve bazı seçeneklerinizin neler olduğunu açıklar.
services: cloud-services
documentationcenter: ''
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 01/29/2018
ms.author: tagore
ms.openlocfilehash: 61c794ba03934ae1828ba310f3f776bfb61b652b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273104"
---
# <a name="introduction-to-cloud-service-monitoring"></a>Bulut Hizmeti İzlemeye Giriş

Herhangi bir bulut hizmeti için önemli performans ölçümlerini izleyebilirsiniz. Her bulut hizmeti rolü en az veri toplar: CPU kullanımı, ağ kullanımı ve disk kullanımı. Bulut hizmetinde bir `Microsoft.Azure.Diagnostics` role uygulanan uzantı varsa, bu rol ek veri noktaları toplayabilir. Bu makalede, Bulut Hizmetleri için Azure Tanılama'ya giriş sağlanmaktadır.

Temel izleme ile, rol örneklerinden performans sayacı verileri örneklenir ve 3 dakikalık aralıklarla toplanır. Bu temel izleme verileri depolama hesabınızda depolanmaz ve bununla ilişkili ek bir maliyeti yoktur.

Gelişmiş izleme ile ek ölçümler 5 dakika, 1 saat ve 12 saatlik aralıklarla örneklenir ve toplanır. Toplanan veriler bir depolama hesabında, tablolarda depolanır ve 10 gün sonra temizlenir. Kullanılan depolama hesabı role göre yapılandırılır; farklı roller için farklı depolama hesapları kullanabilirsiniz. Bu, [.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) ve [.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) dosyalarındaki bir bağlantı dizesi ile yapılandırılır.


## <a name="basic-monitoring"></a>Temel izleme

Girişte belirtildiği gibi, bir bulut hizmeti ana bilgisayar sanal makineden temel izleme verilerini otomatik olarak toplar. Bu veriler CPU yüzdesi, ağ girip/çıkma ve disk okuma/yazma içerir. Toplanan izleme verileri, bulut hizmetinin genel bakış ve ölçümler sayfalarında, Azure portalında otomatik olarak görüntülenir. 

Temel izleme bir depolama hesabı gerektirmez. 

![temel bulut hizmeti izleme karoları](media/cloud-services-how-to-monitor/basic-tiles.png)

## <a name="advanced-monitoring"></a>Gelişmiş izleme

Gelişmiş izleme, izlemek istediğiniz rolde **Azure Tanılama** uzantısını (ve isteğe bağlı olarak Uygulama Öngörüleri SDK'sını) kullanmayı içerir. Tanılama uzantısı, izlenen tanılama ölçümlerini yapılandırmak için **diagnostics.wadcfgx** adlı bir config dosyası (rol başına) kullanır. Azure Tanı uzantısı verileri bir Azure Depolama hesabında toplar ve depolar. Bu ayarlar **.wadcfgx**, [.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef)ve [.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) dosyalarında yapılandırılır. Bu, gelişmiş izlemeyle ilişkili ek bir maliyet olduğu anlamına gelir.

Her rol oluşturulduğunda, Visual Studio azure tanılama uzantısını ekler. Bu tanılama uzantısı aşağıdaki türbilgileri toplayabilir:

* Özel performans sayaçları
* Uygulama günlükleri
* Windows olay günlükleri
* .NET etkinlik kaynağı
* IIS günlükleri
* Bildirim tabanlı ETW
* Kilitlenme bilgi dökümleri
* Müşteri hata günlükleri

> [!IMPORTANT]
> Tüm bu veriler depolama hesabına toplanmış olsa da, portal verileri grafiklemek için yerel bir yol **sağlamaz.** Uygulama Öngörüleri gibi başka bir hizmeti uygulamanıza entegre eleştirmeniz önerilir.

## <a name="setup-diagnostics-extension"></a>Kurulum tanılama uzantısı

İlk olarak, **klasik** bir depolama hesabınız yoksa, [bir tane oluşturun.](../storage/common/storage-account-create.md) **Belirtilen Klasik dağıtım modeliyle** depolama hesabının oluşturulduğundan emin olun.

Ardından, Depolama **hesabı (klasik)** kaynağına gidin. **Ayarlar** > **Erişim tuşlarını** seçin ve Birincil bağlantı **dize** değerini kopyalayın. Bulut hizmeti için bu değere ihtiyacınız var. 

Etkinleştirilmesi için değiştirmeniz gereken iki config dosyası vardır, **ServiceDefinition.csdef** ve **ServiceConfiguration.cscfg**.

### <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef

**ServiceDefinition.csdef** dosyasında, gelişmiş tanılama `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` kullanan her rol için adlandırılmış yeni bir ayar ekleyin. Visual Studio, yeni bir proje oluşturduğunuzda bu değeri dosyaya ekler. Eksik olması durumunda, şimdi ekleyebilirsiniz. 

```xml
<ServiceDefinition name="AnsurCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRoleWithSBQueue1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
```

Bu, her **ServiceConfiguration.cscfg** dosyasına eklenmesi gereken yeni bir ayar tanımlar. 

Büyük olasılıkla biri Azure'da dağıtmak için **ServiceConfiguration.cloud.cscfg** adında iki **.cscfg** dosyanız ve taklit edilmiş ortamda yerel dağıtımlar için kullanılan **ServiceConfiguration.local.cscfg** adlı dosyanız vardır. Her **.cscfg** dosyayı açın ve değiştirin. Adlı `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`ayar ekle Değeri klasik depolama hesabının **Birincil bağlantı dizesine** ayarlayın. Geliştirme makinenizdeki yerel depolama alanını kullanmak istiyorsanız, 'yi kullanın. `UseDevelopmentStorage=true`

```xml
<ServiceConfiguration serviceName="AnsurCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="WorkerRoleWithSBQueue1">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=mystorage;AccountKey=KWwkdfmskOIS240jnBOeeXVGHT9QgKS4kIQ3wWVKzOYkfjdsjfkjdsaf+sddfwwfw+sdffsdafda/w==" />
      
      <!-- or use the local development machine for storage
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
      -->
```

## <a name="use-application-insights"></a>Uygulama Öngörülerini Kullanma

Visual Studio'dan Bulut Hizmeti yayımladığınızda, tanılama verilerini Application Insights'a gönderme seçeneği verilir. O anda Uygulama Öngörüleri Azure kaynağını oluşturabilir veya verileri varolan bir Azure kaynağına gönderebilirsiniz. Bulut hizmetiniz kullanılabilirlik, performans, hatalar ve kullanım için Application Insights tarafından izlenebilir. En önemli verileri görebilmeniz için Uygulama Öngörüleri'ne özel grafikler eklenebilir. Rol örneği verileri, bulut hizmeti projenizde Application Insights SDK kullanılarak toplanabilir. Uygulama Öngörüleri'ni nasıl tümleştirişdireceklerine ilişkin daha fazla bilgi için, [Bulut Hizmetleri yle Uygulama Öngörüleri'ne](../azure-monitor/app/cloudservices.md)bakın.

Windows Azure Tanılama uzantısı aracılığıyla belirttiğiniz performans sayaçlarını (ve diğer ayarları) görüntülemek için Application Insights'ı kullanabiliyorken, Uygulama Öngörüleri SDK'sını yalnızca işçi ve web rolleri.


## <a name="next-steps"></a>Sonraki adımlar

- [Bulut Hizmetleri ile Uygulama Öngörüleri hakkında bilgi edinin](../azure-monitor/app/cloudservices.md)
- [Performans sayaçları ayarlama](diagnostics-performance-counters.md)




