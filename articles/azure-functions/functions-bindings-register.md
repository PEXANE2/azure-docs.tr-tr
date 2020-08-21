---
title: Azure Işlevleri bağlama uzantılarını kaydetme
description: Ortamınıza bağlı olarak bir Azure Işlevleri bağlama uzantısı kaydetmeyi öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 08/16/2020
ms.author: cshoe
ms.openlocfilehash: 942ca3229808b57894598c3477e9dc97e40e8c80
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88689584"
---
# <a name="register-azure-functions-binding-extensions"></a>Azure Işlevleri bağlama uzantılarını kaydetme

Azure Işlevleri sürüm 2. x ile başlayarak, işlevler çalışma zamanı yalnızca HTTP ve Zamanlayıcı tetikleyicilerini varsayılan olarak içerir. Diğer [Tetikleyiciler ve bağlamalar](./functions-triggers-bindings.md) ayrı paketler olarak kullanılabilir.

.NET sınıf kitaplığı işlevleri uygulamaları, NuGet paketleri olarak projeye yüklenen bağlamaları kullanır. Uzantı paketleri, non-.NET Functions uygulamalarının .NET altyapısıyla uğraşmak zorunda kalmadan aynı bağlamaları kullanmasına izin verir.

Aşağıdaki tablo, bağlamaları ne zaman ve nasıl kaydedeceğinizi gösterir.

| Geliştirme ortamı |Kayıt<br/> 1. x Işlevleri içinde  |Kayıt<br/> 3. x/2. x Işlevleri içinde  |
|-------------------------|------------------------------------|------------------------------------|
|Azure portal|Automatic|Otomatik<sup>*</sup>|
|Non-.NET dilleri|Automatic|[Uzantı paketleri](#extension-bundles) kullan (önerilir) veya [uzantıları açıkça yükler](#explicitly-install-extensions)|
|Visual Studio kullanarak C# sınıf kitaplığı|[NuGet araçlarını kullanma](#vs)|[NuGet araçlarını kullanma](#vs)|
|Visual Studio Code kullanarak C# sınıf kitaplığı|N/A|[.NET Core CLI kullan](#vs-code)|

<sup>*</sup> Portalda uzantı paketleri kullanılmaktadır.

## <a name="access-extensions-in-non-net-languages"></a>Non-.NET dillerde erişim uzantıları

Java, JavaScript, PowerShell, Python ve özel Işleyici işlev uygulamaları için, bağlamalara erişmek için uzantı paketleri kullanmanız önerilir. Uzantı paketlerinin kullanılamaz olduğu durumlarda, bağlama uzantılarını açık bir şekilde yükleyebilirsiniz.

### <a name="extension-bundles"></a><a name="extension-bundles"></a>Uzantı demeti

Uzantı demeti, işlev uygulamanıza uyumlu bir bağlama uzantıları kümesi eklemenin bir yoludur. Dosya üzerinde, uygulamanın *host.js* uzantı paketlerini etkinleştirirsiniz.

Uzantı paketlerini, sürüm 2. x ve sonraki sürümler çalışma zamanının sürümleriyle birlikte kullanabilirsiniz.

Uzantı paketleri sürümlüdür. Her sürüm birlikte çalışmak üzere doğrulanan belirli bir bağlama uzantıları kümesi içerir. Uygulamanızda ihtiyaç duyduğunuz uzantılara göre bir paket sürümü seçin.

İşlev uygulamanıza Uzantı paketi eklemek için `extensionBundle` *host.js*bölümüne ekleyin. Çoğu durumda, Visual Studio Code ve Azure Functions Core Tools sizin için otomatik olarak ekler.

[!INCLUDE [functions-extension-bundles-json](../../includes/functions-extension-bundles-json.md)]

Aşağıdaki tabloda, varsayılan *Microsoft. Azure. Functions. extensionpaket* paketi 'nin şu anda kullanılabilir olan sürümleri ve içerdikleri uzantılara bağlantılar listelenmektedir.

| Paket sürümü | host.jssürümünde sürüm | Dahil edilen uzantılar |
| --- | --- | --- |
| 'in | `[1.*, 2.0.0)` | Paketi oluşturmak için kullanılan [extensions.js](https://github.com/Azure/azure-functions-extension-bundles/blob/v1.x/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json) bakın |
| 2.x | `[2.*, 3.0.0)` | Paketi oluşturmak için kullanılan [extensions.js](https://github.com/Azure/azure-functions-extension-bundles/blob/v2.x/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json) bakın |

> [!NOTE]
> Üzerinde host.jsözel bir sürüm aralığı belirtebilirsiniz ancak bu tablodan bir sürüm değeri kullanmanızı öneririz.

### <a name="explicitly-install-extensions"></a><a name="explicitly-install-extensions"></a>Uzantıları açıkça yükler

[!INCLUDE [functions-extension-register-core-tools](../../includes/functions-extension-register-core-tools.md)]

## <a name="install-extensions-from-nuget-in-net-languages"></a><a name="local-csharp"></a>.NET dilinde NuGet 'den uzantıları yükler

C# sınıf kitaplığı tabanlı işlevler projesi için doğrudan uzantıları yüklemelisiniz. Uzantı paketleri özellikle C# sınıf kitaplığı tabanlı olmayan projeler için tasarlanmıştır.

### <a name="c-class-library-with-visual-studio"></a><a name="vs"></a>\#Visual Studio ile C sınıfı kitaplığı

**Visual Studio**'da, aşağıdaki örnekte gösterildiği gibi [Install-Package](/nuget/tools/ps-ref-install-package) komutunu kullanarak paket yöneticisi konsolundan paketleri yükleyebilirsiniz:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

Belirli bir bağlama için kullanılan paketin adı, söz konusu bağlamaya ilişkin başvuru makalesinde sunulmaktadır. Bir örnek için [Service Bus bağlama başvurusu makalesinin paketler bölümüne](functions-bindings-service-bus.md#functions-1x)bakın.

`<TARGET_VERSION>`Örnekteki gibi, paketin belirli bir sürümü ile değiştirin `3.0.0-beta5` . Geçerli sürümler, [NuGet.org](https://nuget.org)adresindeki ayrı paket sayfalarında listelenir. Çalışma zamanı 1. x veya 2. x Işlevlerine karşılık gelen ana sürümler bağlama için başvuru makalesinde belirtilmiştir.

`Install-Package`Bağlama başvurmak için kullanırsanız, [uzantı paketleri](#extension-bundles)kullanmanız gerekmez. Bu yaklaşım, Visual Studio 'da oluşturulan sınıf kitaplıkları için özeldir.

### <a name="c-class-library-with-visual-studio-code"></a><a name="vs-code"></a> Visual Studio Code ile C# sınıf kitaplığı

**Visual Studio Code**' de, bir C# sınıf kitaplığı projesi için paketleri, .NET Core CLI [DotNet Add Package](/dotnet/core/tools/dotnet-add-package) komutunu kullanarak komut isteminden yükler. Aşağıdaki örnek nasıl bağlama ekleneceğini gösterir:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.<BINDING_TYPE_NAME> --version <TARGET_VERSION>
```

.NET Core CLI yalnızca Azure Işlevleri 2. x geliştirme için kullanılabilir.

`<BINDING_TYPE_NAME>`Gereken bağlamayı içeren paketin adıyla değiştirin. İstenen bağlama başvuru makalesini [desteklenen bağlamalar listesinde](./functions-triggers-bindings.md#supported-bindings)bulabilirsiniz.

`<TARGET_VERSION>`Örnekteki gibi, paketin belirli bir sürümü ile değiştirin `3.0.0-beta5` . Geçerli sürümler, [NuGet.org](https://nuget.org)adresindeki ayrı paket sayfalarında listelenir. Çalışma zamanı 1. x veya 2. x Işlevlerine karşılık gelen ana sürümler bağlama için başvuru makalesinde belirtilmiştir.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure Işlev tetikleyicisi ve bağlama örneği](./functions-bindings-example.md)
