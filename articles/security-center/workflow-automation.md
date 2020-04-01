---
title: Azure Güvenlik Merkezi'nde iş akışı otomasyonu | Microsoft Dokümanlar
description: Azure Güvenlik Merkezi'nde iş akışlarını nasıl oluşturup otomatikleştirin
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 5d947cf41e13abdea9a2fd29f8a740d0c101dc6f
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80397904"
---
# <a name="workflow-automation"></a>İş akışı otomasyonu

Her güvenlik programı olay yanıtı için birden çok iş akışı içerir. Bu işlemler, ilgili paydaşları bilgilendirmeyi, bir değişiklik yönetimi süreci başlatmayı ve belirli düzeltme adımlarını uygulamayı içerebilir. Güvenlik uzmanları, bu yordamların olabildiğince çok adımını otomatikleştirmenizi önerir. Otomasyon genel yükü azaltır. Ayrıca, işlem adımlarının hızlı, tutarlı ve önceden tanımlanmış gereksinimlerinize göre yapılmasını sağlayarak güvenliğinizi artırabilir.

Bu makalede, Azure Güvenlik Merkezi'nin iş akışı otomasyonu özelliği açıklanmaktadır. Bu özellik, güvenlik uyarıları ve önerileri logic apps tetikleyebilir. Örneğin, bir uyarı oluştuğunda Güvenlik Merkezi'nin belirli bir kullanıcıya e-posta atmasını isteyebilirsiniz. [Azure Logic Apps'ı](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)kullanarak Mantık Uygulamaları'nı nasıl oluşturabileceğinizi de öğreneceksiniz.

> [!NOTE]
> Kenar çubuğundaki Playbook (Önizleme) görünümünü daha önce kullandıysanız, yeni iş akışı otomasyonu sayfasında genişletilmiş işlevsellikle birlikte aynı özellikleri bulabilirsiniz.


## <a name="requirements"></a>Gereksinimler

* Azure Logic Apps iş akışlarıyla çalışmak için aşağıdaki Logic Apps rollerine/izinlerine sahip olmalısınız:

    * [Logic App Operator](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#logic-app-operator) izinleri gereklidir veya Logic App okuma/tetikleme erişimi (bu rol mantık uygulamaları oluşturamaz veya kaldı; yalnızca varolanları *çalıştıramaz)*

    * [Mantık Uygulaması Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#logic-app-contributor) izinleri Logic App oluşturma ve modifikasyon için gereklidir

* Mantık Uygulaması bağlayıcılarını kullanmak istiyorsanız, ilgili hizmetlerinde oturum açabilmek için ek kimlik bilgilerine ihtiyacınız olabilir (örneğin, Outlook/Teams/Slack örnekleriniz)


## <a name="create-a-logic-app-and-define-when-it-should-automatically-run"></a>Bir Mantık Uygulaması oluşturun ve otomatik olarak ne zaman çalışması gerektiğini tanımlayın 

1. Güvenlik Merkezi'nin kenar çubuğundan **İş Akışı otomasyonu'ni**seçin.

    [![İş akışı otomasyonları listesi](media/workflow-automation/list-of-workflow-automations.png)](media/workflow-automation/list-of-workflow-automations.png#lightbox)

    Bu sayfadan yeni otomasyon kuralları oluşturabilir, varolan kuralları etkinleştirebilir, devre dışı bırakabilir veya silebilirsiniz.  
1. Yeni bir iş akışı tanımlamak için **iş akışı otomasyonu ekle'yi**tıklatın. 

    Yeni otomasyon seçenekleriniz ile birlikte bir bölme görüntülenir. Buraya girebilirsiniz:
    1. Otomasyon için bir ad ve açıklama.
    1. Bu otomatik iş akışını başlatacak tetikleyiciler. Örneğin, "SQL" içeren bir güvenlik uyarısı oluşturulduğunda Mantık Uygulamanızın çalışmasını isteyebilirsiniz.
    1. Tetik koşullarınız karşılandığında çalışacak Mantık Uygulaması. 

        [![İş akışı otomasyonları listesi](media/workflow-automation/add-workflow.png)](media/workflow-automation/add-workflow.png#lightbox)

1. Eylemler bölümünden, Mantık Uygulaması oluşturma işlemini başlatmak için **yeni bir tane oluştur'u** tıklatın.

    Azure Logic Apps'a götürülürsünüz.

    [![Yeni Bir Mantık Uygulaması Oluşturma](media/workflow-automation/logic-apps-create-new.png)](media/workflow-automation/logic-apps-create-new.png#lightbox)

1. Ad, kaynak grubu ve konum girin ve **Oluştur'u**tıklatın.

1. Yeni Mantık Uygulamanızda, güvenlik kategorisinden yerleşik, önceden tanımlanmış şablonlar arasından seçim yapabilirsiniz. Veya bu işlem tetiklendiğinde oluşacak özel bir olay akışı tanımlayabilirsiniz.

    Logic App tasarımcısında Güvenlik Merkezi konektörlerinden aşağıdaki tetikleyiciler desteklenir:

    * **Azure Güvenlik Merkezi Önerisi oluşturulduğunda veya tetiklendiğinde**
    * **Azure Güvenlik Merkezi Uyarısı oluşturulduğunda veya tetiklendiğinde** 
    
    > [!TIP]
    > Tetikleyiciyi yalnızca ilginizi çeken önem düzeylerine sahip uyarılarla ilgili olacak şekilde özelleştirebilirsiniz.
    
    > [!NOTE]
    > "Azure Güvenlik Merkezi uyarısına yanıt tetiklendiğinde" eski tetikleyiciyi kullanıyorsanız, Logic Apps'ınız İş Akışı Otomasyonu özelliği tarafından başlatılacaktır. Bunun yerine, yukarıda belirtilen tetikleyicilerden birini kullanın. 

    [![Örnek Mantık Uygulaması](media/workflow-automation/sample-logic-app.png)](media/workflow-automation/sample-logic-app.png#lightbox)

1. Mantık Uygulamanızı tanımladıktan sonra iş akışı otomasyonu tanım bölmesine ("İş akışı otomasyonu ekle") dönün. Yeni Mantık Uygulamanızın seçilediğinden emin olmak için **Yenile'yi** tıklatın.

    ![Yenile](media/workflow-automation/refresh-the-list-of-logic-apps.png)

1. Mantık Uygulamanızı seçin ve otomasyonu kaydedin. Logic App açılır görülünü yalnızca yukarıda belirtilen güvenlik merkezi bağlayıcılarını destekleyen Logic Apps'ı gösterdiğini unutmayın.


## <a name="manually-trigger-a-logic-app"></a>Bir Mantık Uygulamasını el ile tetikle

Bir güvenlik uyarısını veya [Hızlı Düzeltme düzeltmesi](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#quick-fix-remediation)sunan herhangi bir öneriyi görüntülerken De Logic Apps'ı el ile çalıştırabilirsiniz.

Bir Mantık Uygulamasını el ile çalıştırmak için, Hızlı Düzeltme düzeltmesini destekleyen bir uyarı veya öneri açın ve **Tetikleme Mantığı Uygulaması'nı**tıklatın:

[![Bir Mantık Uygulamasını el ile tetikle](media/workflow-automation/manually-trigger-logic-app.png)](media/workflow-automation/manually-trigger-logic-app.png#lightbox)

## <a name="data-types-schemas"></a>Veri türleri şemaları

Mantık Uygulaması örneğine aktarılan güvenlik uyarıları veya öneriler olaylarının ham olay şemalarını görüntülemek için [İş Akışı otomasyonveri türleri şemalarını](https://aka.ms/ASCAutomationSchemas)ziyaret edin. Bu, Güvenlik Merkezi'nin yukarıda belirtilen yerleşik Mantık Uygulaması konektörlerini kullanmadığınız, ancak bunun yerine Logic App'in genel HTTP konektörünü kullandığınız durumlarda yararlı olabilir - uygun gördüğünüz şekilde el ile ayrıştmak için JSON şemasını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Logic Apps oluşturmayı, Güvenlik Merkezi'nde yürütmelerini otomatikleştirin ve bunları el ile çalıştırmayı öğrendiniz. 

Diğer ilgili materyaller için bkz: 

- [Bir güvenlik yanıtını otomatikleştirmek için iş akışı otomasyonunun nasıl kullanılacağına ilişkin Microsoft Learn modülü](https://docs.microsoft.com/learn/modules/resolve-threats-with-azure-security-center/)
- [Azure Güvenlik Merkezi'nde güvenlik önerileri](security-center-recommendations.md)
- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](security-center-alerts-overview.md)
- [Azure Logic Apps hakkında](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)
- [Logic Apps bağlayıcıları](https://docs.microsoft.com/connectors/)
- [İş akışı otomasyonu veri türleri şemaları](https://aka.ms/ASCAutomationSchemas)
