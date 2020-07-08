---
title: Sıralı bir konvoy kullanarak bağıntılı iletileri sırayla gönderin
description: Azure Service Bus ile Azure Logic Apps içindeki sıralı konvoy düzenini kullanarak ilgili iletileri sırayla gönderin
services: logic-apps
ms.suite: integration
ms.reviewer: apseth, divswa, logicappspm
ms.topic: conceptual
ms.date: 05/29/2020
ms.openlocfilehash: bd6b05489d13f835de4dce2aa3d885132285efca
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84987603"
---
# <a name="send-related-messages-in-order-by-using-a-sequential-convoy-in-azure-logic-apps-with-azure-service-bus"></a>Azure Service Bus ile Azure Logic Apps sıralı bir konvoy kullanarak ilgili iletileri sırayla gönderin

Bağıntılı iletileri belirli bir sırada göndermeniz gerektiğinde, [Azure Service Bus bağlayıcısını](../connectors/connectors-create-api-servicebus.md)kullanarak [Azure Logic Apps](../logic-apps/logic-apps-overview.md) kullanırken [ *sıralı konvoy* düzenini](https://docs.microsoft.com/azure/architecture/patterns/sequential-convoy) takip edebilirsiniz. Bağıntılı iletiler, Service Bus [oturum](../service-bus-messaging/message-sessions.md) kimliği gibi bu iletiler arasındaki ilişkiyi tanımlayan bir özelliğe sahiptir.

Örneğin, "oturum 1" adlı bir oturum için 10 iletiniz olduğunu ve aynı [Service Bus kuyruğuna](../service-bus-messaging/service-bus-queues-topics-subscriptions.md)gönderilen "oturum 2" adlı bir oturum için 5 iletiniz olduğunu varsayalım. Sıradaki iletileri işleyen bir mantıksal uygulama oluşturabilirsiniz, böylece "oturum 1" kaynağından gelen tüm iletiler tek bir tetikleyici çalıştırması tarafından işlenir ve "oturum 2" kaynağından gelen tüm iletiler sonraki tetikleyici çalıştırması tarafından işlenir.

![Genel sıralı konvoy kalıbı](./media/send-related-messages-sequential-convoy/sequential-convoy-pattern-general.png)

Bu makalede **Service Bus oturumları şablonunu kullanarak bağıntılı sıralı teslim** kullanarak bu düzeni uygulayan bir mantıksal uygulamanın nasıl oluşturulacağı gösterilmektedir. Bu şablon, bir **sıraya (Peek kilidi) bir ileti alındığında** , bir [Service Bus kuyruğundan](../service-bus-messaging/service-bus-queues-topics-subscriptions.md)iletileri alan Service Bus Bağlayıcısı ile başlayan bir mantıksal uygulama iş akışını tanımlar. Bu mantıksal uygulamanın gerçekleştirdiği üst düzey adımlar şunlardır:

* Tetikleyicinin Service Bus sırasından okuduğu bir iletiye göre oturum başlatın.

* Geçerli iş akışı çalıştırması sırasında kuyruktaki aynı oturumdaki tüm iletileri okuyun ve işleyin.

Bu şablonun JSON dosyasını gözden geçirmek için bkz. [GitHub: service-bus-sessions.json](https://github.com/Azure/logicapps/blob/master/templates/service-bus-sessions.json).

Daha fazla bilgi için bkz. [sıralı konvoy deseni-Azure mimarisi bulut tasarım desenleri](https://docs.microsoft.com/azure/architecture/patterns/sequential-convoy).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Mantıksal uygulamanızda kullanacağınız bir mesajlaşma varlığı olan bir Service Bus ad alanı ve [Service Bus sırası](../service-bus-messaging/service-bus-queues-topics-subscriptions.md). Bu öğelerin ve mantıksal uygulamanızın aynı Azure aboneliğini kullanması gerekir. Kuyruğunuzu oluştururken **oturumları etkinleştir** ' i seçtiğinizden emin olun. Bu öğeleriniz yoksa, [Service Bus ad alanınızı ve bir kuyruğu oluşturmayı](../service-bus-messaging/service-bus-create-namespace-portal.md)öğrenin.

  [!INCLUDE [Warning about creating infinite loops](../../includes/connectors-infinite-loops.md)]

* Mantıksal uygulamalar oluşturma hakkında temel bilgi. Azure Logic Apps yeni başladıysanız hızlı başlangıcı deneyin, [ilk otomatikleştirilmiş iş akışınızı oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="permissions-connection-string"></a>

## <a name="check-access-to-service-bus-namespace"></a>Service Bus ad alanına erişimi denetle

Mantıksal uygulamanızın Service Bus ad alanına erişim izinleri olup olmadığından emin değilseniz, bu izinleri onaylayın.

1. [Azure portalında](https://portal.azure.com) oturum açın. Service Bus *ad*alanınızı bulun ve seçin.

1. Ad alanı menüsünde, **Ayarlar**altında, **paylaşılan erişim ilkeleri**' ni seçin. **Talepler**altında, bu ad alanı Için izinleri **yönetme** izinlerine sahip olup olmadığınızı kontrol edin.

   ![Service Bus ad alanı için izinleri yönetme](./media/send-related-messages-sequential-convoy/check-service-bus-permissions.png)

1. Şimdi Service Bus ad alanınız için bağlantı dizesini alın. Bu dizeyi daha sonra mantıksal uygulamanızdan ad alanına bir bağlantı oluşturduğunuzda kullanabilirsiniz.

   1. **Paylaşılan erişim ilkeleri** bölmesinde, **Ilke**altında, **RootManageSharedAccessKey**' yi seçin.
   
   1. Birincil bağlantı dizeniz yanındaki Kopyala düğmesini seçin. Bağlantı dizesini daha sonra kullanmak üzere kaydedin.

      ![Service Bus ad alanı bağlantı dizesini Kopyala](./media/send-related-messages-sequential-convoy/copy-service-bus-connection-string.png)

   > [!TIP]
   > Bağlantı dizeniz Service Bus ad alanınız veya bir kuyruk gibi bir mesajlaşma varlığı ile ilişkili olup olmadığını doğrulamak için, parametre için bağlantı dizesini arayın `EntityPath`   . Bu parametreyi bulursanız, bağlantı dizesi belirli bir varlık içindir ve mantıksal uygulamanızla birlikte kullanılacak doğru dize değildir.

## <a name="create-logic-app"></a>Mantıksal uygulama oluşturma

Bu bölümde, bu iş akışı modelini uygulamaya yönelik tetikleyiciyi ve eylemleri içeren **Service Bus oturumları şablonunu kullanarak bağıntılı sıralı teslimi** kullanarak bir mantıksal uygulama oluşturursunuz. Ayrıca, Service Bus ad alanına bir bağlantı oluşturur ve kullanmak istediğiniz Service Bus sırasının adını belirtmeniz gerekir.

1. [Azure Portal](https://portal.azure.com), boş bir mantıksal uygulama oluşturun. Azure giriş sayfasında, **kaynak**  >  **tümleştirme**  >  **mantıksal uygulaması**oluştur ' u seçin.

1. Şablon Galerisi görüntülendikten sonra video ve ortak Tetikleyiciler bölümlerinin ötesine ilerleyin. **Şablonlar** bölümünde, **Service Bus oturumlarını kullanarak ilişkili sıralı teslim olan**şablonu seçin.

   !["Hizmet veri yolu oturumlarını kullanarak bağıntılı sıralı teslim" şablonunu seçme](./media/send-related-messages-sequential-convoy/select-correlated-in-order-delivery-template.png)

1. Onay kutusu göründüğünde, **Bu şablonu kullan**' ı seçin.

1. Mantıksal uygulama Tasarımcısı ' nda, **Service Bus** şeklinin içinde **devam**' ı seçin ve ardından şekil içinde görüntülenen artı işaretini ( **+** ) seçin.

   ![Azure Service Bus bağlanmak için "devam" ı seçin](./media/send-related-messages-sequential-convoy/connect-to-service-bus.png)

1. Şimdi iki seçenekten birini seçerek bir Service Bus bağlantısı oluşturun:

   * Daha önce Service Bus ad alanından kopyaladığınız bağlantı dizesini kullanmak için şu adımları izleyin:

     1. **Bağlantı bilgilerini el ile gir**' i seçin.

     1. **Bağlantı adı**için bağlantınız için bir ad girin. **Bağlantı dizesi**için, ad alanı Bağlantı dizenizi yapıştırın ve **Oluştur**' u seçin, örneğin:

        ![Bağlantı adı ve Service Bus bağlantı dizesi girin](./media/send-related-messages-sequential-convoy/provide-service-bus-connection-string.png)

        > [!TIP]
        > Bu bağlantı dizesine sahip değilseniz, [Service Bus ad alanı bağlantı dizesinin nasıl bulunacağını ve kopyalanacağını](#permissions-connection-string)öğrenin.

   * Geçerli Azure aboneliğinizden bir Service Bus ad alanı seçmek için şu adımları izleyin:

     1. **Bağlantı adı**için bağlantınız için bir ad girin. **Service Bus ad alanı**için Service Bus ad alanınızı seçin, örneğin:

        ![Bağlantı adı girin ve Service Bus ad alanını seçin](./media/send-related-messages-sequential-convoy/create-service-bus-connection.png)

     1. Sonraki bölme göründüğünde Service Bus ilkenizi seçin ve **Oluştur**' u seçin.

        ![Service Bus ilkesi seçin ve sonra "Oluştur"](./media/send-related-messages-sequential-convoy/create-service-bus-connection-2.png)

1. İşiniz bittiğinde **devam**' ı seçin.

   Logic App Designer artık, düzeni izleyen hata işlemeyi uygulayan iki kapsam de dahil olmak üzere bir tetikleyici ve eylemlerle önceden doldurulmuş bir iş akışı içeren **Service Bus oturumları şablonunu kullanarak bağıntılı, ilişkili sıralı teslimi** gösterir `Try-Catch` .

Artık şablondaki tetikleyici ve eylemler hakkında daha fazla bilgi alabilir ya da [mantıksal uygulama şablonu değerlerini sağlamak](#complete-template)için ilerleyebilirsiniz.

<a name="template-summary"></a>

## <a name="template-summary"></a>Şablon Özeti

Ayrıntılar daraltıldığında **Service Bus oturumları şablonu kullanılarak bağıntılı sıralı teslimdeki** en üst düzey iş akışı aşağıda verilmiştir:

![Şablonun en üst düzey iş akışı](./media/send-related-messages-sequential-convoy/template-top-level-flow.png)

| Name | Açıklama |
|------|-------------|
| **`When a message is received in a queue (peek-lock)`** | Bu Service Bus tetikleyicisi, belirtilen tekrara göre, herhangi bir ileti için belirtilen Service Bus sırasını denetler. Kuyrukta bir ileti varsa, tetikleyici ateşlenir ve bir iş akışı örneği oluşturur ve çalıştırır. <p><p>*Peek-kilit* terimi, tetikleyicinin kuyruktan ileti almak için bir istek gönderdiği anlamına gelir. Bir ileti varsa, tetikleyici kilit süresi doluncaya kadar bu iletide başka bir işlem gerçekleşmemesi için iletiyi alır ve kilitler. Ayrıntılar için [oturumu başlatın](#initialize-session). |
| **`Init isDone`** | Bu [ **değişken başlatma** eylemi](../logic-apps/logic-apps-create-variables-store-values.md#initialize-variable) , olarak ayarlanmış bir Boole değişkeni oluşturur `false` ve aşağıdaki koşulların doğru olduğunu gösterir: <p><p>-Oturumda daha fazla ileti okunabilir durumda değil. <br>-Geçerli iş akışı örneğinin tamamlanabilmesi için oturum kilidinin artık yenilenmesi gerekmez. <p><p>Ayrıntılar için bkz. [oturumu başlatma](#initialize-session). |
| **`Try`** | Bu [ **kapsam** eylemi](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md) , bir iletiyi işlemek için çalıştırılan eylemleri içerir. Kapsamda bir sorun oluşursa `Try` , sonraki `Catch` **kapsam** eylemi bu sorunu işler. Daha fazla bilgi için bkz. ["TRY" scope](#try-scope). |
| **`Catch`**| Bu [ **kapsam** eylemi](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md) , önceki kapsamda bir sorun oluşursa çalıştırılan eylemleri içerir `Try` . Daha fazla bilgi için bkz. ["Catch" scope](#catch-scope). |
|||

<a name="try-scope"></a>

### <a name="try-scope"></a>"Dene" kapsamı

`Try`Ayrıntılar daraltıldığında [kapsam eyleminde](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md) en üst düzey akış aşağıda verilmiştir:

!["Dene" kapsam eylemi iş akışı](./media/send-related-messages-sequential-convoy/try-scope-action.png)

| Name | Açıklama |
|------|-------------|
| **`Send initial message to topic`** | Bu eylemi, kuyruktaki oturumdan ilk iletiyi işlemek istediğiniz herhangi bir eylemle değiştirebilirsiniz. Oturum KIMLIĞI, oturumu belirtir. <p><p>Bu şablon için, bir Service Bus eylemi ilk iletiyi bir Service Bus konusuna gönderir. Ayrıntılar için bkz. [ilk Iletiyi işleme](#handle-initial-message). |
| (paralel dal) | Bu [paralel dal eylemi](../logic-apps/logic-apps-control-flow-branches.md) iki yol oluşturur: <p><p>-Branch #1: iletiyi işlemeye devam edin. Daha fazla bilgi için bkz. [Branch #1: kuyruktaki ilk Iletiyi Tamamdır](#complete-initial-message). <p><p>-Branch #2: bir sorun yanlış olursa iletiyi bırakın ve başka bir tetikleyici çalıştırması tarafından toplama için yayın yayınlayın. Daha fazla bilgi için bkz. [Branch #2: kuyruktaki ilk Iletiyi bırakma](#abandon-initial-message). <p><p>Her iki yol daha sonra **bir kuyruktaki kapatma oturumunda ve** sonraki satırda açıklanan başarılı bir eylemde katılır. |
| **`Close a session in a queue and succeed`** | Bu Service Bus eylem, daha önce açıklanan dalları birleştirir ve aşağıdaki olaylardan biri gerçekleştiğinde kuyruktaki oturumu kapatır: <p><p>-İş akışı kuyruktaki kullanılabilir iletileri işlemeyi sonlandırır. <br>-İş akışı, bir sorun oluştuğundan ilk iletiyi terk ettiğinden. <p><p>Ayrıntılar için bkz. [bir kuyruktaki oturumu kapatma ve başarılı oldu](#close-session-succeed). |
|||

<a name="complete-initial-message"></a>

#### <a name="branch-1-complete-initial-message-in-queue"></a>Dal #1: kuyruktaki ilk iletiyi doldurun

| Name | Açıklama |
|------|-------------|
| `Complete initial message in queue` | Bu Service Bus eylemi başarıyla alınan bir iletiyi tamamlandı olarak işaretler ve yeniden işlemeyi engellemek için iletiyi kuyruktan kaldırır. Ayrıntılar için bkz. [ilk Iletiyi işleme](#handle-initial-message). |
| `While there are more messages for the session in the queue` | Bu, ileti varken veya bir saat geçtiğinde ileti almaya devam [ **edene kadar** ](../logic-apps/logic-apps-control-flow-loops.md#until-loop) . Bu döngüdeki eylemler hakkında daha fazla bilgi için bkz. [kuyruktaki oturum için daha fazla ileti var](#while-more-messages-for-session). |
| **`Set isDone = true`** | Daha fazla ileti yoksa, bu [ **değişken ayarla** eylemi](../logic-apps/logic-apps-create-variables-store-values.md#set-variable) olarak ayarlanır `isDone` `true` . |
| **`Renew session lock until cancelled`** | Bu [ **until** döngüsü](../logic-apps/logic-apps-control-flow-loops.md#until-loop) , oturum kilidinin, iletiler mevcut iken veya bir saat geçene kadar bu mantıksal uygulama tarafından tutulduğundan emin olur. Bu döngüdeki eylemler hakkında daha fazla bilgi için bkz. [iptal edilene kadar oturum kilitlemeyi yenileme](#renew-session-while-messages-exist). |
|||

<a name="abandon-initial-message"></a>

#### <a name="branch-2-abandon-initial-message-from-the-queue"></a>Dal #2: kuyruktaki ilk iletiyi bırakma

İlk iletiyi işleyen eylem başarısız olursa, Service Bus eylemi, **Sıradaki ilk Iletiyi iptal**etmek için başka bir iş akışı örneği çalıştırması için iletiyi serbest bırakır. Ayrıntılar için bkz. [ilk Iletiyi işleme](#handle-initial-message).

<a name="catch-scope"></a>

### <a name="catch-scope"></a>"Catch" kapsamı

Kapsamdaki eylemler başarısız olursa `Try` , mantıksal uygulamanın oturumu yine de kapatması gerekir. Kapsam eylemi, kapsam eylemi,,, `Catch` [scope action](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md) `Try` veya durumu ile sonuçlanışında `Failed` çalışır `Skipped` `TimedOut` . Kapsam, sorunun gerçekleştiği oturum KIMLIĞINI içeren bir hata mesajı döndürür ve mantıksal uygulamayı sonlandırır.

Ayrıntılar daraltıldığında kapsam eyleminde en üst düzey akış aşağıda verilmiştir `Catch` :

!["Catch" kapsam eylemi iş akışı](./media/send-related-messages-sequential-convoy/catch-scope-action.png)

| Name | Açıklama |
|------|-------------|
| **`Close a session in a queue and fail`** | Bu Service Bus eylem, oturum kilidinin açık kalması için kuyruktaki oturumu kapatır. Ayrıntılar için bkz. [kuyruktaki oturumu kapatma ve başarısız](#close-session-fail). |
| **`Find failure msg from 'Try' block`** | Bu [ **filtre dizisi** eylemi](../logic-apps/logic-apps-perform-data-operations.md#filter-array-action) , `Try` belirtilen ölçütlere göre kapsam içindeki tüm eylemlerin giriş ve çıktılarından bir dizi oluşturur. Bu durumda, bu eylem durum sonucu oluşan eylemlerden çıkışları geri döndürür `Failed` . Ayrıntılar için bkz. [' TRY ' bloğundan hata Iletisi bulma](#find-failure-message). |
| **`Select error details`** | Bu [ **seçim** eylemi](../logic-apps/logic-apps-perform-data-operations.md#select-action) , belirtilen ölçütlere göre JSON nesnelerini içeren bir dizi oluşturur. Bu JSON nesneleri, önceki eylem tarafından oluşturulan dizideki değerlerden oluşturulur `Find failure msg from 'Try' block` . Bu durumda, bu eylem, önceki eylemden döndürülen hata ayrıntılarından oluşturulan JSON nesnesini içeren bir dizi döndürür. Ayrıntılar için bkz. [hata ayrıntılarını seçme](#select-error-details). |
| **`Terminate`** | Bu [ **sonlandırma** eylemi](../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action) iş akışı için çalışmayı sonlandırır, devam etmekte olan tüm eylemleri iptal eder, kalan tüm eylemleri atlar ve BELIRTILEN durumu, oturum kimliğini ve hata sonucunu eylem sonucu döndürür `Select error details` . Ayrıntılar için bkz. [mantıksal uygulamayı Sonlandır](#terminate-logic-app). |
|||

<a name="complete-template"></a>

## <a name="complete-the-template"></a>Şablonu doldurun

**Service Bus oturumları şablonu kullanılarak bağıntılı sıralı teslim** içindeki tetikleyici ve eylemlerin değerlerini sağlamak için aşağıdaki adımları izleyin. Mantıksal uygulamanızı kaydedebilmeniz için önce bir yıldız işareti () ile işaretlenen tüm gerekli değerleri sağlamanız gerekir **\*** .

<a name="initialize-session"></a>

### <a name="initialize-the-session"></a>Oturumu başlatın

* **Bir kuyruk (Peek kilidi) tetikleyicisinde bir ileti alındığında** , bu bilgileri, şablonun **oturum kimliği** özelliğini kullanarak bir oturumu başlatabilmesi için sağlayın, örneğin:

  !["Kuyruktaki bir ileti alındığında (Peek-kilit)" için tetikleyici ayrıntılarını Service Bus "](./media/send-related-messages-sequential-convoy/service-bus-check-message-peek-lock-trigger.png)

  > [!NOTE]
  > Başlangıçta, mantıksal uygulamanın beklediğinizden daha sık çalışması ve beklenmeyen faturalandırma ücretleri ile sonuçlanması için yoklama aralığı üç dakikaya ayarlanır. İdeal olarak, bir ileti geldiğinde mantıksal uygulamanın hemen tetiklemesini sağlamak için aralığı ve sıklığı 30 saniyeye ayarlayın.

  | Özellik | Bu senaryo için gerekli | Değer | Açıklama |
  |----------|----------------------------|-------|-------------|
  | **Kuyruk adı** | Evet | <*sıra-adı*> | Daha önce oluşturduğunuz Service Bus kuyruğunun adı. Bu örnekte "Fabrikam-Service-Bus-Queue" kullanılmaktadır. |
  | **Sıra türü** | Evet | **Ana** | Birincil Service Bus kuyruğunuz |
  | **Oturum kimliği** | Evet | **Sonraki kullanılabilir** | Bu seçenek, Service Bus sırasındaki iletiden oturum KIMLIĞI temel alınarak her tetikleyici çalıştırması için bir oturum alır. Oturum, başka bir mantıksal uygulama veya başka bir istemcinin bu oturumla ilgili iletileri işleyebilmesi için de kilitlenir. İş akışının sonraki eylemleri, bu makalenin ilerleyen kısımlarında açıklandığı gibi, bu oturumla ilişkili tüm iletileri işler. <p><p>Diğer **oturum kimliği** seçenekleri hakkında daha fazla bilgi aşağıda verilmiştir: <p>- **Hiçbiri**: hiçbir oturum yok ve sıralı konvoy deseninin uygulanması için kullanılamayan varsayılan seçenektir. <p>- **Özel değer girin**: kullanmak ISTEDIĞINIZ oturum kimliğini bildiğiniz ve her zaman bu oturum kimliği için tetikleyiciyi çalıştırmak istediğiniz zaman bu seçeneği kullanın. <p>**Note**: Service Bus bağlayıcısı, Azure Service Bus ile bağlayıcı önbelleğine sınırlı sayıda benzersiz oturum kaydedebilir. Oturum sayısı bu sınırı aşarsa, eski oturumlar önbellekten kaldırılır. Daha fazla bilgi için bkz. [Azure Logic Apps ile buluttaki Exchange iletileri ve Azure Service Bus](../connectors/connectors-create-api-servicebus.md#connector-reference). |
  | **Interval** | Evet | <*Aralık sayısı*> | Bir iletiyi denetlemeden önce Yinelenmeler arasındaki zaman birimi sayısı. |
  | **Sıklık** | Evet | **İkinci**, **dakika**, **saat**, **gün**, **hafta**veya **ay** | Bir ileti denetlenirken yinelenme için zaman birimi. <p>**İpucu**: bir **saat dilimi** veya **Başlangıç saati**eklemek için **yeni parametre Ekle** listesinden bu özellikleri seçin. |
  |||||

  Daha fazla tetikleyici bilgisi için bkz. [Service Bus-bir kuyrukta ileti alındığında (Peek-kilit)](https://docs.microsoft.com/connectors/servicebus/#when-a-message-is-received-in-a-queue-(peek-lock)). Tetikleyici bir [Servicebusmessage](https://docs.microsoft.com/connectors/servicebus/#servicebusmessage)çıkışı verir.

Oturum başlatıldıktan sonra, iş akışı başlangıçta ayarlanmış bir Boole değişkeni oluşturmak için **değişkeni Başlat** eylemini kullanır `false` ve aşağıdaki koşulların doğru olduğunu gösterir: 

* Oturumda daha fazla ileti okunabilir durumda değil.

* Geçerli iş akışı örneğinin tamamlanabilmesi için oturum kilidinin artık yenilenmesi gerekiyor.

!["Init Isdone" için "değişken Başlat" eylem ayrıntıları](./media/send-related-messages-sequential-convoy/init-is-done-variable.png)

Sonra, **TRY** bloğunda, iş akışı okunan ilk ileti üzerinde eylemler gerçekleştirir.

<a name="handle-initial-message"></a>

### <a name="handle-the-initial-message"></a>İlk iletiyi işle

İlk eylem, bir yer tutucu Service Bus eylemdir, ilk **iletiyi**kuyruktaki oturumdan ilk iletiyi işlemek istediğiniz herhangi bir eylem ile değiştirebilirsiniz. Oturum KIMLIĞI, iletinin kaynaklandığı oturumu belirtir.

Yer tutucu Service Bus eylemi, ilk iletiyi **oturum kimliği** özelliği tarafından belirtilen bir Service Bus konusuna gönderir. Bu şekilde, belirli bir oturumla ilişkili tüm iletiler aynı konuya gider. Bu şablondaki sonraki eylemlerin tüm **oturum kimliği** özellikleri aynı oturum kimliği değerini kullanır.

!["İlk iletiyi konuya gönder" için eylem ayrıntılarını Service Bus](./media/send-related-messages-sequential-convoy/send-initial-message-to-topic-action.png)

1. Service Bus eyleminde, **Sıradaki ilk Iletiyi doldurun**, Service Bus kuyruğunuzun adını sağlayın ve tüm diğer varsayılan özellik değerlerini eylemde tutun.

   !["Kuyruktaki ilk ileti tamamı" için eylem ayrıntılarını Service Bus](./media/send-related-messages-sequential-convoy/complete-initial-message-queue.png)

1. Service Bus eyleminde, **kuyruktaki ilk Iletiyi iptal**edin, Service Bus kuyruğunuzun adını sağlayın ve tüm diğer varsayılan özellik değerlerini eylemde tutun.

   !["İlk iletiyi kuyruktan bırak" eylemi ayrıntılarını Service Bus](./media/send-related-messages-sequential-convoy/abandon-initial-message-from-queue.png)

Sonra, **sırasıyla tüm ilk iletiyi** izleyen eylemler için gerekli bilgileri sağlarsınız. **Kuyruk döngüsünde oturum için daha fazla ileti** varken, içindeki eylemlerle karşılaşırsınız.

<a name="while-more-messages-for-session"></a>

### <a name="while-there-are-more-messages-for-the-session-in-the-queue"></a>Kuyrukta oturum için daha fazla ileti var

Bu [ **until** döngüsü](../logic-apps/logic-apps-control-flow-loops.md#until-loop) , iletiler kuyrukta mevcut olduğunda veya bir saat geçtiğinde bu eylemleri çalıştırır. Döngünün zaman sınırını değiştirmek için döngünün **zaman aşımı** özellik değerini düzenleyin.

* İletiler mevcut durumdayken kuyruktan ek iletiler alın.

* Kalan ileti sayısını denetleyin. İletiler hala varsa, iletileri işlemeye devam edin. Daha fazla ileti yoksa, iş akışı `isDone` değişkeni olarak ayarlar `true` ve döngüden çıkar.

![Kuyruk sırasında döngü-Işlem iletileri tamamlanana kadar](./media/send-related-messages-sequential-convoy/while-more-messages-for-session-in-queue.png)

1. Service Bus eyleminde **oturumdan ek Iletiler alın**, Service Bus kuyruğunuzun adını sağlayın. Aksi takdirde, eylemde tüm diğer varsayılan özellik değerlerini saklayın.

   > [!NOTE]
   > Varsayılan olarak, en fazla ileti sayısı olarak ayarlanır `175` , ancak bu sınır Service Bus ileti boyutundan ve en büyük ileti boyutu özelliğinden etkilenir. Daha fazla bilgi için bkz. [bir kuyruğun ileti boyutu](../service-bus-messaging/service-bus-quotas.md).

   ![Service Bus Action-"oturumdan ek iletiler al"](./media/send-related-messages-sequential-convoy/get-additional-messages-from-session.png)

   Sonra, iş akışı şu paralel dallara böler:

   * Ek iletileri denetlerken bir hata veya hata oluşursa, `isDone` değişkenini olarak ayarlayın `true` .

   * **Herhangi bir koşul olursa işlem iletileri** , kalan ileti sayısının sıfır olup olmadığını denetler. Yanlış ve daha fazla ileti varsa işleme devam edin. True ise ve daha fazla ileti yoksa, iş akışı `isDone` değişkenini olarak ayarlar `true` .

   ![Koşul-varsa iletileri Işle](./media/send-related-messages-sequential-convoy/process-messages-if-any.png)

   **Eğer false ise** , **her döngü için** bir, her bir döngüsü ilk ın, Ilk çıkar sırasıyla (FIFO) işler. Döngünün **ayarlarında** **eşzamanlılık denetim** ayarı olarak ayarlanır `1` , bu nedenle aynı anda yalnızca tek bir ileti işlenir.

   !["For each" döngüsü-her iletiyi tek seferde Işle](./media/send-related-messages-sequential-convoy/for-each-additional-message.png)

1. Service Bus eylemler için **bir kuyruktaki Iletiyi tamamlayıp** **bir kuyruktaki iletiyi iptal**edin, Service Bus kuyruğunuzun adını belirtin.

   ![Service Bus eylemler-"iletiyi bir kuyrukta doldurun" ve "iletiyi bir kuyruktaki bırakma"](./media/send-related-messages-sequential-convoy/abandon-or-complete-message-in-queue.png)

   **Sıradaki oturum için daha fazla ileti** olduktan sonra, iş akışı `isDone` değişkenini olarak ayarlar `true` .

Ardından, iptal etme döngüsüne **kadar, oturum kilitinde** bulunan eylemler için gerekli bilgileri sağlarsınız.

<a name="renew-session-while-messages-exist"></a>

### <a name="renew-session-lock-until-cancelled"></a>İptal edilene kadar oturum kilitlemeyi Yenile

Bu [ **until** döngüsü](../logic-apps/logic-apps-control-flow-loops.md#until-loop) , oturum kilidinin bu mantıksal uygulama tarafından tutulduğundan emin olmaya veya bu eylemleri çalıştırarak bir saat geçene kadar geçiş olmasına neden olur. Döngünün zaman sınırını değiştirmek için döngünün **zaman aşımı** özellik değerini düzenleyin.

* 25 saniye veya işlenen sıranın kilit zaman aşımı süresinden daha az bir süre gecikme süresi. En küçük kilit süresi 30 saniyedir, bu nedenle varsayılan değer yeterlidir. Ancak, uygun şekilde ayarlayarak döngünün kaç kez çalışacağını iyileştirebilirsiniz.

* `isDone`Değişkenin olarak ayarlanmış olup olmadığını denetleyin `true` .

  * , `isDone` Olarak ayarlanmamışsa `true` , iş akışı hala iletileri işliyor, bu nedenle iş akışı kuyruktaki oturumdaki kilidi yeniler ve döngü koşulunu yeniden denetler.

    Service Bus eylemde Service Bus kuyruğunuzun adını sağlamanız gerekir, [**bir kuyruktaki oturumdaki kilidi yenileyin**](#renew-lock-on-session).

  * `isDone`Olarak ayarlanırsa `true` , iş akışı kuyruktaki oturumdaki kilidi yenilemez ve döngüden çıkar.

  ![Loop-"iptal edilene kadar oturum kilitlemeyi Yenile"](./media/send-related-messages-sequential-convoy/renew-lock-until-session-cancelled.png)

<a name="renew-lock-on-session"></a>

#### <a name="renew-lock-on-the-session-in-a-queue"></a>Kuyruktaki oturum üzerindeki kilidi Yenile

Bu Service Bus eylem, iş akışı hala iletileri işlerken kuyruktaki oturumun kilidini yeniler.

* Service Bus eyleminde, **bir kuyruktaki oturum üzerindeki kilidi yenileyin**, Service Bus kuyruğunuzun adını sağlayın.

  ![Service Bus Action-"kuyruktaki oturum kilidini Yenile"](./media/send-related-messages-sequential-convoy/renew-lock-on-session-in-queue.png)

Sonra, Service Bus eylemi için gerekli bilgileri sağlayacak, **bir kuyruktaki oturumu kapatıp başarılı bir şekilde kapatacaksınız**.

<a name="close-session-succeed"></a>

### <a name="close-a-session-in-a-queue-and-succeed"></a>Kuyruktaki oturumu kapatma ve başarılı

Bu Service Bus eylem, iş akışı kuyruktaki tüm kullanılabilir iletileri işlemeyi tamamladıktan sonra kuyruktaki oturumu kapatır veya iş akışı ilk iletiyi terk ediyor.

* Service Bus eyleminde bir **kuyruktaki oturumu kapatıp başarılı bir**şekilde Service Bus kuyruğunuzun adını belirtin.

  ![Service Bus Action-"bir kuyruktaki oturumu kapatıp başarılı bırak"](./media/send-related-messages-sequential-convoy/close-session-in-queue-succeed.png)

Aşağıdaki bölümlerde `Catch` , iş akışınızda oluşan hataları ve özel durumları işleyen bölümündeki eylemler açıklanır.

<a name="close-session-fail"></a>

### <a name="close-a-session-in-a-queue-and-fail"></a>Kuyruktaki oturumu kapatma ve başarısız

Bu Service Bus eylem, her zaman kapsamdaki ilk eylem olarak çalışır `Catch` ve kuyruktaki oturumu kapatır.

* Service Bus eyleminde **bir kuyruktaki oturumu kapatıp başarısız yapın ve**Service Bus kuyruğunuzun adını sağlayın.

  ![Service Bus eylem-"bir kuyruktaki oturumu kapatıp başarısız bırak"](./media/send-related-messages-sequential-convoy/close-session-in-queue-fail.png)

Daha sonra, iş akışı, `Try` mantıksal uygulamanın oluşan hata veya hata hakkındaki bilgilere erişebilmeleri için kapsamdaki tüm eylemlerden girişler ve çıktılar içeren bir dizi oluşturur.

<a name="find-failure-message"></a>

### <a name="find-failure-msg-from-try-block"></a>' TRY ' bloğundan hata iletisi bul

Bu [ **filtre dizisi** eylemi](../logic-apps/logic-apps-perform-data-operations.md#filter-array-action) , `Try` [ `result()` işlevi](../logic-apps/workflow-definition-language-functions-reference.md#result)kullanılarak belirtilen ölçütlere göre kapsam içindeki tüm eylemlerden girişler ve çıktılar içeren bir dizi oluşturur. Bu durumda, bu eylem, `Failed` [ `equals()` işlevi](../logic-apps/workflow-definition-language-functions-reference.md#equals) ve [ `item()` işlevini](../logic-apps/workflow-definition-language-functions-reference.md#item)kullanarak durumu olan eylemlerden çıkışları döndürür.

![Diziyi filtrele eylemi-"TRY ' bloğundan hata bulma iletisi"](./media/send-related-messages-sequential-convoy/find-failure-message.png)

Bu eylemin JSON tanımı aşağıdadır:

```json
"Find_failure_msg_from_'Try'_block": {
   "inputs": {
      "from": "@Result('Try')",
      "where": "@equals(item()['status'], 'Failed')"
   },
   "runAfter": {
      "Close_the_session_in_the_queue_and_fail": [
         "Succeeded"
      ]
   },
   "type": "Query"
},
```

Sonra, iş akışı, eylemden döndürülen dizideki hata bilgilerini içeren JSON nesnesiyle bir dizi oluşturur `Find failure msg from 'Try' block` .

<a name="select-error-details"></a>

### <a name="select-error-details"></a>Hata ayrıntılarını seçin

Bu [ **seçim** eylemi](../logic-apps/logic-apps-perform-data-operations.md#select-action) , önceki eylemden çıktı olan GIRIŞ dizisine bağlı olarak JSON nesneleri içeren bir dizi oluşturur `Find failure msg from 'Try' block` . Özellikle, bu eylem dizideki her nesne için yalnızca belirtilen özelliklere sahip bir dizi döndürür. Bu durumda, dizi eylem adı ve hata sonucu özelliklerini içerir.

![Eylem seçin-"hata ayrıntılarını Seç"](./media/send-related-messages-sequential-convoy/select-error-details.png)

Bu eylemin JSON tanımı aşağıdadır:

```json
"Select_error_details": {
   "inputs": {
      "from": "@body('Find_failure_msg_from_''Try''_block')[0]['outputs']",
      "select": {
         "action": "@item()['name']",
         "errorResult": "@item()"
      }
   },
   "runAfter": {
      "Find_failure_msg_from_'Try'_block": [
         "Succeeded"
      ]
   },
   "type": "Select"
},
```

Daha sonra, iş akışı mantıksal uygulama çalıştırmasını durduruyor ve hata veya hata hakkında daha fazla bilgi ile birlikte çalıştırma durumunu döndürür.

<a name="terminate-logic-app"></a>

### <a name="terminate-logic-app-run"></a>Mantıksal uygulama çalıştırmasını Sonlandır

Bu [ **sonlandırma** eylemi](../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action) , mantıksal uygulama çalıştırmasını sonlandırır ve `Failed` mantıksal uygulamanın, oturum kimliği ve işlemin hata sonucuyla birlikte çalıştığı durum olarak döner `Select error details` .

![Mantıksal uygulama çalıştırmasını durdurmak için sonlandırma eylemi](./media/send-related-messages-sequential-convoy/terminate-logic-app-run.png)

Bu eylemin JSON tanımı aşağıdadır:

```json
"Terminate": {
   "description": "This Failure Termination only runs if the Close Session upon Failure action runs - otherwise the LA will be terminated as Success",
   "inputs": {
      "runError": {
         "code": "",
         "message": "There was an error processing messages for Session ID @{triggerBody()?['SessionId']}. The following error(s) occurred: @{body('Select_error_details')['errorResult']}"
         },
         "runStatus": "Failed"
      },
      "runAfter": {
         "Select_error_details": [
            "Succeeded"
         ]
      },
      "type": "Terminate"
   }
},
```

## <a name="save-and-run-logic-app"></a>Logic App 'i Kaydet ve Çalıştır

Şablonu tamamladıktan sonra, artık mantıksal uygulamanızı kaydedebilirsiniz. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

Mantıksal uygulamanızı test etmek için Service Bus kuyruğuna ileti gönderin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Service Bus bağlayıcısının Tetikleyicileri ve eylemleri](https://docs.microsoft.com/connectors/servicebus/) hakkında daha fazla bilgi edinin
