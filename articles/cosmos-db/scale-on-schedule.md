---
title: Azure Işlevleri zamanlayıcısını kullanarak zamanlamaya göre Azure Cosmos DB ölçeklendirme
description: PowerShell ve Azure Işlevleri kullanarak Azure Cosmos DB aktarım hızını nasıl ölçeklendireceğinizi öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 01/13/2020
ms.author: mjbrown
ms.openlocfilehash: ec5c98d90facf9458769f235880f17d14708e425
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87923664"
---
# <a name="scale-azure-cosmos-db-throughput-by-using-azure-functions-timer-trigger"></a>Azure Işlevleri Zamanlayıcı tetikleyicisi kullanarak Azure Cosmos DB aktarım hızını ölçeklendirme

Azure Cosmos hesabının performansı, saniye başına Istek birimi (RU/s) olarak ifade edilen sağlanan aktarım hızı miktarına bağlıdır. Sağlama ikinci bir ayrıntı düzeyine sahiptir ve saat başına en yüksek RU/sn 'ye göre faturalandırılır. Bu sağlanan kapasite modeli, hizmetin öngörülebilir ve tutarlı bir işleme, garantili düşük gecikme süresi ve yüksek kullanılabilirlik sağlamasına olanak sağlar. Çoğu üretim, bu özellikleri iş yükleri. Ancak, Azure Cosmos DB yalnızca çalışma saatlerinde kullanıldığı geliştirme ve test ortamlarında, sabah aktarım hızını ve çalışma saatleri sonrasında akşam geri doğru ölçeği azaltabilirsiniz.

Aktarım hızını, çekirdek (SQL) API hesapları için [Azure Resource Manager şablonları](resource-manager-samples.md), [Azure CLI](cli-samples.md)ve [PowerShell](powershell-samples.md)aracılığıyla ya da dile özgü Azure Cosmos DB SDK 'ları kullanarak ayarlayabilirsiniz. Kaynak Yöneticisi şablonlarını kullanmanın avantajı olan Azure CLı veya PowerShell, tüm Azure Cosmos DB modeli API 'Lerini destekledikleri bir avantajdır.

## <a name="throughput-scheduler-sample-project"></a>Aktarım hızı Zamanlayıcı örnek projesi

Bir zamanlamaya göre Azure Cosmos DB ölçeklendirme sürecini basitleştirmek için [Azure Cosmos üretilen iş Zamanlayıcı](https://github.com/Azure-Samples/azure-cosmos-throughput-scheduler)adlı örnek bir proje oluşturduk. Bu proje, "ScaleUpTrigger" ve "ScaleDownTrigger" adlı iki Zamanlayıcı tetikleyicisine sahip bir Azure Işlevleri uygulamasıdır. Tetikleyiciler her bir kaynaktaki dosyada tanımlanan aktarım hızını ayarlayan bir PowerShell betiği çalıştırır `resources.json` . ScaleUpTrigger, 8: UTC 'de çalışacak şekilde yapılandırılmıştır ve ScaleDownTrigger 6 PM UTC 'de çalışacak şekilde yapılandırılmıştır ve bu süreler `function.json` Her tetikleyici için dosya içinde kolayca güncelleştirilebilen olabilir.

Bu projeyi yerel olarak kopyalayabilir, ölçek artırma ve azaltma yapılacak Azure Cosmos DB kaynakları ve çalıştırılacak zamanlamayı belirtmek için bunu değiştirebilirsiniz. Daha sonra bir Azure aboneliğine dağıtabilir ve Azure Cosmos hesaplarınızda üretilen işi ayarlamak için "Azure Cosmos DB operatörü" rolüyle birlikte yönetilen hizmet kimliğini kullanarak [Azure rol tabanlı erişim denetimi (Azure RBAC)](role-based-access-control.md) izinleriyle güvenli hale getirebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

- Daha fazla bilgi edinin ve [Azure Cosmos DB üretilen iş zamanlayıcısının](https://github.com/Azure-Samples/azure-cosmos-throughput-scheduler)örneğini indirin.
