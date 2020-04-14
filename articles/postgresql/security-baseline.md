---
title: PostgreSQL Tek Sunucu için Azure Veritabanı için Azure Güvenlik Taban Çizgisi
description: PostgreSQL Tek Sunucu için Azure Veritabanı için Azure Güvenlik Taban Çizgisi
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 4034759dc4f20649c15236b057e348966d72409e
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81261169"
---
# <a name="azure-security-baseline-for-azure-database-for-postgresql-single-server"></a>PostgreSQL Tek Sunucu için Azure Veritabanı için Azure Güvenlik Taban Çizgisi

PostgreSQL Single Server için Azure Veritabanı için Azure Güvenlik Taban Çizgisi, dağıtımınızın güvenlik duruşunu geliştirmenize yardımcı olacak öneriler içerir.

Bu hizmetin temeli, en iyi uygulamalar kılavuzumuzla Azure'da bulut çözümlerinizi nasıl güvenebileceğinize ilişkin öneriler sunan [Azure Güvenlik Kıyaslama sürümü 1.0'dan](https://docs.microsoft.com/azure/security/benchmarks/overview)alınmıştır.

Daha fazla bilgi için [Azure Güvenlik Taban Çizgilerini genel olarak](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)görün.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için [Güvenlik Denetimi: Ağ Güvenliği'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)bakın.*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Sanal Ağınızdaki Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'nı kullanarak kaynakları koruyun

**Kılavuz**: PostgreSQL için Azure Veritabanı için Özel Bağlantıyı Özel Uç Noktaları ile Yapılandırın. Private Link, Azure'daki çeşitli PaaS hizmetlerine özel bir bitiş noktası üzerinden bağlanmanızı sağlar. Azure Özel Bağlantı, Azure hizmetlerini aslında özel Sanal Ağınıza (VNet) getirir. Sanal ağınızla PostgreSQL örneğinarasındaki trafik Microsoft omurga ağına doğru ilerler.

Alternatif olarak, PostgreSQL uygulamaları için Azure Veritabanınıza ağ erişimini korumak ve sınırlamak için Sanal Ağ Hizmeti Bitiş Noktaları'nı kullanabilirsiniz. Sanal ağ kuralları, PostgreSQL sunucusu için Azure Veritabanınızın sanal ağlardaki belirli alt ağlardan gönderilen iletişimleri kabul edip etmediğini kontrol eden bir güvenlik duvarı güvenlik özelliğidir.

Ayrıca, PostgreSQL için Azure Veritabanınızı güvenlik duvarı kurallarıyla da güvenebilirsiniz. Sunucu güvenlik duvarı, siz hangi bilgisayarların izni olduğunu belirtene kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarınızı yapılandırmak için kabul edilebilir IP adreslerinin aralıklarını belirten güvenlik duvarı kuralları oluşturun. Sunucu düzeyinde güvenlik duvarı kuralları oluşturabilirsiniz.

PostgreSQL için Azure Veritabanı için Özel Bağlantı nasıl yapılandırılır:https://docs.microsoft.com/azure/postgresql/howto-configure-privatelink-portal

PostgreSQL için Azure Veritabanı'nda VNet hizmet bitiş noktaları ve VNet kuralları oluşturma ve yönetme:https://docs.microsoft.com/azure/postgresql/howto-manage-vnet-using-portal

PostgreSQL güvenlik duvarı kuralları için Azure Veritabanı nasıl yapılandırılabilen:https://docs.microsoft.com/azure/postgresql/howto-manage-firewall-using-portal

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Vnet, Subnet ve NIC'lerin yapılandırmasını ve trafiğini izleyin ve kaydedin

**Kılavuz**: PostgreSQL örneği için Azure Veritabanınız özel bir bitiş noktasına sabitlendiğinde, sanal makineleri aynı sanal ağda dağıtabilirsiniz. Veri sızma riskini azaltmak için bir ağ güvenlik grubu (NSG) kullanabilirsiniz. NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir Depolama Hesabına gönderin. Ayrıca NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzdaki trafik akışıhakkında öngörüler sağlamak için Trafik Analitiği'ni kullanabilirsiniz. Traffic Analytics'in bazı avantajları, ağ etkinliğini görselleştirme ve etkin noktaları belirleme, güvenlik tehditlerini belirleme, trafik akışı desenlerini anlama ve ağ yanlış yapılandırmalarını saptabilme yeteneğidir.

PostgreSQL için Azure Veritabanı için Özel Bağlantı nasıl yapılandırılır:https://docs.microsoft.com/azure/postgresql/howto-configure-privatelink-portal

NSG Akış Günlükleri Nasıl Etkinleştirilir:https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Trafik Analizi ni etkinleştirme ve kullanma:https://docs.microsoft.com/azure/network-watcher/traffic-analytics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="13-protect-critical-web-applications"></a>1.3: Kritik web uygulamalarını koruyun

**Rehberlik**: Geçerli değildir; Bu öneri, Azure Uygulama Hizmeti'nde çalışan web uygulamaları veya bilgi işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Bilinen kötü amaçlı IP adresleriyle iletişimi reddetme

**Rehberlik**: PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması kullanın. Gelişmiş Tehdit Koruması, veritabanlarına erişmek veya veritabanlarından yararlanmak için alışılmadık ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

DDoS saldırılarına karşı korunmak için PostgreSQL örnekleri için Azure Veritabanınızla ilişkili sanal ağlarda DDoS Koruma Standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimi reddetmek için Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı kullanın.

PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması nasıl yapılandırılabilirsiniz:https://docs.microsoft.com/azure/postgresql/howto-database-threat-protection-portal

DDoS koruması nasıl yapılandırılabilen:https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1.5: Ağ paketlerini ve akış günlüklerini kaydetme

**Kılavuz**: PostgreSQL örneği için Azure Veritabanınız özel bir bitiş noktasına sabitlendiğinde, sanal makineleri aynı sanal ağda dağıtabilirsiniz. Daha sonra veri sızma riskini azaltmak için bir ağ güvenlik grubu (NSG) yapılandırabilirsiniz. NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir Depolama Hesabına gönderin. Ayrıca NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzdaki trafik akışıhakkında öngörüler sağlamak için Trafik Analitiği'ni kullanabilirsiniz. Traffic Analytics'in bazı avantajları, ağ etkinliğini görselleştirme ve etkin noktaları belirleme, güvenlik tehditlerini belirleme, trafik akışı desenlerini anlama ve ağ yanlış yapılandırmalarını saptabilme yeteneğidir.

NSG Akış Günlükleri Nasıl Etkinleştirilir:https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Trafik Analizi ni etkinleştirme ve kullanma:https://docs.microsoft.com/azure/network-watcher/traffic-analytics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Ağ tabanlı izinsiz giriş algılama/saldırı önleme sistemlerini (IDS/IPS) dağıtma

**Rehberlik**: PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması kullanın. Gelişmiş Tehdit Koruması, veritabanlarına erişmek veya veritabanlarından yararlanmak için alışılmadık ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması nasıl yapılandırılabilirsiniz:https://docs.microsoft.com/azure/postgresql/howto-database-threat-protection-portal

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Web uygulamalarına trafik yönetme

**Rehberlik**: Geçerli değildir; Bu öneri, Azure Uygulama Hizmeti'nde çalışan web uygulamaları veya bilgi işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Ağ güvenliği kurallarının karmaşıklığını ve yönetim ek lerini en aza indirmek

**Kılavuz**: PostgreSQL örnekleri için Azure Veritabanınıza erişimi gereken kaynaklar için, ağ güvenlik grupları veya Azure Güvenlik Duvarı'ndaki ağ erişim denetimlerini tanımlamak için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Hizmet etiketi adını belirterek (örneğin, SQL. WestUs) bir kuralın uygun kaynak veya hedef alanında, ilgili hizmet için trafik izin verebilir veya reddedebilirsiniz. Microsoft, hizmet etiketinin kapsadığı adres öneklerini yönetir ve adresler değiştikçe servis etiketini otomatik olarak güncelleştirir.

Not: PostgreSQL için Azure Veritabanı "Microsoft.Sql" hizmet etiketini kullanır.

Hizmet etiketlerini kullanma hakkında daha fazla bilgi için:https://docs.microsoft.com/azure/virtual-network/service-tags-overview

PostgreSQL için Azure Veritabanı için hizmet etiketi kullanımını anlayın:https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-vnet#terminology-and-description

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Ağ aygıtları için standart güvenlik yapılandırmalarını koruyun

**Yönerge**: Azure Politikası ile PostgreSQL örnekleri için Azure Veritabanınızla ilişkili ağ ayarları ve ağ kaynakları için standart güvenlik yapılandırmaları tanımlayın ve uygulayın. PostgreSQL örnekleri için Azure Veritabanınızın ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.DBforPostgreSQL" ve "Microsoft.Network" ad alanlarında Azure İlkesi takma adlarını kullanın. Aşağıdakiler gibi PostgreSQL örnekleri için ağ veya Azure Veritabanınızla ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz:

- DDoS Koruma Standardı etkinleştirilmelidir

- PostgreSQL veritabanı sunucuları için SSL bağlantısını uygulayın etkinleştirilmelidir

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Ağ oluşturmak için Azure İlkesi örnekleri:https://docs.microsoft.com/azure/governance/policy/samples/

Azure Blueprint nasıl oluşturulur:https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="110-document-traffic-configuration-rules"></a>1.10: Belge trafiği yapılandırma kuralları

**Kılavuz :** Meta veriler ve mantıksal organizasyon sağlamak için PostgreSQL örnekleri için Azure Veritabanınız için ağ güvenliği ve trafik akışıyla ilgili kaynaklar için etiketleri kullanın.

Tüm kaynakların etiketlerle oluşturulduğundan emin olmak ve varolan etiketlenmemiş kaynakları size bildirmek için etiketlemeyle ilgili "Etiket ve değeri iste" gibi yerleşik Azure ilke tanımlarından herhangi birini kullanın.

Azure PowerShell veya Azure CLI'yi, etiketlerine dayalı kaynaklara bakmak veya eylemler gerçekleştirmek için kullanabilirsiniz.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Ağ kaynağı yapılandırmalarını izlemek ve değişiklikleri algılamak için otomatik araçlar kullanın

**Yönerge**: PostgreSQL örnekleri için ağ kaynağı yapılandırmalarını izlemek ve Azure Veritabanınızla ilgili ağ kaynaklarındaki değişiklikleri algılamak için Azure Etkinlik Günlüğü'ne kullanın. Azure Monitor'da kritik ağ kaynaklarında değişiklikler olduğunda tetikleyecek uyarılar oluşturun.

Azure Etkinlik Günlüğü etkinliklerini görüntüleme ve alma:https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

Azure Monitor'da uyarı oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için [Güvenlik Denetimi: Günlüğe kaydetme ve izleme.](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Onaylanmış zaman senkronizasyon kaynaklarını kullanma

**Kılavuz :** Microsoft, günlüklerde zaman damgaları için PostgreSQL için Azure Veritabanı gibi Azure kaynakları için kullanılan zaman kaynağını korur.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Yönlendirme**: PostgreSQL örnekleri için Azure Veritabanınız tarafından oluşturulan güvenlik verilerini toplamak için Tanılama Ayarlarını ve Sunucu Günlüklerini etkinleştirin ve günlükleri sindirin. Azure Monitor'da, analitiği sorgulamak ve gerçekleştirmek için Log Analytics Workspace(ler)'i kullanın ve uzun vadeli/arşivdepolama için Azure Depolama Hesapları'nı kullanın. Alternatif olarak, Azure Sentinel'e veya bir üçüncü taraf SIEM'e veri etkinleştirebilir ve yerleşik veriler verebilirsiniz.

PostgreSQL için Azure Veritabanı için Sunucu Günlükleri nasıl yapılandırılabilen ve erişilebilen:https://docs.microsoft.com/azure/postgresql/howto-configure-server-logs-in-portal

PostgreSQL için Azure Veritabanı için denetim günlüklerini yapılandırma ve erişim:https://docs.microsoft.com/azure/postgresql/concepts-audit

Azure Sentinel'e nasıl binilir:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure kaynakları için denetim günlüğe kaydetmeyi etkinleştirme

**Kılavuz :** Denetim, güvenlik ve tanılama günlüklerine erişim için PostgreSQL örnekleri için Azure Veritabanınızda Tanılama Ayarlarını etkinleştirin. PostgreSQL Denetim günlüğünü özellikle etkinleştirdiğinizden emin olun. Otomatik olarak kullanılabilen etkinlik günlükleri, olay kaynağı, tarih, kullanıcı, zaman damgası, kaynak adresleri, hedef adresleri ve diğer yararlı öğeleri içerir. Ayrıca Azure Etkinlik Günlüğü Tanılama Ayarlarını etkinleştirebilir ve günlükleri aynı Log Analytics çalışma alanına veya Depolama Hesabına gönderebilirsiniz.

PostgreSQL için Azure Veritabanı için Sunucu Günlükleri nasıl yapılandırılabilen ve erişilebilen:https://docs.microsoft.com/azure/postgresql/howto-configure-server-logs-in-portal

PostgreSQL için Azure Veritabanı için denetim günlüklerini yapılandırma ve erişim:https://docs.microsoft.com/azure/postgresql/concepts-audit

Azure Etkinlik Günlüğü için Tanılama Ayarları nasıl yapılandırılır:https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: İşletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2.5: Güvenlik günlüğü depolama tutma yapılandırma

**Kılavuz :** Azure Monitor'da, PostgreSQL günlükleri için Azure Veritabanınızı tutmak için kullanılan Log Analytics Çalışma Alanı için bekletme süresini kuruluşunuzun uyumluluk yönetmeliklerine göre ayarlayın. Uzun vadeli/arşivdepolama için Azure Depolama Hesapları'nı kullanın.

Log Analytics Çalışma Alanları için günlük bekletme parametreleri nasıl ayarlanır:https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

Kaynak günlüklerini bir Azure Depolama Hesabında depolama:https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-storage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2.6: Günlükleri izleme ve inceleme

**Kılavuz :** Anormal davranışlar için PostgreSQL örnekleri için Azure Veritabanınızdaki günlükleri analiz edin ve izleyin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Monitor'un Günlük Analizi'ni kullanın. Alternatif olarak, Azure Sentinel'e veya üçüncü taraf bir SIEM'e veri etkinleştirebilir ve yerleşik veriler verebilirsiniz.

Azure Sentinel'e nasıl binilir:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

Log Analytics hakkında daha fazla bilgi için:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal

Azure Monitor'da özel sorgular nasıl gerçekleştirililir:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Anormal aktivite için uyarıları etkinleştirme

**Kılavuz**: PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması etkinleştirin. Gelişmiş Tehdit Koruması, veritabanlarına erişmek veya veritabanlarından yararlanmak için alışılmadık ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar.

Ayrıca, PostgreSQL için Sunucu Günlükleri ve Tanılama Ayarlarını etkinleştirebilir ve günlükleri bir Log Analytics Çalışma alanına gönderebilirsiniz. Bir güvenlik düzenleme otomatik yanıt (SOAR) çözümü sağladığı için Log Analytics Workspace'inizi Azure Sentinel'e dahil edin. Bu, oyun kitaplarının (otomatik çözümler) oluşturulmasına ve güvenlik sorunlarını düzeltmek için kullanılmasına olanak tanır.

PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması nasıl etkinleştirilir:https://docs.microsoft.com/azure/postgresql/howto-database-threat-protection-portal

PostgreSQL için Azure Veritabanı için Sunucu Günlükleri nasıl yapılandırılabilen ve erişilebilen:https://docs.microsoft.com/azure/postgresql/howto-configure-server-logs-in-portal

PostgreSQL için Azure Veritabanı için denetim günlüklerini yapılandırma ve erişim:https://docs.microsoft.com/azure/postgresql/concepts-audit

Azure Etkinlik Günlüğü için Tanılama Ayarları nasıl yapılandırılır:https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

Azure Sentinel'e nasıl binilir:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="28-centralize-anti-malware-logging"></a>2.8: Kötü amaçlı yazılımdan koruma günlüğe kaydetmeyi merkezileştirin

**Rehberlik**: Geçerli değildir; PostgreSQL için Azure Veritabanı, kötü amaçlı yazılımla ilgili günlükleri işlemez veya üretmez.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="29-enable-dns-query-logging"></a>2.9: DNS sorgu günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: Geçerli değildir; PostgreSQL için Azure Veritabanı, DNS ile ilgili günlükleri işlemez veya üretmez.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2.10: Komut satırı denetim günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için [güvenlik denetimi: kimlik ve erişim denetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)bakın.*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: İdari hesapların envanterini korumak

**Kılavuz :** PostgreSQL örnekleri için Azure Veritabanınızın denetim düzlemine (örneğin Azure portalı) yönetim erişimi olan kullanıcı hesaplarının envanterini koruyun. Ayrıca, PostgreSQL örnekleri için Azure Veritabanınızın veri düzlemine (veritabanının kendi içinde) erişimi olan yönetim hesaplarının envanterini koruyun. (PostgreSQL sunucusunu oluştururken, bir yönetici kullanıcı için kimlik bilgileri sağlarsınız. Bu yönetici ek PostgreSQL kullanıcıları oluşturmak için kullanılabilir.)

PostgreSQL için Azure Veritabanı yerleşik rol tabanlı erişim denetimini desteklemez, ancak belirli kaynak sağlayıcı işlemlerine dayalı özel roller oluşturabilirsiniz.

Azure aboneliği için özel rolleri anlayın:https://docs.microsoft.com/azure/role-based-access-control/custom-roles 

PostgreSQL kaynak sağlayıcısı işlemleri için Azure Veritabanını Anlayın:https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations#microsoftdbforpostgresql 

PostgreSQL için Azure Veritabanı için erişim yönetimini anlayın:https://docs.microsoft.com/azure/postgresql/concepts-security#access-management

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Varsayılan parolaları varsa değiştirme

**Kılavuz :** PostgreSQL için Azure Active Directory ve Azure Veritabanı varsayılan parola kavramına sahip değildir.

PostgreSQL kaynağı için Azure Veritabanı oluşturulduktan sonra, Azure güçlü bir parolaya sahip bir yönetim kullanıcısı oluşturulmasını zorlar. Ancak, PostgreSQL örneği oluşturulduktan sonra, oluşturduğunuz ilk sunucu yöneticisi hesabını ek kullanıcılar oluşturmak ve bunlara yönetim erişimi sağlamak için kullanabilirsiniz. Bu hesapları oluştururken, her hesap için farklı ve güçlü bir parola yapılandırdığınızdan emin olun.

PostgreSQL için Azure Veritabanı için ek hesaplar oluşturma:https://docs.microsoft.com/azure/postgresql/howto-create-users

Yönetici şifresi nasıl güncellenir:https://docs.microsoft.com/azure/postgresql/howto-create-manage-server-portal#update-admin-password

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Özel yönetim hesaplarını kullanma

**Yönerge**: PostgreSQL örnekleri için Azure Veritabanınıza erişimi olan özel yönetim hesaplarının kullanımı yla ilgili standart işletim yordamları oluşturun. Yönetim hesabı sayısını izlemek için Azure Güvenlik Merkezi Kimliği ve erişim yönetimini kullanın. 

Azure Güvenlik Merkezi Kimliğini ve Erişimini Anlayın:https://docs.microsoft.com/azure/security-center/security-center-identity-access 

PostgreSQL için Azure Veritabanı'nda yönetici kullanıcılarının nasıl oluşturulabildiğini anlayın:https://docs.microsoft.com/azure/postgresql/howto-create-users#the-server-admin-account


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Azure Active Directory ile tek oturum açma (SSO) kullanma

**Yönlendirme**: PostgreSQL için Azure Veritabanında oturum açma, hem doğrudan veritabanında yapılandırılan kullanıcı adı/parola kullanılarak hem de bir Azure Active Directory (AD) kimliği kullanılarak ve bağlanmak için bir Azure AD belirteci kullanılarak desteklenir. Azure AD belirteci kullanırken, Azure AD kullanıcısı, Azure AD grubu veya veritabanına bağlanan bir Azure AD uygulaması gibi farklı yöntemler desteklenir.

Ayrıca, PostgreSQL için kontrol düzlemi erişimi REST API üzerinden kullanılabilir ve SSO'ları destekler. Kimlik doğrulaması yapmak için, istekleriniz için Yetkilendirme üstbilgisini Azure Active Directory'den aldığınız bir JSON Web Belirteci'ne ayarlayın.

PostgreSQL için Azure Veritabanı ile kimlik doğrulaması için Azure Etkin Dizini'ni kullanın:https://docs.microsoft.com/azure/postgresql/howto-configure-sign-in-aad-authentication

PostgreSQL REST API için Azure Veritabanını Anlayın:https://docs.microsoft.com/rest/api/postgresql/

Azure AD ile SSO'yi anlayın:https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Tüm Azure Active Directory tabanlı erişim için çok faktörlü kimlik doğrulamayı kullanın

**Kılavuz**: Azure Active Directory Çok Faktörlü Kimlik Doğrulaması'nı (MFA) etkinleştirin ve Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerini izleyin. Veritabanınızda oturum açmanız için Azure AD belirteçlerini kullanırken, veritabanı oturum açmaiçin çok faktörlü kimlik doğrulaması gerektirmenize olanak tanır.

Azure'da MFA nasıl etkinleştirilir:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

PostgreSQL için Azure Veritabanı ile kimlik doğrulaması için Azure Etkin Dizini'ni kullanın:https://docs.microsoft.com/azure/postgresql/howto-configure-sign-in-aad-authentication

Azure Güvenlik Merkezi'nde kimlik ve erişim nasıl izlenir:https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Tüm idari görevler için özel makineleri (Ayrıcalıklı Erişim İş İstasyonları) kullanın

**Kılavuz :** Azure kaynaklarına giriş yapmak ve yapılandırmak üzere yapılandırılan Çok Faktörlü Kimlik Doğrulama (MFA) ile Ayrıcalıklı Erişim İş İstasyonlarını (PAWs) kullanın.

Ayrıcalıklı Erişim İş İstasyonları hakkında bilgi edinin:https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure'da MFA nasıl etkinleştirilir:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: İdari hesaplardan şüpheli etkinliklere ilişkin günlük ve uyarı

**Kılavuz**: Şüpheli etkinlikler için uyarılar oluşturmak için PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması'nı etkinleştirin.

Ayrıca, azure etkin dizin (AD) Ayrıcalıklı Kimlik Yönetimi'ni (PIM) ortamda şüpheli veya güvensiz etkinlik oluştuğunda günlük ve uyarı oluşturma için kullanabilirsiniz.

Riskli kullanıcı davranışıyla ilgili uyarıları ve raporları görüntülemek için Azure AD Risk Algılamalarını kullanın.

PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması nasıl kurulamaz:https://docs.microsoft.com/azure/postgresql/howto-database-threat-protection-portal

Ayrıcalıklı Kimlik Yönetimi (PIM) nasıl dağıtılır:https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan

Azure AD risk algılamalarını anlayın:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Yönerge**: Portal ve Azure Kaynak Yöneticisi'nin yalnızca IP adresi aralıklarının veya ülkelerin/bölgelerinin belirli mantıksal gruplandırmalarından erişmesine izin vermek için Koşullu Erişim Adlandırılmış Konumları kullanın.

Azure'da Adlandırılmış Konumlar nasıl yapılandırılabilen:https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="39-use-azure-active-directory"></a>3.9: Azure Etkin Dizini Kullanma

**Kılavuz :** Merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (AD) kullanın. Azure AD, veri için güçlü şifreleme kullanarak verileri istirahatte ve aktarım sırasında korur. Azure AD ayrıca kullanıcı kimlik bilgilerini tuzlar, iş lerle ve güvenli bir şekilde depolar.

PostgreSQL için Azure Veritabanı'nda oturum açmaiçin Azure AD'yi kullanman ve bağlanmak için bir Azure AD belirteci kullanmanız önerilir. Azure AD belirteci kullanırken, Azure AD kullanıcısı, Azure AD grubu veya veritabanına bağlanan bir Azure AD uygulaması gibi farklı yöntemler desteklenir.

Azure REKLAM kimlik bilgileri, PostgreSQL yönetici hesaplarını denetlemek için yönetim düzlemi düzeyinde (örneğin Azure portalı) yönetim için de kullanılabilir.

PostgreSQL için Azure Veritabanı ile kimlik doğrulaması için Azure Etkin Dizini'ni kullanın:https://docs.microsoft.com/azure/postgresql/howto-configure-sign-in-aad-authentication

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Kullanıcı erişimini düzenli olarak gözden geçirin ve uzlaştırın

**Yönerge**: PostgreSQL yönetim rolleri için Azure Veritabanı'na sahip hesapları içerebilen eski hesapları keşfetmeye yardımcı olmak için Azure Active Directory günlüğünü inceleyin. Ayrıca, grup üyeliklerini verimli bir şekilde yönetmek, PostgreSQL için Azure Veritabanı'na erişmek için kullanılabilecek kurumsal uygulamalara erişim ve rol atamaları için Azure Kimlik Erişim İncelemeleri'ni kullanın. Kullanıcı erişimi, yalnızca doğru Kullanıcıların sürekli erişime sahip olduğundan emin olmak için her 90 günde bir olduğu gibi düzenli olarak gözden geçirilmelidir.

Azure REKLAM Raporlama'yı anlayın:https://docs.microsoft.com/azure/active-directory/reports-monitoring/

Azure Kimlik Erişim İncelemeleri nasıl kullanılır:https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

PostgreSQL kullanıcılarını ve atanan rolleri gözden geçirin:https://www.postgresql.org/docs/current/database-roles.html

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Devre dışı bırakılmış hesaplara erişim denemelerini izleme

**Kılavuz :** PostgreSQL ve Azure Active Directory için Azure Veritabanı Tanılama Ayarlarını etkinleştirin ve tüm günlükleri bir Log Analytics çalışma alanına gönderin. Log Analytics içinde istenen uyarıları (başarısız kimlik doğrulama denemeleri gibi) yapılandırın.

PostgreSQL için Azure Veritabanı için Sunucu Günlükleri nasıl yapılandırılabilen ve erişilebilen:https://docs.microsoft.com/azure/postgresql/howto-configure-server-logs-in-portal

PostgreSQL için Azure Veritabanı için denetim günlüklerini yapılandırma ve erişim:https://docs.microsoft.com/azure/postgresql/concepts-audit

Azure Etkinlik Günlükleri Azure Monitör'e nasıl entegre eleştirilir:https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Hesap giriş davranış sapması konusunda uyarı

**Kılavuz**: Şüpheli etkinlikler için uyarılar oluşturmak için PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması'nı etkinleştirin.

Algılanan şüpheli eylemlere otomatik yanıtları yapılandırmak için Azure Active Directory'nin Kimlik Koruması ve risk algılama özelliklerini kullanın. Kuruluşunuzun güvenlik yanıtlarını uygulamak için Azure Sentinel aracılığıyla otomatik yanıtlar etkinleştirebilirsiniz.

Ayrıca, daha fazla araştırma için Azure Sentinel'deki günlükleri de alabilirsiniz.

PostgreSQL için Azure Veritabanı için Gelişmiş Tehdit Koruması nasıl kurulamaz:https://docs.microsoft.com/azure/postgresql/howto-database-threat-protection-portal

Azure AD Kimlik Koruması'na Genel Bakış:https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection

Azure AD riskli oturum açmaları nasıl görüntülenebilen:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events

Azure Sentinel'e nasıl binilir:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Destek senaryoları sırasında Microsoft'a ilgili müşteri verilerine erişim sağlama

**Rehberlik**: Şu anda mevcut değil; Müşteri Kilit Kutusu, PostgreSQL için Azure Veritabanı için henüz desteklenmedi.

Müşteri Lockbox desteklenen hizmetler listesi:https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için [güvenlik denetimi: veri koruma.](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Hassas Bilgilerin envanterini koruyun

**Yönerge**: PostgreSQL örnekleri veya hassas bilgileri depolayan veya işleyen ilgili kaynaklar için Azure Veritabanı'nı izlemeye yardımcı olmak için etiketleri kullanın.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Hassas bilgileri depolama veya işleme sistemlerini yalıtma

**Rehberlik**: Geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. PostgreSQL örnekleri için Azure Veritabanınıza ağ erişimini yalıtmak ve sınırlamak için Özel Bağlantı, Hizmet Bitiş Noktaları ve/veya güvenlik duvarı kurallarının bir birleşimini kullanın.

Ek Azure abonelikleri oluşturma:https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları nasıl oluşturulur:https://docs.microsoft.com/azure/governance/management-groups/create

PostgreSQL için Azure Veritabanı için Özel Bağlantı nasıl yapılandırılır:https://docs.microsoft.com/azure/postgresql/howto-configure-privatelink-portal

PostgreSQL için Azure Veritabanı'nda VNet hizmet bitiş noktaları ve VNet kuralları oluşturma ve yönetme:https://docs.microsoft.com/azure/postgresql/howto-manage-vnet-using-portal

PostgreSQL güvenlik duvarı kuralları için Azure Veritabanı nasıl yapılandırılabilen:https://docs.microsoft.com/azure/postgresql/concepts-firewall-rules

**Azure Güvenlik Merkezi izleme**: Kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Hassas bilgilerin yetkisiz aktarımının izlenmesi ve engellenmesi

**Kılavuz**: PostgreSQL örnekleri için Azure Veritabanı'na erişmek için Azure Sanal makineleri kullanırken, veri sızma olasılığını azaltmak için Özel Bağlantı, PostgreSQL ağ yapılandırmaları, ağ güvenlik grupları ve hizmet etiketlerini kullanın.

Microsoft, PostgreSQL için Azure Veritabanı'nın altında yatan altyapıyı yönetir ve müşteri verilerinin kaybolmasını veya açığa dökülmesini önlemek için sıkı denetimler uygulamıştır.

PostgreSQL için Azure Veritabanı için veri sızma nasıl azaltılabilen:https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-private-link

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Tüm hassas bilgileri taşıma sırasında şifreleme

**Yönlendirme**: PostgreSQL için Azure Veritabanı, PostgreSQL sunucunuzu Secure Sockets Layer (SSL) kullanarak istemci uygulamalarına bağlamayı destekler. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur. Azure portalında, varsayılan olarak PostgreSQL örnekleri için tüm Azure Veritabanınız için "SSL bağlantısını zorla" özelliğinin etkin olduğundan emin olun.

Şu anda PostgreSQL için Azure Veritabanı için desteklenen TLS sürümü TLS 1.0, TLS 1.1, TLS 1.2'dir.

PostgreSQL için Azure Veritabanı için aktarımda şifreleme yapılandırma:https://docs.microsoft.com/azure/postgresql/concepts-ssl-connection-security

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Hassas verileri tanımlamak için etkin bir bulma aracı kullanın

**Rehberlik**: PostgreSQL için Azure Veritabanı için veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz kullanılamıyor. Uyumluluk amacıyla gerekirse üçüncü taraf çözümlerini uygulayın.

Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Kaynaklara erişimi denetlemek için Azure RBAC'ı kullanın

**Yönlendirme**: PostgreSQL denetim düzlemi için Azure Veritabanı'na erişimi denetlemek için Azure rol tabanlı erişim denetimini (RBAC) kullanın (örn. Azure portalı). Veri düzlemi erişimi için (veritabanının kendi içinde), kullanıcı oluşturmak ve kullanıcı izinlerini yapılandırmak için SQL sorgularını kullanın. RBAC veritabanındaki kullanıcı izinlerini etkilemez.

Azure'da RBAC nasıl yapılandırılmaz:https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

PostgreSQL için Azure Veritabanı için SQL ile kullanıcı erişimini yapılandırma:https://docs.microsoft.com/azure/postgresql/howto-create-users

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Erişim denetimini uygulamak için ana bilgisayar tabanlı veri kaybı önlemeyi kullanın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

Microsoft, PostgreSQL için Azure Veritabanı'nın altında yatan altyapıyı yönetir ve müşteri verilerinin kaybolmasını veya açığa dökülmesini önlemek için sıkı denetimler uygulamıştır.

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Hassas bilgileri istirahatte şifreleme

**Kılavuz**: PostgreSQL için Azure Veritabanı hizmeti, verilerin dinlenme sırasında depoşifrelemesi için FIPS 140-2 doğrulanmış şifreleme modüllerini kullanır. Yedeklemeler de dahil olmak üzere veriler, sorguları çalıştırırken oluşturulan geçici dosyalar dışında diskte şifrelenir. Hizmet, Azure depolama şifrelemesinde yer alan AES 256 bit şifresini kullanır ve anahtarlar sistem tarafından yönetilir. Depolama şifrelemesi her zaman açıktır ve devre dışı bırakılamaz.

PostgreSQL Single sunucusu için Azure Veritabanı için müşteri tarafından yönetilen anahtarlarla (CMK) veri şifreleme, veri koruması için kendi anahtarınızı (BYOK) geri ödemenize olanak tanır. Şu anda, bu özelliği kullanmak için erişim istemeniz gerekir. Bunu yapmak için şu iletişim kurun:

AskAzureDBforPostgreSQL@service.microsoft.com.

PostgreSQL için Azure Veritabanı için şifrelemeyi anlayın:https://docs.microsoft.com/azure/postgresql/concepts-security

Müşteri tarafından yönetilen anahtarları kullanarak PostgreSQL için Azure Veritabanı için şifrelemeyi anlayın:https://docs.microsoft.com/azure/postgresql/concepts-data-encryption-postgresql


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Kritik Azure kaynaklarında yapılan değişiklikleri kaydedin ve uyarı

**Yönerge**: PostgreSQL ve diğer kritik veya ilgili kaynaklar için Azure Veritabanı'nın üretim örneklerinde değişiklikler olduğunda uyarılar oluşturmak için Azure Etkinlik Günlüğü ile Azure Monitörünü kullanın.

Azure Etkinlik Günlüğü etkinlikleri için uyarılar oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için [Güvenlik Denetimi: Güvenlik Açığı Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)bakın.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Otomatik güvenlik açığı tarama araçlarını çalıştırın

**Rehberlik**: Şu anda mevcut değil; Azure Güvenlik Merkezi, PostgreSQL için Azure Veritabanı için güvenlik açığı değerlendirmesini henüz desteklememektedir.

Azure Güvenlik Merkezi'ndeki Azure PaaS hizmetleri için özellik kapsamı:https://docs.microsoft.com/azure/security-center/features-paas

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Otomatik işletim sistemi yama yönetimi çözümlerini dağıtma

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3: Otomatik üçüncü taraf yazılım yama yönetimi çözümlerini dağıtma

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Arka arkaya güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Keşfedilen güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Kılavuz**: Microsoft, PostgreSQL için Azure Veritabanı'nı destekleyen temel sistemlerde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için [güvenlik denetimi: Envanter ve Varlık Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)bakın.*

### <a name="61-use-azure-asset-discovery"></a>6.1: Azure Varlık Bulma'yı kullanma

**Yönerge**: Aboneliğinizdeki tüm kaynakları (PostgreSQL örnekleri için Azure Veritabanı dahil) sorgulamak ve keşfetmek için Azure Kaynak Grafiği'ni kullanın. Kiracınızda uygun (okuma) izinlere sahip olduğundan ve aboneliklerinizdeki tüm Azure aboneliklerinin yanı sıra kaynakları sıralayabildiğinizden emin olun.

Azure Graph ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure Aboneliklerinizi görüntüleme:https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure RBAC'ı anlayın:https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="62-maintain-asset-metadata"></a>6.2: Varlık meta verilerini koruma

**Yönerge**: Etiketleri, meta verileri mantıksal olarak taksonomi olarak düzenlemek için veren PostgreSQL örnekleri ve diğer ilgili kaynaklar için Azure Veritabanı'na uygulayın.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Yetkisiz Azure kaynaklarını silme

**Kılavuz :** PostgreSQL örnekleri ve ilgili kaynaklar için Azure Veritabanı'nı düzenlemek ve izlemek için etiketlemeyi, yönetim gruplarını ve uygun olduğunda ayrı abonelikleri kullanın. Envanteri düzenli olarak uzlaştırın ve yetkisiz kaynakların abonelikten zamanında silindiğinden emin olun.

Ek Azure abonelikleri oluşturma:https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları nasıl oluşturulur:https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: Onaylanan Azure kaynaklarının ve yazılım başlıklarının envanterini koruyun

**Rehberlik**: Geçerli değildir; bu öneri, bir bütün olarak hesaplama kaynakları ve Azure için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Onaylanmamış Azure kaynakları için izleme

**Yönerge**: Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Ayrıca, abonelik içindeki kaynakları sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Bilgi işlem kaynakları içindeki onaylanmamış yazılım uygulamalarını izleme

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: Geçerli değildir; bu öneri, bir bütün olarak hesaplama kaynakları ve Azure için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="68-use-only-approved-applications"></a>6.8: Yalnızca onaylı uygulamaları kullanın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6.9: Yalnızca onaylanmış Azure hizmetlerini kullanma

**Yönerge**: Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlkesi ile belirli bir kaynak türü nasıl reddedilecek:https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="610-implement-approved-application-list"></a>6.10: Onaylı başvuru listesini uygulama

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6.11: Kullanıcıların komut dosyaları aracılığıyla Azure Kaynakları Yöneticisi ile etkileşim deşmelerini sınırlandırın

**Yönerge**: "Microsoft Azure Yönetimi" Uygulaması için "Erişimi engelle" yapılandırmayı yaparak kullanıcıların Azure Kaynak Yöneticisi ile etkileşim edebilmelerini sınırlamak için Azure Koşullu Erişimi kullanın. Bu, hassas bilgiler içeren PostgreSQL için Azure Veritabanı örnekleri gibi yüksek güvenlik ortamındaki kaynakların oluşturulmasını ve değişikliklerin önüne lenebilir.

Koşullu Erişim'i Azure Kaynak Yöneticisi'ne erişimi engellemek için yapılandırma:https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Kullanıcıların bilgi işlem kaynakları içinde komut dosyalarını yürütme yeteneğini sınırlama

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Yüksek riskli uygulamaları fiziksel veya mantıksal olarak ayırmak

**Rehberlik**: Geçerli değildir; Bu öneri, Azure Uygulama Hizmeti'nde çalışan web uygulamaları veya bilgi işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için güvenlik [denetimi: Güvenli Yapılandırma'ya](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration)bakın.*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Tüm Azure kaynakları için güvenli yapılandırmalar oluşturma

**Yönerge**: Azure İlkesi ile PostgreSQL örnekleri için Azure Veritabanınız için standart güvenlik yapılandırmaları tanımlayın ve uygulayın. PostgreSQL örnekleri için Azure Veritabanınızın ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.DBforPostgreSQL" ad alanında Azure İlkesi takma adlarını kullanın. PostgreSQL örnekleri için Azure Veritabanınızla ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz:

- PostgreSQL veritabanı sunucuları için SSL bağlantısını uygulayın etkinleştirilmelidir

- PostgreSQL veritabanı sunucuları için günlük bağlantıları etkinleştirilmelidir

Kullanılabilir Azure İlkesi Diğer Adları nasıl görüntüleyebilirsiniz:https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Güvenli işletim sistemi yapılandırmaları oluşturma

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Güvenli Azure kaynak yapılandırmalarını koruyun

**Yol gösterici**: Azure kaynaklarınız arasında güvenli ayarlar uygulamak için Azure ilkesini [reddet] ve [varsa dağıt] kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlke Efektlerini Anlama:https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Güvenli işletim sistemi yapılandırmalarını koruyun

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure kaynaklarının yapılandırmayı güvenli bir şekilde depolama

**Kılavuz**: PostgreSQL örnekleri ve ilgili kaynaklar için Azure Veritabanınız için özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos'u kullanın.

Azure DevOps'lerde kod depolama:https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Depobelgeleri:https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Özel işletim sistemi görüntülerini güvenli bir şekilde saklayın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7.7: Sistem yapılandırma yönetim araçlarını dağıtma

**Yönerge**: Sistem yapılandırmalarını uyarmak, denetlemek ve uygulamak için özel ilkeler oluşturmak için "Microsoft.DBforPostgreSQL" ad alanında Azure İlkesi takma adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve ardışık hatlar geliştirin.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: İşletim sistemleri için sistem yapılandırma yönetim araçlarını dağıtma

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Azure hizmetleri için otomatik yapılandırma izleme uygulaması

**Yönerge**: Sistem yapılandırmalarını uyarmak, denetlemek ve uygulamak için özel ilkeler oluşturmak için "Microsoft.DBforPostgreSQL" ad alanında Azure İlkesi takma adlarını kullanın. PostgreSQL örnekleri ve ilgili kaynaklar için Azure Veritabanınızın yapılandırmalarını otomatik olarak uygulamak için Azure ilkesi [denetim], [reddet]' ve [varsa dağıt] kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: İşletim sistemleri için otomatik yapılandırma izleme

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7.11: Azure sırlarını güvenli bir şekilde yönetme

**Yönlendirme**: PostgreSQL örnekleri için Azure Veritabanınıza erişmek için kullanılan Azure Sanal Makineler veya Azure Uygulama Hizmeti'nde çalışan web uygulamaları için, PostgreSQL gizli yönetimi için Azure Veritabanı'nı basitleştirmek ve güvence altına almak için Yönetilen Hizmet Kimliğini Azure Key Vault ile birlikte kullanın. Key Vault Soft Delete etkin olduğundan emin olun.

Azure Yönetilen Kimliklerle nasıl tümleştirilir:https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Anahtar Kasası nasıl oluşturulur:https://docs.microsoft.com/azure/key-vault/quick-create-portal

Key Vault kimlik doğrulaması yönetilen bir kimlikle nasıl sağlayacaksınız:https://docs.microsoft.com/azure/key-vault/managed-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Kimlikleri güvenli ve otomatik olarak yönetme

**Kılavuz**: PostgreSQL sunucusu için Azure Veritabanı, veritabanlarına erişmek için Azure Active Directory kimlik doğrulamasını (önizlemede) destekler.  PostgreSQL sunucusu için Azure Veritabanı oluştururken, yönetici bir kullanıcı için kimlik bilgileri sağlarsınız. Bu yönetici ek veritabanı kullanıcıları oluşturmak için kullanılabilir.  

PostgreSQL sunucusu için Azure Veritabanınıza erişmek için kullanılan Azure Sanal Makineler veya Azure Uygulama Hizmeti'nde çalışan web uygulamaları için, PostgreSQL sunucusu için Azure Veritabanı kimlik bilgilerini depolamak ve almak için Azure Key Vault ile birlikte Yönetilen Hizmet Kimliğini kullanın. Key Vault Soft Delete etkin olduğundan emin olun.

Azure Etkin Dizini'nde (AD) otomatik olarak yönetilen bir kimlikle Azure hizmetlerine yönetilen kimlik sağlamak için Yönetilen Kimlikler'i kullanın. Yönetilen Kimlikler, anahtarınızda herhangi bir kimlik belgesi olmadan Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlere kimlik doğrulamanızı sağlar.

Yönetilen Kimlikler nasıl yapılandırılır:https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

Azure Yönetilen Kimliklerle nasıl tümleştirilir:https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: İstenmeden kimlik bilgisi maruziyetini ortadan kaldırın

**Yönerge**: Kod içindeki kimlik bilgilerini tanımlamak için Kimlik Bilgisi Tarayıcısı'nı uygulayın. Kimlik Bilgisi Tarayıcısı, keşfedilen kimlik bilgilerinin Azure Anahtar Kasası gibi daha güvenli konumlara taşınmasını da teşvik edecektir.

Kimlik Bilgisi Tarayıcı nasıl kurulturur:https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için [Güvenlik Kontrolü: Malware Defense](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense)' e bakın.*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımlarını kullanın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda etkindir (örneğin, Azure Uygulama Hizmeti), ancak müşteri içeriğinde çalışmaz.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: İşlem yapmayan Azure kaynaklarına yüklenecek dosyaları önceden taraya

**Yönlendirme**: Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda etkindir (örneğin, PostgreSQL için Azure Veritabanı), ancak müşteri içeriğinde çalışmaz.

Uygulama Hizmeti, Veri Gölü Depolama, Blob Depolama, PostgreSQL için Azure Veritabanı gibi bilgi işlem dışı Azure kaynaklarına yüklenen tüm içerikleri önceden tarayın. Microsoft bu gibi durumlarda verilerinize erişemez.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Kötü amaçlı yazılımdan koruma yazılımının ve imzalarının güncelleştirilmesini sağlayın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda etkindir (örneğin, PostgreSQL için Azure Veritabanı), ancak müşteri içeriğinde çalışmaz.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Düzenli otomatik yedeklemeler sağlayın

**Rehberlik**: PostgreSQL için Azure Veritabanı, veri dosyalarının ve işlem günlüğünün yedeklerini alır. Desteklenen maksimum depolama boyutuna bağlı olarak, tam ve diferansiyel yedekleme (4 TB max depolama sunucusu) veya anlık görüntü yedekleri (en fazla 16 TB maksimum depolama sunucusu) alabiliriz. Bu yedeklemeler, yapılandırılmış yedekleme bekletme süreniz içinde sunucunuzu zamanında geri yüklemenize olanak sağlar. Varsayılan yedekleme bekletme süresi yedi gündür. İsteğe bağlı olarak 35 güne kadar yapılandırabilirsiniz. Tüm yedeklemeler AES 256 bit şifreleme kullanılarak şifrelenir.

PostgreSQL için Azure Veritabanı'nda bir sunucu nasıl yedekilir:https://docs.microsoft.com/azure/postgresql/howto-restore-server-portal

PostgreSQL ilk yapılandırması için Azure Veritabanını anlayın:https://docs.microsoft.com/azure/postgresql/tutorial-design-database-using-azure-portal

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Komple sistem yedeklemelerini gerçekleştirin ve müşteri yönetilen anahtarları yedekleme

**Yönlendirme**: PostgreSQL için Azure Veritabanı, kullanıcının tercihine göre sunucu yedeklemelerini otomatik olarak oluşturur ve bunları yerel olarak yedekli veya coğrafi olarak yedekli depolama alanında saklar. Sunucunuzu belirli bir noktaya geri yüklemek için yedeklemeler kullanılabilir. Yedekleme ve geri yükleme, verilerinizi yanlışlıkla oluşan bozulmaveya silmeye karşı koruduklarından, herhangi bir iş sürekliliği stratejisinin önemli bir parçasıdır.

PostgreSQL örnekleri için Azure Veritabanınızın kimlik bilgilerini depolamak için Azure Key Vault kullanıyorsanız, anahtarlarınızın düzenli otomatik yedeklemelerini sağlayın.

PostgreSQL için Azure Veritabanı'nda bir sunucu nasıl yedekilir:https://docs.microsoft.com/azure/postgresql/howto-restore-server-portal

Anahtar Vault Tuşları nasıl yedeklenebilir:https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Müşteri yönetilen anahtarlar dahil tüm yedeklemeleri doğrulayın

**Yönerge**: PostgreSQL için Azure Veritabanı'nda, geri yükleme yapmak orijinal sunucunun yedeklerinden yeni bir sunucu oluşturur. Kullanılabilir geri yükleme nin iki türü vardır: Zaman içinde geri yükleme ve Coğrafi geri yükleme. Zamanında geri yükleme, yedekleme artıklığı seçeneğiyle kullanılabilir ve özgün sunucunuzla aynı bölgede yeni bir sunucu oluşturur. Coğrafi geri yükleme, yalnızca sunucunuzu coğrafi yedekli depolama alanı için yapılandırdığınızda ve sunucunuzu farklı bir bölgeye geri yüklemenize olanak tanırsa kullanılabilir.

Tahmini kurtarma süresi veritabanı boyutları, işlem günlüğü boyutu, ağ bant genişliği ve aynı anda aynı bölgede kurtarılan toplam veritabanı sayısı dahil olmak üzere çeşitli etkenlere bağlıdır. Kurtarma süresi genellikle 12 saatten azdır.

PostgreSQL örnekleri için Azure Veritabanınızın geri yüklemesini düzenli aralıklarla test edin.

PostgreSQL için Azure Veritabanı'nda bir sunucu nasıl yedekilir:https://docs.microsoft.com/azure/postgresql/howto-restore-server-portal

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Yedeklerin ve müşteri yönetilen anahtarlarının korunmasını sağlayın

**Rehberlik**: PostgreSQL için Azure Veritabanı tam, diferansiyel ve işlem günlüğü yedeklemelerini alır. Bu yedeklemeler, yapılandırılmış yedekleme bekletme süreniz içinde sunucunuzu zamanında geri yüklemenize olanak sağlar. Varsayılan yedekleme bekletme süresi yedi gündür. İsteğe bağlı olarak 35 güne kadar yapılandırabilirsiniz. Tüm yedeklemeler AES 256 bit şifreleme kullanılarak şifrelenir.

PostgreSQL için Azure Veritabanı'nda yedeklemeyi ve geri yüklemeyi anlayın:https://docs.microsoft.com/azure/postgresql/concepts-backup

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için [Güvenlik Denetimi: Olay Yanıtı'na](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response)bakın.*

### <a name="101-create-an-incident-response-guide"></a>10.1: Olay yanıt kılavuzu oluşturma

**Rehberlik**: Kuruluşunuz için bir olay yanıt kılavuzu oluşturun. Personelin tüm rollerini ve olay işleme/yönetiminin algılamadan olay sonrası incelemeye kadar olan aşamalarını tanımlayan yazılı olay yanıt planları olduğundan emin olun.

Azure Güvenlik Merkezi'nde İş Akışı Otomasyonları nasıl yapılandırılabilen:https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

Kendi güvenlik olayı yanıt sürecinizi oluşturma kılavuzu:https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Microsoft Güvenlik Yanıt Merkezi'nin Bir Olayın Anatomisi:https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Müşteri ayrıca, kendi olay yanıt planının oluşturulmasına yardımcı olmak için NIST'nin Bilgisayar Güvenliği Olay İşlemleri Kılavuzu'ndan da yararlanabilir:https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Olay puanlama ve öncelik lendirme prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, öncelikle hangi uyarıların araştırılması gerektiğine öncelik vermenize yardımcı olmak için her uyarıya önem verir. Önem derecesi, Güvenlik Merkezi'nin bulguda ne kadar emin olduğuna veya uyarıyı vermek için kullanılan analize ve uyarıya yol açan etkinliğin arkasında kötü amaçlı niyet olduğuna dair güven düzeyine dayanır.

Ayrıca, abonelikleri açıkça işaretleyin (eski için. azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10.3: Test güvenlik yanıt yordamları

**Rehberlik**: Sistemlerinizin olay yanıt yeteneklerini düzenli olarak test etmek için alıştırmalar yapın. Zayıf noktaları ve boşlukları belirleyin ve planı gerektiği gibi gözden geçirin.

NIST'in yayınına bakın: BT Planları ve Yetenekleri için Test, Eğitim ve Egzersiz Programları Kılavuzu:https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Güvenlik olayı iletişim bilgilerini sağlayın ve güvenlik olayları için uyarı bildirimlerini yapılandırma

**Kılavuz :** Microsoft Güvenlik Yanıt Merkezi (MSRC), müşterinin verilerine yasadışı veya yetkisiz bir taraf tarafından erişildiğini tespit ederse, güvenlik olayı iletişim bilgileri Microsoft tarafından sizinle iletişim kurmak için kullanılır.  Sorunların çözülmesini sağlamak için olaydan sonraki olayları gözden geçirin.

Azure Güvenlik Merkezi Güvenlik Kişisi nasıl ayarlanır:https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Olay yanıt sisteminize güvenlik uyarıları dahil edin

**Kılavuz**: Sürekli Dışa Aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışa aktarın. Sürekli Dışa Aktarma, uyarı ve önerileri el ile veya sürekli olarak dışa aktarmanıza olanak tanır. Sentinel uyarılarını yayınlamak için Azure Güvenlik Merkezi veri konektörünü kullanabilirsiniz.

Sürekli dışa aktarma nasıl yapılandırılabilen:https://docs.microsoft.com/azure/security-center/continuous-export

Uyarıları Azure Sentinel'e nasıl aktarın:https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Güvenlik uyarılarına yanıtı otomatikleştirin

**Yönerge**: Güvenlik uyarıları ve önerileri nde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek için Azure Güvenlik Merkezi'ndeki İş Akışı Otomasyonu özelliğini kullanın.

İş Akışı Otomasyonu ve Mantık Uygulamaları nasıl yapılandırılabilen:https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için [Güvenlik Kontrolü: Penetrasyon Testleri ve Kırmızı Takım Egzersizleri'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises)bakın.*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11.1: Azure kaynaklarınızı düzenli olarak delme testini gerçekleştirin ve tüm kritik güvenlik bulgularının 60 gün içinde düzeltilmesini sağlayın

**Kılavuz**: Penetrasyon Testlerinizin Microsoft ilkelerini ihlal etmediğini sağlamak için Microsoft Etkileşim Kuralları'na uyun:https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

Microsoft'un Stratejisi ve Red Teaming ve Microsoft tarafından yönetilen bulut altyapısı, hizmetler ve uygulamalara karşı canlı site penetrasyon testi nin uygulanması hakkında daha fazla bilgiyi burada bulabilirsiniz:https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Güvenlik Kıyaslama'ya](https://docs.microsoft.com/azure/security/benchmarks/overview) bakın
- [Azure Güvenlik Taban Çizgileri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
