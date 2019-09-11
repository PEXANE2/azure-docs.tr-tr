---
title: Azure üzerinde Jupyter notebook paketleri yükleme
description: Python, R, yükleme ve F# paketleri gelen içinde Azure üzerinde çalışan bir Jupyter not defteri.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 6f089c12-128b-4dbd-96e3-1320d37eeba4
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: b0881cb6dac9ec83d2126942c758508e760f9c83
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70274433"
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

- [Nasıl yapılır: Projeleri yapılandırma ve yönetme](configure-manage-azure-notebooks-projects.md)
- [Nasıl yapılır: Bir slayt gösterisi sunun](present-jupyter-notebooks-slideshow.md)
