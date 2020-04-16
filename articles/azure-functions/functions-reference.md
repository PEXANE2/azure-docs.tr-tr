---
title: Azure İşlevlerini Geliştirme Kılavuzu
description: Azure'da tüm programlama dilleri ve ciltlemelerde işlevler geliştirmek için ihtiyacınız olan Azure İşlevleri kavramlarını ve tekniklerini öğrenin.
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.topic: conceptual
ms.date: 10/12/2017
ms.openlocfilehash: b6af3d7ab1fdd35391c9a189162c57dfb259f2d4
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81405349"
---
# <a name="azure-functions-developer-guide"></a>Azure İşlevleri geliştirici kılavuzu
Azure İşlevlerinde, kullandığınız dil veya bağlamadan bağımsız olarak belirli işlevler birkaç temel teknik kavramı ve bileşeni paylaşır. Belirli bir dile veya bağlamaya özgü öğrenme ayrıntılarına atlamadan önce, hepsi için geçerli olan bu genel bakışı okuduğunuzdan emin olun.

Bu makalede, [Azure İşlevlerine genel bakışı](functions-overview.md)zaten okuduğunuz varsayar.

## <a name="function-code"></a>İşlev kodu
*İşlev,* Azure İşlevlerinde birincil kavramdır. Bir işlev iki önemli parça içerir - kodunuz, çeşitli dillerde yazılabilir ve bazı config, function.json dosyası. Derlenmiş diller için bu config dosyası, kodunuzdaki ek açıklamalartan otomatik olarak oluşturulur. Komut dosyası dilleri için config dosyasını kendiniz sağlamanız gerekir.

function.json dosyası işlevin tetikleyicisini, bağlamalarını ve diğer yapılandırma ayarlarını tanımlar. Her işlevde tek bir tane tetikleyici olması gerekir. Çalışma zamanı, izlenecek olayları ve bir işlev yürütmesinden veri aktarılabilmek ve döndürmek için bu config dosyasını kullanır. Aşağıda örnek bir function.json dosyası verilmiştir.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Daha fazla bilgi için Azure [İşlevleri tetikleyicileri ve bağlama kavramlarını](functions-triggers-bindings.md)görün.

Özellik, `bindings` hem tetikleyicileri hem de bağlamaları yapılandırdığınız yerdir. Her bağlama, belirli bir bağlama türüne özgü birkaç ortak ayarı ve bazı ayarları paylaşır. Her bağlama aşağıdaki ayarları gerektirir:

| Özellik | Değerler/Türler | Yorumlar |
| --- | --- | --- |
| `type` |string |Bağlama türü. Örneğin, `queueTrigger`. |
| `direction` |'in', 'out' |Bağlamanın işleve veri almak veya işlevden veri göndermek için mi olduğunu gösterir. |
| `name` |string |İşlevdeki bağlı veriler için kullanılan ad. C# için bu bir bağımsız değişken adıdır; JavaScript için, bir anahtar/değer listesindeki anahtardır. |

## <a name="function-app"></a>İşlev uygulaması
İşlev uygulaması, Azure'da işlevlerinizin çalıştığı bir yürütme bağlamı sağlar. Bu nedenle, işlevleriniz için dağıtım ve yönetim birimidir. Bir işlev uygulaması, birlikte yönetilen, dağıtılan ve ölçeklenen bir veya daha fazla bireysel işlevden oluşur. İşlev uygulamasındaki tüm işlevler aynı fiyatlandırma planını, dağıtım yöntemini ve çalışma zamanı sürümünü paylaşır. Bir işlev uygulamasını, işlevlerinizi düzenlemenin ve topluolarak yönetmenin bir yolu olarak düşünün. Daha fazla bilgi edinmek için bir [işlev uygulamasını nasıl yönetebilirsiniz](functions-how-to-use-azure-function-app-settings.md)' e bakın. 

> [!NOTE]
> Bir işlev uygulamasındaki tüm işlevler aynı dilde yazılmalıdır. Azure İşlevleri çalışma zamanının [önceki sürümlerinde](functions-versions.md) bu gerekli değildi.

## <a name="folder-structure"></a>Klasör yapısı
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Yukarıdaki, bir İşlev uygulaması için varsayılan (ve önerilen) klasör yapısıdır. Bir işlevin kodunun dosya konumunu değiştirmek isterseniz, `scriptFile` _function.json_ dosyasının bölümünü değiştirin. Projenizi Azure'daki işlev uygulamanıza dağıtmak için [paket dağıtımı](deployment-zip-push.md) kullanmanızı da öneririz. [Sürekli tümleştirme ve dağıtım ve](functions-continuous-deployment.md) Azure DevOps gibi varolan araçları da kullanabilirsiniz.

> [!NOTE]
> Paketi el ile dağıtıyorsanız, ana _bilgisayar dosyanızı_ ve işlev klasörlerinizi `wwwroot` doğrudan klasöre dağıttığınızdan emin olun. Klasörü `wwwroot` dağıtımlarınıza eklemeyin. Aksi takdirde, klasörler ile `wwwroot\wwwroot` sona erer.

#### <a name="use-local-tools-and-publishing"></a>Yerel araçları ve yayımlamayı kullanma
İşlev uygulamaları [Visual Studio, Visual Studio](./functions-develop-vs.md) [Code,](functions-create-first-function-vs-code.md) [IntelliJ,](./functions-create-maven-intellij.md) [Eclipse](./functions-create-maven-eclipse.md)ve [Azure İşleme Temel Araçları](./functions-develop-local.md)gibi çeşitli araçlar kullanılarak yazılabilir ve yayınlanabilir. Daha fazla bilgi için [Kod'a bakın ve Azure İşlevlerini yerel olarak test edin.](./functions-develop-local.md)

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --glenga -->

## <a name="how-to-edit-functions-in-the-azure-portal"></a><a id="fileupdate"></a>Azure portalında işlevler nasıl dedilir?
Azure portalında yerleşik Olan İşlevler düzenleyicisi, kodunuzu ve *function.json* dosyanızı doğrudan satır satırda güncelleştirmenize olanak tanır. Bu sadece küçük değişiklikler veya kavram kanıtları için tavsiye edilir - en iyi uygulama VS Kodu gibi yerel bir geliştirme aracı kullanmaktır.

## <a name="parallel-execution"></a>Paralel yürütme
Tek iş parçacığı işlevi çalışma süresinden daha hızlı birden çok tetikleyici olay meydana geldiğinde, çalışma zamanı işlevi paralel olarak birden çok kez çağırabilir.  Bir işlev uygulaması [Tüketim barındırma planını](functions-scale.md#how-the-consumption-and-premium-plans-work)kullanıyorsa, işlev uygulaması otomatik olarak ölçeklendirilebilir.  İşlev uygulamasının her örneği, uygulama ister Tüketim barındırma planında ister normal bir [Uygulama Hizmeti barındırma planında](../app-service/overview-hosting-plans.md)çalışır, eşzamanlı işlev çağrılarını birden çok iş parçacığı kullanarak paralel olarak işleyebilir.  Her işlev uygulaması örneğindeki en fazla eşzamanlı işlev çağrı sayısı, kullanılan tetikleyici türüne ve işlev uygulamasındaki diğer işlevler tarafından kullanılan kaynaklara bağlı olarak değişir.

## <a name="functions-runtime-versioning"></a>İşlevler çalışma zamanı sürüm

Uygulama ayarını kullanarak `FUNCTIONS_EXTENSION_VERSION` İşlevler çalışma zamanı sürümünü yapılandırabilirsiniz. Örneğin, "~3" değeri, İşlev Uygulamanızın ana sürümü olarak 3.x kullanacağını gösterir. Fonksiyon Uygulamaları, yayımlandıkça her yeni küçük sürüme yükseltilir. İşlev uygulamanızın tam sürümünü nasıl görüntüleyeceksiniz gibi daha fazla bilgi için [Azure İşlevleri çalışma zamanı sürümlerini nasıl hedefleyene](set-runtime-version.md)bakın.

## <a name="repositories"></a>Depolar
Azure İşlevler için kod açık kaynak koddur ve GitHub depolarında depolanır:

* [Azure İşlevleri](https://github.com/Azure/Azure-Functions)
* [Azure İşlevler ana bilgisayar](https://github.com/Azure/azure-functions-host/)
* [Azure İşlevler portalı](https://github.com/azure/azure-functions-ux)
* [Azure İşlevler şablonları](https://github.com/azure/azure-functions-templates)
* [Azure Web İşleri SDK'sı](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure Web İşleri SDK'sı Uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Bağlamalar
Burada desteklenen tüm bağlamaların bir tablosu vardır.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

Ciltler gelen hatalar ile ilgili sorunlar mı var? Azure [İşlevlerini Bağlama Hata Kodları](functions-bindings-error-pages.md) belgelerini gözden geçirin.

## <a name="reporting-issues"></a>Raporlama Sorunları
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure Fonksiyonları tetikler ve bağlamalar](functions-triggers-bindings.md)
* [Azure İşlevleri’ni yerel olarak kodlama ve test etme](./functions-develop-local.md)
* [Azure İşlevler için En İyi Uygulamalar](functions-best-practices.md)
* [Azure İşlevler C# geliştirici başvurusu](functions-dotnet-class-library.md)
* [Azure İşleçler Düğüm.js geliştirici başvurusu](functions-reference-node.md)
