---
title: Azure Akışı Analizi işinde SQL Veritabanı başvuru verilerini kullanma
description: Bu makalede, Azure portalında ve Visual Studio'daki bir Azure Akış Analizi işi için başvuru veri girişi olarak SQL Veritabanı'nın nasıl kullanılacağı açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/29/2019
ms.openlocfilehash: aebb590d93b3fb26151f15c176a2941845cdd50c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426495"
---
# <a name="use-reference-data-from-a-sql-database-for-an-azure-stream-analytics-job"></a>Azure Akışı Analizi işi için SQL Veritabanı'ndaki başvuru verilerini kullanma

Azure Akış Analizi, referans verileri için giriş kaynağı olarak Azure SQL Veritabanı'nı destekler. SQL Veritabanı'nı Azure portalında ve Visual Studio'da Stream Analytics araçlarıyla Stream Analytics işiniz için referans veri olarak kullanabilirsiniz. Bu makalede, her iki yöntemin nasıl yapılacağını gösterir.

## <a name="azure-portal"></a>Azure portalında

Azure portalını kullanarak Azure SQL Veritabanı'nı başvuru girdi kaynağı olarak eklemek için aşağıdaki adımları kullanın:

### <a name="portal-prerequisites"></a>Portal ön koşulları

1. Stream Analytics işi oluşturma.

2. Stream Analytics işi tarafından kullanılmak üzere bir depolama hesabı oluşturun.

3. Azure SQL Veritabanınızı, Akış Analizi işi tarafından referans veriolarak kullanılacak bir veri kümesiyle oluşturun.

### <a name="define-sql-database-reference-data-input"></a>SQL Veritabanı referans veri girişi tanımlama

1. Akış Analizi **işinizde, İş topolojisi**altında **Girdiler'i** seçin. **Başvuru girişi ekle'yi** tıklatın ve **SQL Veritabanı'nı**seçin.

   ![Stream Analytics iş girişi](./media/sql-reference-data/stream-analytics-inputs.png)

2. Akış Analizi Giriş Yapılandırmalarını doldurun. Veritabanı adını, sunucu adını, kullanıcı adını ve parolayı seçin. Referans veri girişinizin düzenli aralıklarla yenilenmesini istiyorsanız, DD:HH:MM'deki yenileme hızını belirtmek için "Açık"ı seçin. Kısa yenileme hızına sahip büyük veri kümeleriniz varsa, [delta sorgusu](sql-reference-data.md#delta-query)kullanabilirsiniz.

   ![SQL Veritabanı başvuru yapılandırması](./media/sql-reference-data/sql-input-config.png)

3. SQL sorgu düzenleyicisinde anlık görüntü sorgusunu test edin. Daha fazla bilgi için [bkz: Verileri bağlamak ve sorgulamak için Azure portalının SQL sorgu düzenleyicisini kullanın](../sql-database/sql-database-connect-query-portal.md)

### <a name="specify-storage-account-in-job-config"></a>Job config'de depolama hesabı belirtin

**Yapılandırma** altında **Depolama hesabı ayarlarına** gidin ve Depolama Hesabı **Ekle'yi**seçin.

   ![Akış Analizi depolama hesabı ayarları](./media/sql-reference-data/storage-account-settings.png)

### <a name="start-the-job"></a>İşi başlatma

Diğer girdileri, çıktıları ve sorguyu yapılandırıldıktan sonra Akış Analizi işini başlatabilirsiniz.

## <a name="tools-for-visual-studio"></a>Visual Studio Araçları

Visual Studio'yu kullanarak Azure SQL Veritabanı'nı başvuru girdi kaynağı olarak eklemek için aşağıdaki adımları kullanın:

### <a name="visual-studio-prerequisites"></a>Visual Studio ön koşulları

1. [Visual Studio için Stream Analytics araçlarını yükleyin.](stream-analytics-tools-for-visual-studio-install.md) Visual Studio'nun aşağıdaki sürümleri desteklenir:

   * Visual Studio 2015
   * Visual Studio 2019

2. Visual Studio hızlı başlatma [için Stream Analytics araçlarını](stream-analytics-quick-create-vs.md) yakından bilin.

3. Depolama hesabı oluşturma.

### <a name="create-a-sql-database-table"></a>SQL Veritabanı tablosu oluşturma

Başvuru verilerinizi depolamak için bir tablo oluşturmak için SQL Server Management Studio'yu kullanın. Ayrıntılar için [SSMS'i kullanarak ilk Azure SQL veritabanınızı](../sql-database/sql-database-design-first-database.md) tasarla'ya bakın.

Aşağıdaki örnekte kullanılan örnek tablo aşağıdaki ifadeden oluşturuldu:

```SQL
create table chemicals(Id Bigint,Name Nvarchar(max),FullName Nvarchar(max));
```

### <a name="choose-your-subscription"></a>Aboneliğinizi seçin

1. Visual Studio'nun **Görünüm** menüsünde **Sunucu Gezgini**'ni seçin.

2. **Azure'a**sağ tıklayın, **Microsoft Azure Aboneliği'ne Bağlan'ı**seçin ve Azure hesabınızla oturum açın.

### <a name="create-a-stream-analytics-project"></a>Stream Analytics projesi oluşturma

1. **Dosya > Yeni Proje**'yi seçin. 

2. Sol taraftaki şablon listesinden **Stream Analytics**'i ve ardından **Azure Stream Analytics Uygulaması**'nı seçin. 

3. Proje **Adı,** **Konum**ve **Çözüm adını**girin ve **Tamam'ı**seçin.

   ![Visual Studio'da Yeni Stream Analytics projesi](./media/sql-reference-data/stream-analytics-vs-new-project.png)

### <a name="define-sql-database-reference-data-input"></a>SQL Veritabanı referans veri girişi tanımlama

1. Yeni bir giriş oluşturun.

   ![Visual Studio'da Yeni Stream Analytics girişi](./media/sql-reference-data/stream-analytics-vs-input.png)

2. **Çözüm Gezgini'nde** **Giriş.json'a** çift tıklayın.

3. **Akış Analizi Giriş Yapılandırmasını**doldurun. Veritabanı adını, sunucu adını, yenileme türünü ve yenileme hızını seçin. Biçimde `DD:HH:MM`yenileme hızını belirtin.

   ![Visual Studio'da Stream Analytics giriş yapılandırması](./media/sql-reference-data/stream-analytics-vs-input-config.png)

   "Yalnızca bir kez yürüt" veya "Dönemsel olarak yürüt" seçeneğini seçerseniz, **input.json** dosya düğümü altında projede **[Input Alias].snapshot.sql** adlı bir SQL CodeBehind dosyası oluşturulur.

   ![Visual Studio'da giriş kodu](./media/sql-reference-data/once-or-periodically-codebehind.png)

   "Delta ile Periyodik Olarak Yenile" seçeneğini seçerseniz, iki SQL CodeBehind dosyası oluşturulur: **[Input Alias].snapshot.sql** ve **[Input Alias].delta.sql**.

   ![Çözüm gezgininde kod arkada](./media/sql-reference-data/periodically-delta-codebehind.png)

4. Düzenleyicide SQL dosyasını açın ve SQL sorgusunu yazın.

5. Visual Studio 2019 kullanıyorsanız ve SQL Server Data araçlarını yüklediyseniz, **Çalıştır'ı**tıklatarak sorguyu test edebilirsiniz. SQL veritabanına bağlanmanıza yardımcı olmak için bir sihirbaz penceresi açılır ve sorgu sonucu alttaki pencerede görünür.

### <a name="specify-storage-account"></a>Depolama hesabı belirtin

SQL başvuru anlık görüntülerini depolamak için depolama hesabını belirtmek için **JobConfig.json'ı** açın.

   ![Visual Studio'da Stream Analytics iş yapılandırması](./media/sql-reference-data/stream-analytics-job-config.png)

### <a name="test-locally-and-deploy-to-azure"></a>Yerel olarak test edin ve Azure'a dağıtın

İşi Azure'a dağıtmadan önce, sorgu mantığını canlı giriş verilerine karşı yerel olarak sınayabilirsiniz. Bu özellik hakkında daha fazla bilgi için Visual [Studio için Azure Akış Analizi araçlarını kullanarak canlı verileri yerel olarak test edin (Önizleme) bölümüne](stream-analytics-live-data-local-testing.md)bakın. Test etmeyi bitirdiğinizde, **Azure'a Gönder'i**tıklatın. Visual [Studio için Azure Akış Analizi araçlarını kullanarak](stream-analytics-quick-create-vs.md) işe nasıl başlayacağınızı öğrenmek için hızlı bir şekilde başlatın.

## <a name="delta-query"></a>Delta sorgusu

Delta sorgusunu kullanırken, [Azure SQL Veritabanı'ndaki zamansal tablolar](../sql-database/sql-database-temporal-tables.md) önerilir.

1. Azure SQL Veritabanı'nda geçici bir tablo oluşturun.
   
   ```SQL 
      CREATE TABLE DeviceTemporal 
      (  
         [DeviceId] int NOT NULL PRIMARY KEY CLUSTERED 
         , [GroupDeviceId] nvarchar(100) NOT NULL
         , [Description] nvarchar(100) NOT NULL 
         , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
         , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
         , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
      )  
      WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.DeviceHistory));  -- DeviceHistory table will be used in Delta query
   ```
2. Anlık görüntü sorgusunu yazar. 

   Sistem zamanında geçerli olan SQL veritabanı zamansal tablosundan referans veri kümesini elde etmek için Akış Analizi çalışma saatini öğretmek için ** \@anlık** zaman parametresini kullanın. Bu parametreyi sağlamazsanız, saat eğriliği nedeniyle yanlış bir temel başvuru veri kümesi alma riskine dersiniz. Tam anlık görüntü sorgusunun bir örneği aşağıda gösterilmiştir:
   ```SQL
      SELECT DeviceId, GroupDeviceId, [Description]
      FROM dbo.DeviceTemporal
      FOR SYSTEM_TIME AS OF @snapshotTime
   ```
 
2. Delta sorgusunu yazar. 
   
   Bu sorgu, SQL veritabanınızda bir başlangıç süresi, ** \@deltaStartTime**ve bir bitiş zamanı ** \@deltaEndTime**içinde eklenen veya silinen tüm satırları alır. Delta sorgusu, anlık görüntü sorgusuyla aynı sütunları ve sütun **_işlemini_** döndürmelidir. Bu sütun, satır ın ** \@deltaStartTime** ve ** \@deltaEndTime**arasında takılıp veya silinip silinmediği tanımlanır. Kayıtlar eklenirse, ortaya çıkan satırlar **1** veya silinmişse **2** olarak işaretlenir. 

   Güncelleştirilen kayıtlar için, zamansal tablo ekleme ve silme işlemini yakalayarak defter tutma yapar. Akış Analizi çalışma süresi daha sonra başvuru verilerini güncel tutmak için delta sorgusunun sonuçlarını önceki anlık görüntüye uygular. Delta sorgusunun bir örneği aşağıda gösteriştir:

   ```SQL
      SELECT DeviceId, GroupDeviceId, Description, 1 as _operation_
      FROM dbo.DeviceTemporal
      WHERE ValidFrom BETWEEN @deltaStartTime AND @deltaEndTime   -- records inserted
      UNION
      SELECT DeviceId, GroupDeviceId, Description, 2 as _operation_
      FROM dbo.DeviceHistory   -- table we created in step 1
      WHERE ValidTo BETWEEN @deltaStartTime AND @deltaEndTime     -- record deleted
   ```
 
   Akış Analizi çalışma zamanının, denetim noktalarını depolamak için delta sorgusuna ek olarak anlık görüntü sorgusunu düzenli aralıklarla çalıştırabileceğini unutmayın.

## <a name="test-your-query"></a>Sorgunuzu test etme
   Sorgunuzun, Akış Analizi işinin referans veri olarak kullanacağı beklenen veri kümesini döndüreceğini doğrulamak önemlidir. Sorgunuzu test etmek için portaldaki İş Topolojisi bölümüne girin' e gidin. Daha sonra SQL Veritabanı Başvuru girişinizde Örnek Verileri seçebilirsiniz. Örnek kullanılabilir hale geldikten sonra, dosyayı karşıdan yükleyebilir ve döndürülen verilerin beklendiği gibi olup olmadığını kontrol edebilirsiniz. Geliştirme nizi optimize etmek ve yinelemeleri test etmek istiyorsanız, [Visual Studio için Stream Analytics araçlarını](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-install)kullanmanız önerilir. Ayrıca, önce sorgunun Azure SQL Veritabanınızdan doğru sonuçları döndürdüğünden emin olmak ve ardından bunu Akış Analizi işinizde kullanmak için tercih ettiğiniz diğer herhangi bir araç ta. 

## <a name="faqs"></a>SSS

**Azure Akış Analizi'nde SQL referans veri girişi kullanarak ek maliyete tabi olacak mıyım?**

Stream Analytics işinde [akış birimi başına](https://azure.microsoft.com/pricing/details/stream-analytics/) ek ücret yoktur. Ancak, Akış Analizi işinin ilişkili bir Azure depolama hesabı olması gerekir. Akış Analizi işi, başvuru veri kümesini almak için SQL DB'yi (iş başlatma ve yenileme aralığı sırasında) sorgular ve bu anlık görüntünün depo hesabında depolanmasını sağlar. Bu anlık görüntüleri depolamak, Azure depolama hesabının [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/storage/) ayrıntılı olarak ayrıntılı olarak ek ücretlere tabi olacaktır.

**Başvuru verilerinin anlık görüntüsünün SQL DB'den sorgulandığını ve Azure Akışı Analizi işinde kullanıldığını nasıl bilebilirim?**

Mantıksal Ad tarafından filtre uygulanmış iki ölçüm vardır (Metrics Azure Portal altında) SQL veritabanı başvuru veri girişinin durumunu izlemek için kullanabilirsiniz.

   * Giriş Etkinlikleri: Bu metrik, SQL veritabanı başvuru veri kümesinden yüklenen kayıt sayısını ölçer.
   * InputEventBytes: Bu metrik, Akış Analizi işinin anısına yüklenen başvuru verianlık görüntüsünün boyutunu ölçer. 

Bu ölçümlerin her ikisinin birleşimi, işin başvuru veri kümesini almak ve sonra belleğe yüklemek için SQL veritabanını sorgulayıp sorgulamadın olduğu konusunda kullanılabilir.

**Özel bir Azure SQL Veritabanı türüne ihtiyacım olacak mı?**

Azure Akış Analizi, her tür Azure SQL Veritabanı ile çalışır. Ancak, referans veri girişi için ayarlanan yenileme hızının sorgu yükünüzü etkileyebileceğini anlamak önemlidir. Delta sorgusu seçeneğini kullanmak için Azure SQL Veritabanı'nda zamansal tabloların kullanılması önerilir.

**Azure Akış Analizi neden anlık görüntüleri Azure Depolama hesabında saklıyor?**

Stream Analytics tam bir olay işlemesi ve en az bir olay teslimini garanti eder. Geçici sorunların işinizi etkilediği durumlarda, durumu geri yüklemek için az miktarda yeniden oynatma gerekir. Yeniden oynatmayı etkinleştirmek için bu anlık görüntülerinin bir Azure Depolama hesabında depolanması gerekir. Denetim noktası yeniden oynatma hakkında daha fazla bilgi için [Azure Akış Analizi işlerinde Denetim Noktası ve yeniden oynatma kavramlarına](stream-analytics-concepts-checkpoint-replay.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Akış Analizi'nde aramalar için referans verilerini kullanma](stream-analytics-use-reference-data.md)
* [Hızlı başlangıç: Visual Studio için Azure Stream Analytics araçlarını kullanarak bir Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Visual Studio için Azure Akış Analizi araçlarını kullanarak canlı verileri yerel olarak test edin (Önizleme)](stream-analytics-live-data-local-testing.md)
