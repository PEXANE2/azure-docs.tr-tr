---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 873fd8cbc211f098c93b8fb3fbe701e4a34d8487
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68320495"
---
Ayarlar, `Logging` kapsayıcınız için Core günlük ASP.NET desteği yönetir. Bir ASP.NET Core uygulaması için kullandığınız kapsayıcınız için aynı yapılandırma ayarlarını ve değerlerini kullanabilirsiniz. 

Aşağıdaki günlük sağlayıcıları kapsayıcı tarafından desteklenir:

|Sağlayıcı|Amaç|
|--|--|
|[Konsol](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|ASP.NET Core `Console` günlük sağlayıcısı. Bu günlük sağlayıcısının tüm ASP.NET Core yapılandırma ayarları ve varsayılan değerleri desteklenir.|
|[Hata ayıklama](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|ASP.NET Core `Debug` günlük sağlayıcısı. Bu günlük sağlayıcısının tüm ASP.NET Core yapılandırma ayarları ve varsayılan değerleri desteklenir.|
|[Disk](#disk-logging)|JSON günlük sağlayıcısı. Bu günlük sağlayıcısı çıkış montaj günlük verileri yazar.|

Bu kapsayıcı komutu, çıkış yuvasına JSON formatında günlüğe kaydetme bilgilerini depolar:

```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
Logging:Disk:Format=json
```

Bu kapsayıcı komutu, kapsayıcı çalışırken `dbug`önceden belirlenmiş hata ayıklama bilgilerini gösterir:

```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
Logging:Console:LogLevel:Default=Debug
```

### <a name="disk-logging"></a>Disk günlüğü

`Disk` Günlük sağlayıcısı aşağıdaki yapılandırma ayarlarını destekler:

| Adı | Veri türü | Açıklama |
|------|-----------|-------------|
| `Format` | Dize | Günlük dosyaları için çıkış biçimi.<br/> **Not:** Bu değer, günlük `json` sağlayıcısını etkinleştirmek için ayarlanmalıdır. Bu değer, bir kapsayıcıyı anında alırken bir çıkış yuvası belirtmeden de belirtilirse, bir hata oluşur. |
| `MaxFileSize` | Tamsayı | Günlük dosyasının megabayt (MB) boyutu. Geçerli günlük dosyasının boyutu bu değeri karşıladığında veya aştığında, günlük sağlayıcısı tarafından yeni bir günlük dosyası başlatılır. -1 belirtilirse, günlük dosyasının boyutu yalnızca çıktı montajı için yalnızca varsa maksimum dosya boyutuyla sınırlanır. Varsayılan değer 1’dir. |

Core günlük ASP.NET yapılandırma hakkında daha fazla bilgi için [Ayarlar dosya yapılandırması'na](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1)bakın.

