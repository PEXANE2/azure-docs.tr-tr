---
title: ODBC Sürücüsü & PowerShell ile Apache Hive Sorgula - Azure HDInsight
description: Azure HDInsight'ta Apache Hive kümelerini sorgulamak için Microsoft Hive ODBC sürücüsünü ve PowerShell'i kullanın.
keywords: kovan,kovan odbc,powershell
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.date: 06/27/2019
ms.openlocfilehash: f6139bb98fa0272e43c8e180d4ec029f7a7538bb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73494327"
---
# <a name="tutorial-query-apache-hive-with-odbc-and-powershell"></a>Öğretici: ODBC ve PowerShell ile Apache Hive Sorgu

Microsoft ODBC sürücüleri, Apache Hive da dahil olmak üzere farklı veri kaynaklarıyla etkileşim kurmak için esnek bir yol sağlar. Hive kümenize bağlantı açmak, seçtiğiniz bir sorguyu geçmek ve sonuçları görüntülemek için ODBC sürücülerini kullanan PowerShell gibi komut dosyası dillerinde kod yazabilirsiniz.

Bu öğreticide, aşağıdaki görevleri yapacaksınız:

> [!div class="checklist"]
> * Microsoft Hive ODBC sürücüsünü indirin ve yükleyin
> * Kümenize bağlı bir Apache Hive ODBC veri kaynağı oluşturma
> * PowerShell'i kullanarak kümenizdeki örnek bilgileri sorgula

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce aşağıdaki öğelere sahip olmanız gerekir:

* HDInsight'ta Etkileşimli Sorgu kümesi. Bir tane oluşturmak için Azure [HDInsight ile başlayın'](../hdinsight-hadoop-provision-linux-clusters.md)a bakın. Küme türü olarak **Etkileşimli Sorgu'yu** seçin.

## <a name="install-microsoft-hive-odbc-driver"></a>Microsoft Hive ODBC sürücüsünü yükleme

[Microsoft Hive ODBC Sürücüsünü](https://www.microsoft.com/download/details.aspx?id=40886)indirin ve kurun.

## <a name="create-apache-hive-odbc-data-source"></a>Apache Hive ODBC veri kaynağı oluşturma

Aşağıdaki adımlar, apache hive ODBC veri kaynağının nasıl oluşturulacağını gösterir.

1. Windows'dan**Windows Yönetim Araçları** > **ODBC Veri Kaynaklarını Başlat'a (32 bit)/(64-bit)** gidin. **Start** >   **ODBC Veri Kaynağı Yöneticisi** penceresi açılır.

    ![OBDC veri kaynağı yöneticisi](./media/apache-hive-query-odbc-driver-powershell/hive-odbc-driver-dsn-setup.png "ODBC Veri Kaynağı Yöneticisi ni kullanarak Bir DSN'yi yapılandırma")

1. Kullanıcı **DSN** sekmesinden Yeni Veri **Kaynağı Oluştur** penceresini açmak için **Ekle'yi** seçin.

1. **Microsoft Hive ODBC Sürücüsü'nü**seçin ve ardından **Microsoft Hive ODBC Driver DSN Kurulum** penceresini açmak için **Finish'i** seçin.

1. Aşağıdaki değerleri yazın veya seçin:

   | Özellik | Açıklama |
   | --- | --- |
   |  Data Source Name |Veri kaynağınız için bir ad verin |
   |  Ana bilgisayar(lar) |`CLUSTERNAME.azurehdinsight.net` yazın. Örneğin, `myHDICluster.azurehdinsight.net` |
   |  Bağlantı noktası |**443** yazın.|
   |  Database |**Varsayılan kullanın.** |
   |  Mechanism |**Windows Azure HDInsight Hizmetini** Seçin |
   |  User Name |HDInsight kümesi HTTP kullanıcı adını girin. Varsayılan kullanıcı adı **admin** şeklindedir. |
   |  Parola |HDInsight küme kullanıcı parolası girin. Parolayı **Kaydet (Şifreli)** onay kutusunu seçin.|

1. İsteğe bağlı: **Gelişmiş Seçenekleri**seçin.  

   | Parametre | Açıklama |
   | --- | --- |
   |  Yerel Sorgu'yı kullanma |Seçildiğinde, ODBC sürücüsü TSQL'i HiveQL'ye dönüştürmeye çalışmaz. Bu seçeneği yalnızca saf HiveQL ekstreleri gönderdiğinden %100 eminseniz kullanın. SQL Server veya Azure SQL Veritabanı'na bağlanırken, veritabanını işaretsiz bırakmalısınız. |
   |  Blok başına getirilen satırlar |Çok sayıda kayıt alırken, en iyi performansları sağlamak için bu parametrenin alımı gerekebilir. |
   |  Varsayılan dize sütun uzunluğu, İkili sütun uzunluğu, Ondalık sütun ölçeği |Veri türü uzunlukları ve hassasiyetleri verilerin döndürülme şeklini etkileyebilir. Hassaslık ve kesilme kaybı nedeniyle yanlış bilgilerin döndürülmesine neden olurlar. |

    ![Gelişmiş DSN yapılandırma seçenekleri](./media/apache-hive-query-odbc-driver-powershell/odbc-data-source-advanced-options.png "Gelişmiş DSN yapılandırma seçenekleri")

1. Veri kaynağını sınamak için **Test'i** seçin. Veri kaynağı doğru yapılandırıldığında, test sonucu **BAŞARı**gösterir.  

1. Test penceresini kapatmak için **Tamam'ı** seçin.  

1. **Microsoft Hive ODBC Driver DSN Kurulum** penceresini kapatmak için **Tamam'ı** seçin.  

1. **ODBC Veri Kaynağı Yöneticisi** penceresini kapatmak için **Tamam'ı** seçin.  

## <a name="query-data-with-powershell"></a>PowerShell ile verileri sorgula

Aşağıdaki PowerShell komut dosyası, ODBC'nin hive kümesini sorgulayan bir işlevdir.

```powershell
function Get-ODBC-Data {

   param(
   [string]$query=$(throw 'query is required.'),
   [string]$dsn,  
   [PSCredential] $cred = (Get-Credential)  
   )

   $conn = New-Object System.Data.Odbc.OdbcConnection
   $uname = $cred.UserName

   $pswd = (New-Object System.Net.NetworkCredential -ArgumentList "", $cred.Password).Password
   $conn.ConnectionString = "DSN=$dsn;Uid=$uname;Pwd=$pswd;"
   $conn.open()
   $cmd = New-object System.Data.Odbc.OdbcCommand($query,$conn)

   $ds = New-Object system.Data.DataSet

   (New-Object system.Data.odbc.odbcDataAdapter($cmd)).fill($ds) #| out-null
   $conn.close()
   $ds.Tables
}
```

Aşağıdaki kod parçacığı, öğreticinin başında oluşturduğunuz Etkileşimli Sorgu kümesinde bir sorgu yürütmek için yukarıdaki işlevi kullanır. Microsoft `DATASOURCENAME` **Hive ODBC Driver DSN Kurulum** ekranında belirttiğiniz Veri **Kaynağı Adı** ile değiştirin. Kimlik bilgileri istendiğinde, kümeyi oluşturduğunuzda **Cluster login kullanıcı adı** ve Cluster giriş **parolası** altında girdiğiniz kullanıcı adı ve parolayı girin.

```powershell

$dsn = "DATASOURCENAME"

$query = "select count(distinct clientid) AS total_clients from hivesampletable"

Get-ODBC-Data -query $query -dsn $dsn
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu, HDInsight kümesini ve depolama hesabını silin. Bunu yapmak için kümenin oluşturulduğu kaynak grubunu seçin ve **Sil'i**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure HDInsight İnteraktif Sorgu kümenizden veri almak için Microsoft Hive ODBC sürücüsünü ve PowerShell'i nasıl kullanacağınızı öğrendiniz.

> [!div class="nextstepaction"]
> [ODBC kullanarak Excel'i Apache Hive'a bağlayın](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md)
