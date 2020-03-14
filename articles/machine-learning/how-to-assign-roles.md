---
title: Çalışma alanınızdaki rolleri yönetme
titleSuffix: Azure Machine Learning
description: Rol tabanlı erişim denetimi (RBAC) kullanarak bir Azure Machine Learning çalışma alanına erişmeyi öğrenin.
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
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79270101"
---
# <a name="manage-access-to-an-azure-machine-learning-workspace"></a>Azure Machine Learning çalışma alanına erişimi yönetme
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning çalışma alanına erişimi yönetmeyi öğreneceksiniz. [Rol tabanlı erişim denetimi (RBAC)](/azure/role-based-access-control/overview) , Azure kaynaklarına erişimi yönetmek için kullanılır. Azure Active Directory kullanıcılara, kaynaklara erişim izni veren belirli roller atanır. Azure, yerleşik roller ve özel roller oluşturma yeteneği sağlar.

## <a name="default-roles"></a>Varsayılan roller

Azure Machine Learning çalışma alanı bir Azure kaynağıdır. Diğer Azure kaynaklarında olduğu gibi yeni bir Azure Machine Learning çalışma alanı da oluşturulduğunda üç varsayılan role sahip olur. Çalışma alanına kullanıcı ekleyebilir ve bunları bu yerleşik rollerden birine atayabilirsiniz.

| Rol | Erişim düzeyi |
| --- | --- |
| **Okuyucu** | Çalışma alanında salt okunur eylemler. Okuyucular çalışma alanındaki varlıkları listeleyip görüntüleyebilir ancak yeni varlık oluşturamaz ve var olan varlıkları güncelleştiremez. |
| **Katılımcı** | Çalışma alanındaki varlıkları görüntüleme, oluşturma, düzenleme veya silme (geçerli durumlarda). Örneğin katkıda bulunanlar bir deneme oluşturabilir, işlem kümesi oluşturabilir veya ekleyebilir, çalıştırma gönderebilir ve bir web hizmeti dağıtabilir. |
| **Sahip** | Çalışma alanındaki varlıkları görüntüleme, oluşturma, düzenleme veya silme (geçerli durumlarda) dahil olmak üzere tam çalışma alanı erişimi. İsterseniz rol atamalarını değiştirebilirsiniz. |

> [!IMPORTANT]
> Rol erişimi, Azure 'da birden çok düzey kapsamına eklenebilir. Örneğin, bir çalışma alanına sahip olan birisi, çalışma alanını içeren kaynak grubuna sahip erişimine sahip olmayabilir. Daha fazla bilgi için bkz. [RBAC çalışma](/azure/role-based-access-control/overview#how-rbac-works).

Belirli yerleşik roller hakkında daha fazla bilgi için bkz. [Azure Için yerleşik roller](/azure/role-based-access-control/built-in-roles).

## <a name="manage-workspace-access"></a>Çalışma alanı erişimini yönetme

Bir çalışma alanının sahibiyseniz, çalışma alanı için rol ekleyebilir ve kaldırabilirsiniz. Diğer kullanıcılara da rol atayabilirsiniz. Erişim yönetimi süreçlerini keşfetmek için aşağıdaki bağlantıları kullanın:
- [Azure portalı kullanıcı arabirimi](/azure/role-based-access-control/role-assignments-portal)
- [PowerShell](/azure/role-based-access-control/role-assignments-powershell)
- [Azure CLI](/azure/role-based-access-control/role-assignments-cli)
- [REST API](/azure/role-based-access-control/role-assignments-rest)
- [Azure Resource Manager şablonları](/azure/role-based-access-control/role-assignments-template)

[Clı Azure Machine Learning](reference-azure-machine-learning-cli.md)yüklediyseniz, kullanıcılara roller atamak IÇIN bir CLI komutu de kullanabilirsiniz.

```azurecli-interactive 
az ml workspace share -w <workspace_name> -g <resource_group_name> --role <role_name> --user <user_corp_email_address>
```

`user` alanı, çalışma alanı üst aboneliğinin yaşadığı Azure Active Directory örneğinde mevcut bir kullanıcının e-posta adresidir. Bu komutun nasıl kullanılacağına ilişkin bir örnek aşağıda verilmiştir:

```azurecli-interactive 
az ml workspace share -w my_workspace -g my_resource_group --role Contributor --user jdoe@contoson.com
```

## <a name="create-custom-role"></a>Özel rol oluştur

Yerleşik rollerin yetersiz olması durumunda özel roller oluşturabilirsiniz. Özel rollerin bu çalışma alanında okuma, yazma, silme ve hesaplama kaynağı izinleri olabilir. Rolü belirli bir çalışma alanı düzeyinde, belirli bir kaynak grubu düzeyinde veya belirli bir abonelik düzeyinde kullanılabilir hale getirebilirsiniz.

> [!NOTE]
> Bu kaynak içinde özel Roller oluşturmak için bu düzeyde kaynağın sahibi olmanız gerekir.

Özel bir rol oluşturmak için, önce rolün iznini ve kapsamını belirten bir rol tanımı JSON dosyası oluşturun. Aşağıdaki örnek, belirli bir çalışma alanı düzeyinde "veri Bilimcst" kapsamı adlı özel bir rol tanımlar:

`data_scientist_role.json` hatasıyla başarısız oldu:
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

Bu özel rolün kapsamını abonelik düzeyinde, kaynak grubu düzeyinde veya belirli bir çalışma alanı düzeyinde ayarlamak için `AssignableScopes` alanını değiştirebilirsiniz.

Bu özel rol, aşağıdaki eylemler hariç çalışma alanındaki her şeyi gerçekleştirebilir:

- İşlem kaynağı oluşturamaz veya güncelleştiremez.
- İşlem kaynağını silemiyor.
- Rol atamalarını ekleyemez, silemez veya değiştiremez.
- Çalışma alanını silemiyor.

Bu özel rolü dağıtmak için aşağıdaki Azure CLı komutunu kullanın:

```azurecli-interactive 
az role definition create --role-definition data_scientist_role.json
```

Dağıtımdan sonra, bu rol belirtilen çalışma alanında kullanılabilir hale gelir. Artık bu rolü Azure portal ekleyip atayabilirsiniz. Ya da bu rolü bir kullanıcıya `az ml workspace share` CLı komutunu kullanarak atayabilirsiniz:

```azurecli-interactive
az ml workspace share -w my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```

Özel roller hakkında daha fazla bilgi için bkz. [Azure kaynakları Için özel roller](/azure/role-based-access-control/custom-roles).

Özel rollerle kullanılabilen işlemler (Eylemler) hakkında daha fazla bilgi için bkz. [kaynak sağlayıcısı işlemleri](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices).


## <a name="frequently-asked-questions"></a>Sık sorulan sorular


### <a name="q-what-are-the-permissions-needed-to-perform-various-actions-in-the-azure-machine-learning-service"></a>S. Azure Machine Learning hizmetinde çeşitli işlemleri gerçekleştirmek için gereken izinler nelerdir?

Aşağıdaki tabloda Azure Machine Learning etkinliklerin Özeti ve bunları en az kapsamda gerçekleştirmek için gereken izinler verilmiştir. Örnek olarak, bir etkinlik bir çalışma alanı kapsamıyla (sütun 4) gerçekleştirilebileceği takdirde, bu izne sahip tüm daha yüksek kapsam de otomatik olarak çalışacaktır. Bu tablodaki tüm yollar `Microsoft.MachineLearningServices/`**göreli yollardır** .

| Etkinlik | Abonelik düzeyi kapsamı | Kaynak grubu düzeyi kapsamı | Çalışma alanı düzeyi kapsamı |
|---|---|---|---|
| Yeni çalışma alanı oluştur | Gerekli değil | Sahip veya katkıda bulunan | Yok (oluşturulduktan sonra sahip olur veya daha yüksek kapsam rolünü devralır) |
| Yeni işlem kümesi oluştur | Gerekli değil | Gerekli değil | Sahip, katkıda bulunan veya özel rol şunları sağlar: `workspaces/computes/write` |
| Yeni Not defteri VM 'si oluşturma | Gerekli değil | Sahip veya katkıda bulunan | Mümkün değil |
| Yeni işlem örneği oluştur | Gerekli değil | Gerekli değil | Sahip, katkıda bulunan veya özel rol şunları sağlar: `workspaces/computes/write` |
| Çalıştırma, verilere erişme, model dağıtma veya yayımlama işlem hattı gibi veri düzlemi etkinliği | Gerekli değil | Gerekli değil | Sahip, katkıda bulunan veya özel rol şunları sağlar: `workspaces/*/write` <br/> Ayrıca, MSI 'ın Depolama hesabınızdaki verilere erişmesine izin vermek için çalışma alanına kayıtlı bir veri deposuna ihtiyacınız olduğunu unutmayın. |


### <a name="q-how-do-i-list-all-the-custom-roles-in-my-subscription"></a>S. Aboneliğimde tüm özel rolleri listelemek Nasıl yaparım? mı?

Azure CLı 'de aşağıdaki komutu çalıştırın.

```azurecli-interactive
az role definition list --subscription <sub-id> --custom-role-only true
```

### <a name="q-how-do-i-find-the-role-definition-for-a-role-in-my-subscription"></a>S. Nasıl yaparım? Aboneliğimde bir rolün rol tanımını bulamıyor musunuz?

Azure CLı 'de aşağıdaki komutu çalıştırın. `<role-name>` yukarıdaki komutun döndürdüğü biçimde olması gerektiğini unutmayın.

```azurecli-interactive
az role definition list -n <role-name> --subscription <sub-id>
```

### <a name="q-how-do-i-update-a-role-definition"></a>S. Nasıl yaparım? bir rol tanımı güncelleştirilsin mi?

Azure CLı 'de aşağıdaki komutu çalıştırın.

```azurecli-interactive
az role definition update --role-definition update_def.json --subscription <sub-id>
```

Yeni rol tanımınızın tüm kapsamı üzerinde izinleriniz olması gerektiğini unutmayın. Örneğin, bu yeni rolün üç abonelik arasında bir kapsamı varsa, üç abonelik için de izinleriniz olması gerekir. 

> [!NOTE]
> Rol güncelleştirmelerinin, bu kapsamdaki tüm rol atamaları üzerinde uygulanması 15 dakika ila saat arasında sürebilir.
### <a name="q-can-i-define-a-role-that-prevents-updating-the-workspace-edition"></a>S. Çalışma alanı sürümünün güncelleştirilmesini engelleyen bir rol tanımlayabilir miyim? 

Evet, çalışma alanı sürümünün güncelleştirilmesini engelleyen bir rol tanımlayabilirsiniz. Çalışma alanı güncelleştirmesi, çalışma alanı nesnesinde bir yama çağrısı olduğundan, bunu JSON tanımınızda `"NotActions"` dizisine aşağıdaki eylemi koyarak yapabilirsiniz: 

`"Microsoft.MachineLearningServices/workspaces/write"`

### <a name="q-what-permissions-are-needed-to-perform-quota-operations-in-a-workspace"></a>S. Bir çalışma alanında kota işlemleri gerçekleştirmek için hangi izinler gereklidir? 

Çalışma alanındaki kota ile ilgili herhangi bir işlemi gerçekleştirmek için abonelik düzeyi izinlerine sahip olmanız gerekir. Bu, yönetilen işlem kaynaklarınız için abonelik düzeyi kotasının veya çalışma alanı düzeyi kotasının ayarlanması, yalnızca abonelik kapsamında yazma izinleriniz varsa meydana gelebileceği anlamına gelir. 


## <a name="next-steps"></a>Sonraki adımlar

- [Kurumsal güvenliğe genel bakış](concept-enterprise-security.md)
- [Bir sanal ağ içinde denemeleri ve çıkarımı/puanı güvenli bir şekilde çalıştırın](how-to-enable-virtual-network.md)
- [Öğretici: modelleri eğitme](tutorial-train-models-with-aml.md)
- [Kaynak sağlayıcısı işlemleri](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices)
