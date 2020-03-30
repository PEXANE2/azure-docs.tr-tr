---
title: Jupyter dizüstü bilgisayarlara paket yükleme - Azure Dizüstü Bilgisayarlar Önizleme
description: Python, R ve F# paketlerini Azure'da çalışan bir Jupyter dizüstü bilgisayarıniçinden nasıl yükleyin içini öğrenin.
ms.topic: how-to
ms.date: 12/04/2018
ms.openlocfilehash: f6359b4c010834ffaee00b6208d309997339f36e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75646237"
---
# <a name="install-packages-from-within-azure-notebooks-preview"></a>Paketleri Azure Notebook Önizleme'nin içinden yükleme

Proje düzeyinde not [defterinizin ortamını](configure-manage-azure-notebooks-projects.md#configure-the-project-environment)yapılandırabiliyor olsanız da, paketleri doğrudan tek bir not defterine yüklemek isteyebilirsiniz.

Not defterinden yüklenen paketler yalnızca geçerli sunucu oturumuna uygulanır. Sunucu kapatıldıktan sonra paket yüklemeleri kalıcı değildir.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="python"></a>Python

Python'daki paketler, kod hücreleri içindeki komutlar kullanılarak pip veya conda kullanılarak yüklenebilir:

```bash
!pip install <package_name>

!conda install <package_name> -y
```

Komut çıktısı gereksinimin zaten karşılandığını gösteriyorsa, Azure Not Defterleri varsayılan olarak paketi içerebilir. Paket, proje ortamı kurulum [adımı](configure-manage-azure-notebooks-projects.md#configure-the-project-environment)yla da yüklenebilir.

## <a name="r"></a>R

R'deki paketler, bir kod hücresindeki `install.packages` işlevi kullanarak CRAN veya GitHub'dan yüklenebilir:

```r
install.packages("package_name")
```

Ayrıca, devtools kitaplığını kullanarak GitHub'dan yayın öncesi sürüm sürümlerini ve diğer geliştirme paketlerini de yükleyebilirsiniz:

```r
options(unzip = 'internal')
library(devtools)
install_github('<user>/<repo>')
```

## <a name="f"></a>F#

F# [paketleri,](https://www.nuget.org) kod hücrelerinin içinden Paket bağımlılık yöneticisini arayarak nuget.org yüklenebilir. İlk olarak Paket yöneticisini yükleyin:

```fsharp
#load "Paket.fsx"
```

Sonra paketleri yükleyin:

```fsharp
Paket.Package
  [ "MathNet.Numerics"
    "MathNet.Numerics.FSharp"
  ]
```

Daha sonra Paket jeneratörü yükleyin:
```fsharp
#load "Paket.Generated.Refs.fsx"
```

Kitaplığı açın:
```fsharp
open MathNet.Numerics
```

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl yapilir: Projeleri yapılandırma ve yönetme](configure-manage-azure-notebooks-projects.md)
- [Nasıl yapılsın: Slayt gösterisi ni sunma](present-jupyter-notebooks-slideshow.md)
