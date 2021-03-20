---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 6/2/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a0a9bc29c3e20a025fb2c46a71c2f134c37bee04
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "84465111"
---
Aşağıdaki PowerShell komutu, depolama hesabının genel uç noktasındaki tüm trafiği reddeder. Bu komutun `-Bypass` parametresi olarak ayarlandığını unutmayın `AzureServices` . Bu, Azure Dosya Eşitleme gibi güvenilen ilk taraf hizmetlerin, genel uç nokta aracılığıyla depolama hesabına erişmesi için izin verir.

```PowerShell
# This assumes $storageAccount is still defined from the beginning of this of this guide.
$storageAccount | Update-AzStorageAccountNetworkRuleSet `
        -DefaultAction Deny `
        -Bypass AzureServices `
        -WarningAction SilentlyContinue `
        -ErrorAction Stop | `
    Out-Null
```