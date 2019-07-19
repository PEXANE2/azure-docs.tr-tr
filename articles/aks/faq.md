---
title: Azure Kubernetes hizmeti (AKS) için sık sorulan sorular
description: Azure Kubernetes hizmeti (AKS) ile ilgili bazı yaygın soruların yanıtlarını bulun.
services: container-service
author: mlearned
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/08/2019
ms.author: mlearned
ms.openlocfilehash: 554eba87efc56e2dadb3fb2d0cb78cd8b7ea7237
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302720"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Azure Kubernetes hizmeti (AKS) hakkında sık sorulan sorular

Bu makalede, Azure Kubernetes hizmeti (AKS) hakkında sık sorulan sorular ele alınmaktadır.

## <a name="which-azure-regions-currently-provide-aks"></a>Hangi Azure bölgeleri Şu anda AKS sağlıyor?

Kullanılabilir bölgelerin tüm listesi için bkz. [aks bölgeleri ve kullanılabilirliği][aks-regions].

## <a name="does-aks-support-node-autoscaling"></a>AKS düğüm otomatik ölçeklendirmeyi destekliyor mu?

Evet, aracıdaki aracı düğümlerini otomatik olarak ölçeklendirme özelliği şu anda önizlemede kullanılabilir. Bkz. for instructions. AKS autoscaling is based on the [Kubernetes autoscaler][auto-scaler] [aks 'teki uygulama taleplerini karşılamak Için bir kümeyi otomatik olarak ölçeklendirme][aks-cluster-autoscaler] .

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>AKS 'i var olan sanal ağınızdan dağıtabilir miyim?

Evet, [Gelişmiş Ağ özelliğini][aks-advanced-networking]kullanarak bir aks kümesini mevcut bir sanal ağa dağıtabilirsiniz.

## <a name="can-i-limit-who-has-access-to-the-kubernetes-api-server"></a>Kubernetes API sunucusuna kimlerin erişebileceğini sınırlayabilir miyim?

Evet, şu anda önizleme aşamasında olan [API sunucusu YETKILENDIRILMIŞ IP aralıklarını][api-server-authorized-ip-ranges]kullanarak Kubernetes API sunucusuna erişimi sınırlayabilirsiniz.

## <a name="can-i-make-the-kubernetes-api-server-accessible-only-within-my-virtual-network"></a>Kubernetes API sunucusunu yalnızca sanal ağım içinde erişilebilir yapabilir miyim?

Şu anda değil, ancak bu planlanmaktadır. İlerlemeyi [aks GitHub][private-clusters-github-issue]deposunda izleyebilirsiniz.

## <a name="can-i-have-different-vm-sizes-in-a-single-cluster"></a>Tek bir kümede farklı VM boyutlarına sahip olabilir miyim?

Evet, şu anda önizleme aşamasında olan [birden çok düğüm havuzu][multi-node-pools]oluşturarak aks kümenizde farklı sanal makine boyutları kullanabilirsiniz.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Güvenlik güncelleştirmeleri AKS aracı düğümlerine uygulandı mi?

Azure, bir gecelik zamanlamaya göre kümenizdeki Linux düğümlerine otomatik olarak güvenlik düzeltme ekleri uygular. Ancak, bu Linux düğümlerinin gerektiği şekilde yeniden başlatılmasını sağlamaya sorumlusunuz. Düğümleri yeniden başlatmak için çeşitli seçenekleriniz vardır:

- Azure portal veya Azure CLı aracılığıyla el ile.
- AKS kümenizi yükselterek. Küme yükseltmeleri automatically and then bring a new node online with the latest Ubuntu image and a new patch version or a minor Kubernetes version. For more information, see [Upgrade an AKS cluster][aks-upgrade], [düğümleri eş ve boşaltma][cordon-drain] .
- Kubernetes için açık kaynaklı bir yeniden başlatma cini olan [Kured](https://github.com/weaveworks/kured)'yi kullanarak. Kured bir [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) olarak çalışır ve her düğümü, bir yeniden başlatmanın gerekli olduğunu belirten bir dosyanın varlığına izler. Küme genelinde, işletim sistemi yeniden başlatmaları aynı [Cordon ve boşaltma işlemi][cordon-drain] tarafından bir küme yükseltmesi olarak yönetilir.

Kured kullanma hakkında daha fazla bilgi için bkz. [AKS içindeki düğümlere güvenlik ve çekirdek güncelleştirmeleri uygulama][node-updates-kured].

### <a name="windows-server-nodes"></a>Windows Server düğümleri

Windows Server düğümleri için (Şu anda AKS 'de önizlemededir) Windows Update, en son güncelleştirmeleri otomatik olarak çalıştırmaz ve uygulamaz. Windows Update yayın döngüsünün ve kendi doğrulama işleminizin etrafında düzenli bir zamanlamaya göre, AKS kümenizdeki Windows Server düğüm havuzunda bir yükseltme gerçekleştirmeniz gerekir. Bu yükseltme işlemi, en son Windows Server görüntüsünü ve düzeltme eklerini çalıştıran düğümleri oluşturur, daha sonra eski düğümleri kaldırır. Bu işlemle ilgili daha fazla bilgi için bkz. [AKS 'de düğüm havuzunu yükseltme][nodepool-upgrade].

## <a name="why-are-two-resource-groups-created-with-aks"></a>AKS ile neden iki kaynak grubu oluşturulur?

Her bir AKS dağıtımı iki kaynak grubunu kapsar:

1. İlk kaynak grubunu oluşturursunuz. Bu grup yalnızca Kubernetes hizmet kaynağını içerir. AKS kaynak sağlayıcısı, dağıtım sırasında ikinci kaynak grubunu otomatik olarak oluşturur. İkinci kaynak grubuna bir örnek *MC_myResourceGroup_myAKSCluster_eastus*. Bu ikinci kaynak grubunun adını belirtme hakkında daha fazla bilgi için sonraki bölüme bakın.
1. *Düğüm kaynak grubu*olarak bilinen ikinci kaynak grubu, kümeyle ilişkili tüm altyapı kaynaklarını içerir. Bu kaynaklar, Kubernetes düğümü VM 'Leri, sanal ağ ve depolamayı içerir. Varsayılan olarak, düğüm kaynak grubu *MC_myResourceGroup_myAKSCluster_eastus*gibi bir ada sahiptir. AKS, küme her silindiğinde düğüm kaynağını otomatik olarak siler, bu nedenle yalnızca kümenin yaşam döngüsünü paylaşan kaynaklar için kullanılmalıdır.

## <a name="can-i-provide-my-own-name-for-the-aks-node-resource-group"></a>AKS düğümü kaynak grubu için kendi adını verebilir miyim?

Evet. Varsayılan olarak, AKS düğüm grubu *MC_clustername_resourcegroupname_location*olarak ad verir, ancak kendi adınızı de sağlayabilirsiniz.

Kendi kaynak grubu adınızı belirtmek için, [aks-Preview][aks-preview-cli] Azure CLI uzantısı sürüm *0.3.2* veya üstünü yüklemelisiniz. [Az aks Create][az-aks-create] komutunu kullanarak bir aks kümesi oluşturduğunuzda, *--node-Resource-Group* parametresini kullanın ve kaynak grubu için bir ad belirtin. AKS kümesi dağıtmak için [bir Azure Resource Manager şablonu kullanırsanız][aks-rm-template] , *Noderesourcegroup* özelliğini kullanarak kaynak grubu adını tanımlayabilirsiniz.

* İkincil kaynak grubu, kendi aboneliğinizde Azure Kaynak sağlayıcısı tarafından otomatik olarak oluşturulur.
* Yalnızca kümeyi oluştururken özel bir kaynak grubu adı belirtebilirsiniz.

Düğüm kaynak grubuyla çalışırken şunları yapmanız gerektiğini aklınızda bulundurun:

* Düğüm kaynak grubu için mevcut bir kaynak grubu belirtin.
* Düğüm kaynak grubu için farklı bir abonelik belirtin.
* Küme oluşturulduktan sonra düğüm kaynak grubu adını değiştirin.
* Düğüm kaynak grubu içindeki yönetilen kaynakların adlarını belirtin.
* Düğüm kaynak grubu içindeki yönetilen kaynakların etiketlerini değiştirin veya silin. (Sonraki bölümde daha fazla bilgi için bkz.)

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-node-resource-group"></a>Düğüm kaynak grubundaki AKS kaynaklarının etiketlerini ve diğer özelliklerini değiştirebilir miyim?

Düğüm kaynak grubundaki Azure tarafından oluşturulan etiketleri ve diğer kaynak özelliklerini değiştirir veya silerseniz, hataları ölçekleme ve yükseltme gibi beklenmedik sonuçlara ulaşabilirsiniz. AKS özel etiketler oluşturmanıza ve değiştirmenize olanak sağlar. Örneğin, bir iş birimi veya maliyet merkezi atamak için özel etiketler oluşturmak veya değiştirmek isteyebilirsiniz. AKS kümesindeki düğüm kaynak grubu altındaki kaynakları değiştirerek, hizmet düzeyi hedefini (SLO) böedersiniz. Daha fazla bilgi için bkz. [AKS bir hizmet düzeyi sözleşmesi sunuyor mu?](#does-aks-offer-a-service-level-agreement)

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
- *DenyEscalatingExec*
- *Alwaysulmages*

Şu anda AKS 'de giriş denetleyicileri listesini değiştiremezsiniz.

## <a name="is-azure-key-vault-integrated-with-aks"></a>AKS ile tümleşik Azure Key Vault mı?

AKS Şu anda Azure Key Vault ile yerel olarak tümleştirilmiştir. Ancak, [Kubernetes projesi için Azure Key Vault flexvolume][keyvault-flexvolume] , Kubernetes Pod 'den Key Vault gizli anahtarlara doğrudan tümleştirmeyi sağlar.

## <a name="can-i-run-windows-server-containers-on-aks"></a>AKS üzerinde Windows Server kapsayıcıları çalıştırabilir miyim?

Evet, Windows Server kapsayıcıları önizlemede kullanılabilir. AKS 'de Windows Server kapsayıcıları çalıştırmak için, Konuk işletim sistemi olarak Windows Server çalıştıran bir düğüm havuzu oluşturursunuz. Windows Server kapsayıcıları yalnızca Windows Server 2019 kullanabilir. Başlamak için bkz. [Windows Server düğüm havuzu Ile AKS kümesi oluşturma][aks-windows-cli].

Düğüm havuzu için Window Server desteği, Kubernetes projesinde yukarı akış Windows Server 'ın bir parçası olan bazı sınırlamalar içerir. Bu sınırlamalar hakkında daha fazla bilgi için bkz. [AKS kısıtlamalarında Windows Server kapsayıcıları][aks-windows-limitations].

## <a name="does-aks-offer-a-service-level-agreement"></a>AKS bir hizmet düzeyi sözleşmesi sunuyor mu?

Hizmet düzeyi sözleşmesinde (SLA), sağlayıcı, yayımlanan hizmet düzeyi karşılanmazsa müşteriyi hizmetin maliyeti olarak tarafımızca kuruluşlarımız kabul eder. AKS ücretsizdir, tarafımızca kuruluşlarımız için kullanılabilir maliyet yok, bu nedenle AKS 'in resmi SLA 'Sı yok. Ancak, AKS, Kubernetes API sunucusu için en az% 99,5 kullanılabilirlik düzeyini korumak üzere arar.

## <a name="why-cant-i-set-maxpods-below-30"></a>Neden Maxpod 'yi 30 altında ayarlayamıyorum?

Aks 'de, Azure CLI ve Azure Resource Manager `maxPods` şablonlarını kullanarak kümeyi oluştururken değeri ayarlayabilirsiniz. Ancak, hem Kubenet hem de Azure CNı, *en az bir değer* gerektirir (oluşturma sırasında doğrulama):

| Ağ | Minimum | Maksimum |
| -- | :--: | :--: |
| Azure CNı | 30 | 250 |
| Kubernetes kullanan | 30 | 110 |

Aks yönetilen bir hizmet olduğundan, kümenin bir parçası olarak eklentiler ve pod 'leri dağıtıp yönettik. Geçmişte, kullanıcılar yönetilen yığınların çalışması için `maxPods` gereken değerden daha düşük bir değer tanımlayabilir (örneğin, 30). Aks şu formülü kullanarak en az sayıda Pod sayısını hesaplar: ((maxpods veya (maxpods * vm_count)) en düşük > yönetilen eklenti.

Kullanıcılar en düşük `maxPods` doğrulamayı geçersiz kılamaz.

## <a name="can-i-apply-azure-reservation-discounts-to-my-aks-agent-nodes"></a>AKS aracı düğümlerine Azure rezervasyon indirimleri uygulayabilir miyim?

AKS aracı düğümleri standart Azure sanal makineleri olarak faturalandırılır, bu nedenle AKS 'de kullandığınız VM boyutu için [Azure ayırmaları][reservation-discounts] satın aldıysanız, bu indirimler otomatik olarak uygulanır.

## <a name="can-i-movemigrate-my-cluster-between-azure-tenants"></a>Kümemi Azure kiracılar arasında taşıyabilir/geçirebilir miyim?

Bu `az aks update-credentials` komut, bir aks kümesini Azure kiracılar arasında taşımak için kullanılabilir. [Hizmet sorumlusunu güncelleştirmek veya oluşturmak Için seçin](https://docs.microsoft.com/azure/aks/update-credentials) ' deki yönergeleri izleyin ve ardından [aks kümesini yeni kimlik bilgileriyle güncelleştirin](https://docs.microsoft.com/azure/aks/update-credentials#update-aks-cluster-with-new-credentials).

## <a name="can-i-movemigrate-my-cluster-between-subscriptions"></a>Abonelikler arasında kümemi taşıyabilir/geçirebilir miyim?

Abonelikler arasında küme hareketi şu anda desteklenmiyor.

<!-- LINKS - internal -->

[aks-regions]: ./quotas-skus-regions.md#region-availability
[aks-upgrade]: ./upgrade-cluster.md
[aks-cluster-autoscale]: ./autoscaler.md
[virtual-kubelet]: virtual-kubelet.md
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
[reservation-discounts]: ../billing/billing-save-compute-costs-reservations.md
[api-server-authorized-ip-ranges]: ./api-server-authorized-ip-ranges.md
[multi-node-pools]: ./use-multiple-node-pools.md

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
[private-clusters-github-issue]: https://github.com/Azure/AKS/issues/948
