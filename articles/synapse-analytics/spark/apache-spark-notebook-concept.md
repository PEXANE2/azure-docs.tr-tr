---
title: Azure SYNAPSE Analytics not defterlerine genel bakış
description: Bu makalede, Azure SYNAPSE Analytics Not defterleri aracılığıyla kullanılabilen olanaklarla bir genel bakış sunulmaktadır.
services: synapse-analytics
author: midesa
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 11/18/2020
ms.author: midesa
ms.reviewer: jrasnick
ms.subservice: spark
ms.openlocfilehash: bad71cfeb8f15f463bb9659be704374325d1f50a
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96462005"
---
# <a name="azure-synapse-analytics-notebooks"></a>Azure SYNAPSE Analytics Not defterleri

SYNAPSE Studio Not defteri, canlı kod, görselleştirme ve anlatım metni içeren dosyalar oluşturmanız için bir Web arabirimidir. Not defterleri, fikirlerinizi doğrulamak ve hızlı denemeleri kullanarak verilerinizden Öngörüler elde etmek için iyi bir yerdir. 

Bir Azure SYNAPSE Studio not defteriyle şunları yapabilirsiniz:

* Sıfır kurulum çabası ile çalışmaya başlayın.
* Yerleşik kurumsal güvenlik özellikleriyle verileri güvende tutun.
* Ham biçimlerdeki verileri (CSV, txt, JSON, vb.), işlenen dosya biçimlerini (Parquet, Delta Gölü, ORC, vb.) ve SQL tablolu veri dosyalarını Spark ve SQL 'de analiz edin.
* Gelişmiş yazma özellikleri ve yerleşik veri görselleştirmesi ile üretken olun.

Bu bölümde, dilleri karıştırma, veri görselleştirmeleri oluşturma, not defterlerini parametreleme, işlem hattı oluşturma ve daha birçok makale yer almaktadır. Ayrıca, Not defteri geliştirmeye nasıl başlacağınız hakkında başvurular ve öğreticiler de içerir.

## <a name="create-manage-and-use-notebooks"></a>Not defterlerini oluşturma, yönetme ve kullanma
SYNAPSE Studio Kullanıcı arabirimini kullanarak not defterlerini yönetebilirsiniz. 

Not defterlerini nasıl oluşturabileceğiniz ve yönetebileceğiniz hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
  - Not defterlerini yönetme
    - [Not defterleri oluştur](./spark/../apache-spark-development-using-notebooks.md#create-a-notebook)
    - [Not defterleri geliştirme](./spark/../apache-spark-development-using-notebooks.md#develop-notebooks)
    - [Verileri bir not defterine getirme](./spark/../apache-spark-development-using-notebooks.md#bring-data-to-a-notebook)
    - [Sihirli komutları ve geçici tabloları kullanarak birden çok dil kullanma](./spark/../apache-spark-development-using-notebooks.md#integrate-a-notebook)
    - [Cell Magic komutlarını kullanma](./spark/../apache-spark-development-using-notebooks.md#magic-commands)
  - Geliştirme
    - [Spark oturum ayarlarını yapılandırma](./spark/../apache-spark-development-using-notebooks.md#spark-session-config)
    - [Microsoft Spark yardımcı programlarını kullanma](./spark/../microsoft-spark-utilities.md)
    - [Not defterlerini ve kitaplıkları kullanarak verileri görselleştirme](./spark/../apache-spark-data-visualization.md)
    - [Bir not defterini ardışık düzen ile tümleştirme](./spark/../apache-spark-development-using-notebooks.md#integrate-a-notebook)


## <a name="next-steps"></a>Sonraki adımlar
Not defterleri, veri hazırlama, veri görselleştirme, makine öğrenimi ve diğer büyük veri senaryolarında da yaygın olarak kullanılır. Veri analiziniz ve büyük veri senaryolarınız için not defterlerini nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için lütfen aşağıdaki öğreticilere gidin:
  - [Not defteri oluşturma](./spark/../../quickstart-apache-spark-notebook.md)
  - [SYNAPSE Studio not defterlerini kullanarak görselleştirme oluşturma](./spark/../apache-spark-data-visualization-tutorial.md)
  - [Apache Spark MLlib ile makine öğrenimi modelleri oluşturun](./spark/../apache-spark-machine-learning-mllib-notebook.md)
  - [Azure oto ml ile makine öğrenimi modelleri oluşturma](./spark/../apache-spark-azure-machine-learning-tutorial.md)