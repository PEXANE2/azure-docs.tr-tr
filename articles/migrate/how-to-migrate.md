---
title: Azure Geçiş'te geçiş araçları ekleme
description: Azure Geçiş'te geçiş araçlarını nasıl ekleyeceğinizi öğrenin.
author: rayne-wiselman
ms.service: azure-migrate
ms.manager: carmonm
ms.topic: article
ms.date: 11/18/2019
ms.author: raynew
ms.openlocfilehash: 6bcb65912627356215769ccc8380ee1a7404f6f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74185954"
---
# <a name="add-migration-tools"></a>Geçiş araçları ekleme

Bu makalede, [Azure Geçiş'te](migrate-overview.md)geçiş araçlarının nasıl eklendirilen açıklanmaktadır.

Azure Geçişi, değerlendirme ve Azure'a geçiş için bir araç merkezi sağlar. Yerel araçları, diğer Azure hizmetleri tarafından sağlanan araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir.

Bir geçiş aracı eklemek istiyorsanız ve henüz bir Azure Geçiş projesi ayarlamadıysanız, bu [makaleyi](how-to-add-tool-first-time.md)izleyin.



## <a name="selecting-an-isv-tool"></a>ISV aracı seçme

Geçiş için bir [ISV aracı](migrate-services-overview.md#isv-integration) seçerseniz, ISV ilkesine uygun olarak lisans alarak veya ücretsiz deneme sürümüne kaydolarak başlayabilirsiniz. Her araçta, Azure Geçiş'e bağlanma seçeneği vardır. Aracı dağıtın ve araç çalışma alanını Azure Geçiş'e bağlamak için araç yönergelerini ve belgelerini izleyin. 

## <a name="select-a-migration-scenario"></a>Geçiş senaryosu seçin

1. Azure Geçir projesinde **Genel Bakış'ı**tıklatın.
2. Kullanmak istediğiniz geçiş senaryosunu seçin:

    - Makineleri ve iş yüklerini Azure'a geçirmek için **Sunucuları Değerlendir'i ve geçişini**seçin.
    - Şirket içi SQL makinelerini geçirmek için **Veri tabanlarını değerlendir'i ve geçişini**seçin.
    - Şirket içi web uygulamalarını geçirmek için **Web uygulamalarını değerlendir'i seçin ve geçirin.**
    - Büyük miktarda şirket içi veriyi çevrimdışı modda Azure'a geçirmek için Veri Kutusu Sipariş Ver'i'ni seçin. **Order a Data Box**

    ![Değerlendirme senaryosu](./media/how-to-migrate/assess-scenario.png)

## <a name="select-a-server-migration-tool"></a>Sunucu geçiş aracı nı seçin

1. **Sunucuları Değerlendir ve Geçir'i**tıklatın.
2. **Azure Geçiş - Sunucular**, henüz geçiş araçları eklemediyseniz, Geçiş **araçları**altında , geçiş aracı eklemek için **burayı tıklatın'ı**seçin. Geçiş araçları eklediyseniz, daha **fazla geçiş aracı ekle'de** **Değiştir'i**seçin.

    > [!NOTE]
    > **Azure Geçir - Sunucular'da**farklı bir projeye gitmek istiyorsanız, **farklı bir geçiş projesinin ayrıntılarını görmenin**yanında, **burayı tıklatın.**

3. **Azure**Geçir'de, kullanmak istediğiniz geçiş aracını seçin.
    - Azure Geçir Sunucusu Geçişi kullanıyorsanız, geçişleri doğrudan Azure Geçiş projesinde ayarlayabilir ve çalıştırabilirsiniz.
    - Bir üçüncü taraf değerlendirme aracı kullanıyorsanız, ISV için sağlanan bağlantıya gidin ve geçişi sağladıkları yönergelere uygun olarak çalıştırın.

## <a name="select-a-database-migration-tool"></a>Veritabanı geçiş aracı nı seçin

1. **Veritabanlarını Değerlendir'i ve geçişini** tıklatın
2. **Veritabanlarında**, **araçlar ekle'yi**tıklatın.
3. Geçiş aracı > Ekle **aracını**seçin, veritabanınızı geçirmek için kullanmak istediğiniz aracı seçin.

## <a name="select-a-web-app-migration-tool"></a>Bir web uygulaması geçiş aracı seçin

1. **Web uygulamalarını değerlendir'i tıklatın ve geçirin.**
2. Azure Uygulama Hizmeti için Geçiş aracıbağlantısını izleyin. Geçiş aracını aşağıdakiler için kullanın:

    - **Uygulamaları çevrimiçi değerlendirme**: Azure Uygulama Hizmeti Geçiş Yardımcısı'nı kullanarak uygulamaları herkese açık bir URL ile çevrimiçi olarak değerlendirebilir ve geçirebilirsiniz.
    - **.NET/PHP**: Dahili .NET ve PHP uygulamaları için Geçiş Yardımcısı'nı indirip çalıştırabilirsiniz.

## <a name="order-an-azure-data-box"></a>Azure Veri Kutusu Sipariş Iiçin

Büyük miktarda veriyi Azure'a geçirmek için çevrimdışı veri aktarımı için bir Azure DAta Kutusu sipariş edebilirsiniz.

1. **Veri Kutusu Sipariş I'** ni tıklatın.
2. **Azure Veri Kutunuzu Seçin'de**aboneliğinizi belirtin. 
3. Aktarım, Azure'a bir alma işlemi olacaktır. Veri kaynağını ve veriler için Azure bölge hedefini belirtin.

## <a name="next-steps"></a>Sonraki adımlar

[Hyper-V](tutorial-migrate-hyper-v.md) veya [VMware VM'ler](tutorial-migrate-vmware.md) için Azure Geçiş Sunucusu Geçişi'ni kullanarak bir geçiş deneyin.
