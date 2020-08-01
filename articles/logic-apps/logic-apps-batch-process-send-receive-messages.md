---
title: Grup olarak toplu işlem iletileri
description: Azure Logic Apps ' de toplu işlem kullanarak iş akışlarınız arasındaki gruplara ileti gönderin ve alın
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, jonfan, logicappspm
ms.topic: article
ms.date: 07/31/2020
ms.openlocfilehash: 0985afe3ddfd0d9de3c36ad6b030b6f259708c88
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87458354"
---
# <a name="send-receive-and-batch-process-messages-in-azure-logic-apps"></a>Azure Logic Apps iletileri gönderme, alma ve toplu işleme

İletileri belirli bir şekilde gruplar halinde göndermek ve işlemek için bir toplu işleme çözümü oluşturabilirsiniz. Bu çözüm, toplu iletileri serbest bırakarak ve işlemeden önce belirtilen ölçütleriniz karşılanana kadar bekler ve iletileri bir *toplu* olarak toplar. Toplu işleme, mantıksal uygulamanızın iletileri işleme sıklığını azaltabilir.

Bu makalede, aynı Azure aboneliğinde, Azure bölgesinde ve bu sırada iki mantıksal uygulama oluşturarak bir toplu işleme çözümü oluşturma gösterilmektedir:

1. Bu iletilerin serbest bırakılması ve işlenmesi için belirtilen ölçütleriniz karşılanana kadar iletileri kabul eden ve toplayan toplu iş [alıcısı "](#batch-receiver) mantıksal uygulaması. Toplu iş gönderisini oluştururken daha sonra toplu hedefi seçebilmeniz için bu toplu alıcıyı oluşturmanız gerekir.

1. İletileri daha önce oluşturulan toplu iş alıcısına gönderen bir veya daha fazla ["toplu gönderici"](#batch-sender) Logic Apps.

   Ayrıca, bir müşteri numarası gibi benzersiz bir anahtar belirtebilir, bu, hedef toplu işi *bölümler* veya bu anahtara göre mantıksal alt kümeler halinde ayırır. Bu şekilde, alıcı uygulaması aynı anahtara sahip tüm öğeleri toplayabilir ve bunları birlikte işleyebilir.

Batch alıcılarınız ve toplu iş göndericisinin aynı Azure aboneliğini *ve* Azure bölgesini paylaşması gerekir. Böyle bir işlem yoksa, Batch sender 'ı bir diğeri tarafından görülemeyen için oluşturduğunuzda Batch alıcısını seçemezsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure hesabı ve aboneliği Aboneliğiniz yoksa [ücretsiz bir Azure hesabı ile başlayabilirsiniz](https://azure.microsoft.com/free/). Ya da [bir Kullandıkça Öde aboneliğine kaydolun](https://azure.microsoft.com/pricing/purchase-options/).

* [Azure Logic Apps tarafından desteklenen herhangi bir e-posta sağlayıcısına](../connectors/apis-list.md) sahip e-posta hesabı

  > [!IMPORTANT]
  > Gmail bağlayıcısını kullanmak istiyorsanız, mantıksal uygulamalarda kısıtlama olmadan yalnızca G-Suite iş hesapları bu bağlayıcıyı kullanabilir. Gmail tüketicisi hesabınız varsa, bu bağlayıcıyı yalnızca belirli Google onaylı hizmetlerle kullanabilirsiniz veya [Gmail Bağlayıcınız ile kimlik doğrulaması için kullanmak üzere bir Google istemci uygulaması oluşturabilirsiniz](/connectors/gmail/#authentication-and-bring-your-own-application). Daha fazla bilgi için, bkz. [Azure Logic Apps Google bağlayıcıları Için veri güvenliği ve gizlilik ilkeleri](../connectors/connectors-google-data-security-privacy-policy.md).

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgi

* Azure portal yerine Visual Studio 'yu kullanmak için, [Logic Apps birlikte çalışmak üzere Visual Studio 'yu ayarladığınızdan](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)emin olun.

<a name="batch-receiver"></a>

## <a name="create-batch-receiver"></a>Toplu iş alıcısı oluştur

Bir toplu işe ileti gönderebilmeniz için önce bu toplu işin önce bu iletileri göndereceğiniz hedef olarak bulunması gerekir. İlk olarak, **Batch** tetikleyicisiyle başlayan "Batch alıcısı" mantıksal uygulamasını oluşturmanız gerekir. Bu şekilde, "Batch sender" mantıksal uygulamasını oluşturduğunuzda Batch alıcısı mantıksal uygulamasını seçebilirsiniz. Toplu iş alıcısı, bu iletilerin serbest bırakılması ve işlenmesi için belirtilen ölçütleriniz karşılanana kadar iletileri toplamaya devam eder. Batch alıcılarının toplu göndericiler hakkında herhangi bir şeyi bilmeleri gerekmiyorsa, Batch gönderenlerin iletileri gönderdikleri hedefi bilmeleri gerekir.

1. [Azure Portal](https://portal.azure.com) veya Visual Studio 'da, şu ada sahip bir mantıksal uygulama oluşturun:`BatchReceiver`

1. Mantıksal uygulama tasarımcısında, mantıksal uygulama iş akışınızı başlatan **Batch** tetikleyicisini ekleyin. Arama kutusuna girin `batch` ve şu tetikleyiciyi seçin: **Batch iletileri**

   !["Batch iletileri" tetikleyicisi Ekle](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

1. Batch alıcısı için aşağıdaki özellikleri ayarlayın:

   | Özellik | Açıklama |
   |----------|-------------|
   | **Toplu işlem modu** | - **Satır içi**: toplu tetikleyici içindeki yayın ölçütlerini tanımlamak için <br>- **Tümleştirme hesabı**: bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)aracılığıyla birden çok yayın ölçütü yapılandırması tanımlamak için. Tümleştirme hesabıyla, bu yapılandırmaların tümünü ayrı Logic Apps yerine tek bir yerde tutabilirsiniz. |
   | **Toplu iş adı** | Toplu iş için bu örnekteki "TestBatch" olan ve yalnızca **satır içi** Batch modu için geçerli olan ad |
   | **Yayın ölçütleri** | Yalnızca **satır içi** Batch modu için geçerlidir ve her toplu işi işlemeden önce yerine getirme ölçütlerini seçer: <p>- **İleti sayısı tabanlı**: Batch tarafından toplanan ileti sayısına göre toplu işi serbest bırakın. <br>- **Boyut tabanlı**: toplu işi, bu toplu işlem tarafından toplanan tüm iletiler için bayt cinsinden toplam boyuta göre serbest bırakın. <br>- **Zamanlama**: toplu işi, bir Aralık ve Sıklık belirten bir yinelenme zamanlaması temelinde serbest bırakın. Gelişmiş seçeneklerde Ayrıca bir saat dilimi seçebilir ve bir başlangıç tarihi ve saati sağlayabilirsiniz. <br>- **Tümünü Seç**: belirtilen tüm ölçütleri kullan. |
   | **İleti sayısı** | Toplu işte toplanacak ileti sayısı (örneğin, 10 mesaj). Bir toplu işlemin sınırı 8.000 iletilerdir. |
   | **Toplu iş boyutu** | Toplu işte toplanacak bayt cinsinden toplam boyut (örneğin, 10 MB). Bir toplu işlemin boyut sınırı 80 MB 'tır. |
   | **Zamanla** | Toplu yayınlar arasındaki Aralık ve sıklık (örneğin, 10 dakika). Minimum yinelenme 60 saniye veya 1 dakikadır. Kesirli dakikalar etkin bir şekilde 1 dakikaya yuvarlanır. Bir saat dilimi veya başlangıç tarihi ve saati belirtmek için **yeni parametre Ekle** listesini açın ve ilgili özellikleri seçin. |
   |||

   > [!NOTE]
   >
   > Tetikleyici hala toplu halde gönderilmemiş iletiler içeriyorsa yayın ölçütünü değiştirirseniz, tetikleyici gönderilmemiş iletileri işlemeye yönelik güncelleştirilmiş yayın ölçütünü kullanır.

   Bu örnek tüm kriterleri gösterir, ancak kendi testiniz için yalnızca bir ölçüt deneyin:

   ![Toplu tetikleyici ayrıntılarını sağlama](./media/logic-apps-batch-process-send-receive-messages/batch-receiver-criteria.png)

1. Şimdi her toplu işi işleyen bir veya daha fazla eylem ekleyin.

   Bu örnek için, Batch tetikleyicisi tetiklendiğinde e-posta gönderen bir eylem ekleyin. Tetikleyici çalışır ve toplu işte 10 mesaj, 10 MB veya 10 dakikalık bir süre geçtikten sonra bir e-posta gönderir.

   1. Batch tetikleyicisi altında **yeni adım**' ı seçin.

   1. Arama kutusuna `send email` filtreniz olarak yazın. E-posta sağlayıcınıza bağlı olarak bir e-posta Bağlayıcısı seçin.

      Örneğin, veya gibi bir iş veya okul hesabınız varsa @fabrikam.com @fabrikam.onmicrosoft.com **Outlook bağlayıcısını Microsoft 365** seçin. Veya gibi bir kişisel hesabınız varsa, @outlook.com @hotmail.com **Outlook.com** bağlayıcısını seçin. Bu örnek Outlook bağlayıcısını Microsoft 365 kullanır.

   1. Sağlayıcınız için "e-posta gönder" eylemini seçin, örneğin:

      ![E-posta sağlayıcınız için "e-posta gönder" eylemini seçin](./media/logic-apps-batch-process-send-receive-messages/batch-receiver-send-email-action.png)

1. İstenirse, e-posta hesabınızda oturum açın.

1. Eklediğiniz eylemin özelliklerini ayarlayın.

   * **Alıcı** kutusuna alıcının e-posta adresini girin. Test için kendi e-posta adresinizi kullanabilirsiniz.

   * **Konu** kutusunda, dinamik içerik listesi göründüğünde **bölüm adı** alanını seçin.

     ![Dinamik içerik listesinden "bölüm adı" öğesini seçin](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     Daha sonra, toplu iş göndericisinin içinde, hedef toplu işi ileti gönderebileceğiniz mantıksal alt kümelere ayıran benzersiz bir bölüm anahtarı belirtebilirsiniz. Her kümenin toplu gönderici mantıksal uygulaması tarafından oluşturulan benzersiz bir numarası vardır. Bu özellik, birden çok alt küme içeren tek bir toplu işlem kullanmanıza ve her bir alt kümeyi sağladığınız adla tanımlamanıza olanak sağlar.

     > [!IMPORTANT]
     > Bölüm 5.000 ileti veya 80 MB 'lik bir sınıra sahiptir. Her iki koşul da karşılanıyorsa, tanımlanan yayın koşulunuz karşılanmadığında bile toplu işi serbest bırakabilir Logic Apps.

   * **Gövde** kutusunda, dinamik içerik listesi göründüğünde **ileti kimliği** alanını seçin.

     Mantıksal uygulama Tasarımcısı, e-posta gönder eyleminin her bir döngüsü **için** otomatik olarak bir ekler. Bu eylem, bir toplu iş yerine önceki eylemden gelen çıktıyı bir koleksiyon olarak değerlendirir.

     !["Gövde" için "Ileti kimliği" ni seçin](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

1. Mantıksal uygulamanızı kaydedin. Artık bir toplu iş alıcısı oluşturdunuz.

    ![Mantıksal uygulamanızı kaydetme](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

   > [!IMPORTANT]
   > Visual Studio kullanıyorsanız, sonraki bölüme geçmeden önce [toplu iş alıcı mantıksal uygulamanızı Azure 'a *dağıttığınızdan* ](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure)emin olun. Aksi takdirde, Batch sender 'ı oluştururken Batch alıcısını seçemezsiniz.

<a name="batch-sender"></a>

## <a name="create-batch-sender"></a>Toplu gönderici oluştur

Şimdi Batch alıcı mantığı uygulamasına ileti gönderen bir veya daha fazla toplu iş gönderici mantığı uygulaması oluşturun. Her toplu iş göndericisine toplu iş alıcısı ve toplu iş adını, ileti içeriğini ve diğer ayarları belirtirsiniz. İsteğe bağlı olarak, bu anahtarla iletileri toplamak için toplu iş kümelerine bölmek üzere benzersiz bir bölüm anahtarı sağlayabilirsiniz.

* Batch yayınınızı oluştururken daha önce [oluşturduğunuz ve dağıttığınızdan](#batch-receiver) emin olun. bu nedenle, toplu yayınınızı oluşturduğunuzda mevcut toplu alıcıyı hedef toplu iş olarak seçebilirsiniz. Batch alıcılarının toplu göndericiler hakkında herhangi bir şeyi bilmeleri gerekmiyorsa, toplu göndericiler iletilerin nereden gönderileceğini bilmelidir.

* Batch alıcılarınızın ve toplu iş göndericisinin aynı Azure bölgesini *ve* Azure aboneliğini paylaştığından emin olun. Böyle bir işlem yoksa, Batch sender 'ı bir diğeri tarafından görülemeyen için oluşturduğunuzda Batch alıcısını seçemezsiniz.

1. Şu adla başka bir mantıksal uygulama oluşturun:`BatchSender`

   1. Arama kutusuna `recurrence` filtreniz olarak yazın. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **yinelenme**

      ![Yinelenme tetikleyicisini ekleme](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-sender.png)

   1. Her dakika gönderen mantıksal uygulamayı çalıştırmak için aralığı ve sıklığı ayarlayın.

      ![Yinelenme tetikleyicisi için sıklık ve aralığı ayarla](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-sender-details.png)

1. Toplu iş 'e ileti göndermek için yeni bir eylem ekleyin.

   1. **Yinelenme** tetikleyicisi altında **yeni adım**' ı seçin.

   1. Arama kutusuna `batch` filtreniz olarak girin ve şu eylemi seçin: **toplu iş tetikleyicisiyle Logic Apps Iş akışı seçin**

      !["Batch tetikleyicisiyle Logic Apps iş akışı Seç" i seçin](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

      Bir liste görünür ve yalnızca toplu iş tetikleyicisi olan ve aynı Azure bölgesinde *ve* Azure aboneliğinde toplu iş gönderici mantıksal uygulamanız olarak bulunan mantıksal uygulamaları gösterir.

   1. Logic Apps listesinden, daha önce oluşturduğunuz Batch alıcısı mantıksal uygulamasını seçin.

      ![Batch ahize mantıksal uygulamanızı seçin](./media/logic-apps-batch-process-send-receive-messages/batch-sender-select-batch-receiver.png)

      > [!IMPORTANT]
      > Visual Studio kullanıyorsanız ve seçilecek toplu iş alıcılarını görmüyorsanız, Batch alıcılarınızı Azure 'da daha önce oluşturup dağıttığınızı kontrol edin. Yapmadıysanız, [Batch ahize mantıksal uygulamanızı Azure 'a dağıtmayı](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure)öğrenin.

   1. Eylemler listesinden şu eylemi seçin: **Batch_messages-<- *mantıksal uygulama-adı* > **

      ![Şu eylemi seçin: "Batch_messages-<-mantıksal uygulama>"](./media/logic-apps-batch-process-send-receive-messages/batch-sender-select-batch.png)

1. Batch göndericisinin özelliklerini ayarlayın:

   | Özellik | Açıklama |
   |----------|-------------|
   | **Toplu iş adı** | Bu örnekteki alıcı mantığı uygulaması tarafından tanımlanan toplu işlem adı `TestBatch` <p>**Önemli**: Batch adı, çalışma zamanında onaylanır ve alıcı mantıksal uygulaması tarafından belirtilen adla eşleşmelidir. Toplu işlem adının değiştirilmesi toplu iş göndericisinin başarısız olmasına neden olur. |
   | **İleti Içeriği** | Göndermek istediğiniz iletinin içeriği |
   |||

   > [!NOTE]
   > **Tetikleyici adı** ve **iş akışı** özellik değerleri, seçtiğiniz mantıksal uygulamanızdan otomatik olarak doldurulur.

   Bu örnek için, Batch 'e göndereceğiniz ileti içeriğine geçerli tarih ve saati ekleyen bu ifadeyi ekleyin:

   1. **Ileti içeriği** kutusunun içine tıklayın.

   1. Dinamik içerik listesi göründüğünde **ifade**' ı seçin.

   1. İfadeyi girin `utcnow()` ve **Tamam**' ı seçin.

      !["Ileti Içeriği" içinde "Ifade" ı seçin, "UtcNow ()" yazın ve "Tamam" ı seçin.](./media/logic-apps-batch-process-send-receive-messages/batch-sender-details.png)

1. Artık toplu iş için bir bölüm ayarlayın. `BatchReceiver`Eylemde **yeni parametre Ekle** listesini açın ve şu özellikleri seçin:

   | Özellik | Açıklama |
   |----------|-------------|
   | **Bölüm adı** | Hedef Batch 'i mantıksal alt kümelere bölmek ve bu anahtara göre iletileri toplamak için kullanılacak isteğe bağlı benzersiz bölüm anahtarı |
   | **İleti kimliği** | Boş olduğunda oluşturulmuş bir genel benzersiz tanımlayıcı (GUID) olan isteğe bağlı bir ileti tanımlayıcısı |
   |||

   Bu örnekte, **bölüm adı** kutusuna bir ile beş arasında rastgele bir sayı üreten bir ifade ekleyin. **Ileti kimliği** kutusunu boş bırakın.

   1. Dinamik içerik listesinin görünmesi için **bölüm adı** kutusunun içine tıklayın.

   1. Dinamik içerik listesinde **ifade**' yi seçin.

   1. İfadeyi girip `rand(1,6)` **Tamam**' ı seçin.

      ![Hedef Batch için bir bölüm ayarlama](./media/logic-apps-batch-process-send-receive-messages/batch-sender-partition-advanced-options.png)

      Bu **S_SAYI_ÜRET** işlevi bir ile beş arasında bir sayı üretir. Bu nedenle bu toplu işi, bu ifadenin dinamik olarak ayarladığı beş sayılı bölüme böyor olursunuz.

1. Mantıksal uygulamanızı kaydedin. Gönderen mantıksal uygulamanız Şu örneğe benzer şekilde görünür:

   ![Gönderen mantıksal uygulamanızı kaydedin](./media/logic-apps-batch-process-send-receive-messages/batch-sender-finished.png)

## <a name="test-your-logic-apps"></a>Mantıksal uygulamalarınızı test etme

Toplu işlem çözümünüzü test etmek için, mantıksal uygulamalarınızı birkaç dakika boyunca çalışır durumda bırakın. Kısa süre içinde, hepsi aynı bölüm anahtarına sahip olan beş grup ile e-posta almaya başlamanız gerekir.

Batch gönderici mantıksal uygulamanız her dakikada çalışır, bir ile beş arasında rastgele bir sayı üretir ve bu üretilen numarayı iletilerin gönderildiği hedef toplu iş için bölüm anahtarı olarak kullanır. Toplu iş her seferinde aynı bölüm anahtarına sahip beş öğe olduğunda, Batch alıcısı mantıksal uygulamanız ateşlenir ve her ileti için posta gönderir.

> [!IMPORTANT]
> Testi tamamladığınızda, mantıksal uygulamayı devre dışı bırakarak `BatchSender` ileti göndermeyi durdurun ve gelen kutunuzu aşırı yüklemeyi önleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [EDI iletilerini toplayıp gönderme](../logic-apps/logic-apps-scenario-edi-send-batch-messages.md)
* [JSON kullanarak mantıksal uygulama tanımlarında derleme](../logic-apps/logic-apps-author-definitions.md)
* [Azure Logic Apps ve Işlevleri ile Visual Studio 'da sunucusuz bir uygulama oluşturun](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Mantıksal uygulamalar için özel durum işleme ve hata günlüğü](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
