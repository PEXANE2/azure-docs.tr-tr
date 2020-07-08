---
title: Tek seferde birçok çalışma alanında Azure Sentinel olayları ile çalışma | Microsoft Docs
description: Azure Sentinel 'de aynı anda birden fazla çalışma alanındaki olayları görüntüleme.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/20/2020
ms.author: yelevin
ms.openlocfilehash: 448998328ff15b74b0aa0b17e2435a7ff55c54a5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83124202"
---
# <a name="work-with-incidents-in-many-workspaces-at-once"></a>Aynı anda birçok çalışma alanındaki olaylarla çalışma 

 Azure Sentinel 'in yeteneklerini tam olarak gerçekleştirmek için, Microsoft tek çalışma alanı ortamının kullanılmasını önerir. Ancak, bazı durumlarda birkaç çalışma alanı gerektiren bazı kullanım durumları vardır: Örneğin, bir [yönetilen güvenlik hizmeti sağlayıcısı (MSSP)](./multiple-tenants-service-providers.md) ve müşterilerinin birden çok kiracının tamamında. **Birden çok çalışma alanı görünümü** , kiracılar genelinde bile aynı anda birkaç çalışma alanı genelinde güvenlik olaylarını görmenizi ve bunlarla birlikte çalışmanıza olanak tanır. böylece kuruluşunuzun güvenlik yanıt hızını tam olarak görüntüleyebilir ve denetleyebilirsiniz.

## <a name="entering-multiple-workspace-view"></a>Birden çok çalışma alanı görünümü girme

Azure Sentinel 'i açtığınızda, tüm seçili kiracılar ve aboneliklerde erişim haklarınızın olduğu tüm çalışma alanlarının bir listesi sunulur. Her çalışma alanı adının sol tarafında bir onay kutusu bulunur. Tek bir çalışma alanının adına tıklamak sizi bu çalışma alanına getirir. Birden çok çalışma alanı seçmek için ilgili onay kutularına ve ardından sayfanın en üstündeki **çoklu çalışma alanı görünümü** düğmesine tıklayın.

> [!IMPORTANT]
> Birden çok çalışma alanı görünümü şu anda en fazla 10 eşzamanlı görüntülenen çalışma alanını destekliyor. 
> 
> 10 ' dan fazla çalışma alanını denetederseniz bir uyarı iletisi görüntülenir.

Çalışma alanları listesinde, her çalışma alanıyla ilişkili dizin, abonelik, konum ve kaynak grubunu görebileceğinizi unutmayın. Dizin, kiracıya karşılık gelir.

   ![Birden çok çalışma alanı seçin](./media/multiple-workspace-view/workspaces.png)

## <a name="working-with-incidents"></a>Olaylar ile çalışma

**Birden çok çalışma alanı görünümünde**, şimdilik yalnızca **Olaylar** ekranı kullanılabilir. Normal **Olaylar** ekranı gibi birçok şekilde görünür ve çalışır. Birkaç önemli fark vardır, ancak:

   ![Birden çok çalışma alanındaki olayları görüntüleme](./media/multiple-workspace-view/incidents.png)

- Sayfanın üst kısmındaki *Sayaçlar,* *yeni olaylar*, *devam ediyor*vb.-tüm seçili çalışma alanları için sayıları toplu olarak gösterir.

- Tüm seçili çalışma alanları ve dizinlerden (kiracılar) gelen olayları tek bir Birleşik listede görürsünüz. Normal **Olaylar** ekranındaki filtrelere ek olarak, listeyi çalışma alanına ve dizine göre filtreleyebilirsiniz.

- Olayları seçtiğiniz tüm çalışma alanları üzerinde okuma ve yazma izinlerine sahip olmanız gerekir. Bazı çalışma alanlarında yalnızca okuma izinleriniz varsa, bu çalışma alanlarında olaylar ' ı seçerseniz uyarı iletileri görürsünüz. Bu olayları veya seçtiğiniz diğer diğerlerini (diğerleri için izinleriniz olsa bile) Bunlar ile birlikte değiştiremeyeceksiniz.

- Tek bir olay seçer ve **tüm ayrıntıları görüntüle** veya **Araştır**' a tıklarsanız, bu olayın çalışma alanının veri bağlamında ve başka hiç olmayacaktır.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, aynı anda birden çok Azure Sentinel çalışma alanındaki olayları görüntülemeyi ve bunlarla çalışmayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

