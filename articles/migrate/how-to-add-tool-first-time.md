---
title: Azure Geçiş'te bir değerlendirme/geçiş aracı ekleme
description: Azure Geçiş projesi nin nasıl oluşturulup bir değerlendirme/geçiş aracı ekleyeceğiniz açıklanır.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/19/2019
ms.author: raynew
ms.openlocfilehash: 319d97d96bd054aed90079777e2ff83d0e308e5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74185947"
---
# <a name="add-an-assessmentmigration-tool-for-the-first-time"></a>Bir değerlendirmeyi/geçiş aracını ilk kez ekleme

Bu makalede, bir [Azure Geçiş](migrate-overview.md) projesine ilk kez bir değerlendirme veya geçiş aracının nasıl eklenilen açıklanmaktadır.  
Azure Geçiş, şirket içi uygulamalarınızın ve iş yüklerinizin ve özel/genel bulut VM'lerinizin Azure'a keşfini, değerlendirmesini ve geçişini izlemek için merkezi bir hub sağlar. Hub, değerlendirme ve geçiş için Azure Geçiş araçlarının yanı sıra diğer araçları ve bağımsız yazılım satıcısı (ISV) [teklifleri](migrate-services-overview.md#isv-integration) sağlar. 

## <a name="create-a-project-and-add-a-tool"></a>Proje oluşturma ve araç ekleme

Azure aboneliğinde yeni bir Azure Geçir projesi ayarlayın ve bir araç ekleyin.

- Azure Geçiş projesi, değerlendirdiğiniz veya geçirdiğiniz ortamdan toplanan bulma, değerlendirme ve geçiş meta verilerini depolamak için kullanılır. 
- Bir projede keşfedilen varlıkları izleyebilir, değerlendirme ve geçişi düzenleyebilirsiniz.

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.

    ![Azure Geçiş'i ayarlama](./media/how-to-add-tool-first-time/azure-migrate-search.png)

3. **Genel Bakış** bölümünde **Sunucuları değerlendir ve geçir**’e tıklayın.
4. **Sunucuları Keşfedin, değerlendirin ve geçirin,** **Sunucuları Değerlendir'i tıklatın ve sunucuları geçirin.**

    ![Sunucuları keşfedin ve değerlendirin](./media/how-to-add-tool-first-time/assess-migrate.png)

1. **Sunucuları bul, değerlendir ve geçir** bölümünde **Araç ekle**’ye tıklayın.
2. **Projeyi geçir** bölümünde Azure aboneliğinizi seçin ve henüz yapmadıysanız bir kaynak grubu oluşturun.
3. **Proje**Ayrıntıları'nda, proje adını ve projeyi oluşturmak istediğiniz coğrafyayı belirtin. 

    ![Azure Geçiş projesi oluşturma](./media/how-to-add-tool-first-time/migrate-project.png)

    Bu coğrafyalardan herhangi birinde bir Azure Geçiş projesi oluşturabilirsiniz.

   **Coğrafya** | **Depolama yeri bölgesi**
    --- | ---
    Asya   | Güneydoğu Asya veya Doğu Asya
    Avrupa | Kuzey Avrupa veya Batı Avrupa
    Japonya  | Japonya Doğu veya Japonya Batı
    Birleşik Krallık | İngiltere Güney veya İngiltere Batı
    Amerika Birleşik Devletleri | Orta ABD veya Batı ABD 2
    Kanada | Orta Kanada
    Hindistan  | Hindistan Orta veya Hindistan Güney
    Avustralya | Avustralya Güneydoğu

    Proje için belirtilen coğrafya yalnızca şirket içi VM’lerden toplanan meta verileri depolamak için kullanılır. Gerçek geçiş için herhangi bir hedef bölge seçebilirsiniz.

    Geçiş projesini ve ilişkili kaynaklarını dağıtmak için bir coğrafyaiçinde belirli bir bölge belirtmek istiyorsanız (Aboneliğinizdeki ilke kısıtlamaları Azure kaynaklarının yalnızca belirli bir Azure bölgesine dağıtılmasına izin verebilir), aşağıdaki API'yi kullanarak bir geçiş projesi oluşturun. Abonelik Kimliğini, Kaynak grup adını, proje adını konumla birlikte geçir (Azure Geçiş'in dağıtıldığı tabloda belirtilen Azure bölgelerinden herhangi birini) belirtin.

    `PUT /subscriptions/<subid>/resourceGroups/<rg>/providers/Microsoft.Migrate/MigrateProjects/<mymigrateprojectname>?api-version=2018-09-01-preview "{location: 'centralus', properties: {}}"`   


4. **İleri'yi**tıklatın ve bir değerlendirme veya geçiş aracı ekleyin.

    > [!NOTE]
    > Bir proje oluşturduğunuzda en az bir değerlendirme veya geçiş aracı eklemeniz gerekir.

5. **Değerlendirme aracını seç'te**bir değerlendirme aracı ekleyin. Bir değerlendirme aracına ihtiyacınız yoksa, > şimdi**Sonraki** **için bir değerlendirme aracı ekleyerek Atla'yı**seçin. 
2. **Geçiş aracı seç'te,** gerektiği gibi bir geçiş aracı ekleyin. Şu anda bir geçiş aracına ihtiyacınız yoksa, > Şimdi**Sonraki** **için bir geçiş aracı ekleyerek Atla'yı**seçin.
3. **Gözden Geçir + araç ekle,** ayarları gözden geçirve araçları **ekle'yi**tıklatın.

Projeyi oluşturduktan sonra, sunucuların ve iş yüklerinin, veritabanlarının ve web uygulamalarının değerlendirilmesi ve geçişi için ek araçlar seçebilirsiniz.

## <a name="create-additional-projects"></a>Ek projeler oluşturma

Bazı durumlarda, ek Azure Geçiş projeleri oluşturmanız gerekebilir. Örneğin, farklı coğrafyalarda veri merkezleriniz varsa veya meta verileri farklı bir coğrafyada depolamanız gerekir. Aşağıdaki gibi ek bir proje oluşturun:

1. Geçerli Azure Geçiş projesinde **Sunucular** veya **Veritabanları'nı**tıklatın.
2. Sağ üst köşede, geçerli proje adının yanındaki **Değiştir'i** tıklatın.
3. **Ayarlar'da**yeni **bir proje oluşturmak için buraya tıklayın'ı**seçin.
4. Yeni bir proje oluşturun ve önceki yordamda açıklandığı gibi yeni bir araç ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Ek [değerlendirme](how-to-assess.md) ve [geçiş](how-to-migrate.md) araçları eklemeyi öğrenin. 
