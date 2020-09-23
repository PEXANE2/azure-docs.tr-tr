---
title: İçeri ve dışarı aktarma-MySQL için Azure veritabanı
description: Bu makalede, MySQL için Azure veritabanı 'nda MySQL çalışma ekranı gibi araçları kullanarak veritabanlarını içeri ve dışarı aktarmanın yaygın yolları açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 9/22/2020
ms.openlocfilehash: 6d0a29d8ef8123eafd6a1616a24003c1e36e6e59
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90905927"
---
# <a name="migrate-your-mysql-database-by-using-import-and-export"></a>İçeri ve dışarı aktarma kullanarak MySQL veritabanınızı geçirme
[!INCLUDE[applies-to-single-flexible-server](includes/applies-to-single-flexible-server.md)]
Bu makalede MySQL çalışma ekranı kullanarak bir MySQL için Azure veritabanı sunucusuna verileri içeri ve dışarı aktarmaya yönelik iki yaygın yaklaşım açıklanmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce
Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- MySQL için Azure veritabanı sunucusu, aşağıdaki [Azure Portal kullanarak MySQL Için Azure veritabanı sunucusu oluşturun](quickstart-create-mysql-server-database-using-azure-portal.md).
- İçeri/dışarı aktarma işlemini yapmak için [MySQL çalışma ekranı](https://dev.mysql.com/downloads/workbench/) veya başka bir üçüncü taraf MySQL aracı.

## <a name="create-a-database-on-the-azure-database-for-mysql-server"></a>MySQL için Azure veritabanı sunucusunda bir veritabanı oluşturma
Veritabanı oluşturmak için MySQL çalışma ekranı, Toad veya Navıat kullanarak MySQL için Azure veritabanı sunucusunda boş bir veritabanı oluşturun. Veritabanı, dökülebilir verileri içeren veritabanıyla aynı ada sahip olabilir veya farklı bir ada sahip bir veritabanı oluşturabilirsiniz.

Bağlanmak için, MySQL için Azure veritabanı **'Na genel bakış** bölümündeki bağlantı bilgilerini bulun.

:::image type="content" source="./media/concepts-migrate-import-export/1_server-overview-name-login.png" alt-text="Azure portal bağlantı bilgilerini bulun":::

Bağlantı bilgilerini MySQL çalışma ekranına ekleyin.

:::image type="content" source="./media/concepts-migrate-import-export/2_setup-new-connection.png" alt-text="MySQL çalışma ekranı bağlantı dizesi":::

## <a name="determine-when-to-use-import-and-export-techniques"></a>İçeri ve dışarı aktarma tekniklerini ne zaman kullanacağınızı belirleme

> [!TIP]
> Tüm veritabanını dökümünü almak ve geri yüklemek istediğiniz senaryolar için, bunun yerine [döküm ve geri yükleme](concepts-migrate-dump-restore.md) yaklaşımını kullanmanız gerekir.

Aşağıdaki senaryolarda bulunan veritabanlarını Azure MySQL veritabanına içeri ve dışarı aktarmak için MySQL araçlarını kullanın.

- Mevcut bir MySQL veritabanından Azure MySQL veritabanına içeri aktarmak için seçmeli olarak birkaç tablo seçmeniz gerektiğinde, bu, içeri ve dışarı aktarma tekniğinin kullanılması en iyisidir.  Bunu yaptığınızda, zaman ve kaynakları kazanmak için geçişten gereksiz tabloları atlayabilirsiniz. Örneğin, `--include-tables` `--exclude-tables` [mysqlpump](https://dev.mysql.com/doc/refman/5.7/en/mysqlpump.html#option_mysqlpump_include-tables) ile veya anahtarını `--tables` [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html#option_mysqldump_tables)ile anahtarla kullanın.
- Tablo dışındaki veritabanı nesnelerini taşırken, bu nesneleri açıkça oluşturun. Kısıtlama (birincil anahtar, yabancı anahtar, dizinler), görünümler, işlevler, yordamlar, Tetikleyiciler ve geçirmek istediğiniz diğer veritabanı nesneleri dahil edin.
- MySQL veritabanı dışındaki dış veri kaynaklarından veri geçirirken, düz dosyalar oluşturun ve [mysqlimport](https://dev.mysql.com/doc/refman/5.7/en/mysqlimport.html)kullanarak içeri aktarın.

> [!Important]
> Tek sunucu ve esnek sunucu **yalnızca InnoDB depolama altyapısını**destekler. MySQL için Azure veritabanı 'na veri yüklerken veritabanındaki tüm tabloların InnoDB Storage altyapısını kullanmasını sağlayın.
> Kaynak veritabanınız başka bir depolama altyapısı kullanıyorsa veritabanını geçirmeden önce lütfen InnoDB altyapısına dönüştürün. Örneğin, MyISAM altyapısını kullanan bir WordPress veya Web uygulamanız varsa, önce verileri InnoDB tablolarına geçirerek tabloları dönüştürün. Yan tümcesini kullanarak `ENGINE=INNODB` tablo oluşturma altyapısını ayarlayın ve ardından geçişten önce verileri uyumlu tabloya aktarın.

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

> [!NOTE]
> MySQL çalışma ekranına MySQL tek veya esnek sunucusuna (Önizleme) bir bağlantı ekliyorsanız lütfen şunları yaptığınızdan emin olun:
> - MySQL tek sunucusu için Kullanıcı adı bu biçimde olmalıdır, ' username@servername '
> - MySQL esnek sunucusu için ' ' kullanırsanız ' Kullanıcı adı ' kullanabilirsiniz username@servername , bağlantı başarısız olur.

### <a name="table-data-export-and-import-wizards-from-the-object-browsers-context-menu"></a>Nesne tarayıcısının bağlam menüsünden Tablo verilerini dışa aktarma ve içeri aktarma sihirbazları
:::image type="content" source="./media/concepts-migrate-import-export/p1.png" alt-text="Nesne tarayıcısının bağlam menüsündeki MySQL çalışma ekranı sihirbazları":::

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

#### <a name="data-export"></a>Verileri Dışarı Aktarma
:::image type="content" source="./media/concepts-migrate-import-export/p2.png" alt-text="MySQL pencere verileri Gezgin bölmesini kullanarak dışarı aktarma":::

MySQL verilerinizi dışarı aktarmak için **veri dışa aktarma** sekmesini kullanabilirsiniz.
1. Dışarı aktarmak istediğiniz her şemayı seçin, isteğe bağlı olarak her şemadan belirli şema nesneleri/tablolar ' ı seçin ve dışarı aktarmayı oluşturun. Yapılandırma seçenekleri bir proje klasörüne veya kendi içindeki SQL dosyasına dışarı aktarma, saklı yordamlar ve olayların dökümünü alma veya tablo verilerini atlama içerir.

   Alternatif olarak, SQL düzenleyicisinde belirli bir sonuç kümesini CSV, JSON, HTML ve XML gibi başka bir biçime dışarı aktarmak için **bir sonuç kümesini dışarı aktar** ' ı kullanın.
3. Dışarı aktarılacak veritabanı nesnelerini seçin ve ilgili seçenekleri yapılandırın.
4. Geçerli nesneleri yüklemek için **Yenile** ' ye tıklayın.
5. İsteğe bağlı olarak, dışarı aktarma işlemini iyileştirmek için **Gelişmiş Seçenekler** sekmesini açın. Örneğin, tablo kilitleri ekleyin, INSERT deyimleri yerine Replace kullanın, ve tırnak tanımlayıcıları de backtick karakterlerinden oluşur.
6. Dışarı aktarma işlemini başlatmak için **dışarı aktarmayı Başlat** ' a tıklayın.


#### <a name="data-import"></a>Veri Içeri aktarma
:::image type="content" source="./media/concepts-migrate-import-export/p3.png" alt-text="MySQL çalışma ekranı verileri yönetim gezginini kullanarak Içeri aktarma":::

Dışarı aktarılan verileri veri dışa aktarma işleminden veya mysqldump komutundan içeri aktarmak veya geri yüklemek için **veri Içeri aktarma** sekmesini kullanabilirsiniz.
1. Proje klasörünü veya kendi içinde kapsanan SQL dosyasını seçin, içine aktarılacak şemayı seçin veya yeni bir şema tanımlamak için **Yeni** ' yi seçin.
2. İçeri aktarma işlemine başlamak için **Içeri aktarmayı Başlat** ' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
- Başka bir geçiş yaklaşımı olarak MySQL [Için Azure veritabanı 'nda dökümünü ve geri yüklemeyi kullanarak MySQL veritabanınızı geçirin](concepts-migrate-dump-restore.md).
- MySQL için Azure veritabanı 'na veritabanı geçirme hakkında daha fazla bilgi için bkz. [veritabanı geçiş kılavuzu](https://aka.ms/datamigration).
