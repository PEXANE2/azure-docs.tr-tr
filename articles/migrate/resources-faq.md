---
title: Azure geçişi-sık sorulan sorular (SSS) | Microsoft Docs
description: Azure geçişi hakkında sık sorulan soruları adresleyen
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: snehaa
ms.openlocfilehash: bf591306220b2c8c1e6c8166686836d96432fc7d
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68856271"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure geçişi-sık sorulan sorular (SSS)

Bu makalede, Azure geçişi hakkında sık sorulan sorular yer almaktadır. Bu makaleyi okuduktan sonra başka sorgular varsa, bunları [Azure geçişi forumuna](https://aka.ms/AzureMigrateForum)gönderin.

## <a name="general"></a>Genel

### <a name="which-azure-geographies-are-supported-by-azure-migrate"></a>Azure geçişi tarafından hangi Azure coğrafi ve desteklenen?

VMware için listeyi [burada](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) ve [Hyper-V için burada](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects)bulabilirsiniz.

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Azure Azure Site Recovery 'den farklı bir şekilde geçiş?

Azure geçişi, Azure 'da makineler ve iş yüklerini başlatmak, çalıştırmak ve izlemek için merkezi bir merkez sağlar. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) bir olağanüstü durum kurtarma çözümüdür. Azure geçişi sunucu geçişi, şirket içi makinelerin geçiş ve çapraz geçişi için geçiş senaryolarını etkinleştirmek üzere arka uçta Azure Site Recovery yararlanır.

## <a name="azure-migrate-appliance-vmwarephysical-servers"></a>Azure geçişi gereci (VMware/fiziksel sunucular)

### <a name="how-does-the-azure-migrate-appliance-connect-to-azure"></a>Azure geçişi gereci Azure 'a nasıl bağlanır?

Bağlantı Internet üzerinden olabilir veya ExpressRoute 'u ortak/Microsoft eşlemesi ile birlikte kullanabilirsiniz.

### <a name="what-network-connectivity-requirements-are-needed-for-azure-migrate-server-assessment-and-migration"></a>Azure geçişi sunucu değerlendirmesi ve geçişi için gereken ağ bağlantısı gereksinimleri

Azure geçişi 'nin Azure ile iletişim kurması için gereken URL 'Ler ve bağlantı noktaları için, [VMware](migrate-support-matrix-vmware.md) ve [Hyper-V](migrate-support-matrix-hyper-v.md) destek matrislerini gözden geçirin.

### <a name="can-i-harden-the-appliance-vm-i-set-up-with-the-template"></a>Şablonla ayarladığım gereç sanal makinesini kullanabilir miyim?

Azure geçişi gereci için gereken iletişim ve güvenlik duvarı kuralları olduğu sürece şablona ek bileşenler (örneğin, virüsten koruma) eklenebilir.   

### <a name="what-data-is-collected-by-azure-migrate-appliance"></a>Azure geçişi gereci hangi verileri toplamıştır?

Azure geçiş gereci tarafından toplanan veriler hakkında ayrıntılı bilgi edinebilirsiniz. [](https://docs.microsoft.com/azure/migrate/migrate-appliance#collected-performance-data-vmware)

### <a name="is-there-any-performance-impact-on-the-analyzed-vmware-or-hyper-v-environment"></a>Çözümlenen VMware veya Hyper-V ortamında herhangi bir performans etkisi var mı?

Performans verilerinin sürekli profilini oluşturmak için Azure geçişi, sanal makine performans verilerini ölçmek üzere şirket içi makineler sağlar. Bu, Hyper-V/ESXi konaklarının yanı sıra vCenter Server de neredeyse sıfır performans etkisine sahiptir.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Toplanan veriler ve ne kadar süreyle depolanıyor?

Azure geçişi gereci tarafından toplanan veriler, geçiş projesi oluştururken belirttiğiniz Azure konumunda depolanır. Veriler bir Microsoft aboneliğine güvenli bir şekilde depolanır ve Azure geçişi projesini sildiğinizde silinir.

Bağımlılık görselleştirmesi için, VM 'Lere aracılar yüklerseniz, bağımlılık aracıları tarafından toplanan veriler ABD 'de, Azure aboneliğinde oluşturulan bir Log Analytics çalışma alanında depolanır. Bu veriler, aboneliğinizdeki Log Analytics çalışma alanını sildiğinizde silinir. [Daha fazla bilgi edinin](concepts-dependency-visualization.md).

### <a name="what-is-the-volume-of-data-uploaded-by-azure-migrate-appliance-during-continuous-profiling"></a>Sürekli profil oluşturma sırasında Azure geçiş gereci tarafından karşıya yüklenen verilerin hacmi nedir?

Azure geçişi 'ne gönderilen verilerin hacmi, birkaç parametreye göre farklılık gösterir. 10 makineyle (her biri bir disk ve bir NIC ile) bir Azure geçişi projesi olan, bir dizi numaraya sahip olmak için günde 50 MB arasında bir değer gönderir. Bu, NIC 'Lerin ve disklerin veri noktası sayısına göre değişiklik gösteren yaklaşık bir değerdir (makine, NIC 'ler veya disk sayısı artıdıkça gönderilen veriler doğrusal değildir).

### <a name="is-the-data-encrypted-at-rest-and-in-transit"></a>Veriler REST ve aktarım sırasında mi şifrelendi?

Her ikisi için Evet. Meta veriler, https üzerinden Azure geçişi hizmetine internet üzerinden güvenli bir şekilde gönderilir. Meta veriler bir Microsoft aboneliğinde [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest)ve [Azure Blob depolama](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) alanında depolanır ve REST 'te şifrelenir.

Bağımlılık aracıları tarafından toplanan veriler de aktarım (güvenli HTTPS) ile şifrelenir ve Kullanıcı aboneliğindeki bir Log Analytics çalışma alanında depolanır. Ayrıca, Rest 'ten de şifrelenir.

### <a name="how-does-the-azure-migrate-appliance-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Azure geçişi gereci vCenter Server ve Azure geçişi hizmeti ile nasıl iletişim kurar?

Gereç, gereci ayarlarken belirtilen kimlik bilgilerini kullanarak vCenter Server (bağlantı noktası 443) bağlanır. VCenter Server tarafından yönetilen VM 'Lerle ilgili meta verileri toplamak için VMware PowerCLI kullanarak vCenter Server sorgular. VM 'Ler (çekirdekler, bellek, diskler, NIC vs.) ile ilgili yapılandırma verilerini ve geçen ay için her VM 'nin performans geçmişini toplar. Toplanan meta veriler daha sonra değerlendirme için Azure geçişi sunucu değerlendirmesine (HTTPS üzerinden Internet üzerinden) gönderilir.

### <a name="can-i-connect-the-same-appliance-to-multiple-vcenter-servers"></a>Aynı gereci birden çok vCenter sunucusuna bağlanabilir miyim?

Hayır. Bir gereç ve vCenter Server arasında bire bir eşleme vardır. Birden çok vCenter Server üzerinde VM 'Leri bulmanız gerekiyorsa, birden çok gereç dağıtmanız gerekir. 


### <a name="i-changed-my-machine-size-can-i-rerun-an-assessment"></a>Makine boyutumu değiştirdim. Bir değerlendirmeyi yeniden çalıştırabilir miyim?

Azure geçişi gereci, şirket içi ortam hakkındaki bilgileri sürekli olarak toplar. Bununla birlikte, değerlendirme, şirket içi VM 'lerin bir zaman noktası anlık görüntüsüdür. Değerlendirmek istediğiniz bir VM 'deki ayarları değiştirirseniz, en son değişikliklerle değerlendirmesi güncelleştirmek için ' yeniden hesapla ' seçeneğini kullanın.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate-server-assessment"></a>Azure geçişi sunucu değerlendirmesi 'nde çok kiracılı bir ortamı nasıl keşfedebilirim?

VMware için, kiracılar genelinde paylaşılan bir ortamınız varsa ve başka bir kiracının aboneliğindeki bir kiracının VM 'lerini öğrenmek istemiyorsanız, yalnızca öğrenmek istediğiniz VM 'Ler için erişim ile vCenter Server kimlik bilgileri oluşturun. Ardından, Azure geçişi gerecinde keşfi devre dışı bırakıldığında kimlik bilgilerini kullanın.

Hyper-V için bulma, Hyper-v ana bilgisayar kimlik bilgilerini kullanır, VM 'Ler aynı Hyper-V konağını paylaşıyorsa, şu anda bulmayı ayırmanın bir yolu yoktur.  

### <a name="how-many-vms-can-be-discovered-using-a-single-migration-appliance"></a>Tek bir geçiş gereci kullanılarak kaç VM bulunabilir?

En fazla 10.000 VMware VM ve tek bir geçiş gereci kullanarak en çok 5.000 Hyper-V VM 'Leri bulabilirsiniz.  Şirket içi ortamınızda daha fazla makineniz varsa, [Hyper-V](scale-hyper-v-assessment.md) ve [VMware](scale-vmware-assessment.md) değerlendirmesini nasıl ölçeklentireceğinizi öğrenin.

## <a name="azure-migrate-server-assessment"></a>Azure Geçişi: Sunucu Değerlendirmesi

### <a name="does-azure-migrate-server-assessment-support-assessment-of-physical-servers"></a>Azure geçişi yapar: Sunucu değerlendirmesi fiziksel sunucu değerlendirmesi destekliyor mu?

Hayır, Azure geçişi Şu anda fiziksel sunucu değerlendirmesini desteklemiyor.

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Azure geçişi 'nin bir VMware ortamını bulması vCenter Server gerekir mi?

Evet, Azure geçişi 'nin bir VMware ortamını bulması için vCenter Server gerekir. VCenter Server tarafından yönetilmeyen ESXi konaklarının bulunmasını desteklemez.

### <a name="whats-the-difference-between-using-azure-migrate-server-assessment-and-the-map-toolkit"></a>Azure geçişi kullanımı arasındaki fark nedir: Sunucu değerlendirmesi ve harita araç seti?

Azure Geçişi: Sunucu değerlendirmesi, geçiş hazırlığı ve Azure 'a geçiş için iş yüklerinin değerlendirilmesi konusunda yardımcı olmak üzere geçiş değerlendirmesi sağlar. [Microsoft Assessment and planlama (Map) araç seti](https://www.microsoft.com/download/details.aspx?id=7826) , Windows istemci ve sunucu işletim sistemlerinin daha yeni sürümleri için geçiş planlaması ve yazılım kullanımı izleme gibi diğer işlevlere sahiptir. Bu senaryolar için harıta araç setini kullanmaya devam edin.

### <a name="how-is-azure-migrate-server-assessment-different-from-azure-site-recovery-deployment-planner"></a>Azure geçişi: Sunucu değerlendirmesi Azure Site Recovery Dağıtım Planlayıcısı farklı mi?

Azure Geçişi: Sunucu değerlendirmesi bir geçiş planlama aracıdır. Azure Site Recovery Dağıtım Planlayıcısı bir olağanüstü durum kurtarma planlama aracıdır.

- **VMware/Hyper-V ' d e Azure 'A geçiş**: Şirket içi sunucularınızı Azure 'a geçirmeyi planlıyorsanız Azure geçişini kullanın: Geçiş planlaması için sunucu değerlendirmesi aracı. Araç, şirket içi iş yüklerini değerlendirir ve Azure 'a geçiş yaparken size yardımcı olacak yönergeler, Öngörüler ve mekanizmalar sağlar. Geçiş planınızla çalışmaya başladıktan sonra Azure geçişi gibi araçları kullanabilirsiniz: Makineleri Azure 'a geçirmek için sunucu geçişi.
- **VMware/Hyper-V ' d e olağanüstü durum kurtarma**: Site Recovery kullanarak Azure 'a olağanüstü durum kurtarma için, olağanüstü durum kurtarma planlaması için Site Recovery Dağıtım Planlayıcısı kullanın. Site Recovery Dağıtım Planlayıcısı, şirket içi ortamınızın ayrıntılı ve Site Recovery özgü bir değerlendirmesini yapar. Çoğaltma ve VM 'lerin yük devretmesi gibi başarılı olağanüstü durum işlemleri için Site Recovery gereken öneriler sağlar.

### <a name="does-azure-migrate-support-enterprise-agreement-ea-based-cost-estimation"></a>Azure, Kurumsal Anlaşma (EA) tabanlı maliyet tahmini desteği ister misiniz?

Azure geçişi Şu anda [Kurumsal anlaşma teklifi](https://azure.microsoft.com/offers/enterprise-agreement-support/)için maliyet tahmini desteklememektedir. Geçici çözüm, teklif olarak Kullandıkça Öde 'yi belirtmek ve değerlendirme özelliklerinin ' Indirim ' alanındaki indirim yüzdesini (abonelik için geçerlidir) el ile belirtmektir.

  ![İndirim](./media/resources-faq/discount.png)

### <a name="whats-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>Şirket içi boyutlandırma ve performans tabanlı boyutlandırma arasındaki fark nedir?

- Şirket içi boyutlandırma olarak Azure geçişi, VM performans verilerini kabul etmez. VM 'Leri, şirket içi yapılandırmaya göre boyutlandırır. -Performans tabanlı boyutlandırma bölümünde, boyutlandırma kullanım verilerini temel alır.
- Örneğin, şirket içi bir VM 4 çekirdeğe ve% 50 CPU kullanımı ve% 50 bellek kullanımı ile 8 GB belleğe sahipse, şirket içi boyutlandırma dört çekirdekli ve 8 GB bellekle bir Azure VM SKU 'SU önerir. Ancak, performans tabanlı boyutlandırma, kullanım yüzdesi kabul edildiği için iki çekirdeğin ve 4 GB 'lik bir VM SKU 'SU önerir.
- Benzer şekilde, disk boyutlandırma iki değerlendirme özelliklerine ve boyutlandırma ölçütüne ve depolama türüne bağlıdır.
= Boyutlandırma ölçütü performans tabanlı ve depolama türü otomatiktir ise, diskin ıOPS ve aktarım hızı değerleri, hedef disk türü (Standart veya Premium) tanımlanırken kabul edilir.
- Boyutlandırma ölçütü performans tabanlıdır ve depolama türü Premium ise, Premium bir disk önerilir. Premium disk SKU 'SU, şirket içi diskin boyutuna göre belirlenir. Aynı mantık, boyutlandırma ölçütü şirket içi boyutlandırmadır ve depolama türü standart veya Premium olduğunda disk boyutlandırmayı yapmak için kullanılır.

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>Performans geçmişi ve yüzdebirlik kullanımı, boyut önerilerinde ne etkiler?

Bu özellikler yalnızca performans tabanlı boyutlandırma için geçerlidir.

- Azure geçişi, şirket içi makinelerin performans geçmişini toplar ve Azure 'da VM boyutunu ve disk türünü önermek için kullanır.
- Gereç, her 20 saniyede bir gerçek zamanlı kullanım verilerini toplamak için şirket içi ortamı sürekli olarak profillerdir. Alet, 20 saniyelik örnekler toparlar ve her 15 dakika için tek bir veri noktası oluşturur. Tek veri noktasını oluşturmak için alet tüm 20 saniyelik örneklerden en yüksek değerleri seçer ve Azure’a gönderir.
- Azure 'da bir değerlendirme oluşturduğunuzda (performans süresi ve performans geçmişi yüzdelik değeri temelinde), Azure geçişi etkin kullanım değerini hesaplar ve boyutlandırmak için kullanır.
- Örneğin, performans süresini bir gün olacak şekilde ayarlarsanız ve yüzdelik değer olarak 95 yüzdebirlik değerini belirtirseniz Azure geçişi, son gün için toplayıcı tarafından gönderilen 15 dakikalık örnek noktalarını kullanır, bunları artan düzende sıralar ve 95. yüzdebirlik değerini şu şekilde seçer. etkin kullanım.
- 95. yüzdebirlik değeri, 99. yüzdebirlik değerini kullanırsanız oluşabilecek tüm aykırı değerleri yok saymanızı sağlar. Dönemin en yüksek kullanımını seçmek ve aykırı değerleri kaçırmamak istiyorsanız 99. yüzdebirliği seçmelisiniz.

### <a name="what-is-dependency-visualization"></a>Bağımlılık görselleştirmesi nedir?

Bağımlılık görselleştirme, daha fazla güvenle geçiş için VM gruplarını değerlendirmenizi sağlar. Değerlendirme çalıştırmadan önce makine bağımlılıklarını çapraz denetler. Bağımlılık görselleştirme, hiçbir şeyin geri ayrılmaması ve Azure 'a geçiş yaparken beklenmedik kesintilerden kaçınılmasını sağlar. Azure geçişi, bağımlılık görselleştirmesini etkinleştirmek için Azure Izleyici günlüklerindeki Hizmet Eşlemesi çözümünü kullanır.

> [!NOTE]
> Bağımlılık görselleştirme işlevselliği Azure Kamu 'da kullanılamaz.

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>Bağımlılık görselleştirme özelliğini kullanmak için ödeme yapmam gerekir mi?

Hayır. Azure Geçişi fiyatlandırması hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için bir şey yüklemem gerekiyor mu?

Bağımlılık görselleştirmesini kullanmak için, değerlendirmek istediğiniz her şirket içi makineye aracıları indirip yüklemeniz gerekir.

- [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) her makineye yüklenmelidir.
- [Bağımlılık aracısının](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure) her makinede yüklü olması gerekir.
- Ayrıca, internet bağlantısı olmayan makineleriniz varsa Log Analytics ağ geçidini indirip yüklemeniz gerekir.

Bağımlılık görselleştirmesi kullanmadığınız takdirde bu aracılara ihtiyacınız yoktur.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için mevcut bir çalışma alanını kullanabilir miyim?

Evet, var olan bir çalışma alanını geçiş projesine iliştirebilir ve bunu bağımlılık görselleştirmesi için kullanabilirsiniz. [Daha fazla bilgi edinin](concepts-dependency-visualization.md#how-does-it-work).

### <a name="can-i-export-the-dependency-visualization-report"></a>Bağımlılık görselleştirme raporunu dışarı aktarabilir miyim?

Hayır, bağımlılık görselleştirmesi verilemiyor. Bununla birlikte, Azure geçişi bağımlılık görselleştirmesi için Hizmet Eşlemesi kullandığından, bağımlılıkları JSON biçiminde almak için [HIZMET EŞLEMESI REST API 'lerini](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) kullanabilirsiniz.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-dependency-agent"></a>Microsoft Monitoring Agent (MMA) ve bağımlılık aracısının yüklenmesini nasıl otomatikleştirebilirim?

Aracıları yüklemek için [bu betiği kullanın](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) . Komut satırını veya Otomasyonu kullanarak MMA 'yı yüklemek için [Bu yönergeleri izleyin](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) . MMA için [Bu betikten](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab)yararlanın.

Betiklerin yanı sıra, aracıları dağıtmak için System Center Configuration Manager, [ıntigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) vb. gibi dağıtım araçlarını kullanabilirsiniz.

### <a name="what-operating-systems-are-supported-by-mma"></a>MMA tarafından desteklenen işletim sistemleri nelerdir?

- MMA tarafından desteklenen Windows işletim sistemlerinin listesini [gözden geçirin](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) .
- MMA tarafından desteklenen Linux işletim sistemlerinin listesini [gözden geçirin](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) .

### <a name="what-are-the-operating-systems-supported-by-the-dependency-agent"></a>Bağımlılık Aracısı tarafından desteklenen işletim sistemleri nelerdir?

Bağımlılık Aracısı tarafından desteklenen Windows işletim sistemlerini [gözden geçirin](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems) .
Bağımlılık Aracısı tarafından desteklenen Linux işletim sistemlerinin listesini [gözden geçirin](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems) .

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-an-hour"></a>Bir saatten uzun bir süre boyunca Azure geçişi bağımlılıklarını görselleştirebilir miyim?
Hayır, bağımlılıkları bir saate kadar görselleştirebilirsiniz. Geçmişte geçen aya kadar belirli bir tarihe gidebilirsiniz, ancak görselleştirme için en uzun süre bir saattir. Örneğin, dün bağımlılıklarını görüntülemek için bağımlılık eşlemesindeki süre süresini kullanabilirsiniz, ancak yalnızca bir saatlik pencere için görüntüleyebilir. Ancak, Azure Izleyici günlüklerini daha uzun bir süre boyunca [bağımlılık verilerini sorgulamak](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) için kullanabilirsiniz.

### <a name="is-dependency-visualization-supported-for-groups-with-more-than-10-vms"></a>10 ' dan fazla VM 'ye sahip gruplar için bağımlılık görselleştirmesi destekleniyor mu?
10 adede kadar VM içeren [gruplar için bağımlılıkları görselleştirebilirsiniz](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) . 10 ' dan fazla VM içeren bir grubunuz varsa, grubu daha küçük gruplara bölmeniz ve sonra bağımlılıkları görselleştirmeniz önerilir.

## <a name="azure-migrate-server-migration"></a>Azure Geçişi: Sunucu Geçişi

### <a name="how-is-azure-migrate-server-migration-different-from-azure-site-recovery"></a>Azure geçişi: Sunucu geçişi Azure Site Recovery 'den farklı mi?

Azure Geçişi: Sunucu geçişi, VMware VM 'lerinin aracı tabanlı geçişi, Hyper-V VM 'lerinin geçirilmesi ve fiziksel sunucuların Azure 'a geçirilmesi için Site Recovery çoğaltma altyapısından yararlanır. VMware VM 'lerini geçirmeye yönelik aracısız seçeneği, sunucu geçişinde yerel olarak oluşturulmuştur.

## <a name="next-steps"></a>Sonraki adımlar
[Azure geçişi 'ne genel bakış](migrate-services-overview.md) konusunu okuyun
