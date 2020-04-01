---
title: IBM MQ sunucusuna bağlanın
description: Azure veya şirket içi IBM MQ sunucusu ve Azure Mantık Uygulamaları ile ileti gönderme ve alma
services: logic-apps
ms.suite: integration
author: ChristopherHouser
ms.author: chrishou
ms.reviewer: valthom, logicappspm
ms.topic: article
ms.date: 03/31/2020
tags: connectors
ms.openlocfilehash: 737c5b90b216156ca08346f4a64fd0b421ad6c19
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80410190"
---
# <a name="connect-to-an-ibm-mq-server-from-azure-logic-apps"></a>Azure Logic Apps'tan ibm MQ sunucusuna bağlanma

IBM MQ konektörü, bir IBM MQ sunucusunda veya Azure'da depolanan iletileri gönderir ve alır. Bu bağlayıcı, bir TCP/IP ağında ki uzak bir IBM MQ sunucusuyla iletişim kuran bir Microsoft MQ istemcisi içerir. Bu makalede, MQ konektörü kullanmak için bir başlangıç kılavuzu sağlar. Kuyrukta tek bir iletiye göz atarak başlayabilir ve ardından diğer eylemleri deneyebilirsiniz.

IBM MQ bağlayıcısı bu eylemleri içerir, ancak tetikleyici sağlamaz:

- IBM MQ sunucusundan gelen iletiyi silmeden tek bir iletiye göz atın.
- IBM MQ sunucusundaki iletileri silmeden bir dizi iletiye göz atın.
- Tek bir ileti alın ve iletiyi IBM MQ sunucusundan silin.
- Bir dizi ileti alın ve iletileri IBM MQ sunucusundan silin.
- IBM MQ sunucusuna tek bir ileti gönderin.

Burada resmen desteklenen IBM WebSphere MQ sürümleri şunlardır:

  * MQ 7.5
  * M² 8.0
  * M² 9.0

## <a name="prerequisites"></a>Ön koşullar

* Şirket içi bir MQ sunucusu kullanıyorsanız, şirket içi veri ağ geçidini ağınızdaki bir sunucuya [yükleyin.](../logic-apps/logic-apps-gateway-install.md) Şirket içi veri ağ geçidinin yüklü olduğu sunucuda, MQ konektörü için .NET Framework 4.6 yüklü olmalıdır.

  Ağ geçidini yüklemeyi bitirdikten sonra, şirket içi veri ağ geçidi için Azure'da bir kaynak da oluşturmanız gerekir. Daha fazla bilgi için [bkz.](../logic-apps/logic-apps-gateway-connection.md)

  MQ sunucunuz herkese açıksa veya Azure'da kullanılabilmişse, veri ağ geçidini kullanmanız gerekir.

* MQ eylemini eklemek istediğiniz mantık uygulaması. Bu mantık uygulaması, şirket içi veri ağ geçidi bağlantınızla aynı konumu kullanmalı ve iş akışınızı başlatan bir tetikleyiciye sahip olmalıdır.

  MQ konektörüherhangi bir tetikleyicisi yoktur, bu nedenle önce mantık uygulamanıza bir tetikleyici eklemeniz gerekir. Örneğin, Yineleme tetikleyicisini kullanabilirsiniz. Mantık uygulamalarında yeniyseniz, ilk [mantık uygulamanızı oluşturmak için](../logic-apps/quickstart-create-first-logic-app-workflow.md)bu hızlı başlatmayı deneyin.

<a name="create-connection"></a>

## <a name="create-mq-connection"></a>MQ bağlantısı oluşturma

Bir MQ eylemi eklediğinizde zaten bir MQ bağlantınız yoksa, örneğin bağlantıyı oluşturmanız istenir:

![Bağlantı bilgileri sağlama](media/connectors-create-api-mq/connection-properties.png)

1. Şirket içi bir MQ sunucusuna bağlanıyorsanız, **şirket içi veri ağ geçidi üzerinden Bağlan'ı**seçin.

1. MQ sunucunuz için bağlantı bilgilerini sağlayın.

   * **Sunucu**için, MQ sunucu adını girebilir veya ip adresini ve ardından bir üst nokta ve bağlantı noktası numarasını girebilirsiniz.

   * Güvenli Soket katmanı (SSL) kullanmak için **SSL'yi etkinleştir'i**seçin.

     MQ bağlayıcısı şu anda istemci kimlik doğrulamasını değil, yalnızca sunucu kimlik doğrulamasını destekler. Daha fazla bilgi için [bağlantı ve kimlik doğrulama sorunlarına](#connection-problems)bakın.

1. Ağ **geçidi** bölümünde aşağıdaki adımları izleyin:

   1. **Abonelik** listesinden, Azure ağ geçidi kaynağınızla ilişkili Azure aboneliğini seçin.

   1. Bağlantı **Ağ Geçidi** listesinden, kullanmak istediğiniz Azure ağ geçidi kaynağını seçin.

1. İşiniz bittiğinde **Oluştur**’u seçin.

<a name="connection-problems"></a>

### <a name="connection-and-authentication-problems"></a>Bağlantı ve kimlik doğrulama sorunları

Mantık uygulamanız şirket içi MQ sunucunuza bağlanmayı denediğinde şu hatayı alabilirsiniz:

`"MQ: Could not Connect the Queue Manager '<queue-manager-name>': The Server was expecting an SSL connection."`

* MQ bağlayıcısını doğrudan Azure'da kullanıyorsanız, MQ sunucusunun güvenilir bir [sertifika yetkilisi](https://www.ssl.com/faqs/what-is-a-certificate-authority/)tarafından verilmiş bir sertifika kullanması gerekir.

* Şirket içi veri ağ geçidini kullanıyorsanız, güvenilir bir sertifika yetkilisi tarafından verilen bir [sertifikayı](https://www.ssl.com/faqs/what-is-a-certificate-authority/) mümkün olduğunda kullanmayı deneyin. Ancak, bu seçenek mümkün değilse, güvenilir bir [sertifika yetkilisi](https://www.ssl.com/faqs/what-is-a-certificate-authority/) tarafından düzenlenmeyen ve daha az güvenli olarak kabul edilen kendi imzalı bir sertifika kullanabilirsiniz.

  Sunucunun kendi imzalı sertifikasını yüklemek için **Windows Sertifika Yöneticisi** (certmgr.msc) aracını kullanabilirsiniz. Bu senaryo için, şirket içi veri ağ geçidi hizmetinin çalıştığı yerel bilgisayarınızda, sertifikayı Güvenilir Kök **Sertifika Yetkilileri** düzeyinde **Yerel Bilgisayar** sertifikaları mağazanıza yüklemeniz gerekir.

  1. Şirket içi veri ağ geçidi hizmetinin çalıştığı bilgisayarda başlat menüsünü açın, **kullanıcı sertifikalarını bul**ve yönet'i seçin.

  1. Windows Sertifika Yöneticisi aracı açıldıktan **sonra, Sertifikalar - Yerel Bilgisayar** >  **Güvenilen Kök Sertifika Yetkilileri** klasörüne gidin ve sertifikayı yükleyin.

     > [!IMPORTANT]
     > **Sertifikalar** > - Yerel Bilgisayar**Güvenilen Kök Sertifika Yetkilileri** deposuna sertifika yüklediğinizden emin olun.

* MQ sunucusu, SSL bağlantıları için kullanmak istediğiniz şifreleme belirtimini tanımlamanızı gerektirir. Ancak, .NET'teki SsLStream, şifreleme belirtimleri için siparişi belirtmenize izin vermez. Bu sınırlamayı çözmek için, MQ sunucu yapılandırmanızı bağlayıcının SSL anlaşmasında gönderdiği paketteki ilk şifreleme belirtimiyle eşleşecek şekilde değiştirebilirsiniz.

  Bağlantıyı denediğinizde, MQ sunucusu, diğer uç yanlış şifreleme belirtimini kullandığından bağlantının başarısız olduğunu belirten bir olay iletisi kaydeder. Olay iletisi, listede ilk görünen şifreleme belirtimini içerir. Olay iletisindeki şifreleme belirtimine uyacak şekilde kanal yapılandırmasındaki şifreleme belirtimini güncelleştirin.

## <a name="browse-single-message"></a>Tek mesaja göz atma

1. Mantık uygulamanızda, tetikleyici veya başka bir eylem altında **Yeni adım'ı**seçin.

1. Arama kutusuna, `mq` **İletiye Gözat** eylemini girin ve seçin.

   !["İletiye göz at" eylemini seçin](media/connectors-create-api-mq/browse-message.png)

1. Daha önce bir MQ bağlantısı oluşturmadıysanız, bu [bağlantıyı oluşturmanız](#create-connection)istenir.

1. Bağlantıyı oluşturduktan sonra, **Gözat iletisi** eyleminin özelliklerini ayarlayın:

   | Özellik | Açıklama |
   |----------|-------------|
   | **Sıra** | Bağlantıda belirtilen sıradan farklıysa, bu sırayı belirtin. |
   | **MessageId**, **CorrelationId**, **GroupId**, ve diğer özellikleri | Farklı MQ ileti özelliklerini temel alan bir iletiiçin göz atın |
   | **IncludeInfo** | Çıktıya ek ileti bilgileri eklemek için **true'yu**seçin. Çıktıdaki ek ileti bilgilerini atlamak için **yanlış'ı**seçin. |
   | **Zaman aşımı** | İletinin boş bir sıraya gelmesini ne kadar bekleyeceğimi belirlemek için bir değer girin. Hiçbir şey girilmezse, kuyruktaki ilk ileti alınır ve bir iletinin görünmesini beklemek için harcanan zaman yoktur. |
   |||

   Örnek:

   !["İletiye göz at" eylemi için özellikler](media/connectors-create-api-mq/browse-message-properties.png)

1. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin. Uygulamanızı test etmek için **Çalıştır'ı**seçin.

   Çalıştırma bittikten sonra, tasarımcı çıktıyı gözden geçirebilmeniz için iş akışı adımlarını ve durumlarını gösterir.

1. Her adımla ilgili ayrıntıları görüntülemek için adımın başlık çubuğunu tıklatın. Bir adımın çıktısı hakkında daha fazla bilgi gözden geçirmek için **ham çıktıları göster'i**seçin.

   ![İleti çıktısını gözatın](media/connectors-create-api-mq/browse-message-output.png)

   Burada bazı örnek ham çıktı:

   ![İleti ham çıktıya göz atın](media/connectors-create-api-mq/browse-message-raw-output.png)

1. **IncludeInfo'nun** **doğru**olmasını ayarlarsanız, ek çıktı gösterilir:

   ![İletiye göz at: bilgi ekle](media/connectors-create-api-mq/browse-message-include-info.png)

## <a name="browse-multiple-messages"></a>Birden çok iletiye göz atma

**Gözat iletileri** eylemi, kuyruktan kaç iletinin döneceğini belirtmek için Toplu **Boyut** seçeneğini içerir. **BatchSize'ın** değeri yoksa, tüm iletiler döndürülür. Döndürülen çıktı bir dizi iletidir.

1. Önceki adımları izleyin, ancak bunun yerine **Gözat iletileri** eylemini ekleyin.

1. Daha önce bir MQ bağlantısı oluşturmadıysanız, bu [bağlantıyı oluşturmanız](#create-connection)istenir. Aksi takdirde, varsayılan olarak, önceden yapılandırılmış ilk bağlantı kullanılır. Yeni bir bağlantı oluşturmak için **bağlantıyı değiştir'i**seçin. Veya farklı bir bağlantı seçin.

1. Eylem için bilgi sağlayın.

1. Mantık uygulamasını kaydedin ve çalıştırın.

   Mantık uygulaması çalışma bittikten sonra, **Gözat iletileri** eyleminden bazı örnek çıktı aşağıda veremiştir:

   ![Örnek "İletilere göz atma" çıktısı](media/connectors-create-api-mq/browse-messages-output.png)

## <a name="receive-single-message"></a>Tek ileti alma

**İletiyi Al** eylemi, **Gözat iletisi** eylemiyle aynı giriş ve çıktılara sahiptir. **İleti al'ı**kullandığınızda, ileti kuyruktan silinir.

## <a name="receive-multiple-messages"></a>Birden çok ileti alma

**İletileri Al** eylemi, **Gözat iletileri** eylemiyle aynı giriş ve çıktılara sahiptir. **İletileri Al'ı**kullandığınızda, iletiler kuyruktan silinir.

> [!NOTE]
> Herhangi bir ileti soramayan bir kuyruğa göz atma veya alma eylemi çalıştırırken, eylem bu çıktıyla başarısız olur:
>
> ![MQ "ileti yok" hatası](media/connectors-create-api-mq/mq-no-message-error.png)

## <a name="send-message"></a>İleti gönder

1. Önceki adımları izleyin, ancak bunun yerine **ileti gönder** eylemini ekleyin.

1. Daha önce bir MQ bağlantısı oluşturmadıysanız, bu [bağlantıyı oluşturmanız](#create-connection)istenir. Aksi takdirde, varsayılan olarak, önceden yapılandırılmış ilk bağlantı kullanılır. Yeni bir bağlantı oluşturmak için **bağlantıyı değiştir'i**seçin. Veya farklı bir bağlantı seçin.

1. Eylem için bilgi sağlayın. **MessageType**için geçerli bir ileti türü seçin: **Datagram**, **Yanıtla**veya **İstek**

   !["İleti eylemi gönder" için özellikler](media/connectors-create-api-mq/send-message-properties.png)

1. Mantık uygulamasını kaydedin ve çalıştırın.

   Mantık uygulaması çalışma bittikten sonra, **ileti gönder** eyleminden bazı örnek çıktıaşağıda veremiştir:

   ![Örnek "İleti gönder" çıktısı](media/connectors-create-api-mq/send-message-output.png)

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Konektörün Swagger açıklamasıyla açıklanan eylemler ve sınırlar hakkında teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/mq/)inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
