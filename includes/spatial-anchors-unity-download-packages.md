---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 2/3/21
ms.author: parkerra
ms.openlocfilehash: c97067c66296e8980a36b21298a7b2061e0f9b4c
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99550413"
---
Şu komutu çalıştırarak, `<version_number>` geçerli klasöre indirmek Istediğiniz Azure uzamsal çıpası sürümü ile değiştirin:

```bash
npm pack com.microsoft.azure.spatial-anchors-sdk.core@<version_number> --registry https://api.bintray.com/npm/microsoft/AzureMixedReality-NPM
```

> [!NOTE]
> Azure uzamsal bağlayıcı paketinin kullanılabilir sürümlerini listelemek için aşağıdakileri çalıştırın:
>
> ```bash
> npm view com.microsoft.azure.spatial-anchors-sdk.core --registry https://api.bintray.com/npm/microsoft/AzureMixedReality-NPM versions
> ```

Azure uzamsal bağlayıcı çekirdek paketi, komutu çalıştırdığınız klasöre indirilir.

Projenizde desteklemek istediğiniz her platform (Android/iOS/HoloLens) paketini indirmek için bu adımı tekrarlayın.

| Platform | Paket adı                                    |
|----------|-------------------------------------------------|
| Android  | com. Microsoft. Azure. uzamsal-Tutturucuların sdk.android@ <version_number> |
| iOS      | com. Microsoft. Azure. uzamsal-Tutturucuların sdk.ios@ <version_number>     |
| HoloLens | com. Microsoft. Azure. uzamsal-Tutturucuların sdk.windows@ <version_number> |