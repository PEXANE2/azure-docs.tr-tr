---
title: Verileri Azure Blob depolama alanı 'na veya sunucudan taşıma-Team Data Science Işlemi
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76717579"
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>Azure Blob depolamadan veri taşıma

Ekip veri bilimi süreci, verilerin her bir aşamasında işlenmek veya analiz edilmesi için verilerin farklı depolama ortamlarına toplanmasını veya bu ortamlara yüklenmesini gerektirir.

## <a name="different-technologies-for-moving-data"></a>Veri taşımaya yönelik farklı teknolojiler

Aşağıdaki makalelerde, farklı teknolojiler kullanılarak verilerin Azure Blob depolama alanına ve içine nasıl taşınacağı açıklanır.

* [Azure depolama-gezgin](move-data-to-azure-blob-using-azure-storage-explorer.md)
* [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)
* [Python](move-data-to-azure-blob-using-python.md)
* [SSIS](move-data-to-azure-blob-using-ssis.md)

Sizin için en uygun yöntem, senaryonuza bağlıdır. Azure Machine Learning makalesinde gelişmiş analizler [Için senaryolar](plan-sample-scenarios.md) , gelişmiş analiz işleminde kullanılan çeşitli veri bilimi iş akışları için ihtiyaç duyduğunuz kaynakları belirlemenize yardımcı olur.

> [!NOTE]
> Azure Blob depolamaya yönelik kapsamlı bir giriş için [Azure Blob temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx)' ne bakın.
> 
> 

## <a name="using-azure-data-factory"></a>Azure Data Factory'yi kullanma

Alternatif olarak, [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) için kullanabilirsiniz: 

* Azure Blob depolamadan veri yükleyen bir işlem hattı oluşturma ve zamanlama 
* Bunu yayımlanmış bir Azure Machine Learning Web hizmetine geçirin, 
* tahmine dayalı analiz sonuçlarını alın ve 
* sonuçları depolamaya yükleyin. 

Daha fazla bilgi için bkz. [Azure Data Factory ve Azure Machine Learning kullanarak tahmine dayalı işlem hatları oluşturma](../../data-factory/transform-data-using-machine-learning.md).

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, bu hesap için bir Azure aboneliğiniz, bir depolama hesabınız ve karşılık gelen depolama anahtarınız olduğunu varsaymaktadır. Verileri karşıya yüklemeden/indirmeden önce Azure depolama hesabı adınızı ve hesap anahtarınızı bilmeniz gerekir.

* Bir Azure aboneliği ayarlamak için bkz. [ücretsiz bir aylık deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Depolama hesabı oluşturma ve hesap ve anahtar bilgilerini alma hakkında yönergeler için bkz. [Azure Storage hesapları hakkında](../../storage/common/storage-create-storage-account.md).

