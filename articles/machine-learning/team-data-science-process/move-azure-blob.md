---
title: Verileri Azure Blob depolamasına taşıyın - Ekip Veri Bilimi Süreci
description: Azure Depolama Gezgini, AzCopy, Python ve SSIS'i kullanarak Verileri Azure Blob depolama alanına taşıyın.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76717579"
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>Verileri Azure Blob depolama alanına ve buradan taşıma

Ekip Veri Bilimi Süreci, verilerin çeşitli depolama ortamlarına yüklenmesini veya işlenmesini ve sürecin her aşamasında en uygun şekilde işlenmesini veya analiz edilmesini gerektirir.

## <a name="different-technologies-for-moving-data"></a>Verileri taşımak için farklı teknolojiler

Aşağıdaki makaleler, farklı teknolojileri kullanarak verilerin Azure Blob depolama alanına ve Azure Blob depolamasına nasıl taşınacaklarını açıklar.

* [Azure Depolama-Explorer](move-data-to-azure-blob-using-azure-storage-explorer.md)
* [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)
* [Python](move-data-to-azure-blob-using-python.md)
* [SSIS](move-data-to-azure-blob-using-ssis.md)

Sizin için en iyi yöntem senaryonuza bağlıdır. [Azure Machine Learning makalesinde gelişmiş analitik senaryoları,](plan-sample-scenarios.md) gelişmiş analitik sürecinde kullanılan çeşitli veri bilimi iş akışları için ihtiyacınız olan kaynakları belirlemenize yardımcı olur.

> [!NOTE]
> Azure blob depolamasına tam bir giriş için [Azure Blob Temelleri'ne](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob Hizmeti'ne](https://msdn.microsoft.com/library/azure/dd179376.aspx)bakın.
> 
> 

## <a name="using-azure-data-factory"></a>Azure Data Factory'yi kullanma

Alternatif olarak, Azure [Veri Fabrikası'nı](https://azure.microsoft.com/services/data-factory/) şu şekilde kullanabilirsiniz: 

* Azure blob depolamadan veri indiren bir ardışık hatlar oluşturma ve zamanlama, 
* yayınlanan bir Azure Machine Learning web hizmetine aktarın, 
* tahmine dayalı analitik sonuçlarını almak ve 
* sonuçları depolamaya yükleyin. 

Daha fazla bilgi için bkz. Azure [Veri Fabrikası ve Azure Machine Learning'i kullanarak tahmine dayalı ardışık hatlar oluştur.](../../data-factory/transform-data-using-machine-learning.md)

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, bir Azure aboneliğiniz, depolama hesabınız ve bu hesap için karşılık gelen depolama anahtarınız olduğu varsayar. Verileri yüklemeden/indirmeden önce Azure Depolama hesap adınızı ve hesap anahtarınızı bilmeniz gerekir.

* Azure aboneliği ayarlamak için [ücretsiz bir aylık deneme sürümüne](https://azure.microsoft.com/pricing/free-trial/)bakın.
* Depolama hesabı oluşturma ve hesap ve önemli bilgileri alma yla ilgili talimatlar için Azure [Depolama hesapları hakkında'ya](../../storage/common/storage-create-storage-account.md)bakın.

