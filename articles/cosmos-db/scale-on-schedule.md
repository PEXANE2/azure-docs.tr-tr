---
title: Azure İşlevler zamanlayıcıyı kullanarak Azure Cosmos DB'yi zamanlamada ölçeklendirin
description: PowerShell ve Azure Fonksiyonlarını kullanarak Azure Cosmos DB'deki iş gücü değişikliklerini nasıl ölçeklendireceklerini öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: mjbrown
ms.openlocfilehash: 68ba40ea212c061fa5c8bbddc47ea0dfc6d8caa4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75935174"
---
# <a name="scale-azure-cosmos-db-throughput-by-using-azure-functions-timer-trigger"></a>Azure İşzamanı tetikleyicisi kullanarak Azure Cosmos DB iş buzunu ölçeklendirin

Azure Cosmos hesabının performansı, İstek Birimlerinde saniyede (RU/s) ifade edilen sağlanan iş ortası miktarına dayanır. Hüküm ikinci bir tanecikli olup, saat başına en yüksek RU/s'ye göre faturalandırılır. Bu sağlanan kapasite modeli, hizmetin öngörülebilir ve tutarlı bir iş artışı, garantili düşük gecikme sonu ve yüksek kullanılabilirlik sağlamasına olanak tanır. Çoğu üretim bu özellikleri iş yükler. Ancak, Azure Cosmos DB'nin yalnızca çalışma saatlerinde kullanıldığı geliştirme ve test ortamlarında, sabah saatlerinde iş bilgililiği büyütebilir ve çalışma saatlerinden sonra akşam ları küçültebilirsiniz.

İş ortasını, Çekirdek (SQL) API hesapları için [Azure Kaynak Yöneticisi Şablonları](resource-manager-samples.md), [Azure CLI](cli-samples.md)ve [PowerShell](powershell-samples-sql.md)aracılığıyla veya dile özel Azure Cosmos DB SDK'larını kullanarak ayarlayabilirsiniz. Kaynak Yöneticisi Şablonları, Azure CLI veya PowerShell kullanmanın yararı, tüm Azure Cosmos DB model API'lerini desteklemeleridir.

## <a name="throughput-scheduler-sample-project"></a>İş- iş tarifesi örnek proje

Azure Cosmos DB'yi bir zamanlamada ölçeklendirme işlemini basitleştirmek için [Azure Cosmos iş ortası zamanlayıcısı](https://github.com/Azure-Samples/azure-cosmos-throughput-scheduler)adı verilen örnek bir proje oluşturduk. Bu proje, "ScaleUpTrigger" ve "ScaleDownTrigger" olmak gibi iki zamanlayıcı tetikleyicisi olan bir Azure İşlevler uygulamasıdır. Tetikleyiciler, her tetikleyicideki `resources.json` dosyada tanımlandığı şekilde her kaynağın iş kaynağını ayarlayan bir PowerShell komut dosyası çalıştırın. ScaleUpTrigger 8 UTC'de çalışacak şekilde yapılandırılır ve ScaleDownTrigger 18:00 UTC'de çalışacak `function.json` şekilde yapılandırılır ve bu süreler her tetikleyici için dosya içinde kolayca güncellenebilir.

Bu projeyi yerel olarak klonlayabilir, ölçeklendirmek ve küçültmek için Azure Cosmos DB kaynaklarını ve çalışacak zamanlamayı belirtmek için değiştirebilirsiniz. Daha sonra, Azure Cosmos hesaplarınızda iş ortası ayarlamak için "Azure Cosmos DB işleci" rolüyle [Rol tabanlı Erişim Denetimi](role-based-access-control.md) (RBAC) izinleriyle azure aboneliğinde dağıtabilir ve yönetilen hizmet kimliğini kullanarak güvenli hale getirebilir.

## <a name="next-steps"></a>Sonraki Adımlar

- Daha fazla bilgi edinin ve örneği [Azure Cosmos DB iş-iş çözümleyicisinden](https://github.com/Azure-Samples/azure-cosmos-throughput-scheduler)indirin.
