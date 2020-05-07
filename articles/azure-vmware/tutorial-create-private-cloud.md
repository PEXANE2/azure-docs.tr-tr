---
title: Öğretici-Azure 'da vSphere kümesi dağıtma
description: Azure VMWare çözümünü (AVS) kullanarak Azure 'da bir vSphere kümesi dağıtmayı öğrenin
ms.topic: tutorial
ms.date: 05/04/2020
ms.openlocfilehash: 712be25acf5984a4bcdf95ad70e0ccfa660c06bc
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82838815"
---
# <a name="tutorial-deploy-an-avs-private-cloud-in-azure"></a>Öğretici: Azure 'da AVS özel bulutu dağıtma

Azure VMware çözümü (AVS), Azure 'da bir vSphere kümesi dağıtmanıza olanak tanır. En düşük ilk dağıtım üç ana bilgisayar. Küme başına en fazla 16 ana bilgisayar için, ek konaklar tek seferde eklenebilir. 

AVS, özel bulutunuzu başlatma sırasında şirket içi vCenter 'inizle yönetmenizi sağlayacağından, yerel bir vCenter örneğine, sanal ağa ve daha fazlasına yönelik ek yapılandırma ve bağlantı gerçekleştirmeniz gerekir. Bu yordamlar ve ilgili Önkoşullar Bu öğretici serisinde ele alınacaktır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * AVS özel bulutu oluşturma
> * Özel bulutun dağıtıldığını doğrulayın

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Özel bir bulut oluşturmak için uygun yönetici hakları ve izinleri.
- [Öğretici: ağ denetim listesi](tutorial-network-checklist.md)' nde açıklandığı şekilde yapılandırılmış uygun ağa sahip olduğunuzdan emin olun.

## <a name="register-the-resource-provider"></a>Kaynak sağlayıcısını kaydetme

Azure VMWare çözümünü kullanabilmeniz için öncelikle kaynak sağlayıcısını kaydetmeniz gerekir. Aşağıdaki örnek, aboneliğiniz ile kaynak sağlayıcısını kaydeder.

```azurecli-interactive
az provider register -n Microsoft.VMwareVirtustream --subscription <your subscription ID>
```

Kaynak sağlayıcısını kaydetmek için ek yollar için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="create-a-private-cloud"></a>Özel Bulut oluşturma

[Azure Portal](#azure-portal) kullanarak veya [Azure CLı](#azure-cli)kullanarak bir AVS özel bulutu oluşturabilirsiniz.

### <a name="azure-portal"></a>Azure portal

Azure portal **+ Yeni kaynak oluştur**' u seçin. **Market** metin kutusu yazın `Azure VMware Solution`ve listeden **Azure VMware çözümü** ' nü seçin. **Azure VMware Çözüm** penceresinde **Oluştur** ' u seçin.

**Temel bilgiler** sekmesinde, alanlar için değerler girin. Aşağıdaki tabloda özelliklerin ayrıntılı bir listesi gösterilmektedir.

| Alan   | Değer  |
| ---| --- |
| **Abonelik** | Dağıtım için kullanmayı planladığınız abonelik.|
| **Kaynak grubu** | Özel bulut kaynaklarınızın kaynak grubu. |
| **Konum** | **Doğu ABD**gibi bir konum seçin.|
| **Kaynak adı** | AVS özel bulutunuzun adı. |
| **ISTEYIN** | Uygun SKU 'yu seçin, kullanılabilir değerler şunlardır: |
| **Bilgisayarlarınızı** | Bu, özel bulut kümesine eklenecek ana bilgisayar sayısıdır. Varsayılan değer 3 ' dir. Bu değer dağıtımdan sonra yükseltilebilir veya düşürülemez.  |
| **vCenter yönetici parolası** | Bir bulut Yöneticisi parolası girin. |
| **NSX-T Yöneticisi parolası** | NSX-T yönetici parolasını girin. |
| **Adres bloğu** | Özel bulut için CıDR ağı için bir IP adresi bloğu girin. Örneğin, 10.175.0.0/22. |

:::image type="content" source="./media/tutorial-create-private-cloud/create-private-cloud.png" alt-text="özel bulut oluşturma" border="true":::

İşiniz bittiğinde, **gözden geçir + oluştur**' u seçin. Sonraki ekranda, girilen bilgileri doğrulayın. Bilgilerin tümü doğru ise **Oluştur**' u seçin.

> [!NOTE]
> Bu adım kabaca iki saat sürer. 

### <a name="azure-cli"></a>Azure CLI

Alternatif olarak, Azure 'da bir AVS özel bulutu oluşturmak için Azure CLı 'yi de kullanabilirsiniz. Bunu yapmak için Azure Cloud Shell kullanabilirsiniz, aşağıdaki adımlarda bunun nasıl yapılacağı gösterilmektedir.

#### <a name="open-azure-cloud-shell"></a>Azure Cloud Shell’i açma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz https://shell.azure.com/bash adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. Kod bloklarını kopyalamak için **Kopyala** ' yı seçin, Cloud Shell yapıştırın ve **ENTER** tuşuna basarak çalıştırın.

#### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myresourcegroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

#### <a name="create-a-private-cloud"></a>Özel bulut oluşturma

Bir AVS özel bulutu oluşturmak için, bir kaynak grubu adı, özel bulut için bir ad, bir konum, küme boyutu belirtmeniz gerekir


|Özellik  |Açıklama  |
|---------|---------|
|Kaynak grubu adı     | Özel bulutu dağıttığınız kaynak grubunun adı.        |
|Özel bulut adı     | Özel bulutun adı.        |
|Konum     | Özel bulut için kullanılan konum         |
|Küme boyutu     | Kümenin boyutu. En küçük değer 3 ' dir.         |
|Ağ bloğu     | Özel bulut için kullanılacak CıDR aralığı. Azure ortamınızın yanı sıra şirket içi ortamınızdan da benzersiz olması önerilir.        |

```azurecli-interactive
az vmware private-cloud create -g myResourceGroup -n myPrivateCloudName --location eastus --cluster-size 3 --network-block xx.xx.xx.xx/22
```

## <a name="verify-deployment-was-successful"></a>Dağıtımın başarılı olduğunu doğrulama

Oluşturduğunuz kaynak grubuna gidin ve özel bulutunuzu seçin, dağıtım tamamlandığında aşağıdaki ekranı görürsünüz ve **başarılı**durumunu görürsünüz.

:::image type="content" source="./media/tutorial-create-private-cloud/validate-deployment.png" alt-text="Dağıtılan özel bulutu doğrula" border="true":::

## <a name="delete-a-private-cloud"></a>Özel bulut silme

Artık ihtiyacınız olmayan bir AVS özel bulutunuz varsa, bunu silebilirsiniz. Özel bir bulutu sildiğinizde, tüm bileşenleriyle birlikte tüm kümeler silinir.

Bunu yapmak için, Azure portal özel buluta gidin ve **Sil**' i seçin. Onay sayfasında, özel bulutun adını doğrulayın ve **Evet**' i seçin.

> [!CAUTION]
> Özel bulutun silinmesi geri alınamaz bir işlemdir. Özel bulut silindikten sonra, tüm çalışan iş yüklerini, bileşenleri sonlandırdığı ve genel IP adresleri dahil tüm özel bulut verilerini ve yapılandırma ayarlarını yok eden veriler kurtarılamaz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * AVS özel bulutu oluşturma
> * Dağıtılan özel bulut doğrulandı

Özel bulut kümeleriniz için yerel yönetim ayarlamanın bir parçası olarak özel bulutunuz ile kullanmak üzere bir sanal ağ oluşturmayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Sanal ağ oluşturma](tutorial-configure-networking.md)
