---
title: Azure Işlevleri bağlama uzantılarını kaydetme
description: Ortamınıza bağlı olarak bir Azure Işlevleri bağlama uzantısı kaydetmeyi öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 07/08/2019
ms.author: cshoe
ms.openlocfilehash: 1688fe848beb62731391bf4399a0dabec5265320
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79277524"
---
# <a name="register-azure-functions-binding-extensions"></a>Azure Işlevleri bağlama uzantılarını kaydetme

Azure Işlevleri sürüm 2. x, [bağlama](./functions-triggers-bindings.md) işlevleri çalışma zamanından ayrı paketler olarak kullanılabilir. .NET işlevleri NuGet paketleri aracılığıyla bağlamalara erişirken, uzantı paketleri diğer işlevlere bir yapılandırma ayarı aracılığıyla tüm bağlamalara erişmesine izin verir.

Bağlama uzantıları ile ilgili aşağıdaki öğeleri göz önünde bulundurun:

- Bağlama uzantıları, [Visual Studio kullanarak C# sınıf kitaplığı oluşturma](#local-csharp)dışında 1. x işlevlerine açık olarak kaydolmazlar.

- HTTP ve Zamanlayıcı Tetikleyicileri varsayılan olarak desteklenir ve bir uzantı gerektirmez.

Aşağıdaki tablo, bağlamaları ne zaman ve nasıl kaydedeceğinizi gösterir.

| Geliştirme ortamı |Kayıt<br/> 1. x Işlevleri içinde  |Kayıt<br/> 2. x Işlevleri  |
|-------------------------|------------------------------------|------------------------------------|
|Azure portal|Automatic|Automatic|
|Non-.NET dilleri veya yerel Azure Core araçları geliştirme|Automatic|[Azure Functions Core Tools ve uzantı paketleri kullanma](#extension-bundles)|
|Visual Studio kullanarak C# sınıf kitaplığı|[NuGet araçlarını kullanma](#vs)|[NuGet araçlarını kullanma](#vs)|
|Visual Studio Code kullanarak C# sınıf kitaplığı|Yok|[.NET Core CLI kullan](#vs-code)|

## <a name="extension-bundles-for-local-development"></a><a name="extension-bundles"></a>Yerel geliştirme için uzantı paketleri

Uzantı demeti, işlev uygulamanıza uygun bir Işlev kümesi bağlama uzantısı eklemenize olanak sağlayan bir dağıtım teknolojisidir. Uygulamanızı oluşturduğunuzda önceden tanımlanmış bir uzantılar kümesi eklenir. Bir pakette tanımlanan uzantı paketleri birbirleriyle uyumludur, bu da paketler arasındaki çakışmalardan kaçınmanıza yardımcı olur. Uygulamanın Host. json dosyasında uzantı paketleri 'ni etkinleştirirsiniz.  

Uzantı paketlerini, sürüm 2. x ve sonraki sürümler çalışma zamanının sürümleriyle birlikte kullanabilirsiniz. Yerel olarak geliştirme yaparken [Azure Functions Core Tools](functions-run-local.md#v2)en son sürümünü kullandığınızdan emin olun.

Azure Functions Core Tools, Visual Studio Code kullanarak ve uzaktan oluştururken yerel geliştirme için uzantı paketleri kullanın.

Uzantı paketleri kullanmıyorsanız, herhangi bir bağlama uzantısı yüklemeden önce .NET Core 2. x SDK 'sını yerel bilgisayarınıza yüklemelisiniz. Uzantı demeti yerel geliştirme için bu gereksinimi ortadan kaldırır. 

Uzantı paketleri 'ni kullanmak için, *Host. JSON* dosyasını aşağıdaki girişi içerecek şekilde güncelleştirin `extensionBundle`:
 
[!INCLUDE [functions-extension-bundles-json](../../includes/functions-extension-bundles-json.md)]

<a name="local-csharp"></a>

## <a name="c-class-library-with-visual-studio"></a><a name="vs"></a>Visual\# Studio ile C sınıfı kitaplığı

**Visual Studio**'da, aşağıdaki örnekte gösterildiği gibi [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) komutunu kullanarak paket yöneticisi konsolundan paketleri yükleyebilirsiniz:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

Belirli bir bağlama için kullanılan paketin adı, söz konusu bağlamaya ilişkin başvuru makalesinde sunulmaktadır. Bir örnek için [Service Bus bağlama başvurusu makalesinin paketler bölümüne](functions-bindings-service-bus.md#functions-1x)bakın.

Örnekteki `<TARGET_VERSION>` gibi, paketin belirli bir sürümü ile değiştirin `3.0.0-beta5`. Geçerli sürümler, [NuGet.org](https://nuget.org)adresindeki ayrı paket sayfalarında listelenir. Çalışma zamanı 1. x veya 2. x Işlevlerine karşılık gelen ana sürümler bağlama için başvuru makalesinde belirtilmiştir.

Bağlama başvurmak için `Install-Package` kullanırsanız, [uzantı paketleri](#extension-bundles)kullanmanız gerekmez. Bu yaklaşım, Visual Studio 'da oluşturulan sınıf kitaplıkları için özeldir.

## <a name="c-class-library-with-visual-studio-code"></a><a name="vs-code"></a>Visual Studio Code ile C# sınıf kitaplığı

**Visual Studio Code**' de, bir C# sınıf kitaplığı projesi için paketleri, .NET Core CLI [DotNet Add Package](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) komutunu kullanarak komut isteminden yükler. Aşağıdaki örnek nasıl bağlama ekleneceğini gösterir:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.<BINDING_TYPE_NAME> --version <TARGET_VERSION>
```

.NET Core CLI yalnızca Azure Işlevleri 2. x geliştirme için kullanılabilir.

Gereken `<BINDING_TYPE_NAME>` bağlamayı içeren paketin adıyla değiştirin. İstenen bağlama başvuru makalesini [desteklenen bağlamalar listesinde](./functions-triggers-bindings.md#supported-bindings)bulabilirsiniz.

Örnekteki `<TARGET_VERSION>` gibi, paketin belirli bir sürümü ile değiştirin `3.0.0-beta5`. Geçerli sürümler, [NuGet.org](https://nuget.org)adresindeki ayrı paket sayfalarında listelenir. Çalışma zamanı 1. x veya 2. x Işlevlerine karşılık gelen ana sürümler bağlama için başvuru makalesinde belirtilmiştir.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure Işlev tetikleyicisi ve bağlama örneği](./functions-bindings-example.md)
