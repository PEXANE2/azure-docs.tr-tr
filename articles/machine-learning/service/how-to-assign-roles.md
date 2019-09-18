---
title: Azure Machine Learning çalışma alanındaki rolleri yönetme
titleSuffix: Azure Machine Learning
description: Rol tabanlı erişim denetimi (RBAC) kullanarak bir Azure Machine Learning çalışma alanına erişmeyi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: larryfr
author: Blackmist
ms.date: 07/10/2019
ms.custom: seodec18
ms.openlocfilehash: 22ce9ea44dde6da4d1194463fe266ed00c5a3f96
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71067716"
---
# <a name="manage-access-to-an-azure-machine-learning-workspace"></a>Azure Machine Learning çalışma alanına erişimi yönetme

Bu makalede, Azure Machine Learning çalışma alanına erişimi yönetmeyi öğreneceksiniz. [Rol tabanlı erişim denetimi (RBAC)](/azure/role-based-access-control/overview) , Azure kaynaklarına erişimi yönetmek için kullanılır. Azure Active Directory kullanıcılara, kaynaklara erişim izni veren belirli roller atanır. Azure, yerleşik roller ve özel roller oluşturma yeteneği sağlar.

## <a name="default-roles"></a>Varsayılan roller

Azure Machine Learning çalışma alanı bir Azure kaynağıdır. Diğer Azure kaynakları gibi yeni bir Azure Machine Learning çalışma alanı oluşturulduğunda, bu üç varsayılan rolle birlikte gelir. Çalışma alanına kullanıcı ekleyebilir ve bunları bu yerleşik rollerden birine atayabilirsiniz.

| Role | Erişim düzeyi |
| --- | --- |
| **Okuyucu** | Çalışma alanındaki salt okuma eylemleri. Okuyucular bir çalışma alanındaki varlıkları listeleyebilir ve görüntüleyebilir, ancak bu varlıkları oluşturamaz veya güncelleştiremez. |
| **Katılımcı** | Çalışma alanındaki varlıkları görüntüleyin, oluşturun, düzenleyin veya silin (uygulanabilir). Örneğin, katkıda bulunanlar bir deneme oluşturabilir, bir işlem kümesi oluşturabilir veya ekleyebilir, bir çalıştırma gönderebilir ve bir Web hizmeti dağıtabilir. |
| **Sahip** | Çalışma alanına, çalışma alanındaki varlıkları görüntüleme, oluşturma, düzenleme veya silme özelliği de dahil olmak üzere tam erişim. Ayrıca, rol atamalarını değiştirebilirsiniz. |

> [!IMPORTANT]
> Rol erişimi, Azure 'da birden çok düzey kapsamına eklenebilir. Örneğin, bir çalışma alanına sahip olan birisi, çalışma alanını içeren kaynak grubuna sahip erişimine sahip olmayabilir. Daha fazla bilgi için bkz. [RBAC çalışma](/azure/role-based-access-control/overview#how-rbac-works).

Belirli yerleşik roller hakkında daha fazla bilgi için bkz. [Azure Için yerleşik roller](/azure/role-based-access-control/built-in-roles).

## <a name="manage-workspace-access"></a>Çalışma alanı erişimini yönetme

Bir çalışma alanının sahibiyseniz, çalışma alanı için roller ekleyebilir ve kaldırabilirsiniz. Ayrıca, kullanıcılara roller atayabilirsiniz. Erişimin nasıl yönetileceğini saptamak için aşağıdaki bağlantıları kullanın:
- [Azure portal Kullanıcı arabirimi](/azure/role-based-access-control/role-assignments-portal)
- [PowerShell](/azure/role-based-access-control/role-assignments-powershell)
- [Azure CLI](/azure/role-based-access-control/role-assignments-cli)
- [REST API](/azure/role-based-access-control/role-assignments-rest)
- [Azure Resource Manager şablonları](/azure/role-based-access-control/role-assignments-template)

[Clı Azure Machine Learning](reference-azure-machine-learning-cli.md)yüklediyseniz, kullanıcılara roller atamak IÇIN bir CLI komutu de kullanabilirsiniz.

```azurecli-interactive 
az ml workspace share -w <workspace_name> -g <resource_group_name> --role <role_name> --user <user_corp_email_address>
```

`user` Alan, çalışma alanı üst aboneliğinin yaşadığı Azure Active Directory örneğindeki mevcut bir kullanıcının e-posta adresidir. Bu komutun nasıl kullanılacağına ilişkin bir örnek aşağıda verilmiştir:

```azurecli-interactive 
az ml workspace share -w my_workspace -g my_resource_group --role Contributor --user jdoe@contoson.com
```

## <a name="create-custom-role"></a>Özel rol oluştur

Yerleşik roller yetersizse, özel roller oluşturabilirsiniz. Özel rollerin bu çalışma alanında okuma, yazma, silme ve hesaplama kaynağı izinleri olabilir. Rolü belirli bir çalışma alanı düzeyinde, belirli bir kaynak grubu düzeyinde veya belirli bir abonelik düzeyinde kullanılabilir hale getirebilirsiniz.

> [!NOTE]
> Bu kaynak içinde özel Roller oluşturmak için bu düzeyde kaynağın sahibi olmanız gerekir.

Özel bir rol oluşturmak için, önce rolün iznini ve kapsamını belirten bir rol tanımı JSON dosyası oluşturun. Aşağıdaki örnek, belirli bir çalışma alanı düzeyinde "veri Bilimcst" kapsamı adlı özel bir rol tanımlar:

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

Bu özel rolün kapsamını `AssignableScopes` abonelik düzeyinde, kaynak grubu düzeyinde veya belirli bir çalışma alanı düzeyinde ayarlamak için alanı değiştirebilirsiniz.

Bu özel rol, aşağıdaki eylemler hariç çalışma alanındaki her şeyi gerçekleştirebilir:

- İşlem kaynağı oluşturamaz veya güncelleştiremez.
- İşlem kaynağını silemiyor.
- Rol atamalarını ekleyemez, silemez veya değiştiremez.
- Çalışma alanını silemiyor.

Bu özel rolü dağıtmak için aşağıdaki Azure CLı komutunu kullanın:

```azurecli-interactive 
az role definition create --role-definition data_scientist_role.json
```

Dağıtımdan sonra, bu rol belirtilen çalışma alanında kullanılabilir hale gelir. Artık bu rolü Azure portal ekleyip atayabilirsiniz. Ya da, `az ml workspace share` CLI komutunu kullanarak bu rolü bir kullanıcıya atayabilirsiniz:

```azurecli-interactive
az ml workspace share -w my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```

Özel roller hakkında daha fazla bilgi için bkz. [Azure kaynakları Için özel roller](/azure/role-based-access-control/custom-roles).

Özel rollerle kullanılabilen işlemler (Eylemler) hakkında daha fazla bilgi için bkz. [kaynak sağlayıcısı işlemleri](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices).

## <a name="next-steps"></a>Sonraki adımlar

- [Kurumsal güvenliğe genel bakış](concept-enterprise-security.md)
- [Bir sanal ağ içinde denemeleri ve çıkarımı/puanı güvenli bir şekilde çalıştırın](how-to-enable-virtual-network.md)
- [Öğretici: Modelleri eğitme](tutorial-train-models-with-aml.md)
- [Kaynak sağlayıcısı işlemleri](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices)