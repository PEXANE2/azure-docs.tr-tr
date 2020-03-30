---
title: Sizin için önemli olan Azure Danışmanı önerilerini görüntüleyin
description: Gürültüyü azaltmak için Azure Danışmanı önerilerini görüntüleyin ve filtreleyin.
ms.topic: article
ms.date: 04/03/2019
ms.openlocfilehash: 10d7b16864f8e449dc51e870c5ff9f20d8c0dc87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75422364"
---
# <a name="view-azure-advisor-recommendations-that-matter-to-you"></a>Sizin için önemli olan Azure Danışmanı önerilerini görüntüleyin

Azure Danışmanı, Azure dağıtımlarınızı optimize etmenize yardımcı olacak öneriler sunar. Advisor'da, önerilerinizi yalnızca sizin için önemli olan larla daraltmaya yardımcı olan birkaç özelliğe erişebilirsiniz.

## <a name="configure-subscriptions-and-resource-groups"></a>Abonelikleri ve kaynak gruplarını yapılandırma

Danışman, sizin ve kuruluşunuz için önemli olan Abonelikleri ve Kaynak Gruplarını seçmenize izin verir. Yalnızca seçtiğiniz abonelikler ve kaynak grupları için öneriler görürsünüz. Varsayılan olarak, tüm seçilir. Yapılandırma ayarları abonelik veya kaynak grubuna uygulanır, bu nedenle aynı ayarlar söz le abonelik veya kaynak grubuna erişimi olan herkes için de geçerlidir. Azure portalında veya programlı olarak yapılandırma ayarları değiştirilebilir.

Azure portalında değişiklik yapmak için:

1. Azure portalında [Azure Danışmanı'nı](https://aka.ms/azureadvisordashboard) açın.

1. Menüden **Yapılandırma'yı** seçin.

   ![Danışman yapılandırma menüsü](./media/view-recommendations/configuration.png)

1. Danışman önerileri almak için abonelikler veya kaynak grupları için **Ekle** sütunundaki kutuyu işaretleyin. Kutu devre dışı bırakılırsa, bu abonelik veya kaynak grubunda yapılandırma değişikliği yapma izniniz olmayabilir. [Azure Advisor'daki izinler](permissions.md)hakkında daha fazla bilgi edinin.

1. Değişiklik yaptıktan sonra en altta **Uygula'yı** tıklatın.

## <a name="filtering-your-view-in-the-azure-portal"></a>Azure portalında görünümünüzü filtreleme

Yapılandırma ayarları değiştirilene kadar etkin kalır. Tek bir görüntüleme için önerilerin görünümünü sınırlamak istiyorsanız, Danışman panelinin üst kısmında sağlanan açılır iniş leri kullanabilirsiniz. Genel Bakış, Yüksek Kullanılabilirlik, Güvenlik, Performans, Maliyet ve Tüm Öneri panellerinden görmek istediğiniz Abonelikleri, Kaynak Türlerini ve öneri durumunu seçebilirsiniz.

   ![Danışman filtreleme menüsü](./media/view-recommendations/filtering.png)

## <a name="dismissing-and-postponing-recommendations"></a>Önerileri reddetme ve erteleme

Azure Danışmanı, tek bir kaynakla ilgili önerileri reddetmenize veya ertelemenize olanak tanır. Bir öneriyi reddederseniz, el ile etkinleştirmediğiniz sürece bir daha görmezsiniz. Ancak, bir öneriyi ertelemek, önerinin otomatik olarak yeniden etkinleştirildiği bir süre belirtmenize olanak tanır. Erteleme, Azure portalında veya programlı olarak yapılabilir.

### <a name="postpone-a-single-recommendation-in-the-azure-portal"></a>Azure portalında tek bir öneriyi erteleme 

1. Azure portalında [Azure Danışmanı'nı](https://aka.ms/azureadvisordashboard) açın.
1. Önerilerinizi görüntülemek için bir öneri kategorisi seçin
1. Öneriler listesinden bir öneri seçin
1. Ertelemek veya reddetmek istediğiniz öneri için Erteleme veya Kapat'ı seçin

     ![Danışman filtreleme menüsü](./media/view-recommendations/postpone-dismiss.png)

### <a name="postpone-or-dismiss-a-multiple-recommendations-in-the-azure-portal"></a>Azure portalında birden çok öneriyi erteleme veya reddetme

1. Azure portalında [Azure Danışmanı'nı](https://aka.ms/azureadvisordashboard) açın.
1. Önerilerinizi görüntülemek için bir öneri kategorisi seçin.
1. Öneriler listesinden bir öneri seçin.
1. Öneriyi ertelemek veya reddetmek istediğiniz tüm kaynaklar için satırın solundaki onay kutusunu seçin.
1. Tablonun sol üst kısmında **Ertele** veya **Kapat'ı** seçin.

     ![Danışman filtreleme menüsü](./media/view-recommendations/postpone-dismiss-multiple.png)

> [!NOTE]
> Bir öneriyi reddetmek veya ertelemek için katkıda bulunan veya sahip iznine ihtiyacınız var. Azure Advisor'da izinler hakkında daha fazla bilgi edinin.

> [!NOTE]
> Seçim kutuları devre dışı bırakılırsa, öneriler hala yükleniyor olabilir. Lütfen ertelemeye veya reddetmeye çalışmadan önce tüm önerilerin yüklenmesini bekleyin.

### <a name="reactivate-a-postponed-or-dismissed-recommendation"></a>Ertelenen veya reddedilen bir öneriyi yeniden etkinleştirme

Ertelenen veya reddedilen bir öneriyi etkinleştirebilirsiniz. Bu eylem Azure portalında veya programlı olarak yapılabilir. Azure portalında:

1. Azure portalında [Azure Danışmanı'nı](https://aka.ms/azureadvisordashboard) açın.

1. Genel Bakış panelindeki filtreyi Ertelendi olarak **değiştirin.** Danışman daha sonra ertelenmiş veya reddedilen önerileri görüntüler.

    ![Danışman filtreleme menüsü](./media/view-recommendations/activate-postponed.png)

1. **Ertelenen** ve **Reddedilen** önerileri görmek için bir kategori seçin.

1. Öneriler listesinden bir öneri seçin. Bu, bu önerinin ertelendiği veya kapatıldığı kaynakları göstermek için zaten seçilmiş **olan Ertelenmiş & Kapatılmış** sekmesiyle öneriler açılır.

1. Satırın sonunda **etkinleştir'e** tıklayın. Tıklatıldıktan sonra, öneri bu kaynak için etkindir ve bu nedenle bu tablodan kaldırılır. Öneri artık **Etkin** sekmesinde görünür.
 
     ![Danışman filtreleme menüsü](./media/view-recommendations/activate-postponed-2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Danışmanı'nda sizin için önemli olan önerileri nasıl görüntüleyebilirsiniz. Danışman hakkında daha fazla bilgi edinmek için bkz: 

- [Azure Danışmanı nedir?](advisor-overview.md)
- [Danışmanla Başlarken](advisor-get-started.md)
- [Azure Danışmanı'nda izinler](permissions.md)



