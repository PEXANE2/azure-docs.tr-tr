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
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720546"
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Analiz için depolama ortamlarına veri yükleme

Team Data Science süreci, verilerin her aşamada en uygun şekilde belirtilmesini veya yüklenmesini gerektirir. Veri hedefleri Azure Blob depolama, SQL Azure veritabanları, Azure VM 'de SQL Server, HDInsight (Hadoop), SYNAPSE Analytics ve Azure Machine Learning içerebilir. 

Aşağıdaki makaleler, verileri nerede depolanan ve işlenen çeşitli hedef ortamlara verilerin alımı açıklanmaktadır.

* / Veri deposundan [Azure Blob Depolama](move-azure-blob.md)
* İçin [Azure vm'lerde SQL Server](move-sql-server-virtual-machine.md)
* [Azure SQL veritabanı](move-sql-azure.md) 'na
* İçin [Hive tabloları](move-hive-tables.md)
* İçin [SQL bölümlenmiş tabloları](parallel-load-sql-partitioned-tables.md)
* Gelen [şirket içi SQL Server](move-sql-azure-adf.md)

Teknik ve iş ihtiyaçları, verilerinizin ilk konumu, biçimi ve boyutu, en iyi veri alma planını tespit eder. En iyi planda birkaç adım olması çok seyrek değildir. Bu görev dizisi, örneğin, bir veri keşfi, ön işleme, temizleme, aşağı örnekleme ve model eğitiminin içerebilir.  Azure Data Factory, veri hareketini ve dönüştürmeyi düzenlemek için önerilen bir Azure kaynağıdır.
