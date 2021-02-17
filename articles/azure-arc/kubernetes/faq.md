---
title: Azure Arc etkin Kubernetes sık sorulan sorular
services: azure-arc
ms.service: azure-arc
ms.date: 02/15/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: Bu makale, Azure Arc etkin Kubernetes ile ilgili sık sorulan soruların bir listesini içerir
keywords: Kubernetes, yay, Azure, kapsayıcılar, yapılandırma, Gilar, SSS
ms.openlocfilehash: 237b2629b833a63552b172636f46a1ac92e321c0
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100561829"
---
# <a name="frequently-asked-questions---azure-arc-enabled-kubernetes"></a>Sık sorulan sorular-Azure Arc etkin Kubernetes

Bu makalede, Azure Arc etkin Kubernetes hakkında sık sorulan sorular ele alınmaktadır.

## <a name="what-is-the-difference-between-azure-arc-enabled-kubernetes-and-azure-kubernetes-service-aks"></a>Azure Arc etkin Kubernetes ve Azure Kubernetes hizmeti (AKS) arasındaki fark nedir?

AKS, Azure tarafından yönetilen Kubernetes teklifidir. AKS, Azure 'da karmaşıklık ve operasyonel ek yükün çoğunu boşaltarak Azure 'da yönetilen bir Kubernetes kümesinin dağıtılmasını basitleştirir. Kubernetes yöneticileri Azure tarafından yönetildiğinden yalnızca aracı düğümlerini yönetebilir ve bakımını yapabilirsiniz.

Azure Arc etkin Kubernetes, Kubernetes kümelerini Azure 'a bağlayarak Azure 'un yönetim özelliklerini (Azure Izleyici ve Azure Ilkesi gibi) genişletmenizi sağlar. Temel Kubernetes kümesinin kendisini korursunuz.

## <a name="do-i-need-to-connect-my-aks-clusters-running-on-azure-to-azure-arc"></a>Azure 'da çalışan AKS kümelerimi Azure yaya bağlamanız gerekiyor mu?

Hayır. Azure Izleyici ve Azure Ilkesi (Gatekeeper) dahil olmak üzere tüm Azure Arc etkin Kubernetes Özellikleri AKS (Azure Resource Manager içindeki yerel bir kaynak) ile kullanılabilir.
    
## <a name="should-i-connect-my-aks-hci-cluster-and-kubernetes-clusters-on-azure-stack-hub-and-azure-stack-edge-to-azure-arc"></a>AKS-HI kümesi ve Kubernetes kümelerimi Azure Stack hub 'a ve Azure Stack kenarından Azure yaya bağlamanız gerekir mi?

Evet, AKS-HI kümenizi veya Kubernetes kümelerinizi Azure Stack Edge veya Azure Stack hub 'a bağlamak Azure Resource Manager ' de kaynak gösterimine sahip kümeler sağlar. Bu kaynak temsili, bağlandığınız Kubernetes kümelerine küme yapılandırması, Azure Izleyici ve Azure Ilkesi (Gatekeeper) gibi özellikleri genişletir.

## <a name="how-to-address-expired-azure-arc-enabled-kubernetes-resources"></a>Kullanım dışı Azure Arc etkin Kubernetes kaynakları nasıl ele alınacağını?

Azure Arc etkin Kuberenetes ile ilişkili yönetilen hizmet kimliği (MSI) sertifikası, 90 günlük bir süre sonu penceresine sahiptir. Bu sertifikanın süresi dolduktan sonra kaynak kabul edilir `Expired` ve yapılandırma, izleme ve ilke gibi tüm özellikler bu kümede çalışmayı durdurur. Kubernetes kümenizi Azure Arc ile birlikte çalışır şekilde almak için şu adımları izleyin:

1. Kümedeki Azure Arc etkin Kubernetes kaynak ve aracılarını silme 

    ```console
    az connectedk8s delete -n <name> -g <resource-group>
    ```

1. Kümeye aracı dağıtarak Azure Arc etkin Kubernetes kaynağını yeniden oluşturun.
    
    ```console
    az connectedk8s connect -n <name> -g <resource-group>
    ```

> [!NOTE]
> `az connectedk8s delete` Ayrıca, kümenin üstündeki konfigürasyonları da siler. Çalıştıktan sonra, `az connectedk8s connect` el ile veya Azure ilkesi 'ni kullanarak kümedeki konfigürasyonları tekrar oluşturun.

## <a name="if-i-am-already-using-cicd-pipelines-can-i-still-use-azure-arc-enabled-kubernetes-and-configurations"></a>Zaten CI/CD işlem hatları kullanıyorum, Azure Arc etkinleştirilmiş Kubernetes ve konfigürasyonları kullanmaya devam edebilir miyim?

Evet, bir CI/CD işlem hattı aracılığıyla bir küme alma dağıtımında yapılandırma kullanmaya devam edebilirsiniz. Geleneksel CI/CD işlem hatları ile karşılaştırıldığında, yapılandırmaların iki ek avantajı vardır:

**Drift mutabakatı**

CI/CD işlem hattı, işlem hattı çalıştırması sırasında değişiklikleri yalnızca bir kez uygular. Ancak kümedeki Gila işleci, kümedeki Kubernetes kaynaklarının istenen durumunu getirmek için Git deposunu sürekli olarak yoklar. Giüstler işleci, istenen kaynak durumunu kümedeki kaynakların gerçek durumundan farklı olacak şekilde bulursa, bu DRIP mutabakatı yapılır.

**Gilar 'ı ölçeğe göre Uygula**

CI/CD işlem hatları, olayın bir git deposuna gönderim olabilecek Kubernetes kümenize olay temelli dağıtımlar için uygundur. Ancak, tüm Kubernetes kümelerinizde aynı yapılandırmanın dağıtılması CI/CD işlem hattının bu Kubernetes kümelerinin her birinin kimlik bilgileriyle el ile yapılandırılmasını gerektirir. Öte yandan, Azure Arc etkinleştirilmiş Kubernetes söz konusu olduğunda, Azure Resource Manager yapılandırmalarını yönettiğinden, bir abonelik veya kaynak grubu kapsamındaki tüm Kubernetes kümelerinde istenen yapılandırmanın uygulamasını tek bir go 'da otomatik hale getirmek için Azure Ilkesini kullanabilirsiniz. Bu özellik, ilke atamasından sonra oluşturulan Azure Arc etkin Kubernetes kaynakları için de geçerlidir.

Yapılandırma özelliği, uyumluluk ve idare gereksinimleri için Kubernetes kümelerinin tüm envanterinin tamamında ağ ilkeleri, rol bağlamaları ve pod güvenlik ilkeleri gibi temel yapılandırmaların uygulanması için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir kümeyi Azure yaya bağlama](./connect-cluster.md)
* [Arc etkin Kubernetes kümesinde yapılandırma oluşturma](./use-gitops-connected-cluster.md)
* [Uygun ölçekte yapılandırma uygulamak için Azure Ilkesini kullanma](./use-azure-policy.md)
