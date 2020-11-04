---
title: Azure Veri Bilimi Sanal Makinesi nedir?
titleSuffix: Azure Data Science Virtual Machine
description: Azure Veri Bilimi Sanal Makinesi 'ye genel bakış-Azure bulut platformunda, veri bilimi sağlamak için önceden yüklenmiş ve yapılandırılmış araçlar ve kitaplıklar ile kullanımı kolay bir sanal makine.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: overview
ms.date: 04/02/2020
ms.openlocfilehash: 7bcee0f571fe42e4c00499f188e35d0e0ff2ebe7
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93322499"
---
# <a name="what-is-the-azure-data-science-virtual-machine-for-linux-and-windows"></a>Linux ve Windows için Azure Veri Bilimi Sanal Makinesi nedir?

Veri Bilimi Sanal Makinesi (DSVM), veri bilimi yapmak için özel olarak oluşturulan Azure bulut platformunda özelleştirilmiş bir VM görüntüsüdür. Önceden yüklenmiş ve gelişmiş analiz için akıllı uygulamalar oluşturmaya başlamak üzere önceden yapılandırılmış birçok popüler veri bilimi aracı vardır.

DSVM şu şirket üzerinde kullanılabilir:

+ Windows Server 2019
+ Ubuntu 18,04 LTS

## <a name="comparison-with-azure-machine-learning"></a>Azure Machine Learning ile karşılaştırma

DSVM, veri bilimi için özelleştirilmiş bir VM görüntüsüdür ancak [Azure Machine Learning](../overview-what-is-azure-ml.md) (AzureML) şunları kapsayan uçtan uca bir platformdur:

+ Tam olarak yönetilen Işlem
  + İşlem Örnekleri
  + Dağıtılmış ML görevleri için işlem kümeleri
  + Gerçek zamanlı Puanlama için çıkarım kümeleri
+ Veri depoları (örneğin, blob, ADLS 2., SQL DB)
+ Deneme izleme
+ Model yönetimi
+ Notebooks
+ Ortamlar (Conda ve R bağımlılıklarını yönetme)
+ Kapatma
+ İşlem hatları (uçtan uca veri bilimi iş akışlarını otomatikleştirin)

### <a name="comparison-with-azureml-compute-instances"></a>AzureML Işlem örnekleriyle karşılaştırma

[Azure Machine Learning Işlem örnekleri](../concept-compute-instance.md) , dsvm __yönetilmeyen__ bir VM olduğunda, tam olarak yapılandırılmış ve __yönetilen__ bir VM görüntüsüdür.

Bu iki ürün teklifi arasındaki temel farklılıklar aşağıda ayrıntılı olarak verilmiştir:


|Özellik |Veri Bilimi<br>VM |AzureML<br>İşlem Örneği  | 
|---------|---------|---------|
| Tam olarak yönetilen | Hayır        | Evet        |
|Dil Desteği     |  Python, R, Julia, SQL, C#,<br> Java, Node.js, F #       | Python ve R        |
|İşletim Sistemi     | Ubuntu<br>Windows         |    Ubuntu     |
|Önceden yapılandırılmış GPU seçeneği     |  Evet       |    Evet     |
|Ölçeği artırma seçeneği | Evet | Evet |
|SSH erişimi    | Evet        |    Evet     |
|RDP erişimi    | Evet        |     Hayır    |
|Yerleşik<br>Barındırılan Not defterleri     |   No<br>(ek yapılandırma gerektirir)      |      Evet   |
|Yerleşik SSO     | No <br>(ek yapılandırma gerektirir)         |    Evet     |
|Yerleşik Işbirliği     | Hayır         | Evet        |
|Önceden yüklenmiş Araçlar     |  Jupiter (Lab), RStudio Server, VSCode,<br> Visual Studio, Pylt, Juno,<br>Power BI Desktop, SSMS, <br>Microsoft Office 365, Apache detaya gitme       |     Jupyıter (Lab)<br> RStudio Server   |

## <a name="sample-use-cases"></a>Kullanım örnekleri

Aşağıda, DSVM müşterileri için bazı yaygın kullanım durumları gösterilmektedir.

### <a name="short-term-experimentation-and-evaluation"></a>Kısa süreli deneme ve değerlendirme

Yeni veri bilimi [araçlarını](./tools-included.md), özellikle de yayınlanmış [örneklerimizden ve yönergelerden](./dsvm-samples-and-walkthroughs.md)bazılarını izleyerek değerlendirmek ya da öğrenmek için dsvm 'yi kullanabilirsiniz.

### <a name="deep-learning-with-gpus"></a>GPU'larla derin öğrenme

DSVM 'de, eğitim modelleriniz grafik işleme birimlerine (GPU 'Lar) dayalı donanımlar üzerinde derin öğrenme algoritmaları kullanabilir. DSVM, Azure platformunun VM ölçeklendirme özelliğinden yararlanarak, gereksinimlerinize göre bulutta GPU tabanlı donanımlar kullanmanıza yardımcı olur. Büyük modelleri eğitirken veya aynı işletim sistemi diskini kullanıp yüksek hızlı hesaplamalar yapmaya gerek duyduğunuzda GPU tabanlı bir VM’ye geçiş yapabilirsiniz. DSVM ile N serisi GPU 'Lar etkinleştirilmiş sanal makine SKU 'Larını seçebilirsiniz. Not GPU etkin sanal makine SKU 'Ları Azure Ücretsiz hesaplarında desteklenmez.

DSVM 'nin Windows sürümleri, derin öğrenme çerçevelerinin GPU sürücüleri, çerçeveleri ve GPU sürümleriyle önceden yüklenmiş olarak gelir. Linux sürümlerinde, GPU 'Larda derin öğrenme, Ubuntu DSVMs üzerinde etkinleştirilmiştir. 

Ayrıca, DSVM 'nin Ubuntu veya Windows sürümlerini GPU 'lara dayalı olmayan bir Azure sanal makinesine dağıtabilirsiniz. Bu durumda, tüm derin öğrenme çerçeveleri CPU moduna geri dönecektir.

[Kullanılabilir derin öğrenme ve AI çerçeveleri hakkında daha fazla bilgi edinin](dsvm-tools-deep-learning-frameworks.md).

### <a name="data-science-training-and-education"></a>Veri bilimi eğitimi

Veri bilimi dersi veren kurumsal eğitmenler genellikle bir sanal makine görüntüsü sağlar. Görüntü, öğrencilerin tutarlı bir kuruluma sahip olmasını ve örneklerin öngörülebilir bir şekilde çalışmasını sağlar.

DSVM, destek ve uyumsuzluk sorunlarını ele alan tutarlı bir kurulum ile isteğe bağlı bir ortam oluşturur. Başta daha kısa eğitim sınıfları olmak üzere bu ortamların sıklıkla oluşturulması gereken durumlar önemli ölçüde avantajlıdır.


## <a name="whats-included-on-the-dsvm"></a>DSVM 'ye neler dahildir?

[Burada](tools-included.md)hem Windows hem de Linux DSVMs üzerinde araçların tam listesini görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerle daha fazla bilgi edinin:

+ Windows:
  + [Windows DSVM’si ayarlama](provision-vm.md)
  + [Windows DSVM üzerinde veri bilimi](vm-do-ten-things.md)

+ Linux:
  + [Linux DSVM 'YI ayarlama (Ubuntu)](dsvm-ubuntu-intro.md)
  + [Linux DSVM üzerinde veri bilimi](linux-dsvm-walkthrough.md)