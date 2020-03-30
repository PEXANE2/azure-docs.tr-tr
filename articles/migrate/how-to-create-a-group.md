---
title: Azure Geçir ile değerlendirme için makineleri gruplandırma | Microsoft Dokümanlar
description: Azure Geçir hizmetiyle bir değerlendirme çalıştırmadan önce makineleri nasıl gruplandırmanız gerektiğini açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/17/2019
ms.author: hamusa
ms.openlocfilehash: 13c640d25265b2663520ef7ab203b0b0a33829e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68301693"
---
# <a name="create-a-group-for-assessment"></a>Değerlendirme için bir grup oluşturma

Bu makalede, Azure Geçiş: Sunucu Değerlendirmesi ile değerlendirme için makine gruplarının nasıl oluşturulacak olduğu açıklanmaktadır.

[Azure Geçiş,](migrate-services-overview.md) Azure'a geçiş yapmaya yardımcı olur. Azure Geçişi, şirket içi altyapının, uygulamaların ve verilerin Azure'a keşfini, değerlendirmesini ve geçişini izlemek için merkezi bir hub sağlar. Hub, değerlendirme ve geçiş için Azure araçlarının yanı sıra üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar. 

## <a name="grouping-machines"></a>Gruplandırma makineleri

Azure'a geçiş için uygun olup olmadıklarını değerlendirmek ve Azure boyutlandırması ve maliyet tahminlerini almak için makineleri gruplar halinde toplarsınız. Gruplar oluşturmanın birkaç yolu vardır:

- Birlikte geçirilmesi gereken makinelerin birlikte geçirilmelerini biliyorsanız, grubu Azure Geçiş'te el ile oluşturabilirsiniz.
- Birlikte gruplandırılması gereken makineler den emin değilseniz, gruplar oluşturmak için Azure Geçir'deki bağımlılık görselleştirme işlevini kullanabilirsiniz. 

> [!NOTE]
> Bağımlılık görselleştirme işlevi Azure Kamu'da kullanılamaz.

## <a name="create-a-group-manually"></a>El ile grup oluşturma

Bir [değerlendirme oluşturduğunuz](how-to-create-assessment.md)anda bir grup oluşturabilirsiniz.

Değerlendirme oluşturmanın dışında el ile bir grup oluşturmak istiyorsanız, aşağıdakileri yapın:

1. Azure Geçir projesinde **genel >,** **sunucuları değerlendir'i ve geçişini**tıklatın. **Azure Geçir:Sunucu Değerlendirmesi,** **Gruplar'ı** tıklatın
    - Azure Geçir: Sunucu Değerlendirmesi aracını henüz eklemediyseniz, eklemek için tıklatın. [Daha fazla bilgi edinin](how-to-assess.md).
    - Henüz bir Azure Geçiş projesi oluşturmadıysanız, [daha fazla bilgi edinin.](how-to-add-tool-first-time.md)

    ![Grupları seçin](./media/how-to-create-a-group/select-groups.png)

2. **Grup** simgesini tıklatın.
3. **Oluştur grubunda**bir grup adı belirtin ve **Aygıt adına**makine keşfi için kullandığınız Azure Geçiş cihazını seçin.
1. Makine listesinden, **oluşturmak**> gruba eklemek istediğiniz makineleri seçin.

    ![Grup oluşturma](./media/how-to-create-a-group/create-group.png)

Artık [bir değerlendirme oluştururken](how-to-create-assessment.md)bu grubu kullanabilirsiniz.

## <a name="refine-a-group-with-dependency-mapping"></a>Bağımlılık eşleme ile bir grup ayrıntılandırma

Bağımlılık eşleme, makineler deki bağımlılıkları görselleştirmenize yardımcı olur. Daha yüksek güven düzeylerine sahip makine gruplarını değerlendirmek istediğinizde genellikle bağımlılık eşlemi kullanırsınız.
- Bir değerlendirme çalıştırmadan önce makine bağımlılıklarını çapraz kontrol etmenize yardımcı olur. 
- Ayrıca, geride hiçbir şey kalmamasını sağlayarak ve böylece geçiş sırasında sürpriz kesintileri önleyerek Azure'a geçişinizi etkili bir şekilde planlamanıza da yardımcı olur.
- Birlikte geçiş yapmak için birbirine bağlı sistemler keşfedebilir ve çalışan bir sistemin kullanıcılara hizmet vermeye devam edip etmediğini veya geçiş yerine devre dışı bırakmaya aday olup olmadığını belirleyebilirsiniz.

[Bağımlılık eşlemi zaten ayarladıysanız](how-to-create-group-machine-dependencies.md)ve varolan bir grubu hassaslaştırmak istiyorsanız, aşağıdakileri yapın:

1. **Sunucular** sekmesinde, **Azure Geçir: Sunucu Değerlendirme** döşemesinde **Gruplar'ı**tıklatın.
2. Rafine etmek istediğiniz grubu tıklatın.
    - Bağımlılık eşlemi henüz ayarlamadıysanız, **Bağımlılıklar** sütunu **bir Yükleme Durumu Gerektirir'i** gösterir. Bağımlılıkları görselleştirmek istediğiniz her VM için **yükleme gerektirir'ı**tıklatın. Makine bağımlılıklarını haritalayabilemeden önce her VM'ye birkaç aracı yükleyin. [Daha fazla bilgi edinin](how-to-create-group-machine-dependencies.md).

        ![Bağımlılık eşleme ekleme](./media/how-to-create-a-group/add-dependency-mapping.png)

    - Bağımlılık eşlemi zaten ayarladıysanız, grup sayfasında, grup bağımlılık eşlemi açmak için **bağımlılıkları** görüntüle'yi tıklatın.

3. **Bağımlılıkları Görüntüle'yi**tıklattıktan sonra, grup bağımlılık haritası aşağıdakileri gösterir:

    - Bağımlılık aracılarını yüklü olan gruptaki tüm makinelere gelen (istemciler) ve giden (sunucular) TCP bağlantıları.
    - Bağımlılık aracıları yüklü olmayan bağımlı makineler bağlantı noktası numaralarına göre gruplandırılır.
    - Bağımlılık aracıları yüklü bağımlı makineler ayrı kutular olarak gösterilir.
    - Makinenin içinde çalışan süreçler. İşlemleri görüntülemek için her makine kutusunu genişletin.
    - Makine özellikleri (FQDN, işletim sistemi, MAC adresi dahil). Ayrıntıları görüntülemek için her makine kutusuna tıklayın.

4. Bağımlılıkları seçtiğiniz bir zaman aralığında görüntülemek için, başlangıç ve bitiş tarihlerini veya süreyi belirterek zaman aralığını (varsayılan olarak bir saat) değiştirin.

    > [!NOTE]
    > Zaman aralığı bir saate kadar olabilir. Daha uzun bir kapsama alanına ihtiyacınız varsa, bağımlı verileri daha uzun bir süre [sorgulamak](how-to-create-group-machine-dependencies.md) için Azure Monitor'u kullanın.

5. Gruba eklemek veya gruptan kaldırmak istediğiniz bağımlılıkları belirledikten sonra grubu değiştirebilirsiniz. Makineleri gruptan eklemek veya kaldırmak için Ctrl+Click'i kullanın.

    - Yalnızca keşfedilmiş makineleri ekleyebilirsiniz.
    - Makinelerin eklenmesi ve çıkarılması, bir grup için geçmiş değerlendirmeleri geçersiz kılmalıdır.
    - Grubu değiştirdiğinizde isteğe bağlı olarak yeni bir değerlendirme oluşturabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Yüksek güven grupları oluşturmak için [bağımlılık eşlemeyi](how-to-create-group-machine-dependencies.md) nasıl ayarlayıp kullanacağınızı öğrenin.

