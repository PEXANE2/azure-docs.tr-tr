---
title: Azure İşlevlerine Genel Bakış
description: Uyumsuz iş yüklerini dakikalar içinde iyileştirmek için Azure İşlevlerinin nasıl kullanılacağını anlayın.
author: mattchenderson
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.topic: overview
ms.date: 01/16/2020
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 1cd7b3db92de339b7614a4271f9e6793cdf8f2ad
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76292918"
---
# <a name="an-introduction-to-azure-functions"></a>Azure İşlevleri'ne giriş

Azure Işlevleri, uygulama altyapısı hakkında endişelenmeden küçük kod parçalarını ("işlevler" olarak adlandırılır) çalıştırmanıza olanak sağlar. Azure Işlevleri ile bulut altyapısı, uygulamanızı ölçeklendirerek çalışır durumda tutmanız için ihtiyacınız olan tüm güncel sunucuları sağlar.

Bir işlev, belirli bir olay türü tarafından "tetiklenir". [Desteklenen Tetikleyiciler](./functions-triggers-bindings.md) , verilerdeki değişikliklere yanıt vermeyi, iletilere yanıt vermeyi, bir zamanlamaya göre çalışmayı veya bir http isteğinin sonucunu içerir.

Her zaman doğrudan bir hizmetlere yönelik olarak kodlarken, diğer hizmetlerle tümleştirme, bağlamalar kullanılarak basitleştirilmiştir. Bağlamalar, [çok çeşitli Azure ve üçüncü taraf hizmetlere yönelik bildirime dayalı erişim](./functions-triggers-bindings.md)sağlar.

## <a name="features"></a>Özellikler

Azure Işlevlerinin bazı temel özellikleri şunlardır:

- **Sunucusuz uygulamalar**: işlevler Microsoft Azure [sunucusuz](https://azure.microsoft.com/solutions/serverless/) uygulamalar geliştirmenize olanak sağlar.

- **Dil seçimi**: tercih ettiğiniz [ C#, Java, JavaScript, Python ve PowerShell](supported-languages.md)'i kullanarak işlevleri yazın.

- **Kullanım başına ödeme fiyatlandırma modeli**: yalnızca kodunuzu çalıştırmak için harcanan süre için ödeme yapın. [Fiyatlandırma bölümünde](#pricing) Tüketim barındırma planı seçeneğine bakın.  

- **Kendi bağımlılıklarınızı getirin**: işlevler NuGet ve NPM 'yi destekler ve bu sayede en sevdiğiniz kitaplıklara erişebilirsiniz.

- **Tümleşik güvenlik**: Azure Active Directory, Facebook, Google, Twitter ve Microsoft hesabı gibi OAUTH sağlayıcılarıyla http ile tetiklenen işlevleri koruyun.

- **Basitleştirilmiş tümleştirme**: Azure hizmetleri ve hizmet olarak yazılım (SaaS) teklifleriyle kolayca tümleştirin.

- **Esnek geliştirme**: sürekli tümleştirme ayarlayın ve kodunuzu [GitHub](../app-service/scripts/cli-continuous-deployment-github.md), [Azure DevOps Services](../app-service/scripts/cli-continuous-deployment-vsts.md)ve diğer [desteklenen geliştirme araçları](../app-service/deploy-local-git.md)aracılığıyla dağıtın.

- **Durum bilgisi olan sunucusuz mimari**: [dayanıklı işlevler](durable/durable-functions-overview.md)ile sunucusuz uygulamalar düzenleyin.

- **Açık kaynak**: işlevler çalışma zamanı açık kaynaktır ve [GitHub 'da kullanılabilir](https://github.com/azure/azure-webjobs-sdk-script).

## <a name="what-can-i-do-with-functions"></a>İşlevler ile ne yapabilirim?

İşlevler, toplu veri işleme, sistemleri tümleştirme, Nesnelerin İnterneti (IoT) ile çalışma ve basit API 'Ler ve mikro hizmetler oluşturma için harika bir çözümdür.

Aşağıdakiler dahil olmak üzere önemli senaryolar kullanmaya başlamanızı sağlamak için bir dizi şablon mevcuttur:

- **Http**: [http isteklerini](functions-create-first-azure-function.md) temel alan kodu çalıştırma

- **Süreölçer**: kodu [önceden tanımlanmış saatlerde çalışacak](./functions-create-scheduled-function.md) şekilde zamanlayın

- **Azure Cosmos DB**: [yeni ve değiştirilmiş Azure Cosmos DB belgelerini](./functions-create-cosmos-db-triggered-function.md) işleme

- **BLOB depolama**: [yeni ve değiştirilmiş Azure depolama bloblarını](./functions-create-storage-blob-triggered-function.md) işleme

- **Kuyruk depolama**: [Azure depolama kuyruğu iletilerine](./functions-create-storage-queue-triggered-function.md) yanıt verme

- **Event Grid**: [abonelikler ve filtreler aracılığıyla Azure Event Grid olaylarına](../event-grid/resize-images-on-storage-blob-upload-event.md) yanıt verme

- **Olay Hub**'ı: [Azure Olay Hub 'ı olaylarının yüksek hacimlerinde](./functions-bindings-event-hubs.md) yanıt verme

- **Service Bus kuyruğu**: [Service Bus kuyruğu Iletilerini yanıtlayarak](./functions-bindings-service-bus.md) diğer Azure veya şirket içi hizmetlere bağlanın

- **Service Bus konu**: [Service Bus konu iletilerine yanıt](./functions-bindings-service-bus.md) vererek diğer Azure hizmetlerini veya şirket içi hizmetleri bağlama

## <a name="pricing"></a>İşlevlerin maliyeti ne kadardır?

Azure Işlevleri 'nin üç tür fiyatlandırma planı vardır. Gereksinimlerinize en uygun olanı seçin:

- **Tüketim planı**: Azure tüm gerekli hesaplama kaynaklarını sağlar. Kaynak yönetimi hakkında endişelenmeniz gerekmez ve yalnızca kodunuzun çalıştığı süre için ödeme yaparsınız.

- **Premium plan**: her zaman çevrimiçi olan ve hemen yanıt vermeye yönelik bir dizi önceden çarpımış örnek belirtirsiniz. İşleviniz çalıştığında Azure, gereken ek hesaplama kaynaklarını sağlar. Sürekli olarak çalışan önceden çarpımış örnekler için ödeme yaparsınız ve Azure olarak kullandığınız ek örnekler, uygulamanızı içinde ve dışarı ölçeklendirirken.

- **App Service plan**: işlevlerinizi tıpkı Web uygulamalarınız gibi çalıştırın. Diğer uygulamalarınız için App Service kullanıyorsanız, işlevleriniz hiçbir ek ücret ödemeden aynı planda çalışabilir.

Barındırma planları hakkında daha fazla bilgi için bkz. [Azure İşlevleri barındırma planı karşılaştırma](functions-scale.md). Fiyatlandırma ayrıntılarının tümü [İşlevler Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/functions/) bulunur.

## <a name="next-steps"></a>Sonraki Adımlar

- [İlk Azure İşlevinizi oluşturma](functions-create-first-function-vs-code.md)  
  [Visual Studio Code](functions-create-first-function-vs-code.md), [komut satırını](functions-create-first-azure-function-azure-cli.md)kullanmaya başlayın veya [Azure Portal](functions-create-first-azure-function.md)kullanın.

- [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
  Azure İşlevleri çalışma zamanı hakkında daha teknik bilgiler ve işlevlerin kodlanması ve tetikleyicilerin ve bağlamaların tanımlanması hakkında bir başvuru sağlar.

- [Azure İşlevlerini ölçeklendirme](functions-scale.md)  
  Tüketim barındırma planı dahil olmak üzere, Azure İşlevlerinde kullanılabilen hizmet planlarını ve doğru planın nasıl seçileceğini açıklar.

- [Azure Uygulama Hizmeti hakkında daha fazla bilgi edinin](../app-service/overview.md)  
  Azure İşlevleri; dağıtımlar, ortam değişkenleri ve tanılama gibi temel işlevler için Azure App Service’i kullanır.
