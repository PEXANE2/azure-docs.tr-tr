---
title: PostgreSQL için Azure veritabanı için Azure Güvenlik temeli-tek sunucu
description: PostgreSQL için Azure veritabanı-tek sunuculu güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/02/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: fb27868a06e133f6f90d0a7a18d218b74aafdd69
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89420056"
---
# <a name="azure-security-baseline-for-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı için Azure Güvenlik temeli-tek sunucu

PostgreSQL için Azure veritabanı için Azure Güvenlik temeli-tek sunucu, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](../security/benchmarks/overview.md)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](../security/benchmarks/security-baselines-overview.md).

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ağ güvenliği](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: sanal ağlar içindeki Azure kaynaklarını koruma

**Kılavuz**: özel uç noktalarla PostgreSQL Için Azure veritabanı Için özel bağlantı yapılandırın. Özel bağlantı, Azure 'daki çeşitli PaaS hizmetlerine özel bir uç nokta aracılığıyla bağlanmanızı sağlar. Azure özel bağlantısı temel olarak Azure hizmetlerini özel sanal ağınız (VNet) içinde sunar. Sanal ağınız ile PostgreSQL örneği arasındaki trafik, Microsoft omurga ağını de dolaşır.

Alternatif olarak, PostgreSQL için Azure veritabanı uygulamalarına ağ erişimini korumak ve sınırlamak için sanal ağ hizmet uç noktalarını kullanabilirsiniz. Sanal ağ kuralları, PostgreSQL için Azure veritabanı sunucunuzun, sanal ağlardaki belirli alt ağlardan gönderilen iletişimleri kabul edip etmediğini denetleyen bir güvenlik duvarı güvenlik özelliğidir.

PostgreSQL için Azure veritabanı sunucunuzu güvenlik duvarı kurallarıyla da güvenli hale bırakabilirsiniz. Sunucu güvenlik duvarı, hangi bilgisayarların izin olduğunu belirtene kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarınızı yapılandırmak için kabul edilebilir IP adreslerinin aralıklarını belirten güvenlik duvarı kuralları oluşturun. Sunucu düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz.

- [PostgreSQL için Azure veritabanı için özel bağlantı yapılandırma](howto-configure-privatelink-portal.md)

- [PostgreSQL için Azure veritabanı 'nda VNet hizmet uç noktaları ve VNet kuralları oluşturma ve yönetme](howto-manage-vnet-using-portal.md)

- [PostgreSQL için Azure veritabanı güvenlik duvarı kuralları yapılandırma](howto-manage-firewall-using-portal.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1,2: sanal ağların, alt ağların ve ağ arabirimlerinin yapılandırma ve trafiğini izleme ve günlüğe kaydetme

**Rehberlik**: PostgreSQL Için Azure veritabanı örneği özel bir uç noktayla güvenli hale geldiğinde, sanal makineleri aynı sanal ağa dağıtabilirsiniz. Veri sızdırma riskini azaltmak için bir ağ güvenlik grubu (NSG) kullanabilirsiniz. Trafik denetimi için NSG akış günlüklerini etkinleştirin ve günlükleri bir depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

- [PostgreSQL için Azure veritabanı için özel bağlantı yapılandırma](howto-configure-privatelink-portal.md)

- [NSG akış günlüklerini etkinleştirme](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Trafik Analizi etkinleştirme ve kullanma](../network-watcher/traffic-analytics.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: PostgreSQL Için Azure veritabanı Için Gelişmiş tehdit koruması kullanın. Gelişmiş tehdit koruması, veritabanlarına erişmek veya veritabanına yararlanmak için olağan dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

DDoS saldırılarına karşı koruma sağlamak için PostgreSQL için Azure veritabanı örnekleri ile ilişkili sanal ağlarda DDoS koruma standardı 'nı etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

- [PostgreSQL için Azure veritabanı için Gelişmiş tehdit koruması yapılandırma](howto-database-threat-protection-portal.md)

- [DDoS korumasını yapılandırma](../virtual-network/manage-ddos-protection.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets"></a>1,5: ağ paketlerini kaydetme

**Rehberlik**: PostgreSQL Için Azure veritabanı örneği özel bir uç noktayla güvenli hale geldiğinde, sanal makineleri aynı sanal ağa dağıtabilirsiniz. Daha sonra, veri sızdırma riskini azaltmak için bir ağ güvenlik grubu (NSG) yapılandırabilirsiniz. Trafik denetimi için NSG akış günlüklerini etkinleştirin ve günlükleri bir depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

- [NSG akış günlüklerini etkinleştirme](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Trafik Analizi etkinleştirme ve kullanma](../network-watcher/traffic-analytics.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: PostgreSQL Için Azure veritabanı Için Gelişmiş tehdit koruması kullanın. Gelişmiş tehdit koruması, veritabanlarına erişmek veya veritabanına yararlanmak için olağan dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

- [PostgreSQL için Azure veritabanı için Gelişmiş tehdit koruması yapılandırma](howto-database-threat-protection-portal.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: PostgreSQL Için Azure veritabanı örneklerine erişmesi gereken kaynaklar için, ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak üzere sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Hizmet etiketi adı (örneğin, SQL) belirterek. WestUs) bir kuralın uygun kaynak veya hedef alanında ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

Note: PostgreSQL için Azure veritabanı "Microsoft. SQL" hizmet etiketini kullanır.

- [Hizmet etiketlerini kullanma hakkında daha fazla bilgi için](../virtual-network/service-tags-overview.md)

- [PostgreSQL için Azure veritabanı için hizmet etiketi kullanımını anlama](concepts-data-access-and-security-vnet.md#terminology-and-description)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: Azure Ilkesiyle PostgreSQL Için Azure veritabanı örnekleri ile ilişkili ağ ayarları ve ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. PostgreSQL için Azure veritabanı örneklerine yönelik ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. DBforPostgreSQL" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın. Ayrıca, ağ iletişimi veya PostgreSQL için Azure veritabanı örnekleri ile ilgili yerleşik ilke tanımlarını da kullanabilirsiniz; örneğin:

- DDoS koruma standardı etkinleştirilmelidir

- PostgreSQL veritabanı sunucuları için TLS bağlantısının zorla etkinleştirilmesi gerekir

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Ağ için Azure Ilke örnekleri](/azure/governance/policy/samples)

- [Azure Blueprint oluşturma](../governance/blueprints/create-blueprint-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: meta veri ve mantıksal kuruluş sağlamak Için PostgreSQL Için Azure veritabanı örneklerine yönelik ağ güvenliği ve trafik akışı ile ilgili kaynaklar için Etiketler kullanın.

Tüm kaynakların etiketlerle oluşturulduğundan ve mevcut etiketlenmemiş kaynakları bilgilendirmek için, "etiket ve onun değeri gerektir" gibi etiketlemeyle ilgili yerleşik Azure Ilke tanımlarından herhangi birini kullanın.

Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynaklar üzerinde arama yapabilir veya eylemler gerçekleştirebilirsiniz.

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını Izleyin ve PostgreSQL Için Azure veritabanı örnekleri ile ilgili ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

- [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](/azure/azure-monitor/platform/activity-log-view)

- [Azure Izleyici 'de uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: günlüğe kaydetme ve izleme](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, günlüklerdeki zaman damgalarına yönelik PostgreSQL Için Azure veritabanı gibi Azure kaynakları için kullanılan saat kaynağını korur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: PostgreSQL Için Azure veritabanı örnekleri tarafından oluşturulan güvenlik verilerini toplamak Için tanılama ayarlarını ve sunucu günlüklerini ve alma günlüklerini etkinleştirin. Azure Izleyici 'de, Log Analytics çalışma alanı (ler) kullanarak Analizi sorgulayın ve gerçekleştirin ve uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın. Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

- [PostgreSQL için Azure veritabanı 'nda sunucu günlüklerini yapılandırma ve erişme](howto-configure-server-logs-in-portal.md)

- [PostgreSQL için Azure veritabanı için Denetim günlüklerini yapılandırma ve erişme](concepts-audit.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: denetim, güvenlik ve kaynak günlüklerine erişim Için PostgreSQL Için Azure veritabanı örneklerine yönelik tanılama ayarlarını etkinleştirin. PostgreSQL denetim günlüğünü özellikle etkinleştirip etkinleştirdiğinizden emin olun. Otomatik olarak kullanılabilen etkinlik günlükleri Olay kaynağını, tarihi, kullanıcıyı, zaman damgasını, kaynak adreslerini, hedef adreslerini ve diğer yararlı öğeleri içerir. Ayrıca, Azure etkinlik günlüğü tanılama ayarlarını da etkinleştirebilir ve günlükleri aynı Log Analytics çalışma alanına veya depolama hesabına gönderebilirsiniz.

- [PostgreSQL için Azure veritabanı 'nda sunucu günlüklerini yapılandırma ve erişme](howto-configure-server-logs-in-portal.md)

- [PostgreSQL için Azure veritabanı için Denetim günlüklerini yapılandırma ve erişme](concepts-audit.md)

- [Azure etkinlik günlüğü için tanılama ayarlarını yapılandırma](/azure/azure-monitor/platform/diagnostic-settings-legacy)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, PostgreSQL Için Azure veritabanınızı tutmak üzere kullanılan Log Analytics çalışma alanı için, saklama süresini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayın. Uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.

- [Log Analytics çalışma alanları için günlük saklama parametrelerini ayarlama](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

- [Kaynak günlüklerini bir Azure depolama hesabında depolama](/azure/azure-monitor/platform/resource-logs-collect-storage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: anormal davranışlar Için PostgreSQL Için Azure veritabanı örneklerinden günlükleri çözümleyin ve izleyin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici Log Analytics kullanın. Alternatif olarak, Azure Sentinel 'e veya üçüncü taraf SıEM 'ye yönelik verileri etkinleştirebilir.

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

- [Log Analytics hakkında daha fazla bilgi için](../azure-monitor/log-query/get-started-portal.md)

- [Azure Izleyici 'de özel sorgular gerçekleştirme](../azure-monitor/log-query/get-started-queries.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Rehberlik**: PostgreSQL Için Azure veritabanı Için Gelişmiş tehdit korumasını etkinleştirin. Gelişmiş tehdit koruması, veritabanlarına erişmek veya veritabanına yararlanmak için olağan dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

Ayrıca, PostgreSQL için sunucu günlüklerini ve tanılama ayarlarını etkinleştirebilir ve bir Log Analytics çalışma alanına Günlükler gönderebilirsiniz. Log Analytics çalışma alanınızı Azure Sentinel 'e ekleyin. Bu, bir güvenlik Orchestration otomatik yanıtı (SOAR) çözümü sağlar. Bu, güvenlik sorunlarını gidermek için PlayBook 'ları (otomatikleştirilmiş çözümlerin) oluşturulmasına ve kullanılmasına olanak tanır.

- [PostgreSQL için Azure veritabanı için Gelişmiş tehdit koruması 'nı etkinleştirme](howto-database-threat-protection-portal.md)

- [PostgreSQL için Azure veritabanı 'nda sunucu günlüklerini yapılandırma ve erişme](howto-configure-server-logs-in-portal.md)

- [PostgreSQL için Azure veritabanı için Denetim günlüklerini yapılandırma ve erişme](concepts-audit.md)

- [Azure etkinlik günlüğü için tanılama ayarlarını yapılandırma](/azure/azure-monitor/platform/diagnostic-settings-legacy)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz; PostgreSQL için Azure veritabanı, kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmuyor.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; PostgreSQL için Azure veritabanı, DNS ile ilgili günlükleri işlemez veya oluşturmuyor.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kimlik ve erişim denetimi](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: PostgreSQL örnekleri Için Azure veritabanınızın denetim düzlemine (ör. Azure Portal) yönetici erişimi olan kullanıcı hesaplarının envanterini saklayın. Ayrıca, PostgreSQL için Azure veritabanı örnekleri için veri düzlemine erişimi olan yönetim hesaplarının envanterini saklayın (veritabanı içinde). (PostgreSQL sunucusu oluştururken, yönetici kullanıcı için kimlik bilgilerini sağlarsınız. Bu yönetici, ek PostgreSQL kullanıcıları oluşturmak için kullanılabilir.)

PostgreSQL için Azure veritabanı yerleşik rol tabanlı erişim denetimini desteklemez, ancak belirli kaynak sağlayıcısı işlemlerine göre özel roller oluşturabilirsiniz.

- [Azure aboneliği için özel rolleri anlama](../role-based-access-control/custom-roles.md) 

- [PostgreSQL için Azure veritabanı kaynak sağlayıcısı işlemlerini anlama](../role-based-access-control/resource-provider-operations.md#microsoftdbforpostgresql) 

- [PostgreSQL için Azure veritabanı erişim yönetimini anlama](concepts-security.md#access-management)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Kılavuz**: Azure Active Directory ve PostgreSQL Için Azure veritabanı varsayılan parola kavramına sahip değildir.

PostgreSQL için Azure veritabanı kaynağı oluşturulduktan sonra, Azure, bir yönetici kullanıcı tarafından güçlü bir parola oluşturulmasını zorlar. Ancak, PostgreSQL örneği oluşturulduktan sonra, daha fazla kullanıcı oluşturmak ve bunlara yönetici erişimi vermek için oluşturduğunuz ilk sunucu yönetici hesabını kullanabilirsiniz. Bu hesapları oluştururken, her hesap için farklı ve güçlü bir parola yapılandırmadiğinizden emin olun.

- [PostgreSQL için Azure veritabanı için ek hesaplar oluşturma](howto-create-users.md)

- [Yönetici parolasını güncelleştirme](howto-create-manage-server-portal.md#update-admin-password)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: PostgreSQL Için Azure veritabanı örneklerine erişimi olan adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın. 

- [Azure Güvenlik Merkezi kimlik ve erişimini anlama](../security-center/security-center-identity-access.md) 

- [PostgreSQL için Azure veritabanı 'nda yönetici kullanıcılar oluşturmayı anlayın](howto-create-users.md#the-server-admin-account)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3,4: Azure Active Directory çoklu oturum açma kullan (SSO)

**Rehberlik**: PostgreSQL Için Azure veritabanı 'nda oturum açma, hem veritabanında doğrudan yapılandırılan Kullanıcı adı/parola hem de bir Azure ACTIVE DIRECTORY (ad) kimliği kullanarak ve bağlanmak Için BIR Azure AD belirtecinin kullanılmasıyla desteklenir. Bir Azure AD belirteci kullanırken, bir Azure AD kullanıcısı, bir Azure AD grubu veya veritabanına bağlanan bir Azure AD uygulaması gibi farklı yöntemler desteklenir.

Ayrıca, PostgreSQL için denetim düzlemi erişimi, REST API aracılığıyla kullanılabilir ve SSO 'yu destekler. Kimlik doğrulaması yapmak için isteklerinizin yetkilendirme üst bilgisini Azure Active Directory aldığınız JSON Web Token ayarlayın.

- [PostgreSQL için Azure veritabanı 'nda kimlik doğrulaması için Azure Active Directory kullanın](howto-configure-sign-in-aad-authentication.md)

- [PostgreSQL için Azure veritabanı REST API anlayın](/rest/api/postgresql/)

- [Azure AD ile SSO 'yu anlama](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure Active Directory MULTI-Factor AUTHENTICATION (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin. Veritabanınızda oturum açmak için Azure AD belirteçlerini kullanırken, veritabanı oturum açma işlemleri için çok faktörlü kimlik doğrulaması gerektirmesini sağlar.

- [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

- [PostgreSQL için Azure veritabanı 'nda kimlik doğrulaması için Azure Active Directory kullanın](howto-configure-sign-in-aad-authentication.md)

- [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3,6: yönetim görevleri için güvenli, Azure tarafından yönetilen iş istasyonları kullanın

**Kılavuz**: Azure kaynaklarını açmak ve yapılandırmak için yapılandırılmış MULTI-Factor AUTHENTICATION (MFA) Ile ayrıcalıklı erişim iş Istasyonları (Paw) kullanın.

- [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](/windows-server/identity/securing-privileged-access/privileged-access-workstations)

- [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinliklerle ilgili günlüğe kaydet ve uyar

**Rehberlik**: şüpheli etkinlik uyarıları oluşturmak Için PostgreSQL Için Azure veritabanı Için Gelişmiş tehdit koruması 'nı etkinleştirin.

Ayrıca, ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda Günlükler ve uyarılar oluşturmak için Azure Active Directory (AD) Privileged Identity Management (PıM) kullanabilirsiniz.

Riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın.

- [PostgreSQL için Azure veritabanı için Gelişmiş tehdit koruması ayarlama](howto-database-threat-protection-portal.md)

- [Privileged Identity Management dağıtma (PıM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Azure AD risk algılamalarını anlama](/azure/active-directory/reports-monitoring/concept-risk-events)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: portala izin vermek ve IP adresi aralıklarının ya da ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişim Azure Resource Manager Için, koşullu erişim adlı konum kullanın.

- [Azure 'da adlandırılmış konumları yapılandırma](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

PostgreSQL için Azure veritabanı 'nda oturum açmak için Azure AD kullanılması ve bağlanmak üzere bir Azure AD belirteci kullanmanız önerilir. Bir Azure AD belirteci kullanırken, bir Azure AD kullanıcısı, bir Azure AD grubu veya veritabanına bağlanan bir Azure AD uygulaması gibi farklı yöntemler desteklenir.

Azure AD kimlik bilgileri ayrıca, PostgreSQL yönetici hesaplarını denetlemek için yönetim düzlemi düzeyinde (ör. Azure portal) yönetim için de kullanılabilir.

- [PostgreSQL için Azure veritabanı 'nda kimlik doğrulaması için Azure Active Directory kullanın](howto-configure-sign-in-aad-authentication.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: PostgreSQL Için Azure veritabanı yönetici rollerine sahip olabilecek eski hesapları bulmaya yardımcı olması için Azure Active Directory günlüklerini gözden geçirin. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek için Azure kimlik erişimi Incelemelerini kullanın, PostgreSQL için Azure veritabanı 'na erişmek üzere kullanılabilecek kurumsal uygulamalara erişin ve rol atamaları. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişiminin her 90 gün gibi düzenli aralıklarla gözden geçirilmesi gerekir.

- [Azure AD raporlamayı anlama](/azure/active-directory/reports-monitoring/)

- [Azure kimlik erişimi Incelemelerini kullanma](../active-directory/governance/access-reviews-overview.md)

- [PostgreSQL kullanıcılarını ve atanan rolleri gözden geçirin](https://www.postgresql.org/docs/current/database-roles.html)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

**Rehberlik**: tüm günlükleri bir Log Analytics çalışma alanına göndererek PostgreSQL Için Azure veritabanı ve Azure Active Directory Için tanılama ayarlarını etkinleştirin. Log Analytics içinde istenen uyarıları (başarısız kimlik doğrulama girişimleri gibi) yapılandırın.

- [PostgreSQL için Azure veritabanı 'nda sunucu günlüklerini yapılandırma ve erişme](howto-configure-server-logs-in-portal.md)

- [PostgreSQL için Azure veritabanı için Denetim günlüklerini yapılandırma ve erişme](concepts-audit.md)

- [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapması üzerinde uyarı

**Rehberlik**: şüpheli etkinlik uyarıları oluşturmak Için PostgreSQL Için Azure veritabanı Için Gelişmiş tehdit koruması 'nı etkinleştirin.

Algılanan şüpheli eylemlere yönelik otomatik yanıtları yapılandırmak için Azure Active Directory kimlik koruması ve risk algılama özelliklerini kullanın. Kuruluşunuzun güvenlik yanıtlarını uygulamak için Azure Sentinel aracılığıyla otomatikleştirilmiş yanıtları etkinleştirebilirsiniz.

Ayrıca, daha fazla araştırma için günlükleri Azure Sentinel 'e aktarabilirsiniz.

- [PostgreSQL için Azure veritabanı için Gelişmiş tehdit koruması ayarlama](howto-database-threat-protection-portal.md)

- [Azure AD Kimlik Koruması genel bakış](../active-directory/identity-protection/overview-identity-protection.md)

- [Azure AD riskli oturum açma işlemlerini görüntüleme](/azure/active-directory/reports-monitoring/concept-risk-events)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: Şu anda kullanılamıyor; Müşteri Kasası henüz PostgreSQL için Azure veritabanı için desteklenmiyor.

- [Desteklenen Müşteri Kasası hizmetleri listesi](../security/fundamentals/customer-lockbox-overview.md#supported-services-and-scenarios-in-general-availability)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri koruma](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: etiketi, PostgreSQL Için Azure veritabanı örneklerine veya gizli bilgileri depolayan veya işleyen ilgili kaynaklara yardımcı olmak için kullanın.

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. PostgreSQL örnekleri için Azure veritabanınıza ağ erişimini yalıtmak ve sınırlamak üzere özel bağlantı, hizmet uç noktaları ve/veya Güvenlik Duvarı kurallarının bir birleşimini kullanın.

- [Ek Azure abonelikleri oluşturma](/azure/billing/billing-create-subscription)

- [Yönetim Grupları oluşturma](/azure/governance/management-groups/create)

- [PostgreSQL için Azure veritabanı için özel bağlantı yapılandırma](howto-configure-privatelink-portal.md)

- [PostgreSQL için Azure veritabanı 'nda VNet hizmet uç noktaları ve VNet kuralları oluşturma ve yönetme](howto-manage-vnet-using-portal.md)

- [PostgreSQL için Azure veritabanı güvenlik duvarı kuralları yapılandırma](concepts-firewall-rules.md)

**Azure Güvenlik Merkezi izleme**: kullanılamıyor

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Kılavuz**: Azure sanal makinelerini PostgreSQL Için Azure veritabanı örneklerine erişmek üzere kullanırken, veri ayıklanma olasılığını azaltmak Için özel bağlantı, PostgreSQL ağ yapılandırması, ağ güvenlik grupları ve hizmet etiketleri kullanın.

Microsoft, PostgreSQL için Azure veritabanı 'nın temel altyapısını yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

- [PostgreSQL için Azure veritabanı 'nda veri alımını azaltma](concepts-data-access-and-security-private-link.md)

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: PostgreSQL Için Azure veritabanı, daha önce GÜVENLI yuva KATMANı (SSL) olarak bilinen aktarım katmanı GÜVENLIĞI (TLS) kullanarak PostgreSQL sunucunuzu istemci uygulamalarına bağlamayı destekler. Veritabanı sunucunuz ile istemci uygulamalarınız arasında TLS bağlantılarının uygulanması, sunucu ile uygulamanız arasındaki veri akışını şifreleyerek "ortadaki adam" saldırılarına karşı korunmaya yardımcı olur. Azure portal, varsayılan olarak tüm PostgreSQL için Azure veritabanı örneklerine "SSL bağlantısını zorla" özelliğinin etkinleştirildiğinden emin olun.

Şu anda PostgreSQL için Azure veritabanı 'nda desteklenen TLS sürümü TLS 1,0, TLS 1,1, TLS 1,2.

- [PostgreSQL için Azure veritabanı için geçiş sırasında şifrelemeyi yapılandırma](concepts-ssl-connection-security.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz PostgreSQL Için Azure veritabanı 'nda kullanılamıyor. Uyumluluk amacıyla gerekliyse üçüncü taraf çözümünü uygulayın.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için rol tabanlı erişim denetimi kullanma

**Rehberlik**: PostgreSQL Için Azure veritabanı denetim düzlemi 'ne erişimi denetlemek için Azure rol tabanlı erişim denetimi 'Ni (Azure RBAC) kullanın (ör. Azure Portal). Veri düzlemi erişimi için (veritabanının kendisi içinde), SQL sorgularını kullanarak kullanıcı oluşturun ve Kullanıcı izinlerini yapılandırın. Azure RBAC, veritabanı içindeki kullanıcı izinlerini etkilemez.

- [Azure RBAC 'yi yapılandırma](../role-based-access-control/role-assignments-portal.md)

- [PostgreSQL için Azure veritabanı için SQL ile Kullanıcı erişimini yapılandırma](howto-create-users.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft, PostgreSQL için Azure veritabanı 'nın temel altyapısını yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: PostgreSQL Için Azure veritabanı hizmeti, bekleyen verilerin depolama ŞIFRELEMESI için FIPS 140-2 tarafından doğrulanan şifreleme modülünü kullanır. Yedeklemeler de dahil olmak üzere veriler, sorgular çalıştırılırken oluşturulan geçici dosyalar hariç olmak üzere diskte şifrelenir. Hizmet, Azure depolama şifrelemesi 'ne dahil olan AES 256 bitlik şifrelemeyi kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz.

PostgreSQL için Azure veritabanı için müşteri tarafından yönetilen anahtarlarla veri şifreleme (CMK) tek sunucu, bekleyen veri koruması için kendi anahtarınızı (BYOK) yapmanızı sağlar. Şu anda bu özelliği kullanmak için erişim istemeniz gerekir. Bunu yapmak için şu bağlantıya başvurun:

AskAzureDBforPostgreSQL@service.microsoft.com.

- [PostgreSQL için Azure veritabanı 'nda bekleyen şifrelemeyi anlama](concepts-security.md)

- [Müşteri tarafından yönetilen anahtarları kullanarak PostgreSQL için Azure veritabanı için bekleyen şifrelemeyi anlayın](concepts-data-encryption-postgresql.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Izleyici 'Yi PostgreSQL Için Azure veritabanı ve diğer kritik veya ilgili kaynakların üretim örneklerine ne zaman gerçekleştiği hakkında uyarı oluşturmak Için Azure etkinlik günlüğü ile birlikte kullanın.

- [Azure etkinlik günlüğü olayları için uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: güvenlik açığı yönetimi](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: PostgreSQL Için Azure veritabanınızı ve ilgili kaynakları güvenli hale getirmek Için Azure Güvenlik Merkezi önerilerini izleyin.

Microsoft, PostgreSQL için Azure veritabanı 'nı destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

- [Azure Güvenlik Merkezi önerilerini anlama](../security-center/recommendations-reference.md)

- [Azure Güvenlik Merkezi 'nde Azure PaaS hizmetleri için özellik kapsamı](../security-center/features-paas.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5,3: üçüncü taraf yazılım başlıkları için otomatik düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: Microsoft, PostgreSQL Için Azure veritabanı 'nı destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: envanter ve varlık yönetimi](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

**Rehberlik**: abonelikleriniz dahilinde tüm kaynakları (PostgreSQL Için Azure veritabanı örnekleri dahil) sorgulamak ve öğrenmek Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinleriniz olduğundan ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerinin listesini belirleyebildiğinizden emin olun.

- [Azure Kaynak Graf ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

- [Azure aboneliklerinizi görüntüleme](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [Azure RBAC 'yi anlama](../role-based-access-control/overview.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Rehberlik**: PostgreSQL Için Azure veritabanı örneklerine ve diğer ilgili kaynaklara Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler sağlar.

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: PostgreSQL Için Azure veritabanı örnekleri ve ilgili kaynakları düzenlemek ve izlemek için etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

- [Ek Azure abonelikleri oluşturma](/azure/billing/billing-create-subscription)

- [Yönetim Grupları oluşturma](/azure/governance/management-groups/create)

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynaklarının envanterini tanımlayın ve saklayın

**Rehberlik**: uygulanamaz; Bu öneri, bir bütün olarak işlem kaynaklarına ve Azure 'a yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Ayrıca, Azure Kaynak grafiğini kullanarak abonelikler içindeki kaynakları sorgular/bulur.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Graph ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: uygulanamaz; Bu öneri, bir bütün olarak işlem kaynaklarına ve Azure 'a yöneliktir.

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

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6,10: onaylanan yazılım başlıkları envanterini koruyun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak Için Azure koşullu erişimini kullanın. Bu, önemli bilgiler içeren PostgreSQL için Azure veritabanı örnekleri gibi yüksek bir güvenlik ortamındaki kaynaklarda oluşturma ve değişiklik yapılmasını önleyebilir.

- [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: güvenli yapılandırma](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Kılavuz**: Azure Ilkesiyle PostgreSQL Için Azure veritabanı örneklerine yönelik standart güvenlik yapılandırması tanımlayın ve uygulayın. PostgreSQL için Azure veritabanı örneklerine yönelik ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. DBforPostgreSQL" ad alanındaki Azure Ilke diğer adlarını kullanın. Ayrıca, PostgreSQL için Azure veritabanı örnekleri ile ilgili yerleşik ilke tanımlarını kullanabilirsiniz, örneğin:

- PostgreSQL veritabanı sunucuları için TLS bağlantısının zorla etkinleştirilmesi gerekir

- PostgreSQL veritabanı sunucuları için günlük bağlantıları etkinleştirilmelidir

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Rehberlik**: PostgreSQL Için Azure veritabanı örnekleri ve ilgili kaynaklar Için özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos kullanın.

- [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure Repos belgeleri](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturmak üzere "Microsoft. DBforPostgreSQL" ad alanındaki Azure ilke diğer adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturmak üzere "Microsoft. DBforPostgreSQL" ad alanındaki Azure ilke diğer adlarını kullanın. PostgreSQL için Azure veritabanı örneklerine ve ilgili kaynaklara yönelik konfigürasyonları otomatik olarak zorlamak için [Denetim], [reddetme] ve [dağıtım yok] Azure Ilkesini kullanın.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: PostgreSQL Için Azure veritabanı örneklerine erişmek üzere kullanılan Azure App Service üzerinde çalışan Azure sanal makineleri veya Web uygulamaları Için, PostgreSQL Için Azure veritabanı gizli yönetimini basitleştirmek ve güvenli hale getirmek üzere Azure Key Vault ile birlikte yönetilen hizmet kimliği kullanın. Key Vault geçici silmenin etkinleştirildiğinden emin olun.

- [Azure yönetilen kimliklerle tümleştirme](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

- [Key Vault oluşturma](../key-vault/general/quick-create-portal.md)

- [Yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama](/azure/key-vault/general/managed-identity)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: PostgreSQL Için Azure veritabanı sunucusu, veritabanlarına erişmek için Azure Active Directory kimlik doğrulamasını destekler.  PostgreSQL için Azure veritabanı sunucusu oluştururken, yönetici kullanıcı için kimlik bilgilerini sağlarsınız. Bu yönetici, ek veritabanı kullanıcıları oluşturmak için kullanılabilir.  

PostgreSQL için Azure veritabanı sunucusuna erişmek üzere kullanılan Azure App Service üzerinde çalışan Azure sanal makineleri veya Web uygulamaları için, PostgreSQL için Azure veritabanı sunucusu için kimlik bilgilerini depolamak ve almak üzere Azure Key Vault ile birlikte Yönetilen Hizmet Kimliği kullanın. Key Vault geçici silmenin etkinleştirildiğinden emin olun.

Azure Active Directory (AD) içinde otomatik olarak yönetilen bir kimlik ile Azure hizmetleri sağlamak için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

- [Yönetilen kimlikleri yapılandırma](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)

- [Azure yönetilen kimliklerle tümleştirme](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

- [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü amaçlı yazılımdan koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kötü amaçlı yazılımdan koruma](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure App Service) etkinleştirilir, ancak müşteri içeriklerde çalıştırılmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, PostgreSQL Için Azure veritabanı) etkinleştirilir, ancak müşteri içeriğinde çalışmaz.

App Service, Data Lake Storage, BLOB depolama, PostgreSQL için Azure veritabanı vb. gibi işlem dışı Azure kaynaklarına karşıya yüklenen tüm içerikleri önceden tarayın. Microsoft bu örneklerdeki verilerinize erişemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, PostgreSQL için Azure veritabanı) etkinleştirilir, ancak müşteri içeriklerde çalıştırılmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri kurtarma](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli otomatik arka pencerelere emin olun

**Rehberlik**: PostgreSQL Için Azure veritabanı, veri dosyalarının ve işlem günlüğünün yedeklerini alır. Desteklenen en fazla depolama boyutuna bağlı olarak, tam ve fark yedeklemeleri (4 TB maksimum depolama sunucusu) veya anlık görüntü yedeklemeleri (en fazla 16 TB depolama sunucusu) sunuyoruz. Bu yedeklemeler, yapılandırılmış yedekleme saklama döneminizin içindeki herhangi bir zamanda bir sunucuyu geri yüklemenize olanak tanır. Varsayılan yedekleme saklama süresi yedi gündür. İsteğe bağlı olarak 35 güne kadar yapılandırma yapabilirsiniz. Tüm yedeklemeler AES 256 bit şifreleme kullanılarak şifrelenir.

- [PostgreSQL için Azure veritabanı 'nda sunucu yedekleme](howto-restore-server-portal.md)

- [PostgreSQL için Azure veritabanı başlangıç yapılandırmasını anlama](tutorial-design-database-using-azure-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tam sistem yedeklemeleri gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: PostgreSQL Için Azure veritabanı otomatik olarak sunucu yedeklemeleri oluşturur ve Kullanıcı seçimine göre yerel olarak yedekli veya coğrafi olarak yedekli depolamada depolar. Sunucunuzu belirli bir noktaya geri yüklemek için yedeklemeler kullanılabilir. Yedekleme ve geri yükleme, verilerinizi yanlışlıkla bozulmasından veya silmekten koruyan bir iş sürekliliği stratejisinin önemli bir parçasıdır.

PostgreSQL için Azure veritabanı örneklerine ait kimlik bilgilerini depolamak üzere Azure Key Vault kullanıyorsanız, anahtarlarınızın düzenli otomatik yedeklemelerini sağlayın.

- [PostgreSQL için Azure veritabanı 'nda sunucu yedekleme](howto-restore-server-portal.md)

- [Key Vault anahtarlarını yedekleme](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: PostgreSQL Için Azure veritabanı 'nda geri yükleme gerçekleştirmek, özgün sunucunun yedeklemelerinden yeni bir sunucu oluşturur. İki adet geri yükleme türü vardır: zaman içindeki bir noktaya geri yükleme ve coğrafi geri yükleme. Tek noktaya geri yükleme, yedekleme artıklığı seçeneğiyle birlikte kullanılabilir ve özgün sunucunuz ile aynı bölgede yeni bir sunucu oluşturur. Coğrafi geri yükleme yalnızca, sunucunuzu coğrafi olarak yedekli depolama için yapılandırdıysanız kullanılabilir ve sunucunuzu farklı bir bölgeye geri yüklemenize olanak tanır.

Tahmini kurtarma süresi, veritabanı boyutları, işlem günlüğü boyutu, ağ bant genişliği ve aynı bölgedeki aynı bölgede Kurtarılan toplam veritabanı sayısı gibi çeşitli faktörlere bağlıdır. Kurtarma zamanı genellikle 12 saatten düşüktür.

PostgreSQL için Azure veritabanı örneklerini düzenli aralıklarla test edin.

- [PostgreSQL için Azure veritabanı 'nda sunucu yedekleme](howto-restore-server-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: PostgreSQL Için Azure veritabanı, tam, fark ve işlem günlüğü yedeklemeleri gerçekleştirir. Bu yedeklemeler, yapılandırılmış yedekleme saklama döneminizin içindeki herhangi bir zamanda bir sunucuyu geri yüklemenize olanak tanır. Varsayılan yedekleme saklama süresi yedi gündür. İsteğe bağlı olarak 35 güne kadar yapılandırma yapabilirsiniz. Tüm yedeklemeler AES 256 bit şifreleme kullanılarak şifrelenir.

- [PostgreSQL için Azure veritabanı 'nda yedeklemeyi ve geri yüklemeyi anlama](concepts-backup.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: olay yanıtı](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

- [Azure Güvenlik Merkezi 'nde Iş akışı otomasyonlarını yapılandırma](../security-center/security-center-planning-and-operations-guide.md)

- [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Müşteri, kendi olay yanıt planının oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu da kullanabilir](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

- [NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), müşterinin verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır.  Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

- [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Uyarılar Sentinel 'i akışa almak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.

- [Sürekli dışarı aktarmayı yapılandırma](../security-center/continuous-export.md)

- [Uyarıları Azure Sentinel 'e akış](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: güvenlik uyarılarında ve önerilerinde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın.

- [Iş akışı otomasyonu ve Logic Apps yapılandırma](../security-center/workflow-automation.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: Penetme testleri ve Red ekibi alıştırmaları](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin: https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

- [Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma testini yürütme hakkında daha fazla bilgi edinebilirsiniz.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
