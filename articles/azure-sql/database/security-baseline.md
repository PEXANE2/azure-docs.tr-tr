---
title: Azure Güvenlik temeli
description: Azure SQL veritabanı ve Azure SQL yönetilen örneği için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 3c890dc48c97580fd8de5c817c96b62d4eb9c009
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045952"
---
# <a name="azure-security-baseline-for-azure-sql-database--sql-managed-instance"></a>Azure SQL veritabanı & SQL yönetilen örneği için Azure Güvenlik temeli
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Azure SQL veritabanı için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Kılavuz**: Azure özel bağlantısını, sanal ağınızdaki özel bir uç nokta üzerinden Azure PaaS hizmetlerine (ÖRNEĞIN, SQL veritabanı) ve Azure 'da barındırılan müşteri/iş ortağı hizmetlerine erişime izin verecek şekilde etkinleştirebilirsiniz. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır.

Trafiğin Azure SQL veritabanına ulaşmasını sağlamak için, ağ güvenlik grupları aracılığıyla giden trafiğe izin vermek üzere SQL hizmeti etiketlerini kullanın.

Sanal ağ kuralları, Azure SQL veritabanı 'nın yalnızca bir sanal ağ içindeki seçili alt ağlardan gönderilen iletişimleri kabul etmesine olanak tanır.

Azure SQL veritabanı için özel bağlantı ayarlama:

https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview#how-to-set-up-private-link-for-azure-sql-database

Sunucular için sanal ağ hizmet uç noktalarını ve kurallarını kullanma:

https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Kılavuz**: Azure Güvenlik Merkezi 'ni kullanın ve Azure SQL veritabanınızın dağıtıldığı alt ağ için ağ koruma önerilerini düzeltin.

Azure SQL veritabanınıza bağlanacağınız Azure sanal makineleri (VM) için, bu VM 'Leri koruyan NSG 'Ler için ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir Azure depolama hesabına gönderin.

Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

NSG akış günlüklerini etkinleştirme:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlayın:

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

Trafik Analizi etkinleştirme ve kullanma:

https://docs.microsoft.com/azure/network-watcher/traffic-analytics

Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlayın:

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; Bu öneri, Web uygulamalarını barındıran Azure Apps hizmeti veya işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: dağıtılmış hizmet reddi saldırılarına karşı korumalar IÇIN Azure SQL veritabanınızdan Ilişkilendirilen sanal ağlarda DDoS koruma standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

DDoS korumasını yapılandırma:

https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama:

https://docs.microsoft.com/azure/security-center/security-center-alerts-data-services

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Kılavuz**: Azure SQL veritabanı örneğinize bağlanacağınız Azure sanal makineleri (VM) için, bu VM 'Leri koruyan NSG 'ler için ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir Azure depolama hesabına gönderin. Anormal etkinlikleri araştırmak için gerekliyse, ağ Izleyicisi paket yakalamayı etkinleştirin.

NSG akış günlüklerini etkinleştirme:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Ağ Izleyicisi 'ni etkinleştirme:

https://docs.microsoft.com/azure/network-watcher/network-watcher-create

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Kılavuz**: Azure SQL veritabanı Için Gelişmiş tehdit koruması 'Nı (ATP) etkinleştirin.  Kullanıcılar şüpheli veritabanı etkinlikleri, olası güvenlik açıkları ve SQL ekleme saldırılarına ve anormal veritabanı erişimi ve sorgu desenleri üzerine bir uyarı alırlar. Gelişmiş tehdit koruması Ayrıca uyarıları Azure Güvenlik Merkezi ile tümleştirir.

Azure SQL veritabanı için Gelişmiş tehdit koruması 'nı anlayın ve kullanın:https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: uygulanamaz; Bu öneri, Web uygulamalarını barındıran Azure Apps hizmeti veya işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örn., Apimanaya) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

Azure SQL veritabanı için hizmet uç noktaları kullanılırken, Azure SQL veritabanı genel IP adreslerine giden bağlantı gereklidir: bağlantıya izin vermek için ağ güvenlik grupları (NSG 'Ler) Azure SQL veritabanı IP 'lerine açılmalıdır. Bunu Azure SQL veritabanı için NSG hizmet etiketlerini kullanarak yapabilirsiniz.

Hizmet etiketlerini Azure SQL veritabanı için hizmet uç noktaları ile anlayın:

https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview#limitations

Hizmet etiketlerini anlama ve kullanma:

https://docs.microsoft.com/azure/virtual-network/service-tags-overview

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Kılavuz**: Azure Ilkesi Ile Azure SQL veritabanınız için ağ güvenlik yapılandırması tanımlama ve uygulama. Özel ilke tanımlarını tanımlamak için "Microsoft. SQL" ad alanını kullanabilir veya sunucu ağ koruması için tasarlanan yerleşik ilke tanımlarından herhangi birini kullanabilirsiniz. Bir sunucu için geçerli bir yerleşik ağ güvenlik ilkesi örneği şöyle olabilir: "SQL veritabanı bir sanal ağ hizmeti uç noktası kullanmalıdır".

Azure Kaynak Yönetimi şablonları, rol tabanlı erişim denetimi (RBAC) ve ilkeler gibi tek bir şema tanımında, büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanın. Şema 'i yeni abonelikler ve ortamlara kolayca uygulayın ve sürüm oluşturma aracılığıyla denetimi ve yönetimi ayrıntılı olarak ayarlayın.

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Blueprint oluşturma:https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: ağ güvenlik grupları (NSG) ve ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketler kullanın. Bireysel NSG kuralları için "Açıklama" alanını kullanarak ağa giden/giden trafiğe izin veren kuralların iş gereksinimini ve/veya süresini (vs.) belirtin.

Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure Ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın.

Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynaklar üzerinde arama yapabilir veya eylemler gerçekleştirebilirsiniz.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Rehberlik**: ağ kaynak yapılandırmasını izlemek ve sunucunuza ilişkin ağ kaynaklarına yönelik değişiklikleri algılamak Için Azure etkinlik günlüğü 'nü kullanın. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

Azure etkinlik günlüğü olaylarını görüntüleme ve alma:

https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

Azure Izleyici 'de uyarı oluşturma:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, Azure kaynakları için zaman kaynaklarını korur. İşlem dağıtımlarınız için zaman eşitlemesini güncelleştirebilirsiniz.

Azure işlem kaynakları için Saat eşitlemesini yapılandırma:

https://docs.microsoft.com/azure/virtual-machines/windows/time-sync

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Kılavuz**: Azure SQL veritabanı için denetimi, veritabanı olaylarını Izlemek ve Azure Depolama hesabınızdaki bir denetim günlüğüne, Log Analytics çalışma alanına veya Event Hubs yazmak üzere etkinleştirin.

Ayrıca, Azure SQL tanılama telemetrisini, Azure SQL veritabanı ve Azure SQL yönetilen örneği ile aynı ölçekte ve birden çok aboneliğe yönelik performansı izleyen Azure SQL Analytics bir bulut çözümü olarak akışla aktarabilirsiniz. Azure SQL veritabanı performans ölçümlerini toplamanıza ve görselleştirmenize yardımcı olabilir ve performans sorunlarını gidermek için yerleşik zekaya sahiptir.

Azure SQL veritabanınız için Denetim kurma:

https://docs.microsoft.com/azure/sql-database/sql-database-auditing

Azure Izleyici ile platform günlükleri ve ölçümleri toplama:

https://docs.microsoft.com/azure/sql-database/sql-database-metrics-diag-logging

Tanılamayı Azure SQL Analytics olarak akışa alma:

https://docs.microsoft.com/azure/sql-database/sql-database-metrics-diag-logging#stream-into-azure-sql-analytics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: sunucunuzda denetimi etkinleştirin ve denetim günlükleri (Azure depolama, Log Analytics veya Olay Hub 'ı) için bir depolama konumu seçin.

Azure SQL veritabanı için denetimi etkinleştirme:

https://docs.microsoft.com/azure/sql-database/sql-database-auditing

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu kıyaslama, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure SQL veritabanı günlüklerinizi bir Log Analytics çalışma alanında depolarken, kuruluşunuzun uyumluluk düzenlemelerine göre günlük tutma süresini ayarlayın.

Günlük tutma parametrelerini ayarlama:

https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: anormal davranışlar için günlükleri çözümleyin ve izleyin ve sonuçları düzenli olarak gözden geçirin. Azure SQL veritabanı örneğiniz ile ilgili olağandışı etkinlikleri uyarmak için Azure Güvenlik Merkezi 'nin Gelişmiş tehdit koruması 'nı kullanın. Alternatif olarak, Azure SQL veritabanı örneklerinizle ilgili ölçüm değerlerine veya Azure etkinlik günlüğü girişlerine göre uyarıları yapılandırın.

Azure SQL veritabanı için Gelişmiş tehdit koruması ve uyarı vermeyi anlayın:

https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview

Azure SQL veritabanı için özel uyarıları yapılandırma:

https://docs.microsoft.com/azure/sql-database/sql-database-insights-alerts-portal?view=azps-1.4.0

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Kılavuz**: Azure Güvenlik Merkezi Gelişmiş tehdit koruması 'Nı Azure SQL veritabanı için, anormal etkinlikleri izlemek ve uyarmak üzere kullanın. SQL veritabanlarınız için gelişmiş veri güvenliğini etkinleştirin. Gelişmiş veri güvenliği, hassas verileri bulma ve sınıflandırma, olası veritabanı güvenlik açıklarını ortaya koymasının yanı sıra veritabanınıza yönelik bir tehdit oluşturabilecek anormal etkinlikleri algılayarak ilgili işlevsellik içerir.

Azure SQL veritabanı için Gelişmiş tehdit koruması ve uyarı vermeyi anlayın:

https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview

Azure SQL veritabanı için gelişmiş veri güvenliğini etkinleştirme:

https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security

Azure Güvenlik Merkezi 'nde uyarıları yönetme:

https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz; Azure SQL veritabanı için, kötü amaçlı yazılımdan koruma çözümü, temel alınan platformda Microsoft tarafından yönetilir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; DNS günlüğü, Azure SQL veritabanı için geçerli değildir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; komut satırı denetimi, Azure SQL veritabanı için geçerli değildir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve Access Control](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: Azure ACTIVE DIRECTORY (AAD) açıkça atanması ve sorgulanabilir olması gereken yerleşik roller içerir. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için AAD PowerShell modülünü kullanın.

Azure AD 'de PowerShell ile dizin rolü alma:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0

Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: Azure Active Directory varsayılan parola kavramına sahip değildir. Azure SQL veritabanı örneği sağlanırken, kimlik doğrulamasını Azure Active Directory ile tümleştirmeyi tercih etmeniz önerilir.

Azure SQL ile Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme:

https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-configure

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında ilke ve yordamlar oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Azure Güvenlik Merkezi kimliğini ve erişimini anlayın:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: uygulanamaz; Azure Active Directory kimlik doğrulamasını Azure SQL veritabanı ile tümleştirilecek şekilde yapılandırabileceğiniz sürece çoklu oturum açma desteklenmez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure ACTIVE DIRECTORY (AAD) MULTI-Factor AUTHENTICATION (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

Azure 'da MFA 'yı etkinleştirme:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure kaynaklarını açmak ve yapılandırmak için YAPıLANDıRıLMıŞ Multi-Factor Authentication MFA Ile ayrıcalıklı erişim iş istasyonu (Paw) kullanın.

Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin:

https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure 'da MFA 'yı etkinleştirme:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda günlüklerin ve uyarıların üretilmesi için Azure Active Directory güvenlik raporlarını kullanın.

Veritabanları için olağan dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılamak için Azure SQL veritabanı için Gelişmiş tehdit koruması 'nı kullanın.

Riskli etkinlik için işaretlenen Azure AD kullanıcılarını belirleme:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk

Azure Güvenlik Merkezi 'nde Kullanıcı kimliğini ve erişim etkinliğini izleme:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

Gelişmiş tehdit koruması ve olası uyarıları gözden geçirin:

https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview#advanced-threat-protection-alerts

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından portala ve Azure kaynak yönetimine erişim sağlamak Için, koşullu erişim adlı konum kullanın.

Azure 'da adlandırılmış konumlar nasıl yapılandırılır:https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: sunucunuz için bir Azure ACTIVE DIRECTORY (AAD) Yöneticisi oluşturun.

Azure SQL ile Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme:

https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-configure

AAD örneği oluşturma ve yapılandırma:

https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure ACTIVE DIRECTORY (AAD) eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi incelemelerini kullanın. Kullanıcıların erişimi, yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için düzenli aralıklarla gözden geçirilebilir.

Azure kimlik erişimi Incelemelerini kullanma:

https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Rehberlik**: Azure ACTIVE DIRECTORY (AAD) kimlik DOĞRULAMASıNı Azure SQL ile yapılandırma ve Azure Active Directory Kullanıcı Hesapları Için Tanılama ayarları oluşturma, Denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına gönderme. Log Analytics çalışma alanı içinde istenen uyarıları yapılandırın.

Azure SQL ile Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme:

https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-configure

Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: Azure ACTIVE DIRECTORY (AAD) kimlik koruması ve risk algılamalarını kullanarak otomatik yanıtları, Kullanıcı kimlikleriyle ilgili şüpheli eylemler tespit edin. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz.

Azure AD risk oturum açma işlemlerini görüntüleme:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme:

https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Kılavuz**: Microsoft 'un müşteri verilerine erişmesi gereken destek senaryolarında, Azure müşteri kasası müşterilerin müşteri verileri erişim isteklerini gözden geçirmesi ve onaylaması veya reddetmesi için bir arabirim sağlar.

Müşteri Kasası anlayın:

https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Kaynaklar VNET/subnet ile ayrılmalıdır, uygun şekilde etiketlenebilir ve bir NSG veya Azure Güvenlik duvarı içinde güvenliği sağlanmış olmalıdır. Hassas verileri depolayan veya işleyen kaynaklar yalıtılmalıdır. Özel bağlantı kullan; Azure SQL veritabanı 'nı VNET 'iniz içinde dağıtın ve özel uç noktaları kullanarak özel olarak bağlayın.

Ek Azure abonelikleri oluşturma:

https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları oluşturma:

https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Azure SQL veritabanı için özel bağlantı ayarlama:

https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview#how-to-set-up-private-link-for-azure-sql-database

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Kılavuz**: Azure SQL veritabanı 'nda gizli bilgileri depolayan veya işleyen veritabanları Için, etiketleri kullanarak veritabanını ve ilgili kaynakları gizli olarak işaretleyin. Gizli bilgilerin ayıklanmasını engellemek için Azure SQL veritabanı örneklerindeki ağ güvenlik grubu hizmet etiketleriyle birlikte özel bağlantıyı yapılandırın.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

Azure SQL veritabanı örneklerinizin veri taşmasını engellemek için özel bağlantı ve NSG 'ler yapılandırma:

https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview

Azure 'da müşteri veri korumasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: Azure SQL veritabanı, Aktarım Katmanı Güvenliği Ile hareket halindeki verileri şifreleyerek verilerinizi korur. SQL veritabanı, tüm bağlantılar için her zaman şifreleme (SSL/TLS) uygular. Bu, bağlantı dizesinde şifreleme veya TrustServerCertificate ayarından bağımsız olarak, tüm verilerin istemci ve sunucu arasında "geçişte" şifrelendiğinden emin olmanızı sağlar.

Iletimde Azure SQL şifrelemesini anlayın:

https://docs.microsoft.com/azure/sql-database/sql-database-security-overview#information-protection-and-encryption

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Kılavuz**: Azure SQL veritabanı veri bulma ve sınıflandırma özelliğini kullanın. Veri bulma ve sınıflandırma, &amp; veritabanlarınızdaki hassas verileri korumak, sınıflandırmak ve etiketleme Için Azure SQL veritabanı 'nda yerleşik olarak bulunan gelişmiş yetenekler sağlar.

Azure SQL veritabanı için veri bulmayı ve sınıflandırmayı kullanma:

https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Kılavuz**: Azure SQL veritabanı örneklerine erişimi kimlik doğrulaması ve denetleme için Azure ACTIVE DIRECTORY (AAD) kullanın.

Azure SQL veritabanı 'nı kimlik doğrulaması için Azure Active Directory tümleştirme:

https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication

Azure SQL veritabanı 'nda erişimi denetleme:

https://docs.microsoft.com/azure/sql-database/sql-database-control-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: Microsoft, Azure SQL veritabanı için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

Azure 'da müşteri veri korumasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Kılavuz**: saydam veri şifrelemesi (tde), bekleyen verileri şifreleyerek kötü niyetli çevrimdışı etkinlik tehditlerine KARŞı Azure SQL veritabanı, Azure SQL yönetilen örneği ve Azure veri ambarı ' nı korumanıza yardımcı olur. Bu özellik bütün bir veritabanı, yedekleri ve işlem günlüğü dosyaları için gerçek zamanlı şifreleme ve şifre çözme işlemlerini gerçekleştirir ve uygulamada değişiklik yapmayı gerektirmez. Varsayılan olarak, TDE, SQL veritabanı ve SQL yönetilen örneği 'nde yeni dağıtılan tüm veritabanları için etkinleştirilmiştir. TDE şifreleme anahtarı, Microsoft ya da müşteri tarafından yönetilebilir.

Saydam veri şifrelemesini yönetme ve kendi şifreleme anahtarlarınızı kullanma:

https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?tabs=azure-portal#manage-transparent-data-encryption

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure SQL veritabanı 'nın üretim örneklerine ve diğer kritik veya ilgili kaynaklara yönelik değişikliklerin ne zaman gerçekleştiği hakkında uyarı oluşturmak Için Azure etkinlik günlüğü Ile Azure izleyici 'yi kullanın.

Azure etkinlik günlüğü olayları için uyarı oluşturma:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Kılavuz**: Azure SQL veritabanı Için gelişmiş veri güvenliğini etkinleştirin ve sunucularınızda güvenlik açığı değerlendirmelerinin gerçekleştirilmesi Için Azure Güvenlik Merkezi 'ndeki önerileri izleyin.

Azure SQL veritabanı 'nda güvenlik açığı değerlendirmeleri nasıl çalıştırılır:

https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment

Gelişmiş veri güvenliğini etkinleştirme:

https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security

Azure Güvenlik Merkezi güvenlik açığı değerlendirmesi önerilerini uygulama:

https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu kıyaslama, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Kılavuz**: Azure SQL veritabanı örneklerinizin düzenli olarak yinelenen taranmasını etkinleştirin; Bu işlem, bir güvenlik açığı değerlendirmesini, veritabanında haftada bir kez otomatik olarak bir tarama çalıştıracak şekilde yapılandırır. Sağladığınız e-posta adreslerine bir tarama sonucu özeti gönderilir. Güvenlik açıklarının düzeltildiğini doğrulamak için sonuçları karşılaştırın.

Azure Güvenlik Merkezi 'nde güvenlik açığı değerlendirmesi raporunu dışarı aktarma:

https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment#implementing-vulnerability-assessment

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Kılavuz**: Azure Güvenlik Merkezi tarafından sunulan varsayılan risk derecelendirmelerini (güvenli puan) kullanın.

Azure Güvenlik Merkezi güvenli Puanını anlayın:

https://docs.microsoft.com/azure/security-center/security-center-secure-score

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: abonelikleriniz dahilinde (Azure SQL veritabanı dahil) tüm kaynakları sorgulamak ve saptamak Için Azure Kaynak grafiğini kullanın.  Kiracınızda uygun (okuma) izinleriniz olduğundan ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerinin listesini belirleyebildiğinizden emin olun.

Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

Azure Kaynak Graf ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure aboneliklerinizi görüntüleme:https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure RBAC 'yi anlama:https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure kaynaklarına Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler verirsiniz.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: varlıkları düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

Ek Azure abonelikleri oluşturma:

https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları oluşturma:

https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini koruyun

**Rehberlik**: işlem kaynaklarınız Için onaylanan Azure kaynakları ve onaylanan yazılımın listesini tanımlama

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Aboneliklerinizin içindeki kaynakları sorgulamak/öğrenmek için Azure Kaynak grafiğini kullanın. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

**Azure Güvenlik Merkezi izleme**: uygulanamaz

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

Azure Ilkesini yapılandırma ve yönetme:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Ilkesiyle belirli bir kaynak türünü reddetme:https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynakları üzerinde çalışan uygulamalar için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6,11: kullanıcıların betikler aracılığıyla Azure Resources Manager ile etkileşim kurma yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak Için Azure koşullu erişimi kullanın.

Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma:https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: uygulanamaz; Bu öneri, masaüstü veya Web uygulamalarını barındıran App Service veya işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: tüm Azure kaynakları için güvenlik yapılandırmalarının bakımını yapmak üzere Azure ilkesi 'ni veya Azure SQL veritabanı Için Azure Güvenlik Merkezi önerilerini kullanın.

Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Ilke efektlerini anlayın:

https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure devops veya Azure Repos kullanın.

Azure DevOps 'da kod depolama:

https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Repos belgeleri:

https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Rehberlik**: "MICROSOFT. SQL" ad alanındaki Azure ilke diğer adlarını kullanarak, sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturun. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma izlemeyi uygulayın

**Kılavuz**: Azure SQL veritabanı için temel taramalar gerçekleştirmek üzere Azure Güvenlik Merkezi 'nden yararlanın.

Azure Güvenlik Merkezi 'nde öneriler nasıl düzeltileceği:

https://docs.microsoft.com/azure/security-center/security-center-sql-service-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Kılavuz**: Azure SQL veritabanı saydam veri şifrelemesi (tde) için şifreleme anahtarlarını depolamak üzere Azure Key Vault kullanın.

Azure SQL veritabanında depolanan hassas verileri koruma ve şifreleme anahtarlarını Azure Key Vault içinde depolama:

https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: Azure ACTIVE DIRECTORY (AAD) ' de otomatik olarak yönetilen kimlik ile Azure hizmetleri sağlamak Için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Azure Key Vault dahil olmak üzere AAD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

Öğretici: Azure SQL 'e erişmek için Windows VM sistem tarafından atanan bir yönetilen kimlik kullanın:

https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-sql

Yönetilen kimlikleri yapılandırma:

https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kodunuzda kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

Kimlik bilgisi tarayıcısını ayarlama:https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir. Microsoft, temel alınan platform için kötü amaçlı yazılımdan koruma uygular.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft 'un kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure App Service) etkinleştirilir, ancak müşteri içeriğinde çalışmaz.

App Service, Data Lake Storage, BLOB depolama, Azure SQL veritabanı vb. gibi işlem dışı Azure kaynaklarına yüklenen tüm içerikleri önceden tarayın. Microsoft bu örneklerdeki verilerinize erişemez.

Azure Cloud Services ve sanal makineler için Microsoft kötü amaçlı yazılımdan koruma 'i anlayın:https://docs.microsoft.com/azure/security/fundamentals/antimalware

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir. Microsoft, temel alınan platform için kötü amaçlı yazılımdan koruma uygular.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: işinizi veri kaybına karşı korumak IÇIN Azure SQL veritabanı, haftalık olarak tam veritabanı yedeklemeleri, 12 saatte bir fark veritabanı yedeklemeleri ve işlem günlüğü yedeklemelerini her 5-10 dakikada bir otomatik olarak oluşturur. Yedeklemeler, tüm hizmet katmanları için en az 7 gün için RA-GRS depolama alanında depolanır. En fazla 35 güne kadar, noktadan noktaya geri yükleme için yapılandırılabilir yedekleme saklama süresi Temel destek hariç tüm hizmet katmanları.

Farklı uyumluluk gereksinimlerini karşılamak için haftalık, aylık ve/veya yıllık yedeklemeler için farklı saklama süreleri seçebilirsiniz. Depolama alanı tüketimi, seçilen yedekleme sıklığına ve bekletme dönemine bağlıdır.

Azure SQL veritabanı ile yedeklemeleri ve iş devamlılığını anlayın:

https://docs.microsoft.com/azure/sql-database/sql-database-business-continuity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Kılavuz**: Azure SQL veritabanı, 7 ila 35 gün içinde tutulan veritabanı yedeklerini otomatik olarak oluşturur ve veri merkezi kullanılamaz olsa bile korundıklarından emin olmak için Azure Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS) kullanır. Bu yedeklemeler otomatik olarak oluşturulur. Gerekirse, Azure SQL veritabanlarınız için uzun süreli coğrafi olarak yedekli yedeklemeleri etkinleştirin.

Saydam Veri Şifrelemesi için müşteri tarafından yönetilen anahtarlar kullanıyorsanız, anahtarlarınızın yedeklendiğinden emin olun.

Azure SQL veritabanı 'nda yedeklemeleri anlayın:

https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups?tabs=single-database

Azure 'da Anahtar Kasası anahtarları nasıl yedekleirsiniz:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: Azure Backup içindeki içeriğin düzenli olarak veri geri yüklemesini gerçekleştirme olanağı sağlar. Gerekirse, içeriği yalıtılmış bir VLAN 'a geri yükleme testi yapın. Yedeklenen müşteri tarafından yönetilen anahtarların test geri yüklemesi.

Azure 'da Anahtar Kasası anahtarlarını geri yükleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

Azure SQL veritabanı yedeklemelerini, zaman içinde nokta geri yükleme kullanarak kurtarma:

https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups#point-in-time-restore

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Azure Key Vault geçici silme özelliğini etkinleştirin.

Key Vault 'da geçici silmeyi etkinleştirme:

https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: personel rollerinin yanı sıra olay işleme/yönetim aşamalarını tanımlayan yazılı olay yanıt planları olduğundan emin olun.

Azure Güvenlik Merkezi 'nde Iş akışı Otomatiklamalar nasıl yapılandırılır:

https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, her uyarıya katılma sırasını önceliklendirmenize yardımcı olmak için uyarılara önem derecesi atar. böylece, bir kaynak tehlikeye girdiğinde, hemen bu işe başlayabilirsiniz. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Azure Güvenlik Merkezi 'nde güvenlik uyarıları:https://docs.microsoft.com/azure/security-center/security-center-alerts-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

NıST yayınına başvurabilirsiniz: BT planları ve özellikleri için test, eğitim ve alıştırma programlarına kılavuzluk edin:

https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır.

Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama:

https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Uyarıları Sentinel 'e akışa almak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.

Sürekli dışarı aktarmayı yapılandırma:

https://docs.microsoft.com/azure/security-center/continuous-export

Uyarıları Azure Sentinel 'e aktarma:

https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: güvenlik uyarılarında ve önerilerinde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın.

Iş akışı otomasyonunu yapılandırma ve Logic Apps:

https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve 60 gün içinde tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için lütfen Microsoft katılım kurallarını izleyin:

https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1.

Microsoft 'un yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına yönelik kırmızı ekip oluşturma ve canlı site sızma testini yürütmeye yönelik daha fazla bilgi edinebilirsiniz:https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](https://docs.microsoft.com/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
