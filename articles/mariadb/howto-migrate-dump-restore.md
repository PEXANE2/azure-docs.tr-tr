---
title: Döküm ve geri yükleme ile geçirin - MariaDB için Azure Veritabanı
description: Bu makalede, mysqldump, MySQL Workbench ve PHPMyAdmin gibi araçları kullanarak, MariaDB için Azure Veritabanınızdaki veritabanlarını yedeklemenin ve geri yüklemenin iki yaygın yolu açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 2/27/2020
ms.openlocfilehash: 72735e83af97fde8377e27daa45501704ef5a3c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164551"
---
# <a name="migrate-your-mariadb-database-to-azure-database-for-mariadb-using-dump-and-restore"></a>D'pboşve geri yüklemeyi kullanarak MariaDB veritabanınızı Azure Veritabanı'na geçirin
Bu makalede, MariaDB için Azure Veritabanınızdaki veritabanlarını yedeklemenin ve geri yüklemenin iki yaygın yolu açıklanmaktadır
- Komut satırından dökümü ve geri yükleme (mysqldump kullanarak) 
- PHPMyAdmin kullanarak dökümü ve geri yükleme

## <a name="before-you-begin"></a>Başlamadan önce
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- [MariaDB sunucusu için Azure Veritabanı Oluşturma - Azure portalı](quickstart-create-mariadb-server-database-using-azure-portal.md)
- [mysqldump](https://mariadb.com/kb/en/library/mysqldump/) komut satırı yardımcı programı bir makinede yüklü.
- MySQL Workbench [MySQL Workbench İndir](https://dev.mysql.com/downloads/workbench/) veya başka bir üçüncü taraf MySQL aracı dökümü yapmak ve komutları geri yükleyin.

## <a name="use-common-tools"></a>Ortak araçları kullanma
Verileri MariaDB için Azure Veritabanı'na uzaktan bağlamak ve geri yüklemek için MySQL Workbench veya mysqldump gibi yaygın yardımcı programları ve araçları kullanın. MariaDB için Azure Veritabanı'na bağlanmak için internet bağlantısı olan istemci makinenizde bu tür araçları kullanın. En iyi güvenlik uygulamaları için SSL şifreli bir bağlantı kullanın, [ayrıca Bkz. MariaDB için Azure Veritabanı'nda SSL bağlantısını yapılandırın.](concepts-ssl-connection-security.md) MariaDB için Azure Veritabanı'na geçerken döküm dosyalarını özel bulut konumuna taşımanız gerekmez. 

## <a name="common-uses-for-dump-and-restore"></a>Dökümü ve geri yüklemeiçin yaygın kullanımalanları
Mysqldump ve mysqlpump gibi MySQL yardımcı programlarını kullanarak veritabanlarını birkaç yaygın senaryoda MariaDB sunucusu için bir Azure Veritabanına boşaltıp yükleyebilirsiniz. 

<!--In other scenarios, you may use the [Import and Export](howto-migrate-import-export.md) approach instead.-->

- Tüm veritabanını geçirerken veritabanı dökümlerini kullanın. Bu öneri, büyük miktarda veriyi taşınırken veya canlı siteler veya uygulamalar için hizmet kesintisini en aza indirmek istediğinizde geçerlidir. 
-  MariaDB için Azure Veritabanı'na veri yüklerken veritabanındaki tüm tabloların InnoDB depolama altyapısını kullandığından emin olun. MariaDB için Azure Veritabanı yalnızca InnoDB Depolama altyapılarını destekler ve bu nedenle alternatif depolama altyapılarını desteklemez. Tablolarınız diğer depolama motorları ile yapılandırıldıysa, MariaDB için Azure Veritabanı'na geçmeden önce bunları InnoDB altyapı biçimine dönüştürün.
   Örneğin, MyISAM tablolarını kullanan bir WordPress veya WebApp'ınuz varsa, MariaDB için Azure Veritabanı'na geri dönmeden önce bu tabloları InnoDB biçimine geçirerek dönüştürün. Yeni bir `ENGINE=InnoDB` tablo oluştururken kullanılan motoru ayarlamak için yan tümceyi kullanın ve sonra geri yüklemeden önce verileri uyumlu tabloya aktarın. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- Uyumluluk sorunlarından kaçınmak için veritabanlarının dökümünü alırken kaynak ve hedef sistemlerde aynı MariaDB sürümünün bulunduğundan emin olun. Örneğin, varolan MariaDB sunucunuz sürüm 10.2 ise, sürüm 10.2'yi çalıştıracak şekilde yapılandırılan MariaDB için Azure Veritabanı'na geçiş niz gerekir. Komut, `mysql_upgrade` MariaDB sunucusu için bir Azure Veritabanında çalışmaz ve desteklenmez. MariaDB sürümlerinde yükseltme yapmanız gerekiyorsa, önce alt sürüm veritabanınızı kendi ortamınızda MariaDB'nin daha yüksek bir sürümüne boşaltın veya dışa aktarın. Ardından, `mysql_upgrade`MariaDB için bir Azure Veritabanına geçiş denemeden önce çalıştırın.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar
Performansı en iyi duruma getirmek için, büyük veritabanlarını boşaltırken şu hususlara dikkat edin:
-   Veritabanlarını `exclude-triggers` boşaltırken mysqldump seçeneğini kullanın. Veri geri yüklemesi sırasında tetikleyici komutlarının devreye sönmesini önlemek için tetikleyicileri döküm dosyalarından hariç tayın. 
-   İşlem `single-transaction` yalıtım modunu REPEATABLE READ olarak ayarlamak için seçeneği kullanın ve verileri boşaltmadan önce sunucuya start transaction SQL deyimi gönderir. Birçok tabloyu tek bir işlem içinde boşaltmak, geri yükleme sırasında bazı ekstra depolama alanının tüketilmesine neden olur. `single-transaction` LOCK TABLES `lock-tables` bekleyen tüm işlemlerin örtülü olarak işlenmesine neden olduğundan seçenek ve seçenek birbirini dışlar. Büyük tabloları dökümü için `single-transaction` seçeneği `quick` seçeneği birleştirin. 
-   Birkaç `extended-insert` DEĞER listesi içeren çok satırlı sözdizimini kullanın. Bu, daha küçük bir döküm dosyasıyla sonuçlanır ve dosya yeniden yüklendiğinde eklerin hıza uyrur.
-  Veritabanlarını `order-by-primary` boşaltırken mysqldump seçeneğini kullanın, böylece veriler birincil anahtar sırasına göre yazılır.
-   Verileri `disable-keys` boşaltırken mysqldump seçeneğini kullanarak yüklemeden önce yabancı anahtar kısıtlamalarını devre dışı bırakın. Yabancı anahtar denetimlerinin devre dışı bırakılması performans kazançları sağlar. Başvuru bütünlüğünü sağlamak için kısıtlamaları etkinleştirin ve yükten sonra verileri doğrulayın.
-   Uygun olduğunda bölümlenmiş tabloları kullanın.
-   Verileri paralel olarak yükleyin. Kaynak sınırına ulaşabilmek için çok fazla paralellikten kaçının ve Azure portalında bulunan ölçümleri kullanarak kaynakları izleyin. 
-   Veritabanlarını `defer-table-indexes` boşaltırken mysqlpump seçeneğini kullanın, böylece tablolar verileri yüklendikten sonra dizin oluşturma gerçekleşir.
-   Yedekleme dosyalarını Azure blob/deposuna kopyalayın ve geri yüklemeyi Internet'te gerçekleştirmekten çok daha hızlı olması gereken geri yüklemeyi buradan gerçekleştirin.

## <a name="create-a-backup-file"></a>Yedekleme dosyası oluşturma
Varolan bir MariaDB veritabanını yerel şirket içi sunucuda veya sanal bir makinede yedeklemek için mysqldump kullanarak aşağıdaki komutu çalıştırın: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

Sağlayabilecek parametreler şunlardır:
- [uname] Veritabanı kullanıcı adınız 
- [geçiş] Veritabanınızın şifresi (-p ile parola arasında boşluk olmadığını unutmayın) 
- [dbname] Veritabanınızın adı 
- [backupfile.sql] Veritabanı yedekleme için dosya adı 
- [--opt] Mysqldump seçeneği 

Örneğin, MariaDB sunucunuzda 'testuser' adlı bir veritabanını 'testuser' kullanıcı adı ile ve testdb_backup.sql dosyasının şifresi olmadan yedeklemek için aşağıdaki komutu kullanın. Komut, veritabanını `testdb` yeniden oluşturmak için `testdb_backup.sql`gereken tüm SQL deyimlerini içeren , adlı bir dosyaya veritabanını yedekler. 

```bash
$ mysqldump -u root -p testdb > testdb_backup.sql
```
Yedeklemek için veritabanınızdaki belirli tabloları seçmek için, boşluklara göre ayrılmış tablo adlarını listeleyin. Örneğin, 'testdb'den yalnızca tablo1 ve tablo2 tablolarını yedeklemek için aşağıdaki örneği izleyin: 
```bash
$ mysqldump -u root -p testdb table1 table2 > testdb_tables_backup.sql
```
Aynı anda birden fazla veritabanını yedeklemek için veritabanı anahtarını kullanın ve boşluklara göre ayrılmış veritabanı adlarını listeleyin. 
```bash
$ mysqldump -u root -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql 
```

## <a name="create-a-database-on-the-target-server"></a>Hedef sunucuda veritabanı oluşturma
Verileri geçirmek istediğiniz MariaDB sunucusu için hedef Azure Veritabanı'nda boş bir veritabanı oluşturun. Veritabanını oluşturmak için MySQL Workbench gibi bir araç kullanın. Veritabanı, boşatılmış verileri içeren veritabanıyla aynı ada sahip olabilir veya farklı bir ada sahip bir veritabanı oluşturabilirsiniz.

Bağlanmak için bağlantı bilgilerini MariaDB için Azure Veritabanınıza **Genel Bakış'ta** bulun.

![Azure portalında bağlantı bilgilerini bulma](./media/howto-migrate-dump-restore/1_server-overview-name-login.png)

Bağlantı bilgilerini MySQL Çalışma Tezgahınıza ekleyin.

![MySQL Çalışma Tezgahı Bağlantı Dizesi](./media/howto-migrate-dump-restore/2_setup-new-connection.png)

## <a name="restore-your-mariadb-database"></a>MariaDB veritabanınızı geri yükleme
Hedef veritabanını oluşturduktan sonra, verileri döküm dosyasından yeni oluşturulan belirli veritabanına geri yüklemek için mysql komutunu veya MySQL Workbench'i kullanabilirsiniz.
```bash
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
Bu örnekte, verileri MariaDB sunucusu için hedef Azure Veritabanı'nda yeni oluşturulan veritabanına geri yükleyin.
```bash
$ mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p testdb < testdb_backup.sql
```

## <a name="export-using-phpmyadmin"></a>PHPMyAdmin kullanarak dışa aktarma
Dışa aktarmak için, ortamınızda yerel olarak yüklemiş olabileceğiniz phpMyAdmin adlı ortak aracı kullanabilirsiniz. PHPMyAdmin kullanarak MariaDB veritabanınızı dışa aktarmak için:
1. PhpMyAdmin'i açın.
2. Veritabanınızı seçin. Soldaki listedeki veritabanı adını tıklatın. 
3. **Dışa Aktar** bağlantısını tıklatın. Veritabanı dökümünün görüntülenebilmek için yeni bir sayfa görüntülenir.
4. Dışa Aktarma alanında, veritabanınızdaki tabloları seçmek için **Tümünü Seç** bağlantısını tıklatın. 
5. SQL seçenekleri alanında, uygun seçenekleri tıklatın. 
6. Dosya **olarak Kaydet** seçeneğini ve ilgili sıkıştırma seçeneğini tıklatın ve ardından **Git** düğmesini tıklatın. Dosyayı yerel olarak kaydetmenizi isteyen bir iletişim kutusu görünmelidir.

## <a name="import-using-phpmyadmin"></a>PHPMyAdmin kullanarak alma
Veritabanınızı içe aktarma, dışa aktarmaya benzer. Aşağıdaki eylemleri yapın:
1. PhpMyAdmin'i açın. 
2. phpMyAdmin kurulum sayfasında, MariaDB sunucusu için Azure Veritabanınızı eklemek için **Ekle'yi** tıklatın. Bağlantı bilgilerini ve giriş bilgilerini sağlayın.
3. Uygun şekilde adlandırılmış bir veritabanı oluşturun ve ekranın solunda seçin. Varolan veritabanını yeniden yazmak için veritabanı adını tıklatın, tablo adlarının yanındaki tüm onay kutularını seçin ve varolan tabloları silmek için **Bırak'ı** seçin. 
4. SQL komutlarını yazabileceğiniz veya SQL dosyanızı yükleyebileceğiniz sayfayı göstermek için **SQL** bağlantısını tıklatın. 
5. Veritabanı dosyasını bulmak için **gözatma** düğmesini kullanın. 
6. Yedeklemeyi dışa aktarmak, SQL komutlarını yürütmek ve veritabanınızı yeniden oluşturmak için **Git** düğmesini tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
- [Uygulamaları MariaDB için Azure Veritabanı'na bağlayın.](./howto-connection-string.md)
 
<!--
- For more information about migrating databases to Azure Database for MariaDB, see the [Database Migration Guide](https://aka.ms/datamigration).
-->
