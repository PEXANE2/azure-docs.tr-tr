---
title: Azure Işlevleri bağlama uzantılarını kaydetme
description: Ortamınıza bağlı olarak bir Azure Işlevleri bağlama uzantısı kaydetmeyi öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 07/08/2019
ms.author: cshoe
ms.openlocfilehash: a045ef0fea70347f168e8ae0cc93e0c359f31dfa
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88031140"
---
# <a name="register-azure-functions-binding-extensions"></a>Azure Işlevleri bağlama uzantılarını kaydetme

Azure Işlevleri sürüm 2. x ile başlayarak, [bağlama](./functions-triggers-bindings.md) işlevleri çalışma zamanından ayrı paketler olarak kullanılabilir. .NET işlevleri NuGet paketleri aracılığıyla bağlamalara erişirken, uzantı paketleri diğer işlevlere bir yapılandırma ayarı aracılığıyla tüm bağlamalara erişmesine izin verir.

Bağlama uzantıları ile ilgili aşağıdaki öğeleri göz önünde bulundurun:

- Bağlama uzantıları, [Visual Studio kullanarak C# sınıf kitaplığı oluşturma](#local-csharp)dışında 1. x işlevlerine açık olarak kaydolmazlar.

- HTTP ve Zamanlayıcı Tetikleyicileri varsayılan olarak desteklenir ve bir uzantı gerektirmez.

Aşağıdaki tablo, bağlamaları ne zaman ve nasıl kaydedeceğinizi gösterir.

| Geliştirme ortamı |Kayıt<br/> 1. x Işlevleri içinde  |Kayıt<br/> 3. x/2. x Işlevleri içinde  |
|-------------------------|------------------------------------|------------------------------------|
|Azure portal|Automatic|Otomatik<sup>*</sup>|
|Non-.NET dilleri veya yerel Azure Core araçları geliştirme|Automatic|[Azure Functions Core Tools ve uzantı paketleri kullanma](#extension-bundles)|
|Visual Studio kullanarak C# sınıf kitaplığı|[NuGet araçlarını kullanma](#vs)|[NuGet araçlarını kullanma](#vs)|
|Visual Studio Code kullanarak C# sınıf kitaplığı|Yok|[.NET Core CLI kullan](#vs-code)|

<sup>*</sup>Portalda uzantı paketleri kullanılmaktadır.

## <a name="extension-bundles"></a><a name="extension-bundles"></a>Uzantı demeti

Uzantı demeti, işlev uygulamanıza uyumlu bir Işlev kümesi bağlama uzantısı eklemektir. Paketleri kullanırken, uygulamanızı oluşturduğunuzda önceden tanımlanmış bir uzantılar kümesi eklenir. Bir paketteki tanımlı uzantı paketleri birbirleriyle uyumlu olacak şekilde doğrulanır, bu da paketler arasındaki çakışmalardan kaçınmanıza yardımcı olur. Uzantı demeti bir non-.NET Functions projesiyle .NET proje kodu yayımlamak zorunda kalmamak için izin verir. Dosya üzerinde, uygulamanın host.jsuzantı paketlerini etkinleştirirsiniz.  

Uzantı paketlerini, sürüm 2. x ve sonraki sürümler çalışma zamanının sürümleriyle birlikte kullanabilirsiniz. 

Azure Functions Core Tools, Visual Studio Code kullanarak ve uzaktan oluştururken yerel geliştirme için uzantı paketleri kullanın. Yerel olarak geliştirme yaparken [Azure Functions Core Tools](functions-run-local.md#v2)en son sürümünü kullandığınızdan emin olun. Uzantı paketleri, Azure portal işlevler geliştirirken de kullanılır. 

Uzantı paketleri kullanmıyorsanız, [herhangi bir bağlama uzantısını açıkça yüklemeden](#explicitly-install-extensions)önce, yerel bilgisayarınıza .NET Core 2. x SDK 'sını yüklemelisiniz. Gerekli uzantıları açıkça tanımlayan bir Extensions. csproj dosyası projenize eklenir. Uzantı demeti yerel geliştirme için bu gereksinimleri ortadan kaldırır. 

Uzantı paketlerini kullanmak için, dosyadaki *host.js* aşağıdaki girişi içerecek şekilde güncelleştirin `extensionBundle` :
 
[!INCLUDE [functions-extension-bundles-json](../../includes/functions-extension-bundles-json.md)]

## <a name="explicitly-install-extensions"></a>Uzantıları açıkça yükler

[!INCLUDE [functions-extension-register-core-tools](../../includes/functions-extension-register-core-tools.md)]

## <a name="nuget-packages"></a><a name="local-csharp"></a>NuGet paketleri

C# sınıf kitaplığı tabanlı işlevler projesi için, uzantı paketleri, sınıf olmayan projeler için özel olarak tasarlanmıştır. 

### <a name="c-class-library-with-visual-studio"></a><a name="vs"></a>\#Visual Studio ile C sınıfı kitaplığı

**Visual Studio**'da, aşağıdaki örnekte gösterildiği gibi [Install-Package](/nuget/tools/ps-ref-install-package) komutunu kullanarak paket yöneticisi konsolundan paketleri yükleyebilirsiniz:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

Belirli bir bağlama için kullanılan paketin adı, söz konusu bağlamaya ilişkin başvuru makalesinde sunulmaktadır. Bir örnek için [Service Bus bağlama başvurusu makalesinin paketler bölümüne](functions-bindings-service-bus.md#functions-1x)bakın.

`<TARGET_VERSION>`Örnekteki gibi, paketin belirli bir sürümü ile değiştirin `3.0.0-beta5` . Geçerli sürümler, [NuGet.org](https://nuget.org)adresindeki ayrı paket sayfalarında listelenir. Çalışma zamanı 1. x veya 2. x Işlevlerine karşılık gelen ana sürümler bağlama için başvuru makalesinde belirtilmiştir.

`Install-Package`Bağlama başvurmak için kullanırsanız, [uzantı paketleri](#extension-bundles)kullanmanız gerekmez. Bu yaklaşım, Visual Studio 'da oluşturulan sınıf kitaplıkları için özeldir.

## <a name="c-class-library-with-visual-studio-code"></a><a name="vs-code"></a>Visual Studio Code ile C# sınıf kitaplığı

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
