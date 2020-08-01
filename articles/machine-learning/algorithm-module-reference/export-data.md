---
title: 'Dışarı aktarma verileri: modül başvurusu'
titleSuffix: Azure Machine Learning
description: İşlem hatlarınızdan sonuçları, ara verileri ve çalışma verilerini Azure Machine Learning dışında bulut depolama hedeflerine kaydetmek için Azure Machine Learning verileri dışarı aktar modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 07/28/2020
ms.openlocfilehash: d0fd11ce5b25b8796b07176a4ba83091a8f7cc0b
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87461026"
---
# <a name="export-data-module"></a>Veri modülünü dışarı aktarma

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

İşlem hatlarınızdan bulut depolama hedeflerine sonuçları, ara verileri ve çalışma verilerini kaydetmek için bu modülü kullanın. 

Bu modül verilerinizi aşağıdaki bulut veri hizmetlerine aktarmayı destekler:

- Azure Blob kapsayıcısı
- Azure Dosya Paylaşımı
- Azure Data Lake Storage Gen1
- Azure Data Lake Storage Gen2
- Azure SQL veritabanı

Verilerinizi dışarı aktarmadan önce, önce Azure Machine Learning çalışma alanınıza bir veri deposu kaydetmeniz gerekir. Daha fazla bilgi için bkz. [Azure Storage hizmetlerindeki verilere erişme](../how-to-access-data.md).

## <a name="how-to-configure-export-data"></a>Dışarı aktarma verilerini yapılandırma

1. **Veri dışa aktarma** modülünü tasarımcıda işlem hattınızı ekleyin. Bu modülü **giriş ve çıkış** kategorisinde bulabilirsiniz.

1. Dışarı aktarma **verilerini** , dışarı aktarmak istediğiniz verileri içeren modüle bağlayın.

1. **Özellikler** bölmesini açmak Için **verileri dışarı aktar** ' ı seçin.

1. **Veri deposu**için, açılan listeden var olan bir veri deposunu seçin. Yeni bir veri deposu da oluşturabilirsiniz. [Azure depolama hizmetleri 'Nde erişim verilerini](../how-to-access-data.md)ziyaret ederek nasıl olduğunu denetleyin.

    > [!NOTE]
    > Belirli bir veri türünün verilerinin başka bir veri türü olarak belirtilen bir SQL veritabanı sütununa aktarılması desteklenmez. Hedef tablonun önce mevcut olması gerekmez.

1. Onay kutusu, **çıktıyı yeniden oluştur**, çalışma zamanında çıktıyı yeniden oluşturmak için modülün yürütülüp yürütülmeyeceğine karar verir. 

    Bu, varsayılan olarak seçili değildir, bu da modülün aynı parametrelerle daha önce yürütülmesi durumunda sistemin, çalışma süresini azaltmak için son çalıştırışınızda çıktıyı yeniden kullanacağı anlamına gelir. 

    Seçilirse, sistem çıktıyı yeniden oluşturmak için modülü yeniden yürütür.

1. Verilerin bulunduğu veri deposundaki yolu tanımlayın. Yol, göreli bir yoldur. Boş yollara veya URL yollarına izin verilmez.


1. **Dosya biçimi**için verilerin depolanacağı biçimi seçin.
 
1. İşlem hattını gönderme.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
