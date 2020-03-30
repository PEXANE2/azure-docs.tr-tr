---
title: Yönetilen kaynakları güncelleştirme
description: Azure yönetilen bir uygulama için yönetilen kaynak grubundaki kaynaklar üzerinde nasıl çalışılabildiğini açıklar.
author: tfitzmac
ms.topic: conceptual
ms.date: 10/26/2017
ms.author: tomfitz
ms.openlocfilehash: 3dbb88d53a961e277e102d7eec580309951c73b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75651221"
---
# <a name="work-with-resources-in-the-managed-resource-group-for-azure-managed-application"></a>Azure yönetilen uygulama için yönetilen kaynak grubundaki kaynaklarla çalışma

Bu makalede, yönetilen bir uygulamanın parçası olarak dağıtılan kaynakların nasıl güncelleştirilen açıklanmıştır. Yönetilen bir uygulamanın yayımcısı olarak, yönetilen kaynak grubundaki kaynaklara erişebilirsiniz. Bu kaynakları güncelleştirmek için yönetilen bir uygulamayla ilişkili yönetilen kaynak grubunu bulmanız ve bu kaynak grubundaki kaynağa erişmeniz gerekir.

Bu makalede, yönetilen uygulamayı Yönetilen Web [Uygulaması'nda (IaaS) Azure yönetim hizmetleri](https://github.com/Azure/azure-managedapp-samples/tree/master/Managed%20Application%20Sample%20Packages/201-managed-web-app) örnek projesiyle dağıttığınızı varsayar. Yönetilen uygulama **Standard_D1_v2** bir sanal makine içerir. Yönetilen uygulamayı dağıtmadıysanız, yönetilen bir kaynak grubunu güncelleştirme katmak için adımları tanımak için bu makaleyi kullanmaya devam edebilirsiniz.

Aşağıdaki resimde dağıtılan yönetilen uygulamayı gösterilmektedir.

![Dağıtılan yönetilen uygulama](./media/update-managed-resources/deployed.png)

Bu makalede, Azure CLI'yi şunları yapmak için kullanıyorsunuz:

* Yönetilen uygulamayı tanımlama
* Yönetilen kaynak grubunu belirleme
* Yönetilen kaynak grubundaki sanal makine kaynağını(lar) belirleme
* VM boyutunu değiştirin (kullanılmadığı takdirde daha küçük bir boyuta veya daha fazla yükü desteklemek için daha büyük bir boyuta)
* İzne verilen konumları belirten yönetilen kaynak grubuna bir ilke atama

## <a name="get-managed-application-and-managed-resource-group"></a>Yönetilen uygulama ve yönetilen kaynak grubunu alın

Yönetilen uygulamaları bir kaynak grubunda almak için şunları kullanın:

```azurecli-interactive
az managedapp list --query "[?contains(resourceGroup,'DemoApp')]"
```

Yönetilen kaynak grubunun kimliğini almak için şunları kullanın:

```azurecli-interactive
az managedapp list --query "[?contains(resourceGroup,'DemoApp')].{ managedResourceGroup:managedResourceGroupId }"
```

## <a name="resize-vms-in-managed-resource-group"></a>Yönetilen kaynak grubunda ki VM'leri yeniden boyutlandırma

Yönetilen kaynak grubundaki sanal makineleri görmek için yönetilen kaynak grubunun adını sağlayın.

```azurecli-interactive
az vm list -g DemoApp6zkevchqk7sfq --query "[].{VMName:name,OSType:storageProfile.osDisk.osType,VMSize:hardwareProfile.vmSize}"
```

VM'lerin boyutunu güncelleştirmek için şunları kullanın:

```azurecli-interactive
az vm resize --size Standard_D2_v2 --ids $(az vm list -g DemoApp6zkevchqk7sfq --query "[].id" -o tsv)
```

İşlem tamamlandıktan sonra, uygulamanın Standart D2 v2 üzerinde çalıştığını doğrulayın.

![Standart D2 v2 kullanılarak yönetilen uygulama](./media/update-managed-resources/upgraded.png)

## <a name="apply-policy-to-managed-resource-group"></a>Yönetilen kaynak grubuna ilke uygulama

Yönetilen kaynak grubunu alın ve bu kapsamda bir ilke atama. **E56962a6-4747-49cd-b67b-bf8b01975c4c** politikası, izin verilen konumları belirtmek için yerleşik bir ilkedir.

```azurecli-interactive
managedGroup=$(az managedapp show --name <app-name> --resource-group DemoApp --query managedResourceGroupId --output tsv)

az policy assignment create --name locationAssignment --policy e56962a6-4747-49cd-b67b-bf8b01975c4c --scope $managedGroup --params '{
                            "listofallowedLocations": {
                                "value": [
                                    "northeurope",
                                    "westeurope"
                                ]
                            }
                        }'
```

İzin verilen konumları görmek için şunları kullanın:

```azurecli-interactive
az policy assignment show --name locationAssignment --scope $managedGroup --query parameters.listofallowedLocations.value
```

İlke ataması portalda görünür.

![İlke atamayı görüntüleme](./media/update-managed-resources/assignment.png)

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Yönetilen uygulamalara genel bakış](overview.md) konusunu inceleyin.
* Örnek projeler için Azure [yönetilen uygulamalar için Örnek projelere](sample-projects.md)bakın.
