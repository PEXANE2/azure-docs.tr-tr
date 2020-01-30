---
title: 'Dışarı aktarma verileri: modül başvurusu'
titleSuffix: Azure Machine Learning
description: İşlem hatlarınızdan sonuçları, ara verileri ve çalışma verilerini Azure Machine Learning dışında bulut depolama hedeflerine kaydetmek için Azure Machine Learning verileri dışarı aktar modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: c83bfc4a3ec8d1163669f28dfde64c63e7c6ff89
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76775079"
---
# <a name="export-data-module"></a>Veri modülünü dışarı aktarma

Bu makalede Azure Machine Learning tasarımcısında modül (Önizleme) açıklanmaktadır.

İşlem hatlarınızdan sonuçları, ara verileri ve çalışma verilerini Azure Machine Learning dışında bulut depolama hedeflerine kaydetmek için bu modülü kullanın. 

Bu modül verilerinizi aşağıdaki bulut veri hizmetlerine aktarmayı destekler:

- Azure Blob kapsayıcısı
- Azure Dosya Paylaşımı
- Azure Data Lake
- Azure Data Lake Gen2

Verilerinizi dışarı aktarmadan önce önce Azure Machine Learning çalışma alanınıza önce bir veri deposu kaydetmeniz gerekir. Daha fazla bilgi için bkz. [Azure Storage hizmetlerindeki verilere erişme](../how-to-access-data.md).

## <a name="how-to-configure-export-data"></a>Dışarı aktarma verilerini yapılandırma

1. **Veri dışa aktarma** modülünü tasarımcıda işlem hattınızı ekleyin. Bu modülü **giriş ve çıkış** kategorisinde bulabilirsiniz.

1. Dışarı aktarma **verilerini** , dışarı aktarmak istediğiniz verileri içeren modüle bağlayın.

1. **Özellikler** bölmesini açmak Için **verileri dışarı aktar** ' ı seçin.

1. **Veri deposu**için, açılan listeden var olan bir veri deposunu seçin. Yeni bir veri deposu da oluşturabilirsiniz. [Azure depolama hizmetleri 'Nde erişim verilerini](../how-to-access-data.md)ziyaret ederek nasıl olduğunu denetleyin.

1. Verilerin yazılacağı veri deposundaki yolu tanımlayın. 


1. **Dosya biçimi**için verilerin depolanacağı biçimi seçin.
 
1. İşlem hattını çalıştırma.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın. 
