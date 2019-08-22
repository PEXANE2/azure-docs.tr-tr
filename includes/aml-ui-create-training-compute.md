---
title: include dosyası
description: include dosyası
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 05/06/2019
ms.openlocfilehash: eb84dc1b5bf3f756e484ef27aaa998ab6b94cc51
ms.sourcegitcommit: a3a40ad60b8ecd8dbaf7f756091a419b1fe3208e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69891701"
---
Bir deneme, çalışma alanınıza bağlı bir işlem kaynağı olan bir işlem hedefi üzerinde çalışır.  Bir işlem hedefi oluşturduktan sonra, gelecekteki çalıştırmalar için onu yeniden kullanabilirsiniz.

1. Denemeyi çalıştırmak için en altta **Çalıştır** ' ı seçin.

1. **Kurulum Işlem hedefleri** iletişim kutusu göründüğünde, çalışma alanınızın zaten bir işlem kaynağı varsa, bunu şimdi seçebilirsiniz.  Aksi takdirde, **Yeni oluştur**' u seçin.

    > [!NOTE]
    > Görsel arabirim yalnızca Machine Learning İşlem hedeflerinde denemeleri çalıştırabilir. Diğer işlem hedefleri gösterilmeyecektir.

1. İşlem kaynağı için bir ad girin.

1. **Çalıştır**'ı seçin.

    ![Kurulum işlem hedefi](./media/aml-ui-create-training-compute/set-compute.png)

    İşlem kaynağı şimdi oluşturulacak. Denemenin sağ üst köşesindeki durumu görüntüleyin. 

    > [!NOTE]
    > İşlem kaynağının oluşturulması yaklaşık 5 dakika sürer. Kaynak oluşturulduktan sonra yeniden kullanabilir ve gelecekteki çalıştırmalar için bu bekleme süresini atlayabilirsiniz.
    >
    > İşlem kaynağı, maliyeti kaydetmek için boşta kaldığında 0 düğüm için otomatik olarak ayarlanır.  Bir gecikmeden sonra yeniden kullandığınızda, yeniden ölçeklendirilirken yaklaşık 5 dakikalık bekleme süresi daha yaşayabilirsiniz.
