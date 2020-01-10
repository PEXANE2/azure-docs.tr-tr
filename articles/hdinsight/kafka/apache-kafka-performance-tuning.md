---
title: Apache Kafka HDInsight kümeleri için performans iyileştirmesi
description: Azure HDInsight 'ta Apache Kafka iş yüklerini iyileştirmeye yönelik tekniklerin genel bir bakış sunar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/19/2019
ms.openlocfilehash: 752068af531c4a0ecc832d266f88105c14452ecb
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/26/2019
ms.locfileid: "75494918"
---
# <a name="performance-optimization-for-apache-kafka-hdinsight-clusters"></a>Apache Kafka HDInsight kümeleri için performans iyileştirmesi

Bu makale, HDInsight 'ta Apache Kafka iş yüklerinizin performansını iyileştirmeye yönelik bazı öneriler sunar. Odak, Producer ve Broker yapılandırmasını ayarlamasıdır. Performansı ölçmenin farklı yolları vardır ve uyguladığınız iyileştirmeler iş gereksinimlerinize göre değişir.

## <a name="architecture-overview"></a>Mimariye genel bakış

Kafka konuları kayıtları düzenlemek için kullanılır. Kayıtlar, üreticileri tarafından üretilir ve tüketiciler tarafından kullanılır. Üreticileri kayıtları, daha sonra verileri depolayan Kafka aracılarına gönderir. HDInsight kümenizdeki her çalışan düğümü bir Kafka aracısıdır.

Aracılar arasında konuların bölüm kayıtları. Kayıtları tüketirken, verilerin paralel işlemesini elde etmek için bölüm başına en fazla bir tüketici kullanabilirsiniz.

Çoğaltma, düğümler arasında bölümleri çoğaltmak için kullanılır. Bu, düğüm (aracı) kesintilerine karşı koruma sağlar. Çoğaltma grubu arasındaki tek bir bölüm, bölüm lideri olarak atanır. Üretici trafiği ZooKeeper tarafından yönetilen durum kullanılarak her düğümün liderine yönlendirilir.

## <a name="identify-your-scenario"></a>Senaryonuzu tanımlama

Apache Kafka performansın iki ana yönü vardır: aktarım hızı ve gecikme süresi. Verimlilik, verilerin işlenebileceği en yüksek hızdır. Daha yüksek aktarım hızı genellikle daha iyidir. Gecikme süresi, verilerin depolanması veya alınması için gereken süredir. Daha düşük gecikme süresi genellikle daha iyidir. Verimlilik, gecikme süresi ve uygulamanın altyapısının maliyeti arasındaki doğru dengeyi bulma zor olabilir. Performans gereksinimleriniz büyük olasılıkla yüksek aktarım hızı, düşük gecikme süresi veya her ikisinin de gerekli olup olmadığına bağlı olarak aşağıdaki üç yaygın durumlardan biriyle eşleşir:

* Yüksek performans, düşük gecikme süresi. Bu senaryo hem yüksek aktarım hızı hem de düşük gecikme süresi (~ 100 milisaniye) gerektirir. Bu tür bir uygulama örneği hizmet kullanılabilirliği izlemedir.
* Yüksek verimlilik, yüksek gecikme süresi. Bu senaryo yüksek aktarım hızı (~ 1,5 GBps) gerektirir, ancak daha yüksek gecikme süresine (< 250 MS) göre tolerans sağlayabilir. Bu tür bir uygulamaya örnek olarak, güvenlik ve yetkisiz giriş algılama uygulamaları gibi gerçek zamanlı işlemlere yönelik telemetri verileri alma işlemi yer aldığı bir örnektir.
* Düşük performans, düşük gecikme süresi. Bu senaryo gerçek zamanlı işleme için düşük gecikme süresi (< 10 MS) gerektirir, ancak düşük aktarım hızına yanıt verebilir. Bu tür bir uygulama, çevrimiçi yazım ve dilbilgisi denetimleri örneğidir.

## <a name="producer-configurations"></a>Üretici yapılandırması

Aşağıdaki bölümler, Kafka üreticileri 'in performansını iyileştirmek için en önemli yapılandırma özelliklerinden bazılarını vurgulayacaktır. Tüm yapılandırma özelliklerinin ayrıntılı bir açıklaması için bkz. [üretici yapılandırmalarında Apache Kafka belgeleri](https://kafka.apache.org/documentation/#producerconfigs).

### <a name="batch-size"></a>Toplu işlem boyutu

Apache Kafka üreticileri, tek bir depolama bölümünde depolanacak bir birim olarak gönderilen ileti gruplarını (toplu işler olarak adlandırılır) birleştirir. Toplu iş boyutu, Grup iletilmeden önce bulunması gereken bayt sayısını gösterir. `batch.size` parametresinin artırılması, ağ ve GÇ isteklerindeki işleme yükünü azalttığından üretilen işi artırabilir. Bir toplu işin hazırlanmasını beklediği için, hafif yük altında artan toplu iş boyutu Kafka gönderme gecikmesini artırabilir. Ağır yük altında, toplu iş boyutunu artırmanız ve gecikme süresini artırmak için önerilir.

### <a name="producer-required-acknowledgments"></a>Üretici gerekli bildirimleri

Üretici için gereken `acks` yapılandırması, bir yazma isteğinin tamamlandığı kabul edilmeden önce bölüm lideri tarafından gerekli olan bildirimler sayısını belirler. Bu ayar, veri güvenilirliğini etkiler ve `0`, `1`veya `-1`değerlerini alır. `-1` değeri, yazma işlemi tamamlanmadan önce tüm çoğaltmalardan bir bildirim alınması gerektiği anlamına gelir. `acks = -1` ayarlama, veri kaybına karşı daha güçlü garantiler sağlar, ancak Ayrıca gecikme süresi ve daha düşük aktarım hızı ile sonuçlanır. Uygulama gereksinimleriniz daha yüksek verimlilik talebinde bulunursa `acks = 0` veya `acks = 1`ayarlamayı deneyin. Tüm çoğaltmaları bildirmeden veri güvenilirliğini azaltamayacağınızı göz önünde bulundurun.

### <a name="compression"></a>Sıkıştırma

Bir Kafka üreticisi, aracıları aracılarına göndermeden önce iletileri sıkıştırmak üzere yapılandırılabilir. `compression.type` ayarı kullanılacak sıkıştırma codec 'ini belirtir. Desteklenen sıkıştırma codec bileşenleri şunlardır "gzip," "Snappy," ve "lz4." Sıkıştırma yararlı olur ve disk kapasitesinde bir kısıtlama varsa göz önünde bulundurulmalıdır.

Yaygın olarak kullanılan iki sıkıştırma codec bileşeni arasında `gzip` ve `snappy`, `gzip` daha yüksek bir sıkıştırma oranına sahiptir ve bu da daha yüksek CPU yükü maliyetinde daha düşük disk kullanımı elde olur. `snappy` codec bileşeni daha az CPU ek yükü ile daha az sıkıştırma sağlar. Aracı diskine veya üretici CPU kısıtlamalarına göre hangi codec bileşeninin kullanılacağına karar verebilirsiniz. `gzip` verileri `snappy`beş kat daha yüksek bir hızda sıkıştırabilirler.

Veri sıkıştırmayı kullanmak, bir diskte depolanabilecek kayıt sayısını artırır. Ayrıca, üretici ve aracı tarafından kullanılan sıkıştırma biçimleri arasında uyuşmazlık olduğu durumlarda CPU yükünü artırabilir. verilerin gönderilmeden önce sıkıştırılması ve sonra, işlenmeden önce sıkıştırması açılması gerekir.

## <a name="broker-settings"></a>Aracı Ayarları

Aşağıdaki bölümler Kafka aracılarınızın performansını iyileştirmek için en önemli ayarların bazılarını vurgulayacaktır. Tüm aracı ayarlarının ayrıntılı bir açıklaması için bkz. [üretici yapılandırmalarında Apache Kafka belgeleri](https://kafka.apache.org/documentation/#producerconfigs).

### <a name="number-of-disks"></a>Disk sayısı

Depolama disklerinin, saniyede sınırlı ıOPS (saniye başına giriş/çıkış Işlemi) ve okuma/yazma baytları vardır. Yeni bölümler oluştururken, Kafka her yeni bölümü, kullanılabilir diskler arasında dengelemek için en az mevcut bölümlerle diskte depolar. Depolama stratejisine karşın, her diskte yüzlerce bölüm çoğaltmasını işlerken Kafka, kullanılabilir disk verimini kolayca doygunluğu sağlayabilir. Zorunluluğunu getirir, üretilen iş ve maliyet arasındadır. Uygulamanız daha fazla verimlilik gerektiriyorsa, aracı başına daha fazla yönetilen disk içeren bir küme oluşturun. HDInsight Şu anda çalışan bir kümeye yönetilen disklerin eklenmesini desteklememektedir. Yönetilen disklerin sayısını yapılandırma hakkında daha fazla bilgi için bkz. [HDInsight üzerinde Apache Kafka depolama ve ölçeklenebilirlik yapılandırma](apache-kafka-scalability.md). Kümenizdeki düğümlerin depolama alanını artırma maliyeti etkilerini anlayın.

### <a name="number-of-topics-and-partitions"></a>Konu başlıkları ve bölüm sayısı

Kafka üreticileri yazma konuları. Kafka tüketicileri, konulardan okundu. Bir konu, disk üzerindeki veri yapısı olan bir günlük ile ilişkilendirilir. Kafka bir üretici tarafından kayıtları bir konu günlüğünün sonuna ekler. Konu günlüğü, birden fazla dosyanın üzerine yayılan birçok bölümden oluşur. Bu dosyalar, sırayla birden çok Kafka küme düğümüne yayılır. Tüketiciler, Kafka konularındaki konuları okur ve konu günlüğünde konumunu (uzaklığında) seçebilir.

Her Kafka bölümü sistemdeki bir günlük dosyasıdır ve üretici iş parçacıkları aynı anda birden çok günlüğe yazabilir. Benzer şekilde, her tüketici iş parçacığı bir bölümden iletileri okuduğundan, birden çok bölümden alınan iletiler de paralel olarak işlenir.

Bölüm yoğunluğunu artırma (aracı başına bölüm sayısı), bölüm lideri ve onun izleyicileri arasında bölüm isteği/yanıtı ile ilgili bir ek yük ekler. Veri akışı sırasında, Bölüm çoğaltmaları da, ağ üzerinden gönderme ve alma istekleri için ek işleme ile sonuçlanan liderlerden veri getirmeye devam eder.

HDInsight 'ta Apache Kafka kümeleri 1,1 ve üzeri için, çoğaltmalar dahil olmak üzere her aracı için en fazla 1000 bölüm kullanmanızı öneririz. Aracı başına bölüm sayısının artırılması performansı düşürür ve ayrıca konunun kullanılamamasına neden olabilir. Kafka bölüm desteği hakkında daha fazla bilgi için, [1.1.0 sürümündeki desteklenen bölüm sayısı artışına göre resmi Apache Kafka blog gönderisine](https://blogs.apache.org/kafka/entry/apache-kafka-supports-more-partitions)bakın. Konuları değiştirme hakkında daha fazla bilgi için bkz. [Apache Kafka: konuları değiştirme](https://kafka.apache.org/documentation/#basic_ops_modify_topic).

### <a name="number-of-replicas"></a>Çoğaltma sayısı

Daha yüksek çoğaltma faktörü, bölüm lideri ve izleyicileri arasında ek isteklere neden olur. Sonuç olarak, daha yüksek bir çoğaltma faktörü ek istekleri işlemek için daha fazla disk ve CPU kullanır, yazma gecikmesini artırır ve performansı azaltır.

Azure HDInsight 'ta Kafka için en az 3x çoğaltmasını kullanmanızı öneririz. Çoğu Azure bölgesinin üç hata etki alanı vardır, ancak yalnızca iki hata etki alanına sahip bölgelerde, kullanıcıların 4X çoğaltma kullanması gerekir.

Çoğaltma hakkında daha fazla bilgi için bkz. [Apache Kafka: çoğaltma](https://kafka.apache.org/documentation/#replication) ve [Apache Kafka: çoğaltma faktörünü artırma](https://kafka.apache.org/documentation/#basic_ops_increase_replication_factor).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure üzerinde Apache Kafka ile günde milyarlarca olayı işleme](https://azure.microsoft.com/blog/processing-trillions-of-events-per-day-with-apache-kafka-on-azure/)
* [HDInsight 'ta Apache Kafka nedir?](apache-kafka-introduction.md)
