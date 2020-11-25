---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 11/20/2020
ms.author: parkerra
ms.openlocfilehash: 131b21ea7bc47df9654dd7c163eb22adb68e6678
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95971418"
---
## <a name="create-a-spatial-anchors-resource"></a>Uzamsal bağlayıcı kaynağı oluşturma

### <a name="portal"></a>[Portal](#tab/azure-portal)

<a href="https://portal.azure.com" target="_blank">Azure portalına</a> gidin.

Sol bölmede **kaynak oluştur**' u seçin.

**Uzamsal bağlantıları** aramak için arama kutusunu kullanın.

![Uzamsal Tutturucuların aramasının sonuçlarını gösteren ekran görüntüsü.](./media/spatial-anchors-get-started-create-resource/portal-search.png)

**Uzamsal bağlayıcıları** seçin ve ardından **Oluştur**' u seçin.

**Uzamsal bağlayıcı hesabı** bölmesinde şunları yapın:

* Normal alfasayısal karakterleri kullanarak benzersiz bir kaynak adı girin.  
* Kaynağı iliştirmek istediğiniz aboneliği seçin.  
* **Yeni oluştur** seçeneğini belirleyerek bir kaynak grubu oluşturun. **Myresourcegroup** olarak adlandırın ve ardından **Tamam**' ı seçin.  

  [!INCLUDE [resource group intro text](resource-group.md)]
  
* Kaynağın yerleştirileceği bir konum (bölge) seçin.  
* Kaynağı oluşturmaya başlamak için **Yeni** ' yi seçin.

![Kaynak oluşturmak için uzamsal bağlayıcı bölmesinin ekran görüntüsü.](./media/spatial-anchors-get-started-create-resource/create-resource-form.png)

Kaynak oluşturulduktan sonra, Azure portal dağıtımınızın tamamlandığını gösterir. 
   
![Kaynak dağıtımının tamamlandığını gösteren ekran görüntüsü.](./media/spatial-anchors-get-started-create-resource/deployment-complete.png)

**Kaynağa git**’i seçin. Artık kaynak özelliklerini görüntüleyebilirsiniz. 
   
Kaynağın **hesap kimliği** değerini daha sonra kullanmak üzere bir metin düzenleyicisine kopyalayın.

![Kaynak özellikleri bölmesinin ekran görüntüsü.](./media/spatial-anchors-get-started-create-resource/view-resource-properties.png)

Ayrıca, kaynağın **hesap etki alanı** değerini daha sonra kullanmak üzere bir metin düzenleyicisine kopyalayın.

![Kaynağın hesap etki alanı değerini gösteren ekran görüntüsü.](./media/spatial-anchors-get-started-create-resource/view-resource-domain.png)

**Ayarlar** altında **anahtar**' ı seçin. **Birincil anahtar** değerini, **hesap anahtarını** daha sonra kullanmak üzere bir metin düzenleyicisine kopyalayın.

![Hesap için anahtarlar bölmesinin ekran görüntüsü.](./media/spatial-anchors-get-started-create-resource/view-account-key.png)

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Ortamınızı Azure CLı için hazırlarken başlayın:

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](azure-cli-prepare-your-environment-no-header.md)]

1. Oturum açtıktan sonra, uzamsal bağlayıcı hesabının ayarlanacağı aboneliği seçmek için [az Account set](/cli/azure/account#az_account_set) komutunu kullanın:

   ```azurecli
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

1. Bir kaynak grubu oluşturmak için [az Group Create](/cli/azure/group#az_group_create) komutunu çalıştırın veya var olan bir kaynak grubunu kullanın:

   ```azurecli
   az group create --name myResourceGroup --location eastus2
   ```

   [!INCLUDE [resource group intro text](resource-group.md)]

   [Az uzamsal-bağlayıcı hesabı listesi](/cli/azure/ext/mixed-reality/spatial-anchors-account#ext_mixed_reality_az_spatial_anchors_account_list) komutunu kullanarak bir kaynak grubu için geçerli uzamsal bağlayıcı hesaplarınızı görüntüleyebilirsiniz:

   ```azurecli
   az spatial-anchors-account list --resource-group myResourceGroup
   ```

   Aboneliğiniz için de uzamsal bağlayıcı hesaplarını görüntüleyebilirsiniz:

   ```azurecli
   az spatial-anchors-account list
   ```

1. Uzamsal bağlayıcı hesabınızı oluşturmak için [az uzamsal-bağlayıcı-hesap oluştur](/cli/azure/ext/mixed-reality/spatial-anchors-account#ext_mixed_reality_az_spatial_anchors_account_create) komutunu çalıştırın:

   ```azurecli
   az spatial-anchors-account create --resource-group myResourceGroup --name MySpatialAnchorsQuickStart --location eastus2
   ```

1. [Az uzamsal-bağlayıcı-hesap göster](/cli/azure/ext/mixed-reality/spatial-anchors-account#ext_mixed_reality_az_spatial_anchors_account_show) komutunu kullanarak kaynak özelliklerini görüntüleyin:

   ```azurecli
   az spatial-anchors-account show --resource-group myResourceGroup --name MySpatialAnchorsQuickStart
   ```

   Kaynak **hesap kimliği** değerini ve kaynak **hesabı etki alanı** değerini daha sonra kullanmak üzere bir metin düzenleyicisine kopyalayın.

1. Birincil ve ikincil anahtarlarınızı almak için [az uzamsal-bağlayıcı-hesap anahtarı göster](/cli/azure/ext/mixed-reality/spatial-anchors-account/key#ext_mixed_reality_az_spatial_anchors_account_key_show) komutunu çalıştırın:

   ```azurecli
   az spatial-anchors-account key show --resource-group myResourceGroup --name MySpatialAnchorsQuickStart
   ```

   Anahtar değerlerini daha sonra kullanmak üzere bir metin düzenleyicisine kopyalayın.

   Anahtarları yeniden oluşturmanız gerekiyorsa, [az uzamsal-bağlayıcı-hesap anahtar yenileme](/cli/azure/ext/mixed-reality/spatial-anchors-account/key#ext_mixed_reality_az_spatial_anchors_account_key_renew) komutunu kullanın:

   ```azurecli
   az spatial-anchors-account key renew --resource-group myResourceGroup --name example --key primary
   az spatial-anchors-account key renew --resource-group myResourceGroup --name example --key secondary
   ```

[Az uzamsal-bağlayıcı-Account Delete](/cli/azure/ext/mixed-reality/spatial-anchors-account#ext_mixed_reality_az_spatial_anchors_account_delete) komutunu kullanarak bir hesabı silebilirsiniz:

```azurecli
az spatial-anchors-account delete --resource-group myResourceGroup --name MySpatialAnchorsQuickStart
```

---
