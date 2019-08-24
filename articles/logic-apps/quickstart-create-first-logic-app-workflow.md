---
title: Hızlı başlangıç-Azure Logic Apps ilk iş akışınızı oluşturun ve otomatikleştirin
description: Azure Logic Apps kullanarak görevleri, işlemleri ve iş akışlarını otomatikleştiren ilk mantıksal uygulamanızı oluşturun. Bulut Hizmetleri & sistemleriniz için sistem tümleştirme ve kuruluş uygulaması Tümleştirme (EAı) çözümleri için mantıksal uygulamalar oluşturun.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: quickstart
ms.custom: mvc
ms.date: 08/20/2019
ms.openlocfilehash: f0d63b9e01b7e39b03e6a3b1e310b5fae0f8d25f
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69982859"
---
# <a name="quickstart-create-your-first-automated-workflow-with-azure-logic-apps---azure-portal"></a>Hızlı Başlangıç: Azure Logic Apps Azure portal ilk otomatikleştirilmiş iş akışınızı oluşturma

Bu hızlı başlangıçta, [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ile ilk otomatik iş akışınızı nasıl oluşturabileceğiniz konusuna giriş yapılır. Bu makalede, bir web sitesinin RSS akışında yeni öğe olup olmadığını düzenli olarak denetleyen bir mantıksal uygulama oluşturursunuz. Yeni öğeler varsa, mantıksal uygulama her öğe için bir e-posta gönderir. İşlemi tamamladığınızda, mantıksal uygulamanız bu yüksek düzeyli iş akışı gibi görünür:

![Genel bakış - mantıksal uygulama örneği](./media/quickstart-create-first-logic-app-workflow/overview.png)

Bu hızlı başlangıcı izleyebilmeniz için Logic Apps tarafından desteklenen Office 365 Outlook, Outlook.com veya Gmail gibi bir sağlayıcıdan e-posta hesabınız olmalıdır. Diğer sağlayıcılar için [buradaki bağlayıcı listesini inceleyin](https://docs.microsoft.com/connectors/). Bu mantıksal uygulama bir Office 365 Outlook hesabı kullanır. Başka bir e-posta hesabı kullanıyorsanız genel adımlar aynıdır, ancak kullanıcı arabirimi biraz farklı olabilir.

Ayrıca, Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-your-logic-app"></a>Mantıksal uygulamanızı oluşturma

1. Ana Azure menüsünden **kaynak** > **tümleştirme** > **mantıksal uygulaması**oluştur ' u seçin.

   ![Mantıksal uygulama oluşturma](./media/quickstart-create-first-logic-app-workflow/create-logic-app.png)

1. **Mantıksal uygulama oluştur** bölümünde, mantıksal uygulamanızın ayrıntılarını burada gösterildiği gibi sağlayın. İşiniz bittiğinde **Oluştur**' u seçin.

   ![Mantıksal uygulama ayrıntılarını sağlayın](./media/quickstart-create-first-logic-app-workflow/create-logic-app-settings.png)

   | Özellik | Value | Açıklama |
   |----------|-------|-------------|
   | **Name** | <*Logic-App-adı*> | Mantıksal uygulamanızın adı, yalnızca harf, sayı`-`, kısa çizgi (), alt çizgi (`_`), parantez (`(`, `)`) ve nokta (`.`) içerebilir. Bu örnek "My-First-Logic-App" kullanır. |
   | **Abonelik** | <*Azure-subscription-name*> | Azure abonelik adınız |
   | **Kaynak grubu** | <*Azure-Resource-Group-Name*> | İlgili kaynakları düzenlemek için kullanılan [Azure Kaynak grubunun](../azure-resource-manager/resource-group-overview.md) adı. Bu örnek "My-First-LA-RG" kullanır. |
   | **Location** | <*Azure-bölge*> | Mantıksal uygulama bilgilerinizin depolanacağı bölge. Bu örnek, "Batı ABD" kullanır. |
   | **Log Analytics** | Kapalı | Tanılama günlüğüne kaydetme ayarını **Kapalı** durumda bırakın. |
   ||||

1. Azure, uygulamanızı dağıttıktan sonra Azure araç çubuğunda **Bildirimler** > ' i seçerek dağıtılan mantıksal uygulamanız için**kaynağa gidin** .

   ![Kaynağa git](./media/quickstart-create-first-logic-app-workflow/go-to-logic-app.png)

   Ya da, arama kutusuna adı yazarak mantıksal uygulamanızı bulabilir ve seçebilirsiniz.

   Logic Apps Tasarımcısı açılır ve bir tanıtım videosu ile sık kullanılan tetikleyicilerin bulunduğu bir sayfa görüntülenir. **Şablonlar** altında **Boş Mantıksal Uygulama**'yı seçin.

   ![Boş mantıksal uygulama şablonu seçin](./media/quickstart-create-first-logic-app-workflow/choose-logic-app-template.png)

Sonra, yeni bir RSS akışı öğesi göründüğünde tetiklenen bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin. Her mantıksal uygulama, belirli bir olay gerçekleştiğinde ya da belirli bir koşul karşılandığında tetiklenen bir tetikleyiciyle başlamalıdır. Tetikleyici her etkinleştirildiğinde Logic Apps altyapısı iş akışınızı başlatan ve çalıştıran bir mantıksal uygulama örneği oluşturur.

<a name="add-rss-trigger"></a>

## <a name="check-rss-feed-with-a-trigger"></a>Bir tetikleyici ile RSS akışı denetleme

1. Mantıksal uygulama Tasarımcısı ' nda, arama kutusunda, **Tümü**' nü seçin.

1. Arama kutusuna "RSS" yazın. Tetikleyiciler listesinde, bu tetikleyiciyi seçin: **Bir akış öğesi yayımlandığında-RSS**

   ![Tetikleyiciyi seçin: "RSS-bir akış öğesi yayımlandığında"](./media/quickstart-create-first-logic-app-workflow/add-trigger-rss.png)

1. Aşağıda gösterildiği gibi Tetikleyiciniz için bu bilgileri sağlayın ve burada açıklanmıştır:

   ![Tetikleyicinin RSS akışı, sıklık ve aralık ayarını yapma](./media/quickstart-create-first-logic-app-workflow/add-trigger-rss-settings.png)

   | Özellik | Value | Açıklama |
   |----------|-------|-------------|
   | **RSS akışının URL'si** | ```http://feeds.reuters.com/reuters/topNews``` | İzlemek istediğiniz RSS akışının bağlantısı |
   | **Aralık** | 1\. | Denetimler arasında beklenecek aralık sayısı |
   | **Sıklık** | Dakika | Denetimler arası her aralık için zaman birimi  |
   ||||

   Aralık ve sıklık özelliği, mantıksal uygulamanızın tetikleyicisi için zamanlamayı tanımlar. Bu mantıksal uygulama, akışı dakika başı denetler.

1. Tetikleyici ayrıntılarını şimdilik gizlemek için tetikleyicinin başlık çubuğunun içine tıklayın.

   ![Ayrıntıları gizlemek için şekli daraltın](./media/quickstart-create-first-logic-app-workflow/collapse-trigger-shape.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

Mantıksal uygulamanız çalışıyor ancak RSS akışını kontrol etme dışında bir işlem gerçekleştirmiyor. Şimdi, tetikleyici etkinleştirildiğinde gerçekleştirilecek bir eylem ekleyin.

## <a name="send-email-with-an-action"></a>Bir eylemle e-posta gönderme

Şimdi, RSS akışında yeni bir öğe göründüğünde e-posta gönderen bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin.

1. **Bir akış öğesi yayımlandığında** tetikleyicisi bölümünde **yeni adım**' ı seçin.

   ![Eylem ekleme](./media/quickstart-create-first-logic-app-workflow/add-new-action.png)

1. **Eylem Seç** ' in altında, arama kutusu ' nun altında **Tümü**' nü seçin.

1. Arama kutusuna "e-posta gönder" yazın. Eylemler listesinden istediğiniz e-posta sağlayıcısıyla "e-posta gönder" eylemini seçin.

   ![Şu eylemi seçin: "Office 365 Outlook-e-posta gönder"](./media/quickstart-create-first-logic-app-workflow/add-action-send-email.png)

   Eylem listesini belirli bir uygulama veya hizmeti içerek şekilde filtrelemek için önce uygulama ya da hizmeti seçebilirsiniz:

   * Azure iş veya okul hesapları için Office 365 Outlook girişini seçin.
   * Kişisel Microsoft hesapları için Outlook.com girişini seçin.

1. Kimlik bilgileriniz istenirse, Logic Apps’in e-posta hesabınıza yönelik bir bağlantı oluşturması için e-posta hesabınızda oturum açın.

1. **E-posta gönderme** eyleminde, e-postanızın içermesini istediğiniz verileri belirtin.

   1. **Alıcı** kutusuna alıcının e-posta adresini girin. Test için kendi e-posta adresinizi kullanabilirsiniz.

      Görüntülenen **Dinamik içerik ekle** listesini şimdilik yoksayın. Bazı düzenleme kutularının içine tıkladığınızda bu liste görünür ve önceki adımdan iş akışınıza girdi olarak ekleyebileceğiniz tüm kullanılabilir parametreleri gösterir.

   1. **Konu** kutusuna, sonuna bir boşluk koyarak şu metni girin: ```New RSS item:```

      ![E-posta konusunu girin](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-subject.png)

   1. RSS öğesinin başlığını eklemek için **Dinamik içerik ekle** listesinden **Akış başlığı**’nı seçin.

      ![Dinamik içerik listesi - "Akış başlığı"](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-subject-dynamic-content.png)

      İşlem tamamlandığında e-posta konusu şu örnekteki gibi görünür:

      ![Akış başlığı eklendi](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-feed-title.png)

      Tasarımcıda bir "For each" döngüsü görünüyorsa, dizi için belirteç (örneğin, **categories-item** belirteci) seçmişsiniz demektir. Bu tür belirteçler için tasarımcı otomatik olarak ilgili belirtece başvuran eylemin etrafına bu döngüyü ekler. Bu şekilde mantıksal uygulamanız dizideki tüm öğeler için aynı eylemi gerçekleştirir. Döngüyü kaldırmak için döngünün başlık çubuğundaki **üç nokta** ( **...** ) simgesini seçin ve **Sil**' i seçin.

   1. **Gövde** kutusuna bu metni girin ve e-posta gövdesi için bu belirteçleri seçin. Bir düzenleme kutusuna boş satır eklemek için Shift + Enter tuşlarını kullanın.

      ![E-posta gövdesi için içerik ekleme](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-body.png)

      | Özellik | Açıklama |
      |----------|-------------|
      | **Akış başlığı** | Öğenin başlığı |
      | **Akış yayımlanma zamanı** | Öğenin yayımlandığı tarih ve saat |
      | **Birincil akış bağlantısı** | Öğenin URL'si |
      |||

1. Mantıksal uygulamanızı kaydedin.

Daha sonra, mantıksal uygulamanızı test edin.

## <a name="run-your-logic-app"></a>Mantıksal uygulamanızı çalıştırın

Mantıksal uygulamanızı el ile başlatmak için tasarımcı araç çubuğu çubuğunda **Çalıştır**' ı seçin. Veya mantıksal uygulamanızın belirttiğiniz zamanlamaya (dakika başı) göre RSS akışını denetlemesini bekleyin. RSS akışında yeni öğeler olduğunda mantıksal uygulamanız her yeni öğe için bir e-posta gönderir. Aksi takdirde mantıksal uygulamanız yeniden denetlemek için bir sonraki zaman aralığını bekler. E-posta gelmezse istenmeyen e-posta klasörüne bakın.

Örneğin, bu mantıksal uygulamanın gönderdiği örnek bir e-posta aşağıda verilmiştir.

![Yeni RSS akışı öğesi için gönderilen e-posta](./media/quickstart-create-first-logic-app-workflow/monitor-rss-feed-email.png)

Teknik olarak tetikleyici, RSS akışını denetleyip yeni öğeler bulduğunda tetikleyici etkinleşir ve Logic Apps altyapısı, mantıksal uygulama iş akışınızın, iş akışında eylemleri çalıştıran bir örneğini oluşturur. Tetikleyici yeni öğeler bulmazsa tetikleyici etkinleşmez ve iş akışı örneğini oluşturma işlemini "atlar".

Tebrikler, artık Azure portal ilk mantıksal uygulamanızı başarıyla oluşturup çalıştırdık.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık bu örneğe ihtiyacınız yoksa mantıksal uygulamanızı ve ilgili kaynakları içeren kaynak grubunu silin.

1. Ana Azure menüsünde **kaynak grupları**' nı seçin ve ardından mantıksal uygulamanızın kaynak grubunu seçin. **Genel bakış** bölmesinde **kaynak grubunu sil**' i seçin.

   !["Kaynak grupları" > "Genel bakış" > "Kaynak grubunu sil"](./media/quickstart-create-first-logic-app-workflow/delete-resource-group.png)

1. Kaynak grubu adını onay olarak girin ve **Sil**' i seçin.

   ![Silme işlemini onaylama](./media/quickstart-create-first-logic-app-workflow/delete-resource-group-2.png)

> [!NOTE]
> Mantıksal uygulamayı sildiğinizde yeni çalıştırma başlatılmaz. Devam eden ve bekleme durumunda olan tüm çalıştırmalar iptal edilir. Binlerce çalıştırma varsa iptal işleminin tamamlanması zaman alabilir.

## <a name="get-support"></a>Destek alın

Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/home?forum=azurelogicapps) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, belirttiğiniz zamanlamayı (dakika başı) temel alarak RSS güncelleştirmelerini denetleyen ve güncelleştirme olduğunda eylem gerçekleştiren (e-posta gönderen) ilk mantıksal uygulamanızı oluşturdunuz. Daha fazla bilgi edinmek için daha gelişmiş zamanlama temelli iş akışlarının oluşturulduğu şu öğreticiye geçin:

> [!div class="nextstepaction"]
> [Zamanlama tabanlı mantıksal uygulama ile trafiği denetleme](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)
