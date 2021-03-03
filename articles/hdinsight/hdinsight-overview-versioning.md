---
title: Sürüm oluşturma-Azure HDInsight
description: Azure HDInsight 'ta sürüm oluşturma 'nın nasıl çalıştığını öğrenin.
ms.service: hdinsight
ms.topic: conceptual
ms.custom: deshriva
ms.date: 02/08/2021
ms.openlocfilehash: c4dddeef5daf167eeef92532b61ed986861896e8
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101746365"
---
# <a name="how-versioning-works-in-hdinsight"></a>HDInsight 'ta sürüm oluşturma nasıl kullanılır

HDInsight hizmetinde iki ana bileşen vardır: bir küme üzerinde dağıtılan bir kaynak sağlayıcısı ve Apache Hadoop bileşenleri. 

## <a name="hdinsight-resource-provider"></a>HDInsight kaynak sağlayıcısı

Azure 'daki kaynaklar bir kaynak sağlayıcısı tarafından kullanılabilir hale getirilir. HDInsight kaynak sağlayıcısı, kümeleri oluşturmaktan, yönetmekten ve silmekten sorumludur.

## <a name="hdinsight-images"></a>HDInsight görüntüleri

HDInsight, bir kümede dağıtılabilecek açık kaynaklı yazılım (OSS) bileşenlerini birlikte yerleştirmek için görüntüler kullanır. Bu görüntüler, temel Ubuntu işletim sistemi ve Spark, Hadoop, Kafka, HBase veya Hive gibi çekirdek bileşenleri içerir.

## <a name="versioning-in-hdinsight"></a>HDInsight 'ta sürüm oluşturma

Microsoft, görüntüleri ve HDInsight kaynak sağlayıcısını düzenli olarak yeni geliştirmeler ve Özellikler içerecek şekilde yükseltir.

Aşağıdakilerden biri veya daha fazlası doğru olduğunda yeni HDInsight sürümü oluşturulabilir:

- HDInsight kaynak sağlayıcısı işlevlerine yapılan önemli değişiklikler veya güncelleştirmeler
- OSS bileşenlerinin ana sürümleri
- Ubuntu işletim sisteminde yapılan büyük değişiklikler

Aşağıdakilerden biri veya daha fazlası doğru olduğunda yeni bir görüntü sürümü oluşturulur:

- OSS bileşenlerinin büyük veya küçük sürümleri ve güncelleştirmeleri
- Görüntüdeki bir bileşen için düzeltme ekleri veya düzeltmeler

## <a name="next-steps"></a>Sonraki adımlar

- [HDInsight 'ta Apache bileşenleri ve sürümleri](./hdinsight-component-versioning.md)
