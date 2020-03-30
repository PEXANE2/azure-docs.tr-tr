---
title: Azure HDInsight'ta Power BI ile Etkileşimli Sorgu Kovanı verilerine bakın
description: Azure HDInsight'tan Etkileşimli Sorgu Kovanı verilerini görselleştirmek için Microsoft Power BI'yi kullanın
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/17/2019
ms.openlocfilehash: adcd4d9e81eecad9540a4ef1be5e675f940ffb8d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79368001"
---
# <a name="visualize-interactive-query-apache-hive-data-with-microsoft-power-bi-using-direct-query-in-hdinsight"></a>HDInsight'ta doğrudan sorgu yu kullanarak Microsoft Power BI ile Etkileşimli Sorgu Apache Hive verilerini görselleştirin

Bu makalede, Microsoft Power BI'nin Azure HDInsight İnteraktif Sorgu kümelerine nasıl bağlanılabildiği ve doğrudan sorguyu kullanarak Apache Hive verilerini nasıl görselleştireceğiniz açıklanmaktadır. Örnek, verileri Bir `hivesampletable` Kovan tablosundan Power BI'ye yükler. `hivesampletable` Kovan tablosu bazı cep telefonu kullanım verilerini içerir. Daha sonra kullanım verilerini bir dünya haritasında çizersiniz:

![HDInsight Power BI harita raporu](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

Power BI Desktop'daki genel ODBC konektörü aracılığıyla içe aktarma yapmak için [Apache Hive ODBC sürücüsünden](../hadoop/apache-hadoop-connect-hive-power-bi.md) yararlanabilirsiniz. Ancak Hive sorgu motorunun etkileşimli olmayan doğası göz önüne alındığında BI iş yükleri için önerilmez. [HDInsight Interactive Query konektörü](./apache-hadoop-connect-hive-power-bi-directquery.md) ve [HDInsight Apache Spark konektörü](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) performansları için daha iyi seçeneklerdir.

## <a name="prerequisites"></a>Ön koşullar
Bu makaleyi gözden geçirmeden önce aşağıdaki öğelere sahip olmalısınız:

* **HDInsight kümesi**. Küme, Apache Hive'a sahip bir HDInsight kümesi veya yeni yayımlanan Etkileşimli Sorgu kümesi olabilir. Küme oluşturmak için [bkz.](../hadoop/apache-hadoop-linux-tutorial-get-started.md)
* **[Microsoft Power BI Masaüstü](https://powerbi.microsoft.com/desktop/)**. Microsoft [İndirme Merkezi'nden](https://www.microsoft.com/download/details.aspx?id=45331)bir kopyasını indirebilirsiniz.

## <a name="load-data-from-hdinsight"></a>HDInsight'tan veri yükleme

`hivesampletable` Kovan tablosu tüm HDInsight kümeleriyle birlikte gelir.

1. Power BI Desktop'ı başlatın.

2. Menü çubuğundan Ana **Sayfa** > Veri > **Alın'a gidin...**.**Get Data**

    ![HDInsight Power BI Veri Daha Fazla Alın](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)

3. Veri **Al** penceresinden, arama kutusuna **HDInsight** girin.  

4. Arama sonuçlarından **HDInsight Interactive Query'yi**seçin ve ardından **Bağlan'ı**seçin.  **HDInsight Interactive Query'yi**görmüyorsanız, Power BI Masaüstünüzü en son sürüme güncellemeniz gerekir.

5. Üçüncü taraf hizmet iletişim kutusuna **Bağlanma'yı kapatmaya** **devam** et'i seçin.

6. **HDInsight İnteraktif Sorgu** penceresinde, aşağıdaki bilgileri girin ve sonra **Tamam'ı**seçin:

    |Özellik | Değer |
    |---|---|
    |Sunucu |Küme adını girin, örneğin *myiqcluster.azurehdinsight.net.*|
    |Database |Bu makale için **varsayılan** girin.|
    |Veri Bağlantısı modu |Bu makale için **DirectQuery'yi** seçin.|

    ![HDInsight interaktif sorgu Power BI DirectQuery bağlamak](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)

7. HTTP kimlik bilgilerini girin ve sonra **Bağlan'ı**seçin. Varsayılan kullanıcı adı **yöneticidir.**

8. Sol bölmedeki **Navigator** penceresinden **kovan örneği'ni**seçin.

9. Ana pencereden **Yükle'yi** seçin.

    ![HDInsight interaktif sorgu Güç BI kovan örnektablosu](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data-on-a-map"></a>Verileri haritada görselleştir

Son işlemden devam et.

1. Görseller bölmesinden, dünya simgesi **olan Harita'yı**seçin. Genel bir harita daha sonra ana pencerede görüntülenir.

    ![HDInsight Power BI raporu özelleştirin](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)

2. Alanlar bölmesinden **ülke** ve **aygıt make'i**seçin. Birkaç dakika sonra ana pencerede veri noktalarının yer aldığı bir dünya haritası görüntülenir.

3. Haritayı genişletin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Microsoft Power BI'yi kullanarak HDInsight'tan gelen verileri nasıl görselleştirdiğinizi öğrendiniz.  Veri görselleştirme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure HDInsight'ta ODBC kullanarak Microsoft Power BI ile Apache Hive verilerini görselleştirin.](../hadoop/apache-hadoop-connect-hive-power-bi.md) 
* [Azure HDInsight'ta Apache Hive sorgularını çalıştırmak için Apache Zeppelin'i kullanın.](../interactive-query/hdinsight-connect-hive-zeppelin.md)
* [Microsoft Hive ODBC Driver ile Excel'i HDInsight'a bağlayın.](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md)
* [Güç Sorgusu'yu kullanarak Excel'i Apache Hadoop'a bağlayın.](../hadoop/apache-hadoop-connect-excel-power-query.md)
* [Azure HDInsight'a bağlanın ve Visual Studio için Veri Gölü Araçlarını kullanarak Apache Hive sorguları çalıştırın.](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)
* [Visual Studio Kodu için Azure HDInsight Aracı'nı kullanın.](../hdinsight-for-vscode.md)
* [VERILERI HDInsight'a Yükleyin.](./../hdinsight-upload-data.md)
