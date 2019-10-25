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
ms.openlocfilehash: 3f969fb346ce341e694458dcc38bf193f012226b
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792644"
---
İşlem hattı, çalışma alanınıza bağlı bir işlem kaynağı olan bir işlem hedefi üzerinde çalışır. Bir işlem hedefi oluşturduktan sonra, gelecekteki çalıştırmalar için onu yeniden kullanabilirsiniz.

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

    > [!NOTE]
    > Denemeleri Group benzer işlem hattı birlikte çalışır. Birden çok kez işlem hattı çalıştırırsanız, art arda çalıştırmalar için aynı denemeyi seçebilirsiniz.

    * Açıklayıcı bir **deneme adı** girin

    * **Çalıştır** 'ı seçin
    
    Çalışma durumunu ve ayrıntılarını tuvalin sağ üst kısmında görüntüleyebilirsiniz.

    > [!NOTE]
    > İşlem kaynağının oluşturulması yaklaşık 5 dakika sürer. Kaynak oluşturulduktan sonra yeniden kullanabilir ve gelecekteki çalıştırmalar için bu bekleme süresini atlayabilirsiniz.
    >
    > İşlem kaynağı, maliyeti kaydetmek için boşta kaldığında 0 düğüm için otomatik olarak ayarlanır.  Bir gecikmeden sonra yeniden kullandığınızda, yeniden ölçeklendirilirken yaklaşık 5 dakikalık bekleme süresi daha yaşayabilirsiniz.
