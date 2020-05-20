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
ms.openlocfilehash: 64be872e4e76545c015378417410cbbfad95ebfd
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83680670"
---
# <a name="use-azure-policy-to-apply-cluster-configurations-at-scale-preview"></a>Ölçek yapılandırmasında küme yapılandırması uygulamak için Azure Ilkesi kullanma (Önizleme)

## <a name="overview"></a>Genel Bakış

Her bir `Microsoft.Kubernetes/connectedclusters` Git Ops etkin `Microsoft.ContainerService/managedClusters` kaynağın uygulanmasını zorlamak Için Azure ilkesini kullanın `Microsoft.KubernetesConfiguration/sourceControlConfigurations` .  Azure Ilkesini kullanmak için var olan bir ilke tanımı seçin ve bir ilke ataması oluşturun.  İlke atamasını oluştururken atama için kapsamı ayarlarsınız: Bu, bir Azure Kaynak grubu veya aboneliği olacaktır.  Oluşturulacak için parametreleri de ayarlarsınız `sourceControlConfiguration` .  Atama oluşturulduktan sonra, Ilke altyapısı `connectedCluster` `managedCluster` kapsam içinde bulunan tüm veya kaynakları belirler ve `sourceControlConfiguration` her birine uygular.

Her küme için Truth kaynakları olarak birden çok git deposu kullanıyorsanız (örneğin, merkezi BT/küme operatörü için bir depo ve uygulama ekiplerine yönelik diğer depolar), bunu, her ilke atamasını farklı bir git deposu kullanacak şekilde yapılandırılmış birden fazla ilke ataması kullanarak etkinleştirebilirsiniz.

## <a name="create-a-custom-policy-definition"></a>Özel ilke tanımı oluşturma

1. Azure portal, Ilke ' ye gidin ve kenar çubuğunun **yazma** bölümünde **tanımlar**' ı seçin.
2. **+ İlke tanımı**' nı seçin.
3. **Tanım konumunu** aboneliğiniz veya yönetim grubunuz olarak ayarlayın.  Bu, ilke tanımının kullanılabileceği en geniş kapsamı tespit eder.
4. İlkeye bir **ad** ve **Açıklama**verin.
5. Kategori altında **Yeni oluştur**' u seçin ve *Kubernetes kümesi yazın-Azure Arc*
6. **İlke kuralı** düzenleme kutusunda, bu [örnek ilke tanımının](https://raw.githubusercontent.com/Azure/arc-k8s-demo/master/policy/Ensure-GitOps-configuration-for-Kubernetes-cluster.json)içeriğini kopyalayın/yapıştırın.
7. **Kaydedin**.

Özel bir ilke tanımı oluşturmak için bu adım, iş tamamlandıktan sonra bir yerleşik ilke haline getirildikten sonra gerekli olmayacaktır.

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

1. Azure portal, Ilke ' ye gidin ve kenar çubuğunun **yazma** bölümünde **tanımlar**' ı seçin.
2. Yeni oluşturduğunuz tanımı bulun ve seçin.
3. Sayfa eylemleri ' nde **ata**' yı seçin.
4. **Kapsamı** , ilke atamasının uygulanacağı yönetim grubu, abonelik veya kaynak grubu olarak ayarlayın.
5. İlke kapsamından herhangi bir kaynağı dışlamak istiyorsanız, **dışlamaları**ayarlayın.
6. İlke atamaya, kolayca tanımlamak için kullanabileceğiniz bir **ad** ve **Açıklama** sağlayın.
7. **İlke zorlamasının** *etkin*olarak ayarlandığından emin olun.
8. **İleri**’yi seçin.
9. Öğesinin oluşturulması sırasında kullanılacak parametre değerlerini ayarlayın `sourceControlConfiguration` .
10. **İleri**’yi seçin.
11. **Düzeltme görevi oluşturma**özelliğini etkinleştirin.
12. **Yönetilen bir kimlik oluşturmak** ve kimliğin **katkıda** bulunan izinlerine sahip olması güvence altına alınır.  İhtiyacınız olan izinler hakkında daha fazla bilgi [için bu belgeyi ve](https://docs.microsoft.com/azure/governance/policy/assign-policy-portal) [Bu belgedeki yorumu](https://docs.microsoft.com/azure/governance/policy/how-to/remediate-resources) inceleyin.
13. **İncele ve oluştur**’u seçin.

İlke ataması oluşturulduktan sonra, `connectedCluster` atama kapsamında yer alan herhangi bir yeni kaynak (veya `managedCluster` Gile aracıları yüklü olan kaynak) için `sourceControlConfiguration` uygulanır.  Mevcut kümeler için bir düzeltme görevini el ile çalıştırmanız gerekir.  İlke atamasının etkili olması için genellikle 10-20 dakika sürer.

## <a name="verify-a-policy-assignment"></a>İlke atamasını doğrulama

1. Azure portal `connectedCluster` kaynaklarınızdan birine gidin ve kenar çubuğunun **Ayarlar** bölümünde **ilkeler**' i seçin. (AKS tarafından yönetilen küme için UX henüz uygulanmadı, ancak geliyor.)
2. Listede, yukarıda oluşturduğunuz ilke atamasını görmeniz ve **Uyumluluk durumunun** *uyumlu*olması gerekir.
3. Kenar çubuğunun **Ayarlar** bölümünde, **Konfigürasyonlar**' ı seçin.
4. Listede, `sourceControlConfiguration` ilke atamasının oluşturulduğunu görmeniz gerekir.
5. Kümeyi sorgulanamıyor için **kubectl** kullanın: tarafından oluşturulan ad alanını ve yapıtları görmeniz gerekir `sourceControlConfiguration` .
6. 5 dakika içinde, yapılandırılan git deposundaki bildirimlerde açıklanan yapıtları kümede görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Küme yapılandırmasını yönetmek için Azure Ilkesini kullanma](./use-azure-policy.md)
