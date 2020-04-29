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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77523671"
---
# <a name="point-in-time-snapshot"></a>Belirli bir noktanın anlık görüntüsü

Azure Uygulama yapılandırması, anahtar-değer çiftleri üzerinde yapılan değişikliklerin kaydını tutar. Bu kayıt, anahtar-değer değişikliklerinin zaman çizelgesini sağlar. Herhangi bir anahtar-değer geçmişini yeniden oluşturabilir ve önceki yedi gün içinde herhangi bir anda geçmiş değerini sağlayabilirsiniz. Bu özelliği kullanarak geri "zaman gezme" ve eski bir anahtar değeri alabilirsiniz. Örneğin, uygulamayı önceki yapılandırmaya geri almak için en son dağıtımdan önce kullanılan yapılandırma ayarlarını kurtarabilirsiniz.

## <a name="key-value-retrieval"></a>Anahtar-değer alma

Azure PowerShell, geçmiş anahtar değerlerini almak için kullanabilirsiniz.  Gerekli `az appconfig revision list`değerleri almak için uygun parametreler ekleyerek kullanın.  Mağaza adını (`--name {app-config-store-name}`) ya da bir bağlantı dizesini (`--connection-string {your-connection-string}`) kullanarak Azure uygulama yapılandırma örneğini belirtin. Zaman içinde belirli bir nokta (`--datetime`) belirterek ve döndürülecek en fazla öğe sayısını (`--top`) belirterek çıktıyı kısıtlayın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Anahtar değerinizdeki tüm kayıtlı değişiklikleri alın.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name}.
```

Anahtar `environment` ve Etiketler `test` için kaydedilen tüm değişiklikleri alın. `prod`

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --key environment --label test,prod
```

Hiyerarşik anahtar alanındaki tüm kayıtlı değişiklikleri alın `environment:prod`.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --key environment:prod:* 
```

Belirli bir noktadaki anahtar `color` için kaydedilen tüm değişiklikleri alın.

```azurepowershell
az appconfig revision list --connection-string {your-app-config-connection-string} --key color --datetime "2019-05-01T11:24:12Z" 
```

Anahtar değerleriniz için kaydedilen son 10 değişikliği alın ve yalnızca `key`, `label`, ve `last-modified` zaman damgası değerlerini döndürün.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --top 10 --fields key,label,last-modified
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [ASP.NET Core web uygulaması oluşturma](./quickstart-aspnet-core-app.md)  
