---
title: Azure SQL Veritabanı için Azure Güvenlik Taban Çizgisi
description: Azure SQL Veritabanı için Azure Güvenlik Taban Çizgisi
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: d51cf22d7be167501927e54ce159e0b732209b0d
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81011387"
---
# <a name="azure-security-baseline-for-azure-sql-database"></a>Azure SQL Veritabanı için Azure Güvenlik Taban Çizgisi

Azure SQL Veritabanı için Azure Güvenlik Taban Çizgisi, dağıtımınızın güvenlik duruşunu geliştirmenize yardımcı olacak öneriler içerir.

Bu hizmetin temeli, en iyi uygulamalar kılavuzumuzla Azure'da bulut çözümlerinizi nasıl güvenebileceğinize ilişkin öneriler sunan [Azure Güvenlik Kıyaslama sürümü 1.0'dan](https://docs.microsoft.com/azure/security/benchmarks/overview)alınmıştır.

Daha fazla bilgi için [Azure Güvenlik Taban Çizgilerini genel olarak](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)görün.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için [Güvenlik Denetimi: Ağ Güvenliği'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)bakın.*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Sanal Ağınızdaki Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'nı kullanarak kaynakları koruyun

**Yönlendirme**: Azure PaaS Hizmetlerine (örneğin, SQL Veritabanı) ve Azure barındırılan müşteri/iş ortağı hizmetlerine sanal ağınızdaki Özel Bitiş Noktası üzerinden erişmelerine izin vermek için Azure Private Link'i etkinleştirebilirsiniz. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. 

Trafiğin Azure SQL Veritabanı'na erişmesine izin vermek için, Ağ Güvenlik Grupları üzerinden giden trafiğine izin vermek için SQL hizmet etiketlerini kullanın.

Sanal ağ kuralları, Azure SQL Veritabanı'nın yalnızca sanal ağ içindeki seçili alt ağlardan gönderilen iletişimleri kabul etmesini sağlar.

Azure SQL Veritabanı için Özel Bağlantı nasıl ayarlanır:

https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview#how-to-set-up-private-link-for-azure-sql-database

Veritabanı sunucuları için sanal ağ hizmeti uç noktaları ve kuralları nasıl kullanılır:

https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Vnet, Subnet ve NIC'lerin yapılandırmasını ve trafiğini izleyin ve kaydedin

**Kılavuz**: Azure SQL Veritabanı Sunucunuzun dağıtılan alt ağı için Azure Güvenlik Merkezi'ni ve düzeltici ağ koruma önerilerini kullanın. 

Azure SQL Veritabanı Sunucuörneğinize bağlanacak olan Azure Sanal Makineler (VM) için, Bu Sanal Verileri koruyan NSG'ler için ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve trafik denetimi için bir Azure Depolama Hesabına günlükler gönderin. 

Ayrıca NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzdaki trafik akışıhakkında öngörüler sağlamak için Trafik Analitiği'ni kullanabilirsiniz. Traffic Analytics'in bazı avantajları, ağ etkinliğini görselleştirme ve etkin noktaları belirleme, güvenlik tehditlerini belirleme, trafik akışı desenlerini anlama ve ağ yanlış yapılandırmalarını saptabilme yeteneğidir.

NSG Akış Günlükleri Nasıl Etkinleştirilir:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Azure Güvenlik Merkezi tarafından sağlanan Ağ Güvenliğini Anlayın:

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

Trafik Analizi ni etkinleştirme ve kullanma:

https://docs.microsoft.com/azure/network-watcher/traffic-analytics

Azure Güvenlik Merkezi tarafından sağlanan Ağ Güvenliğini Anlayın:

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="13-protect-critical-web-applications"></a>1.3: Kritik web uygulamalarını koruyun

**Rehberlik**: Geçerli değildir; Bu öneri, Azure Apps Hizmeti veya web uygulamalarını barındıran bilgi işlem kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Bilinen kötü amaçlı IP adresleriyle iletişimi reddetme

**Kılavuz**: Dağıtılmış hizmet reddi saldırılarına karşı koruma sağlamak için SQL Server örneklerinizle ilişkili Sanal Ağlarda DDoS Koruma Standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimi reddetmek için Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı kullanın.

DDoS koruması nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı Anlayın:

https://docs.microsoft.com/azure/security-center/security-center-alerts-data-services

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1.5: Ağ paketlerini ve akış günlüklerini kaydetme

**Yönlendirme**: Azure SQL Veritabanı örneğinize bağlanacak olan Azure Sanal Makineler (VM'ler) için, bu SANAL'ları koruyan NSG'ler için ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve trafik denetimi için bir Azure Depolama Hesabına günlükler gönderin. Anormal etkinliği araştırmak için gerekirse, Ağ İzleyicipaket yakalamayı etkinleştirin.

NSG Akış Günlükleri Nasıl Etkinleştirilir:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Ağ İzleyicisi nasıl etkinleştirilir:

https://docs.microsoft.com/azure/network-watcher/network-watcher-create

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Ağ tabanlı izinsiz giriş algılama/saldırı önleme sistemlerini (IDS/IPS) dağıtma

**Kılavuz**: Azure SQL Veritabanı için Gelişmiş Tehdit Koruması (ATP) etkinleştirin.  Kullanıcılar şüpheli veritabanı etkinlikleri, olası güvenlik açıkları ve SQL enjeksiyon saldırılarının yanı sıra anormal veritabanı erişimi ve sorgu desenleri hakkında bir uyarı alır. Gelişmiş Tehdit Koruması, uyarıları Azure Güvenlik Merkezi ile de entegre eder. 

Azure SQL Veritabanı için Gelişmiş Tehdit Koruması'nı anlama ve kullanma:https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Web uygulamalarına trafik yönetme

**Rehberlik**: Geçerli değildir; Bu öneri, Azure Apps Hizmeti veya web uygulamalarını barındıran bilgi işlem kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Ağ güvenliği kurallarının karmaşıklığını ve yönetim ek lerini en aza indirmek

**Kılavuz**: Ağ güvenlik gruplarında veya Azure Güvenlik Duvarı'ndaki ağ erişim denetimlerini tanımlamak için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında servis etiketi adını (örn. ApiManagement) belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz. Microsoft, hizmet etiketinin kapsadığı adres öneklerini yönetir ve adresler değiştikçe servis etiketini otomatik olarak güncelleştirir.

Azure SQL Veritabanı için hizmet bitiş noktaları kullanırken, Azure SQL Veritabanı Genel IP adreslerine giden ler gereklidir: Bağlantıya izin vermek için Ağ Güvenlik Gruplarının (NSG'ler) Azure SQL Veritabanı IP'lerine açılması gerekir. Bunu Azure SQL Veritabanı için NSG hizmet etiketlerini kullanarak yapabilirsiniz.

Azure SQL Veritabanı için Hizmet Bitiş Noktaları ile Hizmet Etiketlerini Anlayın:

https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview#limitations

Hizmet Etiketlerini anlama ve kullanma:

https://docs.microsoft.com/azure/virtual-network/service-tags-overview

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Ağ aygıtları için standart güvenlik yapılandırmalarını koruyun

**Yönerge**: Azure İlkesi ile Azure SQL Veritabanı sunucu örnekleriniz için ağ güvenliği yapılandırmalarını tanımlayın ve uygulayın. Özel ilke tanımlarını tanımlamak için "Microsoft.Sql" ad alanını veya Azure SQL Veritabanı sunucu ağı koruması için tasarlanmış yerleşik ilke tanımlarından herhangi birini kullanabilirsiniz. Azure SQL Veritabanı sunucusu için geçerli bir yerleşik ağ güvenliği ilkesine örnek olarak şu örnek: "SQL Server sanal ağ hizmeti bitiş noktası kullanmalıdır".

 

Azure Kaynak Yönetimi şablonları, Rol tabanlı erişim denetimi (RBAC) ve ilkeler gibi önemli ortam yapılarını tek bir plan tanımında paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure Planları'nı kullanın. Planı yeni aboneliklere ve ortamlara kolayca uygulayın ve sürüm leme yoluyla denetim ve yönetime ince ayar uygulayın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Blueprint nasıl oluşturulur:https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="110-document-traffic-configuration-rules"></a>1.10: Belge trafiği yapılandırma kuralları

**Kılavuz**: Ağ güvenliği grupları (NSG) ve ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için etiketleri kullanın. Tek tek NSG kuralları için, bir ağa/aktan trafiğin alınmasına izin veren kurallar için iş gereksinimini ve/veya süresini (vb.) belirtmek için "Açıklama" alanını kullanın.

Tüm kaynakların etiketlerle oluşturulduğundan emin olmak ve varolan etiketlenmemiş kaynakları size bildirmek için etiketlemeyle ilgili "Etiket ve değeri gerektir" gibi yerleşik Azure ilke tanımlarından herhangi birini kullanın.

Azure PowerShell veya Azure CLI'yi, etiketlerine dayalı kaynaklara bakmak veya eylemler gerçekleştirmek için kullanabilirsiniz.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Ağ kaynağı yapılandırmalarını izlemek ve değişiklikleri algılamak için otomatik araçlar kullanın

**Yönerge**: Ağ kaynak yapılandırmalarını izlemek ve Azure SQL Veritabanı sunucu örneklerinizle ilgili ağ kaynaklarındaki değişiklikleri algılamak için Azure Etkinlik Günlüğü'ne kullanın. Azure Monitor'da kritik ağ kaynaklarında değişiklikler olduğunda tetikleyecek uyarılar oluşturun.

Azure Etkinlik Günlüğü etkinliklerini görüntüleme ve alma:

https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

Azure Monitor'da uyarı oluşturma:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için [Güvenlik Denetimi: Günlüğe kaydetme ve izleme.](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Onaylanmış zaman senkronizasyon kaynaklarını kullanma

**Kılavuz**: Microsoft, Azure kaynakları için zaman kaynakları tutar. Bilgi işlem dağıtımlarınız için zaman eşitlemesi güncelleştirebilirsiniz.

Azure bilgi işlem kaynakları için zaman eşitleme yapılandırması nasıl yapılandırılır:

https://docs.microsoft.com/azure/virtual-machines/windows/time-sync

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Yönerge**: Azure Depolama Hesabınızdaki, Günlük Analizi çalışma alanınızda veya Etkinlik Hub'larınızda veritabanı olaylarını izlemek ve bunları bir denetim günlüğüne yazmak için Azure SQL Veritabanı için denetimi etkinleştirin.

Ayrıca, Azure SQL tanılama telemetrisini Azure SQL veritabanlarının, esnek havuzların ve yönetilen örneklerin performansını ölçekte ve birden çok abonelik te izleyen bir bulut çözümü olan Azure SQL Analytics'e aktarabilirsiniz. Azure SQL Veritabanı performans ölçümlerini toplamanıza ve görselleştirmenize yardımcı olabilir ve performans sorun giderme için yerleşik zekaya sahiptir.

Azure SQL Veritabanınız için denetim kurulumu nasıl yapılır:

https://docs.microsoft.com/azure/sql-database/sql-database-auditing

Azure Monitor ile platform günlükleri ve ölçümleri nasıl toplanır:

https://docs.microsoft.com/azure/sql-database/sql-database-metrics-diag-logging

Tanılamaları Azure SQL Analytics'e nasıl aktarın:

https://docs.microsoft.com/azure/sql-database/sql-database-metrics-diag-logging#stream-into-azure-sql-analytics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure kaynakları için denetim günlüğe kaydetmeyi etkinleştirme

**Yönerge**: Azure SQL Veritabanı sunucu örneğinde denetimi etkinleştirin ve denetim günlükleri (Azure Depolama, Günlük Analizi veya Olay Hub'ı) için bir depolama konumu seçin.

Azure SQL Server için denetimi etkinleştirme:

https://docs.microsoft.com/azure/sql-database/sql-database-auditing

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: İşletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: Geçerli değildir; bu kıyaslama hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2.5: Güvenlik günlüğü depolama tutma yapılandırma

**Kılavuz**: Azure SQL Veritabanı günlüklerinizi bir Log Analytics Çalışma Alanında depolarken, günlük tutma süresini kuruluşunuzun uyumluluk yönetmeliklerine göre ayarlayın.

Günlük bekletme parametreleri nasıl ayarlanır:

https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2.6: Günlükleri izleme ve inceleme

**Rehberlik**: Anormal davranışlar için günlükleri analiz edin ve izleyin ve sonuçları düzenli olarak gözden geçirin. Azure SQL Veritabanı örneğinizle ilgili olağan dışı etkinlikleri uyarmak için Azure Güvenlik Merkezi'nin Gelişmiş Tehdit Koruması'nı kullanın. Alternatif olarak, uyarıları Azure SQL Veritabanı örneklerinizle ilgili Metrik Değerler'e veya Azure Etkinlik Günlüğü girişlerine göre yapılandırın.

Azure SQL Server için Gelişmiş Tehdit Koruması ve uyarıyı anlayın:

https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview

Azure SQL Veritabanı için özel uyarılar nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/sql-database/sql-database-insights-alerts-portal?view=azps-1.4.0

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Anormal aktivite için uyarıları etkinleştirme

**Kılavuz**: Anormal etkinlikleri izlemek ve uyarmak için Azure SQL Veritabanları için Azure Güvenlik Merkezi Gelişmiş Tehdit Koruması'nı kullanın. SQL Veritabanlarınız için Gelişmiş Veri Güvenliği'ni etkinleştirin. Gelişmiş Veri Güvenliği, hassas verileri bulma ve sınıflandırma, olası veritabanı güvenlik açıklarını yüzeye çıkarma ve azaltma ve veritabanınız için bir tehdit oluşturabilecek anormal etkinlikleri algılama işlevlerini içerir.

Azure SQL Veritabanı için Gelişmiş Tehdit Koruması ve uyarıyı anlayın:

https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview

Azure SQL Veritabanı için Gelişmiş Veri Güvenliği nasıl etkinleştirilir:

https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security

Azure Güvenlik Merkezi'nde uyarılar nasıl yönetilir:

https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="28-centralize-anti-malware-logging"></a>2.8: Kötü amaçlı yazılımdan koruma günlüğe kaydetmeyi merkezileştirin

**Rehberlik**: Geçerli değildir; Azure SQL Server için kötü amaçlı yazılımdan koruma çözümü Microsoft tarafından temel platformda yönetilir.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="29-enable-dns-query-logging"></a>2.9: DNS sorgu günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: Geçerli değildir; DNS günlüğe kaydetme Azure SQL Server için geçerli değildir.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2.10: Komut satırı denetim günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: Geçerli değildir; komut satırı denetimi Azure SQL Server için geçerli değildir.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için [güvenlik denetimi: kimlik ve erişim denetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)bakın.*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: İdari hesapların envanterini korumak

**Yol gösterici**: Azure Active Directory (AAD), açıkça atanması gereken ve sorgulanabilen yerleşik rollere sahiptir. Yönetim gruplarına üye hesapları bulmak için geçici sorgular gerçekleştirmek için AAD PowerShell modüllerini kullanın.

PowerShell ile Azure AD'de dizin rolü nasıl elde elabilirsiniz:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0

PowerShell ile Azure AD'de dizin rolünün üyelerini nasıl edinire bilgili olabilirsiniz:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Varsayılan parolaları varsa değiştirme

**Yol gösterici**: Azure Active Directory varsayılan parola kavramına sahip değildir. Bir Azure SQL Veritabanı örneği sağlarken, kimlik doğrulamayı Azure Etkin Dizini ile tümleştirmeyi seçmeniz önerilir.

Azure Active Directory kimlik doğrulamasını Azure SQL ile yapılandırma ve yönetme:

https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Özel yönetim hesaplarını kullanma

**Rehberlik**: Özel yönetim hesaplarının kullanımına ilişkin ilke ve prosedürler oluşturun. Yönetim hesabı sayısını izlemek için Azure Güvenlik Merkezi Kimliği ve Erişim Yönetimi'ni kullanın.

Azure Güvenlik Merkezi Kimliğini ve Erişimini Anlayın:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Azure Active Directory ile tek oturum açma (SSO) kullanma

**Rehberlik**: Geçerli değildir; Azure Active Directory Authentication'ı Azure SQL Server ile tümleştirmek üzere yapılandırabilirsiniz, ancak tek oturum açma desteklenmez.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Tüm Azure Active Directory tabanlı erişim için çok faktörlü kimlik doğrulamayı kullanın

**Kılavuz**: Azure Etkin Dizin (AAD) Çok Faktörlü Kimlik Doğrulaması'nı (MFA) etkinleştirin ve Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerini izleyin.

Azure'da MFA nasıl etkinleştirilir:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Azure Güvenlik Merkezi'nde kimlik ve erişim nasıl izlenir:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Tüm idari görevler için özel makineleri (Ayrıcalıklı Erişim İş İstasyonları) kullanın

**Kılavuz :** Azure kaynaklarına giriş yapmak ve yapılandırmak üzere yapılandırılan Çok Faktörlü Kimlik Doğrulama MFA'sı olan Ayrıcalıklı Erişim İş İstasyonu (PAW) kullanın.

Ayrıcalıklı Erişim İş İstasyonları hakkında bilgi edinin:

https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure'da MFA nasıl etkinleştirilir:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: İdari hesaplardan şüpheli etkinliklere ilişkin günlük ve uyarı

**Yönerge**: Çevrede şüpheli veya güvensiz etkinlik oluştuğunda günlük ve uyarı oluşturma için Azure Active Directory güvenlik raporlarını kullanın.

Veritabanlarına erişmek veya veritabanlarından yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılamak için Azure SQL Veritabanı için Gelişmiş Tehdit Koruması'nı kullanın.

Riskli etkinlikler için işaretlenen Azure AD kullanıcılarını nasıl tanımlarım:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk

Azure Güvenlik Merkezi'nde kullanıcıların kimlik ve erişim etkinliğini izleme:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

Gelişmiş Tehdit Koruması ve olası uyarıları gözden geçirin:

https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview#advanced-threat-protection-alerts


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Yönerge**: Portal ve Azure Kaynak Yönetimi'nin yalnızca IP adresi aralıklarının veya ülkelerinin/bölgelerinin belirli mantıksal gruplandırmalarından erişmesine izin vermek için Koşullu Erişim Adlandırılmış Konumları kullanın.

Azure'da Adlandırılmış Konumlar nasıl yapılandırılabilen:https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="39-use-azure-active-directory"></a>3.9: Azure Etkin Dizini Kullanma

**Kılavuz**: Azure SQL Veritabanı sunucu örnekleriniz için bir Azure Active Directory (AAD) yöneticisi oluşturun.

Azure Active Directory kimlik doğrulamasını Azure SQL ile yapılandırma ve yönetme:

https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure

AAD örneğini oluşturma ve yapılandırma:

https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Kullanıcı erişimini düzenli olarak gözden geçirin ve uzlaştırın

**Rehberlik**: Azure Active Directory (AAD), eski hesapları keşfetmeye yardımcı olmak için günlükler sağlar. Ayrıca, grup üyeliklerini, kurumsal uygulamalara erişimi ve rol atamalarını verimli bir şekilde yönetmek için Azure Kimlik erişim gözden incelemelerini kullanın. Kullanıcıların erişimi, yalnızca doğru kullanıcıların sürekli erişime sahip olduğundan emin olmak için düzenli olarak gözden geçirilebilir.

Azure Kimlik Erişim İncelemeleri nasıl kullanılır:

https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Devre dışı bırakılmış hesaplara erişim denemelerini izleme

**Kılavuz :** Azure Active Directory (AAD) kimlik doğrulamasını Azure SQL ile yapılandırın ve Azure Active Directory kullanıcı hesapları için Tanılama Ayarları oluşturarak denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına gönderin. Log Analytics çalışma alanında istenen Uyarıları yapılandırın.

Azure Active Directory kimlik doğrulamasını Azure SQL ile yapılandırma ve yönetme:

https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure

Azure Etkinlik Günlükleri Azure Monitör'e nasıl entegre eleştirilir:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Hesap giriş davranış sapması konusunda uyarı

**Kılavuz :** Kullanıcı kimlikleri ile ilgili algılanan şüpheli eylemlere otomatik yanıtları yapılandırmak için Azure Active Directory (AAD) Kimlik Koruması ve risk algılamalarını kullanın. Ayrıca, daha fazla araştırma için Azure Sentinel'e veri sindirebilirsiniz.

Azure AD risk oturum açmaları nasıl görüntülenedersiniz:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Kimlik Koruması risk ilkelerinin nasıl yapılandırılabilen ve etkinleştirilir:

https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Destek senaryoları sırasında Microsoft'a ilgili müşteri verilerine erişim sağlama

**Kılavuz**: Microsoft'un müşteri verilerine erişmesi gereken destek senaryolarında Azure Müşteri Kilit Kutusu, müşterilerin müşteri veri erişim isteklerini gözden geçirmesi ve onaylaması veya reddetmesi için bir arabirim sağlar.

Müşteri Kilit Kutusunu Anlayın:

https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için [güvenlik denetimi: veri koruma.](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Hassas Bilgilerin envanterini koruyun

**Yönerge**: Hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olmak için etiketleri kullanın.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Hassas bilgileri depolama veya işleme sistemlerini yalıtma

**Rehberlik**: Geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Kaynaklar Vnet/Subnet tarafından ayrılmalı, uygun şekilde etiketlenmelidir ve bir NSG veya Azure Güvenlik Duvarı içinde güvenli hale edilmelidir. Hassas verileri depolayan veya işleyen kaynaklar yalıtılmalıdır. Özel Bağlantı kullanın; Azure SQL Server'ı Vnet'inizin içine dağıtın ve Özel Uç Noktaları'nı kullanarak özel olarak bağlanın.

Ek Azure abonelikleri oluşturma:

https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları nasıl oluşturulur:

https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler nasıl oluşturulur ve kullanılır:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Azure SQL Veritabanı için Özel Bağlantı nasıl ayarlanır:

https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview#how-to-set-up-private-link-for-azure-sql-database

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Hassas bilgilerin yetkisiz aktarımının izlenmesi ve engellenmesi

**Kılavuz**: Azure SQL Veritabanları için hassas bilgileri depolamaveya işleme, Etiketler'i kullanarak veritabanını ve ilgili kaynakları hassas olarak işaretleyin. Azure SQL Veritabanı örneklerinizde Ağ Güvenliği Grubu Hizmet Etiketleri ile birlikte özel bağlantı yapılandırarak hassas bilgilerin sızmasını önleyin.

Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

Azure SQL Veritabanı örneklerinizde veri sızmasını önlemek için Özel Bağlantı ve NSG'ler nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview

Azure'da müşteri verilerinin korunmasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Tüm hassas bilgileri taşıma sırasında şifreleme

**Kılavuz**: Azure SQL Veritabanı, Transport Layer Security ile verilerinizi hareket halindeşifreleyerek verilerinizi güvence altına alar. SQL Server tüm bağlantılar için her zaman şifreleme (SSL/TLS) uygular. Bu, bağlantı dizesinde Şifreveya TrustServerCertificate ayarına bakılmaksızın istemci ve sunucu arasında tüm verilerin "aktarım halinde" şifrelenmesini sağlar.

Aktarımsırasında Azure SQL Şifrelemesini Anlayın:

https://docs.microsoft.com/azure/sql-database/sql-database-security-overview#information-protection-and-encryption

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Hassas verileri tanımlamak için etkin bir bulma aracı kullanın

**Yönerge**: Azure SQL Veritabanı veri bulma ve sınıflandırma özelliğini kullanın. Veri bulma ve sınıflandırma, veritabanlarınızdaki hassas verileri keşfetmek, sınıflandırmak, &amp; etiketlemek için Azure SQL Veritabanı'nda yerleşik gelişmiş özellikler sağlar.

Azure SQL Server için veri bulma ve sınıflandırma nasıl kullanılır:

https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Kaynaklara erişimi denetlemek için Azure RBAC'ı kullanın

**Yönerge**: Azure SQL Veritabanı örneklerine erişimi doğrulamak ve denetlemek için Azure Active Directory 'i (AAD) kullanın.

Azure SQL Server'ı kimlik doğrulama için Azure Active Directory ile tümleştirme:

https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication

Azure SQL Server'da erişim nasıl denetler:

https://docs.microsoft.com/azure/sql-database/sql-database-control-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Erişim denetimini uygulamak için ana bilgisayar tabanlı veri kaybı önlemeyi kullanın

**Kılavuz**: Microsoft, Azure SQL Veritabanı'nın altında yatan altyapıyı yönetir ve müşteri verilerinin kaybolmasını veya açığa dökülmesini önlemek için sıkı denetimler uygulamıştır.

Azure'da müşteri verilerinin korunmasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Hassas bilgileri istirahatte şifreleme

**Yönlendirme**: Saydam veri şifreleme (TDE), verileri istirahatte şifreleyerek Azure SQL Veritabanı, Azure SQL yönetilen örneği ve Azure Veri Ambarı'nın kötü amaçlı çevrimdışı etkinlik tehdidine karşı korunmasına yardımcı olur. Bu özellik bütün bir veritabanı, yedekleri ve işlem günlüğü dosyaları için gerçek zamanlı şifreleme ve şifre çözme işlemlerini gerçekleştirir ve uygulamada değişiklik yapmayı gerektirmez. TDE varsayılan olarak yeni dağıtılan tüm Azure SQL veritabanlarında etkindir. TDE şifreleme anahtarı Microsoft veya müşteri tarafından yönetilebilir.

Saydam veri şifrelemeyi nasıl yönetebilirsiniz ve kendi şifreleme anahtarlarınızı kullanma:

https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?tabs=azure-portal#manage-transparent-data-encryption

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Kritik Azure kaynaklarında yapılan değişiklikleri kaydedin ve uyarı

**Yönerge**: Azure SQL Veritabanlarının ve diğer kritik veya ilgili kaynakların üretim örneklerinde değişikliklerin ne zaman yapılacağına ilişkin uyarılar oluşturmak için Azure Etkinlik Günlüğü ile Azure Monitörünü kullanın.

Azure Etkinlik Günlüğü etkinlikleri için uyarılar oluşturma:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log


**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için [Güvenlik Denetimi: Güvenlik Açığı Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)bakın.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Otomatik güvenlik açığı tarama araçlarını çalıştırın

**Yönerge**: Azure SQL Veritabanı için Gelişmiş Veri Güvenliği'ni etkinleştirin ve Azure SQL Sunucularınızda güvenlik açığı değerlendirmeleri gerçekleştirme konusunda Azure Güvenlik Merkezi'nin önerilerine uyun.

Azure SQL Veritabanlarınızda güvenlik açığı değerlendirmeleri nasıl çalıştırılanızır:

https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment

Gelişmiş Veri Güvenliği nasıl etkinleştirilir:

https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security

Azure Güvenlik Merkezi güvenlik açığı değerlendirme önerileri nasıl uygulanır:

https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Otomatik işletim sistemi yama yönetimi çözümlerini dağıtma

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3: Otomatik üçüncü taraf yazılım yama yönetimi çözümlerini dağıtma

**Rehberlik**: Geçerli değildir; bu kıyaslama hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Arka arkaya güvenlik açığı taramalarını karşılaştırın

**Kılavuz**: Azure SQL Veritabanı örnekleriniz için periyodik olarak yinelenen taramaları etkinleştirin; bu, haftada bir kez veritabanınızda otomatik olarak bir tarayın çalıştırmak için bir güvenlik açığı değerlendirmesi yapılandıracaktır. Bir tarama sonucu özeti sağladığınız e-posta adresine(es) gönderilecektir. Güvenlik açıklarının düzeltildiğini doğrulamak için sonuçları karşılaştırın.

Azure Güvenlik Merkezi'nde güvenlik açığı değerlendirme raporu nasıl dışa aktarılabilen:

https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment#implementing-vulnerability-assessment

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Keşfedilen güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Yönerge**: Azure Güvenlik Merkezi tarafından sağlanan varsayılan risk derecelendirmelerini (Güvenli Puan) kullanın.

Azure Güvenlik Merkezi Güvenli Puanı Anlayın:

https://docs.microsoft.com/azure/security-center/security-center-secure-score

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için [güvenlik denetimi: Envanter ve Varlık Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)bakın.*

### <a name="61-use-azure-asset-discovery"></a>6.1: Azure Varlık Bulma'yı kullanma

**Yönerge**: Aboneliğinizdeki tüm kaynakları (Azure SQL Server örnekleri dahil) sorgulamak ve keşfetmek için Azure Kaynak Grafiği'ni kullanın.  Kiracınızda uygun (okuma) izinlere sahip olduğundan ve aboneliklerinizdeki tüm Azure aboneliklerinin yanı sıra kaynakları sıralayabildiğinizden emin olun.

Kaynak Grafiği aracılığıyla klasik Azure kaynakları keşfedilebilir, ancak ileriye dönük Olarak Azure Kaynak Yöneticisi kaynaklarının oluşturulması ve kullanılması önerilir.

Azure Graph ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure Aboneliklerinizi görüntüleme:https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure RBAC'ı anlayın:https://docs.microsoft.com/azure/role-based-access-control/overview


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="62-maintain-asset-metadata"></a>6.2: Varlık meta verilerini koruma

**Yönerge**: Meta verileri mantıksal olarak taksonomi olarak düzenlemek için meta veriler veren Azure kaynaklarına etiketler uygulayın.

Etiketler nasıl oluşturulur ve kullanılır:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Yetkisiz Azure kaynaklarını silme

**Rehberlik**: Varlıkları düzenlemek ve izlemek için etiketlemeyi, yönetim gruplarını ve uygun olduğu durumlarda ayrı abonelikleri kullanın. Envanteri düzenli olarak uzlaştırın ve yetkisiz kaynakların abonelikten zamanında silindiğinden emin olun.

Ek Azure abonelikleri oluşturma:

https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları nasıl oluşturulur:

https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler nasıl oluşturulur ve kullanılır:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: Onaylanan Azure kaynaklarının ve yazılım başlıklarının envanterini koruyun

**Yönerge**: İşlem kaynaklarınız için onaylanmış Azure kaynaklarının ve onaylı yazılımların listesini tanımlayın

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Onaylanmamış Azure kaynakları için izleme

**Yönerge**: Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure Politikası'nı kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Aboneliğinizdeki kaynakları sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Bilgi işlem kaynakları içindeki onaylanmamış yazılım uygulamalarını izleme

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="68-use-only-approved-applications"></a>6.8: Yalnızca onaylı uygulamaları kullanın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6.9: Yalnızca onaylanmış Azure hizmetlerini kullanma

**Yönerge**: Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure İlkesi'ni kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Aboneliğinizdeki kaynakları sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlkesi ile belirli bir kaynak türü nasıl reddedilecek:https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="610-implement-approved-application-list"></a>6.10: Onaylı başvuru listesini uygulama

**Rehberlik**: Geçerli değildir; bu öneri, bilgi işlem kaynakları üzerinde çalışan uygulamalar için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6.11: Kullanıcıların komut dosyaları aracılığıyla Azure Kaynakları Yöneticisi ile etkileşim deşmelerini sınırlandırın

**Yönerge**: "Microsoft Azure Yönetimi" Uygulaması için "Erişimi engelle" yapılandırmayı yaparak kullanıcıların Azure Kaynak Yöneticisi ile etkileşim edebilmelerini sınırlamak için Azure Koşullu Erişim'i kullanın.

Koşullu Erişim'i Azure Kaynak Yöneticisi'ne erişimi engellemek için yapılandırma:https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Kullanıcıların bilgi işlem kaynakları içinde komut dosyalarını yürütme yeteneğini sınırlama

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Yüksek riskli uygulamaları fiziksel veya mantıksal olarak ayırmak

**Rehberlik**: Geçerli değildir; Bu öneri, Uygulama Hizmeti veya masaüstü veya web uygulamalarını barındıran bilgi işlem kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için güvenlik [denetimi: Güvenli Yapılandırma'ya](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration)bakın.*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Tüm Azure kaynakları için güvenli yapılandırmalar oluşturma

**Yönerge**: Tüm Azure Kaynakları için güvenlik yapılandırmalarını korumak için Azure SQL Sunucuları/Veritabanları için Azure İlkesi veya Azure Güvenlik Merkezi önerilerini kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Güvenli işletim sistemi yapılandırmaları oluşturma

**Rehberlik**: Geçerli değildir; bu tavsiye, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Güvenli Azure kaynak yapılandırmalarını koruyun

**Yol gösterici**: Azure kaynaklarınız arasında güvenli ayarlar uygulamak için Azure ilkesini [reddet] ve [varsa dağıt] kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlke Efektlerini Anlama:

https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Güvenli işletim sistemi yapılandırmalarını koruyun

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure kaynaklarının yapılandırmayı güvenli bir şekilde depolama

**Yönlendirme**: Özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure DevOps veya Azure Repos'unu kullanın.

Azure DevOps'lerde kod depolama:

https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Depobelgeleri:

https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Özel işletim sistemi görüntülerini güvenli bir şekilde saklayın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7.7: Sistem yapılandırma yönetim araçlarını dağıtma

**Yönerge**: Sistem yapılandırmalarını uyarmak, denetlemek ve zorlamak için özel ilkeler oluşturmak için "Microsoft.SQL" ad alanında Azure İlkesi takma adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve ardışık hatlar geliştirin.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: İşletim sistemleri için sistem yapılandırma yönetim araçlarını dağıtma

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Azure hizmetleri için otomatik yapılandırma izleme uygulaması

**Kılavuz**: Azure SQL Sunucularınız ve Veritabanlarınız için temel taramayapmak için Azure Güvenlik Merkezi'nden yararlanın.

Azure Güvenlik Merkezi'nde öneriler nasıl düzeltilir:

https://docs.microsoft.com/azure/security-center/security-center-sql-service-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: İşletim sistemleri için otomatik yapılandırma izleme

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7.11: Azure sırlarını güvenli bir şekilde yönetme

**Yönlendirme**: Azure SQL Veritabanı Saydam Veri Şifreleme (TDE) için şifreleme anahtarlarını depolamak için Azure Key Vault'u kullanın.

Azure SQL Server'da depolanan hassas verileri koruma ve şifreleme anahtarlarını Azure Key Vault'ta depolama:

https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Kimlikleri güvenli ve otomatik olarak yönetme

**Kılavuz :** Azure hizmetlerine Azure Etkin Dizini'nde (AAD) otomatik olarak yönetilen bir kimlik sağlamak için Yönetilen Kimlikler'i kullanın. Yönetilen Kimlikler, kodunuzda herhangi bir kimlik belgesi olmadan Azure Key Vault da dahil olmak üzere AAD kimlik doğrulamasını destekleyen tüm hizmetlere kimlik doğrulamanızı sağlar.

Öğretici: Azure SQL'e erişmek için Windows VM sistem tarafından atanmış yönetilen bir kimlik kullanın:

https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-sql

Yönetilen Kimlikler nasıl yapılandırılır:

https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: İstenmeden kimlik bilgisi maruziyetini ortadan kaldırın

**Yönerge**: Kodunuzdaki kimlik bilgilerini tanımlamak için Kimlik Bilgisi Tarayıcısı'nı uygulayın. Kimlik Bilgisi Tarayıcısı, keşfedilen kimlik bilgilerinin Azure Anahtar Kasası gibi daha güvenli konumlara taşınmasını da teşvik edecektir. 

Kimlik Bilgisi Tarayıcı nasıl kurulturur:https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için [Güvenlik Kontrolü: Malware Defense](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense)' e bakın.*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımlarını kullanın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır. Microsoft, altta yatan platform için kötü amaçlı yazılımdan koruma yı işler.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: İşlem yapmayan Azure kaynaklarına yüklenecek dosyaları önceden taraya

**Yönlendirme**: Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure Uygulama Hizmeti) Microsoft kötü amaçlı yazılımdan koruma etkindir, ancak müşteri içeriğinde çalışmaz.

Uygulama Hizmeti, Veri Gölü Depolama, Blob Depolama, Azure SQL Server gibi bilgi işlem dışı Azure kaynaklarına yüklenen tüm içerikleri önceden tarayabilirsiniz. Microsoft bu gibi durumlarda verilerinize erişemez.

Azure Bulut Hizmetleri ve Sanal Makineler için Microsoft Kötü Amaçlı Yazılımdan Koruma'yı anlayın:https://docs.microsoft.com/azure/security/fundamentals/antimalware

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Kötü amaçlı yazılımdan koruma yazılımının ve imzalarının güncelleştirilmesini sağlayın

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır. Microsoft, altta yatan platform için kötü amaçlı yazılımdan koruma yı işler.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Düzenli otomatik yedeklemeler sağlayın

**Kılavuz :** İşletmenizi veri kaybına karşı korumak için Azure SQL Veritabanı, haftalık tam veritabanı yedeklemeleri, her 12 saatte bir diferansiyel veritabanı yedeklemeleri ve her 5 -10 dakikada bir işlem günlüğü yedeklemeleri oluşturur. Yedekler, tüm hizmet katmanları için en az 7 gün boyunca RA-GRS depolama alanında saklanır. Temel destek dışındaki tüm hizmet katmanları, 35 güne kadar zaman içinde geri yükleme için yapılandırılabilir yedekleme bekletme süresini destekler.

Farklı uyumluluk gereksinimlerini karşılamak için, haftalık, aylık ve/veya yıllık yedeklemeler için farklı bekletme dönemleri seçebilirsiniz. Depolama tüketimi, yedeklemelerin seçili sıklığına ve bekletme süresine bağlıdır.

Azure SQL Server ile yedeklemeleri ve iş sürekliliğini anlayın:

https://docs.microsoft.com/azure/sql-database/sql-database-business-continuity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Paylaşılan

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Komple sistem yedeklemelerini gerçekleştirin ve müşteri yönetilen anahtarları yedekleme

**Yönlendirme**: Azure SQL Veritabanı, 7 ile 35 gün arasında tutulan veritabanı yedeklemelerini otomatik olarak oluşturur ve veri merkezi kullanılamamış olsa bile bunların korunduğundan emin olmak için Azure okuma-erişim coğrafi yedekli depolama (RA-GRS) kullanır. Bu yedeklemeler otomatik olarak oluşturulur. Gerekirse, Azure SQL Veritabanlarınız için uzun vadeli coğrafi yedekli yedeklemeleri etkinleştirin.

Saydam Veri Şifreleme için müşteri tarafından yönetilen anahtarları kullanıyorsanız, anahtarlarınızın yedeklendiğini sağlayın.

Azure SQL Server'daki yedeklemeleri anlayın:

https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups?tabs=single-database

Azure'da anahtar kasa sıyrık anahtarlarını yedekleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Müşteri yönetilen anahtarlar dahil tüm yedeklemeleri doğrulayın

**Yönerge**: Azure Yedekleme'deki içeriğin düzenli olarak veri geri yüklenmesini gerçekleştirebilme özelliğini sağlayın. Gerekirse, içeriği yalıtılmış bir VLAN'a geri yüklemeyi test edin. Yedeklenen müşteri tarafından yönetilen anahtarların test geri yüklemesi.

Azure'da anahtar kasa sıtuşlarını geri yükleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

Zaman içinde geri yüklemeyi kullanarak Azure SQL Veritabanı yedeklemelerini nasıl kurtarabilirsiniz:

https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups#point-in-time-restore

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Yedeklerin ve müşteri yönetilen anahtarlarının korunmasını sağlayın

**Yönerge**: Tuşları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Azure Key Vault'ta yumuşak silme işlemini etkinleştirin.

Key Vault'ta yumuşak silme nasıl etkinleştirilir:

https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için [Güvenlik Denetimi: Olay Yanıtı'na](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response)bakın.*

### <a name="101-create-an-incident-response-guide"></a>10.1: Olay yanıt kılavuzu oluşturma

**Rehberlik**: Personelin rollerini ve olay işleme/yönetim aşamalarını tanımlayan yazılı olay yanıt planları olduğundan emin olun.

Azure Güvenlik Merkezi'nde İş Akışı Otomasyonları nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Olay puanlama ve öncelik lendirme prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, her uyarıya katılma sırasının önceliklerini belirlemenize yardımcı olmak için uyarılara önem verir, böylece bir kaynak tehlikeye girdiğinde, bu uyarıya hemen ulaşabilirsiniz. Önem derecesi, Güvenlik Merkezi'nin bulguda ne kadar emin olduğuna veya uyarıyı vermek için kullanılan analize ve uyarıya yol açan etkinliğin arkasında kötü amaçlı niyet olduğuna dair güven düzeyine dayanır.

Azure Güvenlik Merkezi'nde güvenlik uyarıları:https://docs.microsoft.com/azure/security-center/security-center-alerts-overview



**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10.3: Test güvenlik yanıt yordamları

**Rehberlik**: Sistemlerinizin olay yanıt yeteneklerini düzenli olarak test etmek için alıştırmalar yapın. Zayıf noktaları ve boşlukları belirleyin ve planı gerektiği gibi gözden geçirin.

NIST'in yayınına bakabilirsiniz: BT Planları ve Yetenekleri için Test, Eğitim ve Egzersiz Programları Kılavuzu:

https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Güvenlik olayı iletişim bilgilerini sağlayın ve güvenlik olayları için uyarı bildirimlerini yapılandırma

**Kılavuz :** Microsoft Güvenlik Yanıt Merkezi (MSRC) verilerinize yasadışı veya yetkisiz bir taraf tarafından erişildiğini tespit ederse, güvenlik olayı iletişim bilgileri Microsoft tarafından sizinle iletişim kurmak için kullanılır.

Azure Güvenlik Merkezi Güvenlik Kişisi nasıl ayarlanır:

https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Olay yanıt sisteminize güvenlik uyarıları dahil edin

**Kılavuz**: Sürekli Dışa Aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışa aktarın. Sürekli Dışa Aktarma, uyarı ve önerileri el ile veya sürekli olarak dışa aktarmanıza olanak tanır. Uyarıları Sentinel'e aktarmak için Azure Güvenlik Merkezi veri konektörünü kullanabilirsiniz.

Sürekli dışa aktarma nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/security-center/continuous-export

Uyarıları Azure Sentinel'e nasıl aktarın:

https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Güvenlik uyarılarına yanıtı otomatikleştirin

**Yönerge**: Güvenlik uyarıları ve önerileri nde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek için Azure Güvenlik Merkezi'ndeki İş Akışı Otomasyonu özelliğini kullanın.

İş Akışı Otomasyonu ve Mantık Uygulamaları nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için [Güvenlik Kontrolü: Penetrasyon Testleri ve Kırmızı Takım Egzersizleri'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises)bakın.*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11.1: Azure kaynaklarınızı düzenli olarak delme testini gerçekleştirin ve tüm kritik güvenlik bulgularının 60 gün içinde düzeltilmesini sağlayın

**Kılavuz**: Penetrasyon Testlerinizin Microsoft ilkelerini ihlal etmediğini sağlamak için lütfen Microsoft Etkileşim Kuralları'na uyun:

https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1.

Microsoft'un stratejisi ve Red Teaming ve Microsoft yönetilen bulut altyapısı, hizmetleri ve uygulamalarına karşı canlı site penetrasyon testi nin uygulanması hakkında daha fazla bilgiyi burada bulabilirsiniz:https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Güvenlik Kıyaslama'ya](https://docs.microsoft.com/azure/security/benchmarks/overview) bakın
- [Azure Güvenlik Taban Çizgileri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
