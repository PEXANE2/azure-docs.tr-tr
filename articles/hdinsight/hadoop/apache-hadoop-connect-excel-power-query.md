---
title: Güç Sorgusu yla Excel'i Apache Hadoop'a bağlayın - Azure HDInsight
description: HDInsight'ta Hadoop'ta depolanan verilere erişmek için iş zekası bileşenlerinden nasıl yararlanarak Excel için Güç Sorgusu'ndan nasıl yararlanışları nızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/17/2019
ms.openlocfilehash: e643c7fe7b18eed30843e7cab3977036435d2112
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75435795"
---
# <a name="connect-excel-to-apache-hadoop-by-using-power-query"></a>Güç Sorgusu'yu kullanarak Excel'i Apache Hadoop'a bağlayın

Microsoft büyük veri çözümünün en önemli özelliklerinden biri, Microsoft iş zekası (BI) bileşenlerinin Azure HDInsight'taki Apache Hadoop kümeleriyle bütünleştirilmesidir. Birincil örnek, Excel için Microsoft Power Query for Excel eklentisini kullanarak Hadoop kümenizle ilişkili verileri içeren Azure Depolama hesabına Excel'i bağlama yeteneğidir. Bu makale, HDInsight ile yönetilen bir Hadoop kümesiyle ilişkili verileri sorgulamak için Power Query'yi nasıl ayarlayıp kullanacağınızı size iletebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir Apache Hadoop kümesi. [Linux'ta HDInsight ile başlayın](./apache-hadoop-linux-tutorial-get-started.md)bakın.
* Windows 10, 7, Windows Server 2008 R2 veya daha sonraki bir işletim sistemini çalıştıran bir iş istasyonu.
* Office 2016, Office 2013 Professional Plus, Office 365 ProPlus, Excel 2013 Bağımsız veya Office 2010 Professional Plus.

## <a name="install-microsoft-power-query"></a>Microsoft Power Sorgusunu Yükleme

Power Query çıktı sıveya HDInsight kümesi üzerinde çalışan bir Hadoop işi tarafından oluşturulan verileri içe aktarabilir.

Excel 2016'da Güç Sorgusu, Dönüşüm& al bölümü altındaki Veri şeridine entegre edilmiştir. Eski Excel sürümleri için, [Microsoft İndirme Merkezi'nden](https://go.microsoft.com/fwlink/?LinkID=286689) Excel için Microsoft Güç Sorgusu'nu indirin ve yükleyin.

## <a name="import-hdinsight-data-into-excel"></a>HDInsight verilerini Excel'e aktarma

Excel için Güç Sorgusu eklentisi, POWERPivot ve Power Map gibi BI araçlarının verileri incelemek, analiz etmek ve sunmak için kullanılabildiği HDInsight kümenizden verileri Excel'e aktarmayı kolaylaştırır.

1. Excel'i başlatın.

1. Yeni bir boş çalışma kitabı oluşturun.

1. Excel sürümünü temel alan aşağıdaki adımları gerçekleştirin:

   * Excel 2016

     *  > **Azure HDInsight'tan (HDFS)**> **Veri** > **Get Data** > **From Azure**Alın'ı seçin.

       ![Hdı. PowerQuery.SelectHdiSource.2016](./media/apache-hadoop-connect-excel-power-query/powerquery-selecthdisource-excel2016.png)

   * Excel 2013/2010

     * **Microsoft Azure HDInsight'tan****Azure'dan** >  **Güç Sorgusu'nı** > seçin.

       ![Hdı. PowerQuery.SelectHdiSource](./media/apache-hadoop-connect-excel-power-query/powerquery-selecthdisource.png)

       **Not:** **Güç Sorgusu** menüsünü görmüyorsanız, **Dosya** > **Seçenekleri** > **Eklentileri'ne**gidin ve sayfanın altındaki açılır yönetme **kutusundan** **COM Eklentileri'ni** seçin. **Git...** düğmesini seçin ve Excel eklentisi için Güç Sorgusu kutusunun işaretlendiğini doğrulayın.

       **Not:** Power Query ayrıca **Diğer Kaynaklardan**seçerek HDFS'den veri almanızı sağlar.

1. Azure **HDInsight (HDFS)** iletişim kutusundan, **Hesap adı veya URL** metin kutusuna kümenizle ilişkili Azure Blob depolama hesabının adını girin. Sonra **Tamam**’ı seçin. Bu hesap varsayılan depolama hesabı veya bağlantılı bir depolama hesabı olabilir.  Biçim şöyledir: `https://StorageAccountName.blob.core.windows.net/`.

1. **Hesap Tuşu**için Blob depolama hesabının anahtarını girin ve ardından **Bağlan'ı**seçin. (Bu mağazaya ilk kez erişdiğinizde hesap bilgilerini girmeniz gerekir.)

1. Sorgu **Düzenleyicisi'nin** solundaki Gezgin bölmesinde, kümenizle ilişkili Blob depolama kapsayıcısı adını çift tıklatın. Varsayılan olarak, kapsayıcı adı küme adı ile aynı adıdır.

1. **Ad** sütununda **HiveSampleData.txt** bulun (klasör yolu **.. /kovan/depo/kovan örnekleme/**) ve ardından HiveSampleData.txt'nin solundaki **İkili'yi** seçin. HiveSampleData.txt tüm küme ile birlikte gelir. İsteğe bağlı olarak, kendi dosyanızı kullanabilirsiniz.

    ![HDI Excel güç sorgusu alma verileri](./media/apache-hadoop-connect-excel-power-query/powerquery-importdata.png)

1. İsterseniz, sütun adlarını yeniden adlandırabilirsiniz. Hazır olduğunuzda, & **Yükle'yi Kapat'ı**seçin.  Veriler çalışma kitabınıza yüklendi:

    ![HDI Excel güç sorgusu içe aktarılan tablo](./media/apache-hadoop-connect-excel-power-query/powerquery-importedtable.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, HDInsight'tan Excel'e veri almak için Güç Sorgusu'nun nasıl kullanılacağını öğrendiniz. Benzer şekilde, HDInsight'tan Azure SQL Veritabanı'na veri alabilirsiniz. HDInsight'a veri yüklemek de mümkündür. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure HDInsight'ta Microsoft Power BI ile Apache Hive verilerini görselleştirin.](apache-hadoop-connect-hive-power-bi.md)
* [Azure HDInsight'ta Power BI ile Etkileşimli Sorgu Kovanı verilerini görselleştirin.](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)
* [Azure HDInsight'ta Apache Hive sorgularını çalıştırmak için Apache Zeppelin'i kullanın.](../interactive-query/hdinsight-connect-hive-zeppelin.md)
* [Microsoft Hive ODBC Driver ile Excel'i HDInsight'a bağlayın.](apache-hadoop-connect-excel-hive-odbc-driver.md)
* [Azure HDInsight'a bağlanın ve Visual Studio için Veri Gölü Araçlarını kullanarak Apache Hive sorguları çalıştırın.](apache-hadoop-visual-studio-tools-get-started.md)
* [Visual Studio Kodu için Azure HDInsight Aracı'nı kullanın.](../hdinsight-for-vscode.md)
* [HDInsight'a veri yükleyin.](./../hdinsight-upload-data.md)
