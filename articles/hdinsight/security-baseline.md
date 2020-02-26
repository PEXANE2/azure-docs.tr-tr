---
title: HDInsight için Azure Güvenlik temeli
description: HDInsight için Azure Güvenlik temeli
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 02/22/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 7d6b4f8a7965a7cbcab2616650b40ed93087072b
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77589980"
---
# <a name="azure-security-baseline-for-hdinsight"></a>HDInsight için Azure Güvenlik temeli

HDInsight için Azure Güvenlik taban çizgisi, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetlerin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Rehberlik**: Azure HDInsight 'ta çevre güvenliği sanal ağlar aracılığıyla elde edilir. Kurumsal Yönetici, sanal ağ içinde bir küme oluşturabilir ve sanal ağa erişimi kısıtlamak için bir ağ güvenlik grubu (NSG) kullanabilir. Yalnızca gelen ağ güvenlik grubu kurallarında izin verilen IP adresleri Azure HDInsight kümesiyle iletişim kurabiliyor. Bu yapılandırma, çevre güvenliği sağlar. Bir sanal ağda dağıtılan tüm kümelerin, küme ağ geçitlerine özel HTTP erişimi için sanal ağ içindeki özel bir IP adresine çözümlenen özel bir uç noktası da olur.


Azure HDInsight 'ı bir sanal ağ içinde dağıtma ve bir ağ güvenlik grubuyla güvenli hale getirme:

https://docs.microsoft.com/azure/hdinsight/hdinsight-create-virtual-network

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Kılavuz**: Azure Güvenlik Merkezi 'ni kullanın ve Azure HDInsight kümenizin güvenliğini sağlamak için kullanılan sanal ağ, alt ağ ve ağ güvenlik grubu için ağ koruma önerilerini düzeltin. Ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve günlükleri bir Azure depolama hesabına trafik denetimine gönderin. Ayrıca Azure Log Analytics çalışma alanına NSG akış günlükleri gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Azure Trafik Analizi kullanabilirsiniz. Azure Trafik Analizi 'nin bazı avantajları, ağ etkinliğini görselleştirme ve etkin noktaları tanımlayabilir, güvenlik tehditlerini belirleyebilir, trafik akışı düzenlerini anlayabilir ve ağ ile hatalı yapılandırmaların nasıl belirlenmesine olanak sağlar.


NSG akış günlüklerini etkinleştirme:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal


Azure Trafik Analizi etkinleştirme ve kullanma:

https://docs.microsoft.com/azure/network-watcher/traffic-analytics


Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlayın:

https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: kullanılamıyor; Kıyaslama, Web uygulamalarını barındıran Azure Apps hizmeti veya bilgi işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Kılavuz**: DDoS saldırılarına karşı korumalar Için Azure HDInsight 'ın dağıtıldığı sanal ağ üzerinde Azure DDoS standart korumasını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.


DDoS korumasını yapılandırma:

https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection


Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama:

https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Kılavuz**: Azure HDInsight kümenizi korumak için kullanılan alt ağa eklenen NSG için ağ güvenlik grubu (NSG) flog günlüklerini etkinleştirin. Flow kayıtları oluşturmak için NSG akış günlüklerini bir Azure depolama hesabına kaydedin. Anormal etkinlikleri araştırmak için gerekliyse, Azure ağ Izleyicisi paket yakalamayı etkinleştirin.


NSG akış günlüklerini etkinleştirme:

https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal


Ağ Izleyicisi 'ni etkinleştirme:

https://docs.microsoft.com/azure/network-watcher/network-watcher-create

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: gereksinim, Azure GÜVENLIK denetim kimliği 1,1; karşılandı. Azure HDInsight kümesini bir sanal ağa dağıtın ve bir ağ güvenlik grubu (NSG) ile güvenli hale getirin.

Azure HDInsight için gelen trafik gerektiren birkaç bağımlılık vardır. Gelen yönetim trafiği bir güvenlik duvarı cihazından gönderilemez. Gerekli yönetim trafiğine yönelik kaynak adresleri bilinirler ve yayımlanır. Yalnızca güvenilen konumlardan gelen trafiğin güvenliğini sağlamak için bu bilgilerle ağ güvenlik grubu kuralları oluşturun.

Bir sanal ağ içinde HDInsight dağıtma ve bir ağ güvenlik grubuyla güvenli hale getirme: https://docs.microsoft.com/azure/hdinsight/hdinsight-create-virtual-network

HDInsight bağımlılıklarını ve Güvenlik Duvarı kullanımını anlayın: https://docs.microsoft.com/azure/hdinsight/hdinsight-restrict-outbound-traffic

HDInsight yönetim IP adresleri: https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: kullanılamıyor; Kıyaslama, Web uygulamalarını barındıran Azure Apps hizmeti veya bilgi işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Kılavuz**: Azure HDInsight kümenizin dağıtıldığı alt ağa bağlı ağ güvenlik grupları (NSG) üzerinde ağ erişim denetimleri tanımlamak için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örn., Apimanaya) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.


Azure HDInsight için hizmet etiketlerini anlama ve kullanma:

https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Kılavuz**: Azure HDInsight kümeniz ile ilgili ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure HDInsight kümenizin ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. HDInsight" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın.


Ayrıca, tek bir şema tanımında Azure Resource Manager şablonları, RBAC denetimleri ve ilkeleri gibi anahtar ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanabilirsiniz. Şema 'i yeni abonelikler ve ortamlara kolayca uygulayın ve sürüm oluşturma aracılığıyla denetimi ve yönetimi ayrıntılı olarak ayarlayın.


Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme:

https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0


Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


Azure Blueprint oluşturma:

https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: ağ güvenlik grubu (NSG) ve Azure HDInsight kümeniz ile ilişkili ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketler kullanın. Bireysel NSG kuralları için "Açıklama" alanını kullanarak ağa giden/giden trafiğe izin veren kuralların iş gereksinimini ve/veya süresini (vs.) belirtin.


Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın.


Azure PowerShell veya Azure komut satırı arabirimi 'ni (CLı) kullanarak bu kaynakları etiketlere göre arayabilir veya bunlarla ilgili eylemler gerçekleştirebilirsiniz.


Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags


Sanal ağ oluşturma:

https://docs.microsoft.com/azure/virtual-network/quick-create-portal


Güvenlik Yapılandırması ile NSG oluşturma:

https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Rehberlik**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını Izleyin ve Azure HDInsight dağıtımlarınızla ilgili ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.


Azure etkinlik günlüğü olaylarını görüntüleme ve alma:

https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view


Azure Izleyici 'de uyarı oluşturma: https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, Azure HDInsight küme bileşenleri için zaman kaynaklarını korur, işlem dağıtımlarınız için zaman eşitlemesini güncelleştirebilirsiniz.


Azure işlem kaynakları için Saat eşitlemesini yapılandırma:

https://docs.microsoft.com/azure/virtual-machines/windows/time-sync

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: küme tarafından oluşturulan güvenlik verilerini toplamak Için Azure HDInsight kümenizi Azure izleyici 'ye ekleyebilirsiniz. Ortamdaki tehditleri algılamak ve yanıtlamak için özel sorgulardan yararlanın. 


Azure Izleyici 'ye Azure HDInsight kümesi ekleme:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-tutorial


Azure HDInsight kümesi için özel sorgular oluşturma:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-use-queries

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: HDInsight kümesi Için Azure izleyicisini etkinleştirin ve bunu bir Log Analytics çalışma alanına yönlendirin. Bu, tüm Azure HDInsight küme düğümleri için ilgili küme bilgilerini ve işletim sistemi ölçümlerini günlüğe kaydeder.


HDInsight kümesi için günlüğe kaydetmeyi etkinleştirme:

 https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-tutorial


HDInsight günlüklerini sorgulama:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-use-queries

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Kılavuz**: Azure HDInsight kümesini Azure izleyici 'ye ekleme. Kullanılan Log Analytics çalışma alanının, kuruluşunuzun uyumluluk düzenlemelerine göre ayarlanmış günlük tutma süresine sahip olduğundan emin olun.


Azure Izleyici 'ye Azure HDInsight kümesi ekleme:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-tutorial


Log Analytics çalışma alanı saklama süresini yapılandırma:

https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure HDInsight kümesini Azure izleyici 'ye ekleme. Kullanılan Azure Log Analytics çalışma alanının, kuruluşunuzun uyumluluk düzenlemelerine göre ayarlanmış günlük tutma süresine sahip olduğundan emin olun.


Azure Izleyici 'ye Azure HDInsight kümesi ekleme:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-tutorial


Log Analytics çalışma alanı saklama süresini yapılandırma:

https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Kılavuz**: Azure HDInsight günlüklerini sorgulamak için Azure Log Analytics çalışma alanı sorgularını kullanın:


Azure HDInsight kümeleri için özel sorgular oluşturma:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-use-queries

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Rehberlik**: Azure Log Analytics çalışma alanını, Güvenlik günlüklerine ve Azure HDInsight kümeniz ile ilgili olaylarda bulunan anormal etkinlikleri izlemek ve uyarmak için kullanın.


Azure Güvenlik Merkezi 'nde uyarıları yönetme:

https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts


Log Analytics günlük verilerinde uyarı alma:

https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Kılavuz**: Azure HDInsight, küme düğümü görüntüleri için önceden yüklenmiş ve etkin olan Azure HDInsight ile birlikte gelir, ancak yazılımı yönetmeniz ve ClamScan 'ın ürettiği tüm günlükleri el ile toplamanız/izlemeniz gerekir.


ClamScan 'ı anlayın:

https://docs.microsoft.com/azure/hdinsight/hdinsight-faq#security-and-certificates

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: DNS günlüğü için üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: konsol günlüğünü her düğüm temelinde el ile yapılandırın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve Access Control](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Kılavuz**: Azure HDInsight kümesinin küme sağlanması sırasında oluşturulan yerel yönetim hesabının kaydını ve oluşturduğunuz diğer hesapları saklayın. Ayrıca, Azure AD tümleştirmesi kullanılıyorsa, Azure AD 'nin açıkça atanması ve bu nedenle sorgulanabilir olması gereken yerleşik rolleri vardır. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın.


Ayrıca, Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini de kullanabilirsiniz.


Azure AD 'de PowerShell ile dizin rolü alma:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0


Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0


Azure Güvenlik Merkezi ile kimlik ve erişimi izleme:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: bir küme sağlanırken Azure, Web portalı ve Secure Shell (SSH) erişimi için yeni parolalar oluşturmanızı gerektirir. Değiştirilecek varsayılan parola yoktur, ancak SSH ve Web portalı erişimi için farklı parolalar belirtebilirsiniz.


Azure HDInsight kümesi sağlanırken parolaları ayarlama:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: Azure Active Directory Ile Azure HDInsight kümesi Için kimlik doğrulamasını tümleştirin. Adanmış yönetim hesaplarının kullanımı etrafında ilke ve yordamlar oluşturun.


Ayrıca, Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini de kullanabilirsiniz.


Azure HDInsight kimlik doğrulamasını Azure Active Directory ile tümleştirme:

https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds


Azure Güvenlik Merkezi ile kimlik ve erişimi izleme:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: herhangi bir parola sağlamadan Multi-Factor Authentication kullanarak kurumsal GÜVENLIK PAKETI (ESP) kümelerinde oturum açmak Için Azure HDInsight kimlik Aracısı 'nı kullanın. Azure portal gibi diğer Azure hizmetlerinde zaten oturum açtıysanız, Azure HDInsight kümenizde çoklu oturum açma (SSO) deneyimiyle oturum açabilirsiniz.


Azure HDInsight KIMLIK Aracısı 'nı etkinleştirme:

https://docs.microsoft.com/azure/hdinsight/domain-joined/identity-broker#enable-hdinsight-id-broker

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Kılavuz**: Azure AD MFA 'yı etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin. Yapılandırılmış Kurumsal Güvenlik Paketi olan Azure HDInsight kümeleri, etki alanı kullanıcılarının, kümelerle kimlik doğrulamak ve büyük veri işleri çalıştırmak için kendi etki alanı kimlik bilgilerini kullanabilmesi için bir etki alanına bağlı olabilir. Multi-Factor Authentication (MFA) ile kimlik doğrulaması etkinleştirildiğinde, kullanıcılardan ikinci bir kimlik doğrulama faktörü sağlaması istenir.


Azure 'da MFA 'yı etkinleştirme:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted


Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure HDInsight kümelerinizi ve ilgili kaynaklarınızı oturum açıp yapılandırmak için yapılandırılmış çok faktörlü kimlik DOĞRULAMASıYLA (MFA) Paws (ayrıcalıklı erişim iş istasyonları) kullanın.


Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin:

https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations


Azure 'da MFA 'yı etkinleştirme:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Rehberlik**: etki alanı kullanıcılarının kimlik doğrulaması için etki alanı kimlik bilgilerini kullanabilmesi için, yapılandırılmış kurumsal güvenlik paketi olan Azure HDInsight kümeleri bir etki alanına bağlanabilir. AAD ortamında şüpheli veya güvenli olmayan bir etkinlik gerçekleştiğinde günlüklerin ve uyarıların oluşturulması için Azure Active Directory (AAD) güvenlik raporları kullanabilirsiniz. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi 'ni kullanın.


Riskli etkinlik için işaretlenen AAD kullanıcılarını belirleme:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk


Azure Güvenlik Merkezi 'nde Kullanıcı kimliğini ve erişim etkinliğini izleme:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: etki alanı kullanıcılarının kimlik doğrulaması için etki alanı kimlik bilgilerini kullanabilmesi için, yapılandırılmış kurumsal güvenlik paketi olan Azure HDInsight kümeleri bir etki alanına bağlanabilir. IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek için adlandırılmış konumlarda koşullu erişim kullanın.


Azure 'da adlandırılmış konumlar nasıl yapılandırılır:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (AAD) kullanın. AAD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. AAD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

Kurumsal Güvenlik Paketi (ESP) ile yapılandırılmış Azure HDInsight kümeleri, etki alanı kullanıcılarının, kümelerle kimlik doğrulaması yapmak için etki alanı kimlik bilgilerini kullanabilmesi için bir etki alanına bağlanabilir.


AAD örneği oluşturma ve yapılandırma:

https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant


Azure HDInsight 'ta Azure Active Directory Domain Services ile Kurumsal Güvenlik Paketi yapılandırma:

https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Kılavuz**: Azure HDInsight kümeniz ile Azure ACTIVE DIRECTORY (AAD) kimlik doğrulaması kullanın. AAD, eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemelerini kullanın. Kullanıcının erişimi, yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için düzenli olarak incelenebilir. 


Azure kimlik erişimi Incelemelerini kullanma:

https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Rehberlik**: devre dışı bırakılmış hesaplara erişim girişimlerini izlemek için Azure ACTIVE DIRECTORY (AAD) oturum açma ve denetim günlüklerini kullanın; Bu Günlükler, herhangi bir üçüncü taraf SıEM/izleme aracıyla tümleştirilebilir.


AAD Kullanıcı hesapları için Tanılama ayarları oluşturarak, Denetim günlüklerini ve oturum açma günlüklerini bir Azure Log Analytics çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. Azure Log Analytics çalışma alanında istenen uyarıları yapılandırın.


Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Kılavuz**: Kurumsal GÜVENLIK PAKETI (ESP) ile yapılandırılmış Azure HDInsight kümeleri, etki alanı kullanıcılarının, kümelerle kimlik doğrulamak için etki alanı kimlik bilgilerini kullanabilmesi için bir etki alanına bağlanabilir.  Otomatik yanıtları Kullanıcı kimlikleriyle ilgili şüpheli eylemler için yapılandırmak üzere Azure Active Directory (AAD) risk algılamaları ve kimlik koruması özelliğini kullanın. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz.


AAD riskli oturum açma işlemlerini görüntüleme:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins


Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme:

https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: kullanılamıyor; Müşteri Kasası henüz Azure HDInsight için desteklenmiyor.

Desteklenen Müşteri Kasası hizmetleri listesi: https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması Için Azure HDInsight dağıtımlarınızla ilgili kaynaklarda yer alan etiketler kullanın.


Etiketler oluşturma ve kullanma:

https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Azure HDInsight kümeleri ve ilişkili tüm depolama hesapları, sanal ağ/alt ağ ile ayrılmalıdır, uygun şekilde etiketlenebilir ve bir ağ güvenlik grubu (NSG) veya Azure Güvenlik duvarı içinde güvenliği sağlanmış olmalıdır. Küme verileri, güvenli bir Azure depolama hesabı veya Azure Data Lake Storage (Gen1 veya Gen2) içinde bulunmalıdır.


Azure HDInsight kümeniz için depolama seçeneklerini belirleyin:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-compare-storage-options


Azure Data Lake Storage güvenliğini sağlama:

https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview


Azure depolama hesaplarının güvenliğini sağlama:

https://docs.microsoft.com/azure/storage/common/storage-security-guide

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: hassas bilgileri depolayan veya Işleyen Azure HDInsight kümeleri için, etiketleri kullanarak kümeyi ve ilgili kaynakları hassas olarak işaretleyin. Veri kaybı riskini azaltmak için Azure Güvenlik Duvarı 'nı kullanarak Azure HDInsight kümeleri için giden ağ trafiğini kısıtlayın.


Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.


Azure Güvenlik Duvarı ile Azure HDInsight kümeleri için giden trafiği kısıtlama:

https://docs.microsoft.com/azure/hdinsight/hdinsight-restrict-outbound-traffic


Azure 'da müşteri veri korumasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: yoldaki tüm hassas bilgileri şifreleyin. Azure HDInsight kümenize veya küme veri Depolarınıza (Azure Storage hesapları veya Azure Data Lake Storage 1./Gen2) bağlanan tüm istemcilerin TLS 1,2 veya üzerini anlaşamadığından emin olun. Microsoft Azure kaynaklar varsayılan olarak TLS 1,2 ' i uzlaşacaktır. 


İletimde Azure Data Lake Storage şifrelemeyi anlayın:

https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview


Aktarım sırasında Azure depolama hesabı şifrelemesini anlayın:

https://docs.microsoft.com/azure/storage/common/storage-security-guide#encryption-in-transit

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: veri tanımlama, sınıflandırma ve kayıp önleme özellikleri, Azure depolama veya işlem kaynakları için henüz kullanılamıyor. Uyumluluk amacıyla gerekliyse üçüncü taraf çözümünü uygulayın.



Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.



Azure 'da müşteri veri korumasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Kılavuz**: Azure HDInsight kurumsal GÜVENLIK PAKETI (ESP) ile, dosyalarda, klasörlerde, veritabanlarında, tablolarda ve satırlarda/sütunlarda depolanan verilerinize ilişkin ayrıntılı erişim denetimi ve veri gizleme ilkeleri oluşturmak ve yönetmek Için Apache Ranger 'ı kullanabilirsiniz. Hadoop Yöneticisi, Apache Ranger 'daki eklentileri kullanarak Apache Hive, HBase, Kafka ve Spark 'ı güvenli hale getirmek için rol tabanlı erişim denetimi 'ni (RBAC) yapılandırabilir.

RBAC ilkelerini Apache Ranger ile yapılandırmak, izinleri kuruluştaki bir rolle ilişkilendirmenize olanak sağlar. Bu soyutlama katmanı, kişilerin yalnızca iş sorumluluklarını gerçekleştirmek için gerekli izinlere sahip olmasını kolaylaştırır.

HDInsight 'ta Azure Active Directory Domain Services olan Kurumsal Güvenlik Paketi konfigürasyonları: https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds

Azure HDInsight 'ta Kurumsal güvenliğe genel bakış: https://docs.microsoft.com/azure/hdinsight/domain-joined/hdinsight-security-overview



**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: hassas bilgileri depolayan veya Işleyen Azure HDInsight kümeleri için, etiketleri kullanarak kümeyi ve ilgili kaynakları hassas olarak işaretleyin. Veri tanımlama, sınıflandırma ve kayıp önleme özellikleri, Azure depolama veya işlem kaynakları için henüz kullanılamıyor. Uyumluluk amacıyla gerekliyse üçüncü taraf çözümünü uygulayın.


Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.


Azure 'da müşteri veri korumasını anlayın:

https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: Apache Hive ve Apache Oozie meta verilerini depolamak IÇIN Azure SQL veritabanı KULLANıYORSANıZ, SQL verilerinin her zaman şifrelenmeye devam ettiğinden emin olun. Azure depolama hesapları ve Data Lake Storage (Gen1 veya Gen2) için, Microsoft 'un şifreleme anahtarlarınızı yönetmesine izin vermeniz önerilir, ancak kendi anahtarlarınızı yönetme seçeneğiniz vardır.



Azure depolama hesapları için şifreleme anahtarlarını yönetme:

https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal



Müşteri tarafından yönetilen şifreleme anahtarlarını kullanarak Azure Data Lake Storage oluşturma:

https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal



Azure SQL veritabanı için şifrelemeyi anlayın:

https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview#data-encryption



Müşteri tarafından yönetilen anahtarları kullanarak SQL veritabanı için Saydam Veri Şifrelemesi yapılandırma:

https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?tabs=azure-portal

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Rehberlik**: küme verilerine KARŞı tüm CRUD işlemlerini izlemek ve günlüğe kaydetmek Için Azure HDInsight kümeleriyle Ilişkili Azure depolama hesapları Için tanılama ayarlarını yapılandırın. Azure HDInsight kümesiyle ilişkili tüm depolama hesapları veya Data Lake depoları için denetimi etkinleştirin.


Azure depolama hesabı için ek günlüğe kaydetme/denetim nasıl etkinleştirilir:

https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account


Azure Data Lake Storage için ek günlüğe kaydetme/denetim nasıl etkinleştirilir:

https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-diagnostic-logs

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: üçüncü taraf bir güvenlik açığı yönetim çözümü uygulayın.


İsteğe bağlı olarak, Rapid7, Qualys veya başka herhangi bir güvenlik açığı yönetim platformu aboneliğiniz varsa, Azure HDInsight küme düğümlerine güvenlik açığı değerlendirmesi aracılarını yüklemek ve ilgili Portal üzerinden düğümleri yönetmek için betik eylemleri ' ni kullanabilirsiniz.


Rapid7 aracısını el Ile yüklemek:

https://insightvm.help.rapid7.com/v1.0/docs/agent-installation-on-linux


Qualys aracısını el Ile yüklemek:

https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf


Betik eylemlerini kullanma:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Kılavuz**: otomatik sistem güncelleştirmeleri küme düğümü görüntüleri için etkinleştirildi, ancak güncelleştirmelerin uygulandığından emin olmak için düzenli aralıklarla küme düğümlerini yeniden başlatmanız gerekir.


Temel Azure HDInsight düğüm görüntülerini sürdürmek ve güncelleştirmek için Microsoft.


HDInsight kümeleri için işletim sistemi düzeltme eki uygulama zamanlamasını yapılandırma:

https://docs.microsoft.com/azure/hdinsight/hdinsight-os-patching

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Kılavuz**: Azure HDInsight kümelerinizi yama için betik eylemlerini veya diğer mekanizmaları kullanın. Yeni oluşturulan kümeler en son güvenlik düzeltme ekleri dahil olmak üzere her zaman en son kullanılabilir güncelleştirmelere sahip olur.


Linux tabanlı Azure HDInsight kümeleri için işletim sistemi düzeltme eki uygulama zamanlamasını yapılandırma:

https://docs.microsoft.com/azure/hdinsight/hdinsight-os-patching


Betik eylemlerini kullanma:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: zaman içinde güvenlik açığı taramalarının karşılaştırılabilmesi için bir üçüncü taraf güvenlik açığı yönetimi çözümü uygulayın. Rapid7 veya Qualys aboneliğiniz varsa, geri dönüş güvenlik açıklarını görüntülemek ve karşılaştırmak için bu satıcının portalını kullanabilirsiniz.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: ortak bir risk Puanlama programı (örneğin, ortak güvenlik açığı Puanlama sistemi) veya üçüncü taraf tarama aracınız tarafından sunulan varsayılan risk derecelendirmelerini kullanın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: aboneliklerinizde Azure HDInsight kümeleri de dahil olmak üzere tüm kaynakları (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) sorgulamak/saptamak Için Azure Kaynak grafiğini kullanın.  Kiracınızda uygun (okuma) izinleriniz olduğundan ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerinin listesini belirleyebildiğinizden emin olun.


Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.


Azure Graph ile sorgu oluşturma:

https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal


Azure aboneliklerinizi görüntüleme:

https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0


Azure RBAC 'yi anlama:

https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

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

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

Aboneliklerinizin içindeki kaynakları sorgulamak/öğrenmek için Azure Kaynak grafiğini kullanın.  Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph ile sorgu oluşturma: https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: onaylanmamış yazılım uygulamalarına yönelik küme düğümlerini izlemek için bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: aboneliklerinizde Azure HDInsight kümeleri de dahil olmak üzere tüm kaynakları (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) sorgulamak/saptamak Için Azure Kaynak grafiğini kullanın.  Keşfettiğiniz onaylanmamış tüm Azure kaynaklarını kaldırın. Azure HDInsight küme düğümleri için, onaylanmamış yazılımları kaldırmak veya uyarmak üzere bir üçüncü taraf çözümü uygulayın.


Azure Graph ile sorgu oluşturma:

https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Kılavuz**: Azure HDInsight küme düğümleri için, yetkisiz yazılımın yürütülmesini engelleyen bir üçüncü taraf çözümü uygulayın.

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

**Kılavuz**: Azure HDInsight küme düğümleri için, yetkisiz dosya türlerinin yürütülmesini engelleyen bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6,11: kullanıcıların betikler aracılığıyla Azure Resources Manager ile etkileşim kurma yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak Için Azure koşullu erişimi kullanın.


Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma:

https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: kullanılamıyor; Bu, kümenin kullanıcıları (yönetici olmayanlar) işleri çalıştırmak için ayrı düğümlere erişime gerek duymadığından, Azure HDInsight için geçerli değildir. Küme yöneticisinin tüm küme düğümlerine kök erişimi vardır.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: kullanılamıyor; Kıyaslama, Web uygulamalarını barındıran Azure Apps hizmeti veya bilgi işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: geçerli değil

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: HDInsight kümenizin ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak Için "Microsoft. HDInsight" ad alanındaki Azure ilke diğer adlarını kullanın.


Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme:

https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0


Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: Microsoft tarafından yönetilen ve korunan Azure HDInsight Işletim sistemi görüntüleri. Küme düğümlerinizin işletim sistemine yönelik güvenli yapılandırmaların uygulamamasından sorumlu müşteri. 

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure HDInsight kümeleriniz ve ilgili kaynaklarınız için güvenli ayarları zorlamak üzere Azure ilkesi [reddetme] ve [dağıtım yok] kullanın.


Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage


Azure Ilke efektlerini anlayın:

https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: Microsoft tarafından yönetilen ve korunan Azure HDInsight Işletim sistemi görüntüleri. İşletim sistemi düzeyinde durum yapılandırması uygulamaktan sorumlu müşteri.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure devops veya Azure Repos kullanın.



Azure DevOps 'da kod depolama:

https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops



Azure Repos belgeleri:

https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: kullanılamıyor; özel görüntüler Azure HDInsight için geçerli değildir.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturmak üzere "Microsoft. HDInsight" ad alanındaki Azure ilke diğer adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.



Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: küme düğümü işletim sistemlerinizde istenen durumu korumak için bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma izlemeyi uygulayın

**Rehberlik**: HDInsight kümenizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak Için "Microsoft. HDInsight" ad alanındaki Azure ilke diğer adlarını kullanın.


Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme:

https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0


Azure Ilkesini yapılandırma ve yönetme:

https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: küme düğümü işletim sistemlerinin durumunu izlemek için bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Kılavuz**: Azure hdınsight, Apache Kafka için kendi anahtarını getir (bYok) desteği içerir. Bu özellik, bekleyen verileri şifrelemek için kullanılan anahtarları sahip olmanızı ve yönetmenizi sağlar.


Azure HDInsight 'taki tüm yönetilen diskler Azure Depolama Hizmeti Şifrelemesi (SSE) ile korunmaktadır. Varsayılan olarak, bu disklerdeki veriler Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. BYOK ' u etkinleştirirseniz, Azure HDInsight 'ın Azure Key Vault kullanarak bunu kullanmasını ve yönetmesini sağlamak için şifreleme anahtarını sağlarsınız.


Key Vault Ayrıca, küme depolama (Azure depolama hesapları ve Azure Data Lake Storage) için anahtarları yönetmek üzere Azure HDInsight dağıtımlarıyla birlikte kullanılabilir.


Azure HDInsight 'ta Apache Kafka için kendi anahtarınızı getirme:

https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-byok


Azure depolama hesapları için şifreleme anahtarlarını yönetme:

https://docs.microsoft.com/azure/storage/common/storage-encryption-keys-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: Yönetilen kimlikler, kümelerinizin Azure Active Directory etki alanı hizmetlerine erişmesine, Azure Key Vault erişmesine veya Azure Data Lake Storage 2. dosyalara erişmesine olanak tanımak Için Azure HDInsight 'ta kullanılabilir.


Azure HDInsight ile yönetilen kimlikleri anlayın:

https://docs.microsoft.com/azure/hdinsight/hdinsight-managed-identities

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Kılavuz**: Azure HDInsight dağıtımınızla ilgili herhangi bir kod kullanıyorsanız, kod içindeki kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayabilirsiniz. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder. 


Kimlik bilgisi tarayıcısını ayarlama:

https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Kılavuz**: Azure HDInsight, küme düğümü görüntüleri için önceden yüklenmiş ve etkin olan Azure HDInsight ile birlikte gelir, ancak yazılımı yönetmeniz ve ClamScan 'ın ürettiği tüm günlükleri el ile toplamanız/izlemeniz gerekir.


Azure HDInsight için ClamScan 'ı anlayın:

https://docs.microsoft.com/azure/hdinsight/hdinsight-faq#security-and-certificates

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel alınan konakta etkinleştirilir, ancak müşteri içeriğinde çalışmaz.


Data Lake Storage, BLOB depolama vb. gibi Azure HDInsight küme dağıtımınızla ilişkili tüm Azure kaynaklarına yüklenen dosyaları önceden tarayın. Microsoft bu örneklerdeki müşteri verilerine erişemez.


Azure Cloud Services ve sanal makineler için Microsoft kötü amaçlı yazılımdan koruma 'i anlayın:

 https://docs.microsoft.com/azure/security/fundamentals/antimalware

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: Azure HDInsight, küme düğümü görüntüleri için önceden yüklenmiş ve etkin bir şekilde birlikte gelir. ClamScan, altyapı ve tanım güncelleştirmelerini otomatik olarak gerçekleştirebilir, ancak günlüklerin toplama ve yönetiminin el ile gerçekleştirilmesi gerekir.


Azure Azure HDInsight için ClamScan 'ı anlayın:

https://docs.microsoft.com/azure/hdinsight/hdinsight-faq#security-and-certificates

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: HDInsight kümesi veri deposu Için bir Azure depolama hesabı kullanırken uygun artıklık seçeneğini (LRS, ZRS, GRS, RA-GRS) seçin.  Azure HDInsight kümesi veri deposu için bir Azure SQL veritabanı kullanırken, etkin coğrafi çoğaltma 'yı yapılandırın.


Azure depolama hesapları için depolama yedekliği nasıl yapılandırılır:

https://docs.microsoft.com/azure/storage/common/storage-redundancy


Azure SQL veritabanları için yedeklilik yapılandırma:

https://docs.microsoft.com/azure/sql-database/sql-database-active-geo-replication

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Kılavuz**: Azure HDInsight kümesi veri deposu Için bir Azure depolama hesabı kullanırken uygun artıklık seçeneğini (LRS, ZRS, GRS, RA-GRS) seçin. Azure HDInsight dağıtımınızın herhangi bir bölümü için Azure Key Vault kullanıyorsanız, anahtarlarınızın yedeklendiğinden emin olun.


Azure HDInsight kümeniz için depolama seçeneklerini belirleyin:

https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-compare-storage-options


Azure depolama hesapları için depolama yedekliği nasıl yapılandırılır:

https://docs.microsoft.com/azure/storage/common/storage-redundancy


Azure 'da Key Vault anahtarlarını yedekleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Kılavuz**: Azure hdınsight dağıtımınızda Azure Key Vault kullanılıyorsa, yedeklenen müşteri tarafından yönetilen anahtarların test geri yüklenmesi.


Azure HDInsight 'ta Apache Kafka için kendi anahtarınızı getirme:

https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-byok


Azure 'da Anahtar Kasası anahtarlarını geri yükleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: Azure Key Vault Azure HDInsight dağıtımıyla birlikte kullanılıyorsa, anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak Için Key Vault geçici silme özelliğini etkinleştirin.


Azure Key Vault 'da geçici silmeyi etkinleştirme:

https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

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

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin. NıST yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin: https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır.



Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama:

https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Uyarılar Sentinel 'i akışa almak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.



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



Microsoft 'un yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı Microsoft 'un stratejisi ve Red ekibi oluşturma ve canlı site sızma testini yürütme hakkında daha fazla bilgi edinebilirsiniz: https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

