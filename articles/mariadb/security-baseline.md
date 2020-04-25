---
title: MariaDB için Azure veritabanı Azure Güvenlik temeli
description: MariaDB için Azure veritabanı Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: babfd9aafa56091bfdd526366b1042966222f993
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82144604"
---
# <a name="azure-security-baseline-for-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı Azure Güvenlik temeli

MariaDB için Azure veritabanı Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Kılavuz**: özel uç noktalarla MariaDB Için Azure veritabanı Için özel bağlantı yapılandırın. Özel bağlantı, Azure 'daki çeşitli PaaS hizmetlerine özel bir uç nokta aracılığıyla bağlanmanızı sağlar. Azure özel bağlantısı temel olarak Azure hizmetlerini özel sanal ağınız (VNet) içinde sunar. Sanal ağınız ve MariaDB örneğiniz arasındaki trafik, Microsoft omurga ağını de dolaşır.

Alternatif olarak, MariaDB uygulamalarına yönelik Azure veritabanınıza ağ erişimini korumak ve sınırlamak için sanal ağ hizmet uç noktalarını kullanabilirsiniz. Sanal ağ kuralları, MariaDB için Azure veritabanınızın sanal ağlardaki belirli alt ağlardan gönderilen iletişimleri kabul edip etmediğini denetleyen bir güvenlik duvarı güvenlik özelliğidir.

Ayrıca, güvenlik duvarı kuralları ile MariaDB için Azure veritabanınızı da güvenli hale bırakabilirsiniz. Sunucu güvenlik duvarı, hangi bilgisayarların izin olduğunu belirtene kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarınızı yapılandırmak için kabul edilebilir IP adreslerinin aralıklarını belirten güvenlik duvarı kuralları oluşturun. Sunucu düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz.

MariaDB için Azure veritabanı için özel bağlantı yapılandırma:https://docs.microsoft.com/azure/mariadb/howto-configure-privatelink-portal

MariaDB sunucusu için Azure veritabanı 'nda VNet hizmet uç noktaları ve VNet kuralları oluşturma ve yönetme:https://docs.microsoft.com/azure/mariadb/howto-manage-vnet-portal

MariaDB güvenlik duvarı kuralları için Azure veritabanı 'nı yapılandırma:https://docs.microsoft.com/azure/mariadb/concepts-firewall-rules

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Rehberlik**: MariaDB sunucusu Için Azure Veritabanınız özel bir uç noktayla güvenli hale geldiğinde, sanal makineleri aynı sanal ağa dağıtabilirsiniz. Veri sızdırma riskini azaltmak için bir ağ güvenlik grubu (NSG) kullanabilirsiniz. Trafik denetimi için NSG akış günlüklerini etkinleştirin ve günlükleri bir depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

MariaDB için Azure veritabanı için özel bağlantı yapılandırma:https://docs.microsoft.com/azure/mariadb/howto-configure-privatelink-portal

NSG akış günlüklerini etkinleştirme: https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal trafik analizi etkinleştirme ve kullanma:https://docs.microsoft.com/azure/network-watcher/traffic-analytics



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: MariaDB Için Azure veritabanı Için Gelişmiş tehdit koruması kullanın. Gelişmiş tehdit koruması, veritabanlarına erişmek veya veritabanına yararlanmak için olağan dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

DDoS saldırılarına karşı koruma sağlamak için, MariaDB örnekleri için Azure veritabanlarınızla ilişkili sanal ağlarda DDoS koruma standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

MariaDB için Azure veritabanı için Gelişmiş tehdit koruması nasıl yapılandırılır:https://docs.microsoft.com/azure/mariadb/howto-database-threat-protection-portal

DDoS korumasını yapılandırma:https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Rehberlik**: MariaDB sunucusu Için Azure Veritabanınız özel bir uç noktayla güvenli hale geldiğinde, sanal makineleri aynı sanal ağa dağıtabilirsiniz. Daha sonra, veri sızdırma riskini azaltmak için bir ağ güvenlik grubu (NSG) yapılandırabilirsiniz. Trafik denetimi için NSG akış günlüklerini etkinleştirin ve günlükleri bir depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

NSG akış günlüklerini etkinleştirme: https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal trafik analizi etkinleştirme ve kullanma:https://docs.microsoft.com/azure/network-watcher/traffic-analytics



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: MariaDB Için Azure veritabanı Için Gelişmiş tehdit koruması kullanın. Gelişmiş tehdit koruması, veritabanlarına erişmek veya veritabanına yararlanmak için olağan dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.
MariaDB için Azure veritabanı için Gelişmiş tehdit koruması nasıl yapılandırılır:https://docs.microsoft.com/azure/mariadb/howto-database-threat-protection-portal


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: MariaDB örnekleri Için Azure veritabanınıza erişmesi gereken kaynaklar için, ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak üzere sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Hizmet etiketi adı (örneğin, SQL) belirterek. WestUs) bir kuralın uygun kaynak veya hedef alanında ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.
Note: MariaDB için Azure veritabanı "Microsoft. SQL" hizmet etiketini kullanır.

Hizmet etiketlerini kullanma hakkında daha fazla bilgi için https://docs.microsoft.com/azure/virtual-network/service-tags-overview : MariaDB Için Azure veritabanı için hizmet etiketi kullanımını anlayın:https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-vnet#terminology-and-description



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Kılavuz**: Azure Ilkesiyle MariaDB örnekleri Için Azure veritabanlarınızla ilişkili ağ ayarları ve ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. MariaDB örnekleri için Azure veritabanınızın ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak üzere, "Microsoft. Dbformarıdb" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın. Ayrıca, ağ veya MariaDB örnekleri için Azure veritabanı ile ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz, örneğin:

- DDoS koruma standardı etkinleştirilmelidir

- MariaDB sunucuları için özel uç nokta etkinleştirilmelidir

- MariaDB sunucusu bir sanal ağ hizmeti uç noktası kullanmalıdır

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Ağ için Azure Ilke örnekleri:https://docs.microsoft.com/azure/governance/policy/samples/

Azure Blueprint oluşturma:https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: meta veri ve mantıksal kuruluş sağlamak üzere MariaDB örneklerinizin ağ güvenliği ve trafik akışıyla ilgili kaynaklar için Etiketler kullanın.

Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın.

Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynaklar üzerinde arama yapabilir veya eylemler gerçekleştirebilirsiniz.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını Izleyin ve MariaDB örnekleri Için Azure veritabanlarınızla ilgili ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.
Azure etkinlik günlüğü olaylarını görüntüleme ve alma: https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view Azure izleyici 'de uyarı oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, günlüklerdeki zaman damgalarına yönelik Azure veritabanı gibi Azure kaynakları için kullanılan saat kaynağını korur.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: MariaDB örnekleri Için Azure veritabanınız tarafından oluşturulan güvenlik verilerini toplamak Için tanılama ayarlarını ve sunucu günlüklerini ve alma günlüklerini etkinleştirin. Azure Izleyici 'de, Log Analytics çalışma alanı (ler) kullanarak Analizi sorgulayın ve gerçekleştirin ve uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın. Alternatif olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.
MariaDB için Azure veritabanı 'nda sunucu günlüklerini yapılandırma ve erişme:https://docs.microsoft.com/azure/mariadb/concepts-server-logs

MariaDB için Azure veritabanı için Denetim günlüklerini yapılandırma ve erişme: https://docs.microsoft.com/azure/mariadb/howto-configure-audit-logs-portal Azure Sentinel 'i ekleme:https://docs.microsoft.com/azure/sentinel/quickstart-onboard



**Azure Güvenlik Merkezi izleme**: kullanılamıyor

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: denetim, güvenlik ve tanılama günlüklerine erişmek Için MariaDB örnekleri Için Azure veritabanınızda tanılama ayarlarını etkinleştirin. MariaDB denetim günlüğünü özellikle etkinleştirdiğinizden emin olun. Otomatik olarak kullanılabilen etkinlik günlükleri Olay kaynağını, tarihi, kullanıcıyı, zaman damgasını, kaynak adreslerini, hedef adreslerini ve diğer yararlı öğeleri içerir. Ayrıca, Azure etkinlik günlüğü tanılama ayarlarını da etkinleştirebilir ve günlükleri aynı Log Analytics çalışma alanına veya depolama hesabına gönderebilirsiniz.

MariaDB için Azure veritabanı 'nda sunucu günlüklerini yapılandırma ve erişme: https://docs.microsoft.com/azure/mariadb/concepts-server-logs MariaDB https://docs.microsoft.com/azure/mariadb/howto-configure-audit-logs-portal için Azure veritabanı için Denetim günlüklerini yapılandırma ve erişme: Azure etkinlik günlüğü için tanılama ayarlarını yapılandırma:https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy



**Azure Güvenlik Merkezi izleme**: kullanılamıyor

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, MariaDB günlükleri Için Azure veritabanınızı tutmak üzere kullanılan Log Analytics çalışma alanı için, saklama süresini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayın. Uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın.
Log Analytics çalışma alanları için günlük bekletme parametreleri ayarlama: https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period kaynak günlüklerini bir Azure depolama hesabında depolama:https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-storage



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: bir anormal davranış Için MariaDB örnekinizdeki günlükleri çözümleyin ve izleyin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Izleyici Log Analytics çalışma alanını kullanın. Alternatif olarak, Azure Sentinel 'e veya üçüncü taraf SıEM 'ye yönelik verileri etkinleştirebilir.

Azure Sentinel 'i ekleme:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

Log Analytics çalışma alanı hakkında daha fazla bilgi için:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal

Azure Izleyici 'de özel sorgular gerçekleştirme:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Rehberlik**: MariaDB Için Gelişmiş tehdit korumasını etkinleştirin. MariaDB için Azure veritabanı için Gelişmiş tehdit koruması, veritabanları için olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri algılar.

Ayrıca, MariaDB için sunucu günlüklerini ve tanılama ayarlarını etkinleştirebilir ve bir Log Analytics çalışma alanına Günlükler gönderebilirsiniz. Log Analytics çalışma alanınızı Azure Sentinel 'e ekleyin. Bu, bir güvenlik Orchestration otomatik yanıtı (SOAR) çözümü sağlar. Bu, güvenlik sorunlarını gidermek için PlayBook 'ları (otomatikleştirilmiş çözümlerin) oluşturulmasına ve kullanılmasına olanak tanır.

MariaDB için Gelişmiş tehdit korumasını etkinleştirme:https://docs.microsoft.com/azure/mariadb/howto-database-threat-protection-portal

MariDB için sunucu günlüklerini yapılandırma ve erişme:https://docs.microsoft.com/azure/mariadb/concepts-server-logs

MariaDB için Denetim günlüklerini yapılandırma ve erişme:https://docs.microsoft.com/azure/mariadb/howto-configure-audit-logs-portal

Azure Sentinel 'i ekleme:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: yok; MariaDB, kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: yok; MariaDB, DNS ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: yok; Kıyaslama işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve Access Control](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: MariaDB örneklerinizin yönetim düzlemine (Azure portal/Azure Resource Manager) yönetici erişimi olan kullanıcı hesaplarının envanterini saklayın. Ayrıca, MariaDB örneklerinizin veri düzlemine erişimi olan yönetim hesaplarının envanterini saklayın. (MariaDB sunucusunu oluştururken, yönetici kullanıcı için kimlik bilgilerini sağlarsınız. Bu yönetici, ek MariaDB kullanıcıları oluşturmak için kullanılabilir.)

MariaDB için erişim yönetimini anlayın:https://docs.microsoft.com/azure/mariadb/concepts-security#access-management

Azure abonelikleri için yerleşik RBAC rollerini anlayın:https://docs.microsoft.com/azure/role-based-access-control/built-in-roles


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: Azure Active Directory varsayılan parola kavramına sahip değildir.

MariaDB kaynağı oluşturulduktan sonra Azure, güçlü bir parola ile yönetici kullanıcı oluşturmaya zorlar. Ancak, MariaDB örneği oluşturulduktan sonra ek kullanıcılar oluşturmak ve bunlara yönetici erişimi vermek için oluşturduğunuz ilk sunucu yönetici hesabını kullanabilirsiniz. Bu hesapları oluştururken, her hesap için farklı ve güçlü bir parola yapılandırmadiğinizden emin olun.

MariaDB için ek hesaplar oluşturma:https://docs.microsoft.com/azure/mariadb/howto-create-users


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: MariaDB örneklerinizin erişimine sahip adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Azure Güvenlik Merkezi kimliğini ve erişimini anlayın:https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: MariaDB 'ye veri düzlemi erişimi, veritabanı içinde depolanan kimlikler tarafından DENETLENIR ve SSO 'yu desteklemez. MariaDB için denetim düzlemi erişimi, REST API aracılığıyla kullanılabilir ve SSO 'yu destekler. Kimlik doğrulaması yapmak için isteklerinizin yetkilendirme üst bilgisini Azure Active Directory aldığınız JSON Web Token ayarlayın.

MariaDB için Azure veritabanı REST API anlayın:https://docs.microsoft.com/rest/api/mariadb/

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Kılavuz**: Azure AD MFA 'yı etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

Azure 'da MFA 'yı etkinleştirme:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme:https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure kaynaklarını açmak ve YAPıLANDıRMAK için MFA Ile Paws (ayrıcalıklı erişim iş istasyonları) kullanın.

Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin:https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure 'da MFA 'yı etkinleştirme:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Rehberlik**: şüpheli etkinlik uyarıları oluşturmak Için MariaDB Için Gelişmiş tehdit koruması 'nı etkinleştirin.

Ayrıca, ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda Günlükler ve uyarılar oluşturmak için Azure AD Privileged Identity Management (PıM) kullanabilirsiniz. Riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın.

MariaDB için Gelişmiş tehdit koruması ayarlama:https://docs.microsoft.com/azure/mariadb/howto-database-threat-protection-portal

Privileged Identity Management dağıtma (PıM):https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan

Azure AD risk algılamalarını anlama:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: MariaDB gibi Azure kaynaklarına erişimi SıNıRLANDıRMAK için IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek üzere, koşullu erişim adlı konum kullanın.

Azure 'da adlandırılmış konumlar nasıl yapılandırılır:https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (AAD) kullanın. AAD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. AAD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

Azure AD kimlik doğrulaması, MariaDB veri düzlemine doğrudan erişim için kullanılamaz, ancak Azure AD kimlik bilgileri, yönetim düzlemi düzeyinde (ör. Azure portal) yönetim için, MariaDB yönetici hesaplarını denetlemek için kullanılabilir.

MariaDB için yönetici parolasını güncelleştirme:https://docs.microsoft.com/azure/mariadb/howto-create-manage-server-portal#update-admin-password

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: MariaDB yönetim rollerine sahip olabilecek eski hesapların keşfedilmesine yardımcı olmak için Azure Active Directory günlüklerini gözden geçirin. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek için Azure kimlik erişimi Incelemelerini kullanın, MariaDB ve rol atamalarına erişmek için kullanılabilecek kurumsal uygulamalara erişin. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişiminin her 90 gün gibi düzenli aralıklarla gözden geçirilmesi gerekir.

Azure AD raporlamayı anlayın:https://docs.microsoft.com/azure/active-directory/reports-monitoring/

Azure kimlik erişimi Incelemelerini kullanma:https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Rehberlik**: tüm günlükleri bir Log Analytics çalışma alanına göndermek Için MariaDB ve Azure Active Directory Için tanılama ayarlarını etkinleştirin. Log Analytics çalışma alanındaki istenen uyarıları (başarısız kimlik doğrulama girişimleri gibi) yapılandırın.

MariaDB için sunucu günlüklerini yapılandırma ve erişme:https://docs.microsoft.com/azure/mariadb/concepts-server-logs

MariaDB için Denetim günlüklerini yapılandırma ve erişme:https://docs.microsoft.com/azure/mariadb/howto-configure-audit-logs-portal

Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme:https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure Güvenlik Merkezi izleme**: kullanılamıyor

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: MariaDB Için Gelişmiş tehdit korumasını etkinleştirin. MariaDB için Azure veritabanı için Gelişmiş tehdit koruması, veritabanları için olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri algılar.

Algılanan şüpheli eylemlere yönelik otomatik yanıtları yapılandırmak için Azure Active Directory kimlik koruması ve risk algılama özelliklerini kullanın. Kuruluşunuzun güvenlik yanıtlarını uygulamak için Azure Sentinel aracılığıyla otomatikleştirilmiş yanıtları etkinleştirebilirsiniz.

MariaDB için Gelişmiş tehdit korumasını etkinleştirme:https://docs.microsoft.com/azure/mariadb/howto-database-threat-protection-portal

Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme:https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

Azure AD riskli oturum açma işlemlerini görüntüleme:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Azure Sentinel 'i ekleme:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: kullanılamıyor

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: uygulanamaz; MariaDB için Azure veritabanı 'nda Müşteri Kasası henüz desteklenmiyor.

Desteklenen Müşteri Kasası hizmetleri listesi:https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: gizli bilgileri depolayan veya Işleyen, MariaDB örnekleri Için Azure veritabanı 'nı veya ilgili kaynakları izlemeye yardımcı olması için etiketleri kullanın.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. MariaDB örneklerinizin ağ erişimini yalıtmak ve sınırlamak için özel bağlantı, hizmet uç noktaları ve/veya MariaDB Güvenlik Duvarı kurallarının bir birleşimini kullanın.

Ek Azure abonelikleri oluşturma:https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları oluşturma:https://docs.microsoft.com/azure/governance/management-groups/create

MariaDB için Azure veritabanı için özel bağlantı yapılandırma:https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-private-link

MariaDB için Azure veritabanı için hizmet uç noktalarını yapılandırma:https://docs.microsoft.com/azure/mariadb/howto-manage-vnet-portal

MariaDB için Azure veritabanı için güvenlik duvarı kuralları nasıl yapılandırılır:https://docs.microsoft.com/azure/mariadb/concepts-firewall-rules

**Azure Güvenlik Merkezi izleme**: kullanılamıyor

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Kılavuz**: Azure VM 'Lerini MariaDB örneklerine erişmek için kullanırken, veri ayıklanma olasılığını azaltmak Için özel bağlantı, MariaDB ağ yapılandırması, ağ güvenlik grupları ve hizmet etiketleri kullanın.

Microsoft, MariaDB için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

MariaDB için Azure veritabanı 'nda veri alımını azaltma:https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-private-link

Azure 'da müşteri veri korumasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: MariaDB Için Azure veritabanı, daha önce GÜVENLI yuva KATMANı (SSL) olarak bilinen aktarım katmanı GÜVENLIĞI (TLS) kullanarak MariaDB sunucusu Için Azure veritabanı 'nı istemci uygulamalarına bağlamayı destekler. Veritabanı sunucunuz ile istemci uygulamalarınız arasında TLS bağlantılarının uygulanması, sunucu ile uygulamanız arasındaki veri akışını şifreleyerek "ortadaki adam" saldırılarına karşı korunmaya yardımcı olur. Azure portal, tüm MariaDB örnekleri için "SSL bağlantısını zorla" özelliğinin etkinleştirildiğinden emin olun.

MariaDB için geçiş sırasında şifrelemeyi yapılandırma:https://docs.microsoft.com/azure/mariadb/howto-configure-ssl

**Azure Güvenlik Merkezi izleme**: kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: veri tanımlama, sınıflandırma ve kayıp önleme özellikleri, MariaDB Için Azure veritabanı 'nda henüz kullanılamıyor. Uyumluluk amacıyla gerekliyse üçüncü taraf çözümünü uygulayın.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

Azure 'da müşteri veri korumasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Rehberlik**: MariaDB yönetim düzlemi Için Azure veritabanı 'na erişimi denetlemek IÇIN Azure AD RBAC kullanın (Azure portal/Azure Resource Manager). Veri düzlemi erişimi için (veritabanının kendisi içinde), SQL sorgularını kullanarak kullanıcı oluşturun ve Kullanıcı izinlerini yapılandırın.

Azure 'da RBAC 'yi yapılandırma:https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

MariaDB için SQL ile Kullanıcı erişimini yapılandırma:https://docs.microsoft.com/azure/mariadb/howto-create-users

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: yok; Bu kılavuz işlem kaynaklarına yöneliktir.

Microsoft, MariaDB için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

Azure 'da müşteri veri korumasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: MariaDB hizmeti Için Azure veritabanı, bekleyen verilerin depolama ŞIFRELEMESI için FIPS 140-2 tarafından doğrulanan şifreleme modülünü kullanır. Yedeklemeler de dahil olmak üzere veriler, sorgular çalıştırılırken oluşturulan geçici dosyalar hariç olmak üzere diskte şifrelenir. Hizmet, Azure depolama şifrelemesi 'ne dahil olan AES 256 bitlik şifrelemeyi kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz.

MariaDB için şifreleme için geri kalanı anlayın:https://docs.microsoft.com/azure/mariadb/concepts-security

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Rehberlik**: Azure izleyici 'Yi, MariaDB Için Azure veritabanı 'nın üretim örneklerine ve diğer önemli veya ilgili kaynaklara ne zaman gerçekleştiği hakkında uyarı oluşturmak Için Azure etkinlik günlüğü ile birlikte kullanın.

Azure etkinlik günlüğü olayları için uyarı oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: Şu anda kullanılamıyor; Azure Güvenlik Merkezi, MariaDB sunucusu için Azure veritabanı 'nın güvenlik açığı değerlendirmesini henüz desteklememektedir.


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: Microsoft, MariaDB sunucusu Için Azure veritabanı 'nı destekleyen temel sistemler üzerinde güvenlik açığı yönetimi gerçekleştirir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: abonelikleriniz dahilinde (MariaDB sunucusu Için Azure veritabanı dahil) tüm kaynakları sorgulamak ve saptamak Için Azure Kaynak Grafiği 'ni kullanın. Kiracınızda uygun (okuma) izinleriniz olduğundan ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerinin listesini belirleyebildiğinizden emin olun.

Azure Kaynak Graf ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure aboneliklerinizi görüntüleme:https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure RBAC 'yi anlama:https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Rehberlik**: MariaDB sunucusu Için Azure veritabanı ve diğer ilgili kaynaklar için Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler sağlar.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: MariaDB sunucusu ve ilgili kaynaklar Için Azure veritabanı 'nı düzenlemek ve izlemek üzere uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

Ek Azure abonelikleri oluşturma:https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları oluşturma:https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini koruyun

**Rehberlik**: uygulanamaz; Bu öneri, bir bütün olarak işlem kaynaklarına ve Azure 'a yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Ayrıca, Azure Kaynak grafiğini kullanarak abonelikler içindeki kaynakları sorgular/bulur.

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

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

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Ilkesiyle belirli bir kaynak türünü reddetme:https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-divlimit-users-ability-to-interact-with-azure-resources-manager-via-scriptsdiv"></a>6,11: <div>Kullanıcıların betikler aracılığıyla Azure Resources Manager ile etkileşim kurma yeteneğini sınırlayın</div>

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak Için Azure koşullu erişimini kullanın. Bu, önemli bilgiler içeren MariaDB sunucusu için Azure veritabanı gibi yüksek bir güvenlik ortamındaki kaynaklarda oluşturma ve değişiklik yapılmasını önleyebilir.

Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma:https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management



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

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: Azure Ilkesiyle MariaDB örnekleri Için Azure veritabanınız için standart güvenlik yapılandırması tanımlayın ve uygulayın. MariaDB sunucuları için Azure veritabanınızın ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için, "Microsoft. Dbformarıdb" ad alanındaki Azure Ilke diğer adlarını kullanın. MariaDB sunucularınız için Azure veritabanı ile ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz, örneğin:

- MariaDB için Azure veritabanı 'nda coğrafi olarak yedekli yedekleme etkinleştirilmelidir

Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme:https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Ilke efektlerini anlayın:https://docs.microsoft.com/azure/governance/policy/concepts/effects



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Rehberlik**: MariaDB sunucuları ve ilgili kaynaklar Için Azure veritabanınız Için özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos kullanın.

Azure DevOps 'da kod depolama:https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Repos belgeleri:https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturmak üzere "Microsoft. Dbformarıdb" ad alanındaki Azure ilke diğer adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma izlemeyi uygulayın

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturmak üzere "Microsoft. Dbformarıdb" ad alanındaki Azure ilke diğer adlarını kullanın. MariaDB örnekleri ve ilgili kaynaklar için Azure veritabanı yapılandırmasını otomatik olarak zorlamak için [Denetim], [reddetme] ve [dağıtım yok] Azure ilkesini kullanın.

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Kılavuz**: Azure sanal makineleri veya MariaDB sunucuları Için Azure veritabanınıza erişmek üzere kullanılan Azure App Service üzerinde çalışan Web uygulamaları Için, MariaDB Server gizli yönetimi Için Azure veritabanı 'nı basitleştirmek ve güvenli hale getirmek üzere Azure Key Vault ile birlikte yönetilen hizmet kimliği kullanın. Key Vault geçici silmenin etkinleştirildiğinden emin olun.

Azure yönetilen kimliklerle tümleştirme:https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Key Vault oluşturma:https://docs.microsoft.com/azure/key-vault/quick-create-portal

Yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama:https://docs.microsoft.com/azure/key-vault/managed-identity 



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: MariaDB sunucusu Için Azure veritabanı şu anda veritabanlarına erişmek için Azure Active Directory kimlik doğrulamasını desteklemiyor.  MariaDB sunucusu için Azure veritabanı oluştururken, yönetici kullanıcı için kimlik bilgilerini sağlarsınız. Bu yönetici, ek MariaDB kullanıcıları oluşturmak için kullanılabilir.  

MariaDB sunucusu için Azure veritabanınıza erişmek üzere kullanılan Azure App Service üzerinde çalışan Azure sanal makineleri veya Web uygulamaları için, MariaDB sunucusu için Azure veritabanı 'nın kimlik bilgilerini depolamak ve almak üzere Azure Key Vault ile birlikte Yönetilen Hizmet Kimliği kullanın.  Key Vault geçici silmenin etkinleştirildiğinden emin olun.

Azure Active Directory (AD) içinde otomatik olarak yönetilen bir kimlik ile Azure hizmetleri sağlamak için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar. Yönetilen kimlikleri yapılandırma: https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm. Azure yönetilen kimliklerle tümleştirme: https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity.



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder. 

Kimlik bilgisi tarayıcısını ayarlama:https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure App Service) etkinleştirilir, ancak müşteri içeriklerde çalıştırılmaz.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, MariaDB sunucusu Için Azure veritabanı) etkinleştirilir, ancak müşteri içeriğinde çalışmaz.

App Service, Data Lake Storage, BLOB depolama, MariaDB sunucusu için Azure veritabanı vb. gibi işlem dışı Azure kaynaklarına karşıya yüklenen tüm içerikleri önceden tarayın. Microsoft bu örneklerdeki verilerinize erişemez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft 'un kötü amaçlı yazılımdan koruma özelliği, Azure hizmetlerini destekleyen temel ana bilgisayar üzerinde (örneğin, MariaDB sunucusu için Azure veritabanı) etkinleştirilir, ancak müşteri içeriklerde çalıştırılmaz.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: MariaDB Için Azure veritabanı, tam, fark ve işlem günlüğü yedeklemeleri alır.  MariaDB için Azure veritabanı otomatik olarak sunucu yedeklemeleri oluşturur ve bunları Kullanıcı tarafından yerel olarak yedekli veya coğrafi olarak yedekli depolama olarak yapılandırılmış şekilde depolar. Sunucunuzu belirli bir noktaya geri yüklemek için yedeklemeler kullanılabilir. Yedekleme ve geri yükleme, verilerinizi yanlışlıkla bozulmasından veya silmekten koruyan bir iş sürekliliği stratejisinin önemli bir parçasıdır.  Varsayılan yedekleme saklama süresi yedi gündür. İsteğe bağlı olarak 35 güne kadar yapılandırma yapabilirsiniz. Tüm yedeklemeler AES 256 bit şifreleme kullanılarak şifrelenir.

MariaDB yedeklemelerini anlayın:https://docs.microsoft.com/azure/mariadb/concepts-backup

MariaDB ilk yapılandırmasını anlayın:https://docs.microsoft.com/azure/mariadb/tutorial-design-database-using-portal



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: MariaDB Için Azure veritabanı otomatik olarak sunucu yedeklemeleri oluşturur ve bunları Kullanıcı tarafından yerel olarak yedekli veya coğrafi olarak yedekli depolama olarak yapılandırılmış şekilde depolar. Sunucunuzu belirli bir noktaya geri yüklemek için yedeklemeler kullanılabilir.  Yedekleme ve geri yükleme, verilerinizi yanlışlıkla bozulmasından veya silmekten koruyan bir iş sürekliliği stratejisinin önemli bir parçasıdır.

MariaDB sunucunuzda depolanan veriler için istemci tarafı veri şifrelemesi için Key Vault kullanıyorsanız, anahtarlarınızın düzenli otomatik yedeklemelerini sağlayın.

MariaDB yedeklemelerini anlayın:https://docs.microsoft.com/azure/mariadb/concepts-backup

Key Vault anahtarlarını yedekleme:https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: MariaDB Için Azure veritabanı 'nda, yedeklemelerin düzenli olarak sınanması için özgün sunucunun yedeklemelerinden geri yükleme gerçekleştirin. İki adet geri yükleme türü vardır: zaman içindeki bir noktaya geri yükleme ve coğrafi geri yükleme. Tek noktaya geri yükleme, yedekleme artıklığı seçeneğiyle birlikte kullanılabilir ve özgün sunucunuz ile aynı bölgede yeni bir sunucu oluşturur. Coğrafi geri yükleme yalnızca, sunucunuzu coğrafi olarak yedekli depolama için yapılandırdıysanız kullanılabilir ve sunucunuzu farklı bir bölgeye geri yüklemenize olanak tanır.

Tahmini kurtarma süresi, veritabanı boyutları, işlem günlüğü boyutu, ağ bant genişliği ve aynı bölgedeki aynı bölgede Kurtarılan toplam veritabanı sayısı gibi çeşitli faktörlere bağlıdır. Kurtarma zamanı genellikle 12 saatten düşüktür.

MariaDB için Azure veritabanı 'nda yedeklemeyi ve geri yüklemeyi anlayın:https://docs.microsoft.com/azure/mariadb/concepts-backup#restore


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: MariaDB Için Azure veritabanı, tam, fark ve işlem günlüğü yedeklemeleri alır. Bu yedeklemeler, yapılandırılmış yedekleme saklama döneminizin içindeki herhangi bir zamanda bir sunucuyu geri yüklemenize olanak tanır. Varsayılan yedekleme saklama süresi yedi gündür. İsteğe bağlı olarak 35 güne kadar yapılandırma yapabilirsiniz. Tüm yedeklemeler AES 256 bit şifreleme kullanılarak şifrelenir.

MariaDB için Azure veritabanı 'nda yedeklemeyi ve geri yüklemeyi anlayın:https://docs.microsoft.com/azure/mariadb/concepts-backup


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.
    

    Guidance on building your own security incident response process: https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

    

    Microsoft Security Response Center's Anatomy of an Incident: https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/

    

    Customer may also leverage NIST's Computer Security Incident Handling Guide to aid in the creation of their own incident response plan: https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final 



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır. 
    

    Additionally, clearly mark subscriptions (for ex. production, non-prod) using tags and create a naming system to clearly identify and categorize Azure resources, especially those processing sensitive data.  It is your responsibility to prioritize the remediation of alerts based on the criticality of the Azure resources and environment where the incident occurred.

    

    Security alerts in Azure Security Center: https://docs.microsoft.com/azure/security-center/security-center-alerts-overview

    

Azure kaynaklarınızı düzenlemek için etiketleri kullanın:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: Azure kaynaklarınızın korunmasına yardımcı olmak için, sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.
    

    Refer to NIST's publication: Guide to Test, Training, and Exercise Programs for IT Plans and Capabilities: https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.
    

    How to set the Azure Security Center Security Contact: https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Kılavuz**: Azure kaynaklarına yönelik riskleri belirlemenize yardımcı olmak Için sürekli dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e akışını sağlayabilirsiniz.
    

    How to configure continuous export: https://docs.microsoft.com/azure/security-center/continuous-export

    

    How to stream alerts into Azure Sentinel: https://docs.microsoft.com/azure/sentinel/connect-azure-security-center



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Kılavuz**: Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanarak, güvenlik uyarılarındaki "Logic Apps" aracılığıyla yanıtları otomatik olarak tetikleyin ve Azure kaynaklarınızı korumaya yönelik öneriler alın.
    

Iş akışı otomasyonunu yapılandırma ve Logic Apps:https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve 60 gün içinde tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin:

https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma testini yürütme hakkında daha fazla bilgi edinebilirsiniz:https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](https://docs.microsoft.com/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
