---
title: "Hızlı başlatma: Azure Sentinel'de Yerleşik"
description: Bu Quickstart belgesini izleyerek Azure Sentinel'de nasıl veri topladığınızı öğrenin.
services: sentinel
author: yelevin
ms.author: yelevin
ms.assetid: d5750b3e-bfbd-4fa0-b888-ebfab7d9c9ae
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: quickstart
ms.date: 12/05/2019
ms.openlocfilehash: 11fecd875385d8ba044cbe44e2270eed11d61ce1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77581558"
---
# <a name="quickstart-on-board-azure-sentinel"></a>Hızlı başlangıç: Yerleşik Azure Sentinel

Bu hızlı başlangıçta, Azure Sentinel'de nasıl binin gerektiğini öğrenin. 

Yerleşik Azure Sentinel için önce Azure Sentinel'i etkinleştirmeniz ve ardından veri kaynaklarınızı bağlamanız gerekir. Azure Sentinel, Microsoft çözümleri için kutudan çıkabilen ve Microsoft Tehdit Koruması çözümleri, Office 365, Azure AD, Azure ATP dahil olmak üzere Microsoft 365 kaynakları ve gerçek zamanlı tümleştirme sağlayan bir dizi bağlayıcıyla birlikte gelir Microsoft Cloud App Security ve daha fazlası. Ayrıca, Microsoft dışı çözümler için daha geniş güvenlik ekosistemine yerleşik bağlayıcılar da vardır. Veri kaynaklarınızı Azure Sentinel'e bağlamak için ortak etkinlik biçimini, Syslog'u veya REST-API'yi de kullanabilirsiniz.  

Veri kaynaklarınızı bağladıktan sonra, verilerinize dayalı öngörüleri yüzeye çıkaran ustalıkla oluşturulmuş çalışma kitaplarından oluşan bir galeriden seçim yapın. Bu çalışma kitapları kolayca ihtiyaçlarınıza göre özelleştirilebilir.

>[!IMPORTANT] 
> Azure Sentinel kullanırken oluşan ücretler hakkında daha fazla bilgi için Azure [Sentinel fiyatlandırması'na](https://azure.microsoft.com/pricing/details/azure-sentinel/)bakın.
  

## <a name="global-prerequisites"></a>Küresel ön koşullar

- Etkin Azure Aboneliği, hesabınız yoksa başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- Günlük Analytics çalışma alanı. [Log Analytics çalışma alanı oluşturmayı](../log-analytics/log-analytics-quick-create-workspace.md)öğrenin. Günlük Analizi çalışma alanları hakkında daha fazla bilgi için Azure [Monitör Günlükleri dağıtımınızı tasarlama](../azure-monitor/platform/design-logs-deployment.md)bölümüne bakın.

- Azure Sentinel'i etkinleştirmek için, Azure Sentinel çalışma alanının bulunduğu abonelik için katılımcı izinlerine ihtiyacınız vardır. 
- Azure Sentinel'i kullanmak için, çalışma alanının ait olduğu kaynak grubunda katkıda bulunan veya okuyucu izinleri gerekir.
- Belirli veri kaynaklarını bağlamak için ek izinler gerekebilir.
- Azure Sentinel ücretli bir hizmettir. Fiyatlandırma bilgileri için Azure [Sentinel Hakkında'ya](https://go.microsoft.com/fwlink/?linkid=2104058)bakın.
 
## <a name="enable-azure-sentinel"></a>Azure Sentinel'i etkinleştirme<a name="enable"></a>

1. Azure Portal’da oturum açın. Azure Sentinel'in oluşturulduğu aboneliğin seçildiğinden emin olun.

1. **Azure Sentinel'i**arayın ve seçin.

   ![search](./media/quickstart-onboard/search-product.png)

1. **Ekle'yi**seçin.

1. Kullanmak istediğiniz çalışma alanını seçin veya yeni bir çalışma oluşturun. Azure Sentinel'i birden fazla çalışma alanında çalıştırabilirsiniz, ancak veriler tek bir çalışma alanına yalıtılır.

   ![search](./media/quickstart-onboard/choose-workspace.png)

   >[!NOTE] 
   > - Azure Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanları listede görünmez; Azure Sentinel'i bunlara yükleyemezsiniz.
   > - Azure Sentinel, [Log Analytics'in](https://azure.microsoft.com/global-infrastructure/services/?products=monitor) Çin, Almanya ve Azure Devlet bölgeleri dışındaki herhangi bir GA bölgesinde çalışma alanlarında çalışabilir. Azure Sentinel tarafından oluşturulan veriler (bu çalışma alanlarından kaynaklanan bazı müşteri verilerini içerebilecek olaylar, yer imleri ve uyarı kuralları gibi) Batı Avrupa'da (Avrupa'da bulunan çalışma alanları için) veya Doğu ABD'de (ABD merkezli tüm çalışma alanları için ve Avrupa hariç başka bir bölge).

1. **Azure Sentinel Ekle'yi**seçin.
  

## <a name="connect-data-sources"></a>Veri kaynaklarını bağlama

Azure Sentinel, hizmete bağlanarak ve olayları ve günlükleri Azure Sentinel'e ileterek hizmetlere ve uygulamalara bağlantı oluşturur. Makineler ve sanal makineler için, günlükleri toplayan ve Azure Sentinel'e ileten Azure Sentinel aracısını yükleyebilirsiniz. Azure Sentinel, Güvenlik Duvarları ve yakınlık birimleri için bir Linux Syslog sunucusu kullanır. Aracı üzerine yüklenir ve aracı günlük dosyalarını toplar ve Azure Sentinel'e iletir. 
 
1. **Veri toplama'yı**tıklatın.
2. Bağlanabileceğiniz her veri kaynağı için bir döşeme vardır.<br>
Örneğin, **Azure Etkin Dizini'ni**tıklatın. Bu veri kaynağını bağlarsanız, Azure AD'deki tüm günlükleri Azure Sentinel'e aktarabilirsiniz. Oturum açma günlükleri ve/veya denetim günlükleri gibi ne tür günlükler alacağınızı seçebilirsiniz. <br>
En altta, Azure Sentinel, verileriniz arasında hemen ilginç bilgiler elde edebilmeniz için her bağlayıcı için yüklemeniz gereken çalışma kitapları için öneriler sağlar. <br> Yükleme yönergelerini izleyin veya daha fazla bilgi için [ilgili bağlantı kılavuzuna bakın.](connect-data-sources.md) Veri bağlayıcıları hakkında bilgi için Microsoft [hizmetlerini bağlayın'](connect-data-sources.md)a bakın.

Veri kaynaklarınız bağlandıktan sonra verileriniz Azure Sentinel'e akmaya başlar ve çalışmaya başlamanız için hazırdır. Yerleşik [panolarda](quickstart-get-visibility.md) günlükleri görüntüleyebilir ve [verileri araştırmak](tutorial-investigate-cases.md)için Log Analytics'te sorgu oluşturmaya başlayabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, veri kaynaklarını Azure Sentinel'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- [Ortak Etkinlik Biçimi cihazlarındaki](connect-common-event-format.md) verileri Azure Sentinel'e aktarın.
