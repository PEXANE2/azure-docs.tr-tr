---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/05/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 09b120838c27f8fa6b4f7bdf8617ce806f740e3c
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96007316"
---
Azure Blob depolama, Microsoft’un buluta yönelik nesne depolama çözümüdür. Blob depolama, çok miktarda yapılandırılmamış veriyi depolamak için iyileştirilmiştir. Yapılandırılmamış veriler, metin veya ikili veriler gibi belirli bir veri modeline veya tanımına bağlı olmayan bir veri.

## <a name="about-blob-storage"></a>Blob depolama hakkında

Blob depolama şunlar için tasarlanmıştır:

* Görüntülerin veya belgelerin doğrudan bir tarayıcıya sunulması.
* Dağıtılan erişim için dosyaların depolanması.
* Video ve ses akışları.
* Günlük dosyalarına yazma.
* Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için verilerin depolanması.
* Şirket içi veya Azure’da barındırılan bir hizmetle analiz için verilerin depolanması.

Kullanıcılar veya istemci uygulamaları, blob depolamadaki nesnelere, dünyanın herhangi bir yerinden HTTP/HTTPS aracılığıyla erişebilir. Blob depolamadaki nesnelere [Azure storage REST API](/rest/api/storageservices/blob-service-rest-api), [Azure POWERSHELL](/powershell/module/az.storage), [Azure CLI](/cli/azure/storage)veya bir Azure Storage istemci kitaplığı aracılığıyla erişilebilir. İstemci kitaplıkları aşağıdakiler de dahil olmak üzere farklı diller için kullanılabilir:

* [.NET](/dotnet/api/overview/azure/storage?view=azure-dotnet)
* [Java](/java/api/overview/azure/storage)
* [Node.js](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage)
* [Python](../articles/storage/blobs/storage-quickstart-blobs-python.md)
* [Git](https://github.com/azure/azure-storage-blob-go/)
* [PHP](https://azure.github.io/azure-storage-php/)
* [Ruby](https://azure.github.io/azure-storage-ruby)

## <a name="about-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. hakkında

Blob depolama, bulut için Microsoft'un kurumsal büyük veri analizi çözümü olan Azure Data Lake Storage 2. Nesil’i destekler. Azure Data Lake Storage 2. hiyerarşik bir dosya sistemi ve aşağıdakiler dahil olmak üzere blob depolamanın avantajlarını sunmaktadır:

* Düşük maliyetli, katmanlı depolama
* Yüksek kullanılabilirlik
* Güçlü tutarlılık
* Olağanüstü durum kurtarma özellikleri

Data Lake Storage 2. hakkında daha fazla bilgi için bkz. [Azure Data Lake Storage 2. giriş](../articles/storage/blobs/data-lake-storage-introduction.md).