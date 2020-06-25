---
title: İlk otomatikleştirilmiş iş akışınızı oluşturma
description: Hızlı başlangıç-sistem tümleştirme ve kuruluş uygulaması Tümleştirme (EAı) çözümleri için Azure Logic Apps kullanarak ilk otomatikleştirilmiş iş akışınızı oluşturun
services: logic-apps
ms.suite: integration
ms.reviewer: logicappspm
ms.topic: quickstart
ms.custom: mvc
ms.date: 10/20/2019
ms.openlocfilehash: 0b3559a27fe9fae6c34b07c648a289d205560bd8
ms.sourcegitcommit: 01cd19edb099d654198a6930cebd61cae9cb685b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85321592"
---
# <a name="quickstart-create-your-first-workflow-by-using-azure-logic-apps---azure-portal"></a>Hızlı başlangıç: Azure Logic Apps Azure portal kullanarak ilk iş akışınızı oluşturun

Bu hızlı başlangıçta, boş bir mantıksal uygulama oluşturma, tetikleyici ve eylem ekleme ve daha sonra mantıksal uygulamanızı test etme gibi [Azure Logic Apps](../logic-apps/logic-apps-overview.md)kullanarak ilk iş akışınızı oluşturma konusunda temel genel kavramlar tanıtılmıştır. Bu hızlı başlangıçta, yeni öğeler için bir Web sitesinin RSS akışını düzenli olarak denetleyen bir mantıksal uygulama oluşturacaksınız. Yeni öğeler varsa, mantıksal uygulama her öğe için bir e-posta gönderir. İşlemi tamamladığınızda, mantıksal uygulamanız bu yüksek düzeyli iş akışı gibi görünür:

![Üst düzey örnek mantıksal uygulama iş akışı](./media/quickstart-create-first-logic-app-workflow/quickstart-workflow-overview.png)

Bu senaryoda, bir Azure aboneliğine sahip olmanız veya [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/), Office 365 Outlook, Outlook.com veya Gmail gibi Azure Logic Apps tarafından desteklenen bir hizmetten e-posta hesabı için kaydolmanız gerekir. Desteklenen diğer e-posta hizmetleri için [burada bağlayıcılar listesini gözden geçirin](https://docs.microsoft.com/connectors/). Bu örnekte, Logic App bir Office 365 Outlook hesabı kullanır. Farklı bir e-posta hizmeti kullanıyorsanız genel adımlar aynıdır, ancak Kullanıcı arabiriminiz biraz farklı görünebilir.

> [!IMPORTANT]
> Gmail bağlayıcısını kullanmak istiyorsanız, mantıksal uygulamalarda kısıtlama olmadan yalnızca G-Suite iş hesapları bu bağlayıcıyı kullanabilir. Gmail tüketicisi hesabınız varsa, bu bağlayıcıyı yalnızca belirli Google onaylı hizmetlerle kullanabilirsiniz veya [Gmail Bağlayıcınız ile kimlik doğrulaması için kullanmak üzere bir Google istemci uygulaması oluşturabilirsiniz](https://docs.microsoft.com/connectors/gmail/#authentication-and-bring-your-own-application). Daha fazla bilgi için, bkz. [Azure Logic Apps Google bağlayıcıları Için veri güvenliği ve gizlilik ilkeleri](../connectors/connectors-google-data-security-privacy-policy.md).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-your-logic-app"></a>Mantıksal uygulamanızı oluşturma

1. Azure giriş sayfasında, arama kutusunda **Logic Apps**bulun ve seçin.

   !["Logic Apps" bul ve Seç](./media/quickstart-create-first-logic-app-workflow/find-select-logic-apps.png)

1. **Logic Apps** sayfasında **Ekle**' yi seçin.

   ![Yeni mantıksal uygulama ekle](./media/quickstart-create-first-logic-app-workflow/add-new-logic-app.png)

1. **Mantıksal uygulama** bölmesinde aşağıda gösterildiği gibi mantıksal uygulamanız hakkında ayrıntılı bilgi sağlayın. İşiniz bittiğinde **Oluştur**' u seçin.

   ![Yeni mantıksal uygulama için ayrıntıları sağlayın](./media/quickstart-create-first-logic-app-workflow/create-logic-app-settings.png)

   | Özellik | Değer | Description |
   |----------|-------|-------------|
   | **Adı** | <*Logic-App-adı*> | Mantıksal uygulamanızın adı, yalnızca harf, sayı, kısa çizgi ( `-` ), alt çizgi ( `_` ), parantez ( `(` , `)` ) ve nokta ( `.` ) içerebilir. Bu örnek "My-First-Logic-App" kullanır. <p><p>**Note**: mantıksal uygulamalar için adların bölgeler arasında benzersiz olması gerekir. |
   | **Abonelik** | <*Azure-abonelik-adı*> | Azure abonelik adınız |
   | **Kaynak grubu** | <*Azure-Resource-Group-Name*> | İlgili kaynakları düzenlemek için kullanılan [Azure Kaynak grubunun](../azure-resource-manager/management/overview.md) adı. Bu örnek "My-First-LA-RG" kullanır. <p><p>**Note**: kaynak gruplarının adları bölgeler arasında benzersiz olmalıdır. |
   | **Konum** | <*Azure-bölge*> | Mantıksal uygulama bilgilerinizin depolanacağı bölge. Bu örnek, "Batı ABD" kullanır. |
   | **Log Analytics** | Kapalı | Tanılama günlüğüne kaydetme ayarını **Kapalı** durumda bırakın. |
   ||||

1. Azure, uygulamanızı dağıttıktan sonra Azure araç çubuğunda bildirimler ' i seçerek **Notifications**  >  dağıtılan mantıksal uygulamanız için**kaynağa gidin** .

   ![Yeni oluşturulan mantıksal uygulama kaynağına git](./media/quickstart-create-first-logic-app-workflow/go-to-new-logic-app-resource.png)

   Ya da, arama kutusuna adı yazarak mantıksal uygulamanızı bulabilir ve seçebilirsiniz.

   Logic Apps Tasarımcısı açılır ve bir tanıtım videosu ile sık kullanılan tetikleyicilerin bulunduğu bir sayfa görüntülenir. **Şablonlar** altında **Boş Mantıksal Uygulama**'yı seçin.

   ![Mantıksal uygulama için boş şablon seçin](./media/quickstart-create-first-logic-app-workflow/choose-logic-app-template.png)

Sonra, yeni bir RSS akışı öğesi göründüğünde tetiklenen bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin. Her mantıksal uygulama, belirli bir olay gerçekleştiğinde ya da belirli bir koşul karşılandığında tetiklenen bir tetikleyiciyle başlamalıdır. Tetikleyici her tetiklendiğinde Azure Logic Apps altyapısı, iş akışınızı başlatan ve çalıştıran bir mantıksal uygulama örneği oluşturur.

<a name="add-rss-trigger"></a>

## <a name="add-the-rss-trigger"></a>RSS tetikleyicisi ekleme

1. **Mantıksal uygulama Tasarımcısı**' nda, arama kutusunda, **Tümü**' nü seçin.

1. Arama kutusuna `rss` RSS bağlayıcısını bulmak için yazın. Tetikleyiciler listesinden **bir akış öğesi yayımlandığında** tetikleyicisi ' ni seçin.

   !["Akış öğesi yayımlandığında" tetikleyicisini seçin](./media/quickstart-create-first-logic-app-workflow/add-rss-trigger-new-feed-item.png)

1. Tetikleyiciniz için tabloda açıklandığı gibi bilgi sağlayın:

   ![Tetikleyicinin RSS akışı, sıklık ve aralık ayarını yapma](./media/quickstart-create-first-logic-app-workflow/add-rss-trigger-settings.png)

   | Özellik | Değer | Description |
   |----------|-------|-------------|
   | **RSS akışının URL'si** | <*RSS-Feed-URL*> | İzlemek istediğiniz RSS akışına yönelik bağlantı. Bu örnekte, kullanılır `http://feeds.reuters.com/reuters/topNews` . |
   | **Interval** | 1 | Denetimler arasında beklenecek aralık sayısı |
   | **Sıklık** | Dakika | Denetimler arası her aralık için zaman birimi  |
   ||||

   Aralık ve sıklık özelliği, mantıksal uygulamanızın tetikleyicisi için zamanlamayı tanımlar. Bu mantıksal uygulama, akışı dakika başı denetler.

1. Tetikleyici ayrıntılarını şimdilik daraltmak için tetikleyicinin başlık çubuğunun içine tıklayın.

   ![Ayrıntıları gizlemek için mantıksal uygulama şeklini daraltın](./media/quickstart-create-first-logic-app-workflow/collapse-trigger-shape.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

Mantıksal uygulamanız çalışıyor ancak RSS akışını kontrol etme dışında bir işlem gerçekleştirmiyor. Şimdi, tetikleyici etkinleştirildiğinde gerçekleştirilecek bir eylem ekleyin.

## <a name="add-the-send-email-action"></a>"E-posta gönder" eylemini ekleyin

Şimdi RSS akışında yeni bir öğe göründüğünde e-posta gönderen bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin.

1. **Bir akış öğesi yayımlandığında** tetikleyicisi bölümünde **yeni adım**' ı seçin.

   ![Tetikleyici altında "yeni adım" i seçin](./media/quickstart-create-first-logic-app-workflow/add-new-step-under-trigger.png)

1. **Eylem Seç** ' in altında, arama kutusu ' nun altında **Tümü**' nü seçin.

1. `send an email`Bu eylemi sunan bağlayıcılar bulmak için arama kutusuna yazın. Eylemler listesinden, kullanmak istediğiniz e-posta hizmeti için "e-posta gönder" eylemini seçin. Bu örnek, **e-posta gönder** eylemi olan Office 365 Outlook bağlayıcısını kullanır.

   ![Office 365 Outlook için "e-posta gönder" eylemini seçin](./media/quickstart-create-first-logic-app-workflow/add-action-send-email.png)

   Eylem listesini belirli bir uygulama veya hizmeti içerek şekilde filtrelemek için önce uygulama ya da hizmeti seçebilirsiniz:

   * Azure iş veya okul hesapları için Office 365 Outlook girişini seçin.
   * Kişisel Microsoft hesapları için Outlook.com girişini seçin.

1. Seçtiğiniz e-posta Bağlayıcınız kimliğinizin kimliğini doğrulamanızı isterse, mantıksal uygulamanız ile e-posta hizmetiniz arasında bir bağlantı oluşturmak için bu adımı hemen doldurun.

   > [!NOTE]
   > Bu belirli örnekte, kimliğinizi el ile doğrukullanırsınız. Ancak, kimlik doğrulaması gerektiren bağlayıcılar destekledikleri kimlik doğrulama türlerinde farklılık gösterir. Ayrıca, kimlik doğrulamasını işlemek istediğiniz şekilde ayarlama seçenekleriniz vardır. Örneğin, dağıtım için Azure Resource Manager Şablonlar kullandığınızda, bağlantı bilgileri gibi sıklıkla veya kolayca değiştirmek istediğiniz girişler üzerindeki güvenliği parametreleştirebilirsiniz ve artırabilirsiniz. Daha fazla bilgi için şu konulara bakın:
   >
   > * [Dağıtım için şablon parametreleri](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#template-parameters)
   > * [OAuth bağlantılarını yetkilendir](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md#authorize-oauth-connections)
   > * [Yönetilen kimliklerle erişimin kimliğini doğrulama](../logic-apps/create-managed-service-identity.md)
   > * [Mantıksal uygulama dağıtımı için bağlantı kimlik doğrulaması](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#authenticate-connections)

1. **E-posta gönderme** eyleminde, e-postanızın içermesini istediğiniz verileri belirtin.

   1. **Alıcı** kutusuna alıcının e-posta adresini girin. Sınama amacıyla e-posta adresinizi kullanabilirsiniz.

      Görüntülenen **Dinamik içerik ekle** listesini şimdilik yoksayın. Bazı düzenleme kutularının içine tıkladığınızda bu liste görünür ve önceki adımdan iş akışınıza girdi olarak ekleyebileceğiniz tüm kullanılabilir parametreleri gösterir.

   1. **Konu** kutusuna, sonuna bir boşluk koyarak şu metni girin: `New RSS item: `

      !["Konu" özelliğine e-posta konusunu girin](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-subject.png)

   1. RSS öğesinin başlığını eklemek için **Dinamik içerik ekle** listesinden **Akış başlığı**’nı seçin.

      ![Dinamik içerik listesinden "akış başlığı" özelliği ' ni seçin.](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-subject-dynamic-content.png)

      İşlem tamamlandığında e-posta konusu şu örnekteki gibi görünür:

      ![Eklenen akış başlığı için e-posta konusu örneği tamamlandı](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-feed-title.png)

      Tasarımcıda bir "For each" döngüsü görünüyorsa, dizi için belirteç (örneğin, **categories-item** belirteci) seçmişsiniz demektir. Bu tür belirteçler için tasarımcı otomatik olarak ilgili belirtece başvuran eylemin etrafına bu döngüyü ekler. Bu şekilde mantıksal uygulamanız dizideki tüm öğeler için aynı eylemi gerçekleştirir. Döngüyü kaldırmak için döngünün başlık çubuğundaki **üç nokta** (**...**) simgesini seçin ve **Sil**' i seçin.

   1. **Gövde** kutusuna bu metni girin ve e-posta gövdesi için bu belirteçleri seçin. Bir düzenleme kutusuna boş satır eklemek için Shift + Enter tuşlarını kullanın.

      ![E-posta gövdesi içeriği için özellikleri seçin](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-body.png)

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

![Yeni RSS Akış öğesi göründüğünde gönderilen örnek e-posta](./media/quickstart-create-first-logic-app-workflow/monitor-rss-feed-email.png)

Teknik olarak, tetikleyici RSS akışını denetlediğinde ve yeni öğeler bulduğunda, tetikleyici ateşlenir ve Azure Logic Apps altyapısı, iş akışındaki eylemleri çalıştıran mantıksal uygulama iş akışınızın bir örneğini oluşturur. Tetikleyici yeni öğeler bulmazsa tetikleyici etkinleşmez ve iş akışı örneğini oluşturma işlemini "atlar".

Tebrikler, artık Azure portal ilk mantıksal uygulamanızı başarıyla oluşturup çalıştırdık.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık bu örneğe ihtiyacınız yoksa mantıksal uygulamanızı ve ilgili kaynakları içeren kaynak grubunu silin.

1. Ana Azure menüsünde **kaynak grupları**' nı seçin ve ardından mantıksal uygulamanızın kaynak grubunu seçin. **Genel bakış** bölmesinde **kaynak grubunu sil**' i seçin.

   ![Kaynak grubunu bulun, seçin ve silin](./media/quickstart-create-first-logic-app-workflow/delete-resource-group.png)

1. Onay bölmesi göründüğünde, kaynak grubu adını girin ve **Sil**' i seçin.

   ![Silmeyi onaylamak için "Sil" i seçin](./media/quickstart-create-first-logic-app-workflow/delete-resource-group-2.png)

> [!NOTE]
> Mantıksal uygulamayı sildiğinizde yeni çalıştırma başlatılmaz. Devam eden ve bekleme durumunda olan tüm çalıştırmalar iptal edilir. Binlerce çalıştırma varsa iptal işleminin tamamlanması zaman alabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, belirttiğiniz zamanlamayı (dakika başı) temel alarak RSS güncelleştirmelerini denetleyen ve güncelleştirme olduğunda eylem gerçekleştiren (e-posta gönderen) ilk mantıksal uygulamanızı oluşturdunuz. Daha fazla bilgi edinmek için daha gelişmiş zamanlama temelli iş akışlarının oluşturulduğu şu öğreticiye geçin:

> [!div class="nextstepaction"]
> [Zamanlama tabanlı mantıksal uygulama ile trafiği denetleme](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)
