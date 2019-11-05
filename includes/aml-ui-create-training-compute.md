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
ms.openlocfilehash: f7cdfb8b9edbce5ef2b094cf6a603904d39a7404
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73493586"
---
İşlem hattı, çalışma alanınıza bağlı bir işlem kaynağı olan bir işlem hedefi üzerinde çalışır. Bir işlem hedefi oluşturduktan sonra, gelecekteki çalıştırmalar için onu yeniden kullanabilirsiniz.

1. İşlem hattını çalıştırmak için tuvalin en üstünde **Çalıştır** ' ı seçin.

1. **Ayarlar** bölmesi göründüğünde, **işlem hedefini seç**' i seçin.

    Zaten kullanılabilir bir işlem hedefi varsa, bu işlem hattını çalıştırmak için seçin.

    > [!NOTE]
    > Tasarımcı yalnızca Machine Learning İşlem hedeflerinde denemeleri çalıştırabilir. Diğer işlem hedefleri gösterilmeyecektir.

1. İşlem kaynağı için bir ad girin.

1. **Kaydet**’i seçin.

    ![Kurulum işlem hedefi](./media/aml-ui-create-training-compute/set-compute.png)

1. **Çalıştır**'ı seçin.

1. İşlem **hattı çalıştırmasını ayarla** iletişim kutusunda, **deneme** için **+ Yeni deneme** ' yı seçin.

    > [!NOTE]
    > Denemeleri Group benzer işlem hattı birlikte çalışır. Birden çok kez işlem hattı çalıştırırsanız, art arda çalıştırmalar için aynı denemeyi seçebilirsiniz.

    * Açıklayıcı bir **deneme adı** girin

    * **Çalıştır** 'ı seçin
    
    Çalışma durumunu ve ayrıntılarını tuvalin sağ üst kısmında görüntüleyebilirsiniz.

    > [!NOTE]
    > İşlem kaynağının oluşturulması yaklaşık 5 dakika sürer. Kaynak oluşturulduktan sonra yeniden kullanabilir ve gelecekteki çalıştırmalar için bu bekleme süresini atlayabilirsiniz.
    >
    > İşlem kaynağı, maliyeti kaydetmek için boşta kaldığında 0 düğüm için otomatik olarak ayarlanır.  Bir gecikmeden sonra yeniden kullandığınızda, yeniden ölçeklendirilirken yaklaşık 5 dakikalık bekleme süresi daha yaşayabilirsiniz.
