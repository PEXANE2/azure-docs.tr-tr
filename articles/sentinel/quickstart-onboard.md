---
title: Azure Sentinel önizlemesine ekleme | Microsoft Docs
description: Azure Sentinel 'de verileri nasıl toplayacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d5750b3e-bfbd-4fa0-b888-ebfab7d9c9ae
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2019
ms.author: rkarlin
ms.openlocfilehash: 0c37d6167012af46204cbca29397f5d226b9649b
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69611891"
---
# <a name="on-board-azure-sentinel-preview"></a>Yerleşik Azure Sentinel önizlemesi

> [!IMPORTANT]
> Azure Sentinel Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu hızlı başlangıçta, yerleşik Azure Sentinel 'i nasıl yapacağınızı öğreneceksiniz. 

Azure Sentinel 'de, ilk olarak Azure Sentinel 'i etkinleştirmeniz ve sonra veri kaynaklarınızı bağlamanız gerekir. Azure Sentinel, Microsoft çözümleri için çok sayıda bağlayıcı sunar ve Microsoft tehdit koruması çözümleri Microsoft 365, Office 365, Azure AD, Azure ATP ve dahil gerçek zamanlı tümleştirme sağlar. Microsoft Cloud App Security ve daha fazlası. Ayrıca, Microsoft dışı çözümler için daha geniş güvenlik ekosistemine yerleşik bağlayıcılar vardır. Veri kaynaklarınızı Azure Sentinel 'e bağlamak için ortak olay biçimi, syslog veya REST API 'sini de kullanabilirsiniz.  

Veri kaynaklarınızı bağlandıktan sonra, verilerinizi temel alan Öngörüler sunan expertly oluşturulan panolar galerisinden seçim yapın. Bu panolar, gereksinimlerinize kolayca kolayca özelleştirilebilir.


## <a name="global-prerequisites"></a>Genel Önkoşullar

- Etkin Azure aboneliği, yoksa, başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- Log Analytics çalışma alanı. [Log Analytics çalışma alanı oluşturmayı](../log-analytics/log-analytics-quick-create-workspace.md) öğrenin

-  Azure Sentinel 'i etkinleştirmek için, Azure Sentinel çalışma alanının bulunduğu abonelik için katkıda bulunan izinlerinizin olması gerekir. 
- Azure Sentinel 'i kullanmak için, çalışma alanının ait olduğu kaynak grubunda katkıda bulunan ya da okuyucu izinlerinizin olması gerekir
- Belirli veri kaynaklarına bağlanmak için ek izinler gerekebilir
 
## Azure Sentinel 'i etkinleştirme<a name="enable"></a>

1. Azure portal gidin.
2. Azure Sentinel 'in oluşturulduğu aboneliğin seçili olduğundan emin olun. 
3. Azure Sentinel 'i arayın. 
   ![aramanız](./media/quickstart-onboard/search-product.png)

1. **+ Ekle**' ye tıklayın.
1. Kullanmak istediğiniz çalışma alanını seçin veya yeni bir tane oluşturun. Azure Sentinel 'i birden fazla çalışma alanında çalıştırabilirsiniz, ancak veriler tek bir çalışma alanına yalıtılmış olur.

   ![search](./media/quickstart-onboard/choose-workspace.png)

   >[!NOTE] 
   > - Azure Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanları listede görünmez; Azure Sentinel 'i bunlara yükleyemezsiniz.
   > - Azure Sentinel, aşağıdaki bölgelerde dağıtılan çalışma alanlarında çalıştırılabilir:  Avustralya Güneydoğu, Kanada Orta, Orta Hindistan, Doğu ABD, Doğu ABD 2 EUAP (Canary), Japonya Doğu, Güneydoğu Asya, UK Güney, Batı Avrupa, Batı ABD 2.

6. **Azure Sentinel Ekle**' ye tıklayın.
  

## <a name="connect-data-sources"></a>Veri kaynaklarını bağlama

Azure Sentinel, hizmete bağlanarak ve olayları ve günlükleri Azure Sentinel 'e ileterek hizmet ve uygulamalarla bağlantı kurar. Makineler ve sanal makineler için, günlükleri toplayan ve bunları Azure Sentinel 'e ileten Azure Sentinel Aracısı 'nı yükleyebilirsiniz. Güvenlik duvarları ve proxy 'ler için Azure Sentinel bir Linux Syslog sunucusu kullanır. Aracı üzerine yüklenir ve aracı günlük dosyalarını toplayıp Azure Sentinel 'e iletir. 
 
1. **Veri koleksiyonu**' na tıklayın.
2. Bağlandığınız her veri kaynağı için bir kutucuk vardır.<br>
Örneğin **Azure Active Directory**' ye tıklayın. Bu veri kaynağını bağladığınızda, Azure AD 'deki tüm günlükleri Azure Sentinel 'e akışla alırsınız. Ne tür Günlükler alınacağını seçin-oturum açma günlükleri ve/veya denetim günlükleri. <br>
Azure Sentinel, en altta, her bağlayıcı için hangi panoları kurmanız gerektiği konusunda öneriler sağlar, böylece verileriniz genelinde ilginç Öngörüler elde edebilirsiniz. <br> Daha fazla bilgi için yükleme yönergelerini izleyin veya [ilgili bağlantı kılavuzuna bakın](connect-data-sources.md) . Veri bağlayıcıları hakkında daha fazla bilgi için bkz. [Microsoft hizmetlerini bağlama](connect-data-sources.md).

Veri kaynaklarınızı bağladıktan sonra Verileriniz Azure Sentinel 'e akışa başlar ve ile çalışmaya başlayabilirsiniz. Günlükleri [yerleşik panolarda](quickstart-get-visibility.md) görüntüleyebilir ve [verileri araştırmak](tutorial-investigate-cases.md)için Log Analytics içinde sorgular oluşturmaya başlayabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, veri kaynaklarını Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
- [Yaygın hata biçimi](connect-common-event-format.md) gereçlerden Azure Sentinel 'e veri akışı.
