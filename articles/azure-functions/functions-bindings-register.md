---
title: Azure İşlevlerini bağlama uzantılarını kaydedin
description: Ortamınıza bağlı olarak Azure İşlevler bağlama uzantısı kaydetmeyi öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 07/08/2019
ms.author: cshoe
ms.openlocfilehash: 1688fe848beb62731391bf4399a0dabec5265320
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277524"
---
# <a name="register-azure-functions-binding-extensions"></a>Azure İşlevlerini bağlama uzantılarını kaydedin

Azure İşlevler sürüm 2.x'te, [bağlamalar](./functions-triggers-bindings.md) çalışma zamanındaki işlevlerden ayrı paketler olarak kullanılabilir. .NET işlevleri NuGet paketleri aracılığıyla bağlamalara erişirken, uzantı paketleri diğer işlevlerin yapılandırma ayarı aracılığıyla tüm bağlamalara erişmesine olanak sağlar.

Bağlama uzantılarıyla ilgili aşağıdaki öğeleri göz önünde bulundurun:

- Bağlama uzantıları, [Visual Studio'yı kullanarak C# sınıfı kitaplığı oluşturmadan](#local-csharp)işlevler 1.x'te açıkça kayıtlı değildir.

- HTTP ve zamanlayıcı tetikleyicileri varsayılan olarak desteklenir ve bir uzantı gerektirmez.

Aşağıdaki tablo, ciltleri ne zaman ve nasıl kaydettiğinizi gösterir.

| Geliştirme ortamı |Kayıt<br/> fonksiyonlar 1.x  |Kayıt<br/> fonksiyonlar 2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Azure portalında|Automatic|Automatic|
|Non-.NET dilleri veya yerel Azure Temel Araçları geliştirme|Automatic|[Azure İşletim Temel Araçları ve uzantı demetlerini kullanma](#extension-bundles)|
|Visual Studio kullanarak C# sınıfı kitaplığı|[NuGet araçlarını kullanma](#vs)|[NuGet araçlarını kullanma](#vs)|
|Visual Studio Code kullanarak C# sınıfı kitaplığı|Yok|[Kullan .NET Çekirdek CLI](#vs-code)|

## <a name="extension-bundles-for-local-development"></a><a name="extension-bundles"></a>Yerel kalkınma için uzatma paketleri

Uzantı paketleri, işlev uygulamanıza uyumlu bir Fonksiyon bağlama uzantıları kümesi eklemenize olanak tanıyan bir dağıtım teknolojisidir. Uygulamanızı oluştururken önceden tanımlanmış bir uzantı kümesi eklenir. Pakette tanımlanan uzantı paketleri birbiriyle uyumludur, bu da paketler arasındaki çakışmaları önlemenize yardımcı olur. Uygulamanın ana bilgisayar.json dosyasında uzantı demetlerini etkinleştirebilirsiniz.  

Sürüm 2.x ve Fonksiyonlar çalışma zamanının sonraki sürümleriyle uzantı demetlerini kullanabilirsiniz. Yerel olarak gelişirken, [Azure İşlevler Temel Araçları'nın](functions-run-local.md#v2)en son sürümünü kullandığınızdan emin olun.

Azure İşleçleri Temel Araçları, Visual Studio Code ve uzaktan oluşturduğunuzda kullanarak yerel geliştirme için uzantı demetlerini kullanın.

Uzantı demetleri kullanmıyorsanız, bağlama uzantılarıyüklemeden önce yerel bilgisayarınıza .NET Core 2.x SDK yüklemeniz gerekir. Uzantı demetleri, yerel geliştirme için bu gereksinimi kaldırır. 

Uzantı paketlerini kullanmak için *host.json* dosyasını aşağıdaki `extensionBundle`girişi içerecek şekilde güncelleştirin:
 
[!INCLUDE [functions-extension-bundles-json](../../includes/functions-extension-bundles-json.md)]

<a name="local-csharp"></a>

## <a name="c-class-library-with-visual-studio"></a><a name="vs"></a>Visual\# Studio ile C sınıfı kütüphane

**Visual**Studio'da, Aşağıdaki örnekte gösterildiği gibi Paket Yöneticisi Konsolundan [Yükleme-Paket](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) komutunu kullanarak paketleri yükleyebilirsiniz:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

Belirli bir bağlama için kullanılan paketin adı, bu bağlama için başvuru makalesinde verilmiştir. Örneğin, [Hizmet Veri Servisi bağlayıcı başvuru makalesinin Paketler bölümüne](functions-bindings-service-bus.md#functions-1x)bakın.

Örnekte paketin belirli bir sürümüyle değiştirin. `<TARGET_VERSION>` `3.0.0-beta5` Geçerli sürümler [NuGet.org](https://nuget.org)tek tek paket sayfalarında listelenir. 1.x veya 2.x işlevlerine karşılık gelen ana sürümler, bağlama için başvuru makalesinde belirtilir.

Bir bağlama `Install-Package` başvurmak için kullanıyorsanız, [uzantı demetleri](#extension-bundles)kullanmanız gerekmez. Bu yaklaşım Visual Studio'da yerleşik sınıf kitaplıkları için özeldir.

## <a name="c-class-library-with-visual-studio-code"></a><a name="vs-code"></a>Visual Studio Code ile C# sınıfı kitaplığı

**Visual Studio Code'da** [,.NET](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) Core CLI'deki paket ekle komutunu kullanarak komut isteminden C# sınıfı kitaplık projesi için paketleri yükleyin. Aşağıdaki örnek, nasıl bir bağlayıcı eklediğinizi gösterir:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.<BINDING_TYPE_NAME> --version <TARGET_VERSION>
```

.NET Core CLI yalnızca Azure İşlevler 2.x geliştirme için kullanılabilir.

İhtiyacınız olan bağlamayı içeren paketin adı ile değiştirin. `<BINDING_TYPE_NAME>` Desteklenen [bağlayıcılar listesinde](./functions-triggers-bindings.md#supported-bindings)istenen bağlayıcı başvuru makalesini bulabilirsiniz.

Örnekte paketin belirli bir sürümüyle değiştirin. `<TARGET_VERSION>` `3.0.0-beta5` Geçerli sürümler [NuGet.org](https://nuget.org)tek tek paket sayfalarında listelenir. 1.x veya 2.x işlevlerine karşılık gelen ana sürümler, bağlama için başvuru makalesinde belirtilir.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure İşlevi tetikleyicisi ve bağlama örneği](./functions-bindings-example.md)
