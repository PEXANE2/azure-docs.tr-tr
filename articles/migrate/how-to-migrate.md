---
title: Azure geçişi 'ne geçiş araçları ekleme
description: Azure geçişi 'ne geçiş araçları ekleme hakkında bilgi edinin.
author: rayne-wiselman
ms.service: azure-migrate
ms.manager: carmonm
ms.topic: article
ms.date: 11/18/2019
ms.author: raynew
ms.openlocfilehash: 6bcb65912627356215769ccc8380ee1a7404f6f7
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185954"
---
# <a name="add-migration-tools"></a>Geçiş araçları ekleme

Bu makalede, [Azure](migrate-overview.md)geçişi 'nde geçiş araçlarının nasıl ekleneceği açıklanmaktadır.

Azure geçişi, değerlendirme ve Azure 'a geçiş için bir araç merkezi sağlar. Bu, yerel araçları, diğer Azure hizmetleri tarafından sunulan araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir.

Bir geçiş aracı eklemek ve henüz bir Azure geçişi projesi ayarlamadıysanız, bu [makaleye](how-to-add-tool-first-time.md)uyun.



## <a name="selecting-an-isv-tool"></a>ISV aracı seçme

Geçiş için bir [ISV aracı](migrate-services-overview.md#isv-integration) seçerseniz, bir lisans alarak başlayabilir veya ücretsiz deneme için kayıt yaparak ISV ilkesine uygun olarak kaydolabilirsiniz. Her araçta Azure geçişi 'ne bağlanma seçeneği vardır. Aracı dağıtın ve araç çalışma alanını Azure geçişi ile bağlamak için araç yönergelerini ve belgeleri izleyin. 

## <a name="select-a-migration-scenario"></a>Bir geçiş senaryosu seçin

1. Azure geçişi projesinde **Genel Bakış ' a**tıklayın.
2. Kullanmak istediğiniz geçiş senaryosunu seçin:

    - Makineleri ve iş yüklerini Azure 'a geçirmek için, **sunucuları değerlendir ve geçir**' i seçin.
    - Şirket içi SQL makinelerini geçirmek için **veritabanlarını değerlendir ve geçir**' i seçin.
    - Şirket içi Web uygulamalarını geçirmek için **Web uygulamalarını değerlendir ve geçir**' i seçin.
    - Çevrimiçi modda büyük miktarlarda şirket içi verileri Azure 'a geçirmek için **Data Box Sırala**' yı seçin.

    ![Değerlendirme senaryosu](./media/how-to-migrate/assess-scenario.png)

## <a name="select-a-server-migration-tool"></a>Sunucu geçiş aracı seçin

1. **Sunucuları değerlendir ve geçir**' e tıklayın.
2. **Azure geçişi-sunucularında**, henüz geçiş araçları eklemediyseniz geçiş **araçları**' nın altında, geçiş **Aracı eklemek için buraya tıklayın ' ı**seçin. Daha önce geçiş araçları eklediyseniz, **daha fazla geçiş Ekle**' de **Değiştir**' i seçin.

    > [!NOTE]
    > Farklı bir projeye gitmeniz gerekiyorsa, **Azure geçişi-sunucular**' da, **farklı bir geçiş projesinin ayrıntılarını gör**' ün yanındaki **buraya tıklayın**' e tıklayın.

3. **Azure geçişi**' nde, kullanmak istediğiniz geçiş aracını seçin.
    - Azure geçişi sunucu geçişini kullanıyorsanız, doğrudan Azure geçişi projesindeki geçişleri ayarlayabilir ve çalıştırabilirsiniz.
    - Üçüncü taraf bir değerlendirme aracı kullanıyorsanız, ISV için sunulan bağlantıya gidin ve bu geçişi sağladığı yönergelere uygun olarak çalıştırın.

## <a name="select-a-database-migration-tool"></a>Veritabanı geçiş aracı seçin

1. **Veritabanlarını değerlendir ve geçir** ' e tıklayın
2. **Veritabanları**' nda **araç ekle**' ye tıklayın.
3. Araç Ekle > **geçiş aracını seçin**bölümünde, veritabanınızı geçirmek için kullanmak istediğiniz aracı seçin.

## <a name="select-a-web-app-migration-tool"></a>Web uygulaması geçiş aracı seçin

1. **Web uygulamalarını değerlendir ve geçir**' e tıklayın.
2. Azure App Service geçiş aracının bağlantısını izleyin. Geçiş aracını kullanarak şunları yapın:

    - **Uygulamaları çevrimiçi değerlendirin**: Azure App Service Geçiş Yardımcısı kullanarak, uygulamaları çevrimiçi olarak ortak bir URL ile değerlendirebilir ve geçirebilirsiniz.
    - **.Net/php**: iç .net ve PHP uygulamaları için Geçiş Yardımcısı indirebilir ve çalıştırabilirsiniz.

## <a name="order-an-azure-data-box"></a>Azure Data Box sıralama

Büyük miktarlardaki verileri Azure 'a geçirmek için çevrimdışı veri aktarımı için bir Azure DAta Box siparişi verebilirsiniz.

1. **Data Box Sırala**' ya tıklayın.
2. **Azure Data Box seçin**bölümünde aboneliğinizi belirtin. 
3. Aktarım, Azure 'a bir içeri aktarma olacaktır. Veri kaynağını ve verilerin Azure bölgesi hedefini belirtin.

## <a name="next-steps"></a>Sonraki adımlar

[Hyper-V](tutorial-migrate-hyper-v.md) veya [VMware](tutorial-migrate-vmware.md) VM 'Leri Için Azure geçişi sunucu geçişini kullanarak geçiş yapmayı deneyin.
