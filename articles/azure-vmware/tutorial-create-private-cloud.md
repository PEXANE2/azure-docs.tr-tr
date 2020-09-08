---
title: Öğretici-Azure 'da vSphere kümesi dağıtma
description: Azure VMWare çözümünü kullanarak Azure 'da bir vSphere kümesi dağıtmayı öğrenin
ms.topic: tutorial
ms.date: 09/07/2020
ms.openlocfilehash: 69a29a459ba283bb34169112ac2fa174ac6a14af
ms.sourcegitcommit: 8791f69d44150767807d215cafc4076f3ed43f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89512394"
---
# <a name="tutorial-deploy-an-azure-vmware-solution-private-cloud-in-azure"></a>Öğretici: Azure 'da Azure VMware çözümü özel bulutu dağıtma

Azure VMware çözümü, Azure 'da bir vSphere kümesi dağıtmanıza olanak tanır. En düşük ilk dağıtım üç ana bilgisayar. Küme başına en fazla 16 ana bilgisayar için, ek konaklar tek seferde eklenebilir. 

Azure VMware çözümü, başlatma sırasında özel bulutunuzu şirket içi vCenter ile yönetmenize izin vermediğinden, yerel bir vCenter örneğine ek yapılandırma ve bağlantı, sanal ağ ve daha fazlası gereklidir. Bu yordamlar ve ilgili Önkoşullar Bu öğreticide ele alınmıştır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure VMware çözümü özel bulutu oluşturma
> * Özel bulutun dağıtıldığını doğrulayın

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Özel bir bulut oluşturmak için uygun yönetici hakları ve izinleri.
- [Öğretici: ağ denetim listesi](tutorial-network-checklist.md)' nde açıklandığı şekilde yapılandırılmış uygun ağa sahip olduğunuzdan emin olun.

## <a name="register-the-resource-provider"></a>Kaynak sağlayıcısını kaydetme

[!INCLUDE [register-resource-provider-steps](includes/register-resource-provider-steps.md)]


## <a name="create-a-private-cloud"></a>Özel Bulut oluşturma

[Azure Portal](#azure-portal) kullanarak veya [Azure CLI](#azure-cli)kullanarak bir Azure VMware çözümü özel bulutu oluşturabilirsiniz.

### <a name="azure-portal"></a>Azure portal

[!INCLUDE [create-avs-private-cloud-azure-portal](includes/create-avs-private-cloud-azure-portal-steps.md)]

### <a name="azure-cli"></a>Azure CLI

Azure VMware çözümü özel bulutu oluşturmak için Azure portal yerine Azure Cloud Shell kullanarak Azure CLı 'yi kullanabilirsiniz. Ortak Azure Araçları önceden yüklenmiş ve hesabınızla kullanılmak üzere yapılandırılmış olan ücretsiz etkileşimli bir kabuktur. 

#### <a name="open-azure-cloud-shell"></a>Azure Cloud Shell’i açma

Cloud Shell açmak için, bir kod bloğunun sağ üst köşesinden **dene** ' yi seçin. Ayrıca, ' a giderek ayrı bir tarayıcı sekmesinde Cloud Shell de başlatabilirsiniz [https://shell.azure.com/bash](https://shell.azure.com/bash) . Kod bloklarını kopyalamak için **Kopyala** ' yı seçin, Cloud Shell yapıştırın ve **ENTER** tuşuna basarak çalıştırın.

#### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myresourcegroup* adlı bir kaynak grubu oluşturur:

```
azurecli-interactive
az group create --name myResourceGroup --location eastus
```

#### <a name="create-a-private-cloud"></a>Özel bulut oluşturma

Bir kaynak grubu adı, özel bulut için bir ad, bir konum, küme boyutu belirtin.

| Özellik  | Açıklama  |
| --------- | ------------ |
| **-g** (kaynak grubu adı)     | Özel bulut kaynaklarınız için kaynak grubunun adı.        |
| **-n** (özel bulut adı)     | Azure VMware çözümünüz özel bulutunuzun adı.        |
| **--konum**     | Özel bulutunuz için kullanılan konum.         |
| **--küme-boyut**     | Kümenin boyutu. En küçük değer 3 ' dir.         |
| **--Ağ-bloğu**     | Özel bulutunuz için kullanılacak CıDR IP adresi ağ bloğu. Adres bloğu, aboneliğinizdeki ve şirket içi ağlardaki diğer sanal ağlarda kullanılan adres bloklarıyla çakışmamalıdır.        |
| **--SKU** | SKU değeri: AV36 |

```
azurecli-interactive
az vmware private-cloud create -g myResourceGroup -n myPrivateCloudName --location eastus --cluster-size 3 --network-block xx.xx.xx.xx/22 --sku AV36
```

## <a name="delete-a-private-cloud-azure-portal"></a>Özel bulutu silme (Azure portal)

Artık ihtiyacınız olmayan bir Azure VMware çözümü özel bulutunuz varsa, bunu silebilirsiniz. Özel bir bulutu sildiğinizde, tüm bileşenleriyle birlikte tüm kümeler silinir.

Bunu yapmak için, Azure portal özel buluta gidin ve **Sil**' i seçin. Onay sayfasında, özel bulutun adını doğrulayın ve **Evet**' i seçin.

> [!CAUTION]
> Özel bulutun silinmesi geri alınamaz bir işlemdir. Özel bulut silindikten sonra, tüm çalışan iş yüklerini sonlandırdığı ve tüm özel bulut verilerini ve genel IP adresleri dahil olmak üzere yapılandırma ayarlarını yok eden verileri kurtarılmaz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure VMware çözümü özel bulutu oluşturma
> * Dağıtılan özel bulut doğrulandı

Özel bulut kümeleriniz için yerel yönetim ayarlamanın bir parçası olarak özel bulutunuz ile kullanmak üzere bir sanal ağ oluşturmayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Sanal ağ oluşturma](tutorial-configure-networking.md)
