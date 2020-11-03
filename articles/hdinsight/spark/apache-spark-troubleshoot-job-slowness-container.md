---
title: Azure HDInsight depolamada birçok dosya olduğunda yavaş Apache Spark
description: Azure depolama kapsayıcısı Azure HDInsight 'ta çok sayıda dosya içerdiğinde Apache Spark iş yavaş çalışır
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/21/2019
ms.openlocfilehash: f14e9bfad959d7f40b2d364c702983c31e0e3cef
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93289757"
---
# <a name="apache-spark-job-run-slowly-when-the-azure-storage-container-contains-many-files-in-azure-hdinsight"></a>Azure HDInsight’ta Azure depolama kapsayıcısı birçok dosya içerdiğinde Apache Spark işleri yavaş çalışıyor

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

HDInsight kümesi çalıştırılırken, çok sayıda dosya/alt klasör olduğunda Azure Storage kapsayıcısına yazan Apache Spark işi yavaş olur. Örneğin, yeni bir kapsayıcıya yazarken 20 saniye sürer, ancak alanlarındaki 200.000 dosyaları olan bir kapsayıcıya yazarken yaklaşık 2 dakika.

## <a name="cause"></a>Nedeni

Bu bilinen bir Spark sorunudur. Yavaşlığın, `ListBlob` `GetBlobProperties` Spark iş yürütmesi sırasında ve işlemlerinden gelir.

Bölümleri izlemek için Spark 'ın `FileStatusCache` Dizin yapısı hakkında bilgi içeren bir bakımını vardır. Spark, bu önbelleği kullanarak yolları ayrıştırarak kullanılabilir bölümlerin farkında olabilir. Bölümleri izlemenin avantajı, Spark 'ın yalnızca verileri okurken gerekli dosyalara dokunmasına yönelik bir avantajdır. Bu bilgileri güncel tutmak için, yeni veriler yazdığınızda Spark 'ın dizin altındaki tüm dosyaları Listeve bu önbelleği güncelleştirmesi gerekir.

Spark 2,1 ' de, her yazma işleminden sonra önbelleği güncelleştirmemiz gerekirken, Spark var olan bir bölüm sütununun geçerli yazma isteğinde önerilen ile eşleşip eşleşmediğini kontrol eder, bu nedenle, her yazma işlemi sırasında listeleme işlemlerine da yol açabilir.

Spark 2,2 ' de, ekleme moduyla veri yazarken bu performans sorunu düzeltilmelidir.

## <a name="resolution"></a>Çözüm

Bölümlenmiş bir veri kümesi oluşturduğunuzda, Spark 'ın güncelleştirmesini listelemesini sağlayacak dosya sayısını sınırlayan bir bölümleme şeması kullanılması önemlidir `FileStatusCache` .

Her nth Micro Batch için N %100 = = 0 (100 yalnızca bir örnektir), var olan verileri Spark tarafından yüklenebilen başka bir dizine taşıyın.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]