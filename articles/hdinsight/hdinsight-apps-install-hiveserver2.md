---
title: Azure HDInsight üzerinde HiveServer2 ölçeklendirin
description: Hata toleransını ve kullanılabilirliği artırmak için kenar düğümlerini kullanarak Azure HDInsight kümelerinde yatay olarak HiveServer2 ölçeklendirin.
services: hdinsight
ms.service: hdinsight
ms.topic: conceptual
ms.reviewer: hrasheed-msft
ms.author: kecheung
author: kcheeeung
ms.date: 08/12/2020
ms.openlocfilehash: d0a0df4791492c1c9f0d600630d723024c46c1b8
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88227523"
---
# <a name="scale-hiveserver2-on-azure-hdinsight-clusters-for-high-availability"></a>Yüksek kullanılabilirlik için Azure HDInsight kümelerinde HiveServer2 ölçeklendirin

Kullanılabilirliği ve yük dağılımını artırmak için kümenize ek bir HiveServer2 dağıtmayı öğrenin. Yayın düğümü boyutunu arttırdığınızda, HiveServer2 dağıtmak için kenar düğümlerini de kullanabilirsiniz. 

> [!NOTE]
> Kullanımınıza bağlı olarak, HiveServer2 sayısının artırılması Hive meta veri deposu bağlantı sayısını artırabilir. Ayrıca, Azure SQL veritabanınızın düzgün boyutlandırıldığından emin olun.

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu kullanmak için aşağıdaki makaleyi anlamanız gerekir:
- [HDInsight 'ta Apache Hadoop kümelerinde boş kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md)

## <a name="install-hiveserver2"></a>HiveServer2 'i yükler

Bu bölümde, hedef konaklarınıza ek bir HiveServer2 yüklersiniz.

1. Tarayıcınızda ambarı açın ve hedef ana bilgisayarınıza tıklayın.

:::image type="content" source="media/hdinsight-apps-install-hiveserver2/hdinsight-install-hiveserver2-a.png" alt-text="Ambarı ana bilgisayarları menüsü.":::

2. Ekle düğmesine tıklayın ve HiveServer2 ' ye tıklayın.

:::image type="content" source="media/hdinsight-apps-install-hiveserver2/hdinsight-install-hiveserver2-b.png" alt-text="Konağın HiveServer2 bölmesini ekleyin.":::

3. Onayla ve işlemin çalıştırılacağı işlem. İstenen tüm konaklar için 1-3 tekrarlayın.

4. Yüklemeyi bitirdikten sonra, eski yapılandırmalarını ile tüm hizmetleri yeniden başlatın ve HiveServer2 'i başlatın.

:::image type="content" source="media/hdinsight-apps-install-hiveserver2/hdinsight-install-hiveserver2-c.png" alt-text="HiveServer2 panelini başlatın.":::

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, HiveServer2 'in kümenize nasıl yükleneceğini öğrendiniz. Kenar düğümleri ve uygulamalar hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Edge düğümünü yükler](hdinsight-apps-use-edge-node.md): HDInsight kümenize bir Edge düğümü yüklemeyi öğrenin.
* [HDInsight uygulamaları yükleme](hdinsight-apps-install-applications.md): HDInsight uygulamalarını kümelerinize yükleme hakkında bilgi alın.
* [Azure SQL DTU bağlantı sınırları](../azure-sql/database/resource-limits-dtu-single-databases.md): DTU kullanarak Azure SQL veritabanı limitleri hakkında bilgi edinin.
* [Azure SQL sanal çekirdek bağlantı sınırları](../azure-sql/database/resource-limits-vcore-elastic-pools.md): vçekirdekleri kullanarak Azure SQL veritabanı sınırlarını öğrenin.
