---
title: Önceden oluşturulmuş şablonları kullanarak mantık uygulaması iş akışlarını daha hızlı oluşturma
description: Azure Logic Apps tarafından sağlanan önceden oluşturulmuş şablonları kullanarak mantık uygulaması iş akışlarını hızla oluşturun
services: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 10/15/2017
ms.openlocfilehash: aac0060527af7b7d880f971e7608be3fa44a2d15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905120"
---
# <a name="create-logic-app-workflows-from-prebuilt-templates"></a>Önceden oluşturulmuş şablonlardan mantıksal uygulama iş akışları oluşturma

İş akışlarını daha hızlı oluşturmaya başlamanızı sağlamak için Logic Apps, yaygın olarak kullanılan desenleri izleyen önceden oluşturulmuş mantık uygulamaları olan şablonlar sağlar. Bu şablonları sağlanan olarak kullanın veya senaryonuza uyacak şekilde bunları edin.

Aşağıda bazı şablon kategorileri verilmiştir:

| Şablon türü | Açıklama | 
| ------------- | ----------- | 
| Kurumsal bulut şablonları | Azure Blob, Dynamics CRM, Salesforce, Box ve kurumsal bulut gereksinimleriniz için diğer bağlayıcıları entegre etmek için. Örneğin, iş müşteri adaylarını düzenlemek veya kurumsal dosya verilerinizi yedeklemek için bu şablonları kullanabilirsiniz. | 
| Kişisel üretkenlik şablonları | Günlük anımsatıcılar ayarlayarak, önemli iş öğelerini yapılacaklar listelerine dönüştürerek ve uzun görevleri tek bir kullanıcı onay adımına indirerek kişisel üretkenliği artırın. | 
| Tüketici bulutu şablonları | Twitter, Slack ve e-posta gibi sosyal medya hizmetlerini entegre etmek için. Sosyal medya pazarlama girişimlerini güçlendirmek için yararlıdır. Bu şablonlar, geleneksel olarak yinelenen görevlerde zamandan tasarruf ederek üretkenliği artıran bulut kopyalama gibi görevleri de içerir. | 
| Kurumsal tümleştirme paketi şablonları | VETER (doğrulama, ayıklama, dönüştürme, zenginleştir, rota) boru hatlarının yapılandırılması, AS2 üzerinden x12 EDI belgesinin alınması ve XML'e dönüştürülmesi ve X12, EDIFACT ve AS2 iletilerinin işlenmesi için. | 
| Protokol desen şablonları | HTTP üzerinden istek yanıtı ve FTP ve SFTP genelinde entegrasyonlar gibi protokol desenleri uygulamak için. Bu şablonları sağlanan olarak kullanın veya karmaşık iletişim kuralı desenleri için bunları geliştirin. | 
||| 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). Bir mantık uygulaması oluşturma hakkında daha fazla bilgi için [bkz.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-logic-apps-from-templates"></a>Şablonlardan mantıksal uygulamalar oluşturma

1. Henüz yapmadıysanız, [Azure portalında](https://portal.azure.com "Azure portalında")oturum açın.

2. Ana Azure menüsünden, > kaynak**Kurumsal Tümleştirme** > Mantığı**Uygulaması** **Oluştur'u**seçin.

   ![Azure portalı, Yeni, Kurumsal Tümleştirme, Mantıksal Uygulama](./media/logic-apps-create-logic-apps-from-templates/azure-portal-create-logic-app.png)

3. Bu görüntünün altındaki tabloda yer alan ayarlarla mantıksal uygulamanızı oluşturun:

   ![Mantıksal uygulama ayrıntılarını sağlayın](./media/logic-apps-create-logic-apps-from-templates/logic-app-settings.png)

   | Ayar | Değer | Açıklama | 
   | ------- | ----- | ----------- | 
   | **Adı** | *sizin-mantık-uygulama adı* | Mantıksal uygulama için benzersiz bir ad girin. | 
   | **Abonelik** | *your-Azure-subscription-name* | Kullanmak istediğiniz Azure aboneliğini seçin. | 
   | **Kaynak grubu** | *Azure-kaynak-grubunuzun-adı* | Bu mantık uygulaması için bir [Azure kaynak grubu](../azure-resource-manager/management/overview.md) oluşturun veya seçin ve bu uygulamayla ilişkili tüm kaynakları düzenleyin. | 
   | **Konum** | *Azure-veri-merkezi-bölgeniz* | Batı ABD gibi mantıksal uygulamanızın dağıtılacağı veri merkezi bölgesini seçin. | 
   | **Log Analytics** | **Kapalı** (varsayılan) veya **A** | [Azure Monitor günlüklerini](../log-analytics/log-analytics-overview.md)kullanarak mantık uygulamanız için [tanılama günlüğe kaydetmeyi](../logic-apps/monitor-logic-apps-log-analytics.md) ayarlayın. Zaten bir Log Analytics çalışma alanınız olması gerekir. | 
   |||| 

4. Hazır olduğunuzda **Panoya sabitle**'yi seçin. Bu şekilde mantıksal uygulamanız otomatik olarak Azure panonuzda görüntülenir ve dağıtımdan sonra açılır. **Oluştur**’u seçin.

   > [!NOTE]
   > Mantıksal uygulamanızı sabitlemek istemezseniz öğreticiye devam edebilmek için dağıtım sonrasında mantıksal uygulamanızı el ile bulup açmanız gerekir.

   Azure mantıksal uygulamanızı dağıttıktan sonra Logic Apps Tasarımcısı açılır ve tanıtım videosu bulunan bir sayfa görüntülenir. 
   Videonun altında sık kullanılan mantıksal uygulama desenlerine ait şablonları bulabilirsiniz. 

5. **Şablonlar'a**giriş videosunu ve ortak tetikleyicileri geride bırakın. Önceden oluşturulmuş bir şablon seçin. Örnek:

   ![Bir mantık uygulaması şablonu seçin](./media/logic-apps-create-logic-apps-from-templates/choose-logic-app-template.png)

   > [!TIP]
   > Mantık uygulamanızı sıfırdan oluşturmak için **Boş Mantık Uygulaması'nı**seçin.

   Önceden oluşturulmuş bir şablon seçtiğinizde, bu şablon hakkında daha fazla bilgi görüntüleyebilirsiniz. 
   Örnek:

   ![Önceden oluşturulmuş bir şablon seçin](./media/logic-apps-create-logic-apps-from-templates/logic-app-choose-prebuilt-template.png)

6. Seçili şablona devam etmek için **bu şablonu kullan'ı**seçin. 

7. Şablondaki bağlayıcılara bağlı olarak, aşağıdaki adımlardan herhangi birini gerçekleştirmeniz istenir:

   * Şablon tarafından başvurulan sistemlere veya hizmetlere kimlik bilgilerinizle oturum açın.

   * Şablon tarafından başvurulan herhangi bir hizmet veya sistem için bağlantılar oluşturun. Bağlantı oluşturmak için bağlantınız için bir ad sağlayın ve gerekirse kullanmak istediğiniz kaynağı seçin. 

   * Bu bağlantıları zaten ayarladıysanız, **Devam et'i**seçin.

   Örnek:

   ![Bağlantı oluşturma](./media/logic-apps-create-logic-apps-from-templates/logic-app-create-connection.png)

   Işiniz bittiğinde, mantık uygulamanız açılır ve Logic Apps Designer'da görünür.

   > [!TIP]
   > Şablon görüntüleyiciye dönmek için, tasarımcı araç çubuğunda **Şablonlar'ı** seçin. Bu eylem kaydedilmemiş değişiklikleri ortadan atar, bu nedenle isteğinizi onaylayan bir uyarı iletisi görüntülenir.

8. Mantık uygulamanızı oluşturmaya devam edin.

   > [!NOTE] 
   > Birçok şablon, önceden doldurulmuş gerekli özelliklere sahip olabilecek bağlayıcıları içerir. Ancak, bazı şablonlar, mantık uygulamasını düzgün bir şekilde dağıtmadan önce değerler sağlamanızı gerektirebilir. Eksik özellik alanlarını tamamlamadan dağıtmaya çalışırsanız, bir hata iletisi alırsınız. 

## <a name="update-logic-apps-with-templates"></a>Mantık uygulamalarını şablonlarla güncelleştirin

1. Azure [portalında](https://portal.azure.com "Azure portalında")mantık uygulamanızı th Logic App Designer'da bulun ve açın.

2. Tasarımcı araç çubuğunda **Şablonlar'ı**seçin. Bu eylem kaydedilmemiş değişiklikleri atar, böylece devam etmek istediğinizi onaylayabilirsiniz bir uyarı iletisi görüntülenir. Onaylamak için **Tamam'ı**seçin. Örnek:

   !["Şablonlar"ı seçin](./media/logic-apps-create-logic-apps-from-templates/logic-app-update-existing-with-template.png)

3. **Şablonlar'a**giriş videosunu ve ortak tetikleyicileri geride bırakın. Önceden oluşturulmuş bir şablon seçin. Örnek:

   ![Bir mantık uygulaması şablonu seçin](./media/logic-apps-create-logic-apps-from-templates/choose-logic-app-template.png)

   Önceden oluşturulmuş bir şablon seçtiğinizde, bu şablon hakkında daha fazla bilgi görüntüleyebilirsiniz. 
   Örnek:

   ![Önceden oluşturulmuş bir şablon seçin](./media/logic-apps-create-logic-apps-from-templates/logic-app-choose-prebuilt-template.png)

4. Seçili şablona devam etmek için **bu şablonu kullan'ı**seçin. 

5. Şablondaki bağlayıcılara bağlı olarak, aşağıdaki adımlardan herhangi birini gerçekleştirmeniz istenir:

   * Şablon tarafından başvurulan sistemlere veya hizmetlere kimlik bilgilerinizle oturum açın.

   * Şablon tarafından başvurulan herhangi bir hizmet veya sistem için bağlantılar oluşturun. Bağlantı oluşturmak için bağlantınız için bir ad sağlayın ve gerekirse kullanmak istediğiniz kaynağı seçin. 

   * Bu bağlantıları zaten ayarladıysanız, **Devam et'i**seçin.

   ![Bağlantı oluşturma](./media/logic-apps-create-logic-apps-from-templates/logic-app-create-connection.png)

   Mantık uygulamanız artık Logic Apps Designer'da açılır ve görünür.

8. Mantık uygulamanızı oluşturmaya devam edin. 

   > [!TIP]
   > Değişikliklerinizi kaydetmediyseniz, çalışmanızı atabilir ve önceki mantık uygulamanıza geri dönebilirsiniz. Tasarımcı araç çubuğunda **At'ı**seçin.

> [!NOTE] 
> Birçok şablon, önceden doldurulmuş gerekli özelliklere sahip olabilecek bağlayıcıları içerir. Ancak, bazı şablonlar, mantık uygulamasını düzgün bir şekilde dağıtmadan önce değerler sağlamanızı gerektirebilir. Eksik özellik alanlarını tamamlamadan dağıtmaya çalışırsanız, bir hata iletisi alırsınız.

## <a name="deploy-logic-apps-built-from-templates"></a>Şablonlardan oluşturulmuş mantık uygulamalarını dağıtma

Şablonda değişiklik yaptıktan sonra, değişikliklerinizi kaydedebilirsiniz. Bu eylem, mantık uygulamanızı da otomatik olarak yayınlar.

Tasarımcı araç çubuğunda **Kaydet**'i seçin.

![Mantık uygulamanızı kaydedin ve yayınlayın](./media/logic-apps-create-logic-apps-from-templates/logic-app-save.png)  

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Örnekler, senaryolar, müşteri hikayeleri ve izgeçitler aracılığıyla mantık uygulamaları oluşturma hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Mantık uygulaması örneklerini, senaryoları ve gözden geçirmeleri gözden geçirme](../logic-apps/logic-apps-examples-and-scenarios.md)