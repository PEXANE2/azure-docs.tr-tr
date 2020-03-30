---
title: IBM MQ sunucusuna bağlanın
description: Azure veya şirket içi IBM MQ sunucusu ve Azure Mantık Uygulamaları ile ileti gönderme ve alma
services: logic-apps
ms.suite: integration
author: ChristopherHouser
ms.author: chrishou
ms.reviewer: valthom, logicappspm
ms.topic: article
ms.date: 06/19/2019
tags: connectors
ms.openlocfilehash: 6bfd626c1ce69029ee720d24b0b143e7b4c3dd56
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77650956"
---
# <a name="connect-to-an-ibm-mq-server-from-azure-logic-apps"></a>Azure Logic Apps'tan ibm MQ sunucusuna bağlanma

IBM MQ konektörü, bir IBM MQ sunucusunda veya Azure'da depolanan iletileri gönderir ve alır. Bu bağlayıcı, bir TCP/IP ağında ki uzak bir IBM MQ sunucusuyla iletişim kuran bir Microsoft MQ istemcisi içerir. Bu makalede, MQ konektörü kullanmak için bir başlangıç kılavuzu sağlar. Kuyrukta tek bir iletiye göz atarak başlayabilir ve ardından diğer eylemleri deneyebilirsiniz.

IBM MQ bağlayıcısı bu eylemleri içerir, ancak tetikleyici sağlamaz:

- IBM MQ sunucusundan iletiyi silmeden tek bir iletiyi göz atın
- IBM MQ sunucusundaki iletileri silmeden bir yığın iletiye göz atın
- Tek bir ileti alın ve iletiyi IBM MQ sunucusundan silin
- Bir yığın ileti alın ve iletileri IBM MQ sunucusundan silin
- IBM MQ sunucusuna tek bir ileti gönderme

## <a name="prerequisites"></a>Ön koşullar

* Şirket içi bir MQ sunucusu kullanıyorsanız, şirket içi veri ağ geçidini ağınızdaki bir sunucuya [yükleyin.](../logic-apps/logic-apps-gateway-install.md) Şirket içi veri ağ geçidinin yüklü olduğu sunucuda, MQ konektörü için .NET Framework 4.6 yüklü olmalıdır. Şirket içi veri ağ geçidi için Azure'da bir kaynak da oluşturmanız gerekir. Daha fazla bilgi için [bkz.](../logic-apps/logic-apps-gateway-connection.md)

  Ancak, MQ sunucunuz azure'da herkese açık veya kullanılabilirse, veri ağ geçidini kullanmanız gerekir.

* Resmi olarak desteklenen IBM WebSphere MQ sürümleri:

  * MQ 7.5
  * M² 8.0
  * M² 9.0

* MQ eylemini eklemek istediğiniz mantık uygulaması. Bu mantık uygulaması, şirket içi veri ağ geçidi bağlantınızla aynı konumu kullanmalı ve iş akışınızı başlatan bir tetikleyiciye sahip olmalıdır. 

  MQ konektörüherhangi bir tetikleyicisi yoktur, bu nedenle önce mantık uygulamanıza bir tetikleyici eklemeniz gerekir. Örneğin, Yineleme tetikleyicisini kullanabilirsiniz. Mantık uygulamalarında yeniyseniz, ilk [mantık uygulamanızı oluşturmak için](../logic-apps/quickstart-create-first-logic-app-workflow.md)bu hızlı başlatmayı deneyin. 

## <a name="browse-a-single-message"></a>Tek bir iletiyi göz atın

1. Mantık uygulamanızda, tetikleyici veya başka bir eylem **altında, Yeni adım**seçin. 

1. Arama kutusunda "mq" yazın ve şu eylemi seçin: **İletiye göz atın**

   ![İletiye göz at](media/connectors-create-api-mq/Browse_message.png)

1. Varolan bir MQ bağlantınız yoksa, bağlantıyı oluşturun:  

   1. Eylemde, **şirket içi veri ağ geçidi üzerinden Bağlan'ı**seçin.
   
   1. MQ sunucunuzun özelliklerini girin.  

      **Sunucu**için, MQ sunucu adını girebilir veya ip adresini ve ardından bir üst nokta ve bağlantı noktası numarasını girebilirsiniz.
    
   1. Daha önce yapılandırılmış ağ geçidi bağlantılarını gösteren **ağ geçidi** listesini açın. Ağ geçidinizi seçin.
    
   1. İşiniz bittiğinde **Oluştur**’u seçin. 
   
      Bağlantınız şu örneğe benzer:

      ![Bağlantı Özellikleri](media/connectors-create-api-mq/Connection_Properties.png)

1. Eylemin özelliklerini ayarlama:

   * **Sıra**: Bağlantıdan farklı bir sıra belirtin.

   * **MessageId,** **CorrelationId,** **GroupId**ve diğer özellikler: Farklı MQ ileti özelliklerine dayalı bir iletiye göz atın

   * **IncludeInfo**: Çıktıya ek ileti bilgileri eklemek için **True'yu** belirtin. Veya çıktıya ek ileti bilgileri eklememek için **False'u** belirtin.

   * **Zaman Sonu**: İletinin boş bir sıraya gelmesini ne kadar bekleyeceğini belirlemek için bir değer girin. Hiçbir şey girilmezse, kuyruktaki ilk ileti alınır ve bir iletinin görünmesini beklemek için harcanan zaman yoktur.

     ![İleti Özelliklerine Gözat](media/connectors-create-api-mq/Browse_message_Props.png)

1. Değişikliklerinizi **kaydedin** ve ardından mantık uygulamanızı **çalıştırın.**

   ![Kaydet ve çalıştır](media/connectors-create-api-mq/Save_Run.png)

   Çalıştırma bittikten sonra, çalıştırmadan gelen adımlar gösterilir ve çıktıyı gözden geçirebilirsiniz.

1. Her adımın ayrıntılarını gözden geçirmek için yeşil onay işaretini seçin. Çıktı verileri hakkında daha fazla bilgi gözden geçirmek için **ham çıktıları göster'i**seçin.

   ![İleti çıktısını gözatın](media/connectors-create-api-mq/Browse_message_output.png)  

   Burada bazı örnek ham çıktı:

   ![İleti ham çıktıya göz atın](media/connectors-create-api-mq/Browse_message_raw_output.png)

1. **IncludeInfo'yu** doğru olarak ayarlarsanız, aşağıdaki çıktı görüntülenir:

   ![İletiye göz at: bilgi ekle](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>Birden çok iletiye göz atma

**Gözat iletileri** eylemi, kuyruktan kaç iletinin döndürüleceğini belirtmek için toplu **iş boyutu** seçeneği içerir.  **BatchSize'ın** girişi yoksa, tüm iletiler döndürülür. Döndürülen çıktı bir dizi iletidir.

1. **Gözat iletileri** eylemini eklediğinizde, önceden yapılandırılmış ilk bağlantı varsayılan olarak seçilir. Yeni bir bağlantı oluşturmak için **bağlantıyı değiştir'i**seçin. Veya farklı bir bağlantı seçin.

1. Mantık uygulaması çalışması bittikten sonra, **Gözat iletileri** eyleminden bazı örnek çıktıaşağıda veremilmektedir:

   ![İleti çıktılarına göz atın](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-single-message"></a>Tek ileti alma

**İletiyi Al** eylemi, **Gözat iletisi** eylemiyle aynı giriş ve çıktılara sahiptir. **İletiyi Al**kullanırken, ileti kuyruktan silinir.

## <a name="receive-multiple-messages"></a>Birden çok ileti alma

**İletileri Al** eylemi, **Gözat iletileri** eylemiyle aynı giriş ve çıktılara sahiptir. **İletileri Al'ı**kullanırken, iletiler kuyruktan silinir.

Göz atma veya alma yaparken kuyrukta ileti yoksa, adım bu çıktıyla başarısız olur:  

![MQ İleti Hatası Yok](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-message"></a>İleti gönder

**İletigönder** eylemini eklediğinizde, önceden yapılandırılmış ilk bağlantı varsayılan olarak seçilir. Yeni bir bağlantı oluşturmak için **bağlantıyı değiştir'i**seçin. Veya farklı bir bağlantı seçin.

1. Geçerli bir ileti türü seçin: **Datagram**, **Yanıtla**, veya **İstek**  

   ![Msg Sahne Gönder](media/connectors-create-api-mq/Send_Msg_Props.png)

1. Mantık uygulaması çalışma bittikten sonra, **ileti gönder** eyleminden bazı örnek çıktıaşağıda veremiştir:

   ![Msg Çıktıgönder](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler, eylemler ve konektörün Swagger dosyasında açıklandığı gibi sınırlar gibi bu bağlayıcı hakkında daha fazla teknik ayrıntı için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/mq/)bakın.

> [!NOTE]
> [Bir entegrasyon hizmeti ortamındaki (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)mantık uygulamaları için, bu bağlayıcının İmKB etiketli sürümü bunun yerine [İmKB ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
