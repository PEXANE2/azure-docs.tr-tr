---
title: Azure DDoS hızlı yanıt
description: Özelleştirilmiş destek için etkin bir saldırı sırasında DDoS uzmanlarının nasıl ekleneceğini öğrenin.
services: ddos-protection
documentationcenter: na
author: yitoh
ms.service: ddos-protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2020
ms.author: yitoh
ms.openlocfilehash: 3655b117b641a6179bd6539aaf3f75af07f21a1a
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94992378"
---
# <a name="azure-ddos-rapid-response"></a>Azure DDoS hızlı yanıt

Etkin erişim sırasında, Azure DDoS koruması standart müşterileri DDoS hızlı yanıt (DRR) takımına erişebilir. Bu, saldırı sırasında saldırı sonrası analizler ve saldırı sırasında saldırı konusunda yardımcı olabilir.

## <a name="prerequisites"></a>Önkoşullar

- Bu öğreticideki adımları tamamlayabilmeniz için önce bir [Azure DDoS standart koruma planı](manage-ddos-protection.md)oluşturmanız gerekir.

## <a name="when-to-engage-drr"></a>DRR 'ye ne zaman katılımını

Yalnızca şu durumlarda DRR 'yi devreye almalısınız: 

- Korumalı kaynağın performansının önemli ölçüde düşürülmüş olduğunu fark ederseniz veya kaynak kullanılabilir değilse, DDoS saldırısı sırasında. Kaynak kullanılabilirliği ve performans sorunlarını algılamak için izleyicileri yapılandırma konusunda adım 2 ' ye bakın.
- Kaynağınızın DDoS saldırısından olduğunu düşündük, ancak DDoS koruma hizmeti saldırıyı etkin bir şekilde karşılamıyor.
- Ağ trafiğinizi önemli ölçüde arttırabileceğiniz bir viral olayı planlıyorsunuz.
- Kritik bir iş etkisi olan saldırılar için.

## <a name="engage-drr-during-an-active-attack"></a>Etkin bir saldırı sırasında DRR 'ye katılın

1. Yeni bir destek isteği oluştururken Azure portal, teknik olarak **sorun türü** ' ni seçin.
2. **DDoS koruması** olarak **hizmet** ' i seçin.
3. Kaynak açılan menüsünde bir kaynak seçin. _DRR 'yi yeniden sağlamak için DDoS koruma standardı tarafından korunan sanal ağa bağlı bir DDoS planı seçmelisiniz._

    ![Kaynak seçin](./media/ddos-rapid-response/choose-resource.png)

4. Bir sonraki **sorun** sayfasında **önem derecesi** ' ne kadar kritik bir etki ve **sorun türü** olarak "saldırı altında" ' ı seçin.

    ![Pönem derecesi ve sorun türü](./media/ddos-rapid-response/severity-and-problem-type.png)

5. Ek ayrıntıları tamamlayıp Destek isteğini gönderebilirsiniz.

DRR Azure Rapid Response destek modelini izler. Hızlı yanıt hakkında daha fazla bilgi için [Destek kapsamı ve yanıt hızı](https://azure.microsoft.com/en-us/support/plans/response/) ' na bakın.

Daha fazla bilgi edinmek için [DDoS koruması standart belgelerini](./ddos-protection-overview.md)okuyun.

## <a name="next-steps"></a>Sonraki adımlar

- [Benzetimler üzerinden test](test-through-simulations.md)yapmayı öğrenin.
- [DDoS koruması telemetrisini görüntülemeyi ve yapılandırmayı](telemetry-monitoring-alerting.md)öğrenin.
- [DDoS saldırı risk azaltma raporlarının ve akış günlüklerinin nasıl yapılandırılacağını](reports-and-flow-logs.md)öğrenin.