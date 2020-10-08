---
title: 'PowerShell: Azure için FHıR sunucusunu dağıtma – FHıR için Azure API'
description: Bu hızlı başlangıçta, PowerShell kullanılarak Microsoft açık kaynak FHıR sunucusunun nasıl dağıtılacağı açıklanmaktadır.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: c6c1a7e21f0a1554c67c7f1860a2bd3382c941f5
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91817964"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-powershell"></a>Hızlı başlangıç: PowerShell kullanarak açık kaynaklı FHıR sunucusunu dağıtma

Bu hızlı başlangıçta, PowerShell kullanarak Azure için açık kaynaklı Microsoft FHıR sunucusunu dağıtmayı öğrenin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu için sağlanan kaynakları içerecek bir ad seçin ve oluşturun:

```azurepowershell-interactive
$fhirServiceName = "MyFhirService"
$rg = New-AzResourceGroup -Name $fhirServiceName -Location westus2
```

## <a name="deploy-the-fhir-server-template"></a>FHıR sunucu şablonunu dağıtma

Azure [GitHub deposu](https://github.com/Microsoft/fhir-server) için Microsoft fhır sunucusu, tüm gerekli kaynakları dağıtan bir şablon içerir. Gerekli Azure kaynakları oluşturulup yapılandırıldığı için dağıtım işlemi birkaç dakika sürer. İle dağıtın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Microsoft/fhir-server/master/samples/templates/default-azuredeploy.json -ResourceGroupName $rg.ResourceGroupName -serviceName $fhirServiceName
```

> [!NOTE]
> Henüz oturum açmadıysanız, önce aşağıdaki komutları çalıştırın.

```azurepowershell-interactive
Connect-AzAccount
get-azsubscription
Set-AzContext -SubscriptionId yoursubscriptionid
```

Mevcut bir kaynak grubunu kullanmak için, kodda gösterildiği gibi, $rg değişken tanımı satırı ve Azure ARM şablonu dağıtım komut satırı $rg değerlerini değiştirin.

```azurepowershell-interactive
$fhirServiceName = "MyFhirService"
$rg = "MyExistingResourceGroupName"
New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Microsoft/fhir-server/master/samples/templates/default-azuredeploy.json -ResourceGroupName $rg -serviceName $fhirServiceName
```

## <a name="verify-fhir-server-is-running"></a>FHıR sunucusunun çalıştığını doğrulama

```azurepowershell-interactive
$metadataUrl = "https://" + $fhirServiceName + ".azurewebsites.net/metadata" 
$metadata = Invoke-WebRequest -Uri $metadataUrl
$metadata.RawContent
```

Sunucunun ilk kez yanıt vermesi birkaç dakika sürer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekecekseniz, kaynak grubunu aşağıdaki adımlarla silin:

```azurepowershell-interactive
Remove-AzResourceGroup -Name $rg.ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure için Microsoft açık kaynak FHıR sunucusunu aboneliğinize dağıttınız. Postman kullanarak FHıR API 'sine nasıl erişebileceğinizi öğrenmek için Postman öğreticisine geçin.
 
>[!div class="nextstepaction"]
>[Postman kullanarak FHıR API 'sine erişme](access-fhir-postman-tutorial.md)
