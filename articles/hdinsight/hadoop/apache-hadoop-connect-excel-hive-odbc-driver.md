---
title: Excel 'i Hive ODBC sürücüsü ile Apache Hadoop bağlama-Azure HDInsight
description: Microsoft Excel 'de HDInsight kümelerindeki verileri sorgulamak için Excel için Microsoft Hive ODBC sürücüsünü ayarlamayı ve kullanmayı öğrenin.
keywords: Hadoop Excel, Hive Excel, Hive ODBC
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: hrasheed
ms.openlocfilehash: b1231a406ac20512e9ce92032379d697c3346af3
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70882755"
---
# <a name="connect-excel-to-apache-hadoop-in-azure-hdinsight-with-the-microsoft-hive-odbc-driver"></a>Microsoft Hive ODBC sürücüsü ile Azure HDInsight 'ta Apache Hadoop Excel 'e bağlanma

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsoft 'un büyük veri çözümü, Microsoft Business Intelligence (BI) bileşenlerini Azure HDInsight 'ta dağıtılan Apache Hadoop kümeleriyle tümleştirir. Bu tümleştirmenin bir örneği, Microsoft Hive açık veritabanı bağlantısı (ODBC) sürücüsünü kullanarak HDInsight 'taki Hadoop kümesinin Hive veri ambarına Excel bağlama olanağıdır.

Excel için Microsoft Power Query eklentisi kullanılarak, HDInsight kümesiyle ilişkili verilerin yanı sıra diğer (HDInsight olmayan) Hadoop kümeleri de dahil diğer veri kaynaklarıyla de Excel 'den bağlanabilirsiniz. Power Query yükleme ve kullanma hakkında bilgi için bkz. [Power Query kullanarak Excel 'ı HDInsight 'A bağlama](../hdinsight-connect-excel-power-query.md).

## <a name="prerequisites"></a>Önkoşullar

Bu makaleye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* Hadoop kümesi An HDInsight. Bir tane oluşturmak için bkz. [Azure HDInsight kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md).
* Office 2010 Professional Plus veya üzeri ya da Excel 2010 veya üzeri bir iş istasyonu.

## <a name="install-microsoft-hive-odbc-driver"></a>Microsoft Hive ODBC sürücüsünü yükler
ODBC sürücüsü kullanacağınız uygulamanın sürümüyle eşleşen [Microsoft Hive ODBC sürücüsü](https://go.microsoft.com/fwlink/?LinkID=286698) sürümünü indirip yükleyin.  Bu makalede, sürücü Office Excel için kullanılır.

## <a name="create-apache-hive-odbc-data-source"></a>Apache Hive ODBC veri kaynağı oluşturma
Aşağıdaki adımlarda, Hive ODBC veri kaynağı oluşturma yöntemi gösterilmektedir.

1. Windows 'da, Windows Yönetim Araçları > ODBC veri kaynakları (32 bit)/(64 bit) > Başlat ' a gidin.  Bu, **ODBC veri kaynağı Yöneticisi** penceresini açar.

    ![OBDC veri kaynağı Yöneticisi](./media/apache-hadoop-connect-excel-hive-odbc-driver/simbahiveodbc-datasourceadmin1.png "ODBC veri kaynağı Yöneticisi 'ni kullanarak BIR DSN yapılandırma")

1. **Yeni veri kaynağı oluştur** penceresini açmak IÇIN **Kullanıcı DSN** sekmesinden **Ekle** ' yi seçin.

1. **Microsoft Hive ODBC sürücüsü**' yi seçin ve ardından **son** ' u seçerek **Microsoft Hive ODBC sürücüsü DSN kurulum** penceresini açın.

1. Aşağıdaki değerleri yazın veya seçin:

   | Özellik | Açıklama |
   | --- | --- |
   |  Data Source Name |Veri kaynağınız için bir ad verin |
   |  Ana bilgisayar (ler) |`HDInsightClusterName.azurehdinsight.net` yazın. Örneğin, `myHDICluster.azurehdinsight.net` |
   |  Port |**443** yazın. (Önceden 563 olan bu bağlantı noktası 443 olarak değiştirilmiştir.) |
   |  Database |**Varsayılanı**kullanın. |
   |  Mekanizma |**Windows Azure HDInsight hizmetini** seçin |
   |  Kullanıcı adı |HDInsight kümesi HTTP Kullanıcı Kullanıcı adı girin. Varsayılan kullanıcı adı **admin** şeklindedir. |
   |  istemcisiyle yönetilen bir cihaz için) |HDInsight kümesi Kullanıcı parolasını girin. **Parolayı Kaydet (şifreli)** onay kutusunu seçin.|

1. İsteğe bağlı: **Gelişmiş seçenekleri seçin...**  

   | Parametre | Açıklama |
   | --- | --- |
   |  Yerel sorgu kullan |Seçildiğinde, ODBC sürücüsü TSQL ' ı HiveQL 'e dönüştürmeye çalışır. Yalnızca, saf HiveQL deyimlerini gönderdikten sonra% 100 olduğundan bunu kullanacaksınız. SQL Server veya Azure SQL veritabanı 'na bağlanırken, işareti kaldırılmış olarak bırakmalısınız. |
   |  Blok başına getirilen satırlar |Çok sayıda kayıt getirilirken, en iyi performansı sağlamak için bu parametreyi ayarlama gerekebilir. |
   |  Varsayılan dize sütunu uzunluğu, Ikili sütun uzunluğu, ondalık sütun ölçeği |Veri türü uzunlukları ve ön ekleri verilerin nasıl döndürüldüğünü etkileyebilir. Duyarlık ve/veya kesme kaybı nedeniyle yanlış bilgilerin döndürülmesine neden olur. |

    ![Gelişmiş Seçenekler](./media/apache-hadoop-connect-excel-hive-odbc-driver/hiveodbc-datasource-advancedoptions1.png "GELIŞMIŞ DSN yapılandırma seçenekleri")

1. Veri kaynağını test etmek için **Test** ' i seçin. Veri kaynağı doğru şekilde yapılandırıldığında, test sonucu **başarılı oldu!** öğesini gösterir.  

1. Test penceresini kapatmak için **Tamam ' ı** seçin.  

1. **MICROSOFT HIVE ODBC sürücüsü DSN kurulum** penceresini kapatmak için **Tamam ' ı** seçin.  

1. **ODBC veri kaynağı Yöneticisi** penceresini kapatmak için **Tamam ' ı** seçin.  

## <a name="import-data-into-excel-from-hdinsight"></a>HDInsight’tan Excel’e veri aktarma

Aşağıdaki adımlarda, bir Hive tablosundan bir Excel çalışma kitabına veri aktarma yolu, önceki bölümde oluşturduğunuz ODBC veri kaynağını kullanarak açıklanır.

1. Excel’de yeni veya mevcut bir çalışma kitabını açın.

2. **ODBC 'den** başlatmak için **veri** sekmesinden**ODBC 'den** **diğer kaynaklardan** >  **veri** > Al ' a gidin.

    ![Veri bağlantısı açma Sihirbazı](./media/apache-hadoop-connect-excel-hive-odbc-driver/simbahiveodbc-excel-dataconnection1.png "Veri bağlantısı açma Sihirbazı")

3. Aşağı açılan listeden, son bölümde oluşturduğunuz veri kaynağı adını seçin ve ardından **Tamam**' ı seçin.

4. İlk kullanım için bir **ODBC sürücüsü** iletişim kutusu açılır. Sol menüden **Windows** ' u seçin. Ardından, **Gezgin** penceresini açmak için **Bağlan** ' ı seçin.

5. **Gezgin**'den **HIVE** > **varsayılan**hivesampletable ' a gidin ve ardından Yükle ' yi seçin. >  Verilerin Excel 'e aktarılması birkaç dakika sürer.

    ![HDInsight HIVE ODBC Gezgini](./media/apache-hadoop-connect-excel-hive-odbc-driver/hdinsight-hive-odbc-navigator.png "Veri bağlantısı açma Sihirbazı")

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Microsoft Hive ODBC sürücüsünü kullanarak HDInsight hizmetinden Excel 'e veri almayı öğrendiniz. Benzer şekilde, HDInsight hizmetinden SQL veritabanı 'na veri alabilirsiniz. Verileri bir HDInsight hizmetine yüklemek de mümkündür. Daha fazla bilgi için bkz:

* [Apache Hive verilerini Azure HDInsight 'Ta Microsoft Power BI Ile görselleştirin](apache-hadoop-connect-hive-power-bi.md).
* [Azure HDInsight 'ta Power BI etkileşimli sorgu Hive verilerini görselleştirin](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Azure HDInsight 'ta Apache Hive sorguları çalıştırmak Için Apache Zeppelin kullanın](../interactive-query/hdinsight-connect-hive-zeppelin.md).
* [Power Query kullanarak Excel 'i Apache Hadoop bağlama](apache-hadoop-connect-excel-power-query.md).
* [Azure HDInsight 'A bağlanın ve Visual Studio Data Lake araçları 'nı kullanarak Apache Hive sorguları çalıştırın](apache-hadoop-visual-studio-tools-get-started.md).
* [Visual Studio Code Için Azure HDInsight aracını kullanın](../hdinsight-for-vscode.md).
* [HDInsight 'a veri yükleme](./../hdinsight-upload-data.md).