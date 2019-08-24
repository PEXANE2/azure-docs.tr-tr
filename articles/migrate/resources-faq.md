---
title: Azure geçişi-sık sorulan sorular (SSS) | Microsoft Docs
description: Azure geçişi hakkında sık sorulan soruları adresleyen
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: snehaa
ms.openlocfilehash: 96fde6ade45bb92036fbc9ac803eebd50640bf85
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70013439"
---
# <a name="azure-migrate-frequently-asked-questions-faq"></a>Azure Geçişi: Sık sorulan sorular (SSS)

Bu makalede, Azure geçişi hakkında sık sorulan sorular yanıtlanmaktadır. Bu makaleyi okuduktan sonra daha fazla sorgunuz varsa, bunları [Azure geçişi forumuna](https://aka.ms/AzureMigrateForum)gönderin.

## <a name="general"></a>Genel

### <a name="which-azure-geographies-does-azure-migrate-support"></a>Azure 'un hangi Azure jeo geçiş desteği

[VMware listesi](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) ve [Hyper-V listesi](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects)için bkz.

### <a name="whats-the-difference-between-azure-migrate-and-azure-site-recovery"></a>Azure geçişi ile Azure Site Recovery arasındaki fark nedir?

Azure geçişi, geçiş işlemini başlatmak, makinelerin ve iş yüklerinin bulmayı ve değerlendirmesini izlemek, makinelerin ve iş yüklerinin Azure 'a geçişini yürütmek ve izlemek için merkezi bir merkez sağlar. [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) bir olağanüstü durum kurtarma çözümüdür. Azure geçişi sunucu geçişi, şirket içi makinelerin geçiş ve geçiş için geçiş senaryolarını etkinleştirmek üzere arka uçta Azure Site Recovery kullanır.

## <a name="azure-migrate-appliance-vmwarephysical-servers"></a>Azure geçişi gereci (VMware/fiziksel sunucular)

### <a name="how-does-the-azure-migrate-appliance-connect-to-azure"></a>Azure geçişi gereci Azure 'a nasıl bağlanır?

Bağlantı Internet üzerinden olabilir veya Azure ExpressRoute 'u ortak/Microsoft eşlemesi ile birlikte kullanabilirsiniz.

### <a name="what-are-the-network-connectivity-requirements-for-azure-migrate-server-assessment-and-migration"></a>Azure geçişi sunucu değerlendirmesi ve geçişi için ağ bağlantısı gereksinimleri nelerdir?

Azure geçişi 'nin Azure ile iletişim kurmasını sağlamak için gereken URL 'Ler ve bağlantı noktaları hakkında bilgi için bkz. [VMware](migrate-support-matrix-vmware.md) ve [Hyper-V](migrate-support-matrix-hyper-v.md) destek matrisleri.

### <a name="can-i-harden-the-appliance-vm-that-i-set-up-with-the-template"></a>Şablonla ayarladığım gereç sanal makinesini kullanabilir miyim?

Azure geçişi gereci için gereken iletişim ve güvenlik duvarı kurallarını olduğu sürece şablona (örneğin, virüsten koruma) bileşen ekleyebilirsiniz.

### <a name="what-data-is-collected-by-the-azure-migrate-appliance"></a>Azure geçişi gereci hangi verileri toplamıştır?

Azure geçişi [belgelerinde](https://docs.microsoft.com/azure/migrate/migrate-appliance#collected-performance-data-vmware)Azure geçişi uygulaması tarafından toplanan verilerle ilgili ayrıntıları görüntüleyebilirsiniz.

### <a name="is-there-any-performance-impact-on-the-analyzed-vmware-or-hyper-v-environment"></a>Çözümlenen VMware veya Hyper-V ortamında herhangi bir performans etkisi var mı?

Azure, sanal makine performans verilerini ölçmek için şirket içi makineleri şirket içi makinelere sürekli olarak geçiş yaptı. Bu profil oluşturma, Hyper-V/ESXi Konakları veya vCenter Server üzerinde neredeyse hiçbir performans etkisi yoktur.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Toplanan veriler nerede depolanıyor ve ne kadar süreyle?

Azure geçişi gereci tarafından toplanan veriler, geçiş projesini oluştururken seçtiğiniz Azure konumunda depolanır. Veriler bir Microsoft aboneliğine güvenli bir şekilde depolanır ve Azure geçişi projesini sildiğinizde silinir.

Bağımlılık görselleştirmesi için, VM 'Lere aracılar yüklerseniz, bağımlılık aracıları tarafından toplanan veriler ABD 'de, Azure aboneliğinde oluşturulan bir Log Analytics çalışma alanında depolanır. Bu veriler, aboneliğinizdeki Log Analytics çalışma alanını sildiğinizde silinir. Daha fazla bilgi için bkz. [bağımlılık görselleştirme](concepts-dependency-visualization.md).

### <a name="what-volume-of-data-is-uploaded-by-the-azure-migrate-appliance-during-continuous-profiling"></a>Sürekli profil oluşturma sırasında Azure geçiş gereci tarafından hangi veri hacmi karşıya yüklendi?

Azure geçişi 'ne gönderilen verilerin hacmi, birkaç parametreye bağlı olarak değişir. Birime ilişkin bir fikir sahibi olmak için: 10 makine içeren bir Azure geçişi projesi (her biri bir disk ve bir NIC ile), günde 50 MB 'lik bir şekilde gönderilir. Bu değer bir yaklaşık değerdir. NIC ve disklerin veri noktası sayısına göre değişir. (Makine, NIC 'ler veya disk sayısı arttıkça, gönderilen verilerdeki artış doğrusal değildir.)

### <a name="is-the-data-encrypted-at-rest-and-in-transit"></a>Veriler, bekleyen ve aktarım sırasında mi şifrelendi?

Evet, her ikisi. Meta veriler, HTTPS üzerinden Azure geçişi hizmetine internet üzerinden güvenli bir şekilde gönderilir. Meta veriler bir Microsoft aboneliğinde [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) veritabanında ve [Azure Blob depolama](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) alanında depolanır. Veriler Rest 'de şifrelenir.

Bağımlılık aracıları tarafından toplanan veriler de aktarım sırasında şifrelenir (güvenli HTTPS). Bu, aboneliğinizdeki bir Log Analytics çalışma alanında depolanır. Ayrıca rest 'de şifrelenir.

### <a name="how-does-the-azure-migrate-appliance-communicate-with-vcenter-server-and-the-azure-migrate-service"></a>Azure geçişi gereci vCenter Server ve Azure geçişi hizmeti ile nasıl iletişim kurar?

Gereç, gereci ayarlarken verdiğiniz kimlik bilgilerini kullanarak vCenter Server (bağlantı noktası 443) öğesine bağlanır. VCenter Server tarafından yönetilen VM 'Lerle ilgili meta verileri toplamak üzere vCenter Server sorgulamak için VMware PowerCLI kullanır. VM 'Ler (çekirdekler, bellek, diskler, NIC 'Ler vb.) hakkında yapılandırma verilerini ve ayrıca geçen ay için her VM 'nin performans geçmişini toplar. Toplanan meta veriler daha sonra değerlendirme için Azure geçişi sunucu değerlendirmesini (HTTPS üzerinden Internet üzerinden) gönderilir.

### <a name="can-i-connect-the-same-appliance-to-multiple-vcenter-server-instances"></a>Aynı gereci birden çok vCenter Server örneğine bağlanabilir miyim?

Hayır. Bir gereç ve vCenter Server arasında bire bir eşleme vardır. Birden çok vCenter Server örneğinde VM 'Leri bulmanız gerekiyorsa, birden çok gereç dağıtmanız gerekir.


### <a name="i-changed-my-machine-size-can-i-run-the-assessment-again"></a>Makine boyutumu değiştirdim. Değerlendirmeyi yeniden çalıştırabilir miyim?

Azure geçişi gereci, şirket içi ortam hakkındaki bilgileri sürekli olarak toplar. Ancak değerlendirme, şirket içi VM 'lerin bir zaman noktası anlık görüntüsüdür. Değerlendirmek istediğiniz bir VM 'deki ayarları değiştirirseniz, en son değişikliklerle değerlendirmesi güncelleştirmek için yeniden hesapla seçeneğini kullanın.

### <a name="how-can-i-perform-discovery-in-a-multitenant-environment-in-azure-migrate-server-assessment"></a>Azure geçişi sunucu değerlendirmesinde çok kiracılı bir ortamda bulma işlemini nasıl yapabilirim?

VMware için, kiracılar genelinde paylaşılan bir ortamınız varsa ve başka bir kiracının aboneliğindeki bir kiracının VM 'lerini öğrenmek istemiyorsanız, yalnızca öğrenmek istediğiniz VM 'lere erişebilen vCenter Server kimlik bilgileri oluşturun. Daha sonra bu kimlik bilgilerini Azure geçiş gereci bulmayı başlattığınızda kullanın.

Hyper-V için bulma, Hyper-V ana bilgisayar kimlik bilgilerini kullanır. VM 'Ler aynı Hyper-V konağını paylaşıyorsa, şu anda bulmayı ayırmanın bir yolu yoktur.  

### <a name="how-many-vms-can-i-discover-with-a-single-migration-appliance"></a>Tek bir geçiş gereci ile kaç VM keşfedebilirim?

En fazla 10.000 VMware VM ve tek bir geçiş gereci olan en fazla 5.000 Hyper-V VM 'Leri bulabilirsiniz. Şirket içi ortamınızda daha fazla makineniz varsa, [Hyper-V](scale-hyper-v-assessment.md) ve [VMware](scale-vmware-assessment.md) değerlendirmesini nasıl ölçeklentireceğinizi öğrenin.

## <a name="azure-migrate-server-assessment"></a>Azure geçişi sunucu değerlendirmesi

### <a name="does-azure-migrate-server-assessment-support-assessment-of-physical-servers"></a>Azure geçişi sunucu değerlendirmesi fiziksel sunucu değerlendirmesi destekliyor mu?

Hayır, Azure geçişi Şu anda fiziksel sunucu değerlendirmesini desteklememektedir.

### <a name="does-azure-migrate-need-vcenter-server-to-perform-discovery-in-a-vmware-environment"></a>Azure geçişi 'nin bir VMware ortamında bulmayı gerçekleştirmesi vCenter Server gerekir mi?

Evet, Azure geçişi 'nin bir VMware ortamında bulmayı gerçekleştirmesi vCenter Server gerekir. VCenter Server tarafından yönetilmeyen ESXi konaklarının bulunmasını desteklemez.

### <a name="whats-the-difference-between-azure-migrate-server-assessment-and-the-map-toolkit"></a>Azure geçişi sunucu değerlendirmesi ve harıta araç seti arasındaki fark nedir?

Azure geçişi sunucu değerlendirmesi, geçiş hazırlığı ve Azure 'a geçiş için iş yüklerinin değerlendirilmesi konusunda yardımcı olmak üzere geçiş değerlendirmesi sağlar. [Microsoft değerlendirme ve planlama (eşleme) araç seti](https://www.microsoft.com/download/details.aspx?id=7826) , Windows istemci ve sunucu işletim sistemlerinin ve yazılım kullanımı izlemenin daha yeni sürümleri için geçiş planlaması gibi diğer görevlerle yardımcı olur. Bu senaryolar için harıta araç setini kullanmaya devam edin.

### <a name="whats-the-difference-between-azure-migrate-server-assessment-and-the-azure-site-recovery-deployment-planner"></a>Azure geçişi sunucu değerlendirmesi ve Azure Site Recovery Dağıtım Planlayıcısı arasındaki fark nedir?

Azure geçişi sunucu değerlendirmesi bir geçiş planlama aracıdır. Azure Site Recovery Dağıtım Planlayıcısı bir olağanüstü durum kurtarma planlama aracıdır.

**VMware/Hyper-V ' d e Azure 'A geçiş**: Şirket içi sunucularınızı Azure 'a geçirmeyi planlıyorsanız, geçiş planlaması için Azure geçişi sunucu değerlendirmesi ' ni kullanın. Şirket içi iş yüklerini değerlendirir ve sunucularınızı Azure 'a geçirmenize yardımcı olacak yönergeler, Öngörüler ve araçlar sağlar. Geçiş planınızla çalışmaya başladıktan sonra, makineleri Azure 'a geçirmek için Azure geçiş sunucusu geçişi gibi araçları kullanabilirsiniz.

**VMware/Hyper-V ' d e olağanüstü durum kurtarma**: Azure 'da Site Recovery aracılığıyla olağanüstü durum kurtarma için, planlama için Site Recovery Dağıtım Planlayıcısı kullanın. Site Recovery Dağıtım Planlayıcısı, şirket içi ortamınızın ayrıntılı ve Site Recovery özgü bir değerlendirmesini yapar. VM 'lerin çoğaltılması ve yük devretmesi gibi başarılı olağanüstü durum işlemleri için Site Recovery gereken öneriler sağlar.

### <a name="does-azure-migrate-support-cost-estimation-for-the-enterprise-agreement-ea-program"></a>Azure geçişi Kurumsal Anlaşma (EA) programı için destek maliyeti tahmini mi?

Azure geçişi Şu anda [Kurumsal Anlaşma program](https://azure.microsoft.com/offers/enterprise-agreement-support/)için maliyet tahmini desteklememektedir. Geçici çözüm, **teklif** olarak **Kullandıkça Öde** ' i belirtmek ve değerlendirme özelliklerinin **indirim** kutusunda indirim yüzdesini (abonelik için geçerlidir) el ile belirtmektir:

  ![Değerlendirme özellikleri](./media/resources-faq/discount.png)

### <a name="whats-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>Şirket içi boyutlandırma ve performans tabanlı boyutlandırma arasındaki fark nedir?

Şirket içi olarak boyutlandırılması sayesinde Azure geçişi, VM performans verilerini kabul etmez. VM 'Leri, şirket içi yapılandırmaya göre boyutlandırır. Performans tabanlı boyutlandırma ile boyutlandırma, kullanım verilerini temel alır.

Örneğin,% 50 CPU kullanımı ve% 50 bellek kullanımında 4 çekirdeğe ve 8 GB belleğe sahip olan bir şirket içi VM 'yi düşünün. Bu VM için, şirket içi boyutlandırma, dört çekirdeğe ve 8 GB belleğe sahip bir Azure VM SKU 'SU önerir. Performans tabanlı boyutlandırma, kullanım yüzdesi kabul edildiği için iki çekirdeğe ve 4 GB belleğe sahip bir VM SKU 'SU önerir.

Benzer şekilde, disk boyutlandırma iki değerlendirme özelliklerine bağlıdır: boyutlandırma ölçütleri ve depolama türü. Boyutlandırma ölçütü performans tabanlıdır ve depolama türü otomatik ise, Azure geçişi, hedef disk türünü (Standart veya Premium) belirlediğinde, diskin ıOPS ve aktarım hızı değerlerini hesaba alır.

Boyutlandırma ölçütü performans tabanlıdır ve depolama türü Premium ise, Azure geçişi bir Premium disk önerir. Premium disk SKU 'SU, şirket içi diskin boyutuna göre belirlenir. Aynı mantık, boyutlandırma ölçütleri şirket içi boyutlandırmadır ve depolama türü standart veya Premium olduğunda disk boyutlandırmayı yapmak için kullanılır.

### <a name="what-impact-does-performance-history-and-utilization-percentile-have-on-size-recommendations"></a>Performans geçmişi ve kullanım yüzdebirlik değeri boyut önerilerinde ne etkiler?

Bu özellikler yalnızca performans tabanlı boyutlandırma için geçerlidir.

Azure geçişi, şirket içi makinelerin performans geçmişini toplar ve Azure 'da VM boyutunu ve disk türünü önermek için kullanır.

Gereç, her 20 saniyede bir gerçek zamanlı kullanım verilerini toplamak için şirket içi ortamı sürekli olarak profillerdir. Gereç, 20 saniyelik örnekleri kaydeder ve 15 dakikada bir tek bir veri noktası oluşturur. Veri noktasını oluşturmak için gereç tüm 20 saniyelik örneklerden tepe değeri seçer. Gereç bu veri noktasını Azure 'a gönderir.

Azure 'da bir değerlendirme oluşturduğunuzda (performans süresi ve performans geçmişi yüzdelik değeri temelinde), Azure geçişi etkin kullanım değerini hesaplar ve boyutlandırma için kullanır.

Örneğin, performans süresini bir güne ve yüzdebirlik değerini 95 yüzdebirlik olarak ayarlayadığınızı varsayalım. Azure geçişi, son gün için toplayıcı tarafından gönderilen 15 dakikalık örnek noktalarını artan düzende sıralar ve geçerli kullanım olarak 95. yüzdebirlik değerini seçer.

95. yüzdebirlik değeri, herhangi bir aykırı şeyi yok saymanızı sağlar. 99. yüzdebirlik 'i kullanırsanız, dış değerler dahil edilebilir. Dönemin en yoğun kullanımını seçmek ve tüm aykırı değerleri kaçırmak istemiyorsanız 99. yüzdebirlik ' i seçin.

### <a name="what-is-dependency-visualization"></a>Bağımlılık görselleştirmesi nedir?

Bağımlılık görselleştirme, daha fazla güvenle geçiş için VM gruplarını değerlendirmenizi sağlar. Değerlendirme çalıştırmadan önce makine bağımlılıklarını çapraz denetler. Bağımlılık görselleştirme, Azure 'a geçiş yaparken beklenmedik kesintilerden kaçınmak için hiçbir şeyin geri ayrılmasına yardımcı olur. Azure geçişi, bağımlılık görselleştirmesini etkinleştirmek için Azure Izleyici günlüklerinde Hizmet Eşlemesi çözümünü kullanır.

> [!NOTE]
> Bağımlılık görselleştirmesi Azure Kamu 'da kullanılamaz.

### <a name="do-i-need-to-pay-to-use-dependency-visualization"></a>Bağımlılık görselleştirmesini kullanmak için ödeme yapmam gerekir mi?

Hayır. Daha fazla bilgi için bkz. [Azure geçiş fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için bir şey yüklemem gerekiyor mu?

Bağımlılık görselleştirmesini kullanmak için, değerlendirmek istediğiniz her şirket içi makineye aracıları indirip yüklemeniz gerekir.

Her makineye aşağıdaki aracıları yüklemeniz gerekir:
- [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows).
- [Bağımlılık Aracısı](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure).
- İnternet bağlantısı olmayan makineleriniz varsa Log Analytics ağ geçidini indirip yüklemeniz gerekir.

Bağımlılık görselleştirmesi kullanmadığınız takdirde bu aracılara ihtiyacınız yoktur.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için mevcut bir çalışma alanını kullanabilir miyim?

Evet, var olan bir çalışma alanını geçiş projesine iliştirebilir ve bunu bağımlılık görselleştirmesi için kullanabilirsiniz. Daha fazla bilgi için [bağımlılık görselleştirme](concepts-dependency-visualization.md#how-does-it-work) makalesinde "nasıl çalışır?" başlığına bakın.

### <a name="can-i-export-the-dependency-visualization-report"></a>Bağımlılık görselleştirme raporunu dışarı aktarabilir miyim?

Hayır, bağımlılık görselleştirmesi verilemiyor. Ancak Azure geçişi bağımlılık görselleştirmesi için Hizmet Eşlemesi kullandığından, bağımlılıkları JSON biçiminde almak için [Hizmet Eşlemesi REST API](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) kullanabilirsiniz.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-the-dependency-agent"></a>Microsoft Monitoring Agent (MMA) ve bağımlılık aracısının yüklenmesini nasıl otomatikleştirebilirim?

[Aracıları yüklemek için bu betiği](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples)kullanın. Komut satırını veya Otomasyonu kullanarak [MMA 'yı yüklemek için bu yönergeleri](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) izleyin. MMA için [bu betiği](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab)kullanın.

Betiklerin yanı sıra, aracıları dağıtmak için System Center Configuration Manager ve [ıntigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) gibi dağıtım araçlarını da kullanabilirsiniz.

### <a name="what-operating-systems-are-supported-by-mma"></a>MMA tarafından desteklenen işletim sistemleri nelerdir?

- [MMA tarafından desteklenen Windows işletim sistemlerinin](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems)listesini görüntüleyin.
- [MMA tarafından desteklenen Linux işletim sistemlerinin](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems)listesini görüntüleyin.

### <a name="what-operating-systems-are-supported-by-the-dependency-agent"></a>Bağımlılık Aracısı tarafından hangi işletim sistemleri destekleniyor?

- [Bağımlılık Aracısı tarafından desteklenen Windows işletim sistemlerinin](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems)listesini görüntüleyin.
- [Bağımlılık Aracısı tarafından desteklenen Linux işletim sistemlerinin](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems)listesini görüntüleyin.

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-an-hour"></a>Bir saatten uzun bir süre boyunca Azure geçişi bağımlılıklarını görselleştirebilir miyim?
Hayır. Bağımlılıkları bir saate kadar görselleştirebilirsiniz. Geçmiş olarak belirli bir tarihe kadar geri dönerek, bir ay olarak geri dönebilirsiniz, ancak görselleştirme için en uzun süre bir saat olabilir. Örneğin, dün bağımlılıklarını görüntülemek için bağımlılık eşlemesindeki süre süresini kullanabilirsiniz, ancak yalnızca bir saatlik pencere için görüntüleyebilirsiniz. Ancak, Azure Izleyici günlüklerini daha uzun bir süre boyunca [bağımlılık verilerini sorgulamak](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) için kullanabilirsiniz.

### <a name="can-i-use-dependency-visualization-for-groups-that-contain-more-than-10-vms"></a>10 ' dan fazla VM içeren gruplar için bağımlılık görselleştirmesini kullanabilir miyim?
10 adede kadar VM içeren [gruplar için bağımlılıkları görselleştirebilirsiniz](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) . 10 ' dan fazla VM 'ye sahip bir grubunuz varsa, grubu daha küçük gruplara bölmeniz ve sonra bağımlılıkları görselleştirmenizi öneririz.

## <a name="azure-migrate-server-migration"></a>Azure geçiş sunucusu geçişi

### <a name="whats-the-difference-between-azure-migrate-server-migration-and-azure-site-recovery"></a>Azure geçişi sunucu geçişi ve Azure Site Recovery arasındaki fark nedir?

Azure geçişi sunucu geçişi, VMware VM 'lerinin aracı tabanlı geçişi, Hyper-V VM 'lerinin geçirilmesi ve fiziksel sunucuların Azure 'a geçirilmesi için Site Recovery çoğaltma altyapısını kullanır. VMware VM 'lerini geçirmeye yönelik aracısız seçenek, sunucu geçişine yerel olarak yerleşik olarak oluşturulmuştur.

## <a name="next-steps"></a>Sonraki adımlar
[Azure geçişi 'ne genel bakış](migrate-services-overview.md)konusunu okuyun.
