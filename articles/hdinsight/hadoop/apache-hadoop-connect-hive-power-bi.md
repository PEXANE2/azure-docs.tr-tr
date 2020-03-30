---
title: Power BI ile Apache Hive verilerini görselleştirin - Azure HDInsight
description: Azure HDInsight tarafından işlenen Hive verilerini görselleştirmek için Microsoft Power BI'yi nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/02/2020
ms.openlocfilehash: d9b64785dbd82842479eb3f313b8394f9f25b40b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79370007"
---
# <a name="visualize-apache-hive-data-with-microsoft-power-bi-using-odbc-in-azure-hdinsight"></a>Azure HDInsight’ta ODBC kullanarak Microsoft Power BI ile Apache Hive verilerini görselleştirme

ODBC'yi kullanarak Microsoft Power BI Desktop'ı Azure HDInsight'a nasıl bağlayabilirsiniz ve Apache Hive verilerini nasıl görselleştireceğinizi öğrenin.

> [!IMPORTANT]
> Power BI Desktop'daki genel ODBC konektörü aracılığıyla içe aktarma yapmak için Hive ODBC sürücüsünden yararlanabilirsiniz. Ancak Hive sorgu motorunun etkileşimli olmayan doğası göz önüne alındığında BI iş yükleri için önerilmez. [HDInsight Interactive Query konektörü](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md) ve [HDInsight Spark konektörü](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) performansları için daha iyi seçeneklerdir.

Bu makalede, verileri bir `hivesampletable` Kovan tablosundan Power BI'ye yüklersiniz. Kovan tablosu bazı cep telefonu kullanım verilerini içerir. Daha sonra kullanım verilerini bir dünya haritasında çizersiniz:

![HDInsight Power BI harita raporu](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

Bilgiler, yeni [Etkileşimli Sorgu](../interactive-query/apache-interactive-query-get-started.md) küme türü için de geçerlidir. Doğrudan sorgu kullanarak HDInsight İnteraktif Sorgu'ya nasıl bağlanınız? [Visualize Interactive Query Hive data with Microsoft Power BI using direct query in Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi gözden geçirmeden önce aşağıdaki öğelere sahip olmalısınız:

* HDInsight kümesi. Küme, Hive ile hdinsight kümesi veya yeni yayımlanan Etkileşimli Sorgu kümesi olabilir. Küme oluşturmak için [bkz.](apache-hadoop-linux-tutorial-get-started.md)

* [Microsoft Power BI Masaüstü](https://powerbi.microsoft.com/desktop/). Microsoft [İndirme Merkezi'nden](https://www.microsoft.com/download/details.aspx?id=45331)bir kopyasını indirebilirsiniz.

## <a name="create-hive-odbc-data-source"></a>Hive ODBC veri kaynağı oluşturma

Bkz. [Kovan Oluştur ODBC veri kaynağı.](apache-hadoop-connect-excel-hive-odbc-driver.md#create-apache-hive-odbc-data-source)

## <a name="load-data-from-hdinsight"></a>HDInsight'tan veri yükleme

**Kovan örneği hive** tablosu tüm HDInsight kümeleri ile birlikte gelir.

1. Power BI Desktop'ı başlatın.

1. Üst menüden Ana **Sayfa** > Veri > **Alın'a gidin...****Get Data**

    ![HDInsight Excel Power BI açık veri](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-open-odbc.png)

1. Veri **Al** iletişim kutusundan soldan **Diğer'i** seçin, sağdan **ODBC'yi** seçin ve sonra alttan **Bağlan'ı** seçin.

1. **ODBC** iletişim kutusundan, açılan listeden son bölümde oluşturduğunuz veri kaynağı adını seçin. Sonra **Tamam**’ı seçin.

1. İlk kullanım için bir **ODBC sürücü** iletişim kutusu açılır. Sol menüden **Varsayılan veya Özel'i** seçin. Ardından **Navigator'ı**açmak için **Bağlan'ı** seçin.

1. **Navigator** iletişim kutusundan, **ODBC > HIVE > varsayılan,** **kovan örnektable**seçin ve sonra **Yük'ünü**seçin.

## <a name="visualize-data"></a>Verileri görselleştirme

Son işlemden devam et.

1. Visualizations bölmesinden **Harita'yı**seçin, bu bir küre simgesidir.

    ![HDInsight Power BI raporu özelleştirin](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)

1. **Alanlar** bölmesinden **ülke** ve **aygıt make'i**seçin. Haritada çizilen verileri görebilirsiniz.

1. Haritayı genişletin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Power BI kullanarak HDInsight verileri görselleştirmek için nasıl öğrendim.  Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure HDInsight'ta Apache Hive sorgularını çalıştırmak için Apache Zeppelin'i kullanın.](../interactive-query/hdinsight-connect-hive-zeppelin.md)
* [Microsoft Hive ODBC Driver ile Excel'i HDInsight'a bağlayın.](./apache-hadoop-connect-excel-hive-odbc-driver.md)
* [Güç Sorgusu'yu kullanarak Excel'i Apache Hadoop'a bağlayın.](apache-hadoop-connect-excel-power-query.md)
* [Azure HDInsight'a bağlanın ve Visual Studio için Veri Gölü Araçlarını kullanarak Apache Hive sorguları çalıştırın.](apache-hadoop-visual-studio-tools-get-started.md)
* [Visual Studio Kodu için Azure HDInsight Aracı'nı kullanın.](../hdinsight-for-vscode.md)
* [VERILERI HDInsight'a Yükleyin.](./../hdinsight-upload-data.md)
