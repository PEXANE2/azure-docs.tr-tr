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
ms.openlocfilehash: e90e750bf248bdcc8e50c6ddc6e9fa0273660195
ms.sourcegitcommit: 05a650752e9346b9836fe3ba275181369bd94cf0
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79136027"
---
Azure Blob depolama, Microsoft’un buluta yönelik nesne depolama çözümüdür. Blob depolama, çok miktarda yapılandırılmamış veriyi depolamak için iyileştirilmiştir. Yapılandırılmamış veriler, metin veya ikili veriler gibi belirli bir veri modeline veya tanıma bağlı olmayan verilerdir.

## <a name="about-blob-storage"></a>Blob depolama hakkında

Blob depolama şunlar için tasarlanmıştır:

* Görüntülerin veya belgelerin doğrudan bir tarayıcıya sunulması.
* Dağıtılan erişim için dosyaların depolanması.
* Video ve ses akışları.
* Günlük dosyalarına yazma.
* Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için verilerin depolanması.
* Şirket içi veya Azure’da barındırılan bir hizmetle analiz için verilerin depolanması.

Kullanıcılar veya istemci uygulamaları HTTP/HTTPS aracılığıyla Blob depolama alanındaki nesnelere dünyanın her yerinden erişebilir. Blob depolama alanındaki nesnelere [Azure Depolama REST API’si](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), [Azure CLI](https://docs.microsoft.com/cli/azure/storage) veya bir Azure Depolama istemci kitaplığı aracılığıyla erişebilir. İstemci kitaplıkları aşağıdakiler gibi farklı diller için sunulur:

* [.NET](/dotnet/api/overview/azure/storage?view=azure-dotnet)
* [Java](https://docs.microsoft.com/java/api/overview/azure/storage)
* [Node.js](https://azure.github.io/azure-storage-node) 
* [Python](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-python)
* [Go](https://github.com/azure/azure-storage-blob-go/)
* [PHP](https://azure.github.io/azure-storage-php/)
* [Ruby](https://azure.github.io/azure-storage-ruby)

## <a name="about-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2.Nesil hakkında

Blob depolama, bulut için Microsoft'un kurumsal büyük veri analizi çözümü olan Azure Data Lake Storage 2. Nesil’i destekler. Azure Data Lake Storage 2. Nesil, hiyerarşik dosya sisteminin yanı sıra aşağıda belirtilenler de dahil olmak üzere Blob depolamanın avantajlarını sunar:

* Düşük maliyetli ve katmanlı depolama
* Yüksek kullanılabilirlik
* Güçlü tutarlılık
* Olağanüstü durum kurtarma özellikleri

Data Lake Storage 2. Nesil hakkında daha fazla bilgi almak için bkz. [Azure Data Lake Storage 2.Nesil’e giriş](../articles/storage/data-lake-storage/introduction.md).
