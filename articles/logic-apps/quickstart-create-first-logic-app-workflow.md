---
title: Otomatikleştirilmiş Tümleştirme iş akışı oluşturma
description: Hızlı başlangıç-sistem tümleştirme ve kuruluş uygulaması Tümleştirme (EAı) çözümleri için Azure Logic Apps kullanarak ilk otomatikleştirilmiş iş akışınızı oluşturun
services: logic-apps
ms.suite: integration
ms.reviewer: logicappspm
ms.topic: quickstart
ms.custom: mvc
ms.date: 07/23/2020
ms.openlocfilehash: c40bec80d9f61cf46221cbfe7dde80f3a7b46f6f
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89658303"
---
# <a name="quickstart-create-your-first-automated-integration-workflow-by-using-azure-logic-apps---azure-portal"></a>Hızlı başlangıç: Azure Logic Apps Azure portal kullanarak ilk otomatikleştirilmiş Tümleştirme iş akışınızı oluşturun

Bu hızlı başlangıçta, boş bir mantıksal uygulama oluşturma, tetikleyici ve eylem ekleme ve daha sonra mantıksal uygulamanızı test etme gibi [Azure Logic Apps](logic-apps-overview.md)kullanarak ilk iş akışınızı oluşturma konusunda temel genel kavramlar tanıtılmıştır. Bu hızlı başlangıçta, yeni öğeler için bir Web sitesinin RSS akışını düzenli olarak denetleyen bir mantıksal uygulama oluşturacaksınız. Yeni öğeler varsa, mantıksal uygulama her öğe için bir e-posta gönderir. İşlemi tamamladığınızda, mantıksal uygulamanız bu yüksek düzeyli iş akışı gibi görünür:

![Üst düzey örnek mantıksal uygulama iş akışını gösteren kavramsal resim.](./media/quickstart-create-first-logic-app-workflow/quickstart-workflow-overview.png)

Bu senaryoda, bir Azure aboneliğine sahip olmanız veya [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), Office 365 Outlook, Outlook.com veya Gmail gibi Azure Logic Apps tarafından desteklenen bir hizmetten e-posta hesabı için kaydolmanız gerekir. Desteklenen diğer e-posta hizmetleri için [burada bağlayıcılar listesini gözden geçirin](/connectors/). Bu örnekte, mantıksal uygulama bir iş veya okul hesabı kullanır. Farklı bir e-posta hizmeti kullanıyorsanız genel adımlar aynıdır, ancak Kullanıcı arabiriminiz biraz farklı görünebilir.

> [!IMPORTANT]
> Gmail bağlayıcısını kullanmak istiyorsanız, mantıksal uygulamalarda kısıtlama olmadan yalnızca G-Suite iş hesapları bu bağlayıcıyı kullanabilir. Gmail tüketicisi hesabınız varsa, bu bağlayıcıyı yalnızca belirli Google onaylı hizmetlerle kullanabilirsiniz veya [Gmail Bağlayıcınız ile kimlik doğrulaması için kullanmak üzere bir Google istemci uygulaması oluşturabilirsiniz](/connectors/gmail/#authentication-and-bring-your-own-application). Daha fazla bilgi için, bkz. [Azure Logic Apps Google bağlayıcıları Için veri güvenliği ve gizlilik ilkeleri](../connectors/connectors-google-data-security-privacy-policy.md).

## <a name="create-your-logic-app"></a>Mantıksal uygulamanızı oluşturma

1. Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure portal arama kutusuna girin `logic apps` ve **Logic Apps**öğesini seçin.

   ![Arama terimi olarak "Logic Apps" ve seçilen arama sonucu olarak "Logic Apps" içeren Azure portal arama kutusunu gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/find-select-logic-apps.png)

1. **Logic Apps** sayfasında **Ekle**' yi seçin.

   ![Logic Apps listesini ve seçili düğmeyi gösteren ekran görüntüsü, "Ekle".](./media/quickstart-create-first-logic-app-workflow/add-new-logic-app.png)

1. **Mantıksal uygulama** bölmesinde aşağıda gösterildiği gibi mantıksal uygulamanız hakkında ayrıntılı bilgi sağlayın.

   ![Mantıksal uygulama oluşturma bölmesini yeni mantıksal uygulama ile ilgili ayrıntılarla gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/create-logic-app-settings.png)

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | **Ad** | <*Logic-App-adı*> | Mantıksal uygulamanızın adı, bölgeler genelinde benzersiz olmalıdır ve yalnızca harf, rakam, kısa çizgi ( `-` ), alt çizgi ( `_` ), parantez ( `(` , `)` ) ve nokta ( `.` ) içerebilir. Bu örnek "My-First-Logic-App" kullanır. |
   | **Abonelik** | <*Azure-abonelik-adı*> | Azure abonelik adınız |
   | **Kaynak grubu** | <*Azure-Resource-Group-Name*> | [Azure Kaynak grubu](../azure-resource-manager/management/overview.md)için, bölgeler genelinde benzersiz olması gereken ve ilgili kaynakları düzenlemek için kullanılan ad. Bu örnek "My-First-LA-RG" kullanır. |
   | **Konum** | <*Azure-bölge*> | Mantıksal uygulama bilgilerinizin depolanacağı bölge. Bu örnek, "Batı ABD" kullanır. |
   | **Log Analytics** | Kapalı | Tanılama günlüğüne kaydetme ayarını **Kapalı** durumda bırakın. |
   ||||

1. Hazırsanız, **gözden geçir + oluştur**' u seçin. Verdiğiniz ayrıntıları onaylayın ve **Oluştur**' u seçin.

1. Azure uygulamanızı başarıyla dağıtduktan sonra **Kaynağa Git**' i seçin.

   ![Kaynak dağıtım sayfasını ve "kaynağa git" için seçili düğmeyi gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/go-to-new-logic-app-resource.png)

   Ya da, arama kutusuna adı yazarak mantıksal uygulamanızı bulabilir ve seçebilirsiniz.

   Logic Apps Tasarımcısı açılır ve bir tanıtım videosu ile sık kullanılan tetikleyicilerin bulunduğu bir sayfa görüntülenir. **Şablonlar** altında **Boş Mantıksal Uygulama**'yı seçin.

   ![Logic Apps tasarımcı şablonu galerisini ve seçili şablon, "boş mantıksal uygulama" gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/choose-logic-app-template.png)

Sonra, yeni bir RSS akışı öğesi göründüğünde tetiklenen bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin. Her mantıksal uygulama, belirli bir olay gerçekleştiğinde ya da belirli bir koşul karşılandığında tetiklenen bir tetikleyiciyle başlamalıdır. Tetikleyici her tetiklendiğinde Azure Logic Apps altyapısı, iş akışınızı başlatan ve çalıştıran bir mantıksal uygulama örneği oluşturur.

<a name="add-rss-trigger"></a>

## <a name="add-the-rss-trigger"></a>RSS tetikleyicisi ekleme

1. **Mantıksal uygulama Tasarımcısı**' nda, arama kutusunda, **Tümü**' nü seçin.

1. RSS bağlayıcısını bulmak için arama kutusuna girin `rss` . Tetikleyiciler listesinden, **bir akış öğesi YAYıMLANDıĞıNDA**RSS tetikleyicisini seçin.

   ![Arama kutusunda "RSS" ile birlikte Logic Apps tasarımcı ve seçili tetikleyici "bir akış öğesi yayımlandığında" gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/add-rss-trigger-new-feed-item.png)

1. Bu adımda açıklandığı gibi tetikleyicinizin bilgilerini sağlayın:

   ![RSS URL 'SI, sıklık ve Aralık dahil olmak üzere RSS tetikleyici ayarları ile Logic Apps tasarımcısını gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/add-rss-trigger-settings.png)

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | **RSS akışının URL'si** | <*RSS-Feed-URL*> | İzlemek istediğiniz RSS akışına yönelik bağlantı. Bu örnek, adresindeki duvar Caddesi günlüğü RSS akışını kullanır `https://feeds.a.dj.com/rss/RSSMarketsMain.xml` , ancak isterseniz kendı RSS AKıŞ URL 'nizi kullanabilirsiniz. |
   | **Aralık** | 1 | Denetimler arasında beklenecek aralık sayısı |
   | **Sıklık** | Dakika | Denetimler arası her aralık için zaman birimi |
   ||||

   Aralık ve sıklık özelliği, mantıksal uygulamanızın tetikleyicisi için zamanlamayı tanımlar. Bu mantıksal uygulama, akışı dakika başı denetler.

1. Tetikleyici ayrıntılarını şimdilik daraltmak için tetikleyicinin başlık çubuğunun içine tıklayın.

   ![Daraltılmış Logic Apps şekliyle Logic Apps tasarımcı gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/collapse-trigger-shape.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

Mantıksal uygulamanız çalışıyor ancak RSS akışını kontrol etme dışında bir işlem gerçekleştirmiyor. Şimdi, tetikleyici etkinleştirildiğinde gerçekleştirilecek bir eylem ekleyin.

## <a name="add-the-send-email-action"></a>"E-posta gönder" eylemini ekleyin

Şimdi RSS akışında yeni bir öğe göründüğünde e-posta gönderen bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin.

1. **Bir akış öğesi yayımlandığında** tetikleyicisi bölümünde **yeni adım**' ı seçin.

   !["Yeni adım" ile Logic Apps tasarımcısını gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/add-new-step-under-trigger.png)

1. **Eylem Seç** ' in altında, arama kutusu ' nun altında **Tümü**' nü seçin.

1. Arama kutusuna, `send an email` Bu eylemi sunan bağlayıcıları bulabilmeniz için yazın. Eylemler listesini belirli bir uygulama veya hizmete göre filtrelemek için, önce bu uygulamayı veya hizmeti seçebilirsiniz.

   Örneğin, bir Microsoft iş veya okul hesabı kullanıyorsanız ve Office 365 Outlook 'u kullanmak istiyorsanız **office 365 Outlook**' u seçin. Ya da kişisel Microsoft hesabı kullanıyorsanız, Outlook.com ' yi seçebilirsiniz. Bu örnek, Office 365 Outlook ile devam eder:

   ![Logic Apps Designer ve seçili Office 365 Outlook bağlayıcısını gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/select-connector.png)

   Artık kullanmak istediğiniz eylemi daha kolay bulabilir ve seçebilirsiniz, örneğin `send an email` :

   ![Logic Apps tasarımcı ve filtrelenmiş eylemlerle liste gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/filtered-actions-list.png)

1. Seçtiğiniz e-posta Bağlayıcınız kimliğinizin kimliğini doğrulamanızı isterse, mantıksal uygulamanız ile e-posta hizmetiniz arasında bir bağlantı oluşturmak için bu adımı hemen doldurun.

   > [!NOTE]
   > Bu belirli örnekte, kimliğinizi el ile doğrukullanırsınız. Ancak, kimlik doğrulaması gerektiren bağlayıcılar destekledikleri kimlik doğrulama türlerinde farklılık gösterir. Ayrıca, kimlik doğrulamasını işlemek istediğiniz şekilde ayarlama seçenekleriniz vardır. Örneğin, dağıtım için Azure Resource Manager Şablonlar kullandığınızda, bağlantı bilgileri gibi sıklıkla veya kolayca değiştirmek istediğiniz girişler üzerindeki güvenliği parametreleştirebilirsiniz ve artırabilirsiniz. Daha fazla bilgi için şu konulara bakın:
   >
   > * [Dağıtım için şablon parametreleri](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#template-parameters)
   > * [OAuth bağlantılarını yetkilendir](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md#authorize-oauth-connections)
   > * [Yönetilen kimliklerle erişimin kimliğini doğrulama](../logic-apps/create-managed-service-identity.md)
   > * [Mantıksal uygulama dağıtımı için bağlantı kimlik doğrulaması](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#authenticate-connections)

1. **E-posta gönder** eyleminde, e-postaya eklenecek bilgileri belirtin.

   1. **Alıcı** kutusuna alıcının e-posta adresini girin. Sınama amacıyla e-posta adresinizi kullanabilirsiniz.

      Görüntülenen **Dinamik içerik ekle** listesini şimdilik yoksayın. Bazı düzenleme kutularının içine tıkladığınızda, bu liste görünür ve önceki adımdan geçerli eylem için giriş olarak kullanabileceğiniz tüm kullanılabilir çıktıları gösterir.

   1. **Konu** kutusuna, sonuna bir boşluk koyarak şu metni girin: `New RSS item: `

      !["Konu" özellik kutusunun içinde "e-posta gönder" eylemi ve imleci içeren Logic Apps tasarımcısını gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/send-email-subject.png)

   1. **Dinamik Içerik Ekle** listesinden, "bir akış öğesi yayımlandığında" tetikleyiciden çıktı olan **akış başlığı**' nı SEÇIN, bu da RSS öğesi başlığını kullanabilmeniz için kullanılabilir hale getirir.

      !["Konu" özellik kutusunun içindeki "e-posta gönder" eylemi ve imleci açık bir dinamik içerik listesi ve seçili çıkış "akış başlığı" ile birlikte Logic Apps tasarımcısını gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/send-email-subject-dynamic-content.png)

      > [!TIP]
      > Dinamik içerik listesinde, "bir akış öğesi yayımlandığında" tetikleyicisinden hiçbir çıkış görünmezse, eylem üstbilgisinin yanında, **daha fazla göster**' i seçin.
      > 
      > ![Açık dinamik içerik listesi ile Logic Apps tasarımcısını gösteren ekran görüntüsü ve tetikleyici için "daha fazla gör" seçilidir.](./media/quickstart-create-first-logic-app-workflow/dynamic-content-list-see-more-actions.png)

      İşlem tamamlandığında e-posta konusu şu örnekteki gibi görünür:

      !["E-posta gönder" eylemine sahip Logic Apps tasarımcısını ve dahil edilen "akış başlığı" özelliği ile örnek bir e-posta konusunu gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/send-email-feed-title.png)

      Tasarımcıda bir "For each" döngüsü görünüyorsa, dizi için belirteç (örneğin, **categories-item** belirteci) seçmişsiniz demektir. Bu tür belirteçler için tasarımcı otomatik olarak ilgili belirtece başvuran eylemin etrafına bu döngüyü ekler. Bu şekilde mantıksal uygulamanız dizideki tüm öğeler için aynı eylemi gerçekleştirir. Döngüyü kaldırmak için döngünün başlık çubuğundaki **üç nokta** (**...**) simgesini seçin ve **Sil**' i seçin.

   1. **Gövde** kutusuna bu metni girin ve e-posta gövdesi için bu belirteçleri seçin. Bir düzenleme kutusuna boş satır eklemek için Shift + Enter tuşlarını kullanın.

      !["E-posta gönder" eylemi ve "gövde" kutusunun içinde seçili özellikler içeren Logic Apps tasarımcısını gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/send-email-body.png)

      | Özellik | Açıklama |
      |----------|-------------|
      | **Akış başlığı** | Öğenin başlığı |
      | **Akış yayımlanma zamanı** | Öğenin yayımlandığı tarih ve saat |
      | **Birincil akış bağlantısı** | Öğenin URL'si |
      |||

1. Mantıksal uygulamanızı kaydedin.

Daha sonra, mantıksal uygulamanızı test edin.

## <a name="run-your-logic-app"></a>Mantıksal uygulamanızı çalıştırın

Mantıksal uygulamanızı el ile başlatmak için tasarımcı araç çubuğu çubuğunda **Çalıştır**' ı seçin. Veya mantıksal uygulamanızın belirttiğiniz zamanlamaya (dakika başı) göre RSS akışını denetlemesini bekleyin.

![Tasarımcı araç çubuğunda "Çalıştır" düğmesi seçili olan Logic Apps tasarımcısını gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/run-logic-app-test.png)

RSS akışında yeni öğeler olduğunda mantıksal uygulamanız her yeni öğe için bir e-posta gönderir. Aksi takdirde mantıksal uygulamanız yeniden denetlemek için bir sonraki zaman aralığını bekler. E-posta gelmezse istenmeyen e-posta klasörüne bakın.

Örneğin, bu mantıksal uygulamanın gönderdiği örnek bir e-posta aşağıda verilmiştir.

![Yeni RSS Akış öğesi göründüğünde alınan örnek e-postayı gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/monitor-rss-feed-email.png)

Teknik olarak, tetikleyici RSS akışını denetlediğinde ve yeni öğeler bulduğunda, tetikleyici ateşlenir ve Azure Logic Apps altyapısı, iş akışındaki eylemleri çalıştıran mantıksal uygulama iş akışınızın bir örneğini oluşturur. Tetikleyici yeni öğeler bulmazsa tetikleyici etkinleşmez ve iş akışı örneğini oluşturma işlemini "atlar".

Tebrikler, artık Azure portal ilk mantıksal uygulamanızı başarıyla oluşturup çalıştırdık.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık bu örneğe ihtiyacınız yoksa mantıksal uygulamanızı ve ilgili kaynakları içeren kaynak grubunu silin.

1. Azure Arama kutusuna girin `resource groups` ve ardından **kaynak grupları**' nı seçin.

   ![Arama terimi "kaynak grupları" olan Azure portal arama kutusunu gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/find-resource-groups.png)

1. Mantıksal uygulamanızın kaynak grubunu bulun ve seçin. **Genel bakış** bölmesinde **kaynak grubunu sil**' i seçin.

   !["Kaynak grubunu sil" için seçili kaynak grubu ve düğmeyle birlikte Azure portal gösteren ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/delete-resource-group.png)

1. Onay bölmesi göründüğünde, kaynak grubu adını girin ve **Sil**' i seçin.

   ![Onay bölmesinin Azure portal ve silinecek kaynak grubu adının gösterildiği ekran görüntüsü.](./media/quickstart-create-first-logic-app-workflow/delete-resource-group-2.png)

> [!NOTE]
> Mantıksal uygulamayı sildiğinizde yeni çalıştırma başlatılmaz. Devam eden ve bekleme durumunda olan tüm çalıştırmalar iptal edilir. Binlerce çalıştırma varsa iptal işleminin tamamlanması zaman alabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, belirttiğiniz zamanlamayı (dakika başı) temel alarak RSS güncelleştirmelerini denetleyen ve güncelleştirme olduğunda eylem gerçekleştiren (e-posta gönderen) ilk mantıksal uygulamanızı oluşturdunuz. Daha fazla bilgi edinmek için daha gelişmiş zamanlama temelli iş akışlarının oluşturulduğu şu öğreticiye geçin:

> [!div class="nextstepaction"]
> [Zamanlama tabanlı mantıksal uygulama ile trafiği denetleme](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)
