---
author: baanders
description: Azure dijital TWINS örneğine model yüklemek için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 3/23/2021
ms.author: baanders
ms.openlocfilehash: 987409f070f34f0fd9ee212fab8cc57e889e0146
ms.sourcegitcommit: ac035293291c3d2962cee270b33fca3628432fac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "104950607"
---
Model şuna benzer:
:::code language="json" source="~/digital-twins-docs-samples/models/Thermostat.json":::

**Bu modeli TWINS örneğinizi karşıya yüklemek** Için AŞAĞıDAKI Azure CLI komutunu çalıştırın, bu, yukarıdaki modeli satır içi JSON olarak yükler. Komutunu tarayıcınızda [Azure Cloud Shell](../articles/cloud-shell/overview.md) veya CLI 'niz [yerel olarak yüklüyse](/cli/azure/install-azure-cli)makinenizde çalıştırabilirsiniz.

```azurecli-interactive
az dt model create --models '{  "@id": "dtmi:contosocom:DigitalTwins:Thermostat;1",  "@type": "Interface",  "@context": "dtmi:dtdl:context;2",  "contents": [    {      "@type": "Property",      "name": "Temperature",      "schema": "double"    }  ]}' -n {digital_twins_instance_name}
```

> [!Note]
> PowerShell ortamında Cloud Shell kullanıyorsanız, değerlerinin doğru ayrıştırılabilmesi için satır içi JSON alanlarındaki tırnak işareti karakterlerini atlamanız gerekebilir. Bu değişiklik ile modeli karşıya yükleme komutu aşağıda verilmiştir:
>
> Modeli karşıya yükle:
> ```azurecli-interactive
> az dt model create --models '{  \"@id\": \"dtmi:contosocom:DigitalTwins:Thermostat;1\",  \"@type\": \"Interface\",  \"@context\": \"dtmi:dtdl:context;2\",  \"contents\": [    {      \"@type\": \"Property\",      \"name\": \"Temperature\",      \"schema\": \"double\"    }  ]}' -n {digital_twins_instance_name}
> ```