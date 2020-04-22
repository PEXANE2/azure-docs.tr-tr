---
title: HDInsight için Azure Güvenlik Taban Çizgisi
description: HDInsight için Azure Güvenlik Taban Çizgisi
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 2ddb9a9c205135707d7d96beb106074baa1ce324
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758272"
---
# <a name="azure-security-baseline-for-hdinsight"></a>HDInsight için Azure Güvenlik Taban Çizgisi

HDInsight için Azure Güvenlik Taban Çizgisi, dağıtımınızın güvenlik duruşunu geliştirmenize yardımcı olacak öneriler içerir.

Bu hizmetin temeli, en iyi uygulamalar kılavuzumuzla Azure'da bulut çözümlerinizi nasıl güvenebileceğinize ilişkin öneriler sunan [Azure Güvenlik Kıyaslama sürümü 1.0'dan](https://docs.microsoft.com/azure/security/benchmarks/overview)alınmıştır.

Daha fazla bilgi için [Azure Güvenlik Taban Çizgilerini genel olarak](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)görün.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için [Güvenlik Denetimi: Ağ Güvenliği'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)bakın.*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Sanal Ağınızdaki Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'nı kullanarak kaynakları koruyun

**Rehberlik**: Azure HDInsight'ta çevre güvenliği sanal ağlar aracılığıyla sağlanır. Bir kuruluş yöneticisi sanal ağ içinde bir küme oluşturabilir ve sanal ağa erişimi kısıtlamak için bir ağ güvenlik grubu (NSG) kullanabilir. Yalnızca gelen Ağ Güvenliği Grubu kurallarında izin verilen IP adresleri Azure HDInsight kümesiyle iletişim kurabilir. Bu yapılandırma çevre güvenliği sağlar. Sanal ağda dağıtılan tüm kümeler, küme ağ geçitlerine özel HTTP erişimi için Sanal Ağ içindeki özel bir IP adresine giden özel bir bitiş noktasına da sahip olur.

Dışa filtreleme yoluyla veri kaybı riskini azaltmak için, Azure Güvenlik Duvarı'nı kullanarak Azure HDInsight kümeleri için giden ağ trafiğini kısıtlayın.

Azure HDInsight'ı Sanal Ağ'da dağıtma ve Ağ Güvenlik Grubu yla Güvenli Hale Nasıl:https://docs.microsoft.com/azure/hdinsight/hdinsight-create-virtual-network

Azure Güvenlik Duvarı ile Azure HDInsight Kümeleri için giden trafiği sınırlama:https://docs.microsoft.com/azure/hdinsight/hdinsight-restrict-outbound-traffic

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Vnet, Subnet ve NIC'lerin yapılandırmasını ve trafiğini izleyin ve kaydedin

**Kılavuz**: Azure HDInsight kümenizi korumak için kullanılan sanal ağ, alt ağ ve ağ güvenlik grubu için Azure Güvenlik Merkezi'ni ve ağ koruma önerilerini düzeltin. Ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve günlükleri trafik denetimine Azure Depolama Hesabına gönderin. Ayrıca NSG akış günlüklerini bir Azure Log Analytics Çalışma Alanına gönderebilir ve Azure bulutunuzdaki trafik akışıhakkında bilgi sağlamak için Azure Trafik Analizi'ni kullanabilirsiniz. Azure Trafik Analitiği'nin bazı avantajları, ağ etkinliğini görselleştirebilme ve etkin noktaları belirleyebilme, güvenlik tehditlerini belirleyebilme, trafik akışı desenlerini anlama ve ağ yanlış yapılandırmalarını saptabilme yeteneğidir.

NSG Akış Günlükleri Nasıl Etkinleştirilir:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Azure Trafik Analizini Etkinleştirme ve Kullanma:

https://docs.microsoft.com/azure/network-watcher/traffic-analytics

Azure Güvenlik Merkezi tarafından sağlanan Ağ Güvenliğini Anlayın:

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="13-protect-critical-web-applications"></a>1.3: Kritik web uygulamalarını koruyun

**Rehberlik**: Geçerli değildir; kıyaslama, Azure Apps Hizmeti veya web uygulamalarını barındıran bilgi işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Bilinen kötü amaçlı IP adresleriyle iletişimi reddetme

**Yönlendirme**: DDoS saldırılarına karşı koruma sağlamak için Azure HDInsight'ınızın dağıtıldığı sanal ağda Azure DDoS Standart korumasını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimi reddetmek için Azure Güvenlik Merkezi tümleşik tehdit istihbaratını kullanın.

DDoS koruması nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı Anlayın:

https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1.5: Ağ paketlerini ve akış günlüklerini kaydetme

**Kılavuz**: Azure HDInsight kümenizi korumak için kullanılan alt ağa bağlı NSG için ağ güvenlik grubu (NSG) flog günlüklerini etkinleştirin. Akış kayıtları oluşturmak için NSG akış günlüklerini bir Azure Depolama Hesabına kaydedin. Anormal etkinliği araştırmak için gerekirse, Azure Ağ İzleyicipaket yakalamayı etkinleştirin.

NSG Akış Günlükleri Nasıl Etkinleştirilir:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Ağ İzleyicisi nasıl etkinleştirilir:

https://docs.microsoft.com/azure/network-watcher/network-watcher-create

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Ağ tabanlı izinsiz giriş algılama/saldırı önleme sistemlerini (IDS/IPS) dağıtma

**Kılavuz**: Gereksinim azure güvenlik denetimi KIMLIĞI 1.1;; Azure HDInsight kümesini sanal bir ağa dağıtın ve bir ağ güvenlik grubu (NSG) ile güvenli hale getirin.

Azure HDInsight için gelen trafik gerektiren çeşitli bağımlılıklar vardır. Gelen yönetim trafiği bir güvenlik duvarı aygıtı ndan gönderilemez. Gerekli yönetim trafiği için kaynak adresleri bilinir ve yayımlanır. Kümelere gelen trafiği güvence altına alarak yalnızca güvenilen konumlardan gelen trafiğe izin vermek için bu bilgilerle Ağ Güvenlik Grubu kuralları oluşturun.

Dışa filtreleme yoluyla veri kaybı riskini azaltmak için, Azure Güvenlik Duvarı'nı kullanarak Azure HDInsight kümeleri için giden ağ trafiğini kısıtlayın.

HDInsight'ı Sanal Ağ'da dağıtma ve Ağ Güvenlik Grubu yla Güvenli Hale Nasıl:https://docs.microsoft.com/azure/hdinsight/hdinsight-create-virtual-network

HDInsight bağımlılıklarını ve güvenlik duvarı kullanımını anlayın:https://docs.microsoft.com/azure/hdinsight/hdinsight-restrict-outbound-traffic

HDInsight yönetimi IP adresleri:https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Web uygulamalarına trafik yönetme

**Rehberlik**: Geçerli değildir; kıyaslama, Azure Apps Hizmeti veya web uygulamalarını barındıran bilgi işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Ağ güvenliği kurallarının karmaşıklığını ve yönetim ek lerini en aza indirmek

**Kılavuz :** Azure HDInsight kümenizin dağıtılan alt ağına bağlı ağ güvenlik gruplarındaki (NSG) ağ erişim denetimlerini tanımlamak için Sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında servis etiketi adını (örn. ApiManagement) belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz. Microsoft, hizmet etiketinin kapsadığı adres öneklerini yönetir ve adresler değiştikçe servis etiketini otomatik olarak güncelleştirir.

Azure HDInsight için Hizmet Etiketlerini anlama ve kullanma:

https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Ağ aygıtları için standart güvenlik yapılandırmalarını koruyun

**Yönerge**: Azure HDInsight kümenizle ilgili ağ kaynakları için standart güvenlik yapılandırmaları tanımlayın ve uygulayın. Azure HDInsight kümenizin ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.HDInsight" ve "Microsoft.Network" ad alanlarında Azure İlkesi takma adlarını kullanın.

Azure Kaynak Yöneticisi şablonları, RBAC denetimleri ve ilkeleri gibi önemli ortam yapılarını tek bir plan tanımında paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure Planları'nı da kullanabilirsiniz. Planı yeni aboneliklere ve ortamlara kolayca uygulayın ve sürüm leme yoluyla denetim ve yönetime ince ayar uygulayın.

Kullanılabilir Azure İlkesi diğer adlarını görüntüleme:

https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Blueprint nasıl oluşturulur:

https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="110-document-traffic-configuration-rules"></a>1.10: Belge trafiği yapılandırma kuralları

**Kılavuz :** Ağ güvenliği grubu (NSG'ler) ve Azure HDInsight kümenizle ilişkili ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketleri kullanın. Tek tek NSG kuralları için, bir ağa/aktan trafiğin alınmasına izin veren kurallar için iş gereksinimini ve/veya süresini (vb.) belirtmek için "Açıklama" alanını kullanın.

Tüm kaynakların Etiketler ile oluşturulduğundan emin olmak ve varolan etiketlenmemiş kaynakları size bildirmek için etiketlemeyle ilgili "Etiket ve değeri gerektir" gibi yerleşik Azure ilke tanımlarından herhangi birini kullanın.

Azure PowerShell veya Azure komut satırı arabirimini (CLI) Etiketleri'ne dayalı kaynaklara bakmak veya eylemler gerçekleştirmek için kullanabilirsiniz.

Etiketler nasıl oluşturulur ve kullanılır:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Sanal ağ nasıl oluşturulur:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal

Güvenlik Config ile bir NSG oluşturmak için nasıl:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Ağ kaynağı yapılandırmalarını izlemek ve değişiklikleri algılamak için otomatik araçlar kullanın

**Yönerge**: Ağ kaynak yapılandırmalarını izlemek ve Azure HDInsight dağıtımlarınızla ilgili ağ kaynaklarındaki değişiklikleri algılamak için Azure Etkinlik Günlüğü'ne kullanın. Azure Monitor'da kritik ağ kaynaklarında değişiklikler olduğunda tetikleyecek uyarılar oluşturun.

Azure Etkinlik Günlüğü etkinliklerini görüntüleme ve alma:

https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

Azure Monitor'da uyarı oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için [Güvenlik Denetimi: Günlüğe kaydetme ve izleme.](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Onaylanmış zaman senkronizasyon kaynaklarını kullanma

**Kılavuz**: Microsoft, Azure HDInsight küme bileşenleri için zaman kaynaklarını korur, bilgi işlem dağıtımlarınız için zaman eşitlemesi güncelleştirebilirsiniz.

Azure bilgi işlem kaynakları için zaman eşitleme yapılandırması nasıl yapılandırılır:

https://docs.microsoft.com/azure/virtual-machines/windows/time-sync

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Yönerge**: Küme tarafından oluşturulan güvenlik verilerini toplamak için Azure HDInsight kümenizde Azure HDInsight kümesine binebilirsiniz. Çevredeki tehditleri algılamak ve yanıtlamak için özel sorgulardan yararlanın. 

Azure Monitor'a Azure HDInsight kümesinde nasıl binilir:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-tutorial

Azure HDInsight kümesi için özel sorgular oluşturma:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-use-queries

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure kaynakları için denetim günlüğe kaydetmeyi etkinleştirme

**Yönlendirme**: HDInsight kümesi için Azure Monitör'ü etkinleştirin, log Analytics çalışma alanına yönlendirin. Bu, tüm Azure HDInsight küme düğümleri için ilgili küme bilgilerini ve işletim sistemi ölçümlerini kaydeder.

HDInsight Cluster için günlüğe kaydetme nasıl etkinleştirilir:

 https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-tutorial

HDInsight günlükleri nasıl sorgulanır:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-use-queries

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: İşletim sistemlerinden güvenlik günlüklerini toplama

**Kılavuz**: Yerleşik Azure HDInsight kümesini Azure Monitor'a taşıyın. Kullanılan Log Analytics çalışma alanının kuruluşunuzun uyumluluk yönetmeliklerine göre günlük saklama süresi ne zaman ayarlandığından emin olun.

Azure HDInsight Cluster'da Azure Monitor'a nasıl binilir:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-tutorial

Günlük Analizi Çalışma Alanı Bekletme Süresi nasıl yapılandırılatır:

https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="25-configure-security-log-storage-retention"></a>2.5: Güvenlik günlüğü depolama tutma yapılandırma

**Kılavuz**: Yerleşik Azure HDInsight kümesini Azure Monitor'a taşıyın. Kullanılan Azure Günlük Analizi çalışma alanının kuruluşunuzun uyumluluk yönetmeliklerine göre günlük bekletme süresine sahip olduğundan emin olun.

Azure HDInsight Cluster'da Azure Monitor'a nasıl binilir:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-tutorial

Günlük Analizi Çalışma Alanı Bekletme Süresi nasıl yapılandırılatır:

https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2.6: Günlükleri izleme ve inceleme

**Yönerge**: Azure HDInsight günlüklerini sorgulamak için Azure Günlük Analizi çalışma alanı sorgularını kullanın:

Azure HDInsight Kümeleri için Özel Sorgular Nasıl Oluşturulur:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-use-queries

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Anormal aktivite için uyarıları etkinleştirme

**Kılavuz :** Azure HDInsight kümenizle ilgili güvenlik günlükleri ve olaylardaki anormal etkinlikleri izlemek ve uyarmak için Azure Log Analytics çalışma alanını kullanın.

Azure Güvenlik Merkezi'nde uyarılar nasıl yönetilir:

https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts

Günlük analitiği günlük verilerinde nasıl uyarı olun:

https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="28-centralize-anti-malware-logging"></a>2.8: Kötü amaçlı yazılımdan koruma günlüğe kaydetmeyi merkezileştirin

**Yönlendirme**: Azure HDInsight, cluster düğüm görüntüleri için önceden yüklenmiş ve etkin leştirilmiş Clamscan ile birlikte gelir, ancak yazılımı yönetmeniz ve Clamscan'ın ürettiği günlükleri el ile toplamanız/izlemeniz gerekir.

Clamscan'ı anlayın:

https://docs.microsoft.com/azure/hdinsight/hdinsight-faq#security-and-certificates

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="29-enable-dns-query-logging"></a>2.9: DNS sorgu günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: DNS günlüğü için üçüncü taraf çözümuygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="210-enable-command-line-audit-logging"></a>2.10: Komut satırı denetim günlüğe kaydetmeyi etkinleştirme

**Kılavuz**: Konsol günlüğe kaydetmeyi düğüm başına göre el ile yapılandırın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için [güvenlik denetimi: kimlik ve erişim denetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)bakın.*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: İdari hesapların envanterini korumak

**Yönerge**: Azure HDInsight kümesinin küme sağlanması sırasında oluşturulan yerel yönetim hesabının ve oluşturduğunuz diğer hesapların kaydını koruyun. Ayrıca, Azure AD tümleştirmesi kullanılıyorsa, Azure AD'de açıkça atanması gereken ve bu nedenle sorgulanabilen yerleşik roller vardır. Yönetim gruplarının üyesi hesapları bulmak için geçici sorgular gerçekleştirmek için Azure AD PowerShell modüllerini kullanın.

Ayrıca, Azure Güvenlik Merkezi Kimliği ve Erişim Yönetimi önerilerini de kullanabilirsiniz.

PowerShell ile Azure AD'de dizin rolü nasıl elde elabilirsiniz:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0

PowerShell ile Azure AD'de dizin rolünün üyelerini nasıl edinire bilgili olabilirsiniz:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0

Azure Güvenlik Merkezi ile kimlik ve erişim nasıl izlenir:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Varsayılan parolaları varsa değiştirme

**Kılavuz**: Bir küme oluştururken Azure, web portalı ve Secure Shell (SSH) erişimi için yeni parolalar oluşturmanızı gerektirir. Değiştirilen varsayılan parola lar yoktur, ancak SSH ve web portalı erişimi için farklı parolalar belirtebilirsiniz.

Azure HDInsight kümesini sağlarken parolalar nasıl ayarlanır:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Özel yönetim hesaplarını kullanma

**Yönerge**: Azure HDInsight kümesi için Kimlik Doğrulamayı Azure Active Directory ile tümleştirin. Özel yönetim hesaplarının kullanımı yla ilgili ilke ler ve yordamlar oluşturun.

Ayrıca, Azure Güvenlik Merkezi Kimliği ve Erişim Yönetimi önerilerini de kullanabilirsiniz.

Azure HDInsight kimlik doğrulamasını Azure Active Directory ile nasıl tümleştirilir:

https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds

Azure Güvenlik Merkezi ile kimlik ve erişim nasıl izlenir:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Azure Active Directory ile tek oturum açma (SSO) kullanma

**Yönlendirme**: Birden Çok Faktörlü Kimlik Doğrulama'yı kullanarak, parola sağlamadan Kurumsal Güvenlik Paketi (ESP) kümelerinde oturum açabilmek için Azure HDInsight ID Aracısını kullanın. Azure portalı gibi diğer Azure hizmetlerinde oturum açtıysanız, tek bir oturum açma (SSO) deneyimiyle Azure HDInsight kümenizde oturum açabilirsiniz.

Azure HDInsight ID Aracısı nasıl etkinleştirilir:

https://docs.microsoft.com/azure/hdinsight/domain-joined/identity-broker#enable-hdinsight-id-broker

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Tüm Azure Active Directory tabanlı erişim için çok faktörlü kimlik doğrulamayı kullanın

**Kılavuz**: Azure AD MFA'sını etkinleştirin ve Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerini izleyin. Yapılandırılan Kurumsal Güvenlik Paketi'ne sahip Azure HDInsight kümeleri, etki alanı kullanıcılarının kümeleri doğrulamak ve büyük veri işleri çalıştırmak için etki alanı bilgilerini kullanabilmeleri için bir etki alanına bağlanabilir. Çok faktörlü kimlik doğrulama (MFA) etkinleştirilmiş kimlik doğrulaması yapıldığında, kullanıcılara ikinci bir kimlik doğrulama faktörü sağlaması zorlanacaktır.

Azure'da MFA nasıl etkinleştirilir:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Azure Güvenlik Merkezi'nde kimlik ve erişim nasıl izlenir:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Tüm idari görevler için özel makineleri (Ayrıcalıklı Erişim İş İstasyonları) kullanın

**Yönlendirme**: Azure HDInsight kümelerinize ve ilgili kaynaklarınızda oturum açmak ve yapılandırmak üzere yapılandırılan çok faktörlü kimlik doğrulama (MFA) ile PAW'ları (ayrıcalıklı erişim iş istasyonları) kullanın.

Ayrıcalıklı Erişim İş İstasyonları hakkında bilgi edinin:

https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure'da MFA nasıl etkinleştirilir:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: İdari hesaplardan şüpheli etkinliklere ilişkin günlük ve uyarı

**Yönlendirme**: Kurumsal Güvenlik Paketi yapılandırılan Azure HDInsight kümeleri, etki alanı kullanıcılarının kimlik doğrulamaiçin etki alanı bilgilerini kullanabilmeleri için bir etki alanına bağlanabilir. AAD ortamında şüpheli veya güvensiz etkinlik oluştuğunda günlük ve uyarı oluşturma için Azure Active Directory (AAD) güvenlik raporlarını kullanabilirsiniz. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi'ni kullanın.

Riskli etkinlik için işaretlenmiş AAD kullanıcılarını nasıl tanımlar:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk

Azure Güvenlik Merkezi'nde kullanıcıların kimlik ve erişim etkinliğini izleme:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Yönlendirme**: Kurumsal Güvenlik Paketi yapılandırılan Azure HDInsight kümeleri, etki alanı kullanıcılarının kimlik doğrulamaiçin etki alanı bilgilerini kullanabilmeleri için bir etki alanına bağlanabilir. Yalnızca IP adresi aralıklarının veya ülkelerin/bölgelerinin belirli mantıksal gruplandırmalarından erişime izin vermek için Koşullu Erişim Adlandırılmış Konumlar'ı kullanın.

Azure'da Adlandırılmış Konumlar nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="39-use-azure-active-directory"></a>3.9: Azure Etkin Dizini Kullanma

**Kılavuz :** Merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory 'i (AAD) kullanın. AAD, istirahatte ve aktarım sırasında veriler için güçlü şifreleme kullanarak verileri korur. AAD ayrıca kullanıcı kimlik bilgilerini tuzlar, iş lerle ve güvenli bir şekilde saklar.

Kurumsal Güvenlik Paketi (ESP) yapılandırılan Azure HDInsight kümeleri, etki alanı kullanıcılarının kümelerle kimlik doğrulama yapmak için etki alanı bilgilerini kullanabilmeleri için bir etki alanına bağlanabilir.

AAD örneğini oluşturma ve yapılandırma:

https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant

Azure HDInsight'ta Azure Active Directory Etki Alanı Hizmetleri ile Kurumsal Güvenlik Paketi nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Kullanıcı erişimini düzenli olarak gözden geçirin ve uzlaştırın

**Kılavuz :** Azure HDInsight kümenizle Azure Active Directory (AAD) kimlik doğrulamasını kullanın. AAD, eski hesapları keşfetmeye yardımcı olmak için günlükler sağlar. Ayrıca, grup üyeliklerini, kurumsal uygulamalara erişimi ve rol atamalarını verimli bir şekilde yönetmek için Azure Kimlik Erişim İncelemeleri'ni kullanın. Yalnızca doğru Kullanıcıların sürekli erişime sahip olduğundan emin olmak için kullanıcıerişimi düzenli olarak gözden geçirilebilir. 

Azure Kimlik Erişim İncelemeleri nasıl kullanılır:

https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Devre dışı bırakılmış hesaplara erişim denemelerini izleme

**Rehberlik**: Devre dışı bırakılmış hesaplara erişim girişimlerini izlemek için Azure Active Directory (AAD) Oturum Açma ve Denetleme günlüklerini kullanın; bu günlükler herhangi bir üçüncü taraf SIEM/izleme aracına entegre edilebilir.

AAD kullanıcı hesapları için Tanılama Ayarları oluşturarak, denetim günlüklerini ve oturum açma günlüklerini Azure Log Analytics çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. Azure Log Analytics çalışma alanı içinde istediğiniz Uyarıları yapılandırın.

Azure Etkinlik Günlükleri Azure Monitör'e nasıl entegre eleştirilir:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Hesap giriş davranış sapması konusunda uyarı

**Yönlendirme**: Kurumsal Güvenlik Paketi (ESP) yapılandırılan Azure HDInsight kümeleri, etki alanı kullanıcılarının kümelerle kimlik doğrulama yapmak için etki alanı bilgilerini kullanabilmeleri için bir etki alanına bağlanabilir.  Kullanıcı kimlikleri ile ilgili algılanan şüpheli eylemlere otomatik yanıtları yapılandırmak için Azure Etkin Dizin (AAD) Risk Algılamaları ve Kimlik Koruması özelliğini kullanın. Ayrıca, daha fazla araştırma için Azure Sentinel'e veri sindirebilirsiniz.

AAD riskli oturum açma nasıl görüntülenebilir:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Kimlik Koruması risk ilkelerinin nasıl yapılandırılabilen ve etkinleştirilir:

https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Destek senaryoları sırasında Microsoft'a ilgili müşteri verilerine erişim sağlama

**Rehberlik**: Mevcut değil; Müşteri Kilit Kutusu, Azure HDInsight için henüz desteklenmedi.

Müşteri Lockbox desteklenen hizmetler listesi:https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için [güvenlik denetimi: veri koruma.](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Hassas Bilgilerin envanterini koruyun

**Kılavuz**: Azure HDInsight dağıtımlarınızla ilgili kaynaklardaki etiketleri kullanarak hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olun.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Hassas bilgileri depolama veya işleme sistemlerini yalıtma

**Rehberlik**: Geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Azure HDInsight kümeleri ve ilişkili depolama hesapları sanal ağ/alt ağ tarafından ayrılmalı, uygun şekilde etiketlenmelidir ve bir ağ güvenlik grubu (NSG) veya Azure Güvenlik Duvarı içinde güvenli hale edilmelidir. Küme verileri güvenli bir Azure Depolama Hesabı veya Azure Veri Gölü Depolama (Gen1 veya Gen2) içinde yer almalıdır.

Azure HDInsight kümeniz için depolama seçenekleri seçin:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-compare-storage-options

Azure Veri Gölü Depolaması nasıl güvenli hale ilir:

https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview

Azure Depolama Hesapları nasıl güvenli hale ilir:

https://docs.microsoft.com/azure/storage/common/storage-security-guide

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Hassas bilgilerin yetkisiz aktarımının izlenmesi ve engellenmesi

**Kılavuz**: Azure HDInsight kümeleri için hassas bilgileri depolama veya işleme, kümeyi ve ilgili kaynakları etiketleri kullanarak hassas olarak işaretleyin. Dışa filtreleme yoluyla veri kaybı riskini azaltmak için, Azure Güvenlik Duvarı'nı kullanarak Azure HDInsight kümeleri için giden ağ trafiğini kısıtlayın.

Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

Azure Güvenlik Duvarı ile Azure HDInsight Kümeleri için giden trafiği sınırlama:

https://docs.microsoft.com/azure/hdinsight/hdinsight-restrict-outbound-traffic

Azure'da müşteri verilerinin korunmasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Tüm hassas bilgileri taşıma sırasında şifreleme

**Rehberlik**: Tüm hassas bilgileri geçiş sırasında şifreleyin. Azure HDInsight kümenize veya küme veri depolarınıza (Azure Depolama Hesapları veya Azure Veri Gölü Depolama Gen1/Gen2) bağlanan istemcilerin TLS 1,2 veya daha büyük bir anlaşma sağlayabilmesini sağlayın. Microsoft Azure kaynakları TLS 1.2'yi varsayılan olarak görüşür. 

Aktarım sırasında Azure Veri Gölü Depolama şifrelemesi anlayın:

https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview

Aktarım sırasında Azure Depolama Hesabı şifrelemesini anlayın:

https://docs.microsoft.com/azure/storage/common/storage-security-guide#encryption-in-transit

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Hassas verileri tanımlamak için etkin bir bulma aracı kullanın

**Yönlendirme**: Azure Depolama veya bilgi işlem kaynakları için veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz kullanılamıyor. Uyumluluk amacıyla gerekirse üçüncü taraf çözümlerini uygulayın.

Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

Azure'da müşteri verilerinin korunmasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Kaynaklara erişimi denetlemek için Azure RBAC'ı kullanın

**Kılavuz :** Azure HDInsight Kurumsal Güvenlik Paketi (ESP) ile, dosyalarda, klasörlerde, veritabanlarında, tablolarda ve sütunlarda/sütunlarda depolanan verileriniz için ince taneli erişim denetimi ve veri gizleme ilkeleri oluşturmak ve yönetmek için Apache Ranger'ı kullanabilirsiniz. Hadoop yöneticisi, Apache Ranger'daki eklentileri kullanarak Apache Hive, HBase, Kafka ve Spark'ı güvence altına almak için rol tabanlı erişim kontrolünü (RBAC) yapılandırabilir.

RBAC ilkelerini Apache Ranger ile yapılandırmak, izinleri kuruluştaki bir rolile ilişkilendirmenize olanak tanır. Bu soyutlama katmanı, insanların yalnızca iş sorumluluklarını yerine getirmek için gereken izinlere sahip olmalarını kolaylaştırır.

HDInsight'ta Azure Active Directory Domain Services ile Kurumsal Güvenlik Paketi yapılandırmaları:

https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds

Azure HDInsight'ta kurumsal güvenliğe genel bakış:

https://docs.microsoft.com/azure/hdinsight/domain-joined/hdinsight-security-overview



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Erişim denetimini uygulamak için ana bilgisayar tabanlı veri kaybı önlemeyi kullanın

**Kılavuz**: Azure HDInsight kümeleri için hassas bilgileri depolama veya işleme, kümeyi ve ilgili kaynakları etiketleri kullanarak hassas olarak işaretleyin. Azure Depolama veya bilgi işlem kaynakları için veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz kullanılamıyor. Uyumluluk amacıyla gerekirse üçüncü taraf çözümlerini uygulayın.

Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

Azure'da müşteri verilerinin korunmasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Hassas bilgileri istirahatte şifreleme

**Kılavuz**: Apache Hive ve Apache Oozie meta verilerini depolamak için Azure SQL Veritabanı kullanıyorsanız, SQL verilerinin her zaman şifrelenmiş olduğundan emin olun. Azure Depolama Hesapları ve Veri Gölü Depolama (Gen1 veya Gen2) için, Microsoft'un şifreleme anahtarlarınızı yönetmesine izin vermeniz önerilir, ancak kendi anahtarlarınızı yönetme seçeneğiniz vardır.

Azure Depolama Hesapları için şifreleme anahtarlarını yönetme:

https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal

Müşteri tarafından yönetilen şifreleme anahtarlarını kullanarak Azure Veri Gölü Depolaması nasıl oluşturulur:

https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal

Azure SQL Veritabanı için şifrelemeyi anlama:

https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview#data-encryption

Müşteri yönetilen anahtarları kullanarak SQL Veritabanı için Saydam Veri Şifrelemesi nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?tabs=azure-portal

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Paylaşılan

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Kritik Azure kaynaklarında yapılan değişiklikleri kaydedin ve uyarı

**Yönerge**: Tüm CRUD işlemlerini küme verilerine karşı izlemek ve günlüğe kaydetmek için Azure HDInsight kümeleriyle ilişkili Azure Depolama Hesapları için Tanı Ayarlarını Yapılandırın. Azure HDInsight kümesiyle ilişkili tüm Depolama Hesapları veya Veri Gölü Mağazaları için Denetimi Etkinleştirin.

Azure Depolama Hesabı için ek günlüğe kaydetme/denetlemeyi etkinleştirme:

https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account

Azure Veri Gölü Depolaması için ek günlüğe kaydetme/denetleme yi etkinleştirme:

https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-diagnostic-logs

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için [Güvenlik Denetimi: Güvenlik Açığı Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)bakın.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Otomatik güvenlik açığı tarama araçlarını çalıştırın

**Kılavuz**: Üçüncü taraf güvenlik açığı yönetimi çözümü uygulayın.

İsteğe bağlı olarak, Rapid7, Qualys veya başka bir güvenlik açığı yönetim platformu aboneliğiniz varsa, Azure HDInsight küme düğümlerinize güvenlik açığı değerlendirme aracıları yüklemek ve düğümleri ilgili portal üzerinden yönetmek için komut dosyası eylemlerini kullanabilirsiniz.

Rapid7 Agent El Ile Nasıl Yüklenir:

https://insightvm.help.rapid7.com/docs/install

Qualys Agent El ile nasıl yüklenir:

https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf

Komut dosyası eylemleri nasıl kullanılır:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Otomatik işletim sistemi yama yönetimi çözümlerini dağıtma

**Yönerge :** Küme düğümü görüntüleri için otomatik sistem güncelleştirmeleri etkinleştirildi, ancak güncelleştirmelerin uygulandığından emin olmak için küme düğümlerini düzenli aralıklarla yeniden başlatmanız gerekir.

Microsoft, temel Azure HDInsight düğüm görüntülerini korumak ve güncellemek için.

HDInsight kümeleri için işletim sistemi yama zamanlaması nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/hdinsight/hdinsight-os-patching

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3: Otomatik üçüncü taraf yazılım yama yönetimi çözümlerini dağıtma

**Yönerge**: Azure HDInsight kümelerinizi yamalamak için komut dosyası eylemlerini veya diğer mekanizmaları kullanın. Yeni oluşturulan kümeler, en son güvenlik düzeltme eki de dahil olmak üzere her zaman kullanılabilir en son güncelleştirmelere sahip olur.

Linux tabanlı Azure HDInsight kümeleri için işletim sistemi yama zamanlaması nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/hdinsight/hdinsight-os-patching

Komut dosyası eylemleri nasıl kullanılır:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Arka arkaya güvenlik açığı taramalarını karşılaştırın

**Yönerge**: Güvenlik açığı taramalarını zaman içinde karşılaştırma olan bir üçüncü taraf güvenlik açığı yönetimi çözümü uygulayın. Rapid7 veya Qualys aboneliğiniz varsa, arka arkaya güvenlik açığı taramalarını görüntülemek ve karşılaştırmak için bu satıcının portalını kullanabilirsiniz.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Keşfedilen güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Yönlendirme**: Ortak bir risk puanlama programı (örneğin, Ortak Güvenlik Açığı Puanlama Sistemi) veya üçüncü taraf tarama aracınız tarafından sağlanan varsayılan risk derecelendirmeleri kullanın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için [güvenlik denetimi: Envanter ve Varlık Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)bakın.*

### <a name="61-use-azure-asset-discovery"></a>6.1: Azure Varlık Bulma'yı kullanma

**Yönlendirme**: Aboneliğinizde Azure HDInsight kümeleri de dahil olmak üzere tüm kaynakları (bilgi işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın.  Kiracınızda uygun (okuma) izinlere sahip olduğundan ve aboneliklerinizdeki tüm Azure aboneliklerinin yanı sıra kaynakları sıralayabildiğinizden emin olun.

Kaynak Grafiği aracılığıyla klasik Azure kaynakları keşfedilebilir, ancak ileriye dönük Olarak Azure Kaynak Yöneticisi kaynaklarının oluşturulması ve kullanılması önerilir.

Azure Kaynak Grafiği ile sorgu oluşturma:

https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure Aboneliklerinizi görüntüleme:

https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure RBAC'ı anlayın:

https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="62-maintain-asset-metadata"></a>6.2: Varlık meta verilerini koruma

**Yönerge**: Meta verileri mantıksal olarak taksonomi olarak düzenlemek için meta veriler veren Azure kaynaklarına etiketler uygulayın.

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Yetkisiz Azure kaynaklarını silme

**Rehberlik**: Varlıkları düzenlemek ve izlemek için etiketlemeyi, yönetim gruplarını ve uygun olduğu durumlarda ayrı abonelikleri kullanın. Envanteri düzenli olarak uzlaştırın ve yetkisiz kaynakların abonelikten zamanında silindiğinden emin olun.

Ek Azure abonelikleri oluşturma:

https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları nasıl oluşturulur:

https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: Onaylanan Azure kaynaklarının ve yazılım başlıklarının envanterini koruyun

**Yönerge**: İşlem kaynaklarınız için onaylanmış Azure kaynaklarının ve onaylı yazılımların listesini tanımlayın

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Onaylanmamış Azure kaynakları için izleme

**Yönerge**: Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Aboneliğinizdeki kaynakları sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Bilgi işlem kaynakları içindeki onaylanmamış yazılım uygulamalarını izleme

**Yönerge**: Onaylanmamış yazılım uygulamaları için küme düğümlerini izlemek için üçüncü taraf bir çözüm uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Yönlendirme**: Aboneliğinizde Azure HDInsight kümeleri de dahil olmak üzere tüm kaynakları (bilgi işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın.  Keşfettiğiniz onaylanmamış Azure kaynaklarını kaldırın. Azure HDInsight küme düğümleri için, onaylanmamış yazılımları kaldırmak veya uyarmak için bir üçüncü taraf çözümü uygulayın.

Azure Graph ile sorgu oluşturma:

https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="68-use-only-approved-applications"></a>6.8: Yalnızca onaylı uygulamaları kullanın

**Kılavuz :** Azure HDInsight küme düğümleri için, yetkisiz yazılımın yürütülmesini önlemek için bir üçüncü taraf çözümü uygulayın.


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="69-use-only-approved-azure-services"></a>6.9: Yalnızca onaylanmış Azure hizmetlerini kullanma

**Yönerge**: Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlkesi ile belirli bir kaynak türü nasıl reddedilecek:https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="610-implement-approved-application-list"></a>6.10: Onaylı başvuru listesini uygulama

**Kılavuz**: Azure HDInsight küme düğümleri için, yetkisiz dosya türlerinin yürütülmesini önlemek için bir üçüncü taraf çözümü uygulayın.


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6.11: Kullanıcıların komut dosyaları aracılığıyla Azure Kaynakları Yöneticisi ile etkileşim deşmelerini sınırlandırın

**Yönerge**: "Microsoft Azure Yönetimi" Uygulaması için "Erişimi engelle" yapılandırmayı yaparak kullanıcıların Azure Kaynak Yöneticisi ile etkileşim edebilmelerini sınırlamak için Azure Koşullu Erişim'i kullanın.

Koşullu Erişim'i Azure Kaynak Yöneticisi'ne erişimi engellemek için yapılandırma:https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Kullanıcıların bilgi işlem kaynakları içinde komut dosyalarını yürütme yeteneğini sınırlama

**Rehberlik**: Geçerli değildir; Kümenin kullanıcılarının (yönetici olmayanların) işleri çalıştırmak için tek tek düğümlere erişmesi gerekmeden, bu Azure HDInsight için geçerli değildir. Küme yöneticisitüm küme düğümlerine kök erişimi vardır.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Yüksek riskli uygulamaları fiziksel veya mantıksal olarak ayırmak

**Rehberlik**: Geçerli değildir; kıyaslama, Azure Apps Hizmeti veya web uygulamalarını barındıran bilgi işlem kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Geçerli değil

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için güvenlik [denetimi: Güvenli Yapılandırma'ya](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration)bakın.*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Tüm Azure kaynakları için güvenli yapılandırmalar oluşturma

**Yönerge**: HDInsight kümenizin ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.HDInsight" ad alanında Azure İlkesi takma adlarını kullanın.

Kullanılabilir Azure İlkesi diğer adlarını görüntüleme:

https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Güvenli işletim sistemi yapılandırmaları oluşturma

**Rehberlik**: Azure HDInsight İşletim Sistemi Görüntüleri Microsoft tarafından yönetilir ve korunur. Küme düğümlerinizin işletim sistemi için güvenli yapılandırmaları uygulamaktan sorumlu müşteri. 


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Güvenli Azure kaynak yapılandırmalarını koruyun

**Yol gösterici**: Azure HDInsight kümeleriniz ve ilgili kaynaklarınız için güvenli ayarlar uygulamak için Azure ilkesini [reddet] ve [varsa dağıt] kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlke Efektlerini Anlama:

https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Güvenli işletim sistemi yapılandırmalarını koruyun

**Rehberlik**: Azure HDInsight İşletim Sistemi Görüntüleri Microsoft tarafından yönetilir ve korunur. İşletim sistemi düzeyinde durum yapılandırması uygulamaktan sorumlu müşteri.


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure kaynaklarının yapılandırmayı güvenli bir şekilde depolama

**Yönlendirme**: Özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure DevOps veya Azure Repos'unu kullanın.

Azure DevOps'lerde kod depolama:

https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

Azure Depobelgeleri:

https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Özel işletim sistemi görüntülerini güvenli bir şekilde saklayın

**Rehberlik**: Geçerli değildir; Azure HDInsight için geçerli olmayan özel görüntüler.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7.7: Sistem yapılandırma yönetim araçlarını dağıtma

**Yönerge**: Sistem yapılandırmalarını uyarmak, denetlemek ve uygulamak için özel ilkeler oluşturmak için "Microsoft.HDInsight" ad alanında Azure İlkesi takma adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve ardışık hatlar geliştirin.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: İşletim sistemleri için sistem yapılandırma yönetim araçlarını dağıtma

**Yönerge**: Küme düğüm işletim sistemleriniz için istenilen durumu korumak için üçüncü taraf bir çözüm uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Azure hizmetleri için otomatik yapılandırma izleme uygulaması

**Yönerge**: HDInsight kümenizin yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.HDInsight" ad alanında Azure İlkesi takma adlarını kullanın.

Kullanılabilir Azure İlkesi diğer adlarını görüntüleme:

https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: İşletim sistemleri için otomatik yapılandırma izleme

**Kılavuz :** Küme düğüm işletim sistemlerinizin durumunu izlemek için üçüncü taraf bir çözüm uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="711-manage-azure-secrets-securely"></a>7.11: Azure sırlarını güvenli bir şekilde yönetme

**Rehberlik**: Azure HDInsight, Apache Kafka için Bring Your Own Key (BYOK) desteğini içerir. Bu özellik, verileri istirahatte şifrelemek için kullanılan anahtarlara sahip olabilirsiniz ve bunları yönetmenize olanak tanır.

Azure HDInsight'taki tüm yönetilen diskler Azure Depolama Hizmeti Şifrelemesi (SSE) ile korunur. Varsayılan olarak, bu diskler üzerindeki veriler Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. BYOK'u etkinleştiriseniz, Azure HDInsight'ın azure key vault'u kullanarak kullanması ve yönetmesi için şifreleme anahtarını sağlarsınız.

Anahtar Kasası, küme depolama (Azure Depolama Hesapları ve Azure Veri Gölü Depolama) için anahtarları yönetmek için Azure HDInsight dağıtımları ile de kullanılabilir

Azure HDInsight'ta Apache Kafka için kendi anahtarınızı nasıl getireniz:

https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-byok

Azure Depolama Hesapları için şifreleme anahtarlarını yönetme:

https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Kimlikleri güvenli ve otomatik olarak yönetme

**Yönlendirme**: Yönetilen kimlikler, kümelerinizin Azure Active Directory etki alanı hizmetlerine erişmesine, Azure Key Vault'a erişmesine veya Azure Veri Gölü Depolama Gen2'deki dosyalara erişmesine izin vermek için Azure HDInsight'ta kullanılabilir.

Azure HDInsight ile Yönetilen Kimlikleri Anlayın:

https://docs.microsoft.com/azure/hdinsight/hdinsight-managed-identities

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: İstenmeden kimlik bilgisi maruziyetini ortadan kaldırın

**Kılavuz**: Azure HDInsight dağıtımınızla ilgili herhangi bir kod kullanıyorsanız, kod içindeki kimlik bilgilerini tanımlamak için Kimlik Bilgisi Tarayıcısı'nı uygulayabilirsiniz. Kimlik Bilgisi Tarayıcısı, keşfedilen kimlik bilgilerinin Azure Anahtar Kasası gibi daha güvenli konumlara taşınmasını da teşvik edecektir. 

Kimlik Bilgisi Tarayıcı nasıl kurulturur:

https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için [Güvenlik Kontrolü: Malware Defense](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense)' e bakın.*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımlarını kullanın

**Yönlendirme**: Azure HDInsight, cluster düğüm görüntüleri için önceden yüklenmiş ve etkin leştirilmiş Clamscan ile birlikte gelir, ancak yazılımı yönetmeniz ve Clamscan'ın ürettiği günlükleri el ile toplamanız/izlemeniz gerekir.

Azure HDInsight için Clamscan'ı anlayın:

https://docs.microsoft.com/azure/hdinsight/hdinsight-faq#security-and-certificates

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: İşlem yapmayan Azure kaynaklarına yüklenecek dosyaları önceden taraya

**Yol gösterici**: Azure hizmetlerini destekleyen temel ana bilgisayarda Microsoft Antimalware etkindir, ancak müşteri içeriğinde çalışmaz.

Veri Gölü Depolama, Blob Depolama, vb. gibi Azure HDInsight küme dağıtımınızla ilgili tüm Azure kaynaklarına yüklenen dosyaları önceden tarayabilirsiniz. Microsoft bu gibi durumlarda müşteri verilerine erişemez.

Azure Bulut Hizmetleri ve Sanal Makineler için Microsoft Kötü Amaçlı Yazılımdan Koruma'yı anlayın:

 https://docs.microsoft.com/azure/security/fundamentals/antimalware

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Kötü amaçlı yazılımdan koruma yazılımının ve imzalarının güncelleştirilmesini sağlayın

**Yönlendirme**: Azure HDInsight, cluster düğüm görüntüleri için önceden yüklenmiş ve etkin leştirilmiş Clamscan ile birlikte gelir. Clamscan motor ve tanım güncelleştirmelerini otomatik olarak gerçekleştirir, ancak günlüklerin toplanması ve yönetiminin el ile yapılması gerekir.

Azure Azure HDInsight için Clamscan'ı anlayın:

https://docs.microsoft.com/azure/hdinsight/hdinsight-faq#security-and-certificates

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Düzenli otomatik yedeklemeler sağlayın

**Yönlendirme**: HDInsight küme veri deposu için bir Azure Depolama Hesabı kullanırken, uygun artıklık seçeneğini (LRS,ZRS, GRS, RA-GRS) seçin.  Azure HDInsight küme veri deposu için bir Azure SQL Veritabanı kullanırken, Etkin Coğrafi çoğaltmayı yapılandırın.

Azure Depolama Hesapları için depolama artıklığı nasıl yapılandırılmaz:

https://docs.microsoft.com/azure/storage/common/storage-redundancy

Azure SQL Veritabanları için artıklık nasıl yapılandırılır:

https://docs.microsoft.com/azure/sql-database/sql-database-active-geo-replication

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Komple sistem yedeklemelerini gerçekleştirin ve müşteri yönetilen anahtarları yedekleme

**Yönlendirme**: Azure HDInsight küme veri deposu için Bir Azure Depolama Hesabı kullanırken, uygun artıklık seçeneğini (LRS, ZRS, GRS, RA-GRS) seçin. Azure HDInsight dağıtımınızın herhangi bir bölümü için Azure Key Vault kullanıyorsanız, anahtarlarınızın yedekolduğundan emin olun.

Azure HDInsight kümeniz için depolama seçenekleri seçin:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-compare-storage-options

Azure Depolama Hesapları için depolama artıklığı nasıl yapılandırılmaz:

https://docs.microsoft.com/azure/storage/common/storage-redundancy

Azure'da Key Vault tuşlarını yedekleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Müşteri yönetilen anahtarlar dahil tüm yedeklemeleri doğrulayın

**Kılavuz**: Azure KEY Vault, Azure HDInsight dağıtımınızda kullanılıyorsa, yedeklenen müşteri yönetilen anahtarlarının geri yüklemesini test edin.

Azure HDInsight'ta Apache Kafka için kendi anahtarınızı nasıl getireniz:

https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-byok

Azure'da anahtar kasa sıtuşlarını geri yükleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Yedeklerin ve müşteri yönetilen anahtarlarının korunmasını sağlayın

**Kılavuz**: Azure KEY Vault'unuz Azure HDInsight dağıtımınızda kullanılıyorsa, anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Anahtar Kasası'ndaki Yumuşak Silme'yi etkinleştirin.

Azure Anahtar Kasası'nda Yumuşak Silme nasıl etkinleştirilir:

https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

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

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10.3: Test güvenlik yanıt yordamları

**Rehberlik**: Sistemlerinizin olay yanıt yeteneklerini düzenli olarak test etmek için alıştırmalar yapın. Zayıf noktaları ve boşlukları belirleyin ve planı gerektiği gibi gözden geçirin. NIST'in yayınına bakın: BT Planları ve Yetenekleri için Test, Eğitim ve Egzersiz Programları Kılavuzu:https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Güvenlik olayı iletişim bilgilerini sağlayın ve güvenlik olayları için uyarı bildirimlerini yapılandırma

**Kılavuz :** Microsoft Güvenlik Yanıt Merkezi (MSRC) verilerinize yasadışı veya yetkisiz bir taraf tarafından erişildiğini tespit ederse, güvenlik olayı iletişim bilgileri Microsoft tarafından sizinle iletişim kurmak için kullanılır.

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
