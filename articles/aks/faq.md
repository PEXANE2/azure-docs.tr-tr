---
title: Azure Kubernetes hizmeti (AKS) için sık sorulan sorular
description: Azure Kubernetes hizmeti (AKS) ile ilgili bazı yaygın soruların yanıtlarını bulun.
ms.topic: conceptual
ms.date: 05/14/2020
ms.openlocfilehash: 136f79df43bcc1730f187980df8726d693390faa
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84300935"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) hakkında sık sorulan sorular

Bu makalede, Azure Kubernetes hizmeti (AKS) hakkında sık sorulan sorular ele alınmaktadır.

## <a name="which-azure-regions-currently-provide-aks"></a>Hangi Azure bölgeleri Şu anda AKS sağlıyor?

Kullanılabilir bölgelerin tüm listesi için bkz. [aks bölgeleri ve kullanılabilirliği][aks-regions].

## <a name="can-i-spread-an-aks-cluster-across-regions"></a>Bir AKS kümesini bölgeler arasında yayılabilir miyim?

Hayır. AKS kümeleri bölgesel kaynaklardır ve bölgelere yayılamaz. Birden çok bölge içeren bir mimari oluşturma hakkında yönergeler için bkz. [iş sürekliliği ve olağanüstü durum kurtarma için en iyi uygulamalar][bcdr-bestpractices] .

## <a name="can-i-spread-an-aks-cluster-across-availability-zones"></a>Kullanılabilirlik alanları arasında bir AKS kümesini yayı yapabilir miyim?

Evet. Bir AKS kümesini, [bunları destekleyen bölgelerde][az-regions]bir veya daha fazla [kullanılabilirlik][availability-zones] alanında dağıtabilirsiniz.

## <a name="can-i-limit-who-has-access-to-the-kubernetes-api-server"></a>Kubernetes API sunucusuna kimlerin erişebileceğini sınırlayabilir miyim?

Evet. API sunucusuna erişimi kısıtlamak için iki seçenek vardır:

- API sunucusu için genel bir uç nokta sürdürmek istiyorsanız ancak güvenilen bir IP aralığı kümesine erişimi kısıtlamak istiyorsanız, [API sunucusu YETKILENDIRILMIŞ IP aralıklarını][api-server-authorized-ip-ranges] kullanın.
- API sunucusunu *yalnızca* sanal ağınızın içinden erişilebilir olacak şekilde sınırlamak istiyorsanız [özel bir küme][private-clusters] kullanın.

## <a name="can-i-have-different-vm-sizes-in-a-single-cluster"></a>Tek bir kümede farklı VM boyutlarına sahip olabilir miyim?

Evet, [birden çok düğüm havuzu][multi-node-pools]oluşturarak aks kümenizde farklı sanal makine boyutları kullanabilirsiniz.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Güvenlik güncelleştirmeleri AKS aracı düğümlerine uygulandı mi?

Azure, bir gecelik zamanlamaya göre kümenizdeki Linux düğümlerine otomatik olarak güvenlik düzeltme ekleri uygular. Ancak, bu Linux düğümlerinin gerektiği şekilde yeniden başlatılmasını sağlamaya sorumlusunuz. Düğümleri yeniden başlatmak için çeşitli seçenekleriniz vardır:

- Azure portal veya Azure CLı aracılığıyla el ile.
- AKS kümenizi yükselterek. Küme yükseltmeleri, [düğümleri][cordon-drain] otomatik olarak kaldırır ve yeni bir düğümü en son Ubuntu görüntüsü ve yeni bir yama sürümü ya da bir Ikincil Kubernetes sürümü ile çevrimiçi duruma getirir. Daha fazla bilgi için bkz. [AKS kümesini yükseltme][aks-upgrade].
- Kubernetes için açık kaynaklı bir yeniden başlatma cini olan [Kured](https://github.com/weaveworks/kured)'yi kullanarak. Kured bir [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) olarak çalışır ve her düğümü, bir yeniden başlatmanın gerekli olduğunu belirten bir dosyanın varlığına izler. Küme genelinde, işletim sistemi yeniden başlatmaları aynı [Cordon ve boşaltma işlemi][cordon-drain] tarafından bir küme yükseltmesi olarak yönetilir.

Kured kullanma hakkında daha fazla bilgi için bkz. [AKS içindeki düğümlere güvenlik ve çekirdek güncelleştirmeleri uygulama][node-updates-kured].

### <a name="windows-server-nodes"></a>Windows Server düğümleri

Windows Server düğümleri için Windows Update otomatik olarak çalıştırılmaz ve en son güncelleştirmeleri uygulamaz. Windows Update yayın döngüsünün ve kendi doğrulama işleminizin etrafında düzenli bir zamanlamaya göre, küme üzerinde ve AKS kümenizdeki Windows Server düğüm havuzunda bir yükseltme gerçekleştirmeniz gerekir. Bu yükseltme işlemi, en son Windows Server görüntüsünü ve düzeltme eklerini çalıştıran düğümleri oluşturur, daha sonra eski düğümleri kaldırır. Bu işlemle ilgili daha fazla bilgi için bkz. [AKS 'de düğüm havuzunu yükseltme][nodepool-upgrade].

## <a name="why-are-two-resource-groups-created-with-aks"></a>AKS ile neden iki kaynak grubu oluşturulur?

AKS, sanal makine ölçek kümeleri, sanal ağlar ve yönetilen diskler dahil olmak üzere bir dizi Azure altyapı kaynağı üzerinde oluşturulur. Bu, AKS tarafından sunulan yönetilen Kubernetes ortamında Azure platformunun birçok temel özelliği avantajlarından yararlanmanızı sağlar. Örneğin, çoğu Azure sanal makine türü doğrudan AKS ile kullanılabilir ve Azure ayırmaları bu kaynaklara otomatik olarak iskontolar almak için kullanılabilir.

Bu mimariyi etkinleştirmek için, her bir AKS dağıtımı iki kaynak grubuna yaymıştır:

1. İlk kaynak grubunu oluşturursunuz. Bu grup yalnızca Kubernetes hizmet kaynağını içerir. AKS kaynak sağlayıcısı, dağıtım sırasında ikinci kaynak grubunu otomatik olarak oluşturur. İkinci kaynak grubuna bir örnek *MC_myResourceGroup_myAKSCluster_eastus*. Bu ikinci kaynak grubunun adını belirtme hakkında daha fazla bilgi için sonraki bölüme bakın.
1. *Düğüm kaynak grubu*olarak bilinen ikinci kaynak grubu, kümeyle ilişkili tüm altyapı kaynaklarını içerir. Bu kaynaklar, Kubernetes düğümü VM 'Leri, sanal ağ ve depolamayı içerir. Varsayılan olarak, düğüm kaynak grubunun *MC_myResourceGroup_myAKSCluster_eastus*gibi bir adı vardır. AKS, küme her silindiğinde düğüm kaynağını otomatik olarak siler, bu nedenle yalnızca kümenin yaşam döngüsünü paylaşan kaynaklar için kullanılmalıdır.

## <a name="can-i-provide-my-own-name-for-the-aks-node-resource-group"></a>AKS düğümü kaynak grubu için kendi adını verebilir miyim?

Evet. Varsayılan olarak, AKS, düğüm kaynak grubunu *MC_resourcegroupname_clustername_location*olarak adlandırın, ancak kendi adınızı de sağlayabilirsiniz.

Kendi kaynak grubu adınızı belirtmek için, [aks-Preview][aks-preview-cli] Azure CLI uzantısı sürüm *0.3.2* veya üstünü yüklemelisiniz. [Az aks Create][az-aks-create] komutunu kullanarak bir aks kümesi oluşturduğunuzda, *--node-Resource-Group* parametresini kullanın ve kaynak grubu için bir ad belirtin. AKS kümesi dağıtmak için [bir Azure Resource Manager şablonu kullanırsanız][aks-rm-template] , *Noderesourcegroup* özelliğini kullanarak kaynak grubu adını tanımlayabilirsiniz.

* İkincil kaynak grubu, kendi aboneliğinizde Azure Kaynak sağlayıcısı tarafından otomatik olarak oluşturulur.
* Yalnızca kümeyi oluştururken özel bir kaynak grubu adı belirtebilirsiniz.

Düğüm kaynak grubuyla çalışırken şunları yapmanız gerektiğini aklınızda bulundurun:

* Düğüm kaynak grubu için mevcut bir kaynak grubu belirtin.
* Düğüm kaynak grubu için farklı bir abonelik belirtin.
* Küme oluşturulduktan sonra düğüm kaynak grubu adını değiştirin.
* Düğüm kaynak grubu içindeki yönetilen kaynakların adlarını belirtin.
* Düğüm kaynak grubu içinde yönetilen kaynakların Azure tarafından oluşturulan etiketlerini değiştirin veya silin. (Sonraki bölümde daha fazla bilgi için bkz.)

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-node-resource-group"></a>Düğüm kaynak grubundaki AKS kaynaklarının etiketlerini ve diğer özelliklerini değiştirebilir miyim?

Düğüm kaynak grubundaki Azure tarafından oluşturulan etiketleri ve diğer kaynak özelliklerini değiştirir veya silerseniz, hataları ölçekleme ve yükseltme gibi beklenmedik sonuçlara ulaşabilirsiniz. AKS, son kullanıcılar tarafından oluşturulan özel etiketler oluşturup değiştirmenize olanak sağlar. Örneğin, bir iş birimi veya maliyet merkezi atamak için özel etiketler oluşturmak veya değiştirmek isteyebilirsiniz. Bu, yönetilen kaynak grubundaki bir kapsamla Azure Ilkeleri oluşturularak elde edilebilir.

Ancak, AKS kümesindeki düğüm kaynak grubu altındaki kaynaklarda bulunan **Azure tarafından oluşturulan etiketlerin** değiştirilmesi, hizmet düzeyi HEDEFINI (SLO) kesen desteklenmeyen bir eylemdir. Daha fazla bilgi için bkz. [AKS bir hizmet düzeyi sözleşmesi sunuyor mu?](#does-aks-offer-a-service-level-agreement)

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed"></a>Kubernetes giriş denetleyicileri AKS 'i destekliyor? Giriş denetleyicileri eklenebilir mi, kaldırılabilir mi?

AKS aşağıdaki [giriş denetleyicilerini][admission-controllers]destekler:

- *Namespaceyaşam döngüsü*
- *LimitRanger*
- *Çalıştırıp*
- *DefaultStorageClass*
- *Defaulttoleranationseconds*
- *Değiştirici Tingadmissionweb kancası*
- *ValidatingAdmissionWebhook*
- *ResourceQuota*

Şu anda AKS 'de giriş denetleyicileri listesini değiştiremezsiniz.

## <a name="can-i-use-admission-controller-webhooks-on-aks"></a>AKS 'de giriş denetleyicisi Web kancalarını kullanabilir miyim?

Evet, AKS üzerinde giriş denetleyicisi Web kancalarını kullanabilirsiniz. **Denetim düzlemi etiketiyle** işaretlenen iç aks ad alanlarını dışlayamazsınız önerilir. Örneğin, aşağıdaki Web kancası yapılandırmasına ekleyerek:

```
namespaceSelector:
    matchExpressions:
    - key: control-plane
      operator: DoesNotExist
```

## <a name="can-admission-controller-webhooks-impact-kube-system-and-internal-aks-namespaces"></a>Erişim Denetleyicisi Web kancaları, kuin sistemi ve iç AKS ad alanlarını etkileyebilir mi?

Sistemin kararlılığını korumak ve özel giriş denetleyicilerinin kuin sisteminde iç Hizmetleri etkilemeleri önlemesini engellemek için, ad alanı AKS 'leri, kuin sistemi ve AKS iç ad alanlarını otomatik olarak dışlayan bir **Admissions Enforcer**içerir. Bu hizmet, özel giriş denetleyicilerinin kuin sisteminde çalışan hizmetleri etkilememesini sağlar.

Özel giriş Web kancasının kapsamında olmasını istediğiniz bir şeyin Kuto sistemine dağıtılması için kritik bir kullanım örneğine sahipseniz, Admissions Enforcer bunu yok sayabilmesi için aşağıdaki etiketi veya ek açıklamayı ekleyebilirsiniz.

Etiket: ```"admissions.enforcer/disabled": "true"``` veya ek açıklama:```"admissions.enforcer/disabled": true```

## <a name="is-azure-key-vault-integrated-with-aks"></a>AKS ile tümleşik Azure Key Vault mı?

AKS Şu anda Azure Key Vault ile yerel olarak tümleştirilmiştir. Ancak, [CSI Gizli dizileri için Azure Key Vault sağlayıcısı][csi-driver] , Kubernetes Pod 'den Key Vault gizli anahtarlara doğrudan tümleştirmeyi sağlar.

## <a name="can-i-run-windows-server-containers-on-aks"></a>AKS üzerinde Windows Server kapsayıcıları çalıştırabilir miyim?

Evet, Windows Server kapsayıcıları AKS 'de kullanılabilir. AKS 'de Windows Server kapsayıcıları çalıştırmak için, Konuk işletim sistemi olarak Windows Server çalıştıran bir düğüm havuzu oluşturursunuz. Windows Server kapsayıcıları yalnızca Windows Server 2019 kullanabilir. Başlamak için bkz. [Windows Server düğüm havuzu Ile AKS kümesi oluşturma][aks-windows-cli].

Düğüm havuzu için Windows Server desteği, Kubernetes projesinde yukarı akış Windows Server 'ın bir parçası olan bazı sınırlamalar içerir. Bu sınırlamalar hakkında daha fazla bilgi için bkz. [AKS kısıtlamalarında Windows Server kapsayıcıları][aks-windows-limitations].

## <a name="does-aks-offer-a-service-level-agreement"></a>AKS bir hizmet düzeyi sözleşmesi sunuyor mu?

AKS, [çalışma süresi SLA 'sı][uptime-sla]ile isteğe bağlı ekleme ÖZELLIĞI olarak SLA garantisi sağlar.

## <a name="can-i-apply-azure-reservation-discounts-to-my-aks-agent-nodes"></a>AKS aracı düğümlerine Azure rezervasyon indirimleri uygulayabilir miyim?

AKS aracı düğümleri standart Azure sanal makineleri olarak faturalandırılır, bu nedenle AKS 'de kullandığınız VM boyutu için [Azure ayırmaları][reservation-discounts] satın aldıysanız, bu indirimler otomatik olarak uygulanır.

## <a name="can-i-movemigrate-my-cluster-between-azure-tenants"></a>Kümemi Azure kiracılar arasında taşıyabilir/geçirebilir miyim?

Bu `az aks update-credentials` komut, BIR AKS kümesini Azure kiracılar arasında taşımak için kullanılabilir. [Hizmet sorumlusunu güncelleştirmek veya oluşturmak Için seçin](https://docs.microsoft.com/azure/aks/update-credentials) ' deki yönergeleri izleyin ve ardından [aks kümesini yeni kimlik bilgileriyle güncelleştirin](https://docs.microsoft.com/azure/aks/update-credentials#update-aks-cluster-with-new-service-principal-credentials).

## <a name="can-i-movemigrate-my-cluster-between-subscriptions"></a>Abonelikler arasında kümemi taşıyabilir/geçirebilir miyim?

Abonelikler arasında küme hareketi şu anda desteklenmiyor.

## <a name="can-i-move-my-aks-clusters-from-the-current-azure-subscription-to-another"></a>AKS kümelerimi geçerli Azure aboneliğinden diğerine taşıyabilir miyim? 

AKS kümenizi ve bunun ilişkili kaynaklarını Azure abonelikleri arasında taşıma desteklenmez.

## <a name="why-is-my-cluster-delete-taking-so-long"></a>Kümemin neden bu kadar uzun sürüyor? 

Çoğu küme Kullanıcı isteğiyle silinir; Bazı durumlarda, özellikle de müşteriler kendi kaynak grubunu getiriyor ya da faaliyetsiz görevler silme işlemleri ek zaman alabilir veya başarısız olabilir. Silme işlemiyle ilgili bir sorununuz varsa, RG üzerinde kilit olmadığından emin olun, RG 'nin dışındaki tüm kaynakların RG, vb. ile ilişkisi olduğunu kontrol edin.

## <a name="if-i-have-pod--deployments-in-state-nodelost-or-unknown-can-i-still-upgrade-my-cluster"></a>' Nodekaybedildi ' veya ' bilinmiyor ' durumunda Pod/dağıtımlar varsa, kümemi yükseltebilirim?

, Ancak AKS bunun için önerilmez. Yükseltme, kümenin durumu bilindiğinde ve sağlıklı olduğunda gerçekleştirilmesi ideal olmalıdır.

## <a name="if-i-have-a-cluster-with-one-or-more-nodes-in-an-unhealthy-state-or-shut-down-can-i-perform-an-upgrade"></a>Sağlıksız bir durumda bir veya daha fazla düğümü olan bir kümeniz varsa veya bilgisayarı kapatırsanız, bir yükseltme gerçekleştirebilir miyim?

Hayır, yükseltmeden önce lütfen başarısız durumundaki düğümleri silin/kaldırın veya kümeden kaldırın.

## <a name="i-ran-a-cluster-delete-but-see-the-error-errno-11001-getaddrinfo-failed"></a>Bir küme silme çalıştırdım, ancak hataya bakın`[Errno 11001] getaddrinfo failed` 

En yaygın olarak, bunun nedeni bir veya daha fazla ağ güvenlik grubu (NSG) hala kullanımda ve kümeyle ilişkili olan kullanıcılardır.  Lütfen bunları kaldırın ve silmeyi yeniden deneyin.

## <a name="i-ran-an-upgrade-but-now-my-pods-are-in-crash-loops-and-readiness-probes-fail"></a>Bir yükseltme çalıştırdım, ancak şu anda My Pod çökme döngülerinde ve hazırlık araştırmaları başarısız oldu mu?

Lütfen hizmet sorumlunun süresi dolmadığından emin olun.  Lütfen bkz: [aks hizmet sorumlusu](https://docs.microsoft.com/azure/aks/kubernetes-service-principal) ve [aks güncelleştirme kimlik bilgileri](https://docs.microsoft.com/azure/aks/update-credentials).

## <a name="my-cluster-was-working-but-suddenly-cannot-provision-loadbalancers-mount-pvcs-etc"></a>Kümem çalışıyor, ancak aniden LoadBalancers, bağlama PVC 'leri vb. sağlayamaz. 

Lütfen hizmet sorumlunun süresi dolmadığından emin olun.  Lütfen bkz: [aks hizmet sorumlusu](https://docs.microsoft.com/azure/aks/kubernetes-service-principal) ve [aks güncelleştirme kimlik bilgileri](https://docs.microsoft.com/azure/aks/update-credentials).

## <a name="can-i-use-the-virtual-machine-scale-set-apis-to-scale-manually"></a>Sanal makine ölçek kümesi API 'Lerini el ile ölçeklendirmek için kullanabilir miyim?

Hayır, sanal makine ölçek kümesi API 'Lerini kullanarak ölçeklendirme işlemleri desteklenmez. AKS API 'Lerini ( `az aks scale` ) kullanın.

## <a name="can-i-use-virtual-machine-scale-sets-to-manually-scale-to-0-nodes"></a>Sanal makine ölçek kümelerini, 0 düğüme el ile ölçeklendirmek için kullanabilir miyim?

Hayır, sanal makine ölçek kümesi API 'Lerini kullanarak ölçeklendirme işlemleri desteklenmez.

## <a name="can-i-stop-or-de-allocate-all-my-vms"></a>Tüm VM 'lerimi durdurabilir veya serbest bırakabilirsiniz miyim?

AKS, bu tür bir yapılandırmaya ve kurtarmaya yönelik esnekliği mekanizmalarına sahip olsa da önerilen bir yapılandırma değildir.

## <a name="can-i-use-custom-vm-extensions"></a>Özel VM uzantılarını kullanabilir miyim?

Hiçbir AKS yönetilen bir hizmettir ve IaaS kaynaklarını düzenleme desteklenmez. Özel bileşenleri yüklemek için, vb. Lütfen Kubernetes API 'Leri ve mekanizmalarından yararlanın. Örneğin, gerekli bileşenleri yüklemek için DaemonSets 'ten yararlanın.

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
[private-clusters]: ./private-clusters.md
[bcdr-bestpractices]: ./operator-best-practices-multi-region.md#plan-for-multiregion-deployment
[availability-zones]: ./availability-zones.md
[az-regions]: ../availability-zones/az-region.md
[uptime-sla]: ./uptime-sla.md

<!-- LINKS - external -->
[aks-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[private-clusters-github-issue]: https://github.com/Azure/AKS/issues/948
[csi-driver]: https://github.com/Azure/secrets-store-csi-driver-provider-azure
[vm-sla]: https://azure.microsoft.com/support/legal/sla/virtual-machines/