---
title: Azure Işlevleri tüketim planı barındırma
description: Azure Işlevi tüketim planı barındırma, kodunuzu dinamik olarak ölçeklenen bir ortamda çalıştırmanızı sağlar, ancak yalnızca yürütme sırasında kullanılan kaynaklar için ödeme yaparsınız.
ms.date: 8/31/2020
ms.topic: conceptual
ms.openlocfilehash: ffb556ce48f18702e06fcdb02dda84f41ae5f906
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98684700"
---
# <a name="azure-functions-consumption-plan-hosting"></a>Azure Işlevleri tüketim planı barındırma

Tüketim planını kullanırken, Azure Işlevleri ana bilgisayarının örnekleri, gelen olayların sayısına göre dinamik olarak eklenir ve kaldırılır. Tüketim planı, Azure Işlevleri için tam <em>sunucusuz</em> barındırma seçeneğidir.

## <a name="benefits"></a>Avantajlar

Tüketim planı, yüksek yük dönemlerinde bile otomatik olarak ölçeklendirilir. İşlevleri bir tüketim planında çalıştırırken, yalnızca işlevleriniz çalışırken işlem kaynakları için ücret ödersiniz. Tüketim planında, yapılandırılabilir bir sürenin sonunda işlev yürütme zaman aşımına uğrar.

Tüketim planının diğer plan ve barındırma türlerine karşı karşılaştırması için bkz. [işlev ölçekleme ve barındırma seçenekleri](functions-scale.md).

## <a name="billing"></a>Faturalandırma

Fatura oluşturulurken yürütme sayısı, yürütme süresi ve kullanılan bellek temel alınır. Kullanım, bir işlev uygulaması içindeki tüm işlevler arasında toplanır. Daha fazla bilgi için bkz. [Azure işlevleri fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/functions/).

Tüketim planında çalışırken maliyetleri tahmin etme hakkında daha fazla bilgi edinmek için bkz. [Tüketim planı maliyetlerini anlama](functions-consumption-costs.md).

## <a name="create-a-consumption-plan-function-app"></a>Tüketim planı işlev uygulaması oluşturma

Azure portal bir işlev uygulaması oluşturduğunuzda, tüketim planı varsayılandır. İşlev uygulaması oluşturmak için API 'Leri kullanırken, öncelikle Premium ve adanmış planlarla yaptığınız gibi App Service bir plan oluşturmanız gerekmez.

Program aracılığıyla veya Azure portal bir tüketim planında bir sunucusuz işlev uygulaması oluşturmayı öğrenmek için aşağıdaki bağlantıları kullanın:

+ [Azure CLI](./scripts/functions-cli-create-serverless.md)
+ [Azure portalı](./functions-get-started.md)
+ [Azure Resource Manager şablonu](functions-create-first-function-resource-manager.md)

Ayrıca, [Visual Studio Code](./create-first-function-vs-code-csharp.md#publish-the-project-to-azure) veya [Visual Studio](functions-create-your-first-function-visual-studio.md#publish-the-project-to-azure)'dan bir işlevler projesi yayımladığınızda tüketim planında işlev uygulamaları da oluşturabilirsiniz.

## <a name="multiple-apps-in-the-same-plan"></a>Aynı planda birden çok uygulama

Aynı bölgedeki işlev uygulamaları aynı tüketim planına atanabilir. Aynı tüketim planında çalışan birden çok uygulamayı kullanmanın bir kısmı veya etkisi yoktur. Aynı tüketim planına birden fazla uygulamanın atanması, her uygulamanın esnekliği, ölçeklenebilirlik veya güvenilirliğini etkilemez.

## <a name="next-steps"></a>Sonraki adımlar

+ [Azure Işlevleri barındırma seçenekleri](functions-scale.md)
+ [Azure Işlevlerinde olay odaklı ölçeklendirme](event-driven-scaling.md)