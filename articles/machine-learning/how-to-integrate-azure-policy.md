---
title: İlke uyumluluğunu denetleme ve yönetme
titleSuffix: Azure Machine Learning
description: Çalışma alanlarınızın gereksinimlerinize uygun olduğundan emin olmak üzere Azure Machine Learning için yerleşik ilkeleri kullanmak üzere Azure Ilkesi 'ni nasıl kullanacağınızı öğrenin.
author: jhirono
ms.author: jhirono
ms.date: 09/15/2020
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: larryfr
ms.openlocfilehash: 22901c4e8409fc4846c1566a57b2679f4fa92396
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94444577"
---
# <a name="audit-and-manage-azure-machine-learning-using-azure-policy"></a>Azure Ilkesi kullanarak Azure Machine Learning denetleme ve yönetme

[Azure ilkesi](../governance/policy/index.yml) , Azure kaynaklarının ilkelerinizle uyumlu olduğundan emin olmanızı sağlayan bir idare aracıdır. Azure Machine Learning, aşağıdaki ilkeleri atayabilirsiniz:

* **Müşteri tarafından yönetilen anahtar** : çalışma alanlarının müşteri tarafından yönetilen anahtar kullanması gerekip gerekmediğini denetleyin veya zorlayın.
* **Özel bağlantı** : çalışma alanlarının bir sanal ağla iletişim kurmak için özel uç nokta kullanıp kullanmadığını denetleyin.

İlkeler, abonelik veya kaynak grubu düzeyinde gibi farklı kapsamlardan ayarlanabilir. Daha fazla bilgi için bkz. [Azure ilkesi belgeleri](../governance/policy/overview.md).

## <a name="built-in-policies"></a>Yerleşik ilkeler

Azure Machine Learning, Azure Machine Learning ortak senaryolar için kullanabileceğiniz bir ilke kümesi sağlar. Bu ilke tanımlarını mevcut aboneliğinize atayabilir veya kendi özel tanımlarınızı oluşturmak için temel olarak kullanabilirsiniz. Azure Machine Learning yerleşik ilkelerinin tam bir listesi için, bkz. [Azure Machine Learning Için yerleşik ilkeler](../governance/policy/samples/built-in-policies.md#machine-learning).

Azure Machine Learning ilgili yerleşik ilke tanımlarını görüntülemek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com) __Azure ilkesi__ ' ne gidin.
1. __Tanımlar__ ' ı seçin.
1. __Tür__ için _yerleşik_ ' i seçin ve __Kategori__ için __Machine Learning__ ' yi seçin.

Buradan görüntülenecek ilke tanımlarını seçebilirsiniz. Bir tanımı görüntülerken, ilkeyi belirli bir kapsama atamak için __ata__ bağlantısını kullanabilir ve ilke için parametreleri yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Ilke atama-Portal](../governance/policy/assign-policy-portal.md).

[Azure PowerShell](../governance/policy/assign-policy-powershell.md), [Azure CLI](../governance/policy/assign-policy-azurecli.md)ve [şablonları](../governance/policy/assign-policy-template.md)kullanarak da ilke atayabilirsiniz.

## <a name="workspaces-encryption-with-customer-managed-key"></a>Müşteri tarafından yönetilen anahtarla çalışma alanları şifrelemesi

Çalışma alanlarının, müşteri tarafından yönetilen bir anahtarla mi şifrelendiğini yoksa ölçümleri ve meta verileri şifrelemek için Microsoft tarafından yönetilen bir anahtar mı kullandığını denetler. Müşteri tarafından yönetilen anahtarı kullanma hakkında daha fazla bilgi için veri şifreleme makalesinin [Azure Cosmos DB](concept-data-encryption.md#azure-cosmos-db) bölümüne bakın.

Bu ilkeyi yapılandırmak için, efekt parametresini __Denetim__ veya __reddetme__ olarak ayarlayın. __Denetim__ olarak ayarlanırsa, müşteri tarafından yönetilen anahtar olmadan çalışma alanları oluşturabilir ve etkinlik günlüğünde bir uyarı olayı oluşturulur.

İlke __Reddet__ olarak ayarlandıysa, müşteri tarafından yönetilen bir anahtar belirtmediği takdirde bir çalışma alanı oluşturamazsınız. Müşteri tarafından yönetilen anahtar olmadan bir çalışma alanı oluşturmaya çalışmak, şuna benzer bir hatayla sonuçlanır `Resource 'clustername' was disallowed by policy` ve etkinlik günlüğünde bir hata oluşturur. İlke tanımlayıcısı bu hatanın bir parçası olarak da döndürülür.

## <a name="workspaces-should-use-private-link"></a>Çalışma alanlarının özel bağlantı kullanması gerekir

Çalışma alanlarının Azure sanal ağı ile iletişim kurmak için Azure özel bağlantısı kullanması gerekip gerekmediğini denetler. Özel bağlantı kullanma hakkında daha fazla bilgi için bkz. [bir çalışma alanı için özel bağlantı yapılandırma](how-to-configure-private-link.md).

Bu ilkeyi yapılandırmak için, efekt parametresini __audit__ olarak ayarlayın. Özel bağlantı kullanmadan bir çalışma alanı oluşturursanız, etkinlik günlüğünde bir uyarı olayı oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İlkesi belgeleri](../governance/policy/overview.md)
* [Azure Machine Learning için yerleşik ilkeler](policy-reference.md)
* [Azure Güvenlik Merkezi ile güvenlik ilkeleriyle çalışma](../security-center/tutorial-security-policy.md)