---
title: İlke uyumluluğunu denetleme ve yönetme
titleSuffix: Azure Machine Learning
description: Çalışma alanlarınızın gereksinimlerinize uygun olduğundan emin olmak üzere Azure Machine Learning için yerleşik ilkeleri kullanmak üzere Azure Ilkesi 'ni nasıl kullanacağınızı öğrenin.
author: aashishb
ms.author: aashishb
ms.date: 03/12/2021
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: larryfr
ms.openlocfilehash: 21b07130e99ad4fac9a0a9b2d11aca852a1f205f
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104584321"
---
# <a name="audit-and-manage-azure-machine-learning-using-azure-policy"></a>Azure Ilkesi kullanarak Azure Machine Learning denetleme ve yönetme

[Azure ilkesi](../governance/policy/index.yml) , Azure kaynaklarının ilkelerinizle uyumlu olduğundan emin olmanızı sağlayan bir idare aracıdır. Azure Machine Learning, aşağıdaki ilkeleri atayabilirsiniz:

* **Müşteri tarafından yönetilen anahtar**: çalışma alanlarının müşteri tarafından yönetilen anahtar kullanması gerekip gerekmediğini denetleyin veya zorlayın.
* **Özel bağlantı**: çalışma alanlarının bir sanal ağla iletişim kurmak için özel uç nokta kullanıp kullanmadığını denetleyin veya zorlayın.
* **Özel uç nokta**: özel uç noktanın oluşturulması gereken Azure sanal ağ alt ağını yapılandırın.
* **Özel DNS bölgesi**: özel bağlantı için kullanılacak özel DNS bölgesini yapılandırın.

İlkeler, abonelik veya kaynak grubu düzeyinde gibi farklı kapsamlardan ayarlanabilir. Daha fazla bilgi için bkz. [Azure ilkesi belgeleri](../governance/policy/overview.md).

## <a name="built-in-policies"></a>Yerleşik ilkeler

Azure Machine Learning, Azure Machine Learning ortak senaryolar için kullanabileceğiniz bir ilke kümesi sağlar. Bu ilke tanımlarını mevcut aboneliğinize atayabilir veya kendi özel tanımlarınızı oluşturmak için temel olarak kullanabilirsiniz. Azure Machine Learning yerleşik ilkelerinin tam bir listesi için, bkz. [Azure Machine Learning Için yerleşik ilkeler](../governance/policy/samples/built-in-policies.md#machine-learning).

Azure Machine Learning ilgili yerleşik ilke tanımlarını görüntülemek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com) __Azure ilkesi__ ' ne gidin.
1. __Tanımlar__' ı seçin.
1. __Tür__ için _yerleşik_' i seçin ve __Kategori__ için __Machine Learning__' yi seçin.

Buradan görüntülenecek ilke tanımlarını seçebilirsiniz. Bir tanımı görüntülerken, ilkeyi belirli bir kapsama atamak için __ata__ bağlantısını kullanabilir ve ilke için parametreleri yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Ilke atama-Portal](../governance/policy/assign-policy-portal.md).

[Azure PowerShell](../governance/policy/assign-policy-powershell.md), [Azure CLI](../governance/policy/assign-policy-azurecli.md)ve [şablonları](../governance/policy/assign-policy-template.md)kullanarak da ilke atayabilirsiniz.

## <a name="workspace-encryption-with-customer-managed-key"></a>Müşteri tarafından yönetilen anahtarla çalışma alanı şifrelemesi

Bir çalışma alanının müşteri tarafından yönetilen bir anahtarla şifrelenip şifrelenmeyeceğini veya ölçümleri ve meta verileri şifrelemek için Microsoft tarafından yönetilen bir anahtar kullanıp kullanmadığını denetler. Müşteri tarafından yönetilen anahtarı kullanma hakkında daha fazla bilgi için veri şifreleme makalesinin [Azure Cosmos DB](concept-data-encryption.md#azure-cosmos-db) bölümüne bakın.

Bu ilkeyi yapılandırmak için, efekt parametresini __Denetim__ veya __reddetme__ olarak ayarlayın. __Denetim__ olarak ayarlanırsa, müşteri tarafından yönetilen anahtar olmadan bir çalışma alanı oluşturabilirsiniz ve etkinlik günlüğünde bir uyarı olayı oluşturulur.

İlke __Reddet__ olarak ayarlandıysa, müşteri tarafından yönetilen bir anahtar belirtmediği takdirde bir çalışma alanı oluşturamazsınız. Müşteri tarafından yönetilen anahtar olmadan bir çalışma alanı oluşturmaya çalışmak, şuna benzer bir hatayla sonuçlanır `Resource 'clustername' was disallowed by policy` ve etkinlik günlüğünde bir hata oluşturur. İlke tanımlayıcısı bu hatanın bir parçası olarak da döndürülür.

## <a name="workspace-should-use-private-link"></a>Çalışma alanı özel bağlantı kullanmalıdır

Bir çalışma alanının Azure sanal ağı ile iletişim kurmak için Azure özel bağlantısı kullanması gerekip gerekmediğini denetler. Özel bağlantı kullanma hakkında daha fazla bilgi için bkz. [bir çalışma alanı için özel bağlantı yapılandırma](how-to-configure-private-link.md).

Bu ilkeyi yapılandırmak için, efekt parametresini __Denetim__ veya __reddetme__ olarak ayarlayın. __Denetim__ olarak ayarlanırsa, özel bağlantı kullanmadan bir çalışma alanı oluşturabilirsiniz ve etkinlik günlüğünde bir uyarı olayı oluşturulur.

İlke __Reddet__ olarak ayarlandıysa, özel bir bağlantı kullanmadığı takdirde bir çalışma alanı oluşturamazsınız. Özel bağlantı olmadan çalışma alanı oluşturma girişimi bir hatayla sonuçlanır. Hata ayrıca etkinlik günlüğüne kaydedilir. İlke tanımlayıcısı bu hatanın bir parçası olarak döndürülür.

## <a name="workspace-should-use-private-endpoint"></a>Çalışma alanı özel uç nokta kullanmalıdır

Bir Azure sanal ağının belirtilen alt ağı içinde özel bir uç nokta oluşturmak için bir çalışma alanı yapılandırır.

Bu ilkeyi yapılandırmak için, efekt parametresini __Deployifnotexists__ olarak ayarlayın. __Privateendpointsubnetıd__ 'yi alt AĞıN Azure Resource Manager kimliği olarak ayarlayın.
## <a name="workspace-should-use-private-dns-zones"></a>Çalışma alanı özel DNS bölgelerini kullanmalıdır

Özel bir uç nokta için varsayılan DNS çözümlemesini geçersiz kılarak özel bir DNS bölgesi kullanmak üzere bir çalışma alanı yapılandırır.

Bu ilkeyi yapılandırmak için, efekt parametresini __Deployifnotexists__ olarak ayarlayın. __PrivateDnsZoneId__ öğesini kullanmak IÇIN özel DNS BÖLGESININ Azure Resource Manager kimliği olarak ayarlayın. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İlkesi belgeleri](../governance/policy/overview.md)
* [Azure Machine Learning için yerleşik ilkeler](policy-reference.md)
* [Azure Güvenlik Merkezi ile güvenlik ilkeleriyle çalışma](../security-center/tutorial-security-policy.md)