---
title: Azure Güvenlik kıyaslama v2-günlük kaydı ve tehdit algılama
description: Azure Güvenlik kıyaslaması v2 günlüğe kaydetme ve tehdit algılama
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 02/22/2021
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 43119a51be2359ed2ddfb4e5d5fa629badedb054
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101720690"
---
# <a name="security-control-v2-logging-and-threat-detection"></a>Güvenlik denetimi v2: günlük kaydı ve tehdit algılama

Günlüğe kaydetme ve tehdit algılama, Azure 'da tehditleri algılama ve Azure hizmetleri için Denetim günlüklerini etkinleştirme, toplama ve depolama denetimlerini ele alır. Bu, Azure hizmetlerinde yerel tehdit algılama ile yüksek kaliteli uyarılar oluşturmaya yönelik denetimlerle algılama, araştırma ve düzeltme süreçlerini etkinleştirmeyi içerir; Ayrıca Azure Izleyici ile günlüklerin toplanmasını, Azure Sentinel ile güvenlik analizini, zaman eşitlemesini ve günlük tutmayı de kapsar.

Geçerli yerleşik Azure Ilkesini görmek için bkz. [Azure Güvenlik kıyaslaması uyumluluğu yerleşik girişiminin ayrıntıları: günlük kaydı ve tehdit algılama](../../governance/policy/samples/azure-security-benchmark#logging-and-threat-detection)

## <a name="lt-1-enable-threat-detection-for-azure-resources"></a>LT-1: Azure kaynakları için tehdit algılamayı etkinleştirme

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| LT-1 | 6.7 | AU-3, AU-6, AU-12, SI-4 |

Olası tehditler ve bozukluklar için farklı türlerde Azure varlıklarını izlemediğinizden emin olun. Analistlerin sıralama için yanlış pozitif durumları azaltmaya yönelik yüksek kaliteli uyarılar almaya odaklanın. Uyarılar, günlük verilerinden, aracılardan veya diğer verilerden kaynaklıdır.

Azure hizmet telemetrisini izlemeyi ve hizmet günlüklerini çözümlemeyi temel alan Azure Güvenlik Merkezi 'nin yerleşik tehdit algılama özelliğini kullanın. Veriler, sistemden güvenlikle ilgili çeşitli yapılandırma ve olay günlüklerini okuyan ve analiz için verileri çalışma alanınıza kopyalayan Log Analytics Aracı kullanılarak toplanır. 

Ayrıca, ortamınızda belirli ölçütlerle eşleşen tehditleri araya analizi kuralları oluşturmak için Azure Sentinel kullanın. Kurallar, her olayı araştırmak için ölçütler eşleştiğinde olaylar oluşturur. Azure Sentinel, tehdit algılama yeteneklerini geliştirmek için üçüncü taraf tehdit bilgilerini de içeri aktarabilir. 

- [Azure Güvenlik Merkezi’nde tehdit koruması](../../security-center/azure-defender.md)

- [Azure Güvenlik Merkezi güvenlik uyarıları başvuru kılavuzu](../../security-center/alerts-reference.md)

- [Tehditleri algılamak için özel analiz kuralları oluşturma](../../sentinel/tutorial-detect-threats-custom.md)

- [Azure Sentinel ile Cyber Threat Intelligence](/azure/architecture/example-scenario/data/sentinel-threat-intelligence)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Güvenlik işlemleri](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2: Azure kimlik ve erişim yönetimi için tehdit algılamayı etkinleştirin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| LT-2 | 6.8 | AU-3, AU-6, AU-12, SI-4 |

Azure AD, Azure AD raporlama 'da görüntülenebilen veya Azure Izleyici, Azure Sentinel veya diğer SıEM/izleme araçlarıyla tümleştirilen, daha karmaşık izleme ve analiz kullanım durumları için kullanılabilen aşağıdaki kullanıcı günlüklerini sağlar: 
-   Oturum açma bilgileri – Oturum açma bilgileri raporu, yönetilen uygulamaların kullanımı ve kullanıcı oturum açma etkinlikleri hakkında bilgi sağlar.

-   Denetim günlükleri - Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. Azure AD içindeki herhangi bir kaynakta yapılan kullanıcı, uygulama, grup, rol ve ilkeleri ekleme veya kaldırma işlemleri gibi değişiklikler, denetim günlüklerine örnek gösterilebilir.

-   Riskli oturum açma işlemleri - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir.

-   Riskli oldukları belirlenen kullanıcılar - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir.

Azure Güvenlik Merkezi, çok fazla sayıda başarısız kimlik doğrulama girişimi ve abonelikteki kullanım dışı hesaplar gibi bazı şüpheli etkinlikler üzerinde de uyarı verebilir. Azure Güvenlik Merkezi 'nin tehdit koruması modülü, temel güvenlik durumu izlemeye ek olarak, bireysel Azure işlem kaynaklarından (sanal makineler, kapsayıcılar, App Service), veri kaynakları (SQL DB ve depolama gibi) ve Azure hizmet katmanlarından daha ayrıntılı güvenlik uyarıları da toplayabilirler. Bu özellik, hesap bozuklularını tek tek kaynaklar içinde görmenizi sağlar.

- [Azure AD 'de denetim etkinlik raporları](../../active-directory/reports-monitoring/concept-audit-logs.md)

- [Azure Kimlik Koruması’nı etkinleştirme](../../active-directory/identity-protection/overview-identity-protection.md)

- [Azure Güvenlik Merkezi’nde tehdit koruması](../../security-center/azure-defender.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Güvenlik işlemleri](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3: Azure ağ etkinlikleri için günlüğe kaydetmeyi etkinleştirin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| LT-3 | 9,3, 12,2, 12,5, 12,8 | AU-3, AU-6, AU-12, SI-4 |

Olay analizlerini, tehdit arayışınızı ve güvenlik uyarısı oluşturmayı desteklemek üzere güvenlik çözümlemesi için ağ güvenlik grubu (NSG) kaynak günlüklerini, NSG akış günlüklerini, Azure Güvenlik Duvarı günlüklerini ve Web uygulaması güvenlik duvarı (WAF) günlüklerini etkinleştirin ve toplayın. Akış günlüklerini bir Azure Izleyici Log Analytics çalışma alanına gönderebilir ve sonra öngörüleri sağlamak için Trafik Analizi kullanabilirsiniz.

Diğer ağ verilerinin bağıntılandırmasına yardımcı olmak için DNS sorgu günlükleri toplatığınızdan emin olun.

- [Ağ güvenlik grubu akış günlüklerini etkinleştirme](../../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Azure Güvenlik Duvarı günlükleri ve ölçümleri](../../firewall/logs-and-metrics.md)

- [Trafik Analizi etkinleştirme ve kullanma](../../network-watcher/traffic-analytics.md)

- [Ağ Izleyicisi ile izleme](../../network-watcher/network-watcher-monitoring-overview.md)

- [Azure Izleyici 'de Azure ağ izleme çözümleri](../../azure-monitor/insights/azure-networking-analytics.md)

- [DNS Analizi çözümü ile DNS altyapınız hakkında Öngörüler toplayın](../../azure-monitor/insights/dns-analytics.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Güvenlik işlemleri](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Azure kaynakları için günlüğe kaydetmeyi etkinleştirin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| LT-4 | 6,2, 6,3, 8,8 | AU-3, AU-12 |

Uyumluluk, tehdit algılama, araştırma ve olay araştırması gereksinimlerini karşılamak için Azure kaynakları için günlük kaydını etkinleştirin. 

Azure Güvenlik Merkezi 'ni ve Azure Ilkesini, denetim, güvenlik ve kaynak günlüklerine erişim için Azure kaynaklarında kaynak günlüklerini ve günlük verilerini toplamayı etkinleştirmek üzere kullanabilirsiniz. Otomatik olarak kullanılabilen etkinlik günlükleri Olay kaynağını, tarihi, kullanıcıyı, zaman damgasını, kaynak adreslerini, hedef adreslerini ve diğer yararlı öğeleri içerir.

- [Azure 'da günlüğe kaydetme ve farklı günlük türlerini anlama](../../azure-monitor/essentials/platform-logs-overview.md)

- [Azure Güvenlik Merkezi veri toplamayı anlama](../../security-center/security-center-enable-data-collection.md)

**Sorumluluk**: Paylaşılan

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik işlemleri](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

Altyapı ve uç nokta güvenliği 

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: Güvenlik günlüğü yönetim ve analiz süreçlerini merkezileştirin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| LT-5 | 6,5, 6,6 | AU-3, SI-4 |

Bağıntıyı etkinleştirmek için günlük depolamayı ve çözümlemeyi merkezileştirin. Her günlük kaynağı için bir veri sahibi, erişim Kılavuzu, depolama konumu, verileri işlemek ve erişmek için kullanılan araçları ve veri saklama gereksinimlerini atamış olduğunuzdan emin olun.

Azure etkinlik günlüklerini merkezi günlükliğinizdeki tümleştirdiğinizden emin olun. Endpoint cihazları, ağ kaynakları ve diğer güvenlik sistemleri tarafından oluşturulan güvenlik verilerini toplamak için Azure Izleyici aracılığıyla günlük alma. Azure Izleyici 'de analiz sorgulamak ve gerçekleştirmek için Log Analytics çalışma alanlarını kullanın ve uzun süreli ve arşiv depolama için Azure depolama hesaplarını kullanın.

Ayrıca, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'e verileri etkinleştirin ve ekleyin.

Birçok kuruluş, daha az sıklıkta kullanılan "soğuk" verileri için sık kullanılan ve Azure depolama için Azure Sentinel ' i "etkin" veriler için kullanmayı seçer.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../../azure-monitor/essentials/diagnostic-settings.md)

- [Azure Sentinel 'i ekleme](../../sentinel/quickstart-onboard.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

## <a name="lt-6-configure-log-storage-retention"></a>LT-6: Günlük depolama alanının saklama süresini yapılandırın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| LT-6 | 6.4 | AU-3, AU-11 |

Günlük saklama alanınızı uyumluluk, düzenleme ve iş gereksinimlerinize göre yapılandırın. 

Azure Izleyici 'de, Log Analytics çalışma alanı saklama dönemini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayabilirsiniz. Uzun süreli ve arşiv depolama için Azure depolama, Data Lake veya Log Analytics çalışma alanı hesapları kullanın.

- [Log Analytics veri saklama süresini değiştirme](../../azure-monitor/logs/manage-cost-storage.md#change-the-data-retention-period)

- [Azure depolama hesabı günlükleri için bekletme ilkesini yapılandırma](../../storage/common/manage-storage-analytics-logs.md#configure-logging)

- [Azure Güvenlik Merkezi uyarıları ve önerileri dışarı aktarma](../../security-center/continuous-export.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Güvenlik işlemleri](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Güvenlik uyumluluğu yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="lt-7-use-approved-time-synchronization-sources"></a>LT-7: onaylanan zaman eşitleme kaynaklarını kullanın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| LT-7 | 6.1 | AU-8 |

Microsoft, çoğu Azure PaaS ve SaaS hizmeti için zaman kaynaklarını korur. Sanal makineleriniz için, belirli bir gereksinimin olmadığı durumlar dışında, zaman eşitleme için Microsoft varsayılan NTP sunucusu ' nu kullanın. Kendi ağ zaman Protokolü (NTP) sunucunuzu oluşturmanız gerekiyorsa, UDP hizmet bağlantı noktası 123 ' ı güvence altına aldığınızdan emin olun.

Azure içindeki kaynaklar tarafından oluşturulan tüm Günlükler varsayılan olarak belirtilen saat dilimiyle birlikte zaman damgaları sağlar.

- [Azure Windows işlem kaynakları için Saat eşitlemesini yapılandırma](../../virtual-machines/windows/time-sync.md)

- [Azure Linux işlem kaynakları için Saat eşitlemesini yapılandırma](../../virtual-machines/linux/time-sync.md)

- [Azure hizmetleri için gelen UDP 'yi devre dışı bırakma](https://support.microsoft.com/help/4558520/how-to-disable-inbound-udp-for-azure-services)

**Sorumluluk**: Paylaşılan

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [İlke ve standartlar](/azure/cloud-adoption-framework/organize/cloud-security-policy-standards)

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)