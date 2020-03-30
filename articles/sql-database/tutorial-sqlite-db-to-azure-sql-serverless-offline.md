---
title: "Öğretici: SQLite veritabanınızı Azure SQL Database Serverless'a nasıl geçirebilirsiniz?"
description: Azure Veri Fabrikası'nı kullanarak SQLite'dan Azure SQL Database Serverless'a çevrimdışı geçiş gerçekleştirmeyi öğrenin.
services: sql-database
author: joplum
ms.author: joplum
manager: prda
ms.service: sql-database
ms.workload: data-services
ms.topic: article
ms.date: 01/08/2020
ms.openlocfilehash: c718daa4bc99bffd6fcfeb084299bed6682fe884
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75780515"
---
# <a name="how-to-migrate-your-sqlite-database-to-azure-sql-database-serverless"></a>SQLite veritabanınızı Azure SQL Database Serverless'a nasıl geçirilir?
Birçok kişi için, SQLite veritabanları ve SQL programlama ilk deneyimini sağlar. Birçok işletim sistemi ve popüler uygulamalarda dahil SQLite bir dünyanın en yaygın olarak dağıtılan ve kullanılan veritabanı motorları yapar. Ve büyük olasılıkla birçok kişi kullanmak ilk veritabanı motoru olduğundan, genellikle projeler veya uygulamaların merkezi bir parçası olarak sona erebilir. Proje veya uygulamanın ilk SQLite uygulamasını uzattığı durumlarda, geliştiricilerin verilerini güvenilir, merkezi leştirilmiş bir veri deposuna geçirmeleri gerekebilir.

Azure SQL Veritabanı sunucusuz, iş yükü talebine göre otomatik olarak hesaplama yı ölçeklendiren tek veritabanları için bir bilgi işlem katmanı ve saniyede kullanılan işlem miktarı için faturalardır. Sunucusuz bilgi işlem katmanı, yalnızca depolamanın faturalandırıldığu etkin olmayan dönemlerde veritabanlarını otomatik olarak duraklatır ve etkinlik döndüğünde veritabanlarını otomatik olarak devam ettirir.

Aşağıdaki adımları izledikten sonra, veritabanınız Azure SQL Database Serverless'a geçirilerek veritabanınızı buluttaki diğer kullanıcılar veya uygulamalar için kullanılabilir hale getirmenize ve yalnızca kullandığınız kadar ını en az uygulama kodu değişikliğiyle ödeme yapmanızı sağlar.

## <a name="prerequisites"></a>Ön koşullar
- Azure Aboneliği
- Geçirmek istediğiniz SQLite2 veya SQLite3 veritabanı
- Windows ortamı
  - Yerel bir Windows ortamınız yoksa, geçiş için Azure'da bir Windows VM kullanabilirsiniz. Azure Dosyaları ve Depolama Gezgini'ni kullanarak SQLite veritabanı dosyanızı VM'de taşıyın ve kullanılabilir hale getirin.

## <a name="steps"></a>Adımlar

1. Serverless bilgi işlem katmanında yeni bir Azure SQL Veritabanı sağlama.

    ![azure sql veritabanı sunucusuz için sağlama örneğini gösteren Azure portalıekran görüntüsü](./media/tutorial-sqlite-db-to-azure-sql-serverless-offline/provision-serverless.png)

2. Windows ortamınızda SQLite veritabanı dosyanızın kullanılabilir olduğundan emin olun. Zaten bir tane yoksa bir SQLite ODBC Sürücüsü yükleyin (örneğin, Açık http://www.ch-werner.de/sqliteodbc/)Kaynak'ta çok sayıda var.

3. Veritabanı için bir Sistem DSN oluşturun. Sistem mimarisiyle eşleşen Veri Kaynağı Yöneticisi uygulamasını kullandığınızdan emin olun (32 bit vs 64-bit). Sistem ayarlarınızda hangi sürümü çalıştırdığınızı bulabilirsiniz.

    - Ortamınızda ODBC Veri Kaynağı Yöneticisi'ni açın.
    - Sistem DSN sekmesine tıklayın ve "Ekle"ye tıklayın
    - Yüklediğiniz SQLite ODBC konektörünü seçin ve bağlantıya anlamlı bir ad verin, örneğin sqlitemigrationsource
    - Veritabanı adını .db dosyasına ayarlama
    - Kaydet ve çık

4. Kendi kendine barındırılan tümleştirme çalışma süresini indirin ve yükleyin. Bunu yapmanın en kolay yolu, belgelerde ayrıntılı olarak belirtildiği gibi Express yükleme seçeneğidir. El ile yüklemeyi seçerseniz, uygulamaya Veri Fabrikası örneğinde şu şekilde bulunabilecek bir kimlik doğrulama anahtarı sağlamanız gerekir:

    - ADF'yi başlatma (Azure portalındaki hizmetten yazar ve monitör)
    - Soldaki "Yazar" sekmesine (Mavi kalem) tıklayın
    - Bağlantılar'ı tıklatın (sol altta), ardından Tümleştirme çalışma saatleri
    - Yeni Self-Hosted Integration Runtime ekleyin, bir ad verin, *Seçenek 2'yi*seçin.

5. Veri Fabrikanızdaki kaynak SQLite veritabanı için yeni bir bağlantılı hizmet oluşturun.

    ![Azure Veri Fabrikası'nda boş bağlantılı hizmetler bıçağını gösteren ekran görüntüsü](./media/tutorial-sqlite-db-to-azure-sql-serverless-offline/linked-services-create.png)

6. Bağlantılar'da, Bağlantılı Hizmet altında Yeni

7. "ODBC" bağlayıcısını arayın ve seçin


    ![Azure Veri Fabrikası'ndaki bağlantılı hizmetler bıçağında ODBC bağlayıcı logosunu gösteren ekran görüntüsü](./media/tutorial-sqlite-db-to-azure-sql-serverless-offline/linked-services-odbc.png)

8. Bağlantılı hizmete anlamlı bir ad verin, örneğin, "sqlite_odbc". "Tümleştirme çalışma süresi yle bağlan" açılır tarihinden entegrasyon çalışma sürenizi seçin. İlk Katalog değişkenini .db dosyası için dosya yolu ile ve DSN'yi sistem DSN bağlantısının adıyla değiştirerek aşağıdaki bağlantı dizesini girin: 

    ```
    Connection string: Provider=MSDASQL.1;Persist Security Info=False;Mode=ReadWrite;Initial Catalog=C:\sqlitemigrationsource.db;DSN=sqlitemigrationsource
    ```

9. Kimlik doğrulama türünü Anonymous olarak ayarlama

10. Bağlantıyı sınama

    ![Azure Veri Fabrikası'nda başarılı bağlantı gösteren ekran görüntüsü](./media/tutorial-sqlite-db-to-azure-sql-serverless-offline/linked-services-test-successful.png)

11. Serverless SQL hedefiniz için başka bir bağlantılı hizmet oluşturun. Bağlı hizmet sihirbazını kullanarak veritabanını seçin ve SQL kimlik bilgilerini sağlayın.

    ![Azure Veri Fabrikası'nda seçilen Azure SQL Veritabanını gösteren ekran görüntüsü](./media/tutorial-sqlite-db-to-azure-sql-serverless-offline/linked-services-create-target.png)

12. CREATE TABLE deyimlerini SQLite veritabanınızdan ayıklayın. Bunu veritabanı dosyanızda aşağıdaki Python komut dosyasını çalıştırarak yapabilirsiniz.

    ```
    #!/usr/bin/python
    import sqlite3
    conn = sqlite3.connect("sqlitemigrationsource.db")
    c = conn.cursor()

    print("Starting extract job..")
    with open('CreateTables.sql', 'w') as f:
        for tabledetails in c.execute("SELECT * FROM sqlite_master WHERE type='table'"):
            print("Extracting CREATE statement for " + (str(tabledetails[1])))
            print(tabledetails)
            f.write(str(tabledetails[4].replace('\n','') + ';\n'))
    c.close()
    ```

13. CreateTables.sql dosyasından CREATE tablo ekstrelerini kopyalayarak ve Azure portalındaki Sorgu Düzenleyicisi'ndeki SQL deyimlerini çalıştırarak Serverless SQL hedef ortamınızda açılış tablolarını oluşturun.

14. Veri Fabrikanızın ana ekranına dönün ve iş oluşturma sihirbazı üzerinden çalıştırmak için "Verileri Kopyala"yı tıklatın.

    ![Azure Veri Fabrikası'nda Veri Kopyalama sihirbazı logosunu gösteren ekran görüntüsü](./media/tutorial-sqlite-db-to-azure-sql-serverless-offline/copy-data.png)

15. Onay kutularını kullanarak kaynak SQLite veritabanındaki tüm tabloları seçin ve bunları Azure SQL'deki hedef tablolarla eşleyin. İş ilerledikten sonra verilerinizi SQLite'dan Azure SQL'e başarıyla aktarmış sınız!

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için [Bkz. Quickstart: Azure portalını kullanarak Azure SQL Veritabanı'nda tek bir veritabanı oluşturun.](sql-database-single-database-get-started.md)
- Kaynak sınırları için Bkz. [Serverless bilgi işlem katmanı kaynak sınırları.](sql-database-vCore-resource-limits-single-databases.md#general-purpose---serverless-compute---gen5)
