---
title: Anahtar değer çiftlerini zamanında alma
titleSuffix: Azure App Configuration
description: Azure Uygulama Yapılandırması'nda anlık nokta görüntülerini kullanarak eski anahtar değer çiftlerini alın
services: azure-app-configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 1e2a4f7a7bc5db1b6a49f085821f7fa2bde54229
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77523671"
---
# <a name="point-in-time-snapshot"></a>Belirli bir noktanın anlık görüntüsü

Azure Uygulama Yapılandırması, anahtar değer çiftlerinde yapılan değişikliklerin kaydını tutar. Bu kayıt, anahtar değeri değişikliklerinin bir zaman çizelgesi sağlar. Herhangi bir anahtar değerin geçmişini yeniden oluşturabilirsiniz ve önceki yedi gün içinde herhangi bir anda geçmiş değerini sağlayabilirsiniz. Bu özelliği kullanarak, geriye doğru "zaman yolculuğu" yapabilir ve eski bir anahtar değerini alabilirsiniz. Örneğin, uygulamayı önceki yapılandırmaya geri almak için en son dağıtımdan önce kullanılan yapılandırma ayarlarını kurtarabilirsiniz.

## <a name="key-value-retrieval"></a>Anahtar değeri alma

Geçmiş anahtar değerlerini almak için Azure PowerShell'i kullanabilirsiniz.  Gerekli `az appconfig revision list`değerleri almak için uygun parametrelerekleyerek kullanın.  Mağaza adını ( )`--name {app-config-store-name}`sağlayarak veya bağlantı dizesini kullanarak`--connection-string {your-connection-string}`Azure Uygulama Yapılandırmaörneğini belirtin. Zaman içinde belirli bir nokta ()`--datetime`belirterek ve döndürülecek maksimum madde`--top`sayısını belirterek çıktıyı kısıtlayın ( ).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Anahtar değerlerinizde kaydedilen tüm değişiklikleri alın.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name}.
```

Anahtar `environment` ve etiketler `test` için kaydedilen tüm `prod`değişiklikleri ve .

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --key environment --label test,prod
```

Hiyerarşik anahtar alanında `environment:prod`kaydedilen tüm değişiklikleri alın.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --key environment:prod:* 
```

Belirli bir noktada anahtar `color` için kaydedilen tüm değişiklikleri alın.

```azurepowershell
az appconfig revision list --connection-string {your-app-config-connection-string} --key color --datetime "2019-05-01T11:24:12Z" 
```

Anahtar değerleriniz için kaydedilen son 10 değişikliği alın ve `key` `label`yalnızca `last-modified` , ve zaman damgası değerlerini döndürün.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --top 10 --fields key,label,last-modified
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [ASP.NET Core web uygulaması oluşturma](./quickstart-aspnet-core-app.md)  
