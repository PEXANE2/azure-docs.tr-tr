---
title: Verileri Azure Depolama ortamlarına yükleyin - Ekip Veri Bilimi Süreci
description: Verilerin depolandığı ve işlendiği çeşitli hedef ortamlara nasıl veri sindirebileceğiniz hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720546"
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Analiz için depolama ortamlarına veri yükleme

Ekip Veri Bilimi Süreci, verilerin her aşamada en uygun şekilde yutulması veya yüklenmesini gerektirir. Veri hedefleri arasında Azure Blob Depolama, SQL Azure veritabanları, Azure VM'de SQL Server, HDInsight (Hadoop), Synapse Analytics ve Azure Machine Learning sayılabilir. 

Aşağıdaki makaleler, verilerin depolandığı ve işlendiği çeşitli hedef ortamlara verilerin nasıl depolanacağını açıklar.

* [Azure Blob](move-azure-blob.md) Depolama'ya/Gönderen
* [Azure VM'de SQL Server'a](move-sql-server-virtual-machine.md)
* [Azure SQL Veritabanına](move-sql-azure.md)
* [Kovan tablolarına](move-hive-tables.md)
* [SQL bölümlü tablolara](parallel-load-sql-partitioned-tables.md)
* [Şirket Içi SQL Server'dan](move-sql-azure-adf.md)

Teknik ve iş gereksinimlerinin yanı sıra ilk konum, biçim ve verilerinizin boyutu en iyi veri alım planını belirleyecektir. En iyi bir planın birkaç adım atması nadir değildir. Bu görev sırası, örneğin veri arama, ön işleme, temizleme, aşağı örnekleme ve model eğitimini içerebilir.  Azure Veri Fabrikası, veri hareketi ve dönüşümlerini düzenlemek için önerilen bir Azure kaynağıdır.
