---
title: Excel & Apache Hadoop Açık Veritabanı Bağlantısı (ODBC) Sürücüsü - Azure HDInsight
description: Microsoft Excel'den HDInsight kümelerinde veri sorgulamak için Excel için Microsoft Hive ODBC sürücüsünü nasıl ayarlayıp kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 04/22/2020
ms.openlocfilehash: 5aefd2c344565bf6dcb384996c42c1bc30e7291d
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82024940"
---
# <a name="connect-excel-to-apache-hadoop-in-azure-hdinsight-with-the-microsoft-hive-odbc-driver"></a>Microsoft Hive ODBC sürücüsüyle Excel'i Azure HDInsight'ta Apache Hadoop'a bağlama

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsoft'un Büyük Veri çözümü, Microsoft İş Zekası (BI) bileşenlerini HDInsight'ta dağıtılan Apache Hadoop kümeleriyle bütünleştirir. Bir örnek, Excel'i hadoop kümesinin Hive veri ambarına bağlama yeteneğidir. Microsoft Hive Open Database Connectivity (ODBC) Sürücüsünü kullanarak bağlanın.

Bir HDInsight kümesiyle ilişkili verileri Excel'den Microsoft Power Query eklentisi ile Excel'den bağlayabilirsiniz. Daha fazla bilgi için [bkz.](../hdinsight-connect-excel-power-query.md)

## <a name="prerequisites"></a>Önkoşullar

Bu makaleye başlamadan önce aşağıdaki öğelere sahip olmalısınız:

* Bir HDInsight Hadoop kümesi. Bir tane oluşturmak için Azure [HDInsight ile başlayın'](apache-hadoop-linux-tutorial-get-started.md)a bakın.
* Office 2010 Professional Plus veya daha sonra veya Daha sonra excel 2010 veya daha sonra içeren bir iş istasyonu.

## <a name="install-microsoft-hive-odbc-driver"></a>Microsoft Hive ODBC sürücüsünü yükleme

Microsoft [Hive ODBC Driver'ı](https://www.microsoft.com/download/details.aspx?id=40886)indirin ve kurun. ODBC sürücüsünü kullanacağınız uygulamanın sürümüyle eşleşen sürümü seçin.  Bu makalede, sürücü Office Excel için kullanılır.

## <a name="create-apache-hive-odbc-data-source"></a>Apache Hive ODBC veri kaynağı oluşturma

Aşağıdaki adımlar, hive ODBC Veri Kaynağı'nı nasıl oluşturabileceğinizi gösterir.

1. Windows'dan, **ODBC Veri Kaynakları > (32-bit)/(64-bit)**> Windows Yönetim Araçları > Başlat'a gidin.  Bu **eylem, ODBC Veri Kaynağı Yöneticisi** penceresini açar.

    ![OBDC veri kaynağı yöneticisi](./media/apache-hadoop-connect-excel-hive-odbc-driver/simbahiveodbc-datasourceadmin1.png "ODBC Veri Kaynağı Yöneticisi ni kullanarak Bir DSN'yi yapılandırma")

1. Kullanıcı **DSN** sekmesinden Yeni Veri **Kaynağı Oluştur** penceresini açmak için **Ekle'yi** seçin.

1. **Microsoft Hive ODBC Sürücüsü'nü**seçin ve ardından **Microsoft Hive ODBC Driver DSN Kurulum** penceresini açmak için **Finish'i** seçin.

1. Aşağıdaki değerleri yazın veya seçin:

   | Özellik | Açıklama |
   | --- | --- |
   |  Data Source Name |Veri kaynağınız için bir ad verin |
   |  Ana bilgisayar(lar) |`HDInsightClusterName.azurehdinsight.net` yazın. Örneğin, `myHDICluster.azurehdinsight.net`. Not: `HDInsightClusterName-int.azurehdinsight.net` istemci VM aynı sanal ağa bakıldığı sürece desteklenir. |
   |  Bağlantı noktası |**443** yazın. (Önceden 563 olan bu bağlantı noktası 443 olarak değiştirilmiştir.) |
   |  Veritabanı |**Varsayılan kullanın.** |
   |  Mechanism |**Windows Azure HDInsight Hizmetini** Seçin |
   |  User Name |HDInsight kümesi HTTP kullanıcı adını girin. Varsayılan kullanıcı adı **admin** şeklindedir. |
   |  Parola |HDInsight küme kullanıcı parolası girin. Parolayı **Kaydet (Şifreli)** onay kutusunu seçin.|

1. İsteğe Bağlı: **Gelişmiş Seçenekleri Seçin...**  

   | Parametre | Açıklama |
   | --- | --- |
   |  Yerel Sorgu'yı kullanma |Seçildiğinde, ODBC sürücüsü TSQL'i HiveQL'ye dönüştürmeye çalışmaz. Yalnızca saf HiveQL ekstreleri gönderdiğinden %100 eminseniz kullanırsınız. SQL Server veya Azure SQL Veritabanı'na bağlanırken, veritabanını işaretsiz bırakmalısınız. |
   |  Blok başına getirilen satırlar |Çok sayıda kayıt alırken, en iyi performansları sağlamak için bu parametrenin alımı gerekebilir. |
   |  Varsayılan dize sütun uzunluğu, İkili sütun uzunluğu, Ondalık sütun ölçeği |Veri türü uzunlukları ve hassasiyetleri verilerin döndürülme şeklini etkileyebilir. Hassaslık ve kesilme nedeniyle yanlış bilgilerin döndürülmesine neden olurlar. |

    ![Gelişmiş DSN yapılandırma seçenekleri](./media/apache-hadoop-connect-excel-hive-odbc-driver/hiveodbc-datasource-advancedoptions1.png "Gelişmiş DSN yapılandırma seçenekleri")

1. Veri kaynağını sınamak için **Test'i** seçin. Veri kaynağı doğru yapılandırıldığında, test sonucu **BAŞARı gösterir!**

1. Test penceresini kapatmak için **Tamam'ı** seçin.  

1. **Microsoft Hive ODBC Driver DSN Kurulum** penceresini kapatmak için **Tamam'ı** seçin.  

1. **ODBC Veri Kaynağı Yöneticisi** penceresini kapatmak için **Tamam'ı** seçin.  

## <a name="import-data-into-excel-from-hdinsight"></a>HDInsight’tan Excel’e veri aktarma

Aşağıdaki adımlar, önceki bölümde oluşturduğunuz ODBC veri kaynağını kullanarak hive tablosundan bir Excel çalışma kitabına veri almanın yolunu açıklar.

1. Excel’de yeni veya mevcut bir çalışma kitabını açın.

2. **Veri** sekmesinden, ODBC'den Veri Almak için**From Other Sources** > **ODBC'den** veri >  **alın'a**gidin. **From ODBC**

    ![Excel veri bağlantısı sihirbazı açma](./media/apache-hadoop-connect-excel-hive-odbc-driver/simbahiveodbc-excel-dataconnection1.png "Excel veri bağlantısı sihirbazı açma")

3. Açılan listeden, son bölümde oluşturduğunuz veri kaynağı adını seçin ve ardından **Tamam'ı**seçin.

4. İlk kullanım için bir **ODBC sürücü** iletişim kutusu açılır. Sol menüden **Windows'u** seçin. Ardından **Gezgin** penceresini açmak için **Bağlan'ı** seçin.

5. **Navigator'dan,** **KOVAN** > **varsayılan** > **kovan örnek tablosuna**gidin ve ardından **Yükle'yi**seçin. Verilerin Excel'e aktarılması birkaç dakika sürer.

    ![HDInsight Excel Hive ODBC navigasyon](./media/apache-hadoop-connect-excel-hive-odbc-driver/hdinsight-hive-odbc-navigator.png "HDInsight Excel Hive ODBC navigasyon")

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, HDInsight Hizmeti'nden Excel'e veri almak için Microsoft Hive ODBC sürücüsünü nasıl kullanacağınızı öğrendiniz. Benzer şekilde, HDInsight Hizmeti'nden SQL Veritabanına veri alabilirsiniz. Bir HDInsight Hizmetine veri yüklemek de mümkündür. Daha fazla bilgi için bkz:

* [Azure HDInsight'ta Microsoft Power BI ile Apache Hive verilerini görselleştirin.](apache-hadoop-connect-hive-power-bi.md)
* [Azure HDInsight'ta Power BI ile Etkileşimli Sorgu Kovanı verilerini görselleştirin.](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)
* [Güç Sorgusu'yu kullanarak Excel'i Apache Hadoop'a bağlayın.](apache-hadoop-connect-excel-power-query.md)
* [Azure HDInsight'a bağlanın ve Visual Studio için Veri Gölü Araçlarını kullanarak Apache Hive sorguları çalıştırın.](apache-hadoop-visual-studio-tools-get-started.md)
