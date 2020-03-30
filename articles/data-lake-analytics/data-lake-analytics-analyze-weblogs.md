---
title: Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme
description: U-SQL işlevlerini ve sorgularını çalıştırmak için Azure Data Lake Analytics'i kullanarak web sitesi günlüklerini nasıl analiz edebilirsiniz öğrenin.
services: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 3a196735-d0d9-4deb-ba68-c4b3f3be8403
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 04c6d4c74a82ccfbcbb0faecb0dca5ec495f6663
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71672873"
---
# <a name="analyze-website-logs-using-azure-data-lake-analytics"></a>Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme
Data Lake Analytics'i kullanarak web sitesi günlüklerini nasıl analiz edeceğiz, özellikle de web sitesini ziyaret etmeye çalıştıklarında hangi başvuru yapanların hatalarla karşılaştığını bulma konusunda.

## <a name="prerequisites"></a>Ön koşullar
* **Visual Studio 2015 veya Visual Studio 2013**.
* **[Visual Studio için Data Lake Araçları](https://aka.ms/adltoolsvs)**.

    Visual Studio için Data Lake Tools yüklendikten sonra Visual Studio'daki **Araçlar** menüsünde bir **Veri Gölü** öğesi görürsünüz:

    ![U-SQL Visual Studio menüsü](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)
* **Data Lake Analytics ve Visual Studio için Data Lake Tools temel bilgi.** Başlamak için bkz:

  * [Visual Studio için Data Lake araçlarını kullanarak U-SQL komut dosyası geliştirin.](data-lake-analytics-data-lake-tools-get-started.md)
* **Bir Data Lake Analytics hesabı.**  Bkz. [Bir Azure Veri Gölü Analizi hesabı oluşturun.](data-lake-analytics-get-started-portal.md)
* **Örnek verileri yükleyin.** Azure Portalı'nda, Veri Gölü Analizi hesabını açın ve sol menüde **Örnek Komut Dosyaları'nı** tıklatın ve ardından **Örnek Verileri Kopyala'yı**tıklatın. 

## <a name="connect-to-azure"></a>Azure'a Bağlanma
Herhangi bir U-SQL komut dosyası oluşturmadan ve test etmeden önce azure'a bağlanmanız gerekir.

**Data Lake Analytics'e bağlanmak için**

1. Visual Studio'yu açın.
2. **Veri Gölü > Seçenekleri ve Ayarları'nı**tıklatın.
3. **Oturum Aç'ı**veya birisi oturum açmışsa **Kullanıcıyı Değiştir'i** tıklatın ve yönergeleri izleyin.
4. Seçenekler ve Ayarlar iletişim kutusunu kapatmak için **Tamam'ı** tıklatın.

**Veri Gölü Analizi hesaplarınıza göz atmak için**

1. Visual Studio'dan **CTRL+ALT+S**tuşuna basarak **Server Explorer'ı** açın.
2. **Sunucu Gezgini**'nden, **Azure** seçeneğini ve ardından **Data Lake Analytics** seçeneğini genişletin. Varsa Data Lake Analytics hesaplarınızın listesini görürsünüz. Stüdyodan Data Lake Analytics hesapları oluşturamazsınız. Hesap oluşturmak için bkz. [Azure Portal'ı kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md) veya [Azure PowerShell'i kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>U-SQL uygulamasını geliştirin
U-SQL uygulaması çoğunlukla bir U-SQL komut dosyasıdır. U-SQL hakkında daha fazla bilgi edinmek için [u-SQL ile başlayın'a](data-lake-analytics-u-sql-get-started.md)bakın.

Uygulamaya kullanıcı tanımlı ek işleçler ekleyebilirsiniz.  Daha fazla bilgi için bkz: [Data Lake Analytics işleri için U-SQL kullanıcı tanımlı işleçler geliştirin.](data-lake-analytics-u-sql-develop-user-defined-operators.md)

**Data Lake Analytics işi oluşturma ve gönderme**

1. Yeni **> Projesi> dosyayı**tıklatın.
2. U-SQL Projesi türünü seçin.

    ![yeni U-SQL Visual Studio projesi](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. **Tamam**'a tıklayın. Visual studio, Script.usql dosyası yla bir çözüm oluşturur.
4. Script.usql dosyasına aşağıdaki komut dosyasını girin:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            DISTRIBUTED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    U-SQL'i anlamak için [bkz.](data-lake-analytics-u-sql-get-started.md)    
5. Projenize yeni bir U-SQL komut dosyası ekleyin ve aşağıdakileri girin:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();
6. İlk U-SQL komut dosyasına geri dön ve **Gönder** düğmesinin yanında Analytics hesabınızı belirtin.
7. **Çözüm Gezgini**'nden, **Script.usql** öğesine sağ tıklayın ve ardından **Betik Oluştur**'a tıklayın. Çıktı bölmesinde sonuçları doğrulayın.
8. **Çözüm Gezgini**'nden, **Script.usql** öğesine sağ tıklayın ve ardından **Betiği Gönder**'e tıklayın.
9. **Analytics Hesabı'nın** işi çalıştırmak istediğiniz hesap olduğunu doğrulayın ve sonra **Gönder'i**tıklatın. Gönderim tamamlandığında, gönderme işleminin sonuçları ve iş bağlantısı Visual Studio için Data Lake Araçları içinde sunulur.
10. İş başarıyla tamamlanana kadar bekleyin.  İş başarısız olduysa, büyük olasılıkla kaynak dosyayı eksik.  Lütfen bu öğreticinin Önkoşul bölümüne bakın. Ek sorun giderme bilgileri için [Bkz.](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

    İş tamamlandığında, aşağıdaki ekranı görürsünüz:

    ![veri gölü analitik weblogs web sitesi günlükleri analiz](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)
11. Şimdi **Script1.usql**için 7-10 adımlarını tekrarlayın.

**İş çıktısını görmek için**

1. **Sunucu Gezgini**'nden, **Azure** seçeneğini, **Data Lake Analytics** seçeneğini, Data Lake Analytics hesabınızı ve **Depolama Hesapları** seçeneğini genişletin, varsayılan Data Lake Store hesabına sağ tıklayın ve ardından **Explorer**'a tıklayın.
2. Klasörü açmak için **Örnekler'i** çift tıklatın ve ardından **Çıktılar'ı**çift tıklatın.
3. Double-click **BaşarısızYanıtlar.log**.
4. Ayrıca, doğrudan çıktıya gitmek için işin grafik görünümü içindeki çıktı dosyasını çift tıklatabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.
Farklı araçlar kullanarak Data Lake Analytics ile çalışmaya başlamak için bkz.

* [Azure Portal'ı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md)
* [Azure PowerShell'i kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [.NET SDK'yı kullanarak Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-net-sdk.md)
