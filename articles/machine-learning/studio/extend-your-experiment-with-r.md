---
title: R ile denemenizi genişletme
titleSuffix: ML Studio (classic) - Azure
description: Execute R Script modüllerini kullanarak Azure Machine Learning Studio'nun (klasik) işlevselliğini R dili üzerinden nasıl genişletebilirsiniz?
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: 7b4b869695eb2073121a889cd81d99c4fc06d4b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218045"
---
# <a name="azure-machine-learning-studio-classic-extend-your-experiment-with-r"></a>Azure Machine Learning Studio (klasik): R ile denemenizi genişletin 

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]
Azure Machine Learning Studio'nun (klasik) işlevselliğini [Execute R Script][execute-r-script] modülünün kullanarak R dili üzerinden genişletebilirsiniz.

Bu modül birden çok giriş veri kümesini kabul eder ve çıktı olarak tek bir veri kümesi verir. [Execute R Script][execute-r-script] modülünün R **Script** parametresine bir R komut dosyası yazabilirsiniz.

Modülün her giriş bağlantı noktasına aşağıdakilere benzer bir kod kullanarak erişebilirsiniz:

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a>Şu anda yüklenmiş tüm paketleri listeleme
Yüklenen paketlerin listesi değişebilir. Şu anda yüklü paketlerin bir listesini [Azure Machine Learning Studio (klasik) tarafından desteklenen R Paketleri](https://msdn.microsoft.com/library/azure/mt741980.aspx)bulunabilir.

Ayrıca, Aşağıdaki kodu [Yürüt R Script][execute-r-script] modülüne girerek yüklü paketlerin tam, güncel listesini de alabilirsiniz:

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

Bu, paketlerin listesini Execute R [Script][execute-r-script] modülünün çıkış noktasına gönderir.
Paket listesini görüntülemek için, [CSV'ye Dönüştür][convert-to-csv] gibi bir dönüşüm modülünü [Execute R Script][execute-r-script] modülünün sol çıktısına bağlayın, denemeyi çalıştırın, ardından dönüşüm modülünün çıktısını tıklatın ve **İndir'i**seçin. 

!["CSV'ye Dönüştür" modülünün çıktısını indirin](./media/extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is the [current full list with version numbers in Excel format](https://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a>Paketleri alma
[Yürüt R Script][execute-r-script] modülünde aşağıdaki komutları kullanarak zaten yüklü olmayan paketleri içe aktarabilirsiniz:

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

dosyanın `my_favorite_package.zip` paketinizi içerdiği yer.




<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
