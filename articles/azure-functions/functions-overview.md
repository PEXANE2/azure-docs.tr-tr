---
title: Azure İşlevlerine Genel Bakış
description: Uyumsuz iş yüklerini dakikalar içinde iyileştirmek için Azure İşlevlerinin nasıl kullanılacağını anlayın.
author: mattchenderson
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.topic: overview
ms.date: 01/16/2020
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: f0948cb58ba9403a34fbfd61ec43c29bed3440bc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77621014"
---
# <a name="an-introduction-to-azure-functions"></a>Azure İşlevleri'ne giriş

Azure İşlevleri, uygulama altyapısı konusunda endişelenmeden küçük kod parçalarını ("işlevler" olarak adlandırılır) çalıştırmanızı sağlar. Azure İşlevler ile bulut altyapısı, uygulamanızın ölçekte çalışmasını sağlamak için ihtiyacınız olan tüm güncel sunucuları sağlar.

Bir işlev, belirli bir olay türü tarafından "tetiklenir". [Desteklenen tetikleyiciler](./functions-triggers-bindings.md) arasında verilerdeki değişikliklere yanıt verme, iletilere yanıt verme, zamanlamada çalışma veya bir HTTP isteğinin sonucu olarak yer almak yer alıyor.

Her zaman sayısız hizmete karşı doğrudan kodlar ken, diğer hizmetlerle tümleştirme bağlamaları kullanılarak düzene sokulmaktadır. Bağlamalar, [çok çeşitli Azure ve üçüncü taraf hizmetlerine bildirimsel erişim](./functions-triggers-bindings.md)sunar.

## <a name="features"></a>Özellikler

Azure İşlevlerinin bazı temel özellikleri şunlardır:

- **Sunucusuz uygulamalar**: İşlevler Microsoft Azure'da [sunucusuz](https://azure.microsoft.com/solutions/serverless/) uygulamalar geliştirmenize olanak tanır.

- **Dil seçimi**: [C#, Java, JavaScript, Python ve PowerShell](supported-languages.md)seçiminizi kullanarak fonksiyonlar yazın.

- **Kullanım başına ödeme fiyatlandırma modeli**: Yalnızca kodunuzu çalıştırmak için harcanan süre için ödeme. [Fiyatlandırma bölümünde](#pricing) Tüketim barındırma planı seçeneğine bakın.  

- **Kendi bağımlılıklarınızı getirin**: Fonksiyonlar NuGet ve NPM'yi destekler ve en sevdiğiniz kitaplıklara erişmenizi sağlar.

- **Tümleşik güvenlik**: AZURE Active Directory, Facebook, Google, Twitter ve Microsoft Hesabı gibi OAuth sağlayıcılarıyla HTTP tarafından tetiklenen işlevleri koruyun.

- **Basitleştirilmiş tümleştirme**: Azure hizmetleri ve hizmet olarak yazılım (SaaS) teklifleri ile kolayca tümleştirin.

- **Esnek geliştirme**: Sürekli tümleştirme ayarlayın ve [kodunuzu GitHub,](../app-service/scripts/cli-continuous-deployment-github.md) [Azure DevOps Hizmetleri](../app-service/scripts/cli-continuous-deployment-vsts.md)ve diğer desteklenen geliştirme araçları aracılığıyla [dağıtın.](../app-service/deploy-local-git.md)

- **Stateful sunucusuz mimari**: [Dayanıklı Fonksiyonlar](durable/durable-functions-overview.md)ile sunucusuz uygulamaları düzenleyin.

- **Açık kaynak**: İşlevler çalışma zamanı açık kaynak kodludur ve [GitHub'da kullanılabilir.](https://github.com/azure/azure-webjobs-sdk-script)

## <a name="what-can-i-do-with-functions"></a>İşlevler ile ne yapabilirim?

Fonksiyonlar, toplu verilerin işlenmesi, sistemlerin entegre edilmesi, nesnelerin interneti (IoT) ile çalışmak ve basit API'ler ve mikro hizmetler oluşturmak için mükemmel bir çözümdür.

Şu gibi önemli senaryolarla başlamanızı sağlamak için bir dizi şablon kullanılabilir:

- **HTTP**: HTTP [isteklerine](functions-create-first-azure-function.md) göre kod çalıştırma

- **Zamanlayıcı**: [Önceden tanımlanmış zamanlarda çalışacak](./functions-create-scheduled-function.md) zamanlama kodu

- **Azure Cosmos DB**: [Yeni ve değiştirilmiş Azure Cosmos DB belgelerini](./functions-create-cosmos-db-triggered-function.md) işleme

- **Blob depolama**: [Yeni ve değiştirilmiş Azure Depolama lekelerini](./functions-create-storage-blob-triggered-function.md) işleme

- **Sıra depolama**: [Azure Depolama sıra iletilerine](./functions-create-storage-queue-triggered-function.md) yanıt verme

- **Olay Ağış**: [Azure Olay Ağı etkinliklerine abonelikler ve filtreler aracılığıyla](../event-grid/resize-images-on-storage-blob-upload-event.md) yanıt verme

- **Etkinlik Hub'ı**: [Yüksek hacimli Azure Event Hub etkinliklerine](./functions-bindings-event-hubs.md) yanıt verme

- **Servis Veri Servisi Kuyruğu**: Servis Veri [Gönderi sıra iletilerini yanıtlayarak](./functions-bindings-service-bus.md) diğer Azure veya şirket içi hizmetlere bağlanma

- **Hizmet Veri Servisi Konusu**: Servis Veri [Gönderi konu iletilerine yanıt](./functions-bindings-service-bus.md) vererek diğer Azure hizmetlerini veya şirket içi hizmetleri bağlama

## <a name="how-much-does-functions-cost"></a><a name="pricing"></a>İşlevlerin maliyeti ne kadardır?

Azure İşlevlerinin üç tür fiyatlandırma planı vardır. Gereksinimlerinize en uygun olanı seçin:

- **Tüketim planı**: Azure gerekli tüm hesaplama kaynaklarını sağlar. Kaynak yönetimi konusunda endişelenmenize gerek yoktur ve yalnızca kodunuzu çalıştırdığınız süre için ödeme yapmak zorunda değilsiniz.

- **Premium plan**: Her zaman çevrimiçi olan ve hemen yanıt vermeye hazır olan önceden ısıtılmış bir dizi örnek belirtirsiniz. Işleviniz çalıştığında, Azure gereken ek hesaplama kaynakları sağlar. Sürekli olarak çalışan önceden ısıtılmış örnekler ve Azure uygulamanızı girip çıkarırken kullandığınız ek örnekler için ödeme yapın.

- **Uygulama Hizmeti planı**: İşlerinizi tıpkı web uygulamalarınız gibi çalıştırın. Diğer uygulamalarınız için Uygulama Hizmeti kullanıyorsanız, işlevleriniz ek ücret ödemeden aynı planda çalıştırılabilir.

Barındırma planları hakkında daha fazla bilgi için bkz. [Azure İşlevleri barındırma planı karşılaştırma](functions-scale.md). Fiyatlandırma ayrıntılarının tümü [İşlevler Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/functions/) bulunur.

## <a name="next-steps"></a>Sonraki Adımlar

- [İlk Azure İşlevinizi oluşturma](functions-create-first-function-vs-code.md)  
  [Komut satırı](functions-create-first-azure-function-azure-cli.md)olan Visual [Studio Code](functions-create-first-function-vs-code.md)ile başlayın veya [Azure portalını](functions-create-first-azure-function.md)kullanın.

- [Azure İşlevleri geliştirici başvurusu](functions-reference.md)  
  Azure İşlevleri çalışma zamanı hakkında daha teknik bilgiler ve işlevlerin kodlanması ve tetikleyicilerin ve bağlamaların tanımlanması hakkında bir başvuru sağlar.

- [Azure İşlevlerini ölçeklendirme](functions-scale.md)  
  Tüketim barındırma planı dahil olmak üzere, Azure İşlevlerinde kullanılabilen hizmet planlarını ve doğru planın nasıl seçileceğini açıklar.

- [Azure App Service hakkında daha fazla bilgi edinin](../app-service/overview.md)  
  Azure İşlevleri; dağıtımlar, ortam değişkenleri ve tanılama gibi temel işlevler için Azure App Service’i kullanır.
