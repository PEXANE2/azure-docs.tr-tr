---
title: Jupyıter not defterlerine paket yükler-Azure Notebooks önizlemesi
description: 'Azure üzerinde çalışan bir Jupyter Not Defteri içinden Python, R ve F # paketleri yüklemeyi öğrenin.'
ms.topic: how-to
ms.date: 12/04/2018
ms.openlocfilehash: f6359b4c010834ffaee00b6208d309997339f36e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75646237"
---
# <a name="install-packages-from-within-azure-notebooks-preview"></a>Azure Notebooks Preview içinden paket yüklemesi

[Not defteriniz için projeyi proje düzeyinde](configure-manage-azure-notebooks-projects.md#configure-the-project-environment)yapılandırabilseniz de, paketleri doğrudan ayrı bir not defteri içinde yüklemek isteyebilirsiniz.

Not defterinden yüklenen paketler yalnızca geçerli sunucu oturumu için geçerlidir. Sunucu kapatıldıktan sonra paket yüklemeleri kalıcı olmaz.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="python"></a>Python

Python 'daki paketler, kod hücrelerinin içindeki komutlar kullanılarak PIP veya Conda ile yüklenebilir:

```bash
!pip install <package_name>

!conda install <package_name> -y
```

Komut çıktısı, gereksinimin zaten karşılandığını gösteriyorsa, Azure Notebooks paketi varsayılan olarak içerebilir. Paket ayrıca bir [Proje ortamı Kurulum adımı](configure-manage-azure-notebooks-projects.md#configure-the-project-environment)ile yüklenebilir.

## <a name="r"></a>R

R 'deki paketler, `install.packages` bir kod hücresindeki işlevi kullanılarak Cran veya GitHub 'dan yüklenebilir:

```r
install.packages("package_name")
```

Ayrıca, devtools kitaplığını kullanarak GitHub 'dan ön sürüm sürümlerini ve diğer geliştirme paketlerini de yükleyebilirsiniz:

```r
options(unzip = 'internal')
library(devtools)
install_github('<user>/<repo>')
```

## <a name="f"></a>F#

F # içindeki paketler, kod hücrelerinin içinden paket bağımlılığı Yöneticisi çağırarak, [NuGet.org](https://www.nuget.org) adresinden yüklenebilir. İlk olarak, paket yöneticisini yükleyin:

```fsharp
#load "Paket.fsx"
```

Ardından paketleri yükler:

```fsharp
Paket.Package
  [ "MathNet.Numerics"
    "MathNet.Numerics.FSharp"
  ]
```

Ardından, paket oluşturucuyu yükleyin:
```fsharp
#load "Paket.Generated.Refs.fsx"
```

Kitaplığı açın:
```fsharp
open MathNet.Numerics
```

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl yapılır: projeleri yapılandırma ve yönetme](configure-manage-azure-notebooks-projects.md)
- [Nasıl yapılır: bir slayt gösterisi sunma](present-jupyter-notebooks-slideshow.md)
