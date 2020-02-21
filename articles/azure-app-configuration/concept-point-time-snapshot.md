---
title: Anahtar-değer çiftlerini bir noktadan zaman al
titleSuffix: Azure App Configuration
description: Azure Uygulama yapılandırmasındaki noktadan noktaya anlık görüntüleri kullanarak eski anahtar-değer çiftlerini al
services: azure-app-configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 1e2a4f7a7bc5db1b6a49f085821f7fa2bde54229
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77523671"
---
# <a name="point-in-time-snapshot"></a>Belirli bir noktanın anlık görüntüsü

Azure Uygulama yapılandırması, anahtar-değer çiftleri üzerinde yapılan değişikliklerin kaydını tutar. Bu kayıt, anahtar-değer değişikliklerinin zaman çizelgesini sağlar. Herhangi bir anahtar-değer geçmişini yeniden oluşturabilir ve önceki yedi gün içinde herhangi bir anda geçmiş değerini sağlayabilirsiniz. Bu özelliği kullanarak geri "zaman gezme" ve eski bir anahtar değeri alabilirsiniz. Örneğin, uygulamayı önceki yapılandırmaya geri almak için en son dağıtımdan önce kullanılan yapılandırma ayarlarını kurtarabilirsiniz.

## <a name="key-value-retrieval"></a>Anahtar-değer alma

Azure PowerShell, geçmiş anahtar değerlerini almak için kullanabilirsiniz.  Gerekli değerleri almak için uygun parametreler ekleyerek `az appconfig revision list`kullanın.  Mağaza adı (`--name {app-config-store-name}`) ya da bir bağlantı dizesi (`--connection-string {your-connection-string}`) sağlayarak Azure uygulama yapılandırma örneğini belirtin. Belirli bir zaman noktası (`--datetime`) belirterek ve döndürülecek en fazla öğe sayısını belirterek (`--top`) çıktıyı kısıtlayın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Anahtar değerinizdeki tüm kayıtlı değişiklikleri alın.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name}.
```

Anahtar `environment` ve Etiketler `test` ve `prod`için kaydedilen tüm değişiklikleri alın.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --key environment --label test,prod
```

`environment:prod`hiyerarşik anahtar alanındaki tüm kayıtlı değişiklikleri alın.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --key environment:prod:* 
```

Belirli bir noktada anahtar `color` kaydedilen tüm değişiklikleri alın.

```azurepowershell
az appconfig revision list --connection-string {your-app-config-connection-string} --key color --datetime "2019-05-01T11:24:12Z" 
```

Anahtar değerleriniz için kaydedilen son 10 değişikliği alın ve yalnızca `key`, `label`ve `last-modified` zaman damgası değerlerini döndürün.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --top 10 --fields key,label,last-modified
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [ASP.NET Core Web uygulaması oluşturma](./quickstart-aspnet-core-app.md)  
