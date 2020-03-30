---
title: 'İhracat Verileri: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Sonuçları, ara verileri ve çalışma verilerini azure machine learning dışındaki bulut depolama hedeflerine kaydetmek için Azure Machine Learning'deki Dışa Aktarma Verileri modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 807771fd4018c9666f059c965370ebc36d0105df
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456310"
---
# <a name="export-data-module"></a>İhracat Verileri modülü

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Sonuçları, ara verileri ve çalışma verilerini bulut depolama hedeflerine kaydetmek için bu modülü kullanın. 

Bu modül, verilerinizi aşağıdaki bulut veri hizmetlerine dışa aktarmayı destekler:

- Azure Blob Konteyner
- Azure Dosya Paylaşımı
- Azure Data Lake
- Azure Veri Gölü Gen2

Verilerinizi dışa aktarmadan önce, azure makine öğrenimi çalışma alanınızda bir veri deposu kaydetmeniz gerekir. Daha fazla bilgi için Azure [depolama hizmetlerinde Verilere Erişim'e](../how-to-access-data.md)bakın.

## <a name="how-to-configure-export-data"></a>Dışa Aktarma Verileri nasıl yapılandırılır?

1. Tasarımcıda ardışık boru hattınıza **Dışa** Aktarma Verileri modüllerini ekleyin. Bu modülü **Giriş ve Çıkış** kategorisinde bulabilirsiniz.

1. **Dışa Aktarma Verilerini** dışa aktarmak istediğiniz verileri içeren modüle bağlayın.

1. **Özellikler** bölmesini açmak için **Veri Aktar'ı** seçin.

1. **Datastore**için açılan listeden varolan bir veri deposu seçin. Ayrıca yeni bir veri deposu da oluşturabilirsiniz. Azure depolama [hizmetlerinde Access verilerini](../how-to-access-data.md)ziyaret ederek nasıl yapılacağını kontrol edin.

1. Onay kutusu, **Çıktıyı Yeniden Oluşturma**, çalışma zamanında çıktıyı yeniden oluşturmak için modülü n için çalıştırıp çalıştırmayacağına karar verir. 

    Varsayılan olarak seçili değil, yani modül daha önce aynı parametrelerle yürütülmüşse, sistem çalışma süresini azaltmak için son çalıştırmadaki çıktıyı yeniden kullanır. 

    Seçilirse, sistem çıkışı yeniden oluşturmak için modülü yeniden çalıştıracaktır.

1. Verilerin bulunduğu veri deposundaki yolu tanımlayın. Yol göreceli bir yoldur. Boş yollara veya URL yollarına izin verilmez.


1. **Dosya biçimi**için, verilerin depolandığı biçimi seçin.
 
1. Boru hattını gönderin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
