---
title: Azure Container Service (ACS) ile Azure Kubernetes Service 'e (AKS) geçiş
description: Azure Container Service (ACS) konumundan Azure Kubernetes hizmetine (AKS) geçiş yapın.
services: container-service
author: noelbundick
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: nobun
ms.custom: mvc
ms.openlocfilehash: 84e0af89e2b3247bc922ab84286a79a0934323a8
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73473004"
---
# <a name="migrate-from-azure-container-service-acs-to-azure-kubernetes-service-aks"></a>Azure Container Service (ACS) ile Azure Kubernetes Service 'e (AKS) geçiş

Bu makale, Kubernetes ve Azure Kubernetes hizmeti (AKS) ile Azure Container Service (ACS) arasında başarılı bir geçişi planlayıp yürütmenize yardımcı olur. Bu kılavuzda, önemli kararlar almanıza yardımcı olmak için, ACS ve AKS arasındaki farklılıklar ayrıntıları verilir ve geçiş sürecine genel bir bakış sağlanır.

## <a name="differences-between-acs-and-aks"></a>ACS ve AKS arasındaki farklar

ACS ve AKS, geçişi etkileyen bazı önemli alanlarda farklılık gösterir. Herhangi bir geçişten önce, aşağıdaki farkları gözden geçirmeniz ve planlamanız gerekir:

* AKS düğümleri [yönetilen diskleri](../virtual-machines/windows/managed-disks-overview.md)kullanır.
    * Yönetilmeyen disklerin, AKS düğümlerine iliştirilebilmesi için önce dönüştürülmesi gerekir.
    * Azure diskleri için özel `StorageClass` nesnelerinin `unmanaged` `managed`olarak değiştirilmesi gerekir.
    * Herhangi bir `PersistentVolumes` `kind: Managed`kullanmalıdır.
* AKS [birden çok düğüm havuzunu](https://docs.microsoft.com/azure/aks/use-multiple-node-pools)destekler.
* Windows Server tabanlı düğümler Şu anda [AKS 'de önizlemededir](https://azure.microsoft.com/blog/kubernetes-on-azure/).
* AKS, sınırlı bir [bölge](https://docs.microsoft.com/azure/aks/quotas-skus-regions)kümesini destekler.
* AKS, barındırılan bir Kubernetes denetim düzlemi içeren bir yönetilen hizmettir. Daha önce ACS asıllarınızın yapılandırmasını değiştirdiyseniz uygulamalarınızı değiştirmeniz gerekebilir.

## <a name="differences-between-kubernetes-versions"></a>Kubernetes sürümleri arasındaki farklar

Kubernetes 'in daha yeni bir sürümüne geçiş yapıyorsanız, Kubernetes sürüm oluşturma stratejilerini anlamak için aşağıdaki kaynakları gözden geçirin:

* [Kubernetes sürümü ve sürüm eğriltme destek ilkesi](https://kubernetes.io/docs/setup/release/version-skew-policy/#supported-versions)

## <a name="migration-considerations"></a>Geçiş sırasında dikkat edilmesi gerekenler

### <a name="agent-pools"></a>Aracı havuzları

AKS, Kubernetes denetim düzlemi 'ni yönetse de, yeni kümenize eklenecek düğümlerin boyutunu ve sayısını tanımlarsınız. ACS 'den AKS 'e 1:1 eşleme istediğinizi varsayarsak, var olan ACS düğüm bilgilerinizi yakalamak isteyeceksiniz. Yeni AKS kümenizi oluştururken bu verileri kullanın.

Örnek:

| Ad | Sayı | VM boyutu | İşletim sistemi |
| --- | --- | --- | --- |
| agentpool0 | 3 | Standard_D8_v2 | Linux |
| agentpool1 | 1 | Standard_D2_v2 | Windows |

Geçiş sırasında aboneliğinize daha fazla sanal makine dağıtılacağından, Kotalarınızın ve limitlerinizin bu kaynaklar için yeterli olduğunu doğrulamanız gerekir. 

Daha fazla bilgi için bkz. [Azure aboneliği ve hizmet sınırları](https://docs.microsoft.com/azure/azure-subscription-service-limits). Geçerli kotalarınızı denetlemek için, Azure portal [abonelikler dikey penceresine](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)gidin, aboneliğinizi seçin ve ardından **kullanım + kotalar**' ı seçin.

### <a name="networking"></a>Ağ

Karmaşık uygulamalar için genellikle her seferinde değil zaman içinde geçiş yapabilirsiniz. Bu, eski ve yeni ortamların ağ üzerinden iletişim kurması gerekebilecek anlamına gelir. Daha önce iletişim kurmak için `ClusterIP` hizmetleri kullanan uygulamaların, tür `LoadBalancer` olarak sunulup güvenli bir şekilde sağlanması gerekebilir.

Geçişi gerçekleştirmek için, istemcileri AKS üzerinde çalışan yeni hizmetlere işaret etmek isteyeceksiniz. DNS 'yi, AKS kümenizin önünde bulunan Load Balancer işaret etmek üzere güncelleştirerek trafiği yeniden yönlendirmenizi öneririz.

### <a name="stateless-applications"></a>Durum bilgisiz uygulamalar

Durum bilgisiz uygulama geçişi en kolay durumdur. YAML tanımlarınızı yeni kümeye uygulayacaksınız, her şeyin beklendiği gibi çalıştığından emin olun ve yeni kümenizi etkinleştirmek için trafiği yeniden yönlendirin.

### <a name="stateful-applications"></a>Durum bilgisi olan uygulamalar

Veri kaybını veya beklenmedik kapalı kalma süresini önlemek için durum bilgisi olan uygulamaların geçişini dikkatle planlayın.

#### <a name="highly-available-applications"></a>Yüksek oranda kullanılabilir uygulamalar

Yüksek kullanılabilirlik yapılandırmasında, durum bilgisi olan bazı uygulamalar dağıtabilirsiniz. Bu uygulamalar, çoğaltmaları arasında veri kopyalayabilir. Şu anda bu tür bir dağıtım kullanıyorsanız, yeni AKS kümesinde yeni bir üye oluşturabilir ve ardından aşağı akış çağıranları üzerinde en az etkiyle geçiş yapabilirsiniz. Genellikle, bu senaryonun geçiş adımları şunlardır:

1. AKS üzerinde yeni bir ikincil çoğaltma oluşturun.
2. Verilerin çoğaltılmasını bekleyin.
3. Yeni birincili ikincil çoğaltma yapmak için yük devretmek.
4. AKS kümesine giden trafiği işaret edin.

#### <a name="migrating-persistent-volumes"></a>Kalıcı birimleri geçirme

Mevcut kalıcı birimleri AKS 'e geçiriyorsanız, genellikle bu adımları takip edersiniz:

1. Uygulamaya sessiz yazma yazma. (Bu adım isteğe bağlıdır ve kapalı kalma süresi gerektirir.)
2. Disklerin anlık görüntülerini alın.
3. Anlık görüntülerden yeni yönetilen diskler oluşturun.
4. AKS 'de kalıcı birimler oluşturun.
5. Pod belirtimlerini, Persistentvolumeclaim (statik sağlama) yerine [mevcut birimleri kullanacak](https://docs.microsoft.com/azure/aks/azure-disk-volume) şekilde güncelleştirin.
6. Uygulamayı AKS 'e dağıtın.
7. Doğrulamalısınız.
8. AKS kümesine giden trafiği işaret edin.

> [!IMPORTANT]
> Yazmaları sessiz değil ' i seçerseniz, verileri yeni dağıtıma çoğaltmanız gerekir. Aksi takdirde, disk anlık görüntülerini aldıktan sonra yazılmış verileri kaçırılırsınız.

Bazı açık kaynaklı araçlar, yönetilen diskler oluşturmanıza ve birimleri Kubernetes kümeleri arasında geçirmeye yardımcı olabilir:

* [Azure CLI disk kopyalama uzantısı](https://github.com/noelbundick/azure-cli-disk-copy-extension) , diskleri kaynak grupları ve Azure bölgeleri arasında kopyalar ve dönüştürür.
* [Azure KUTO CLI uzantısı](https://github.com/yaron2/azure-kube-cli) , ACS Kubernetes birimlerini numaralandırır ve bunları bir aks kümesine geçirir.

#### <a name="azure-files"></a>Azure Dosyaları

Disklerden farklı olarak, Azure dosyaları aynı anda birden çok konağa bağlanabilir. AKS kümenizde, Azure ve Kubernetes, ACS kümenizin hala kullandığı bir pod oluşturmanızı engellemez. Veri kaybını ve beklenmedik davranışı engellemek için, kümelerin aynı anda aynı dosyalara yazmayın olduğundan emin olun.

Uygulamanız aynı dosya paylaşımının işaret eden birden çok kopyayı barındırabiliyorsanız, durum bilgisi olmayan geçiş adımlarını izleyin ve YAML tanımlarınızı yeni kümenize dağıtın. Aksi takdirde, olası bir geçiş yaklaşımı aşağıdaki adımları içerir:

1. Uygulamanızı 0 çoğaltma sayısı ile AKS 'e dağıtın.
2. ACS üzerinde uygulamayı 0 olarak ölçeklendirin. (Bu adım kapalı kalma süresi gerektirir.)
3. AKS üzerinde uygulamayı 1 ' e kadar ölçeklendirin.
4. Doğrulamalısınız.
5. AKS kümesine giden trafiği işaret edin.

Boş bir paylaşımdan başlamak ve kaynak verilerin bir kopyasını oluşturmak istiyorsanız, [`az storage file copy`](https://docs.microsoft.com/cli/azure/storage/file/copy?view=azure-cli-latest) komutlarını kullanarak verilerinizi geçirebilirsiniz.

### <a name="deployment-strategy"></a>Dağıtım stratejisi

AKS 'e bilinen iyi bir yapılandırma dağıtmak için mevcut CI/CD işlem hattınızı kullanmanızı öneririz. Mevcut dağıtım görevlerinizi kopyalayın ve `kubeconfig` yeni AKS kümesine işaret ettiğini doğrulayın.

Bu mümkün değilse, kaynak tanımlarını ACS 'den dışarı aktarın ve AKS 'e uygulayın. Nesneleri dışarı aktarmak için `kubectl` kullanabilirsiniz.

```console
kubectl get deployment -o=yaml --export > deployments.yaml
```

Çeşitli açık kaynaklı araçlar, dağıtım gereksinimlerinize bağlı olarak yardımcı olabilir:

* [Velero](https://github.com/heptio/ark) (Bu araç, Kubernetes 1,7 gerektirir.)
* [Azure Kuto CLı uzantısı](https://github.com/yaron2/azure-kube-cli)
* [ReShifter](https://github.com/mhausenblas/reshifter)

## <a name="migration-steps"></a>Geçiş adımları

1. Azure portal, Azure CLı veya Azure Resource Manager şablonu aracılığıyla [BIR AKS kümesi oluşturun](https://docs.microsoft.com/azure/aks/create-cluster) .

   > [!NOTE]
   > GitHub 'daki [Azure/aks](https://github.com/Azure/AKS/tree/master/examples/vnet) deposunda aks için örnek Azure Resource Manager şablonları bulun.

2. YAML tanımlarınızda gerekli değişiklikleri yapın. Örneğin, `apps/v1beta1` `Deployments`için `apps/v1` ile değiştirin.

3. ACS kümenizdeki birimleri (isteğe bağlı) AKS kümenize [geçirin](#migrating-persistent-volumes) .

4. AKS 'e uygulama dağıtmak için CI/CD sisteminizi kullanın. Ya da YAML tanımlarını uygulamak için kubectl kullanın.

5. Doğrulamalısınız. Uygulamalarınızın beklenen şekilde çalıştığından ve geçirilen verilerin üzerine kopyalandığından emin olun.

6. Trafiği yeniden yönlendirin. İstemcileri AKS dağıtımınıza işaret eden DNS 'yi güncelleştirme.

7. Geçiş sonrası görevleri bitti. Birimleri geçirdikten sonra yazma işlemlerini sessiz yapmayı seçtiyseniz, bu verileri yeni kümeye kopyalayın.
