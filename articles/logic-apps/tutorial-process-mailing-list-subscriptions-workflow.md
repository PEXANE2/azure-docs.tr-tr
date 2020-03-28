---
title: Onay tabanlı otomatik iş akışları oluşturma
description: Öğretici - Azure Logic Apps'ı kullanarak posta listesi aboneliklerini işleyen onay tabanlı otomatik iş akışı oluşturma
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: tutorial
ms.custom: mvc
ms.date: 09/20/2019
ms.openlocfilehash: 7d7f573e5b18e6e0e63d3275aecefe408a9143fb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75456605"
---
# <a name="tutorial-create-automated-approval-based-workflows-by-using-azure-logic-apps"></a>Öğretici: Azure Logic Apps'ı kullanarak otomatik onay tabanlı iş akışları oluşturun

Bu öğretici, onay tabanlı bir iş akışını otomatikleştiren bir [mantık uygulamasının](../logic-apps/logic-apps-overview.md) nasıl oluşturulabildiğini gösterir. Özellikle, bu mantık uygulaması [MailChimp](https://mailchimp.com/) hizmeti tarafından yönetilen bir posta listesi için abonelik isteklerini işler. Bu mantıksal uygulama e-posta hesabını bu istekler için izler, bu istekleri onaya gönderir ve onaylanan üyeleri posta listesine ekler.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Boş bir mantıksal uygulama oluşturma.
> * E-postalardaki abonelik isteklerini izleyen bir tetikleyici ekleme.
> * Bu istekleri onaylamak veya reddetmek için e-posta gönderen bir eylem ekleme.
> * Onay yanıtını denetleyen bir koşul ekleme.
> * Onaylanan üyeleri posta listesine ekleyen bir eylem ekleme.
> * Bu üyelerin listeye başarıyla katılıp katılmadığını denetleyen bir koşul ekleme.
> * Bu üyelerin listeye başarıyla katılıp katılmadığına ilişkin onay e-postaları gönderen bir eylem ekleme.

İşlemi tamamladığınızda, mantıksal uygulamanız bu yüksek düzeyli iş akışı gibi görünür:

![Üst düzey bitmiş mantık uygulamasına genel bakış](./media/tutorial-process-mailing-list-subscriptions-workflow/tutorial-high-level-overview.png)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, başlamadan önce [ücretsiz bir Azure hesabına kaydolun.](https://azure.microsoft.com/free/)

* Mantık uygulamanızın onaylanmış üyeler için e-posta adresleri ekleyebileceği "test üyeleri-ML" adlı bir liste içeren bir MailChimp hesabı. Hesabınız yoksa, ücretsiz bir [hesap için kaydolun](https://login.mailchimp.com/signup/)ve ardından [MailChimp listesi oluşturmayı](https://us17.admin.mailchimp.com/lists/#)öğrenin.

* Office 365 Outlook veya Outlook.com'da onay iş akışlarını destekleyen bir e-posta hesabı. Bu makalede Office 365 Outlook kullanılır. Farklı bir e-posta hesabı kullanırsanız genel adımlar aynı kalır, ancak kullanıcı arabiriminiz biraz farklı görünebilir.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-your-logic-app"></a>Mantıksal uygulamanızı oluşturma

1. Ana Azure menüsünden **kaynak** > **Tümleştirme** > **Mantığı Uygulaması**oluştur'u seçin.

   ![Yeni mantık uygulama kaynağınızı oluşturun](./media/tutorial-process-mailing-list-subscriptions-workflow/create-new-logic-app-resource.png)

1. **Mantıksal uygulama oluştur** bölümünde, gösterildiği ve açıklandığı gibi mantıksal uygulamanızla ilgili bu bilgileri sağlayın. İşiniz bittiğinde **Oluştur**’u seçin.

   ![Mantık uygulamanız hakkında bilgi sağlayın](./media/tutorial-process-mailing-list-subscriptions-workflow/create-logic-app-settings.png)

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | **Adı** | LA-MailingList | Mantık uygulamanızın adı, yalnızca harfleri, sayıları, tireleri`-`( ),`_`alt çizer`(` `)`( ),`.`parantez ( , ), ve dönemleri ( ) içerebilen. Bu örnekte "LA-MailingList" kullanılıyor. |
   | **Abonelik** | <*sizin-Azure abonelik-adı*> | Azure abonelik adınız |
   | **Kaynak grubu** | LA-MailingList-RG | İlgili kaynakları düzenlemek için kullanılan [Azure kaynak grubunun](../azure-resource-manager/management/overview.md)adı. Bu örnekte "LA-MailingList-RG" kullanışıktır. |
   | **Konum** | Batı ABD | TMantık uygulama bilgilerinizi depolayabilmek için bölge. Bu örnekte "Batı ABD" kullanılıyor. |
   | **Log Analytics** | Kapalı | Tanılama günlüğüne kaydetme ayarını **Kapalı** durumda bırakın. |
   ||||

1. Azure uygulamanızı dağıttıktan sonra, Azure araç çubuğunda, dağıtılan mantık uygulamanız için **Bildirimler** > **Git'i seçin.**

   ![Yeni mantık uygulaması kaynağınıza gidin](./media/tutorial-process-mailing-list-subscriptions-workflow/go-to-logic-app-resource.png)

   Veya, arama kutusuna adını yazarak mantık uygulamanızı bulabilir ve seçebilirsiniz.

   Logic Apps Designer, giriş videosu ve yaygın olarak kullanılan tetikleyiciler ve mantıksal uygulama desenleri içeren bir sayfayı açar ve gösterir. **Şablonlar** altında **Boş Mantıksal Uygulama**'yı seçin.

   ![Boş mantık uygulaması şablonu seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/select-logic-app-template.png)

Sonra abonelik isteklerinin bulunduğu gelen e-postaları dinleyen bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin. Her mantık uygulaması, belirli bir olay olduğunda veya yeni veriler belirli bir koşulu karşıladığında ateşleyen bir tetikleyiciyle başlasın. Daha fazla bilgi için bkz. [İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-trigger-to-monitor-emails"></a>E-postaları izlemek için tetikleyici ekleme

1. Logic App Designer'da, arama kutusuna filtreniz olarak girin. `when email arrives` **Tetikleyiciler** listesinden, e-posta sağlayıcınız için **yeni bir e-posta geldiğinde** tetikleyiciyi seçin.

   Bu örnek, Office 365 Outlook tetikleyicisini kullanır:

   ![E-posta sağlayıcısı için "Yeni bir e-posta geldiğinde" tetikleyicisi seçeneğini belirleyin](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-new-email.png)

   * Azure iş veya okul hesapları için Office 365 Outlook girişini seçin.
   * Kişisel Microsoft hesapları için Outlook.com girişini seçin.

1. İstenirse, Logic Apps'ın e-posta hesabınıza bağlantı oluşturabilmesi için kimlik bilgilerinizle e-posta hesabınızda oturum açın.

1. Tetikleyicide, tüm yeni e-postaları denetlemek için ölçütler sağlayın.

   1. E-postaları denetlemeye ilişkin klasörü, aralığı ve sıklığı belirtin.

      ![Postaları denetlemeye ilişkin klasörü, aralığı ve sıklığı belirtin](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-set-up-email.png)

      | Özellik | Değer | Açıklama |
      |----------|-------|-------------|
      | **Klasör** | `Inbox` | İzlenecek e-posta klasörü |
      | **Interval** | `1` | Denetimler arasında beklenecek aralık sayısı |
      | **Frequency** | `Hour` | Yinelenme için kullanılacak zaman birimi |
      ||||

   1. Şimdi tetikleyiciye başka bir özellik ekleyin, böylece e-posta konusu satırına filtre uygulayabilirsiniz. Yeni **parametre ekle listesini**açın ve **Konu Filtresi** özelliğini seçin.

      ![Tetiklemek için "Konu Filtresi" özelliği ekleme](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-add-properties.png)

      Bu tetikleyicinin özellikleri hakkında daha fazla bilgi için [Office 365 Outlook bağlayıcı sı veya](https://docs.microsoft.com/connectors/office365/) Outlook.com bağlayıcı [başvurusuna](https://docs.microsoft.com/connectors/outlook/)bakın.

   1. Özellik tetikleyicide göründükten sonra şu metni girin:`subscribe-test-members-ML`

      !["Konu Filtresi" özelliği için metin girin](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-subject-filter-property.png)

1. Tetikleyicinin ayrıntılarını şimdilik gizlemek için tetikleyicinin başlık çubuğuna tıklayın.

   ![Ayrıntıları gizlemek için şekli daraltın](./media/tutorial-process-mailing-list-subscriptions-workflow/collapse-trigger-shape.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

Mantıksal uygulamanız çalışıyor ancak gelen e-postanızı denetleme dışında bir işlem gerçekleştirmiyor. Şimdi, tetikleyici etkinleştirildiğinde gerçekleştirilecek bir eylem ekleyin.

## <a name="send-approval-email"></a>Onay e-postası gönderme

Artık tetikleyiciniz olduğuna göre, isteği onaylamak veya reddetmek üzere e-posta gönderen bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyin.

1. Tetikleyicinin altında **Yeni adım'ı**seçin. 

1. Bir **eylem seçin**altında, arama `approval` kutusuna filtreniz olarak girin. Eylemler listesinden, e-posta sağlayıcınız için **onay e-postası gönder** eylemini seçin. 

   Bu örnek, Office 365 Outlook eylemini kullanır:

   !["Onay e-postası gönder" eylemini seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-send-approval-email.png)

1. Açıklandığı gibi bu eylem hakkında bilgi sağlayın: 

   ![Onay e-posta özellikleri gönderme](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-approval-email-settings.png)

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | **Hedef** | <*e-posta adresiniz*> | Onaylayanın e-posta adresi. Test için kendi adresinizi kullanabilirsiniz. Bu örnekte kurgusalsophia.owen@fabrikam.com" " e-posta adresi kullanır. |
   | **Konu** | `Approve member request for test-members-ML` | Açıklayıcı bir e-posta konusu |
   | **Kullanıcı Seçenekleri** | `Approve, Reject` | Onaylayanın seçebileceği yanıt seçenekleri. Varsayılan olarak, onaylayan yanıt olarak "Onayla" veya "Reddet" seçeneğini seçebilir. |
   ||||

   Şimdilik, belirli bir edit kutularının içini tıklattığınızda görünen dinamik içerik listesini yoksayın. Bu liste, iş akışınızda girdi olarak kullanabileceğiniz önceki eylemlerden kullanılabilir çıktıseçmenize olanak tanır.

   Bu eylemin özellikleri hakkında daha fazla bilgi için [Office 365 Outlook bağlayıcı sı veya](https://docs.microsoft.com/connectors/office365/) Outlook.com bağlayıcı [başvurusuna](https://docs.microsoft.com/connectors/outlook/)bakın.
 
1. Mantıksal uygulamanızı kaydedin.

Ardından, onaylayanın seçili yanıtını denetlemek için bir koşul ekleyin.

## <a name="check-approval-response"></a>Onay yanıtını denetleme

1. Onay **e-posta gönder** eylemi altında Yeni **adım**"'ı seçin.

1. Bir **eylem seçin,** **Dahili'yi**seçin. Arama kutusuna filtreniz olarak girin. `condition` Eylemler listesinden **Durum** eylemini seçin.

   !["Koşul" eylemini bulma ve seçme](./media/tutorial-process-mailing-list-subscriptions-workflow/select-condition-action.png)

1. Koşulu daha iyi bir açıklama ile yeniden adlandırın.

   1. Koşulun başlık çubuğunda, **elipsleri** (**...**) düğmesini seçin > **Yeniden Adlandır.**

      ![Koşul açıklamasını yeniden adlandırın](./media/tutorial-process-mailing-list-subscriptions-workflow/rename-condition-description.png)

   1. Koşulunuzu şu açıklama ile yeniden adlandırın: `If request approved`

1. Onaylayanın **Onayla'yı**seçip seçmediğini denetleyen bir koşul oluşturun.

   1. Bu durumda, koşulun sol tarafındaki bir değer kutusunu **seç'in** içini tıklatın.

   1. Onay **e-postası gönder**altında görünen dinamik içerik listesinden **SelectedOption** özelliğini seçin.

      ![Dinamik içerik listesinden "SelectedOption" seçeneğini belirleyin](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-approval-response.png)

   1. Orta karşılaştırma kutusunda, işleç **eşit olduğunu** seçin.

   1. Koşulun sağ tarafındaki **bir değer** kutusu seç kutusuna şu metni girin:`Approve`

      İşi bittiğinde, durum aşağıdaki örneğe benzer:

      ![Onaylanan örnek için bitmiş koşul](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-approval-response-2.png)

1. Mantıksal uygulamanızı kaydedin.

Ardından, gözden geçiren bir isteği onayladığında mantıksal uygulamanızın gerçekleştireceği eylemi belirtin. 

## <a name="add-member-to-mailchimp-list"></a>MailChimp listesine üye ekleme

Şimdi, onaylanan üyeyi posta listenize ekleyen bir eylem ekleyin.

1. If **true** dalında eylem **ekle'yi**seçin.

1. Bir **eylem seçin** `mailchimp` , filtreolarak girin ve liste eylemi **için üye ekle'yi** seçin.

   !["Üyeyi listeye ekle" eylemini seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-mailchimp-add-member.png)

1. MailChimp hesabınıza erişim için istenirse, MailChimp kimlik bilgilerinizle oturum açın.

1. Burada gösterildiği ve açıklandığı şekilde bu eylem hakkında bilgi sağlayın:

   !["Listeye üye ekle" için bilgileri sağlayın](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-mailchimp-add-member-settings.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Liste Kimliği** | Evet | `test-members-ML` | MailChimp posta listenizin adı. Bu örnekte "test üyeleri-ML" kullanır. |
   | **Durum** | Evet | `subscribed` | Yeni üyenin abonelik durumunu seçin. Bu örnekte "abone" kullanır. <p>Daha fazla bilgi için bkz. [MailChimp API'siyle aboneleri yönetme](https://developer.mailchimp.com/documentation/mailchimp/guides/manage-subscribers-with-the-mailchimp-api/). |
   | **E-posta Adresi** | Evet | <*yeni üye-e-posta adresi*> | Dinamik içerik listesinden, yeni üyenin e-posta adresine geçen **yeni bir posta geldiğinde**Alttan'ı seçin. **From** |
   ||||

   Bu eylemin özellikleri hakkında daha fazla bilgi için [MailChimp bağlayıcı sı'na](https://docs.microsoft.com/connectors/mailchimp/)bakın.

1. Mantıksal uygulamanızı kaydedin.

Ardından, yeni üyenin posta listenize başarıyla katılıp katılmadığını denetlerken kullanabileceğiniz bir koşul ekleyin. Bu şekilde, mantıksal uygulamanız bu işlemin başarılı veya başarısız olduğunu size bildirir.

## <a name="check-for-success-or-failure"></a>Başarı veya başarısızlık durumunu denetleme

1. Gerçek **Olan** dalda, **liste eylemine üye ekle** altında **eylem ekle'yi**seçin.

1. Bir **eylem seçin,** **Dahili'yi**seçin. Arama kutusuna filtreniz olarak girin. `condition` Eylemler listesinden **Koşul'u**seçin.

1. Koşulu şu açıklama ile yeniden adlandırın: `If add member succeeded`

1. Onaylanan üyenin posta listenize katılımının başarılı mı yoksa başarısız mı olduğunu denetleyen bir koşul oluşturun:

   1. Bu durumda, koşulun sol tarafında bulunan bir değer kutusu **seçin'in** içini tıklatın. Dinamik içerik listesinden, **üyeyi listeye ekle**altında **Durum** özelliğini seçin.

      Örneğin, durumunuz şu örnekte görünür:

      !["Listeye üye ekle" öğesinin altında "Durum" seçin](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-added-member.png)

   1. Orta karşılaştırma kutusunda, işleç **eşit olduğunu** seçin.

   1. Koşulun sağ tarafındaki **bir değer** kutusu seç kutusuna şu metni girin:`subscribed`

      İşi bittiğinde, durum aşağıdaki örneğe benzer:

      ![Abone olunan örnek için bitmiş koşul](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-added-member-2.png)

Ardından, onaylanan üyenin posta listenize katılımının başarılı veya başarısız olması durumunda gönderilecek e-postaları ayarlayın.

## <a name="send-email-if-member-added"></a>Üye eklendiyse e-posta gönderme

1. If **üye başarılı** koşulu altında, **If true** dalında, **eylem ekle'yi**seçin.

   !["Doğruysa" dalında "Eylem ekle" seçeneğini belirleyin](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success.png)

1. Arama kutusuna **bir eylem seçin,** filtreniz olarak girin `outlook send email` ve **e-posta gönder** eylemini seçin.

   !["E-posta gönder" eylemi ekleme](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success-2.png)

1. Eylemi şu açıklama ile yeniden adlandırın: `Send email on success`

1. Bu eylem için gösterildiği ve açıklandığı gibi bilgi sağlayın:

   ![Başarılı oldu e-postası için bilgi sağlama](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success-settings.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Hedef** | Evet | <*e-posta adresiniz*> | Başarı e-postasının gönderileceği e-posta adresi. Test için kendi e-posta adresinizi kullanabilirsiniz. |
   | **Konu** | Evet | <*başarı için konu-e-posta*> | Başarı e-postasının konusu. Bu öğretici için şu metni girin: <p>`Success! Member added to "test-members-ML": ` <p>Dinamik içerik listesinden, **üye yi listeye ekle**altında, **E-posta Adresi** özelliğini seçin. |
   | **Gövde** | Evet | <*vücut-başarı-e-posta için*> | Başarı e-postasının gövde içeriği. Bu öğretici için şu metni girin: <p>`New member has joined "test-members-ML":` <p>Dinamik içerik listesinden **E-posta Adresi** özelliğini seçin. <p>Bir sonraki satırda şu metni girin:`Member opt-in status: ` <p> Dinamik içerik listesinden, **üyeyi listeye ekle**altında **Durum** özelliğini seçin. |
   |||||

1. Mantıksal uygulamanızı kaydedin.

## <a name="send-email-if-member-not-added"></a>Üye eklenmediyse e-posta gönderme

1. If **üye ekle** koşulu altında, **If false** dalında **eylem ekle'yi**seçin.

   !["False if" dalında "Eylem ekle" seçeneğini belirleyin](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed.png)

1. Arama kutusuna **bir eylem seçin,** filtreniz olarak girin `outlook send email` ve **e-posta gönder** eylemini seçin.

   !["E-posta gönder" eylemini ekleme](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed-2.png)

1. Eylemi şu açıklama ile yeniden adlandırın: `Send email on failure`

1. Burada gösterildiği ve açıklandığı şekilde bu eylem hakkında bilgi sağlayın:

   ![Başarısız oldu e-postası için bilgi sağlama](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed-settings.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Hedef** | Evet | <*e-posta adresiniz*> | Başarısızlık e-postasının gönderileceği e-posta adresi. Test için kendi e-posta adresinizi kullanabilirsiniz. |
   | **Konu** | Evet | <*hata için özne-e-posta*> | Başarısızlık e-postasının konusu. Bu öğretici için şu metni girin: <p>`Failed, member not added to "test-members-ML": ` <p>Dinamik içerik listesinden, **üye yi listeye ekle**altında, **E-posta Adresi** özelliğini seçin. |
   | **Gövde** | Evet | <*vücut-for-failure-e-posta*> | Başarısızlık e-postasının gövde içeriği. Bu öğretici için şu metni girin: <p>`Member might already exist. Check your MailChimp account.` |
   |||||

1. Mantıksal uygulamanızı kaydedin. 

Ardından mantıksal uygulamanızı test edin; mantıksal uygulamanız şu örnek gibi görünür:

![Örnek bitmiş mantık uygulaması iş akışı](./media/tutorial-process-mailing-list-subscriptions-workflow/tutorial-high-level-completed.png)

## <a name="run-your-logic-app"></a>Mantıksal uygulamanızı çalıştırın

1. Posta listenize katılmak için kendinize bir e-posta isteği gönderin. İsteğin gelen kutunuzda gösterilmesini bekleyin.

1. Mantık uygulamanızı el ile başlatmak için, tasarımcı araç çubuğunda **Çalıştır'ı**seçin. 

   E-postanızın konusu tetikleyicinin konu filtresiyle eşleşiyorsa, mantıksal uygulamanız abonelik isteğini onaylamak için size bir e-posta gönderir.

1. Onay e-postasında **Onayla'yı**seçin.

1. Abonenin e-posta adresi posta listenizde yoksa, mantıksal uygulamanız bu kişinin e-posta adresini ekler ve size şu örnekteki gibi bir e-posta gönderir:

   ![Örnek e-posta - başarılı abonelik](./media/tutorial-process-mailing-list-subscriptions-workflow/add-member-mailing-list-success.png)

   Mantıksal uygulamanız aboneyi ekleyemezse, şu örnekteki gibi bir e-posta alırsınız:

   ![Örnek e-posta - başarısız abonelik](./media/tutorial-process-mailing-list-subscriptions-workflow/add-member-mailing-list-failed.png)

   E-posta gelmezse istenmeyen e-posta klasörüne bakın. E-postanızın istenmeyen posta filtresi bu tür postaları yeniden yönlendirebilir. Mantıksal uygulamanızın düzgün bir şekilde çalışıp çalışmadığından emin değilseniz bkz. [Mantıksal uygulama sorunlarını giderme](../logic-apps/logic-apps-diagnosing-failures.md).

Tebrikler, Azure, Microsoft hizmetleri ve diğer SaaS uygulamaları arasında bilgileri tümleştiren bir mantıksal uygulama oluşturdunuz ve çalıştırdınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Örnek mantık uygulamasına artık ihtiyacınız olmadığında, mantık uygulamanızı ve ilgili kaynakları içeren kaynak grubunu silin. 

1. Ana Azure menüsünde **Kaynak grupları**’na gidin ve mantıksal uygulamanızın kaynak grubunu seçin.

1. Kaynak grubu menüsünde, Kaynak grubunu **genel bakış** > **silme'yi**seçin. 

   !["Genel Bakış" > "Kaynak grubunu sil"](./media/tutorial-process-mailing-list-subscriptions-workflow/delete-resource-group.png)

1. Kaynak grup adını onay olarak girin ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, posta listesi istekleri için onayları yöneten bir mantıksal uygulama oluşturdunuz. Şimdi, Azure Depolama ve Azure İşlevleri gibi Azure hizmetlerini tümleştirerek e-posta eklerini işleyen ve depolayan bir mantıksal uygulama oluşturmayı öğrenin.

> [!div class="nextstepaction"]
> [E-posta eklerini işleme](../logic-apps/tutorial-process-email-attachments-workflow.md)
