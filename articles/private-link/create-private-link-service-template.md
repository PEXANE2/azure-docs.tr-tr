---
title: Azure özel bağlantısında özel bağlantı hizmeti oluşturma
description: Bu hızlı başlangıçta, bir özel bağlantı hizmeti oluşturmak için bir Azure Resource Manager şablonu (ARM şablonu) kullanırsınız.
services: private-link
author: mblanco77
ms.service: private-link
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 05/29/2020
ms.author: allensu
ms.openlocfilehash: 2a3c7245a4e6c69e87791ca3364ad588b82572c6
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86529616"
---
# <a name="quickstart-create-a-private-link-service-by-using-an-arm-template"></a>Hızlı başlangıç: ARM şablonu kullanarak özel bağlantı hizmeti oluşturma

Bu hızlı başlangıçta, bir özel bağlantı hizmeti oluşturmak için bir Azure Resource Manager şablonu (ARM şablonu) kullanırsınız.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Bu hızlı başlangıcı [Azure Portal](create-private-link-service-portal.md), [Azure POWERSHELL](create-private-link-service-powershell.md)veya [Azure CLI](create-private-link-service-cli.md)kullanarak da tamamlayabilirsiniz.

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-privatelink-service%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

Etkin aboneliği olan bir Azure hesabınızın olması gerekir. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu şablon bir özel bağlantı hizmeti oluşturur.

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/101-privatelink-service/) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-privatelink-service/azuredeploy.json" range="001-432" highlight="263-289":::

Şablonda birden çok Azure kaynağı tanımlanmış:

- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks): her bir sanal makine için bir sanal ağ bulunur.
- [**Microsoft. Network/loadBalancers**](/azure/templates/microsoft.network/loadBalancers): hizmeti barındıran sanal makineleri kullanıma sunan yük dengeleyici.
- [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces): her bir sanal makine için bir tane olmak üzere iki ağ arabirimi vardır.
- [**Microsoft. COMPUTE/virtualmachines**](/azure/templates/microsoft.compute/virtualmachines): biri hizmeti barındıran ve özel uç noktayla bağlantıyı test eden iki sanal makine vardır.
- [**Microsoft. COMPUTE/virtualMachines/Extensions**](/azure/templates/Microsoft.Compute/virtualMachines/extensions): bir Web sunucusu yükleyen uzantı.
- [**Microsoft. Network/privateLinkServices**](/azure/templates/microsoft.network/privateLinkServices): hizmeti kullanıma sunmak için özel bağlantı hizmeti.
- [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicIpAddresses): her bir sanal makine için bir tane olmak üzere ıkı genel IP adresi vardır.
- [**Microsoft. Network/privateendpoints**](/azure/templates/microsoft.network/privateendpoints): hizmete erişmek için özel uç nokta.

## <a name="deploy-the-template"></a>Şablonu dağıtma

ARM şablonunu Azure 'a dağıtma:

1. Azure 'da oturum açmak ve şablonu açmak için **Azure 'A dağıt**' ı seçin. Şablon bir sanal makine, standart yük dengeleyici, özel bağlantı hizmeti, Özel uç nokta, ağ ve doğrulanacak bir sanal makine oluşturur.

   [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-privatelink-service%2Fazuredeploy.json)

2. Kaynak grubunuzu seçin veya oluşturun.
3. Sanal Makine Yöneticisi Kullanıcı adını ve parolasını yazın.
4. Hüküm ve koşullar ekstresini okuyun. Kabul ediyorsanız, satın alma sırasında **belirtilen hüküm ve koşulları kabul**ediyorum ' u seçin  >  **Purchase**.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

> [!NOTE]
> ARM şablonu, sanal makine myConsumerVm<b>{UniqueId}</b> kaynağı için benzersiz bir ad oluşturur. **{UniqueId}** için üretilen değeri değiştirin.

### <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

İnternet 'ten _Myconsumervm {UniqueId}_ sanal makinesine şu şekilde bağlanın:

1.  Portalın arama çubuğunda _Myconsumervm {UniqueId}_ girin.

2.  **Bağlan**'ı seçin. **Sanal makineye bağlan** açılır.

3.  **RDP Dosyasını İndir**’i seçin. Azure bir Uzak Masaüstü Protokolü (_. rdp_) dosyası oluşturur ve bilgisayarınıza indirir.

4.  İndirilen .rdp dosyasını açın.

    a. İstendiğinde **Bağlan**’ı seçin.

    b. VM oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.
    
    > [!NOTE]
    > **More choices**  >  VM oluştururken girdiğiniz kimlik bilgilerini belirtmek için**farklı bir hesap kullan**' ı seçmeniz gerekebilir.

5.  **Tamam**’ı seçin.

6.  Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız **Evet** ' i veya **devam et**' i seçin.

7.  VM masaüstü görüntülendikten sonra, bunu yerel masaüstünüze geri dönmek için simge durumuna küçültün.

### <a name="access-the-http-service-privately-from-the-vm"></a>Http hizmetine özel olarak VM 'den erişin

Özel uç nokta kullanılarak VM 'den http hizmetine nasıl bağlanaöğreneceksiniz.

1.  _Myconsumervm {UniqueId}_ Uzak masaüstüne gidin.
2.  Bir tarayıcı açın ve özel uç nokta adresini girin: `http://10.0.0.5/` .
3.  Varsayılan IIS sayfası görüntülenir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel bağlantı hizmeti ile oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, kaynak grubunu silin. Bu, özel bağlantı hizmetini ve ilgili tüm kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet 'ini çağırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure özel bağlantısı](private-link-overview.md)hakkında daha fazla bilgi edinin.
