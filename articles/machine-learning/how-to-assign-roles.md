---
title: Çalışma alanınızdaki rolleri yönetme
titleSuffix: Azure Machine Learning
description: Rol tabanlı erişim denetimi (RBAC) kullanarak bir Azure Machine Learning çalışma alanına erişmeyi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: Blackmist
ms.author: nigup
author: nishankgu
ms.date: 07/24/2020
ms.custom: how-to, seodec18
ms.openlocfilehash: afffdd0267cde8ffc841587748e51dd27e021369
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88079595"
---
# <a name="manage-access-to-an-azure-machine-learning-workspace"></a>Azure Machine Learning çalışma alanına erişimi yönetme
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning çalışma alanına erişimi yönetmeyi öğreneceksiniz. Azure [rol tabanlı erişim denetimi (Azure RBAC)](/azure/role-based-access-control/overview) , Azure kaynaklarına erişimi yönetmek için kullanılır. Azure Active Directory kullanıcılara, kaynaklara erişim izni veren belirli roller atanır. Azure, yerleşik roller ve özel roller oluşturma yeteneği sağlar.

## <a name="default-roles"></a>Varsayılan roller

Azure Machine Learning çalışma alanı bir Azure kaynağıdır. Diğer Azure kaynaklarında olduğu gibi yeni bir Azure Machine Learning çalışma alanı da oluşturulduğunda üç varsayılan role sahip olur. Çalışma alanına kullanıcı ekleyebilir ve bunları bu yerleşik rollerden birine atayabilirsiniz.

| Rol | Erişim düzeyi |
| --- | --- |
| **Okuyucu** | Çalışma alanındaki salt okuma eylemleri. Okuyucular, bir çalışma alanında [veri deposu](how-to-access-data.md) kimlik bilgileri de dahil olmak üzere varlıkları listeleyebilir ve görüntüleyebilir. Okuyucular bu varlıkları oluşturamaz veya güncelleştiremez. |
| **Katkıda Bulunan** | Çalışma alanındaki varlıkları görüntüleyin, oluşturun, düzenleyin veya silin (uygulanabilir). Örneğin katkıda bulunanlar bir deneme oluşturabilir, işlem kümesi oluşturabilir veya ekleyebilir, çalıştırma gönderebilir ve bir web hizmeti dağıtabilir. |
| **Sahibi** | Çalışma alanına, çalışma alanındaki varlıkları görüntüleme, oluşturma, düzenleme veya silme özelliği de dahil olmak üzere tam erişim. İsterseniz rol atamalarını değiştirebilirsiniz. |
| **Özel rol** | Çalışma alanındaki belirli denetim veya veri düzlemi işlemlerine erişimi özelleştirmenizi sağlar. Örneğin, bir çalıştırma gönderme, işlem oluşturma, model dağıtma veya bir veri kümesini kaydetme. |

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

[Azure MACHINE LEARNING CLI](reference-azure-machine-learning-cli.md)'yı yüklediyseniz, kullanıcılara roller atamak için CLI komutlarını kullanabilirsiniz:

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

Birçok işlem ve görev için yerleşik Eylemler Azure Machine Learning. Tüm liste için bkz. [Azure Kaynak sağlayıcısı işlemleri](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices).

## <a name="create-custom-role"></a>Özel rol oluştur

Yerleşik rollerin yetersiz olması durumunda özel roller oluşturabilirsiniz. Özel rollerin bu çalışma alanında okuma, yazma, silme ve hesaplama kaynağı izinleri olabilir. Rolü belirli bir çalışma alanı düzeyinde, belirli bir kaynak grubu düzeyinde veya belirli bir abonelik düzeyinde kullanılabilir hale getirebilirsiniz.

> [!NOTE]
> Bu kaynak içinde özel Roller oluşturmak için bu düzeyde kaynağın sahibi olmanız gerekir.

Özel bir rol oluşturmak için, önce rolün iznini ve kapsamını belirten bir rol tanımı JSON dosyası oluşturun. Aşağıdaki örnek, belirli bir çalışma alanı düzeyinde "Data Bilimentist Custom" adlı özel bir rol tanımlar:

`data_scientist_custom_role.json` :
```json
{
    "Name": "Data Scientist Custom",
    "IsCustom": true,
    "Description": "Can run experiment but can't create or delete compute.",
    "Actions": ["*"],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/*/delete",
        "Microsoft.MachineLearningServices/workspaces/write",
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
> Yukarıdaki özel rol yalnızca bir örnektir. [Azure Machine Learning hizmeti için önerilen bazı özel roller](#customroles)bölümüne bakın.

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

Özel roller hakkında daha fazla bilgi için bkz. [Azure özel rolleri](/azure/role-based-access-control/custom-roles). Özel rollerle kullanılabilen işlemler (Eylemler ve eylemler) hakkında daha fazla bilgi için bkz. [kaynak sağlayıcısı işlemleri](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular


### <a name="q-what-are-the-permissions-needed-to-perform-some-common-scenarios-in-the-azure-machine-learning-service"></a>S. Azure Machine Learning hizmetinde bazı yaygın senaryoları gerçekleştirmek için gereken izinler nelerdir?

Aşağıdaki tabloda Azure Machine Learning etkinliklerin Özeti ve bunları en az kapsamda gerçekleştirmek için gereken izinler verilmiştir. Örneğin, bir etkinlik bir çalışma alanı kapsamıyla (sütun 4) gerçekleştirilebileceği takdirde, bu izne sahip tüm daha yüksek kapsam de otomatik olarak çalışır:

> [!IMPORTANT]
> Bu tablodaki ile başlayan tüm yollar `/` **göreli yollardır** `Microsoft.MachineLearningServices/` :

| Etkinlik | Abonelik düzeyi kapsamı | Kaynak grubu düzeyi kapsamı | Çalışma alanı düzeyi kapsamı |
| ----- | ----- | ----- | ----- |
| Yeni çalışma alanı oluştur | Gerekli değil | Sahip veya katkıda bulunan | Yok (oluşturulduktan sonra sahip olur veya daha yüksek kapsam rolünü devralır) |
| Çalışma alanının sürümünü Güncelleştir | Gerekli değil | Gerekli değil | Sahibi, katkıda bulunan veya özel rol şunları sağlar:`/workspaces/write` |
| Abonelik düzeyi Amlcompute kotası iste veya çalışma alanı düzeyi kotasını ayarla | Sahip veya katkıda bulunan veya özel rol </br>enlere`/locations/updateQuotas/action`</br> Abonelik kapsamında | Yetkilendirilmemiş | Yetkilendirilmemiş |
| Yeni işlem kümesi oluştur | Gerekli değil | Gerekli değil | Sahibi, katkıda bulunan veya özel rol şunları sağlar:`/workspaces/computes/write` |
| Yeni işlem örneği oluştur | Gerekli değil | Gerekli değil | Sahibi, katkıda bulunan veya özel rol şunları sağlar:`/workspaces/computes/write` |
| Herhangi bir türdeki çalışma gönderiliyor | Gerekli değil | Gerekli değil | Sahibi, katkıda bulunan veya özel rol şunları sağlar:`"/workspaces/*/read", "/workspaces/environments/write", "/workspaces/experiments/runs/write", "/workspaces/metadata/artifacts/write", "/workspaces/metadata/snapshots/write", "/workspaces/environments/build/action", "/workspaces/experiments/runs/submit/action", "/workspaces/environments/readSecrets/action"` |
| Ardışık düzen uç noktası yayımlama | Gerekli değil | Gerekli değil | Sahibi, katkıda bulunan veya özel rol şunları sağlar:`"/workspaces/pipelines/write", "/workspaces/endpoints/pipelines/*", "/workspaces/pipelinedrafts/*", "/workspaces/modules/*"` |
| AKS/ACI kaynağına kayıtlı model dağıtma | Gerekli değil | Gerekli değil | Sahibi, katkıda bulunan veya özel rol şunları sağlar:`"/workspaces/services/aks/write", "/workspaces/services/aci/write"` |
| Dağıtılan bir AKS uç noktasına göre Puanlama | Gerekli değil | Gerekli değil | Sahip, katkıda bulunan veya özel rol: `"/workspaces/services/aks/score/action", "/workspaces/services/aks/listkeys/action"` (Azure Active Directory kimlik doğrulaması kullanmadığınız zaman) veya `"/workspaces/read"` (belirteç kimlik doğrulamasını kullanırken) |
| Etkileşimli not defterleri kullanarak depolamaya erişme | Gerekli değil | Gerekli değil | Sahibi, katkıda bulunan veya özel rol şunları sağlar:`"/workspaces/computes/read", "/workspaces/notebooks/samples/read", "/workspaces/notebooks/storage/*"` |
| Yeni özel rol oluştur | Sahip, katkıda bulunan veya özel rol`Microsoft.Authorization/roleDefinitions/write` | Gerekli değil | Sahibi, katkıda bulunan veya özel rol şunları sağlar:`/workspaces/computes/write` |

> [!TIP]
> İlk kez çalışma alanı oluşturmaya çalışırken bir hata alırsanız, rolünüzün izin verdiğinden emin olun `Microsoft.MachineLearningServices/register/action` . Bu eylem, Azure aboneliğinize Azure Machine Learning kaynak sağlayıcısını kaydetmenizi sağlar.

### <a name="q-are-we-publishing-azure-built-in-roles-for-the-machine-learning-service"></a>S. Machine Learning hizmeti için Azure yerleşik rollerini yayımlıyoruz mı?

Şu anda Machine Learning hizmeti için [Azure yerleşik rollerini](/azure/role-based-access-control/built-in-roles) yayımlıyoruz. Yayımlandıktan sonra yerleşik bir rol güncelleştirilemez ve müşteri senaryolarına ve geri bildirimlerine göre rol tanımlarını kesinleştiriyoruz. 

<a id="customroles"></a>

### <a name="q-are-there-some-custom-role-templates-for-the-most-common-scenarios-in-machine-learning-service"></a>S. Machine Learning hizmetindeki en yaygın senaryolar için bazı özel rol şablonları var mı?

Aşağıda, kendi özel rollerinizi tanımlamak için temel olarak kullanabileceğiniz özel önerilen rol tanımlarına sahip bazı yaygın senaryolar verilmiştir:

* __Veri Bilimconu özel__: bir veri bilimi 'nin bir çalışma alanı içindeki tüm işlemleri gerçekleştirmesine izin verir, **ancak şunları hariç**:

    * İşlem oluşturma
    * Bir üretim AKS kümesine model dağıtma
    * Üretimde bir işlem hattı uç noktası dağıtma

    `data_scientist_custom_role.json` :
    ```json
    {
        "Name": "Data Scientist Custom",
        "IsCustom": true,
        "Description": "Can run experiment but can't create or delete compute or deploy production endpoints.",
        "Actions": [
            "Microsoft.MachineLearningServices/workspaces/*/read",
            "Microsoft.MachineLearningServices/workspaces/*/action",
            "Microsoft.MachineLearningServices/workspaces/*/delete",
            "Microsoft.MachineLearningServices/workspaces/*/write"
        ],
        "NotActions": [
            "Microsoft.MachineLearningServices/workspaces/delete",
            "Microsoft.MachineLearningServices/workspaces/write",
            "Microsoft.MachineLearningServices/workspaces/computes/*/write",
            "Microsoft.MachineLearningServices/workspaces/computes/*/delete", 
            "Microsoft.Authorization/*",
            "Microsoft.MachineLearningServices/workspaces/computes/listKeys/action",
            "Microsoft.MachineLearningServices/workspaces/listKeys/action",
            "Microsoft.MachineLearningServices/workspaces/services/aks/write",
            "Microsoft.MachineLearningServices/workspaces/services/aks/delete",
            "Microsoft.MachineLearningServices/workspaces/endpoints/pipelines/write"
        ],
        "AssignableScopes": [
            "/subscriptions/<subscription_id>"
        ]
    }
    ```

* __Data Bilimcst kısıtlı özel__: izin verilen eylemlerde joker karakter olmadan daha kısıtlı bir rol tanımı. **Aşağıdakiler dışında**, çalışma alanındaki tüm işlemleri gerçekleştirebilir:

    * İşlem oluşturma
    * Bir üretim AKS kümesine model dağıtma
    * Üretimde bir işlem hattı uç noktası dağıtma

    `data_scientist_restricted_custom_role.json` :
    ```json
    {
        "Name": "Data Scientist Restricted Custom",
        "IsCustom": true,
        "Description": "Can run experiment but can't create or delete compute or deploy production endpoints",
        "Actions": [
            "Microsoft.MachineLearningServices/workspaces/*/read",
            "Microsoft.MachineLearningServices/workspaces/computes/start/action",
            "Microsoft.MachineLearningServices/workspaces/computes/stop/action",
            "Microsoft.MachineLearningServices/workspaces/computes/restart/action",
            "Microsoft.MachineLearningServices/workspaces/computes/applicationaccess/action",
            "Microsoft.MachineLearningServices/workspaces/notebooks/storage/read",
            "Microsoft.MachineLearningServices/workspaces/notebooks/storage/write",
            "Microsoft.MachineLearningServices/workspaces/notebooks/storage/delete",
            "Microsoft.MachineLearningServices/workspaces/notebooks/samples/read",
            "Microsoft.MachineLearningServices/workspaces/experiments/runs/write",
            "Microsoft.MachineLearningServices/workspaces/experiments/write",
            "Microsoft.MachineLearningServices/workspaces/experiments/runs/submit/action",
            "Microsoft.MachineLearningServices/workspaces/pipelinedrafts/write",
            "Microsoft.MachineLearningServices/workspaces/metadata/snapshots/write",
            "Microsoft.MachineLearningServices/workspaces/metadata/artifacts/write",
            "Microsoft.MachineLearningServices/workspaces/environments/write",
            "Microsoft.MachineLearningServices/workspaces/models/write",
            "Microsoft.MachineLearningServices/workspaces/modules/write",
            "Microsoft.MachineLearningServices/workspaces/datasets/registered/write", 
            "Microsoft.MachineLearningServices/workspaces/datasets/registered/delete",
            "Microsoft.MachineLearningServices/workspaces/datasets/unregistered/write",
            "Microsoft.MachineLearningServices/workspaces/datasets/unregistered/delete",
            "Microsoft.MachineLearningServices/workspaces/computes/listNodes/action",
            "Microsoft.MachineLearningServices/workspaces/environments/build/action"
        ],
        "NotActions": [
            "Microsoft.MachineLearningServices/workspaces/computes/write",
            "Microsoft.MachineLearningServices/workspaces/write",
            "Microsoft.MachineLearningServices/workspaces/computes/delete",
            "Microsoft.MachineLearningServices/workspaces/delete",
            "Microsoft.MachineLearningServices/workspaces/computes/listKeys/action",
            "Microsoft.MachineLearningServices/workspaces/listKeys/action",
            "Microsoft.Authorization/*",
            "Microsoft.MachineLearningServices/workspaces/datasets/registered/profile/read",
            "Microsoft.MachineLearningServices/workspaces/datasets/registered/preview/read",
            "Microsoft.MachineLearningServices/workspaces/datasets/unregistered/profile/read",
            "Microsoft.MachineLearningServices/workspaces/datasets/unregistered/preview/read",
            "Microsoft.MachineLearningServices/workspaces/datasets/registered/schema/read",    
            "Microsoft.MachineLearningServices/workspaces/datasets/unregistered/schema/read",
            "Microsoft.MachineLearningServices/workspaces/datastores/write",
            "Microsoft.MachineLearningServices/workspaces/datastores/delete"
        ],
        "AssignableScopes": [
            "/subscriptions/<subscription_id>"
        ]
    }
    ```

* __Mlops Custom__: bir hizmet sorumlusuna rol atamanıza ve MLOps işlem hatlarınızı otomatikleştirmek için bunu kullanmanıza olanak sağlar. Örneğin, önceden yayımlanmış bir işlem hattına karşı çalıştırmaları göndermek için:

    `mlops_custom_role.json` :
    ```json
    {
        "Name": "MLOps Custom",
        "IsCustom": true,
        "Description": "Can run pipelines against a published pipeline endpoint",
        "Actions": [
            "Microsoft.MachineLearningServices/workspaces/read",
            "Microsoft.MachineLearningServices/workspaces/endpoints/pipelines/read",
            "Microsoft.MachineLearningServices/workspaces/metadata/artifacts/read",
            "Microsoft.MachineLearningServices/workspaces/metadata/snapshots/read",
            "Microsoft.MachineLearningServices/workspaces/environments/read",    
            "Microsoft.MachineLearningServices/workspaces/metadata/secrets/read",
            "Microsoft.MachineLearningServices/workspaces/modules/read",
            "Microsoft.MachineLearningServices/workspaces/experiments/runs/read",
            "Microsoft.MachineLearningServices/workspaces/datasets/registered/read",
            "Microsoft.MachineLearningServices/workspaces/datastores/read",
            "Microsoft.MachineLearningServices/workspaces/environments/write",
            "Microsoft.MachineLearningServices/workspaces/experiments/runs/write",
            "Microsoft.MachineLearningServices/workspaces/metadata/artifacts/write",
            "Microsoft.MachineLearningServices/workspaces/metadata/snapshots/write",
            "Microsoft.MachineLearningServices/workspaces/environments/build/action",
            "Microsoft.MachineLearningServices/workspaces/experiments/runs/submit/action"
        ],
        "NotActions": [
            "Microsoft.MachineLearningServices/workspaces/computes/write",
            "Microsoft.MachineLearningServices/workspaces/write",
            "Microsoft.MachineLearningServices/workspaces/computes/delete",
            "Microsoft.MachineLearningServices/workspaces/delete",
            "Microsoft.MachineLearningServices/workspaces/computes/listKeys/action",
            "Microsoft.MachineLearningServices/workspaces/listKeys/action",
            "Microsoft.Authorization/*"
        ],
        "AssignableScopes": [
            "/subscriptions/<subscription_id>"
        ]
    }
    ```

* __Çalışma alanı yöneticisi__: **aşağıdakiler dışında**tüm işlemleri bir çalışma alanının kapsamında gerçekleştirmenize olanak tanır:

    * Yeni bir çalışma alanı oluşturma
    * Abonelik veya çalışma alanı düzeyinde kotalar atama
    * Çalışma alanının sürümü yükseltiliyor

    Çalışma alanı yöneticisi de yeni bir rol oluşturamaz. Yalnızca mevcut yerleşik veya özel rolleri, çalışma alanının kapsamı içinde atayabilir:

    `workspace_admin_custom_role.json` :
    ```json
    {
        "Name": "Workspace Admin Custom",
        "IsCustom": true,
        "Description": "Can perform all operations except quota management and upgrades",
        "Actions": [
            "Microsoft.MachineLearningServices/workspaces/*/read",
            "Microsoft.MachineLearningServices/workspaces/*/action",
            "Microsoft.MachineLearningServices/workspaces/*/write",
            "Microsoft.MachineLearningServices/workspaces/*/delete",
            "Microsoft.Authorization/roleAssignments/*"
        ],
        "NotActions": [
            "Microsoft.MachineLearningServices/workspaces/write"
        ],
        "AssignableScopes": [
            "/subscriptions/<subscription_id>"
        ]
    }
    ```

<a name="labeler"></a>
* __Labeler Custom__: yalnızca verileri etiketlemeye yönelik bir rol tanımlamanızı sağlar:

    `labeler_custom_role.json` :
    ```json
    {
        "Name": "Labeler Custom",
        "IsCustom": true,
        "Description": "Can label data for Labeling",
        "Actions": [
            "Microsoft.MachineLearningServices/workspaces/read",
            "Microsoft.MachineLearningServices/workspaces/labeling/projects/read",
            "Microsoft.MachineLearningServices/workspaces/labeling/labels/write"
        ],
        "NotActions": [
            "Microsoft.MachineLearningServices/workspaces/labeling/projects/summary/read"
        ],
        "AssignableScopes": [
            "/subscriptions/<subscription_id>"
        ]
    }
    ```

### <a name="q-how-do-i-list-all-the-custom-roles-in-my-subscription"></a>S. Aboneliğimde tüm özel rolleri listelemek Nasıl yaparım? mı?

Azure CLı 'de aşağıdaki komutu çalıştırın.

```azurecli-interactive
az role definition list --subscription <sub-id> --custom-role-only true
```

### <a name="q-how-do-i-find-the-operations-supported-by-the-machine-learning-service"></a>S. Machine Learning hizmeti tarafından desteklenen işlemler mi Nasıl yaparım??

Azure CLı 'de aşağıdaki komutu çalıştırın.

```azurecli-interactive
az provider operation show –n Microsoft.MachineLearningServices
```

Bunlar ayrıca, [kaynak sağlayıcısı işlemleri](/azure/role-based-access-control/resource-provider-operations#microsoftmachinelearningservices)listesinde bulunabilir.


### <a name="q-what-are-some-common-gotchas-when-using-azure-rbac"></a>S. Azure RBAC kullanırken bazı yaygın tuzakları nedir?

Azure rol tabanlı erişim denetimi (Azure RBAC) kullanırken dikkat etmeniz gereken birkaç nokta aşağıda verilmiştir:

- Azure 'da bir kaynak oluşturduğunuzda bir çalışma alanı söylediğinizde, doğrudan çalışma alanının sahibi değilsiniz. Rolünüz, bu abonelikte yetkilendirdiğiniz en yüksek kapsam rolünden devralınır. Örnek olarak, bir ağ yöneticisiyseniz ve bir Machine Learning çalışma alanı oluşturma izinlerine sahipseniz, bu çalışma alanına karşı ağ yöneticisi rolüne sahip olursunuz ve sahip rolü yoktur.
- Aynı Azure Active Directory kullanıcıya eylem/NotActions bölümleri ile aynı olan iki rol ataması olduğunda, bir rolden NotActions bölümünde listelenen işlemler, başka bir roldeki eylemler olarak da listelendiklerinde etkili olmayabilir. Azure 'un rol atamalarını nasıl ayrıştırdığı hakkında daha fazla bilgi edinmek için, [kullanıcının bir kaynağa erişip erişemeyeceğini Azure RBAC 'Nin nasıl belirlediğini](/azure/role-based-access-control/overview#how-azure-rbac-determines-if-a-user-has-access-to-a-resource) okuyun
- İşlem kaynaklarınızı bir sanal ağ içinde dağıtmak için aşağıdaki eylemler için açıkça izinlerinizin olması gerekir:
    - VNet kaynağında "Microsoft. Network/virtualNetworks/JOIN/Action".
    - Alt ağ kaynağında "Microsoft. Network/virtualNetworks/subnet/JOIN/Action".
    
    Ağ iletişimi ile RBAC hakkında daha fazla bilgi için bkz. [ağ yerleşik rolleri](/azure/role-based-access-control/built-in-roles#networking).

- Yeni rol atamalarınızın yığın genelindeki önbellekteki izinlerle etkili olması için bazen 1 saate kadar zaman alabilir.

### <a name="q-what-permissions-do-i-need-to-use-a-user-assigned-managed-identity-with-my-amlcompute-clusters"></a>S. Amlcompute kümelerimde Kullanıcı tarafından atanan yönetilen kimlik kullanmak için hangi izinlere ihtiyacım var?

Amlcompute kümelerinde Kullanıcı tarafından atanan bir kimlik atamak için, birinin işlem oluşturmak için yazma izinlerine sahip olması ve [yönetilen kimlik operatörü rolü](/azure/role-based-access-control/built-in-roles#managed-identity-operator)olması gerekir. Yönetilen kimliklerle RBAC hakkında daha fazla bilgi için, [Kullanıcı tarafından atanan kimliği yönetme](/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal) konusunu okuyun


### <a name="q-do-we-support-role-based-access-control-on-the-studio-portal"></a>S. Studio portalında rol tabanlı erişim denetimini destekliyoruz mi?

Azure Machine Learning Studio, Azure rol tabanlı erişim denetimini (Azure RBAC) destekler. 

> [!IMPORTANT]
> Çalışma alanınızda bir veri bilimcilerinin belirli izinleri olan özel bir rol atadıktan sonra, bunlara karşılık gelen eylemler (örneğin, bir işlem düğmesi ekleme), kullanıcılardan otomatik olarak gizlenir. Bu öğelerin gizlenmesi, kullanıldığında hizmetten yetkisiz erişim bildirimi döndüren denetimleri görmekten kaynaklanan karışıklıklara engel olur.

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
