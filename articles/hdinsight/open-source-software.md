---
title: Azure HDInsight'ta açık kaynak yazılım desteği
description: Microsoft Azure, açık kaynak teknolojileri için genel bir destek düzeyi sağlar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/21/2020
ms.openlocfilehash: f93be73af4bbbd159ffc01804617892251d96347
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81772177"
---
# <a name="open-source-software-support-in-azure-hdinsight"></a>Azure HDInsight'ta açık kaynak yazılım desteği

Microsoft Azure HDInsight hizmeti, Apache Hadoop çevresinde oluşturulmuş açık kaynak teknolojilerinden oluşan bir ortam kullanır. Microsoft Azure, açık kaynak teknolojileri için genel bir destek düzeyi sağlar. Daha fazla bilgi için Azure [Destek SSS'lerinin](https://azure.microsoft.com/support/faq/) **Destek Kapsamı** bölümüne bakın. HDInsight hizmeti, yerleşik bileşenler için ek bir destek düzeyi sağlar.

## <a name="components"></a>Bileşenler

HDInsight hizmetinde iki tür açık kaynak bileşeni mevcuttur:

### <a name="built-in-components"></a>Dahili bileşenler

Bu bileşenler HDInsight kümelerine önceden yüklenir ve kümenin temel işlevselliğini sağlar. Aşağıdaki bileşenler bu kategoriye aittir:

* [Apache Hadoop İplik](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) Kaynak Yöneticisi.
* Hive sorgu dili [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).
* [Apaçi Mahout.](https://mahout.apache.org/)

Küme bileşenlerinin tam listesi [HDInsight ile kullanılabilen Apache Hadoop bileşenleri ve sürümleri nelerdir?](hdinsight-component-versioning.md)

### <a name="custom-components"></a>Özel bileşenler

Kümenin bir kullanıcısı olarak, toplulukta bulunan veya sizin oluşturduğunuz herhangi bir bileşeni iş yükler veya iş yüklersiniz.

> [!WARNING]  
> HDInsight kümesiyle sağlanan bileşenler tam olarak desteklenir. Microsoft Destek, bu bileşenlerle ilgili sorunları yalıtmaya ve çözmeye yardımcı olur.
>
> Özel bileşenler, sorunu daha fazla gidermenize yardımcı olmak için ticari olarak makul destek alır. Microsoft Destek sorunu çözebilir. Ya da bu teknoloji için derin uzmanlık bulunan açık kaynak teknolojileri için mevcut kanallar meşgul isteyebilirsiniz. Birçok topluluk sitesi kullanılabilir. Örnekler HDInsight ve [Stack Overflow](https://stackoverflow.com) [için MSDN forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight) vardır.
>
> Apaçi projeleri de [Apache web sitesinde](https://apache.org)proje siteleri var. Bir örnek [Hadoop](https://hadoop.apache.org/)olduğunu.

## <a name="component-usage"></a>Bileşen kullanımı

HDInsight hizmeti, özel bileşenleri kullanmak için çeşitli yollar sağlar. Bir bileşen kümeye nasıl kullanılır veya yüklenirse yüklensin, aynı destek düzeyi uygulanır. Aşağıdaki tablo, HDInsight kümelerinde özel bileşenlerin en yaygın şekilde kullanıldığını açıklar:

|Kullanım |Açıklama |
|---|---|
|İş teslimi|Hadoop veya özel bileşenleri çalıştıran veya kullanan diğer iş türleri kümeye gönderilebilir.|
|Küme özelleştirme|Küme oluşturma sırasında, küme düğümlerine yüklenen ek ayarlar ve özel bileşenler belirtebilirsiniz.|
|Örnekler|Popüler özel bileşenler için Microsoft ve diğerleri, bu bileşenlerin HDInsight kümelerinde nasıl kullanılabileceğinin örneklerini sağlayabilir. Bu örnekler destek siz sağlanmaktadır.|

## <a name="next-steps"></a>Sonraki adımlar

* [Komut dosyası eylemlerini kullanarak Azure HDInsight kümelerini özelleştirme](./hdinsight-hadoop-customize-cluster-linux.md)
* [HDInsight için komut dosyası eylem komut dosyaları geliştirme](hdinsight-hadoop-script-actions-linux.md)
* [Betik Eylemi kullanarak Azure HDInsight üzerinde Python ortamını güvenli bir şekilde yönetin](./spark/apache-spark-python-package-installation.md)
