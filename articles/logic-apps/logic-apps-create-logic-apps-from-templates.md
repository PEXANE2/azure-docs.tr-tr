---
title: Şablonlardan iş akışları oluşturma
description: Azure Logic Apps içindeki Logic App şablonlarını kullanarak iş akışlarını daha hızlı oluşturun
services: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 10/15/2017
ms.openlocfilehash: c7033185ac3a5411568195d788242a674b569183
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74793189"
---
# <a name="create-logic-app-workflows-from-prebuilt-templates"></a>Önceden oluşturulmuş şablonlardan mantıksal uygulama iş akışları oluşturma

İş akışlarını daha hızlı oluşturmaya başlamanızı sağlamak için Logic Apps, yaygın olarak kullanılan desenleri izleyen önceden oluşturulmuş mantıksal uygulamalar olan şablonlar sağlar. Bu şablonları belirtilen şekilde kullanın veya senaryonuza uyacak şekilde düzenleyin.

Bazı Şablon kategorileri şunlardır:

| Şablon türü | Açıklama | 
| ------------- | ----------- | 
| Kurumsal bulut şablonları | Azure blob, Dynamics CRM, Salesforce, Box ve Kurumsal bulut gereksinimleriniz için diğer bağlayıcılar dahil olmak üzere. Örneğin, bu şablonları iş adaylarını düzenlemek veya kurumsal dosya verilerinizi yedeklemek için kullanabilirsiniz. | 
| Kişisel üretkenlik şablonları | Günlük anımsatıcıları ayarlayarak, önemli iş öğelerini Yapılacaklar listelerine açıp uzun görevleri tek bir Kullanıcı onay adımına otomatik hale getirerek kişisel verimliliği artırın. | 
| Tüketici bulutu şablonları | Twitter, bolluk ve e-posta gibi sosyal medya hizmetlerini tümleştirmek için. Sosyal medya pazarlama girişimlerini güçlendirerek faydalıdır. Bu şablonlar, bulut kopyalama gibi görevleri de içerir, bu da geleneksel olarak yinelenen görevlerde zaman tasarrufu yaparak üretkenliği artırır. | 
| Enterprise Integration Pack şablonları | VETER 'ı yapılandırmak için (doğrulama, ayıklama, dönüştürme, zenginleştirme, yönlendirme) işlem hatları, AS2 üzerinde bir x12 EDI belgesi alma, XML 'e dönüştürme ve x12, EDIOLGU ve AS2 iletilerini işleme. | 
| Protokol deseninin şablonları | HTTP üzerinden istek yanıtı ve FTP ve SFTP arasında tümleştirmeler gibi protokol desenleri uygulamak için. Bu şablonları, belirtilen şekilde kullanın veya karmaşık protokol desenleri için oluşturun. | 
||| 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). Mantıksal uygulama oluşturma hakkında daha fazla bilgi için bkz. [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-logic-apps-from-templates"></a>Şablonlardan Logic Apps oluşturma

1. Henüz yapmadıysanız [Azure Portal](https://portal.azure.com "Azure portalı")oturum açın.

2. Azure ana menüsünden **Kaynak oluştur** > **Kurumsal Tümleştirme** > **Mantıksal Uygulama**'yı seçin.

   ![Azure portalı, Yeni, Kurumsal Tümleştirme, Mantıksal Uygulama](./media/logic-apps-create-logic-apps-from-templates/azure-portal-create-logic-app.png)

3. Bu görüntünün altındaki tabloda yer alan ayarlarla mantıksal uygulamanızı oluşturun:

   ![Mantıksal uygulama ayrıntılarını sağlayın](./media/logic-apps-create-logic-apps-from-templates/logic-app-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Adı** | *mantıksal-uygulamanızın-adı* | Mantıksal uygulama için benzersiz bir ad girin. | 
   | **Abonelik** | *Azure-aboneliğinizin-adı* | Kullanmak istediğiniz Azure aboneliğini seçin. | 
   | **Kaynak grubu** | *Azure-kaynak-grubunuzun-adı* | Bu mantıksal uygulama için bir [Azure Kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun veya seçin ve bu uygulamayla ilişkili tüm kaynakları düzenleyin. | 
   | **Konum** | *Azure-veri-merkezi-bölgeniz* | Batı ABD gibi mantıksal uygulamanızın dağıtılacağı veri merkezi bölgesini seçin. | 
   | **Log Analytics** | **Kapalı** (varsayılan) veya **Açık** | [Azure izleyici günlükleri](../log-analytics/log-analytics-overview.md)aracılığıyla mantıksal uygulamanız için [Tanılama günlüğünü](../logic-apps/logic-apps-monitor-your-logic-apps.md#turn-on-diagnostics-logging-for-your-logic-app) açın. Zaten bir Log Analytics çalışma alanına sahip olmanızı gerektirir. | 
   |||| 

4. Hazır olduğunuzda **Panoya sabitle**'yi seçin. Bu şekilde mantıksal uygulamanız otomatik olarak Azure panonuzda görüntülenir ve dağıtımdan sonra açılır. **Oluştur**’u seçin.

   > [!NOTE]
   > Mantıksal uygulamanızı sabitlemek istemezseniz öğreticiye devam edebilmek için dağıtım sonrasında mantıksal uygulamanızı el ile bulup açmanız gerekir.

   Azure mantıksal uygulamanızı dağıttıktan sonra Logic Apps Tasarımcısı açılır ve tanıtım videosu bulunan bir sayfa görüntülenir. 
   Videonun altında sık kullanılan mantıksal uygulama desenlerine ait şablonları bulabilirsiniz. 

5. Tanıtım videosunu ve ortak Tetikleyicileri **şablonlara**göre kaydırın. Önceden oluşturulmuş bir şablon seçin. Örnek:

   ![Mantıksal uygulama şablonu seçin](./media/logic-apps-create-logic-apps-from-templates/choose-logic-app-template.png)

   > [!TIP]
   > Mantıksal uygulamanızı sıfırdan oluşturmak için **boş mantıksal uygulama**' yı seçin.

   Önceden oluşturulmuş bir şablon seçtiğinizde, bu şablon hakkında daha fazla bilgi görüntüleyebilirsiniz. 
   Örnek:

   ![Önceden oluşturulmuş bir şablon seçin](./media/logic-apps-create-logic-apps-from-templates/logic-app-choose-prebuilt-template.png)

6. Seçili şablonla devam etmek için **Bu şablonu kullan**' ı seçin. 

7. Şablondaki bağlayıcılar temelinde, şu adımlardan herhangi birini gerçekleştirmeniz istenir:

   * Şablon tarafından başvurulan sistemler veya hizmetlerle ilgili kimlik bilgilerinizle oturum açın.

   * Şablon tarafından başvurulan tüm hizmet veya sistemler için bağlantı oluşturun. Bir bağlantı oluşturmak için, bağlantınız için bir ad sağlayın ve gerekirse, kullanmak istediğiniz kaynağı seçin. 

   * Bu bağlantıları zaten ayarladıysanız **devam**' ı seçin.

   Örnek:

   ![Bağlantı oluştur](./media/logic-apps-create-logic-apps-from-templates/logic-app-create-connection.png)

   İşiniz bittiğinde mantıksal uygulamanız açılır ve Logic Apps tasarımcısında görünür.

   > [!TIP]
   > Şablon görüntüleyicisine dönmek için tasarımcı araç çubuğunda **Şablonlar** ' ı seçin. Bu eylem kaydedilmemiş değişiklikleri atar, bu nedenle isteğinizi onaylamak için bir uyarı iletisi görünür.

8. Mantıksal uygulamanızı oluşturmaya devam edin.

   > [!NOTE] 
   > Birçok şablon, önceden doldurulmuş gerekli özelliklere sahip olabilecek bağlayıcıları içerir. Ancak, mantıksal uygulamayı düzgün bir şekilde dağıtabilmeniz için bazı şablonlar yine de değer sağlamanızı gerektirebilir. Eksik özellik alanlarını tamamlamadan dağıtmayı denerseniz, bir hata iletisi alırsınız. 

## <a name="update-logic-apps-with-templates"></a>Mantıksal uygulamaları şablonlarla güncelleştirme

1. [Azure Portal](https://portal.azure.com "Azure portalı"), mantıksal uygulama Tasarımcısı ' nda mantıksal uygulamanızı bulun ve açın.

2. Tasarımcı araç çubuğunda **Şablonlar**' ı seçin. Bu eylem kaydedilmemiş değişiklikleri atar, bu nedenle devam etmek istediğinizi doğrulayabilmeniz için bir uyarı iletisi görüntülenir. Onaylamak için **Tamam**' ı seçin. Örnek:

   !["Şablonlar" ı seçin](./media/logic-apps-create-logic-apps-from-templates/logic-app-update-existing-with-template.png)

3. Tanıtım videosunu ve ortak Tetikleyicileri **şablonlara**göre kaydırın. Önceden oluşturulmuş bir şablon seçin. Örnek:

   ![Mantıksal uygulama şablonu seçin](./media/logic-apps-create-logic-apps-from-templates/choose-logic-app-template.png)

   Önceden oluşturulmuş bir şablon seçtiğinizde, bu şablon hakkında daha fazla bilgi görüntüleyebilirsiniz. 
   Örnek:

   ![Önceden oluşturulmuş bir şablon seçin](./media/logic-apps-create-logic-apps-from-templates/logic-app-choose-prebuilt-template.png)

4. Seçili şablonla devam etmek için **Bu şablonu kullan**' ı seçin. 

5. Şablondaki bağlayıcılar temelinde, şu adımlardan herhangi birini gerçekleştirmeniz istenir:

   * Şablon tarafından başvurulan sistemler veya hizmetlerle ilgili kimlik bilgilerinizle oturum açın.

   * Şablon tarafından başvurulan tüm hizmet veya sistemler için bağlantı oluşturun. Bir bağlantı oluşturmak için, bağlantınız için bir ad sağlayın ve gerekirse, kullanmak istediğiniz kaynağı seçin. 

   * Bu bağlantıları zaten ayarladıysanız **devam**' ı seçin.

   ![Bağlantı oluştur](./media/logic-apps-create-logic-apps-from-templates/logic-app-create-connection.png)

   Mantıksal uygulamanız artık açılır ve Logic Apps tasarımcısında görünür.

8. Mantıksal uygulamanızı oluşturmaya devam edin. 

   > [!TIP]
   > Değişikliklerinizi kaydetmediyseniz, çalışmanızı atabilir ve önceki mantıksal uygulamanıza dönebilirsiniz. Tasarımcı araç çubuğunda, **at**' ı seçin.

> [!NOTE] 
> Birçok şablon, önceden doldurulmuş gerekli özelliklere sahip olabilecek bağlayıcıları içerir. Ancak, mantıksal uygulamayı düzgün bir şekilde dağıtabilmeniz için bazı şablonlar yine de değer sağlamanızı gerektirebilir. Eksik özellik alanlarını tamamlamadan dağıtmayı denerseniz, bir hata iletisi alırsınız.

## <a name="deploy-logic-apps-built-from-templates"></a>Şablonlardan oluşturulan Logic Apps 'i dağıtma

Şablonda yaptığınız değişiklikleri yaptıktan sonra değişikliklerinizi kaydedebilirsiniz. Bu eylem ayrıca mantıksal uygulamanızı otomatik olarak yayımlar.

Tasarımcı araç çubuğunda **Kaydet**'i seçin.

![Mantıksal Uygulamanızı kaydetme ve yayımlama](./media/logic-apps-create-logic-apps-from-templates/logic-app-save.png)  

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Örnekler, senaryolar, müşteri hikayeleri ve izlenecek yollar aracılığıyla mantıksal uygulamalar oluşturma hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Mantıksal uygulama örneklerini, senaryoları ve izlenecek yolları gözden geçirin](../logic-apps/logic-apps-examples-and-scenarios.md)