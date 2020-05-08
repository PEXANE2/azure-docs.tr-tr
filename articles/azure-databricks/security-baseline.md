---
title: Azure Databricks için Azure Güvenlik temeli
description: Azure Databricks için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 7d417f412592d87dec751b9d98a26bf786c719e5
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82796856"
---
# <a name="azure-security-baseline-for-azure-databricks"></a>Azure Databricks için Azure Güvenlik temeli

Azure Databricks için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Rehberlik**: kendi Azure sanal ağınızda (VNet) Azure Databricks dağıtın. Varsayılan Azure Databricks dağıtımı, Azure üzerinde tam olarak yönetilen bir hizmettir: tüm kümelerin ilişkilendirilebildiği bir sanal ağ dahil tüm veri düzlemi kaynakları, kilitli bir kaynak grubuna dağıtılır. Ancak ağ özelleştirmeye ihtiyacınız varsa, kendi sanal ağınızda (VNet ekleme) Azure Databricks veri düzlemi kaynaklarını dağıtabilir ve özel ağ yapılandırması uygulamanızı sağlayabilirsiniz. Belirli çıkış trafiği kısıtlamalarına özel kurallarla kendi ağ güvenlik grubunuzu (NSG) uygulayabilirsiniz.

Ayrıca, NSG kurallarını, Azure Databricks örneğinizin dağıtıldığı alt ağda çıkış trafiği kısıtlamalarını belirtecek şekilde yapılandırabilirsiniz. Azure Güvenlik Duvarı, VNET 'e eklenen çalışma alanları için yapılandırılabilir.

* [Azure Databricks kendi sanal ağınıza nasıl dağıtılır](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [NSG 'leri yönetme](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Rehberlik**: kendi Azure sanal ağınızda (VNet) Azure Databricks dağıtın. Bir ağ güvenlik grubu (NSG) otomatik olarak oluşturulmaz. Yalnızca varsayılan Azure kuralları ile bir temel NSG oluşturmanız gerekir. Bir çalışma alanını dağıtırken, Databricks için gereken kurallar eklenir. Ayrıca boş bir NSG ile başlayabilir ve uygun kurallar otomatik olarak eklenir. Trafik denetimleri için NSG akış günlüklerini etkinleştirin ve günlükleri bir depolama hesabına gönderin. Ayrıca, akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

* [Azure Databricks kendi sanal ağınıza nasıl dağıtılır](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [NSG akış günlüklerini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Trafik Analizi etkinleştirme ve kullanma](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

* [Ağ İzleyicisini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: dağıtılmış hizmet reddi saldırılarına karşı koruma için Azure Databricks örneklarınızla Ilişkili Azure sanal ağlarında Azure DDoS koruma standardı 'nı etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan genel IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

* [Azure portal kullanarak Azure DDoS koruması standardını yönetme](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

* [Azure Güvenlik Merkezi 'nde Azure ağ katmanı için tehdit korumasını anlama](https://docs.microsoft.com/azure/security-center/threat-protection#threat-protection-for-azure-network-layer-)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Rehberlik**: kendi Azure sanal ağınızda (VNet) Azure Databricks dağıtın. Bir ağ güvenlik grubu (NSG) otomatik olarak oluşturulmaz. Yalnızca varsayılan Azure kuralları ile bir temel NSG oluşturmanız gerekir. Bir çalışma alanını dağıtırken, Databricks için gereken kurallar eklenir. Trafik denetimi için NSG akış günlüklerini etkinleştirin ve günlükleri bir depolama hesabına gönderin. Ayrıca, akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

* [Azure Databricks kendi sanal ağınıza nasıl dağıtılır](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [NSG akış günlüklerini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Trafik Analizi etkinleştirme ve kullanma](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

* [Ağ İzleyicisini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Kılavuz**: Azure Marketi 'nden bir IDP/IP özellikli ağ sanal gereci (NVA), tüm Azure Databricks kümelerinin tek bir IP giden adresine sahip olduğu sanal ağ ile tümleşik bir çalışma alanı oluşturmak için uygulayın. Tek IP adresi, belirli IP adreslerine göre erişime izin veren diğer Azure hizmetleri ve uygulamaları ile ek bir güvenlik katmanı olarak kullanılabilir. Giriş ve çıkış trafiğini yönetmek için bir Azure Güvenlik Duvarı veya NVA gibi diğer 3. taraf araçlarını kullanabilirsiniz.

Yük incelemesini temel alan yetkisiz giriş algılama ve/veya önleme bir gereksinim değilse, tehdit zekasıyla Azure Güvenlik Duvarı kullanılabilir. Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme, bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen ve giden trafiği uyarabilir ve reddedebilir. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından kaynaklıdır.

* [Azure Güvenlik Duvarı Kullanarak Sanal Ağa Eklenmiş Çalışma Alanları için Tek Bir Genel IP Atama](https://docs.microsoft.com/azure/databricks/kb/cloud/azure-vnet-single-ip)

* [NVA oluşturma](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-portal)

* [Azure Market](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: Azure Databricks örneğinizin Ilişkilendirildiği alt ağlara bağlı ağ güvenlik grupları üzerinde ağ erişim denetimleri tanımlamak Için hizmet etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı (örn., Apimanaya) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir. Hizmet etiketleri, eklenen VNET çalışma alanlarıyla desteklenmez.

* [Hizmet etiketlerini anlama](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Kılavuz**: Azure ilkesiyle Azure Databricks örneklerinizin ağ güvenlik yapılandırmasını tanımlayın ve uygulayın. Özel ilke tanımlarını tanımlamak için, "Microsoft. Databricks" ad alanındaki Azure Ilke diğer adlarını kullanabilirsiniz. İlkeler, yönetilen kaynak grubu içindeki kaynaklar için uygulanmaz.

Ayrıca, Azure Kaynak Yönetimi şablonları, rol tabanlı erişim denetimi (RBAC) ve ilkeler gibi tek bir şema tanımında temel ortam yapıtları sunarak büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanabilirsiniz. Şema 'i yeni abonelikler ve ortamlara kolayca uygulayın ve sürüm oluşturma aracılığıyla denetimi ve yönetimi ayrıntılı olarak ayarlayın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Ilkesi diğer adlarını ve tanım yapısını anlama](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure)

* [Azure Blueprint oluşturma](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: NSG 'ler ve Azure Databricks örneğiniz ile ilişkili ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketler kullanın. Bireysel NSG kuralları için "Açıklama" alanını kullanarak ağa giden/giden trafiğe izin veren kuralların iş gereksinimini ve/veya süresini (vs.) belirtin.

* [Etiketler oluşturma ve kullanma](httphttps://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını izleyin ve Azure Databricks örneklerinizin ilişkili olduğu ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

* [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Azure Izleyici 'de uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, Azure kaynakları için zaman kaynaklarını korur, ancak işlem kaynaklarınızın zaman eşitleme ayarlarını yönetme seçeneğiniz vardır. Azure Databricks bir PaaS hizmeti olduğundan, bir Azure Databricks kümesindeki VM 'lere doğrudan erişiminiz yoktur ve zaman eşitleme ayarlarını ayarlayamazsınız.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: Azure Databricks tarafından oluşturulan güvenlik verilerini toplamak Için Azure izleyici aracılığıyla günlükleri alma. Azure Izleyici 'de, Databricks ve tanılama günlüklerinizi alacak şekilde yapılandırılan Log Analytics çalışma alanını sorgulayabilirsiniz. Diğer sistemlere veri aktarmak için uzun süreli/arşiv günlüğü depolaması veya Olay Hub 'ları için Azure depolama hesaplarını kullanın. Alternatif olarak, Azure Sentinel veya bir üçüncü taraf güvenlik olayı ve olay yönetimi (SıEM) üzerinde verileri etkinleştirebilir ve bu verilere izin verebilir.

Note: Azure Databricks tanılama günlükleri Azure Databricks Premium planı gerektirir

* [Tanılama ayarlarını yapılandırma](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings#create-diagnostic-settings-in-azure-portal)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Azure Izleyici ve üçüncü taraf SıEM tümleştirmesi ile çalışmaya başlama](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Kılavuz**: Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Azure etkinlik günlüğü verilerini kullanarak, Azure kaynaklarınızın denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, SILME) için "ne, kim ve ne zaman" seçeneğini belirleyebilirsiniz.

Azure Databricks denetim günlüğü için, Azure Databricks kullanıcılar tarafından gerçekleştirilen, kuruluşunuzun ayrıntılı Azure Databricks kullanım düzenlerini izlemesine izin veren kapsamlı uçtan uca tanılama günlükleri sağlar.

Note: Azure Databricks tanılama günlükleri Azure Databricks Premium planı gerektirir

* [Azure etkinlik günlüğü Için tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy))

* [Azure Databricks için tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: Azure Databricks Azure Databricks kullanıcılar tarafından gerçekleştirilen ve kuruluşunuzun ayrıntılı Azure Databricks kullanım düzenlerini izlemesine olanak tanıyan kapsamlı bir uçtan uca tanılama günlükleri sağlar.

Note: Azure Databricks tanılama günlükleri Azure Databricks Premium planı gerektirir. İşletim sistemi güvenlik günlüğü kullanılamıyor.

* [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Azure Databricks için tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Rehberlik**: Azure Databricks için tanılama ayarlarını etkinleştirin. Günlükleri bir Log Analytics çalışma alanında depolamayı seçerseniz, kuruluşunuzun uyumluluk düzenlemelerine göre Log Analytics çalışma alanı saklama süresini ayarlayın. Uzun süreli/arşiv depolama için Azure depolama hesaplarını kullanın. Güvenlikle ilgili etkinlikler Databricks denetim günlüklerinde izlenir.

Note: Azure Databricks tanılama günlükleri Azure Databricks Premium planı gerektirir

* [Azure Databricks tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

* [Log Analytics çalışma alanları için günlük saklama parametrelerini ayarlama](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: Azure Databricks için tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Log Analytics çalışma alanını kullanarak anormal davranış için Azure Databricks günlüklerinizi çözümleyin ve izleyin ve sonuçları düzenli olarak gözden geçirin.

Alternatif olarak, Azure Sentinel 'e veya üçüncü taraf SıEM 'ye yönelik verileri etkinleştirebilir.

Note: Azure Databricks tanılama günlükleri Azure Databricks Premium planı gerektirir

* [Azure Databricks tanılama ayarlarını etkinleştirme](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs)

* [Log Analytics çalışma alanına gönderilen Azure Databricks günlüklerini sorgulama](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs#analyze-diagnostic-logs)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Rehberlik**: Azure Databricks örneğiniz için tanılama ayarlarını yapılandırın ve günlükleri Log Analytics çalışma alanına gönderin. Log Analytics çalışma alanınızda, önceden tanımlanmış bir koşullar kümesi gerçekleşirken yapılacak uyarıları yapılandırın.

Alternatif olarak, Azure Sentinel 'e veya üçüncü taraf SıEM 'ye yönelik verileri etkinleştirebilir.

Note: Azure Databricks tanılama günlükleri Azure Databricks Premium planı gerektirir

* [Log Analytics çalışma alanına Azure Databricks günlükleri gönderme](https://docs.microsoft.com/azure/databricks/administration-guide/account-settings/azure-diagnostic-logs#configure-diagnostic-log-delivery))

* [Log Analytics çalışma alanında uyarıları yapılandırma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Azure Databricks, Kullanıcı tarafından erişilebilen DNS ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve erişim denetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: bir Azure Databricks çalışma alanındaki kullanıcıları yönetmek ve belirlenen kullanıcılara yönetim ayrıcalıkları vermek için Azure Databricks SCIM API 'lerini kullanabilirsiniz. Bunları Azure Databricks Kullanıcı arabirimi aracılığıyla da yönetebilirsiniz.

* [SCıM API 'Lerini kullanma](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [Azure Databricks Kullanıcı ekleme ve kaldırma](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/users)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Rehberlik**: Azure Databricks, Azure portal ve Azure Databricks yönetim konsoluna erişim sağlamak için Azure ACTIVE DIRECTORY (ad) kullanır. Azure AD varsayılan parola kavramına sahip değildir, ancak tüm özel veya üçüncü taraf uygulamalar için varsayılan parolalara izin vermekten veya buna izin verilmez.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: yönetici ayrıcalıklarına sahip kullanıcıları bir Azure Databricks eklemek için Azure Databricks SCIM API 'lerini kullanabilirsiniz. Bunları Azure Databricks Kullanıcı arabirimi aracılığıyla da yönetebilirsiniz.

* [SCıM API 'Lerini kullanma](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [Azure Databricks Kullanıcı ekleme ve kaldırma](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/users)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: Azure Databricks, kullanıcıların kimliğini doğrulamak için çoklu oturum açma Azure Active Directory kullanacak şekilde otomatik olarak ayarlanır. Kuruluşunuzun dışındaki kullanıcılar, davet işlemini tamamlamalı ve çoklu oturum açma yoluyla Azure Databricks oturum açabilmek için Active Directory kiracınıza eklenmelidir. Kullanıcıların çalışma alanlarından sağlama ve sağlama işlemlerini otomatik hale getirmek için SCıM 'i uygulayabilirsiniz.

* [SCıM API 'Lerini kullanma](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [Azure Databricks için çoklu oturum açmayı anlayın](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/single-sign-on/)

* [Azure AD kiracınıza Kullanıcı davet etme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-external-users)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın.

**Kılavuz**: Azure AD MFA 'yı etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

* [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure kaynaklarını açmak ve YAPıLANDıRMAK için MFA Ile Paws (ayrıcalıklı erişim iş istasyonları) kullanın.

* [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda günlüklerin ve uyarıların üretilmesi için Azure Active Directory güvenlik raporlarını kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi 'ni kullanın. Ayrıca, Azure Databricks tanı günlüklerinden yararlanabilirsiniz.

* [Riskli etkinlik için işaretlenen Azure AD kullanıcılarını belirleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk)

* [Azure Güvenlik Merkezi 'nde kullanıcıların kimlik ve erişim etkinliğini izleme](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek Için adlandırılmış konumlar kullanın.

* [Azure 'da adlandırılmış konumları yapılandırma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: Azure Databricks, kullanıcıların kimliğini doğrulamak için çoklu oturum açma Azure Active Directory kullanacak şekilde otomatik olarak ayarlanır. Kuruluşunuzun dışındaki kullanıcılar, davet işlemini tamamlamalı ve çoklu oturum açma yoluyla Azure Databricks oturum açabilmek için Active Directory kiracınıza eklenmelidir.

* [Azure Databricks için çoklu oturum açmayı anlayın](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/single-sign-on/)

* [Azure AD kiracınıza Kullanıcı davet etme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-external-users)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Kılavuz**: Azure AD, eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi incelemelerini kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir. Ayrıca, Kullanıcı erişimini gözden geçirmek için SCıM API 'Leri ve tanılama günlükleri Azure Databricks de uygulayabilirsiniz. Ayrıca, Kullanıcı erişimini gözden geçirmek için SCıM API 'Lerini ve tanılama günlüklerini Azure Databricks de kullanabilirsiniz.

Ayrıca, Azure Databricks Yönetici Konsolu 'nda Kullanıcı erişimini düzenli olarak gözden geçirin ve yönetin.

* [Azure AD raporlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

* [Azure kimlik erişimi incelemelerini kullanma](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

* [Azure Databricks Yönetici Konsolu 'nda Kullanıcı erişimini yönetme](https://docs.microsoft.com/azure/databricks/administration-guide/users-groups/users)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Kılavuz**: Azure AD oturum açma etkinliğine, denetim ve risk olay günlüğü kaynaklarına erişerek herhangi bir SIEM/izleme aracıyla tümleştirmenize imkan tanır. Ayrıca, Kullanıcı erişimi etkinliğini gözden geçirmek için Azure Databricks tanılama günlüklerini kullanabilirsiniz.

Azure Active Directory Kullanıcı hesapları için Tanılama ayarları oluşturarak ve denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. İstenen uyarıları Log Analytics çalışma alanı içinde yapılandırabilirsiniz.

* [SCıM API 'Lerini kullanma](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/scim/)

* [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: Kullanıcı kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırmak IÇIN Azure AD risk ve kimlik koruması özelliklerini kullanın. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz. Ayrıca, Kullanıcı erişimi etkinliğini gözden geçirmek için Azure Databricks tanılama günlüklerini kullanabilirsiniz.

* [Azure AD riskli oturum açma işlemlerini görüntüleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: destek biletleri açık olduğunda, müşteri hizmetleri ve destek mühendisleri ilgili müşteri verilerine erişmeyi ister.

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Şu anda kullanılamıyor

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri işleyen Azure Databricks örneklerini izlemeye yardımcı olması için Etiketler kullanın.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Azure Databricks varsayılan dağıtımı, kendi sanal ağı içinde dağıtılan tam olarak yönetilen bir hizmettir. Ağ özelleştirmeye ihtiyacınız varsa, Azure Databricks kendi sanal ağınıza dağıtabilirsiniz. Farklı iş ekiplerine veya departmanlara yönelik ayrı Azure Databricks çalışma alanları oluşturmak en iyi uygulamadır.

* [Azure Databricks kendi sanal ağınıza nasıl dağıtılır](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)

* [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleyin ve engelleyin.

**Rehberlik**: veri kaybı olasılığını azaltmak Için Databricks koruma mimarisini izleyin.

* [Azure Databricks ile veri kaybı koruması](https://databricks.com/blog/2020/03/27/data-exfiltration-protection-with-azure-databricks.html)

Microsoft, Azure Databricks için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Şu anda kullanılamıyor

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: Microsoft, Azure portal veya Azure Databricks konsolu aracılığıyla Azure Databricks örneğinizi yönetirken varsayılan olarak TLS 1,2 ' i anlaşacaktır. Databricks Web uygulaması TLS 1,3 ' i destekler.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: Şu anda kullanılamıyor; veri tanımlama, sınıflandırma ve kayıp önleme özellikleri şu anda Azure Databricks için kullanılamaz. Bu gibi hassas bilgileri işleyen ve uyumluluk amaçları için gerekliyse üçüncü taraf çözümü uygulayan, Azure Databricks örnekleri ve ilgili kaynakları etiketleyin.

Databricks platformu yalnızca işlem amaçlıdır ve tüm veriler diğer Azure veri hizmetlerinde depolanır. Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Şu anda kullanılamıyor

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Kılavuz**: Azure Databricks ' de, veri tablolarına, kümelerine, havuzlara, işlere ve Not defterleri, denemeleri ve klasörler gibi çalışma alanı nesnelerine erişim iznini yapılandırmak için erişim denetim listelerini (ACL 'ler) kullanabilirsiniz. Tüm yönetici kullanıcılar erişim denetim listelerini yönetebilir, böylece erişim denetim listelerini yönetmek için temsilci izinleri verilmiş olan kullanıcılar. Azure Databricks çalışma alanında izinleri ayarlamak için Azure RBAC kullanabilirsiniz.

Note: tablo, küme, havuz, iş ve çalışma alanı erişim denetimi yalnızca Azure Databricks Premium planında kullanılabilir

* [Azure Databricks 'de erişim denetimini yönetme](https://docs.microsoft.com/azure/databricks/administration-guide/access-control/)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

Microsoft, Azure Databricks için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

* [Azure 'da müşteri veri korumasını anlama](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: bir Azure Databricks çalışma alanı, Azure Databricks yönetilen bir sanal ağda barındırılan bir yönetim düzlemine ve müşterinin yönettiği bir sanal ağda dağıtılan bir veri düzlemine sahiptir. Denetim düzlemi tüm kullanıcıların not defterlerini ve ilişkili Not defteri sonuçlarını bir veritabanında depolar. Varsayılan olarak, tüm not defterleri ve sonuçlar, farklı bir şifreleme anahtarıyla Rest 'de şifrelenir.

Databricks dosya sistemi (DBFS), bir Azure Databricks çalışma alanına bağlanmış ve Azure Databricks kümelerinde kullanılabilen bir dağıtılmış dosya sistemidir. DBFS, Azure Databricks çalışma alanınızın yönetilen kaynak grubunda bir depolama hesabı olarak uygulanır. Varsayılan olarak, depolama hesabı Microsoft tarafından yönetilen anahtarlarla şifrelenir. Verileriniz, sahip olduğunuz Azure veri hizmetlerinde depolanır ve bunu şifrelemek için seçeneğiniz vardır. Üretim verilerini depolamak için DBFS kullanımı önerilmez

Not: Bu özellikler tüm Azure Databricks abonelikleri için kullanılamaz. Erişim istemek için Microsoft veya Databricks hesap temsilcinizle iletişime geçin.

* [Azure Databricks Not defterleri için müşteri tarafından yönetilen anahtarları etkinleştirme](https://docs.microsoft.com/azure/databricks/security/customer-managed-key-notebook)

* [Azure Databricks dosya sistemi için müşteri tarafından yönetilen anahtarları etkinleştirme](https://docs.microsoft.com/azure/databricks/security/customer-managed-keys-dbfs)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Izleyici 'Yi Azure etkinlik günlüğü ile kullanarak, kritik Azure Databricks çalışma alanlarında değişiklik gerçekleşirken uyarılar oluşturun.

* [Azure etkinlik günlüğü olayları için uyarı oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: üçüncü taraf bir güvenlik açığı yönetim çözümü uygulayın.

Bir güvenlik açığı yönetimi platformu aboneliğiniz varsa, güvenlik açığı değerlendirme aracılarını Azure Databricks kümesi düğümlerine yüklemek ve ilgili portaldan düğümleri yönetmek için Azure Databricks başlatma betikleri kullanabilirsiniz. Her üçüncü taraf çözümünün farklı şekilde çalıştığın unutmayın.

* [Rapid7 aracısını el Ile yüklemek](https://insightagent.help.rapid7.com/docs/install)

* [Qualys aracısını el Ile nasıl yüklenir](https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf)

* [Azure Databricks başlatma betikleri](https://docs.microsoft.com/azure/databricks/clusters/init-scripts)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: Microsoft Azure Databricks küme düğümü temel görüntülerini korur, ancak Küme düğümlerinizin düzeltme eki uygulanmış olduğundan emin olmak sizin sorumluluğunuzdadır. Var olan çalışan bir kümeye bakım güncelleştirmesi eklemek için kümeyi yeniden başlatmanız gerekir.

* [Azure Databricks için çalışma zamanı bakım güncelleştirmelerini anlayın](https://docs.microsoft.com/azure/databricks/release-notes/runtime/maintenance-updates)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: Microsoft, Azure Databricks küme düğümü temel görüntülerini korur, ancak yüklediğiniz tüm üçüncü taraf uygulamaların düzeltme eki uygulanmış olduğundan emin olmak sizin sorumluluğunuzdadır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: zaman içinde güvenlik açığı taramalarının karşılaştırılabilmesi için bir üçüncü taraf güvenlik açığı yönetimi çözümü uygulayın. Bir güvenlik açığı yönetimi aboneliğiniz varsa, geri dönüş güvenlik açıklarını görüntülemek ve karşılaştırmak için bu satıcının portalını kullanabilirsiniz. Her üçüncü taraf çözümünün farklı şekilde çalıştığın unutmayın.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için bir risk derecelendirme işlemi kullanın.

**Rehberlik**: ortak bir risk Puanlama programı (örneğin, ortak güvenlik açığı Puanlama sistemi) veya üçüncü taraf tarama aracınız tarafından sunulan varsayılan risk derecelendirmelerini kullanın.

**Azure Güvenlik Merkezi izleme**: yok

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: abonelikleriniz dahilinde (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) tüm kaynakları sorgulamak/öğrenmek Için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinlerinin bulunduğundan emin olun ve tüm Azure aboneliklerinin yanı sıra aboneliklerinizde bulunan kaynakları numaralandırın.

Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

* [Azure Kaynak Graf ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

* [Azure aboneliklerinizi görüntüleme](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Azure RBAC 'yi anlama](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure kaynaklarına Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler verirsiniz.

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure kaynaklarını düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

Ayrıca, aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri abonelikleri içinde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkesi 'ni kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

* [Ek Azure abonelikleri oluşturma](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Yönetim Grupları oluşturma](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Etiketler oluşturma ve kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-maintain-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini saklayın.

**Rehberlik**: işlem kaynakları Için onaylanan Azure kaynakları ve onaylanan yazılımlar tanımlayın.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

Azure Kaynak Grafiği 'ni kullanarak aboneliklerinde kaynakları sorgulama/bulma. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Graph ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Azure Databricks bir PaaS hizmetidir ve müşterilerin bir Azure Databricks kümesindeki VM 'lere doğrudan erişimi yoktur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: abonelikleriniz dahilinde Azure Databricks örnekleri de dahil olmak üzere tüm kaynakları (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) sorgulamak/öğrenmek Için Azure Kaynak grafiğini kullanın. Keşfettiğiniz onaylanmamış tüm Azure kaynaklarını kaldırın. Azure Databricks küme düğümleri için, onaylanmamış yazılımları kaldırmak veya uyarmak üzere bir üçüncü taraf çözümü uygulayın.

* [Azure Graph ile sorgu oluşturma](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz; Bir PaaS hizmeti olan Azure Databricks, bir Azure Databricks kümesindeki VM 'lere doğrudan erişiminiz yoktur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:
- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Ilkesiyle belirli bir kaynak türünü reddetme](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: uygulanamaz; Bir PaaS hizmeti olan Azure Databricks, bir Azure Databricks kümesindeki VM 'lere doğrudan erişiminiz yoktur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="611-divlimit-users-ability-to-interact-with-azure-resource-manager-via-scriptsdiv"></a>6,11: <div>Kullanıcıların betikler aracılığıyla Azure Resource Manager etkileşim kurma yeteneğini sınırlayın</div>

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak Için Azure koşullu erişimi kullanın.

* [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz; Bu, kümenin kullanıcıları (yönetici olmayanlar) işleri çalıştırmak için ayrı düğümlere erişmesi gerekmediğinden Azure Databricks için geçerli değildir. Küme yöneticisinin varsayılan olarak tüm küme düğümlerine kök erişimi vardır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: uygulanamaz; Kıyaslama, Web uygulamalarını barındıran Azure Apps hizmeti veya bilgi işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Kılavuz**: Azure ilkesiyle Azure Databricks örneklerinizin standart güvenlik yapılandırmasını tanımlayın ve uygulayın. Azure Databricks örneklerinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Databricks" ad alanındaki Azure Ilke diğer adlarını kullanın. Uygulanan tüm ilkelerin Databricks yönetilen kaynak grubu üzerinde çalışmadığına unutmayın.

* [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bir PaaS hizmeti olan Azure Databricks, bir Azure Databricks kümesindeki VM 'lere doğrudan erişiminiz yoktur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure ilkesiyle Azure Databricks örneklerinizin standart güvenlik yapılandırmasını tanımlayın ve uygulayın. Azure Databricks örneklerinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Databricks" ad alanındaki Azure Ilke diğer adlarını kullanın. Azure kaynaklarınızın tamamında güvenli ayarları zorlamak için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Ilke efektlerini anlama](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: Microsoft tarafından yönetilen ve korunan Işletim sistemi görüntülerini Azure Databricks. İşletim sistemi düzeyinde durum yapılandırması uygulamaktan siz sorumlusunuz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure devops veya Azure Repos kullanın.

* [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Azure Repos belgeleri](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: Azure Databricks kümesi düğümleriniz için özel görüntüler kullanıyorsanız, özel taban görüntünüzü Azure Container Registry (ACR) gibi bir Docker kayıt defterine gönderin.

* [Databricks kapsayıcı hizmetleriyle kapsayıcıları özelleştirme](https://docs.microsoft.com/azure/databricks/clusters/custom-containers)

* [Azure Container Registry anlayın](https://docs.microsoft.com/azure/container-registry/container-registry-intro)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Kılavuz**: Azure ilkesiyle Azure Databricks örneklerinizin standart güvenlik yapılandırmasını tanımlayın ve uygulayın. Azure Databricks örneklerinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Databricks" ad alanındaki Azure Ilke diğer adlarını kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bir PaaS hizmeti olan Azure Databricks, bir Azure Databricks kümesindeki VM 'lere doğrudan erişiminiz yoktur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma Izlemeyi uygulayın

**Kılavuz**: Azure ilkesiyle Azure Databricks örneklerinizin standart güvenlik yapılandırmasını tanımlayın ve uygulayın. Azure Databricks örneklerinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. Databricks" ad alanındaki Azure Ilke diğer adlarını kullanın.

* [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Azure Ilkesini yapılandırma ve yönetme](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: Azure Databricks küme düğümü işletim sistemlerinin durumunu izlemek için bir üçüncü taraf çözümü uygulayın.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: gizli dizileri güvenli bir şekilde yönetmek ve kullanmak için Azure Databricks gizli kapsamlı Azure Key Vault kullanın.

* [Azure Databricks ile Azure Key Vault kullanma](https://docs.microsoft.com/azure/azure-databricks/store-secrets-azure-key-vault)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: Şu anda kullanılamıyor; Yönetilen kimlikler Şu anda Azure Databricks için kullanılamıyor

* [Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Şu anda kullanılamıyor

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

* [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü amaçlı yazılımdan koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: bir güvenlik açığı yönetimi platformu aboneliğiniz varsa, güvenlik açığı değerlendirme aracılarını Azure Databricks kümesi düğümlerine yüklemek ve ilgili portaldan düğümleri yönetmek için Azure Databricks başlatma betikleri kullanabilirsiniz. Her üçüncü taraf çözümünün farklı şekilde çalıştığın unutmayın.

* [Rapid7 aracısını el Ile yüklemek](https://insightagent.help.rapid7.com/docs/install)

* [Qualys aracısını el Ile nasıl yüklenir](https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf)

* [Azure Databricks başlatma betikleri](https://docs.microsoft.com/azure/databricks/clusters/init-scripts)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel alınan konakta (örneğin, Azure App Service) etkinleştirilir, ancak içeriğiniz üzerinde çalışmaz.

Azure Databricks kümesi düğümleriniz veya ilgili kaynaklarınız için karşıya yüklenen tüm dosyaları önceden tarayın.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: bir güvenlik açığı yönetimi platformu aboneliğiniz varsa, güvenlik açığı değerlendirme aracılarını Azure Databricks kümesi düğümlerine yüklemek ve ilgili portaldan düğümleri yönetmek için Azure Databricks başlatma betikleri kullanabilirsiniz. Her üçüncü taraf çözümünün farklı şekilde çalıştığın unutmayın.

* [Rapid7 aracısını el Ile yüklemek](https://insightagent.help.rapid7.com/docs/install)

* [Qualys aracısını el Ile nasıl yüklenir](https://www.qualys.com/docs/qualys-cloud-agent-linux-install-guide.pdf)

* [Azure Databricks başlatma betikleri](https://docs.microsoft.com/azure/databricks/clusters/init-scripts)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: Azure Databricks veri kaynaklarınız için, kullanım durumu için uygun bir veri artıklığı yapılandırdığınızdan emin olun. Örneğin, Azure Databricks veri depolduğunuz için bir Azure depolama hesabı kullanıyorsanız uygun artıklık seçeneğini (LRS, ZRS, GRS, RA-GRS) seçin.

* [Azure Databricks için veri kaynakları](https://docs.microsoft.com/azure/azure-databricks/databricks-connect-to-data-sources#data-sources-for-azure-databricks)

* [Azure Databricks kümeleri için bölgesel olağanüstü durum kurtarma](https://docs.microsoft.com/azure/azure-databricks/howto-regional-disaster-recovery)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: Azure Key Vault içindeki Azure Databricks uygulamalarınız ile ilgili müşteri tarafından yönetilen tüm anahtarları yedekleyin. Databricks yapılandırmalarının günlük yedeklemesini oluşturmak için REST API ve CLı de kullanabilirsiniz. Databricks yapılandırmalarının günlük yedeklemesini oluşturmak için REST API/CLı de kullanabilirsiniz.

* [Azure 'da Anahtar Kasası anahtarlarını yedekleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: Azure Databricks uygulamalarınız ile ilgili, yedeklenen müşteri tarafından yönetilen anahtarların test geri yüklenmesi.

* [Azure 'da Anahtar Kasası anahtarlarını geri yükleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: tuşları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Key Vault ' de geçici silme özelliğinin etkinleştirildiğinden emin olun.

* [Key Vault 'da geçici silme özelliğini etkinleştirme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

* [Azure Güvenlik Merkezi 'nde Iş akışı otomasyonlarını yapılandırma](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)

* [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Ayrıca, NıST 'nin bilgisayar güvenliği olay Işleme kılavuzumuzu, kendi olay yanıt planının oluşturulmasına yardımcı olması için de kullanabilirsiniz](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

* [NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), müşteri verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

* [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Uyarılar Sentinel 'i akışa almak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.

* [Sürekli dışarı aktarmayı yapılandırma](https://docs.microsoft.com/azure/security-center/continuous-export)

* [Uyarıları Azure Sentinel 'e akış](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: güvenlik uyarılarında ve önerilerinde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın.

* [Iş akışı otomasyonu ve Logic Apps yapılandırma](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını 60 gün içinde düzeltmeye dikkat edin.

**Rehberlik**: * [Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma testini yürütme hakkında daha fazla bilgi edinebilirsiniz.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](https://docs.microsoft.com/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
