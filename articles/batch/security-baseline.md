---
title: Toplu İşlem için Azure Güvenlik Taban Çizgisi
description: Toplu İşlem için Azure Güvenlik Taban Çizgisi
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: a9569c1f5de797c77f447b5df15e85a57e8be84b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79472937"
---
# <a name="azure-security-baseline-for-batch"></a>Toplu İşlem için Azure Güvenlik Taban Çizgisi

Toplu İşlem için Azure Güvenlik Taban Çizgisi, dağıtımınızın güvenlik duruşunu geliştirmenize yardımcı olacak öneriler içerir.

Bu hizmetlerin temeli, en iyi uygulamalar kılavuzumuzla Azure'da bulut çözümlerinizi nasıl güvenebileceğinize ilişkin öneriler sunan [Azure Güvenlik Kıyaslama sürümü 1.0'dan](https://docs.microsoft.com/azure/security/benchmarks/overview)alınmıştır.

Daha fazla bilgi için [Azure Güvenlik Taban Çizgilerini genel olarak](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)görün.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için [Güvenlik Denetimi: Ağ Güvenliği'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)bakın.*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Sanal Ağınızdaki Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'nı kullanarak kaynakları koruyun

**Yönerge**: Azure Toplu Iş havuzu(lar)ı sanal ağ içinde dağıtın. Havuz bilgi işlem düğümlerinin diğer sanal makinelerle veya şirket içi bir ağla güvenli bir şekilde iletişim kurmasına izin vermek için, havuzu bir Azure sanal ağının alt ağında sağlayabilirsiniz. Ayrıca, Havuzunuzu sanal ağ içinde dağıtmak, tek tek düğümlerin ağ arabirimlerinin (NIC) yanı sıra alt ağı güvenli hale getirmek için kullanılan ağ güvenlik grubu (NSG) üzerinde denetim sağlar. NSG'yi, Internet'te yalnızca güvenilen IP(ler)/konumlardan gelen trafiğe izin verecek şekilde yapılandırın.


Sanal Ağ içinde Azure Toplu Birleştirme Havuzu oluşturma:

https://docs.microsoft.com/azure/batch/batch-virtual-network

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Vnets, Subnets ve NICS'in yapılandırmasını ve trafiğini izleyin ve kaydedin

**Kılavuz**: Toplu Iş havuzunuzla ilişkili sanal ağ/ ağ güvenlik grubu (NSG) ile ilgili Azure Güvenlik Merkezi'ni ve ağ koruma önerilerini düzeltin. Toplu Iş havuzunuzu korumak için kullanılan NSG akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir Azure Depolama Hesabına gönderin. Ayrıca NSG akış günlüklerini bir Azure Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzdaki trafik akışıhakkında öngörüler sağlamak için Azure Trafik Analizi'ni kullanabilirsiniz. Azure Trafik Analitiği'nin bazı avantajları, ağ etkinliğini görselleştirebilme ve etkin noktaları belirleyebilme, güvenlik tehditlerini belirleyebilme, trafik akışı desenlerini anlama ve ağ yanlış yapılandırmalarını saptabilme yeteneğidir.


NSG Akış Günlükleri Nasıl Etkinleştirilir:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal


Trafik Analizi ni etkinleştirme ve kullanma:

https://docs.microsoft.com/azure/network-watcher/traffic-analytics


Azure Güvenlik Merkezi tarafından sağlanan Ağ Güvenliğini Anlayın:

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="13-protect-critical-web-applications"></a>1.3: Kritik Web Uygulamalarını Koruyun

**Kılavuz**: Geçerli değildir, Benchmark Azure Uygulama Hizmeti veya IaaS örneklerinde çalışan web uygulamaları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Bilinen Kötü Amaçlı IP Adresleriyle İletişimi İnkar Etme

**Kılavuz :** DDoS saldırılarına karşı korunmak için Azure Toplu İşlem havuzunuzu koruyan sanal ağda Azure DDoS (dağıtılmış hizmet reddi) Standart korumayı etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimi reddetmek için Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı kullanın.


DDoS koruması nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection


Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı Anlayın:

https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1.5: Ağ Paketlerini ve Akış Günlüklerini Kaydetme

**Yönerge**: Azure Toplu İş havuzunuzu korumak için kullanılan ağ güvenlik grubunda (NSG) akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir Azure Depolama Hesabı'na gönderin.


NSG Akış Günlükleri Nasıl Etkinleştirilir:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems"></a>1.6: Ağ Tabanlı Saldırı Algılama/Saldırı Önleme Sistemleri

**Yönlendirme**: Uyumluluk amacıyla gerekliyse, Azure Marketi'nden, yük denetimi yetenekleriyle izinsiz giriş algılama sistemleri (IDS) ve izinsiz giriş önleme sistemleri (IPS) işlevlerini destekleyen bir ağ sanal cihazı seçin.


Yük denetimine dayalı izinsiz giriş algılama ve/veya önleme bir gereklilik değilse, tehdit istihbaratına sahip Azure Güvenlik Duvarı kullanılabilir. Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme, bilinen kötü amaçlı IP adreslerine ve etki alanlarındaki trafiği uyarabilir ve reddedebilir. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından kaynaklanmaktadır.


Azure Toplu Havuz düğümlerinizle aynı sanal ağda herkese açık bir IP adresiyle Azure Güvenlik Duvarı'nı dağıtın. Internet'teki güvenilen konumlar ile bireysel havuz düğümlerinizin özel IP adresleri arasında ağ adresi çevirisi (NAT) kurallarını yapılandırın. Azure Güvenlik Duvarı'nda, Tehdit İstihbaratı altında, bilinen kötü amaçlı IP adreslerine ve etki adlarına/gelen trafiği uyarmak ve engellemek için "Uyarı ve reddet" yapılatın. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından elde edilir ve yalnızca en yüksek güven kayıtları dahildir. 


Sanal Ağ içinde Azure Toplu Birleştirme Havuzu oluşturma:

https://docs.microsoft.com/azure/batch/batch-virtual-network


Azure Güvenlik Duvarı nasıl dağıtılır:

https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal


Azure Marketi:

https://azuremarketplace.microsoft.com/marketplace/?term=Firewall

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="17-manage-traffic-to-your-web-applications"></a>1.7: Web uygulamalarınızdaki trafiği yönetme

**Kılavuz**: Geçerli değildir, Benchmark Azure Uygulama Hizmeti veya IaaS örneklerinde çalışan web uygulamaları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Ağ güvenliği kurallarının karmaşıklığını ve yönetim ek lerini en aza indirmek

**Kılavuz**: Ağ güvenlik gruplarındaki ağ erişim denetimlerini veya Azure Toplu Iş havuzunuzun (lar) ilişkili Azure Güvenlik Duvarlarını tanımlamak için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında servis etiketi adını (örn. ApiManagement) belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz. Microsoft, hizmet etiketinin kapsadığı adres öneklerini yönetir ve adresler değiştikçe servis etiketini otomatik olarak güncelleştirir.


Hizmet etiketlerini anlama ve kullanma:

https://docs.microsoft.com/azure/virtual-network/service-tags-overview

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Ağ Aygıtları için Standart Güvenlik Yapılandırmalarını Koruyun

**Yönerge**: Azure Toplu İş havuzunuzla ilişkili ağ kaynakları için Azure İlkesi ile standart güvenlik yapılandırmaları tanımlayın ve uygulayın. Azure Toplu Iş havuzlarınızın ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.Batch" ve "Microsoft.Network" ad alanlarında Azure İlkesi takma adlarını kullanın.



Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="110-document-traffic-configuration-rules"></a>1.10: Belge Trafiği Yapılandırma Kuralları

**Kılavuz**: Ağ hizmet grupları (NSG'ler) ve Azure toplu iş havuzlarınızla ilişkili ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için etiketlerkullanın. Tek tek NSG kuralları için, bir ağa/aktan trafiğin alınmasına izin veren kurallar için iş gereksinimini ve/veya süresini (vb.) belirtmek için "Açıklama" alanını kullanın.


Tüm kaynakların etiketlerle oluşturulduğundan emin olmak ve varolan etiketlenmemiş kaynakları size bildirmek için etiketlemeyle ilgili "Etiket ve değeri gerektir" gibi yerleşik Azure ilke tanımlarından herhangi birini kullanın.


Azure PowerShell veya Azure CLI'yi, etiketlerine dayalı kaynaklara bakmak veya eylemler gerçekleştirmek için kullanabilirsiniz.


Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags


Sanal ağ nasıl oluşturulur:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal


NSG nasıl oluşturulur:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Ağ Kaynak Yapılandırmalarını İzlemek ve Değişiklikleri Algılamak için Otomatik Araçlar Kullanın

**Yönerge**: Ağ kaynak yapılandırmalarını izlemek ve Azure Toplu Iş havuzlarınızla ilgili ağ kaynaklarının değişikliklerini algılamak için Azure Etkinlik Günlüğü'ne kullanın. Azure Monitor'da kritik ağ kaynaklarında değişiklikler olduğunda tetikleyecek uyarılar oluşturun.

Azure Etkinlik Günlüğü etkinliklerini görüntüleme ve alma:https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view 

Azure Monitor'da uyarı oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için [Güvenlik Denetimi: Günlüğe kaydetme ve izleme.](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Onaylı Zaman Senkronizasyon Kaynaklarını Kullanma

**Kılavuz**: Azure Toplu İş için varsayılan olarak Microsoft zaman eşitleme sağlar. Ancak, belirli zaman eşitleme gereksinimleriniz varsa, bu değişiklikleri uygulayabilirsiniz.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Merkezi Güvenlik Günlüğü Yönetimini Yapılandırma

**Kılavuz**: Küme aygıtları tarafından oluşturulan güvenlik verilerini toplamak için Azure Toplu İş hesabından Azure Monitor'a aktarılır. Çevredeki tehditleri algılamak ve yanıtlamak için özel sorgulardan yararlanın.  Azure Toplu Iş Düzeyi düzeyi izleme için, iş, görev, düğüm ve havuzlar da dahil olmak üzere kaynaklarınızın durumunu izlemek veya sorgulamak için Toplu İş API'lerini kullanın.



Azure Toplu İş hesabından Azure Monitor'a nasıl binilir:

https://docs.microsoft.com/azure/batch/batch-diagnostics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure Kaynakları için denetim günlüğe kaydetmeyi etkinleştirme

**Kılavuz**: Azure Toplu Hesap düzeyi izleme için, Azure Monitor özelliklerini kullanarak her Toplu hesabı izleyin. Azure Monitor, havuzlar, işler ve görevler gibi Toplu Iş hesabı düzeyinde kapsama giren kaynaklar için ölçümler ve isteğe bağlı tanılama günlükleri toplar. Toplu Iş hesabınızdaki etkinlikleri izlemek ve sorunları tanılamak için bu verileri el ile veya programlı olarak toplayın ve tüketin.


Azure Toplu Iş düzeyi izleme için, iş, görev, düğüm ve havuzlar da dahil olmak üzere kaynaklarınızın durumunu izlemek veya sorgulamak için Azure Toplu İş API'lerini kullanın.


Azure Toplu Iş Düzeyi izleme ve günlüğe kaydetme nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/batch/monitoring-overview


Toplu kaynak düzeyinde izlemeyi anlayın:

https://docs.microsoft.com/azure/batch/monitoring-overview#batch-resource-monitoring

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="24-collect-security-logs-from-operating-system"></a>2.4: İşletim Sisteminden Güvenlik Günlüklerini Topla

**Yönlendirme**: Azure Monitor, Azure Toplu İş hesabınızdaki kaynaklar için ölçümler ve tanılama günlükleri toplar. Azure Toplu İş hesabınızı izlemek ve sorunları tanılamak için bu verileri çeşitli yollarla toplayın ve tüketin. Bir metrik belirli bir değere ulaştığında bildirimleri almak için metrik uyarıları da yapılandırabilirsiniz.


Gerekirse, yerel işletim sistemi günlüklerine erişmek için Güvenli Kabuk (SSH) veya Uzak Masaüstü Protokolü (RDP) aracılığıyla bireysel havuz düğümlerinize bağlanabilirsiniz.


Azure Toplu İş hesabınızdan tanılama günlükleri nasıl toplanır:

https://docs.microsoft.com/azure/batch/batch-diagnostics#batch-diagnostics


Azure Toplu Iş havuzu düğümlerinize uzaktan nasıl bağlanabilirsiniz:

https://docs.microsoft.com/azure/batch/batch-api-basics#error-handling

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="25-configure-security-log-storage-retention"></a>2.5: Güvenlik Günlüğü Depolama Tutma Yapılandırma

**Kılavuz**: Azure Monitor'a Dahili Azure Toplu Hesabı. Kullanılan Azure Günlük Analizi çalışma alanının kuruluşunuzun uyumluluk yönetmeliklerine göre günlük saklama süresi ne zaman ayarlandığından emin olun


Azure Toplu İşlem izleme ve günlüğe kaydetme nasıl yapılandırılır:

https://docs.microsoft.com/azure/batch/monitoring-overview


Azure Günlük Analizi çalışma alanı bekletme süresi nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2.6: İzleme ve İnceleme Günlükleri

**Yönerge**: Belirli bir metnin değeri belirli bir eşiği geçtiğinde tetikleyen Azure Toplu Iş metrik uyarıları oluşturun.


Azure Toplu Iş metrik uyarıları nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/batch/batch-diagnostics

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Anormal Etkinlik Uyarıları Etkinleştir

**Yönerge**: Belirli bir metnin değeri belirli bir eşiği geçtiğinde tetikleyen Azure Toplu Iş metrik uyarıları oluşturun.


Azure Toplu Iş metrik uyarıları nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/batch/batch-diagnostics

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="28-centralize-anti-malware-logging"></a>2.8: Kötü Amaçlı Yazılımdan Koruma Günlüğü'ne Merkezileştirme

**Rehberlik**: Windows işletim sistemleri söz konusu olduğunda tek tek toplu iş düğümlerinizde Windows Defender'ı kullanın veya Linux kullanıyorsanız kendi kötü amaçlı yazılımdan koruma çözümünüzü sağlayın.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="29-enable-dns-query-logging"></a>2.9: DNS Sorgu Günlüğe Kaydetmeyi Etkinleştir

**Rehberlik**: dns günlüğü için üçüncü taraf çözüm uygulama

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="210-enable-command-line-audit-logging"></a>2.10: Komut Satırı Denetim Günlüğe Kaydetmeyi Etkinleştir

**Kılavuz**: Konsol günlüğe kaydetmeyi ve PowerShell transkripsiyonlarını düğüm başına göre el ile yapılandırın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için [güvenlik denetimi: kimlik ve erişim denetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)bakın.*

### <a name="31-maintain-inventory-of-administrative-accounts"></a>3.1: İdari Hesapların Envanterini Koruyun

**Yönerge**: Azure Toplu İş havuzunun yanı sıra oluşturduğunuz diğer hesapların sağlanması sırasında oluşturulan yerel yönetim hesabının kaydını koruyun. Ayrıca, Azure Etkin Dizin (AAD) tümleştirmesi kullanılıyorsa, AAD'de açıkça atanması gereken ve bu nedenle sorgulanabilen yerleşik roller vardır. Yönetim gruplarına üye hesapları bulmak için geçici sorgular gerçekleştirmek için AAD PowerShell modüllerini kullanın.


Ayrıca, Azure Güvenlik Merkezi Kimliği ve Erişim Yönetimi önerilerini de kullanabilirsiniz.


Nasıl PowerShell ile AAD bir dizin rolü almak için:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0


PowerShell ile AAD'de dizin rolü ne kadar çok şey elde edilir:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0


Azure Güvenlik Merkezi ile kimlik ve erişim nasıl izlenir:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Geçerli olduğu Durumlarda Varsayılan Parolaları Değiştir

**Yol gösterici**: Azure Toplu İş havuzu nu oluştururken, yerel makine hesapları oluşturma seçeneği sunulur. Değiştirilebilen varsayılan parola yoktur, ancak Güvenli Kabuk (SSH) ve Uzak Masaüstü Protokolü (RDP) erişimi için farklı parolalar belirtebilirsiniz. Azure Toplu Birleştirme Havuzu yapılandırıldıktan sonra, Azure portalındaki tek tek düğümler için veya Azure Kaynak Yöneticisi API aracılığıyla rasgele bir kullanıcı oluşturabilirsiniz.


Belirli bir bilgi işlem düğümüne kullanıcı ekleme:

https://docs.microsoft.com/rest/api/batchservice/computenode/adduser

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="33-ensure-the-use-of-dedicated-administrative-accounts"></a>3.3: Özel İdari Hesapların Kullanımını Sağlamak

**Kılavuz**: Azure Toplu İş Uygulamaları için Kimlik Doğrulamasını Azure Etkin Dizini ile tümleştirin. Özel yönetim hesaplarının kullanımı yla ilgili ilke ler ve yordamlar oluşturun.


Ayrıca, Azure Güvenlik Merkezi Kimliği ve Erişim Yönetimi önerilerini de kullanabilirsiniz.


Azure Active Directory ile Toplu Iş uygulamalarının kimliğinin doğrulanması:

https://docs.microsoft.com/azure/batch/batch-aad-auth


Azure Güvenlik Merkezi ile kimlik ve erişim nasıl izlenir:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="34-utilize-single-sign-on-sso-with-azure-active-directory"></a>3.4: Azure Active Directory ile Tek Oturum Açma (SSO) kullanımı

**Kılavuz**: Geçerli değildir, Azure Toplu İşlem Azure AD kimlik doğrulamasını desteklerken, tek oturum açma desteklenmez.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="35-use-multifactor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Tüm Azure Active Directory tabanlı erişim için Çok Faktörlü Kimlik Doğrulama'yı kullanın.

**Kılavuz**: Azure Toplu İş Uygulamaları için Kimlik Doğrulamasını Azure Etkin Dizini (AAD) ile tümleştirin. AAD çok faktörlü kimlik doğrulamasını (MFA) etkinleştirin ve Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerini izleyin.
 


Azure'da MFA nasıl etkinleştirilir:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted


Azure Güvenlik Merkezi'nde kimlik ve erişim nasıl izlenir:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Tüm İdari Görevler için Özel Makineleri (Ayrıcalıklı Erişim İş İstasyonları) Kullanın

**Yönlendirme**: Azure Toplu İş kaynaklarınızda oturum açmak ve yapılandırmak üzere yapılandırılan çok faktörlü kimlik doğrulama (MFA) ile PAW'ları (ayrıcalıklı erişim iş istasyonları) kullanın.


Ayrıcalıklı Erişim İş İstasyonları hakkında bilgi edinin:

https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations


Azure'da MFA nasıl etkinleştirilir:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: İdari Hesaplardan Şüpheli Faaliyette Oturum açma ve Uyarı

**Kılavuz**: Azure Toplu İş Uygulamaları için Azure Toplu İş Dizini (AAD) ile tümleşik kimlik doğrulaması varsa, çevrede şüpheli veya güvenli olmayan etkinlik oluştuğunda günlük ve uyarı oluşturma için Azure Active Directory güvenlik raporlarını kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi'ni kullanın.


Riskli etkinlikler için işaretlenen Azure AD kullanıcılarını nasıl tanımlarım:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk


Azure Güvenlik Merkezi'nde kullanıcıların kimlik ve erişim etkinliğini izleme:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resource-from-only-approved-locations"></a>3.8: Azure Kaynağını yalnızca Onaylanan Konumlardan Yönetme

**Kılavuz**: Azure Toplu İş Uygulamaları için Azure Toplu İş Dizini ile tümleşik kimlik doğrulaması varsa, Yalnızca IP adresi aralıkları veya ülke/bölgelerinin belirli mantıksal gruplandırmalarından erişime izin vermek için Koşullu Erişim Adlandırılmış Konumlar'ı kullanabilirsiniz.



Azure'da Adlandırılmış Konumlar nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="39-use-azure-active-directory"></a>3.9: Azure Etkin Dizini Kullanma

**Rehberlik**: Merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory 'i (AAD) kullanın ve Azure Toplu İş Uygulamaları için Kimlik Doğrulama'yı AAD ile tümleştirin. AAD, istirahatte ve aktarım sırasında veriler için güçlü şifreleme kullanarak verileri korur. AAD ayrıca kullanıcı kimlik bilgilerini tuzlar, iş lerle ve güvenli bir şekilde saklar.


AAD örneğini oluşturma ve yapılandırma:

https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance


AAD ile Toplu iş uygulamalarının doğrulaması:

https://docs.microsoft.com/azure/batch/batch-aad-auth

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Kullanıcı Erişimini Düzenli Olarak Gözden Geçirin ve Uzlaştırın

**Rehberlik**: Azure Active Directory (AAD), eski hesapları keşfetmeye yardımcı olmak için günlükler sağlar. Ayrıca, grup üyeliklerini, kurumsal uygulamalara erişimi ve rol atamalarını verimli bir şekilde yönetmek için Azure Kimlik Erişim İncelemelerini kullanabilirsiniz. Kullanıcıların erişimi, yalnızca doğru kullanıcıların sürekli erişime sahip olduğundan emin olmak için düzenli olarak gözden geçirilebilir.


Azure Kimlik Erişim İncelemeleri nasıl kullanılır:

https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Devre Dışı Bırakılan Hesaplara Erişim Denemelerini İzle

**Kılavuz**: Azure Active Directory kullanıcı hesapları için Tanılama Ayarları oluşturun, denetim günlüklerini ve oturum açma günlüklerini Azure Log Analytics çalışma alanına gönderin. Azure Log Analytics çalışma alanı içinde istediğiniz Uyarıları yapılandırın.


Azure Etkinlik Günlükleri Azure Monitör'e nasıl entegre eleştirilir:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Hesap Giriş Davranış Sapması Uyarısı

**Kılavuz :** Kullanıcı kimlikleri ile ilgili algılanan şüpheli eylemlere otomatik yanıtları yapılandırmak için Azure Active Directory (AAD) Risk Algılamaları ve Kimlik Koruması özelliğini kullanın. Ayrıca, daha fazla araştırma için Azure Sentinel'e veri sindirebilirsiniz.


AAD riskli oturum açma nasıl görüntülenebilir:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins


Kimlik Koruması risk ilkelerinin nasıl yapılandırılabilen ve etkinleştirilir:

https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies


Azure Sentinel'e nasıl binilir:

https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="313-divprovide-microsoft-with-access-to-relevant-customer-data-during-support-scenariosbrdiv"></a>3.13: <div>Destek senaryoları sırasında Microsoft'a ilgili müşteri verilerine erişim sağlama<br></div>

**Rehberlik**: Mevcut değil; Müşteri Kilit Kutusu henüz Azure Toplu İş için desteklenmedi. Müşteri Lockbox desteklenen hizmetler listesi:https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability

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

**Rehberlik**: Geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Azure Toplu Havuzlar sanal ağ/alt ağ ile ayrılmalı, uygun şekilde etiketlenmelidir ve bir ağ güvenlik grupları (NSG) ile güvenli hale edilmelidir. Azure Toplu İşlem verileri güvenli bir Azure Depolama Hesabı içinde yer almalıdır.


Sanal Ağ içinde Azure Toplu Birleştirme Havuzu oluşturma:

https://docs.microsoft.com/azure/batch/batch-virtual-network


Azure Depolama Hesapları nasıl güvenli hale ilir:

https://docs.microsoft.com/azure/storage/common/storage-security-guide

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Hassas bilgilerin yetkisiz aktarımının izlenmesi ni ve engellenmesi.

**Kılavuz :** Azure Toplu İş Havuzunuzla ilişkili ve hassas bilgiler içeren Azure Depolama Hesapları(lar) için, Etiketler'i kullanarak bunları hassas olarak işaretleyin ve bunları Azure en iyi uygulamalarıyla güvence altına alın.


Azure Depolama veya bilgi işlem kaynakları için veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz kullanılamıyor. Uyumluluk amacıyla gerekirse üçüncü taraf çözümlerini uygulayın.


Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.


Azure'da müşteri verilerinin korunmasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


Azure Depolama Hesapları nasıl güvenli hale ilir:

https://docs.microsoft.com/azure/storage/common/storage-security-guide

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Tüm hassas bilgileri taşıma sırasında şifreleme

**Rehberlik**: Tüm hassas bilgileri geçiş sırasında şifreleyin. Microsoft Azure kaynakları TLS 1.2'yi varsayılan olarak görüşür. Azure Toplu Toplu Havuzlarınıza veya veri depolarınıza (Azure Depolama Hesapları) bağlanan tüm istemcilerin TLS 1,2 veya daha büyük bir anlaşma sağlayabilmesini sağlayın.


Azure Toplu İş verilerinizi içeren Depolama Hesabına erişmek için HTTPS'nin gerekli olduğundan emin olun.


Toplu Taşıma sırasında Azure Depolama Hesabı Şifrelemesini Anlayın:

https://docs.microsoft.com/azure/storage/blobs/security-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Hassas verileri tanımlamak için etkin bir bulma aracı kullanın

**Kılavuz**: Azure Toplu İş Havuzunuzla ilişkili ve hassas bilgiler içeren Azure Depolama Hesapları(lar) için, etiketleri kullanarak bunları hassas olarak işaretleyin ve bunları Azure en iyi uygulamalarıyla güvence altına alın.


Azure Depolama veya bilgi işlem kaynakları için veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz kullanılamıyor. Uyumluluk amacıyla gerekirse üçüncü taraf çözümlerini uygulayın.


Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.


Azure'da müşteri verilerinin korunmasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data


Azure Depolama Hesapları nasıl güvenli hale ilir:

https://docs.microsoft.com/azure/storage/common/storage-security-guide

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Kaynaklara erişimi denetlemek için Azure RBAC'ı kullanın

**Rehberlik**: Toplu İş Hesabı, Toplu Havuz(lar) ve Depolama Hesapları gibi Azure kaynaklarının yönetim düzlemine erişimi denetlemek için Azure Active Directory(AAD) Role tabanlı erişim denetimini (RBAC) kullanın.


Azure RBAC'ı anlayın:

https://docs.microsoft.com/azure/role-based-access-control/overview


Azure'da RBAC nasıl yapılandırılmaz:

https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Erişim denetimini sağlamak için ana bilgisayar tabanlı Veri Kaybı Önleme'yi kullanın

**Yönlendirme**: Azure Depolama veya bilgi işlem kaynakları için veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz kullanılamıyor. Uyumluluk amacıyla gerekirse üçüncü taraf çözümlerini uygulayın.



Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.



Azure'da müşteri verilerinin korunmasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Hassas Bilgileri Istirahatte Şifreleme

**Yönerge**: Azure Toplu İş hesabınızla ilişkili depolama hesapları için, Microsoft'un şifreleme anahtarlarını yönetmesine izin vermeniz önerilir, ancak gerekirse kendi anahtarlarınızı yönetme seçeneğiniz vardır.



Azure Depolama Hesapları için şifreleme anahtarlarını yönetme:

https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Kritik Azure kaynaklarında yapılan değişiklikleri kaydedin ve uyarı

**Yönerge**: Azure Toplu İş hesaplarınızla/havuzlarınızla ilgili veya ilişkili kritik Azure kaynaklarında ne zaman değişiklikler yapılacağına ilişkin uyarılar oluşturmak için Azure Etkinlik Günlüğü ile Azure Monitörünü kullanın.



Tüm CRUD işlemlerini havuz verilerine karşı izlemek ve günlüğe kaydetmek için Azure Toplu Birleştirme Havuzu ile ilişkili Depolama Hesapları için Tanılama Ayarlarını yapılandırın.



Azure Etkinlik Günlüğü etkinlikleri için uyarılar oluşturma:

https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log



Azure Depolama Hesabı için ek günlüğe kaydetme/denetlemeyi etkinleştirme:

https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için [Güvenlik Denetimi: Güvenlik Açığı Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)bakın.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Otomatik Güvenlik Açığı Tarama Araçlarını Çalıştırın

**Kılavuz**: Azure Toplu Birleştirme düğümleri için güvenlik açığı yönetimi çözümlerini yönetmeksizin sorumlusunuz.


İsteğe bağlı olarak, Rapid7, Qualys veya başka bir güvenlik açığı yönetim platformu aboneliğiniz varsa, Toplu havuz düğümlerine güvenlik açığı değerlendirme aracılarını el ile yükleyebilir ve düğümleri ilgili portal üzerinden yönetebilirsiniz.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Otomatik İşletim Sistemi Yama Yönetimi Çözümü Dağıtmak

**Kılavuz :** Microsoft, temel Azure Toplu Havuz düğümü görüntülerini korumak ve güncelleştirmek için. Azure Toplu Birleştirme Düğümü düğümlerinin işletim sisteminin, otomatik güncelleştirmeleri etkinleştirme, düğümleri izleme veya periyodik yeniden başlatmalar gerçekleştirme gerektirebilecek küme ömrü boyunca yamalı kalmasını sağlayın.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Paylaşılan

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3: Otomatik Üçüncü Taraf Yazılım Yama Yönetimi Çözümü Dağıt

**Kılavuz :** Azure Toplu Birleştirme düğümlerinin üçüncü taraf uygulamalarının, otomatik güncelleştirmeleri etkinleştirme, düğümleri izleme veya periyodik yeniden başlatma lar gerçekleştirme gerektirebilecek küme ömrü boyunca yamalı kalmasını sağlayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Arka arkaya Güvenlik Açığı Taramalarını Karşılaştırın

**Kılavuz**: Rapid7, Qualys veya başka bir güvenlik açığı yönetim platformu aboneliğiniz varsa, arka arkaya güvenlik açığı taramalarını görüntülemek ve karşılaştırmak için satıcının portalını kullanabilirsiniz.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Keşfedilen güvenlik açıklarının düzeltilmesine öncelik vermek için bir risk derecelendirme işlemi kullanın.

**Yönlendirme**: Ortak bir risk puanlama programı (örneğin, Ortak Güvenlik Açığı Puanlama Sistemi) veya üçüncü taraf tarama aracınız tarafından sağlanan varsayılan risk derecelendirmeleri kullanın.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için [güvenlik denetimi: Envanter ve Varlık Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)bakın.*

### <a name="61-use-azure-asset-discovery"></a>6.1: Azure Varlık Bulma'yı kullanma

**Yönerge**: Aboneliğinizdeki tüm kaynakları (bilgi işlem, depolama, ağ vb.) sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın. Kiracınızda uygun (okuma) izinlere sahip olduğundan ve aboneliklerinizdeki tüm Azure aboneliklerinin yanı sıra kaynakları sıralayabildiğinizden emin olun.


Azure Kaynak Grafiği Gezgini aracılığıyla klasik Azure kaynakları keşfedilebilir, ancak ileriye dönük Olarak Azure Kaynak Yöneticisi (ARM) kaynaklarının oluşturulması ve kullanılması önerilir.


Azure Kaynak Grafiği Gezgini ile sorgu oluşturma:

https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal


Azure Aboneliklerinizi görüntüleme:

https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0


Azure RBAC'ı anlayın:

https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="62-maintain-asset-metadata"></a>6.2: Varlık Meta Verilerini Koruyun

**Yönerge**: Meta verileri mantıksal olarak taksonomi olarak düzenlemek için meta veriler veren Azure kaynaklarına etiketler uygulayın.


Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Yetkisiz Azure Kaynaklarını Silme

**Rehberlik**: Varlıkları düzenlemek ve izlemek için etiketlemeyi, yönetim gruplarını ve uygun olduğu durumlarda ayrı abonelikleri kullanın. Envanteri düzenli olarak uzlaştırın ve yetkisiz kaynakların abonelikten zamanında silindiğinden emin olun.



Ek Azure abonelikleri oluşturma:

https://docs.microsoft.com/azure/billing/billing-create-subscription



Yönetim Grupları nasıl oluşturulur:

https://docs.microsoft.com/azure/governance/management-groups/create



Etiketler nasıl oluşturulur ve kullanılır:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="64-maintain-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: Onaylanan Azure kaynaklarının ve yazılım başlıklarının envanterini koruyun.

**Yönerge**: İşlem kaynakları için onaylanmış Azure kaynaklarının ve onaylı yazılımların listesini tanımlayın

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Onaylanmamış Azure Kaynakları için Monitör

**Yönerge**: Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri


Aboneliğinizdeki kaynakları sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.


Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


Azure Kaynak Grafiği Gezgini ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Bilgi İşlem Kaynakları İçinde onaylanmamış Yazılım Uygulamaları için Monitör

**Kılavuz**: Azure Toplu Birleştirme düğümleri için, onaylanmamış yazılım uygulamaları için küme düğümlerini izlemek için bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Onaylanmamış Azure Kaynaklarını ve Yazılım Uygulamalarını Kaldırma

**Kılavuz**: Azure Toplu Birleştirme düğümleri için, onaylanmamış yazılım uygulamaları için küme düğümlerini izlemek için bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="68-use-only-approved-applications"></a>6.8: Yalnızca onaylı uygulamaları kullanın

**Kılavuz**: Azure Toplu Birleştirme düğümleri için, yetkisiz yazılımın yürütülmesini önlemek için bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="69-use-only-approved-azure-services"></a>6.9: Yalnızca onaylanmış Azure Hizmetlerini kullanma

**Yönerge**: Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure ilkesini kullanın:


- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri


Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


Azure İlkesi ile belirli bir kaynak türü nasıl reddedilecek:https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="610-implement-approved-application-list"></a>6.10: Onaylı başvuru listesini uygulama

**Kılavuz**: Azure Toplu Birleştirme düğümleri için, yetkisiz dosya türlerinin yürütülmesini önlemek için bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="611-divlimit-users-ability-to-interact-with-azure-resource-manager-via-scriptsdiv"></a>6.11: <div>Kullanıcıların Komut Dosyaları aracılığıyla Azure Kaynak Yöneticisi ile etkileşim deşme lerini sınırlandırın</div>

**Yönerge**: "Microsoft Azure Yönetimi" Uygulaması için "Erişimi engelle" yapılandırmayı yaparak kullanıcıların Azure Kaynak Yöneticisi ile etkileşim edebilmelerini sınırlamak için Azure Koşullu Erişim'i kullanın.


Koşullu Erişim'i Azure Kaynak Yöneticisi'ne erişimi engellemek için yapılandırma:https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Kullanıcıların Bilgi İşlem Kaynakları İçinde Ki Komut Dosyalarını Yürütme Yeteneğini Sınırlayın

**Rehberlik**: Geçerli değil,

Azure Toplu İş havuzlarının kullanıcılarının (yönetici olmayanlar) işleri çalıştırmak için tek tek düğümlere erişmelerine gerek olmadığından, bu Azure Toplu İş için geçerli değildir. Küme yöneticisi zaten tüm düğümlere kök erişimi vardır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Yüksek Riskli Uygulamaları Fiziksel veya Mantıksal Olarak Ayırın

**Kılavuz**: Geçerli değildir, Benchmark Azure Uygulama Hizmeti veya IaaS örneklerinde çalışan web uygulamaları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için güvenlik [denetimi: Güvenli Yapılandırma'ya](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration)bakın.*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Tüm Azure Kaynakları için Güvenli Yapılandırmalar Oluşturma

**Yönerge**: Azure Toplu Iş birimi hesaplarınızın ve havuzlarınızın yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.Batch" ad alanında Azure İlkesi takma adlarını kullanın.


Kullanılabilir Azure İlkesi Diğer Adları nasıl görüntüleyebilirsiniz:

https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0


Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="72-establish-secure-configurations-for-your-operating-system"></a>7.2: İşletim Sisteminiz için Güvenli Yapılandırmalar Kurun

**Kılavuz :** Toplu Havuz düğümlerinizin işletim sistemi için güvenli yapılandırmalar kurun.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="73-maintain-secure-configurations-for-all-azure-resources"></a>7.3: Tüm Azure Kaynakları için Güvenli Yapılandırmaları Koruyun

**Yönlendirme**: Toplu Iş hesabınız ve havuzlarınızla ilgili Azure kaynakları (sanal ağlar, alt ağlar, Azure Güvenlik Duvarları, Azure Depolama Hesapları vb.) için güvenli ayarlar uygulamak için Azure ilkesini [reddet] ve [varsa dağıt] kullanın. Özel ilkeler oluşturmak için aşağıdaki ad alanlarından Azure İlkesi Takma Adları'nı kullanabilirsiniz:

- Microsoft.Batch
- Microsoft.Storage
- Microsoft.Network


Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


Azure İlke Efektlerini Anlama:https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="74-maintain-secure-configurations-for-operating-systems"></a>7.4: İşletim Sistemleri için Güvenli Yapılandırmaları Koruyun

**Rehberlik**: Azure Toplu Havuz İşletim Sistemi Görüntüleri Microsoft tarafından yönetilir ve korunur. İşletim sistemi düzeyinde durum yapılandırması uygulamaktan siz sorumlusunuz.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure Kaynaklarının Yapılandırmayı Güvenli Bir Şekilde Depolama

**Kılavuz**: Azure Toplu İş hesaplarınız, havuzlarınız veya ilgili kaynaklarınız için özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos'unu kullanın.


Azure DevOps'lerde kod depolama:

https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops


Azure Depobelgeleri:

https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Özel İşletim Sistemi Görüntülerini Güvenli Bir Şekilde Saklayın

**Kılavuz**: Azure Toplu Toplu Havuzlarınız için özel resimler kullanıyorsanız, resimlere yalnızca yetkili kullanıcıların erişebilmesini sağlamak için Rol tabanlı erişim denetimini (RBAC) kullanın.


Azure'da RBAC'ı anlayın:

https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles


Azure'da RBAC nasıl yapılandırılmaz:

https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="77-deploy-system-configuration-management-tools"></a>7.7: Sistem Yapılandırma Yönetim Araçlarını Dağıtma

**Yönerge**: Azure Toplu İşlemle ilgili kaynak yapılandırmalarını uyarmak, denetlemek ve uygulamak için yerleşik Azure İlkesi tanımlarını kullanın.  Azure Toplu İş hesaplarınız ve havuzlarınız için özel ilkeler oluşturmak için "Microsoft.Batch" ad alanında Azure İlkesi takma adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve ardışık hatlar geliştirin.



Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: İşletim Sistemleri için Sistem Yapılandırma Yönetim Araçlarını Dağıtma

**Kılavuz :** Azure Toplu Havuz düğümlerinizin işletim sistemleri için istenen durumu korumak için üçüncü taraf bir çözüm uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Azure Hizmetleri için Otomatik Yapılandırma İzleme uygulaması

**Yönerge**: Azure Toplu İşlem örneğinizin yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.Batch" ad alanında Azure İlkesi takma adlarını kullanın. Azure Toplu İş için özel olarak oluşturulan yerleşik ilkeleri veya Azure Toplu İş'i tarafından kullanılan kaynakları da kullanabilirsiniz:

- Alt ağlar bir Ağ Güvenlik Grubu -Depolama Hesapları ile ilişkili olmalıdır sanal ağ hizmeti bitiş noktası kullanmalıdır
- Toplu iş hesaplarındaki tanılama günlükleri etkinleştirilmeli

Kullanılabilir Azure İlkesi Diğer Adları nasıl görüntüleyebilirsiniz:https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: İşletim Sistemleri için Otomatik Yapılandırma İzleme uygulaması

**Kılavuz :** Azure Toplu Havuz düğümlerinizin işletim sistemlerinin durumunu izlemek için üçüncü taraf bir çözüm uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="711-securely-manage-azure-secrets"></a>7.11: Azure sırlarını güvenli bir şekilde yönetin

**Yönlendirme**: Azure Anahtar Kasası, Azure Depolama Hesapları'nda havuz depolama anahtarlarının yönetilmesi için Azure Toplu İşlem dağıtımları ile kullanılabilir.


Azure Yönetilen Kimliklerle nasıl tümleştirilir:

https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity


Azure Anahtar Kasası nasıl oluşturulur:

https://docs.microsoft.com/azure/key-vault/quick-create-portal


Key Vault kimlik doğrulaması yönetilen bir kimlikle nasıl sağlayacaksınız:

https://docs.microsoft.com/azure/key-vault/managed-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="712-securely-and-automatically-manage-identities"></a>7.12: Kimlikleri güvenli ve otomatik olarak yönetin

**Kılavuz**: Kullanılabilir değil, Yönetilen Hizmet Kimliği Azure Toplu İş tarafından desteklenmiyor

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: İstenmeden kimlik bilgisi maruziyetini ortadan kaldırın

**Yönerge**: Kod içindeki kimlik bilgilerini tanımlamak için Kimlik Bilgisi Tarayıcısı'nı uygulayın. Kimlik Bilgisi Tarayıcısı, keşfedilen kimlik bilgilerinin Azure Anahtar Kasası gibi daha güvenli konumlara taşınmasını da teşvik edecektir. 

Kimlik Bilgisi Tarayıcı nasıl kurulturur:https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için [Güvenlik Kontrolü: Malware Defense](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense)' e bakın.*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Merkezi Yönetilen Kötü Amaçlı Yazılımdan Koruma Yazılımı Kullanın

**Kılavuz :** Windows işletim sistemleri söz konusu olduğunda bireysel Azure Toplu iş havuzu düğümlerinizde Windows Defender'ı kullanın veya Linux kullanıyorsanız kendi kötü amaçlı yazılımdan koruma çözümünüzü sağlayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: İşlem yapmayan Azure kaynaklarına yüklenecek dosyaları önceden taraya

**Yönlendirme**: Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure Toplu İş) Microsoft Kötü Amaçlı Yazılımdan Koruma etkindir, ancak müşteri içeriğinde çalışmaz.


Uygulama Hizmeti, Veri Gölü Depolama, Blob Depolama vb. gibi bilgi işlem dışı Azure kaynaklarına yüklenen dosyaları önceden tarayabilirsiniz. Microsoft bu gibi durumlarda müşteri verilerine erişemez.


Azure Bulut Hizmetleri ve Sanal Makineler için Microsoft Kötü Amaçlı Yazılımdan Koruma'yı anlayın:

https://docs.microsoft.com/azure/security/fundamentals/antimalware

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Kötü Amaçlı Yazılımdan Koruma Yazılımı ve İmzalarının Güncelleştirilmesini Sağlayın

**Kılavuz :** Windows işletim sistemleri durumunda bireysel Azure Toplu iş havuzu düğümlerinizde Windows Defender'ı kullanın ve otomatik güncelleştirmenin etkin olduğundan emin olun. Linux kullanıyorsanız kendi kötü amaçlı yazılımdan koruma çözümünüzü sağlayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Düzenli Otomatik Yedekleme'leri Sağlayın

**Kılavuz**: Azure Toplu Toplu Havuz veri deposu için bir Azure Depolama Hesabı kullanırken, uygun artıklık seçeneğini (LRS,ZRS, GRS, RA-GRS) seçin. 


Azure Depolama Hesapları için depolama artıklığı nasıl yapılandırılmaz:

https://docs.microsoft.com/azure/storage/common/storage-redundancy

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Komple Sistem Yedeklemeleri Gerçekleştirin ve Müşteri Yönetilen Anahtarları Yedekleme

**Kılavuz**: Azure Toplu Toplu Havuz veri deposu için bir Azure Depolama Hesabı kullanırken, uygun artıklık seçeneğini (LRS,ZRS, GRS, RA-GRS) seçin.  Azure Toplu İşlem dağıtımınızın herhangi bir bölümü için Azure Key Vault kullanıyorsanız, anahtarlarınızın yedekolduğundan emin olun.


Azure Depolama Hesapları için depolama artıklığı nasıl yapılandırılmaz:

https://docs.microsoft.com/azure/storage/common/storage-redundancy


Azure'da anahtar kasa sıyrık anahtarlarını yedekleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Müşteri Yönetilen Anahtarlar dahil tüm Yedeklemeleri doğrulayın

**Kılavuz**: Azure Depolama Hesapları veya Azure Toplu İşlem uygulamanızla ilgili başka bir kaynak için kendi anahtarlarınızı yönetiyorsanız, yedeklenmiş anahtarların düzenli olarak geri yüklemesini test edin.


Azure'da anahtar kasa sıyrık anahtarlarını yedekleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0


PowerShell ile Müşteri Yönetilen Anahtar nasıl geri yüklenir:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Yedeklerin ve Müşteri Yönetilen Anahtarların Korunmasını Sağlayın

**Kılavuz**: Azure Key Vault, Azure Toplu Toplu Havuz Depolama Hesapları ile ilgili anahtarları tutmak için kullanılıyorsa, yanlışlıkla veya kötü amaçlı silmeye karşı anahtarları korumak için Azure Key Vault'ta Yumuşak Silme'yi etkinleştirin.


Azure Anahtar Kasası'nda Yumuşak Silme nasıl etkinleştirilir:

https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için [Güvenlik Denetimi: Olay Yanıtı'na](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response)bakın.*

### <a name="101-create-incident-response-guide"></a>10.1: Olay yanıt kılavuzu oluşturma

**Rehberlik**: Personelin rollerini ve olay işleme/yönetim aşamalarını tanımlayan yazılı olay müdahale planlarının olduğundan emin olun.



Azure Güvenlik Merkezi'nde İş Akışı Otomasyonları nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="102-create-incident-scoring-and-prioritization-procedure"></a>10.2: Olay Puanlama ve Öncelik Lendirme Prosedürü Oluşturma

**Rehberlik**: Güvenlik Merkezi, her uyarıya katılma sırasının önceliklerini belirlemenize yardımcı olmak için uyarılara önem verir, böylece bir kaynak tehlikeye girdiğinde, bu uyarıya hemen ulaşabilirsiniz. Önem derecesi, Güvenlik Merkezi'nin bulguda ne kadar emin olduğuna veya uyarıyı vermek için kullanılan analize ve uyarıya yol açan etkinliğin arkasında kötü amaçlı niyet olduğuna dair güven düzeyine dayanır.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10.3: Test güvenlik yanıt yordamları

**Rehberlik**: Sistemlerinizin olay yanıt yeteneklerini düzenli olarak test etmek için alıştırmalar yapın. Zayıf noktaları ve boşlukları belirleyin ve planı gerektiği gibi gözden geçirin.

NIST'in yayınına bakın: BT Planları ve Yetenekleri için Test, Eğitim ve Egzersiz Programları Kılavuzu:https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-nbspfor-security-incidents"></a>10.4: Güvenlik Olayı İletişim Bilgilerini Sağlayın &nbsp;ve Güvenlik Olayları Için Uyarı Bildirimlerini Yapılandırın

**Kılavuz :** Microsoft Güvenlik Yanıt Merkezi (MSRC), verilerinize yasadışı veya yetkisiz bir taraf tarafından erişildiğini tespit ederse, güvenlik olayı iletişim bilgileri Microsoft tarafından sizinle iletişim kurmak için kullanılır.



Azure Güvenlik Merkezi Güvenlik Kişisi nasıl ayarlanır:

https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Olay yanıt sisteminize güvenlik uyarıları dahil edin

**Kılavuz**: Sürekli Dışa Aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışa aktarın. Sürekli Dışa Aktarma, uyarı ve önerileri el ile veya sürekli olarak dışa aktarmanıza olanak tanır. Uyarıları Azure Sentinel'e aktarmak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.


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

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-to-remediate-all-critical-security-findings-within-60-days"></a>11.1: Azure kaynaklarınızın düzenli Penetrasyon Testini gerçekleştirin ve tüm kritik güvenlik bulgularını 60 gün içinde düzeltin.

**Kılavuz**: Penetrasyon Testlerinizin Microsoft ilkelerini ihlal etmediğini sağlamak için lütfen Microsoft Etkileşim Kuralları'na uyun:

https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1.



Microsoft'un stratejisi ve Red Teaming ve Microsoft yönetilen bulut altyapısı, hizmetleri ve uygulamalarına karşı canlı site penetrasyon testi nin uygulanması hakkında daha fazla bilgiyi burada bulabilirsiniz: 

https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Güvenlik Kıyaslama'ya](https://docs.microsoft.com/azure/security/benchmarks/overview) bakın
- [Azure Güvenlik Taban Çizgileri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
