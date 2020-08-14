---
title: SYNAPSE Analytics için Azure Güvenlik temeli
description: SYNAPSE Analytics güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 07/22/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 7f05e4fb0443107370f9182706bd35b45771e0f2
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88210899"
---
# <a name="azure-security-baseline-for-synapse-analytics"></a>SYNAPSE Analytics için Azure Güvenlik temeli

SYNAPSE Analytics için Azure Güvenlik taban çizgisi, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: sanal ağlar içindeki Azure kaynaklarını koruma

**Rehberlik**: Azure SQL Server özel bağlantı aracılığıyla bir sanal ağ ile güvenli hale getirin. Azure özel bağlantısı, Azure PaaS hizmetlerine sanal ağınızdaki özel bir uç nokta üzerinden erişmenizi sağlar. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağını de dolaşır.

Alternatif olarak, SYNAPSE SQL havuzunuza bağlanırken, bir ağ güvenlik grubu kullanarak SQL veritabanı 'na giden bağlantının kapsamını daraltın. Azure hizmetlerinin kapalı çalışmasına Izin ver ayarını yaparak, genel uç nokta aracılığıyla SQL veritabanı 'na yönelik tüm Azure hizmet trafiğini devre dışı bırakın. Güvenlik duvarı kurallarında genel IP adreslerine izin verilmediğinden emin olun.

* [Azure özel bağlantısını anlama](https://docs.microsoft.com/azure/private-link/private-link-overview)

* [Azure SYNAPSE SQL için özel bağlantıyı anlama](https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview)

* [Sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

* [Güvenlik Yapılandırması ile NSG oluşturma](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1,2: sanal ağların, alt ağların ve ağ arabirimlerinin yapılandırma ve trafiğini izleme ve günlüğe kaydetme

**Kılavuz**: Azure SYNAPSE SQL havuzunuza bağlanırken ve ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirdiyseniz, trafik denetimi Için günlükleri Azure depolama hesabına gönderin.

Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

* [NSG akış günlüklerini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

* [Trafik Analizi etkinleştirme ve kullanma](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

* [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](https://docs.microsoft.com/azure/security-center/security-center-network-recommendations)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; Bu öneri, Web uygulamalarını barındıran Azure Apps hizmeti veya işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Kılavuz**: Azure SYNAPSE SQL Için Gelişmiş tehdit KORUMASı (ATP) kullanın. ATP, veritabanlarına erişmek veya veritabanına yararlanmak için olağan dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar ve "olası SQL ekleme," ve "olağan dışı konumdan erişim" gibi çeşitli uyarılar tetikleyebilirler. ATP, gelişmiş veri güvenliği (ADS) sunumunun bir parçasıdır ve merkezi SQL ADS portalı aracılığıyla erişilebilir ve yönetilebilir.

Dağıtılmış hizmet reddi saldırılarına karşı koruma için Azure SYNAPSE SQL ile ilişkili sanal ağlarda DDoS koruma standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

* [Azure SYNAPSE SQL için ATP 'yi anlama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)

* [Azure SQL veritabanı için gelişmiş veri güvenliğini etkinleştirme](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)

* [ADS genel bakış](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)

* [DDoS korumasını yapılandırma](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

* [Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-data-services)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets"></a>1,5: ağ paketlerini kaydetme

**Kılavuz**: Azure SYNAPSE SQL havuzunuza bağlanırken ve ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirdiyseniz, trafik denetimi Için günlükleri Azure depolama hesabına gönderin. Akış günlüklerini bir Log Analytics çalışma alanına da gönderebilir veya Event Hubs için akışla aktarabilirsiniz. Anormal etkinlikleri araştırmak için gerekliyse, ağ Izleyicisi paket yakalamayı etkinleştirin.

* [NSG akış günlüklerini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Ağ İzleyicisini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Kılavuz**: Azure SYNAPSE SQL Için Gelişmiş tehdit KORUMASı (ATP) kullanın. ATP, veritabanlarına erişmek veya veritabanına yararlanmak için olağan dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar ve "olası SQL ekleme," ve "olağan dışı konumdan erişim" gibi çeşitli uyarılar tetikleyebilirler. ATP, gelişmiş veri güvenliği (ADS) sunumunun bir parçasıdır ve merkezi SQL ADS portalı aracılığıyla erişilebilir ve yönetilebilir. ATP Ayrıca uyarıları Azure Güvenlik Merkezi ile tümleştirir.

* [Azure SYNAPSE SQL için ATP 'yi anlama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: uygulanamaz; Bu öneri, Web uygulamalarını barındıran Azure Apps hizmeti veya işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örn., Apimanaya) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

Azure SYNAPSE SQL havuzunuz için bir hizmet uç noktası kullanırken, Azure SQL veritabanı genel IP adreslerine giden bağlantılar gereklidir: bağlantı sağlamak için ağ güvenlik grupları (NSG 'Ler) Azure SQL veritabanı IP 'lerine açılmalıdır. Bunu Azure SQL veritabanı için NSG hizmet etiketlerini kullanarak yapabilirsiniz.

* [Hizmet etiketlerini Azure SQL veritabanı için hizmet uç noktaları ile anlayın](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview#limitations)

* [Hizmet etiketlerini anlama ve kullanma](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: Azure ilkesiyle SQL havuzunuza ilişkin kaynaklar için ağ güvenlik yapılandırması tanımlama ve uygulama. Özel ilke tanımlarını tanımlamak veya Azure SQL veritabanı/sunucu ağ koruması için tasarlanan yerleşik ilke tanımlarından herhangi birini kullanmak için "Microsoft. SQL" ad alanını kullanabilirsiniz. Azure SQL veritabanı sunucusu için geçerli bir yerleşik ağ güvenlik ilkesi örneği şöyle olabilir: "SQL Server sanal ağ hizmeti uç noktası kullanmalıdır".

Azure Kaynak Yönetimi şablonları, rol tabanlı erişim denetimi (RBAC) ve ilkeler gibi tek bir şema tanımında temel ortam yapıtları sunarak büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanın. Şema 'i yeni abonelikler ve ortamlara kolayca uygulayın ve sürüm oluşturma aracılığıyla denetimi ve yönetimi ayrıntılı olarak ayarlayın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Blueprint oluşturma](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: ağ güvenlik grupları (NSG) ve ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketler kullanın. Bireysel NSG kuralları için "Açıklama" alanını kullanarak ağa giden/giden trafiğe izin veren kuralların iş gereksinimini ve/veya süresini (vs.) belirtin.

Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure Ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın.

Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynaklar üzerinde arama yapabilir veya eylemler gerçekleştirebilirsiniz.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını Izleyin ve Azure SYNAPSE SQL havuzunuza ilişkin ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

* [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Azure Izleyici 'de uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, Azure kaynakları için zaman kaynaklarını korur. İşlem dağıtımlarınız için zaman eşitlemesini güncelleştirebilirsiniz.

* [Azure işlem kaynakları için zaman eşitlemesini yapılandırma](https://docs.microsoft.com/azure/virtual-machines/windows/time-sync)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: bir denetim ilkesi, belirli bir veritabanı Için veya Azure 'da varsayılan sunucu ilkesi olarak tanımlanabilir (Azure SYNAPSE ' i barındırır). Sunucu ilkesi, sunucuda var olan ve yeni oluşturulan tüm veritabanları için geçerlidir.

Sunucu denetimi etkinse, her zaman veritabanına uygulanır. Veritabanı denetim ayarlarından bağımsız olarak veritabanını denetlenecektir.

Denetimi etkinleştirdiğinizde, bunları Azure Depolama hesabınızdaki bir denetim günlüğüne, Log Analytics çalışma alanına veya Event Hubs yazabilirsiniz.

Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

* [Azure SQL kaynaklarınız için Denetim kurma](https://docs.microsoft.com/azure/azure-sql/database/auditing-overview#server-vs-database-level)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: SYNAPSE SQL havuzunuzun Azure SQL Server düzeyinde denetimini etkinleştirin ve denetim günlükleri için bir depolama konumu seçin (Azure depolama, Log Analytics veya Event Hubs).

Denetim hem veritabanı hem de sunucu düzeyinde etkinleştirilebilir ve belirli bir veritabanı için ayrı bir veri havuzu veya bekletme yapılandırması gerekmedikçe yalnızca sunucu düzeyinde etkin olmak üzere önerilir.

* [Azure SQL veritabanı için denetimi etkinleştirme](https://docs.microsoft.com/azure/sql-database/sql-database-auditing)

* [Sunucunuz için denetimi etkinleştirme](https://docs.microsoft.com/azure/azure-sql/database/auditing-overview#setup-auditing)

* [Sunucu düzeyinde ve veritabanı düzeyinde denetim ilkelerine göre farklılıklar](https://docs.microsoft.com/azure/sql-database/sql-database-auditing#server-vs-database-level)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu kıyaslama, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Rehberlik**: SYNAPSE SQL havuzunuzla ilgili günlükleri bir depolama hesabında, Log Analytics çalışma alanında veya Olay Hub 'larıyla depolarken, kuruluşunuzun uyumluluk düzenlemelerine göre günlük saklama süresini ayarlayın.

* [Azure Blob depolama yaşam döngüsünü yönetme](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts?tabs=azure-portal)

* [Log Analytics çalışma alanında günlük tutma parametrelerini ayarlama](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

* [Event Hubs akış olaylarını yakala](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: anormal davranışlar için günlükleri çözümleyin ve izleyin ve sonuçları düzenli olarak gözden geçirin. SQL veritabanınız ile ilgili olağandışı etkinliklerle ilgili uyarı almak için Azure Güvenlik Merkezi ile birlikte Azure SQL veritabanı için Gelişmiş tehdit koruması 'nı kullanın. Alternatif olarak, SQL veritabanı ile ilgili ölçüm değerlerine veya Azure etkinlik günlüğü girişlerine göre uyarıları yapılandırın.

Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

* [Azure SQL veritabanı için Gelişmiş tehdit koruması ve uyarı vermeyi anlama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)

* [Azure SQL veritabanı için gelişmiş veri güvenliğini etkinleştirme](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)

* [Azure SQL veritabanı için özel uyarıları yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-insights-alerts-portal?view=azps-1.4.0)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Rehberlik**: anormal etkinlikleri izlemek ve uyarı almak Için Azure Güvenlik Merkezi Ile bırlıkte Azure SQL veritabanı Için Gelişmiş tehdit KORUMASı (ATP) kullanın. ATP, gelişmiş veri güvenliği (ADS) sunumunun bir parçasıdır ve portalda merkezi SQL REKLAMLARı aracılığıyla erişilebilir ve yönetilebilir. ADS, hassas verileri bulma ve sınıflandırma, olası veritabanı güvenlik açıklarını ortaya koymasının yanı sıra veritabanlarınızın bir tehdidi oluşturabilecek anormal etkinlikleri algılamayla ilgili işlevsellik içerir.

Alternatif olarak, Azure Sentinel 'de ve yerleşik verileri etkinleştirebilir.

* [Azure SQL veritabanı için Gelişmiş tehdit koruması ve uyarı vermeyi anlama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)

* [Azure SQL veritabanı için gelişmiş veri güvenliğini etkinleştirme](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)

* [Azure Güvenlik Merkezi 'nde uyarıları yönetme](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz; SYNAPSE SQL havuzunuzla ilgili kaynaklar için, kötü amaçlı yazılımdan koruma çözümü, temel alınan platformda Microsoft tarafından yönetilir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; SYNAPSE SQL havuzunuz ile ilgili kaynaklar tarafından hiçbir DNS günlüğü üretilmez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; komut satırı denetimi Azure SYNAPSE SQL için geçerli değildir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve erişim denetimi](/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: kullanıcıların kimliği Azure Active Directory ya da SQL kimlik doğrulamasıyla doğrulanır.

Azure SQL 'i ilk kez dağıttığınızda, bu oturum açma için yönetici oturumu ve ilişkili bir parola belirtirsiniz. Bu yönetim hesabına Sunucu Yöneticisi adı verilir. Bir veritabanının yönetici hesaplarını, Azure portal açıp sunucunuzun veya yönetilen örneğinizin Özellikler sekmesine giderek belirleyebilirsiniz. Ayrıca, tam yönetim izinleriyle bir Azure AD yönetici hesabı da yapılandırabilirsiniz, bu, Azure Active Directory kimlik doğrulamasını etkinleştirmek istiyorsanız gereklidir.

Yönetim işlemleri için, açıkça atanması gereken Azure yerleşik rollerini kullanın. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın.

* [SQL veritabanı için kimlik doğrulaması](https://docs.microsoft.com/azure/azure-sql/database/security-overview#authentication)

* [Yönetici olmayan kullanıcılar için hesap oluşturma](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#create-accounts-for-non-administrator-users)

* [Kimlik doğrulaması için bir Azure Active Directory hesabı kullanma](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#create-additional-logins-and-users-having-administrative-permissions)

* [Azure AD 'de PowerShell ile dizin rolü alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

* [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

* [Azure SQL 'de var olan oturum açma işlemlerini ve yönetici hesaplarını yönetme](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database)

* [Azure yerleşik rolleri](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: Azure Active Directory varsayılan parola kavramına sahip değildir. Azure SYNAPSE SQL havuzu sağlanırken, kimlik doğrulamasını Azure Active Directory ile tümleştirmeyi tercih etmeniz önerilir. Bu kimlik doğrulama yöntemiyle kullanıcı, bir kullanıcı hesabı adı ve hizmetin Azure Active Directory (Azure AD) ' de depolanan kimlik bilgisi bilgilerini kullandığı istekleri gönderir.

* [Azure SQL ile Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure?tabs=azure-powershell#active-directory-password-authentication)

* [Azure SQL 'de kimlik doğrulamasını anlama](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında ilke ve yordamlar oluşturun. Azure Active Directory aracılığıyla oturum açma yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Bir veritabanının yönetici hesaplarını belirlemek için, Azure portal açın ve sunucunuzun veya yönetilen örneğinizin Özellikler sekmesine gidin.

* [Azure Güvenlik Merkezi kimlik ve erişimini anlama](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

* [Azure SQL 'de var olan oturum açma işlemlerini ve yönetici hesaplarını yönetme](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3,4: Azure Active Directory çoklu oturum açma kullan (SSO)

**Rehberlik**: API çağrıları aracılığıyla denetim düzleminde (Azure Portal) veri Ambarınızla etkileşim kurmak için kullanılabilecek bir belirteç almak üzere bir Azure uygulama kaydı (hizmet sorumlusu) kullanın.

* [Azure REST API 'Lerini çağırma](https://docs.microsoft.com/rest/api/azure/#how-to-call-azure-rest-apis-with-postman)

* [Azure AD ile istemci uygulamanızı (hizmet sorumlusu) kaydetme](https://docs.microsoft.com/rest/api/azure/#register-your-client-application-with-azure-ad)

* [Azure SYNAPSE SQL REST API bilgileri](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-manage-compute-rest-api)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure ACTIVE DIRECTORY (AD) MULTI-Factor AUTHENTICATION (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

* [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

* [Azure SQL 'de MFA 'yı anlama](https://docs.microsoft.com/azure/azure-sql/database/authentication-mfa-ssms-overview)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3,6: yönetim görevleri için güvenli, Azure tarafından yönetilen iş istasyonları kullanın

**Kılavuz**: Azure kaynaklarını açmak ve yapılandırmak için yapılandırılmış MULTI-Factor AUTHENTICATION (MFA) Ile ayrıcalıklı erişim iş istasyonu (Paw) kullanın.

* [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinliklerle ilgili günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda günlüklerin ve uyarıların üretilmesi için Azure Active Directory güvenlik raporlarını kullanın.

Azure SQL veritabanı için Gelişmiş tehdit koruması 'nı Azure Güvenlik Merkezi ile birlikte kullanarak, veritabanları için olağan dışı ve olası zararlı girişimleri belirten anormal etkinlikleri algılayıp uyarma.

SQL Server denetim, sunucu seviyesi olayları için sunucu denetim belirtimleri ve veritabanı düzeyindeki olaylar için veritabanı denetim belirtimleri içerebilen sunucu denetimleri oluşturmanızı sağlar. Denetlenen olaylar olay günlüklerine yazılabilir veya dosyaları denetlemek için kullanılabilir.

* [Riskli etkinlik için işaretlenen Azure AD kullanıcılarını belirleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk)

* [Azure Güvenlik Merkezi 'nde Kullanıcı kimliğini ve erişim etkinliğini izleme](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

* [Gelişmiş tehdit koruması ve olası uyarıları gözden geçirme](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview#advanced-threat-protection-alerts)

* [Azure SQL 'de oturum açma işlemlerini ve Kullanıcı hesaplarını anlama](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage)

* [SQL Server denetlemeyi anlayın](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine?view=sql-server-ver15)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından portala ve Azure kaynak yönetimine erişim sağlamak Için, koşullu erişim adlı konum kullanın.

* [Azure 'da adlandırılmış konumları yapılandırma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Kılavuz**: SYNAPSE SQL havuzunuzdaki Azure SQL veritabanı sunucusu için bir Azure ACTIVE DIRECTORY (ad) Yöneticisi oluşturun.

* [Azure SQL ile Azure AD kimlik doğrulamasını yapılandırma ve yönetme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)

* [Azure AD örneği oluşturma ve yapılandırma](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure Active Directory eski hesapları keşfetmenize yardımcı olacak Günlükler sağlar. Ayrıca, grup üyeliklerini verimli bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure Active Directory erişim gözden geçirmeleri kullanın. Kullanıcıların erişimi, yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için düzenli aralıklarla gözden geçirilebilir.

SQL kimlik doğrulaması kullanırken, veritabanında kapsanan veritabanı kullanıcıları oluşturun. Bir veya daha fazla veritabanı kullanıcıyı bu kullanıcı grubuna uygun belirli izinlerle özel bir veritabanı rolüne yerleştirdiğinizden emin olun.

* [Erişim incelemelerini kullanma](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

* [Azure SQL 'de oturum açma işlemlerini ve Kullanıcı hesaplarını anlama](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

**Rehberlik**: Azure SQL ile Azure ACTIVE DIRECTORY (ad) kimlik doğrulamasını yapılandırın ve Azure Active Directory Kullanıcı hesapları için tanılama ayarlarını etkinleştirerek, Denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına gönderir. Log Analytics içindeki istenen uyarıları yapılandırın.

SQL kimlik doğrulaması kullanırken, veritabanında kapsanan veritabanı kullanıcıları oluşturun. Bir veya daha fazla veritabanı kullanıcıyı bu kullanıcı grubuna uygun belirli izinlerle özel bir veritabanı rolüne yerleştirdiğinizden emin olun.

* [Erişim incelemelerini kullanma](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

* [Azure SQL veritabanı ile Azure AD kimlik doğrulamasını yapılandırma ve yönetme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)

* [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

* [Azure SQL 'de oturum açma işlemlerini ve Kullanıcı hesaplarını anlama](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapması üzerinde uyarı

**Rehberlik**: Kullanıcı kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırmak için Azure Active Directory (Azure AD) kimlik koruması ve risk algılama özelliklerini kullanın. Ayrıca, daha fazla araştırma için Pano ve verileri Azure Sentinel 'e aktarabilirsiniz.

SQL kimlik doğrulaması kullanırken, veritabanında kapsanan veritabanı kullanıcıları oluşturun. Bir veya daha fazla veritabanı kullanıcıyı bu kullanıcı grubuna uygun belirli izinlerle özel bir veritabanı rolüne yerleştirdiğinizden emin olun.

* [Azure AD risk oturumunu görüntüleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [Yerleşik Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-data-sources)

* [Azure SQL 'de oturum açma işlemlerini ve Kullanıcı hesaplarını anlama](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Kılavuz**: Microsoft 'un SYNAPSE SQL HAVUZUNDAKI Azure SQL veritabanı ile ilgili verilere erişmesi gereken destek senaryolarında, Azure müşteri Kasası, veri erişim isteklerini gözden geçirmeniz ve onaylamanız veya reddetmeniz için bir arabirim sağlar.

* [Müşteri Kasası anlayın](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

Veri bulma &amp; sınıflandırması, Azure SYNAPSE SQL 'de yerleşik olarak bulunur. Veritabanlarınızdaki hassas verileri bulmak, sınıflandırmak, etiketlemek ve raporlamak için gelişmiş yetenekler sağlar.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Veri bulma &amp; sınıflandırmasını anlama](https://docs.microsoft.com/azure/azure-sql/database/data-discovery-and-classification-overview)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Kaynaklar, sanal ağ/alt ağ ile ayrılmalıdır, uygun şekilde etiketlenebilir ve bir ağ güvenlik grubu ya da Azure Güvenlik duvarı içinde güvenliği sağlanmış olmalıdır. Hassas verileri depolayan veya işleyen kaynaklar yalıtılmalıdır. Özel bağlantı kullan; Azure SQL Server sanal bir ağ içinde dağıtın ve özel bağlantı kullanarak güvenli bir şekilde bağlanın.

* [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Azure SQL veritabanı için özel bağlantı ayarlama](https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview#how-to-set-up-private-link-for-azure-sql-database)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Kılavuz**: SYNAPSE SQL havuzunuzdaki herhangi BIR Azure SQL veritabanı için gizli bilgileri depolayan veya işleyen, etiketleri kullanarak veritabanını ve ilgili kaynakları gizli olarak işaretleyin. Gizli bilgilerin ayıklanmasını engellemek için Azure SQL veritabanı örneklerindeki ağ güvenlik grubu (NSG) hizmet etiketleriyle birlikte özel bağlantıyı yapılandırın.

Ayrıca, Azure SQL veritabanı için Gelişmiş tehdit koruması, Azure SQL yönetilen örneği ve Azure SYNAPSE, veritabanları için olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri algılar.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

* [Azure SQL veritabanı örneklerinizdeki verilerin ayıklanmasını engellemek için özel bağlantı ve NSG 'ler yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview)

* [Azure SQL veritabanı için Gelişmiş tehdit korumasını anlama](https://docs.microsoft.com/azure/azure-sql/database/threat-detection-overview)

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: Azure SQL veritabanı, Aktarım Katmanı Güvenliği Ile hareket halindeki verileri şifreleyerek verilerinizi korur. SQL Server tüm bağlantılar için şifrelemeyi (SSL/TLS) her zaman uygular. Bu, bağlantı dizesinde şifreleme veya TrustServerCertificate ayarından bağımsız olarak, tüm verilerin istemci ve sunucu arasında "geçişte" şifrelendiğinden emin olmanızı sağlar.

* [Yoldaki Azure SQL şifrelemeyi anlama](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview#information-protection-and-encryption)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Kılavuz**: Azure SYNAPSE SQL veri bulma &amp; sınıflandırması özelliğini kullanın. Veri bulma &amp; sınıflandırması, &amp; veritabanlarınızdaki hassas verileri korumak, sınıflandırmak ve etiketleme IÇIN Azure SQL veritabanı 'nda yerleşik olarak bulunan gelişmiş yetenekler sağlar.

Veri bulma &amp; sınıflandırması, GELIŞMIŞ SQL güvenlik özelliklerine yönelik Birleşik bir paket olan gelişmiş veri güvenliği sunumunun bir parçasıdır. Veri bulma &amp; sınıflandırmasına, MERKEZI SQL ads portalı aracılığıyla erişilebilir ve yönetilebilir.

Ayrıca, Azure portal bir dinamik veri maskeleme (DDM) ilkesi ayarlayabilirsiniz. DDM öneriler altyapısı, veritabanınızdaki belirli alanları, maskeleme için iyi aday olabilecek, potansiyel olarak hassas alanlar olarak işaretler.

* [Azure SQL Server veri bulma ve sınıflandırma kullanma](https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification)

* [Azure SYNAPSE SQL için dinamik veri maskeleme 'yi anlama](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için rol tabanlı erişim denetimi kullanma

**Kılavuz**: SYNAPSE SQL HAVUZUNUZDAKI Azure SQL veritabanlarına erişimi yönetmek için Azure rol tabanlı erişim denetimi (Azure RBAC) kullanın.

Yetkilendirme, Kullanıcı hesabınızın veritabanı rolü üyelikleri ve nesne düzeyi izinleri tarafından denetlenir. En iyi uygulama olarak, kullanıcılarınıza gerekli olan en düşük ayrıcalıkları tanımanız gerekir.

* [Azure SQL Server kimlik doğrulaması için Azure Active Directory tümleştirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication)

* [Azure SQL Server erişimi denetleme](https://docs.microsoft.com/azure/sql-database/sql-database-control-access)

* [Azure SQL 'de yetkilendirmeyi ve kimlik doğrulamasını anlama](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Microsoft, Azure SYNAPSE SQL için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Kılavuz**: saydam veri şifrelemesi (tde), bekleyen verileri şifreleyerek kötü amaçlı çevrimdışı etkinlik tehditlerine karşı Azure SYNAPSE SQL 'i korumanıza yardımcı olur. Bu özellik bütün bir veritabanı, yedekleri ve işlem günlüğü dosyaları için gerçek zamanlı şifreleme ve şifre çözme işlemlerini gerçekleştirir ve uygulamada değişiklik yapmayı gerektirmez. Azure 'da, TDE için varsayılan ayar, DEK 'ın yerleşik bir sunucu sertifikasıyla korunmakta olması. Alternatif olarak, Ayrıca, TDE için Kendi Anahtarını Getir (BYOK) desteği olarak da adlandırılan müşteri tarafından yönetilen TDE kullanabilirsiniz. Bu senaryoda, 1 ' i şifreleyen TDE koruyucu, müşterinin sahip olduğu ve yönetilen bir Azure Key Vault (Azure 'un bulut tabanlı dış anahtar yönetim sistemi) depolanan ve anahtar kasasını hiçbir şekilde bırakmayan, müşteri tarafından yönetilen bir asimetrik anahtardır.

* [Hizmet tarafından yönetilen saydam veri şifrelemesini anlama](https://docs.microsoft.com/azure/azure-sql/database/transparent-data-encryption-tde-overview?tabs=azure-portal)

* [Müşterinin yönettiği saydam veri şifrelemesini anlama](https://docs.microsoft.com/azure/azure-sql/database/transparent-data-encryption-tde-overview?tabs=azure-portal#customer-managed-transparent-data-encryption---bring-your-own-key)

* [Kendi anahtarınızı kullanarak TDE nasıl devre dışı bırakılır](https://docs.microsoft.com/azure/azure-sql/database/transparent-data-encryption-byok-configure)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: SYNAPSE SQL havuzlarının üretim örneklerine ve diğer kritik veya ilgili kaynaklara yönelik değişikliklerin ne zaman gerçekleştiği hakkında uyarı oluşturmak Için Azure etkinlik günlüğü Ile Azure izleyici 'yi kullanın.

Ayrıca, Azure portal kullanarak SQL SYNAPSE havuzunuzdaki veritabanları için uyarılar ayarlayabilirsiniz. Uyarılar bir e-posta gönderebilir veya bazı bir ölçüm (örneğin, veritabanı boyutu veya CPU kullanımı) eşiğe ulaştığında bir Web kancası çağırabilir.

* [Azure etkinlik günlüğü olayları için uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

* [Azure SQL SYNAPSE için uyarılar oluşturma](https://docs.microsoft.com/azure/azure-sql/database/alerts-insights-configure-portal)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: Gelişmiş veri güvenliğini etkinleştirin ve Azure SQL veritabanlarınızda güvenlik açığı değerlendirmelerinin gerçekleştirilmesi Için Azure Güvenlik Merkezi 'ndeki önerileri izleyin.

* [Azure SQL veritabanlarınızda güvenlik açığı değerlendirmeleri çalıştırma](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment)

* [Gelişmiş veri güvenliğini etkinleştirme](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)

* [Azure Güvenlik Merkezi güvenlik açığı değerlendirmesi önerilerini uygulama](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5,3: üçüncü taraf yazılım başlıkları için otomatik düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: güvenlik açığı değerlendirmesi, Azure SYNAPSE SQL 'de yerleşik olarak bulunan bir tarama hizmetidir. Hizmet, güvenlik açıklarına işaret eden kuralların bilgi bankasını kullanır. Yanlış yapılandırma, aşırı izin ve korunmayan hassas veriler gibi en iyi uygulamalardan sapmaları vurgular. Güvenlik açığı değerlendirmesi, merkezi SQL gelişmiş veri güvenliği (ADS) portalı aracılığıyla erişilebilir ve yönetilebilir.

* [SQL ADS portalındaki güvenlik açığı değerlendirme taramalarını yönetme ve dışarı aktarma](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Kılavuz**: Azure Güvenlik Merkezi tarafından sunulan varsayılan risk derecelendirmelerini (güvenli puan) kullanın.

Veri bulma &amp; sınıflandırması, Azure SYNAPSE SQL 'de yerleşik olarak bulunur. Veritabanlarınızdaki hassas verileri bulmak, sınıflandırmak, etiketlemek ve raporlamak için gelişmiş yetenekler sağlar.

* [Azure Güvenlik Merkezi güvenli Puanını anlama](https://docs.microsoft.com/azure/security-center/security-center-secure-score)

* [Veri bulma &amp; sınıflandırmasını anlama](https://docs.microsoft.com/azure/azure-sql/database/data-discovery-and-classification-overview)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

**Rehberlik**: abonelikleriniz DAHILINDE SYNAPSE SQL havuzunuz ile ilgili tüm kaynakları sorgulamak ve öğrenmek Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinleriniz olduğundan ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerinin listesini belirleyebildiğinizden emin olun.

Klasik Azure kaynakları Azure Kaynak Grafiği aracılığıyla bulunabilir, ancak ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

* [Azure Kaynak Graf ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

* [Azure aboneliklerinizi görüntüleme](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Azure RBAC 'yi anlama](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure kaynaklarına Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler verirsiniz.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: varlıkları düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

* [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynaklarının envanterini tanımlayın ve saklayın

**Kılavuz**: SYNAPSE SQL havuzunuz ile Ilgili onaylanan Azure kaynaklarının bir listesini tanımlayın.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerinde oluşturulabilen kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

Aboneliklerinizde kaynakları sorgulamak/öğrenmek için Azure Kaynak Grafiği ' ni kullanın. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Kaynak Graf ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

Aboneliklerinizin içindeki kaynakları sorgulamak/öğrenmek için Azure Kaynak grafiğini kullanın. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Ilkesiyle belirli bir kaynak türünü reddetme](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6,10: onaylanan yazılım başlıkları envanterini koruyun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynakları üzerinde çalışan uygulamalar için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak Için Azure koşullu erişimi kullanın.

* [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: SYNAPSE SQL havuzunuzla ilgili, iş işlemleri için gerekli olan, ancak kuruluşa daha fazla risk doğurabilecek herhangi bir kaynak, kendi sanal makinesi ve/veya sanal ağı içinde yalıtılmalı ve bir Azure Güvenlik Duvarı veya ağ güvenlik grubuyla yeterince güvenmelidir.

* [Sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

* [Güvenlik Yapılandırması ile NSG oluşturma](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Kılavuz**: SYNAPSE SQL havuzunuza ilişkin kaynakların yapılandırılmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için, "Microsoft. SQL" ad alanındaki Azure ilke diğer adlarını kullanın. Azure veritabanı/sunucusu için yerleşik ilke tanımlarından da yararlanabilirsiniz, örneğin:
- SQL Server 'lar üzerinde tehdit algılamayı dağıtma
- SQL Server bir sanal ağ hizmeti uç noktası kullanmalıdır

* [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Ilke efektlerini anlama](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure devops veya Azure Repos kullanın.

* [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Azure Repos belgeleri](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Azure SYNAPSE SQL, yapılandırılabilir güvenlik ayarlarına sahip değil.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

**Kılavuz**: SYNAPSE SQL havuzunuz ile ilgili herhangi bir kaynak için temel taramalar gerçekleştirmek üzere Azure Güvenlik Merkezi 'nden yararlanın.

* [Azure Güvenlik Merkezi 'nde öneriler nasıl düzeltileceği](https://docs.microsoft.com/azure/security-center/security-center-sql-service-recommendations)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: Azure Key Vault içindeki müşteri tarafından yönetilen anahtarlarla saydam veri şifrelemesi (tde), otomatik olarak oluşturulan veritabanı şifreleme anahtarı 'Nı (dek) TDE koruyucusu adlı, müşteri tarafından yönetilen bir asimetrik anahtarla şifrelemeye izin verir. Bu Ayrıca, Saydam Veri Şifrelemesi için genellikle Kendi Anahtarını Getir (BYOK) desteği olarak adlandırılır. BYOK senaryosunda, TDE koruyucusu müşterinin sahip olduğu ve yönetilen bir Azure Key Vault depolanır. Ayrıca, Azure Key Vault ' de geçici silme özelliğinin etkinleştirildiğinden emin olun.

* [Azure Key Vault 'tan müşteri tarafından yönetilen anahtarla TDE 'yi etkinleştirme](https://docs.microsoft.com/azure/azure-sql/database/transparent-data-encryption-byok-configure?tabs=azure-powershell)

* [Azure Key Vault 'da geçici silme nasıl etkinleştirilir](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) içinde otomatik olarak yönetilen kimlik ile Azure hizmetleri sağlamak Için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Azure Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

* [Öğretici: Azure SQL hizmetine erişmek için Windows VM sistem tarafından atanan yönetilen kimlik kullanma](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-sql)

* [Yönetilen kimlikleri yapılandırma](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kodunuzda kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

* [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü amaçlı yazılımdan koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir. Microsoft, temel alınan platform için kötü amaçlı yazılımdan koruma uygular.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetleri 'ni destekleyen temel alınan konakta (örneğin, Azure SYNAPSE SQL) etkinleştirilir; Ancak, müşteri içeriği üzerinde çalışmaz.

App Service, Data Lake Storage, BLOB depolama, Azure SQL Server vb. gibi işlem dışı Azure kaynaklarına yüklenen tüm içerikleri önceden tarayın. Microsoft bu örneklerdeki verilerinize erişemez.

* [Azure Cloud Services ve sanal makineler için Microsoft Antimalware 'i anlayın](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir. Microsoft, temel alınan platform için kötü amaçlı yazılımdan koruma uygular.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli otomatik arka pencerelere emin olun

**Kılavuz**: SYNAPSE SQL havuzunuzun anlık görüntüleri, yedi gün boyunca kullanılabilen geri yükleme noktaları oluşturma gününde otomatik olarak alınır. Bu saklama dönemi değiştirilemez. SQL havuzu sekiz saatlik bir kurtarma noktası hedefini (RPO) destekler. Son yedi gün içinde alınan anlık görüntülerden herhangi birinden, birincil bölgedeki veri Ambarınızı geri yükleyebilirsiniz. Gerekirse, anlık görüntüleri el ile de tetikleyebileceğinizi unutmayın.

* [Azure SYNAPSE SQL havuzunda yedekleme ve geri yükleme](/azure/synapse-analytics/sql-data-warehouse/backup-and-restore)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tam sistem yedeklemeleri gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: veri ambarınızın anlık görüntüleri, yedi gün boyunca kullanılabilen geri yükleme noktaları oluşturma gününde otomatik olarak alınır. Bu saklama dönemi değiştirilemez. SQL havuzu sekiz saatlik bir kurtarma noktası hedefini (RPO) destekler. Son yedi gün içinde alınan anlık görüntülerden herhangi birinden, birincil bölgedeki veri Ambarınızı geri yükleyebilirsiniz. Gerekirse, anlık görüntüleri el ile de tetikleyebileceğinizi unutmayın.

Veritabanı şifreleme anahtarınızı şifrelemek için müşteri tarafından yönetilen bir anahtar kullanıyorsanız, anahtarınızın yedeklendiğinden emin olun.

* [Azure SYNAPSE SQL havuzunda yedekleme ve geri yükleme](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/backup-and-restore)

* [Azure Key Vault anahtarlarını yedekleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: anlık görüntülerinizin geçerli olduğundan emin olmak için geri yükleme noktalarınızı düzenli olarak test edin. Mevcut bir SQL havuzunu geri yükleme noktasından geri yüklemek için Azure portal ya da PowerShell kullanabilirsiniz. Yedeklenen müşteri tarafından yönetilen anahtarların test geri yüklemesi.

* [Azure Key Vault anahtarlarını geri yükleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

* [Azure SYNAPSE SQL havuzunda yedekleme ve geri yükleme](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/backup-and-restore)

* [Mevcut bir SQL havuzunu geri yükleme](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-restore-active-paused-dw)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Kılavuz**: Azure SQL veritabanı 'nda, veritabanı yedeklerini 10 yıla kadar ayrı Azure Blob depolama kapsayıcılarında otomatik olarak saklamak için uzun süreli bir yedekleme bekletme ILKESIYLE (LTR) tek veya havuza alınmış bir veritabanı yapılandırabilirsiniz. Azure depolama 'daki veriler, 256 bit AES şifrelemesi kullanılarak şifrelenmiş ve şifresi çözülür, en güçlü blok şifrelemeleri kullanılabilir ve FIPS 140-2 uyumludur.

Varsayılan olarak, bir depolama hesabındaki veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Verilerinizin şifrelenmesi için Microsoft tarafından yönetilen anahtarları kullanabilir veya kendi anahtarlarınız ile şifrelemeyi yönetebilirsiniz. Kendi anahtarlarınızı Key Vault ile yönetiyorsanız, geçici silme özelliğinin etkinleştirildiğinden emin olun.

* [Azure SQL veritabanı uzun süreli yedekleme bekletmesini yönetme](https://docs.microsoft.com/azure/sql-database/sql-database-long-term-backup-retention-configure)

* [Bekleyen veri için Azure Depolama şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)

* [Key Vault 'da geçici silme nasıl etkinleştirilir](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: personel rollerinin yanı sıra olay işleme/yönetim aşamalarını tanımlayan yazılı olay yanıt planları olduğundan emin olun.

* [Azure Güvenlik Merkezi 'nde Iş akışı otomasyonlarını yapılandırma](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, her uyarıya katılma sırasını önceliklendirmenize yardımcı olmak için uyarılara önem derecesi atar. böylece, bir kaynak tehlikeye girdiğinde, hemen bu işe başlayabilirsiniz. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

* [Azure Güvenlik Merkezi'nde güvenlik uyarıları](https://docs.microsoft.com/azure/security-center/security-center-alerts-overview)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

* [NıST yayınına başvurabilirsiniz: BT planları ve özellikleri için test, eğitim ve alıştırma programları](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır.

* [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Uyarıları Sentinel 'e akışa almak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.

* [Sürekli dışarı aktarmayı yapılandırma](https://docs.microsoft.com/azure/security-center/continuous-export)

* [Uyarıları Azure Sentinel 'e akış](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: güvenlik uyarılarında ve önerilerinde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın.

* [Iş akışı otomasyonu ve Logic Apps yapılandırma](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: * [lütfen Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1.)

* [Microsoft 'un Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma sınamasını yürütme hakkında daha fazla bilgi edinebilirsiniz.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
