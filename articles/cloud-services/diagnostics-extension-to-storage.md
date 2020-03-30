---
title: Azure Depolama’da Tanılama Verilerini Depolama ve Görüntüleme
description: Azure tanılama verilerini bir Azure Depolama hesabında nasıl topladığınızı öğrenin, böylece azure verilerini kullanılabilir birkaç araçtan biriyle görüntüleyebilirsiniz.
services: azure-monitor
author: bwren
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 08/01/2016
ms.author: bwren
ms.subservice: diagnostic-extension
ms.openlocfilehash: 17430330d068fb55b45f073afecb8ba348286cb5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77472680"
---
# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Azure Depolama’daki tanılama verilerini depolama ve görüntüleme
Tanılama verileri, Microsoft Azure depolama emülatörüne veya Azure depolama alanına aktarmadığınız sürece kalıcı olarak depolanmaz. Depolama alanına alındıktan sonra, kullanılabilir birkaç araçtan biriyle görüntülenebilir.

## <a name="specify-a-storage-account"></a>Depolama hesabı belirtin
ServiceConfiguration.cscfg dosyasında kullanmak istediğiniz depolama hesabını belirtirsiniz. Hesap bilgileri, yapılandırma ayarında bağlantı dizesi olarak tanımlanır. Aşağıdaki örnek, Visual Studio'da yeni bir Bulut Hizmeti projesi için oluşturulan varsayılan bağlantı dizesini gösterir:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Bir Azure depolama hesabının hesap bilgilerini sağlamak için bu bağlantı dizesini değiştirebilirsiniz.

Toplanan tanılama verilerinin türüne bağlı olarak Azure Tanılama, Blob hizmetini veya Tablo hizmetini kullanır. Aşağıdaki tablo, kalıcı olan veri kaynaklarını ve biçimlerini gösterir.

| Veri kaynağı | Depolama biçimi |
| --- | --- |
| Azure günlükleri |Tablo |
| IIS 7.0 günlükleri |Blob |
| Azure Tanılama altyapısı günlükleri |Tablo |
| Başarısız İstek İzleme günlükleri |Blob |
| Windows Olay günlükleri |Tablo |
| Performans sayaçları |Tablo |
| Kilitlenme bilgi dökümleri |Blob |
| Özel hata günlükleri |Blob |

## <a name="transfer-diagnostic-data"></a>Tanılama verilerini aktarma
SDK 2.5 ve sonrası için tanılama verilerinin aktarılması isteği yapılandırma dosyası üzerinden oluşabilir. Tanılama verilerini yapılandırmada belirtildiği şekilde zamanlanmış aralıklarla aktarabilirsiniz.

SDK 2.4 ve önceki için tanılama verilerini yapılandırma dosyası üzerinden ve programlı olarak aktarmayı talep edebilirsiniz. Programlı yaklaşım, isteğe bağlı transferler yapmanızı da sağlar.

> [!IMPORTANT]
> Tanılama verilerini bir Azure depolama hesabına aktardığınızda, tanılama verilerinizin kullandığı depolama kaynakları için maliyete maruz kaldığınız zaman.
> 
> 

## <a name="store-diagnostic-data"></a>Tanılama verilerini depolama
Günlük verileri blob veya tablo depolama sında aşağıdaki adlarla depolanır:

**Tablolar**

* **WadLogsTable** - İzleme dinleyicisi kullanılarak kodla yazılmış günlükler.
* **WADDiagnosticInfrastructureLogsTable** - Tanılama monitörü ve yapılandırma değişiklikleri.
* **WADDirectoriesTable** – Tanı monitörü izleme dizinleri.  Buna IIS günlükleri, IIS başarısız istek günlükleri ve özel dizinler dahildir.  Blob günlük dosyasının konumu Kapsayıcı alanında belirtilir ve blob adı RelativePath alanındadır.  AbsolutePath alanı, Azure sanal makinesinde var olduğu gibi dosyanın konumunu ve adını gösterir.
* **WADPerformanceCountersTable** – Performans sayaçları.
* **WADWindowsEventLogsTable** – Windows Olay günlükleri.

**Bloblar**

* **wad-control-container** – (Yalnızca SDK 2.4 ve öncekiler için) Azure tanılamalarını kontrol eden XML yapılandırma dosyalarını içerir.
* **wad-iis-failedreqlogfiles** – IIS Failed Request günlüklerinden bilgi içerir.
* **wad-iis-logfiles** – IIS günlükleri hakkında bilgi içerir.
* **"Özel"** – Tanı monitörü tarafından izlenen dizinleri yapılandırmaya dayalı özel bir kapsayıcı.  Bu blob kapsayıcının adı WADDirectoriesTable'da belirtilir.

## <a name="tools-to-view-diagnostic-data"></a>Tanılama verilerini görüntülemek için araçlar
Depolama alanına aktarıldıktan sonra verileri görüntülemek için çeşitli araçlar kullanılabilir. Örnek:

* Visual Studio'da Sunucu Gezgini - Microsoft Visual Studio için Azure Araçlarını yüklediyseniz, Azure depolama hesaplarınızdaki salt okunur blob ve tablo verilerini görüntülemek için Server Explorer'daki Azure Depolama düğümunu kullanabilirsiniz. Verileri yerel depolama emülatör hesabınızdan ve Azure için oluşturduğunuz depolama hesaplarından görüntüleyebilirsiniz. Daha fazla bilgi için Bkz. [Sunucu Gezgini ile Depolama Kaynaklarını Tarama ve Yönetme.](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage)
* [Microsoft Azure Depolama Gezgini,](../vs-azure-tools-storage-manage-with-storage-explorer.md) Windows, OSX ve Linux'taki Azure Depolama verileriyle kolayca çalışmanızı sağlayan bağımsız bir uygulamadır.
* [Azure Yönetim Stüdyosu,](https://www.cerebrata.com/products/azure-management-studio/introduction) Azure'da çalışan uygulamalar tarafından toplanan tanılama verilerini görüntülemenize, indirmenize ve yönetmenize olanak tanıyan Azure Diagnostics Manager'ı içerir.

## <a name="next-steps"></a>Sonraki Adımlar
[Azure Tanılama ile Bulut Hizmetleri uygulamasındaki akışı izleme](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)


