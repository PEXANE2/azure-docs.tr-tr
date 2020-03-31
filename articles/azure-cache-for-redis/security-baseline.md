---
title: Redis için Azure Önbelleği için Azure Güvenlik Taban Çizgisi
description: Redis için Azure Önbelleği için Azure Güvenlik Taban Çizgisi
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 364b87e4d64b8cc65fdf293032f4340bddec957c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79479143"
---
# <a name="azure-security-baseline-for-azure-cache-for-redis"></a>Redis için Azure Önbelleği için Azure Güvenlik Taban Çizgisi

Redis için Azure Önbelleği için Azure Güvenlik Taban Çizgisi, dağıtımınızın güvenlik duruşunu geliştirmenize yardımcı olacak öneriler içerir.

Bu hizmetin temeli, en iyi uygulamalar kılavuzumuzla Azure'da bulut çözümlerinizi nasıl güvenebileceğinize ilişkin öneriler sunan [Azure Güvenlik Kıyaslama sürümü 1.0'dan](https://docs.microsoft.com/azure/security/benchmarks/overview)alınmıştır.

Daha fazla bilgi için [Azure Güvenlik Taban Çizgilerini genel olarak](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)görün.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için [Güvenlik Denetimi: Ağ Güvenliği'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)bakın.*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Sanal Ağınızdaki Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'nı kullanarak kaynakları koruyun

**Kılavuz**: Azure Önbelleğinizi redis örneğini sanal ağ (VNet) içinde dağıtın. VNet, buluttaki özel bir ağdır. Redis örneği için bir Azure Önbelleği bir VNet ile yapılandırıldığında, genel olarak ele alınamaz ve yalnızca VNet içindeki sanal makinelerden ve uygulamalardan erişilebilir.

Ayrıca, IP adresi aralığını başlat ve sonla ile güvenlik duvarı kuralları da belirtebilirsiniz. Güvenlik duvarı kuralları yapılandırıldığında, yalnızca belirtilen IP adresi aralıklarından istemci bağlantıları önbelleğe bağlanabilir.

Redis için Premium Azure Önbelleği için Sanal Ağ Desteği nasıl yapılandırılır:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-vnet

Redis güvenlik duvarı kuralları için Azure Önbelleği nasıl yapılandırılır:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-configure#firewall

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Vnet, Subnet ve NIC'lerin yapılandırmasını ve trafiğini izleyin ve kaydedin

**Kılavuz**: Sanal Makineler Redis örneğiniz için Azure Önbelleğiile aynı sanal ağda dağıtıldığında, veri sızma riskini azaltmak için ağ güvenlik gruplarını (NSG) kullanabilirsiniz. NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir Azure Depolama Hesabına gönderin. Ayrıca NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzdaki trafik akışıhakkında öngörüler sağlamak için Trafik Analitiği'ni kullanabilirsiniz. Traffic Analytics'in bazı avantajları, ağ etkinliğini görselleştirme ve etkin noktaları belirleme, güvenlik tehditlerini belirleme, trafik akışı desenlerini anlama ve ağ yanlış yapılandırmalarını saptabilme yeteneğidir.

NSG Akış Günlükleri Nasıl Etkinleştirilir:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Trafik Analizi ni etkinleştirme ve kullanma:

https://docs.microsoft.com/azure/network-watcher/traffic-analytics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="13-protect-critical-web-applications"></a>1.3: Kritik web uygulamalarını koruyun

**Rehberlik**: Geçerli değildir; Bu öneri, Azure Uygulama Hizmeti'nde çalışan web uygulamaları veya bilgi işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Bilinen kötü amaçlı IP adresleriyle iletişimi reddetme

**Yönlendirme**: Azure Sanal Ağ (VNet) dağıtımı, erişimi daha da kısıtlamak için Azure Önbelleğiniz redis için gelişmiş güvenlik ve yalıtımın yanı sıra alt ağlar, erişim denetim ilkeleri ve diğer özellikler sağlar. Bir VNet'te dağıtıldığında, Redis için Azure Önbelleği herkese açık değildir ve yalnızca VNet içindeki sanal makinelerden ve uygulamalardan erişilebilir.

Dağıtılmış hizmet reddi (DDoS) saldırılarına karşı korunmak için Redis için Azure Önbelleği'nizle ilişkili VNet'lerde DDoS Koruma Standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimi reddetmek için Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı kullanın.

Redis için Premium Azure Önbelleği için Sanal Ağ Desteği nasıl yapılandırılır:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-vnet

Azure portalını kullanarak Azure DDoS Koruma Standardını yönetin:

https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1.5: Ağ paketlerini ve akış günlüklerini kaydetme

**Kılavuz**: Redis örneğiniz için Azure Önbelleği'nizle aynı sanal ağda sanal makineler dağıtıldığında, veri sızma riskini azaltmak için ağ güvenlik gruplarını (NSG) kullanabilirsiniz. NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir Azure Depolama Hesabına gönderin. Ayrıca NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzdaki trafik akışıhakkında öngörüler sağlamak için Trafik Analitiği'ni kullanabilirsiniz. Traffic Analytics'in bazı avantajları, ağ etkinliğini görselleştirme ve etkin noktaları belirleme, güvenlik tehditlerini belirleme, trafik akışı desenlerini anlama ve ağ yanlış yapılandırmalarını saptabilme yeteneğidir.

NSG Akış Günlükleri Nasıl Etkinleştirilir:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Trafik Analizi ni etkinleştirme ve kullanma:

https://docs.microsoft.com/azure/network-watcher/traffic-analytics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Ağ tabanlı izinsiz giriş algılama/saldırı önleme sistemlerini (IDS/IPS) dağıtma

**Kılavuz**: Azure Uygulama Hizmeti'nde çalışan web uygulamalarınız veya bilgi işlem örneklerinizde Redis için Azure Önbelleği kullanırken, azure sanal ağ (VNet) içindeki tüm kaynakları dağıtın ve Web Uygulama Ağ Geçidi'nde bir Azure Web Uygulama Güvenlik Duvarı (WAF) ile güvenli hale alın. WAF'ı "Önleme Modu"nda çalışacak şekilde yapılandırın. Önleme Modu, kuralların algıladığının izinsiz girişlerini ve saldırılarını engeller. Saldırgan bir "403 yetkisiz erişim" özel durumu alır ve bağlantı kapatılır. Önleme modu waf günlüklerinde bu tür saldırıları kaydeder.

Alternatif olarak, Azure Marketi'nden, yük denetimi ve/veya anormallik algılama özellikleriyle IDS/IPS işlevselliğini destekleyen bir teklif seçebilirsiniz.

Azure WAF özelliklerini anlayın:

https://docs.microsoft.com/azure/web-application-firewall/ag/ag-overview

Azure WAF nasıl dağıtılır:

https://docs.microsoft.com/azure/web-application-firewall/ag/create-waf-policy-ag

Azure Marketi:

https://azuremarketplace.microsoft.com/marketplace/?term=Firewall

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Web uygulamalarına trafik yönetme

**Rehberlik**: Geçerli değildir; Bu öneri, Azure Uygulama Hizmeti'nde çalışan web uygulamaları veya bilgi işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Ağ güvenliği kurallarının karmaşıklığını ve yönetim ek lerini en aza indirmek

**Kılavuz**: Ağ güvenlik grupları (NSG) veya Azure Güvenlik Duvarı'ndaki ağ erişim denetimlerini tanımlamak için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında servis etiketi adını (örn. ApiManagement) belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz. Microsoft, hizmet etiketinin kapsadığı adres öneklerini yönetir ve adresler değiştikçe servis etiketini otomatik olarak güncelleştirir.

Karmaşık güvenlik yapılandırmasını basitleştirmeye yardımcı olmak için uygulama güvenlik gruplarını (ASG) de kullanabilirsiniz. ASG'ler, sanal makineleri gruplandırmanıza ve bu gruplara göre ağ güvenliği ilkelerini tanımlamanıza olanak sağlayarak ağ güvenliğini bir uygulamayapısının doğal uzantısı olarak yapılandırmanıza olanak tanır.

Sanal ağ hizmeti etiketleri:

https://docs.microsoft.com/azure/virtual-network/service-tags-overview

Uygulama Güvenlik Grupları:

https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Ağ aygıtları için standart güvenlik yapılandırmalarını koruyun

**Yönerge**: Azure İlkesi ile Redis örnekleri için Azure Önbelleğinizle ilgili ağ kaynakları için standart güvenlik yapılandırmaları tanımlayın ve uygulayın. Redis örnekleri için Azure Önbelleği'nizin ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.Önbellek" ve "Microsoft.Network" ad alanlarında Azure İlkesi takma adlarını kullanın. Ayrıca aşağıdakiler gibi yerleşik ilke tanımlarından da yararlanabilirsiniz:

Yalnızca Redis Önbelleğinize güvenli bağlantılar etkinleştirilmelidir

DDoS Koruma Standardı etkinleştirilmelidir

Azure Kaynak Yöneticisi (ARM) şablonları, rol tabanlı erişim denetimi (RBAC) ve ilkeler gibi önemli ortam yapılarını tek bir plan tanımında paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure Planları'nı da kullanabilirsiniz. Planı yeni aboneliklere ve ortamlara kolayca uygulayın ve sürüm leme yoluyla denetim ve yönetime ince ayar uygulayın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Blueprint nasıl oluşturulur:

https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="110-document-traffic-configuration-rules"></a>1.10: Belge trafiği yapılandırma kuralları

**Yönerge**: Redis dağıtımı için Azure Önbelleği'nizle ilişkili ağ kaynakları için etiketleri kullanarak bunları mantıksal olarak taksonomi olarak düzenleyin.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Ağ kaynağı yapılandırmalarını izlemek ve değişiklikleri algılamak için otomatik araçlar kullanın

**Yönerge**: Ağ kaynak yapılandırmalarını izlemek ve Redis örnekleri için Azure Önbelleği'nizle ilgili ağ kaynaklarındaki değişiklikleri algılamak için Azure Etkinliği günlüğünü kullanın. Azure Monitor'da kritik ağ kaynaklarında değişiklikler olduğunda tetikleyecek uyarılar oluşturun.

Azure Etkinlik Günlüğü etkinliklerini görüntüleme ve alma:

https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

Azure Monitor'da uyarı oluşturma:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için [Güvenlik Denetimi: Günlüğe kaydetme ve izleme.](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Onaylanmış zaman senkronizasyon kaynaklarını kullanma

**Kılavuz :** Microsoft, günlüklerde zaman damgaları için Azure önbelleği redis için Azure kaynakları için kullanılan zaman kaynağını korur.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Yönlendirme**: Azure Etkinlik Günlüğü tanı ayarlarını etkinleştirin ve günlükleri arşiv için Bir Günlük Analizi çalışma alanına, Azure etkinlik merkezine veya Azure depolama hesabına gönderin. Etkinlik günlükleri, denetim düzlemi düzeyindeki Redis örnekleri için Azure Önbelleğinizde gerçekleştirilen işlemlerhakkında bilgi sağlar. Azure Etkinlik Günlüğü verilerini kullanarak, Redis örnekleri için Azure Önbelleğiniz için denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" olarak belirleyebilirsiniz.

Azure Etkinlik Günlüğü için Tanı lama Ayarları nasıl etkinleştirilir:https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure kaynakları için denetim günlüğe kaydetmeyi etkinleştirme

**Yönlendirme**: Azure Etkinlik Günlüğü tanı ayarlarını etkinleştirin ve günlükleri arşiv için Bir Günlük Analizi çalışma alanına, Azure etkinlik merkezine veya Azure depolama hesabına gönderin. Etkinlik günlükleri, denetim düzlemi düzeyindeki Redis örnekleri için Azure Önbelleğinizde gerçekleştirilen işlemlerhakkında bilgi sağlar. Azure Etkinlik Günlüğü verilerini kullanarak, Redis örnekleri için Azure Önbelleğiniz için denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" olarak belirleyebilirsiniz.

Tanılama Ayarları etkinleştirilerek ölçümler kullanılabilir olsa da, veri düzleminde denetim günlüğü henüz Redis için Azure Önbelleği için kullanılamaz.

Azure Etkinlik Günlüğü için Tanı lama Ayarları nasıl etkinleştirilir:https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: İşletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2.5: Güvenlik günlüğü depolama tutma yapılandırma

**Kılavuz :** Azure Monitor'da, kuruluşunuzun uyumluluk yönetmeliklerine göre Redis örnekleri için Azure Önbelleğiile ilişkili Günlük Analizi çalışma alanları için günlük bekletme süresini ayarlayın.

Redis için Azure Önbelleği için veri düzleminde denetim günlüğünün henüz kullanılamadığını unutmayın.

Günlük bekletme parametreleri nasıl ayarlanır:

https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2.6: Günlükleri izleme ve inceleme

**Yönerge**: Azure Etkinlik Günlüğü tanı ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Redis için Azure Önbelleği için toplanmış olabilecek Etkinlik Günlüğü Verilerini temel almak üzere terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve diğer birçok öngörüyü sağlamak için Log Analytics'te sorgular gerçekleştirin.

Redis için Azure Önbelleği için veri düzleminde denetim günlüğünün henüz kullanılamadığını unutmayın.

Azure Etkinlik Günlüğü için Tanı lama Ayarları nasıl etkinleştirilir:https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

Azure Monitor'da Log Analytics çalışma alanında Azure etkinlik günlüklerini toplama ve analiz etme:https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-collect

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Anormal aktivite için uyarıları etkinleştirme

**Yönerge**: Redis örnekleri için Azure Önbelleği'nizle ilgili ölçümlere ve etkinlik günlüklerine göre uyarı alacak şekilde yapılandırabilirsiniz. Azure Monitor, e-posta bildirimi göndermek, web hook aramak veya bir Azure Mantık Uygulaması çağırmak için bir uyarı yapılandırmanıza olanak tanır.

Tanılama Ayarları etkinleştirilerek ölçümler kullanılabilir olsa da, veri düzleminde denetim günlüğü henüz Redis için Azure Önbelleği için kullanılamaz.

Redis için Azure Önbelleği için uyarılar nasıl yapılandırılır:https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-monitor#alerts

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="28-centralize-anti-malware-logging"></a>2.8: Kötü amaçlı yazılımdan koruma günlüğe kaydetmeyi merkezileştirin

**Rehberlik**: Geçerli değildir; Redis için Azure Önbelleği, kötü amaçlı yazılımdan koruma yla ilgili günlükleri işlemez veya üretmez.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="29-enable-dns-query-logging"></a>2.9: DNS sorgu günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: Geçerli değildir; Redis için Azure Önbelleği, DNS ile ilgili günlükleri işlemez veya üretmez.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2.10: Komut satırı denetim günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için [güvenlik denetimi: kimlik ve erişim denetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)bakın.*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: İdari hesapların envanterini korumak

**Yönerge**: Azure Active Directory (AD), açıkça atanması gereken ve sorgulanabilen yerleşik rollere sahiptir. Yönetim gruplarına üye hesapları bulmak için geçici sorgular gerçekleştirmek için Azure AD PowerShell modüllerini kullanın.

PowerShell ile Azure AD'de dizin rolü nasıl elde elabilirsiniz:https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0

PowerShell ile Azure AD'de dizin rolünün üyelerini nasıl edinire bilgili olabilirsiniz:https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Varsayılan parolaları varsa değiştirme

**Yönlendirme**: Redis için Azure Önbelleği'ne denetim düzlemi erişimi Azure Etkin Dizin (AD) aracılığıyla denetlenir. Azure AD varsayılan parola kavramına sahip değildir. 

Redis için Azure Önbelleği'ne veri düzlemi erişimi erişim anahtarları aracılığıyla denetlenir. Bu anahtarlar önbelleğinize bağlanan istemciler tarafından kullanılır ve herhangi bir zamanda yeniden oluşturulabilir.

Uygulamanızda varsayılan parolalar oluşturmanız önerilmez. Bunun yerine, parolalarınızı Azure Anahtar Kasası'nda saklayabilir ve bunları almak için Azure Active Directory'yi kullanabilirsiniz.

Redis erişim anahtarları için Azure Önbelleği nasıl yeniden oluşturur:https://docs.microsoft.com/azure/azure-cache-for-redis/cache-configure#settings

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Özel yönetim hesaplarını kullanma

**Rehberlik**: Özel yönetim hesaplarının kullanımı yla ilgili standart işletim prosedürleri oluşturun. Yönetim hesabı sayısını izlemek için Azure Güvenlik Merkezi Kimliği ve Erişim Yönetimi'ni kullanın.

Ayrıca, özel yönetim hesaplarını izlemenize yardımcı olmak için Azure Güvenlik Merkezi'nden veya yerleşik Azure İlkeleri'nden öneriler kullanabilirsiniz:

- Aboneliğinize birden fazla sahip atanmalıdır

- Sahip izinleri olan amortismana alınmış hesaplar aboneliğinizden kaldırılmalıdır

- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

Kimlik ve erişimi izlemek için Azure Güvenlik Merkezi nasıl kullanılır (Önizleme):https://docs.microsoft.com/azure/security-center/security-center-identity-access

Azure İlkesi nasıl kullanılır:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Azure Active Directory ile tek oturum açma (SSO) kullanma

**Yönerge**: Redis için Azure Önbelleği, kullanıcıların kimliğini doğrulamak için erişim anahtarlarını kullanır ve veri düzlemi düzeyinde tek oturum açma (SSO) desteklemez. Redis için Azure Önbellek denetim düzlemine erişim REST API üzerinden kullanılabilir ve SSO'yu destekler. Kimlik doğrulaması yapmak için, istekleriniz için Yetkilendirme üstbilgisini Azure Active Directory'den aldığınız bir JSON Web Belirteci'ne ayarlayın.

Redis REST API için Azure Önbelleğini Anlayın:https://docs.microsoft.com/rest/api/redis/

Azure AD ile SSO'yi anlayın:https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Tüm Azure Active Directory tabanlı erişim için çok faktörlü kimlik doğrulamayı kullanın

**Kılavuz**: Azure Etkin Dizin (AD) Çok Faktörlü Kimlik Doğrulamasını (MFA) etkinleştirin ve Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerini izleyin.

Azure'da MFA nasıl etkinleştirilir:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Azure Güvenlik Merkezi'nde kimlik ve erişim nasıl izlenir:https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Tüm idari görevler için özel makineleri (Ayrıcalıklı Erişim İş İstasyonları) kullanın

**Kılavuz :** Azure kaynaklarına giriş yapmak ve yapılandırmak üzere yapılandırılan Çok Faktörlü Kimlik Doğrulama (MFA) ile ayrıcalıklı erişim iş istasyonları (PAW) kullanın.

Ayrıcalıklı Erişim İş İstasyonları hakkında bilgi edinin:

https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure'da MFA nasıl etkinleştirilir:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: İdari hesaplardan şüpheli etkinliklere ilişkin günlük ve uyarı

**Kılavuz :** Çevrede şüpheli veya güvensiz etkinlik oluştuğunda günlük ve uyarı oluşturma için Azure Active Directory (AD) Ayrıcalıklı Kimlik Yönetimi 'ni (PIM) kullanın.

Ayrıca, riskli kullanıcı davranışıyla ilgili uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın.

Ayrıcalıklı Kimlik Yönetimi (PIM) nasıl dağıtılır:https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan

Azure AD risk algılamalarını anlayın:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Yönerge**: Yalnızca IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplandırmalarından erişime izin vermek için Azure Active Directory (AD) Koşullu Erişim'de adlandırılmış konumları yapılandırın.

Azure'da Adlandırılmış Konumlar nasıl yapılandırılabilen:https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="39-use-azure-active-directory"></a>3.9: Azure Etkin Dizini Kullanma

**Kılavuz :** Merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (AD) kullanın. Azure AD, veri için güçlü şifreleme kullanarak verileri istirahatte ve aktarım sırasında korur. Azure AD ayrıca kullanıcı kimlik bilgilerini tuzlar, iş lerle ve güvenli bir şekilde depolar.

Azure AD kimlik doğrulaması Redis'in veri düzlemi için Azure Önbelleği'ne doğrudan erişim için kullanılamaz, ancak Azure AD kimlik bilgileri, Redis erişim anahtarları için Azure Önbelleğini denetlemek için denetim düzlemi düzeyinde (örneğin Azure portalı) yönetim için kullanılabilir.


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Kullanıcı erişimini düzenli olarak gözden geçirin ve uzlaştırın

**Rehberlik**: Azure Active Directory (AD), eski hesapları keşfetmenize yardımcı olacak günlükler sağlar. Ayrıca, grup üyeliklerini, kurumsal uygulamalara erişimi ve rol atamalarını verimli bir şekilde yönetmek için Azure Kimlik Erişim İncelemeleri'ni kullanın. Kullanıcı erişimi, yalnızca doğru Kullanıcıların sürekli erişime sahip olduğundan emin olmak için düzenli olarak gözden geçirilebilir. 

Azure AD raporlamalarını anlayın:https://docs.microsoft.com/azure/active-directory/reports-monitoring/

Azure Kimlik Erişim İncelemeleri nasıl kullanılır:https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Devre dışı bırakılmış hesaplara erişim denemelerini izleme

**Kılavuz**: Azure Sentinel veya üçüncü taraf SIEM ile tümleştirmenize olanak tanıyan Azure Active Directory (AD) oturum açma etkinliği, denetim ve risk olay günlüğü kaynaklarına erişebilirsiniz.

Azure AD kullanıcı hesapları için tanılama ayarları oluşturarak ve denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. Log Analytics içinde istediğiniz günlük uyarılarını yapılandırabilirsiniz.

Azure Etkinlik Günlükleri Azure Monitör'e nasıl entegre eleştirilir:https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

Azure Sentinel'de nasıl iş yüzenebilen:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Hesap giriş davranış sapması konusunda uyarı

**Kılavuz :** Denetim düzleminde hesap giriş davranışı sapması için, kullanıcı kimlikleri ile ilgili algılanan şüpheli eylemlere otomatik yanıtları yapılandırmak için Azure Active Directory (AD) Kimlik Koruması ve risk algılama özelliklerini kullanın. Ayrıca, daha fazla araştırma için Azure Sentinel'e veri sindirebilirsiniz.

Azure AD riskli oturum açmaları nasıl görüntülenebilen:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Kimlik Koruması risk ilkelerinin nasıl yapılandırılabilen ve etkinleştirilir:https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

Azure Sentinel'e nasıl binilir:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Destek senaryoları sırasında Microsoft'a ilgili müşteri verilerine erişim sağlama

**Rehberlik**: Henüz mevcut değil; Müşteri Kilit Kutusu, Redis için Azure Önbelleği için henüz desteklenmedi.

Müşteri Kilit Kutusu destekli hizmetler listesi:

https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Geçerli değil

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için [güvenlik denetimi: veri koruma.](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Hassas Bilgilerin envanterini koruyun

**Yönerge**: Hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olmak için etiketleri kullanın.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Hassas bilgileri depolama veya işleme sistemlerini yalıtma

**Rehberlik**: Geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Redis örnekleri için Azure Önbelleği sanal ağ/alt ağ tarafından ayrılmalı ve uygun şekilde etiketlenmelidir. İsteğe bağlı olarak, yalnızca belirtilen IP adresi aralıklarından gelen istemci bağlantılarının önbelleğe bağlanabilmesi için kuralları tanımlamak için Redis güvenlik duvarı için Azure Önbelleğini kullanın.

Ek Azure abonelikleri oluşturma:

https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları nasıl oluşturulur:

https://docs.microsoft.com/azure/governance/management-groups/create

Redis için Azure Önbelleği Vnet'e nasıl dağıtılır:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-vnet

Redis güvenlik duvarı kuralları için Azure Önbelleği nasıl yapılandırılır:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-configure#firewall

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Hassas bilgilerin yetkisiz aktarımının izlenmesi ve engellenmesi

**Rehberlik**: Henüz mevcut değil; Redis için Azure Önbelleği için veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz kullanılamıyor.

Microsoft, Redis için Azure Önbelleği'nin altında yatan altyapıyı yönetir ve müşteri verilerinin kaybolmasını veya açığa dökülmesini önlemek için sıkı denetimler uygulamıştır.

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Tüm hassas bilgileri taşıma sırasında şifreleme

**Kılavuz**: Redis için Azure Önbelleği varsayılan olarak TLS şifreli iletişim gerektirir. TLS sürümleri 1.0, 1.1 ve 1.2 şu anda desteklenmiştir. Ancak, TLS 1.0 ve 1.1 endüstri çapında amortismana giden bir yoldadır, bu nedenle mümkünse TLS 1.2'yi kullanın. İstemci kitaplığınız veya aracınız TLS'yi desteklemiyorsa, şifrelenmemiş bağlantıları etkinleştirme Azure portalı veya yönetim API'leri aracılığıyla yapılabilir. Şifreli bağlantıların mümkün olmadığı durumlarda, önbelleğinizi ve istemci uygulamanızı sanal bir ağa yerleştirmeniz önerilir.

Redis için Azure Önbelleği için aktarımda şifrelemeyi anlayın:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-best-practices

Vnet önbellek senaryolarında kullanılan gerekli bağlantı noktalarını anlayın:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-vnet#outbound-port-requirements

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Hassas verileri tanımlamak için etkin bir bulma aracı kullanın

**Kılavuz :** Redis için Azure Önbelleği için veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz kullanılamıyor. Bu gibi hassas bilgiler içeren örnekleri etiketle ve uyumluluk amacıyla gerekirse üçüncü taraf çözüm uygulayın.

Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Kaynaklara erişimi denetlemek için Azure RBAC'ı kullanın

**Rehberlik**: Redis için Azure Önbelleği denetim düzlemine (örneğin Azure portalı) erişimi denetlemek için Azure Active Directory (AAD) rol tabanlı erişim denetimini (RBAC) kullanın. 

Azure'da RBAC nasıl yapılandırılmaz:

https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Erişim denetimini uygulamak için ana bilgisayar tabanlı veri kaybı önlemeyi kullanın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

Microsoft, Redis için Azure Önbelleği'nin altında yatan altyapıyı yönetir ve müşteri verilerinin kaybolmasını veya açığa dökülmesini önlemek için sıkı denetimler uygulamıştır.

Azure'da müşteri verilerinin korunmasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Hassas bilgileri istirahatte şifreleme

**Yönlendirme**: Redis için Azure Önbelleği müşteri verilerini bellekte depolar ve Microsoft tarafından uygulanan birçok denetim tarafından güçlü bir şekilde korunurken, bellek varsayılan olarak şifrelenmez. Kuruluşunuz tarafından isteniyorsa, Redis için Azure Önbelleğinde depolamadan önce içeriği şifreleyin.

Redis için Azure Önbelleği "Redis Data Persistence" özelliğini kullanıyorsanız, veriler sahip olduğunuz ve yönettiğiniz bir Azure Depolama hesabına gönderilir. Kalıcılığı önbellek oluşturma sırasında "Redis için Yeni Azure Önbelleği" bıçağından ve varolan premium önbellekler için Kaynak menüsünden yapılandırabilirsiniz.

Azure Depolama'daki veriler, mevcut en güçlü blok şifrelerinden biri olan 256 bit AES şifreleme kullanılarak şifrelenir ve şifresi saydam olarak çözülür ve FIPS 140-2 uyumludur. Azure Depolama şifrelemesi devre dışı tutulamaz. Depolama hesabınızın şifrelemesi için Microsoft tarafından yönetilen anahtarlara güvenebilir veya şifrelemeyi kendi anahtarlarınızla yönetebilirsiniz.

Redis için Azure Önbelleğinde kalıcılığı yapılandırma:https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-persistence

Azure Depolama hesapları için şifrelemeyi anlayın:https://docs.microsoft.com/azure/storage/common/storage-service-encryption

Azure müşteri veri korumayı anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Kritik Azure kaynaklarında yapılan değişiklikleri kaydedin ve uyarı

**Yönerge**: Redis için Azure Önbelleği'nin üretim örneklerinde ve diğer kritik veya ilgili kaynaklarda değişiklikler olduğunda uyarılar oluşturmak için Azure Etkinliği günlüğüyle Azure Monitörünü kullanın.

Azure Etkinlik Günlüğü etkinlikleri için uyarılar oluşturma:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için [Güvenlik Denetimi: Güvenlik Açığı Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)bakın.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Otomatik güvenlik açığı tarama araçlarını çalıştırın

**Yönerge**: Azure Güvenlik Merkezi'nin Redis örnekleri ve ilgili kaynaklar için Azure Önbelleğinizi güvence altına alma konusundaki önerileri izleyin.

Microsoft, Redis için Azure Önbelleğini destekleyen temel sistemlerde güvenlik açığı yönetimi gerçekleştirir.

Azure Güvenlik Merkezi önerilerini anlayın:https://docs.microsoft.com/azure/security-center/recommendations-reference

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Paylaşılan

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

**Kılavuz**: Microsoft, Redis için Azure Önbelleğini destekleyen temel sistemlerde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için [güvenlik denetimi: Envanter ve Varlık Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)bakın.*

### <a name="61-use-azure-asset-discovery"></a>6.1: Azure Varlık Bulma'yı kullanma

**Yönerge**: Aboneliğinizdeki tüm kaynakları (bilgi işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın.  Kiracınızda uygun (okundu) izinler sağlayın ve aboneliklerinizdeki tüm Azure aboneliklerinin yanı sıra kaynakları sırala.

Kaynak Grafiği aracılığıyla klasik Azure kaynakları keşfedilebilir, ancak ileriye dönük Olarak Azure Kaynak Yöneticisi kaynaklarının oluşturulması ve kullanılması önerilir.

Azure Kaynak Grafiği ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure Aboneliklerinizi görüntüleme:https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure RBAC'ı anlayın:https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="62-maintain-asset-metadata"></a>6.2: Varlık meta verilerini koruma

**Yönerge**: Meta verileri mantıksal olarak taksonomi olarak düzenlemek için meta veriler veren Azure kaynaklarına etiketler uygulayın.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Yetkisiz Azure kaynaklarını silme

**Kılavuz:** Redis örnekleri ve ilgili kaynaklar için Azure Önbelleğini düzenlemek ve izlemek için uygun olduğunda etiketlemeyi, yönetim gruplarını ve ayrı abonelikleri kullanın. Envanteri düzenli olarak uzlaştırın ve yetkisiz kaynakların abonelikten zamanında silindiğinden emin olun.

Ayrıca, aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Ek Azure abonelikleri oluşturma:https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları nasıl oluşturulur:https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler nasıl oluşturulur ve kullanılır:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: Onaylanan Azure kaynaklarının ve yazılım başlıklarının envanterini koruyun

**Rehberlik**: Geçerli değildir; bu öneri, bir bütün olarak hesaplama kaynakları ve Azure için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Onaylanmamış Azure kaynakları için izleme

**Yönerge**: Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure ilkesini kullanın:

İzin verilmeyen kaynak türleri

İzin verilen kaynak türleri

Ayrıca, abonelik içindeki kaynakları sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph ile sorgu oluşturma:

https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

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

**Yönerge**: Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure Politikası'nı kullanın:

İzin verilmeyen kaynak türleri

İzin verilen kaynak türleri

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlkesi ile belirli bir kaynak türü nasıl reddedilecek:

https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="610-implement-approved-application-list"></a>6.10: Onaylı başvuru listesini uygulama

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6.11: Kullanıcıların komut dosyaları aracılığıyla Azure Kaynakları Yöneticisi ile etkileşim deşmelerini sınırlandırın

**Kılavuz**: "Microsoft Azure Yönetimi" Uygulaması için "Erişimi Engelle" özelliğini yapılandırarak kullanıcıların Azure Kaynak Yöneticisi (ARM) ile etkileşim edebilmelerini sınırlamak için Azure Koşullu Erişimi yapılandırın.

ARM'a erişimi engellemek için Koşullu Erişim nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

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

**Yönerge**: Azure İlkesi ile Redis örnekleri için Azure Önbelleğiniz için standart güvenlik yapılandırmaları tanımlayın ve uygulayın. Redis örnekleri için Azure Önbelleği'nizin yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.Cache" ad alanında Azure İlkesi takma adlarını kullanın. Redis örnekleri için Azure Önbelleği'nizle ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz:

Yalnızca Redis Önbelleğinize güvenli bağlantılar etkinleştirilmelidir

Kullanılabilir Azure İlkesi Diğer Adları nasıl görüntüleyebilirsiniz:https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Güvenli işletim sistemi yapılandırmaları oluşturma

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Güvenli Azure kaynak yapılandırmalarını koruyun

**Yol gösterici**: Azure kaynaklarınız arasında güvenli ayarlar uygulamak için Azure ilkesini [reddet] ve [varsa dağıt] kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlke Efektlerini Anlama:https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Güvenli işletim sistemi yapılandırmalarını koruyun

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure kaynaklarının yapılandırmayı güvenli bir şekilde depolama

**Yol gösterici**: Redis örnekleri ve ilgili kaynaklar için Azure Önbelleği'niz için özel Azure İlkesi tanımları veya Azure Kaynak Yöneticisi şablonları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos'unu kullanın.

Azure DevOps'lerde kod depolama:https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Depobelgeleri:https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Özel işletim sistemi görüntülerini güvenli bir şekilde saklayın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7.7: Sistem yapılandırma yönetim araçlarını dağıtma

**Yönerge**: Sistem yapılandırmalarını uyarmak, denetlemek ve uygulamak için özel ilkeler oluşturmak için "Microsoft.Cache" ad alanında Azure İlkesi takma adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve ardışık hatlar geliştirin.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: İşletim sistemleri için sistem yapılandırma yönetim araçlarını dağıtma

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Azure hizmetleri için otomatik yapılandırma izleme uygulaması

**Yönerge**: Sistem yapılandırmalarını uyarmak, denetlemek ve uygulamak için özel ilkeler oluşturmak için "Microsoft.Cache" ad alanında Azure İlkesi takma adlarını kullanın. Redis örnekleri ve ilgili kaynaklar için Azure Önbelleğiniz için yapılandırmaları otomatik olarak uygulamak için Azure ilkesi [denetim], [reddet], [varsa dağıt] kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: İşletim sistemleri için otomatik yapılandırma izleme

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7.11: Azure sırlarını güvenli bir şekilde yönetme

**Yönlendirme**: Azure Uygulama Hizmeti'nde çalışan Azure sanal makineleri veya Redis örnekleri için Azure Önbelleği'ne erişmek için kullanılan web uygulamaları için, Redis gizli yönetimi için Azure Önbelleğini basitleştirmek ve güvence altına almak için Azure Key Vault ile birlikte Yönetilen Hizmet Kimliğini kullanın. Key Vault yumuşak silme etkin olduğundan emin olun.

Azure Yönetilen Kimliklerle nasıl tümleştirilir:

https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Anahtar Kasası nasıl oluşturulur:

https://docs.microsoft.com/azure/key-vault/quick-create-portal

Key Vault kimlik doğrulaması yönetilen bir kimlikle nasıl sağlayacaksınız:

https://docs.microsoft.com/azure/key-vault/managed-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Kimlikleri güvenli ve otomatik olarak yönetme

**Yönlendirme**: Azure Uygulama Hizmeti'nde çalışan Azure sanal makineleri veya Redis örnekleri için Azure Önbelleği'ne erişmek için kullanılan web uygulamaları için, Redis gizli yönetimi için Azure Önbelleğini basitleştirmek ve güvence altına almak için Azure Key Vault ile birlikte Yönetilen Hizmet Kimliğini kullanın. Key Vault Soft Delete etkin olduğundan emin olun.

Azure Etkin Dizini'nde otomatik olarak yönetilen bir kimlikle Azure hizmetlerine yönetilen kimlik sağlamak için Yönetilen Kimlikler'i kullanın. Yönetilen Kimlikler, kodunuzda herhangi bir kimlik belgesi olmadan Azure Key Vault da dahil olmak üzere AAD kimlik doğrulamasını destekleyen tüm hizmetlere kimlik doğrulamanızı sağlar.

Yönetilen Kimlikler nasıl yapılandırılır:

https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

Azure Yönetilen Kimliklerle nasıl tümleştirilir:

https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

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

**Sorumluluk**: Geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: İşlem yapmayan Azure kaynaklarına yüklenecek dosyaları önceden taraya

**Yönlendirme**: Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Redis için Azure Önbelleği) Microsoft kötü amaçlı yazılımdan koruma etkindir, ancak müşteri içeriğinde çalışmaz.

Uygulama Hizmeti, Veri Gölü Depolama, Blob Depolama, PostgreSQL için Azure Veritabanı gibi bilgi işlem dışı Azure kaynaklarına yüklenen tüm içerikleri önceden tarayın. Microsoft bu gibi durumlarda verilerinize erişemez.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Kötü amaçlı yazılımdan koruma yazılımının ve imzalarının güncelleştirilmesini sağlayın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda etkindir (örneğin, Redis için Azure Önbelleği), ancak müşteri içeriğinde çalışmaz.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Düzenli otomatik yedeklemeler sağlayın

**Rehberlik**: Redis'in kalıcılığını etkinleştirin. Redis kalıcılığı, Redis'te depolanan verileri devam ettiremenizi sağlar. Ayrıca anlık görüntüleri alabilir ve bir donanım hatası durumunda yükleyebilirsiniz verileri yedeklemek. Bu, tüm verilerin bellekte depolandığı ve Önbellek düğümlerinin düştüğü bir hata durumunda olası veri kaybı olabileceği Temel veya Standart katmana göre büyük bir avantajdır.

Redis Dışa Aktarma için Azure Önbelleğini de kullanabilirsiniz. Dışa aktarma, Redis için Azure Önbelleğinde depolanan verileri Redis uyumlu RDB dosyasına(lar) dışa aktarmanızı sağlar. Bu özelliği, Redis örneği için bir Azure Önbelleğinden diğerine veya başka bir Redis sunucusuna taşımak için kullanabilirsiniz. Dışa aktarma işlemi sırasında, Redis sunucu örneği için Azure Önbelleğini barındıran sanal makinede geçici bir dosya oluşturulur ve dosya belirlenen depolama hesabına yüklenir. Dışa aktarma işlemi başarı veya hata durumuyla tamamlandığında, geçici dosya silinir.

Redis kalıcılığı nasıl etkinleştirilir:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-persistence

Redis Dışa Aktarma için Azure Önbelleği nasıl kullanılır:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-import-export-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Komple sistem yedeklemelerini gerçekleştirin ve müşteri yönetilen anahtarları yedekleme

**Rehberlik**: Redis'in kalıcılığını etkinleştirin. Redis kalıcılığı, Redis'te depolanan verileri devam ettiremenizi sağlar. Ayrıca anlık görüntüleri alabilir ve bir donanım hatası durumunda yükleyebilirsiniz verileri yedeklemek. Bu, tüm verilerin bellekte depolandığı ve Önbellek düğümlerinin düştüğü bir hata durumunda olası veri kaybı olabileceği Temel veya Standart katmana göre büyük bir avantajdır.

Redis Dışa Aktarma için Azure Önbelleğini de kullanabilirsiniz. Dışa aktarma, Redis için Azure Önbelleğinde depolanan verileri Redis uyumlu RDB dosyasına(lar) dışa aktarmanızı sağlar. Bu özelliği, Redis örneği için bir Azure Önbelleğinden diğerine veya başka bir Redis sunucusuna taşımak için kullanabilirsiniz. Dışa aktarma işlemi sırasında, Redis sunucu örneği için Azure Önbelleğini barındıran sanal makinede geçici bir dosya oluşturulur ve dosya belirlenen depolama hesabına yüklenir. Dışa aktarma işlemi başarı veya hata durumuyla tamamlandığında, geçici dosya silinir.

Redis örnekleri için Azure Önbelleği'niz için kimlik bilgilerini depolamak için Azure Key Vault kullanıyorsanız, anahtarlarınızın düzenli otomatik yedeklemelerini sağlayın.

Redis kalıcılığı nasıl etkinleştirilir:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-premium-persistence

Redis Dışa Aktarma için Azure Önbelleği nasıl kullanılır:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-import-export-data

Anahtar Vault Tuşları nasıl yedeklenebilir:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Müşteri yönetilen anahtarlar dahil tüm yedeklemeleri doğrulayın

**Kılavuz**: Redis Alma için Azure Önbelleğini kullanın. Alma, Linux, Windows veya Amazon Web Hizmetleri ve diğerleri gibi herhangi bir bulut sağlayıcısında çalışan Redis de dahil olmak üzere herhangi bir bulut veya ortamda çalışan herhangi bir Redis sunucusundan Redis uyumlu RDB dosyalarını getirmek için kullanılabilir. Veri alma, önceden doldurulmuş verilerle bir önbellek oluşturmanın kolay bir yoludur. Alma işlemi sırasında, Redis için Azure Önbelleği RDB dosyalarını Azure depolama alanından belleğe yükler ve anahtarları önbelleğe ekler.

Azure Key Vault sırlarınızın veri geri yüklemesini düzenli olarak test edin.

Redis Alma için Azure Önbelleği nasıl kullanılır:

https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-import-export-data

Key Vault Secrets nasıl geri yüklenir:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultsecret?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Yedeklerin ve müşteri yönetilen anahtarlarının korunmasını sağlayın

**Kılavuz :** Redis Dışa Aktarım ve Redis kalıcılığından redis yedeklemeleri için Azure Önbelleği, seçtiğiniz Azure Depolama hesabınızda depolanır. Azure Depolama'daki veriler, mevcut en güçlü blok şifrelerinden biri olan 256 bit AES şifreleme kullanılarak şifrelenir ve şifresi saydam olarak çözülür ve FIPS 140-2 uyumludur. Azure Depolama şifrelemesi devre dışı tutulamaz. Depolama hesabınızın şifrelemesi için Microsoft tarafından yönetilen anahtarlara güvenebilir veya şifrelemeyi kendi anahtarlarınızla yönetebilirsiniz.

Azure Depolama hesapları için şifrelemeyi anlayın:https://docs.microsoft.com/azure/storage/common/storage-service-encryption

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Microsoft

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için [Güvenlik Denetimi: Olay Yanıtı'na](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response)bakın.*

### <a name="101-create-an-incident-response-guide"></a>10.1: Olay yanıt kılavuzu oluşturma

**Rehberlik**: Kuruluşunuz için bir olay yanıt kılavuzu oluşturun. Personelin tüm rollerini ve olay işleme/yönetiminin algılamadan olay sonrası incelemeye kadar olan aşamalarını tanımlayan yazılı olay yanıt planları olduğundan emin olun.

Azure Güvenlik Merkezi'nde İş Akışı Otomasyonları nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

Kendi güvenlik olayı yanıt sürecinizi oluşturma kılavuzu:

https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Microsoft Güvenlik Yanıt Merkezi'nin Bir Olayın Anatomisi:

https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Müşteri ayrıca, kendi olay yanıt planının oluşturulmasına yardımcı olmak için NIST'nin Bilgisayar Güvenliği Olay İşlemleri Kılavuzu'ndan da yararlanabilir:

https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Olay puanlama ve öncelik lendirme prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, öncelikle hangi uyarıların araştırılması gerektiğine öncelik vermenize yardımcı olmak için her uyarıya önem verir. Önem derecesi, Güvenlik Merkezi'nin bulguda ne kadar emin olduğuna veya uyarıyı vermek için kullanılan analize ve uyarıya yol açan etkinliğin arkasında kötü amaçlı niyet olduğuna dair güven düzeyine dayanır.

Ayrıca, abonelikleri açıkça işaretleyin (eski için. azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10.3: Test güvenlik yanıt yordamları

**Rehberlik**: Sistemlerinizin olay yanıt yeteneklerini düzenli olarak test etmek için alıştırmalar yapın. Zayıf noktaları ve boşlukları belirleyin ve planı gerektiği gibi gözden geçirin.

NIST'in yayınına bakın: BT Planları ve Yetenekleri için Test, Eğitim ve Egzersiz Programları Kılavuzu:

https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Güvenlik olayı iletişim bilgilerini sağlayın ve güvenlik olayları için uyarı bildirimlerini yapılandırma

**Kılavuz :** Microsoft Güvenlik Yanıt Merkezi (MSRC), müşterinin verilerine yasadışı veya yetkisiz bir taraf tarafından erişildiğini tespit ederse, güvenlik olayı iletişim bilgileri Microsoft tarafından sizinle iletişim kurmak için kullanılır.  Sorunların çözülmesini sağlamak için olaydan sonraki olayları gözden geçirin.

Azure Güvenlik Merkezi Güvenlik Kişisi nasıl ayarlanır:

https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Olay yanıt sisteminize güvenlik uyarıları dahil edin

**Kılavuz**: Sürekli Dışa Aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışa aktarın. Sürekli Dışa Aktarma, uyarı ve önerileri el ile veya sürekli olarak dışa aktarmanıza olanak tanır. Sentinel uyarılarını yayınlamak için Azure Güvenlik Merkezi veri konektörünü kullanabilirsiniz.

Sürekli dışa aktarma nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/security-center/continuous-export

Uyarıları Azure Sentinel'e nasıl aktarın:

https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Güvenlik uyarılarına yanıtı otomatikleştirin

**Yönerge**: Güvenlik uyarıları ve önerileri nde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek için Azure Güvenlik Merkezi'ndeki İş Akışı Otomasyonu özelliğini kullanın.

İş Akışı Otomasyonu ve Mantık Uygulamaları nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için [Güvenlik Kontrolü: Penetrasyon Testleri ve Kırmızı Takım Egzersizleri'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises)bakın.*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11.1: Azure kaynaklarınızı düzenli olarak delme testini gerçekleştirin ve tüm kritik güvenlik bulgularının 60 gün içinde düzeltilmesini sağlayın

**Kılavuz**: Penetrasyon Testlerinizin Microsoft ilkelerini ihlal etmediğini sağlamak için Microsoft Etkileşim Kuralları'na uyun:

https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

Microsoft'un Stratejisi ve Red Teaming ve Microsoft tarafından yönetilen bulut altyapısı, hizmetler ve uygulamalara karşı canlı site penetrasyon testi nin uygulanması hakkında daha fazla bilgiyi burada bulabilirsiniz: 

https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Güvenlik Kıyaslama'ya](https://docs.microsoft.com/azure/security/benchmarks/overview) bakın
- [Azure Güvenlik Taban Çizgileri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
