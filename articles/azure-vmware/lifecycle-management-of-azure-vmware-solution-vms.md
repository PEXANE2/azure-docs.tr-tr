---
title: Azure VMware Çözüm VM 'lerinin yaşam döngüsü yönetimi
description: Azure VMware Çözüm sanal makinelerinizin yaşam döngüsünün tüm yönlerini Microsoft Azure yerel araçlarla yönetmeyi öğrenin.
ms.topic: conceptual
ms.date: 09/11/2020
ms.openlocfilehash: 20948ec088d11468b5750ca89979050965246b58
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89664739"
---
# <a name="lifecycle-management-of-azure-vmware-solution-vms"></a>Azure VMware Çözüm VM 'lerinin yaşam döngüsü yönetimi

Microsoft Azure yerel araçlar, sanal makinelerinizi (VM) Azure ortamında izlemenize ve yönetmenize olanak tanır. Henüz Azure VMware çözümünde ve şirket içi sanal makinelerinizdeki VM 'lerinizi izlemenizi ve yönetmenizi de sağlar. Bu genel bakışta, Azure 'un sunduğu tümleşik izleme mimarisine ve kendi yaşam döngüsü boyunca Azure VMware Çözüm sanal makinelerinizi yönetmek için yerel araçlarını nasıl kullanabileceğinizi inceleyeceğiz.

## <a name="benefits"></a>Yararları

- Azure yerel hizmetleri, sanal makinelerinizi bir karma ortamda (Azure, Azure VMware çözümü ve şirket içi) yönetmek için kullanılabilir.
- Azure, Azure VMware çözümünüz ve şirket içi VM 'lerinizi tümleşik izleme ve görünürlük.
- Azure Otomasyonu 'nda Azure Güncelleştirme Yönetimi ile hem Windows hem de Linux makineleriniz için işletim sistemi güncelleştirmelerini yönetebilirsiniz. 
- Azure Güvenlik Merkezi, aşağıdakiler dahil olmak üzere Gelişmiş tehdit koruması sağlar:
    - Dosya bütünlüğünü izleme
    - Dosya daha az güvenlik uyarısı
    - İşletim sistemi düzeltme eki değerlendirmesi
    - Güvenlik yapılandırması hataları değerlendirmesi
    - Endpoint Protection değerlendirmesi 
- Yeni VM 'Ler için Azure yay kullanarak Microsoft Monitoring Agent (MMA) öğesini kolayca dağıtın. 
- Azure Izleyici 'de Log Analytics çalışma alanınız, MMA veya uzantıları kullanarak günlük toplama ve performans sayacı toplamayı mümkün bir şekilde sunar. Verileri ve günlükleri tek bir noktaya toplayın ve bu verileri farklı Azure yerel hizmetlerine sunun. 
- Azure Izleyici 'nin ek avantajları şunlardır: 
    - Sorunsuz izleme 
    - Daha iyi altyapı görünürlüğü 
    - Anında bildirimler 
    - Otomatik çözüm 
    - Maliyet verimliliği 

## <a name="integrated-azure-monitoring-architecture"></a>Tümleşik Azure izleme mimarisi

Aşağıdaki diyagramda Azure VMware Çözüm VM 'Leri için tümleşik izleme mimarisi gösterilmektedir.

![Tümleşik Azure izleme mimarisi](media/lifecycle-mgmt-avs-vms/integrated-azure-monitoring-architecture.png)

## <a name="integrating-and-deploying-azure-native-services"></a>Azure yerel hizmetlerini tümleştirme ve dağıtma

Azure **Arc** , Azure Yönetim 'ı Azure VMware çözümü, şirket içi veya diğer bulut platformları dahil olmak üzere herhangi bir altyapıya genişletir. Azure ARC, Azure VMware Çözüm ortamına bir Azure Kubernetes hizmeti (AKS) kümesi yüklenerek dağıtılabilir. Daha fazla bilgi için bkz. [Azure Arc etkin bir Kubernetes kümesi bağlama](../azure-arc/kubernetes/connect-cluster.md).

Azure VMware Çözüm VM 'Leri MMA aracılığıyla izlenebilir (Log Analytics Aracısı veya OMS Linux Aracısı olarak da adlandırılır). Sanal makineler ARC VM yaşam döngüsü iş akışlarıyla sağlandığında, MMA otomatik olarak yüklenebilir. VM, vCenter 'daki bir şablondan dağıtıldığında de yüklenebilir; , yay iş akışlarıyla sağlanan VM 'lerle birlikte. Tüm sağlanan Azure VMware Çözüm VM 'Leri daha sonra MMA 'yı yükleyebilir ve günlükleri Azure Log Analytics çalışma alanına gönderebilir. Daha fazla bilgi için bkz. [Log Analytics aracısına genel bakış](../azure-monitor/platform/log-analytics-agent.md).

**Log Analytics çalışma alanı** , MMA veya uzantıları kullanarak günlük toplama ve performans sayacı toplamayı mümkün. Log Analytics çalışma alanınızı oluşturmak için, bkz. [Azure portal Log Analytics çalışma alanı oluşturma](../azure-monitor/learn/quick-create-workspace.md).
- Log Analytics çalışma alanınıza Windows VM 'Leri eklemek için bkz. [Windows bilgisayarlarına Log Analytics Aracısı 'Nı yüklemek](../azure-monitor/platform/agent-windows.md).
- Log Analytics çalışma alanınıza Linux VM 'Leri eklemek için bkz. [Linux bilgisayarlarına ınstall Log Analytics Agent](../azure-monitor/platform/agent-linux.md).

Azure Otomasyonu 'ndaki **azure güncelleştirme yönetimi** , karma bir ortamda Windows ve Linux makineleriniz için işletim sistemi güncelleştirmelerini yönetir. Düzeltme Eki uyumluluklarını izler ve düzeltme için düzeltme eki uygulama Izleme için Azure Izleyici 'ye iletir. Azure Güncelleştirme Yönetimi, sanal makinelerinizdeki güncelleştirmelerin durumunu değerlendirmek üzere depolanan verileri kullanmak için Log Analytics çalışma alanınıza bağlanmalıdır.
- Log Analytics Azure Güncelleştirme Yönetimi eklemek için önce [bir Azure Otomasyonu hesabı oluşturmanız](../automation/automation-create-standalone-account.md)gerekir.
- Log Analytics çalışma alanınızı Otomasyon hesabınızla bağlamak için, bkz. [Log Analytics çalışma alanı ve Otomasyon hesabı](../azure-monitor/insights/solutions.md#log-analytics-workspace-and-automation-account).
- Sanal makinelerinize yönelik Azure Güncelleştirme Yönetimi etkinleştirmek için bkz. [Otomasyon hesabından güncelleştirme yönetimi etkinleştirme](../automation/update-management/update-mgmt-enable-automation-account.md).
- Azure Güncelleştirme Yönetimi VM 'Leri ekledikten sonra, [güncelleştirmeleri sanal makinelere dağıtabilir ve sonuçları gözden](../automation/update-management/update-mgmt-deploy-updates.md)geçirebilirsiniz. 

**Azure Güvenlik Merkezi** , bulutta ve şirket içinde hibrit iş yükleriniz genelinde gelişmiş tehdit koruması sağlar. Bu işlem, Azure VMware Çözüm VM 'lerinin güvenlik açığını değerlendirir ve gerektiğinde uyarı yükseltir. Bu güvenlik uyarıları, çözüm için Azure Izleyici 'ye iletilebilir.
- Azure Güvenlik Merkezi dağıtım gerektirmez. Daha fazla bilgi için bkz. [sanal makineler Için desteklenen özelliklerin](../security-center/security-center-services.md)listesi.
- Azure VMware Çözüm VM 'lerini ve Azure olmayan VM 'Leri Azure Güvenlik Merkezi 'ne eklemek için bkz. [Windows bilgisayarlarını Azure Güvenlik Merkezi 'ne](../security-center/quick-onboard-windows-computer.md) ekleme ve [Linux bilgisayarlarını Azure Güvenlik Merkezi](../security-center/quick-onboard-linux-computer.md)'ne ekleme.
- VM 'Ler eklendikten sonra Azure Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için kaynakların güvenlik durumunu analiz eder. Ayrıca Genel Bakış sekmesinde öneriler de sağlar. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'Nde güvenlik önerileri](../security-center/security-center-recommendations.md).
- Azure Güvenlik Merkezi 'nde güvenlik ilkeleri tanımlayabilirsiniz. Güvenlik ilkelerinizi yapılandırma hakkında daha fazla bilgi için bkz. [güvenlik Ilkeleriyle çalışma](../security-center/tutorial-security-policy.md).

**Azure izleyici** , bulut ve şirket içi ortamlarınızdaki telemetri toplama, çözümleme ve üzerinde işlem yapmaya yönelik kapsamlı bir çözümdür. Dağıtım gerektirmez.
- Azure Izleyici, izlemek ve analiz etmek için farklı kaynaklardan veri toplamanıza olanak tanır. Daha fazla bilgi için bkz. [Azure izleyici için izleme verileri kaynakları](../azure-monitor/platform/data-sources.md).
- Analiz, görselleştirme ve uyarı için farklı veri türleri de toplayabilirsiniz. Daha fazla bilgi için bkz. [Azure izleyici veri platformu](../azure-monitor/platform/data-platform.md).
- Azure Izleyicisini Log Analytics çalışma alanınıza göre yapılandırmak için, bkz. [VM'ler için Azure izleyici için Log Analytics çalışma alanını yapılandırma](../azure-monitor/insights/vminsights-configure-workspace.md).
- Ortamınızdaki sorunları, kaynakların yüksek kullanımı, eksik düzeltme ekleri, yetersiz disk alanı ve sanal makinelerinizin sinyali gibi konularda belirlemek için uyarı kuralları oluşturabilirsiniz. Ayrıca, bir uyarı hizmet yönetimi (ıTSM) araçlarına bir uyarı göndererek algılanan olaylara otomatik bir yanıt da ayarlayabilirsiniz. Uyarı algılama bildirimi, e-posta yoluyla da gönderilebilir. Bu tür kuralları oluşturmak için, bkz.:
    - [Azure izleyici 'yi kullanarak ölçüm uyarıları oluşturun, görüntüleyin ve yönetin](../azure-monitor/platform/alerts-metric.md).
    - [Azure izleyici 'yi kullanarak günlük uyarıları oluşturun, görüntüleyin ve yönetin](../azure-monitor/platform/alerts-log.md).
    - Otomatik eylemleri ve bildirimleri ayarlamak için [Eylem kuralları](../azure-monitor/platform/alerts-action-rules.md) .
    - [Azure 'ı BT hizmet yönetimi Bağlayıcısı kullanarak ıtssm araçlarına bağlayın](../azure-monitor/platform/itsmc-overview.md).

**Hizmet olarak Azure platformu (PaaS)** , bulut tabanlı uygulamalar sunmanıza olanak sağlayan, bulutta bir geliştirme ve dağıtım ortamıdır. Örneğin, Azure SQL veritabanı 'nı Azure VMware Çözüm VM 'leriniz ile tümleştirebilirsiniz. SQL uyarıları daha sonra Azure VMware Çözüm VM uyarıları ile bağıntılı olabilir. Örneğin, uygulamanızın SQL veritabanı BAI 'nin Azure PAAS dahilinde olduğunu ve aynı uygulamanın Web uygulaması katmanının Azure VMware Çözüm sanal makinelerinizde barındırıldığını varsayalım. Daha sonra veritabanı uyarıları, Web uygulaması uyarıları ile bağıntılı olabilir. Azure, Azure VMware çözümü ve şirket içi VM 'lerin tek, tümleşik bir görünürlüğünde sorun giderme basitleştirilmiştir.
