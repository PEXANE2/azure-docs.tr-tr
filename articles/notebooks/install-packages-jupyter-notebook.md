---
title: Azure üzerinde Jupyter notebook paketleri yükleme
description: Python, R, yükleme ve F# paketleri gelen içinde Azure üzerinde çalışan bir Jupyter not defteri.
ms.topic: article
ms.date: 12/04/2018
ms.openlocfilehash: 5d85c8e936ce7c8bf38ec7bc9c27d9066cc8b155
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277544"
---
# <a name="install-packages-from-within-a-notebook"></a>İçinde bir not defteri paketleri yükleme

Yapılandırmanız mümkün olmakla birlikte [ortamı için dizüstü bilgisayarınızda proje düzeyinde](configure-manage-azure-notebooks-projects.md#configure-the-project-environment), doğrudan tek bir not defteri içindeki paketleri yüklemek isteyebilirsiniz.

Not defterinden yüklü paketleri yalnızca geçerli sunucu oturumu için geçerlidir. Sunucu kapatıldığında paket yüklemeleri kalıcı değildir.

## <a name="python"></a>Python

Python paketleri pip ya da kod hücreleri komutlarını kullanarak conda kullanarak yüklenebilir:

```bash
!pip install <package_name>

!conda install <package_name> -y
```

Komut çıktısı olduğunu gösteriyorsa gereksinim uyulmuş olur ve ardından Azure not defterleri, varsayılan olarak paket içerebilir. Paket yoluyla da yüklenebilir bir [proje ortamı Kurulum adımı](configure-manage-azure-notebooks-projects.md#configure-the-project-environment).

## <a name="r"></a>R

R 'deki paketler, bir kod hücresindeki `install.packages` işlevi kullanılarak CRAN veya GitHub 'dan yüklenebilir:

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

İçindeki paketleri F# yüklenebilir [nuget.org](https://www.nuget.org) Paket bağımlılık Yöneticisi'nden kod hücreleri çağırarak. İlk olarak, Paket Yöneticisi'ni Yükle:

```fsharp
#load "Paket.fsx"
```

Ardından, paketleri yükleyin:

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

Tüm ormalleri açın:
```fsharp
open MathNet.Numerics
```

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl yapılır: projeleri yönetme ve yapılandırma](configure-manage-azure-notebooks-projects.md)
- [Nasıl yapılır: bir slayt gösterisi sunar](present-jupyter-notebooks-slideshow.md)
