---
title: Azure HDInsight'ta kurumsal güvenliğe genel bakış
description: Azure HDInsight'ta kurumsal güvenliği sağlamak için çeşitli yöntemleri öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.date: 03/03/2020
ms.openlocfilehash: 95bfe7d7788133d8548598cb30c8084bf64a977f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78267715"
---
# <a name="overview-of-enterprise-security-in-azure-hdinsight"></a>Azure HDInsight'ta kurumsal güvenliğe genel bakış

Azure HDInsight, kurumsal güvenlik gereksinimlerinizi karşılamak için çeşitli yöntemler sunar. Bu çözümlerin çoğu varsayılan olarak etkinleştirilmez. Bu esneklik, sizin için en önemli olan güvenlik özelliklerini seçmenize olanak tanır ve istemediğiniz özellikler için ödeme yapmaktan kaçınmanıza yardımcı olur. Bu aynı zamanda, kurulum ve ortamınız için doğru çözümlerin etkin olduğundan emin olmak sizin sorumluluğunuzda dır.

Bu makalede, güvenlik çözümlerini dört geleneksel güvenlik sütunu boyunca bölerek güvenlik çözümleri ne kadar görünür: çevre güvenliği, kimlik doğrulama, yetkilendirme ve şifreleme.

Bu makalede ayrıca, HDInsight kümeleri için Active Directory tabanlı kimlik doğrulama, çok kullanıcılı destek ve rol tabanlı erişim denetimi sağlayan **Azure HDInsight Kurumsal Güvenlik Paketi (ESP)** de tanıtıştır.

## <a name="enterprise-security-pillars"></a>Kurumsal güvenlik direkleri

Kurumsal güvenliğe bakmanın bir yolu, güvenlik çözümlerini denetim türüne göre dört ana gruba böler. Bu gruplara güvenlik sütunları da denir ve bunlar şunlardır: çevre güvenliği, kimlik doğrulama, yetkilendirme ve şifreleme.

### <a name="perimeter-security"></a>Çevre güvenliği

HDInsight'ta çevre güvenliği [sanal ağlar](../hdinsight-plan-virtual-network-deployment.md)aracılığıyla sağlanır. Bir kuruluş yöneticisi sanal ağ (VNET) içinde bir küme oluşturabilir ve sanal ağa erişimi kısıtlamak için ağ güvenlik gruplarını (NSG) kullanabilir. Yalnızca gelen NSG kurallarında izin verilen IP adresleri HDInsight kümesiyle iletişim kurabilir. Bu yapılandırma çevre güvenliği sağlar.

VNET'te dağıtılan tüm kümelerin, küme ağ geçitlerine özel HTTP erişimi için VNET içindeki özel bir IP'ye göre çözülen özel bir bitiş noktası da vardır.

### <a name="authentication"></a>Kimlik doğrulaması

HDInsight'ın [Kurumsal Güvenlik Paketi](apache-domain-joined-architecture.md) Active Directory tabanlı kimlik doğrulama, çok kullanıcılı destek ve rol tabanlı erişim denetimi sağlar. Etkin Dizin tümleştirmesi [Azure Active Directory Etki Alanı Hizmetleri'nin](../../active-directory-domain-services/overview.md)kullanımıyla elde edilir. Bu özelliklerle, yönetilen active directory etki alanına katılan bir HDInsight kümesi oluşturabilirsiniz. Daha sonra, kümenin kimliğini doğrulayıp oturum açabilen kuruluşçalışanlarının listesini yapılandırabilirsiniz.

Bu kurulumla, kurumsal çalışanlar etki alanı kimlik bilgilerini kullanarak küme düğümlerinde oturum açabilirler. Ayrıca, kümeyle etkileşimde kalmak için Apache Ambari Views, ODBC, JDBC, PowerShell ve REST API'leri gibi diğer onaylı uç noktalarıyla kimlik doğrulamayapmak için etki alanı kimlik bilgilerini de kullanabilirler.

### <a name="authorization"></a>Yetkilendirme

Çoğu işletmenin izlediği en iyi uygulama, her çalışanın tüm kurumsal kaynaklara erişemediğinden emin olmaktır. Aynı şekilde, yönetici küme kaynakları için rol tabanlı erişim denetimi ilkeleri tanımlayabilir. Bu yalnızca ESP kümelerinde kullanılabilir.

Hadoop admin apache [Hive](apache-domain-joined-run-hive.md), [HBase](apache-domain-joined-run-hbase.md)ve [Kafka](apache-domain-joined-run-kafka.md) Apache Ranger bu eklentileri kullanarak güvenli rol tabanlı erişim kontrolü (RBAC) yapılandırabilirsiniz. RBAC ilkelerini yapılandırmak, izinleri kuruluştaki bir rolile ilişkilendirmenize olanak tanır. Bu soyutlama katmanı, insanların yalnızca iş sorumluluklarını yerine getirmek için gereken izinlere sahip olmalarını kolaylaştırır. Ranger ayrıca çalışanların veri erişimini ve denetim politikalarına erişmek için yapılan değişiklikleri denetlemenize de olanak tanır.

Örneğin yönetici [Apache Ranger](https://ranger.apache.org/)’ı Hive için erişim denetim ilkeleri belirleyecek şekilde yapılandırabilir. Bu işlevsellik satır düzeyinde ve sütun düzeyinde filtreleme (veri maskeleme) sağlar ve hassas verileri yetkisiz kullanıcılardan filtreler.

### <a name="auditing"></a>Denetim

Küme kaynaklarına ve verilerin tüm erişimlerinin denetlenilmesi, kaynakların yetkisiz veya kasıtsız erişimini izlemek için gereklidir. HDInsight küme kaynaklarını yetkisiz kullanıcılardan korumak ve verileri güvence altına almak kadar önemlidir.

Yönetici, HDInsight küme kaynakları ve verilerini görüntüleyebilir ve raporlayabilir. Yönetici ayrıca Apache Ranger desteklenen uç noktalarda oluşturulan erişim denetimi ilkelerindeki tüm değişiklikleri görüntüleyebilir ve raporlayabilir.

Apache Ranger ve Ambari denetim günlüklerine erişmek ve erişim günlüklerini ssh olarak erişmek için [Azure Monitor'u etkinleştirin](../hdinsight-hadoop-oms-log-analytics-tutorial.md#cluster-auditing) ve denetim kayıtları sağlayan tabloları görüntüleyin.

### <a name="encryption"></a>Şifreleme

Verilerin korunması, kuruluş güvenliği ve uyumluluk gereksinimlerini karşılamak için önemlidir. Yetkisiz çalışanların verilerine erişimi kısıtlamanın yanı sıra, verileri şifrelemeniz gerekir.

HDInsight kümeleri, Azure Blob depolama ve Azure Veri Gölü Depolama Gen1/Gen2 için her iki veri deposu da, veri şeffaf sunucu tarafından [şifrelemeyi](../../storage/common/storage-service-encryption.md) destekler. Güvenli HDInsight kümeleri, sunucu tarafındaki bu veri şifreleme özelliğiyle sorunsuz bir şekilde çalışır.

### <a name="compliance"></a>Uyumluluk

Azure uyumluluk teklifleri, resmi sertifikalar, attestation'lar, doğrulamalar, yetkilendirmeler ve bağımsız üçüncü taraf denetim firmaları tarafından üretilen değerlendirmeler, sözleşme değişiklikleri dahil olmak üzere çeşitli güvence türlerine dayanır. öz değerlendirmeler ve Microsoft tarafından üretilen müşteri yönlendirme belgeleri. HDInsight uyumluluk bilgileri için [Microsoft Güven Merkezi'ne](https://www.microsoft.com/trust-center) ve Microsoft [Azure uyumluluğuna Genel Bakış'a](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)bakın.

## <a name="shared-responsibility-model"></a>Paylaşılan sorumluluk modeli

Aşağıdaki resim, ana sistem güvenlik alanlarını ve her birinde kullanabileceğiniz güvenlik çözümlerini özetler. Ayrıca, müşteri olarak hangi güvenlik alanlarının sizin sorumluluğunuzda olduğunu ve servis sağlayıcısı olarak HANGI alanların HDInsight'ın sorumluluğunda olduğunu da vurgular.

![HDInsight paylaşılan sorumluluklar diyagramı](./media/hdinsight-security-overview/hdinsight-shared-responsibility.png)

Aşağıdaki tablo, her güvenlik çözümü türü için kaynaklara bağlantılar sağlar.

| Güvenlik alanı | Çözümler mevcuttur | Sorumlu taraf |
|---|---|---|
| Veri Erişim Güvenliği | Azure Veri Gölü Depolama Gen1 ve Gen2 için [erişim denetim listelerini](../../storage/blobs/data-lake-storage-access-control.md) yapılandırma  | Müşteri |
|  | Depolama [hesaplarındaki "Güvenli aktarım gerekli"](../../storage/common/storage-require-secure-transfer.md) özelliğini etkinleştirin. | Müşteri |
|  | Azure [Depolama güvenlik duvarlarını](../../storage/common/storage-network-security.md) ve sanal ağları yapılandırma | Müşteri |
|  | Cosmos DB ve [Azure SQL DB](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview) için [Azure sanal ağ hizmeti bitiş noktalarını](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) yapılandırma | Müşteri |
|  | Aktarım sırasındaki veriler için [TLS şifrelemesi](../../storage/common/storage-security-tls.md) etkinleştirildiğinden emin olun. | Müşteri |
|  | Azure Depolama şifrelemesi için [müşteri tarafından yönetilen anahtarları](../../storage/common/storage-encryption-keys-portal.md) yapılandırma | Müşteri |
| Uygulama ve ara yazılım güvenliği | AAD-DS ile tümleştir ve [Kimlik Doğrulamayı Yapılandır](apache-domain-joined-configure-using-azure-adds.md) | Müşteri |
|  | [Apache Ranger Yetkilendirme](apache-domain-joined-run-hive.md) ilkelerini yapılandırma | Müşteri |
|  | [Azure Monitör günlüklerini](../hdinsight-hadoop-oms-log-analytics-tutorial.md) kullanma | Müşteri |
| İşletim sistemi güvenliği | En son güvenli temel görüntüye sahip kümeler oluşturma | Müşteri |
|  | Düzenli aralıklarla [Işletim Sistemi Yama'sı](../hdinsight-os-patching.md) | Müşteri |
| Ağ güvenliği | [Sanal ağı](../hdinsight-plan-virtual-network-deployment.md) yapılandırma |
|  | Gelen [ağ güvenlik grubu (NSG) kurallarını](../hdinsight-plan-virtual-network-deployment.md#networktraffic) yapılandırma | Müşteri |
|  | Güvenlik Duvarı ile [Giden Trafik Kısıtlaması'nı](../hdinsight-restrict-outbound-traffic.md) yapılandırma | Müşteri |
| Sanallaştırılmış altyapı | Yok | HDInsight (Bulut sağlayıcısı) |
| Fiziksel altyapı güvenliği | Yok | HDInsight (bulut sağlayıcısı) |

## <a name="next-steps"></a>Sonraki adımlar

* [ESP ile HDInsight kümeleri için plan](apache-domain-joined-architecture.md)
* [HDInsight kümelerini ESP ile yapılandırın](apache-domain-joined-configure.md)
* [ESP ile HDInsight kümelerini yönetme](apache-domain-joined-manage.md)
