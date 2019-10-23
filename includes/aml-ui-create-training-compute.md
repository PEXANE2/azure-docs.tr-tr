---
title: include dosyası
description: include dosyası
services: machine-learning
author: peterclu
ms.service: machine-learning
ms.author: peterlu
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 10/09/2019
ms.openlocfilehash: 28478f38df6ba6ea356626cc36c23ad498fc1f47
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72692417"
---
İşlem hattı, çalışma alanınıza bağlı bir işlem kaynağı olan işlem hedefi üzerinde çalışır.  Bir işlem hedefi oluşturduktan sonra, gelecekteki çalıştırmalar için onu yeniden kullanabilirsiniz.

1. İşlem hattını çalıştırmak için tuvalin en üstünde **Çalıştır** ' ı seçin.

1. **Ayarlar** bölmesi göründüğünde, **işlem hedefini seç**' i seçin.

    Zaten kullanılabilir bir işlem hedefi varsa, bu işlem hattını çalıştırmak için seçin.

    > [!NOTE]
    > Görsel arabirim yalnızca Machine Learning İşlem hedeflerinde denemeleri çalıştırabilir. Diğer işlem hedefleri gösterilmeyecektir.

1. İşlem kaynağı için bir ad girin.

1. **Kaydet**’i seçin.

    ![Kurulum işlem hedefi](./media/aml-ui-create-training-compute/set-compute.png)

1. **Çalıştır**'ı seçin.

1. İşlem **hattı çalıştırmasını ayarla** iletişim kutusunda, **deneme** için **+ Yeni deneme** ' yı seçin.

    * Açıklayıcı bir **deneme adı** girin

    * **Çalıştır** 'ı seçin
    
    Çalışma durumunu ve ayrıntılarını tuvalin sağ üst kısmında görüntüleyebilirsiniz.

    > [!NOTE]
    > İşlem kaynağının oluşturulması yaklaşık 5 dakika sürer. Kaynak oluşturulduktan sonra yeniden kullanabilir ve gelecekteki çalıştırmalar için bu bekleme süresini atlayabilirsiniz.
    >
    > İşlem kaynağı, maliyeti kaydetmek için boşta kaldığında 0 düğüm için otomatik olarak ayarlanır.  Bir gecikmeden sonra yeniden kullandığınızda, yeniden ölçeklendirilirken yaklaşık 5 dakikalık bekleme süresi daha yaşayabilirsiniz.
