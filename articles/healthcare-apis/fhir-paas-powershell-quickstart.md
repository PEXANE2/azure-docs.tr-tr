---
title: 'Hızlı başlangıç: PowerShell kullanarak FHıR için Azure API dağıtma'
description: Bu hızlı başlangıçta PowerShell kullanarak FHıR için Azure API 'yi dağıtmayı öğreneceksiniz.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 10/15/2019
ms.author: matjazl
ms.openlocfilehash: 4b2772b449b3c398c8c8932db58b7078b7501824
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87852030"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-powershell"></a>Hızlı başlangıç: PowerShell kullanarak FHıR için Azure API dağıtma

Bu hızlı başlangıçta PowerShell kullanarak FHıR için Azure API 'yi dağıtmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="register-the-azure-api-for-fhir-resource-provider"></a>FHıR kaynak sağlayıcısı için Azure API 'sini kaydetme

`Microsoft.HealthcareApis`Kaynak sağlayıcısı aboneliğiniz için zaten kayıtlı değilse, şunu kullanarak kaydedebilirsiniz:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.HealthcareApis
```

## <a name="create-azure-resource-group"></a>Azure Kaynak grubu oluştur

```azurepowershell-interactive
New-AzResourceGroup -Name "myResourceGroupName" -Location westus2
```

## <a name="deploy-azure-api-for-fhir"></a>FHIR için Azure API'sini dağıtma

```azurepowershell-interactive
New-AzHealthcareApisService -Name nameoffhirservice -ResourceGroupName myResourceGroupName -Location westus2 -Kind fhir-R4
```

> [!NOTE]
> Yüklediğiniz PowerShell modülünün sürümüne bağlı olarak `Az` , sağlanan fhır sunucusu [Yerel RBAC](configure-local-rbac.md) 'yi kullanacak şekilde yapılandırılabilir ve dağıtılmış fhır hizmeti için izin verilen kimlik nesnesi kimlikleri listesinde ayarlanmış olan PowerShell kullanıcısına şu anda oturum açmış durumda sahip olacak. Bundan sonra, veri düzlemi rolleri atamak için [Azure RBAC kullanmanızı](configure-azure-rbac.md) öneririz ve Azure RBAC modunu etkinleştirmek için dağıtımdan sonra bu kullanıcı nesne kimliğini silmeniz gerekebilir.


## <a name="fetch-capability-statement"></a>Fetch özelliği ekstresi

Fhır hesabı için Azure API 'sinin çalıştığını doğrulayabileceksiniz ve bir FHıR yetenek beyanı getirmeyi öğreneceksiniz:

```azurepowershell-interactive
$metadata = Invoke-WebRequest -Uri "https://nameoffhirservice.azurehealthcareapis.com/metadata"
$metadata.RawContent
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekecekseniz, kaynak grubunu aşağıdaki adımlarla silin:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç kılavuzunda, FHıR için Azure API 'sini aboneliğinize dağıttınız. FHıR için Azure API 'niz üzerinde ek ayarlar ayarlamak için, ek ayarlar nasıl yapılır Kılavuzu ' na ilerleyin.

>[!div class="nextstepaction"]
>[FHıR için Azure API 'de ek ayarlar](azure-api-for-fhir-additional-settings.md)
