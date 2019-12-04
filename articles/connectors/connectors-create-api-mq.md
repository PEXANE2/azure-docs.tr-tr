---
title: IBM MQ sunucusuna bağlan
description: Azure veya şirket içi IBM MQ Server ve Azure Logic Apps ileti gönderin ve alın
services: logic-apps
ms.suite: integration
author: valrobb
ms.author: valthom
ms.reviewer: chrishou, logicappspm
ms.topic: article
ms.date: 06/19/2019
tags: connectors
ms.openlocfilehash: ef9e91b526055ece58ce283572deb98cff951653
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789580"
---
# <a name="connect-to-an-ibm-mq-server-from-azure-logic-apps"></a>Azure Logic Apps bir IBM MQ sunucusuna bağlanma

IBM MQ Bağlayıcısı, şirket içinde veya Azure 'da bir IBM MQ sunucusunda depolanan iletileri gönderir ve alır. Bu bağlayıcı bir TCP/IP ağı üzerinde uzak IBM MQ sunucusuyla iletişim kuran bir Microsoft MQ istemcisi içerir. Bu makale, MQ bağlayıcısını kullanmak için bir başlangıç kılavuzu sağlar. Bir kuyruktaki tek bir iletiye göz atarak başlayabilir ve ardından diğer eylemleri deneyebilirsiniz.

IBM MQ Bağlayıcısı bu eylemleri içerir, ancak hiçbir tetikleyici sağlamaz:

- IBM MQ sunucusundan iletiyi silmeden tek bir iletiye gözatın
- IBM MQ sunucusundan iletileri silmeden bir toplu işe gözatamıyorum
- Tek bir ileti alın ve IBM MQ sunucusundan iletiyi silin
- Bir toplu ileti alın ve IBM MQ sunucusundan iletileri silin
- IBM MQ sunucusuna tek bir ileti gönderin

## <a name="prerequisites"></a>Önkoşullar

* Şirket içi MQ sunucusu kullanıyorsanız, Şirket [içi veri ağ geçidini](../logic-apps/logic-apps-gateway-install.md) ağınız içindeki bir sunucuya yükleyebilirsiniz. Şirket içi veri ağ geçidinin yüklü olduğu sunucuda, MQ bağlayıcısının çalışması için .NET Framework 4,6 yüklü olmalıdır. Ayrıca, şirket içi veri ağ geçidi için Azure 'da bir kaynak oluşturmanız gerekir. Daha fazla bilgi için bkz. [Data Gateway bağlantısını ayarlama](../logic-apps/logic-apps-gateway-connection.md).

  Ancak, MQ sunucunuz Azure 'da herkese açık veya kullanılabilir ise, veri ağ geçidini kullanmanız gerekmez.

* Resmi olarak desteklenen IBM WebSphere MQ sürümleri:

  * MQ 7,5
  * MQ 8,0
  * MQ 9,0

* MQ eylemini eklemek istediğiniz mantıksal uygulama. Bu mantıksal uygulama, şirket içi veri ağ geçidi bağlantınızla aynı konumu kullanmalıdır ve iş akışınızı Başlatan bir tetikleyicisine sahip olmalıdır. 

  MQ bağlayıcısının hiçbir tetikleyicisi yoktur, bu nedenle önce mantıksal uygulamanıza bir tetikleyici eklemeniz gerekir. Örneğin, yinelenme tetikleyicisini kullanabilirsiniz. Logic Apps 'e yeni başladıysanız, [ilk mantıksal uygulamanızı oluşturmak için bu hızlı](../logic-apps/quickstart-create-first-logic-app-workflow.md)başlangıcı deneyin. 

## <a name="browse-a-single-message"></a>Tek bir iletiye gözatın

1. Mantıksal uygulamanızda, tetikleyici veya başka bir eylem altında **yeni adım**' ı seçin. 

1. Arama kutusuna "MQ" yazın ve şu eylemi seçin: **Iletiye gözatamazsınız**

   ![İletiye gözatamıyorum](media/connectors-create-api-mq/Browse_message.png)

1. Mevcut bir MQ bağlantınız yoksa bağlantıyı oluşturun:  

   1. Eylemde Şirket **içi veri ağ geçidi üzerinden Bağlan**' ı seçin.
   
   1. MQ sunucunuzun özelliklerini girin.  

      **Sunucu**için MQ sunucu adını gırebılır veya IP adresini, ardından iki nokta üst üste ve bağlantı noktası numarası ile girebilirsiniz.
    
   1. Daha önce yapılandırılmış ağ geçidi bağlantılarını gösteren **ağ geçidi** listesini açın. Ağ geçidinizi seçin.
    
   1. İşiniz bittiğinde **Oluştur**’u seçin. 
   
      Bağlantınız Şu örneğe benzer şekilde görünür:

      ![Bağlantı özellikleri](media/connectors-create-api-mq/Connection_Properties.png)

1. Eylemin özelliklerini ayarlayın:

   * **Kuyruk**: bağlantıdan farklı bir sıra belirtin.

   * **MessageID**, **bağıntıkimliği**, **GroupID**ve diğer özellikler: farklı MQ Ileti özelliklerine göre bir iletiye gözatamazsınız

   * **Includeınfo**: çıkışa ek ileti bilgilerini eklemek Için **true değerini** belirtin. Ya da, çıkışa ek ileti bilgileri dahil etmek için **false değerini** belirtin.

   * **Zaman aşımı**: bir iletinin boş bir sıraya gelmesi için ne kadar bekleneceğini öğrenmek için bir değer girin. Hiçbir şey girilmişse, sıradaki ilk ileti alınır ve bir iletinin görünmesini beklerken zaman harcanması beklenmez.

     ![Ileti özelliklerine gözatamıyorum](media/connectors-create-api-mq/Browse_message_Props.png)

1. Değişikliklerinizi **kaydedin** ve mantıksal uygulamanızı **çalıştırın** .

   ![Kaydet ve Çalıştır](media/connectors-create-api-mq/Save_Run.png)

   Çalıştırma bittikten sonra, çalıştıralım adımları gösterilir ve çıktıyı gözden geçirebilirsiniz.

1. Her adımın ayrıntılarını gözden geçirmek için yeşil onay işaretini seçin. Çıktı verileri hakkında daha fazla bilgi için **Ham çıkışları göster**' i seçin.

   ![İleti çıktısına gözatıp](media/connectors-create-api-mq/Browse_message_output.png)  

   Örnek ham çıktı aşağıda verilmiştir:

   ![İletiye yönelik ham çıkışa gözatıp](media/connectors-create-api-mq/Browse_message_raw_output.png)

1. **Includeınfo** değerini true olarak ayarlarsanız aşağıdaki çıktı görüntülenir:

   ![İleti ekleme bilgisini görüntüle](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>Birden çok iletiye gözatın

**Iletileri gözden** geçirme eylemi, kuyruktan kaç tane ileti döndürüleceğini göstermek Için bir **BatchSize** seçeneği içerir.  **BatchSize** girişi yoksa, tüm iletiler döndürülür. Döndürülen çıktı bir ileti dizisidir.

1. **Iletileri görüntüle** eylemini eklediğinizde, önceden yapılandırılmış ilk bağlantı varsayılan olarak seçilidir. Yeni bir bağlantı oluşturmak için **Bağlantıyı Değiştir**' i seçin. Ya da farklı bir bağlantı seçin.

1. Mantıksal uygulama çalıştırıldıktan sonra, **Iletilere gözatamıyorum** eyleminin bazı örnek çıktıları aşağıda verilmiştir:

   ![İletilere gitmek için çıkış](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-single-message"></a>Tek ileti al

**Ileti al** eylemi, **ileti görüntüle** eylemiyle aynı girişlere ve çıkışlara sahiptir. İleti **Al iletisi**kullanılırken ileti kuyruktan silinir.

## <a name="receive-multiple-messages"></a>Birden çok ileti alma

**Iletileri al** eylemi, **iletileri görüntüle** eylemiyle aynı giriş ve çıkışlara sahiptir. **Iletileri al**' ı kullanırken İletiler kuyruktan silinir.

Kuyrukta bir gözatmanız veya alma işlemi yaparken herhangi bir ileti yoksa, adım bu çıktıda başarısız olur:  

![MQ Ileti hatası yok](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-message"></a>İleti gönder

**Ileti gönder** eylemini eklediğinizde, önceden yapılandırılmış ilk bağlantı varsayılan olarak seçilidir. Yeni bir bağlantı oluşturmak için **Bağlantıyı Değiştir**' i seçin. Ya da farklı bir bağlantı seçin.

1. Geçerli bir ileti türü seçin: **Datagram**, **Yanıt**veya **istek**  

   ![Ileti props gönder](media/connectors-create-api-mq/Send_Msg_Props.png)

1. Mantıksal uygulama çalışmayı bitirdikten sonra **Ileti gönder** eyleminden alınan bazı örnek çıktıları aşağıda verilmiştir:

   ![Ileti çıkışı gönder](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Openapı (eski adıyla Swagger) açıklaması tarafından tanımlanan eylemler ve sınırlar hakkında teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/mq/)gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
