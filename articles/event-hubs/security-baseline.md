---
title: Etkinlik Hub'ları için Azure Güvenlik Taban Çizgisi
description: Etkinlik Hub'ları için Azure Güvenlik Taban Çizgisi
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: f16f6ef38b221e525340858fc7ba131f8a1bdf5a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80289582"
---
# <a name="azure-security-baseline-for-event-hubs"></a>Etkinlik Hub'ları için Azure Güvenlik Taban Çizgisi

Etkinlik Hub'ları için Azure Güvenlik Taban Çizgisi, dağıtımınızın güvenlik duruşunu geliştirmenize yardımcı olacak öneriler içerir.

Bu hizmetin temeli, en iyi uygulamalar kılavuzumuzla Azure'da bulut çözümlerinizi nasıl güvenebileceğinize ilişkin öneriler sunan [Azure Güvenlik Kıyaslama sürümü 1.0'dan](https://docs.microsoft.com/azure/security/benchmarks/overview)alınmıştır.

Daha fazla bilgi için [Azure Güvenlik Taban Çizgilerini genel olarak](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)görün.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için [Güvenlik Denetimi: Ağ Güvenliği'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)bakın.*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Sanal Ağınızdaki Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'nı kullanarak kaynakları koruyun

**Kılavuz**: Etkinlik hub'larının sanal ağ hizmeti uç noktalarıyla tümleştirilmesi, sanal ağlara bağlı sanal makineler gibi iş yüklerinden mesajlaşma yeteneklerine güvenli erişim sağlar ve ağ trafik yolu her iki uçta da güvenli hale gelir.

Bir kez en az bir sanal ağ alt ağ hizmet bitiş noktasına bağlı, ilgili Olay Hub'ları ad alanı artık sanal ağlarda herhangi bir yerden değil, yetkili alt ağlardan gelen trafiği kabul eder. Sanal ağ açısından bakıldığında, Olay Hub'larınızı bir hizmet bitiş noktasına bağlamak, sanal ağ alt ağından mesajlaşma hizmetine yalıtılmış bir ağ tünelini yapılandırır. 

Azure Özel Bağlantı hizmetini kullanarak sizi özel ve güvenli bir şekilde Azure Etkinlik Hub'ları hizmetine bağlayan bir ağ arabirimi olan özel bir bitiş noktası da oluşturabilirsiniz. Özel bitiş noktası, VNet'inizden gelen özel bir IP adresini kullanır ve hizmeti VNet'inize etkin bir şekilde getirir. Hizmete giden tüm trafik özel bitiş noktasından yönlendirilebilir, bu nedenle ağ geçidi, NAT aygıtları, ExpressRoute veya VPN bağlantıları veya genel IP adresleri gerekmez. 

Güvenlik duvarlarını kullanarak Azure Etkinlik Hub'larınızı da güvene alabilirsiniz. Azure Olay Hub'ları, gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini destekler. Azure portalını, Azure Kaynak Yöneticisi şablonlarını kullanarak veya Azure CLI veya Azure PowerShell aracılığıyla güvenlik duvarı kuralları ayarlayabilirsiniz.

Azure Etkinlik Hub'ları ile sanal ağ hizmeti uç noktaları nasıl kullanılır:https://docs.microsoft.com/azure/event-hubs/event-hubs-service-endpoints

Daha fazla bilgi için bkz: Azure Etkinlik https://docs.microsoft.com/azure/event-hubs/private-link-serviceHub'larını Azure Özel Bağlantısıyla Tümleştirin: .

Olay Hub'ları ad alanında Sanal Ağlar Tümleştirmesini ve Güvenlik Duvarlarını etkinleştirin:https://docs.microsoft.com/azure/event-hubs/event-hubs-tutorial-virtual-networks-firewalls

Azure Olay Hub'ları ad alanları için IP güvenlik duvarı kuralları nasıl yapılandırılabilen:https://docs.microsoft.com/azure/event-hubs/event-hubs-ip-filtering

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Vnet, Subnet ve NIC'lerin yapılandırmasını ve trafiğini izleyin ve kaydedin

**Yönlendirme**: Azure Güvenlik Merkezi'ni kullanın ve Azure'daki Etkinlik Hub'ları kaynaklarınızın güvenliğini sağlamaya yardımcı olmak için ağ koruma önerilerine uyun. Etkinlik hub'larınıza erişmek için Azure sanal makineleri kullanıyorsanız, ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve trafik denetimi için bir depolama hesabına günlükler gönderin.

NSG Akış Günlükleri Nasıl Etkinleştirilir:https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Azure Güvenlik Merkezi tarafından sağlanan Ağ Güvenliğini Anlama:https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="13-protect-critical-web-applications"></a>1.3: Kritik web uygulamalarını koruyun

**Rehberlik**: Geçerli değildir; Bu öneri, Azure Uygulama Hizmeti'nde çalışan web uygulamaları veya bilgi işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Bilinen kötü amaçlı IP adresleriyle iletişimi reddetme

**Kılavuz :** Dağıtılmış hizmet reddi (DDoS) saldırılarına karşı korunmak için etkinlik hub'larınızla ilişkili sanal ağlarda DDoS Koruma Standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimi reddetmek için Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı kullanın.

DDoS koruması nasıl yapılandırılabilen:https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı hakkında daha fazla bilgi için:https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1.5: Ağ paketlerini ve akış günlüklerini kaydetme

**Kılavuz**: Etkinlik hub'larınıza erişmek için Azure sanal makineleri kullanıyorsanız, ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir depolama hesabına gönderin. Ayrıca NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzdaki trafik akışıhakkında öngörüler sağlamak için Trafik Analitiği'ni kullanabilirsiniz. Traffic Analytics'in bazı avantajları, ağ etkinliğini görselleştirme ve etkin noktaları belirleme, güvenlik tehditlerini belirleme, trafik akışı desenlerini anlama ve ağ yanlış yapılandırmalarını saptabilme yeteneğidir.

Anormal etkinliği araştırmak için gerekirse, Ağ İzleyicipaket yakalamayı etkinleştirin.

NSG Akış Günlükleri Nasıl Etkinleştirilir:https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Trafik Analizi ni etkinleştirme ve kullanma:https://docs.microsoft.com/azure/network-watcher/traffic-analytics

Ağ İzleyicisi nasıl etkinleştirilir:https://docs.microsoft.com/azure/network-watcher/network-watcher-create

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Ağ tabanlı izinsiz giriş algılama/saldırı önleme sistemlerini (IDS/IPS) dağıtma

**Kılavuz**: Etkinlik hub'larınıza erişmek için Azure sanal makineleri kullanıyorsanız, Azure Marketi'nden yük denetimi özellikleriyle IDS/IPS işlevselliğini destekleyen bir teklif seçin. Kuruluşunuz için yük denetimine dayalı izinsiz giriş tespiti ve/veya önleme gerekli değilse, Azure Etkinlik Hub'larının yerleşik güvenlik duvarı özelliğini kullanabilirsiniz. Güvenlik Duvarı kurallarını kullanarak, sınırlı sayıda IP adresi veya belirli bir IP adresi için Olay Hub'larınıza erişimi sınırlandırabilirsiniz.

Azure Marketi:

https://azuremarketplace.microsoft.com/marketplace/?term=Firewall

Belirli bir IP adresi için Olay Hub'larına güvenlik duvarı kuralı ekleme:

 https://docs.microsoft.com/azure/event-hubs/event-hubs-tutorial-virtual-networks-firewalls#add-firewall-for-specified-ip

**Azure Güvenlik Merkezi izleme**: Henüz kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Web uygulamalarına trafik yönetme

**Rehberlik**: Geçerli değildir; Bu öneri, Azure Uygulama Hizmeti'nde çalışan web uygulamaları veya bilgi işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Ağ güvenliği kurallarının karmaşıklığını ve yönetim ek lerini en aza indirmek

**Yönerge**: Bu öneri, Azure Uygulama Hizmeti'nde çalışan web uygulamaları veya bilgi işlem kaynakları için geçerli değildir.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Ağ aygıtları için standart güvenlik yapılandırmalarını koruyun

**Yönerge**: Azure İlkesi ile Azure Etkinlik Hub'ları ad alanlarınızla ilişkili ağ kaynakları için standart güvenlik yapılandırmaları tanımlayın ve uygulayın. Olay Hub'larınızın ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.EventHub" ve "Microsoft.Network" ad alanlarında Azure İlkesi takma adlarını kullanın. Azure Etkinlik Hub'ları ile ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz:

- Olay Hub sanal ağ hizmeti bitiş noktası kullanmalıdır.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Etkinlik Hub'ları için Azure Yerleşik İlkesi ad alanı:https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#event-hub



Ağ oluşturmak için Azure İlkesi örnekleri:https://docs.microsoft.com/azure/governance/policy/samples/#network



Azure Blueprint nasıl oluşturulur:https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="110-document-traffic-configuration-rules"></a>1.10: Belge trafiği yapılandırma kuralları

**Yönerge**: Sanal ağlar ve ağ güvenliği ve olay hub'larınızla ilişkili trafik akışıyla ilgili diğer kaynaklar için etiketler kullanın.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Ağ kaynağı yapılandırmalarını izlemek ve değişiklikleri algılamak için otomatik araçlar kullanın

**Yönerge**: Ağ kaynak yapılandırmalarını izlemek ve Azure Olay Hub'ları ile ilgili ağ kaynaklarının değişikliklerini algılamak için Azure Etkinlik Günlüğü'ne kullanın. Azure Monitor'da kritik ağ kaynaklarında değişiklikler olduğunda tetikleyecek uyarılar oluşturun.

Azure Etkinlik Günlüğü etkinliklerini görüntüleme ve alma:https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

Azure Monitor'da uyarı oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için [Güvenlik Denetimi: Günlüğe kaydetme ve izleme.](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Onaylanmış zaman senkronizasyon kaynaklarını kullanma

**Rehberlik**: Geçerli değildir; Microsoft, günlüklerde zaman damgaları için Azure Etkinlik Hub'ları gibi Azure kaynakları için kullanılan zaman kaynağını tutar.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Yönlendirme**: Azure Monitor'da, Etkinlik Günlüğü ve Etkinlik Merkezi tanı lama ayarlarındaki olay hub'ları ile ilgili günlükleri, sorgulanmak üzere Günlük Analizi çalışma alanına veya uzun süreli arşiv depolama için bir depolama hesabına göndermek üzere yapılandırın.

Azure Olay Hub'ları için Tanılama Ayarları nasıl yapılandırılabilen:https://docs.microsoft.com/azure/event-hubs/event-hubs-diagnostic-logs

Azure Etkinlik Günlüğünü Anlama:https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure kaynakları için denetim günlüğe kaydetmeyi etkinleştirme

**Kılavuz**: Azure Etkinlik Hub'larınız ad alanınız için Tanılama ayarlarını etkinleştirin. Azure Etkinlik Hub'ları için tanılama ayarlarının üç kategorisi vardır: Arşiv Günlükleri, Operasyonel Günlükler ve Otomatik Ölçekgünlükleri. Olay Hub'ları işlemleri sırasında neler olduğu hakkında bilgi almak için Operasyonel Günlükleri etkinleştirin, özellikle olay hub'ı oluşturma, kullanılan kaynaklar ve işlemin durumu dahil olmak üzere işlem türü.

Ayrıca, Azure Etkinliği günlük tanılama ayarlarını etkinleştirebilir ve bunları bir Azure Depolama Hesabı'na, etkinlik merkezine veya Log Analytics çalışma alanına gönderebilirsiniz. Etkinlik günlükleri, Azure Etkinlik Hub'larınızda ve diğer kaynaklarda gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlüklerini kullanarak, Azure Etkinlik Hub'ları ad alanlarınızda alınan herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" olduğunu belirleyebilirsiniz.

Azure Etkinlik Hub'ları için Tanı lama Ayarları nasıl etkinleştirilir:https://docs.microsoft.com/azure/event-hubs/event-hubs-diagnostic-logs

Azure Etkinlik Günlüğü için Tanı lama Ayarları nasıl etkinleştirilir:https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: İşletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2.5: Güvenlik günlüğü depolama tutma yapılandırma

**Kılavuz**: Azure Monitor'da, etkinlik merkeziyle ilgili olayları yakalamak ve incelemek için Log Analytics çalışma alanı saklama sürenizi kuruluşunuzun uyumluluk yönetmeliklerine göre ayarlayın.

Log Analytics çalışma alanları için günlük bekletme parametreleri nasıl ayarlanır:https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2.6: Günlükleri izleme ve inceleme

**Rehberlik**: Anormal davranışlar için günlükleri analiz edin ve izleyin ve olay merkezlerinizle ilgili sonuçları düzenli olarak gözden geçirin. Günlükleri gözden geçirmek ve günlük verilerinde sorgular gerçekleştirmek için Azure Monitor'un Günlük Analizi'ni kullanın. Alternatif olarak, Azure Sentinel'e veya üçüncü taraf bir SIEM'e veri etkinleştirebilir ve yerleşik veriler verebilirsiniz.
 

Log Analytics çalışma alanı hakkında daha fazla bilgi için:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal

Azure Monitor'da özel sorgular nasıl gerçekleştirililir:https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries

Azure Sentinel'e nasıl binilir:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Anormal aktivite için uyarıları etkinleştirme

**Kılavuz :** Azure Monitor'da, Etkinlik Günlüğü içindeki Azure Etkinlik Hub'ları ve Olay Hub'ları ile ilgili günlükleri yapılandırarak günlükleri sorgulanmak üzere Günlük Analizi çalışma alanına veya uzun süreli arşiv depolama alanı için bir depolama hesabına göndermek için Etkinlik Hub'ları tanıayarlarına dönüştürün. Güvenlik günlüklerinde ve olaylarında bulunan anormal etkinlikler için uyarılar oluşturmak için Günlük Analizi çalışma alanını kullanın.

Alternatif olarak, Azure Sentinel'e veri etkinleştirebilir ve yerleşik veriler de dahil olabilirsiniz. 

Azure Etkinlik Günlüğünü Anlayın:https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview

Azure Olay Hub'ları için Tanılama Ayarları nasıl yapılandırılabilen:https://docs.microsoft.com/azure/event-hubs/event-hubs-diagnostic-logs

Log Analytics çalışma alanı günlük verilerinde nasıl uyarı olun:https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response

Azure Sentinel'e nasıl binilir:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Henüz kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="28-centralize-anti-malware-logging"></a>2.8: Kötü amaçlı yazılımdan koruma günlüğe kaydetmeyi merkezileştirin

**Rehberlik**: Geçerli değildir; Olay Hub'ı kötü amaçlı yazılımdan koruma günlüğe kaydetmeişlemini işlemez.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="29-enable-dns-query-logging"></a>2.9: DNS sorgu günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: Geçerli değildir; Olay Hub'ları DNS ile ilgili günlükleri işlemez veya üretmez.

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

**Yönlendirme**: Etkinlik Hub'larına düzlem erişimini kontrol edin Azure Active Directory (AD) aracılığıyla denetlenir. Azure AD varsayılan parola kavramına sahip değildir.

Etkinlik Hub'larına veri düzlemi erişimi, Yönetilen Kimlikler veya Uygulama kayıtlarının yanı sıra paylaşılan erişim imzalarıyla Azure AD aracılığıyla denetlenir. Paylaşılan erişim imzaları, etkinlik hub'larınıza bağlanan istemciler tarafından kullanılır ve herhangi bir zamanda yeniden oluşturulabilir.

Etkinlik Hub'ları için paylaşılan erişim imzalarını anlayın:https://docs.microsoft.com/azure/event-hubs/authenticate-shared-access-signature

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

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

**Kılavuz**: Microsoft Azure, Azure Etkin Dizini'ni (AD) temel alan kaynaklar ve uygulamalar için tümleşik erişim denetimi yönetimi sağlar. Azure Etkinlik Hub'ları ile Azure AD kullanmanın önemli bir avantajı, kimlik bilgilerinizi artık kodda depolamanız gerekmemese de. Bunun yerine, Microsoft Identity platformundan Bir OAuth 2.0 erişim jetonu isteyebilirsiniz. Belirteç istemek için kaynak https://eventhubs.azure.net/adı. Azure AD, uygulamayı çalıştıran güvenlik ilkesinin (kullanıcı, grup veya hizmet sorumlusu) kimliğini doğrular. Kimlik doğrulama başarılı olursa, Azure AD uygulamaya bir erişim belirteci döndürür ve uygulama daha sonra isteği Azure Olay Hub'ları kaynaklarına yetkilendirmek için erişim belirteci'ni kullanabilir.

Etkinlik Hub'ları kaynaklarına erişmek için Azure AD ile bir uygulamanın kimliğinin doğrulanması:https://docs.microsoft.com/azure/event-hubs/authenticate-application

Azure AD ile SSO'nun anlaşılması:https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Tüm Azure Active Directory tabanlı erişim için çok faktörlü kimlik doğrulamayı kullanın

**Yönerge**: Azure Etkin Dizin Çok Faktörlü Kimlik Doğrulaması'nı (MFA) etkinleştirin ve Etkinlik Hub'ı tarafından etkinleştirilen kaynaklarınızın korunmasına yardımcı olmak için Azure Güvenlik Merkezi Kimliği ve erişim yönetimi önerilerini izleyin.

Azure'da MFA nasıl etkinleştirilir:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Azure Güvenlik Merkezi'nde kimlik ve erişim nasıl izlenir:https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Tüm idari görevler için özel makineleri (Ayrıcalıklı Erişim İş İstasyonları) kullanın

**Kılavuz :** Event Hub özellikli kaynaklara giriş yapmak ve yapılandırmak üzere yapılandırılan Çok Faktörlü Kimlik Doğrulama (MFA) ile ayrıcalıklı erişim iş istasyonları (PAW) kullanın.

Ayrıcalıklı Erişim İş İstasyonları hakkında bilgi edinin:https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure'da MFA nasıl etkinleştirilir:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: İdari hesaplardan şüpheli etkinliklere ilişkin günlük ve uyarı

**Kılavuz :** Çevrede şüpheli veya güvensiz etkinlik oluştuğunda günlük ve uyarı oluşturma için Azure Active Directory (AD) Ayrıcalıklı Kimlik Yönetimi 'ni (PIM) kullanın. Riskli kullanıcı davranışıyla ilgili uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın. Ek günlüğe kaydetme için Azure Güvenlik Merkezi risk algılama uyarılarını Azure Monitor'a gönderin ve eylem gruplarını kullanarak özel uyarı/bildirimleri yapılandırın.



Ayrıcalıklı Kimlik Yönetimi (PIM) nasıl dağıtılır:https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan


Azure AD risk algılamalarını anlayın:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events

Özel uyarı ve bildirim için eylem grupları nasıl yapılandırılabilen:https://docs.microsoft.com/azure/azure-monitor/platform/action-groups

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Yönerge**: Yalnızca IP adresi aralıklarının veya ülkelerin/bölgelerin inbelirli mantıksal gruplandırmalarından erişime izin vermek için Koşullu Erişim Adlandırılmış Konumlar'ı kullanın.



Azure'da Adlandırılmış Konumlar nasıl yapılandırılabilen:https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="39-use-azure-active-directory"></a>3.9: Azure Etkin Dizini Kullanma

**Yönerge**: Azure Etkin Dizin (AD) kullanıcı, Etkinlik Hub'ları gibi Azure kaynakları için merkezi kimlik doğrulama ve yetkilendirme sistemi olarak kullanın. Bu, rol tabanlı erişim denetimine (RBAC) yönetime duyarlı kaynaklara izin verir.

 Azure AD örneğini oluşturma ve yapılandırma:https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant

Azure Etkinlik Hub'larının Azure Etkin Dizini (AAD) ile nasıl entegre olduğunu öğrenmek için Bkz. Azure Etkin Dizin'i kullanarak Etkinlik Hub kaynaklarına erişimi yetkilendirme:https://docs.microsoft.com/azure/event-hubs/authorize-access-azure-active-directory

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Kullanıcı erişimini düzenli olarak gözden geçirin ve uzlaştırın

**Rehberlik**: Azure Active Directory (AD), eski hesapları keşfetmenize yardımcı olacak günlükler sağlar. Ayrıca, grup üyeliklerini, kurumsal uygulamalara erişimi ve rol atamalarını verimli bir şekilde yönetmek için Azure Kimlik Erişim İncelemeleri'ni kullanın. Kullanıcı erişimi, yalnızca doğru Kullanıcıların sürekli erişime sahip olduğundan emin olmak için düzenli olarak gözden geçirilebilir.

Ayrıca, Etkinlik Hub'larınızın paylaşılan erişim imzalarını düzenli olarak döndürün.

Azure AD raporlamalarını anlayın:https://docs.microsoft.com/azure/active-directory/reports-monitoring/

Azure Kimlik Erişim İncelemeleri nasıl kullanılır:https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

Etkinlik Hub'ları için paylaşılan erişim imzalarını anlama:https://docs.microsoft.com/azure/event-hubs/authenticate-shared-access-signature

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Devre dışı bırakılmış hesaplara erişim denemelerini izleme

**Kılavuz**: Herhangi bir SIEM/İzleme aracıyla tümleştirmenize olanak tanıyan Azure Active Directory (AD) oturum açma etkinliği, denetim ve risk olay günlüğü kaynaklarına erişebilirsiniz.

Azure AD kullanıcı hesapları için tanılama ayarları oluşturarak ve denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. Log Analytics içinde istediğiniz günlük uyarılarını yapılandırabilirsiniz.

Azure Etkinlik Günlükleri Azure Monitör'e nasıl entegre eleştirilir:https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

Azure Etkin Dizin'i kullanarak Etkinlik Hub kaynaklarına erişimi yetkilendirmeyin:https://docs.microsoft.com/azure/event-hubs/authorize-access-azure-active-directory

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Hesap giriş davranış sapması konusunda uyarı

**Yönlendirme**: Olay Hub'ları etkin leştirilmiş kaynaklarınız ile ilgili algılanan şüpheli eylemlere otomatik yanıtları yapılandırmak için Azure Active Directory'nin Kimlik Koruması ve risk algılama özelliklerini kullanın. Kuruluşunuzun güvenlik yanıtlarını uygulamak için Azure Sentinel aracılığıyla otomatik yanıtları etkinleştirmelisiniz.

Azure AD riskli oturum açmaları nasıl görüntülenebilen:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Kimlik Koruması risk ilkelerinin nasıl yapılandırılabilen ve etkinleştirilir:https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

Azure Sentinel'e nasıl binilir:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Destek senaryoları sırasında Microsoft'a ilgili müşteri verilerine erişim sağlama

**Rehberlik**: Şu anda mevcut değil; Müşteri Kilit Kutusu henüz Olay Hub'ları için desteklenmedi.

Müşteri Kilit Kutusu destekli hizmetler listesi:https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Şu anda mevcut değil

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için [güvenlik denetimi: veri koruma.](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Hassas Bilgilerin envanterini koruyun

**Yönerge**: Hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olmak için Etkinlik Hub'larınızla ilgili kaynaklardaki etiketleri kullanın.

Etiketler nasıl oluşturulur ve kullanılır:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Hassas bilgileri depolama veya işleme sistemlerini yalıtma

**Rehberlik**: Geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Olay Hub'ları ad alanları, hizmet bitiş noktaları etkinleştirilmiş ve uygun şekilde etiketlenmiş sanal ağ tarafından ayrılmalıdır.

Güvenlik duvarları kullanarak Azure Etkinlik Hub'larınızı da güvene alabilirsiniz. Azure Olay Hub'ları, gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini destekler. Azure portalını, Azure Kaynak Yöneticisi şablonlarını kullanarak veya Azure CLI veya Azure PowerShell aracılığıyla güvenlik duvarı kuralları ayarlayabilirsiniz.

Ek Azure abonelikleri oluşturma:https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları nasıl oluşturulur:https://docs.microsoft.com/azure/governance/management-groups/create

Azure Olay Hub'ları ad alanları için IP güvenlik duvarı kurallarını yapılandırın:https://docs.microsoft.com/azure/event-hubs/event-hubs-ip-filtering

Etiketlernasıl oluşturulur ve kullanılır:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Sanal Ağ nasıl oluşturulur:https://docs.microsoft.com/azure/virtual-network/quick-create-portal

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Hassas bilgilerin yetkisiz aktarımının izlenmesi ve engellenmesi

**Kılavuz**: Etkinlik hub'larınıza erişmek için sanal makineleri kullanırken, veri sızma olasılığını azaltmak için sanal ağları, hizmet bitiş noktalarını, Olay Hub'larını, ağ güvenlik gruplarını ve hizmet etiketlerini kullanın.

Microsoft, Azure Etkinlik Hub'ları için temel altyapıyı yönetir ve müşteri verilerinin kaybolmasını veya açığa dökülmesini önlemek için sıkı denetimler uygulamıştır.

Azure Olay Hub'ları ad alanları için IP güvenlik duvarı kurallarını yapılandırın:https://docs.microsoft.com/azure/event-hubs/event-hubs-ip-filtering

Azure Etkinlik Hub'ları ile Sanal Ağ Hizmeti Bitiş Noktalarını Anlayın:https://docs.microsoft.com/azure/event-hubs/event-hubs-service-endpoints

Azure Etkinlik Hub'larını Azure Özel Bağlantısıyla tümleştirin:https://docs.microsoft.com/azure/event-hubs/private-link-service

Ağ Güvenlik Gruplarını ve Hizmet Etiketlerini Anlayın:https://docs.microsoft.com/azure/virtual-network/security-overview

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Tüm hassas bilgileri taşıma sırasında şifreleme

**Yönerge**: Azure Etkinlik Hub'ları varsayılan olarak TLS şifreli iletişimleri zorlar. TLS sürümleri 1.0, 1.1 ve 1.2 şu anda desteklenmiştir. Ancak, TLS 1.0 ve 1.1 endüstri çapında amortismana giden bir yoldadır, bu nedenle mümkünse TLS 1.2'yi kullanın.

Olay Hub'larının güvenlik özelliklerini anlamak için Ağ güvenliği ne bakınız:https://docs.microsoft.com/azure/event-hubs/network-security

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Hassas verileri tanımlamak için etkin bir bulma aracı kullanın

**Kılavuz :** Azure Etkinlik Hub'ları için veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz kullanılamıyor. Uyumluluk amacıyla gerekirse üçüncü taraf çözümlerini uygulayın.

Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Kaynaklara erişimi denetlemek için Azure RBAC'ı kullanın

**Yönlendirme**: Azure Etkinlik Hub'ları, İstekleri Etkinlik Hub'ları kaynaklarına yetkilendirmek için Azure Active Directory (AD) kullanarak destekler. Azure AD ile, bir kullanıcı veya uygulama hizmeti ilkesi olabilecek bir güvenlik ilkesine izin vermek için rol tabanlı erişim denetimini (RBAC) kullanabilirsiniz.

Azure Etkinlik Hub'ları için Azure AD RBAC'ı ve kullanılabilir rolleri anlayın:https://docs.microsoft.com/azure/event-hubs/authorize-access-azure-active-directory

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Erişim denetimini uygulamak için ana bilgisayar tabanlı veri kaybı önlemeyi kullanın

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.

Microsoft, Olay Hub'ları için temel altyapıyı yönetir ve müşteri verilerinin kaybolmasını veya açığa dökülmesini önlemek için sıkı denetimler uygulamıştır.

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Hassas bilgileri istirahatte şifreleme

**Yönerge**: Azure Olay Hub'ları, Microsoft tarafından yönetilen anahtarlar veya müşteri tarafından yönetilen anahtarlarla verileri istirahatte şifreleme seçeneğini destekler. Bu özellik, Azure Etkinlik Hub'ları verilerini kolaylaştırmak için kullanılan müşteri tarafından yönetilen anahtarlara erişimi oluşturmanıza, döndürmenize, devre dışı etmenizi ve iptal etmenizi sağlar.

Azure Etkinlik Hub'larını şifrelemek için müşteri tarafından yönetilen anahtarları yapılandırma:https://docs.microsoft.com/azure/event-hubs/configure-customer-managed-key

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Kritik Azure kaynaklarında yapılan değişiklikleri kaydedin ve uyarı

**Yönerge**: Azure Etkinlik Hub'larının ve diğer kritik veya ilgili kaynakların üretim örneklerinde değişikliklerin ne zaman yapılacağına ilişkin uyarılar oluşturmak için Azure Etkinliği günlüğüyle Azure Monitörünü kullanın.

Azure Etkinlik Günlüğü etkinlikleri için uyarılar oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için [Güvenlik Denetimi: Güvenlik Açığı Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)bakın.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Otomatik güvenlik açığı tarama araçlarını çalıştırın

**Rehberlik**: Geçerli değildir; Microsoft, Olay Hub'larını destekleyen temel sistemlerde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Otomatik işletim sistemi yama yönetimi çözümlerini dağıtma

**Rehberlik**: Geçerli değildir; Microsoft, Olay Hub'larını destekleyen temel sistemlerde yama yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3: Otomatik üçüncü taraf yazılım yama yönetimi çözümlerini dağıtma

**Rehberlik**: Geçerli değildir; kıyaslama hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Arka arkaya güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: Geçerli değildir; Microsoft, Olay Hub'larını destekleyen temel sistemlerde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Keşfedilen güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: Geçerli değildir; Microsoft, Olay Hub'larını destekleyen temel sistemlerde güvenlik açığı yönetimi gerçekleştirir.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için [güvenlik denetimi: Envanter ve Varlık Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)bakın.*

### <a name="61-use-azure-asset-discovery"></a>6.1: Azure Varlık Bulma'yı kullanma

**Yönerge**: Aboneliğinizdeki tüm kaynakları (Azure Olay Hub'ları ad alanları dahil) sorgulamak ve keşfetmek için Azure Kaynak Grafiği'ni kullanın. Kiracınızda uygun (okuma) izinlere sahip olduğundan ve aboneliklerinizdeki tüm Azure aboneliklerinin yanı sıra kaynakları sıralayabildiğinizden emin olun.

Azure Graph ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure Aboneliklerinizi görüntüleme:https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure RBAC'ı anlayın:https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="62-maintain-asset-metadata"></a>6.2: Varlık meta verilerini koruma

**Yönerge**: Meta verileri mantıksal olarak taksonomi olarak düzenlemek için meta veriler veren Azure kaynaklarına etiketler uygulayın.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Yetkisiz Azure kaynaklarını silme

**Yönerge**: Azure Etkinlik Hub'larının ad alanlarını ve ilgili kaynaklarını düzenlemek ve izlemek için uygun olduğunda etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanteri düzenli olarak uzlaştırın ve yetkisiz kaynakların abonelikten zamanında silindiğinden emin olun.

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

### <a name="611-divlimit-users-ability-to-interact-with-azure-resource-manager-via-scriptsdiv"></a>6.11: <div>Kullanıcıların komut dosyaları aracılığıyla Azure Kaynak Yöneticisi ile etkileşim deşme lerini sınırlandırın</div>

**Kılavuz**: "Microsoft Azure Yönetimi" Uygulaması için "Erişimi engelle" yapılandırarak kullanıcıların Azure Kaynak Yöneticisi ile etkileşim edebilmelerini sınırlamak için Azure Koşullu Erişimi yapılandırın.

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

**Yönerge**: Azure Etkinlik Hub'larınız için standart güvenlik yapılandırmalarını tanımlayın ve uygulayın. Yapılandırmaları denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.EventHub" ad alanında Azure İlkesi takma adlarını kullanın. Azure Etkinlik Hub'ları için yerleşik ilke tanımlarından da yararlanabilirsiniz:

- Event Hub'daki tanılama günlükleri etkinleştirilmeli

- Olay Hub sanal ağ hizmeti bitiş noktası kullanmalıdır

Etkinlik Hub'ları için Azure Yerleşik https://docs.microsoft.com/en-\ İlkesi ad alanı: abd/azure/yönetişim/ilke/örnekler/yerleşik ilkeler#olay-hub

Kullanılabilir Azure İlkesi Diğer Adları nasıl görüntüleyebilirsiniz:https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Güvenli işletim sistemi yapılandırmaları oluşturma

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Güvenli Azure kaynak yapılandırmalarını koruyun

**Yol gösterici**: Olay Hub'ları etkin kaynaklarınızda güvenli ayarlar uygulamak için Azure ilkesini [reddet] ve [varsa dağıt] kullanın. 

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

 
Azure İlke Efektleri hakkında daha fazla bilgi için:https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Güvenli işletim sistemi yapılandırmalarını koruyun

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure kaynaklarının yapılandırmayı güvenli bir şekilde depolama

**Yönlendirme**: Etkinlik Hub'larınız veya ilgili kaynaklarınız için özel Azure İlkesi tanımlarını kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos'unu kullanın.

Azure DevOps'lerde kod depolama:https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Depobelgeleri:https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Özel işletim sistemi görüntülerini güvenli bir şekilde saklayın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7.7: Sistem yapılandırma yönetim araçlarını dağıtma

**Yönerge**: Sistem yapılandırmalarını uyarmak, denetlemek ve uygulamak için özel ilkeler oluşturmak için "Microsoft.EventHub" ad alanında Azure İlkesi takma adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve ardışık hatlar geliştirin.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: İşletim sistemleri için sistem yapılandırma yönetim araçlarını dağıtma

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Azure hizmetleri için otomatik yapılandırma izleme uygulaması

**Yönerge**: Sistem yapılandırmalarını uyarmak, denetlemek ve uygulamak için özel ilkeler oluşturmak için "Microsoft.EventHub" ad alanında Azure İlkesi takma adlarını kullanın. Azure Etkinlik Hub'ları dağıtımlarınız ve ilgili kaynaklarınız için yapılandırmaları otomatik olarak uygulamak için Azure ilkesi [denetim], [reddet], [varsa dağıt] ve [varsa dağıt] kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: İşletim sistemleri için otomatik yapılandırma izleme

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7.11: Azure sırlarını güvenli bir şekilde yönetme

**Yönlendirme**: Azure Uygulama Hizmeti'nde çalışan Azure sanal makineleri veya web uygulamaları için etkinlik hub'larınıza erişmek için kullanılabilir, Azure Etkinlik Hub'ları dağıtımlarınız için paylaşılan erişim imza yönetimini basitleştirmek ve güvence altına almak için Azure Key Vault ile birlikte Yönetilen Hizmet Kimliğini kullanın. Key Vault yumuşak silme etkin olduğundan emin olun.

Etkinlik Hub'ları kaynaklarına erişmek için Azure Etkin Dizini ile yönetilen bir kimliğin kimliğini doğrulatın:https://docs.microsoft.com/azure/event-hubs/authenticate-managed-identity?tabs=latest

Etkinlik Hub'ları için müşteri tarafından yönetilen anahtarları yapılandırın:https://docs.microsoft.com/azure/event-hubs/configure-customer-managed-key 

Azure Yönetilen Kimliklerle nasıl tümleştirilir:https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Anahtar Kasası nasıl oluşturulur:https://docs.microsoft.com/azure/key-vault/quick-create-portal

Key Vault kimlik doğrulaması yönetilen bir kimlikle nasıl sağlayacaksınız:https://docs.microsoft.com/azure/key-vault/managed-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Kimlikleri güvenli ve otomatik olarak yönetme

**Yönlendirme**: Azure Uygulama Hizmeti'nde çalışan Azure sanal makineleri veya web uygulamaları etkinlik hub'larınıza erişmek için kullanılıyorsa, Azure Olay Hub'larını basitleştirmek ve güvence altına almak için Azure Key Vault ile birlikte Yönetilen Hizmet Kimliğini kullanın. Key Vault yumuşak silme etkin olduğundan emin olun.

Azure Etkin Dizini'nde (AD) otomatik olarak yönetilen bir kimlikle Azure hizmetlerine yönetilen kimlik sağlamak için Yönetilen Kimlikler'i kullanın. Yönetilen Kimlikler, kodunuzda herhangi bir kimlik belgesi olmadan Azure Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlere kimlik doğrulamanızı sağlar.

Etkinlik Hub'ları Kaynaklarına erişmek için Azure Etkin Dizini ile yönetilen bir kimliğin kimliğini doğrulatın:https://docs.microsoft.com/azure/event-hubs/authenticate-managed-identity?tabs=latest 

Etkinlik Hub'ları için müşteri tarafından yönetilen anahtarları yapılandırın:https://docs.microsoft.com/azure/event-hubs/configure-customer-managed-key 

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

**Sorumluluk**: Geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: İşlem yapmayan Azure kaynaklarına yüklenecek dosyaları önceden taraya

**Yönlendirme**: Azure Etkinlik Hub'ları, Uygulama Hizmeti, Veri Gölü Depolama, Blob Depolama, PostgreSQL için Azure Veritabanı gibi bilgi işlem dışı Azure kaynaklarına yüklenen tüm içerikleri önceden tarayın. Microsoft bu gibi durumlarda verilerinize erişemez.

Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda etkindir (örneğin, Redis için Azure Önbelleği), ancak müşteri içeriğinde çalışmaz.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Kötü amaçlı yazılımdan koruma yazılımının ve imzalarının güncelleştirilmesini sağlayın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Düzenli otomatik yedeklemeler sağlayın

**Yönerge**: Azure Olay Hub'ları için coğrafi durum kurtarmayı yapılandırın. Tüm Azure bölgeleri veya veri merkezleri (kullanılabilirlik bölgeleri kullanılıyorsa) kapalı kalma süresi yle karşılaştığında, veri işlemenin farklı bir bölgede veya veri merkezinde çalışmaya devam etmesi çok önemlidir. Bu nedenle, Jeo-felaket kurtarma ve Geo-çoğaltma herhangi bir kuruluş için önemli özelliklerdir. Azure Olay Hub'ları, ad alanı düzeyinde hem coğrafi felaket kurtarma yı hem de coğrafi çoğaltmayı destekler. 

Azure Etkinlik Hub'ları için coğrafi durum kurtarmayı anlayın:https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr#availability-zones-preview

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Komple sistem yedeklemelerini gerçekleştirin ve müşteri yönetilen anahtarları yedekleme

**Yönlendirme**: Azure Olay Hub'ları, Azure Depolama Hizmeti Şifrelemesi (Azure SSE) ile veri şifrelemesini sağlar. Olay Hub'ları verileri depolamak için Azure Depolama'ya güvenir ve varsayılan olarak Azure Depolama ile depolanan tüm veriler Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. Müşteri tarafından yönetilen anahtarları depolamak için Azure Key Vault kullanıyorsanız, Anahtarlarınızın düzenli otomatik yedeklemelerini sağlayın.

Aşağıdaki PowerShell komutu yla Key Vault Secrets düzenli otomatik yedekleme sağlamak: Backup-AzKeyVaultSecret

Azure Event Hub'ları verilerini yeniden şifrelemek için müşteri tarafından yönetilen anahtarlar nasıl yapılandırılabilen:https://docs.microsoft.com/azure/event-hubs/configure-customer-managed-key

Anahtar Vault Sırları yedeklemek için nasıl:https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultsecret

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Müşteri yönetilen anahtarlar dahil tüm yedeklemeleri doğrulayın

**Rehberlik**: Yedeklenen müşteri yönetilen anahtarlarının test restorasyonu.

 

Azure'da anahtar kasa sıtuşlarını geri yükleme:https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Yedeklerin ve müşteri yönetilen anahtarlarının korunmasını sağlayın

**Kılavuz**: Tuşları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Key Vault'ta yumuşak silme işlemini etkinleştirin. Azure Etkinlik Hub'ları, müşteri tarafından yönetilen anahtarların Yumuşak Silme ve Temizleme'yi yapılandırmak için gerekli.

Olay Hub'ları verilerini yakalamak için kullanılan Azure Depolama hesabı için yumuşak silme yapılandırması. Bu özelliğin Azure Veri Gölü Depolama Gen 2 için henüz desteklenmediğini unutmayın.

Key Vault'ta yumuşak silme nasıl etkinleştirilir:https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

Anahtarlarla bir anahtar kasası ayarlayın:https://docs.microsoft.com/azure/event-hubs/configure-customer-managed-key

Azure Depolama lekeleri için yumuşak silme:https://docs.microsoft.com//azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için [Güvenlik Denetimi: Olay Yanıtı'na](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response)bakın.*

### <a name="101-create-an-incident-response-guide"></a>10.1: Olay yanıt kılavuzu oluşturma

**Rehberlik**: Personelin rollerini ve olay işleme/yönetim aşamalarını tanımlayan yazılı olay yanıt planları olduğundan emin olun.

Azure Güvenlik Merkezi'nde İş Akışı Otomasyonları nasıl yapılandırılabilen:https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Olay puanlama ve öncelik lendirme prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, her uyarıya katılma sırasının önceliklerini belirlemenize yardımcı olmak için uyarılara önem verir, böylece bir kaynak tehlikeye girdiğinde, bu uyarıya hemen ulaşabilirsiniz. Önem derecesi, Güvenlik Merkezi'nin bulguda ne kadar emin olduğuna veya uyarıyı vermek için kullanılan analize ve uyarıya yol açan etkinliğin arkasında kötü amaçlı niyet olduğuna dair güven düzeyine dayanır.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10.3: Test Güvenliği Yanıt Prosedürleri

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

**Kılavuz**: Penetrasyon Testlerinizin Microsoft ilkelerini ihlal etmediğini sağlamak https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1için lütfen Microsoft Etkileşim Kuralları'nı izleyin: .
Microsoft'un stratejisi ve Red Teaming ve Microsoft yönetilen bulut altyapısı, hizmetleri ve uygulamalarına karşı canlı site penetrasyon testi nin uygulanması hakkında daha fazla bilgiyi burada bulabilirsiniz:https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Güvenlik Kıyaslama'ya](https://docs.microsoft.com/azure/security/benchmarks/overview) bakın
- [Azure Güvenlik Taban Çizgileri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
