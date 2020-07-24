---
title: Çalışma alanınızdaki rolleri yönetme
titleSuffix: Azure Machine Learning
description: Rol tabanlı erişim denetimi (RBAC) kullanarak bir Azure Machine Learning çalışma alanına erişmeyi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: jmartens
ms.author: larryfr
author: Blackmist
ms.date: 06/30/2020
ms.custom: seodec18
ms.openlocfilehash: ff8d532bf1c19ded9567e8c1e4b63e674c01d0d8
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87125182"
---
# <a name="manage-access-to-an-azure-machine-learning-workspace"></a>Azure Machine Learning çalışma alanına erişimi yönetme
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning çalışma alanına erişimi yönetmeyi öğreneceksiniz. [Rol tabanlı erişim denetimi (RBAC)](/azure/role-based-access-control/overview) , Azure kaynaklarına erişimi yönetmek için kullanılır. Azure Active Directory kullanıcılara, kaynaklara erişim izni veren belirli roller atanır. Azure, yerleşik roller ve özel roller oluşturma yeteneği sağlar.

## <a name="default-roles"></a>Varsayılan roller

Azure Machine Learning çalışma alanı bir Azure kaynağıdır. Diğer Azure kaynaklarında olduğu gibi yeni bir Azure Machine Learning çalışma alanı da oluşturulduğunda üç varsayılan role sahip olur. Çalışma alanına kullanıcı ekleyebilir ve bunları bu yerleşik rollerden birine atayabilirsiniz.

| Rol | Erişim düzeyi |
| --- | --- |
| **Okuyucu** | Çalışma alanındaki salt okuma eylemleri. Okuyucular bir çalışma alanında varlıkları ( [veri deposu](how-to-access-data.md) kimlik bilgileri dahil) listeleyebilir ve görüntüleyebilir, ancak bu varlıkları oluşturamaz veya güncelleştiremez. |
| **Katkıda Bulunan** | Çalışma alanındaki varlıkları görüntüleyin, oluşturun, düzenleyin veya silin (uygulanabilir). Örneğin katkıda bulunanlar bir deneme oluşturabilir, işlem kümesi oluşturabilir veya ekleyebilir, çalıştırma gönderebilir ve bir web hizmeti dağıtabilir. |
| **Sahibi** | Çalışma alanına, çalışma alanındaki varlıkları görüntüleme, oluşturma, düzenleme veya silme özelliği de dahil olmak üzere tam erişim. İsterseniz rol atamalarını değiştirebilirsiniz. |

> [!IMPORTANT]
> Rol erişimi, Azure 'da birden çok düzey kapsamına eklenebilir. Örneğin, bir çalışma alanına sahip olan birisi, çalışma alanını içeren kaynak grubuna sahip erişimine sahip olmayabilir. Daha fazla bilgi için bkz. [RBAC çalışma](/azure/role-based-access-control/overview#how-rbac-works).

Belirli yerleşik roller hakkında daha fazla bilgi için bkz. [Azure yerleşik rolleri](/azure/role-based-access-control/built-in-roles).

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

`user`Alan, çalışma alanı üst aboneliğinin yaşadığı Azure Active Directory örneğindeki mevcut bir kullanıcının e-posta adresidir. Bu komutun nasıl kullanılacağına ilişkin bir örnek aşağıda verilmiştir:

```azurecli-interactive 
az ml workspace share -w my_workspace -g my_resource_group --role Contributor --user jdoe@contoson.com
```

> [!NOTE]
> "az ml çalışma alanı paylaşma" komutu, Azure Active Directory B2B tarafından Federal hesap için çalışmıyor. Lütfen komut yerine Azure UI portalını kullanın.


## <a name="azure-machine-learning-operations"></a>Azure Machine Learning işlemler

Birçok işlem ve görev için yerleşik Eylemler Azure Machine Learning. Tüm liste için bkz. [Azure kaynak sağlayıcıları işlemleri](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices).

## <a name="create-custom-role"></a>Özel rol oluştur

Yerleşik rollerin yetersiz olması durumunda özel roller oluşturabilirsiniz. Özel rollerin bu çalışma alanında okuma, yazma, silme ve hesaplama kaynağı izinleri olabilir. Rolü belirli bir çalışma alanı düzeyinde, belirli bir kaynak grubu düzeyinde veya belirli bir abonelik düzeyinde kullanılabilir hale getirebilirsiniz.

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

> [!TIP]
> `AssignableScopes`Bu özel rolün kapsamını abonelik düzeyinde, kaynak grubu düzeyinde veya belirli bir çalışma alanı düzeyinde ayarlamak için alanı değiştirebilirsiniz.

Bu özel rol, aşağıdaki eylemler hariç çalışma alanındaki her şeyi gerçekleştirebilir:

- İşlem kaynağı oluşturamaz veya güncelleştiremez.
- İşlem kaynağını silemiyor.
- Rol atamalarını ekleyemez, silemez veya değiştiremez.
- Çalışma alanını silemiyor.

Bu özel rolü dağıtmak için aşağıdaki Azure CLı komutunu kullanın:

```azurecli-interactive 
az role definition create --role-definition data_scientist_role.json
```

Dağıtımdan sonra, bu rol belirtilen çalışma alanında kullanılabilir hale gelir. Artık bu rolü Azure portal ekleyip atayabilirsiniz. Ya da, CLI komutunu kullanarak bu rolü bir kullanıcıya atayabilirsiniz `az ml workspace share` :

```azurecli-interactive
az ml workspace share -w my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```

Özel roller hakkında daha fazla bilgi için bkz. [Azure özel rolleri](/azure/role-based-access-control/custom-roles).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular


### <a name="q-what-are-the-permissions-needed-to-perform-various-actions-in-the-azure-machine-learning-service"></a>S. Azure Machine Learning hizmetinde çeşitli işlemleri gerçekleştirmek için gereken izinler nelerdir?

Aşağıdaki tabloda Azure Machine Learning etkinliklerin Özeti ve bunları en az kapsamda gerçekleştirmek için gereken izinler verilmiştir. Örnek olarak, bir etkinlik bir çalışma alanı kapsamıyla (sütun 4) gerçekleştirilebileceği takdirde, bu izne sahip tüm daha yüksek kapsam de otomatik olarak çalışacaktır. Bu tablodaki tüm yollar **göreli yollardır** `Microsoft.MachineLearningServices/` .

| Etkinlik | Abonelik düzeyi kapsamı | Kaynak grubu düzeyi kapsamı | Çalışma alanı düzeyi kapsamı |
|---|---|---|---|
| Yeni çalışma alanı oluştur | Gerekli değil | Sahip veya katkıda bulunan | Yok (oluşturulduktan sonra sahip olur veya daha yüksek kapsam rolünü devralır) |
| Yeni işlem kümesi oluştur | Gerekli değil | Gerekli değil | Sahibi, katkıda bulunan veya özel rol şunları sağlar:`workspaces/computes/write` |
| Yeni Not defteri VM 'si oluşturma | Gerekli değil | Sahip veya katkıda bulunan | Mümkün değil |
| Yeni işlem örneği oluştur | Gerekli değil | Gerekli değil | Sahibi, katkıda bulunan veya özel rol şunları sağlar:`workspaces/computes/write` |
| Çalıştırma, verilere erişme, model dağıtma veya yayımlama işlem hattı gibi veri düzlemi etkinliği | Gerekli değil | Gerekli değil | Sahibi, katkıda bulunan veya özel rol şunları sağlar:`workspaces/*/write` <br/> Ayrıca, MSI 'ın Depolama hesabınızdaki verilere erişmesine izin vermek için çalışma alanına kayıtlı bir veri deposuna ihtiyacınız vardır. |


### <a name="q-how-do-i-list-all-the-custom-roles-in-my-subscription"></a>S. Aboneliğimde tüm özel rolleri listelemek Nasıl yaparım? mı?

Azure CLı 'de aşağıdaki komutu çalıştırın.

```azurecli-interactive
az role definition list --subscription <sub-id> --custom-role-only true
```

### <a name="q-how-do-i-find-the-role-definition-for-a-role-in-my-subscription"></a>S. Nasıl yaparım? Aboneliğimde bir rolün rol tanımını bulamıyor musunuz?

Azure CLı 'de aşağıdaki komutu çalıştırın. `<role-name>`Yukarıdaki komutla döndürülen biçimde olmalıdır.

```azurecli-interactive
az role definition list -n <role-name> --subscription <sub-id>
```

### <a name="q-how-do-i-update-a-role-definition"></a>S. Nasıl yaparım? bir rol tanımı güncelleştirilsin mi?

Azure CLı 'de aşağıdaki komutu çalıştırın.

```azurecli-interactive
az role definition update --role-definition update_def.json --subscription <sub-id>
```

Yeni rol tanımınızın tüm kapsamındaki izinlere sahip olmanız gerekir. Örneğin, bu yeni rolün üç abonelik arasında bir kapsamı varsa, üç abonelik için de izinleriniz olması gerekir. 

> [!NOTE]
> Rol güncelleştirmelerinin, bu kapsamdaki tüm rol atamaları üzerinde uygulanması 15 dakika ila saat arasında sürebilir.
### <a name="q-can-i-define-a-role-that-prevents-updating-the-workspace-edition"></a>S. Çalışma alanı sürümünün güncelleştirilmesini engelleyen bir rol tanımlayabilir miyim? 

Evet, çalışma alanı sürümünün güncelleştirilmesini engelleyen bir rol tanımlayabilirsiniz. Çalışma alanı güncelleştirmesi, çalışma alanı nesnesi üzerinde bir yama çağrısı olduğundan, bunu `"NotActions"` JSON tanımınızdaki diziye aşağıdaki eylemi koyarak yapabilirsiniz: 

`"Microsoft.MachineLearningServices/workspaces/write"`

### <a name="q-what-permissions-are-needed-to-perform-quota-operations-in-a-workspace"></a>S. Bir çalışma alanında kota işlemleri gerçekleştirmek için hangi izinler gereklidir? 

Çalışma alanında kotayla ilgili herhangi bir işlemi gerçekleştirmek için abonelik düzeyinde izinlere ihtiyacınız vardır. Bu, yönetilen işlem kaynaklarınız için abonelik düzeyi kotasının veya çalışma alanı düzeyi kotasının ayarlanması, yalnızca abonelik kapsamında yazma izinleriniz varsa meydana gelebileceği anlamına gelir. 


## <a name="next-steps"></a>Sonraki adımlar

- [Kurumsal güvenliğe genel bakış](concept-enterprise-security.md)
- [Bir sanal ağ içinde denemeleri ve çıkarımı/puanı güvenli bir şekilde çalıştırın](how-to-enable-virtual-network.md)
- [Öğretici: modelleri eğitme](tutorial-train-models-with-aml.md)
- [Kaynak sağlayıcısı işlemleri](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices)