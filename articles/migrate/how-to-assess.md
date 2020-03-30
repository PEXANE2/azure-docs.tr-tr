---
title: Azure Geçiş'te değerlendirme araçları ekleme
description: Azure Geçiş'te değerlendirme araçları eklemeyi öğrenin.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.manager: carmonm
ms.date: 11/19/2019
ms.author: raynew
ms.openlocfilehash: 64af78abd8f82b41d4a03fbb56c96e3038cef5a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74185904"
---
# <a name="add-assessment-tools"></a>Değerlendirme araçları ekleme

Bu makalede, [Azure Geçiş'te](migrate-overview.md)değerlendirme araçlarının nasıl eklenilen açıklanmaktadır.

Azure Geçişi, değerlendirme ve Azure'a geçiş için bir araç merkezi sağlar. Azure Geçiş araçlarının yanı sıra diğer araçları ve bağımsız yazılım satıcısı (ISV) tekliflerini içerir.

Bir değerlendirme aracı eklemek istiyorsanız ve henüz bir Azure Geçiş projeniz yoksa, bu [makaleyi](how-to-add-tool-first-time.md)izleyin.

## <a name="select-a-tool"></a>Bir araç seçin

Değerlendirme için Azure'a aktarma yapmayan bir araç seçerseniz, araç ilkesine uygun olarak lisans alarak veya ücretsiz deneme sürümüne kaydolarak başlayın. Araçlar, Azure Geçiş'e bağlanma seçeneğine sahiptir. Aracı Azure Geçiş'e bağlamak için yönergeleri ve belgeleri izleyin. Araçlar hakkında [daha fazla bilgi edinin.](migrate-services-overview.md)


## <a name="select-an-assessment-scenario"></a>Değerlendirme senaryosu seçin

1. Azure Geçir projesinde **Genel Bakış'ı**tıklatın.
2. Kullanmak istediğiniz değerlendirme senaryosunu seçin:

    - Azure'a geçiş için makineleri ve iş yüklerini keşfetmek ve değerlendirmek için **Sunucuları Değerlendir'i ve geçişini**seçin.
    - Şirket içi SQL makinelerini değerlendirmek için **veri tabanlarını değerlendir'i ve geçişini**seçin.
    - Şirket içi web uygulamalarını değerlendirmek için **Web uygulamalarını değerlendir'i seçin ve web uygulamalarını geçirin.**

    ![Değerlendirme senaryosu](./media/how-to-assess/assess-scenario.png)

## <a name="select-a-server-assessment-tool"></a>Sunucu değerlendirme aracı nı seçin 

1. **Sunucuları Değerlendir ve Geçir'i**tıklatın.
2. **Azure Geçir - Sunucular**, Değerlendirme araçları altında bir değerlendirme aracı eklemediyseniz, değerlendirme **aracı** **eklemek için burayı tıklatın'ı**seçin. Daha önce değerlendirme araçları eklediyseniz, **daha fazla değerlendirme aracı ekle'de** **Değiştir'i**seçin.

    > [!NOTE]
    > **Azure Geçir - Sunucular'da**farklı bir projeye gitmek istiyorsanız, **farklı bir geçiş projesinin ayrıntılarını görmenin**yanında, **burayı tıklatın.**

3. **Azure**Geçir'de, kullanmak istediğiniz değerlendirme aracını seçin.

    - Azure Geçir Sunucusu Değerlendirmesi kullanıyorsanız, değerlendirmeleri doğrudan Azure Geçiş projesinde ayarlayabilir, çalıştırabilir ve görüntüleyebilirsiniz.
    - Farklı bir değerlendirme aracı kullanıyorsanız, siteleri için sağlanan bağlantıya gidin ve değerlendirmeyi sağladıkları yönergelere uygun olarak çalıştırın.


## <a name="select-a-database-assessment-tool"></a>Veritabanı değerlendirme aracı nı seçme

1. **Veritabanlarını Değerlendir'i ve geçişini** tıklatın
2. **Veritabanlarında**, **araçlar ekle'yi**tıklatın.
3. Bir araç > **Select değerlendirme aracında,** veritabanınızı değerlendirmek için kullanmak istediğiniz aracı seçin.

## <a name="select-a-web-app-assessment-tool"></a>Bir web uygulaması değerlendirme aracı seçin

1. **Web uygulamalarını değerlendir'i tıklatın ve geçirin.**
2. Azure Uygulama Hizmeti için Geçiş aracıbağlantısını izleyin. Geçiş aracını aşağıdakiler için kullanın:

    - **Uygulamaları çevrimiçi değerlendirme**: Azure Uygulama Hizmeti Geçiş Yardımcısı'nı kullanarak uygulamaları herkese açık bir URL ile çevrimiçi olarak değerlendirebilirsiniz.
    - **.NET/PHP**: Dahili .NET ve PHP uygulamaları için Geçiş Yardımcısı'nı indirip çalıştırabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

[VMware VM'ler,](tutorial-prepare-vmware.md) [Hyper-V](tutorial-prepare-hyper-v.md)veya [fiziksel sunucular](tutorial-prepare-physical.md) için Azure Geçir Sunucu Değerlendirmesi'ni kullanarak bir değerlendirme yi deneyin
