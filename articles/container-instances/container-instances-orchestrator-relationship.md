---
title: Konteyner örnekleri ve konteyner orkestrasyonu
description: Azure kapsayıcı örneklerinin kapsayıcı orkestratörleri ile nasıl etkileşimde olduğunu anlayın.
ms.topic: article
ms.date: 04/15/2019
ms.custom: mvc
ms.openlocfilehash: f3f8693d1a9a12e7c35d126ab3e3ca53448e5e40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74533656"
---
# <a name="azure-container-instances-and-container-orchestrators"></a>Azure Konteyner Örnekleri ve konteyner orkestratörleri

Küçük boyutları ve uygulama yönlendirmeleri nedeniyle, kapsayıcılar çevik dağıtım ortamları ve mikro hizmet tabanlı mimariler için uygundur. Çok sayıda kapsayıcıyı otomatikleştirmek ve yönetmek ve nasıl etkileşimde bulundukları *orkestrasyon*olarak bilinir. Popüler konteyner orkestratörleri arasında Kubernetes, DC/OS ve Docker Swarm sayılabilir.

Azure Kapsayıcı Örnekleri, düzenleme platformlarının bazı temel zamanlama özelliklerinden bazılarını sağlar. Ayrıca, bu platformların sağladığı yüksek değerli hizmetleri kapsamasa da, Azure Kapsayıcı Örnekleri bunlara tamamlayıcı olabilir. Bu makalede, Azure Kapsayıcı Örnekleri'nin işlettiği kapsam ve tam kapsayıcı orkestratörlerinin bu siyle nasıl etkileşimde olabileceği açıklanmaktadır.

## <a name="traditional-orchestration"></a>Geleneksel orkestrasyon

Orkestrasyonun standart tanımı aşağıdaki görevleri içerir:

- **Zamanlama**: Bir kapsayıcı görüntüsü ve kaynak isteği göz önüne alındığında, kapsayıcıyı çalıştıracak uygun bir makine bulun.
- **Yakınlık/Karşıtlık**: Bir kapsayıcı kümesinin birbirine yakın (performans için) veya yeterince birbirinden uzak (kullanılabilirlik için) çalışması gerektiğini belirtin.
- **Sistem durumu izleme**: Konteyner arızalarını izleyin ve otomatik olarak yeniden zamanlayın.
- **Failover**: Her makinede ne çalıştığını takip edin ve konteynırları başarısız makinelerden sağlıklı düğümlere yeniden zamanlayın.
- **Ölçekleme**: Talebi el ile veya otomatik olarak eşleştirmek için kapsayıcı örnekleri ekleyin veya kaldırın.
- **Ağ :** Birden çok ana bilgisayar makinesi arasında iletişim kurmak için kapsayıcıları koordine etmek için bir bindirme ağı sağlayın.
- **Hizmet bulma**: Kapsayıcıların ana bilgisayar makineleri arasında hareket edip IP adreslerini değiştirseler bile otomatik olarak birbirlerini bulmalarını sağlar.
- **Eşgüdümlü uygulama yükseltmeleri**: Uygulama kapalı kalma süresini önlemek için konteyner yükseltmelerini yönetin ve bir şeyler ters giderse geri alma olanağı nı etkinleştirin.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Azure Kapsayıcı Örnekleri ile Düzenleme: Katmanlı bir yaklaşım

Azure Kapsayıcı Örnekleri, tek bir kapsayıcıyı çalıştırmak için gereken tüm zamanlama ve yönetim özelliklerini sağlarken, orkestratör platformlarının birden fazla kapsayıcı görevlerini yönetmesine olanak sağlayarak, orkestrasyona katmanlı bir yaklaşım sağlar.

Kapsayıcı örnekleriiçin temel altyapı Azure tarafından yönetildiği için, bir orkestratör platformunun tek bir kapsayıcıyı çalıştıracak uygun bir ana bilgisayar makinesi bulmakla ilgilenmesi gerekmez. Bulutun esnekliği, her zaman kullanılabilir olmasını sağlar. Bunun yerine, orkestratör ölçekleme ve eşgüdümlü yükseltmeler de dahil olmak üzere çok kapsayıcı mimarilerin gelişimini basitleştiren görevlere odaklanabilir.

## <a name="scenarios"></a>Senaryolar

Azure Kapsayıcı Örnekleri ile orchestrator entegrasyonu hala yeni bir zaman olsa da, birkaç farklı ortamın ortaya çıkacağını tahmin ediyoruz:

### <a name="orchestration-of-container-instances-exclusively"></a>Konteyner örneklerinin düzenlenmesi

Hızlı bir şekilde başlayıp ikinci ye doğru faturalandırdıkları için, yalnızca Azure Kapsayıcı Örnekleri'ne dayalı bir ortam, başlamak ve son derece değişken iş yükleriyle başa çıkmanın en hızlı yolunu sunar.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Sanal Makinelerde konteyner örnekleri ve konteynerlerin birleşimi

Uzun soluklu, kararlı iş yükleri için, kapsayıcıları özel sanal makineler kümesinde düzenlemek genellikle Azure Kapsayıcı Örnekleri ile aynı kapsayıcıları çalıştırmaktan daha ucuzdur. Ancak, konteyner örnekleri, kullanımdaki beklenmeyen veya kısa ömürlü ani artışlarla başa çıkmak için genel kapasitenizi hızla genişletmek ve daraltmak için harika bir çözüm sunar.

Kümenizdeki sanal makinelerin sayısını ölçeklemek ve ardından bu makinelere ek kapsayıcılar dağıtmak yerine, orkestratör Azure Kapsayıcı Örnekleri'ndeki ek kapsayıcıları zamanlayabilir ve artık bunlar olmadığında silebilir Gerekli.

## <a name="sample-implementation-virtual-nodes-for-azure-kubernetes-service-aks"></a>Örnek uygulama: Azure Kubernetes Hizmeti (AKS) için sanal düğümler

[Bir Azure Kubernetes Hizmeti](../aks/intro-kubernetes.md) (AKS) kümesinde uygulama iş yüklerini hızla ölçeklendirmek için, Azure Kapsayıcı Örnekleri'nde dinamik olarak oluşturulan sanal *düğümleri* kullanabilirsiniz. Sanal düğümler, ACI ve AKS kümesinde çalışan bölmeler arasındaki ağ iletişimini sağlar. 

Sanal düğümler şu anda Linux kapsayıcı örneklerini destekler. [Azure CLI](https://go.microsoft.com/fwlink/?linkid=2047538) veya [Azure portalını](https://go.microsoft.com/fwlink/?linkid=2047545)kullanarak sanal düğümlerle başlayın.

Sanal düğümler sınırsız kapasiteye sahip bir düğüm olarak kaydolarak Kubernetes [kubelet][kubelet-doc] taklit etmek için açık kaynak [Sanal Kubelet][aci-connector-k8s] kullanın. Sanal Kubelet, Azure Kapsayıcı Örnekleri'nde [bölme][pod-doc] oluşturmayı kapsayıcı grupları olarak gönderir.

Kubernetes API'sini sunucusuz kapsayıcı platformlara genişletmeye yönelik ek örnekler için [Sanal Kubelet](https://github.com/virtual-kubelet/virtual-kubelet) projesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı başlangıç kılavuzunu](container-instances-quickstart.md)kullanarak Azure Kapsayıcı Örnekleri ile ilk kapsayıcınızı oluşturun.

<!-- IMAGES -->

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/virtual-kubelet/azure-aci
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/
