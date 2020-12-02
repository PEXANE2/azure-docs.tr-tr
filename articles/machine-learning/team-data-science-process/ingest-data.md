---
title: Azure depolama ortamlarına veri yükleme-Team Data Science süreci
description: Verilerin depolandığı ve işlendiği çeşitli hedef ortamlara veri alma hakkında bilgi edinin.
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
ms.openlocfilehash: 46e911ad6bdf5a478d46e425f8700740ece56c6e
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96452663"
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Analiz için depolama ortamlarına veri yükleme

Team Data Science süreci, verilerin her aşamada en uygun şekilde belirtilmesini veya yüklenmesini gerektirir. Veri hedefleri Azure Blob depolama, SQL Azure veritabanları, Azure VM 'de SQL Server, HDInsight (Hadoop), Azure SYNAPSE Analytics ve Azure Machine Learning dahil olabilir. 

Aşağıdaki makalelerde verilerin depolandığı ve işlendiği çeşitli hedef ortamlara veri alma işlemi açıklanır.

* [Azure Blob depolama alanı](move-azure-blob.md) /
* [Azure VM 'de SQL Server](move-sql-server-virtual-machine.md) için
* [Azure SQL veritabanı](move-sql-azure.md) 'na
* [Hive tablolarına](move-hive-tables.md)
* [SQL bölümlenmiş tablolara](parallel-load-sql-partitioned-tables.md)
* Şirket [içi SQL Server](move-sql-azure-adf.md)

Teknik ve iş ihtiyaçları, verilerinizin ilk konumu, biçimi ve boyutu, en iyi veri alma planını tespit eder. En iyi planda birkaç adım olması çok seyrek değildir. Bu görev dizisi, örneğin veri araştırma, ön işleme, Temizleme, alt örnekleme ve model eğitimi gibi olabilir.  Azure Data Factory, veri hareketini ve dönüştürmeyi düzenlemek için önerilen bir Azure kaynağıdır.
