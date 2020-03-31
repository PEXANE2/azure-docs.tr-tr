---
title: Apache Kafka HDInsight kümeleri için performans iyileştirme
description: Azure HDInsight'ta Apache Kafka iş yüklerini optimize etme tekniklerine genel bir bakış sağlar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/19/2019
ms.openlocfilehash: 752068af531c4a0ecc832d266f88105c14452ecb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75494918"
---
# <a name="performance-optimization-for-apache-kafka-hdinsight-clusters"></a>Apache Kafka HDInsight kümeleri için performans iyileştirme

Bu makalede, HDInsight'ta Apache Kafka iş yüklerinizin performansını optimize etmek için bazı öneriler yer alıyor. Odak noktası üretici ve broker yapılandırmasını ayarlamaktır. Performansı ölçmenin farklı yolları vardır ve uyguladığınız optimizasyonlar iş gereksinimlerinize bağlıdır.

## <a name="architecture-overview"></a>Mimariye genel bakış

Kafka konuları kayıtları düzenlemek için kullanılır. Kayıtlar, Üreticiler tarafından oluşturulur ve tüketiciler tarafından kullanılır. Üreticiler kayıtları Kafka brokerlarına gönderir, daha sonra verileri saklarlar. HDInsight kümenizdeki her çalışan düğümü bir Kafka aracısıdır.

Aracılar arasında konuların bölüm kayıtları. Kayıtları tüketirken, verilerin paralel işlemesini elde etmek için bölüm başına en fazla bir tüketici kullanabilirsiniz.

Çoğaltma düğümleri arasında bölümleri çoğaltmak için kullanılır. Bu düğüm (broker) kesintileri karşı korur. Yinelemeler grubu arasında tek bir bölüm bölüm lideri olarak adatır. Üretici trafiği ZooKeeper tarafından yönetilen durum kullanılarak her düğümün liderine yönlendirilir.

## <a name="identify-your-scenario"></a>Senaryonuzu tanımlama

Apache Kafka performansının iki ana yönü vardır – iş ve gecikme. İşlem, verilerin işlenebileceği maksimum orandır. Daha yüksek iş artışı genellikle daha iyidir. Gecikme süresi, verilerin depolanması veya alınması için gereken süredir. Düşük gecikme genellikle daha iyidir. İş sonu, gecikme süresi ve uygulama altyapısının maliyeti arasında doğru dengeyi bulmak zor olabilir. Performans gereksinimleriniz, yüksek iş sonu, düşük gecikme veya her ikisine gerek olup olmadığınıza bağlı olarak büyük olasılıkla aşağıdaki üç yaygın durumdan biriyle eşleşir:

* Yüksek iş, düşük gecikme. Bu senaryo hem yüksek iş ve düşük gecikme (~ 100 milisaniye) gerektirir. Bu tür bir uygulama örneği hizmet kullanılabilirliği izlemedir.
* Yüksek iş, yüksek gecikme. Bu senaryo yüksek iş kolu gerektirir (~1,5 GBps) ancak daha yüksek gecikmeyi tolere edebilir (< 250 ms). Bu tür uygulamalara örnek olarak, güvenlik ve izinsiz giriş algılama uygulamaları gibi yakın gerçek zamanlı işlemler için telemetri veri sindirilmesi örnektir.
* Düşük iş, düşük gecikme. Bu senaryo, gerçek zamanlı işleme için düşük gecikme süresi (< 10 ms) gerektirir, ancak daha düşük iş ortalığını tolere edebilir. Bu tür bir uygulama örneği çevrimiçi yazım ve dilbilgisi denetimleridir.

## <a name="producer-configurations"></a>Üretici yapılandırmaları

Aşağıdaki bölümlerde Kafka üreticilerinizin performansını optimize etmek için en önemli yapılandırma özelliklerinden bazıları vurgulanacaktır. Tüm yapılandırma özelliklerinin ayrıntılı bir açıklaması [için, üretici yapılandırmaları hakkındaki Apache Kafka belgelerine](https://kafka.apache.org/documentation/#producerconfigs)bakın.

### <a name="batch-size"></a>Toplu iş boyutu

Apache Kafka üreticileri, tek bir depolama bölmesinde depolanacak bir birim olarak gönderilen ileti gruplarını (toplu iş olarak adlandırılır) bir araya toplarlar. Toplu iş boyutu, bu grup iletilmeden önce bulunması gereken bayt sayısı anlamına gelir. Parametrenin `batch.size` artırılması, ağ ve IO isteklerinin işlem ek lerini azalttığıiçin iş verisini artırabilir. Hafif yük altında, artan toplu iş boyutu Kafka'nın gecikme süresini artırabilir, çünkü üretici bir partinin hazır olmasını bekler. Ağır yük altında, iş ve gecikmeyi artırmak için toplu iş boyutunu n artırılması önerilir.

### <a name="producer-required-acknowledgments"></a>Üretici gerekli bildirimler

Üretici nin `acks` gerekli yapılandırması, yazma isteği tamamlanmadan önce bölüm lideri tarafından gerekli olan bildirim sayısını belirler. Bu ayar veri güvenilirliğini etkiler `0`ve `1`" `-1`veya . Değer, `-1` yazma tamamlanmadan önce tüm yinelemelerden bir bildirim alınması gerektiği anlamına gelir. Ayar, `acks = -1` veri kaybına karşı daha güçlü garantiler sağlar, ancak aynı zamanda daha yüksek gecikme ve daha düşük iş kaynağı sağlar. Başvuru gereksinimleriniz daha yüksek iş `acks = 0` bilgililiği talep ederse, ayarlamayı deneyin veya `acks = 1`. Tüm yinelemeleri kabul etmemenin veri güvenilirliğini azaltabileceğini unutmayın.

### <a name="compression"></a>Sıkıştırma

Bir Kafka üreticisi, iletileri brokerlere göndermeden önce sıkıştıracak şekilde yapılandırılabilir. Ayar, `compression.type` kullanılacak sıkıştırma codec'ini belirtir. Desteklenen sıkıştırma codec "gzip", "snappy" ve "lz4." Sıkıştırma yararlıdır ve disk kapasitesinde bir sınırlama varsa göz önünde bulundurulmalıdır.

İki yaygın olarak kullanılan sıkıştırma codec `snappy` `gzip` arasında, `gzip` ve, daha yüksek bir sıkıştırma oranı vardır, hangi daha yüksek CPU yükü pahasına daha düşük disk kullanımı ile sonuçlanır. Codec daha `snappy` az CPU yükü ile daha az sıkıştırma sağlar. Broker diskveya üretici CPU sınırlamalarına göre hangi codec'in kullanılacağına karar verebilirsiniz. `gzip`verileri beş kat daha yüksek `snappy`bir oranda sıkıştırabilir.

Veri sıkıştırma kullanarak bir diskte depolanabilir kayıt sayısını artırır. Ayrıca, üretici ve komisyoncu tarafından kullanılan sıkıştırma biçimleri arasında bir uyumsuzluk olduğu durumlarda CPU genel merkezlerini artırabilir. veriler göndermeden önce sıkıştırılmalı ve işlemeden önce dekomprese edilmelidir.

## <a name="broker-settings"></a>Broker ayarları

Aşağıdaki bölümlerde Kafka brokerlarınızın performansını optimize etmek için en önemli ayarlardan bazıları vurgulanacaktır. Tüm broker ayarlarının ayrıntılı bir açıklaması [için, üretici yapılandırmaları hakkında Apache Kafka belgelerine](https://kafka.apache.org/documentation/#producerconfigs)bakın.

### <a name="number-of-disks"></a>Disk sayısı

Depolama diskleri sınırlı IOPS (Saniyede Giriş/Çıkış İşlemleri) ve saniyede bayt okuma/yazma vardır. Kafka, yeni bölümler oluştururken, her yeni bölümü kullanılabilir diskler arasında dengelemek için varolan en az bölümle birlikte depolar. Depolama stratejisine rağmen, her diskte yüzlerce bölüm yinelemesi işlenirken Kafka kullanılabilir disk verime sini kolayca doygunlaştırabilir. Buradaki denge, maliyet ve maliyet arasındadır. Uygulamanız daha fazla iş elde etmeyi gerektiriyorsa, aracı başına daha yönetilen disklere sahip bir küme oluşturun. HDInsight şu anda çalışan bir kümeye yönetilen diskler eklemeyi desteklemiyor. Yönetilen disk lerin sayısının nasıl yapılandırılabildiği hakkında daha fazla bilgi için [HDInsight'ta Apache Kafka için depolama yı ve ölçeklenebilirliği yapılandırın'](apache-kafka-scalability.md)a bakın. Kümenizdeki düğümler için depolama alanını artırmanın maliyet sonuçlarını anlayın.

### <a name="number-of-topics-and-partitions"></a>Konu ve bölüm sayısı

Kafka yapımcıları konulara yazıyorlar. Kafka tüketicileri konuları okuyor. Bir konu, diskteki bir veri yapısı olan bir günlükle ilişkilidir. Kafka, bir üreticinin (lar) bir konu günlüğünün sonuna kadar olan kayıtları ekler. Konu günlüğü, birden çok dosyaya yayılan birçok bölümden oluşur. Bu dosyalar sırayla birden fazla Kafka küme düğümüne yayılmıştır. Tüketiciler kafka konularını kendi ortamlarında okuyabilirler ve konu günlüğündeki konumlarını (ofset) seçebilirler.

Her Kafka bölümü sistemdeki bir günlük dosyasıdır ve üretici iş parçacıkları aynı anda birden çok günlüklere yazabilir. Benzer şekilde, her tüketici iş parçacığı bir bölümden iletileri okuduğundan, birden çok bölümden alan tüketim de paralel olarak işlenir.

Bölüm yoğunluğunu (aracı başına bölüm sayısının) artırılması, meta veri işlemleri ve bölüm lideri ile takipçileri arasındaki bölüm isteği/yanıtı yla ilgili bir ek yükü ekler. Üzerinden akan veri yokluğunda bile, bölüm yinelemeleri yine de liderlerden veri getirir ve bu da ağ üzerinden istek gönderme ve alma için ekstra işlemle sonuçlanır.

HDInsight'taki Apache Kafka kümeleri 1.1 ve üzeri için, yinelemeler de dahil olmak üzere broker başına en fazla 1000 bölüme sahip olmamızı öneririz. Aracı başına bölüm sayısını artırmak iş ortasını azaltır ve aynı zamanda konunun kullanılamamasına neden olabilir. Kafka bölüm desteği hakkında daha fazla bilgi için, [sürüm 1.1.0 desteklenen bölüm sayısındaki artış resmi Apache Kafka blog yazısı](https://blogs.apache.org/kafka/entry/apache-kafka-supports-more-partitions)bakın. Konuları değiştirme hakkında ayrıntılı bilgi için [Bkz. Apache Kafka: konuları değiştirme.](https://kafka.apache.org/documentation/#basic_ops_modify_topic)

### <a name="number-of-replicas"></a>Yineleme sayısı

Daha yüksek çoğaltma faktörü, bölüm lideri ve takipçiler arasında ek istekler ile sonuçlanır. Sonuç olarak, daha yüksek bir çoğaltma faktörü ek istekleri işlemek için daha fazla disk ve CPU tüketir, yazma gecikmesini artırır ve iş çıktısını azaltır.

Azure HDInsight'ta Kafka için en az 3x çoğaltma kullanmanızı öneririz. Çoğu Azure bölgesinde üç hata etki alanı vardır, ancak yalnızca iki hata etki alanı olan bölgelerde, kullanıcılar 4x çoğaltma kullanmalıdır.

Çoğaltma hakkında daha fazla bilgi için, [Bkz. Apache Kafka: çoğaltma](https://kafka.apache.org/documentation/#replication) ve [Apache Kafka: artan çoğaltma faktörü](https://kafka.apache.org/documentation/#basic_ops_increase_replication_factor).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure üzerinde Apache Kafka ile günde milyarlarca olayı işleme](https://azure.microsoft.com/blog/processing-trillions-of-events-per-day-with-apache-kafka-on-azure/)
* [HDInsight üzerinde Apache Kafka nedir?](apache-kafka-introduction.md)
