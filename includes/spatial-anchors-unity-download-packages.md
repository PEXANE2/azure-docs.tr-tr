---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 2/3/21
ms.author: parkerra
ms.openlocfilehash: f6c2780ccbb914228a9870cb3b5fe4b0e3d0b214
ms.sourcegitcommit: f82e290076298b25a85e979a101753f9f16b720c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99569606"
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

Azure uzamsal bağlayıcı çekirdek paketi, komutu çalıştırdığınız klasöre indirilir.

Projenizde desteklemek istediğiniz her platform (Android/iOS/HoloLens) paketini indirmek için bu adımı tekrarlayın.

| Platform | Paket adı                                    |
|----------|-------------------------------------------------|
| Android  | com. Microsoft. Azure. uzamsal-Tutturucuların sdk.android@ <version_number> |
| iOS      | com. Microsoft. Azure. uzamsal-Tutturucuların sdk.ios@ <version_number>     |
| HoloLens | com. Microsoft. Azure. uzamsal-Tutturucuların sdk.windows@ <version_number> |