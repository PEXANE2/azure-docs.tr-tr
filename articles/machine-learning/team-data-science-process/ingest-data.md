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
ms.openlocfilehash: f8eab59d810fb825dbebf80d01d8efd2dd0a9841
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76720546"
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Analiz için depolama ortamlarına veri yükleme

Team Data Science süreci, verilerin her aşamada en uygun şekilde belirtilmesini veya yüklenmesini gerektirir. Veri hedefleri Azure Blob depolama, SQL Azure veritabanları, Azure VM 'de SQL Server, HDInsight (Hadoop), SYNAPSE Analytics ve Azure Machine Learning içerebilir. 

Aşağıdaki makalelerde verilerin depolandığı ve işlendiği çeşitli hedef ortamlara veri alma işlemi açıklanır.

* [Azure Blob depolama alanı](move-azure-blob.md) /
* [Azure VM 'de SQL Server](move-sql-server-virtual-machine.md) için
* [Azure SQL veritabanı](move-sql-azure.md) 'na
* [Hive tablolarına](move-hive-tables.md)
* [SQL bölümlenmiş tablolara](parallel-load-sql-partitioned-tables.md)
* Şirket [içi SQL Server](move-sql-azure-adf.md)

Teknik ve iş ihtiyaçları, verilerinizin ilk konumu, biçimi ve boyutu, en iyi veri alma planını tespit eder. En iyi planda birkaç adım olması çok seyrek değildir. Bu görev dizisi, örneğin veri araştırma, ön işleme, Temizleme, alt örnekleme ve model eğitimi gibi olabilir.  Azure Data Factory, veri hareketini ve dönüştürmeyi düzenlemek için önerilen bir Azure kaynağıdır.
