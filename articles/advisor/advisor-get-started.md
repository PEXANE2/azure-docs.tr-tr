---
title: Azure Advisor’ı kullanmaya başlama
description: Azure Advisor ile başlayın.
ms.topic: article
ms.date: 02/01/2019
ms.openlocfilehash: 8c2699030b1a6d428ddc2a4db40a66003824cf10
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259636"
---
# <a name="get-started-with-azure-advisor"></a>Azure Advisor’ı kullanmaya başlama

Azure portalı aracılığıyla Danışman'a nasıl erişacağınızı, öneriler almayı ve önerileri nasıl uygulayacağınızı öğrenin.

> [!NOTE]
> Azure Danışmanı, yeni oluşturulan kaynakları bulmak için arka planda otomatik olarak çalışır. Bu kaynaklar hakkında öneriler de sağlamak 24 saat kadar sürebilir.

## <a name="get-recommendations"></a>Öneriler alın

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol **bölmede, Danışman'ı**tıklatın.  Sol bölmede Danışman'ı görmüyorsanız, **Tüm hizmetler'i**tıklatın.  Hizmet menüsü bölmesinde, **İzleme ve Yönetim** **altında, Danışman'ı**tıklatın. Danışman panosu görüntülenir.

   ![Azure portalını kullanarak Azure Danışmanı'na erişin](./media/advisor-get-started/advisor-portal-menu.png) 

1. Danışman panosu, seçilen tüm abonelikler için önerilerinizin özetini görüntüler.  Abonelik filtresi açılan listesini kullanarak önerilerin görüntülenmesini istediğiniz abonelikleri seçebilirsiniz.

1. Belirli bir kategori için öneriler almak için sekmelerden birini tıklatın: **Yüksek Kullanılabilirlik,** **Güvenlik,** **Performans**veya **Maliyet**. 

   ![Azure Danışmanı panosu](./media/advisor-overview/advisor-dashboard.png)

## <a name="get-recommendation-details-and-implement-a-solution"></a>Öneri ayrıntıları alın ve bir çözüm uygulayın

Tavsiye eylemleri ve etkilenen kaynaklar gibi ek ayrıntıları görüntülemek ve önerinin çözümünü uygulamak için Danışman'da bir öneri seçebilirsiniz.  

1. [Azure portalında](https://portal.azure.com)oturum açın ve [ardından Danışman'ı](https://aka.ms/azureadvisordashboard)açın.

1. Bu kategorideki öneriler listesini görüntülemek için bir öneri kategorisi seçin veya tüm önerilerinizi görüntülemek için **Tümü** sekmesini seçin.

1. Ayrıntılı olarak gözden geçirmek istediğiniz bir öneriyi tıklatın.

1. Öneri hakkındaki bilgileri ve önerinin geçerli olduğu kaynakları gözden geçirin.

1. Öneriyi uygulamak için **Önerilen Eylem'i** tıklatın.

## <a name="filter-recommendations"></a>Filtre önerileri

Sizin için en önemli olan şeyi detaylandırmak için önerilere filtre uygulayabilirsiniz.  Abonelik, kaynak türü veya öneri durumuna göre filtreuygulayabilirsiniz.  

1. [Azure portalında](https://portal.azure.com)oturum açın ve [ardından Danışman'ı](https://aka.ms/azureadvisordashboard)açın.

1. Abonelik, kaynak türü veya öneri durumuna göre filtrelemek için Danışman panosundaki açılır bilgileri kullanın.

    ![Danışman arama filtresi ölçütleri](./media/advisor-get-started/advisor-filters.png)

## <a name="postpone-or-dismiss-recommendations"></a>Önerileri erteleme veya reddetme

1. [Azure portalında](https://portal.azure.com)oturum açın ve [ardından Danışman'ı](https://aka.ms/azureadvisordashboard)açın.

1. Ertelemek veya reddetmek istediğiniz öneriye gidin.

1. Öneriyi tıklatın.

1. **Erteley'i**tıklatın. 

1. Erteleme süresi belirtin veya öneriyi asla **reddetmeyi** seçin.

## <a name="exclude-subscriptions-or-resource-groups"></a>Abonelikleri veya kaynak gruplarını hariç tutma

'Test' kaynakları gibi Danışman önerileri almak istemediğiniz kaynak gruplarınız veya abonelikleriniz olabilir.  Danışman'ı yalnızca belirli abonelikler ve kaynak grupları için öneriler oluşturacak şekilde yapılandırabilirsiniz.

> [!NOTE]
> Danışman'dan bir abonelik veya kaynak grubu eklemek veya hariç tutmak için bir abonelik Sahibi olmalısınız.  Bir abonelik veya kaynak grubu için gerekli izinlere sahip değilseniz, kullanıcı arabiriminde ekleme veya hariç tutma seçeneği devre dışı bırakılır.

1. [Azure portalında](https://portal.azure.com)oturum açın ve [ardından Danışman'ı](https://aka.ms/azureadvisordashboard)açın.

1. Eylem çubuğunda **Yapıla'yı** tıklatın.

1. Danışman önerileri almak istemediğiniz aboneliklerin veya kaynak gruplarının denetimini kaldırın.

    ![Danışman kaynakları yapılandırma örneği](./media/advisor-get-started/advisor-configure-resources.png)

1. **Uygula** düğmesini tıklatın.

## <a name="configure-low-usage-vm-recommendation"></a>Düşük kullanımlı VM önerisini yapılandırma

Bu yordam, düşük kullanımlı sanal makine önerisi için ortalama CPU kullanım kuralını yapılandırır.

Danışman, sanal makine kullanımınızı 7 gün boyunca izler ve düşük kullanımlı sanal makineleri tanımlar. CPU kullanımları %5 veya daha az sayılsa ve ağ kullanımları %2'den azsa veya geçerli iş yükü daha küçük bir sanal makine boyutuyla karşılanabiliyorsa, sanal makineler düşük kullanım olarak kabul edilir.

Düşük kullanımlı sanal makineleri tanımlamada daha agresif olmak istiyorsanız, ortalama CPU kullanım kuralını abonelik başına ayarlayabilirsiniz.  CPU kullanım kuralı %5, %10, %15 veya %20 olarak ayarlanabilir.

> [!NOTE]
> Düşük kullanımlı sanal makineleri tanımlamak için ortalama CPU kullanım kuralını ayarlamak için bir abonelik *Sahibi*olmalısınız.  Bir abonelik veya kaynak grubu için gerekli izinlere sahip değilseniz, bu izni ekleme veya hariç tutma seçeneği kullanıcı arabiriminde devre dışı bırakılır. 

1. [Azure portalında](https://portal.azure.com)oturum açın ve [ardından Danışman'ı](https://aka.ms/azureadvisordashboard)açın.

1. Eylem çubuğunda **Yapıla'yı** tıklatın.

1. **Kurallar** sekmesini tıklatın.

1. Ortalama CPU kullanım kuralını ayarlamak istediğiniz abonelikleri seçin ve ardından **Edit'i**tıklatın.

1. İstenilen ortalama CPU kullanım değerini seçin ve **Uygula'yı**tıklatın.

1. Yeni ortalama CPU kullanım kuralını kullanmak için varolan önerilerinizi güncelleştirmek için **önerilerinizi** yenile'yi tıklatın. 

   ![Danışman yapılandırma öneri kuralları örneği](./media/advisor-get-started/advisor-configure-rules.png)

## <a name="download-recommendations"></a>Önerileri indirin

Danışman, önerilerinizin bir özetini indirmenizi sağlar.  Önerilerinizi PDF dosyası veya CSV dosyası olarak indirebilirsiniz.  Önerilerinizi indirmek, öneri verilerinizin üzerine iş arkadaşlarınızla kolayca paylaşımda bulunmanızı veya kendi çözümlemenizi yapmanızı sağlar.

1. [Azure portalında](https://portal.azure.com)oturum açın ve [ardından Danışman'ı](https://aka.ms/azureadvisordashboard)açın.

1. **CSV olarak indir'i** veya eylem çubuğuna **PDF olarak indir'i** tıklatın.

İndirme seçeneği, Danışman panosuna uyguladığınız filtrelere saygı duyar.  Belirli bir öneri kategorisi veya önerisi görüntülerken indirme seçeneğini seçerseniz, indirilen özet yalnızca söz konusu kategori veya öneriye ait bilgileri içerir. 

## <a name="next-steps"></a>Sonraki adımlar

Danışman hakkında daha fazla bilgi edinmek için bkz:

- [Azure Danışmanına Giriş](advisor-overview.md)
- [Danışman Yüksek Kullanılabilirlik önerileri](advisor-high-availability-recommendations.md)
- [Danışman Güvenlik önerileri](advisor-security-recommendations.md)
- [Danışman Performans önerileri](advisor-performance-recommendations.md)
- [Danışman Maliyet önerileri](advisor-performance-recommendations.md)
