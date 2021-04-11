---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 03/18/2021
ms.author: parkerra
ms.openlocfilehash: d91c0aeda2b7ae2f133d2099cbc9d238fd19d287
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104719800"
---
Gerekli paketleri indirmek için <a href="https://www.npmjs.com/get-npm" target="_blank">NPM</a> 'nin yüklü olması gerekir.

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

> [!WARNING]
> ASA SDK 2.7.0 desteklenen en düşük sürümdür. Unity 2020 kullanıyorsanız, ASA SDK 2.9.0 desteklenen en düşük sürümdür.

Azure uzamsal bağlayıcı çekirdek paketi, komutu çalıştırdığınız klasöre indirilir.

Projenizde desteklemek istediğiniz her platform (Android/iOS/HoloLens) paketini indirmek için bu adımı tekrarlayın.

| Platform | Paket adı                                    |
|----------|-------------------------------------------------|
| Android  | com. Microsoft. Azure. uzamsal-Tutturucuların sdk.android@ <version_number> |
| iOS      | com. Microsoft. Azure. uzamsal-Tutturucuların sdk.ios@ <version_number>     |
| HoloLens | com. Microsoft. Azure. uzamsal-Tutturucuların sdk.windows@ <version_number> |