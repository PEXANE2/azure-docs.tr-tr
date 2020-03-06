---
title: Batch için Azure Güvenlik temeli
description: Batch için Azure Güvenlik temeli
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: af0466299d1f972fe9334beb5f31c886c1928a1c
ms.sourcegitcommit: 021ccbbd42dea64d45d4129d70fff5148a1759fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78329008"
---
# <a name="azure-security-baseline-for-batch"></a>Batch için Azure Güvenlik temeli

Batch için Azure Güvenlik taban çizgisi, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetlerin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Rehberlik**: sanal ağ içinde Azure Batch havuzları dağıtın. Havuz işlem düğümlerinin diğer sanal makinelerle veya şirket içi bir ağla güvenli bir şekilde iletişim kurmasına izin vermek için, havuzu bir Azure sanal ağının alt ağında sağlayabilirsiniz. Ayrıca, havuzunuzu bir sanal ağ içinde dağıtmak, tek tek düğümlerin ağ arabirimlerinin (NIC) ve alt ağın güvenliğini sağlamak için kullanılan ağ güvenlik grubu (NSG) üzerinde denetim sağlamanıza olanak tanır. NSG 'yi Internet 'teki yalnızca güvenilir IP (ler) e-konumlarından trafiğe izin verecek şekilde yapılandırın.


Bir sanal ağ içinde Azure Batch havuzu oluşturma:

https://docs.microsoft.com/azure/batch/batch-virtual-network

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Kılavuz**: Azure Güvenlik Merkezi 'ni kullanın ve Batch havuzunuzun ilişkili sanal ağ/ağ güvenlik grubu (NSG) ile ilgili ağ koruması önerilerini düzeltin. Batch havuzunu korumak için kullanılan NSG 'de akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri Azure depolama hesabına gönderin. Ayrıca Azure Log Analytics çalışma alanına NSG akış günlükleri gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Azure Trafik Analizi kullanabilirsiniz. Azure Trafik Analizi 'nin bazı avantajları, ağ etkinliğini görselleştirme ve etkin noktaları tanımlayabilir, güvenlik tehditlerini belirleyebilir, trafik akışı düzenlerini anlayabilir ve ağ ile hatalı yapılandırmaların nasıl belirlenmesine olanak sağlar.


NSG akış günlüklerini etkinleştirme:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal


Trafik Analizi etkinleştirme ve kullanma:

https://docs.microsoft.com/azure/network-watcher/traffic-analytics


Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlayın:

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: geçerli değil, kıyaslama Azure App Service veya IaaS örneklerinde çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle Iletişimleri reddetme

**Rehberlik**: DDoS saldırılarına karşı koruma için Azure Batch havuzunuzu koruyan sanal ağ üzerinde Azure DDoS (dağıtılmış hizmet reddi) standart korumasını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.


DDoS korumasını yapılandırma:

https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection


Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama:

https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Rehberlik**: Azure Batch havuzunuzu korumak için kullanılan ağ güvenlik grubu 'NDA (NSG) akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir Azure depolama hesabına gönderin.


NSG akış günlüklerini etkinleştirme:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemlerini dağıtma

**Rehberlik**: uyumluluk amaçları için gerekliyse, Azure Marketi 'nden, yük İnceleme özelliklerine sahip yetkisiz giriş algılama sıstemlerı (kimlikler) ve yetkisiz erişim önleme sıstemlerı (IP) işlevselliğini destekleyen bir ağ sanal gereci seçin.


Yük incelemesini temel alan yetkisiz giriş algılama ve/veya önleme bir gereksinim değilse, tehdit zekasıyla Azure Güvenlik Duvarı kullanılabilir. Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme, bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen ve giden trafiği uyarabilir ve reddedebilir. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından kaynaklıdır.


Azure Güvenlik duvarını, Azure Batch havuz düğümleriyle aynı sanal ağdaki genel IP adresiyle dağıtın. Ağ adresi çevirisi (NAT) kurallarını Internet 'teki güvenilen konumlar ve ayrı havuz düğümlerinizin özel IP adresleri arasında yapılandırın. Azure Güvenlik duvarında, tehdit zekasından, bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen/giden trafiği uyarma ve engellemeye engel olmak için "uyarı ve reddetme" seçeneğini yapılandırın. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından kaynaklıdır ve yalnızca en yüksek güvenirlik kayıtları dahildir. 


Bir sanal ağ içinde Azure Batch havuzu oluşturma:

https://docs.microsoft.com/azure/batch/batch-virtual-network


Azure Güvenlik duvarını dağıtma:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal


Azure Marketi:

https://azuremarketplace.microsoft.com/marketplace/?term=Firewall

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-your-web-applications"></a>1,7: Web uygulamalarınıza giden trafiği yönetme

**Rehberlik**: geçerli değil, kıyaslama Azure App Service veya IaaS örneklerinde çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: ağ güvenlik grupları veya Azure Batch havuzlarınız Ile Ilişkili Azure Güvenlik duvarları üzerinde ağ erişim denetimleri tanımlamak için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örn., Apimanaya) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.


Hizmet etiketlerini anlama ve kullanma:

https://docs.microsoft.com/azure/virtual-network/service-tags-overview

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: Azure ilkesiyle Azure Batch havuzlarınızla ilişkili ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure Batch havuzlarınızın ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Batch" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın.



Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: ağ hizmet grupları (NSG 'ler) ve Azure Batch havuzlarınızla ilişkili ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketler kullanın. Bireysel NSG kuralları için "Açıklama" alanını kullanarak ağa giden/giden trafiğe izin veren kuralların iş gereksinimini ve/veya süresini (vs.) belirtin.


Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın.


Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynaklar üzerinde arama yapabilir veya eylemler gerçekleştirebilirsiniz.


Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags


Sanal ağ oluşturma:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal


NSG oluşturma:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını Izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını izleyin ve Azure Batch havuzlarınızla ilgili ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

Azure etkinlik günlüğü olaylarını görüntüleme ve alma: https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view 

Azure Izleyici 'de uyarı oluşturma: https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Azure Batch için, varsayılan olarak Microsoft zaman eşitlemesi sağlar. Ancak, belirli zaman eşitleme gereksinimleriniz varsa, bu değişiklikleri uygulayabilirsiniz.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: küme cihazları tarafından oluşturulan güvenlik verilerini toplamak Için Azure izleyici 'ye Azure Batch hesabı ekleyin. Ortamdaki tehditleri algılamak ve yanıtlamak için özel sorgulardan yararlanın.  Kaynak düzeyinde izleme Azure Batch için Batch API 'Lerini kullanarak işler, görevler, düğümler ve havuzlar dahil kaynaklarınızın durumunu izleyin veya sorgulayın.



Azure Izleyici 'ye Azure Batch hesabı ekleme:

https://docs.microsoft.com/azure/batch/batch-diagnostics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Kılavuz**: Azure Batch hesap düzeyinde izleme Için, Azure izleyici 'nin özelliklerini kullanarak her Batch hesabını izleyin. Azure Izleyici, havuzlar, işler ve görevler gibi Batch hesabı düzeyinde kapsamlı kaynaklar için ölçümleri ve isteğe bağlı tanılama günlüklerini toplar. Batch hesabınızdaki etkinlikleri izlemek ve sorunları tanılamak için bu verileri el ile veya program aracılığıyla toplayıp kullanın.


Kaynak düzeyinde izleme Azure Batch için, işler, görevler, düğümler ve havuzlar dahil olmak üzere kaynaklarınızın durumunu izlemek veya sorgulamak için Azure Batch API 'Lerini kullanın.


Azure Batch hesap düzeyinde izleme ve günlüğe kaydetme 'yi yapılandırma:

https://docs.microsoft.com/azure/batch/monitoring-overview


Batch kaynak düzeyinde izlemeyi anlama:

https://docs.microsoft.com/azure/batch/monitoring-overview#batch-resource-monitoring

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-system"></a>2,4: Işletim sisteminden güvenlik günlüklerini toplama

**Kılavuz**: Azure izleyici, Azure Batch hesabınızdaki kaynaklar için ölçümleri ve tanılama günlüklerini toplar. Azure Batch hesabınızı izlemek ve sorunları tanılamak için kullanabileceğiniz çeşitli yollarla bu verileri toplayın ve kullanın. Ölçüm uyarılarını Ayrıca, bir ölçüm belirtilen bir değere ulaştığında bildirimler almanızı sağlayacak şekilde de yapılandırabilirsiniz.


Gerekirse, yerel işletim sistemi günlüklerine erişmek için güvenli kabuk (SSH) veya Uzak Masaüstü Protokolü (RDP) aracılığıyla ayrı havuz düğümlerine bağlanırsınız.


Azure Batch hesabınızdan tanılama günlüklerini toplama:

https://docs.microsoft.com/azure/batch/batch-diagnostics#batch-diagnostics


Azure Batch havuz düğümlerine uzaktan bağlanma:

https://docs.microsoft.com/azure/batch/batch-api-basics#error-handling

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Rehberlik**: Azure Batch hesabını Azure izleyici 'ye ekleyin. Kullanılan Azure Log Analytics çalışma alanının, kuruluşunuzun uyumluluk düzenlemelerine göre ayarlanmış günlük tutma süresine sahip olduğundan emin olun


Azure Batch izlemeyi ve günlüğe kaydetmeyi yapılandırma:

https://docs.microsoft.com/azure/batch/monitoring-overview


Azure Log Analytics çalışma alanı saklama süresini yapılandırma:

https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: belirtilen ölçüm değeri belirli bir eşiği aştığında tetiklenecek Azure Batch ölçüm uyarıları oluşturun.


Azure Batch ölçüm uyarılarını yapılandırma:

https://docs.microsoft.com/azure/batch/batch-diagnostics

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Rehberlik**: belirtilen ölçüm değeri belirli bir eşiği aştığında tetiklenecek Azure Batch ölçüm uyarıları oluşturun.


Azure Batch ölçüm uyarılarını yapılandırma:

https://docs.microsoft.com/azure/batch/batch-diagnostics

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: Windows işletim sistemleri durumunda ayrı toplu Iş düğümlerinde Windows Defender 'ı kullanın veya Linux kullanıyorsanız kendi kötü amaçlı yazılımdan koruma çözümünüzü sağlayın.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: DNS günlüğü için üçüncü taraf çözümü uygulama

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: konsol günlüğünü ve PowerShell dökümünü, düğüm başına temelinde el ile yapılandırın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve Access Control](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tut

**Rehberlik**: Azure Batch havuzunun sağlanması sırasında oluşturulan yerel yönetim hesabının kaydını ve oluşturduğunuz diğer hesapları saklayın. Ayrıca, Azure Active Directory (AAD) Tümleştirmesi kullanılırsa, AAD 'nin açıkça atanması ve bu nedenle sorgulanabilir olması gereken yerleşik rolleri vardır. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için AAD PowerShell modülünü kullanın.


Ayrıca, Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini de kullanabilirsiniz.


PowerShell ile AAD 'de dizin rolü alma:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0


PowerShell ile AAD 'de bir dizin rolünün üyelerini alma:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0


Azure Güvenlik Merkezi ile kimlik ve erişimi izleme:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: bir Azure Batch havuzu sağlanırken, yerel makine hesapları oluşturma seçeneği verilir. Değiştirilecek varsayılan parola yoktur, ancak güvenli kabuk (SSH) ve Uzak Masaüstü Protokolü (RDP) erişimi için farklı parolalar belirtebilirsiniz. Azure Batch havuzu yapılandırıldıktan sonra, Azure portal veya Azure Resource Manager API aracılığıyla tek tek düğümler için rastgele bir kullanıcı oluşturabilirsiniz.


Belirli bir işlem düğümüne Kullanıcı ekleme:

https://docs.microsoft.com/rest/api/batchservice/computenode/adduser

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-ensure-the-use-of-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullandığınızdan emin olun

**Rehberlik**: Azure Active Directory Azure Batch uygulamalar Için kimlik doğrulamasını tümleştirin. Adanmış yönetim hesaplarının kullanımı etrafında ilke ve yordamlar oluşturun.


Ayrıca, Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini de kullanabilirsiniz.


Azure Active Directory ile Batch uygulamalarının kimliğini doğrulama:

https://docs.microsoft.com/azure/batch/batch-aad-auth


Azure Güvenlik Merkezi ile kimlik ve erişimi izleme:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="34-utilize-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: uygulanamaz, Azure Batch Azure AD kimlik doğrulamasını destekliyorsa, çoklu oturum açma desteklenmez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="35-use-multifactor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için çok faktörlü kimlik doğrulaması kullanın.

**Rehberlik**: Azure ACTIVE DIRECTORY (AAD) Ile Azure Batch uygulamalar Için kimlik doğrulamasını tümleştirin. AAD Multi-Factor Authentication 'ı (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.
 


Azure 'da MFA 'yı etkinleştirme:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted


Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Rehberlik**: Azure Batch kaynaklarınızı oturum açmak ve yapılandırmak için yapılandırılmış çok faktörlü kimlik DOĞRULAMASıYLA (MFA) Paws (ayrıcalıklı erişim iş istasyonları) kullanın.


Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin:

https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations


Azure 'da MFA 'yı etkinleştirme:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Rehberlik**: Azure Batch uygulamalar için Azure ACTIVE DIRECTORY (AAD) ile tümleşik kimlik doğrulaması yaptıysanız, ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda Günlükler ve uyarılar oluşturmak için Azure Active Directory güvenlik raporları kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi 'ni kullanın.


Riskli etkinlik için işaretlenen Azure AD kullanıcılarını belirleme:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk


Azure Güvenlik Merkezi 'nde Kullanıcı kimliğini ve erişim etkinliğini izleme:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resource-from-only-approved-locations"></a>3,8: Azure kaynağını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: Azure Active Directory olan Azure Batch uygulamalar için tümleşik kimlik doğrulaması YAPTıYSANıZ, IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek üzere koşullu erişim adlı konumları kullanabilirsiniz.



Azure 'da adlandırılmış konumlar nasıl yapılandırılır:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (AAD) kullanın ve AAD Ile Azure Batch uygulamalar Için kimlik doğrulamasını tümleştirin. AAD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. AAD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.


AAD örneği oluşturma ve yapılandırma:

https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance


AAD ile Batch uygulamalarının kimliğini doğrulama:

https://docs.microsoft.com/azure/batch/batch-aad-auth

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure ACTIVE DIRECTORY (AAD) eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, grup üyeliklerini verimli bir şekilde yönetmek için Azure kimlik erişimi Incelemelerini, kurumsal uygulamalara erişimi ve rol atamalarını da kullanabilirsiniz. Kullanıcıların erişimi, yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için düzenli aralıklarla gözden geçirilebilir.


Azure kimlik erişimi Incelemelerini kullanma:

https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Rehberlik**: Azure Active Directory Kullanıcı Hesapları Için Tanılama ayarları oluşturun, Denetim günlüklerini ve oturum açma günlüklerini bir Azure Log Analytics çalışma alanına gönderir. Azure Log Analytics çalışma alanında istenen uyarıları yapılandırın.


Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: otomatik yanıtları, Kullanıcı kimlikleriyle ilgili şüpheli eylemleri tespit etmek üzere yapılandırmak için Azure ACTIVE DIRECTORY (AAD) risk algılamaları ve kimlik koruması özelliğini kullanın. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz.


AAD riskli oturum açma işlemlerini görüntüleme:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins


Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme:

https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies


Azure Sentinel 'i ekleme:

https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="313-divprovide-microsoft-with-access-to-relevant-customer-data-during-support-scenariosbrdiv"></a>3,13: <div>Destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişmesine izin verin<br></div>

**Rehberlik**: kullanılamıyor; Müşteri Kasası Azure Batch için henüz desteklenmiyor. Desteklenen Müşteri Kasası hizmetleri listesi: https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability

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

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Azure Batch havuzları, sanal ağ/alt ağ ile ayrılmalıdır, uygun şekilde etiketlenebilir ve bir ağ güvenlik grupları (NSG) ile korunmuş olmalıdır. Azure Batch verileri, güvenli bir Azure depolama hesabı içinde bulunmalıdır.


Bir sanal ağ içinde Azure Batch havuzu oluşturma:

https://docs.microsoft.com/azure/batch/batch-virtual-network


Azure depolama hesaplarının güvenliğini sağlama:

https://docs.microsoft.com/azure/storage/common/storage-security-guide

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleyin ve engelleyin.

**Rehberlik**: hassas bilgiler Içeren Azure Batch havuzlarınızla Ilişkili Azure depolama hesapları Için, etiketleri kullanarak bunları gizli olarak Işaretleyin ve Azure en iyi uygulamaları ile güvenli hale getirin.


Veri tanımlama, sınıflandırma ve kayıp önleme özellikleri, Azure depolama veya işlem kaynakları için henüz kullanılamıyor. Uyumluluk amacıyla gerekliyse üçüncü taraf çözümünü uygulayın.


Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.


Azure 'da müşteri veri korumasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


Azure depolama hesaplarının güvenliğini sağlama:

https://docs.microsoft.com/azure/storage/common/storage-security-guide

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: yoldaki tüm hassas bilgileri şifreleyin. Microsoft Azure kaynaklar varsayılan olarak TLS 1,2 ' i uzlaşacaktır. Azure Batch havuzlarınıza veya veri depolarına (Azure Storage hesapları) bağlanan tüm istemcilerin TLS 1,2 veya üzerini anlaşamadığından emin olun.


Azure Batch verilerinizi içeren depolama hesabına erişmek için HTTPS 'nin gerekli olduğundan emin olun.


Aktarım sırasında Azure depolama hesabı şifrelemesini anlayın:

https://docs.microsoft.com/azure/storage/blobs/security-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: hassas bilgiler Içeren Azure Batch havuzlarınızla Ilişkili Azure depolama hesapları için, etiketleri kullanarak bunları gizli olarak Işaretleyin ve Azure en iyi uygulamaları ile güvenli hale getirin.


Veri tanımlama, sınıflandırma ve kayıp önleme özellikleri, Azure depolama veya işlem kaynakları için henüz kullanılamıyor. Uyumluluk amacıyla gerekliyse üçüncü taraf çözümünü uygulayın.


Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.


Azure 'da müşteri veri korumasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


Azure depolama hesaplarının güvenliğini sağlama:

https://docs.microsoft.com/azure/storage/common/storage-security-guide

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Rehberlik**: Batch hesabı, Batch havuzları ve depolama hesapları dahil olmak üzere Azure kaynaklarının yönetim düzlemine erişimi denetlemek için Azure ACTIVE DIRECTORY (AAD) rol tabanlı erişim denetımı (RBAC) kullanın.


Azure RBAC 'yi anlama:

https://docs.microsoft.com/azure/role-based-access-control/overview


Azure 'da RBAC 'yi yapılandırma:

https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: veri tanımlama, sınıflandırma ve kayıp önleme özellikleri, Azure depolama veya işlem kaynakları için henüz kullanılamıyor. Uyumluluk amacıyla gerekliyse üçüncü taraf çözümünü uygulayın.



Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.



Azure 'da müşteri veri korumasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: Azure Batch hesabınızla ilişkili depolama hesapları Için, Microsoft 'un şifreleme anahtarlarını yönetmesine izin vermeniz önerilir, ancak gerekirse kendi anahtarlarınızı yönetme seçeneğiniz vardır.



Azure depolama hesapları için şifreleme anahtarlarını yönetme:

https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Batch hesaplarınız/havuzlarınızla ilişkili veya bunlarla Ilişkili kritik Azure kaynaklarında değişiklik gerçekleşirken uyarı oluşturmak Için Azure etkinlik günlüğü Ile Azure izleyici 'yi kullanın.



Havuz verilerine karşı tüm CRUD işlemlerini izlemek ve günlüğe kaydetmek için Azure Batch havuzuyla ilişkili depolama hesapları için tanılama ayarlarını yapılandırın.



Azure etkinlik günlüğü olayları için uyarı oluşturma:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log



Azure depolama hesabı için ek günlüğe kaydetme/denetim nasıl etkinleştirilir:

https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: Azure Batch havuz düğümleri için, güvenlik açığı yönetimi çözümünü yönetmekten siz sorumlusunuz.


İsteğe bağlı olarak, Rapid7, Qualys veya başka bir güvenlik açığı yönetim platformu aboneliğiniz varsa, güvenlik açığı değerlendirmesi aracılarını Batch havuzu düğümlerine el ile yükleyebilirsiniz ve düğümleri ilgili portaldan yönetebilirsiniz.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik Işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: temel Azure Batch havuzu düğüm görüntülerini sürdürmek ve güncelleştirmek için Microsoft. Azure Batch havuz düğümlerinin işletim sisteminin, otomatik güncelleştirmeleri etkinleştirmeyi, düğümleri izlemeyi veya düzenli olarak yeniden başlatmalar gerçekleştirmeyi gerektirebilecek küme ömrü boyunca düzeltme eki uygulandığından emin olun.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: Azure Batch havuz düğümlerinin üçüncü taraf uygulamalarının, otomatik güncelleştirmeleri etkinleştirmeyi, düğümleri izlemeyi veya düzenli olarak yeniden başlatmalar gerçekleştirmeyi gerektirebilecek küme ömrü boyunca düzeltme eki uygulandığından emin olun.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: bir Rapid7, Qualys veya başka bir güvenlik açığı yönetim platformu aboneliğiniz varsa, geri dönüş güvenlik açıklarını görüntülemek ve karşılaştırmak için bu satıcının portalını kullanabilirsiniz.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için bir risk derecelendirme işlemi kullanın.

**Rehberlik**: ortak bir risk Puanlama programı (örneğin, ortak güvenlik açığı Puanlama sistemi) veya üçüncü taraf tarama aracınız tarafından sunulan varsayılan risk derecelendirmelerini kullanın.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: abonelikleriniz dahilinde (işlem, depolama, ağ, vb.) tüm kaynakları sorgulamak/öğrenmek Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinlere sahip olduğunuzdan ve aboneliklerinizin içindeki kaynakların yanı sıra tüm Azure aboneliklerinin numaralandırılmasını sağlayabildiğinizden emin olun.


Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilir, ancak ileriye doğru Azure Resource Manager (ARM) kaynakları oluşturmanız ve kullanılması kesinlikle önerilir.


Azure Graph ile sorgu oluşturma:

https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal


Azure aboneliklerinizi görüntüleme:

https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0


Azure RBAC 'yi anlama:

https://docs.microsoft.com/azure/role-based-access-control/overview

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



Ve Kullanıcı etiketleri oluşturma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-maintain-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini saklayın.

**Rehberlik**: onaylanan Azure kaynakları listesini ve işlem kaynakları için onaylanan yazılımı tanımlayın

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri


Aboneliklerinizin içindeki kaynakları sorgulamak/öğrenmek için Azure Kaynak grafiğini kullanın. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.


Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


Azure Graph ile sorgu oluşturma: https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: Işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: Azure Batch havuz düğümleri için, onaylanmamış yazılım uygulamalarına yönelik küme düğümlerini izlemek üzere bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: Azure Batch havuz düğümleri için, onaylanmamış yazılım uygulamalarına yönelik küme düğümlerini izlemek üzere bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: Azure Batch havuz düğümleri için, yetkisiz yazılımın yürütülmesini engelleyen bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkesini kullanın:


- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri


Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


Azure Ilkesi ile belirli bir kaynak türünü reddetme: https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: Azure Batch havuz düğümleri için, yetkisiz dosya türlerinin yürütülmesini engelleyen bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="611-divlimit-users-ability-to-interact-with-azure-resource-manager-via-scriptsdiv"></a>6,11: <div>Kullanıcıların betikler aracılığıyla Azure Resource Manager etkileşim kurma yeteneğini sınırlayın</div>

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak Için Azure koşullu erişimi kullanın.


Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma: https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların Işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz,

Bu, Azure Batch havuzlarının kullanıcıları (yönetici olmayanlar) işleri çalıştırmak için ayrı düğümlere erişmesi gerekmediğinden Azure Batch için geçerli değildir. Küme yöneticisinin tüm düğümlere kök erişimi zaten var.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: geçerli değil, kıyaslama Azure App Service veya IaaS örneklerinde çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: Azure Batch hesaplarınızın ve havuzlarınızın yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak Için "Microsoft. Batch" ad alanındaki Azure ilke diğer adlarını kullanın.


Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme:

https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0


Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-configurations-for-your-operating-system"></a>7,2: Işletim sisteminiz için güvenli yapılandırma oluşturma

**Rehberlik**: Batch havuzu düğümleriniz için güvenli yapılandırma oluşturma.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="73-maintain-secure-configurations-for-all-azure-resources"></a>7,3: tüm Azure kaynakları için güvenli yapılandırmaların bakımını yapma

**Rehberlik**: Batch hesabınızla ve havuzlarınızla ilgili Azure kaynakları için (sanal ağlar, alt ağlar, Azure Güvenlik duvarları, Azure depolama hesapları, vb.) güvenlik ayarlarını zorlamak üzere Azure ilkesi [reddetme] ve [dağıtım yok] kullanın. Özel ilkeler oluşturmak için aşağıdaki ad alanlarından Azure Ilke diğer adlarını kullanabilirsiniz:

- Microsoft.Batch
- Microsoft.Storage
- Microsoft.Network


Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


Azure Ilke efektlerini anlayın: https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-configurations-for-operating-systems"></a>7,4: Işletim sistemleri için güvenli yapılandırmaların bakımını yapma

**Rehberlik**: Microsoft tarafından yönetilen ve korunan Azure Batch havuzu Işletim sistemi görüntüleri. İşletim sistemi düzeyinde durum yapılandırması uygulamaktan siz sorumlusunuz.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Rehberlik**: Azure Batch hesaplarınız, havuzlarınız veya ilgili kaynaklarınız Için özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos kullanın.


Azure DevOps 'da kod depolama:

https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops


Azure Repos belgeleri:

https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel Işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Kılavuz**: Azure Batch havuzlarınız için özel görüntüler kullanıyorsanız, yalnızca yetkili kullanıcıların görüntülere erişebildiğinden emin olmak için rol tabanlı erişim denetımı (RBAC) kullanın.


Azure 'da RBAC 'yi anlayın:

https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles


Azure 'da RBAC 'yi yapılandırma:

https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Rehberlik**: yerleşik Azure ilke tanımlarını kullanarak Azure Batch ilgili kaynak yapılandırmasını uyarır, denetleyebilir ve zorunlu kılabilirsiniz.  Azure Batch hesaplarınız ve havuzlarınız için özel ilkeler oluşturmak üzere "Microsoft. Batch" ad alanındaki Azure Ilke diğer adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.



Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: Işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: Azure Batch havuz düğümlerinizin işletim sistemlerine yönelik istenen durumu korumak için bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma Izlemeyi uygulayın

**Rehberlik**: Azure Batch örneğinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak Için "Microsoft. Batch" ad alanındaki Azure ilke diğer adlarını kullanın. Ayrıca, Azure Batch için özel olarak oluşturulan yerleşik ilkeleri veya Azure Batch tarafından kullanılan kaynakları da kullanabilirsiniz; örneğin:

- Alt ağlar bir ağ güvenlik grubu ile ilişkilendirilmelidir-depolama hesapları bir sanal ağ hizmeti uç noktası kullanmalıdır
- Batch hesaplarındaki tanılama günlükleri etkinleştirilmelidir

Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme: https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: Işletim sistemleri için otomatik yapılandırma Izlemeyi Uygula

**Rehberlik**: Azure Batch havuz düğümlerinizin işletim sistemlerinin durumunu izlemek için bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="711-securely-manage-azure-secrets"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: Azure Key Vault, Azure depolama hesapları içindeki havuz depolama için anahtarları yönetmek üzere Azure Batch dağıtımlarıyla birlikte kullanılabilir.


Azure yönetilen kimliklerle tümleştirme:

https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity


Azure Key Vault oluşturma:

https://docs.microsoft.com/azure/key-vault/quick-create-portal


Yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama:

https://docs.microsoft.com/azure/key-vault/managed-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="712-securely-and-automatically-manage-identities"></a>7,12: kimlikleri güvenli ve otomatik olarak Yönet

**Rehberlik**: kullanılamıyor, yönetilen hizmet kimliği Azure Batch tarafından desteklenmiyor

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder. 

Kimlik bilgileri tarayıcısını ayarlama: https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: Windows işletim sistemleri durumunda tek tek Azure Batch havuz düğümlerinde Windows Defender 'ı kullanın veya Linux kullanıyorsanız kendi kötü amaçlı yazılımdan koruma çözümünüzü sağlayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel alınan konakta (örneğin, Azure Batch) etkinleştirilir, ancak müşteri içeriğinde çalışmaz.


App Service, Data Lake Storage, BLOB depolama vb. gibi işlem dışı Azure kaynaklarına yüklenen tüm dosyaları önceden tarayın. Microsoft bu örneklerdeki müşteri verilerine erişemez.


Azure Cloud Services ve sanal makineler için Microsoft kötü amaçlı yazılımdan koruma 'i anlayın:

https://docs.microsoft.com/azure/security/fundamentals/antimalware

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve Imzaların güncelleştirildiğinden emin olun

**Rehberlik**: Windows işletim sistemleri durumunda tek tek Azure Batch havuz düğümlerinde Windows Defender 'ı kullanın ve otomatik güncelleştirme özelliğinin etkinleştirildiğinden emin olun. Linux kullanıyorsanız kendi kötü amaçlı yazılımdan koruma çözümünüzü sağlayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: Azure Batch havuz veri deposu Için bir Azure depolama hesabı kullanırken, uygun artıklık seçeneğini (LRS, ZRS, GRS, RA-GRS) seçin. 


Azure depolama hesapları için depolama yedekliği nasıl yapılandırılır:

https://docs.microsoft.com/azure/storage/common/storage-redundancy

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: Azure Batch havuz veri deposu Için bir Azure depolama hesabı kullanırken, uygun artıklık seçeneğini (LRS, ZRS, GRS, RA-GRS) seçin.  Azure Batch dağıtımınızın herhangi bir bölümü için Azure Key Vault kullanıyorsanız, anahtarlarınızın yedeklendiğinden emin olun.


Azure depolama hesapları için depolama yedekliği nasıl yapılandırılır:

https://docs.microsoft.com/azure/storage/common/storage-redundancy


Azure 'da Anahtar Kasası anahtarları nasıl yedekleirsiniz:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Kılavuz**: Azure depolama hesapları için kendi anahtarlarınızı veya Azure Batch uygulamanız ile ilgili başka bir kaynak yönetiyorsanız, yedeklenen anahtarların düzenli olarak geri yüklenmesini test edin.


Azure 'da Anahtar Kasası anahtarları nasıl yedekleirsiniz:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0


Bir müşteri tarafından yönetilen anahtarı PowerShell ile geri yükleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: Azure Key Vault Azure Batch havuz depolama hesaplarıyla ilgili herhangi bir anahtarı tutmak için kullanılıyorsa, anahtarları yanlışlıkla veya kötü amaçlı olarak silinmeye karşı korumak Için Azure Key Vault geçici silme özelliğini etkinleştirin.


Azure Key Vault 'da geçici silmeyi etkinleştirme:

https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-incident-response-guide"></a>10,1: olay yanıtı oluştur Kılavuzu

**Rehberlik**: personel rollerinin yanı sıra olay işleme/yönetim aşamalarını tanımlayan yazılı olay yanıt planları olduğundan emin olun.



Azure Güvenlik Merkezi 'nde Iş akışı Otomatiklamalar nasıl yapılandırılır:

https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi yordamı oluşturma

**Rehberlik**: Güvenlik Merkezi, her uyarıya katılma sırasını önceliklendirmenize yardımcı olmak için uyarılara önem derecesi atar. böylece, bir kaynak tehlikeye girdiğinde, hemen bu işe başlayabilirsiniz. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

NıST yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin: https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-nbspfor-security-incidents"></a>10,4: güvenlik olayları için güvenlik olayı Iletişim ayrıntılarını sağlayın ve uyarı bildirimlerini yapılandırın &nbsp;

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır.



Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama:

https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e akışını sağlayabilirsiniz.


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

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-to-remediate-all-critical-security-findings-within-60-days"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini yapın ve tüm kritik güvenlik bulgularını 60 gün içinde düzeltdiğinizden emin olun.

**Rehberlik**: Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için lütfen Microsoft katılım kurallarını izleyin:

https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1.



Microsoft 'un yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına yönelik kırmızı ekip oluşturma ve canlı site sızma testini yürütmeye yönelik daha fazla bilgi edinebilirsiniz: 

https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](https://docs.microsoft.com/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
