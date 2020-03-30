---
title: Azure Red Hat OpenShift için sık sorulan sorular
description: İşte Microsoft Azure Red Hat OpenShift ile ilgili sık sorulan soruların yanıtları
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: f468cb294d79c44f92ef95437c0d88639a78b9a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77619497"
---
# <a name="azure-red-hat-openshift-faq"></a>Azure Red Hat OpenShift SSS

Bu makalede, Microsoft Azure Red Hat OpenShift hakkında sık sorulan sorular (SSS) giderilmiştir.

## <a name="which-azure-regions-are-supported"></a>Hangi Azure bölgeleri desteklenir?

Azure Red Hat OpenShift'in desteklendiği küresel bölgelerin listesi için [desteklenen kaynaklara](supported-resources.md#azure-regions) bakın.

## <a name="can-i-deploy-a-cluster-into-an-existing-virtual-network"></a>Bir kümeyi varolan bir sanal ağa dağıtabilir miyim?

Hayır. Ancak, bir Azure Red Hat OpenShift kümesini gözleyerek varolan bir VNET'e bağlayabilirsiniz. Bkz. Bir kümenin sanal ağını ayrıntılar için [varolan bir sanal ağa bağlayın.](tutorial-create-cluster.md#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network)

## <a name="what-cluster-operations-are-available"></a>Hangi küme işlemleri kullanılabilir?

Yalnızca işlem düğümlerinin sayısını ölçeklendirebilir veya küçültebilirsiniz. Oluşturulduktan sonra `Microsoft.ContainerService/openShiftManagedClusters` kaynağa başka değişiklik yapılmasına izin verilmez. En fazla işlem düğümü sayısı 20 ile sınırlıdır.

## <a name="what-virtual-machine-sizes-can-i-use"></a>Hangi sanal makine boyutlarını kullanabilirim?

Azure Red Hat OpenShift kümesiyle kullanabileceğiniz sanal makine boyutlarının listesi için [Azure Red Hat OpenShift sanal makine boyutlarına](supported-resources.md#virtual-machine-sizes) bakın.

## <a name="is-data-on-my-cluster-encrypted"></a>Kümemdeki veriler şifrelenmiş mi?

Varsayılan olarak, istirahatte şifreleme vardır. Azure Depolama platformu, verilerinizi kalıcı hale gelmeden önce otomatik olarak şifreler ve almadan önce verilerin şifresini çözer. Ayrıntılar [için veriler için Azure Depolama Hizmeti Şifrelemesi'ne](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) bakın.

## <a name="can-i-use-prometheusgrafana-to-monitor-my-applications"></a>Uygulamalarımı izlemek için Prometheus/Grafana kullanabilir miyim?

Evet, Prometheus'u ad alanınızda dağıtabilir ve uygulamaları ad alanınızda izleyebilirsiniz.

## <a name="can-i-use-prometheusgrafana-to-monitor-metrics-related-to-cluster-health-and-capacity"></a>Küme durumu ve kapasitesiyle ilgili ölçümleri izlemek için Prometheus/Grafana kullanabilir miyim?

Hayır, şu anda değil.

## <a name="is-the-docker-registry-available-externally-so-i-can-use-tools-such-as-jenkins"></a>Docker kayıt defteri, Jenkins gibi araçları kullanabilmek için dışarıdan kullanılabilir mi?

Docker kayıt defteri ancak `https://docker-registry.apps.<clustername>.<region>.azmosa.io/` kullanılabilir, güçlü bir depolama dayanıklılık garantisi sağlanmaz. Azure Kapsayıcı [Kayıt Defteri'ni](https://azure.microsoft.com/services/container-registry/)de kullanabilirsiniz.

## <a name="is-cross-namespace-networking-supported"></a>Çapraz ad alanı ağı desteklenir mi?

Müşteri ve tek tek proje yöneticileri, nesneleri kullanarak `NetworkPolicy` proje bazında çapraz ad alanı ağ oluşturmayı (reddetme dahil) özelleştirebilir.

## <a name="can-an-admin-manage-users-and-quotas"></a>Bir yönetici kullanıcıları ve kotaları yönetebilir mi?

Evet. Azure Red Hat OpenShift yöneticisi, kullanıcı tarafından oluşturulan tüm projelere erişmeye ek olarak kullanıcıları ve kotaları yönetebilir.

## <a name="can-i-restrict-a-cluster-to-only-certain-azure-ad-users"></a>Bir kümeyi yalnızca belirli Azure AD kullanıcılarıyla sınırlayabilir miyim?

Evet. Azure AD Uygulamasını yapılandırarak hangi Azure AD kullanıcılarının kümede oturum açabileceğini kısıtlayabilirsiniz. Ayrıntılar için [bkz: Uygulamanızı bir kullanıcı kümesiyle sınırlandırın](https://docs.microsoft.com/azure/active-directory/develop/howto-restrict-your-app-to-a-set-of-users)

## <a name="can-i-restrict-users-from-creating-projects"></a>Kullanıcıların proje oluşturmalarını kısıtlayabilir miyim?

Evet. Kümenizde Azure Red Hat OpenShift yöneticisi olarak oturum açın ve şu komutu uygulayın:

```
oc adm policy \
    remove-cluster-role-from-group self-provisioner \
    system:authenticated:oauth
```

Daha fazla bilgi için, kendi kendini sağlama devre dışı bırakma yla ilgili OpenShift [belgelerine](https://docs.openshift.com/container-platform/3.11/admin_guide/managing_projects.html#disabling-self-provisioning)bakın.

## <a name="can-a-cluster-have-compute-nodes-across-multiple-azure-regions"></a>Bir kümenin birden çok Azure bölgesinde işlem düğümleri olabilir mi?

Hayır. Azure Kırmızı Şapka OpenShift kümesindeki tüm düğümlerin aynı Azure bölgesinden kaynaklanması gerekir.

## <a name="are-master-and-infrastructure-nodes-abstracted-away-as-they-are-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS) ile olduğu gibi ana ve altyapı düğümleri soyutlandı mı?

Hayır. Küme yöneticisi de dahil olmak üzere tüm kaynaklar müşteri aboneliğinizde çalışır. Bu tür kaynaklar salt okunur kaynak grubuna konur.

## <a name="is-open-service-broker-for-azure-osba-supported"></a>Azure için Open Service Broker (OSBA) desteklendi mi?

Evet. AZURE Red Hat OpenShift ile OSBA'yı kullanabilirsiniz. Daha fazla bilgi [için Azure için Açık Servis Aracı'na](https://github.com/Azure/open-service-broker-azure#openshift-project-template) bakın.

## <a name="i-am-trying-to-peer-into-a-virtual-network-in-a-different-subscription-but-getting-failed-to-get-vnet-cidr-error"></a>Ben farklı bir abonelik sanal bir ağa peer `Failed to get vnet CIDR` çalışıyorum ama hata alıyorum.

Sanal ağa sahip abonelikte, `Microsoft.ContainerService` sağlayıcıyı`az provider register -n Microsoft.ContainerService --wait` 

## <a name="what-is-the-azure-red-hat-openshift-aro-maintenance-process"></a>Azure Red Hat OpenShift (ARO) bakım işlemi nedir?

ARO için üç tür bakım vardır: yükseltmeler, etcd verilerinin yedeklenme ve geri yüklemesi ve bulut sağlayıcısı tarafından başlatılan bakım.

+ Yükseltmeler yazılım yükseltmeleri ve CV'ler içerir. CVE düzeltme çalıştırArak başlangıç `yum update` oluşur ve hemen azaltma sağlar.  Paralel olarak yeni bir görüntü oluşturma gelecek küme oluşturur için oluşturulur.

+ ETCD verilerinin yedeklemesi ve yönetimi, eyleme bağlı olarak küme kapalı kalma süresi gerektirebilecek otomatik bir işlemdir. ETCD veritabanı bir yedeklemeden geri yükleniyorsa, kapalı kalma süresi olacaktır. Biz etcd saatlik yedekleme ve yedekleri son 6 saat korumak.

+ Bulut sağlayıcısı tarafından başlatılan bakım, ağ, depolama ve bölgesel kesintileri içerir. Bakım bulut sağlayıcısına bağlıdır ve sağlayıcı tarafından sağlanan güncelleştirmeleri dayanır.

## <a name="what-is-the-general-upgrade-process"></a>Genel yükseltme işlemi nedir?

Yükseltme çalıştırmak, çalıştırmak için güvenli bir işlem olmalıdır ve küme hizmetlerini kesintiye uğratmamalıdır. SRE, yeni sürümler kullanılabilir olduğunda veya CV'ler olağanüstü olduğunda yükseltme işlemini tetikleyebilir.

Kullanılabilir güncelleştirmeler bir aşama ortamında sınanır ve daha sonra üretim kümelerine uygulanır. Uygulandığında, yeni bir düğüm geçici olarak eklenir ve bölmelerin yineleme sayılarını koruyabilmesi için düğümler dönen bir şekilde güncelleştirilir. En iyi uygulamaları takip etmek, en az ve hiçbir kapalı kalma süresini sağlamaya yardımcı olur.

Bekleyen yükseltme veya güncelleştirmenin önem derecesine bağlı olarak, güncelleştirmelerin hizmetin CVE'ye maruz kalmasını azaltmak için hızlı bir şekilde uygulanabileceği nde işlem farklılık gösterebilir. Yeni bir görüntü eşzamanlı olarak oluşturulur, sınanacaktır ve küme yükseltmesi olarak kullanıma sunulacaktır. Bunun dışında, acil durum ve planlı bakım arasında hiçbir fark yoktur. Planlanan bakım müşteri ile önceden planlanmaz.

Bildirimler, müşteriyle iletişim e-posta yoluyla gönderilebilir.

## <a name="what-about-emergency-vs-planned-maintenance-windows"></a>Peki ya acil durum ve planlı bakım pencereleri?

İki bakım türünü birbirinden ayırmıyoruz. Ekiplerimiz 7/24 mevcuttur ve geleneksel zamanlanmış "mesai dışı" bakım pencerelerini kullanmaz.

## <a name="how-will-host-operating-system-and-openshift-software-be-updated"></a>Barındırma işletim sistemi ve OpenShift yazılımı nasıl güncellenecek?

Ana bilgisayar işletim sistemi ve OpenShift yazılımı, genel yükseltme ve görüntü oluşturma sürecimiz aracılığıyla güncellenir.

## <a name="whats-the-process-to-reboot-the-updated-node"></a>Güncelleştirilmiş düğümü yeniden başlatma işlemi nedir?

Bu, yükseltmenin bir parçası olarak ele alınmalıdır.

## <a name="is-data-stored-in-etcd-encrypted-on-aro"></a>ARO'da veri depolanır mı?

Etcd düzeyinde şifrelenmez. Açma seçeneği şu anda desteklenmiş. OpenShift bu özelliği destekler, ancak yol haritasıüzerinde yapmak için mühendislik çabaları gereklidir. Veriler disk düzeyinde şifrelenir. Daha fazla bilgi için [Datastore Katmanı'ndaki Verileri Şifreleme'ye](https://docs.openshift.com/container-platform/3.11/admin_guide/encrypting_data.html) bakın.

## <a name="can-logs-of-underlying-vms-be-streamed-out-to-a-customer-log-analysis-system"></a>Temel VM'lerin günlükleri müşteri günlüğü analiz sistemine aktarılabilir mi?

Syslog, docker günlükleri, günlük ve dmesg yönetilen hizmet tarafından işlenir ve müşterilere açık değildir.

## <a name="how-can-a-customer-get-access-to-metrics-like-cpumemory-at-the-node-level-to-take-action-to-scale-debug-issues-etc-i-cannot-seem-to-run-kubectl-top-on-an-aro-cluster"></a>Bir müşteri, ölçeklendirme, hata ayıklama sorunları vb. için harekete geçmek için düğüm düzeyinde CPU/bellek gibi ölçümlere nasıl erişebilir? Ben bir ARO kümesi üzerinde çalıştırmak `kubectl top` için görünmüyor olabilir.

Müşteriler komutu `oc adm top nodes` kullanarak veya `kubectl top nodes` müşteri-yönetici kümelemesiyle cpu/bellek ölçümlerine düğüm düzeyinde erişebilir.  Müşteriler ayrıca CPU/Bellek ölçümlerine `pods` komutla `oc adm top pods` veya`kubectl top pods`

## <a name="what-is-the-default-pod-scheduler-configuration-for-aro"></a>ARO için varsayılan pod zamanlayıcı yapılandırması nedir?

ARO, OpenShift'te bulunan varsayılan zamanlayıcıyı kullanır. ARO'da desteklenmeyen birkaç ek mekanizma vardır. Daha fazla ayrıntı için [varsayılan zamanlayıcı belgelerine](https://docs.openshift.com/container-platform/3.11/admin_guide/scheduling/scheduler.html#generic-scheduler) ve [ana zamanlayıcı belgelerine](https://github.com/openshift/openshift-azure/blob/master/pkg/startup/v6/data/master/etc/origin/master/scheduler.json) bakın.

Gelişmiş/Özel zamanlama şu anda desteklenmiş durumda. Daha fazla ayrıntı için [Zamanlama belgelerine](https://docs.openshift.com/container-platform/3.11/admin_guide/scheduling/index.html) bakın.

## <a name="if-we-scale-up-the-deployment-how-do-azure-fault-domains-map-into-pod-placement-to-ensure-all-pods-for-a-service-do-not-get-knocked-out-by-a-failure-in-a-single-fault-domain"></a>Dağıtımı büyütürsek, bir hizmetin tüm bölmelerinin tek bir hata etki alanında bir hata tarafından nakavt edilmemesini sağlamak için Azure hata etki alanları pod yerleşimine nasıl eşlenir?

Azure'da sanal makine ölçek kümeleri kullanırken varsayılan olarak beş hata etki alanı vardır. Bir ölçek kümesindeki her sanal makine örneği bu hata etki alanlarından birine yerleştirilir. Bu, kümedeki bilgi işlem düğümlerine dağıtılan uygulamaların ayrı hata etki alanlarında yerleştirilmesini sağlar.

Daha fazla ayrıntı [için sanal makine ölçeği kümesi için doğru sayıda hata etki alanı seçme](https://docs.microsoft.com//azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-fault-domains) bölümüne bakın.

## <a name="is-there-a-way-to-manage-pod-placement"></a>Pod yerleşimini yönetmenin bir yolu var mı?

Müşteriler düğüm alma ve etiketleri müşteri yöneticisi olarak görüntüleme olanağına sahiptir.  Bu, ölçek kümesindeki herhangi bir VM'yi hedeflemenin bir yolunu sağlar.

Belirli etiketler kullanılırken dikkatli olunmalıdır:

- Ana bilgisayar adı kullanılmamalıdır. Ana bilgisayar adı genellikle yükseltmeleri ve güncellemeleri ile döndürülür ve değiştirmek için garanti edilir.

- Müşterinin belirli etiketler veya dağıtım stratejisi için bir isteği varsa, bu gerçekleştirilebilir, ancak mühendislik çabaları gerektirir ve bugün desteklenmez.

## <a name="what-is-the-maximum-number-of-pods-in-an-aro-cluster-what-is-the-maximum-number-of-pods-per-node-in-aro"></a>Bir ARO kümesindeki en fazla bölme sayısı nedir?ARO'da düğüm başına en fazla bölme sayısı nedir?

 Azure Red Hat OpenShift 3.11, [ARO'nun 20 işlem düğümü sınırına sahip](https://docs.microsoft.com/azure/openshift/openshift-faq#what-cluster-operations-are-available)olmasıyla düğüm başına 50 bölme sınırına sahiptir, böylece bir ARO kümesinde desteklenen maksimum bölme sayısını 50*20 = 1000'e kapsar.

## <a name="can-we-specify-ip-ranges-for-deployment-on-the-private-vnet-avoiding-clashes-with-other-corporate-vnets-once-peered"></a>Özel VNET'te dağıtım için IP aralıkları belirtebilir miyiz?

Azure Red Hat OpenShift, VNET'in eşlemesine ve OpenShift ağının çalışacağı bir VNET'e olanak tanır.

ARO tarafından oluşturulan VNET korunacak ve yapılandırma değişikliklerini kabul etmeyecektir. Bakan VNET müşteri tarafından denetlenir ve aboneliklerinde bulunur.

## <a name="does-the-cluster-reside-in-a-customer-subscription"></a>Küme müşteri aboneliğinde mi yaşıyor? 

Azure Yönetilen Uygulama, müşteri aboneliğiyle birlikte kilitli bir Kaynak Grubunda yaşar. Müşteri bu RG'deki nesneleri görüntüleyebilir, ancak değiştiremez.

## <a name="is-the-sdn-module-configurable"></a>SDN modülü yapılandırılabilir mi?

SDN openshift-ovs-networkilkesidir ve yapılandırılamaz.

## <a name="which-unix-rights-in-iaas-are-available-for-mastersinfraapp-nodes"></a>Masters/Infra/App Düğümleri için hangi UNIX hakları (IaaS'de) kullanılabilir?

Bu teklif için geçerli değildir. Düğüm erişimi yasaktır.

## <a name="which-ocp-rights-do-we-have-cluster-admin-project-admin"></a>Hangi OCP haklarına sahibiz? Küme yöneticisi mi? Proje yöneticisi mi?

Ayrıntılar için Azure Red Hat OpenShift [küme yönetimine genel bakış](https://docs.openshift.com/aro/admin_guide/index.html)bölümüne bakın.

## <a name="which-kind-of-federation-with-ldap"></a>LDAP ile federasyon ne tür?

Bu, Azure AD tümleştirmesi ile elde edilebilir. 

## <a name="is-there-any-element-in-aro-shared-with-other-customers-or-is-everything-independent"></a>ARO'da diğer müşterilerle paylaşılan herhangi bir öğe var mı? Yoksa her şey bağımsız mı?

Her Azure Red Hat OpenShift kümesi belirli bir müşteriye adanmıştır ve müşterinin aboneliği içinde yaşar. 

## <a name="can-we-choose-any-persistent-storage-solution-like-ocs"></a>OCS gibi kalıcı depolama çözümlerini seçebilir miyiz? 

İki depolama sınıfı arasından seçim yapmak için kullanılabilir: Azure Disk ve Azure Dosyası.

## <a name="how-is-a-cluster-updated-including-majors-and-minors-due-to-vulnerabilities"></a>Bir küme nasıl güncelleştirilir (güvenlik açıkları nedeniyle büyükler ve reşit olmayanlar dahil)?

Genel [yükseltme işlemi nedir?](https://docs.microsoft.com/azure/openshift/openshift-faq#what-is-the-general-upgrade-process)

## <a name="what-azure-load-balancer-is-used-by-aro-is-it-standard-or-basic-and-is-it-configurable"></a>ARO tarafından hangi Azure Yük dengeleyicisi kullanılır?Standart mı, Temel mi ve yapılandırılabilir mi?

ARO Standart Azure Yük Dengeleyicisi kullanır ve yapılandırılamaz.

## <a name="can-aro-use-netapp-based-storage"></a>ARO NetApp tabanlı depolama yı kullanabilir mi?

Şu anda desteklenen tek depolama seçeneği Azure Disk ve Azure Dosyası depolama sınıflarıdır. 


