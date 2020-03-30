---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/12/2020
ms.author: mimart
ms.openlocfilehash: d43b879057001d62ea72bd2e011ad52957d47470
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78189046"
---
## <a name="sample-templates"></a>Örnek şablonlar
Kullanıcı Arabirimi özelleştirmesi için örnek şablonları burada bulabilirsiniz:

```bash
git clone https://github.com/Azure-Samples/Azure-AD-B2C-page-templates
```

Bu proje aşağıdaki şablonları içerir:
- [Okyanus Mavisi](https://github.com/Azure-Samples/Azure-AD-B2C-page-templates/tree/master/ocean_blue)
- [Kayrak Gri](https://github.com/Azure-Samples/Azure-AD-B2C-page-templates/tree/master/slate_gray)

Örneği kullanmak için:

1. Yerel makinenizdeki repoyu klonla. Bir şablon `/ocean_blue` klasörü seçin veya `/slate_gray`.
1. Şablon klasörü ve `/assets` klasöraltındaki tüm dosyaları önceki bölümlerde açıklandığı şekilde Blob depolama alanına yükleyin.
1. Ardından, her `\*.html` dosyayı kökünde `/ocean_blue` `/slate_gray`açın veya, tüm göreli URL örneklerini adım 2'ye yüklediğiniz css, resim ve yazı tipi dosyalarının URL'leri ile değiştirin. Örnek:
    ```html
    <link href="./css/assets.css" rel="stylesheet" type="text/css" />
    ```

    Alıcı
    ```html
    <link href="https://your-storage-account.blob.core.windows.net/your-container/css/assets.css" rel="stylesheet" type="text/css" />
    ```
1. Dosyaları `\*.html` kaydedin ve Blob depolama alanına yükleyin.
1. Şimdi, daha önce de belirtildiği gibi HTML dosyanızı işaret ederek ilkeyi değiştirin.
1. Eksik yazı tipleri, resimler veya CSS görürseniz, uzantılar ilkesinde ve \*.html dosyalarında başvurularınızı kontrol edin.
