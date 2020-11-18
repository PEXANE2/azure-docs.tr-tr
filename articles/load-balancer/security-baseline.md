---
title: Azure Load Balancer için Azure Güvenlik temeli
description: Azure Load Balancer güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/28/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 07dd369ceeefee85f4d9180a0b9b33a0d4c8bbdb
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94698215"
---
# <a name="azure-security-baseline-for-azure-load-balancer"></a>Azure Load Balancer için Azure Güvenlik temeli

Microsoft Azure Load Balancer için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir. Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](../security/benchmarks/overview.md)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar. Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](../security/benchmarks/security-baselines-overview.md).

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ağ güvenliği](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: sanal ağlar içindeki Azure kaynaklarını koruma

**Rehberlik**: yalnızca belirli sanal ağlardan veya eşlenmiş sanal ağlardan internet 'e maruz kalmadan arka uç kaynaklarına giden trafiğe izin vermek Için Iç Azure Yük dengeleyicileri kullanın. Doğrudan internet 'ten korunmak üzere arka uç kaynaklarının IP adreslerini geçici olarak almak için kaynak ağ adresi çevirisi (SNAT) ile bir dış Load Balancer uygulayın.

Azure, standart ve temel olmak üzere iki tür Load Balancer sunumu sunmaktadır. Tüm üretim iş yükleri için Standart Load Balancer kullanın. Ağ güvenlik gruplarını uygulayın ve yalnızca uygulamanızın güvenilen bağlantı noktalarına ve IP adresi aralıklarına erişime izin verin. Arka uç alt ağına veya arka uç sanal makinelerinin NIC 'sine atanmış bir ağ güvenlik grubu olmadığı durumlarda, trafiğe yük dengeleyiciden bu kaynaklarla izin verilmez. Standart yük dengeleyiciler ile, bir ağ güvenlik grubuyla giden NAT tanımlamak için giden kuralları sağlayın. Giden bağlantılarınızın davranışını ayarlamak için bu giden kuralları gözden geçirin. 

Üretim iş yükleriniz için Standart Load Balancer kullanılması önerilir ve genellikle temel Load Balancer yalnızca test için kullanılır çünkü temel tür varsayılan olarak internet 'ten gelen bağlantılara açıktır ve işlem için ağ güvenlik grupları gerektirmez. 

- [Azure’da giden bağlantılar](load-balancer-outbound-connections.md)

- [Azure genel Load Balancer yükseltme](./upgrade-basic-standard.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1,2: sanal ağların, alt ağların ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Rehberlik**: Load Balancer, arka uç kaynaklarına uygulanan ağ güvenlik grupları kurallarını ve internet erişimini denetlemek için yapılandırılmış giden kuralları temel aldığından bir geçiş hizmetidir.

Load Balancer giden kuralları dikey penceresinden Standart Load Balancer için yapılandırılan giden kuralları ve örtük giden kuralların etkin olduğu yük dengeleme kuralları dikey penceresini gözden geçirin.

Kaynaklarınızın internet 'e ne sıklıkta ulaşacağını izlemek için giden bağlantılarınızın sayısını izleyin. 

Güvenlik Merkezi 'ni kullanın ve Azure ağ kaynaklarınızın güvenliğini sağlamaya yardımcı olmak için ağ koruma önerilerini izleyin.

Arka uç kaynaklarınız için güvenlik önerilerini izleyin ve ağ güvenlik grubu akış günlüklerini etkinleştirin ve günlükleri denetim için bir Azure depolama hesabına gönderin.

Ayrıca, akış günlüklerini bir Log Analytics çalışma alanına gönderin ve ardından Azure bulutunuzda trafik desenlerine yönelik Öngörüler sağlamak için Trafik Analizi kullanın. Trafik Analizi avantajları arasında ağ etkinliğini görselleştirme, sık kullanılan noktaları ve güvenlik tehditlerini anlama, trafik akışı desenlerini anlama ve ağ yapılandırmalarını sağlama yeteneği dahildir.

- [Ağ güvenlik grubu akış günlüklerini etkinleştirme](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Trafik Analizi etkinleştirme ve kullanma](../network-watcher/traffic-analytics.md)

- [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](../security-center/security-center-network-recommendations.md)

- [Giden bağlantı istatistiklerimi denetlemek Nasıl yaparım?](./load-balancer-standard-diagnostics.md#how-do-i-check-my-outbound-connection-statistics)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: Web uygulamalarınızı korumak için Microsoft 'un tehdit bilgilerini kullanmak üzere Load Balancer ile giden kurallar ve ağ güvenlik grupları aracılığıyla internet bağlantısı ve geçerli kaynak IP 'leri açıkça tanımlayın.

- [Azure Güvenlik duvarını tümleştirme](../firewall/integrate-lb.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: DDoS saldırılarına karşı koruma sağlamak Için Azure sanal ağınızda Azure dağıtılmış hizmet reddi (DDoS) standart korumasını etkinleştirin. 

Tehdit zekası tabanlı filtreleme etkin ve kötü amaçlı ağ trafiği için "uyarı ve reddetme" olarak yapılandırılmış bir kuruluşun ağ sınırlarının her birinde Azure Güvenlik Duvarı 'Nı dağıtın.

 

Bilinen kötü amaçlı IP adresleriyle iletişimleri algılamak için Güvenlik Merkezi tehdit koruması 'nı kullanın. 

Standart Load Balancer, varsayılan olarak ve özel ve yalıtılmış bir sanal ağın bir parçası olarak güvenli olacak şekilde tasarlanmıştır. İzin verilen trafiğe açıkça izin vermek ve bilinen kötü amaçlı IP adreslerine izin vermemek için ağ güvenlik grupları tarafından açılmadığı müddetçe gelen akışlara kapalıdır. Sanal makine kaynağınızın bir alt ağda veya NIC 'inde bir ağ güvenlik grubu Load Balancer arkasında yoksa, trafiğin bu kaynağa erişmesine izin verilmez. 

Tehdit zekası tabanlı filtreleme etkin ve kötü amaçlı IP adreslerinden gelen saldırıları engellemek için kötü amaçlı ağ trafiği için "uyarı ve reddetme" olarak yapılandırılmış bir kuruluşun ağ sınırlarının her birinde Azure Güvenlik Duvarı 'Nı dağıtın. 

Load Balancer Azure Güvenlik duvarını tümleştirme kılavuzunu uygulayın.

Bilinen kötü amaçlı IP adresleriyle iletişimleri algılamak için Güvenlik Merkezi tehdit koruması özelliklerini kullanın. 

Güvenlik Merkezi (Standart katman) tam zamanında sanal makine erişimi sağlar ve izin verilen kaynak IP adreslerini yalnızca onaylanan IP adreslerinden/aralıklarından erişime izin verecek şekilde yapılandırır.
 

Bağlantı noktalarını ve kaynak IP 'Leri gerçek trafik ve tehdit zeka göre sınırlayan ağ güvenlik grubu yapılandırmalarının kullanılması için Güvenlik Merkezi 'nin Uyarlamalı ağ sağlamlaştırma özelliğini kullanın.
 

- [Azure portal kullanarak Azure DDoS koruması standardını yönetme](../ddos-protection/manage-ddos-protection.md)

- [Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme](../firewall/threat-intel.md)

- [Azure Güvenlik Merkezi’nde tehdit koruması](../security-center/azure-defender.md)

- [Tam zamanında erişim ile yönetim bağlantı noktalarınızın güvenliğini sağlama](../security-center/security-center-just-in-time.md)

- [Azure Güvenlik Merkezi 'nde Uyarlamalı ağ sağlamlaştırma](../security-center/security-center-adaptive-network-hardening.md)

- [Azure Güvenlik duvarını Load Balancer tümleştirme](../firewall/overview.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets"></a>1,5: ağ paketlerini kaydetme

**Rehberlik**: anormal etkinlikleri araştırmak Için Ağ İzleyicisi paket yakalamayı etkinleştirin.

- [Ağ Izleyicisi örneği oluşturma](../network-watcher/network-watcher-create.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Kılavuz**: Azure Marketi 'nden, yük İnceleme ÖZELLIKLERI olan kimlik/ıp 'leri Load Balancer ortamınıza yönelik bir teklif uygulayın. 

Yük incelemesi bir gereksinim değilse Azure Güvenlik Duvarı tehdit zekasını kullanın. Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme, bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen ve giden trafiği filtrelemek ve/veya engellemek için kullanılır. IP adresleri ve etki alanları, Microsoft Tehdit Analizi akışından alınır.

Kötü amaçlı trafiği algılamak ve/veya engellemek için kuruluşunuzun ağ sınırlarının her birinde tercih ettiğiniz güvenlik duvarı çözümünü dağıtın.

- [Azure Market](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

- [Azure Güvenlik duvarını dağıtma](../firewall/tutorial-firewall-deploy-portal.md)

- [Azure Güvenlik Duvarı ile uyarıları yapılandırma](../firewall/threat-intel.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: Web uygulamalarınızı korumak için Microsoft 'un tehdit bilgileri özelliklerini kullanmak üzere Load Balancer ile giden kurallar ve ağ güvenlik grupları aracılığıyla internet bağlantısı ve geçerli kaynak IP 'leri açıkça tanımlayın.

- [Azure Güvenlik duvarını tümleştirme](../firewall/integrate-lb.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: güvenlik kuralları oluştururken belirli IP adreslerinin yerine hizmet etiketleri kullanın. Karşılık gelen hizmet için trafiğe izin vermek veya reddetmek üzere bir kuralın kaynak veya hedef alanında hizmet etiketi adını belirtin. 

Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir. 

Varsayılan olarak, her ağ güvenlik grubu, durum araştırma trafiğine izin vermek için AzureLoadBalancer hizmet etiketini içerir. 

Ağ güvenlik grubu kurallarında kullanılabilecek tüm hizmet etiketleri için Azure belgelerine bakın.

- [Kullanılabilir hizmet etiketleri](../virtual-network/service-tags-overview.md#available-service-tags)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Kılavuz**: Azure ilkesiyle ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın.

Tek bir şema tanımında Azure Resources Manager şablonları, Azure RBAC denetimleri ve ilkeleri gibi temel ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanın. 

Şema ' i yeni aboneliklere uygulayın ve sürüm oluşturma aracılığıyla denetim ve yönetime ince ayar yapın.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Ağ için Azure Ilke örnekleri](../governance/policy/samples/built-in-policies.md#network)

- [Azure Blueprint oluşturma](../governance/blueprints/create-blueprint-portal.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: ağ güvenlik grupları ve ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için kaynak etiketleri kullanın. 

Tek tek ağ güvenlik grubu kuralları için bir ağa/ağdan giden trafiğe izin veren kuralları belgelemek için "Açıklama" alanını kullanın.

Etiketlemeyle ilgili yerleşik Azure Ilke tanımlarından herhangi birini ("etiket gerektir ve değerini ıste" gibi), tüm kaynakların etiketlerle oluşturulmasını ve var olan etiketlenmemiş kaynakları bilgilendirmesini sağlar.

Azure PowerShell veya Azure CLı kullanarak, etiketlerine göre kaynaklar üzerinde eylem arayabilir veya eylemleri gerçekleştirin.

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

- [Azure sanal ağı oluşturma](../virtual-network/quick-create-portal.md)

- [Ağ güvenlik grubu kuralları ile ağ trafiğini filtreleme](../virtual-network/tutorial-filter-network-traffic.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak kaynak yapılandırmalarının yanı sıra Azure kaynaklarınızda yapılan değişiklikleri tespit edin. 

Kritik kaynaklar değiştirildiğinde sizi bilgilendirmek için Azure Izleyici 'de uyarılar oluşturun.

- [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](../azure-monitor/platform/activity-log.md#view-the-activity-log)

- [Azure Izleyici 'de uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: günlüğe kaydetme ve izleme](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: aboneliklerinizde etkinlik günlüğünü görüntüleyerek yük dengeleyicileri için giden kurallarınıza ve ağ güvenlik gruplarına yapılan değişiklikleri gözden geçirin. 

Arka uç kaynaklarınızın güvende olduğundan emin olmak için iç konak günlüklerini görüntüleyin.

Bu günlükleri Log Analytics veya başka bir depolama platformuna dışarı aktarın. Azure Izleyici 'de analiz sorgulamak ve gerçekleştirmek için Log Analytics çalışma alanlarını kullanın ve uzun süreli ve arşiv depolama için Azure depolama hesaplarını kullanın.

Kurumsal iş gereksinimlerinize bağlı olarak bu verileri Azure Sentinel 'e veya üçüncü taraf bir SıEM 'e etkinleştirin ve yerleşik olarak yapın.

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md)

- [Azure Izleyici ile Azure sanal makine iç konak günlüklerini toplama](../azure-monitor/learn/quick-collect-azurevm.md)

- [Azure Izleyici ve üçüncü taraf SıEM tümleştirmesi ile çalışmaya başlama](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

- [Platform etkinlik günlükleri](../azure-monitor/platform/activity-log.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Kılavuz**: denetim ve yönetim düzlemi günlük kaydını ve temel Load Balancer etkinlik günlükleriyle yakalanan denetim bilgilerini gözden geçirin. Bu yakalama ayarları varsayılan olarak etkindir. 

Tüm etkinlikleri ve bunların durumunu görüntülemek için kaynakların eylemlerini izlemek üzere etkinlik günlüklerini kullanın. 

Aboneliğinizdeki kaynaklar üzerinde etkinlik günlükleriyle hangi işlemlerin alındığını belirleme: 

- işlemi kim başlattı
- işlem oluştuğunda
- işlemin durumu

- işlemi araştırmanıza yardımcı olabilecek diğer özelliklerin değerleri

Azure PowerShell, Azure komut satırı arabirimi (CLı), Azure REST API veya Azure portal aracılığıyla etkinlik günlüğünden bilgi alın. 

Azure Izleyici ile Standart Load Balancer yapılandırma özellikleri için çok boyutlu tanılama uygulayın.  Bunlar, kaynak ağ adresi çevirisi (SNAT) bağlantıları, bağlantı noktaları hakkındaki bilgileri içeren güvenlik için kullanılabilir ölçümleri içerir. SYN (Synchronize) paketleri ve paket sayaçlarındaki ek ölçümler de mevcuttur. 

Çok boyutlu ölçümleri API 'Ler aracılığıyla programlı bir şekilde alın ve ' tüm ölçümler ' seçeneği aracılığıyla bunları bir depolama hesabına yazın.

Verilerinizi önceden yapılandırılmış panolar ile analiz etmek veya gereksinimlerinizi temel alarak görünümleri özelleştirmek için Microsoft Power BI ile Azure denetim günlükleri içerik paketini kullanın.

Günlükleri bir olay hub 'ına veya Log Analytics çalışma alanına akış. Bunlar ayrıca Azure Blob depolamadan ayıklanırlar ve Excel ve Power BI gibi farklı araçlarla görüntülenebilir. 

İş gereksinimlerinize bağlı olarak, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'e verileri etkinleştirin.

- [Kaynak Yöneticisi ile denetim işlemlerinde ayrıntılı olarak açıklanan her yöntem için adım adım yönergeler ile bu makaleyi gözden geçirin.](../azure-resource-manager/management/view-activity-logs.md)

- [Genel Temel Yük Dengeleyici için Azure İzleyici günlükleri](./load-balancer-monitor-log.md)

- [Kaynaklardaki eylemleri izlemek için etkinlik günlüklerini görüntüleme](../azure-resource-manager/management/view-activity-logs.md)

- [API 'Ler aracılığıyla çok boyutlu ölçümleri program aracılığıyla alma](./load-balancer-standard-diagnostics.md#retrieve-multi-dimensional-metrics-programmatically-via-apis)

- [Azure Izleyici ve üçüncü taraf SıEM tümleştirmesi ile çalışmaya başlama](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Rehberlik**: etkinlik günlüğü varsayılan olarak etkindir ve Azure 'un olay günlükleri deposunda 90 gün boyunca korunur. Log Analytics çalışma alanı saklama dönemini, kuruluşunuzun Azure Izleyici 'deki uyumluluk düzenlemelerine göre ayarlayın. Uzun süreli ve arşiv depolama için Azure depolama hesaplarını kullanın.

- [Kaynaklardaki eylemleri izlemek için etkinlik günlüklerini görüntüleme makalesi](../azure-resource-manager/management/view-activity-logs.md)

- [Log Analytics veri saklama süresini değiştirme](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

- [Azure depolama hesabı günlükleri için bekletme ilkesini yapılandırma](../storage/common/storage-monitor-storage-account.md#configure-logging)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: Azure Portal ve Azure izleyici altındaki Kaynak Durumu sayfasındaki Load Balancer sayfasını kullanarak standart Load Balancer kaynakları izleyin, yönetin ve sorun giderin. Güvenlik için kullanılabilir ölçümler, kaynak ağ adresi çevirisi (SNAT) bağlantıları, bağlantı noktaları hakkında bilgi içerir. Ayrıca, SYN (Synchronize) paketleri ve paket sayaçlarındaki ölçümler de mevcuttur. 

Standart, dış ve dahili yük dengeleyiciler için çok boyutlu ölçümler ile Endpoint Health araştırma durumunu gözden geçirmek üzere Azure Izleyici 'yi kullanın. Bu ölçümleri API 'Ler aracılığıyla programlı bir şekilde toplayın ve ' tüm ölçümler ' seçeneği aracılığıyla bir depolama hesabına yazılır.

Azure Izleyici günlükleri, Iç temel yük dengeleyiciler için kullanılamaz. 

Arka uç havuzunuzdaki örnek sayısı, sistem durumu araştırma hatalarından dolayı Load Balancer istek almama gibi temel dış Load Balancer için arka uç havuzu başına özetlenen durum araştırma durumunu görmek üzere Azure Izleyici 'yi kullanın. 

Yük Dengeleyici Sistem durumu hakkında istatistikler sağlamak için Azure operasyonel içgörüler ile günlüğe kaydetme uygulayın. 

Etkinlik günlüklerini kullanarak uyarıları görüntüleyin ve kaynakları ve bunların durumlarını Azure aboneliklerinizde izler. Etkinlik günlükleri varsayılan olarak etkindir ve Azure portal görüntülenebilir. 

Azure denetim günlükleri içerik paketi ile Microsoft Power BI kullanın ve verilerinizi önceden yapılandırılmış panolar ile çözümleyin. Görünümleri iş gereksinimine göre gereksinimlerinize uyacak şekilde özelleştirin. 

Günlükleri bir olay hub 'ına veya Log Analytics çalışma alanına akış. Bunlar ayrıca Azure Blob depolamadan ayıklanırlar ve Excel ve Power BI gibi farklı araçlarla görüntülenebilir. Azure Sentinel 'e veya bir üçüncü taraf SıEM 'ye veri etkinleştirebilir ve bu verileri ayarlayabilirsiniz.

- [Load Balancer durum yoklamaları](./load-balancer-custom-probe-overview.md)

- [Azure İzleyici REST API'si](/rest/api/monitor)

- [REST API aracılığıyla ölçümleri alma](/rest/api/monitor/metrics/list)

- [Ölçümler, uyarılar ve kaynak sistem durumu ile tanılamayı Standart Load Balancer](./load-balancer-standard-diagnostics.md)

- [Genel Temel Yük Dengeleyici için Azure İzleyici günlükleri](./load-balancer-monitor-log.md)

- [Yük dengeleyici ölçülerinizi Azure portal görüntüleyin](./load-balancer-standard-diagnostics.md#view-your-load-balancer-metrics-in-the-azure-portal)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Rehberlik**: Güvenlik Merkezi 'ni, güvenlik günlükleri ve olaylarında Load Balancer ilgili anormal etkinlikleri izlemek ve uyarmak için Log Analytics çalışma alanıyla birlikte kullanın.

Azure Sentinel veya bir üçüncü taraf SıEM aracında verileri etkinleştirin ve yerleşik olarak etkinleştirin.

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

- [Azure Güvenlik Merkezi 'nde uyarıları yönetme](../security-center/security-center-managing-and-responding-alerts.md)

- [Log Analytics günlük verilerinde uyarı alma](../azure-monitor/learn/tutorial-response.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: Azure Load Balancer için geçerli değildir. Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: Azure Load Balancer, DNS sorguları olmayan bir çekirdek ağ hizmeti olduğundan uygulanamaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: Bu öneri işlem kaynakları için geçerli olduğundan Azure Load Balancer için geçerli değildir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kimlik ve erişim denetimi](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Kılavuz**: Azure rol tabanlı erişim denetimi (Azure RBAC), rol atamaları aracılığıyla Load Balancer gibi Azure kaynaklarına erişimi yönetmenizi sağlar. Bu rolleri kullanıcılara, gruplar hizmet sorumlularına ve yönetilen kimliklere atayın. 

Azure CLı, Azure PowerShell veya Azure portal gibi araçlarla belirli kaynaklar için önceden tanımlı ve yerleşik roller.

- [Azure AD 'de PowerShell ile dizin rolü alma](/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri koruma](../security/benchmarks/security-control-data-protection.md).*

### <a name="46-use-azure-rbac-to-manage-access-to-resources"></a>4,6: kaynaklara erişimi yönetmek için Azure RBAC kullanma

**Rehberlik**: Load Balancer kaynaklarınıza erişimi denetlemek IÇIN Azure RBAC kullanın.

- [Azure RBAC 'yi yapılandırma](../role-based-access-control/role-assignments-portal.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: Load Balancer, müşteri verilerini depolamamayan bir geçiş hizmetidir. Bu, Microsoft tarafından yönetilen temel platformun bir parçasıdır. 

Microsoft, tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. 

Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar. 

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Rehberlik**: önemli üretim iş yükleri Için kullanılan yük dengeleyiciler gibi kritik Azure kaynaklarında değişiklik yapıldığında uyarı oluşturmak Için Azure etkinlik günlüğü Ile Azure izleyici 'yi kullanın.

- [Azure etkinlik günlüğü olayları için uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: envanter ve varlık yönetimi](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

**Rehberlik**: aboneliklerinizde tüm kaynakları sorgulamak ve (işlem, depolama, ağ, bağlantı noktaları, protokoller vb.) sorgulamak Için Azure Kaynak grafiğini kullanın. Geçerli kaynakların oluşturulması ve kullanılması için Azure Resource Manager önerilir. 

Kiracınızda uygun (okuma) izinlere sahip olun ve aboneliklerinizdeki tüm Azure aboneliklerini ve kaynaklarını numaralandırın.

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

- [Azure aboneliklerinizi görüntüleme](/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [Azure RBAC 'yi anlama](../role-based-access-control/overview.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Rehberlik**: bir sınıflandırmaya göre mantıksal olarak düzenlemek için meta verilerle Azure kaynaklarına Etiketler uygulayın.

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: varlıkları düzenlemek ve izlemek için etiketleme, yönetim grupları ve uygun durumlarda ayrı abonelikler kullanın. 

Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliklerinizden zamanında silindiğinden emin olun.

- [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

- [Yönetim grupları oluşturma](../governance/management-groups/create-management-group-portal.md)

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynakları envanterini tanımlama ve sürdürme

**Rehberlik**: Kurumsal gereksinimlerinize göre onaylanan Azure kaynaklarının bir listesini oluşturun ve bu sayede izin verilenler listesi mekanizması olarak yararlanabilirsiniz. Bu, kuruluşunuzun normal güvenlik değerlendirme süreçlerine göre incelenip onaylandıktan sonra yeni kullanılabilir Azure hizmetlerini eklemesine olanak tanır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aboneliklerinizde oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın.

Sahip olunan aboneliklerdeki Azure Kaynak Grafını kullanarak kaynakları sorgulayın ve bulun. 

Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak IÇIN Azure AD koşullu erişimi kullanın.

- [Azure Kaynak Yöneticisi 'ne erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: iş işlemleri için gerekli olan, ancak kuruluşa daha fazla risk doğurabilecek yazılımlar, kendi sanal makinesi ve/veya sanal ağı içinde yalıtılmalı ve bir Azure Güvenlik duvarı ya da bir ağ güvenlik grubu ile yeterince güvenli hale gelmelidir.

- [Sanal ağ oluşturma](../virtual-network/quick-create-portal.md)

- [Güvenlik Yapılandırması ile ağ güvenlik grubu oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: güvenli yapılandırma](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: Azure kaynaklarınızın diğer adlarını denetlemek veya Azure kaynaklarınızın yapılandırmasını zorlamak için özel ilkeler oluşturmak için kullanın. Yerleşik Azure Ilke tanımlarını kullanın.

Azure Resource Manager, yapılandırmaların kuruluşunuzun güvenlik gereksinimlerini karşıladığından emin olmak için gözden geçirilmesi gereken JavaScript Nesne Gösterimi (JSON) içinde şablonu dışarı aktarma özelliğine sahiptir.

Azure Resource Manager şablonlarını JavaScript Nesne Gösterimi (JSON) biçimlerine dışarı aktarın ve yapılandırmaların kurumsal güvenlik gereksinimlerinizi karşıladığından emin olmak için düzenli aralıklarla gözden geçirin. 

Azure kaynaklarınız için güvenli bir yapılandırma temeli olarak güvenlik merkezi 'nden öneriler uygulayın. 

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure portal bir şablona tek ve çoklu kaynak verme](../azure-resource-manager/templates/export-template-portal.md)

- [Güvenlik önerileri - başvuru kılavuzu](../security-center/recommendations-reference.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.  Ayrıca, kuruluşunuz tarafından gerekli olan Azure kaynaklarınızın güvenlik yapılandırmasını sürdürmek için Azure Resource Manager şablonlarını kullanabilirsiniz. 

- [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md)

- [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Resource Manager şablonlarına genel bakış](../azure-resource-manager/templates/overview.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilke tanımları, Azure Resource Manager şablonları ve istenen durum yapılandırması betikleri gibi kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure DevOps kullanın. 

Belirli kullanıcılar, yerleşik güvenlik grupları veya Azure Active Directory (Azure AD) içinde tanımlanan gruplar, Azure DevOps ile tümleşikse veya TFS ile tümleşikse Active Directory izin verin veya reddedin.

- [Azure DevOps 'da kod depolama](/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure DevOps 'da izinler ve gruplar hakkında](/azure/devops/organizations/security/about-permissions)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Kılavuz**: Azure Ilkesini kullanarak Azure kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın.  Azure kaynaklarınızın ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak üzere Azure Ilke diğer adlarını kullanın. Belirli Azure Load Balancer kaynaklarınızla ilgili yerleşik ilke tanımlarını uygulayın.  Ayrıca, yapılandırma değişikliklerini dağıtmak için Azure Otomasyonu 'nu kullanın. Azure Load Balancer kaynaklarınıza.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Diğer adları kullanma](../governance/policy/concepts/definition-structure.md#aliases)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

**Rehberlik**: Azure kaynaklarınız ve Azure ilkenize yönelik temel taramaların kaynak yapılandırmalarına uyarı ve denetim için temel taramalar gerçekleştirmesini sağlamak Için Güvenlik Merkezi 'ni kullanın.

- [Azure Güvenlik Merkezi 'nde öneriler nasıl düzeltileceği](../security-center/security-center-remediate-recommendations.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: olay yanıtı](../security/benchmarks/security-control-incident-response.md).*

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. 

Önem derecesi, güvenlik merkezi 'nin uyarıyı vermek için kullanılan bulma veya analizte ne kadar önemli olduğunu ve uyarıya yönelik etkinliğin arkasında kötü amaçlı bir amaç olduğunu bilmenin yanı sıra güven düzeyini temel alır.

Abonelikleri kullanarak abonelikleri işaretleyin ve Azure kaynaklarını tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun.  

Olayın gerçekleştiği Azure kaynakları ve ortamının önem derecesine bağlı olarak, uyarıların düzeltilmesine öncelik vermek sizin sorumluluğunuzdadır.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../security-center/security-center-alerts-overview.md)

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Kılavuz**: Azure kaynaklarına riskleri belirlemenize yardımcı olmak için sürekli dışarı aktarma özelliğini kullanarak güvenlik merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. 

Güvenlik Merkezi 'nde, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmaya olanak sağlayan sürekli dışarı aktarma özelliğini kullanın. 

Uyarıları Azure Sentinel 'e akışa almak için Güvenlik Merkezi veri bağlayıcısını kullanın.

- [Sürekli dışarı aktarmayı yapılandırma](../security-center/continuous-export.md)

- [Uyarıları Azure Sentinel 'e akış](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Kılavuz**: Azure kaynaklarınızı korumaya yönelik güvenlik uyarılarına ve önerilerine yönelik yanıtları otomatik olarak tetiklemek Için Güvenlik Merkezi 'Ndeki Iş akışı Otomasyonu özelliğini kullanın.

- [Güvenlik ENTER 'ta iş akışı Otomasyonu nasıl yapılandırılır](../security-center/workflow-automation.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: Penetme testleri ve Red ekibi alıştırmaları](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft bulut Penme test kurallarını izleyin. Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı kırmızı ekip oluşturma ve canlı site sızma testi stratejisini kullanın. 

- [Katılım test kurallarını oyma](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Bulut Kırmızı ekip oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](../security/benchmarks/overview.md)
- [Azure güvenlik temelleri](../security/benchmarks/security-baselines-overview.md) hakkında daha fazla bilgi edinin