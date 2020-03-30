---
title: Dökümü ve geri yüklemeyi kullanarak geçirin - MySQL için Azure Veritabanı
description: Bu makalede, mysqldump, MySQL Workbench ve PHPMyAdmin gibi araçları kullanarak MySQL için Azure Veritabanınızdaki veritabanlarını yedeklemenin ve geri yüklemenin iki yaygın yolu açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 2/27/2020
ms.openlocfilehash: b15da2aa83231bfdc8732995888349b06ab56d15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78163786"
---
# <a name="migrate-your-mysql-database-to-azure-database-for-mysql-using-dump-and-restore"></a>Döküm alma ve geri yükleme işlemlerini kullanarak MySQL veritabanınızı MySQL için Azure Veritabanı'na geçirme
Bu makalede, MySQL için Azure Veritabanınızdaki veritabanlarını yedeklemenin ve geri yüklemenin iki yaygın yolu açıklanmaktadır
- Komut satırından dökümü ve geri yükleme (mysqldump kullanarak) 
- PHPMyAdmin kullanarak dökümü ve geri yükleme 

## <a name="before-you-begin"></a>Başlamadan önce
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- [MySQL sunucusu için Azure Veritabanı Oluşturma - Azure portalı](quickstart-create-mysql-server-database-using-azure-portal.md)
- [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) komut satırı yardımcı programı bir makinede yüklü.
- MySQL Workbench [MySQL Workbench İndir](https://dev.mysql.com/downloads/workbench/) veya başka bir üçüncü taraf MySQL aracı dökümü yapmak ve komutları geri yükleyin.

## <a name="use-common-tools"></a>Ortak araçları kullanma
MySQL Workbench veya mysqldump gibi ortak yardımcı programları ve araçları kullanarak mySQL için verileri Azure Veritabanı'na uzaktan bağlayın ve geri yükleyin. MySQL için Azure Veritabanı'na bağlanmak için internet bağlantısı olan istemci makinenizde bu tür araçları kullanın. En iyi güvenlik uygulamaları için SSL şifreli bir bağlantı kullanın, [ayrıca MySQL için Azure Veritabanı'nda SSL bağlantısını yapılandırın.'a](concepts-ssl-connection-security.md)bakın. MySQL için Azure Veritabanı'na geçerken döküm dosyalarını özel bulut konumuna taşımanız gerekmez. 

## <a name="common-uses-for-dump-and-restore"></a>Dökümü ve geri yüklemeiçin yaygın kullanımalanları
Çeşitli yaygın senaryolarda veritabanlarını Azure MySQL Veritabanına boşaltmak ve yüklemek için mysqldump ve mysqlpump gibi MySQL yardımcı hizmetlerini kullanabilirsiniz. Diğer senaryolarda, bunun yerine [İçe Aktar ve Dışa Aktar](concepts-migrate-import-export.md) yaklaşımını kullanabilirsiniz.

- Tüm veritabanını geçirerken veritabanı dökümlerini kullanın. Bu öneri, büyük miktarda MySQL verisini taşınırken veya canlı siteler veya uygulamalar için hizmet kesintisini en aza indirmek istediğinizde geçerlidir. 
-  MySQL için Azure Veritabanı'na veri yüklerken veritabanındaki tüm tabloların InnoDB depolama altyapısını kullandığından emin olun. MySQL için Azure Veritabanı yalnızca InnoDB Depolama altyapılarını destekler ve bu nedenle alternatif depolama altyapılarını desteklemez. Tablolarınız diğer depolama motorları ile yapılandırıldıysa, MySQL için Azure Veritabanı'na geçmeden önce bunları InnoDB altyapı biçimine dönüştürün.
   Örneğin, MyISAM tablolarını kullanan bir WordPress veya WebApp'ınuz varsa, MySQL için Azure Veritabanı'na geri dönmeden önce bu tabloları InnoDB biçimine geçirerek dönüştürün. Yeni bir `ENGINE=InnoDB` tablo oluştururken kullanılan motoru ayarlamak için yan tümceyi kullanın ve sonra geri yüklemeden önce verileri uyumlu tabloya aktarın. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- Uyumluluk sorunlarından kaçınmak için veritabanlarının dökümünü alırken kaynak ve hedef sistemlerde aynı MySQL sürümünün bulunduğundan emin olun. Örneğin, varolan MySQL sunucunuz sürüm 5.7 ise, sürüm 5.7'yi çalıştıracak şekilde yapılandırılan MySQL için Azure Veritabanı'na geçiş niz gerekir. Komut `mysql_upgrade` MySQL sunucusu için bir Azure Veritabanında çalışmaz ve desteklenmez. MySQL sürümlerinde yükseltme yapmanız gerekiyorsa, önce alt sürüm veritabanınızı kendi ortamınızda MySQL'in daha yüksek bir sürümüne boşaltın veya dışa aktarın. Ardından, `mysql_upgrade`MySQL için bir Azure Veritabanına geçiş denemeden önce çalıştırın.

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
-   Görünümler `skip-definer` ve depolanan yordamlar için create deyimlerinden tanımlayıcı yı ve SQL SECURITY yan tümcelerini atlamak için mysqlpump'daki seçeneği kullanın.  Döküm dosyasını yeniden yüklediğinizde, varsayılan DEFINER ve SQL SECURITY değerlerini kullanan nesneler oluşturur.
-   Yedekleme dosyalarını Azure blob/deposuna kopyalayın ve geri yüklemeyi Internet'te gerçekleştirmekten çok daha hızlı olması gereken geri yüklemeyi buradan gerçekleştirin.

## <a name="create-a-backup-file-from-the-command-line-using-mysqldump"></a>mysqldump kullanarak komut satırından yedek dosya oluşturma
Varolan bir MySQL veritabanını yerel şirket içi sunucuda veya sanal bir makinede yedeklemek için aşağıdaki komutu çalıştırın: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

Sağlayabilecek parametreler şunlardır:
- [uname] Veritabanı kullanıcı adınız 
- [geçiş] Veritabanınızın şifresi (-p ile parola arasında boşluk olmadığını unutmayın) 
- [dbname] Veritabanınızın adı 
- [backupfile.sql] Veritabanı yedekleme için dosya adı 
- [--opt] Mysqldump seçeneği 

Örneğin, MySQL sunucunuzda 'testuser' adlı bir veritabanını 'testuser' kullanıcı adı ile ve testdb_backup.sql dosyasının şifresi olmadan yedeklemek için aşağıdaki komutu kullanın. Komut, veritabanını `testdb` yeniden oluşturmak için `testdb_backup.sql`gereken tüm SQL deyimlerini içeren , adlı bir dosyaya veritabanını yedekler. 

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

## <a name="create-a-database-on-the-target-azure-database-for-mysql-server"></a>MySQL sunucusu için hedef Azure Veritabanı'nda bir veritabanı oluşturma
Verileri geçirmek istediğiniz MySQL sunucusu için hedef Azure Veritabanı'nda boş bir veritabanı oluşturun. Veritabanını oluşturmak için MySQL Workbench gibi bir araç kullanın. Veritabanı, boşatılmış verileri içeren veritabanıyla aynı ada sahip olabilir veya farklı bir ada sahip bir veritabanı oluşturabilirsiniz.

Bağlanmak için MySQL için Azure Veritabanınızın **Genel Görünümü'ndeki** bağlantı bilgilerini bulun.

![Azure portalında bağlantı bilgilerini bulma](./media/concepts-migrate-dump-restore/1_server-overview-name-login.png)

Bağlantı bilgilerini MySQL Çalışma Tezgahınıza ekleyin.

![MySQL Çalışma Tezgahı Bağlantı Dizesi](./media/concepts-migrate-dump-restore/2_setup-new-connection.png)


## <a name="restore-your-mysql-database-using-command-line-or-mysql-workbench"></a>Komut satırı veya MySQL Workbench kullanarak MySQL veritabanınızı geri yükleme
Hedef veritabanını oluşturduktan sonra, verileri döküm dosyasından yeni oluşturulan belirli veritabanına geri yüklemek için mysql komutunu veya MySQL Workbench'i kullanabilirsiniz.
```bash
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
Bu örnekte, MySQL sunucusu için hedef Azure Veritabanı'nda yeni oluşturulan veritabanına verileri geri yükleyin.
```bash
$ mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p testdb < testdb_backup.sql
```

## <a name="export-using-phpmyadmin"></a>PHPMyAdmin kullanarak dışa aktarma
Dışa aktarmak için, ortamınızda yerel olarak yüklemiş olabileceğiniz phpMyAdmin adlı ortak aracı kullanabilirsiniz. PHPMyAdmin kullanarak MySQL veritabanınızı dışa aktarmak için:
1. PhpMyAdmin'i açın.
2. Veritabanınızı seçin. Soldaki listedeki veritabanı adını tıklatın. 
3. **Dışa Aktar** bağlantısını tıklatın. Veritabanı dökümünün görüntülenebilmek için yeni bir sayfa görüntülenir.
4. Dışa Aktarma alanında, veritabanınızdaki tabloları seçmek için **Tümünü Seç** bağlantısını tıklatın. 
5. SQL seçenekleri alanında, uygun seçenekleri tıklatın. 
6. Dosya **olarak Kaydet** seçeneğini ve ilgili sıkıştırma seçeneğini tıklatın ve ardından **Git** düğmesini tıklatın. Dosyayı yerel olarak kaydetmenizi isteyen bir iletişim kutusu görünmelidir.

## <a name="import-using-phpmyadmin"></a>PHPMyAdmin kullanarak alma
Veritabanınızı içe aktarma, dışa aktarmaya benzer. Aşağıdaki eylemleri yapın:
1. PhpMyAdmin'i açın. 
2. phpMyAdmin kurulum sayfasında, MySQL sunucusu için Azure Veritabanınızı eklemek için **Ekle'yi** tıklatın. Bağlantı bilgilerini ve giriş bilgilerini sağlayın.
3. Uygun şekilde adlandırılmış bir veritabanı oluşturun ve ekranın solunda seçin. Varolan veritabanını yeniden yazmak için veritabanı adını tıklatın, tablo adlarının yanındaki tüm onay kutularını seçin ve varolan tabloları silmek için **Bırak'ı** seçin. 
4. SQL komutlarını yazabileceğiniz veya SQL dosyanızı yükleyebileceğiniz sayfayı göstermek için **SQL** bağlantısını tıklatın. 
5. Veritabanı dosyasını bulmak için **gözatma** düğmesini kullanın. 
6. Yedeklemeyi dışa aktarmak, SQL komutlarını yürütmek ve veritabanınızı yeniden oluşturmak için **Git** düğmesini tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
- [MySQL için uygulamaları Azure Veritabanı'na bağlayın.](./howto-connection-string.md)
- Veritabanlarını MySQL için Azure Veritabanına geçirme hakkında daha fazla bilgi için [Veritabanı Geçiş Kılavuzu'na](https://aka.ms/datamigration)bakın.
