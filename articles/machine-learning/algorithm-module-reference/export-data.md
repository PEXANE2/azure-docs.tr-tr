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
ms.date: 02/22/2020
ms.openlocfilehash: 807771fd4018c9666f059c965370ebc36d0105df
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79456310"
---
# <a name="export-data-module"></a>Veri modülünü dışarı aktarma

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

İşlem hatlarınızdan bulut depolama hedeflerine sonuçları, ara verileri ve çalışma verilerini kaydetmek için bu modülü kullanın. 

Bu modül verilerinizi aşağıdaki bulut veri hizmetlerine aktarmayı destekler:

- Azure Blob kapsayıcısı
- Azure Dosya Paylaşımı
- Azure Data Lake
- Azure Data Lake Gen2

Verilerinizi dışarı aktarmadan önce, önce Azure Machine Learning çalışma alanınıza bir veri deposu kaydetmeniz gerekir. Daha fazla bilgi için bkz. [Azure Storage hizmetlerindeki verilere erişme](../how-to-access-data.md).

## <a name="how-to-configure-export-data"></a>Dışarı aktarma verilerini yapılandırma

1. **Veri dışa aktarma** modülünü tasarımcıda işlem hattınızı ekleyin. Bu modülü **giriş ve çıkış** kategorisinde bulabilirsiniz.

1. Dışarı aktarma **verilerini** , dışarı aktarmak istediğiniz verileri içeren modüle bağlayın.

1. **Özellikler** bölmesini açmak Için **verileri dışarı aktar** ' ı seçin.

1. **Veri deposu**için, açılan listeden var olan bir veri deposunu seçin. Yeni bir veri deposu da oluşturabilirsiniz. [Azure depolama hizmetleri 'Nde erişim verilerini](../how-to-access-data.md)ziyaret ederek nasıl olduğunu denetleyin.

1. Onay kutusu, **çıktıyı yeniden oluştur**, çalışma zamanında çıktıyı yeniden oluşturmak için modülün yürütülüp yürütülmeyeceğine karar verir. 

    Bu, varsayılan olarak seçili değildir, bu da modülün aynı parametrelerle daha önce yürütülmesi durumunda sistemin, çalışma süresini azaltmak için son çalıştırışınızda çıktıyı yeniden kullanacağı anlamına gelir. 

    Seçilirse, sistem çıktıyı yeniden oluşturmak için modülü yeniden yürütür.

1. Verilerin bulunduğu veri deposundaki yolu tanımlayın. Yol, göreli bir yoldur. Boş yollara veya URL yollarına izin verilmez.


1. **Dosya biçimi**için verilerin depolanacağı biçimi seçin.
 
1. İşlem hattını gönderme.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
