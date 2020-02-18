---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/12/2020
ms.author: marsma
ms.openlocfilehash: 264604bfdf0c514e6464854f431addbc9d2dcdef
ms.sourcegitcommit: ef568f562fbb05b4bd023fe2454f9da931adf39a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2020
ms.locfileid: "77373139"
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

1. Yerel makinenizde depoyu kopyalayın. `/ocean_blue` veya `/slate_gray`bir şablon klasörü seçin.
1. Şablon klasörü ve `/assets` klasörü altındaki tüm dosyaları önceki bölümlerde açıklandığı gibi BLOB depolama alanına yükleyin.
1. Sonra, `/ocean_blue` veya `/slate_gray`kökündeki her bir `\*.html` dosyasını açın, göreli URL 'lerin tüm örneklerini adım 2 ' de karşıya yüklediğiniz CSS, resim ve yazı tipi dosyalarının URL 'Leriyle değiştirin. Örnek:
    ```html
    <link href="./css/assets.css" rel="stylesheet" type="text/css" />
    ```

    Alıcı 
    ```html
    <link href="https://your-storage-account.blob.core.windows.net/your-container/css/assets.css" rel="stylesheet" type="text/css" />
    ```
1. `\*.html` dosyalarını kaydedin ve BLOB depolamaya yükleyin.
1. Şimdi, daha önce belirtildiği gibi, HTML dosyanıza işaret eden ilkeyi değiştirin.
1. Eksik yazı tiplerini, görüntüleri veya CSS 'yi görürseniz, uzantılar ilkesinde veya \*. html dosyalarında başvurularınızı kontrol edin.
