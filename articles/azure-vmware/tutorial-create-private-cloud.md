---
title: Öğretici-Azure 'da vSphere kümesi dağıtma
description: Azure VMware çözümünü kullanarak Azure 'da vSphere kümesi dağıtmayı öğrenin
ms.topic: tutorial
ms.date: 11/19/2020
ms.openlocfilehash: 3c8ae3673ad049153c2b9700bd7efae6c4c286ed
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100093956"
---
# <a name="tutorial-deploy-an-azure-vmware-solution-private-cloud-in-azure"></a>Öğretici: Azure 'da Azure VMware çözümü özel bulutu dağıtma

Azure VMware çözümü, Azure 'da bir vSphere kümesi dağıtmanıza olanak tanır. En düşük ilk dağıtım üç ana bilgisayar. Küme başına en fazla 16 ana bilgisayar için, ek konaklar tek seferde eklenebilir. 

Azure VMware çözümü, başlatma sırasında özel bulutunuzu şirket içi vCenter 'ünüzle yönetmenize izin vermediğinden ek yapılandırma gerekir. Bu yordamlar ve ilgili Önkoşullar Bu öğreticide ele alınmıştır.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Azure VMware çözümü özel bulutu oluşturma
> * Özel bulutun dağıtıldığını doğrulayın

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Özel bir bulut oluşturmak için uygun yönetici hakları ve izinleri.
- [Öğretici: ağ denetim listesi](tutorial-network-checklist.md)' nde açıklandığı şekilde yapılandırılmış uygun ağa sahip olduğunuzdan emin olun.

## <a name="register-the-resource-provider"></a>Kaynak sağlayıcısını kaydetme

[!INCLUDE [register-resource-provider-steps](includes/register-resource-provider-steps.md)]


## <a name="create-a-private-cloud"></a>Özel Bulut oluşturma

[Azure Portal](#azure-portal) kullanarak veya [Azure CLI](#azure-cli)kullanarak bir Azure VMware çözümü özel bulutu oluşturabilirsiniz.

### <a name="azure-portal"></a>Azure portal

[!INCLUDE [create-avs-private-cloud-azure-portal](includes/create-private-cloud-azure-portal-steps.md)]

### <a name="azure-cli"></a>Azure CLI

Azure VMware çözümü özel bulutu oluşturmak için Azure portal yerine Azure Cloud Shell kullanarak Azure CLı 'yi kullanabilirsiniz.  Azure VMware çözümü ile kullanabileceğiniz komutların listesi için bkz. [Azure VMware komutları](/cli/azure/ext/vmware/vmware).

#### <a name="open-azure-cloud-shell"></a>Azure Cloud Shell’i açma

Bir kod bloğunun sağ üst köşesinden **deneyin** öğesini seçin. Ayrıca, ' a giderek ayrı bir tarayıcı sekmesinde Cloud Shell de başlatabilirsiniz [https://shell.azure.com/bash](https://shell.azure.com/bash) . Kod bloklarını kopyalamak için **Kopyala** ' yı seçin, Cloud Shell yapıştırın ve **ENTER** tuşuna basarak çalıştırın.

#### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

`[az group create](/cli/azure/group)` komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myresourcegroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive

az group create --name myResourceGroup --location eastus
```

#### <a name="create-a-private-cloud"></a>Özel bulut oluşturma

Kaynak grubu ve özel bulut, bir konum ve kümenin boyutu için bir ad sağlayın.

| Özellik  | Açıklama  |
| --------- | ------------ |
| **-g** (kaynak grubu adı)     | Özel bulut kaynaklarınız için kaynak grubunun adı.        |
| **-n** (özel bulut adı)     | Azure VMware çözümünüz özel bulutunuzun adı.        |
| **--konum**     | Özel bulutunuz için kullanılan konum.         |
| **--küme-boyut**     | Kümenin boyutu. En küçük değer 3 ' dir.         |
| **--Ağ-bloğu**     | Özel bulutunuz için kullanılacak CıDR IP adresi ağ bloğu. Adres bloğu, aboneliğinizdeki ve şirket içi ağlardaki diğer sanal ağlarda kullanılan adres bloklarıyla çakışmamalıdır.        |
| **--SKU** | SKU değeri: AV36 |

```azurecli-interactive
az vmware private-cloud create -g myResourceGroup -n myPrivateCloudName --location eastus --cluster-size 3 --network-block xx.xx.xx.xx/22 --sku AV36
```

## <a name="azure-vmware-commands"></a>Azure VMware komutları

Azure VMware çözümü ile kullanabileceğiniz komutların listesi için bkz. [Azure VMware komutları](/cli/azure/ext/vmware/vmware).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure VMware çözümü özel bulutu oluşturma
> * Özel bulutun dağıtıldığını doğrulayın
> * Azure VMware çözümünün özel bulutunu silme

Bir sıçrama kutusu oluşturmayı öğrenmek için bir sonraki öğreticiye geçin. Özel bulutunuzu yerel olarak yönetebilmeniz için ortamınıza bağlanmak üzere bağlantı kutusunu kullanın.


> [!div class="nextstepaction"]
> [Azure VMware çözümüne özel buluta erişme](tutorial-access-private-cloud.md)