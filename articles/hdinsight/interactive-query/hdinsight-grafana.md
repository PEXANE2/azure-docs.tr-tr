---
title: Azure HDInsight'ta Grafana'yı kullanma
description: Azure HDInsight'ta Apache Hadoop kümeleriyle Grafana panosuna nasıl erişeceğinizi öğrenin
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/27/2019
ms.openlocfilehash: cd515bfd1dc57e78a041ed96686e1ba692bf6d3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79082872"
---
# <a name="access-grafana-in-azure-hdinsight"></a>Azure HDInsight'ta Grafana'ya erişme

[Grafana](https://grafana.com/) popüler, açık kaynak grafik ve pano oluşturucu. Grafana zengin özelliği; yalnızca kullanıcıların özelleştirilebilir ve paylaşılabilir panolar oluşturmasına izin vermekle kalmıyor, aynı zamanda şablonlu/komut dosyası lı panolar, LDAP entegrasyonu, birden çok veri kaynağı ve daha fazlasını da sunuyor.

Şu anda Azure HDInsight'ta Grafana, Spark, HBase, Kafka ve Etkileşimli Sorgu küme türleri ile desteklenir. Kurumsal Güvenlik Paketi etkinleştirilmiş kümeler için desteklenmez.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-an-apache-hadoop-cluster"></a>Apache Hadoop kümesi oluşturma

Bkz. [Azure portalını kullanarak Apache Hadoop kümeleri oluşturun.](../hdinsight-hadoop-create-linux-clusters-portal.md) **Küme türü için,** **Spark**, **Kafka**, **HBase**veya **İnteraktif Sorgu'u**seçin.

## <a name="access-the-grafana-dashboard"></a>Grafana panosuna erişin

1. Bir web tarayıcısından `https://CLUSTERNAME.azurehdinsight.net/grafana/` CLUSTERNAME'nin kümenizin adı olduğu yere gidin.

1. Hadoop küme kullanıcı kimlik bilgilerini girin.

1. Grafana panosu görüntülenir ve aşağıdaki örneğe benzer:

    ![HDInsight Grafana web panosu](./media/hdinsight-grafana/hdinsight-grafana-dashboard.png "HDInsight Grafana panosu")

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, oluşturduğunuz kümeyi aşağıdaki adımlarla silin:

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Üstteki **Arama** kutusuna **HDInsight**yazın.

1. **Hizmetler**altında **HDInsight kümelerini** seçin.

1. Görünen HDInsight kümeleri listesinde, oluşturduğunuz kümenin yanındaki **...** kümesini seçin.

1. **Sil**’i seçin. **Evet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight ile veri çözümleme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight ile Apache Hive kullanın.](../hadoop/hdinsight-use-hive.md)

* [HDInsight ile MapReduce'ı kullanın.](../hadoop/hdinsight-use-mapreduce.md)

* [HDInsight için Visual Studio Hadoop araçlarını kullanmaya başlayın.](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)
