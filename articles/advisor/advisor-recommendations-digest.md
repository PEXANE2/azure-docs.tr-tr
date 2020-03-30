---
title: Azure Danışmanı için öneri özeti
description: Etkin önerileriniz için periyodik özeti alın
ms.topic: article
ms.date: 03/16/2020
ms.author: sagupt
ms.openlocfilehash: 98f1bfd42a486e005cd1e777a83f5b615a7c8304
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502470"
---
# <a name="configure-periodic-summary-for-recommendations"></a>Öneriler için periyodik özeti yapılandırma

Danışman **önerisi özetleri,** farklı kategorilerde etkin önerilerinizi en üst te de sürdürmenin kolay ve proaktif bir yolunu sağlar. Bu özellik, tüm etkin önerilerinizin özeti için farklı kategorilerde periyodik bildirimleri yapılandırma olanağı sağlar. Eylem gruplarını kullanarak e-posta, sms veya diğer bildirimler için istediğiniz kanalı seçebilirsiniz. Bu makalede, Danışman önerileriniz için bir **öneri özetinasıl** ayarlayabileceğiniz gösterilmektedir.


## <a name="setting-up-your-recommendation-digest"></a>Öneri özetinizi ayarlama 

**Öneri özeti** oluşturma deneyimi, özeti yapılandırmanıza yardımcı olur. Yapılandırmalar için aşağıdaki parametreleri seçebilirsiniz:
1. Kategori: Maliyet, yüksek kullanılabilirlik, performans ve operasyonel mükemmellik gibi tavsiye kategorilerimiz vardır. Bu özellik henüz güvenlik önerileri için kullanılamıyor.
2. Özet bildirim sıklığı haftalık, iki haftada bir ve aylık olabilir.
3. Eylem grubu: Varolan bir eylem grubu seçebilir veya yeni bir eylem grubu oluşturabilirsiniz. Eylem grupları hakkında daha fazla bilgi edinmek için [eylem grupları oluşturma ve yönetme'ye](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)bakın.
4. Sindirim dili
5. Öneri özeti adı: Özetleri daha iyi izlemek ve izlemek için kullanıcı dostu bir dize kullanabilirsiniz.

## <a name="steps-to-create-recommendation-digest-in-azure-portal"></a>Azure portalında öneri özeti oluşturma adımları

Öneri özeti oluşturma adımları **şunlardır:**
* **Adım 1:** Azure **portalında, Advisor'a** gidin ve **İzleme** bölümü altında **Öneri özetini** seçin 

   ![Öneri giriş noktasını sindirir](./media/digest-0.png)

* **Adım 2:** Aşağıdaki gibi üst çubuktan **Yeni öneri özetini** seçin:

   ![Öneri özeti oluşturma](./media/digest-5.png)

* **Adım 3:** **Kapsam** bölümünde, özetiniz için **aboneliği** seçin

   ![Öneri özeti girdileri sağlayın](./media/digest-1.png)

* **Adım 4:** **Koşul** bölümünde, **kategori,** **sıklık** ve **dil** gibi yapılandırmaları seçin

   ![Öneri sindirimi giriş koşulları sağlayın](./media/digest-2.png)

* **Adım 5:** Eylem **grubu** bölümünde, özet için **eylem grubunu** seçin. Burada daha fazla bilgi edinebilirsiniz - [Eylem grupları oluşturun ve yönetin](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups)

   ![Öneri özeti giriş eylem grubu sağlama](./media/digest-3.png)

* **Adım 6:** **Özet ayrıntıları**için bu son bölümde, tavsiye özetinize ad ve durum atayabilirsiniz. Kurulumu tamamlamak için **öneri özeti oluşturun'a** basın.
   ![Tam öneri özeti oluşturma](./media/digest-4.png)

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi için bkz:
* [Azure Danışmanına Giriş](advisor-overview.md)
* [Danışman’ı kullanmaya başlama](advisor-get-started.md)
* [Danışman Maliyet önerileri](advisor-cost-recommendations.md)
* [Danışman Performans önerileri](advisor-performance-recommendations.md)
* [Danışman Güvenlik önerileri](advisor-security-recommendations.md)
* [Danışman Operasyonel Mükemmellik önerileri](advisor-operational-excellence-recommendations.md)
* [Danışman REST API](https://docs.microsoft.com/rest/api/advisor/)
