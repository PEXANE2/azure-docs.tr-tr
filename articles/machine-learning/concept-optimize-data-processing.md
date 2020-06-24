---
title: Veri işlemeyi iyileştirme
titleSuffix: Azure Machine Learning
description: Veri işleme hızlarını iyileştirmek için en iyi yöntemleri ve ölçekteki veri işleme için hangi tümleştirmelerin Azure Machine Learning destekleyeceğinizi öğrenin.
services: machine-learning
ms.service: machine-learning
author: sgilley
ms.author: sgilley
ms.subservice: core
ms.reviewer: nibaccam
ms.topic: conceptual
ms.date: 05/29/2020
ms.openlocfilehash: 507b03266e70ae082872890c9d5cddb50fdeb703
ms.sourcegitcommit: a8928136b49362448e992a297db1072ee322b7fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84719993"
---
# <a name="optimize-data-processing-with-azure-machine-learning"></a>Azure Machine Learning ile veri işlemeyi iyileştirme

Bu makalede, veri işleme hızlarını yerel olarak ve ölçekteki iyileştirmenize yardımcı olacak en iyi yöntemler hakkında bilgi edineceksiniz.

Azure Machine Learning, veri işleme için açık kaynaklı paketler ve çerçeveler ile tümleşiktir. Bu tümleştirmelere ve en iyi uygulama önerilerini uygulayarak, veri işleme hızlarınızın hem yerel olarak hem de ölçekteki kullanımını geliştirebilirsiniz.

## <a name="parquet-and-csv-file-formats"></a>Parquet ve CSV dosya biçimleri

Virgülle ayrılmış değerler (CSV) dosyaları, veri işleme için ortak dosya formatlardır. Ancak, Machine Learning görevleri için Parquet dosya biçimleri önerilir.

[Parquet dosyaları](https://parquet.apache.org/) , verileri ikili sütunlu biçimde depolar. Bu biçim, verilerin birden çok dosyaya bölünmesi gerektiğinde faydalıdır. Ayrıca, bu biçim makinenizin öğrenimi denemeleri ilgili alanlarını hedefetmenize olanak tanır. Bir 20 GB veri dosyasında okumak zorunda kalmak yerine, ML modelinizi eğitebilmeniz için gerekli sütunları seçerek bu veri yükünü azaltabilirsiniz. Parquet dosyaları, işlem gücünü en aza indirmek ve daha az alan kazanmak için de sıkıştırılabilir.

CSV dosyaları, Excel 'de düzenleme ve okuma kolaylıdıklarından verileri içeri ve dışarı aktarmak için yaygın olarak kullanılır. CSV 'lerde bulunan veriler, satır tabanlı bir biçimde dizeler olarak depolanır ve dosyalar, veri aktarımı yüklerini azaltmak için sıkıştırılabilirler. Sıkıştırılmamış CSV 'ler, yaklaşık 2-10 faktörüyle genişletebilir ve sıkıştırılan CSV 'leri daha da artırabilir. Bellekteki 5 GB CSV, makinenizde bulunan 8 GB RAM 'e kadar genişleyecektir. Bu sıkıştırma davranışı, işlemek için büyük miktarlardaki verilere sahipseniz ideal olmayan veri aktarımı gecikmesini artırabilir. 

## <a name="pandas-dataframe"></a>Pandas dataframe

[Pandas dataframes](https://pandas.pydata.org/pandas-docs/stable/getting_started/overview.html) genellikle veri işleme ve analiz için kullanılır. `Pandas`1 GB 'tan küçük veri boyutları için iyi işlem yapın, ancak `pandas` dosya boyutları 1 GB 'a ulaştığında veri çerçevelerinin işlenme süreleri yavaşlar. Bu yavaşlatma, depolama alanındaki verilerinizin boyutunun bir veri çerçevesindeki verilerin boyutuyla aynı olmaması nedeniyle oluşur. Örneğin, CSV dosyalarındaki veriler bir veri çerçevesinde en fazla 10 kez genişleyebilir, bu nedenle 1 GB CSV dosyası bir veri çerçevesinde 10 GB olabilir.

`Pandas`tek iş parçacıklı olduğundan, tek bir CPU 'da tek seferde bir işlem yapılır. Tek bir Azure Machine Learning işlem örneğinde iş yüklerini, dağıtılmış bir arka uç kullanarak bu [Sardaki modlarda](https://modin.readthedocs.io/en/latest/) bulunan paketlerle birlikte birden çok sanal CPU 'ya paralel hale getirmek `Pandas` .

Görevlerinizi ve Gamze ile paralel hale getirmek `Modin` için [Dask](https://dask.org)Bu kod satırını olarak değiştirmeniz yeterlidir `import pandas as pd` `import modin.pandas as pd` .

## <a name="dataframe-out-of-memory-error"></a>Dataframe: yetersiz bellek hatası 

Veri çerçeveli, makinenizde kullanılabilir RAM 'in üzerine genişliyorsa genellikle *yetersiz bellek* hatası oluşur. Bu kavram, veya gibi dağıtılmış bir çerçeve için de geçerlidir `Modin` `Dask` .  Diğer bir deyişle, işlem, kümenizdeki her düğümdeki veri çerçevesini bellekte yüklemeye çalışır, ancak bunu yapmak için yeterli RAM yok.

Bir çözüm, RAM 'nizin veri çerçevesini bellekte sığacak şekilde artırması. İşlem boyutunuzu ve işleme gücünün, RAM boyutunun iki katı olması önerilir. Bu nedenle, veri çerçevünüz 10 GB ise, veri çerçevesinin belleğe uygun olduğundan ve işlenebilmesi için en az 20 GB RAM içeren bir işlem hedefi kullanın. 

Birden çok sanal CPU için vCPU, bir bölümün makinede her vCPU 'Nun sahip olduğu RAM 'e rahat bir şekilde sığması istediğinizi aklınızda bulundurun. Diğer bir deyişle, 16 GB RAM 4 vCPU varsa her vCPU için 2 GB veri çerçevesi isteyebilirsiniz.

### <a name="minimize-cpu-workloads"></a>CPU iş yüklerini en aza indirme

Makinenize daha fazla RAM ekleyemiyorum, CPU iş yüklerini en aza indirmek ve işlem sürelerini iyileştirmek için aşağıdaki teknikleri uygulayabilirsiniz. Bu öneriler hem tek hem de Dağıtılmış sistemlere aittir.

Teknik | Description
----|----
Sıkıştırma | Verileriniz için, daha az bellek kullanan ve hesaplamanızın sonuçlarını önemli ölçüde etkilemeyen bir şekilde farklı bir gösterim kullanın.<br><br>*Örnek:* Girişleri her giriş için yaklaşık 10 bayt veya daha fazla olan bir dize olarak depolamak yerine, bunları bir Boolean, true veya false olarak depolayarak 1 baytlık bir biçimde saklayabilirsiniz.
Öbekleme | Verileri alt kümeler (parçalar) halinde belleğe yükleyin, verileri zamanında bir alt küme veya paralel olarak birden çok alt küme ile işleme. Bu yöntem, tüm verileri işleyebilmeniz, ancak tüm verileri tek seferde belleğe yüklemeniz gerekmiyorsa en iyi şekilde geçerlidir. <br><br>*Örnek:* Her seferinde bir yıl boyunca verilerin tam olarak işlenmesi yerine verileri bir ay yükleyin ve işleyin.
Dizinleme | Size önem verdiğiniz verileri nerede bulabileceğinizi belirten bir Özet olan bir dizin uygulayın ve kullanın. Dizin oluşturma, tam küme yerine yalnızca verilerin bir alt kümesini kullanmanız gerektiğinde yararlıdır.<br><br>*Örnek:* Aya göre sıralanmış satış verilerinin tam yıllarına sahipseniz bir dizin, işlemek istediğiniz ay için hızlı bir şekilde arama yapmanıza yardımcı olur.

## <a name="scale-data-processing"></a>Ölçek verileri işleme

Önceki öneriler yeterli değilse ve verilerinize uygun bir sanal makine alamazsanız, şunları yapabilirsiniz 

* `Spark` `Dask` ' Bellek yetersiz ' verilerini işlemek için veya gibi bir çerçeve kullanın. Bu seçenekte, veri çerçevesi, son olarak toplanmakta olan son sonuçla birlikte bölüm tarafından RAM bölümüne yüklenir ve işlenir. 

* Dağıtılmış bir çerçeve kullanarak bir kümeye Genişleme. Bu seçenekte, veri işleme yükleri, son olarak toplanan son sonuçla birlikte paralel olarak çalışan birden çok CPU üzerinde bölünür ve işlenir.

### <a name="recommended-distributed-frameworks"></a>Önerilen dağıtılmış çerçeveler

Aşağıdaki tabloda, kod tercihlerinize veya veri boyutuna göre Azure Machine Learning tümleştirilmiş dağıtılmış çerçeveler önerilmektedir.

Deneyim veya veri boyutu | Öneri
------|------
Hakkında bilginiz varsa`Pandas`| `Modin`veya `Dask` veri çerçevesi
Tercih ediyorsanız`Spark` | `PySpark`
1 GB 'tan küçük veriler için | `Pandas`Yerel olarak **veya** bir uzak Azure Machine Learning işlem örneği
10 GB 'tan büyük veriler için| `Ray`, Veya kullanarak bir kümeye taşıma `Dask``Spark`

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning Ile veri alma seçenekleri](concept-data-ingestion.md).
* [Azure Data Factory Ile veri](how-to-data-ingest-adf.md)alımı.
