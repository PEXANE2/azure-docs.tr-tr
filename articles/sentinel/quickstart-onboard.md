---
title: "Hızlı başlangıç: Azure Sentinel 'e ekleme"
description: Bu hızlı başlangıçta, ilk olarak Sentinel 'i etkinleştirerek ve sonra veri kaynaklarını bağlayarak yerleşik Azure Sentinel 'i nasıl yapacağınızı öğrenin.
services: sentinel
author: yelevin
ms.author: yelevin
ms.assetid: d5750b3e-bfbd-4fa0-b888-ebfab7d9c9ae
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: quickstart
ms.date: 04/27/2020
ms.openlocfilehash: 3efd0303b447242c0229bc71720e55c8aa3a2f2d
ms.sourcegitcommit: 37afde27ac137ab2e675b2b0492559287822fded
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88566259"
---
# <a name="quickstart-on-board-azure-sentinel"></a>Hızlı başlangıç: yerleşik Azure Sentinel

Bu hızlı başlangıçta, yerleşik Azure Sentinel 'i nasıl yapacağınızı öğrenin. 

Azure Sentinel 'de, ilk olarak Azure Sentinel 'i etkinleştirmeniz ve sonra veri kaynaklarınızı bağlamanız gerekir. Azure Sentinel, Microsoft çözümleri için çok sayıda bağlayıcıyla sunulan Microsoft tehdit koruması çözümleri, Microsoft 365 kaynakları (Office 365 dahil), Azure AD, Azure ATP, Microsoft Cloud App Security ve daha fazlasını içeren gerçek zamanlı tümleştirme sağlar. Ayrıca, Microsoft dışı çözümler için daha geniş güvenlik ekosistemine yerleşik bağlayıcılar vardır. Veri kaynaklarınızı Azure Sentinel 'e bağlamak için ortak olay biçimi (CEF), syslog veya REST API 'yi de kullanabilirsiniz.  

Veri kaynaklarınızı bağlandıktan sonra, verilerinizi temel alan bir expertly oluşturulan çalışma kitapları galerisinden seçim yapın. Bu çalışma kitapları, gereksinimlerinize kolayca özelleştirilebilir.

>[!IMPORTANT] 
> Azure Sentinel 'i kullanırken oluşan ücretler hakkında bilgi için bkz. [Azure Sentinel fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-sentinel/).
  

## <a name="global-prerequisites"></a>Genel Önkoşullar

- Etkin Azure aboneliği, yoksa, başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- Log Analytics çalışma alanı. [Log Analytics çalışma alanı oluşturmayı](../log-analytics/log-analytics-quick-create-workspace.md)öğrenin. Log Analytics çalışma alanları hakkında daha fazla bilgi için bkz. [Azure Izleyici günlükleri dağıtımınızı tasarlama](../azure-monitor/platform/design-logs-deployment.md).

- Azure Sentinel 'i etkinleştirmek için, Azure Sentinel çalışma alanının bulunduğu abonelik için katkıda bulunan izinlerinizin olması gerekir. 
- Azure Sentinel 'i kullanmak için, çalışma alanının ait olduğu kaynak grubunda katkıda bulunan ya da okuyucu izinlerinizin olması gerekir.
- Belirli veri kaynaklarına bağlanmak için ek izinler gerekebilir.
- Azure Sentinel, ücretli bir hizmettir. Fiyatlandırma bilgileri için bkz. [Azure Sentinel hakkında](https://go.microsoft.com/fwlink/?linkid=2104058).
 
## <a name="enable-azure-sentinel"></a>Azure Sentinel 'i etkinleştirme <a name="enable"></a>

1. Azure Portal’da oturum açın. Azure Sentinel 'in oluşturulduğu aboneliğin seçildiğinden emin olun.

1. **Azure Sentinel**' i arayıp seçin.

   ![search](./media/quickstart-onboard/search-product.png)

1. **Ekle**’yi seçin.

1. Kullanmak istediğiniz çalışma alanını seçin veya yeni bir tane oluşturun. Azure Sentinel 'i birden fazla çalışma alanında çalıştırabilirsiniz, ancak veriler tek bir çalışma alanına yalıtılmış olur.

   ![search](./media/quickstart-onboard/choose-workspace.png)

   >[!NOTE] 
   > - Azure Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanları listede görünmez; Azure Sentinel 'i bunlara yükleyemezsiniz.
   > - Azure Sentinel, Çin ve Almanya (Sovereign) bölgeleri hariç Log Analytics herhangi bir [ga bölgesindeki](https://azure.microsoft.com/global-infrastructure/services/?products=monitor) çalışma alanlarında çalıştırılabilir. Azure Sentinel tarafından oluşturulan (olaylar, yer işaretleri ve uyarı kuralları gibi) veriler, bu çalışma alanlarından birinde bulunan bazı müşteri verilerini içeren Avrupa (Avrupa 'da bulunan çalışma alanları için), Avustralya (Avustralya 'da bulunan çalışma alanları için) veya Doğu ABD (diğer bölgelerde bulunan çalışma alanları için).

   >[!IMPORTANT]
   >
   > - Bir çalışma alanına dağıtıldıktan sonra, Azure Sentinel bu çalışma alanının diğer kaynak gruplarına veya aboneliklerine taşınmasını **desteklememektedir** . 
   >
   >   Çalışma alanını zaten taşıdıysanız, **analiz** altındaki tüm etkin kuralları devre dışı bırakın ve beş dakika sonra yeniden etkinleştirin. Bu durum çoğu durumda etkili olmalıdır, ancak yeniden yinelemek için, bu, sizin sorumluluğunuzdadır ve riski size aittir.

1. **Azure Sentinel Ekle**' yi seçin.
  

## <a name="connect-data-sources"></a>Veri kaynaklarını bağlama

Azure Sentinel, hizmete bağlanarak ve olayları ve günlükleri Azure Sentinel 'e ileterek hizmet ve uygulamalarla bağlantı kurar. Makineler ve sanal makineler için, günlükleri toplayan ve bunları Azure Sentinel 'e ileten Azure Sentinel Aracısı 'nı yükleyebilirsiniz. Güvenlik duvarları ve proxy 'ler için Azure Sentinel bir Linux Syslog sunucusu kullanır. Aracı üzerine yüklenir ve aracı günlük dosyalarını toplayıp Azure Sentinel 'e iletir. 
 
1. **Veri bağlayıcıları**' na tıklayın.
1. Bağlandığınız her veri kaynağı için bir kutucuk vardır.<br>
Örneğin **Azure Active Directory**' ye tıklayın. Bu veri kaynağını bağladığınızda, Azure AD 'deki tüm günlükleri Azure Sentinel 'e akışla alırsınız. Ne tür Günlükler alınacağını seçin-oturum açma günlükleri ve/veya denetim günlükleri. <br>
Azure Sentinel, en altta, her bağlayıcı için hangi çalışma kitaplarını yükleyeceksiniz? böylece verileriniz genelinde ilginç Öngörüler elde edebilirsiniz. <br> Daha fazla bilgi için yükleme yönergelerini izleyin veya [ilgili bağlantı kılavuzuna bakın](connect-data-sources.md) . Veri bağlayıcıları hakkında daha fazla bilgi için bkz. [Microsoft hizmetlerini bağlama](connect-data-sources.md).

Veri kaynaklarınızı bağladıktan sonra Verileriniz Azure Sentinel 'e akışa başlar ve ile çalışmaya başlayabilirsiniz. Günlükleri [yerleşik panolarda](quickstart-get-visibility.md) görüntüleyebilir ve [verileri araştırmak](tutorial-investigate-cases.md)için Log Analytics içinde sorgular oluşturmaya başlayabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, veri kaynaklarını Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- [Ortak olay biçimi gereçlerden](connect-common-event-format.md) Azure Sentinel 'e veri akışı yapın.
