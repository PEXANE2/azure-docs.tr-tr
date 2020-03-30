---
title: Çalışma alanınızdaki rolleri yönetme
titleSuffix: Azure Machine Learning
description: Rol tabanlı erişim denetimini (RBAC) kullanarak Azure Machine Learning çalışma alanına nasıl erişin gerektiğini öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: larryfr
author: Blackmist
ms.date: 03/06/2020
ms.custom: seodec18
ms.openlocfilehash: 127a0a2b7f7573db91df9347169e90de3e14c4c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270101"
---
# <a name="manage-access-to-an-azure-machine-learning-workspace"></a>Azure Machine Learning çalışma alanına erişimi yönetme
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning çalışma alanına erişimi nasıl yönetdiğinizi öğreneceksiniz. [Rol tabanlı erişim denetimi (RBAC),](/azure/role-based-access-control/overview) Azure kaynaklarına erişimi yönetmek için kullanılır. Azure Etkin Dizininizdeki kullanıcılara, kaynaklara erişim sağlayan belirli roller atanır. Azure hem yerleşik roller hem de özel roller oluşturma olanağı sağlar.

## <a name="default-roles"></a>Varsayılan roller

Azure Machine Learning çalışma alanı bir Azure kaynağıdır. Diğer Azure kaynakları gibi, yeni bir Azure Machine Learning çalışma alanı oluşturulduğunda da üç varsayılan rol gelir. Kullanıcıları çalışma alanına ekleyebilir ve bunları bu yerleşik rollerden birine atayabilirsiniz.

| Rol | Erişim düzeyi |
| --- | --- |
| **Okuyucu** | Çalışma alanında salt okunur eylemler. Okuyucular bir çalışma alanında varlıkları listeleyebilir ve görüntüleyebilir, ancak bu varlıkları oluşturamaz veya güncelleştiremez. |
| **Katkıda Bulunan** | Çalışma alanında varlıkları görüntüleyin, oluşturun, düzenleyin veya silin (varsa) Örneğin, katkıda bulunanlar bir deneme oluşturabilir, bir işlem kümesi oluşturabilir veya ekleyebilir, bir çalıştırma gönderebilir ve bir web hizmeti dağıtabilir. |
| **Sahibi** | Çalışma alanında varlıkları görüntüleme, oluşturma, düzenleme veya silme (varsa) varlıkları da dahil olmak üzere çalışma alanına tam erişim. Ayrıca, rol atamaları değiştirebilirsiniz. |

> [!IMPORTANT]
> Rol erişimi Azure'da birden çok düzeye kadar kapsama bulaşabilir. Örneğin, çalışma alanına sahip olan bir kişinin çalışma alanını içeren kaynak grubuna sahip erişimi olmayabilir. Daha fazla bilgi için [RBAC'ın nasıl çalıştığını](/azure/role-based-access-control/overview#how-rbac-works)görün.

Belirli yerleşik roller hakkında daha fazla bilgi için [Azure için Yerleşik rollere](/azure/role-based-access-control/built-in-roles)bakın.

## <a name="manage-workspace-access"></a>Çalışma alanı erişimini yönetme

Bir çalışma alanının sahibiyseniz, çalışma alanı için roller ekleyebilir ve kaldırabilirsiniz. Ayrıca kullanıcılara roller atayabilirsiniz. Erişimi nasıl yöneteceklerini keşfetmek için aşağıdaki bağlantıları kullanın:
- [Azure portalı UI](/azure/role-based-access-control/role-assignments-portal)
- [Powershell](/azure/role-based-access-control/role-assignments-powershell)
- [Azure CLI](/azure/role-based-access-control/role-assignments-cli)
- [REST API](/azure/role-based-access-control/role-assignments-rest)
- [Azure Kaynak Yöneticisi şablonları](/azure/role-based-access-control/role-assignments-template)

Azure Machine Learning [CLI'yi](reference-azure-machine-learning-cli.md)yüklediyseniz, kullanıcılara rol atamak için cli komutunu da kullanabilirsiniz.

```azurecli-interactive 
az ml workspace share -w <workspace_name> -g <resource_group_name> --role <role_name> --user <user_corp_email_address>
```

Alan, `user` çalışma alanı üst aboneliğinin yaşadığı Azure Etkin Dizini örneğinde varolan bir kullanıcının e-posta adresidir. Bu komutun nasıl kullanılacağına bir örnek aşağıda verilmiştir:

```azurecli-interactive 
az ml workspace share -w my_workspace -g my_resource_group --role Contributor --user jdoe@contoson.com
```

## <a name="create-custom-role"></a>Özel rol oluştur

Yerleşik roller yetersizse, özel roller oluşturabilirsiniz. Özel roller, bu çalışma alanında kaynak izinlerini okumuş, yazmış, silebilir ve işlem üstleyebilir. Rolü belirli bir çalışma alanı düzeyinde, belirli bir kaynak grubu düzeyinde veya belirli bir abonelik düzeyinde kullanılabilir hale getirebilirsiniz.

> [!NOTE]
> Bu kaynak içinde özel roller oluşturmak için bu düzeyde kaynağın sahibi olmalısınız.

Özel bir rol oluşturmak için, önce rolün izin ve kapsamını belirten bir rol tanımı JSON dosyası oluşturun. Aşağıdaki örnek, belirli bir çalışma alanı düzeyinde kapsamlı "Veri Bilim Adamı" adlı özel bir rol tanımlar:

`data_scientist_role.json` :
```json
{
    "Name": "Data Scientist",
    "IsCustom": true,
    "Description": "Can run experiment but can't create or delete compute.",
    "Actions": ["*"],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/*/delete",
        "Microsoft.MachineLearningServices/workspaces/computes/*/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/delete", 
        "Microsoft.Authorization/*/write"
    ],
    "AssignableScopes": [
        "/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace_name>"
    ]
}
```

`AssignableScopes` Alanı, bu özel rolün kapsamını abonelik düzeyinde, kaynak grubu düzeyinde veya belirli bir çalışma alanı düzeyinde ayarlamak üzere değiştirebilirsiniz.

Bu özel rol, aşağıdaki eylemler dışında çalışma alanındaki her şeyi yapabilir:

- Bir bilgi işlem kaynağı oluşturamaz veya güncelleştiremez.
- Bir bilgi işlem kaynağını silemez.
- Rol atamaları ekleyebilir, silemez veya değiştiremez.
- Çalışma alanını silemez.

Bu özel rolü dağıtmak için aşağıdaki Azure CLI komutunu kullanın:

```azurecli-interactive 
az role definition create --role-definition data_scientist_role.json
```

Dağıtımdan sonra, bu rol belirtilen çalışma alanında kullanılabilir hale gelir. Artık bu rolü Azure portalına ekleyebilir ve atayabilirsiniz. Veya CLI komutunu `az ml workspace share` kullanarak bu rolü bir kullanıcıya atayabilirsiniz:

```azurecli-interactive
az ml workspace share -w my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```

Özel roller hakkında daha fazla bilgi için [Azure kaynakları için Özel rolleri](/azure/role-based-access-control/custom-roles)görün.

Özel rollerle kullanılabilir işlemler (eylemler) hakkında daha fazla bilgi için [Kaynak sağlayıcı işlemlerine](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices)bakın.


## <a name="frequently-asked-questions"></a>Sık sorulan sorular


### <a name="q-what-are-the-permissions-needed-to-perform-various-actions-in-the-azure-machine-learning-service"></a>S. Azure Machine Learning hizmetinde çeşitli eylemleri gerçekleştirmek için gereken izinler nelerdir?

Aşağıdaki tablo, Azure Machine Learning etkinliklerinin ve bunları en az kapsamda gerçekleştirmek için gereken izinlerin bir özetidir. Örnek olarak, bir etkinlik çalışma alanı kapsamıyla (Sütun 4) gerçekleştirilebiliyorsa, bu izinle birlikte tüm yüksek kapsam da otomatik olarak çalışır. Bu tablodaki tüm yollar `Microsoft.MachineLearningServices/` **göreli yollardır.**

| Etkinlik | Abonelik düzeyi kapsamı | Kaynak grup düzeyinde kapsam | Çalışma alanı düzeyi kapsamı |
|---|---|---|---|
| Yeni çalışma alanı oluşturma | Gerekli değil | Sahibi veya katılımcısı | N/A (Sahip olur veya oluşturulduktan sonra daha yüksek kapsam rolü devralır) |
| Yeni işlem kümesi oluşturma | Gerekli değil | Gerekli değil | Sahibi, katkıda bulunan veya özel role izin veren:`workspaces/computes/write` |
| Yeni Notebook VM oluşturma | Gerekli değil | Sahibi veya katılımcısı | Mümkün değil |
| Yeni işlem örneği oluşturma | Gerekli değil | Gerekli değil | Sahibi, katkıda bulunan veya özel role izin veren:`workspaces/computes/write` |
| Çalıştırma gönderme, verilere erişim, model dağıtma veya ardışık kaynak yayımlama gibi veri düzlemi etkinliği | Gerekli değil | Gerekli değil | Sahibi, katkıda bulunan veya özel role izin veren:`workspaces/*/write` <br/> MSI'ın depolama hesabınızdaki verilere erişmesine izin vermek için çalışma alanına kayıtlı bir veri deposuna da ihtiyacınız olduğunu unutmayın. |


### <a name="q-how-do-i-list-all-the-custom-roles-in-my-subscription"></a>S. Aboneliğimdeki tüm özel rolleri nasıl listelerim?

Azure CLI'de aşağıdaki komutu çalıştırın.

```azurecli-interactive
az role definition list --subscription <sub-id> --custom-role-only true
```

### <a name="q-how-do-i-find-the-role-definition-for-a-role-in-my-subscription"></a>S. Aboneliğimdeki bir rolün rol tanımını nasıl bulabilirim?

Azure CLI'de aşağıdaki komutu çalıştırın. Yukarıdaki `<role-name>` komutla döndürülen aynı biçimde olması gerektiğini unutmayın.

```azurecli-interactive
az role definition list -n <role-name> --subscription <sub-id>
```

### <a name="q-how-do-i-update-a-role-definition"></a>S. Rol tanımını nasıl güncellerim?

Azure CLI'de aşağıdaki komutu çalıştırın.

```azurecli-interactive
az role definition update --role-definition update_def.json --subscription <sub-id>
```

Yeni rol tanımınızın tüm kapsamında izinlere sahip olmanız gerektiğini unutmayın. Örneğin, bu yeni rolün üç abonelik arasında bir kapsamı varsa, üç abonelikte de izinlere sahip olmanız gerekir. 

> [!NOTE]
> Rol güncelleştirmelerinin bu kapsamdaki tüm rol atamalarında uygulanması 15 dakika ile bir saat arasında sürebilir.
### <a name="q-can-i-define-a-role-that-prevents-updating-the-workspace-edition"></a>S. Çalışma alanı Sürümünü güncelleştirmeyi engelleyen bir rol tanımlayabilir miyim? 

Evet, çalışma alanı sürümünü güncelleştirmeyi engelleyen bir rol tanımlayabilirsiniz. Çalışma alanı güncelleştirmesi çalışma alanı nesnesi üzerinde bir PATCH çağrısı olduğundan, JSON tanımınızdaki `"NotActions"` diziye aşağıdaki eylemi koyarak bunu yaparsınız: 

`"Microsoft.MachineLearningServices/workspaces/write"`

### <a name="q-what-permissions-are-needed-to-perform-quota-operations-in-a-workspace"></a>S. Çalışma alanında kota işlemleri gerçekleştirmek için hangi izinler gereklidir? 

Çalışma alanında herhangi bir kota ile ilgili işlemi gerçekleştirmek için abonelik düzeyi izinleri gerekir. Bu, yönetilen işlem kaynaklarınız için abonelik düzeyi kotası veya çalışma alanı düzeyi kotasının ayarlanması nın yalnızca abonelik kapsamında yazma izinleriniz varsa gerçekleşebileceği anlamına gelir. 


## <a name="next-steps"></a>Sonraki adımlar

- [Kurumsal güvenliğe genel bakış](concept-enterprise-security.md)
- [Sanal ağ içinde denemeleri ve çıkarımları/skoru güvenli bir şekilde çalıştırın](how-to-enable-virtual-network.md)
- [Öğretici: Tren modelleri](tutorial-train-models-with-aml.md)
- [Kaynak sağlayıcısı işlemleri](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices)
