---
title: Çoğaltma gecikmesi sorunlarını giderme-MySQL için Azure veritabanı
description: MySQL için Azure veritabanı okuma çoğaltmalarıyla çoğaltma gecikmesini nasıl giderebileceğinizi öğrenin
keywords: MySQL, sorun giderme, saniye cinsinden çoğaltma gecikme süresi
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: troubleshooting
ms.date: 10/08/2020
ms.openlocfilehash: 16a502a53b4441faf68ea342e0bc865731d38b1a
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91877166"
---
# <a name="troubleshoot-replication-latency-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda çoğaltma gecikmesi sorunlarını giderme

[Çoğaltma oku](concepts-read-replicas.md) özelliği, bir MySQL Için Azure veritabanı sunucusu sunucusundan salt bir çoğaltma sunucusuna veri çoğaltmanıza olanak sağlar. Okuma/raporlama sorgularını uygulamadan çoğaltma sunucularına yönlendirerek iş yükünü ölçeklendirmek için okuma çoğaltmaları kullanılır. Bu, birincil sunucunun basıncını azaltır ve ölçeklendirilirken uygulamanın genel performansını ve gecikme süresini geliştirir. Çoğaltmalar, MySQL altyapısının yerel ikili günlük (binlog) dosya konumu tabanlı çoğaltma teknolojisini kullanılarak zaman uyumsuz olarak güncelleştirilir. Binlog çoğaltma hakkında daha fazla bilgi edinmek için [MySQL binlog çoğaltmasına genel bakış](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html)bölümüne bakın. 

İkincil okuma çoğaltmalarının çoğaltma gecikmesi, dahil ancak bunlarla sınırlı olmamak üzere faktör sayısına bağlıdır 

- Ağ gecikmesi
- Kaynak sunucudaki işlem hacmi
- Kaynak ve ikincil okuma çoğaltması sunucusunun işlem katmanı
- Birincil ve ikincil sunucuda çalışan sorgular. 

Bu belgede, MySQL için Azure veritabanı 'nda çoğaltma gecikmesini nasıl giderebileceğinizi öğreneceksiniz. Ayrıca, çoğaltma sunucularında daha fazla çoğaltma gecikme süresinin bazı yaygın nedenlerini de anlamış olursunuz.

## <a name="replication-concepts"></a>Çoğaltma kavramları

İkili günlük etkinleştirildiğinde, kaynak sunucu, çoğaltma için kullanılan ikili günlüğe kaydedilmiş işlemi yazar. En fazla 16 TB depolamayı destekleyen yeni sağlanan tüm sunucular için ikili günlük varsayılan olarak açıktır. Çoğaltma sunucularında, biri GÇ iş parçacığı ve diğeri de SQL iş parçacığı adlı çoğaltma sunucusu başına çalışan iki iş parçacığı vardır.

- **GÇ iş parçacığı** kaynak sunucuya bağlanır ve ikili günlükleri güncelleştirilmiş olarak ister. Bu iş parçacığı ikili günlük güncelleştirmelerini aldıktan sonra, geçiş günlüğü adlı yerel bir günlüğe bir çoğaltma sunucusuna kaydedilir.
- **SQL iş parçacığı** geçiş günlüğünü okur ve veri değişikliğini çoğaltma sunucularına uygular.

## <a name="monitoring-replication-latency"></a>İzleme çoğaltma gecikmesi

MySQL için Azure veritabanı, [Azure izleyici](concepts-monitoring.md)'de saniye cinsinden yineleme gecikmesi sağlar. Bu ölçüm yalnızca okuma çoğaltmaları sunucularında kullanılabilir. Bu ölçüm, MySQL 'de kullanılabilir seconds_behind_master ölçümü kullanılarak hesaplanır. Daha fazla çoğaltma gecikme süresinin kök nedenini anlamak için [MySQL](connect-workbench.md) çalışma sunucusunu veya [Azure Cloud Shell](https://shell.azure.com) 'i kullanarak çoğaltma sunucusuna bağlanın ve aşağıdaki komutu yürütün:

 Değerleri gerçek çoğaltma sunucunuzun adı ve Yönetici Kullanıcı oturum açma adı ile değiştirin. Yönetici Kullanıcı adı, \<servername> MySQL Için Azure veritabanı için ' @ ' gerektirir:

  ```azurecli-interactive
  mysql --host=myreplicademoserver.mysql.database.azure.com --user=myadmin@mydemoserver -p 
  ```

  Deneyim Cloud Shell terminalinde şöyle görünür
  ```
  Requesting a Cloud Shell.Succeeded.
  Connecting terminal...

  Welcome to Azure Cloud Shell

  Type "az" to use Azure CLI
  Type "help" to learn about Cloud Shell

  user@Azure:~$mysql -h myreplicademoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
  Enter password:
  Welcome to the MySQL monitor.  Commands end with ; or \g.
  Your MySQL connection id is 64796
  Server version: 5.6.42.0 Source distribution

  Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

  Oracle is a registered trademark of Oracle Corporation and/or its
  affiliates. Other names may be trademarks of their respective
  owners.

  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
  mysql>
  ```
  Aynı Azure Cloud Shell terminalde, aşağıdaki komutu yürütün

  ```
  mysql> SHOW SLAVE STATUS;
  ```

  Tipik bir çıktı şöyle görünür:
  
>[!div class="mx-imgBorder"]
> :::image type="content" source="./media/howto-troubleshoot-replication-latency/show-status.png" alt-text="İzleme çoğaltma gecikmesi&quot;:::


Çıktı çok fazla bilgi içerir, ancak normalde aşağıdaki sütunlara odaklanmak önemlidir:

|Ölçüm|Açıklama|
|---|---|
|Slave_IO_State| GÇ iş parçacığının geçerli durumu. Normalde durum, eşitleme ise &quot;ana öğe gönderilmesi bekleniyor&quot; olur. Bununla birlikte, &quot;ana ağa bağlanma&quot; gibi bir durum görürseniz, çoğaltma ana sunucuyla bağlantıyı kaybetti. Ana bağlantının çalışıp çalışmadığını veya bağlantıyı engelleyip engellemediğini denetleyin.|
|Master_Log_File| Ana dosyanın yazıldığı ikili günlük dosyası.|
|Read_Master_Log_Pos| Yukarıdaki ikili günlük dosyasında, ana dosyanın yazıldığı konumu temsil eder.|
|Relay_Master_Log_File| Belirtilen, çoğaltma sunucusunun ana bilgisayardan okuduğu ikili günlük dosyasını temsil eder.|
|Slave_IO_Running| GÇ iş parçacığının çalışıp çalışmadığını belirtir. &quot;Yes&quot; olması gerekir. &quot;Hayır&quot; ise, büyük olasılıkla çoğaltma bozulur.|
|Slave_SQL_Running| SQL iş parçacığının çalışıp çalışmadığını belirtir. &quot;Yes&quot; olması gerekir. &quot;Hayır&quot; ise, büyük olasılıkla çoğaltma bozulur.|
|Exec_Master_Log_Pos| Çoğaltmanın uygulama Relay_Master_Log_File konumunu görüntüler. Gecikme varsa, bu konum sırası Read_Master_Log_Pos daha küçük olmalıdır.|
|Relay_Log_Space|Geçiş günlük boyutunun üst sınırını görüntüler. &quot;Relay_log_space_limit&quot; gibi genel değişkenleri göster ' i sorgulayarak boyutu kontrol edebilirsiniz.|
|Seconds_Behind_Master| Çoğaltma gecikmesini saniye cinsinden görüntüler.|
|Last_IO_Errno|Varsa, GÇ iş parçacığı hata kodunu görüntüler. Bu kodlar hakkında daha fazla bilgi için bkz. [MySQL belgeleri](https://dev.mysql.com/doc/refman/5.7/en/server-error-reference.html).|
|Last_IO_Error| Varsa, GÇ iş parçacığı hata iletisini görüntüler.|
|Last_SQL_Errno|Varsa, SQL iş parçacığı hata kodunu görüntüler. Bu kodlar hakkında daha fazla bilgi için bkz. [MySQL belgeleri](https://dev.mysql.com/doc/refman/5.7/en/server-error-reference.html).|
|Last_SQL_Error|Varsa, SQL iş parçacığı hata iletisini görüntüler.|
|Slave_SQL_Running_State| Geçerli SQL iş parçacığı durumunu gösterir. Bu durumda gösterilen &quot;sistem kilidi&quot; nin normal bir davranış olduğunu unutmayın. &quot;Bağımlı işlemin yürütülmesi bekleniyor" durumuna bakmak normaldir. Çoğaltmanın, yöneticinin yürütülen işlemleri güncelleştirmesini beklediğini belirtir.|

Slave_IO_Running Evet ise ve Slave_SQL_Running Evet ise, çoğaltma sorunsuz bir şekilde çalışır. 

Ardından Last_IO_Errno, Last_IO_Error, Last_SQL_Errno ve Last_SQL_Error denetlemeniz gerekir.  Bu alanlar, SQL iş parçacığının durdurulmasına neden olan en son hatanın hata numarasını ve hata iletisini tutar. Hata numarası 0 ve boş ileti bir hata olmadığı anlamına gelir. [MySQL belgelerindeki](https://dev.mysql.com/doc/refman/5.7/en/server-error-reference.html)hata koduna bakarak hata içindeki sıfır olmayan bir değer araştırılmalıdır.

## <a name="common-scenarios-for-high-replication-latency"></a>Yüksek çoğaltma gecikmesi için yaygın senaryolar

### <a name="network-latency-or-high-cpu-on-source-server"></a>Kaynak sunucuda ağ gecikmesi veya yüksek CPU

Aşağıdaki değerleri gözlemlerseniz, çoğaltma gecikme süresinin en yaygın nedeni, kaynak sunucuda yüksek ağ gecikmesi veya yüksek CPU tüketimidir. Bu durumda, GÇ iş parçacığı çalışıyor ve ana bilgisayarda bekliyor. Ana (kaynak sunucu), çoğaltma yalnızca dosya #10 aldığından #20 ikili günlük dosyasına zaten yazıldı. Bu senaryoda yüksek çoğaltma gecikmesi için katkıda bulunan temel etkenler, kaynak sunucuda ağ hızı veya yüksek CPU kullanımlardır.  Azure 'da, bir bölgedeki ağ gecikmesi genellikle milisaniye cinsinden ve bölge genelinde aralıklar arasında olabilir. Çoğu durumda, kaynak sunucuya bağlanmak için GÇ iş parçacığındaki gecikme, GÇ iş parçacığı işlemenin yavaş olmasına neden olan kaynak sunucuda yüksek CPU kullanımı nedeniyle oluşur. Bu, CPU kullanımını izleyerek ve kaynak sunucudaki eşzamanlı bağlantı sayısını Azure izleyici kullanılarak gözlemleyerek tespit edilebilir.

Kaynak sunucuda yüksek CPU kullanımı görmüyorsanız olası nedenler ağ gecikmesi olabilir. ani bir şekilde yüksek ağ gecikmesi görürseniz, bilinen sorunlar veya kesintiler olduğundan emin olmak için [Azure durum sayfasını](https://status.azure.com/status) kontrol etmenizi öneririz. 

```
Slave_IO_State: Waiting for master to send event
Master_Log_File: the binary file sequence is larger then Relay_Master_Log_File, e.g. mysql-bin.00020
Relay_Master_Log_File: the file sequence is smaller than Master_Log_File, e.g. mysql-bin.00010
```

### <a name="heavy-burst-of-transactions-on-source-server"></a>Kaynak sunucudaki işlemlerin ağır patlaması

Aşağıdaki değerleri gözlemlerseniz, çoğaltma gecikme süresinin en yaygın nedeni, kaynak sunucu üzerindeki işlemlerin yoğun bir şekilde patlaması olur. Aşağıdaki çıktıda çoğaltma, ana kopyanın arkasındaki ikili günlüğü alabilse de, çoğaltma GÇ iş parçacığı geçiş günlük alanının dolu olduğunu gösterir. Bu nedenle, çoğaltma daha hızlı bir şekilde yakalamaya çalıştığından, gecikme nedeniyle ağ hızı gecikmeye neden olmaz. Bunun yerine, güncelleştirilmiş ikili günlük boyutu, geçiş günlük alanının üst sınırını aşıyor. Bu sorunu gidermek için, ana sunucuda [yavaş sorgu günlüğü](concepts-server-logs.md) etkinleştirilmelidir. Yavaş sorgu günlükleri, kaynak sunucuda uzun süre çalışan işlemleri tanımlamanızı sağlar. Sunucuda gecikme süresini azaltmak için tanımlanan sorguların ayarlanmış olması gerekir. 

```
Slave_IO_State: Waiting for the slave SQL thread to free enough relay log space
Master_Log_File: the binary file sequence is larger then Relay_Master_Log_File, e.g. mysql-bin.00020
Relay_Master_Log_File: the file sequence is smaller then Master_Log_File, e.g. mysql-bin.00010
```

Bu kategorideki gecikme süresinin yaygın nedenleri aşağıda verilmiştir:

#### <a name="replication-latency-due-to-data-load-on-source-server"></a>Kaynak sunucuda veri yükleme nedeniyle çoğaltma gecikmesi
Bazı durumlarda, kaynak sunucularda haftalık veya aylık veri yükü vardır. Ne yazık ki bu durumda çoğaltma gecikmesi kaçınılmaz. Bu senaryoda, kaynak sunucudaki veri yüklemesi tamamlandıktan sonra çoğaltma sunucuları sonunda yakalar.


### <a name="slowness-on-the-replica-server"></a>Çoğaltma sunucusunda yavaşlık

Aşağıdaki değerleri gözlemlerseniz, en sık karşılaşılan neden Çoğaltma sunucusunda daha fazla araştırma gerektiren bir sorun olabilir. Bu senaryoda, çıkışta görüldüğü gibi, hem GÇ hem de SQL iş parçacıkları iyi çalışıyor ve çoğaltma, ana yazma işlemleri ile aynı ikili günlük dosyasını okuyor. Ancak, Çoğaltma sunucusunda, kaynak sunucudan aynı işlemi yansıtacak bir gecikme süresi oluşur. 

```
Slave_IO_State: Waiting for master to send event
Master_Log_File: The binary log file sequence equals to Relay_Master_Log_File, e.g. mysql-bin.000191
Read_Master_Log_Pos: The position of master server written to the above file is larger than Relay_Log_Pos, e.g. 103978138
Relay_Master_Log_File: mysql-bin.000191
Slave_IO_Running: Yes
Slave_SQL_Running: Yes
Exec_Master_Log_Pos: The position of slave reads from master binary log file is smaller than Read_Master_Log_Pos, e.g. 13468882
Seconds_Behind_Master: There is latency and the value here is greater than 0
```

Bu kategorideki gecikme süresinin yaygın nedenleri aşağıda verilmiştir:

#### <a name="no-primary-or-unique-key-on-a-table"></a>Tabloda birincil veya benzersiz anahtar yok

MySQL için Azure veritabanı satır tabanlı çoğaltmayı kullanır. Satır tabanlı çoğaltma ile, ana sunucu her tablo satırı değişikliğine ilişkin olayları ikili günlüğe yazar. SQL Iş parçacığı, bu değişiklikleri çoğaltma sunucusundaki ilgili tablo satırlarında yürütür. Bir tabloda birincil veya benzersiz anahtar, çoğaltma gecikmesi genel nedenleriyle ilgili değildir. Birincil veya benzersiz anahtarların olmaması, değişiklikleri uygulamak için SQL iş parçacığına göre hedef tablodaki tüm satırları taramaya yol açar.

MySQL 'de birincil anahtar, NULL değerler içermediğinden hızlı sorgu performansını sağlayan ilişkili bir dizindir. InnoDB Storage altyapısı ile tablo verileri, son derece hızlı aramalar yapmak ve birincil anahtara göre sıralama yapmak için fiziksel olarak düzenlenir. Bu nedenle, çoğaltma sunucusunu oluşturmadan önce kaynak sunucudaki tablolarda birincil anahtar eklemeniz önerilir. Bu senaryoda, kaynak sunucuya birincil anahtarlar eklemeniz ve çoğaltma gecikmesini artırmaya yardımcı olmak için okuma çoğaltmalarının yeniden oluşturulması gerekir.

Kaynak sunucuda birincil anahtarı eksik olan tabloları belirleyebilmek için aşağıdaki sorguyu kullanabilirsiniz:

```sql 
select tab.table_schema as database_name, tab.table_name 
from information_schema.tables tab left join 
information_schema.table_constraints tco 
on tab.table_schema = tco.table_schema 
and tab.table_name = tco.table_name 
and tco.constraint_type = 'PRIMARY KEY' 
where tco.constraint_type is null 
and tab.table_schema not in('mysql', 'information_schema', 'performance_schema', 'sys') 
and tab.table_type = 'BASE TABLE' 
order by tab.table_schema, tab.table_name;

```

#### <a name="replication-latency-due-to-long-running-queries-on-replica-server"></a>Çoğaltma sunucusunda uzun süre çalışan sorgular nedeniyle çoğaltma gecikme süresi

Çoğaltma sunucusundaki iş yükünün, SQL iş parçacığını GÇ iş parçacığına devam etmesini engelleyebilmesini sağlayabilirsiniz. Bu, Çoğaltma sunucusunda uzun süre çalışan bir sorgu varsa yüksek çoğaltma gecikmesinin yaygın nedenleriyle biridir. Bu durumda, sorunu gidermeye yardımcı olmak için Çoğaltma sunucusunda [yavaş sorgu günlüğü](concepts-server-logs.md) etkinleştirilmelidir. Yavaş sorgular kaynak tüketimlerini artırabilir veya sunucunun yavaşlamasına neden olabilir, bu nedenle çoğaltma ana öğe ile yakalayamaz. Bu senaryoda, yavaş sorguları ayarlamanız gerekir. Daha hızlı sorgular SQL iş parçacığının engellenmesini önler ve çoğaltma gecikmesini önemli ölçüde geliştirir.


#### <a name="replication-latency-due-to-ddl-queries-on-source-server"></a>Kaynak sunucudaki DDL sorguları nedeniyle çoğaltma gecikme süresi
Kaynak sunucuda yürütülen [alter table](https://dev.mysql.com/doc/refman/5.7/en/alter-table.html) gibi uzun süre çalışan bir ddl komutu varsa ve yürütülmesi 1 saat sürdü. Bu süre boyunca, kaynak sunucuda paralel olarak çalışan binlerce başka sorgu olabilir. DDL, çoğaltmaya çoğaltıldığında, veritabanının tutarlılığını sağlamak için MySQL altyapısının DDL 'yi tek bir çoğaltma iş parçacığında çalıştırması gerekir. Bu nedenle, diğer tüm çoğaltılan sorgular engellenir ve bu Çoğaltma sunucusunda DDL işlemi tamamlanana kadar bir saat veya daha fazla bekleme yapması gerekecektir. Bu, çevrimiçi DDL işleminden bağımsız olarak geçerlidir. DDL işlemleri ile çoğaltma, daha fazla çoğaltma gecikmesini görmeniz beklenir.

Kaynak sunucuda [yavaş sorgu günlüğü](concepts-server-logs.md) etkinse bu senaryo, kaynak sunucuda bir DDL komutunun yürütülüp yürütülmadığını görmek için yavaş sorgu günlüklerine bakarak algılanabilir. Dizin bırakma, yeniden adlandırma ve oluşturma, tablo verilerinin kopyalanmasını ve tablonun yeniden oluşturulmasını gerektirebilecek ALTER TABLE için ıNPLACE algoritması kullanmalıdır. Genellikle ıNPLACE algoritması için eşzamanlı DML desteklenir, ancak tablodaki bir özel meta veri kilidi, işlemin hazırlanması ve yürütme aşamaları sırasında kısaca alınmış olabilir. Bu nedenle, CREATE INDEX deyimleri için, yan tümce ALGORITMASı ve KILIDI, okumak ve yazmak üzere tablo kopyalama yöntemini ve eşzamanlılık düzeyini etkilemek için kullanılabilir, ancak tam metın veya uzamsal dizin eklemek DML işlemlerini de engeller. ALGORITMA ve kılıt yan tümceleri ile dizin oluşturma örneği hakkında daha fazla bilgi için bkz:

```sql
ALTER TABLE table_name ADD INDEX index_name (column), ALGORITHM=INPLACE, LOCK=NONE;
```

Ne yazık ki, bir kilit gerektiren DDL ifadesinde, çoğaltma gecikmesi önlenemez, bunun yerine bu tür DDL işlemleri, olası etkiyi azaltmak için nibir süre boyunca yoğun saatlerde gerçekleştirilmelidir.

#### <a name="replication-latency-due-to-replica-server-lower-sku"></a>Çoğaltma sunucusu alt SKU 'SU nedeniyle çoğaltma gecikme süresi

MySQL için Azure veritabanı okuma çoğaltmaları, ana sunucuyla aynı sunucu yapılandırmasıyla oluşturulur. Çoğaltma sunucusu yapılandırması oluşturulduktan sonra değiştirilebilir. Ancak, çoğaltma sunucusu indirgenirse, iş yükü çoğaltma gecikmesine yol açabilecek şekilde daha yüksek kaynak tüketimine neden olabilir. Bu, Azure Izleyici 'den çoğaltma sunucusunun CPU ve bellek tüketimini izleyerek gözlemlenebilir. Bu senaryoda, çoğaltmanın ana öğe ile devam edebileceğinden emin olmak için çoğaltma sunucusunun yapılandırmasının kaynaktan eşit veya daha büyük bir değer tutulması önerilir.

#### <a name="improving-replication-latency-using-server-parameter-tuning-on-source-server"></a>Kaynak sunucuda sunucu parametre ayarlamayı kullanarak çoğaltma gecikmesini artırma

MySQL için Azure veritabanı 'nda, çoğaltma varsayılan olarak çoğaltmalarda paralel iş parçacıklarıyla çalışacak şekilde iyileştirilmiştir. Çoğaltma sunucusunun yakalayamamakta olduğu kaynak sunucudaki yüksek eşzamanlılık iş yükleri için, kaynak sunucuda binlog_group_commit_sync_delay parametresi yapılandırılarak çoğaltma gecikmesi artırılabilir. Bu parametre, ikili günlük dosyasının eşitlenmesi için kaç mikrosaniye ikili günlük işlemesini denetler. Bu avantaj, uygulanan her işlemi hemen uygulamak yerine, ana öğe ikili günlük güncelleştirmelerini toplu olarak göndermelidir. Bu, çoğaltmada GÇ 'yi azaltır ve performansı artırmaya yardımcı olur. Bu senaryoda, binlog_group_commit_sync_delay 1000 veya bu şekilde ayarlamak ve çoğaltma gecikmesini izlemek faydalı olabilir. Bu parametre, yalnızca yüksek eşzamanlı iş yükleri için, dikkatli ve yararlanılabilir ayarlanmalıdır. Çok sayıda tekil işlem içeren düşük eşzamanlılık senaryosunda, GÇ iş parçacığı toplu ikili günlük güncelleştirmelerini beklerken yalnızca birkaç işlem yürütüldüğünden, binlog_group_commit_sync_delay ayarlanması gecikme süresine eklenebilir. 

## <a name="next-steps"></a>Sonraki adımlar
[MySQL binlog çoğaltmasına genel bakış](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html)hakkında daha fazla bilgi edinin.
