---
title: Azure Geçiş'te bir değerlendirme/geçiş aracı ekleme
description: Azure Geçiş projesi nin nasıl oluşturulup bir değerlendirme/geçiş aracı ekleyeceğiniz açıklanır.
ms.topic: how-to
ms.date: 04/16/2020
ms.openlocfilehash: 48bdea31d17ea1ddf0b983af962dce30b22d8dcf
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537738"
---
# <a name="add-an-assessmentmigration-tool-for-the-first-time"></a>Bir değerlendirmeyi/geçiş aracını ilk kez ekleme

Bu makalede, bir [Azure Geçiş](migrate-overview.md) projesine ilk kez bir değerlendirme veya geçiş aracının nasıl eklenilen açıklanmaktadır.  
Azure Geçiş, şirket içi uygulamalarınızın ve iş yüklerinizin ve özel/genel bulut VM'lerinizin Azure'a keşfini, değerlendirmesini ve geçişini izlemek için merkezi bir hub sağlar. Hub, değerlendirme ve geçiş için Azure Geçiş araçlarının yanı sıra diğer araçları ve bağımsız yazılım satıcısı (ISV) [teklifleri](migrate-services-overview.md#isv-integration) sağlar. 

## <a name="create-a-project-and-add-a-tool"></a>Proje oluşturma ve araç ekleme

Azure aboneliğinde yeni bir Azure Geçir projesi ayarlayın ve bir araç ekleyin.

- Azure Geçiş projesi, değerlendirdiğiniz veya geçirdiğiniz ortamdan toplanan bulma, değerlendirme ve geçiş meta verilerini depolamak için kullanılır. 
- Bir projede keşfedilen varlıkları izleyebilir, değerlendirmeyi ve geçişi düzenleyebilirsiniz.

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.

    ![Azure Geçiş'i ayarlama](./media/how-to-add-tool-first-time/azure-migrate-search.png)

3. **Genel Bakış** bölümünde **Sunucuları değerlendir ve geçir**’e tıklayın.
4. **Sunucuları Keşfedin, değerlendirin ve geçirin,** **Sunucuları Değerlendir'i tıklatın ve sunucuları geçirin.**

    ![Sunucuları keşfedin ve değerlendirin](./media/how-to-add-tool-first-time/assess-migrate.png)

1. **Sunucuları bul, değerlendir ve geçir** bölümünde **Araç ekle**’ye tıklayın.
2. **Projeyi geçir** bölümünde Azure aboneliğinizi seçin ve henüz yapmadıysanız bir kaynak grubu oluşturun.
3. **Proje**Ayrıntıları'nda, proje adını ve projeyi oluşturmak istediğiniz coğrafyayı belirtin.  Kamu ve [hükümet bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafyaları gözden [geçirin.](migrate-support-matrix.md#supported-geographies-public-cloud)

    ![Azure Geçiş projesi oluşturma](./media/how-to-add-tool-first-time/migrate-project.png)

    - Proje için belirtilen coğrafya yalnızca şirket içi VM’lerden toplanan meta verileri depolamak için kullanılır. Gerçek geçiş için herhangi bir hedef bölge seçebilirsiniz.
    - Bir coğrafyada belirli bir bölge içinde bir proje dağıtmanız gerekiyorsa, proje oluşturmak için aşağıdaki API'yi kullanın. Abonelik kimliği, kaynak grubu adı ve proje adını ve konumu belirtin. Kamu ve [devlet bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için coğrafyaları/bölgeleri gözden [geçirin.](migrate-support-matrix.md#supported-geographies-public-cloud)

        `PUT /subscriptions/<subid>/resourceGroups/<rg>/providers/Microsoft.Migrate/MigrateProjects/<mymigrateprojectname>?api-version=2018-09-01-preview "{location: 'centralus', properties: {}}"`   


4. **İleri'yi**tıklatın ve bir değerlendirme veya geçiş aracı ekleyin.

    > [!NOTE]
    > Bir proje oluşturduğunuzda en az bir değerlendirme veya geçiş aracı eklemeniz gerekir.

5. **Değerlendirme aracını seç'te**bir değerlendirme aracı ekleyin. Bir değerlendirme aracına ihtiyacınız yoksa, > şimdi**Sonraki** **için bir değerlendirme aracı ekleyerek Atla'yı**seçin. 
2. **Geçiş aracı seç'te,** gerektiği gibi bir geçiş aracı ekleyin. Şu anda bir geçiş aracına ihtiyacınız yoksa, > Şimdi**Sonraki** **için bir geçiş aracı ekleyerek Atla'yı**seçin.
3. **Gözden Geçir + araç ekle,** ayarları gözden geçir ve araçları **ekle'yi**tıklatın.

Projeyi oluşturduktan sonra, sunucuların ve iş yüklerinin, veritabanlarının ve web uygulamalarının değerlendirilmesi ve geçişi için ek araçlar seçebilirsiniz.

## <a name="create-additional-projects"></a>Ek projeler oluşturma

Bazı durumlarda, ek Azure Geçiş projeleri oluşturmanız gerekebilir. Örneğin, farklı coğrafyalarda veri merkezleriniz varsa veya meta verileri farklı bir coğrafyada depolamanız gerekir. Aşağıdaki gibi ek bir proje oluşturun:

1. Geçerli Azure Geçiş projesinde **Sunucular** veya **Veritabanları'nı**tıklatın.
2. Sağ üst köşede, geçerli proje adının yanındaki **Değiştir'i** tıklatın.
3. **Ayarlar'da**yeni **bir proje oluşturmak için buraya tıklayın'ı**seçin.
4. Yeni bir proje oluşturun ve önceki yordamda açıklandığı gibi yeni bir araç ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Ek [değerlendirme](how-to-assess.md) ve [geçiş](how-to-migrate.md) araçları eklemeyi öğrenin. 
