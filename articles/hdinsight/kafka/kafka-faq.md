---
title: Azure HDInsight'ta Apache Kafka hakkında SSS
description: Yönetilen bir Hadoop bulut hizmeti olan Azure HDInsight'ta Apache Kafka ile ilgili sık sorulan soruların yanıtlarını alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/14/2019
ms.openlocfilehash: d27c648980338b3a9e12bd618eb4620fe9988fd7
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80436901"
---
# <a name="frequently-asked-questions-about-apache-kafka-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Kafka hakkında sık sorulan sorular

Bu makale, Azure HDInsight'ta Apache Kafka ile ilgili bazı sık sorulan soruları ele adaıdır.

## <a name="what-kafka-versions-are-supported-by-hdinsight"></a>Hangi Kafka versiyonları HDInsight tarafından desteklenir?

HDInsight resmi olarak desteklenen bileşen sürümleri hakkında daha fazla bilgi bulabilirsiniz [Apache Hadoop bileşenleri ve HDInsight ile kullanılabilir sürümleri nelerdir?](../hdinsight-component-versioning.md#supported-hdinsight-versions) Mümkün olan en iyi performansı ve kullanıcı deneyimini sağlamak için her zaman en son sürümü kullanmanızı öneririz.

## <a name="what-resources-are-provided-in-an-hdinsight-kafka-cluster-and-what-resources-am-i-charged-for"></a>HDInsight Kafka kümesinde hangi kaynaklar sağlanır ve hangi kaynaklar için ücretlendirilirim?

BIR HDInsight Kafka kümesi aşağıdaki kaynakları içerir:

* Kafa düğümleri
* Zookeeper düğümleri
* Broker (işçi) düğümleri 
* Broker düğümlerine bağlı Azure Yönetilen Diskler
* Ağ geçidi düğümleri

Bu kaynakların tümü, ağ geçidi düğümleri hariç [HDInsight fiyatlandırma modelimize](https://azure.microsoft.com/pricing/details/hdinsight/)göre ücretlendirilir. Ağ geçidi düğümleri için ücretlendirilmez.

Çeşitli düğüm türlerinin daha ayrıntılı bir açıklaması için [Azure HDInsight sanal ağ mimarisine](../hdinsight-virtual-network-architecture.md)bakın. Fiyatlandırma, dakika düğümü başına kullanımına bağlıdır. Fiyatlar düğüm boyutuna, düğüm sayısına, kullanılan yönetilen disk türüne ve bölgeye bağlı olarak değişir.

## <a name="do-apache-kafka-apis-work-with-hdinsight"></a>Apache Kafka API'leri HDInsight ile çalışıyor mu?

Evet, HDInsight yerli Kafka API'larını kullanır. İstemci uygulama kodunuzu değiştirmeniz gerekmez. Bkz. Öğretici: Kümenizde Java tabanlı üretici/tüketici API'lerini nasıl kullanabileceğinizi görmek için [Apache Kafka Üretici ve Tüketici API'lerini kullanın.](./apache-kafka-producer-consumer-api.md)

## <a name="can-i-change-cluster-configurations"></a>Küme yapılandırmalarını değiştirebilir miyim?

Evet, Ambari portalından. Portaldaki her bileşen, bileşen yapılandırmalarını değiştirmek için kullanılabilecek bir **configs** bölümüne sahiptir. Bazı değişiklikler broker yeniden başlatılmasını gerektirebilir.

## <a name="what-type-of-authentication-does-hdinsight-support-for-apache-kafka"></a>HDInsight, Apache Kafka için ne tür bir kimlik doğrulamasını destekliyor?

[Kurumsal Güvenlik Paketi'ni (ESP)](../domain-joined/apache-domain-joined-architecture.md)kullanarak Kafka kümeleri için konu düzeyinde güvenlik elde edebilirsiniz. Bkz. Öğretici: Daha fazla bilgi için [Kurumsal Güvenlik Paketi (Önizleme) ile HDInsight'ta Apache Kafka politikalarını yapılandırın.](../domain-joined/apache-domain-joined-run-kafka.md)

## <a name="is-my-data-encrypted-can-i-use-my-own-keys"></a>Verilerim şifrelendi mi? Kendi anahtarlarımı kullanabilir miyim?

Yönetilen disklerde ki tüm Kafka iletileri [Azure Depolama Hizmeti Şifrelemesi (SSE)](../../storage/common/storage-service-encryption.md)ile şifrelenir. Veri-in-aktarım (örneğin, veri istemcilerden aracılara aktarılan ve diğer yolu) varsayılan olarak şifrelenmez. TlS'yi kendi başına kurarak bu tür trafiği [şifrelemek mümkündür.](./apache-kafka-ssl-encryption-authentication.md) Ayrıca, HDInsight istirahat verileri şifrelemek için kendi anahtarlarını yönetmenize olanak sağlar. Daha fazla bilgi için [Müşteri tarafından yönetilen anahtar disk şifrelemeye](../disk-encryption.md)bakın.

## <a name="how-do-i-connect-clients-to-my-cluster"></a>İstemcileri kümeme nasıl bağlarım?

Kafka müşterilerinin Kafka brokerlarıyla iletişim kurabilmeleri için, a.ş. üzerinden brokerlara ulaşabilmeleri gerekir. HDInsight kümeleri için Sanal Ağ (VNet) güvenlik sınırıdır. Bu nedenle, istemcileri HDInsight kümenize bağlamanın en kolay yolu kümeyle aynı VNet içinde istemciler oluşturmaktır. Diğer senaryolar şunlardır:

* Müşterileri farklı bir Azure VNet'te bağlama – VNet kümesini eşler ve istemci VNet'i eşler ve [KÜMEyi IP Advertising](apache-kafka-connect-vpn-gateway.md#configure-kafka-for-ip-advertising)için yapılandırın. KAFKA müşterileri IP reklamcılığı nı kullanırken, Tam Nitelikli Alan Adları (FQDNs) yerine brokerlerle bağlantı kurmak için Broker IP adreslerini kullanmalıdır.

* Şirket içi istemcileri bağlama – Vpn ağı kullanma ve [Azure HDInsight için sanal ağ planı'nda](../hdinsight-plan-virtual-network-deployment.md)açıklandığı gibi özel DNS sunucuları ayarlama.

* Kafka hizmetiniz için genel bir bitiş noktası oluşturma – Kurumsal güvenlik gereksinimleriniz izin veriyorsa, Kafka brokerlarınız için bir ortak bitiş noktası veya ortak bir bitiş noktasıyla kendi kendini yöneten açık kaynak REST bitiş noktası dağıtabilirsiniz.

## <a name="can-i-add-more-disk-space-on-an-existing-cluster"></a>Varolan bir kümeye daha fazla disk alanı ekleyebilir miyim?

Kafka iletileri için kullanılabilir alan miktarını artırmak için düğüm sayısını artırabilirsiniz. Şu anda, varolan bir kümeye daha fazla disk eklemek desteklenmez.

## <a name="can-a-kafka-cluster-work-with-databricks"></a>Kafka kümesi Databricks ile çalışabilir mi? 

Evet, Kafka kümeleri databricks ile aynı VNet'te oldukları sürece çalışabilirler. Databricks içeren bir Kafka kümesi kullanmak için, içinde HDInsight Kafka kümesi bulunan bir VNet oluşturun ve Databricks çalışma alanınızı oluştururken ve VNet enjeksiyonunu kullandığınızda VNet'in bu kümeyi belirtin. Daha fazla bilgi için azure [sanal ağınızda (VNet Injection) Azure Veri Tuğlaları Dağıt'a](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)bakın. Databricks çalışma alanını oluştururken Kafka kümesinin bootstrap broker adlarını sağlamanız gerekir. Kafka broker adlarını alma hakkında daha fazla bilgi için [Apache Zookeeper ve Broker host bilgilerini alın.](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-get-started#getkafkainfo)

## <a name="how-can-i-have-maximum-data-durability"></a>Maksimum veri dayanıklılığına nasıl sahip olabilirim?

Veri dayanıklılığı, ileti kaybı riskini en düşük şekilde elde etmenizi sağlar. Maksimum veri dayanıklılığı elde etmek için aşağıdaki ayarları öneririz:

* çoğu bölgede en az 3 çoğaltma faktörü kullanın
* yalnızca iki hata etki alanı olan bölgelerde en az 4 çoğaltma faktörü kullanın
* kirli lider seçimlerini devre dışı
* **set min.insync.replicas** 2 veya daha fazla - Bu bir yazma devam etmeden önce lideri ile tamamen senkronize olması gereken çoğaltmaların sayısını değiştirir
* **tüm** **acks** özelliği ayarlamak - bu özellik tüm iletileri kabul etmek için tüm yinelemeler gerektirir

Kafka'yı daha yüksek veri tutarlılığı için yapılandırmak, istek üretmek için aracıların kullanılabilirliğini etkiler.

## <a name="can-i-replicate-my-data-to-multiple-clusters"></a>Verilerimi birden çok kümeye çoğaltabilir miyim?

Evet, kafka MirrorMaker kullanılarak veriler birden çok kümeye çoğaltılabilir. MirrorMaker kurma ile ilgili ayrıntılara bakın [Mirror Apache Kafka konular](apache-kafka-mirroring.md)bulunabilir. Ayrıca, [Brooklin](https://github.com/linkedin/Brooklin/)gibi birden çok kümeye çoğaltma elde yardımcı olabilir diğer kendi kendini yöneten açık kaynak teknolojileri ve satıcıları vardır.

## <a name="can-i-upgrade-my-cluster-how-should-i-upgrade-my-cluster"></a>Kümemi yükseltebilir miyim? Kümemi nasıl yükseltmem gerekir?

Şu anda yerinde küme sürümü yükseltmelerini destekliyoruz. Kümenizi daha yüksek bir Kafka sürümüne güncellemek için, istediğiniz sürümle yeni bir küme oluşturun ve Kafka istemcilerinizi yeni kümeyi kullanmak üzere geçirin.

## <a name="how-do-i-monitor-my-kafka-cluster"></a>Kafka kümemi nasıl izleyebilirim?

[Kafka günlüklerinizi](./apache-kafka-log-analytics-operations-management.md)analiz etmek için Azure monitörünü kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight'ta Apache Kafka için TLS şifreleme ve kimlik doğrulaması ayarlama](./apache-kafka-ssl-encryption-authentication.md)
* [MirrorMaker kullanarak Apache Kafka konularını HDInsight üzerinde Kafka ile çoğaltma](./apache-kafka-mirroring.md)
