---
title: Büyük ölçekte küme yapılandırmaları uygulamak için Azure İlkesi'ni kullanma (Önizleme)
services: azure-arc
ms.service: azure-arc
ms.date: 02/10/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: Ölçek yapılandırmasında küme yapılandırması uygulamak için Azure Ilkesini kullanma
keywords: Kubernetes, yay, Azure, K8s, kapsayıcılar
ms.openlocfilehash: ce9ba75e200a02654cac4c50303cc90fd0c1a5fd
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100390921"
---
# <a name="use-azure-policy-to-apply-cluster-configurations-at-scale-preview"></a>Büyük ölçekte küme yapılandırmaları uygulamak için Azure İlkesi'ni kullanma (Önizleme)

## <a name="overview"></a>Genel Bakış

Belirli bir uygulamaya uygulanmasını sağlamak için aşağıdaki kaynaklardan birini zorlamak üzere Azure Ilkesini kullanabilirsiniz `Microsoft.KubernetesConfiguration/sourceControlConfigurations` :
*  `Microsoft.Kubernetes/connectedclusters` Kaynak.
* Gilar özellikli `Microsoft.ContainerService/managedClusters` kaynak. 

Azure Ilkesini kullanmak için, var olan bir ilke tanımı seçin ve bir ilke ataması oluşturun. İlke atamasını oluştururken:
1. Atama için kapsamı ayarlayın.
    * Kapsam bir Azure Kaynak grubu veya aboneliği olacaktır. 
2. Oluşturulacak için parametrelerini ayarlayın `sourceControlConfiguration` . 

Atama oluşturulduktan sonra, Azure Ilke altyapısı `connectedCluster` `managedCluster` kapsam içinde bulunan tüm veya kaynakları tanımlar ve `sourceControlConfiguration` her birine uygular.

Birden çok Git deposunu, birden çok ilke ataması kullanarak her küme için doğru kaynaklar olarak etkinleştirebilirsiniz. Her ilke ataması, farklı bir git deposu kullanacak şekilde yapılandırılır; Örneğin, merkezi BT/küme operatörü ve uygulama ekiplerine yönelik diğer depolar için bir depo.

## <a name="prerequisite"></a>Önkoşul

`Microsoft.Authorization/policyAssignments/write`Bu ilke atamasını oluşturacağınız kapsam (abonelik veya kaynak grubu) üzerinde izinleriniz olduğunu doğrulayın.

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

1. Azure portal **ilke**' ye gidin.
1. Kenar çubuğunun **yazma** bölümünde **tanımlar**' ı seçin.
1. "Kubernetes" kategorisinde, "gire 'leri Kubernetes kümesine dağıt" yerleşik ilkesini seçin. 
1. **Ata**' ya tıklayın.
1. **Kapsamı** , ilke atamasının uygulanacağı yönetim grubu, abonelik veya kaynak grubu olarak ayarlayın.
    * İlke kapsamından herhangi bir kaynağı dışlamak istiyorsanız, **dışlamaları** ayarlayın.
1. İlke atamaya kolay tanımlanabilir bir **ad** ve **Açıklama** sağlayın.
1. **İlke zorlamasının** **etkin** olarak ayarlandığından emin olun.
1. **İleri**’yi seçin.
1. Oluştururken kullanılacak parametre değerlerini ayarlayın `sourceControlConfiguration` .
1. **İleri**’yi seçin.
1. **Düzeltme görevi oluşturma** özelliğini etkinleştirin.
1. **Yönetilen kimlik oluşturma** seçeneğinin işaretli olduğunu ve kimliğin **katkıda** bulunan izinlerine sahip olacağını doğrulayın. 
    * Daha fazla bilgi için bkz. [ilke ataması oluşturma hızlı](../../governance/policy/assign-policy-portal.md) başlangıcı ve [uyumlu olmayan kaynakları Azure ilkesiyle](../../governance/policy/how-to/remediate-resources.md)düzeltme.
1. **Gözden geçir ve oluştur**’u seçin.

İlke atamasını oluşturduktan sonra, `sourceControlConfiguration` atama kapsamı içinde yer alan aşağıdaki kaynaklardan herhangi biri için uygulanır:
* Yeni `connectedCluster` kaynaklar.
* `managedCluster`Gile aracıları yüklü yeni kaynaklar. 

Mevcut kümeler için bir düzeltme görevini el ile çalıştırmanız gerekir. Bu görev, ilke atamasının etkili olması için genellikle 10 ila 20 dakika sürer.

## <a name="verify-a-policy-assignment"></a>İlke atamasını doğrulama

1. Azure portal `connectedCluster` kaynaklarınızdan birine gidin.
1. Kenar çubuğunun **Ayarlar** bölümünde **ilkeler**' i seçin. 
    * AKS kümesi UX henüz uygulanmadı.
    * İlkeler listesinde, daha önce oluşturduğunuz ilke atamasını **uyumluluk durumu** ile *uyumlu* olarak ayarlanmış olarak görmeniz gerekir.
1. Kenar çubuğunun **Ayarlar** bölümünde, **Konfigürasyonlar**' ı seçin.
    * Konfigürasyonlar listesinde, `sourceControlConfiguration` ilke atamasının oluşturulduğunu görmeniz gerekir.
1. `kubectl`Kümeyi sorgulanamıyor için kullanın. 
    * Tarafından oluşturulan ad alanını ve yapıtları görmeniz gerekir `sourceControlConfiguration` .
    * 5 dakika içinde, yapılandırılan git deposundaki bildirimlerde açıklanan yapıtları kümede görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Arc etkin Kubernetes kümelerine sahip kapsayıcılar için Azure Izleyici 'yi ayarlama](../../azure-monitor/insights/container-insights-enable-arc-enabled-clusters.md)
