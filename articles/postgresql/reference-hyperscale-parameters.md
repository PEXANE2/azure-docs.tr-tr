---
title: Sunucu parametreleri – Hyperscale (Hiperscale (Citus)-PostgreSQL için Azure veritabanı
description: Hyperscale (Citus) SQL API 'sindeki parametreler
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: reference
ms.date: 08/10/2020
ms.openlocfilehash: 57258540f3cd7b4c47b662b0885453cedd188e5b
ms.sourcegitcommit: 1aef4235aec3fd326ded18df7fdb750883809ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88136967"
---
# <a name="server-parameters"></a>Sunucu parametreleri

Hyperscale (Citus) davranışını etkileyen çeşitli sunucu parametreleri vardır, her iki parametre de standart PostgreSQL 'den ve Hyperscale 'e özgü parametrelerdir (Citus). PostgreSQL yapılandırma parametreleri hakkında daha fazla bilgi edinmek için PostgreSQL belgelerinin [çalışma zamanı yapılandırması](http://www.postgresql.org/docs/current/static/runtime-config.html) bölümünü ziyaret edebilirsiniz.

Bu başvurunun geri kalanı, aynı Hyperscale (Citus) belirli yapılandırma parametrelerini ele alacağından oluşur. Bu parametreler, bir hiper ölçek (Citus) sunucu grubu **ayarları** altındaki **çalışan düğümü parametreleri** altında Azure Portal ayarlanabilir.

> [!NOTE]
>
> Citus altyapısının eski sürümlerini çalıştıran hiper ölçekli sunucu grupları aşağıda listelenen tüm parametreleri sunmayabilir.

## <a name="general-configuration"></a>Genel yapılandırma

### <a name="citususe_secondary_nodes-enum"></a>citus. \_ İkincil \_ düğümleri kullan (enum)

Seçme sorguları için düğüm seçerken kullanılacak ilkeyi ayarlar. ' Always ' olarak ayarlanırsa, Planner yalnızca [pg_dist_node](reference-hyperscale-metadata.md#worker-node-table)' ikincil ' noderole olarak işaretlenen düğümleri sorgular.

Bu sabit listesi için desteklenen değerler şunlardır:

-   **asla:** (varsayılan) birincil düğümlerde tüm okumalar gerçekleşir.
-   **her zaman:** Bunun yerine ikincil düğümlere karşı çalıştırılan okuma ve INSERT/update deyimleri devre dışı bırakıldı.

### <a name="cituscluster_name-text"></a>citus. Cluster \_ adı (metin)

Koordinatör düğümü planlayıcısı 'nda hangi kümenin koordinatının olduğunu bildirir. Küme adı ayarlandıktan sonra \_ , Planner yalnızca o kümedeki çalışan düğümlerini sorgular.

### <a name="citusenable_version_checks-boolean"></a>citus. \_ Sürüm \_ denetimlerini etkinleştir (Boole)

Hiper ölçek (Citus) sürümünü yükseltmek için sunucu yeniden başlatması (yeni paylaşılan kitaplığı seçmek için) ve ardından ALTER EXTENSION UPDATE komutu gerekir.  İki adımı yürütme hatası muhtemelen hatalara veya kilitlenmelere neden olabilir.
Hiper ölçek (Citus), bu nedenle kodun sürümünü ve uzantı eşleşmesini doğrular ve hata vermez \' .

Bu değer varsayılan olarak true 'dur ve düzenleyici üzerinde etkilidir. Nadir durumlarda, karmaşık yükseltme işlemlerinde bu parametrenin false olarak ayarlanması, dolayısıyla denetimi devre dışı bırakılması gerekebilir.

### <a name="cituslog_distributed_deadlock_detection-boolean"></a>citus. log \_ Dağıtılmış \_ kilitlenme \_ algılaması (Boolean)

Dağıtılmış kilitlenme algılamayla ilgili işlemenin sunucu günlüğünde günlüğe kaydedilip edilmeyeceğini belirtir. Varsayılan olarak false değerini alır.

### <a name="citusdistributed_deadlock_detection_factor-floating-point"></a>citus. dağıtılmış \_ kilitlenme \_ algılama \_ faktörü (kayan nokta)

Dağıtılmış kilitlenmeleri denetlemeden önce beklenecek süreyi ayarlar. Bu değer özellikle bekleme süresi, PostgreSQL \' s [kilitlenme \_ zaman aşımı](https://www.postgresql.org/docs/current/static/runtime-config-locks.html) ayarıyla çarpılacak. Varsayılan değer: `2`. Değeri, `-1` Dağıtılmış kilitlenme algılamasını devre dışı bırakır.

### <a name="citusnode_connection_timeout-integer"></a>citus. Node \_ bağlantısı \_ zaman aşımı (tamsayı)

`citus.node_connection_timeout`Guc, bağlantı kurulması için beklenecek en uzun süreyi (milisaniye cinsinden) ayarlar. Hiper ölçek (Citus), en az bir çalışan bağlantısı oluşturulmadan önce zaman aşımı geçtiğinde bir hata oluşturur. Bu GUC, düzenleyicilere ve çalışanlara olan bağlantıları etkiler.

-   Varsayılan: beş saniye
-   En az: 10 milisaniye
-   Maksimum: bir saat

```postgresql
-- set to 30 seconds
ALTER DATABASE foo
SET citus.node_connection_timeout = 30000;
```

## <a name="query-statistics"></a>Sorgu Istatistikleri

### <a name="citusstat_statements_purge_interval-integer"></a>citus. stat \_ deyimlerini \_ Temizleme \_ aralığı (tamsayı)

Bakım arka plan programının ' de eşleşmeyen [citus_stat_statements](reference-hyperscale-metadata.md#query-statistics-table) kayıtları kaldırabileceği sıklığı ayarlar `pg_stat_statements` . Bu yapılandırma değeri, varsayılan değer olan 10 ' a kadar saniye cinsinden, zaman aralığını ayarlar. 0 değeri, temizler öğesini devre dışı bırakır.

```psql
SET citus.stat_statements_purge_interval TO 5;
```

Bu parametre düzenleyici üzerinde etkilidir ve çalışma zamanında değiştirilebilir.

## <a name="data-loading"></a>Veri yükleme

### <a name="citusmulti_shard_commit_protocol-enum"></a>citus. Multi \_ kıard \_ COMMIT \_ Protokolü (enum)

Karma Dağıtılmış bir tabloda kopyalama gerçekleştirirken kullanılacak tamamlama protokolünü ayarlar. Her bir parça yerleşimi üzerinde, kopyalama sırasında bir hata oluşursa hiçbir veri kullanılmadığından emin olmak için kopya bir işlem bloğunda gerçekleştirilir. Ancak, KOPYALAMANıN tüm yerleştirmede başarılı olduğu belirli bir hata durumu vardır ancak tüm işlemler kaydedilmeden önce bir (donanım) arızası oluşur. Bu parametre, aşağıdaki tamamlama protokolleri arasında seçim yaparak bu durumda veri kaybını engellemek için kullanılabilir:

-   **2PC:** (varsayılan) parça yerleşimi ÜZERINDE yapılan kopyanın Ilk kez PostgreSQL \' s [iki aşamalı işleme](http://www.postgresql.org/docs/current/static/sql-prepare-transaction.html) kullanılarak hazırlanmakta ve sonra kararlıdır. Başarısız işlemeler, sırasıyla yürütme veya GERI alma PREPARED kullanılarak el ile kurtarılabilir veya iptal edilebilir.
    2PC kullanılırken, genellikle en fazla [ \_ hazırlanan \_ işlem](http://www.postgresql.org/docs/current/static/runtime-config-resource.html) , en fazla bağlantı ile aynı değere sahip tüm çalışanlar üzerinde artmalıdır \_ .
-   **1pc:** Parça yerleştirmelerinin üzerinde gerçekleştirileceği işlemler tek bir turda kaydedilir. Tüm yerleştirmelerin (nadir) kopyalama başarılı olduktan sonra bir işlem başarısız olursa, veriler kaybolabilir.

### <a name="citusshard_replication_factor-integer"></a>citus. Shard \_ çoğaltma \_ faktörü (tamsayı)

Parçalar için çoğaltma faktörünü, parçaların yerleştirileceği düğüm sayısını ve varsayılan değer olan 1 ' i ayarlar. Bu parametre çalışma zamanında ayarlanabilir ve düzenleyici üzerinde etkilidir. Bu parametre için ideal değer kümenin boyutuna ve düğüm hatası hızına bağlıdır.  Örneğin, büyük kümeler çalıştırırsanız ve düğüm başarısızlıklarını daha sık bir sıklıkta gözlemlerseniz, bu çoğaltma faktörünü artırmak isteyebilirsiniz.

### <a name="citusshard_count-integer"></a>citus. Shard \_ sayısı (tamsayı)

Karma bölümlenmiş tablolar için parça sayısını ve varsayılan değer olarak 32 ' i belirler.  Bu değer, karma bölümlenmiş tablolar oluşturulurken [create_distributed_table](reference-hyperscale-functions.md#create_distributed_table) UDF tarafından kullanılır. Bu parametre çalışma zamanında ayarlanabilir ve düzenleyici üzerinde etkilidir.

### <a name="citusshard_max_size-integer"></a>citus. Shard \_ en büyük \_ boyutu (tamsayı)

Parçalanmadan önce bir parçanın büyüeceği en büyük boyutu ayarlar ve varsayılan olarak 1 GB olur. Bir parça için kaynak dosyası \' s boyutu (hazırlama için kullanılan) Bu yapılandırma değerini aşarsa, veritabanı yeni bir parçanın oluşturulmasını sağlar. Bu parametre çalışma zamanında ayarlanabilir ve düzenleyici üzerinde etkilidir.

## <a name="planner-configuration"></a>Planner yapılandırması

### <a name="cituslimit_clause_row_fetch_count-integer"></a>citus. limit \_ yan tümce \_ satırı \_ getirme \_ sayısı (tamsayı)

Limit yan tümcesi iyileştirmesi için görev başına getirilecek satır sayısını ayarlar.
Bazı durumlarda, sınır yan tümceleri olan sorguların seçim yapması, her bir görevden sonuçlar oluşturmak için tüm satırları getirmesi gerekebilir. Bu durumlarda ve yaklaşık anlamlı sonuçlar üretmesi durumunda, bu yapılandırma değeri her bir parçadan getirilecek satır sayısını ayarlar. Sınır sayısı varsayılan olarak devre dışıdır ve bu parametre-1 ' e ayarlanır. Bu değer, çalışma zamanında ayarlanabilir ve düzenleyici üzerinde etkilidir.

### <a name="cituscount_distinct_error_rate-floating-point"></a>citus. Count \_ ayrı \_ hata \_ oranı (kayan nokta)

Hiper ölçek (Citus), PostgreSQL-hll uzantısını kullanarak sayıyı (benzersiz) tahmin edebilir. Bu yapılandırma girişi, Count (benzersiz) hesaplanırken istenen hata oranını ayarlar. Varsayılan değer olan 0,0, sayı (benzersiz) için yaklaşık olarak devre dışı bırakır; ve 1,0 sonuçların doğruluğu hakkında garanti vermez. En iyi sonuçlar için bu parametreyi 0,005 olarak ayarlamayı öneririz. Bu değer, çalışma zamanında ayarlanabilir ve düzenleyici üzerinde etkilidir.

### <a name="citustask_assignment_policy-enum"></a>citus. Task \_ atama \_ ilkesi (enum)

> [!NOTE]
> Bu GUC yalnızca [shard_replication_factor](reference-hyperscale-parameters.md#citusshard_replication_factor-integer) bir veya daha büyük olduğunda veya [reference_tables](concepts-hyperscale-distributed-data.md#type-2-reference-tables)sorguları için geçerlidir.

Çalışanlara görev atarken kullanılacak ilkeyi ayarlar. Düzenleyici, işleri parça konumlarına göre çalışanlara atar. Bu yapılandırma değeri, bu atamalar yapılırken kullanılacak ilkeyi belirtir.
Şu anda kullanılabilecek üç olası görev atama ilkesi vardır.

-   **Greedy:** Doyumsuz ilkesi, çalışanlar arasında görevleri eşit bir şekilde dağıtmak için varsayılan ve amaçlar 'dir.
-   Hepsini bir **kez deneme:** Hepsini bir kez deneme ilkesi, işleri farklı çoğaltmalar arasında değişen bir şekilde bir kez daha kez çalışan bir biçimde atar. Bu ilke, bir tablonun parça sayısı, çalışan sayısına göre düşük olduğunda daha iyi küme kullanımını sağlar.
-   **ilk çoğaltma:** İlk çoğaltma ilkesi, parçaları için yerleştirme sırası (çoğaltmalar) temelinde görevler atar. Diğer bir deyişle, bir parça için parça sorgusu, bu parçanın ilk çoğaltması olan çalışana atanır.
    Bu yöntem, hangi parçalar için hangi parçaların kullanılacağı (diğer bir deyişle, daha güçlü bellek daha fazla garanti güvence altına alınır) için güçlü garanti sağlar.

Bu parametre çalışma zamanında ayarlanabilir ve düzenleyici üzerinde etkilidir.

## <a name="intermediate-data-transfer"></a>Ara Veri Aktarımı

### <a name="citusbinary_worker_copy_format-boolean"></a>citus. Binary \_ çalışan \_ kopya \_ biçimi (Boolean)

Çalışanlar arasında ara verileri aktarmak için ikili kopya biçimini kullanın.
Büyük tablo birleşimleri sırasında, hiper ölçek (Citus) farklı çalışanlar arasında verileri dinamik olarak yeniden bölümlemek ve karıştırmak zorunda kalabilir. Varsayılan olarak, bu veriler metin biçiminde aktarılır. Bu parametrenin etkinleştirilmesi, veritabanının bu verileri aktarmak için PostgreSQL 'in ikili serileştirme biçimini kullanmasını söyler. Bu parametre, çalışanlar üzerinde etkilidir ve PostgreSQL. conf dosyasında değiştirilmesi gerekir. Yapılandırma dosyasını düzenledikten sonra, kullanıcılar bu değişikliğin etkili olması için bir veya daha fazla sinyal gönderebilir veya sunucuyu yeniden başlatabilir.

### <a name="citusbinary_master_copy_format-boolean"></a>citus. Binary \_ ana \_ kopya \_ biçimi (Boolean)

Koordinatör ve çalışanlar arasında veri aktarmak için ikili kopya biçimini kullanın. Dağıtılmış sorgular çalıştırılırken, çalışanlar ara sonuçları son toplama için düzenleyiciye aktarır. Varsayılan olarak, bu veriler metin biçiminde aktarılır. Bu parametrenin etkinleştirilmesi, veritabanının bu verileri aktarmak için PostgreSQL 'in ikili serileştirme biçimini kullanmasını söyler.
Bu parametre, çalışma zamanında ayarlanabilir ve düzenleyici üzerinde etkin olabilir.

### <a name="citusmax_intermediate_result_size-integer"></a>citus. Max \_ Ara \_ sonuç \_ boyutu (tamsayı)

Yürütme için çalışan düğümlerine itilmeyecek ve karmaşık alt sorgularda, CTEs için ara sonuçların KB cinsinden en büyük boyutu. Varsayılan 1 GB 'tır ve-1 değeri sınır yoktur.
Sınırı aşan sorgular iptal edilecek ve bir hata mesajı oluşturacak.

## <a name="ddl"></a>DDL

### <a name="citusenable_ddl_propagation-boolean"></a>citus. \_ DDL \_ yaymayı etkinleştir (Boolean)

DDL değişikliklerinin düzenleyicilere otomatik olarak tüm çalışanlara yayıp yaymayacağını belirtir. Varsayılan değer true şeklindedir. Bazı şema değişiklikleri tablolar üzerinde özel bir erişim kilidi gerektirdiğinden ve otomatik yayma tüm çalışanlar için ardışık olarak yapıldığından, hiper ölçek (Citus) kümesini geçici olarak daha az yanıt verebilir hale getirebilirsiniz. Bu ayarı devre dışı bırakmayı ve değişiklikleri el ile yaymaya yönelik seçim yapabilirsiniz.

## <a name="executor-configuration"></a>Yürütücü yapılandırması

### <a name="general"></a>Genel

#### <a name="citusall_modifications_commutative"></a>citus. tüm \_ değişiklikler \_ yürütülüyor

Hiper ölçek (Citus), çalışma kuralları uygular ve davranışın doğruluğundan emin olmak için değiştirme işlemleri için uygun kilitleri alır. Örneğin, bir INSERT ifadesinin başka bir INSERT ifadesiyle ilgili olduğunu varsayar, ancak bir UPDATE veya DELETE ifadesiyle değildir. Benzer şekilde, bir UPDATE veya DELETE ifadesinin başka bir UPDATE veya DELETE ifadesiyle ilgili olduğunu varsaymaktadır. Bu önlem, güncelleştirmelerin ve silinmenin daha güçlü kilitler elde etmek için hiper ölçek (Citus) gerektirmesidir.

Eklemelerinizin veya diğer güncelleştirmelerinizin bulunduğu GÜNCELLEŞTIRME deyimlerine sahipseniz, bu parametreyi true olarak ayarlayarak bu ilgili bu varsayımları hafifleyerek rahatlığınızı yapabilirsiniz. Bu parametre true olarak ayarlandığında, tüm komutlar işlem için kabul edilir ve paylaşılan bir kilit talep edilir ve bu da genel üretilen işi geliştirebilirler. Bu parametre, çalışma zamanında ayarlanabilir ve düzenleyici üzerinde etkin olabilir.

#### <a name="citusremote_task_check_interval-integer"></a>citus. uzak \_ görev \_ Denetim \_ aralığı (tamsayı)

, Hiper ölçeğin (Citus), görev izleyici yürütücüsü tarafından yönetilen işlerin durumlarını denetlediği sıklığı ayarlar. Varsayılan olarak 10 MS olur. Düzenleyici çalışanları çalışanlara atar ve sonra her bir görev ilerlemesi hakkında düzenli olarak kontrol eder \' . Bu yapılandırma değeri iki arada bir denetim arasındaki zaman aralığını ayarlar. Bu parametre düzenleyici üzerinde etkilidir ve çalışma zamanında ayarlanabilir.

#### <a name="citustask_executor_type-enum"></a>citus. Task \_ yürütücü \_ türü (enum)

Hiper ölçekte (Citus) dağıtılmış SELECT sorguları çalıştırmak için üç yürütücü türü vardır.  İstenen yürütücü bu yapılandırma parametresi ayarlanarak seçilebilir. Bu parametre için kabul edilen değerler şunlardır:

-   **Uyarlamalı:** Varsayılan değer. Birden çok parça arasında yayılan toplamaları ve birlikte bulunan birleştirmeleri içeren sorgulara hızlı yanıt almak için idealdir.
-   **görev-İzleyici:** Görev izleyici yürütücüsü, çalışan düğümlere ve verimli kaynak yönetimine kadar veri karıştırma gerektiren uzun süre çalışan, karmaşık sorgular için idealdir.
-   **gerçek zamanlı:** (kullanım dışı) Uyarlamalı yürütücü olarak benzer bir amaca hizmet eder, ancak daha az esnektir ve çalışan düğümlerinde daha fazla bağlantı baskısı oluşmasına neden olabilir.

Bu parametre çalışma zamanında ayarlanabilir ve düzenleyici üzerinde etkilidir.

#### <a name="citusmulti_task_query_log_level-enum-multi_task_logging"></a>citus. Multi \_ görevi \_ sorgu \_ günlüğü \_ düzeyi (enum) {#multi_task_logging}

Birden fazla görev üreten herhangi bir sorgu için günlük düzeyi ayarlar (yani, birden fazla parça ziyaret eden). Birden çok kiracılı uygulama geçişi sırasında günlüğe kaydetme yararlı olur. bu şekilde, bu tür sorgular için hata veya uyarma yapabilir, bunları bulabilir ve bunlara bir kiracı \_ kimliği filtresi ekleyebilirsiniz. Bu parametre, çalışma zamanında ayarlanabilir ve düzenleyici üzerinde etkin olabilir. Bu parametre için varsayılan değer \' kapalıdır \' .

Bu sabit listesi için desteklenen değerler şunlardır:

-   **Kapalı:** Birden çok görev üreten tüm sorguları günlüğe kaydetmeyi devre dışı bırakma (yani birden çok parçaya yayın)
-   **Hata Ayıkla:** Hata ayıklama önem düzeyinde logs deyimleri.
-   **günlüğe kaydet:** Günlük önem düzeyinde LOG ifadesini kaydeder. Günlük satırına çalıştırılan SQL sorgusu dahil edilir.
-   **Uyarı:** Günlük bildirimini BILDIRIM önem düzeyine göre günlüğe kaydeder.
-   **Uyarı:** Uyarı önem derecesine sahip deyimleri günlüğe kaydeder.
-   **hata:** HATA önem derecesine sahip deyimleri günlüğe kaydeder.

`error`Geliştirme testi sırasında ve `log` gerçek üretim dağıtımı sırasında olduğu gibi daha düşük bir günlük düzeyinde kullanılması faydalı olabilir.
Seçim `log` yapmak, çok görev sorgularının sorgu tarafından gösterilen sorgu ile veritabanı günlüklerinde görünmesine neden olur \" .\"

```
LOG:  multi-task query about to be executed
HINT:  Queries are split to multiple tasks if they have to be split into several queries on the workers.
STATEMENT:  select * from foo;
```

#### <a name="citusenable_repartition_joins-boolean"></a>citus. yeniden \_ bölümleme \_ katılımları etkinleştir (Boolean)

Normalde, uyarlamalı yürütücü ile yeniden bölümleme birleştirmeleri gerçekleştirmeye çalışmak bir hata iletisiyle başarısız olur.  Bununla birlikte, `citus.enable_repartition_joins` doğru olarak ayarlanması, hiper ölçeklendirmeyi (Citus) geçici olarak görev izleyici Yürütücüsü ' ne, katılmayı gerçekleştirecek şekilde değiştirmesine izin verir.  Varsayılan değer false'tur.

### <a name="task-tracker-executor-configuration"></a>Görev izleyici yürütücü yapılandırması

#### <a name="citustask_tracker_delay-integer"></a>citus. Task \_ Tracker \_ gecikmesi (tamsayı)

Bu parametre, görev yönetimi ve varsayılan değer 200 ms olarak görev izleyici uyku süresini ayarlar. Görev izleyici işlemi düzenli olarak uyandırır, kendisine atanan tüm görevleri açıklar ve bu görevleri zamanlar ve yürütür.  Daha sonra görev izleyici, bu görevleri yeniden yürüten önce bir süre boyunca uyku moduna geçer.
Bu yapılandırma değeri, bu uyku döneminin uzunluğunu belirler. Bu parametre, çalışanlar üzerinde etkilidir ve PostgreSQL. conf dosyasında değiştirilmesi gerekir. Yapılandırma dosyasını düzenledikten sonra, kullanıcılar değişikliğin etkili olması için bir işlem sinyali gönderebilir veya sunucuyu yeniden başlatabilir.

Bu parametre, yönetim 'in her ne kadar zaman aralığı azaltıldığında, görev izleyici yürütücüsü nedeniyle oluşan gecikmeyi kırpmak için azaltılabilir.
Gecikmeyi azaltmak, parça sorgularının kısa olduğu durumlarda ve bu nedenle durumlarını düzenli olarak güncelleştirmede yararlıdır.

#### <a name="citusmax_assign_task_batch_size-integer"></a>citus. en \_ fazla \_ görev ata \_ toplu iş \_ boyutu (tamsayı)

Düzenleyicinin görev izleyici yürütücüsü, çalışanlara zaman uyumlu olarak görevler halinde görev atar. Bu parametre, tek bir toplu işte atanacak en fazla görev sayısını ayarlar. Daha büyük bir toplu iş boyutu seçilmesi daha hızlı görev atamasına izin verir. Ancak, çalışan sayısı büyükse, tüm çalışanların görevleri alması daha uzun sürebilir.  Bu parametre, çalışma zamanında ayarlanabilir ve düzenleyici üzerinde etkin olabilir.

#### <a name="citusmax_running_tasks_per_node-integer"></a>citus. \_ düğüm başına en fazla çalışan \_ görev \_ \_ (tamsayı)

Görev izleyici işlemi, uygun şekilde kendisine atanan görevleri zamanlar ve yürütür. Bu yapılandırma değeri, belirli bir zamanda bir düğümde aynı anda yürütülecek en fazla görev sayısını ayarlar ve varsayılan değer 8 ' dir.

Sınır, \' diskte aynı anda çok sayıda görev vurmasını ve disk g/ç çekişmesini önlemeye yardımcı olur. Sorgularınız bellekten veya SSDs 'den sunuluyorsa, \_ \_ çok önemli olmayan, düğüm başına en fazla çalışan görevi artırabilirsiniz \_ \_ .

#### <a name="cituspartition_buffer_size-integer"></a>citus. Partition \_ arabellek \_ boyutu (tamsayı)

Bölüm işlemleri için kullanılacak arabellek boyutunu ve varsayılan olarak 8 MB 'ı belirler.
Hiper ölçek (Citus), iki büyük tablo birleştirilirken tablo verilerinin birden çok dosyaya yeniden bölümlenmesi için izin verir. Bu bölüm arabelleği doldurulduktan sonra, yeniden bölümlenen veriler diskteki dosyalara temizlenir.  Bu yapılandırma girişi, çalışma zamanında ayarlanabilir ve çalışanlar üzerinde etkilidir.

### <a name="explain-output"></a>Çıktıyı açıkla

#### <a name="citusexplain_all_tasks-boolean"></a>citus. \_ tüm \_ görevleri açıkla (Boolean)

Varsayılan olarak, hiper ölçek (Citus), dağıtılmış bir sorguda [açıkla](http://www.postgresql.org/docs/current/static/sql-explain.html) çalışırken tek ve rastgele bir görevin çıkışını gösterir. Çoğu durumda, açıklama çıktısı görevler arasında benzerdir. Bazen bazı görevler farklı planlanacaktır veya çok daha yüksek yürütme zamanına sahip olur. Bu gibi durumlarda, bu parametreyi etkinleştirmek yararlı olabilir ve bu parametrenin ardından, açıklama çıkışının tüm görevleri içermesi gerekir. Tüm görevlerin açıklanması, anlayarak daha uzun sürmesine neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

* Sunucu parametrelerinin yanı sıra bir hiper ölçek (Citus) sunucu grubundaki kaynak [yapılandırma seçenekleri](concepts-hyperscale-configuration-options.md) de farklı bir yapılandırma biçimidir.
* Temel alınan PostgreSQL veri tabanında [yapılandırma parametreleri](http://www.postgresql.org/docs/current/static/runtime-config.html)de vardır.
