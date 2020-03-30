---
title: Azure HDInsight'ta Etkileşimli Sorgu Nedir?
description: Azure HDInsight'ta Apache Hive LLAP olarak da adlandırılan Etkileşimli Sorgu'ya giriş
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.custom: hdinsightactive
ms.date: 03/03/2020
ms.openlocfilehash: e133e08e333cb478269a93cce963566e195d6949
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78271948"
---
# <a name="what-is-interactive-query-in-azure-hdinsight"></a>Azure HDInsight'ta Etkileşimli Sorgu Nedir

Etkileşimli Sorgu (Apache Hive LLAP veya [Düşük GecikmeLi Analitik İşleme](https://cwiki.apache.org/confluence/display/Hive/LLAP)olarak da adlandırılır) Bir Azure HDInsight [küme türüdür.](../hdinsight-hadoop-provision-linux-clusters.md#cluster-type) Etkileşimli Sorgu, Apache Hive sorgularını daha hızlı ve çok daha etkileşimli hale getiren bellek içi önbelleğe alma'yı destekler. Müşteriler, Azure depolama & Azure Veri Gölü Depolama'da depolanan verileri süper hızlı bir şekilde sorgulamak için Etkileşimli Sorgu'yu kullanır. Etkileşimli sorgu, geliştiricilerin ve veri bilimcinin en çok sevdikleri BI araçlarını kullanarak büyük verilerle çalışmasını kolaylaştırır. HDInsight Interactive Query, büyük verilere kolay bir şekilde erişmek için çeşitli araçları destekler.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

Etkileşimli Sorgu kümesi, Apache Hadoop kümesinden farklıdır. Sadece Hive servisini içerir.

İnteraktif Sorgu kümesindeki Hive hizmetine yalnızca Apache Ambari Hive View, Beeline ve Microsoft Hive Open Database Connectivity sürücüsü (Hive ODBC) üzerinden erişebilirsiniz. Hive konsolu, Templeton, Azure Classic CLI veya Azure PowerShell üzerinden erişemezsiniz.

## <a name="create-an-interactive-query-cluster"></a>Etkileşimli Sorgu kümesi Oluşturma

HDInsight kümesi oluşturma hakkında daha fazla bilgi için, [HDInsight'ta Apache Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)bilgisine bakın. Etkileşimli Sorgu küme türünü seçin.

> [!IMPORTANT]
> Etkileşimli Sorgu kümeleri için minimum headnode boyutu Standard_D13_v2. Daha fazla bilgi için [Azure VM Boyutlandırma Grafiği'ne](../../cloud-services/cloud-services-sizes-specs.md#dv2-series)bakın.

## <a name="execute-apache-hive-queries-from-interactive-query"></a>Etkileşimli Sorgu'dan Apache Hive sorgularını yürütme

Hive sorgularını yürütmek için aşağıdaki seçeneklere sahipsiniz:

|Yöntem |Açıklama |
|---|---|
|Microsoft Power BI|[Azure HDInsight'ta Power BI ile Etkileşimli Sorgu Apache Hive verilerini görselleştir interaktif](./apache-hadoop-connect-hive-power-bi-directquery.md)sorgu verilerini görün ve Azure [HDInsight'ta Power BI ile büyük verileri görselleştirin.](../hadoop/apache-hadoop-connect-hive-power-bi.md)|
|Visual Studio|Bkz. [Azure HDInsight'a Bağlan ve Visual Studio için Veri Gölü Araçlarını kullanarak Apache Hive sorguları çalıştırın.](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-apache-hive-queries)|
|Visual Studio Code|Bkz. [Apache Hive, LLAP veya pySpark için Görsel Stüdyo Kodunu Kullanın.](../hdinsight-for-vscode.md)|
|Apache Ambari Hive Görünümü|Bkz. [Azure HDInsight'ta Apache Hadoop ile Apache Hive Görünümü Kullanın.](../hadoop/apache-hadoop-use-hive-ambari-view.md) Kovan Görünümü HDInsight 4.0 için kullanılamaz.|
|Apaçi Beeline|Beeline [ile HDInsight Apache Hadoop ile Apache Hive kullanın](../hadoop/apache-hadoop-use-hive-beeline.md)bakın. Beeline'i baş düğümünden veya boş bir kenar düğümünden kullanabilirsiniz. Beeline'i boş bir kenar düğümünden kullanmanızı öneririz. Boş bir kenar düğümü kullanarak BIR HDInsight kümesi oluşturma hakkında bilgi [için](../hdinsight-apps-use-edge-node.md)bkz.|
|Kovan ODBC|Bkz. [Excel'i Microsoft Hive ODBC sürücüsü yle Apache Hadoop'a bağlayın.](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md)|

Java Veritabanı Bağlantısı (JDBC) bağlantı dizesini bulmak için:

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net/#/main/services/HIVE/summary`kümenizin adı nerede' `CLUSTERNAME` ye gidin.
1. URL'yi kopyalamak için pano simgesini seçin:

   ![HDInsight Hadoop İnteraktif Sorgu LLAP JDBC](./media/apache-interactive-query-get-started/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight'ta Etkileşimli Sorgu kümelerini](../hdinsight-hadoop-provision-linux-clusters.md)nasıl oluşturabilirsiniz öğrenin.
* [Azure HDInsight'ta Power BI ile büyük verileri nasıl görselleştirebilirsiniz](../hadoop/apache-hadoop-connect-hive-power-bi.md)öğrenin.
* [Azure HDInsight'ta Apache Hive sorgularını çalıştırmak için Apache Zeppelin'i](../interactive-query/hdinsight-connect-hive-zeppelin.md)nasıl kullanacağınızı öğrenin.
