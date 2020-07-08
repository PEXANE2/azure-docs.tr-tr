---
title: Döküm ve geri yükleme kullanarak geçirme-MySQL için Azure veritabanı
description: Bu makalede, mysqldump, MySQL II ve PHPMyAdmin gibi araçları kullanarak MySQL için Azure veritabanınızdaki veritabanlarını yedeklemenin ve geri yüklemenin iki yaygın yolu açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 2/27/2020
ms.openlocfilehash: c30faa31f6f733f80d4bfd5184c09d9fdbd6f389
ms.sourcegitcommit: f684589322633f1a0fafb627a03498b148b0d521
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85971190"
---
# <a name="migrate-your-mysql-database-to-azure-database-for-mysql-using-dump-and-restore"></a>Döküm alma ve geri yükleme işlemlerini kullanarak MySQL veritabanınızı MySQL için Azure Veritabanı'na geçirme
Bu makalede, MySQL için Azure veritabanınızdaki veritabanlarını yedeklemenin ve geri yüklemenin iki yaygın yolu açıklanmaktadır
- Komut satırından dökümünü al ve geri yükle (mysqldump kullanarak) 
- PHPMyAdmin kullanarak döküm ve geri yükleme 

## <a name="before-you-begin"></a>Başlamadan önce
Bu nasıl yapılır kılavuzunda ilerlemek için şunları yapmanız gerekir:
- [MySQL için Azure veritabanı sunucusu oluşturma-Azure portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- bir makineye yüklü [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) komut satırı yardımcı programı.
- MySQL çalışma ekranı [MySQL araç yüklemesi](https://dev.mysql.com/downloads/workbench/) veya başka bir üçüncü taraf MySQL Aracı, döküm ve geri yükleme komutlarını sağlar.

Veritabanı boyutlarına sahip büyük veritabanlarını 1 ' den fazla TBs 'ye geçirmek istiyorsanız, paralel dışarı ve içeri aktarmayı destekleyen mydumper/myloader gibi topluluk araçlarını kullanmayı düşünebilirsiniz. Paralel döküm ve geri yükleme, büyük veritabanlarının geçiş süresini önemli ölçüde azaltmaya yardımcı olabilir. Mydumper/myloader araçlarını kullanarak büyük veritabanlarını MySQL için Azure veritabanı hizmetine geçirmek için en iyi yöntemler için [techcommunity blogumuza](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/best-practices-for-migrating-large-databases-to-azure-database/ba-p/1362699) başvurabilirsiniz.

## <a name="use-common-tools"></a>Ortak araçları kullanma
MySQL için Azure veritabanı 'na uzaktan bağlanmak ve veri geri yüklemek için MySQL çalışma ekranı veya mysqldump gibi ortak yardımcı programları ve araçları kullanın. MySQL için Azure veritabanı 'na bağlanmak üzere bir internet bağlantısı ile istemci makinenizde bu tür araçları kullanın. En iyi güvenlik uygulamaları için SSL şifreli bir bağlantı kullanın. Ayrıca bkz. [MySQL Için Azure veritabanı 'NDA SSL bağlantısını yapılandırma](concepts-ssl-connection-security.md). MySQL için Azure veritabanı 'na geçiş yaparken döküm dosyalarını herhangi bir özel bulut konumuna taşımanız gerekmez. 

## <a name="common-uses-for-dump-and-restore"></a>Döküm ve geri yükleme için ortak kullanımlar
Birçok yaygın senaryoda veritabanlarını bir Azure MySQL veritabanına dökmek ve yüklemek için mysqldump ve mysqlpump gibi MySQL yardımcı programlarını kullanabilirsiniz. Diğer senaryolarda, bunun yerine [içeri ve dışarı aktarma](concepts-migrate-import-export.md) yaklaşımını kullanabilirsiniz.

- Tüm veritabanını geçirirken veritabanı dökümlerini kullanın. Bu öneri, büyük miktarda MySQL verisi taşırken veya canlı siteler veya uygulamalar için hizmet kesintisini en aza indirmek istediğinizde geçerlidir. 
-  MySQL için Azure Veritabanı'na veri yüklerken veritabanındaki tüm tabloların InnoDB depolama altyapısını kullandığından emin olun. MySQL için Azure veritabanı yalnızca InnoDB depolama altyapısını destekler ve bu nedenle alternatif depolama altyapılarını desteklemez. Tablolarınız diğer depolama altyapılarıyla yapılandırıldıysa, MySQL için Azure veritabanı 'na geçişten önce bunları InnoDB Engine biçimine dönüştürün.
   Örneğin, MyISAM tablolarını kullanarak bir WordPress veya WebApp varsa, önce MySQL için Azure veritabanı 'na geri yüklemeden önce bu tabloları InnoDB biçimine geçirerek dönüştürün. `ENGINE=InnoDB`Yeni bir tablo oluştururken kullanılan altyapıyı ayarlamak için yan tümcesini kullanın, ardından geri yüklemeden önce verileri uyumlu tabloya aktarın. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- Uyumluluk sorunlarından kaçınmak için veritabanlarının dökümünü alırken kaynak ve hedef sistemlerde aynı MySQL sürümünün bulunduğundan emin olun. Örneğin, var olan MySQL sunucunuz sürüm 5,7 ise, sürüm 5,7 ' yi çalıştırmak için yapılandırılmış MySQL için Azure veritabanı 'na geçiş yapmanız gerekir. `mysql_upgrade`Komut, MySQL Için Azure veritabanı sunucusunda çalışmaz ve desteklenmez. MySQL sürümleri arasında yükseltmeniz gerekiyorsa, ilk olarak kendi ortamınızda daha yüksek bir MySQL sürümüne daha düşük sürüm dökümünü alın veya dışarı aktarın. Ardından `mysql_upgrade` , MySQL Için Azure veritabanı 'na geçişi denemeden önce çalıştırın.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar
Performansı iyileştirmek için, büyük veritabanlarının dökümünü yaparken aşağıdaki noktalara dikkat edin:
-   `exclude-triggers`Veritabanları dökümünü alırken mysqldump içindeki seçeneğini kullanın. Veri geri yükleme sırasında tetikleyici komutlarının tetiklemesini önlemek için Tetikleyicileri döküm dosyalarından hariç tutun. 
-   `single-transaction`Veri dökümünü yapmadan, işlem yalıtım modunu YINELENEBILIR okuma olarak ayarlama ve sunucuya BIR başlangıç IŞLEMI SQL açıklaması gönderme seçeneğini kullanın. Tek bir işlem içindeki birçok tablo dökümünü almak, bazı ek depolamanın geri yükleme sırasında tüketilmesine neden olur. `single-transaction` `lock-tables` Kilitleme tabloları bekleyen işlemlerin örtük olarak kaydedilmesine neden olduğundan, seçeneği ve seçeneği birbirini dışlıyor. Büyük tabloların dökümünü yapmak için `single-transaction` seçeneğini `quick` seçeneğiyle birleştirin. 
-   `extended-insert`ÇEŞITLI değer listeleri içeren birden çok satırlık sözdizimini kullanın. Bu, daha küçük bir döküm dosyası ile sonuçlanır ve dosya yeniden yüklendiğinde daha fazla hızlanır.
-  `order-by-primary`Verileri birincil anahtar sırasıyla betikleştirilmiş hale getirmek için veritabanları dökümünü yaparken mysqldump içindeki seçeneğini kullanın.
-   `disable-keys`Verileri dökümünde, yüklemeden önce yabancı anahtar kısıtlamalarını devre dışı bırakmak için mysqldump içindeki seçeneğini kullanın. Yabancı anahtar denetimlerini devre dışı bırakmak, performans artışı sağlar. Kısıtlamaları etkinleştirin ve bilgi tutarlılığı sağlamak için yüklemeden sonra verileri doğrulayın.
-   Uygun olduğunda bölümlenmiş tabloları kullanın.
-   Verileri paralel olarak yükleyin. Kaynak sınırına ulaşmanıza ve Azure portal bulunan ölçümleri kullanarak kaynakları izlemenize neden olacak çok fazla paralellik yapmaktan kaçının. 
-   `defer-table-indexes`Tablo verileri yüklendikten sonra Dizin oluşturma işleminin gerçekleşmemesi için, veritabanları dökümünü alırken mysqlpump içindeki seçeneğini kullanın.
-   `skip-definer`Görünümler ve saklı yordamlar için oluşturma deyimlerinden definer ve SQL SECURITY yan tümcelerini atlamak için mysqlpump içindeki seçeneğini kullanın.  Döküm dosyasını yeniden yüklediğinizde, varsayılan DEFINER ve SQL GÜVENLIK değerlerini kullanan nesneler oluşturulur.
-   Yedekleme dosyalarını bir Azure Blob 'una/deposuna kopyalayın ve geri yükleme işlemini Internet 'te gerçekleştirmekten daha hızlı bir şekilde gerçekleştirin.

## <a name="create-a-backup-file-from-the-command-line-using-mysqldump"></a>Mysqldump kullanarak komut satırından bir yedekleme dosyası oluşturma
Yerel Şirket içi sunucuda veya bir sanal makinede var olan bir MySQL veritabanını yedeklemek için aşağıdaki komutu çalıştırın: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

Sağlanacak parametreler şunlardır:
- [uname] Veritabanınızın Kullanıcı adı 
- aktar Veritabanınızın parolası (-p ile parola arasında boşluk yok) 
- dbname Veritabanınızın adı 
- [BackupFile. SQL] veritabanı yedeklemenizin dosya adı 
- [--opt] Mysqldump seçeneği 

Örneğin, MySQL sunucunuzdaki ' TestDB ' adlı bir veritabanını ' testuser ' Kullanıcı adı ile ve testdb_backup. SQL dosyasına parolasız yedeklemek için aşağıdaki komutu kullanın. Komutu, veritabanını `testdb` `testdb_backup.sql` yeniden oluşturmak için gereken tüm SQL deyimlerini içeren adlı bir dosyaya veritabanını yedekler. ' Testuser ' Kullanıcı adının en azından dökülebilir tablolar için SEÇIM ayrıcalığına sahip olduğundan, dökülebilir görünümler için Görünüm ' ün ve--Single-Transaction seçeneği kullanılmazsa TABLOLARı KILITLEYECEĞINDEN emin olun.

```bash
GRANT SELECT, LOCK TABLES, SHOW VIEW ON *.* TO 'testuser'@'hostname' IDENTIFIED BY 'password';
```

```bash
$ mysqldump -u root -p testdb > testdb_backup.sql
```
Veritabanınızdaki belirli tabloları seçerek, yedeklenecek tablo adlarını boşluklarla ayırarak listeleyin. Örneğin, ' TestDB ' öğesinden yalnızca Table1 ve Table2 tablolarını yedeklemek için şu örneği izleyin: 
```bash
$ mysqldump -u root -p testdb table1 table2 > testdb_tables_backup.sql
```
Aynı anda birden fazla veritabanını yedeklemek için--Database anahtarını kullanın ve boşluklarla ayrılmış veritabanı adlarını listeleyin. 
```bash
$ mysqldump -u root -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql 
```

## <a name="create-a-database-on-the-target-azure-database-for-mysql-server"></a>MySQL için Azure veritabanı sunucusu için hedef veritabanı oluşturma
Verileri geçirmek istediğiniz MySQL için hedef Azure veritabanı sunucusunda boş bir veritabanı oluşturun. Veritabanını oluşturmak için MySQL çalışma ekranı gibi bir araç kullanın. Veritabanı, dökülebilir verileri içeren veritabanıyla aynı ada sahip olabilir veya farklı bir ada sahip bir veritabanı oluşturabilirsiniz.

Bağlanmak için, MySQL için Azure veritabanı **'Na genel bakış** bölümündeki bağlantı bilgilerini bulun.

![Azure portal bağlantı bilgilerini bulun](./media/concepts-migrate-dump-restore/1_server-overview-name-login.png)

Bağlantı bilgilerini MySQL çalışma ekranına ekleyin.

![MySQL çalışma ekranı bağlantı dizesi](./media/concepts-migrate-dump-restore/2_setup-new-connection.png)

## <a name="preparing-the-target-azure-database-for-mysql-server-for-fast-data-loads"></a>Hızlı veri yükleri için MySQL için Azure veritabanı sunucusu için hedef hazırlama
Daha hızlı veri yükleri için hedef Azure veritabanını MySQL sunucusuna hazırlamak üzere aşağıdaki sunucu parametreleri ve yapılandırmasının değiştirilmesi gerekir.
- max_allowed_packet – uzun satırlardan kaynaklanan taşma sorununa engel olmak için 1073741824 (ör. 1GB) olarak ayarlayın.
- slow_query_log: yavaş sorgu günlüğünü kapatmak için kapalı olarak ayarlayın. Bu, veri yükleri sırasında yavaş sorgu günlüğü 'nün neden olduğu yükü ortadan kaldırır.
- query_store_capture_mode – sorgu deposunu kapatmak için NONE olarak ayarlayın. Bu, sorgu deposu tarafından örnekleme etkinliklerinin neden olduğu yükü ortadan kaldırır.
- innodb_buffer_pool_size: innodb_buffer_pool_size artırmak için, geçiş sırasında portalın fiyatlandırma katmanından 32 sanal çekirdek bellek için Iyileştirilmiş SKU 'SU ölçeğini artırın. Innodb_buffer_pool_size, yalnızca MySQL için Azure veritabanı sunucusu için işlem ölçeklendirerek artırılabilir.
- innodb_io_capacity & innodb_io_capacity_max-geçiş hızını iyileştirmek üzere GÇ kullanımını iyileştirmek için Azure portal sunucu parametrelerinden 9000 olarak değiştirin.
- innodb_write_io_threads & innodb_write_io_threads-geçiş hızını artırmak için Azure portal içindeki sunucu parametrelerinden 4 ' e geçin.
- Depolama katmanını ölçeklendirin: MySQL için Azure veritabanı sunucusu için IOPS, depolama katmanındaki artışla aşamalı olarak artar. Daha hızlı yüklemeler için, sağlanan IOPS 'yi artırmak üzere depolama katmanını artırmak isteyebilirsiniz. Lütfen depolamanın yalnızca ölçeği küçültüleceğini unutmayın.

Geçiş tamamlandıktan sonra, sunucu parametreleri ve işlem katmanı yapılandırmasını önceki değerlerine geri döndürebilirsiniz. 

## <a name="restore-your-mysql-database-using-command-line-or-mysql-workbench"></a>Komut satırı veya MySQL çalışma ekranı kullanarak MySQL veritabanınızı geri yükleme
Hedef veritabanını oluşturduktan sonra, verileri döküm dosyasından yeni oluşturulan belirli veritabanına geri yüklemek için MySQL komutunu veya MySQL çalışma ekranı ' nı kullanabilirsiniz.
```bash
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
Bu örnekte, verileri yeni oluşturulan veritabanına MySQL için Azure veritabanı sunucusuna geri yükleyin.
```bash
$ mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p testdb < testdb_backup.sql
```
## <a name="export-using-phpmyadmin"></a>PHPMyAdmin kullanarak dışarı aktarma
Dışarı aktarmak için, ortamınızda yerel olarak zaten yüklü olabilecek olan phpMyAdmin adlı ortak aracı kullanabilirsiniz. PHPMyAdmin kullanarak MySQL veritabanınızı dışarı aktarmak için:
1. PhpMyAdmin öğesini açın.
2. Veritabanınızı seçin. Sol taraftaki listede veritabanı adına tıklayın. 
3. **Dışarı aktarma** bağlantısına tıklayın. Veritabanının dökümünü görüntülemek için yeni bir sayfa görüntülenir.
4. Dışarı aktarma alanında, veritabanınızdaki tabloları seçmek için **Tümünü Seç** bağlantısına tıklayın. 
5. SQL seçenekleri alanında uygun seçeneklere tıklayın. 
6. **Dosya olarak kaydet** seçeneğine ve ilgili sıkıştırma seçeneğine tıklayın ve sonra **Git** düğmesine tıklayın. Dosyayı yerel olarak kaydetmenizi isteyen bir iletişim kutusu görüntülenmelidir.

## <a name="import-using-phpmyadmin"></a>PHPMyAdmin kullanarak içeri aktarma
Veritabanınızı içeri aktarma işlemi dışa aktarmaya benzer. Aşağıdaki eylemleri yapın:
1. PhpMyAdmin öğesini açın. 
2. MySQL için Azure veritabanı sunucunuzu eklemek için phpMyAdmin kurulum sayfasında **Ekle** ' ye tıklayın. Bağlantı ayrıntılarını ve oturum açma bilgilerini girin.
3. Uygun şekilde adlandırılmış bir veritabanı oluşturun ve ekranın sol tarafında seçin. Var olan veritabanını yeniden yazmak için veritabanı adına tıklayın, tablo adlarının yanındaki tüm onay kutularını seçin ve var olan tabloları silmek için **bırak** ' ı seçin. 
4. SQL komutlarına ekleyebileceğiniz sayfayı göstermek için **SQL** bağlantısına tıklayın veya SQL dosyanızı karşıya yükleyin. 
5. Veritabanı dosyasını bulmak için **Araştır** düğmesini kullanın. 
6. Yedeklemeyi dışarı aktarmak, SQL komutlarını yürütmek ve veritabanınızı yeniden oluşturmak için **Git** düğmesine tıklayın.

## <a name="known-issues"></a>Bilinen Sorunlar
Bilinen sorunlar, ipuçları ve püf noktaları için [techcommunity blogumuza](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/tips-and-tricks-in-using-mysqldump-and-mysql-restore-to-azure/ba-p/916912)bakmanız önerilir.

## <a name="next-steps"></a>Sonraki adımlar
- [Uygulamaları MySQL Için Azure veritabanı 'Na bağlayın](./howto-connection-string.md).
- MySQL için Azure veritabanı 'na veritabanı geçirme hakkında daha fazla bilgi için bkz. [veritabanı geçiş kılavuzu](https://aka.ms/datamigration).
