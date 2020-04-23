---
title: Azure Synapse Analytics'te Apache Spark için kitaplık ekleme ve yönetme
description: Azure Synapse Analytics'te Apache Spark tarafından kullanılan kitaplıkları nasıl ekleyeceğinizi ve yöneteceğinizi öğrenin.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: euang
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 80414ccd6d5797614dd15bd61af8f37b3d2be05c
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81870377"
---
# <a name="add-and-manage-libraries-for-apache-spark-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te Apache Spark için kitaplık ekleme ve yönetme

Apache Spark işlevsellik sağlamak için birçok kitaplık bağlıdır. Bu kitaplıklar artırılabilir veya ek kitaplıklar veya eskilerinin güncelleştirilmiş sürümleriyle değiştirilebilir.

Python paketleri Spark havuzu (önizleme) düzeyinde eklenebilir ve .jar tabanlı paketler Spark iş tanımı düzeyinde eklenebilir.

## <a name="adding-or-updating-python-libraries"></a>Python kitaplıklarını ekleme veya güncelleştirme

Azure Synapse Analytics'teki Apache Spark'ta tam bir Anacondas yüklemesi ve ek kitaplıklar vardır. Tam kitaplıklar listesi [Apache Spark sürüm desteği](apache-spark-version-support.md)bulunabilir.

Bir Spark örneği başlatıldığında, bu yükleme temel olarak kullanılarak yeni bir sanal ortam oluşturulur. Ayrıca, bir *requirements.txt* dosyası `pip freeze` (komutçıktı) sanal ortamı yükseltmek için kullanılabilir. Yüklemek veya yükseltmek için bu dosyada listelenen paketler küme başlatma sırasında PyPi'den indirilir. Bu gereksinimler dosyası, bu Spark havuzundan her Spark örneği oluşturulduğunda kullanılır.

> [!IMPORTANT]
>
> - Yüklediğiniz paketin büyük olması veya yüklenmesi uzun sürüyorsa, bu Durum Spark örneğinin başlatma süresini etkiler.
> - GCC gibi yükleme zamanında derleyici desteği gerektiren paketler desteklenmez.
> - Paketler indirgenemez, yalnızca eklenebilir veya yükseltilir.

### <a name="requirements-format"></a>Gereksinimler biçimi

Aşağıdaki snippet gereksinimler dosyasının biçimini gösterir. PyPi paket adı tam bir sürümü ile birlikte listelenir. Bu [dosya, pip donma](https://pip.pypa.io/en/stable/reference/pip_freeze/) başvuru belgelerinde açıklanan biçimi izler. Bu örnek, belirli bir sürümü sabitler. Ayrıca bu dosyada "daha büyük" ve "daha az" sürümleri ni de belirtebilirsiniz.

```
absl-py==0.7.0

adal==1.2.1

alabaster==0.7.10
```

### <a name="python-library-user-interface"></a>Python kitaplığı kullanıcı arabirimi

Kitaplık ekleme için ui, Azure portalındaki **Apache Spark havuzu oluştur** sayfasının **Ek ayarlar** sekmesinde yer alır.

Sayfanın **Paketler** bölümündeki dosya seçicisini kullanarak ortam yapılandırma dosyasını yükleyin.

![Python kitaplıkları ekleme](./media/apache-spark-azure-portal-add-libraries/add-python-libraries.png "Python kitaplıkları ekleme")

### <a name="verifying-installed-libraries"></a>Yüklü kitaplıkları doğrulama

Doğru kitaplıkların doğru sürümlerinin yüklü olup olmadığını doğrulamak için aşağıdaki kodu çalıştırın

```python
import pip #needed to use the pip functions
for i in pip.get_installed_distributions(local_only=True):
    print(i)
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
- [Apache Kıvılcım Dokümantasyon](https://spark.apache.org/docs/2.4.4/)
