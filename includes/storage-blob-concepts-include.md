---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/18/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 1950e55cf669ea7502e8523d7f8fe429c2caeb49
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69903936"
---
Azure Blob depolama, Microsoft’un buluta yönelik nesne depolama çözümüdür. Blob depolama, çok miktarda yapılandırılmamış veriyi depolamak için iyileştirilmiştir. Yapılandırılmamış veriler, metin veya ikili veriler gibi belirli bir veri modeline veya tanımına bağlı olmayan bir veri.

## <a name="about-blob-storage"></a>BLOB depolama hakkında

Blob depolama şunlar için tasarlanmıştır:

* Görüntülerin veya belgelerin doğrudan bir tarayıcıya sunulması.
* Dağıtılan erişim için dosyaların depolanması.
* Video ve ses akışları.
* Günlük dosyalarına yazma.
* Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için verilerin depolanması.
* Şirket içi veya Azure’da barındırılan bir hizmetle analiz için verilerin depolanması.

Kullanıcılar veya istemci uygulamaları, blob depolamadaki nesnelere, dünyanın herhangi bir yerinden HTTP/HTTPS aracılığıyla erişebilir. Blob depolamadaki nesnelere [Azure storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure POWERSHELL](https://docs.microsoft.com/powershell/module/azure.storage), [Azure CLI](https://docs.microsoft.com/cli/azure/storage)veya bir Azure Storage istemci kitaplığı aracılığıyla erişilebilir. İstemci kitaplıkları, [.net](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage), [Node. js](https://azure.github.io/azure-storage-node), [Python](https://docs.microsoft.com/python/azure/), [Go](https://github.com/azure/azure-storage-blob-go/), [php](https://azure.github.io/azure-storage-php/)ve [Ruby](https://azure.github.io/azure-storage-ruby)gibi çeşitli dillerde kullanılabilir.

## <a name="about-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. hakkında

Blob depolama, bulut için Microsoft'un kurumsal büyük veri analizi çözümü olan Azure Data Lake Storage 2. Nesil’i destekler. Azure Data Lake Storage 2. Nesil; düşük maliyetli, katmanlı depolama, yüksek kullanılabilirlik, güçlü tutarlılık ve olağanüstü durum kurtarma özellikleri dahil olmak üzere blob depolamanın avantajlarının yanı sıra hiyerarşik bir dosya sistemi sunar.

Data Lake Storage 2. hakkında daha fazla bilgi için bkz. [Azure Data Lake Storage 2. giriş](../articles/storage/data-lake-storage/introduction.md).
