---
title: Azure özel bağlantı hizmeti ARM şablonu
description: Özel bağlantı hizmeti ARM şablonu
services: private-link
author: mblanco77
ms.service: private-link
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 05/29/2020
ms.author: allensu
ms.openlocfilehash: 304ee8c1180c318dd6e99b6e81eb964e264951d1
ms.sourcegitcommit: eeba08c8eaa1d724635dcf3a5e931993c848c633
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84667038"
---
# <a name="quickstart-create-a-private-link-service---resource-manager-template"></a>Hızlı başlangıç: özel bağlantı hizmeti oluşturma-Kaynak Yöneticisi şablonu

Bu hızlı başlangıçta, bir özel bağlantı hizmeti oluşturmak için bir Kaynak Yöneticisi şablonu kullanırsınız.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Bu hızlı başlangıcı [Azure Portal](create-private-link-service-portal.md), [Azure POWERSHELL](create-private-link-service-powershell.md)veya [Azure CLI](create-private-link-service-cli.md)kullanarak da tamamlayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-private-link-service"></a>Özel bağlantı hizmeti oluşturma

Bu şablon bir özel bağlantı hizmeti oluşturur.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-privatelink-service/).

:::code language="json" source="~/quickstart-templates/101-privatelink-service/azuredeploy.json" range="001-432" highlight="263-289":::

Şablonda birden çok Azure kaynağı tanımlanmış:

- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks) : her bir sanal makine için bir tane
- [**Microsoft. Network/loadBalancers**](/azure/templates/microsoft.network/loadBalancers) : hizmeti barındıran sanal makineleri kullanıma sunan yük dengeleyici
- [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces) : her bir sanal makine için bir tane olmak üzere 2 ağ arabirimi
- [**Microsoft. COMPUTE/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines) : hizmeti barındıran 2 sanal makine ve özel uç nokta bağlantısını test etmek için
- [**Microsoft. COMPUTE/virtualMachines/Extensions**](/azure/templates/Microsoft.Compute/virtualMachines/extensions) : Web sunucusu yükleyen uzantı
- [**Microsoft. Network/privateLinkServices**](/azure/templates/microsoft.network/privateLinkServices) : özel olarak hizmeti kullanıma sunmak için özel bağlantı hizmeti
- Her sanal makine için bir tane olmak üzere [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicIpAddresses) : 2 genel IP adresi
- [**Microsoft. Network/privateendpoints**](/azure/templates/microsoft.network/privateendpoints) : özel olarak hizmete erişmek için özel uç nokta

### <a name="deploy-the-template"></a>Şablonu dağıtma

Kaynak Yöneticisi şablonu Azure 'a dağıtma:

1. Azure 'da oturum açmak için **Azure 'A dağıt** ' ı seçin ve şablonu açın. Şablon bir sanal makine, standart yük dengeleyici, özel bağlantı hizmeti, Özel uç nokta, ağ ve doğrulanacak bir sanal makine oluşturur.

   [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-privatelink-service%2Fazuredeploy.json)

2. Kaynak grubunuzu seçin veya oluşturun,
3. Sanal Makine Yöneticisi Kullanıcı adını ve parolasını yazın.
4. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum** ' u seçin ve ardından **satın al**' ı seçin.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

> [!NOTE]
> ARM şablonu, myConsumerVm<b>{UniqueId}</b> kaynağı sanal makinesi için benzersiz bir ad oluşturuyor, lütfen <b>{UniqueId}</b> değerini üretilen değer ile değiştirin.

### <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

İnternet 'ten _Myconsumervm {UniqueId}_ sanal makinesine şu şekilde bağlanın:

1.  Portalın arama çubuğunda _Myconsumervm {UniqueId}_ girin.

2.  **Bağlan** düğmesini seçin. **Bağlan** düğmesini seçtikten sonra **sanal makineye bağlan** açılır.

3.  **RDP dosyasını indir**' i seçin. Azure bir Uzak Masaüstü Protokolü (_. rdp_) dosyası oluşturur ve bilgisayarınıza indirir.

4.  İndirilen. rdp dosyasını açın \* .

    a. İstendiğinde **Bağlan**’ı seçin.

    b. VM oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.
    
    > [!NOTE]
    > **More choices**  >  VM oluştururken girdiğiniz kimlik bilgilerini belirtmek için**farklı bir hesap kullan**' ı seçmeniz gerekebilir.

5.  **Tamam**’ı seçin.

6.  Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız **Evet** ' i veya **devam et**' i seçin.

7.  VM masaüstü seçildikten sonra, bunu yerel masaüstünüze geri dönmek için simge durumuna küçültün.

### <a name="access-the-http-service-privately-from-the-vm"></a>Http hizmetine özel olarak VM 'den erişin

Bu bölümde, Özel uç nokta kullanarak VM 'den http hizmetine bağlanırsınız.

1.  _Myconsumervm {UniqueId}_ uzak masaüstünde
2.  Bir tarayıcı açın ve özel uç nokta adresini girinhttp://10.0.0.5/
3.  Varsayılan IIS sayfasını görürsünüz

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel bağlantı hizmeti ile oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, kaynak grubunu silin. Bu, özel bağlantı hizmetini ve ilgili tüm kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet 'ini çağırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure özel bağlantısı](private-link-overview.md) hakkında daha fazla bilgi edinin
