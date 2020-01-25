---
title: Verileri Azure Blob depolamadan - Team Data Science Process taşıma
description: Azure Depolama Gezgini, AzCopy, Python ve SSIS kullanarak Azure Blob Storage 'a ve bu depolama alanından veri taşıma.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: fc58651bcb3b266b981fb953fd7341427d47fb2c
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76717579"
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>İçin ve Azure Blob depolamadan/depolamaya veri taşıma

Team Data Science Process veri veya alınan farklı depolama ortamları işlenen ya da işlemin her aşamasında en uygun şekilde analiz için çeşitli yüklenmiş olmasını gerektirir.

## <a name="different-technologies-for-moving-data"></a>Verileri taşımak için farklı teknolojiler

Aşağıdaki makaleleri ve farklı teknolojileri kullanarak Azure Blob depolama alanından veri taşıma konusunda açıklanmaktadır.

* [Azure Depolama Gezgini](move-data-to-azure-blob-using-azure-storage-explorer.md)
* [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)
* [Python](move-data-to-azure-blob-using-python.md)
* [SSIS](move-data-to-azure-blob-using-ssis.md)

Hangi sizin için en iyi bir yöntemdir, senaryoya bağlıdır. [Azure Machine learning'de Gelişmiş analiz senaryoları](plan-sample-scenarios.md) makale Gelişmiş analiz işleminde kullanılan veri bilimi iş akışları çeşitli için ihtiyacınız olan kaynakları belirlemenize yardımcı olur.

> [!NOTE]
> Azure blob depolama için bir tam giriş için başvurmak [Azure Blob Temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="using-azure-data-factory"></a>Azure Data Factory'yi kullanma

Alternatif olarak, kullandığınız [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) için: 

* oluşturun ve Azure blob depolamadan veri yükleyen işlem hattı zamanlama 
* yayımlanan bir Azure Machine Learning web hizmeti ile geçirin, 
* Tahmine dayalı analiz sonuçlarını almak ve 
* Sonuçları depolama alanına yükleyin. 

Daha fazla bilgi için [Azure Data Factory ve Azure Machine Learning kullanarak öngörülebilir komut zincirleri oluşturma](../../data-factory/transform-data-using-machine-learning.md).

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, bir Azure aboneliği, bir depolama hesabı ve karşılık gelen depolama anahtarını ilgili hesabın sahibi olduğunuzu varsayar. Verileri karşıya yüklemeden/indirmeden önce Azure depolama hesabı adınızı ve hesap anahtarınızı bilmeniz gerekir.

* Bir Azure aboneliğini ayarlama hakkında bilgi için bkz: [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Depolama hesabı oluşturma ve hesap ve anahtar bilgilerini alma hakkında yönergeler için bkz. [Azure Storage hesapları hakkında](../../storage/common/storage-create-storage-account.md).

