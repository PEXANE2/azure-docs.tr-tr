---
title: Azure geçişi 'nde değerlendirme/geçiş aracı ekleme
description: Azure geçişi projesi oluşturmayı ve değerlendirme/geçiş aracının nasıl ekleneceğini açıklar.
ms.topic: how-to
ms.date: 04/16/2020
ms.openlocfilehash: a94e3cc18f46c457d6ed54ef88c62adefb07c5b9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86102540"
---
# <a name="add-an-assessmentmigration-tool-for-the-first-time"></a>Bir değerlendirmeyi/geçiş aracını ilk kez ekleme

Bu makalede, bir [Azure geçiş](./migrate-services-overview.md) projesine ilk kez nasıl değerlendirme veya geçiş aracı ekleneceği açıklanmaktadır.  
Azure geçişi, şirket içi uygulamalarınızı, iş yüklerinizi ve özel/genel bulut VM 'lerini Azure 'a bulmayı, değerlendirmeyi ve geçirmeyi izlemek için bir merkezi Merkez sağlar. Hub, değerlendirme ve geçiş için Azure geçiş araçları ve diğer araçlar ve bağımsız yazılım satıcısı (ISV) [tekliflerini](migrate-services-overview.md#isv-integration) sağlar. 

## <a name="check-permissions-to-create-project"></a>Proje oluşturmak için izinleri denetle

Henüz bir Azure geçişi projesi oluşturmadıysanız, doğru izinlere sahip olup olmadığınızı kontrol edin.

1. Azure portal aboneliğini açın ve **erişim denetimi (IAM)** seçeneğini belirleyin.
2. Erişimi denetle ' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın. Katkıda bulunan veya sahip izinlerinizin olması gerekir.
    - Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz.
    - Abonelik sahibi değilseniz, rolü atamak için sahip ile çalışın.

## <a name="create-a-project-and-add-a-tool"></a>Proje oluşturma ve araç ekleme

Azure aboneliğinde yeni bir Azure geçişi projesi ayarlayın ve bir araç ekleyin.

- Değerlendirme veya geçiş yaptığınız ortamdan toplanan bulma, değerlendirme ve geçiş meta verilerini depolamak için bir Azure geçişi projesi kullanılır. 
- Bir projede keşfedilen varlıkları izleyebilir, değerlendirmeyi ve geçirmeyi organize edebilirsiniz.

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.

    ![Azure geçişi 'ni ayarlama](./media/how-to-add-tool-first-time/azure-migrate-search.png)

3. **Genel Bakış** bölümünde **Sunucuları değerlendir ve geçir**’e tıklayın.
4. **Sunucuları bul, değerlendir ve geçir**altında, **sunucuları değerlendir ve geçir**' e tıklayın.

    ![Sunucuları bulma ve değerlendirme](./media/how-to-add-tool-first-time/assess-migrate.png)

1. **Sunucuları bul, değerlendir ve geçir** bölümünde **Araç ekle**’ye tıklayın.
2. **Projeyi geçir** bölümünde Azure aboneliğinizi seçin ve henüz yapmadıysanız bir kaynak grubu oluşturun.
3. **Proje ayrıntıları**' nda projeyi oluşturmak istediğiniz proje adını ve coğrafi konumu belirtin.  [Kamu](migrate-support-matrix.md#supported-geographies-public-cloud) ve [kamu bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafi lıkları gözden geçirin.

    ![Azure geçişi projesi oluşturma](./media/how-to-add-tool-first-time/migrate-project.png)

    - Proje için belirtilen coğrafya yalnızca şirket içi VM’lerden toplanan meta verileri depolamak için kullanılır. Gerçek geçiş için herhangi bir hedef bölge seçebilirsiniz.
    - Bir coğrafya içindeki belirli bir bölge içinde bir proje dağıtmanız gerekiyorsa, bir proje oluşturmak için aşağıdaki API 'yi kullanın. Abonelik KIMLIĞINI, kaynak grubu adını ve proje adını, konum ile birlikte belirtin. [Kamu](migrate-support-matrix.md#supported-geographies-public-cloud) ve [kamu bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için coğrafi Bölümleri/bölgeleri gözden geçirin.

        `PUT /subscriptions/<subid>/resourceGroups/<rg>/providers/Microsoft.Migrate/MigrateProjects/<mymigrateprojectname>?api-version=2018-09-01-preview "{location: 'centralus', properties: {}}"`   


4. **İleri**' ye tıklayın ve bir değerlendirme veya geçiş aracı ekleyin.

    > [!NOTE]
    > Bir proje oluşturduğunuzda, en az bir değerlendirme veya geçiş aracı eklemeniz gerekir.

5. **Değerlendirme aracı seçin**' de bir değerlendirme aracı ekleyin. Bir değerlendirme aracına ihtiyacınız yoksa **Şimdi bir değerlendirme aracı eklemeyi atla**' yı seçin  >  **Next**. 
2. **Geçiş aracı Seç**bölümünde, gereken şekilde bir geçiş aracı ekleyin. Şu anda bir geçiş aracına ihtiyacınız yoksa **Şimdi bir geçiş aracı eklemeyi atla**' yı seçin  >  **Next**.
3. **Gözden geçir + araçlar Ekle**' de ayarları gözden geçirin ve **araç ekle**' ye tıklayın.

Projeyi oluşturduktan sonra, sunucuların ve iş yüklerinin, veritabanlarının ve Web uygulamalarının değerlendirilmesi ve geçirilmesi için ek araçlar seçebilirsiniz.

## <a name="create-additional-projects"></a>Ek projeler oluşturma

Bazı durumlarda, ek Azure geçişi projeleri oluşturmanız gerekebilir. Örneğin, farklı coğrafi bölgelerde Veri merkezleriniz varsa veya meta verileri farklı bir Coğrafya içinde depolamanız gerekirse. Ek bir projeyi aşağıdaki gibi oluşturun:

1. Geçerli Azure geçişi projesinde **sunucular** veya **veritabanları**' na tıklayın.
2. Sağ üst köşede, geçerli proje adının yanındaki **Değiştir** ' e tıklayın.
3. **Ayarlar**' da, **Yeni bir proje oluşturmak Için buraya tıklayın ' ı**seçin.
4. Yeni bir proje oluşturun ve önceki yordamda açıklandığı gibi yeni bir araç ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure geçişi ile çalışmaya başlama [: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool)veya [Azure geçişi: sunucu geçişi](migrate-services-overview.md#azure-migrate-server-migration-tool).
- Bir ISV aracı veya Movere eklediyseniz, aracı Azure geçişi 'ne bağlamaya hazırlanma [adımlarını gözden geçirin](prepare-isv-movere.md) .
- Ek [değerlendirme](how-to-assess.md) ve [geçiş](how-to-migrate.md) araçları eklemeyi öğrenin. 
