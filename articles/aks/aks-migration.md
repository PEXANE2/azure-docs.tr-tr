---
title: Azure Kubernetes Hizmetine (AKS) geçiş
description: Azure Kubernetes Hizmetine (AKS) geçirin.
services: container-service
ms.topic: article
ms.date: 02/25/2020
ms.custom: mvc
ms.openlocfilehash: 8315560c679f9807715af14dc315fa3000be0472
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77624819"
---
# <a name="migrate-to-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetine (AKS) geçiş

Bu makale, Azure Kubernetes Hizmetine (AKS) başarılı bir geçiş planlamanıza ve yürütmenize yardımcı olur. Önemli kararlar alabildiğinize yardımcı olmak için bu kılavuz, AKS için önerilen geçerli yapılandırmaiçin ayrıntılar sağlar. Bu makale her senaryoyu kapsamaz ve uygun olduğunda, makale başarılı bir geçiş planlamak için daha ayrıntılı bilgilere bağlantılar içerir.

Bu belge, aşağıdaki senaryoları desteklemeye yardımcı olmak için kullanılabilir:

* [Kullanılabilirlik Kümeleri](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) tarafından desteklenen bir AKS Kümesini [Sanal Makine Ölçeği Kümelerine](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview) geçirme
* [Standart SKU yük dengeleyicisi](https://docs.microsoft.com/azure/aks/load-balancer-standard) kullanmak için bir AKS kümesini geçirme
* [Azure Konteyner Hizmetinden (ACS) geçiş - 31 Ocak 2020'de](https://azure.microsoft.com/updates/azure-container-service-will-retire-on-january-31-2020/) AKS'den emekli olma
* [AKS motorundan](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1908) AKS'ye geçiş
* Azure tabanlı olmayan Kubernetes kümelerinden AKS'ye geçiş

Geçiş yaparken, hedef Kubernetes sürümünün AKS için desteklenen pencere içinde olduğundan emin olun. Eski bir sürüm kullanıyorsanız, desteklenen aralık içinde olmayabilir ve AKS tarafından desteklenecek sürümleri yükseltme gerektirir. Daha fazla bilgi için [AKS destekli Kubernetes sürümlerine](https://docs.microsoft.com/azure/aks/supported-kubernetes-versions) bakın.

Kubernetes'in daha yeni bir sürümüne geçiş yapıyorsanız, [Kubernetes sürümünü ve sürüm çarpık destek ilkesini](https://kubernetes.io/docs/setup/release/version-skew-policy/#supported-versions)gözden geçirin.

Senaryonuza bağlı olarak, çeşitli açık kaynak araçları geçişinize yardımcı olabilir:

* [Velero](https://velero.io/) (Kubernetes 1.7+gerektirir)
* [Azure Kube CLI uzantısı](https://github.com/yaron2/azure-kube-cli)
* [ReShifter](https://github.com/mhausenblas/reshifter)

Bu makalede, geçiş ayrıntılarını özetleyeceğiz:

> [!div class="checklist"]
> * Standart Yük Dengeleyici ve Sanal Makine Ölçek Setleri ile AKS
> * Mevcut ekli Azure Hizmetleri
> * Geçerli kotaları sağlama
> * Yüksek Kullanılabilirlik ve iş sürekliliği
> * Stateless uygulamaları için dikkat edilmesi gerekenler
> * Devlet uygulamaları için dikkat edilmesi gerekenler
> * Küme yapılandırmanızın dağıtımı

## <a name="aks-with-standard-load-balancer-and-virtual-machine-scale-sets"></a>Standart Yük Dengeleyici ve Sanal Makine Ölçek Setleri ile AKS

AKS, daha düşük yönetim yükü ile benzersiz yetenekler sunan yönetilen bir hizmettir. Yönetilen bir hizmet olmanın bir sonucu olarak, AKS'nin desteklediği bölgeler kümesinden seçim yapmalısınız. [regions](https://docs.microsoft.com/azure/aks/quotas-skus-regions) Varolan kümenizden AKS'ye geçiş, varolan uygulamalarınızın AKS yönetilen kontrol düzleminde sağlıklı kalması için değiştirilmesini gerektirebilir.

Birden fazla düğüm havuzları, [Kullanılabilirlik Bölgeleri,](https://docs.microsoft.com/azure/availability-zones/az-overview) [Yetkili IP aralıkları,](https://docs.microsoft.com/azure/aks/api-server-authorized-ip-ranges)Cluster [Autoscaler,](https://docs.microsoft.com/azure/aks/cluster-autoscaler) [AKS için Azure Politikası](https://docs.microsoft.com/azure/governance/policy/concepts/rego-for-aks)ve piyasaya sürüldükleri gibi diğer yeni özellikler elde etmenizi sağlamak için Sanal Makine Ölçek [Kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets) ve Azure Standart Yük [Dengeleyicisi](https://docs.microsoft.com/azure/aks/load-balancer-standard) tarafından desteklenen AKS kümelerini kullanmanızı öneririz. [multiple node pools](https://docs.microsoft.com/azure/aks/use-multiple-node-pools)

[Sanal Makine Kullanılabilirlik Kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/availability#availability-sets) tarafından desteklenen AKS kümeleri bu özelliklerin çoğu için destek eksikliği.

Aşağıdaki örnek, sanal makine ölçeği kümesi tarafından desteklenen tek düğüm havuzuna sahip bir AKS kümesi oluşturur. Standart yük dengeleyicisi kullanır. Ayrıca küme için düğüm havuzunda küme otomatik ölçeklendirici sağlar ve en az *1* ve en fazla *3* düğüm ayarlar:

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location eastus

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 1 \
  --vm-set-type VirtualMachineScaleSets \
  --load-balancer-sku standard \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

## <a name="existing-attached-azure-services"></a>Mevcut ekli Azure Hizmetleri

Kümeleri geçiş yaparken harici Azure hizmetleri eklenmiş olabilirsiniz. Bunlar kaynak rekreasyon gerektirmez, ancak işlevselliği korumak için önceki yeni kümelerden yeni kümelere bağlantıları güncelleştirmeyi gerektirir.

* Azure Container Kayıt Defteri
* Log Analytics
* Application Insights
* Traffic Manager
* Depolama Hesabı
* Dış Veritabanları

## <a name="ensure-valid-quotas"></a>Geçerli kotaları sağlama

Geçiş sırasında aboneliğinize ek sanal makineler dağıtılacağına göre, kotalarınızın ve limitlerinizin bu kaynaklar için yeterli olduğunu doğrulamanız gerekir. [VCPU kotasında](https://docs.microsoft.com/azure/azure-portal/supportability/per-vm-quota-requests)artış talep etmeniz gerekebilir.

IP'leri tüketmediğinizden emin olmak için [Ağ kotaları](https://docs.microsoft.com/azure/azure-portal/supportability/networking-quota-requests) için bir artış istemeniz gerekebilir. Ek bilgi [için AKS için ağ ve IP aralıklarına](https://docs.microsoft.com/azure/aks/configure-kubenet) bakın.

Daha fazla bilgi için [Azure abonelik ve hizmet sınırlarına](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)bakın. Geçerli kotalarınızı kontrol etmek için Azure portalında [abonelikler bıçağına](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)gidin, aboneliğinizi seçin ve ardından **Kullanım + kotalarını**seçin.

## <a name="high-availability-and-business-continuity"></a>Yüksek Kullanılabilirlik ve İş Sürekliliği

Uygulamanız kapalı kalma süresini kaldıramıyorsa, yüksek kullanılabilirlik geçiş senaryoları için en iyi uygulamaları izlemeniz gerekir.  Karmaşık iş sürekliliği planlaması, olağanüstü durum kurtarma ve çalışma süresini en üst düzeye çıkarmak için en iyi uygulamalar bu belgenin kapsamı dışındadır.  Daha fazla bilgi edinmek [için Azure Kubernetes Hizmeti'nde (AKS) iş sürekliliği ve olağanüstü durum kurtarma yla ilgili en iyi uygulamalar](https://docs.microsoft.com/azure/aks/operator-best-practices-multi-region) hakkında daha fazla bilgi edinin.

Karmaşık uygulamalar için, genellikle aynı anda değil, zaman içinde geçiş yaparız. Bu, eski ve yeni ortamların ağ üzerinden iletişim kurması gerekebileceği anlamına gelir. Daha önce iletişim `ClusterIP` kurmak için hizmetleri kullanan uygulamaların `LoadBalancer` tür olarak açıkta kalması ve uygun şekilde güvenli hale alınması gerekebilir.

Geçişi tamamlamak için istemcileri AKS'de çalışan yeni hizmetlere işaret etmek istersiniz. DNS'yi güncelleyerek AKS kümenizin önünde bulunan Yük Dengeleyicisini işaret ederek trafiği yeniden yönlendirmenizi öneririz.

[Azure Trafik Yöneticisi,](https://docs.microsoft.com/azure/traffic-manager/) müşterileri istenilen Kubernetes kümesine ve uygulama örneğine yönlendirebilir.  Trafik Yöneticisi, ağ trafiğini bölgeler arasında dağıtabilen DNS tabanlı bir trafik yük dengeleyicisidir.  En iyi performans ve artıklık için, AKS kümenize gitmeden önce tüm uygulama trafiğini Trafik Yöneticisi'ne yönlendirin.  Çok kümeli dağıtımda, müşterilerin her AKS kümesindeki hizmetlere işaret eden bir Trafik Yöneticisi DNS adına bağlanması gerekir. Trafik Yöneticisi uç noktalarını kullanarak bu hizmetleri tanımlayın. Her uç nokta *hizmet yük dengeleyici*IP'dir. Bir bölgedeki Trafik Yöneticisi bitiş noktasından farklı bir bölgedeki bitiş noktasına ağ trafiğini yönlendirmek için bu yapılandırmayı kullanın.

![Trafik Müdürü ile AKS](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

[Azure Ön Kapı Hizmeti,](https://docs.microsoft.com/azure/frontdoor/front-door-overview) AKS kümeleri için trafiği yönlendirmeiçin başka bir seçenektir.  Azure Front Door Service, yüksek kullanılabilirliğe yönelik en iyi performans ve anında genel yük devretme için iyileştirerek, web trafiğinizin genel yönlendirmesini tanımlamanızı, yönetmenizi ve izlemenizi sağlar. 

### <a name="considerations-for-stateless-applications"></a>Stateless uygulamaları için dikkat edilmesi gerekenler

Devletsiz uygulama geçişi en basit durumdur. Kaynak tanımlarınızı (YAML veya Miğfer) yeni kümeye uygulayın, her şeyin beklendiği gibi çalıştığından emin olun ve yeni kümenizi etkinleştirmek için trafiği yeniden yönlendirin.

### <a name="considerations-for-stateful-applications"></a>Devlet uygulamaları için dikkat edilmesi gerekenler

Veri kaybını veya beklenmeyen kapalı kalma süresini önlemek için durum durumu yla ilgili uygulamalar geçişinizi dikkatle planlayın.

Azure Dosyaları kullanıyorsanız, dosya paylaşımını birim olarak yeni kümeye monte edebilirsiniz:
* [Statik Azure Dosyalarını Birim Olarak Dağın](https://docs.microsoft.com/azure/aks/azure-files-volume#mount-the-file-share-as-a-volume)

Azure Yönetilen Diskler kullanıyorsanız, diski yalnızca herhangi bir VM'ye bağlı değilse bağlayabilirsiniz:
* [Statik Azure Diski Birim Olarak Dağıla](https://docs.microsoft.com/azure/aks/azure-disk-volume#mount-disk-as-volume)

Bu yaklaşımlardan hiçbiri işe yaramazsa, yedekleme ve geri yükleme seçeneklerini kullanabilirsiniz:
* [Azure'da Velero](https://github.com/heptio/velero/blob/master/site/docs/master/azure-config.md)

#### <a name="azure-files"></a>Azure Dosyaları

Disklerin aksine, Azure Dosyaları aynı anda birden çok ana bilgisayara monte edilebilir. AKS kümenizde Azure ve Kubernetes, ACS kümenizin hala kullandığı bir bölme oluşturmanızı engellemez. Veri kaybını ve beklenmeyen davranışları önlemek için, kümelerin aynı dosyalara aynı anda yazmadığından emin olun.

Uygulamanız aynı dosya paylaşımına işaret eden birden çok yineleme barındırabiliyorsa, durum tanımaz geçiş adımlarını izleyin ve YAML tanımlarınızı yeni kümenize dağıtın. Değilse, olası bir geçiş yaklaşımı aşağıdaki adımları içerir:

* Uygulamanızın doğru çalıştığını doğrulayın.
* Canlı trafiğinizi yeni AKS kümenize yönlendirin.
* Eski kümenin bağlantısını kes.

Boş bir paylaşımla başlamak ve kaynak verilerin bir kopyasını yapmak istiyorsanız, verilerinizi geçirmek için [`az storage file copy`](https://docs.microsoft.com/cli/azure/storage/file/copy?view=azure-cli-latest) komutları kullanabilirsiniz.


#### <a name="migrating-persistent-volumes"></a>Kalıcı birimleri geçirme

Varolan kalıcı birimleri AKS'ye geçiriyorsanız, genellikle aşağıdaki adımları izlersiniz:

* Quiesce uygulamaya yazıyor. (Bu adım isteğe bağlıdır ve kapalı kalma süresi gerektirir.)
* Disklerin anlık görüntülerini alın.
* Anlık görüntülerden yeni yönetilen diskler oluşturun.
* AKS'de kalıcı birimler oluşturun.
* Kalıcı VolumeClaims (statik provisioning) yerine [varolan birimleri kullanmak](https://docs.microsoft.com/azure/aks/azure-disk-volume) için pod belirtimlerini güncelleştirin.
* Uygulamanızı AKS'ye dağıtın.
* Uygulamanızın doğru çalıştığını doğrulayın.
* Canlı trafiğinizi yeni AKS kümenize yönlendirin.

> [!IMPORTANT]
> Yazı yazmamayı seçerseniz, verileri yeni dağıtıma çoğaltmanız gerekir. Aksi takdirde, disk anlık görüntülerini aldıktan sonra yazılan verileri kaçırırsınız.

Bazı açık kaynak araçları yönetilen diskler oluşturmanıza ve Kubernetes kümeleri arasında birimleri geçirmenize yardımcı olabilir:

* [Azure CLI Disk Kopyalama uzantısı,](https://github.com/noelbundick/azure-cli-disk-copy-extension) diskleri kaynak grupları ve Azure bölgeleri arasında kopyalar ve dönüştürür.
* [Azure Kube CLI uzantısı](https://github.com/yaron2/azure-kube-cli) ACS Kubernetes birimlerini numaralandırır ve bunları bir AKS kümesine geçirtir.


### <a name="deployment-of-your-cluster-configuration"></a>Küme yapılandırmanızın dağıtımı

AKS'ye bilinen iyi bir yapılandırma dağıtmak için varolan Sürekli Tümleştirme (CI) ve Sürekli Teslim (CD) ardışık sisteminizi kullanmanızı öneririz. [Uygulamalarınızı oluşturmak ve AKS'ye dağıtmak için](https://docs.microsoft.com/azure/devops/pipelines/ecosystems/kubernetes/aks-template?view=azure-devops)Azure Pipelines'ı kullanabilirsiniz. Varolan dağıtım görevlerinizi klonlayın ve yeni AKS kümesini işaret etmesini `kubeconfig` sağlayın.

Bu mümkün değilse, kaynak tanımlarını mevcut Kubernetes kümenizden dışa aktarın ve bunları AKS'ye uygulayın. Nesneleri dışa aktarmak için kullanabilirsiniz. `kubectl`

```console
kubectl get deployment -o=yaml --export > deployments.yaml
```

### <a name="moving-existing-resources-to-another-region"></a>Varolan kaynakları başka bir bölgeye taşıma

AKS kümenizi AKS tarafından desteklenen farklı bir bölgeye taşımak [isteyebilirsiniz.][region-availability] Diğer bölgede yeni bir küme oluşturmanızı ve kaynaklarınızı ve uygulamalarınızı yeni kümenize dağıtmanızı öneririz. Ayrıca, AKS kümenizde çalışan [Azure Geliştirme Alanları][azure-dev-spaces] gibi hizmetleriniz varsa, bu hizmetleri yeni bölgede kümenizde yüklemeniz ve yapılandırmanız gerekir.


Bu makalede geçiş ayrıntılarını özetledik:

> [!div class="checklist"]
> * Standart Yük Dengeleyici ve Sanal Makine Ölçek Setleri ile AKS
> * Mevcut ekli Azure Hizmetleri
> * Geçerli kotaları sağlama
> * Yüksek Kullanılabilirlik ve iş sürekliliği
> * Stateless uygulamaları için dikkat edilmesi gerekenler
> * Devlet uygulamaları için dikkat edilmesi gerekenler
> * Küme yapılandırmanızın dağıtımı


[region-availability]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[azure-dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/