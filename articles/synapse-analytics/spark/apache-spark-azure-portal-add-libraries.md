---
title: Azure SYNAPSE Analytics 'te Apache Spark için kitaplıklar ekleme ve yönetme
description: Azure SYNAPSE Analytics 'te Apache Spark tarafından kullanılan kitaplıkları ekleme ve yönetme hakkında bilgi edinin.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: euang
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 80414ccd6d5797614dd15bd61af8f37b3d2be05c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81870377"
---
# <a name="add-and-manage-libraries-for-apache-spark-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te Apache Spark için kitaplıklar ekleme ve yönetme

Apache Spark işlevselliği sağlamak için birçok kitaplıklara bağımlıdır. Bu kitaplıklar, ek kitaplıklar veya eski sürümlerin güncelleştirilmiş sürümleri ile genişletilebilir veya değiştirilebilir.

Python paketleri Spark Havuzu (Önizleme) düzeyinde eklenebilir ve. jar tabanlı paketler Spark iş tanımı düzeyine eklenebilir.

## <a name="adding-or-updating-python-libraries"></a>Python kitaplıklarını ekleme veya güncelleştirme

Azure SYNAPSE Analytics 'teki Apache Spark, tam bir anacondas yüklemesi ve ek kitaplıklar içerir. Tam kitaplıklar listesi, [Apache Spark sürüm desteği](apache-spark-version-support.md)' nde bulunabilir.

Bir Spark örneği başlatıldığında, bu yükleme temel olarak kullanılarak yeni bir sanal ortam oluşturulur. Ayrıca, sanal ortamı yükseltmek için bir *requirements. txt* dosyası `pip freeze` (komuttan çıktı) kullanılabilir. Bu dosyada yüklenmek veya yükseltmek üzere listelenen paketler, küme başlatma sırasında PyPi 'den indirilir. Bu gereksinimler dosyası, bu Spark havuzundan bir Spark örneği oluşturulduğu her seferinde kullanılır.

> [!IMPORTANT]
>
> - Yüklemekte olduğunuz Paket büyükse veya yüklemesi uzun sürüyorsa, bu, Spark örneği başlangıç süresini etkiler.
> - GCC gibi yüklemede, derleme zamanında derleyici desteği gerektiren paketler desteklenmez.
> - Paketler düşürülemez, yalnızca eklenebilir veya yükseltilebilir.

### <a name="requirements-format"></a>Gereksinimler biçimi

Aşağıdaki kod parçacığında gereksinimler dosyasının biçimi gösterilmektedir. Pypı paket adı, tam bir sürüm ile birlikte listelenir. Bu dosya, [PIP dondurma](https://pip.pypa.io/en/stable/reference/pip_freeze/) başvuru belgelerinde açıklanan biçimi izler. Bu örnek, belirli bir sürümü sabitler. Ayrıca, bu dosyadaki "daha büyük olmayan" ve "küçüktür" sürümlerinden de bir de belirtebilirsiniz.

```
absl-py==0.7.0

adal==1.2.1

alabaster==0.7.10
```

### <a name="python-library-user-interface"></a>Python kitaplığı Kullanıcı arabirimi

Kitaplık ekleme için Kullanıcı arabirimi, Azure portal **Apache Spark havuzu oluştur** sayfasının **ek ayarlar** sekmesindedir.

Sayfanın **paketler** bölümündeki dosya seçicisini kullanarak ortam yapılandırma dosyasını karşıya yükleyin.

![Python kitaplıklarını ekleme](./media/apache-spark-azure-portal-add-libraries/add-python-libraries.png "Python kitaplıklarını ekleme")

### <a name="verifying-installed-libraries"></a>Yüklü kitaplıklar doğrulanıyor

Doğru kitaplıkların doğru sürümlerinin yüklenip yüklenmediğini doğrulamak için aşağıdaki kodu çalıştırın

```python
import pip #needed to use the pip functions
for i in pip.get_installed_distributions(local_only=True):
    print(i)
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
- [Apache Spark belgeleri](https://spark.apache.org/docs/2.4.4/)
