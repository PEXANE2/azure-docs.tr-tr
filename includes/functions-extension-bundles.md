---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/27/2019
ms.author: glenga
ms.openlocfilehash: d697334fe56fb9133a06cee79067c60bc3a37281
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68639156"
---
Bağlama uzantılarını yüklemenin en kolay yolu [uzantı demetlerini](../articles/azure-functions/functions-bindings-register.md#extension-bundles)etkinleştirmektir. Paketleri etkinleştirdiğinizde, önceden tanımlanmış bir uzatma paketi kümesi otomatik olarak yüklenir.

Uzantı demetlerini etkinleştirmek için host.json dosyasını açın ve içeriğini aşağıdaki kodla eşleşecek şekilde güncelleştirin:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```