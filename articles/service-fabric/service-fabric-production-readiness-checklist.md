---
title: Azure Hizmet Kumaş Üretimine Hazırlık Kontrol Listesi
description: En iyi uygulamaları takip ederek Servis Kumaşı uygulamanızı ve küme üretiminizi hazırlayın.
ms.topic: conceptual
ms.date: 6/05/2019
ms.openlocfilehash: 90d600b01aa870f7b3a58e70ef32e774e7107524
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75376809"
---
# <a name="production-readiness-checklist"></a>Üretim hazırlığı denetim listesi

Uygulamanız ve kümeniz üretim trafiğine hazır mı? Uygulamanızı ve kümenizi çalıştırmak ve test etmek, uygulamanın üretime geçmeye hazır olduğu anlamına gelmez. Aşağıdaki denetim listesini inceleyerek uygulamanızın ve kümelemenin sorunsuz çalışmasını engelleyin. Tüm bu öğelerin kontrol edilmesine şiddetle tavsiye ediyoruz. Açıkçası, belirli bir satır öğesi için alternatif çözümler kullanmayı seçebilirsiniz (örneğin, kendi tanılama çerçeveleri).


## <a name="prerequisites-for-production"></a>Üretim için ön koşullar
1. Azure Hizmet Kumaşı en iyi uygulamalar: [Uygulama Tasarımı,](./service-fabric-best-practices-applications.md) [Güvenlik,](./service-fabric-best-practices-security.md) [Ağ,](./service-fabric-best-practices-networking.md) [Kapasite planlama ve ölçeklendirme,](./service-fabric-best-practices-capacity-scaling.md) [Kod olarak Altyapı](./service-fabric-best-practices-infrastructure-as-code.md)ve İzleme [ve Tanılama](./service-fabric-best-practices-monitoring.md). 
1. Aktörler programlama modelini kullanıyorsanız Güvenilir Aktörler güvenlik yapılandırmasını uygulayın
1. 20'den fazla çekirdeği veya 10 düğümü olan kümeler için sistem hizmetleri için özel bir birincil düğüm türü oluşturun. Sistem hizmetleri için birincil düğüm türünü ayırmak için [yerleşim kısıtlamaları](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) ekleyin.
1. Birincil düğüm türü için D2v2 veya daha yüksek Bir SKU kullanın. En az 50 GB sabit disk kapasitesine sahip bir SKU seçmeniz önerilir.
1. Üretim kümeleri [güvenli](service-fabric-cluster-security.md)olmalıdır. Güvenli küme ayarlama örneği için bu [küme şablonuna](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/7-VM-Windows-3-NodeTypes-Secure-NSG)bakın. Sertifikalar için ortak adlar kullanın ve kendi imzalı sertifikaları kullanmaktan kaçının.
1. Düğüm kaynaklarının %75'inden fazlasını tüketmesinler diye [kapsayıcılara ve hizmetlere kaynak kısıtlamaları](service-fabric-resource-governance.md)ekleyin. 
1. Dayanıklılık [düzeyini](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster)anlayın ve ayarlayın. Gümüş veya daha yüksek dayanıklılık düzeyi, durum lu iş yüklerini çalıştıran düğüm türleri için önerilir. Birincil düğüm türü, Gümüş veya daha yüksek olarak ayarlanmış bir dayanıklılık düzeyine sahip olmalıdır.
1. Düğüm türünün [güvenilirlik düzeyini](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) anlayın ve seçin. Gümüş veya daha yüksek güvenilirlik önerilir.
1. Kümenizin [kapasite gereksinimlerini](service-fabric-cluster-capacity.md) belirlemek için iş yüklerinizi yükleyin ve ölçeklendirin. 
1. Hizmetleriniz ve uygulamalarınız izlenir ve uygulama günlükleri uyarı yla oluşturulur ve saklanır. Örneğin, [Bkz. Hizmet Kumaşı uygulamanıza günlük ekle](service-fabric-how-to-diagnostics-log.md) ve [Azure Monitor günlükleriyle kapsayıcıları izleyin.](service-fabric-diagnostics-oms-containers.md)
1. Küme uyarı ile izlenir (örneğin, [Azure Monitor günlükleri](service-fabric-diagnostics-event-analysis-oms.md)ile). 
1. Altta yatan sanal makine ölçeği kümesi altyapısı uyarı yla izlenir (örneğin, [Azure Monitor günlükleri](service-fabric-diagnostics-oms-agent.md)ile.
1. Kümenin [birincil ve ikincil sertifikaları](service-fabric-cluster-security-update-certs-azure.md) her zaman vardır (böylece kilitlenemezsin.
1. Geliştirme, evreleme ve üretim için ayrı kümeler koruyun. 
1. [Uygulama yükseltmeleri](service-fabric-application-upgrade.md) ve [küme yükseltmeleri](service-fabric-tutorial-upgrade-cluster.md) önce geliştirme ve evreleme kümelerinde test edilir. 
1. Üretim kümelerinde otomatik yükseltmeleri kapatın ve geliştirme ve evreleme kümeleri (gerektiğinde geri alma) için açın. 
1. Hizmetiniz için bir Kurtarma Noktası Hedefi (RPO) kurun ve bir [olağanüstü durum kurtarma işlemi](service-fabric-disaster-recovery.md) ayarlayın ve test edin.
1. Kümenizi el ile veya programlı olarak [ölçeklemeyi](service-fabric-cluster-scaling.md) planlayın.
1. Küme düğümlerinizi [yamalamayı](service-fabric-patch-orchestration-application.md) planlayın. 
1. En son değişikliklerinizin sürekli olarak test edilmesi için bir CI/CD ardışık etki alanı oluşturun. Örneğin, [Azure DevOps](service-fabric-tutorial-deploy-app-with-cicd-vsts.md) veya [Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md) kullanarak
1. [Hata Analizi Hizmeti](service-fabric-testability-overview.md) ile geliştirme & evreleme kümelerinizi test edin ve kontrollü [kaosa](service-fabric-controlled-chaos.md)neden edin. 
1. Uygulamalarınızı [ölçekleme](service-fabric-concepts-scalability.md) için planlayın. 


Service Fabric Reliable Services veya Reliable Actors programlama modelini kullanıyorsanız, aşağıdaki öğelerin işaretlenilmesi gerekir:
1. Servis kodunun `RunAsync` yöntemdeki iptal jetonuna sadık kalıp olmadığını ve özel iletişim dinleyicilerini kapadığını kontrol etmek için yerel geliştirme sırasında uygulamaları yükseltin.
1. Güvenilir Koleksiyonlar kullanırken [sık karşılaşılan tuzaklardan](service-fabric-work-with-reliable-collections.md) kaçının.
1. Yük testleri çalıştırırken .NET CLR bellek performans sayaçlarını izleyin ve yüksek oranda Çöp Toplama veya kaçak yığın büyümesini denetleyin.
1. [Güvenilir Hizmetler ve Güvenilir Aktörler'in](service-fabric-reliable-services-backup-restore.md) çevrimdışı yedeklemesini koruyun ve geri yükleme işlemini test edin.
1. Birincil Düğüm Tipi Sanal Makine örnek sayınız ideal olarak Kümeler Güvenilirlik katmanınız için minimuma eşit olmalıdır; Tier minimumunu aşmaya uygun koşullar şunları içerir: Birincil Düğüm Tiplerinizi dikey olarak ölçeklendirirken geçici olarak SKU.

## <a name="optional-best-practices"></a>İsteğe bağlı en iyi uygulamalar

Yukarıdaki listeler üretime geçmek için ön koşullar olmakla birlikte, aşağıdaki öğeler de göz önünde bulundurulmalıdır:
1. Dahili sağlık değerlendirme ve raporlamagenişletmek için [Service Fabric sağlık modeline](service-fabric-health-introduction.md) takın.
1. Uygulamanızı izleyen özel bir izleme ekibi dağıtın ve [kaynak dengeleme](service-fabric-cluster-resource-manager-balancing.md)için [yük](service-fabric-cluster-resource-manager-metrics.md) raporedin. 


## <a name="next-steps"></a>Sonraki adımlar
* [Hizmet Kumaşı Windows kümesini dağıtma](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* [Service Fabric Linux kümesini dağıtma](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* Service Fabric [uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) hakkında bilgi edinin.
