---
title: Azure Işlevleri bağlama uzantılarını kaydetme
description: Ortamınıza bağlı olarak bir Azure Işlevleri bağlama uzantısı kaydetmeyi öğrenin.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: reference
ms.date: 07/08/2019
ms.author: cshoe
ms.openlocfilehash: 93ced443a73d5499d8b305770c3c866c26d540f0
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70086475"
---
# <a name="register-azure-functions-binding-extensions"></a>Azure Işlevleri bağlama uzantılarını kaydetme

Azure Işlevleri sürüm 2. x, [bağlama](./functions-triggers-bindings.md) işlevleri çalışma zamanından ayrı paketler olarak kullanılabilir. .NET işlevleri NuGet paketleri aracılığıyla bağlamalara erişirken, uzantı paketleri diğer işlevlere bir yapılandırma ayarı aracılığıyla tüm bağlamalara erişmesine izin verir.

Bağlama uzantıları ile ilgili aşağıdaki öğeleri göz önünde bulundurun:

- Bağlama uzantıları, [Visual Studio kullanarak bir C# sınıf kitaplığı oluşturma](#local-csharp)dışında, 1. x işlevlerine açık olarak kayıtlı değildir.

- HTTP ve Zamanlayıcı Tetikleyicileri varsayılan olarak desteklenir ve bir uzantı gerektirmez.

Aşağıdaki tablo, bağlamaları ne zaman ve nasıl kaydedeceğinizi gösterir.

| Geliştirme ortamı |Kayıt<br/> 1\. x Işlevleri içinde  |Kayıt<br/> 2\. x Işlevleri  |
|-------------------------|------------------------------------|------------------------------------|
|Azure portal|Otomatik|Otomatik|
|Non-.NET dilleri veya yerel Azure Core araçları geliştirme|Otomatik|[Azure Functions Core Tools ve uzantı paketleri kullanma](#extension-bundles)|
|C#Visual Studio kullanarak sınıf kitaplığı|[NuGet araçlarını kullanma](#vs)|[NuGet araçlarını kullanma](#vs)|
|C#Visual Studio Code kullanarak sınıf kitaplığı|Yok|[.NET Core CLI kullan](#vs-code)|

## <a name="extension-bundles"></a>Yerel geliştirme için uzantı paketleri

Uzantı demeti, sürüm 2. x çalışma zamanına yönelik bir yerel geliştirme teknolojisidir. Bu, işlev uygulama projenize uyumlu bir Işlev kümesi bağlama uzantısı eklemenize olanak tanır. Bu uzantı paketleri daha sonra Azure 'a dağıtırken dağıtım paketine dahil edilir. Demeti, Microsoft tarafından yayımlanan tüm bağlamaları *Host. JSON* dosyasındaki bir ayar aracılığıyla kullanılabilir hale getirir. Bir pakette tanımlanan uzantı paketleri birbirleriyle uyumludur, bu da paketler arasındaki çakışmalardan kaçınmanıza yardımcı olur. Yerel olarak geliştirme yaparken [Azure Functions Core Tools](functions-run-local.md#v2)en son sürümünü kullandığınızdan emin olun.

Azure Functions Core Tools veya Visual Studio Code kullanarak tüm yerel geliştirmelerde uzantı paketleri kullanın.

Uzantı paketleri kullanmıyorsanız, herhangi bir bağlama uzantısı yüklemeden önce .NET Core 2. x SDK 'sını yerel bilgisayarınıza yüklemelisiniz. Demeti yerel geliştirme için bu gereksinimi ortadan kaldırır. 

Uzantı paketleri 'ni kullanmak için, *Host. JSON* dosyasını aşağıdaki girişi `extensionBundle`içerecek şekilde güncelleştirin:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

Aşağıdaki özellikler ' de `extensionBundle`mevcuttur:

| Özellik | Açıklama |
| -------- | ----------- |
| **`id`** | Microsoft Azure Işlevleri Uzantısı paketleri için ad alanı. |
| **`version`** | Yüklenecek paket sürümü. Işlevler çalışma zamanı her zaman, sürüm aralığı veya aralığı tarafından tanımlanan izin verilen en fazla sürümü seçer. Yukarıdaki sürüm değeri, 1.0.0 'e kadar tüm paket sürümlerinin 2.0.0 dahil değil, izin vermez. Daha fazla bilgi için bkz. [Sürüm aralıklarını belirtmek için Aralık gösterimi](https://docs.microsoft.com/nuget/reference/package-versioning#version-ranges-and-wildcards). |

Paket sürümleri artıkında paket değişim artışı. Ana sürüm değişiklikleri, paketteki paketler büyük bir sürümle artıkken oluşur. Bu, genellikle Işlevler çalışma zamanının ana sürümünde bir değişiklikle saatle çakışan.  

Varsayılan paket tarafından yüklenen geçerli uzantılar kümesi, bu [Extensions. json dosyasında](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json)numaralandırılır.

<a name="local-csharp"></a>

## <a name="vs"></a>Visual\# Studio ile C sınıfı kitaplığı

**Visual Studio**'da, aşağıdaki örnekte gösterildiği gibi [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) komutunu kullanarak paket yöneticisi konsolundan paketleri yükleyebilirsiniz:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

Belirli bir bağlama için kullanılan paketin adı, söz konusu bağlamaya ilişkin başvuru makalesinde sunulmaktadır. Bir örnek için [Service Bus bağlama başvurusu makalesinin paketler bölümüne](functions-bindings-service-bus.md#packages---functions-1x)bakın.

Örnekteki `<TARGET_VERSION>` gibi, `3.0.0-beta5`paketin belirli bir sürümü ile değiştirin. Geçerli sürümler, [NuGet.org](https://nuget.org)adresindeki ayrı paket sayfalarında listelenir. Çalışma zamanı 1. x veya 2. x Işlevlerine karşılık gelen ana sürümler bağlama için başvuru makalesinde belirtilmiştir.

Bağlama başvurmak için `Install-Package` kullanırsanız, [uzantı paketleri](#extension-bundles)kullanmanız gerekmez. Bu yaklaşım, Visual Studio 'da oluşturulan sınıf kitaplıkları için özeldir.

## <a name="vs-code"></a>C# Visual Studio Code ile sınıf kitaplığı

> [!NOTE]
> Işlevlerin, uyumlu bir bağlama uzantısı paketleri kümesini otomatik olarak yüklemesini sağlamak için [uzantı paketleri](#extension-bundles) kullanmanızı öneririz.

**Visual Studio Code**, .NET Core CLI bir sınıf kitaplığı projesi C# için paketleri, bir komut isteminden, [DotNet Add Package](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) komutunu kullanarak yükler. Aşağıdaki örnek nasıl bağlama ekleneceğini gösterir:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.<BINDING_TYPE_NAME> --version <TARGET_VERSION>
```

.NET Core CLI yalnızca Azure Işlevleri 2. x geliştirme için kullanılabilir.

İstediğiniz `<BINDING_TYPE_NAME>` bağlamanın başvuru makalesinde belirtilen paketin adıyla değiştirin. İstenen bağlama başvuru makalesini [desteklenen bağlamalar listesinde](./functions-triggers-bindings.md#supported-bindings)bulabilirsiniz.

Örnekteki `<TARGET_VERSION>` gibi, `3.0.0-beta5`paketin belirli bir sürümü ile değiştirin. Geçerli sürümler, [NuGet.org](https://nuget.org)adresindeki ayrı paket sayfalarında listelenir. Çalışma zamanı 1. x veya 2. x Işlevlerine karşılık gelen ana sürümler bağlama için başvuru makalesinde belirtilmiştir.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure Işlev tetikleyicisi ve bağlama örneği](./functions-bindings-example.md)
