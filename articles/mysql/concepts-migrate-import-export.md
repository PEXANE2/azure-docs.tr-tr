---
title: İçe aktarma ve dışa aktarma - MySQL için Azure Veritabanı
description: Bu makalede, MySQL Workbench gibi araçları kullanarak MySQL için Azure Veritabanı'nda veritabanlarını alma ve dışa aktarma ortak yolları açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 2/27/2020
ms.openlocfilehash: 83b0a69e063e9427c726216ef873f5a1c97f9582
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78163735"
---
# <a name="migrate-your-mysql-database-by-using-import-and-export"></a>Alma ve dışa aktarma kullanarak MySQL veritabanınızı geçirin
Bu makalede, MySQL Workbench kullanarak MySQL sunucusu için bir Azure Veritabanına veri alma ve dışa aktarma için iki yaygın yaklaşım açıklanmaktadır. 

## <a name="before-you-begin"></a>Başlamadan önce
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- [Azure portalı kullanarak MySQL sunucusu için Bir Azure Veritabanı Oluştur'u](quickstart-create-mysql-server-database-using-azure-portal.md)izleyerek MySQL sunucusu için bir Azure Veritabanı.
- MySQL Workbench [MySQL Workbench İndir](https://dev.mysql.com/downloads/workbench/) veya başka bir üçüncü taraf MySQL aracı alma /dışa aktarma yapmak için.

## <a name="use-common-tools"></a>Ortak araçları kullanma
MySQL Workbench veya mysqldump gibi ortak yardımcı programları ve araçları kullanarak mySQL için verileri Uzaktan bağlayın ve azure veritabanına aktarın veya dışa aktarın. 

MySQL için Azure Veritabanı'na bağlanmak için Internet bağlantısı olan istemci makinenizde bu tür araçları kullanın. MySQL için [Azure Veritabanı'nda SSL bağlantıverimliliğini yapılandırın'da](concepts-ssl-connection-security.md)açıklandığı gibi, en iyi güvenlik uygulamaları için SSL şifreli bir bağlantı kullanın.

MySQL için Azure Veritabanı'na geçerken alma ve dışa aktarma dosyalarınızı özel bulut konumuna taşımanız gerekmez. 

## <a name="create-a-database-on-the-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure Veritabanı'nda veritabanı oluşturma
Verileri geçirmek istediğiniz MySQL sunucusu için Azure Veritabanı'nda boş bir veritabanı oluşturun. Veritabanını oluşturmak için MySQL Workbench, Toad veya Navicat gibi bir araç kullanın. Veritabanı, boşatılmış verileri içeren veritabanıyla aynı ada sahip olabilir veya farklı bir ada sahip bir veritabanı oluşturabilirsiniz.

Bağlanmak için MySQL için Azure Veritabanınızın **Genel Görünümü'ndeki** bağlantı bilgilerini bulun.

![Azure portalında bağlantı bilgilerini bulma](./media/concepts-migrate-import-export/1_server-overview-name-login.png)

Bağlantı bilgilerini MySQL Workbench'e ekleyin.

![MySQL Çalışma Tezgahı bağlantı dizesi](./media/concepts-migrate-import-export/2_setup-new-connection.png)

## <a name="determine-when-to-use-import-and-export-techniques-instead-of-a-dump-and-restore"></a>Bir döküm ve geri yükleme yerine alma ve dışa aktarma tekniklerini ne zaman kullanacağımı belirleyin
Aşağıdaki senaryolarda veritabanlarını Azure MySQL Veritabanı'na aktarmak ve dışa aktarmak için MySQL araçlarını kullanın. Diğer senaryolarda, bunun yerine [dökümü ve geri yükleme](concepts-migrate-dump-restore.md) yaklaşımı kullanarak yararlanabilir. 

- Varolan bir MySQL veritabanından Azure MySQL Veritabanına aktarmak için seçici olarak birkaç tablo seçmeniz gerektiğinde, içe aktarma ve verme tekniğini kullanmak en iyisidir.  Bunu yaparak, zaman ve kaynak tasarrufu için geçişten gereksiz tabloları atlayabilirsiniz. Örneğin, `--include-tables` mysqlpump `--exclude-tables` ile veya anahtarı `--tables` ve [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html#option_mysqldump_tables)ile geçiş kullanın. [mysqlpump](https://dev.mysql.com/doc/refman/5.7/en/mysqlpump.html#option_mysqlpump_include-tables)
- Veritabanı nesnelerini tablolar dışında hareket ettirirken, bu nesneleri açıkça oluşturun. Kısıtlamaları (birincil anahtar, yabancı anahtar, dizinler), görünümler, işlevler, yordamlar, tetikleyiciler ve geçirmek istediğiniz diğer veritabanı nesnelerini ekleyin.
- MySQL veritabanı dışındaki dış veri kaynaklarından veri aktarırken, düz dosyalar oluşturun ve mysqlimport kullanarak bunları [aktarın.](https://dev.mysql.com/doc/refman/5.7/en/mysqlimport.html)

MySQL için Azure Veritabanı'na veri yüklerken veritabanındaki tüm tabloların InnoDB depolama motorlarını kullandığından emin olun. MySQL için Azure Veritabanı yalnızca InnoDB depolama motorlarını destekler, bu nedenle alternatif depolama altyapılarını desteklemez. Tablolarınız alternatif depolama altyapısı gerektiriyorsa, MySQL için Azure Veritabanı'na geçişten önce bunları InnoDB altyapı biçimini kullanacak şekilde dönüştürdüğünüzden emin olun. 

Örneğin, MyISAM altyapısını kullanan bir WordPress veya web uygulamanız varsa, önce verileri InnoDB tablolarına geçirerek tabloları dönüştürün. Ardından MySQL için Azure Veritabanı'na geri yükleyin. Tablo oluşturmak `ENGINE=INNODB` için motoru ayarlamak ve geçişten önce verileri uyumlu tabloya aktarmak için yan tümceyi kullanın. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```

## <a name="performance-recommendations-for-import-and-export"></a>İthalat ve ihracat için performans önerileri
-   Verileri yüklemeden önce kümelenmiş dizinler ve birincil anahtarlar oluşturun. Verileri birincil anahtar sırasına göre yükleyin. 
-   İkincil dizinlerin oluşturulmasını veriler yüklenene kadar geciktirin. Yükledikten sonra tüm ikincil dizinleri oluşturun. 
-   Yüklemeden önce yabancı anahtar kısıtlamalarını devre dışı kılabilir. Yabancı anahtar denetimlerinin devre dışı bırakılması önemli performans kazançları sağlar. Başvuru bütünlüğünü sağlamak için kısıtlamaları etkinleştirin ve yükten sonra verileri doğrulayın.
-   Verileri paralel olarak yükleyin. Kaynak sınırına ulaşabilmek için çok fazla paralellikten kaçının ve Azure portalında bulunan ölçümleri kullanarak kaynakları izleyin. 
-   Uygun olduğunda bölümlenmiş tabloları kullanın.

## <a name="import-and-export-by-using-mysql-workbench"></a>MySQL Workbench kullanarak alma ve dışa aktarma
MySQL Workbench'te veri aktarmanın ve almanın iki yolu vardır. Her biri farklı bir amaca hizmet ediyor. 

### <a name="table-data-export-and-import-wizards-from-the-object-browsers-context-menu"></a>Nesne tarayıcısının bağlam menüsünden tablo veri dışa aktarma ve alma sihirbazları
![Nesne tarayıcısının bağlam menüsünde MySQL Çalışma Tezgahı sihirbazları](./media/concepts-migrate-import-export/p1.png)

Tablo verileri sihirbazları CSV ve JSON dosyalarını kullanarak alma ve dışa aktarma işlemlerini destekler. Ayırıcılar, sütun seçimi ve kodlama seçimi gibi çeşitli yapılandırma seçenekleri içerir. Her sihirbazı yerel veya uzaktan bağlı MySQL sunucularına karşı gerçekleştirebilirsiniz. Alma eylemi tablo, sütun ve tür eşleme içerir. 

Bu sihirbazlara nesne tarayıcısının bağlam menüsünden bir tabloyu sağ tıklatarak erişebilirsiniz. Sonra **Tablo Veri Dışa Aktarma Sihirbazı** veya **Tablo Veri Alma Sihirbazı'nı**seçin. 

#### <a name="table-data-export-wizard"></a>Tablo Veri Dışa Aktarma Sihirbazı
Aşağıdaki örnek tabloyu bir CSV dosyasına aktarıyor: 
1. Dışa aktarılmak üzere veritabanıtablosuna sağ tıklayın. 
2. **Tablo Veri Dışa Aktarma Sihirbazı'nı**seçin. Dışa aktak edilecek sütunları seçin, satır ofset (varsa) ve say (varsa). 
3. **Dışa aktarma** sayfası için veri seç'te **İleri'yi**tıklatın. Dosya yolunu, CSV'yi veya JSON dosya türünü seçin. Ayrıca satır ayırıcısını, dizeleri toplama yöntemini ve alan ayırıcısını seçin. 
4. Çıktı **dosyası konum** ını seç sayfasında **İleri'yi**tıklatın. 
5. **Dışa Aktarma verileri** sayfasında **İleri'yi**tıklatın.

#### <a name="table-data-import-wizard"></a>Tablo Verileri Alma Sihirbazı
Aşağıdaki örnek tabloyu bir CSV dosyasından içeri aktarıyor:
1. İçe aktarılacak veritabanı tablosuna sağ tıklayın. 
2. İçe aktarılacak CSV dosyasına göz atın ve seçin ve sonra **İleri'yi**tıklatın. 
3. Hedef tabloyu (yeni veya varolan) seçin ve içe aktarma onay kutusunu **önce Truncate tablosunu** seçin veya temizleyin. **İleri**'ye tıklayın.
4. Kodlamayı ve alınacak sütunları seçin ve sonra **İleri'yi**tıklatın. 
5. Veri **Alma** sayfasında **İleri'yi**tıklatın. Sihirbaz verileri buna göre aktarın.

### <a name="sql-data-export-and-import-wizards-from-the-navigator-pane"></a>Sql veri dışa aktarma ve gezinme bölmesinden aktarım sihirbazları
MySQL Workbench'ten oluşturulan veya mysqldump komutundan oluşturulan SQL'i dışa aktarmak veya almak için bir sihirbaz kullanın. Bu sihirbazlara **Navigator** bölmesinden veya ana menüden **Server'ı** seçerek erişin. Ardından **Veri Dışa Aktarma** veya **Veri Aktar'ı'nı**seçin. 

#### <a name="data-export"></a>Veri İhracatı
![Navigator bölmesini kullanarak MySQL Workbench veri dışa aktarma](./media/concepts-migrate-import-export/p2.png)

MySQL verilerinizi dışa aktarmak için **Veri Dışa Aktarma** sekmesini kullanabilirsiniz. 
1. Dışa aktarmak istediğiniz her şemayı seçin, isteğe bağlı olarak her şemadaki belirli şema nesnelerini/tablolarını seçin ve dışa aktarmayı oluşturun. Yapılandırma seçenekleri, proje klasörüne veya bağımsız SQL dosyasına dışa aktarmayı, depolanan yordamları ve olayları boşaltmayı veya tablo verilerini atlamayı içerir. 
 
   Alternatif olarak, SQL düzenleyicisinde ayarlanan belirli bir sonucu CSV, JSON, HTML ve XML gibi başka bir biçime aktarmak için **Sonuç Kümesi'ni dışa** aktar'ı kullanın. 
3. Dışa aktarmak için veritabanı nesnelerini seçin ve ilgili seçenekleri yapılandırın.
4. Geçerli nesneleri yüklemek için **Yenile'yi** tıklatın.
5. İsteğe bağlı olarak, dışa aktarma işlemini hassaslaştırmak için **Gelişmiş Seçenekler** sekmesini açın. Örneğin, tablo kilitleri ekleyin, ekleme deyimleri yerine değiştir'i kullanın ve backtick karakterleri olan tanımlayıcıları belirtin.
6. Dışa aktarma işlemini başlatmak için **Dışa Aktar'ı başlat'ı** tıklatın.


#### <a name="data-import"></a>Veri Alma
![MySQL Çalışma Tezgahı Veri Alma Yönetimi Navigator kullanarak](./media/concepts-migrate-import-export/p3.png)

Veri dışa aktarma işleminden veya mysqldump komutundan dışa aktarılan verileri almak veya geri yüklemek için **Veri Alma** sekmesini kullanabilirsiniz. 
1. Proje klasörünü veya bağımsız SQL dosyasını seçin, içe aktarılan şemayı seçin veya yeni bir şema tanımlamak için **Yeni'yi** seçin. 
2. Alma işlemini başlatmak için **İçe Aktarma'yı başlat'ı** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
- Başka bir geçiş yaklaşımı olarak, [MySQL için Azure Veritabanı'nda dökümü ve geri yükleme kullanarak MySQL veritabanınızı geçir'i](concepts-migrate-dump-restore.md)okuyun.
- Veritabanlarını MySQL için Azure Veritabanına geçirme hakkında daha fazla bilgi için [Veritabanı Geçiş Kılavuzu'na](https://aka.ms/datamigration)bakın. 
