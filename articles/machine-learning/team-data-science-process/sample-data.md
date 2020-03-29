---
title: Farklı Azure Depolama konumlarındaki örnek veriler - Ekip Veri Bilimi Süreci
description: Azure blob kapsayıcıları, SQL Server ve Hive tablolarında örnek veriler daha küçük ama temsili ve daha yönetilebilir boyuta düşürmek için.
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
ms.openlocfilehash: 22e91d50227fcb44c7b90478d76379c14161ae05
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76718612"
---
# <a name="sample-data-in-azure-blob-containers-sql-server-and-hive-tables"></a><a name="heading"></a>Azure blob kapsayıcıları, SQL Server ve Hive tablolarındaki örnek veriler

Aşağıdaki makaleler, üç farklı Azure noktasından birinde depolanan verilerin nasıl örneklenebildiğini açıklar:

* [**Azure blob kapsayıcı verileri,**](sample-data-blob.md) programlı olarak indirilerek ve ardından örnek Python koduyla örnekleyerek örneklenir.
* [**SQL Server verileri**](sample-data-sql-server.md) hem SQL hem de Python Programlama Dili kullanılarak örneklenir. 
* [**Hive tablo verileri**](sample-data-hive.md) Hive sorguları kullanılarak örneklenir.

Bu örnekleme [görevi, Ekip Veri Bilimi Süreci'nin (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bir adımıdır.

**Neden örnek veri?**

Çözümlemayı planladığınız veri kümesi büyükse, verileri daha küçük ancak temsili ve daha yönetilebilir boyuta düşürmek için verileri alttan örneklemek genellikle iyi bir fikirdir. Küçülme, veri anlaşılmasını, keşfini ve özellik mühendisliğini kolaylaştırabilir. Cortana Analytics Process'deki bu örnekleme rolü, veri işleme işlevlerinin ve makine öğrenimi modellerinin hızlı prototiplemesini sağlamaktır.

