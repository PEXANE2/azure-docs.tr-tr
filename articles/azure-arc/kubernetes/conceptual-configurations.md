---
title: Konfigürasyonlar ve Gilar-Azure yay etkin Kubernetes
services: azure-arc
ms.service: azure-arc
ms.date: 02/17/2021
ms.topic: conceptual
author: shashankbarsin
ms.author: shasb
description: Bu makalede, Azure Arc etkin Kubernetes 'in Gilar ve yapılandırma özelliklerine kavramsal bir genel bakış sunulmaktadır.
keywords: Kubernetes, yay, Azure, kapsayıcılar, yapılandırma, Gilar
ms.openlocfilehash: f8fe1522eee4cc855ae1f396d9c98323114a25ce
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100652556"
---
# <a name="configurations-and-gitops-with-azure-arc-enabled-kubernetes"></a>Azure Arc etkin Kubernetes ile yapılandırma ve Gilar

Kubernetes ile ilgili olarak, Giüstler, bir git deposunda Kubernetes küme yapılandırmalarının (dağıtımlar, ad alanları vb.) istenen durumunu bildirme uygulamasıdır. Bu bildirimin ardından bir işleç kullanarak bu küme yapılandırmalarının yoklaması ve çekme tabanlı dağıtımı gelir. Git deposu şunları içerebilir:
* YAML-ad alanları, ConfigMaps, dağıtımlar, DaemonSets vb. gibi geçerli bir Kubernetes kaynağını tanımlayan bildirimleri biçimlendirir.
* Uygulamaları dağıtmak için Held grafikleri.

Bir git deposundan bir Kubernetes kümesine yapılandırmaların akışını kolaylaştırmak için, Gilar alanındaki popüler bir açık kaynak aracı olan [Flox](https://docs.fluxcd.io/), Kubernetes kümesine dağıtılabilir. Flox, hem küme hem de ad alanı kapsamlarındaki işlecinin dağıtımını destekler. Ad alanı kapsamıyla dağıtılan bir Flox işleci, yalnızca ilgili ad alanı içinde Kubernetes nesnelerini dağıtabilir. Küme veya ad alanı kapsamı arasında seçim yapma özelliği, aynı Kubernetes kümesinde çok kiracılı dağıtım desenleri almanıza yardımcı olur.

## <a name="configurations"></a>Yapılandırmalar

[![Yapılandırma mimarisi ](./media/conceptual-configurations.png)](./media/conceptual-configurations.png#lightbox)

Kümeniz ve git deposu arasındaki bağlantı, `Microsoft.KubernetesConfiguration/sourceControlConfigurations` Azure Arc etkin Kubernetes kaynağı (tarafından temsil edilen) üzerinde Azure Resource Manager içinde bir uzantı kaynağı olarak oluşturulur `Microsoft.Kubernetes/connectedClusters` . 

Kaynak özellikleri, bildirim almak için gereken `sourceControlConfiguration` Git deposu ve bunların alınacağı yoklama aralığı gibi uygun parametrelerle küme üzerinde Flox operatörü dağıtmak için kullanılır. Veriler, `sourceControlConfiguration` verilerin gizliliğini sağlamak için şifrelenmiş ve geri kalan bir Azure Cosmos DB veritabanında depolanır.

`config-agent`Kümenizde çalışan şu şekilde sorumludur:
* `sourceControlConfiguration`Azure Arc etkin Kubernetes kaynağında yeni veya güncelleştirilmiş uzantı kaynaklarını izleme.
* Her biri için Git deposunu izlemek üzere Flox işleci dağıtma `sourceControlConfiguration` . '
* Herhangi bir güncelleştirme uygulanıyor `sourceControlConfiguration` . 

`sourceControlConfiguration`Çoklu kiracı elde etmek için aynı Azure Arc etkin Kubernetes kümesinde birden çok ad alanı kapsamlı kaynak oluşturabilirsiniz.

> [!NOTE]
> * `config-agent``sourceControlConfiguration`Azure Arc etkin Kubernetes kaynağında bulunan yeni veya güncelleştirilmiş uzantı kaynaklarını sürekli olarak izler. Bu nedenle, aracıların istenen durum özelliklerini kümeye çekmek için tutarlı bir bağlantı gerekir. Aracılar Azure 'a bağlanamazsa, istenen durum kümede uygulanmaz.
> * Özel anahtar, bilinen ana bilgisayar içeriği, HTTPS Kullanıcı adı ve belirteç veya parola gibi hassas müşteri girişleri, Azure Arc etkinleştirilmiş Kubernetes hizmetlerinde 48 saate kadar saklanır. Yapılandırma için hassas girişler kullanıyorsanız, kümeleri mümkün olduğunca düzenli olarak çevrimiçi duruma getirin.

## <a name="apply-configurations-at-scale"></a>Yapılandırma ölçeğini ölçekli uygulama

Azure Resource Manager yapılandırmalarını yönettiğinden, bir aboneliğin veya bir kaynak grubunun kapsamındaki Azure Ilkesi 'ni kullanarak tüm Azure Arc etkin Kubernetes kaynakları genelinde aynı yapılandırmanın oluşturulmasını otomatik hale getirebilirsiniz. 

Bu ölçekli uygulama, ortak bir temel yapılandırma (ClusterRoleBindings, RoleBindings ve NetworkPolicy gibi yapılandırmalar içeren), Azure Arc etkinleştirilmiş Kubernetes kümelerinin tamamına veya envanterine yönelik olarak bir dünya genelinde uygulanabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir kümeyi Azure yaya bağlama](./connect-cluster.md)
* [Arc etkin Kubernetes kümesinde yapılandırma oluşturma](./use-gitops-connected-cluster.md)
* [Uygun ölçekte yapılandırma uygulamak için Azure Ilkesini kullanma](./use-azure-policy.md)