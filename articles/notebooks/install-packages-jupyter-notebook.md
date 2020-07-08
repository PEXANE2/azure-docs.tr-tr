---
title: Jupyıter not defterlerine paket yükler-Azure Notebooks önizlemesi
description: 'Azure üzerinde çalışan bir Jupyter Not Defteri içinden Python, R ve F # paketleri yüklemeyi öğrenin.'
ms.topic: how-to
ms.date: 12/04/2018
ms.openlocfilehash: 43d38c9f3a4b0095967ab3e103ea729ec86ea2bd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85831225"
---
# <a name="install-packages-from-within-azure-notebooks-preview"></a>Azure Notebooks Preview içinden paket yüklemesi

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

[Not defteriniz için projeyi proje düzeyinde](configure-manage-azure-notebooks-projects.md#configure-the-project-environment)yapılandırabilseniz de, paketleri doğrudan ayrı bir not defteri içinde yüklemek isteyebilirsiniz.

Not defterinden yüklenen paketler yalnızca geçerli sunucu oturumu için geçerlidir. Sunucu kapatıldıktan sonra paket yüklemeleri kalıcı olmaz.

## <a name="python"></a>Python

Python 'daki paketler, kod hücrelerinin içindeki komutlar kullanılarak PIP veya Conda ile yüklenebilir:

```bash
!pip install <package_name>

!conda install <package_name> -y
```

Komut çıktısı, gereksinimin zaten karşılandığını gösteriyorsa, Azure Notebooks paketi varsayılan olarak içerebilir. Paket ayrıca bir [Proje ortamı Kurulum adımı](configure-manage-azure-notebooks-projects.md#configure-the-project-environment)ile yüklenebilir.

## <a name="r"></a>R

R 'deki paketler, `install.packages` bir kod hücresindeki işlevi kullanılarak CRAN veya GitHub 'dan yüklenebilir:

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
