---
title: Öğretici-Azure 'da vSphere kümesi dağıtma
description: Azure VMWare çözümünü (AVS) kullanarak Azure 'da bir vSphere kümesi dağıtmayı öğrenin
ms.topic: tutorial
ms.date: 07/15/2020
ms.openlocfilehash: 4f3b33ea401c62124ae5f8a4c881d86d2f19b40c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079426"
---
# <a name="tutorial-deploy-an-avs-private-cloud-in-azure"></a>Öğretici: Azure 'da AVS özel bulutu dağıtma

Azure VMware çözümü (AVS), Azure 'da bir vSphere kümesi dağıtmanıza olanak tanır. En düşük ilk dağıtım üç ana bilgisayar. Küme başına en fazla 16 ana bilgisayar için, ek konaklar tek seferde eklenebilir. 

AVS, başlatma sırasında özel bulutunuzu şirket içi vCenter ile yönetmenize izin vermediğinden, yerel bir vCenter örneğine ek yapılandırma ve bağlantı, sanal ağ ve daha fazlası gereklidir. Bu yordamlar ve ilgili Önkoşullar Bu öğreticide ele alınmıştır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * AVS özel bulutu oluşturma
> * Özel bulutun dağıtıldığını doğrulayın

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Özel bir bulut oluşturmak için uygun yönetici hakları ve izinleri.
- [Öğretici: ağ denetim listesi](tutorial-network-checklist.md)' nde açıklandığı şekilde yapılandırılmış uygun ağa sahip olduğunuzdan emin olun.

## <a name="register-the-resource-provider"></a>Kaynak sağlayıcısını kaydetme

AVS 'yi kullanmak için, öncelikle kaynak sağlayıcısını aboneliğinize kaydetmeniz gerekir.

```
azurecli-interactive
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

Kaynak sağlayıcısını kaydetmek için ek yollar için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md).


## <a name="create-a-private-cloud"></a>Özel Bulut oluşturma

[Azure Portal](#azure-portal) kullanarak veya [Azure CLı](#azure-cli)kullanarak bir AVS özel bulutu oluşturabilirsiniz.

### <a name="azure-portal"></a>Azure portalı

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. **Yeni kaynak oluştur**' u seçin. **Market** metin kutusu yazın `Azure VMware Solution` ve listeden **Azure VMware çözümü** ' nü seçin. **Azure VMware Çözüm** penceresinde **Oluştur** ' u seçin.

1. **Temel bilgiler** sekmesinde, alanlar için değerler girin. Aşağıdaki tabloda alanlarıyla ilgili özellikler listelenmiştir.

   | Alan   | Değer  |
   | ---| --- |
   | **Abonelik** | Dağıtım için kullanmayı planladığınız abonelik.|
   | **Kaynak grubu** | Özel bulut kaynaklarınızın kaynak grubu. |
   | **Konum** | **Doğu ABD**gibi bir konum seçin.|
   | **Kaynak adı** | AVS özel bulutunuzun adı. |
   | **SKU** | Şu SKU değerini seçin: AV36 |
   | **Bilgisayarlarınızı** | Özel bulut kümesine eklenecek ana bilgisayar sayısı. Varsayılan değer, dağıtımdan sonra ortaya çıkarılan veya düşürülen 3 ' dir.  |
   | **vCenter yönetici parolası** | Bir bulut Yöneticisi parolası girin. |
   | **NSX-T Yöneticisi parolası** | NSX-T yönetici parolası girin. |
   | **Adres bloğu** | Özel bulut için CıDR ağı için bir IP adresi bloğu girin, örneğin, 10.175.0.0/22. |

   :::image type="content" source="./media/tutorial-create-private-cloud/create-private-cloud.png" alt-text="özel bulut oluşturma" border="true":::

1. İşiniz bittiğinde, **gözden geçir + oluştur**' u seçin. Sonraki ekranda, girilen bilgileri doğrulayın. Bilgilerin tümü doğru ise **Oluştur**' u seçin.

   > [!NOTE]
   > Bu adım kabaca iki saat sürer. 

1. Dağıtımın başarılı olduğunu doğrulayın. Oluşturduğunuz kaynak grubuna gidin ve özel bulutunuzu seçin.  Dağıtım tamamlandığında **başarılı** durumunu görürsünüz. 

   :::image type="content" source="./media/tutorial-create-private-cloud/validate-deployment.png" alt-text="Dağıtılan özel bulutu doğrula" border="true":::

### <a name="azure-cli"></a>Azure CLI

Azure portal yerine, bir AVS özel bulutu oluşturmak için Azure Cloud Shell kullanarak Azure CLı 'yi kullanabilirsiniz. Ortak Azure Araçları önceden yüklenmiş ve hesabınızla kullanılmak üzere yapılandırılmış olan ücretsiz etkileşimli bir kabuktur. 

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
| **-n** (özel bulut adı)     | AVS özel bulutunuzun adı.        |
| **--konum**     | Özel bulutunuz için kullanılan konum.         |
| **--küme-boyut**     | Kümenin boyutu. En küçük değer 3 ' dir.         |
| **--Ağ-bloğu**     | Özel bulutunuz için kullanılacak CıDR IP adresi ağ bloğu. Adres bloğu, aboneliğinizdeki ve şirket içi ağlardaki diğer sanal ağlarda kullanılan adres bloklarıyla çakışmamalıdır.        |
| **--SKU** | SKU değeri: AV36 |

```
azurecli-interactive
az vmware private-cloud create -g myResourceGroup -n myPrivateCloudName --location eastus --cluster-size 3 --network-block xx.xx.xx.xx/22 --sku AV36
```

## <a name="delete-a-private-cloud-azure-portal"></a>Özel bulutu silme (Azure portal)

Artık ihtiyacınız olmayan bir AVS özel bulutunuz varsa, bunu silebilirsiniz. Özel bir bulutu sildiğinizde, tüm bileşenleriyle birlikte tüm kümeler silinir.

Bunu yapmak için, Azure portal özel buluta gidin ve **Sil**' i seçin. Onay sayfasında, özel bulutun adını doğrulayın ve **Evet**' i seçin.

> [!CAUTION]
> Özel bulutun silinmesi geri alınamaz bir işlemdir. Özel bulut silindikten sonra, tüm çalışan iş yüklerini sonlandırdığı ve tüm özel bulut verilerini ve genel IP adresleri dahil olmak üzere yapılandırma ayarlarını yok eden verileri kurtarılmaz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * AVS özel bulutu oluşturma
> * Dağıtılan özel bulut doğrulandı

Özel bulut kümeleriniz için yerel yönetim ayarlamanın bir parçası olarak özel bulutunuz ile kullanmak üzere bir sanal ağ oluşturmayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Sanal ağ oluşturma](tutorial-configure-networking.md)
