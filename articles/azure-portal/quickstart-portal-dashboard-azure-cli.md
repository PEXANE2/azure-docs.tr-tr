---
title: Azure CLı ile Azure portal panosu oluşturma
description: 'Hızlı başlangıç: Azure CLı kullanarak Azure portal Pano oluşturmayı öğrenin. Pano, bulut kaynaklarınızın odaklanmış ve düzenlenmiş bir görünümüdür.'
ms.topic: quickstart
ms.custom: devx-track-azurepowershell
ms.date: 12/4/2020
ms.openlocfilehash: ede915df5cd2967c3b6b700bcb9174c89af8f233
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2020
ms.locfileid: "96745664"
---
# <a name="quickstart-create-an-azure-portal-dashboard-with-azure-cli"></a>Hızlı başlangıç: Azure CLı ile Azure portal panosu oluşturma

Azure portal bir Pano, bulut kaynaklarınızın odaklanmış ve düzenlenmiş bir görünümüdür.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

- Birden çok Azure aboneliğiniz varsa, kaynakların faturalandırılbileceği uygun aboneliği seçin.
[Az Account set](/cli/azure/account#az_account_set) komutunu kullanarak bir abonelik seçin:

  ```azurecli
  az account set --subscription 00000000-0000-0000-0000-000000000000
  ```

- [Az Group Create](/cli/azure/group#az_group_create) komutunu kullanarak bir [Azure Kaynak grubu](../azure-resource-manager/management/overview.md) oluşturun veya var olan bir kaynak grubunu kullanın:

  ```azurecli
  az group create --name myResourceGroup --location centralus
  ```

   Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

[Az VM Create](/cli/azure/vm#az_vm_create) komutunu kullanarak bir sanal makine oluşturun:

```azurecli
az vm create --resource-group myResourceGroup --name SimpleWinVM --image win2016datacenter \
   --admin-username azureuser --admin-password 1StrongPassword$
```

> [!Note]
> Parolanın karmaşık olması gerekir.
> Bu yeni bir Kullanıcı adı ve paroladır.
> Örneğin, Azure 'da oturum açmak için kullandığınız hesap değildir.
> Daha fazla bilgi için bkz. [Kullanıcı adı gereksinimleri](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm) ve [Parola gereksinimleri](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

Dağıtım başlar ve genellikle birkaç dakika sürer.
Dağıtım tamamlandıktan sonra bir sonraki bölüme geçin.

## <a name="download-the-dashboard-template"></a>Pano şablonunu indirme

Azure panoları kaynak olduğundan, bu değerler JSON olarak temsil edilebilir.
Daha fazla bilgi için bkz. [Azure panoları yapısı](./azure-portal-dashboards-structure.md).

Şu dosyayı indirin: [ üzerindeportal-dashboard-template-testvm.js](https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/azure-portal/portal-dashboard-template-testvm.json).

Aşağıdaki değerleri değerlerinizle değiştirerek indirilen şablonu özelleştirin:

* `<subscriptionID>`: Aboneliğiniz
* `<rgName>`: Kaynak grubu, örneğin `myResourceGroup`
* `<vmName>`: Sanal makine adı, örneğin `SimpleWinVM`
* `<dashboardTitle>`: Pano başlığı, örneğin `Simple VM Dashboard`
* `<location>`: Örneğin, Azure bölgeniz `centralus`

Daha fazla bilgi için bkz. [Microsoft Portal panoları şablon başvurusu](/azure/templates/microsoft.portal/dashboards).

## <a name="deploy-the-dashboard-template"></a>Pano şablonunu dağıtma

Şimdi de şablonu Azure CLı içinden dağıtabilirsiniz.

1. Şablonu dağıtmak için [az Portal Dashboard Create](/cli/azure/ext/portal/portal/dashboard#ext_portal_az_portal_dashboard_create) komutunu çalıştırın:

   ```azurecli
   az portal dashboard create --resource-group myResourceGroup --name 'Simple VM Dashboard' \
      --input-path portal-dashboard-template-testvm.json --location centralus
   ```

1. Panonun, [az Portal panosu göster](/cli/azure/ext/portal/portal/dashboard#ext_portal_az_portal_dashboard_show) komutunu çalıştırarak başarıyla oluşturulduğunu denetleyin:

   ```azurecli
   az portal dashboard show --resource-group myResourceGroup --name 'Simple VM Dashboard'
   ```

Geçerli abonelik için tüm panoları görmek için [az Portal Dashboard List](/cli/azure/ext/portal/portal/dashboard#ext_portal_az_portal_dashboard_list)kullanın:

```azurecli
az portal dashboard list
```

Ayrıca, bir kaynak grubu için tüm panoları görebilirsiniz:

```azurecli
az portal dashboard list --resource-group myResourceGroup
```

[Az Portal Dashboard Update](/cli/azure/ext/portal/portal/dashboard#ext_portal_az_portal_dashboard_update) komutunu kullanarak bir panoyu güncelleştirebilirsiniz:

```azurecli
az portal dashboard update --resource-group myResourceGroup --name 'Simple VM Dashboard' \
   --input-path portal-dashboard-template-testvm.json --location centralus
```

Sanal makineyle ilgili verileri Azure portal içinden görebildiğinizi doğrulayın.

1. Azure portalında **Pano**’yu seçin.

   ![Panoya gezinti Azure portal](media/quickstart-portal-dashboard-powershell/navigate-to-dashboards.png)

1. Pano sayfasında, **basıt VM panosu**' nu seçin.

   ![Basit VM panosuna git](media/quickstart-portal-dashboard-powershell/select-simple-vm-dashboard.png)

1. Panoyu gözden geçirin. İçeriğin bazılarının statik olduğunu, ancak VM 'nin performansını gösteren grafikler de olduğunu görebilirsiniz.

   ![Basit VM panosunu gözden geçirme](media/quickstart-portal-dashboard-powershell/review-simple-vm-dashboard.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal makineyi ve ilişkili panoyu kaldırmak için, bunları içeren kaynak grubunu silin.

> [!CAUTION]
> Aşağıdaki örnek, belirtilen kaynak grubunu ve içinde yer alan tüm kaynakları siler.
> Bu makalenin kapsamı dışındaki kaynaklar belirtilen kaynak grubunda mevcutsa, bunlar da silinir.

```azurecli
az group delete --name myResourceGroup
```

Yalnızca panoyu kaldırmak için [az Portal Dashboard Delete](/cli/azure/ext/portal/portal/dashboard#ext_portal_az_portal_dashboard_delete) komutunu kullanın:

```azurecli
az portal dashboard delete --resource-group myResourceGroup --name "Simple VM Dashboard"
```

## <a name="next-steps"></a>Sonraki adımlar

Panolar için Azure CLı desteği hakkında daha fazla bilgi için, bkz. [az Portal Dashboard](/cli/azure/ext/portal/portal/dashboard).
