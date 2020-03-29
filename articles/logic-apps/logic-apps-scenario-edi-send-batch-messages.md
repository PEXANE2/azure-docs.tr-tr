---
title: Grup olarak toplu işlem EDI iletileri
description: Azure Mantık Uygulamaları'nda toplu işleme yi kullanarak EDI iletilerini toplu olarak, grup veya koleksiyon olarak gönderme ve alma
services: logic-apps
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 08/19/2018
ms.openlocfilehash: 6fc0833f70e3e9cd98100f193b52e5a1bfa4d651
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75666678"
---
# <a name="exchange-edi-messages-as-batches-or-groups-between-trading-partners-in-azure-logic-apps"></a>Azure Logic Apps'taki işlem ortakları arasında EDI iletilerini toplu olarak veya grup olarak değiştirin

İş-iş (B2B) senaryolarında, iş ortakları genellikle gruplar halinde veya *toplu iş lerle*mesaj alışverişinde bulunun. Logic Apps ile toplu çözüm oluşturduğunuzda, ticaret ortaklarına ileti gönderebilir ve bu iletileri toplu olarak işleyebilirsiniz. Bu makalede, bir "toplu gönderen" mantık uygulaması ve bir "toplu alıcı" mantık uygulaması oluşturarak, örnek olarak X12 kullanarak EDI iletileri toplu işleyebilir nasıl gösterir. 

X12 iletilerini toplu olarak gruplandırmak, diğer iletileri toplu olarak gruplandırmak gibi çalışır; iletileri bir toplu iş haline toplayan bir toplu iş tetikleyicisi ve toplu iş partisine ileti gönderen bir toplu iş eylemi kullanırsınız. Ayrıca, X12 toplu iletileri ticaret ortağı veya diğer hedefe gitmeden önce bir X12 kodlama adımı içerir. Toplu iş tetikleyicisi ve eylemi hakkında daha fazla bilgi edinmek için [Toplu İşlem iletilerine](../logic-apps/logic-apps-batch-process-send-receive-messages.md)bakın.

Bu makalede, aynı Azure aboneliği, Azure bölgesi içinde iki mantık uygulaması oluşturarak ve bu özel siparişi izleyerek bir toplu iş çözümü oluşturursunuz:

* Belirtilen ölçütleriniz bu iletilerin serbest bırakılması ve işlenmesi için karşılanana kadar iletileri kabul eden ve toplu olarak toplayan bir ["toplu alıcı"](#receiver) mantık uygulaması. Bu senaryoda, toplu iş alıcısı da belirtilen X12 anlaşması veya ortak kimlikleri kullanarak toplu iş teki iletileri kodlar.

  Toplu iş göndereni oluştururken daha sonra toplu iş hedefini seçebilmeniz için önce toplu alıcıyı oluşturduğunuzdan emin olun.

* İletileri daha önce oluşturulan toplu alıcıya gönderen bir ["toplu gönderen"](#sender) mantık uygulaması. 

Toplu alıcınızın ve toplu iş göndereninizin aynı Azure aboneliğini *ve* Azure bölgesini paylaştığından emin olun. Bunlar değilse, toplu iş göndereni oluştururken toplu alıcıyı seçemezsiniz, çünkü birbirleri tarafından görülemezler.

## <a name="prerequisites"></a>Ön koşullar

Bu örneği izlemek için aşağıdaki öğelere ihtiyacınız vardır:

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabıyla başlayabilirsiniz.](https://azure.microsoft.com/free/) Veya, [Bir You-As-You-Go aboneliği için kaydolun.](https://azure.microsoft.com/pricing/purchase-options/)

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Azure aboneliğinizle ilişkili ve mantıksal uygulamalarınızla bağlantılı varolan bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)

* Entegrasyon hesabınızda en az iki varolan [ortak.](../logic-apps/logic-apps-enterprise-integration-partners.md) Her ortak, x12 (Standart Taşıyıcı Alfa Kodu) niteleyicisini ortağın özelliklerinde iş kimliği olarak kullanmalıdır.

* Entegrasyon hesabınızda varolan bir [X12 anlaşması](../logic-apps/logic-apps-enterprise-integration-x12.md)

* Azure portalı yerine Visual Studio'yu kullanmak [için Visual Studio'yu Logic Apps ile çalışmak üzere ayarladığınızdan](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)emin olun.

<a name="receiver"></a>

## <a name="create-x12-batch-receiver"></a>X12 toplu alıcı oluşturma

Toplu iş bir toplu iş için ileti gönderebilmek için önce bu toplu iş, bu iletileri gönderdiğiniz hedef olarak var olmalıdır. Yani ilk olarak, **Toplu iş** tetikleyicisi ile başlayan "toplu alıcı" mantık uygulamasını oluşturmanız gerekir. Bu şekilde, "toplu gönderen" mantık uygulamasını oluşturduğunuzda, toplu alıcı mantığı uygulamasını seçebilirsiniz. Toplu işlem alıcısı, bu iletileri serbest bırakmak ve işlemek için belirtilen ölçütleriniz karşılanana kadar ileti toplamaya devam eder. Toplu alıcıların toplu gönderenler hakkında hiçbir şey bilmesi gerekmezken, toplu gönderenler iletileri gönderdikleri hedefi bilmeleri gerekir. 

Bu toplu işlem alıcısı için toplu iş modunu, adı, sürüm ölçütlerini, X12 anlaşmasını ve diğer ayarları belirtirsiniz. 

1. Azure [portalında](https://portal.azure.com) veya Visual Studio'da bu ada sahip bir mantık uygulaması oluşturun: "BatchX12Messages"

2. [Mantık uygulamanızı entegrasyon hesabınıza bağla.](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account)

3. Logic Apps Designer'da, mantık uygulaması iş akışınızı başlatan **Toplu İşlem** tetikleyicisini ekleyin. Arama kutusuna filtreniz olarak "toplu iş" girin. Bu tetikleyiciyi seçin: **Toplu iletiler**

   ![Toplu İşlem tetikleyicisi ekle](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-receiver-trigger.png)

4. Toplu alıcı özelliklerini ayarlayın: 

   | Özellik | Değer | Notlar | 
   |----------|-------|-------|
   | **Toplu İşlem Modu** | Satır içi |  |  
   | **Toplu İş Adı** | TestBatch | Yalnızca **Satır İçi** toplu iş moduyla kullanılabilir | 
   | **Yayın Kriterleri** | İleti sayısı tabanlı, Zamanlama tabanlı | Yalnızca **Satır İçi** toplu iş moduyla kullanılabilir | 
   | **İleti Sayısı** | 10 | Yalnızca **İleti sayısına dayalı** sürüm ölçütleriyle kullanılabilir | 
   | **Interval** | 10 | Yalnızca **Zamanlama tabanlı** sürüm ölçütleriyle kullanılabilir | 
   | **Frequency** | dakika | Yalnızca **Zamanlama tabanlı** sürüm ölçütleriyle kullanılabilir | 
   ||| 

   ![Toplu tetikleyici ayrıntıları sağlama](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-release-criteria.png)

   > [!NOTE]
   > Bu örnek toplu iş için bir bölüm ayarlamaz, bu nedenle her toplu iş aynı bölüm anahtarını kullanır. Bölümler hakkında daha fazla bilgi edinmek için [Toplu işlem iletilerine](../logic-apps/logic-apps-batch-process-send-receive-messages.md#batch-sender)bakın.

5. Şimdi her toplu iş kodlayan bir eylem ekleyin: 

   1. Toplu iş tetikleyicisi altında **Yeni adımı**seçin.

   2. Arama kutusuna, filtreniz olarak "X12 toplu iş" girin ve bu eylemi (herhangi bir sürüm) seçin: **Toplu *<sürümünü*> - X12** 

      ![X12 Toplu İş Şifrele eylemini seçin](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-encode-action.png)

   3. Daha önce tümleştirme hesabınıza bağlanmadıysanız, bağlantıyı şimdi oluşturun. Bağlantınız için bir ad sağlayın, istediğiniz tümleştirme hesabını seçin ve sonra **Oluştur'u**seçin.

      ![Toplu kodlayıcı ve tümleştirme hesabı arasında bağlantı oluşturma](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encoder-connect-integration-account.png)

   4. Toplu iş kodlayıcı eyleminiz için bu özellikleri ayarlayın:

      | Özellik | Açıklama |
      |----------|-------------|
      | **X12 sözleşmesinin adı** | Listeyi açın ve varolan sözleşmenizi seçin. <p>Listeniz boşsa, mantık uygulamanızı istediğiniz anlaşmaya sahip [tümleştirme hesabına](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account) bağladığınızdan emin olun. | 
      | **Toplu İş Adı** | Bu kutunun içini tıklatın ve dinamik içerik listesi göründükten sonra **Toplu İş Adı** belirteci'ni seçin. | 
      | **Partitionname** | Bu kutunun içini tıklatın ve dinamik içerik listesi göründükten sonra **Bölüm Adı** belirteci'ni seçin. | 
      | **Öğeler** | Öğe ayrıntıları kutusunu kapatın ve sonra bu kutunun içini tıklatın. Dinamik içerik listesi göründükten **sonra, Toplu Öğeler** belirteci'ni seçin. | 
      ||| 

      ![Toplu Kodlama eylem ayrıntıları](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encode-action-details.png)

      **Öğeler** kutusu için:

      ![Toplu Kodlama eylem öğeleri](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encode-action-items.png)

6. Mantıksal uygulamanızı kaydedin. 

7. Visual Studio kullanıyorsanız, [toplu alıcı mantığı uygulamanızı Azure'a dağıttığınızdan](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure)emin olun. Aksi takdirde, toplu göndereni oluştururken toplu alıcıyı seçemezsiniz.

### <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

Toplu alıcınızın beklendiği gibi çalıştığından emin olmak için, sınama amacıyla bir HTTP eylemi ekleyebilir ve [İstek Kutusu hizmetine](https://requestbin.com/)toplu ileti gönderebilirsiniz. 

1. X12 kodlama eylemi altında **Yeni adımı**seçin. 

2. Arama kutusuna filtreniz olarak "http" girin. Bu eylemi seçin: **HTTP - HTTP**
    
   ![HTTP eylemini seçin](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-add-http-action.png)

3. HTTP eyleminin özelliklerini ayarlayın:

   | Özellik | Açıklama | 
   |----------|-------------|
   | **Yöntem** | Bu listeden **POST'u**seçin. | 
   | **Urı** | İstek kutunuz için bir URI oluşturun ve sonra bu kutuya URI'yi girin. | 
   | **Gövde** | Bu kutunun içini tıklatın ve dinamik içerik listesi açıldıktan sonra, bölümünde görünen **Gövde** belirteci, **anlaşma adına göre Toplu kodlama**. <p>**Gövde** belirteci görmüyorsanız, **anlaşma adına göre Toplu kod kodunun**yanında, daha fazla **sını gör'üni**seçin. | 
   ||| 

   ![HTTP eylem ayrıntılarını sağlayın](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-add-http-action-details.png)

4. Mantıksal uygulamanızı kaydedin. 

   Toplu alıcı mantık uygulamanız şu örneğe benzer: 

   ![Toplu alıcı mantık uygulamanızı kaydedin](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-finished.png)

<a name="sender"></a>

## <a name="create-x12-batch-sender"></a>X12 toplu gönderen oluşturma

Şimdi toplu alıcı mantık uygulamasına mesaj gönderen bir veya daha fazla mantık uygulaması oluşturun. Her toplu işlem gönderende, toplu alıcı mantığı uygulamasını ve toplu iş adını, ileti içeriğini ve diğer ayarları belirtirsiniz. İsteğe bağlı olarak, bu anahtarla ileti leri toplamak için toplu işlemi alt kümelere bölmek için benzersiz bir bölüm anahtarı sağlayabilirsiniz. 

* Toplu işlem alıcınızı zaten [oluşturduğunuzdan](#receiver) emin olun, böylece toplu göndereninizi oluşturduğunuzda, varolan toplu alıcıyı hedef toplu iş olarak seçebilirsiniz. Toplu alıcıların toplu gönderenler hakkında hiçbir şey bilmesi gerekmezken, toplu gönderenler iletilerin nereye gönderileceklerini bilmelidir. 

* Toplu alıcınızın ve toplu iş göndereninizin aynı Azure bölgesini *ve* Azure aboneliğini paylaştığından emin olun. Bunlar değilse, toplu iş göndereni oluştururken toplu alıcıyı seçemezsiniz, çünkü birbirleri tarafından görülemezler.

1. Bu adla başka bir mantık uygulaması oluşturun: "SendX12MessagesToBatch" 

2. Arama kutusuna filtreniz olarak "ne zaman bir http isteği" girin. Bu tetikleyiciyi seçin: **Bir HTTP isteği aldığında** 
   
   ![İstek tetikleyicisini ekleme](./media/logic-apps-scenario-EDI-send-batch-messages/add-request-trigger-sender.png)

3. Toplu iş partisine ileti göndermek için bir eylem ekleyin.

   1. HTTP istek eylemi altında **Yeni adımı**seçin.

   2. Arama kutusuna filtreniz olarak "toplu iş" girin. 
   **Eylemler** listesini seçin ve ardından şu eylemi seçin: **Toplu iş tetikleyicisi olan bir Mantık Uygulamaları iş akışı seçin - Toplu iş yerine ileti gönderme**

      !["Toplu tetikleyiciyle Bir Mantık Uygulamaları iş akışı seçin" seçeneğini belirleyin](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-select-batch-trigger.png)

   3. Şimdi daha önce oluşturduğunuz "BatchX12Messages" mantık uygulamanızı seçin.

      !["Toplu alıcı" mantık uygulamasını seçin](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-select-batch-receiver.png)

   4. Bu eylemi seçin: **Batch_messages - <toplu *alıcınızı* > **

      !["Batch_messages" eylemini seçin](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-select-batch-messages-action.png)

4. Toplu iş gönderenin özelliklerini ayarlayın.

   | Özellik | Açıklama | 
   |----------|-------------| 
   | **Toplu İş Adı** | Bu örnekte "TestBatch" olan alıcı mantığı uygulaması tarafından tanımlanan toplu iş adı <p>**Önemli**: Toplu iş adı çalışma zamanında doğrulanır ve alıcı mantığı uygulaması tarafından belirtilen adla eşleşmelidir. Toplu iş adını değiştirmek, toplu gönderenin başarısız olması nedeniyle. | 
   | **İleti İçeriği** | Bu örnekte **Gövde** belirteci olan göndermek istediğiniz iletinin içeriği | 
   ||| 
   
   ![Toplu iş özelliklerini ayarlama](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-set-batch-properties.png)

5. Mantıksal uygulamanızı kaydedin. 

   Toplu gönderen mantık uygulamanız şu örneğe benzer:

   ![Toplu gönderen mantık uygulamanızı kaydedin](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-finished.png)

## <a name="test-your-logic-apps"></a>Mantık uygulamalarınızı test edin

Toplu işleme çözümünüzü test etmek için [Postacı'dan](https://www.getpostman.com/postman) veya benzer bir araçtan toplu gönderen mantık uygulamanıza X12 iletileri gönderin. Yakında, istek kutunuzda x12 iletileri almaya başlarsınız, her 10 dakikada bir veya aynı bölüm tuşu yla 10'lu gruplar halinde.

## <a name="next-steps"></a>Sonraki adımlar

* [İletileri toplu olarak işleme](../logic-apps/logic-apps-batch-process-send-receive-messages.md) 
