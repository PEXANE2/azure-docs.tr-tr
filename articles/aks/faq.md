---
title: Azure Kubernetes Hizmeti (AKS) için sık sorulan sorular
description: Azure Kubernetes Hizmeti (AKS) ile ilgili sık sorulan bazı soruların yanıtlarını bulun.
ms.topic: conceptual
ms.date: 10/02/2019
ms.openlocfilehash: c4bb4328af5df7f729967c7b249847b2ab098770
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79497759"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) hakkında sık sorulan sorular

Bu makalede, Azure Kubernetes Hizmeti (AKS) ile ilgili sık sorulan sorular ele alçıkmaktadır.

## <a name="which-azure-regions-currently-provide-aks"></a>Şu anda hangi Azure bölgeleri AKS sağlıyor?

Kullanılabilir bölgelerin tam listesi için [AKS bölgelerine ve kullanılabilirliğine][aks-regions]bakın.

## <a name="does-aks-support-node-autoscaling"></a>AKS düğüm otomatikleştiriyi destekliyor mu?

Evet, AKS'de aracı düğümlerini yatay olarak otomatik olarak ölçeklendirme yeteneği şu anda önizlemede kullanılabilir. Bkz. Yönergeler için [AKS'deki uygulama taleplerini karşılamak][aks-cluster-autoscaler] için bir kümeyi otomatik olarak ölçeklendirin. AKS otomatik ölçeklendirme [Kubernetes autoscaler][auto-scaler]dayanmaktadır.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>AKS'yi mevcut sanal ağıma dağıtabilir miyim?

Evet, [gelişmiş ağ özelliğini][aks-advanced-networking]kullanarak bir AKS kümesini varolan bir sanal ağa dağıtabilirsiniz.

## <a name="can-i-limit-who-has-access-to-the-kubernetes-api-server"></a>Kubernetes API sunucusuna kimlerin erişimini sınırlandırabilir miyim?

Evet, [API Server Yetkili IP Aralıkları][api-server-authorized-ip-ranges]kullanarak Kubernetes API sunucusuna erişimi sınırlayabilirsiniz.

## <a name="can-i-make-the-kubernetes-api-server-accessible-only-within-my-virtual-network"></a>Kubernetes API sunucusunu yalnızca sanal ağımda erişilebilir hale getirebilir miyim?

Şu anda değil, ama bu planlanmış. [AKS GitHub repo'sundaki][private-clusters-github-issue]ilerlemeyi izleyebilirsiniz.

## <a name="can-i-have-different-vm-sizes-in-a-single-cluster"></a>Tek bir kümede farklı VM boyutlarına sahip olabilir miyim?

Evet, [birden çok düğüm havuzları][multi-node-pools]oluşturarak AKS kümenizde farklı sanal makine boyutları kullanabilirsiniz.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>GÜVENLIK güncelleştirmeleri AKS aracı düğümlerine uygulanır mı?

Azure, güvenlik yamaları kümesinizdeki Linux düğümlerine gecelik bir programla otomatik olarak uygular. Ancak, bu Linux düğümlerinin gerektiği gibi yeniden başlatılmasını sağlamaktan siz sorumlusunuz. Düğümleri yeniden başlatmak için birkaç seçeneğiniz vardır:

- Azure portalı veya Azure CLI üzerinden el ile.
- AKS kümenizi yükselterek. Küme, [kordon ve drenaj düğümlerini][cordon-drain] otomatik olarak yükseltir ve ardından en son Ubuntu görüntüsü ve yeni bir yama sürümü veya küçük bir Kubernetes sürümüyle yeni bir düğümü çevrimiçi duruma getirir. Daha fazla bilgi için [bir AKS kümesini yükseltme'ye][aks-upgrade]bakın.
- [Kured](https://github.com/weaveworks/kured)kullanarak , Kubernetes için bir açık kaynak yeniden başlatma daemon. Kured, [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) olarak çalışır ve yeniden başlatma nın gerekli olduğunu belirten bir dosyanın varlığını her düğümü izler. Küme genelinde, işletim sistemi yeniden başlatmaları küme yükseltmesi ile aynı [akkor ve boşaltma işlemi][cordon-drain] yle yönetilir.

Kured kullanma hakkında daha fazla bilgi için, [AKS'deki düğümlere güvenlik ve çekirdek güncelleştirmeleri uygula'ya][node-updates-kured]bakın.

### <a name="windows-server-nodes"></a>Windows Server düğümleri

Windows Server düğümleri (şu anda AKS önizlemede), Windows Update otomatik olarak çalışmaz ve en son güncelleştirmeleri uygulamaz. Windows Update sürüm döngüsü ve kendi doğrulama işleminiz etrafında düzenli bir zamanlamada, AKS kümenizde küme ve Windows Server düğüm havuzunda(lar) bir yükseltme gerçekleştirmeniz gerekir. Bu yükseltme işlemi, en son Windows Server görüntüsünü çalıştıran düğümler ve düzeltme etekleri oluşturur ve eski düğümleri kaldırır. Bu işlem hakkında daha fazla bilgi için [AKS'deki düğüm havuzuna yükseltme bölümüne][nodepool-upgrade]bakın.

## <a name="why-are-two-resource-groups-created-with-aks"></a>Neden AKS ile iki kaynak grubu oluşturulur?

AKS, sanal makine ölçek kümeleri, sanal ağlar ve yönetilen diskler de dahil olmak üzere bir dizi Azure altyapı kaynağı üzerine inşa edilmiştir. Bu, AKS tarafından sağlanan yönetilen Kubernetes ortamında Azure platformunun birçok temel özelliğinden yararlanmanızı sağlar. Örneğin, çoğu Azure sanal makine türü doğrudan AKS ile kullanılabilir ve Azure Rezervasyonları bu kaynaklarda otomatik olarak indirim almak için kullanılabilir.

Bu mimariyi etkinleştirmek için, her AKS dağıtımı iki kaynak grubuna yayılır:

1. İlk kaynak grubunu oluşturursunuz. Bu grup yalnızca Kubernetes hizmet kaynağını içerir. AKS kaynak sağlayıcısı dağıtım sırasında ikinci kaynak grubunu otomatik olarak oluşturur. İkinci kaynak grubunun bir örneği *MC_myResourceGroup_myAKSCluster_eastus.* Bu ikinci kaynak grubunun adını nasıl belirtinen hakkında bilgi için sonraki bölüme bakın.
1. *Düğüm kaynak grubu*olarak bilinen ikinci kaynak grubu, kümeyle ilişkili tüm altyapı kaynaklarını içerir. Bu kaynaklar Kubernetes düğüm VM'leri, sanal ağ ve depolamayı içerir. Varsayılan olarak, düğüm kaynak grubunun *MC_myResourceGroup_myAKSCluster_eastus*gibi bir adı vardır. AKS, küme silindiğinde düğüm kaynağını otomatik olarak siler, bu nedenle yalnızca kümenin yaşam döngüsünü paylaşan kaynaklar için kullanılmalıdır.

## <a name="can-i-provide-my-own-name-for-the-aks-node-resource-group"></a>AKS düğüm kaynak grubu için kendi adımı verebilir miyim?

Evet. Varsayılan olarak, AKS düğüm kaynak grubunu *MC_resourcegroupname_clustername_location*adlar, ancak kendi adınızı da sağlayabilirsiniz.

Kendi kaynak grup adınızı belirtmek için [aks önizleme][aks-preview-cli] Azure CLI uzantısı sürümünü *0.3.2* veya sonraki sürüm yükleyin. [Az aks oluşturma][az-aks-create] komutunu kullanarak bir AKS kümesi oluşturduğunuzda, *--düğüm-kaynak grubu* parametresini kullanın ve kaynak grubu için bir ad belirtin. Aks kümesini dağıtmak için [bir Azure Kaynak Yöneticisi şablonu kullanıyorsanız,][aks-rm-template] kaynak grubu adını *düğümKaynak Grubu* özelliğini kullanarak tanımlayabilirsiniz.

* İkincil kaynak grubu, kendi aboneliğinizde Azure kaynak sağlayıcısı tarafından otomatik olarak oluşturulur.
* Özel bir kaynak grubu adı yalnızca küme oluştururken belirtebilirsiniz.

Düğüm kaynak grubuyla çalışırken şunları yapamayacağınızı unutmayın:

* Düğüm kaynak grubu için varolan bir kaynak grubu belirtin.
* Düğüm kaynak grubu için farklı bir abonelik belirtin.
* Küme oluşturulduktan sonra düğüm kaynak grubu adını değiştirin.
* Düğüm kaynak grubundaki yönetilen kaynakların adlarını belirtin.
* Düğüm kaynak grubundaki yönetilen kaynakların etiketlerini değiştirin veya silin. (Sonraki bölümdeki ek bilgilere bakın.)

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-node-resource-group"></a>Düğüm kaynak grubundaki AKS kaynaklarının etiketlerini ve diğer özelliklerini değiştirebilir miyim?

Düğüm kaynak grubundaki Azure tarafından oluşturulan etiketleri ve diğer kaynak özelliklerini değiştirir veya silerseniz, ölçekleme ve yükseltme hataları gibi beklenmeyen sonuçlar elde edebilirsiniz. AKS, özel etiketler oluşturmanıza ve değiştirmenize olanak tanır. Örneğin, bir iş birimi veya maliyet merkezi atamak için özel etiketler oluşturmak veya değiştirmek isteyebilirsiniz. AKS kümesindeki düğüm kaynak grubunun altındaki kaynakları değiştirerek, hizmet düzeyi hedefini (SLO) bozarsınız. Daha fazla bilgi için [bkz: AKS hizmet düzeyi sözleşmesi sunuyor mu?](#does-aks-offer-a-service-level-agreement)

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed"></a>Aks hangi Kubernetes kabul kontrolörlerini destekliyor? Kabul denetleyicileri eklenebilir veya kaldırılabilir mi?

AKS aşağıdaki [kabul kontrolörlerini][admission-controllers]destekler:

- *NamespaceYaşam Döngüsü*
- *LimitRanger*
- *ServiceAccount*
- *Varsayılan StorageClass*
- *DefaultTolerationSeconds*
- *MutasyonA UğramışKabulWebhook*
- *DoğrulamaKabulWebhook*
- *Kaynak Kotası*

Şu anda, AKS'deki kabul denetleyicilerinin listesini değiştiremezsiniz.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Azure Key Vault AKS ile entegre mi?

AKS şu anda Azure Key Vault ile yerel olarak entegre değil. Ancak, [Kubernetes için Azure Key Vault FlexVolume projesi,][keyvault-flexvolume] Kubernetes bölmelerinden Key Vault sırlarına doğrudan entegrasyon sağlar.

## <a name="can-i-run-windows-server-containers-on-aks"></a>Windows Server kapsayıcılarını AKS'de çalıştırabilir miyim?

Evet, Windows Server kapsayıcıları önizlemede kullanılabilir. AKS'de Windows Server kapsayıcılarını çalıştırmak için, Windows Server'ı konuk işletim sistemi olarak çalıştıran bir düğüm havuzu oluşturursunuz. Windows Server kapsayıcıları yalnızca Windows Server 2019'u kullanabilir. Başlamak için bkz: [Windows Server düğüm havuzu olan bir AKS kümesi oluştur.][aks-windows-cli]

Düğüm havuzu için Windows Server desteği, Kubernetes projesinde yukarı Windows Server'ın bir parçası olan bazı sınırlamalar içerir. Bu sınırlamalar hakkında daha fazla bilgi [için, AKS sınırlamalarında Windows Server kapsayıcılarına][aks-windows-limitations]bakın.

## <a name="does-aks-offer-a-service-level-agreement"></a>AKS hizmet düzeyi sözleşmesi sunuyor mu?

Hizmet düzeyi sözleşmesinde (SLA), yayınlanan hizmet düzeyi karşılanmazsa sağlayıcı müşteriye hizmet in maliyetini geri ödemeyi kabul eder. AKS ücretsiz olduğundan, hiçbir maliyet geri ödeme için kullanılabilir, bu nedenle AKS hiçbir resmi SLA vardır. Ancak AKS, Kubernetes API sunucusu için en az yüzde 99,5'lik kullanılabilirliği korumayı amaçlamaktadır.

Kubernetes kontrol düzleminin çalışma süresi ile Azure Sanal Makinelerde çalışan belirli iş yükünüzün kullanılabilirliği anlamına gelen AKS hizmet kullanılabilirliği arasındaki ayrımı tanımak önemlidir. Denetim düzlemi hazır değilse denetim düzlemi kullanılamıyor olsa da, Azure VM'lerinde çalışan küme iş yükleriniz çalışmaya devam edebilir. Azure VM'ler ücretli kaynaklar göz önüne alındığında, bir finansal SLA tarafından desteklenen. Azure VM SLA hakkında daha fazla bilgi ve [Kullanılabilirlik Bölgeleri][availability-zones]gibi özelliklerle kullanılabilirliği nasıl artıracağınız [hakkında daha fazla bilgi için burayı](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) okuyun.

## <a name="why-cant-i-set-maxpods-below-30"></a>Neden maxPods'u 30'un altına ayarlamıyorum?

AKS'de, Azure `maxPods` CLI ve Azure Kaynak Yöneticisi şablonlarını kullanarak küme oluştururken değeri ayarlayabilirsiniz. Ancak, hem Kubenet hem de Azure CNI *en az değer* gerektirir (oluşturma zamanında doğrulanır):

| Ağ Oluşturma | Minimum | Maksimum |
| -- | :--: | :--: |
| Azure CNI | 30 | 250 |
| Kubenet | 30 | 110 |

AKS yönetilen bir hizmet olduğundan, kümenin bir parçası olarak eklentileri ve bölmeleri dağıtıp yönetiyoruz. Geçmişte, kullanıcılar yönetilen bölmelerin çalışması gereken değerden daha düşük bir `maxPods` değer tanımlayabilirdi (örneğin, 30). AKS artık bu formülü kullanarak en az pod sayısını hesaplar: ((maxPods veya (maxPods * vm_count)) yönetilen eklenti bölmeleri > minimum.

Kullanıcılar minimum `maxPods` doğrulamayı geçersiz kılabilir.

## <a name="can-i-apply-azure-reservation-discounts-to-my-aks-agent-nodes"></a>AKS aracı düğümlerime Azure rezervasyon indirimleri uygulayabilir miyim?

AKS aracı düğümleri standart Azure sanal makineleri olarak faturalandırılır, bu nedenle AKS'de kullandığınız VM boyutu için [Azure rezervasyonları][reservation-discounts] satın aldıysanız, bu indirimler otomatik olarak uygulanır.

## <a name="can-i-movemigrate-my-cluster-between-azure-tenants"></a>Kümemi Azure kiracıları arasında taşıyabilir/taşıyabilir miyim?

Komut, `az aks update-credentials` bir AKS kümesini Azure kiracıları arasında taşımak için kullanılabilir. Bir hizmet [sorumlusu güncelleştirmeyi veya oluşturmayı seç'teki](https://docs.microsoft.com/azure/aks/update-credentials) yönergeleri izleyin ve ardından [aks kümesini yeni kimlik bilgileriyle güncelleştirin.](https://docs.microsoft.com/azure/aks/update-credentials#update-aks-cluster-with-new-service-principal-credentials)

## <a name="can-i-movemigrate-my-cluster-between-subscriptions"></a>Kümemi abonelikler arasında taşıyabilir/geçirebilir miyim?

Kümelerin abonelikler arasındaki hareketi şu anda desteklenmiş.

## <a name="can-i-move-my-aks-clusters-from-the-current-azure-subscription-to-another"></a>AKS kümelerimi geçerli azure aboneliğinden başka bir kümeye taşıyabilir miyim? 

AKS kümenizi ve ilişkili kaynaklarıAzure abonelikleri arasında taşıma sı desteklenmez.

## <a name="why-is-my-cluster-delete-taking-so-long"></a>Küme silme misim neden bu kadar uzun sürüyor? 

Çoğu küme, kullanıcı isteği üzerine silinir; bazı durumlarda, özellikle müşterilerin kendi Kaynak Gruplarını getirdikleri veya RG'ler arası görevlerin silinmesi ek zaman alabilir veya başarısız olabilir. Silmelerle ilgili bir sorununuz varsa, RG'de kilit olmadığını, RG dışındaki kaynakların RG ile bağlantısının kesilmediğini, vb. iki kez kontrol edin.

## <a name="if-i-have-pod--deployments-in-state-nodelost-or-unknown-can-i-still-upgrade-my-cluster"></a>'NodeLost' veya 'Unknown' durumunda pod / dağıtımlarım varsa, kümemi yükseltebilir miyim?

Yapabilirsiniz, ancak AKS bunu önermez. Yükseltmeler ideal kümenin durumu bilinen ve sağlıklı olduğunda yapılmalıdır.

## <a name="if-i-have-a-cluster-with-one-or-more-nodes-in-an-unhealthy-state-or-shut-down-can-i-perform-an-upgrade"></a>Sağlıksız bir durumda bir veya daha fazla düğümiçeren bir kümem varsa veya kapatılırsa, yükseltme gerçekleştirebilir miyim?

Hayır, lütfen yükseltmeden önce başarısız durumdaki veya kümeden kaldırılan düğümleri silin/kaldırın.

## <a name="i-ran-a-cluster-delete-but-see-the-error-errno-11001-getaddrinfo-failed"></a>Ben bir küme silme koştu, ama hatayı görmek`[Errno 11001] getaddrinfo failed` 

En yaygın olarak, bunun nedeni bir veya daha fazla Ağ Güvenlik Grubu (NSG) hala kullanımda olan ve kümeyle ilişkili olan kullanıcılardır.  Lütfen kaldırın ve silmeyi yeniden deneyin.

## <a name="i-ran-an-upgrade-but-now-my-pods-are-in-crash-loops-and-readiness-probes-fail"></a>Ben bir yükseltme koştu, ama şimdi benim pods çarpışma döngüleri ve hazırlık sondaları başarısız?

Lütfen servis müdürünün süresinin dolmadığını onaylayın.  Lütfen bakın: [AKS hizmet sorumlusu](https://docs.microsoft.com/azure/aks/kubernetes-service-principal) ve [AKS güncelleştirme kimlik bilgileri.](https://docs.microsoft.com/azure/aks/update-credentials)

## <a name="my-cluster-was-working-but-suddenly-can-not-provision-loadbalancers-mount-pvcs-etc"></a>Benim küme çalışıyordu, ama aniden loadbalancers, mount PVC, vb hükmü olamaz? 

Lütfen servis müdürünün süresinin dolmadığını onaylayın.  Lütfen bakın: [AKS hizmet sorumlusu](https://docs.microsoft.com/azure/aks/kubernetes-service-principal) ve [AKS güncelleştirme kimlik bilgileri.](https://docs.microsoft.com/azure/aks/update-credentials)

## <a name="can-i-use-the-virtual-machine-scale-set-apis-to-scale-manually"></a>Sanal makine ölçeği kümesi API'lerini el ile ölçeklendirmek için kullanabilir miyim?

Hayır, sanal makine ölçeği kümesi API'leri kullanarak ölçek işlemleri desteklenmez. AKS API'lerini`az aks scale`kullanın ( ).

## <a name="can-i-use-virtual-machine-scale-sets-to-manually-scale-to-0-nodes"></a>0 düğümlerine el ile ölçeklendirmek için sanal makine ölçek kümelerini kullanabilir miyim?

Hayır, sanal makine ölçeği kümesi API'leri kullanarak ölçek işlemleri desteklenmez.

## <a name="can-i-stop-or-de-allocate-all-my-vms"></a>Tüm VM'lerimi durdurabilir veya ayırabilir miyim?

AKS böyle bir config dayanacak ve kurtarmak için esneklik mekanizmaları olsa da, bu önerilen bir yapılandırma değildir.

## <a name="can-i-use-custom-vm-extensions"></a>Özel VM uzantıları kullanabilir miyim?

Hiçbir AKS yönetilen bir hizmet değildir ve IaaS kaynaklarının manipülasyonu desteklenmez. Özel bileşenleri vb yüklemek için lütfen Kubernetes API'lerinden ve mekanizmalarından yararlanın. Örneğin, gerekli bileşenleri yüklemek için DaemonSets'den yararlanın.

<!-- LINKS - internal -->

[aks-upgrade]: ./upgrade-cluster.md
[aks-cluster-autoscale]: ./autoscaler.md
[aks-advanced-networking]: ./configure-azure-cni.md
[aks-rbac-aad]: ./azure-ad-integration.md
[node-updates-kured]: node-updates-kured.md
[aks-preview-cli]: /cli/azure/ext/aks-preview/aks
[az-aks-create]: /cli/azure/aks#az-aks-create
[aks-rm-template]: /azure/templates/microsoft.containerservice/2019-06-01/managedclusters
[aks-cluster-autoscaler]: cluster-autoscaler.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[aks-windows-cli]: windows-container-cli.md
[aks-windows-limitations]: windows-node-limitations.md
[reservation-discounts]:../cost-management-billing/reservations/save-compute-costs-reservations.md
[api-server-authorized-ip-ranges]: ./api-server-authorized-ip-ranges.md
[multi-node-pools]: ./use-multiple-node-pools.md
[availability-zones]: ./availability-zones.md

<!-- LINKS - external -->
[aks-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
[private-clusters-github-issue]: https://github.com/Azure/AKS/issues/948
