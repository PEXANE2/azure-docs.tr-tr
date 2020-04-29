---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 873fd8cbc211f098c93b8fb3fbe701e4a34d8487
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "68320495"
---
Ayarlar `Logging` , Kapsayıcınız için ASP.NET Core günlük desteğini yönetir. ASP.NET Core bir uygulama için kullandığınız, Kapsayıcınız için aynı yapılandırma ayarlarını ve değerlerini kullanabilirsiniz. 

Aşağıdaki günlük oluşturma sağlayıcıları kapsayıcı tarafından desteklenir:

|Sağlayıcı|Amaç|
|--|--|
|[Konsol](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|ASP.NET Core `Console` günlük sağlayıcısı. Bu günlüğe kaydetme sağlayıcısı için ASP.NET Core yapılandırma ayarları ve varsayılan değerler desteklenir.|
|[H](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|ASP.NET Core `Debug` günlük sağlayıcısı. Bu günlüğe kaydetme sağlayıcısı için ASP.NET Core yapılandırma ayarları ve varsayılan değerler desteklenir.|
|[Disk](#disk-logging)|JSON günlük sağlayıcısı. Bu günlüğe kaydetme sağlayıcısı günlük verilerini çıkış bağlamalarına yazar.|

Bu kapsayıcı komutu, günlük bilgilerini JSON biçiminde çıktı bağlamaya depolar:

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

Bu kapsayıcı komutu, kapsayıcı çalışırken, ön eki `dbug`olan hata ayıklama bilgilerini gösterir:

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
| `Format` | Dize | Günlük dosyaları için çıkış biçimi.<br/> **Note:** Günlüğe kaydetme sağlayıcısını etkinleştirmek için bu `json` değerin olarak ayarlanması gerekir. Bu değer, bir kapsayıcıyı örneklarken bir çıkış bağlaması belirtilmeden belirtilirse bir hata oluşur. |
| `MaxFileSize` | Tamsayı | Günlük dosyasının megabayt (MB) cinsinden en büyük boyutu. Geçerli günlük dosyasının boyutu bu değeri karşıladığında veya aştığında, günlük sağlayıcısı tarafından yeni bir günlük dosyası başlatılır. -1 belirtilirse, günlük dosyasının boyutu, varsa çıkış bağlaması için en fazla dosya boyutuyla sınırlıdır. Varsayılan değer 1’dir. |

ASP.NET Core günlüğü desteğini yapılandırma hakkında daha fazla bilgi için bkz. [Ayarlar dosya yapılandırması](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1).

