---
title: Microsoft Graph Security & güvenlik işlemlerini tümleştirme ve yönetme
description: Microsoft Graph Security & Azure Logic Apps ile uygulamanızın tehdit koruması, algılama ve yanıtını geliştirin
services: logic-apps
ms.suite: integration
author: preetikr
ms.author: preetikr
ms.reviewer: v-ching, estfan, logicappspm
ms.topic: article
ms.date: 02/21/2020
tags: connectors
ms.openlocfilehash: b4f51b192d1a7c0ee14a769321793753e8217dea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77598842"
---
# <a name="improve-threat-protection-by-integrating-security-operations-with-microsoft-graph-security--azure-logic-apps"></a>Güvenlik işlemlerini Microsoft Graph Security & Azure Logic Apps ile tümleştirerek tehdit koruması geliştirme

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve Microsoft Graph [Security](https://docs.microsoft.com/graph/security-concept-overview) bağlayıcısı ile, Microsoft güvenlik ürünlerini, hizmetlerini ve iş ortaklarını entegre etmek için otomatik iş akışları oluşturarak uygulamanızın tehditleri nasıl algıladığını, koruduğunu ve yanıt verdiğinizi geliştirebilirsiniz. Örneğin, uyarılar gibi Microsoft Graph Security varlıklarını izleyen ve yöneten [Azure Güvenlik Merkezi oyun kitapları](../security-center/security-center-playbooks.md) oluşturabilirsiniz. Microsoft Graph Security bağlayıcısı tarafından desteklenen bazı senaryolar şunlardır:

* Sorgulara veya uyarı kimliğine dayalı uyarılar alın. Örneğin, yüksek önem uyarılarını içeren bir liste alabilirsiniz.

* Uyarıları güncelleştirin. Örneğin, uyarı atamalarını güncelleyebilir, uyarılara yorum ekleyebilir veya uyarıları etiketleyebilirsiniz.

* [Uyarı abonelikleri (webhooks)](https://docs.microsoft.com/graph/api/resources/webhooks)oluşturarak uyarılar oluşturulduğunda veya değiştirildiğinde izleyin.

* Uyarı aboneliklerinizi yönetin. Örneğin, etkin abonelikler alabilir, aboneliğin son kullanma süresini uzatabilir veya abonelikleri silebilirsiniz.

Mantık uygulamanızın iş akışı, Microsoft Graph Security bağlayıcısından yanıt alan eylemleri kullanabilir ve bu çıktıyı iş akışınızdaki diğer eylemler için kullanılabilir hale getirebilir. Microsoft Graph Security bağlayıcı sıyrık eylemlerinden elde edilen çıktıyı kullanarak iş akışınızda başka eylemler de yapabilirsiniz. Örneğin, Microsoft Graph Security bağlayıcısı aracılığıyla yüksek önem uyarılarını alırsanız, bu uyarıları Outlook bağlayıcısını kullanarak bir e-posta iletisinde gönderebilirsiniz. 

Microsoft Graph Security hakkında daha fazla bilgi edinmek için [Microsoft Graph Security API genel bakış'a](https://aka.ms/graphsecuritydocs)bakın. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir inceleyin?](../logic-apps/logic-apps-overview.md) Microsoft Flow veya PowerApps'ı arıyorsanız, [Bkz. Akış nedir](https://flow.microsoft.com/) veya [PowerApps nedir?](https://powerapps.microsoft.com/)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Microsoft Graph Güvenlik bağlayıcısını kullanmak için Azure Active Directory (AD) kiracısının yöneticisinin *açıkça verilmiş* onayı olması gerekir. Bu, [Microsoft Graph Güvenlik Kimlik Doğrulaması gereksinimlerinin](https://aka.ms/graphsecurityauth) bir parçasıdır. Bu onay, [Azure portalında](https://portal.azure.com)da bulabileceğiniz Microsoft Graph Security bağlayıcısının uygulama kimliğini ve adını gerektirir:

  | Özellik | Değer |
  |----------|-------|
  | **Uygulama Adı** | `MicrosoftGraphSecurityConnector` |
  | **Başvuru Kimliği** | `c4829704-0edc-4c3d-a347-7c4a67586f3c` |
  |||

  Bağlayıcı için izin vermek için Azure AD kiracı yöneticiniz aşağıdaki adımları izleyebilir:

  * [Azure AD uygulamalarına kiracı yöneticisi onayı verme](../active-directory/develop/v2-permissions-and-consent.md).

  * Mantıksal uygulamanız ilk kez çalıştırıldığında [uygulama onayı deneyimi](../active-directory/develop/application-consent-experience.md) aracılığıyla uygulamanız Azure AD kiracınızın yöneticisinden onay isteyebilir.
   
* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Uyarılar gibi Microsoft Graph Security varlıklarınıza erişmek istediğiniz mantık uygulaması. Microsoft Graph Security tetikleyicisi kullanmak için boş bir mantık uygulamasına ihtiyacınız var. Microsoft Graph Security eylemini kullanmak için, senaryonuz için uygun tetikleyiciyle başlayan bir mantık uygulamasına ihtiyacınız var.

## <a name="connect-to-microsoft-graph-security"></a>Microsoft Graph Security'ye bağlanma 

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com/)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Boş mantık uygulamaları için, microsoft grafik güvenlik eylemi eklemeden önce tetikleyiciyi ve istediğiniz diğer eylemleri ekleyin.

   -veya-

   Varolan mantık uygulamaları için, Microsoft Graph Security eylemi eklemek istediğiniz son adım altında **Yeni adım'ı**seçin.

   -veya-

   Adımlar arasında eylem eklemek için işaretçinizin üzerine adımların arasında ilerleyin. Görünen artı işaretini (+) seçin ve **eylem ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak "microsoft graph security" girin. Eylemler listesinden, istediğiniz eylemi seçin.

1. Microsoft Graph Security kimlik bilgilerinizle oturum açın.

1. Seçtiğiniz eylem için gerekli ayrıntıları sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="add-triggers"></a>Tetikleyiciler ekleme

Azure Logic Apps'ta, her mantık uygulaması, belirli bir olay olduğunda veya belirli bir koşul yerine getirildiğinde ateş eden bir [tetikleyiciyle](../logic-apps/logic-apps-overview.md#logic-app-concepts)başlamalıdır. Tetikleyici her ateşleninse, Logic Apps motoru bir mantık uygulaması örneği oluşturur ve uygulamanızın iş akışını çalıştırmaya başlar.

> [!NOTE] 
> Bir tetikleyici ateşlendiğinde, tetikleyici tüm yeni uyarıları işler. Uyarı alınmazsa, tetikleyici çalıştırma atlanır. Bir sonraki tetikleyici yoklaması, tetikleyicinin özelliklerinde belirttiğiniz yineleme aralığına göre gerçekleşir.

Bu örnek, uygulamanıza yeni uyarılar gönderildiğinde bir mantık uygulaması iş akışını nasıl başlatabileceğinizi gösterir.

1.  Azure portalında veya Visual Studio'da, Logic App Designer'ı açan boş bir mantık uygulaması oluşturun. Bu örnekte Azure portalı kullanır.

1.  Tasarımcıda, arama kutusuna filtreniz olarak "microsoft graph security" girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Tüm yeni uyarılarda**

1.  Tetikleyicide, izlemek istediğiniz uyarılar hakkında bilgi sağlayın. Daha fazla özellik için **yeni parametre** listesi ekle'yi açın ve bu özelliği tetikleyiciye eklemek için bir parametre seçin.

   | Özellik | Emlak (JSON) | Gerekli | Tür | Açıklama |
   |----------|-----------------|----------|------|-------------|
   | **Interval** | `interval` | Evet | Tamsayı | İş akışının sıklığına bağlı olarak ne sıklıkta çalıştığını açıklayan pozitif bir tamsayı. Minimum ve maksimum aralıklar şunlardır: <p><p>- Ay: 1-16 ay <br>- Gün: 1-500 gün <br>- Saat: 1-12.000 saat <br>- Dakika: 1-72.000 dakika <br>- İkinci: 1-9,999,999 saniye <p>Örneğin, aralık 6 ise ve sıklık "Ay" ise, yineleme her 6 ayda bir olur. |
   | **Frequency** | `frequency` | Evet | Dize | Yineleme için zaman birimi: **İkinci**, **Dakika**, **Saat**, **Gün**, **Hafta**, veya **Ay** |
   | **Saat dilimi** | `timeZone` | Hayır | Dize | Bu tetikleyici [UTC ofset](https://en.wikipedia.org/wiki/UTC_offset)kabul etmediğinden, yalnızca bir başlangıç saati belirttiğiniz zaman geçerlidir. Uygulamak istediğiniz saat dilimini seçin. |
   | **Başlangıç saati** | `startTime` | Hayır | Dize | Bu biçimde bir başlangıç tarihi ve saati sağlayın: <p><p>Saat dilimi seçerseniz YYYY-MM-DDThh:mm:ss <p>-veya- <p>Saat dilimi seçmezseniz YYYY-MM-DDThh:mm:ssZ <p>Örneğin, 18 Eylül 2017 saat 14:00'te istiyorsanız, "2017-09-18T14:00:00" seçeneğini belirleyin ve Pasifik Standart Saati gibi bir saat dilimi seçin. Veya saat dilimi olmadan "2017-09-18T14:00:00Z" olarak belirtiniz. <p>**Not:** Bu başlangıç süresi gelecekte en fazla 49 yıl vardır ve [UTC tarih saati biçiminde](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) [ISO 8601 tarih saati belirtimini](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) takip etmelidir, ancak [UTC mahsup](https://en.wikipedia.org/wiki/UTC_offset)olmadan. Bir saat dilimi seçmezseniz, herhangi bir boşluk olmadan sonuna "Z" harfini eklemeniz gerekir. Bu "Z" eşdeğer [denizcilik zamanı](https://en.wikipedia.org/wiki/Nautical_time)anlamına gelir. <p>Basit zamanlamalar için başlangıç zamanı ilk olaydır, karmaşık zamanlamalar için tetikleyici başlangıç saatinden daha erken ateş etmez. [*Başlangıç tarihini ve saatini kullanma yolları nelerdir?*](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time) |
   ||||||

1.  Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

1.  Şimdi, tetikleyici sonuçlarıyla gerçekleştirmek istediğiniz görevler için mantık uygulamanıza bir veya daha fazla eylem eklemeye devam edin.

## <a name="add-actions"></a>Eylem ekleme

Aşağıda, Microsoft Graph Security bağlayıcısıyla kullanılabilen çeşitli eylemleri kullanma yla ilgili daha ayrıntılı ayrıntılar verilmiştir.

### <a name="manage-alerts"></a>Uyarıları yönetme

Filtrelemek, sıralamak veya en son sonuçları almak için yalnızca [Microsoft Graph tarafından desteklenen ODATA sorgu parametrelerini](https://docs.microsoft.com/graph/query-parameters)sağlayın. *only* Temel *URL'nin* tamamını veya HTTP eylemini (örneğin) `https://graph.microsoft.com/v1.0/security/alerts` `GET` veya `PATCH` işlemi belirtmeyin. Aşağıda, yüksek önem **uyarılarına** sahip bir liste istediğinizde uyarı al eyleminin parametrelerini gösteren belirli bir örnek verilmiştir:

`Filter alerts value as Severity eq 'high'`

Bu bağlayıcıyla kullanabileceğiniz sorgular hakkında daha fazla bilgi için [Microsoft Graph Security uyarıları başvuru belgelerine](https://docs.microsoft.com/graph/api/alert-list)bakın. Bu bağlayıcıyla gelişmiş deneyimler oluşturmak için, konektörün desteklediği [şema özellikleri uyarıları](https://docs.microsoft.com/graph/api/resources/alert) hakkında daha fazla bilgi edinin.

| Eylem | Açıklama |
|--------|-------------|
| **Uyarıları alın** | Örneğin, `Provider eq 'Azure Security Center' or 'Palo Alto Networks'`bir veya daha fazla [uyarı özelliğine](https://docs.microsoft.com/graph/api/resources/alert)göre filtreuygulanmış uyarıları alın. | 
| **Kimlikle uyarı alın** | Uyarı kimliğine göre belirli bir uyarı alın. | 
| **Uyarıyı güncelleştir** | Uyarı kimliğine dayalı olarak belirli bir uyarıyı güncelleştirin. İsteğinizde gerekli ve değiştirilebilir özellikleri geçtiğinizden emin olmak [için uyarılar için değiştirilebilir özelliklere](https://docs.microsoft.com/graph/api/alert-update)bakın. Örneğin, bir güvenlik analistine bir uyarı atamak böylece araştırma yapabilir, uyarının **Atanmış** özelliğini güncelleştirebilirsiniz. |
|||

### <a name="manage-alert-subscriptions"></a>Uyarı aboneliklerini yönetme

Microsoft Graph abonelikleri veya [*web hook'ları*](https://docs.microsoft.com/graph/api/resources/webhooks)destekler. [*subscriptions*](https://docs.microsoft.com/graph/api/resources/subscription) Abonelikleri almak, güncelleştirmek veya silmek için Microsoft [Graph tarafından desteklenen ODATA sorgu parametrelerini](https://docs.microsoft.com/graph/query-parameters) Microsoft Graph varlık yapısına sağlayın ve ardından ODATA sorgusunu ekleyin. `security/alerts` Temel *URL'yi eklemeyin,* `https://graph.microsoft.com/v1.0`örneğin. Bunun yerine, bu örnekte biçimi kullanın:

`security/alerts?$filter=status eq 'New'`

| Eylem | Açıklama |
|--------|-------------|
| **Abonelikleri oluşturma** | Değişiklikler hakkında sizi belirten [bir abonelik oluşturun.](https://docs.microsoft.com/graph/api/subscription-post-subscriptions) Bu aboneliği istediğiniz belirli uyarı türleri için filtreleyebilirsiniz. Örneğin, yüksek önem uyarılarını size bildiren bir abonelik oluşturabilirsiniz. |
| **Etkin abonelikler alın** | [Süresi dolmamış abonelikleri alın.](https://docs.microsoft.com/graph/api/subscription-list) | 
| **Aboneliği güncelleştir** | Abonelik kimliğini sağlayarak [aboneliği güncelleştirin.](https://docs.microsoft.com/graph/api/subscription-update) Örneğin, aboneliğinizi genişletmek için aboneliğin `expirationDateTime` özelliğini güncelleştirebilirsiniz. | 
| **Aboneliği silme** | Abonelik kimliğini sağlayarak [aboneliği silin.](https://docs.microsoft.com/graph/api/subscription-delete) | 
||| 

### <a name="manage-threat-intelligence-indicators"></a>Tehdit istihbaratı göstergelerini yönetme

Filtrelemek, sıralamak veya en son sonuçları almak için yalnızca [Microsoft Graph tarafından desteklenen ODATA sorgu parametrelerini](https://docs.microsoft.com/graph/query-parameters)sağlayın. *only* Temel *URL'nin* tamamını veya HTTP eylemini (örneğin) `https://graph.microsoft.com/beta/security/tiIndicators` `GET` veya `PATCH` işlemi belirtmeyin. Tehdit türüne sahip `DDoS` bir liste istediğinizde **tiIndicators alın** eyleminin parametrelerini gösteren belirli bir örnek aşağıda verilmiştir:

`Filter threat intelligence indicator value as threatType eq 'DDoS'`

Bu bağlayıcıyla kullanabileceğiniz sorgular hakkında daha fazla bilgi için [Microsoft Graph Security tehdit zekası göstergesi başvuru belgelerinde "İsteğe Bağlı Sorgu Parametreleri"](https://docs.microsoft.com/graph/api/tiindicators-list?view=graph-rest-beta&tabs=http)adlı başvuruya bakın. Bu bağlayıcıyla gelişmiş deneyimler oluşturmak için, konektörün desteklediği [şema özellikleri tehdit istihbarat göstergesi](https://docs.microsoft.com/graph/api/resources/tiindicator?view=graph-rest-beta) hakkında daha fazla bilgi edinin.

| Eylem | Açıklama |
|--------|-------------|
| **Tehdit istihbaratı göstergelerini alın** | TiIndicators'ı bir veya daha fazla [tiIndicator özelliğine](https://docs.microsoft.com/graph/api/resources/tiindicator?view=graph-rest-beta)göre filtreleme (örneğin,`threatType eq 'MaliciousUrl' or 'DDoS'` |
| **Kimlikle tehdit istihbarat göstergesi alma** | tiIndicator KIMLIĞIne dayalı belirli bir tiIndicator alın. | 
| **Tehdit istihbarat göstergesi oluşturma** | tiIndicators koleksiyonuna göndererek yeni bir tiIndicator oluşturun. İsteğinizde gerekli özellikleri geçtiğinizden emin olmak [için, tiIndicator oluşturmak için gerekli özelliklere](https://docs.microsoft.com/graph/api/tiindicators-post?view=graph-rest-beta&tabs=http)bakın. |
| **Birden çok tehdit istihbarat göstergesi gönderme** | Bir tiIndicators koleksiyonu göndererek birden çok yeni tiIndicators oluşturun. İsteğiniz içinde gerekli özellikleri geçtiğinizden emin olmak [için, birden çok tiIndicators göndermek için gerekli özelliklere](https://docs.microsoft.com/graph/api/tiindicator-submittiindicators?view=graph-rest-beta&tabs=http)bakın. |
| **Tehdit istihbarat göstergesini güncelleştirme** | tiIndicator kimliğini temel alan belirli bir tiIndicator'ı güncelleştirin. İsteğinizde gerekli ve değiştirilebilir özellikleri geçtiğinizden emin olmak [için, tiIndicator için değiştirilebilir özelliklere](https://docs.microsoft.com/graph/api/tiindicator-update?view=graph-rest-beta&tabs=http)bakın. Örneğin, gösterge hedefÜrün güvenlik aracı nın içinden eşleşirse uygulanacak eylemi güncelleştirmek için tiIndicator'ın **eylem** özelliğini güncelleştirebilirsiniz. |
| **Birden çok tehdit istihbarat göstergesini güncelleştirme** | Birden çok tiIndicators güncelleştirin. İsteğiniz içinde gerekli özellikleri geçtiğinizden emin olmak için, [birden çok tiIndicators güncelleştirmek için gerekli özelliklere](https://docs.microsoft.com/graph/api/tiindicator-updatetiindicators?view=graph-rest-beta&tabs=http)bakın. |
| **Tehdit istihbarat göstergesini kimlikle silme** | tiIndicator kimliğine göre belirli bir tiIndicator'ı silin. |
| **Birden çok tehdit istihbarat göstergesini dizinlere göre silme** | Birden çok tiIndicators'ı kimliklerini silebilmektedir. İsteğiniz içinde gerekli özellikleri geçtiğinizden emin olmak [için, birden çok tiIndicators'ı kimliklerle silerlemek için gerekli özelliklere](https://docs.microsoft.com/graph/api/tiindicator-deletetiindicators?view=graph-rest-beta&tabs=http)bakın. |
| **Dış tetkiler tarafından birden çok tehdit istihbarat göstergesini silme** | Harici kimlikler tarafından birden çok tiIndicators silin. İsteğiniz içinde gerekli özellikleri geçtiğinizden emin olmak [için, birden çok tiIndicator'ı harici kimliklerle silerlemek için gerekli özelliklere](https://docs.microsoft.com/graph/api/tiindicator-deletetiindicatorsbyexternalid?view=graph-rest-beta&tabs=http)bakın. |
|||

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için bağlayıcının [başvuru sayfasını](https://aka.ms/graphsecurityconnectorreference)inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
