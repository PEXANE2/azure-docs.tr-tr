---
author: craigktreasure
ms.service: azure-object-anchors
ms.topic: include
ms.date: 02/18/2021
ms.author: crtreasu
ms.openlocfilehash: ada83d6263ef033208200d810c53c5f045acc9a7
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105104473"
---
Sonraki adım, Unity için Azure nesne bağlantıları paketini indirmenin bir sonraki adımıdır.

# <a name="download-with-web-browser"></a>[Web tarayıcısı ile indir](#tab/unity-package-web-ui)

Unity için Azure nesne bağlantıları paketini [buradan](https://aka.ms/aoa/unity-sdk/package)bulun. İstediğiniz sürümü seçin ve **İndir** düğmesini kullanarak paketi indirin.

# <a name="download-with-npm"></a>[NPM ile indir](#tab/unity-package-npm)

Bu adım <a href="https://www.npmjs.com/get-npm" target="_blank">NPM</a> 'nin yüklü ve kullanılabilir olmasını gerektirir.

Aşağıdaki komutu çalıştırarak, `<version_number>` indirmek Istediğiniz Azure nesne çıpası sürümü ile değiştirin:

```bash
npm pack com.microsoft.azure.object-anchors.runtime@<version_number> --registry https://pkgs.dev.azure.com/aipmr/MixedReality-Unity-Packages/_packaging/Unity-packages/npm/registry/
```

> [!NOTE]
> Azure nesne bağlantıları paketinin kullanılabilir sürümlerini listelemek için aşağıdakileri çalıştırın:
>
> ```bash
> npm view com.microsoft.azure.object-anchors.runtime --registry https://pkgs.dev.azure.com/aipmr/MixedReality-Unity-Packages/_packaging/Unity-packages/npm/registry/ versions
> ```

Azure nesne bağlantıları paketi, komutu çalıştırdığınız klasöre indirilir.

# <a name="install-with-mixed-reality-feature-tool-beta"></a>[Karma Gerçeklik Özellik Aracı (Beta) ile Install](#tab/unity-package-mixed-reality-feature-tool)

Sonraki adımla devam edin. Daha sonraki bir adımda <a a href="/windows/mixed-reality/develop/unity/welcome-to-mr-feature-tool" target="_blank">karma gerçeklik özelliği aracını</a> kullanacaksınız.

---