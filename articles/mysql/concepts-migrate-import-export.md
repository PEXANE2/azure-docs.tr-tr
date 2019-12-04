---
title: İçeri ve dışarı aktarma-MySQL için Azure veritabanı
description: Bu makalede, MySQL için Azure veritabanı 'nda MySQL çalışma ekranı gibi araçları kullanarak veritabanlarını içeri ve dışarı aktarmanın yaygın yolları açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 8cf0b88ddc24bfc6bc293dd62416417f1eec3a06
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74770960"
---
# <a name="migrate-your-mysql-database-by-using-import-and-export"></a>İçeri ve dışarı aktarma kullanarak MySQL veritabanınızı geçirme
Bu makalede MySQL çalışma ekranı kullanarak bir MySQL için Azure veritabanı sunucusuna verileri içeri ve dışarı aktarmaya yönelik iki yaygın yaklaşım açıklanmaktadır. 

## <a name="before-you-begin"></a>Başlamadan önce
Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- MySQL için Azure veritabanı sunucusu, aşağıdaki [Azure Portal kullanarak MySQL Için Azure veritabanı sunucusu oluşturun](quickstart-create-mysql-server-database-using-azure-portal.md).
- MySQL çalışma ekranı veya içeri aktarmak ve dışarı aktarmak için başka bir MySQL aracı [indirildi](https://dev.mysql.com/downloads/workbench/).

## <a name="use-common-tools"></a>Ortak araçları kullanma
MySQL için Azure veritabanı 'na uzaktan bağlanıp içeri veya dışarı aktarmak için MySQL çalışma ekranı, Toad veya Navicat gibi yaygın araçları kullanın. 

MySQL için Azure veritabanı 'na bağlanmak üzere bir Internet bağlantısı ile istemci makinenizde bu tür araçları kullanın. [MySQL Için Azure veritabanı 'NDA SSL bağlantısını yapılandırma](concepts-ssl-connection-security.md)bölümünde açıklandığı gibi en iyi güvenlik UYGULAMALARı için SSL şifreli bir bağlantı kullanın.

MySQL için Azure veritabanı 'na geçiş yaparken, içeri ve dışarı aktarma dosyalarınızı herhangi bir özel bulut konumuna taşımanız gerekmez. 

## <a name="create-a-database-on-the-azure-database-for-mysql-server"></a>MySQL için Azure veritabanı sunucusunda bir veritabanı oluşturma
Verileri geçirmek istediğiniz MySQL için Azure veritabanı sunucusunda boş bir veritabanı oluşturun. Veritabanını oluşturmak için MySQL çalışma ekranı, Toad veya gezinti \ gibi bir araç kullanın. Veritabanı, dökülebilir verileri içeren veritabanıyla aynı ada sahip olabilir veya farklı bir ada sahip bir veritabanı oluşturabilirsiniz.

Bağlanmak için, MySQL için Azure veritabanı **'Na genel bakış** bölümündeki bağlantı bilgilerini bulun.

![Azure portal bağlantı bilgilerini bulun](./media/concepts-migrate-import-export/1_server-overview-name-login.png)

Bağlantı bilgilerini MySQL çalışma ekranına ekleyin.

![MySQL çalışma ekranı bağlantı dizesi](./media/concepts-migrate-import-export/2_setup-new-connection.png)

## <a name="determine-when-to-use-import-and-export-techniques-instead-of-a-dump-and-restore"></a>Bir döküm ve geri yükleme yerine içeri ve dışarı aktarma tekniklerini ne zaman kullanacağınızı belirleme
Aşağıdaki senaryolarda bulunan veritabanlarını Azure MySQL veritabanına içeri ve dışarı aktarmak için MySQL araçlarını kullanın. Diğer senaryolarda, bunun yerine [döküm ve geri yükleme](concepts-migrate-dump-restore.md) yaklaşımını kullanmaktan faydalanabilirsiniz. 

- Mevcut bir MySQL veritabanından Azure MySQL veritabanına içeri aktarmak için seçmeli olarak birkaç tablo seçmeniz gerektiğinde, bu, içeri ve dışarı aktarma tekniğinin kullanılması en iyisidir.  Bunu yaptığınızda, zaman ve kaynakları kazanmak için geçişten gereksiz tabloları atlayabilirsiniz. Örneğin, [mysqlpump](https://dev.mysql.com/doc/refman/5.7/en/mysqlpump.html#option_mysqlpump_include-tables) ile `--include-tables` veya `--exclude-tables` anahtarını ve [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html#option_mysqldump_tables)ile `--tables` anahtarını kullanın.
- Tablo dışındaki veritabanı nesnelerini taşırken, bu nesneleri açıkça oluşturun. Kısıtlama (birincil anahtar, yabancı anahtar, dizinler), görünümler, işlevler, yordamlar, Tetikleyiciler ve geçirmek istediğiniz diğer veritabanı nesneleri dahil edin.
- MySQL veritabanı dışındaki dış veri kaynaklarından veri geçirirken, düz dosyalar oluşturun ve [mysqlimport](https://dev.mysql.com/doc/refman/5.7/en/mysqlimport.html)kullanarak içeri aktarın.

MySQL için Azure veritabanı 'na veri yüklerken veritabanındaki tüm tabloların InnoDB Storage altyapısını kullanmasını sağlayın. MySQL için Azure veritabanı yalnızca InnoDB depolama altyapısını destekler, bu nedenle alternatif depolama altyapılarını desteklemez. Tablolarınız alternatif depolama motorları gerektiriyorsa, MySQL için Azure veritabanı 'na geçişten önce InnoDB Engine biçimini kullanmak üzere bunları dönüştürdiğinizden emin olun. 

Örneğin, MyISAM altyapısını kullanan bir WordPress veya Web uygulamanız varsa, önce verileri InnoDB tablolarına geçirerek tabloları dönüştürün. Ardından MySQL için Azure veritabanı 'na geri yükleyin. Bir tablo oluşturmak için altyapıyı ayarlamak ve sonra verileri geçişten önce uyumlu tabloya aktarmak için `ENGINE=INNODB` yan tümcesini kullanın. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```

## <a name="performance-recommendations-for-import-and-export"></a>İçeri ve dışarı aktarma için performans önerileri
-   Verileri yüklemeden önce kümelenmiş dizinler ve birincil anahtarlar oluşturun. Verileri birincil anahtar sırasıyla yükleyin. 
-   Veri yüklenene kadar ikincil dizinlerin oluşturulmasını geciktir. Yüklemeden sonra tüm ikincil dizinleri oluştur. 
-   Yüklemeden önce yabancı anahtar kısıtlamalarını devre dışı bırakın. Yabancı anahtar denetimlerinin devre dışı bırakılması, önemli ölçüde performans artışı sağlar. Kısıtlamaları etkinleştirin ve bilgi tutarlılığı sağlamak için yüklemeden sonra verileri doğrulayın.
-   Verileri paralel olarak yükleyin. Kaynak sınırına ulaşmanıza ve Azure portal bulunan ölçümleri kullanarak kaynakları izlemenize neden olacak çok fazla paralellik yapmaktan kaçının. 
-   Uygun olduğunda bölümlenmiş tabloları kullanın.

## <a name="import-and-export-by-using-mysql-workbench"></a>MySQL çalışma ekranı kullanarak içeri ve dışarı aktarma
MySQL çalışma ekranına verileri dışarı ve içeri aktarmanın iki yolu vardır. Her biri farklı bir amaca hizmet eder. 

### <a name="table-data-export-and-import-wizards-from-the-object-browsers-context-menu"></a>Nesne tarayıcısının bağlam menüsünden Tablo verilerini dışa aktarma ve içeri aktarma sihirbazları
![Nesne tarayıcısının bağlam menüsündeki MySQL çalışma ekranı sihirbazları](./media/concepts-migrate-import-export/p1.png)

Tablo verilerine yönelik sihirbazlar CSV ve JSON dosyalarını kullanarak içeri ve dışarı aktarma işlemlerini destekler. Ayırıcılar, sütun seçimi ve kodlama seçimi gibi çeşitli yapılandırma seçeneklerini içerirler. Her Sihirbazı yerel veya uzaktan bağlanan MySQL sunucularında gerçekleştirebilirsiniz. İçeri aktarma eylemi tablo, sütun ve tür eşlemeyi içerir. 

Bu sihirbazlara, bir tabloya sağ tıklayarak nesne tarayıcısının bağlam menüsünden erişebilirsiniz. Ardından **Tablo verileri dışarı aktarma Sihirbazı** veya **tablo verilerini içeri aktarma Sihirbazı**' nı seçin. 

#### <a name="table-data-export-wizard"></a>Tablo verilerini dışarı aktarma Sihirbazı
Aşağıdaki örnek, tabloyu bir CSV dosyasına dışarı aktarır: 
1. Aktarılacak veritabanının tablosuna sağ tıklayın. 
2. **Tablo verilerini dışarı aktarma Sihirbazı**' nı seçin. Aktarılacak sütunları, satır sapmasını (varsa) ve sayıyı (varsa) seçin. 
3. **Dışarı aktarma için veri Seç** sayfasında **İleri**' ye tıklayın. Dosya yolu, CSV veya JSON dosya türünü seçin. Ayrıca satır ayırıcı, kapsayan dizeler yöntemi ve alan ayırıcısı ' nı da seçin. 
4. **Çıkış dosyası konumunu seçin** sayfasında **İleri**' ye tıklayın. 
5. **Verileri dışarı aktar** sayfasında **İleri**' ye tıklayın.

#### <a name="table-data-import-wizard"></a>Tablo verileri Içeri aktarma Sihirbazı
Aşağıdaki örnek, tabloyu bir CSV dosyasından içe aktarır:
1. İçeri aktarılacak veritabanının tablosuna sağ tıklayın. 
2. İçeri aktarılacak CSV dosyasına gidin ve seçin ve ardından **İleri**' ye tıklayın. 
3. Hedef tabloyu seçin (yeni veya var olan) ve **içeri aktarmadan önce tabloyu kes** onay kutusunu seçin veya temizleyin. **İleri**’ye tıklayın.
4. Kodlama ve içeri aktarılacak sütunları seçin ve ardından **İleri**' ye tıklayın. 
5. **Verileri Içeri aktar** sayfasında, **İleri**' ye tıklayın. Sihirbaz verileri buna göre içe aktarır.

### <a name="sql-data-export-and-import-wizards-from-the-navigator-pane"></a>Gezgin bölmesinden SQL verileri dışarı aktarma ve içeri aktarma sihirbazları
MySQL çalışma sunucusundan oluşturulan veya mysqldump komutundan oluşturulan SQL 'i içeri veya dışarı aktarmak için bir sihirbaz kullanın. Bu sihirbazlara **Gezgin** bölmesinden veya ana menüden **sunucu** ' yı seçerek erişin. Ardından **veri dışarı aktarma** veya **veri içeri aktarma**' yı seçin. 

#### <a name="data-export"></a>Veri dışarı aktarma
![MySQL pencere verileri Gezgin bölmesini kullanarak dışarı aktarma](./media/concepts-migrate-import-export/p2.png)

MySQL verilerinizi dışarı aktarmak için **veri dışa aktarma** sekmesini kullanabilirsiniz. 
1. Dışarı aktarmak istediğiniz her şemayı seçin, isteğe bağlı olarak her şemadan belirli şema nesneleri/tablolar ' ı seçin ve dışarı aktarmayı oluşturun. Yapılandırma seçenekleri bir proje klasörüne veya kendi içindeki SQL dosyasına dışarı aktarma, saklı yordamlar ve olayların dökümünü alma veya tablo verilerini atlama içerir. 
 
   Alternatif olarak, SQL düzenleyicisinde belirli bir sonuç kümesini CSV, JSON, HTML ve XML gibi başka bir biçime dışarı aktarmak için **bir sonuç kümesini dışarı aktar** ' ı kullanın. 
3. Dışarı aktarılacak veritabanı nesnelerini seçin ve ilgili seçenekleri yapılandırın.
4. Geçerli nesneleri yüklemek için **Yenile** ' ye tıklayın.
5. İsteğe bağlı olarak, dışarı aktarma işlemini iyileştirmek için **Gelişmiş Seçenekler** sekmesini açın. Örneğin, tablo kilitleri ekleyin, INSERT deyimleri yerine Replace kullanın, ve tırnak tanımlayıcıları de backtick karakterlerinden oluşur.
6. Dışarı aktarma işlemini başlatmak için **dışarı aktarmayı Başlat** ' a tıklayın.


#### <a name="data-import"></a>Veri Içeri aktarma
![MySQL çalışma ekranı verileri yönetim gezginini kullanarak Içeri aktarma](./media/concepts-migrate-import-export/p3.png)

Dışarı aktarılan verileri veri dışa aktarma işleminden veya mysqldump komutundan içeri aktarmak veya geri yüklemek için **veri Içeri aktarma** sekmesini kullanabilirsiniz. 
1. Proje klasörünü veya kendi içinde kapsanan SQL dosyasını seçin, içine aktarılacak şemayı seçin veya yeni bir şema tanımlamak için **Yeni** ' yi seçin. 
2. İçeri aktarma işlemine başlamak için **Içeri aktarmayı Başlat** ' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
- Başka bir geçiş yaklaşımı olarak MySQL [Için Azure veritabanı 'nda dökümünü ve geri yüklemeyi kullanarak MySQL veritabanınızı geçirin](concepts-migrate-dump-restore.md).
- MySQL için Azure veritabanı 'na veritabanı geçirme hakkında daha fazla bilgi için bkz. [veritabanı geçiş kılavuzu](https://aka.ms/datamigration). 
