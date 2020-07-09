---
title: Ölçek yapılandırmasında küme yapılandırması uygulamak için Azure Ilkesi kullanma (Önizleme)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Ölçek yapılandırmasında küme yapılandırması uygulamak için Azure Ilkesini kullanma
keywords: Kubernetes, yay, Azure, K8s, kapsayıcılar
ms.openlocfilehash: 26b291e2a957047361d4f52eeff58cbe8aa8c633
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86111278"
---
# <a name="use-azure-policy-to-apply-cluster-configurations-at-scale-preview"></a>Ölçek yapılandırmasında küme yapılandırması uygulamak için Azure Ilkesi kullanma (Önizleme)

## <a name="overview"></a>Genel Bakış

Her `Microsoft.Kubernetes/connectedclusters` kaynak veya git Ops özellikli `Microsoft.ContainerService/managedClusters` kaynağın bu kaynağa uygulanmasını zorlamak Için Azure ilkesini kullanın `Microsoft.KubernetesConfiguration/sourceControlConfigurations` . Azure Ilkesini kullanmak için var olan bir ilke tanımı seçin ve bir ilke ataması oluşturun. İlke atamasını oluştururken atama için kapsamı ayarlarsınız: Bu, bir Azure Kaynak grubu veya aboneliği olacaktır. Oluşturulacak için parametreleri de ayarlarsınız `sourceControlConfiguration` . Atama oluşturulduktan sonra, Ilke altyapısı `connectedCluster` `managedCluster` kapsam içinde bulunan tüm veya kaynakları belirler ve `sourceControlConfiguration` her birine uygular.

Her küme için Truth kaynakları olarak birden çok git deposu kullanıyorsanız (örneğin, merkezi BT/küme operatörü için bir depo ve uygulama ekiplerine yönelik diğer depolar), bunu, her ilke atamasını farklı bir git deposu kullanacak şekilde yapılandırılmış birden fazla ilke ataması kullanarak etkinleştirebilirsiniz.

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

1. Azure portal, Ilke ' ye gidin ve kenar çubuğunun **yazma** bölümünde **tanımlar**' ı seçin.
2. "Kubernetes" kategorisinde yerleşik ilke olan "Kubernetes" konumunda Gima 'yı Dağıt ' ı seçin ve **ata**' ya tıklayın.
3. **Kapsamı** , ilke atamasının uygulanacağı yönetim grubu, abonelik veya kaynak grubu olarak ayarlayın.
4. İlke kapsamından herhangi bir kaynağı dışlamak istiyorsanız, **dışlamaları**ayarlayın.
5. İlke atamaya, kolayca tanımlamak için kullanabileceğiniz bir **ad** ve **Açıklama** sağlayın.
6. **İlke zorlamasının** *etkin*olarak ayarlandığından emin olun.
7. **İleri**’yi seçin.
8. Öğesinin oluşturulması sırasında kullanılacak parametre değerlerini ayarlayın `sourceControlConfiguration` .
9. **İleri**’yi seçin.
10. **Düzeltme görevi oluşturma**özelliğini etkinleştirin.
11. **Yönetilen bir kimlik oluşturmak** ve kimliğin **katkıda** bulunan izinlerine sahip olması güvence altına alınır. İhtiyacınız olan izinler hakkında daha fazla bilgi [için bu belgeyi ve](../../governance/policy/assign-policy-portal.md) [Bu belgedeki yorumu](../../governance/policy/how-to/remediate-resources.md) inceleyin.
12. **İncele ve oluştur**’u seçin.

İlke ataması oluşturulduktan sonra, `connectedCluster` atama kapsamında yer alan herhangi bir yeni kaynak (veya `managedCluster` Gile aracıları yüklü olan kaynak) için `sourceControlConfiguration` uygulanır. Mevcut kümeler için bir düzeltme görevini el ile çalıştırmanız gerekir. İlke atamasının etkili olması için genellikle 10-20 dakika sürer.

## <a name="verify-a-policy-assignment"></a>İlke atamasını doğrulama

1. Azure portal `connectedCluster` kaynaklarınızdan birine gidin ve kenar çubuğunun **Ayarlar** bölümünde **ilkeler**' i seçin. (AKS tarafından yönetilen küme için UX henüz uygulanmadı, ancak geliyor.)
2. Listede, yukarıda oluşturduğunuz ilke atamasını görmeniz ve **Uyumluluk durumunun** *uyumlu*olması gerekir.
3. Kenar çubuğunun **Ayarlar** bölümünde, **Konfigürasyonlar**' ı seçin.
4. Listede, `sourceControlConfiguration` ilke atamasının oluşturulduğunu görmeniz gerekir.
5. Kümeyi sorgulanamıyor için **kubectl** kullanın: tarafından oluşturulan ad alanını ve yapıtları görmeniz gerekir `sourceControlConfiguration` .
6. 5 dakika içinde, yapılandırılan git deposundaki bildirimlerde açıklanan yapıtları kümede görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Arc etkin Kubernetes kümelerine sahip kapsayıcılar için Azure Izleyici 'yi ayarlama](../../azure-monitor/insights/container-insights-enable-arc-enabled-clusters.md)
