---
title: Ölçek yapılandırmasında küme yapılandırması uygulamak için Azure Ilkesini kullanma
services: azure-arc
ms.service: azure-arc
ms.date: 03/03/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: Ölçek yapılandırmasında küme yapılandırması uygulamak için Azure Ilkesini kullanma
keywords: Kubernetes, yay, Azure, K8s, kapsayıcılar
ms.openlocfilehash: 05a6665a985ef8b229ee58082dc9b2c10cdcece3
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102121465"
---
# <a name="use-azure-policy-to-apply-cluster-configurations-at-scale"></a>Ölçek yapılandırmasında küme yapılandırması uygulamak için Azure Ilkesini kullanma

Azure `Microsoft.KubernetesConfiguration/sourceControlConfigurations` yay etkinleştirilmiş Kubernetes kümeleri () üzerinde ölçekli yapılandırma (kaynak türü) uygulamak Için Azure ilkesini kullanabilirsiniz `Microsoft.Kubernetes/connectedclusters` .

Azure Ilkesini kullanmak için, var olan bir ilke tanımı seçin ve bir ilke ataması oluşturun. İlke atamasını oluştururken:
1. Atama için kapsamı ayarlayın.
    * Kapsam bir Azure Kaynak grubu veya aboneliği olacaktır. 
2. Oluşturulacak yapılandırmanın parametrelerini ayarlayın. 

Atama oluşturulduktan sonra, Azure Ilke altyapısı kapsamda bulunan tüm Azure Arc etkin Kubernetes kümelerini tanımlar ve her kümeye yapılandırmayı uygular.

Birden çok ilke ataması kullanarak, her biri farklı bir git deposuna işaret eden birden çok yapılandırma oluşturabilirsiniz. Örneğin, merkezi BT/küme operatörü ve uygulama ekiplerine yönelik diğer depolar için bir depo.

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

İlke atamasını oluşturduktan sonra yapılandırma, ilke atamasının kapsamında oluşturulan yeni Azure Arc etkin Kubernetes kümelerine uygulanır.

Mevcut kümeler için bir düzeltme görevini el ile çalıştırmanız gerekir. Bu görev, ilke atamasının etkili olması için genellikle 10 ila 20 dakika sürer.

## <a name="verify-a-policy-assignment"></a>İlke atamasını doğrulama

1. Azure portal, Azure Arc etkin Kubernetes kümelerinden birine gidin.
1. Kenar çubuğunun **Ayarlar** bölümünde **ilkeler**' i seçin. 
    * İlkeler listesinde, daha önce oluşturduğunuz ilke atamasını **uyumluluk durumu** ile *uyumlu* olarak ayarlanmış olarak görmeniz gerekir.
1. Kenar çubuğunun **Ayarlar** bölümünde, **Konfigürasyonlar**' ı seçin.
    * Yapılandırmalar listesinde, ilke ataması tarafından oluşturulan yapılandırmayı görmeniz gerekir.
1. `kubectl`Kümeyi sorgulanamıyor için kullanın. 
    * Yapılandırma kaynakları tarafından oluşturulan ad alanını ve yapıtları görmeniz gerekir.
    * 5 dakika içinde (kümenin Azure ile ağ bağlantısı olduğunu varsayarsak), git deposunda bildirimler tarafından tanımlanan nesneleri küme üzerinde oluşturulmuş olarak görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Arc etkinleştirilmiş Kubernetes kümeleriyle kapsayıcılar Için Azure izleyici 'Yi ayarlayın](../../azure-monitor/containers/container-insights-enable-arc-enabled-clusters.md).
