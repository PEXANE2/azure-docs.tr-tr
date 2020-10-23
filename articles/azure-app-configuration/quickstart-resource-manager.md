---
title: Azure Resource Manager şablonu kullanarak bir Azure uygulama yapılandırma deposu oluşturma (ARM şablonu)
titleSuffix: Azure App Configuration
description: Azure Resource Manager şablonu (ARM şablonu) kullanarak bir Azure uygulama yapılandırma deposu oluşturmayı öğrenin.
author: ZhijunZhao
ms.author: zhijzhao
ms.date: 10/16/2020
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.openlocfilehash: feabac62564729338e41bf30eaf8d9f5a6317126
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92148997"
---
# <a name="quickstart-create-an-azure-app-configuration-store-by-using-an-arm-template"></a>Hızlı başlangıç: ARM şablonu kullanarak Azure uygulama yapılandırma deposu oluşturma

Bu hızlı başlangıçta nasıl yapılacağı açıklanmaktadır:

- Azure Resource Manager şablonu kullanarak bir uygulama yapılandırma deposu dağıtın (ARM şablonu).
- ARM şablonunu kullanarak bir uygulama yapılandırma deposunda anahtar değerleri oluşturma.
- ARM şablonundan uygulama yapılandırma deposundaki anahtar değerlerini okuyun.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration-store-kv%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/101-app-configuration-store-kv/) alınmıştır. İçinde iki anahtar değeri olan yeni bir uygulama yapılandırma deposu oluşturur. Ardından, `reference` iki anahtar-değer kaynağı değerlerini çıkarmak için işlevini kullanır. Anahtarın değerini bu şekilde okumak, şablonun şablondaki diğer yerlerde kullanılmasına izin verir.

Hızlı başlangıç, `copy` anahtar-değer kaynağının birden çok örneğini oluşturmak için öğesini kullanır. Öğesi hakkında daha fazla bilgi edinmek için `copy` bkz. [ARM şablonlarındaki kaynak yinelemesi](../azure-resource-manager/templates/copy-resources.md).

> [!IMPORTANT]
> Bu şablon, uygulama yapılandırma kaynak sağlayıcısı sürümü `2020-07-01-preview` veya üzerini gerektirir. Bu sürüm, `reference` anahtar değerlerini okumak için işlevini kullanır. `listKeyValue`Önceki sürümdeki anahtar değerlerini okumak için kullanılan işlev sürümden itibaren kullanılamıyor `2020-07-01-preview` .

:::code language="json" source="~/quickstart-templates/101-app-configuration-store-kv/azuredeploy.json":::

Şablonda iki Azure kaynağı tanımlanmıştır:

- [Microsoft. AppConfiguration/Configurationmağazaların](/azure/templates/microsoft.appconfiguration/2020-06-01/configurationstores): bir uygulama yapılandırma deposu oluşturun.
- Microsoft. AppConfiguration/Configurationmağazaları/keyValues: uygulama yapılandırma deposu içinde bir anahtar değeri oluşturun.

> [!NOTE]
> `keyValues`Kaynağın adı, anahtar ve etiketin bir birleşimidir. Anahtar ve etiket `$` sınırlayıcıyla birleştirilir. Etiket isteğe bağlıdır. Yukarıdaki örnekte, `keyValues` adı olan kaynak, `myKey` etiket olmadan bir anahtar değeri oluşturur.
>
> URL kodlaması olarak da bilinen yüzde Encoding, anahtar veya etiketlerin ARM şablon kaynak adlarında izin verilmeyen karakterler içermesini sağlar. `%` , izin verilen bir karakter değil, `~` onun yerinde kullanılır. Bir adı doğru bir şekilde kodlamak için aşağıdaki adımları izleyin:
>
> 1. URL kodlaması Uygula
> 2. Değiştir `~``~7E`
> 3. Değiştir `%``~`
>
> Örneğin, anahtar adı ve etiket adı ile bir anahtar-değer çifti oluşturmak için `AppName:DbEndpoint` `Test` , kaynak adı olmalıdır `AppName~3ADbEndpoint$Test` .

## <a name="deploy-the-template"></a>Şablonu dağıtma

Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon, içinde iki anahtar değeri olan bir uygulama yapılandırma deposu oluşturur.

[![Azure’a dağıtın](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-app-configuration-store-kv%2Fazuredeploy.json)

Ayrıca, aşağıdaki PowerShell cmdlet 'ini kullanarak şablonu dağıtabilirsiniz. Anahtar değerleri PowerShell konsolunun çıkışında yer alacak.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-app-configuration-store-kv/azuredeploy.json"

$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri

Read-Host -Prompt "Press [ENTER] to continue ..."
```

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Azure portal arama kutusuna **uygulama yapılandırması**yazın. Listeden **uygulama yapılandırması** ' nı seçin.
1. Yeni oluşturulan uygulama yapılandırma kaynağını seçin.
1. **İşlemler**altında **yapılandırma Gezgini**' ne tıklayın.
1. İki anahtar değerinin mevcut olduğunu doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, uygulama yapılandırma deposunu ve tüm ilgili kaynakları silin. Uygulama yapılandırma deposunu gelecekte kullanmayı planlıyorsanız, silme işlemini atlayabilirsiniz. Bu depoyu kullanmaya devam edemeyecekecekseniz, aşağıdaki cmdlet 'i çalıştırarak bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silin:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Uygulama yapılandırması ile başka uygulamalar oluşturma hakkında bilgi edinmek için aşağıdaki makaleye geçin:

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure Uygulama yapılandırmasıyla ASP.NET Core uygulaması oluşturma](quickstart-aspnet-core-app.md)
