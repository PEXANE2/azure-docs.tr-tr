---
title: Azure HDInsight'ta kurumsal güvenliğe genel bakış
description: Azure HDInsight'ta kurumsal güvenliği sağlamak için çeşitli yöntemleri öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: 797767e0c463161f29e486aef7db0ccaf459e299
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733563"
---
# <a name="overview-of-enterprise-security-in-azure-hdinsight"></a>Azure HDInsight'ta kurumsal güvenliğe genel bakış

Azure HDInsight, kurumsal güvenlik gereksinimlerinizi karşılamak için çeşitli yöntemler sunar. Bu çözümlerin çoğu varsayılan olarak etkinleştirilmez. Bu esneklik, sizin için en önemli güvenlik özelliklerini seçmenize olanak tanır. Ve istemediğiniz özellikler için ödeme yapmaktan kaçınmanıza yardımcı olur. Bu esneklik aynı zamanda kurulum ve ortamınız için doğru çözümlerin etkin olduğundan emin olmak sizin sorumluluğunuzdadır.

Bu makalede, güvenlik çözümlerini dört geleneksel güvenlik sütununa bölerek güvenlik çözümleri ne zaman ait: çevre güvenliği, kimlik doğrulama, yetkilendirme ve şifreleme.

Bu makalede ayrıca, HDInsight kümeleri için Active Directory tabanlı kimlik doğrulama, çok kullanıcılı destek ve rol tabanlı erişim denetimi sağlayan **Azure HDInsight Kurumsal Güvenlik Paketi (ESP)** de tanıtıştır.

## <a name="enterprise-security-pillars"></a>Kurumsal güvenlik direkleri

Kurumsal güvenliğe bakmanın bir yolu, güvenlik çözümlerini denetim türüne göre dört ana gruba böler. Bu gruplar açısından güvenlik sütunları olarak da adverilir ve bunlar ıntılar: çeşitli güvenlik, kimlik doğrulama, yetkilendirme ve şifreleme.

### <a name="perimeter-security"></a>Çevre güvenliği

HDInsight'ta çevre güvenliği [sanal ağlar](../hdinsight-plan-virtual-network-deployment.md)aracılığıyla sağlanır. Bir kuruluş yöneticisi sanal ağ (VNET) içinde bir küme oluşturabilir ve sanal ağa erişimi kısıtlamak için ağ güvenlik gruplarını (NSG) kullanabilir. Yalnızca gelen NSG kurallarında izin verilen IP adresleri HDInsight kümesiyle iletişim kurabilir. Bu yapılandırma çevre güvenliği sağlar.

VNET'te dağıtılan tüm kümelerin de özel bir bitiş noktası olacaktır. Bitiş noktası, küme ağ geçitlerine özel HTTP erişimi için VNET içindeki özel bir IP'ye gider.

### <a name="authentication"></a>Kimlik Doğrulaması

[HDInsight'ın Kurumsal Güvenlik Paketi](apache-domain-joined-architecture.md) Active Directory tabanlı kimlik doğrulama, çok kullanıcılı destek ve rol tabanlı erişim denetimi sağlar. Etkin Dizin tümleştirmesi [Azure Active Directory Etki Alanı Hizmetleri'nin](../../active-directory-domain-services/overview.md)kullanımıyla elde edilir. Bu özelliklerle, Active Directory etki alanına katılan bir HDInsight kümesi oluşturabilirsiniz. Ardından, kuruluştan kümeye kimlik doğrulaması yapabilecek çalışanların listesini yapılandırın.

Bu kurulumla, kurumsal çalışanlar etki alanı kimlik bilgilerini kullanarak küme düğümlerinde oturum açabilirler. Ayrıca, diğer onaylı uç noktalarıyla kimlik doğrulaması yapmak için etki alanı kimlik bilgilerini de kullanabilirler. Apache Ambari Views, ODBC, JDBC, PowerShell ve REST API'leri gibi kümeyle etkileşime girilir.

### <a name="authorization"></a>Yetkilendirme

Çoğu işletmenin izlediği en iyi uygulama, her çalışanın tüm kurumsal kaynaklara tam erişime sahip olmadığından emin olmaktır. Aynı şekilde, yönetici küme kaynakları için rol tabanlı erişim denetimi ilkeleri tanımlayabilir. Bu eylem yalnızca ESP kümelerinde kullanılabilir.

Hadoop yöneticisi rol tabanlı erişim denetimini (RBAC) yapılandırabilir. Yapılandırmaları Apache [Hive](apache-domain-joined-run-hive.md)güvenli , [HBase](apache-domain-joined-run-hbase.md), ve [Kafka](apache-domain-joined-run-kafka.md) Apache Aralığı eklentileri ile. RBAC ilkelerini yapılandırmak, izinleri kuruluştaki bir rolile ilişkilendirmenize olanak tanır. Bu soyutlama katmanı, insanların yalnızca iş sorumluluklarını yerine getirmek için gereken izinlere sahip olmalarını kolaylaştırır. Ranger ayrıca çalışanların veri erişimini ve denetim politikalarına erişmek için yapılan değişiklikleri denetlemenize de olanak tanır.

Örneğin yönetici [Apache Ranger](https://ranger.apache.org/)’ı Hive için erişim denetim ilkeleri belirleyecek şekilde yapılandırabilir. Bu işlevsellik satır düzeyinde ve sütun düzeyinde filtreleme (veri maskeleme) sağlar. Ve yetkisiz kullanıcılardan gelen hassas verileri filtreler.

### <a name="auditing"></a>Denetim

Kaynakların yetkisiz veya kasıtsız erişimini izlemek için küme kaynak erişimini denetlemek gereklidir. Küme kaynaklarını yetkisiz erişimden korumak kadar önemlidir.

Yönetici, HDInsight küme kaynakları ve verilerini görüntüleyebilir ve raporlayabilir. Yönetici, erişim denetimi ilkelerindeki değişiklikleri görüntüleyebilir ve raporlayabilir.

Apache Ranger ve Ambari denetim günlüklerine erişmek ve erişim günlüklerine erişmek için [Azure Monitor'u etkinleştirin.](../hdinsight-hadoop-oms-log-analytics-tutorial.md#cluster-auditing) Ve denetim kayıtları sağlayan tabloları görüntüleyin.

### <a name="encryption"></a>Şifreleme

Verilerin korunması, kuruluş güvenliği ve uyumluluk gereksinimlerini karşılamak için önemlidir. Yetkisiz çalışanların verilerine erişimi kısıtlamanın yanı sıra, verileri şifrelemeniz gerekir.

Azure depolama ve Veri Gölü Depolama Gen1/Gen2, veri şeffaf sunucu tarafında [şifreleme](../../storage/common/storage-service-encryption.md) yi destekler. Güvenli HDInsight kümeleri, sunucu tarafındaki veri şifrelemesi ile sorunsuz bir şekilde çalışır.

### <a name="compliance"></a>Uyumluluk

Azure uyumluluk teklifleri, resmi sertifikalar da dahil olmak üzere çeşitli güvence türlerine dayanır. Ayrıca, attestation, doğrulamalar ve yetkilendirmeler. Bağımsız üçüncü taraf denetim firmaları tarafından yapılan değerlendirmeler. Microsoft tarafından üretilen sözleşme değişiklikleri, öz değerlendirmeler ve müşteri yönlendirme belgeleri. HDInsight uyumluluk bilgileri için [Microsoft Güven Merkezi'ne](https://www.microsoft.com/trust-center) ve Microsoft [Azure uyumluluğuna Genel Bakış'a](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)bakın.

## <a name="shared-responsibility-model"></a>Paylaşılan sorumluluk modeli

Aşağıdaki resim, ana sistem güvenlik alanlarını ve her birinde kullanabileceğiniz güvenlik çözümlerini özetler. Ayrıca, müşteri olarak hangi güvenlik alanlarının sizin sorumluluğunuzda olduğunu da vurgular. Ve hizmet sağlayıcı olarak HDInsight'ın sorumluluğu hangi alanlardadır.

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
