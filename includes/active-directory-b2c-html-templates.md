---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/12/2020
ms.author: mimart
ms.openlocfilehash: d43b879057001d62ea72bd2e011ad52957d47470
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78189046"
---
## <a name="sample-templates"></a>Örnek şablonlar
UI özelleştirmesi için örnek şablonları buradan bulabilirsiniz:

```bash
git clone https://github.com/Azure-Samples/Azure-AD-B2C-page-templates
```

Bu proje aşağıdaki şablonları içerir:
- [Okyanus Mavisi](https://github.com/Azure-Samples/Azure-AD-B2C-page-templates/tree/master/ocean_blue)
- [Kurşun Grisi](https://github.com/Azure-Samples/Azure-AD-B2C-page-templates/tree/master/slate_gray)

Örneği kullanmak için:

1. Yerel makinenizde depoyu kopyalayın. Bir şablon klasörü `/ocean_blue` veya seçin `/slate_gray` .
1. Şablon klasörü ve klasörü altındaki tüm dosyaları `/assets` , önceki bölümlerde açıklandığı gibi BLOB depolama alanına yükleyin.
1. Ardından, her `\*.html` bir dosyayı veya kökündeki her birini açın `/ocean_blue` `/slate_gray` , göreli URL 'lerin tüm örneklerini adım 2 ' de karşıya yüklediğiniz CSS, resim ve yazı tipi dosyalarının URL 'leriyle değiştirin. Örneğin:
    ```html
    <link href="./css/assets.css" rel="stylesheet" type="text/css" />
    ```

    Alıcı
    ```html
    <link href="https://your-storage-account.blob.core.windows.net/your-container/css/assets.css" rel="stylesheet" type="text/css" />
    ```
1. Dosyaları kaydedin `\*.html` ve BLOB depolamaya yükleyin.
1. Şimdi, daha önce belirtildiği gibi, HTML dosyanıza işaret eden ilkeyi değiştirin.
1. Eksik yazı tiplerini, görüntüleri veya CSS 'yi görürseniz, uzantılar ilkesinde ve. html dosyalarında başvurularınızı kontrol edin \* .
