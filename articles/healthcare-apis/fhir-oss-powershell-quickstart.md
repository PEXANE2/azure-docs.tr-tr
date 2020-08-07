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
ms.openlocfilehash: c133a309cc85ffcfb69be2ae7bbb614cbb540f2e
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87847151"
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

Azure [GitHub deposu](https://github.com/Microsoft/fhir-server) için Microsoft fhır sunucusu, tüm gerekli kaynakları dağıtan bir şablon içerir. İle dağıtın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Microsoft/fhir-server/master/samples/templates/default-azuredeploy.json -ResourceGroupName $rg.ResourceGroupName -serviceName $fhirServiceName
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
