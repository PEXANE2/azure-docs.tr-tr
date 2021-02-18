---
title: Azure VMware Çözüm VM 'lerinin yaşam döngüsü yönetimi
description: Azure VMware Çözüm sanal makinelerinizin yaşam döngüsünün tüm yönlerini Microsoft Azure yerel araçlarla yönetmeyi öğrenin.
ms.topic: conceptual
ms.date: 02/08/2021
ms.openlocfilehash: 2cb9964b68769b1e784cebf62b4d336b355c68fb
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100572210"
---
# <a name="lifecycle-management-of-azure-vmware-solution-vms"></a>Azure VMware Çözüm VM 'lerinin yaşam döngüsü yönetimi

Microsoft Azure yerel araçlar, sanal makinelerinizi (VM) Azure ortamında izlemenize ve yönetmenize olanak tanır. Henüz Azure VMware çözümünde ve şirket içi sanal makinelerinizdeki VM 'lerinizi izlemenizi ve yönetmenizi de sağlar. Bu genel bakışta, Azure 'un sunduğu tümleşik izleme mimarisine ve kendi yaşam döngüsü boyunca Azure VMware Çözüm sanal makinelerinizi yönetmek için yerel araçlarını nasıl kullanabileceğinizi inceleyeceğiz.

## <a name="benefits"></a>Avantajlar

- Azure yerel hizmetleri, sanal makinelerinizi bir karma ortamda (Azure, Azure VMware çözümü ve şirket içi) yönetmek için kullanılabilir.
- Azure, Azure VMware çözümünüz ve şirket içi VM 'lerinizi tümleşik izleme ve görünürlük.
- Azure Otomasyonu 'nda Azure Güncelleştirme Yönetimi ile hem Windows hem de Linux makineleriniz için işletim sistemi güncelleştirmelerini yönetebilirsiniz. 
- Azure Güvenlik Merkezi, aşağıdakiler dahil olmak üzere Gelişmiş tehdit koruması sağlar:
    - Dosya bütünlüğünü izleme
    - Dosya daha az güvenlik uyarısı
    - İşletim sistemi düzeltme eki değerlendirmesi
    - Güvenlik yapılandırması hataları değerlendirmesi
    - Endpoint Protection değerlendirmesi 
- Yeni ve mevcut VM 'Ler için Azure Arc etkin sunucuları VM uzantısı desteğini kullanarak Log Analytics aracısını kolayca dağıtın. 
- Azure Izleyici 'de Log Analytics çalışma alanınız, Log Analytics Aracısı veya uzantıları kullanılarak günlük toplama ve performans sayacı toplamayı mümkün bir şekilde sunar. Verileri ve günlükleri tek bir noktaya toplayın ve bu verileri farklı Azure yerel hizmetlerine sunun. 
- Azure Izleyici 'nin ek avantajları şunlardır: 
    - Sorunsuz izleme 
    - Daha iyi altyapı görünürlüğü 
    - Anında bildirimler 
    - Otomatik çözüm 
    - Maliyet verimliliği 

## <a name="integrated-azure-monitoring-architecture"></a>Tümleşik Azure izleme mimarisi

Aşağıdaki diyagramda Azure VMware Çözüm VM 'Leri için tümleşik izleme mimarisi gösterilmektedir.

![Tümleşik Azure izleme mimarisi](media/lifecycle-management-azure-vmware-solutions-virtual-machines/integrated-azure-monitoring-architecture.png)

## <a name="before-you-start"></a>Başlamadan önce

Azure 'u yeni kullanmaya veya daha önce bahsedilen hizmetlerden herhangi birini bilmiyorsanız aşağıdaki makalelere göz atın:

- [Otomasyon hesabı kimlik doğrulamasına genel bakış](../automation/automation-security-overview.md)
- [Azure Izleyici günlükleri dağıtımını](../azure-monitor/logs/design-logs-deployment.md) ve [Azure izleyicisini](../azure-monitor/overview.md) tasarlama
- Azure Güvenlik Merkezi için [planlama](../security-center/security-center-planning-and-operations-guide.md) ve [Desteklenen platformlar](../security-center/security-center-os-coverage.md)
- [VM'ler için Azure İzleyici genel bakış 'ı etkinleştir](../azure-monitor/vm/vminsights-enable-overview.md)
- [Azure Arc etkin sunucuları nedir?](../azure-arc/servers/overview.md) [Azure Arc etkin Kubernetes nedir?](../azure-arc/kubernetes/overview.md)
- [Güncelleştirme Yönetimine genel bakış](../automation/update-management/overview.md)

## <a name="integrating-and-deploying-azure-native-services"></a>Azure yerel hizmetlerini tümleştirme ve dağıtma

### <a name="enable-azure-update-management"></a>Azure Güncelleştirme Yönetimi etkinleştirme

Azure Otomasyonu 'ndaki Azure Güncelleştirme Yönetimi, karma bir ortamda Windows ve Linux makineleriniz için işletim sistemi güncelleştirmelerini yönetir. Düzeltme Eki uyumluluklarını izler ve düzeltme için düzeltme eki uygulama Izleme için Azure Izleyici 'ye iletir. Azure Güncelleştirme Yönetimi, sanal makinelerinizdeki güncelleştirmelerin durumunu değerlendirmek üzere depolanan verileri kullanmak için Log Analytics çalışma alanınıza bağlanmalıdır.

1.  Log Analytics Azure Güncelleştirme Yönetimi ekleyebilmeniz için önce [bir Azure Otomasyonu hesabı oluşturmanız](../automation/automation-create-standalone-account.md)gerekir. Hesabınızı bir şablon kullanarak oluşturmayı tercih ediyorsanız, bkz. [Azure Resource Manager şablonu kullanarak Otomasyon hesabı oluşturma](../automation/quickstart-create-automation-account-template.md).

2. **Log Analytics çalışma alanı** , Log Analytics Aracısı veya uzantıları kullanılarak günlük toplama ve performans sayacı toplamayı mümkün. Log Analytics çalışma alanınızı oluşturmak için, bkz. [Azure portal Log Analytics çalışma alanı oluşturma](../azure-monitor/logs/quick-create-workspace.md). İsterseniz, [CLI](../azure-monitor/logs/quick-create-workspace-cli.md), [PowerShell](../azure-monitor/logs/powershell-workspace-configuration.md)veya [Azure Resource Manager şablonu](../azure-monitor/logs/resource-manager-workspace.md)aracılığıyla da bir çalışma alanı oluşturabilirsiniz.

3. Sanal makinelerinize yönelik Azure Güncelleştirme Yönetimi etkinleştirmek için bkz. [Otomasyon hesabından güncelleştirme yönetimi etkinleştirme](../automation/update-management/enable-from-automation-account.md). İşlemde, Log Analytics çalışma alanınızı Otomasyon hesabınızla bağlayacaksınız. 
 
4. Azure Güncelleştirme Yönetimi VM 'Leri ekledikten sonra, [güncelleştirmeleri sanal makinelere dağıtabilir ve sonuçları gözden](../automation/update-management/deploy-updates.md)geçirebilirsiniz. 

### <a name="enable-azure-security-center"></a>Azure Güvenlik Merkezi 'Ni etkinleştir

Azure Güvenlik Merkezi, bulutta ve şirket içinde hibrit iş yükleriniz genelinde gelişmiş tehdit koruması sağlar. Bu işlem, Azure VMware Çözüm VM 'lerinin güvenlik açığını değerlendirir ve gerektiğinde uyarı yükseltir. Bu güvenlik uyarıları, çözüm için Azure Izleyici 'ye iletilebilir.

Azure Güvenlik Merkezi dağıtım gerektirmez. Daha fazla bilgi için bkz. [sanal makineler Için desteklenen özelliklerin](../security-center/security-center-services.md)listesi.

1. Azure VMware Çözüm VM 'lerini ve Azure dışı VM 'Leri Güvenlik Merkezi 'ne eklemek için bkz. [hızlı başlangıç: Azure Güvenlik Merkezi 'Ni ayarlama](../security-center/security-center-get-started.md). 

2. Azure olmayan bir ortamdan Azure VMware Çözüm VM 'lerini veya VM 'Leri ekledikten sonra, güvenlik merkezi 'nde Azure Defender 'ı etkinleştirin. Güvenlik Merkezi, olası güvenlik sorunları için VM 'Leri değerlendirecek. Ayrıca Genel Bakış sekmesinde öneriler de sağlar. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'Nde güvenlik önerileri](../security-center/security-center-recommendations.md).

3. Azure Güvenlik Merkezi 'nde güvenlik ilkeleri tanımlayabilirsiniz. Güvenlik ilkelerinizi yapılandırma hakkında daha fazla bilgi için bkz. [güvenlik Ilkeleriyle çalışma](../security-center/tutorial-security-policy.md).

### <a name="onboard-vms-to-azure-arc-enabled-servers"></a>Azure Arc etkin sunucularına VM ekleme

Azure Arc, Azure Yönetim 'i Azure VMware çözümü, şirket içi veya diğer bulut platformları dahil olmak üzere herhangi bir altyapıya genişletir.

- Birden çok Windows veya Linux VM için Azure Arc etkin sunucularını etkinleştirme hakkında daha fazla bilgi için bkz. [ölçekli karma makineleri Azure 'A bağlama](../azure-arc/servers/onboard-service-principal.md).

### <a name="onboard-hybrid-kubernetes-clusters-with-arc-enabled-kubernetes"></a>Arc etkin Kubernetes ile karma Kubernetes kümeleri ekleme

Azure Arc etkin Kubernetes kullanarak Azure VMware Çözüm ortamınızda barındırılan bir Kubernetes kümesi ekleyebilirsiniz. 

- Daha fazla bilgi için bkz. [Azure yay özellikli bir ekleme hizmeti sorumlusu oluşturma](../azure-arc/kubernetes/create-onboarding-service-principal.md).

### <a name="deploy-the-log-analytics-agent"></a>Log Analytics aracısını dağıtma

Azure VMware Çözüm VM 'Leri, Log Analytics aracısında (Microsoft Monitoring Agent (MMA) veya OMS Linux Aracısı olarak da adlandırılır) izlenebilir. Azure Otomasyonu Güncelleştirme Yönetimi etkinleştirilirken zaten bir Log Analytics çalışma alanı oluşturdunuz.

- Log Analytics aracısını [Azure Arc etkin sunucular VM uzantısı desteğini](../azure-arc/servers/manage-vm-extensions.md)kullanarak dağıtın.

### <a name="enable-azure-monitor"></a>Azure Izleyicisini etkinleştirme

Azure Izleyici, bulut ve şirket içi ortamlarınızdaki telemetri toplama, çözümleme ve üzerinde işlem yapmaya yönelik kapsamlı bir çözümdür. Dağıtım gerektirmez. Azure Izleyici ile, Konuk işletim sistemi performansını izleyebilir ve Azure VMware çözümü veya şirket içi VM 'Ler için uygulama bağımlılıklarını bulabilir ve eşleyebilirsiniz.

- Azure Izleyici, izlemek ve analiz etmek için farklı kaynaklardan veri toplamanıza olanak tanır. Daha fazla bilgi için bkz. [Azure izleyici için izleme verileri kaynakları](../azure-monitor/agents/data-sources.md).

- Analiz, görselleştirme ve uyarı için farklı veri türleri toplayın. Daha fazla bilgi için bkz. [Azure izleyici veri platformu](../azure-monitor/data-platform.md).

- Azure Izleyicisini Log Analytics çalışma alanınıza göre yapılandırmak için, bkz. [VM'ler için Azure izleyici için Log Analytics çalışma alanını yapılandırma](../azure-monitor/vm/vminsights-configure-workspace.md).

- Ortamınızdaki sorunları, kaynakların yüksek kullanımı, eksik düzeltme ekleri, yetersiz disk alanı ve sanal makinelerinizin sinyali gibi konularda belirlemek için uyarı kuralları oluşturabilirsiniz. Ayrıca, bir uyarı hizmet yönetimi (ıTSM) araçlarına bir uyarı göndererek algılanan olaylara otomatik bir yanıt da ayarlayabilirsiniz. Uyarı algılama bildirimi, e-posta yoluyla da gönderilebilir. Bu tür kuralları oluşturmak için, bkz.:
    - [Azure izleyici 'yi kullanarak ölçüm uyarıları oluşturun, görüntüleyin ve yönetin](../azure-monitor/alerts/alerts-metric.md).
    - [Azure izleyici 'yi kullanarak günlük uyarıları oluşturun, görüntüleyin ve yönetin](../azure-monitor/alerts/alerts-log.md).
    - Otomatik eylemleri ve bildirimleri ayarlamak için [Eylem kuralları](../azure-monitor/alerts/alerts-action-rules.md) .
    - [Azure 'ı BT hizmet yönetimi Bağlayıcısı kullanarak ıtssm araçlarına bağlayın](../azure-monitor/alerts/itsmc-overview.md).
