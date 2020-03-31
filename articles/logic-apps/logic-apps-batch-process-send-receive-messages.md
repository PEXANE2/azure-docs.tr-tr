---
title: Toplu işlem iletileri grup olarak
description: Azure Logic Apps'ta toplu işleme yi kullanarak iş akışlarınız arasındaki gruplar halinde ileti gönderme ve alma
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, jonfan, logicappspm
ms.topic: article
ms.date: 01/16/2019
ms.openlocfilehash: e48d2bb2ffce0dd4f9293417534165165d426784
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75666763"
---
# <a name="send-receive-and-batch-process-messages-in-azure-logic-apps"></a>Azure Logic Apps'ta işleme iletileri gönderme, alma ve toplu işlem

İletileri grup olarak belirli bir şekilde birlikte göndermek ve işlemek için, toplu iletileri serbest bırakmak ve işlemek için belirtilen ölçütleriniz karşılanana kadar iletileri toplu iş olarak toplayan bir *toplu* iş çözümü oluşturabilirsiniz. Toplu işlem, mantık uygulamanızın iletileri işleme süresini azaltabilir. Bu makalede, aynı Azure aboneliği, Azure bölgesi içinde iki mantık uygulaması oluşturarak ve bu özel siparişi izleyerek toplu çözüm oluşturmanın nasıl yapılacağını gösterilmektedir: 

* Belirtilen ölçütleriniz bu iletilerin serbest bırakılması ve işlenmesi için karşılanana kadar iletileri kabul eden ve toplu olarak toplayan ["toplu alıcı"](#batch-receiver) mantık uygulaması.

  Toplu iş göndereni oluştururken daha sonra toplu iş hedefini seçebilmeniz için önce toplu alıcıyı oluşturduğunuzdan emin olun.

* İletileri daha önce oluşturulan toplu alıcıya gönderen bir veya daha fazla ["toplu gönderen"](#batch-sender) mantık uygulaması. 

   Ayrıca, hedef toplu işlemi bu anahtara göre mantıksal alt kümelere *bölümlere* ayıran veya bölen müşteri numarası gibi benzersiz bir anahtar da belirtebilirsiniz. Bu şekilde, alıcı uygulaması tüm öğeleri aynı anahtarla toplayabilir ve birlikte işleyebilir.

Toplu alıcınızın ve toplu iş göndereninizin aynı Azure aboneliğini *ve* Azure bölgesini paylaştığından emin olun. Bunlar değilse, toplu iş göndereni oluştururken toplu alıcıyı seçemezsiniz, çünkü birbirleri tarafından görülemezler.

## <a name="prerequisites"></a>Ön koşullar

Bu örneği izlemek için aşağıdaki öğelere ihtiyacınız vardır:

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabıyla başlayabilirsiniz.](https://azure.microsoft.com/free/) Veya, [Bir You-As-You-Go aboneliği için kaydolun.](https://azure.microsoft.com/pricing/purchase-options/)

* Azure Logic Apps tarafından desteklenen tüm [e-posta sağlayıcılarının](../connectors/apis-list.md) yer verdiği bir e-posta hesabı

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler 

* Azure portalı yerine Visual Studio'yu kullanmak [için Visual Studio'yu Logic Apps ile çalışmak üzere ayarladığınızdan](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)emin olun.

<a name="batch-receiver"></a>

## <a name="create-batch-receiver"></a>Toplu alıcı oluşturma

Toplu iş bir toplu iş için ileti gönderebilmek için önce bu toplu iş, bu iletileri gönderdiğiniz hedef olarak var olmalıdır. Yani ilk olarak, **Toplu iş** tetikleyicisi ile başlayan "toplu alıcı" mantık uygulamasını oluşturmanız gerekir. Bu şekilde, "toplu gönderen" mantık uygulamasını oluşturduğunuzda, toplu alıcı mantığı uygulamasını seçebilirsiniz. Toplu işlem alıcısı, bu iletileri serbest bırakmak ve işlemek için belirtilen ölçütleriniz karşılanana kadar ileti toplamaya devam eder. Toplu alıcıların toplu gönderenler hakkında hiçbir şey bilmesi gerekmezken, toplu gönderenler iletileri gönderdikleri hedefi bilmeleri gerekir. 

1. Azure [portalında](https://portal.azure.com) veya Visual Studio'da bu ada sahip bir mantık uygulaması oluşturun: "BatchReceiver" 

2. Logic Apps Designer'da, mantık uygulaması iş akışınızı başlatan **Toplu İşlem** tetikleyicisini ekleyin. Arama kutusuna filtreniz olarak "toplu iş" girin. Bu tetikleyiciyi seçin: **Toplu iletiler**

   !["Toplu iletiler" tetikleyicisi ekleme](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. Toplu iş alıcısı için bu özellikleri ayarlayın: 

   | Özellik | Açıklama | 
   |----------|-------------|
   | **Toplu İşlem Modu** | - **Satır Içi**: Toplu iş tetikleyicisi içindeki sürüm ölçütlerini tanımlamak için <br>- **Tümleştirme Hesabı**: Bir [entegrasyon hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)üzerinden birden çok sürüm ölçüt yapılandırması tanımlamak için. Bir tümleştirme hesabıyla, bu yapılandırmaları ayrı mantık uygulamaları yerine tek bir yerde koruyabilirsiniz. | 
   | **Toplu İş Adı** | Bu örnekte "TestBatch" olan ve yalnızca **Satır İçi** toplu iş modu için geçerli olan toplu iş ınızın adı |  
   | **Yayın Kriterleri** | Yalnızca Satır **İçi** toplu iş moduna uygulanır ve her toplu iş işlemeden önce karşılaması gereken ölçütleri seçer: <p>- **İleti sayısı tabanlı**: Toplu iş, toplu iş tarafından toplanan ileti sayısına göre toplu iş bırakın. <br>- **Boyut tabanlı**: Toplu işlemi, o toplu iş tarafından toplanan tüm iletiler için baytların toplam boyutuna göre serbest bırakın. <br>- **Zamanlama**: Bir aralık ve sıklık belirten bir yineleme çizelgesine göre toplu işlemi serbest bırakın. Gelişmiş seçeneklerde, bir saat dilimi seçebilir ve bir başlangıç tarihi ve saati sağlayabilirsiniz. <br>- **Tümünü Seçin**: Belirtilen tüm ölçütleri kullanın. | 
   | **İleti Sayısı** | Toplu iş te toplanacak ileti sayısı, örneğin, 10 ileti. Bir partinin limiti 8.000 mesajdır. | 
   | **Toplu İşlem Boyutu** | Toplu iş başına toplanacak baytların toplam boyutu, örneğin, 10 MB. Bir toplu iş boyutu sınırı 80 MB'dır. | 
   | **Zamanlama** | Toplu iş sürümleri arasındaki aralık ve sıklık, örneğin, 10 dakika. Minimum yineleme 60 saniye veya 1 dakikadır. Kesirli dakikalar etkili bir şekilde 1 dakikaya kadar yuvarlanır. Saat dilimini veya başlangıç tarihini ve saatini belirtmek için **gelişmiş seçenekleri göster'i**seçin. | 
   ||| 

   > [!NOTE]
   > 
   > Tetikleyici hala toplu halde yken sürüm ölçütlerini değiştirirseniz, tetikleyici gönderilmemiş iletileri işlemek için güncelleştirilmiş sürüm ölçütlerini kullanır. 

   Bu örnek tüm ölçütleri gösterir, ancak kendi testiçin yalnızca bir ölçüt deneyin:

   ![Toplu başlatma ayrıntılarını sağlama](./media/logic-apps-batch-process-send-receive-messages/batch-receiver-criteria.png)

4. Şimdi, her toplu işlemi işleyen bir veya daha fazla eylem ekleyin. 

   Bu örnekte, toplu iş tetiklediğinde e-posta gönderen bir eylem ekleyin. 
   Tetikleyici, toplu iş 10 iletiye sahipolduğunda, 10 MB'a ulaştığında veya 10 dakika geçtikten sonra bir e-posta gönderir ve çalışır.

   1. Toplu iş tetikleyicisi altında **Yeni adımı**seçin.

   2. Arama kutusuna filtreniz olarak "e-posta gönder" yazın.
   E-posta sağlayıcınıza bağlı olarak bir e-posta bağlayıcısı seçin.

      Örneğin, kişisel bir hesabınız varsa @outlook.com veya @hotmail.comOutlook.com bağlayıcısını seçin. 
      Gmail hesabınız varsa Gmail bağlayıcısını seçin. 
      Bu örnek, Office 365 Outlook kullanır. 

   3. Bu eylemi seçin: **E-posta gönderin - *<e-posta sağlayıcısı* > **

      Örnek:

      ![E-posta sağlayıcınız için "E-posta gönder" eylemini seçin](./media/logic-apps-batch-process-send-receive-messages/batch-receiver-send-email-action.png)

5. İstenirse, e-posta hesabınızda oturum açın. 

6. Eklediğiniz eylem için özellikleri ayarlayın.

   * **Alıcı** kutusuna alıcının e-posta adresini girin. 
   Test için kendi e-posta adresinizi kullanabilirsiniz.

   * **Konu** kutusunda, dinamik içerik listesi **göründüğünde, Bölüm Adı** alanını seçin.

     ![Dinamik içerik listesinden "Bölüm Adı"nı seçin](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     Daha sonra, toplu iş gönderende, hedef toplu işlemi ileti gönderebileceğiniz mantıksal alt kümelere bölen benzersiz bir bölüm anahtarı belirtebilirsiniz. 
     Her küme, toplu gönderen mantık uygulaması tarafından oluşturulan benzersiz bir numaraya sahiptir. 
     Bu özellik, birden çok alt kümeye sahip tek bir toplu iş kullanmanızı ve her alt kümeyi sağladığınız adla tanımlamanıza olanak tanır.

     > [!IMPORTANT]
     > Bir bölümün 5.000 ileti veya 80 MB sınırı vardır. Her iki koşul da karşılanırsa, Tanımlanan sürüm durumunuz karşılanmasa bile Logic Apps toplu işlemi yayımlayabilir.

   * **Gövde** kutusunda, dinamik içerik listesi göründüğünde İleti **Kimliği** alanını seçin. 

     Logic Apps Tasarımcısı, e-posta gönderme eyleminin etrafına otomatik olarak bir "Her biri için" döngüsü ekler, çünkü bu eylem önceki eylemden çıktıyı toplu iş yerine bir koleksiyon olarak ele alır. 

     !["Gövde" için "İleti Kimliği"ni seçin](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

7.  Mantıksal uygulamanızı kaydedin. Şimdi bir toplu iş alıcısı oluşturdunuz.

    ![Mantıksal uygulamanızı kaydetme](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

8. Visual Studio kullanıyorsanız, [toplu alıcı mantığı uygulamanızı Azure'a dağıttığınızdan](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure)emin olun. Aksi takdirde, toplu göndereni oluştururken toplu alıcıyı seçemezsiniz.

<a name="batch-sender"></a>

## <a name="create-batch-sender"></a>Toplu gönderen oluşturma

Şimdi toplu alıcı mantık uygulamasına mesaj gönderen bir veya daha fazla toplu gönderen mantık uygulaması oluşturun. Her toplu işlem gönderende, toplu iş alıcısı ve toplu iş adı, ileti içeriği ve diğer ayarları belirtirsiniz. İsteğe bağlı olarak, toplu işlemi bu anahtarla ileti toplamak için mantıksal alt kümelere bölmek için benzersiz bir bölüm anahtarı sağlayabilirsiniz. 

* Toplu işlem alıcınızı zaten [oluşturduğunuzdan](#batch-receiver) emin olun, böylece toplu göndereninizi oluşturduğunuzda, varolan toplu alıcıyı hedef toplu iş olarak seçebilirsiniz. Toplu alıcıların toplu gönderenler hakkında hiçbir şey bilmesi gerekmezken, toplu gönderenler iletilerin nereye gönderileceklerini bilmelidir. 

* Toplu alıcınızın ve toplu iş göndereninizin aynı Azure bölgesini *ve* Azure aboneliğini paylaştığından emin olun. Bunlar değilse, toplu iş göndereni oluştururken toplu alıcıyı seçemezsiniz, çünkü birbirleri tarafından görülemezler.

1. Bu adla başka bir mantık uygulaması oluşturun: "BatchSender"

   1. Arama kutusuna filtreniz olarak "yineleme" girin. 
   Bu tetikleyiciyi seçin: **Yineleme - Zamanlama**

      !["Yineleme - Zamanlama" tetikleyicisini ekleyin](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-sender.png)

   2. Gönderen mantık uygulamasını her dakika çalıştırmak için sıklığı ve aralığı ayarlayın.

      ![Nüks tetikleyicisi için frekans ve aralık ayarlama](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-sender-details.png)

2. Toplu iş partisine ileti göndermek için yeni bir eylem ekleyin.

   1. Yineleme tetikleyicisi altında **Yeni adımı**seçin.

   2. Arama kutusuna filtreniz olarak "toplu iş" girin. 
   **Eylemler** listesini seçin ve ardından şu eylemi seçin: **Toplu iş tetikleyicisi olan bir Mantık Uygulamaları iş akışı seçin - Toplu iş yerine ileti gönderme**

      !["Toplu tetikleyiciyle Bir Mantık Uygulamaları iş akışı seçin" seçeneğini belirleyin](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   3. Daha önce oluşturduğunuz toplu alıcı mantık uygulamanızı seçin.

      !["Toplu alıcı" mantık uygulamasını seçin](./media/logic-apps-batch-process-send-receive-messages/batch-sender-select-batch-receiver.png)

      > [!NOTE]
      > Liste ayrıca toplu iş tetikleyicileri olan diğer mantık uygulamalarını da gösterir. 
      > 
      > Visual Studio kullanıyorsanız ve seçilecek toplu iş alıcısı görmüyorsanız, toplu işlem alıcınızı Azure'a dağıtıp dağıtmadığınızı kontrol edin. Yapmadıysanız, [toplu alıcı mantık uygulamanızı Azure'a nasıl dağıtabileceğinizi](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure)öğrenin. 

   4. Bu eylemi seçin: **Batch_messages - <toplu *alıcınızı* > **

      ![Bu eylemi seçin: "Batch_messages - <-mantık-uygulama>"](./media/logic-apps-batch-process-send-receive-messages/batch-sender-select-batch.png)

3. Toplu iş gönderenin özelliklerini ayarlayın:

   | Özellik | Açıklama | 
   |----------|-------------| 
   | **Toplu İş Adı** | Bu örnekte "TestBatch" olan alıcı mantığı uygulaması tarafından tanımlanan toplu iş adı <p>**Önemli**: Toplu iş adı çalışma zamanında doğrulanır ve alıcı mantığı uygulaması tarafından belirtilen adla eşleşmelidir. Toplu iş adını değiştirmek, toplu gönderenin başarısız olması nedeniyle. | 
   | **İleti İçeriği** | Göndermek istediğiniz iletinin içeriği | 
   ||| 

   Bu örnek için, toplu iş partisine gönderdiğiniz ileti içeriğine geçerli tarih ve saati ekleyen bu ifadeyi ekleyin:

   1. **İleti İçeriği** kutusunun içini tıklatın. 

   2. Dinamik içerik listesi göründüğünde, **İfade'yi**seçin. 

   3. İfadeyi `utcnow()`girin ve sonra **Tamam'ı**seçin. 

      !["İleti İçeriği"nde "İfade"yi seçin, "utcnow()" girin ve "Tamam"ı seçin.](./media/logic-apps-batch-process-send-receive-messages/batch-sender-details.png)

4. Şimdi toplu iş için bir bölüm ayarlayın. "BatchReceiver" eyleminde **gelişmiş seçenekleri göster'i** seçin ve şu özellikleri ayarlayın:

   | Özellik | Açıklama | 
   |----------|-------------| 
   | **Bölüm Adı** | Hedef toplu işlemi mantıksal alt kümelere bölmek ve bu anahtara göre ileti toplamak için kullanılacak isteğe bağlı benzersiz bölüm anahtarı | 
   | **İleti Kimliği** | Boşken oluşturulan genel olarak benzersiz bir tanımlayıcı (GUID) olan isteğe bağlı ileti tanımlayıcısı | 
   ||| 

   Bu örnekte, **Bölüm Adı** kutusunda, bir ile beş arasında rasgele bir sayı oluşturan bir ifade ekleyin. İleti **Kimliği** kutusunu boş bırakın.
   
   1. Dinamik içerik listesinin görünmesi için **Bölüm Adı** kutusunun içini tıklatın. 

   2. Dinamik içerik listesinde **İfade**’yi seçin.
   
   3. İfadeyi `rand(1,6)`girin ve sonra **Tamam'ı**seçin.

      ![Hedef toplu iş için bir bölüm ayarlama](./media/logic-apps-batch-process-send-receive-messages/batch-sender-partition-advanced-options.png)

      Bu **rand** işlevi bir ile beş arasında bir sayı oluşturur. 
      Yani bu toplu işlemi, bu ifadenin dinamik olarak ayarladığı beş numaralı bölüme bölüyorsunuz.

5. Mantıksal uygulamanızı kaydedin. Gönderen mantık uygulamanız artık bu örneğe benzer:

   ![Gönderen mantık uygulamanızı kaydedin](./media/logic-apps-batch-process-send-receive-messages/batch-sender-finished.png)

## <a name="test-your-logic-apps"></a>Mantık uygulamalarınızı test edin

Toplu iş çözümünüzü test etmek için mantık uygulamalarınızı birkaç dakika bekletin. Yakında, beş kişilik gruplar halinde e-postalar almaya başlarsınız, hepsi de aynı bölüm tuşuyla.

Toplu gönderen mantık uygulamanız her dakika çalışır, bir ile beş arasında rastgele bir sayı oluşturur ve bu oluşturulan sayıyı iletilerin gönderildiği hedef toplu iş için bölüm anahtarı olarak kullanır. Toplu iş her zaman aynı bölüm tuşuna sahip beş öğe vardır, toplu alıcı mantık uygulaması yangınları ve her ileti için posta gönderir.

> [!IMPORTANT]
> Test etmeyi bitirdiğinizde, ileti göndermeyi durdurmak ve gelen kutunuzu aşırı yüklemekten kaçınmak için BatchSender mantık uygulamasını devre dışı bıraktığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

* [EDI iletilerini toplayıp gönderme](../logic-apps/logic-apps-scenario-edi-send-batch-messages.md)
* [JSON kullanarak mantık uygulama tanımları üzerine inşa](../logic-apps/logic-apps-author-definitions.md)
* [Azure Logic Uygulamaları ve Işlevleri ile Visual Studio'da sunucusuz bir uygulama oluşturun](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Mantık uygulamaları için özel durum işleme ve hata günlüğü](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
