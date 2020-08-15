---
title: Azure Uygulama yapılandırması hızlı başlangıç ile otomatik VM dağıtımı
description: Bu hızlı başlangıçta, Azure uygulama yapılandırma deposu dağıtmak için Azure PowerShell modülünün ve Azure Resource Manager şablonlarının nasıl kullanılacağı gösterilmektedir. Ardından, bir VM dağıtmak için depodaki değerleri kullanın.
author: lisaguthrie
ms.author: lcozzens
ms.date: 08/11/2020
ms.topic: quickstart
ms.service: azure-app-configuration
ms.custom:
- mvc
- subject-armqs
ms.openlocfilehash: 9b609d4571d6240f428a0210aa5108ff19dc753b
ms.sourcegitcommit: 3bf69c5a5be48c2c7a979373895b4fae3f746757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88235188"
---
# <a name="quickstart-automated-vm-deployment-with-app-configuration-and-resource-manager-template-arm-template"></a>Hızlı başlangıç: uygulama yapılandırması ve Kaynak Yöneticisi şablonuyla otomatik VM dağıtımı (ARM şablonu)

Azure Resource Manager şablonları ve Azure PowerShell kullanarak bir Azure uygulama yapılandırma deposu dağıtma, depoya anahtar değerleri ekleme ve bu örnekteki Azure sanal makinesi gibi bir Azure kaynağını dağıtmak için depodaki anahtar değerlerini kullanma hakkında bilgi edinin.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration-store%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="review-the-templates"></a>Şablonları gözden geçirin

Bu hızlı başlangıçta kullanılan şablonlar [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/)alınır. [İlk şablon](https://azure.microsoft.comresources/templates/101-app-configuration-store/) bir uygulama yapılandırma deposu oluşturur:

:::code language="json" source="~/quickstart-templates/101-app-configuration-store/azuredeploy.json" range="1-37" highlight="27-35":::

Şablonda bir Azure kaynağı tanımlanmıştır:

- [Microsoft. AppConfiguration/Configurationmağazaların](/azure/templates/microsoft.appconfiguration/2019-10-01/configurationstores): bir uygulama yapılandırma deposu oluşturun.

[İkinci şablon](https://azure.microsoft.com/resources/templates/101-app-configuration/) , depodaki anahtar değerlerini kullanarak bir sanal makine oluşturur. Bu adımdan önce, portal veya Azure CLı kullanarak anahtar değerleri eklemeniz gerekir.

:::code language="json" source="~/quickstart-templates/101-app-configuration/azuredeploy.json" range="1-217" highlight="77, 181,189":::

## <a name="deploy-the-templates"></a>Şablonları dağıtma

### <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon bir uygulama yapılandırma deposu oluşturur.

    [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration-store%2Fazuredeploy.json)

1. Aşağıdaki değerleri seçin veya girin.

    - **abonelik**: uygulama yapılandırma deposunu oluşturmak Için kullanılan Azure aboneliğini seçin.
    - **Kaynak grubu**: var olan bir kaynak grubunu kullanmak istemediğiniz müddetçe yeni bir kaynak grubu oluşturmak Için **Yeni oluştur** ' u seçin.
    - **Bölge**: kaynak grubu için bir konum seçin.  Örneğin, **Doğu ABD**.
    - **Yapılandırma deposu adı**: yeni bir uygulama yapılandırma deposu adı girin.
    - **Konum**: uygulama yapılandırma deposunun konumunu belirtin.  Varsayılan değeri kullanın.
    - **SKU adı**: uygulama yapılandırma deposunun SKU adını belirtin. Varsayılan değeri kullanın.

1. **Gözden geçir + oluştur**’u seçin.
1. Sayfanın **doğrulama geçtiğini**gösterdiğini doğrulayın ve ardından **Oluştur**' u seçin.

Kaynak grubu adını ve uygulama yapılandırma deposu adını bir yere getirin.  Sanal makineyi dağıtırken bu değerlere ihtiyacınız vardır
### <a name="add-vm-configuration-key-values"></a>VM yapılandırması anahtar değerlerini ekleme

Bir uygulama yapılandırma deposu oluşturduktan sonra, depoya anahtar değerler eklemek için Azure portal veya Azure CLı 'yi kullanabilirsiniz.

1. [Azure Portal](https://portal.azure.com)oturum açın ve ardından yeni oluşturulan uygulama yapılandırma deposuna gidin.
1. Sol menüden **yapılandırma Gezgini** ' ni seçin.
1. Aşağıdaki anahtar-değer çiftlerini eklemek için **Oluştur** ' u seçin:

   |Anahtar|Değer|Etiketle|
   |-|-|-|
   |windowsOsVersion|2019-veri merkezi|şablon|
   |diskSizeGB|1023|şablon|

   **Içerik türünü** boş tut.

Azure CLı 'yi kullanmak için bkz. [Azure uygulama yapılandırma deposundaki anahtar değerleriyle çalışma](./scripts/cli-work-with-keys.md).

### <a name="deploy-vm-using-stored-key-values"></a>Depolanan anahtar değerlerini kullanarak VM 'yi dağıtma

Artık anahtar değerlerini depoya eklemişseniz, bir Azure Resource Manager şablonu kullanarak bir VM dağıtmaya hazırsınız demektir. Şablon, oluşturduğunuz **Windowsosversion** ve **disksizegb** anahtarlarına başvurur.

> [!WARNING]
> ARM şablonları, özel bağlantısı etkinleştirilmiş bir uygulama yapılandırma deposundaki anahtarlara başvuramaz.

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon, uygulama yapılandırma deposundaki depolanan anahtar değerlerini kullanarak bir sanal makine oluşturur.

    [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration%2Fazuredeploy.json)

1. Aşağıdaki değerleri seçin veya girin.

    - **abonelik**: sanal makineyi oluşturmak Için kullanılan Azure aboneliğini seçin.
    - **Kaynak grubu**: uygulama yapılandırma deposuyla aynı kaynak grubunu belirtin ya da yeni bir kaynak grubu oluşturmak Için **Yeni oluştur** ' u seçin.
    - **Bölge**: kaynak grubu için bir konum seçin.  Örneğin, **Doğu ABD**.
    - **Konum**: sanal makinenin konumunu belirtin. Varsayılan değeri kullanın.
    - **Yönetici Kullanıcı adı**: sanal makine için Yönetici Kullanıcı adı belirtin.
    - **Yönetici parolası**: sanal makine için bir yönetici parolası belirtin.
    - **Etki alanı adı etiketi**: benzersiz bir etki alanı adı belirtin.
    - **Depolama hesabı adı**: sanal makineyle ilişkili bir depolama hesabı için benzersiz bir ad belirtin.
    - **Uygulama yapılandırma deposu kaynak grubu**: uygulama yapılandırma deponuzi içeren kaynak grubunu belirtin.
    - **Uygulama yapılandırma deposu adı**: Azure uygulama yapılandırma deponuzın adını belirtin.
    - **VM SKU anahtarı**: **Windowsosversion**belirtin.  Bu, depoya eklediğiniz anahtar değer adıdır.
    - **Disk boyutu anahtarı**: **disksizegb**belirtin. Bu, depoya eklediğiniz anahtar değer adıdır.

1. **Gözden geçir + oluştur**’u seçin.
1. Sayfanın **doğrulama geçtiğini**gösterdiğini doğrulayın ve ardından **Oluştur**' u seçin.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

1. [Azure Portal](https://portal.azure.com)oturum açın ve ardından yeni oluşturulan sanal makineye gidin.
1. Sol menüden **genel bakış** ' ı seçin ve **SKU** 'nun **2019-Datacenter**olduğunu doğrulayın.
1. Sol menüden **diskler** ' i seçin ve veri diskinin boyutunun **2013**olduğunu doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, kaynak grubunu, uygulama yapılandırma deposunu, VM 'yi ve tüm ilgili kaynakları silin. Uygulama yapılandırma deposunu veya VM 'yi gelecekte kullanmayı planlıyorsanız, silme işlemini atlayabilirsiniz. Bu işi kullanmaya devam edemeyecekecekseniz, aşağıdaki cmdlet 'i çalıştırarak bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silin:

```azurepowershell-interactive
Remove-AzResourceGroup `
  -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Uygulama yapılandırmasından Azure Resource Manager şablonu ve anahtar değerlerini kullanarak bir VM dağıttınız.

Azure Uygulama yapılandırması ile başka uygulamalar oluşturma hakkında bilgi edinmek için aşağıdaki makaleye geçin:

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure Uygulama yapılandırmasıyla ASP.NET Core uygulaması oluşturma](quickstart-aspnet-core-app.md)
