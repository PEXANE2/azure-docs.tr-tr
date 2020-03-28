---
title: Zamanlama tabanlı otomatik iş akışları oluşturma
description: Öğretici - Azure Logic Apps'ı kullanarak zamanlama tabanlı, yinelenen, otomatik leştirilmiş bir iş akışı oluşturun
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: tutorial
ms.custom: mvc
ms.date: 09/12/2019
ms.openlocfilehash: 17802228c8f08e3c8f1533296e2d39080f6f8b7a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75456623"
---
# <a name="tutorial-create-automated-schedule-based-recurring-workflows-by-using-azure-logic-apps"></a>Öğretici: Azure Logic Apps'ı kullanarak otomatik, zamanlama tabanlı, yinelenen iş akışları oluşturun

Bu öğretici, bir [mantık uygulaması](../logic-apps/logic-apps-overview.md) oluşturmak ve bir zamanlama üzerinde çalışan yinelenen bir iş akışını otomatikleştirmek nasıl gösterir. Özellikle, bu örnek mantık uygulaması hafta içi her sabah çalışır ve iki yer arasında trafik de dahil olmak üzere seyahat süresini denetler. Zaman belirli bir sınırı aşarsa mantıksal uygulama, hedefiniz için seyahat süresini ve gerekli ek süreyi içeren bir e-posta gönderir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Boş bir mantıksal uygulama oluşturma.
> * Mantık uygulamanızın zamanlamasını belirten bir Yineleme tetikleyicisi ekleyin.
> * Bir rotaiçin seyahat süresini alan bing haritaları eylemi ekleyin.
> * Bir değişken oluşturan, seyahat süresini saniyeden dakikaya dönüştüren ve değişkenle sonuçlanan depolayan bir eylem ekleyin.
> * Seyahat süresini belirtilen bir sınırla karşılaştıran bir koşul ekleme.
> * Seyahat süresi sınırı aşıyorsa size e-posta gönderen bir eylem ekleyin.

İşlemi tamamladığınızda, mantıksal uygulamanız bu yüksek düzeyli iş akışı gibi görünür:

![Üst düzey mantık uygulaması iş akışına genel bakış](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-overview.png)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, başlamadan önce [ücretsiz bir Azure hesabına kaydolun.](https://azure.microsoft.com/free/)

* Office 365 Outlook, Outlook.com veya Gmail gibi Logic Apps tarafından desteklenen bir e-posta sağlayıcısından gelen e-posta hesabı. Diğer sağlayıcılar için [buradaki bağlayıcı listesini inceleyin](https://docs.microsoft.com/connectors/). Bu hızlı başlangıç bir Office 365 Outlook hesabı kullanır. Farklı bir e-posta hesabı kullanıyorsanız, genel adımlar aynı kalır, ancak uI'niz biraz farklı olabilir.

* Bir rotaya ilişkin seyahat süresini almak için, Bing Haritalar API’sinin erişim anahtarı gerekir. Bu anahtarı almak için [Bing Haritalar anahtarını alma](https://docs.microsoft.com/bingmaps/getting-started/bing-maps-dev-center-help/getting-a-bing-maps-key) adımlarını izleyin.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure hesabınızın kimlik bilgileriyle [Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-your-logic-app"></a>Mantıksal uygulamanızı oluşturma

1. Ana Azure menüsünden **kaynak** > **Tümleştirme** > **Mantığı Uygulaması**oluştur'u seçin.

   ![Mantık uygulaması kaynağınızı oluşturun](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-new-logic-app-resource.png)

1. **Mantıksal uygulama oluştur** bölümünde, gösterildiği ve açıklandığı gibi mantıksal uygulamanızla ilgili bu bilgileri sağlayın. İşiniz bittiğinde **Oluştur**’u seçin.

   ![Mantık uygulamanız hakkında bilgi sağlayın](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-logic-app-settings.png)

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | **Adı** | LA-TravelTime | Mantık uygulamanızın adı, yalnızca harfleri, sayıları, tireleri`-`( ),`_`alt çizer`(` `)`( ),`.`parantez ( , ), ve dönemleri ( ) içerebilen. Bu örnekte "LA-TravelTime" kullanılıyor. |
   | **Abonelik** | <*sizin-Azure abonelik-adı*> | Azure abonelik adınız |
   | **Kaynak grubu** | LA-TravelTime-RG | İlgili kaynakları düzenlemek için kullanılan [Azure kaynak grubunun](../azure-resource-manager/management/overview.md)adı. Bu örnekte "LA-TravelTime-RG" kullanışıktır. |
   | **Konum** | Batı ABD | TMantık uygulama bilgilerinizi depolayabilmek için bölge. Bu örnekte "Batı ABD" kullanılıyor. |
   | **Log Analytics** | Kapalı | Tanılama günlüğüne kaydetme ayarını **Kapalı** durumda bırakın. |
   ||||

1. Azure uygulamanızı dağıttıktan sonra, Azure araç çubuğunda, dağıtılan mantık uygulamanız için **Bildirimler** > **Git'i seçin.**

   ![Yeni mantık uygulaması kaynağınıza gidin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/go-to-new-logic-app-resource.png)

   Veya, arama kutusuna adını yazarak mantık uygulamanızı bulabilir ve seçebilirsiniz.

   Logic Apps Designer, giriş videosu ve yaygın olarak kullanılan tetikleyiciler ve mantıksal uygulama desenleri içeren bir sayfayı açar ve gösterir. **Şablonlar** altında **Boş Mantıksal Uygulama**'yı seçin.

   ![Boş mantık uygulaması şablonu seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-logic-app-template.png)

Ardından, belirli bir zamanlamaya göre ateşleyen Yineleme [tetikleyicisini](../logic-apps/logic-apps-overview.md#logic-app-concepts)ekleyin. Her mantıksal uygulama, belirli bir olay gerçekleştiğinde veya yeni veriler belirli bir koşulu karşıladığında tetiklenen bir tetikleyiciyle başlamalıdır. Daha fazla bilgi için bkz. [İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-the-recurrence-trigger"></a>Yineleme tetikleyicisini ekleme

1. Logic App Designer'da, arama kutusuna filtreniz olarak "yineleme" girin. **Tetikleyiciler** listesinden **Yineleme** tetikleyicisini seçin.

   !["Yineleme" tetikleyicisi ekleme](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-schedule-recurrence-trigger.png)

1. **Yineleme** şeklinde, **elipsleri** (**...**) düğmesini seçin ve sonra **Yeniden Adlandır'ı**seçin. Tetikleyiciyi şu açıklama ile yeniden adlandırın: `Check travel time every weekday morning`

   ![Yineleme tetikleyici açıklamasını yeniden adlandırın](./media/tutorial-build-scheduled-recurring-logic-app-workflow/rename-recurrence-schedule-trigger.png)

1. Tetikleyicinin içinde, bu özellikleri değiştirin.

   ![Nüks tetikleyicisinin aralığını ve sıklığını değiştirme](./media/tutorial-build-scheduled-recurring-logic-app-workflow/change-interval-frequency.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Interval** | Evet | 1 | Denetimler arasında beklenecek aralık sayısı |
   | **Frequency** | Evet | Hafta | Yinelenme için kullanılacak zaman birimi |
   |||||

1. **Aralık** ve **Sıklık**altında, **yeni parametre** ekle listesini açın ve tetikleyiciye eklemek için bu özellikleri seçin.

   * **Şu günlerde**
   * **Şu saatlerde**
   * **Şu dakikalarda**

   ![Yineleme tetikleyicisi için özellikler ekleme](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-trigger-properties.png)

1. Şimdi burada gösterildiği ve açıklandığı gibi ek özellikleri için değerleri ayarlayın.

   ![Zamanlama ve yinelenme ayrıntılarını sağlayın](./media/tutorial-build-scheduled-recurring-logic-app-workflow/recurrence-trigger-property-values.png)

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | **Şu günlerde** | Pazartesi,Salı,Çarşamba,Perşembe,Cuma | Yalnızca **Sıklık** "Hafta" olarak ayarlandığında kullanılabilir |
   | **Şu saatlerde** | 7,8,9 | Yalnızca **Sıklık** "Hafta" veya "Gün" olarak ayarlandığında kullanılabilir. Bu yinelenmenin çalıştırılacağı günün saatlerini seçin. Bu örnek, 7, 8 ve 9 saat işaretlerinde çalıştırılır. |
   | **Şu dakikalarda** | 0,15,30,45 | Yalnızca **Sıklık** "Hafta" veya "Gün" olarak ayarlandığında kullanılabilir. Bu yinelenmenin çalıştırılacağı günün dakikalarını seçin. Bu örnek, sıfır saat işaretinden başlayarak her 15 dakikada bir çalıştırılır. |
   ||||

   Bu tetikleyici hafta içi her gün 7:00’da başlayıp 9:45’e kadar her 15 dakikada bir tetiklenir. **Önizleme** kutusu, yinelenme zamanlamasını gösterir. Daha fazla bilgi için bkz. [Görevleri ve iş akışlarını zamanlama](../connectors/connectors-native-recurrence.md) ve [İş akışı eylemleri ve tetikleyicileri](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger).

1. Tetikleyicinin ayrıntılarını şimdilik gizlemek için şeklin başlık çubuğunun içine tıklayın.

   ![Ayrıntıları gizlemek için şekli daraltın](./media/tutorial-build-scheduled-recurring-logic-app-workflow/collapse-trigger-shape.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

Şimdi mantıksal uygulamanız çalışıyor ancak yinelenme dışında bir işlem gerçekleştirmiyordur. Şimdi, tetikleyici etkinleştirildiğinde gerçekleştirilecek bir eylem ekleyin.

## <a name="get-the-travel-time-for-a-route"></a>Bir rota için seyahat süresi alma

Şimdi bir tetikleyiciniz olduğuna göre artık iki yer arasındaki seyahat süresini alan bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) ekleyebilirsiniz. Logic Apps, bu bilgileri kolayca alabilmeniz için Bing Haritalar API’si için bir bağlayıcı sağlar. Bu göreve başlamadan önce, bu öğreticinin önkoşullarında açıklandığı şekilde Bing Haritalar API anahtarına sahip olduğunuzdan emin olun.

1. Mantık Uygulama Tasarımcısı'nda, tetikleyiciniz altında **Yeni adımı**seçin.

1. Bir **eylem seçin**altında, **Standart'ı**seçin. Arama kutusuna, filtreniz olarak "bing haritaları" girin ve rota eylemini **al'ı** seçin.

   !["Rota al" eylemini seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-get-route-action.png)

1. Bing Haritalar bağlantınız yoksa bir bağlantı oluşturmanız istenir. Bu bağlantı ayrıntılarını sağlayın ve **Oluştur'u**seçin.

   ![Bing Haritalar API'sine bağlantı oluşturma](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-maps-connection.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Bağlantı Adı** | Evet | BingMapsConnection | Bağlantınıza bir ad verin. Bu örnekte "BingMapsConnection" kullanmıştır. |
   | **API Anahtarı** | Evet | <*sizin-Bing-Haritalar-anahtar*> | Daha önce aldığınız Bing Haritalar anahtarını girin. Bing Haritalar anahtarınız yoksa [nasıl anahtar alacağınızı](https://msdn.microsoft.com/library/ff428642.aspx) öğrenin. |
   |||||

1. Eylemi şu açıklama ile yeniden adlandırın: `Get route and travel time with traffic`

1. Eylemin içinde, **yeni parametre listesi ekle'yi**açın ve eyleme eklemek için bu özellikleri seçin.

   * **Optimize**
   * **Mesafe birimi**
   * **Seyahat modu**

   !["Rota al" eylemine özellikler ekleme](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-bing-maps-action-properties.png) 

1. Şimdi burada gösterildiği ve açıklandığı gibi eylem özellikleri için değerleri ayarlayın.

   !["Rota yı al" eylemi için ayrıntılar sağlayın](./media/tutorial-build-scheduled-recurring-logic-app-workflow/get-route-action-settings.png) 

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Güzergah noktası 1** | Evet | <*başlangıç konumu*> | Rotanızın başlangıç noktası |
   | **Güzergah noktası 2** | Evet | <*son konum*> | Rotanızın hedefi |
   | **Optimize** | Hayır | timeWithTraffic | Rotanızı iyileştirmeye yönelik bir parametre; örneğin, mesafe, mevcut trafik ile seyahat süresi vb. "TimeWithTraffic" parametresini seçin. |
   | **Mesafe birimi** | Hayır | <*sizin tercihiniz*> | Rotanız için mesafe birimi. Bu örnek, birim olarak "Mile" kullanır. |
   | **Seyahat modu** | Hayır | Sürüş | Rotanız için seyahat modu. "Sürüş" modunu seçin. |
   ||||

   Bu parametreler hakkında daha fazla bilgi için bkz. [Rota hesaplama](https://docs.microsoft.com/bingmaps/rest-services/routes/calculate-a-route).

1. Mantıksal uygulamanızı kaydedin.

Ardından, geçerli seyahat süresini saniyeler olarak değil, dakikalar olarak dönüştürebilmeniz ve depolayabilmeniz için bir değişken oluşturun. Böylece dönüştürmeyi yinelemekten kaçınabilir ve sonraki adımlarda değeri daha kolayca kullanabilirsiniz. 

## <a name="create-a-variable-to-store-travel-time"></a>Seyahat süresini depolamak için bir değişken oluşturma

Bazen, iş akışınızdaki veriler üzerindeki işlemleri çalıştırmak ve daha sonraki eylemlerde sonuçları kullanmak isteyebilirsiniz. Kolayca bu sonuçları yeniden kullanabilmek veya bu sonuçlara başvurabilmek amacıyla sonuçları kaydetmek için, sonuçları işledikten sonra depolamak üzere değişkenler oluşturabilirsiniz. Yalnızca mantıksal uygulamanızın en üst düzeyinde değişkenler oluşturabilirsiniz.

Varsayılan olarak, önceki **Get rota** eylemi, geçerli seyahat süresini Seyahat **Süresi Trafiği** özelliğinden saniyeler içinde trafikle döndürür. Bunun yerine bu değeri dönüştürüp depolayarak daha sonra yeniden dönüştürme olmadan değerin yeniden kullanımını kolaylaştırırsınız.

1. Rota **yı al** eyleminin altında **Yeni adım'ı**seçin.

1. Bir **eylem seçin,** **Dahili'yi**seçin. Arama kutusuna "değişkenler" girin ve **Değişken** eylemi Başlatma'yı seçin.

   !["Değişkeni başlatma" eylemini seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-initialize-variable-action.png)

1. Bu eylemi şu açıklama ile yeniden adlandırın: `Create variable to store travel time`

1. Burada açıklandığı gibi değişkeniniz için ayrıntıları sağlayın:

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Adı** | Evet | travelTime | Değişkeninizin adı. Bu örnekte "travelTime" kullanılıyor. |
   | **Tür** | Evet | Tamsayı | Değişkeninizin veri türü |
   | **Değer** | Hayır| Geçerli seyahat süresini saniyelerden dakikalara dönüştüren bir ifade (bu tablonun altındaki adımlara bakın). | Değişkeninizin ilk değeri |
   ||||

   1. **Değer** özelliği için ifade oluşturmak için, dinamik içerik listesinin görünmesi için kutunun içini tıklatın. Gerekirse liste görüntüleninceye kadar tarayıcınızı genişletin. Dinamik içerik listesinde **İfade'yi**seçin.

      !["Değişkeni başlatma" eylemi için bilgi sağlama](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings.png)

      Bazı edit kutularının içine tıkladığınızda, dinamik içerik listesi görüntülenir. Bu liste, iş akışınızda giriş olarak kullanabileceğiniz önceki eylemlere ait tüm özellikleri gösterir. Dinamik içerik listesinde işlemleri yürütmek için işlevleri seçebileceğiniz bir ifade düzenleyicisi vardır. Bu ifade düzenleyicisi yalnızca dinamik içerik listesinde görüntülenir.

   1. İfade düzenleyicisinde şu ifadeyi girin: `div(,60)`

      ![Şu ifadeyi girin: "div(,60)"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-2.png)

   1. İmlecinizi, sol parantez (**(**) ile virgül (**,**) arasındaki ifadeye getirin. 
   **Dinamik içeriği**seçin.

      ![Konum imleci, "Dinamik içerik" seçeneğini belirleyin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-3.png)

   1. Dinamik içerik listesinden **Seyahat Süresi Trafik** seçeneğini belirleyin.

      !["Seyahat Süresi Trafiği" özelliğini seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-4.png)

   1. Özellik değeri ifadenin içinde çözüldükten sonra **Tamam'ı**seçin.

      ![İfadeyi oluşturmayı bitirmek için "Tamam" seçeneğini belirleyin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-5.png)

      **Değer** özelliği şimdi burada gösterildiği gibi görünür:

      !["Değer" özelliği çözümlenmiş ifadeyle birlikte görünür](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-6.png)

1. Mantıksal uygulamanızı kaydedin.

Ardından, geçerli seyahat süresinin belirli bir sınırdan büyük olup olmadığını denetleyen bir koşul ekleyin.

## <a name="compare-the-travel-time-with-limit"></a>Seyahat süresini limitle karşılaştırın

1. Önceki eylemaltında, **Yeni adım'ı**seçin.

1. Bir **eylem seçin,** **Dahili'yi**seçin. Arama kutusuna filtreniz olarak "koşul" girin. Eylemler listesinden **Durum** eylemini seçin.

   !["Koşul" eylemini seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-condition-action.png)

1. Koşulu şu açıklama ile yeniden adlandırın: `If travel time exceeds limit`

1. **TravelTime** özellik değerinin açıklandığı ve burada gösterildiği gibi belirtilen sınırı aşıp aşmadığını kontrol eden bir koşul oluşturun:

   1. Bu durumda, koşulun sol tarafındaki bir değer kutusunu **seç'in** içini tıklatın.

   1. **Değişkenler**altında görünen dinamik içerik listesinden **travelTime** özelliğini seçin.

      ![Koşulun sol tarafını oluşturun](./media/tutorial-build-scheduled-recurring-logic-app-workflow/build-condition-left-side.png)

   1. Orta karşılaştırma kutusunda, **işleçten daha büyük olduğunu** seçin.

   1. Koşulun sağ tarafındaki bir değer kutusu **seç'e** şu sınırı girin:`15`

      İşi bittiğinde, durum aşağıdaki örneğe benzer:

      ![Seyahat süresini kontrol etmek için bitmiş koşul](./media/tutorial-build-scheduled-recurring-logic-app-workflow/build-condition-check-travel-time.png)

1. Mantıksal uygulamanızı kaydedin.

Ardından, seyahat süresi sınırınızı aştığında çalışacak eylemi ekleyin.

## <a name="send-email-when-limit-exceeded"></a>Sınır aşıldığında e-posta gönder

Şimdi seyahat süresi sınırınızı aştığında size e-posta gönderen bir eylem ekleyin. Bu e-posta, geçerli seyahat süresini ve belirtilen rotada seyahat etmek için gerekli ek süreyi içerir.

1. If **true** dalında eylem **ekle'yi**seçin.

1. Bir **eylem seçin**altında, **Standart'ı**seçin. Arama kutusuna "e-posta gönder" girin. Liste birçok sonuç döndürür, bu nedenle önce istediğiniz e-posta bağlayıcısını seçin, örneğin:

   ![İstediğiniz e-posta bağlayıcısını seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-action-send-email.png)

   * Azure iş veya okul hesapları için **Office 365 Outlook** girişini seçin.
   * Kişisel Microsoft hesapları için **Outlook.com' yi**seçin.

1. Bağlayıcının eylemleri göründüğünde, örneğin kullanmak istediğiniz "e-posta eylemi gönder"i seçin:

   !["E-posta gönder" eylemini seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-send-email-action.png)

1. Önceden bir bağlantınız yoksa e-posta hesabınızda oturum açmanız istenir.

   Logic Apps, e-posta hesabınıza bir bağlantı oluşturur.

1. Eylemi şu açıklama ile yeniden adlandırın: `Send email with travel time`

1. **Alıcı** kutusuna alıcının e-posta adresini girin. Test amacıyla e-posta adresinizi kullanın.

1. **Konu** kutusunda e-postanın konusunu belirtin ve **travelTime** değişkenini dahil edin.

   1. Sonunda boşluk olacak şekilde `Current travel time (minutes):` metnini girin. 

   1. Dinamik içerik listesinde, **Değişkenler**altında, **daha fazla sını gör'ün**

      !["travelTime" değişkenini bulma](./media/tutorial-build-scheduled-recurring-logic-app-workflow/find-travelTime-variable.png)

   1. **TravelTime** **Değişkenler**altında göründükten sonra, **travelTime'ı**seçin.

      ![Konu metnini ve seyahat süresini döndüren ifadeyi girin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-travelTime-variable.png)

1. **Gövde** kutusunda e-posta gövdesi için içeriği belirtin.

   1. Sonunda boşluk olacak şekilde `Add extra travel time (minutes):` metnini girin.

   1. Dinamik içerik listesinde **İfade'yi**seçin.

      ![E-posta gövdesi için ifade oluşturma](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings.png)

   1. İfade düzenleyicisinde, sınırınızı aşan dakika sayısını hesaplayabilmeniz için şu ifadeyi girin: ```sub(,15)```

      ![Ek seyahat süresi dakikasını hesaplamak için ifade girin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-2.png)

   1. İmlecinizi, sol parantez (**(**) ile virgül (**,**) arasındaki ifadeye getirin. **Dinamik içeriği**seçin.

      ![Ek seyahat süresi dakikasını hesaplamak için ifade oluşturmaya devam edin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-3.png)

   1. **Değişkenler** bölümünde **travelTime** seçeneğini belirleyin.

      ![İfadede kullanmak için "travelTime" özelliğini seçin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-4.png)

   1. Özellik ifade nin içinde çözüldükten sonra **Tamam'ı**seçin.

      !["Gövde" özelliği çözüldükten sonra "Tamam" seçeneğini belirleyin](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-5.png)

      **Vücut** özelliği şimdi burada gösterildiği gibi görünür:

      ![İfadede çözülmüş "Gövde" özelliği](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-6.png)

1. Mantıksal uygulamanızı kaydedin.

Ardından mantıksal uygulamanızı test edin; mantıksal uygulamanız şu örnek gibi görünür:

![Bitmiş örnek mantık uygulaması iş akışı](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-finished.png)

## <a name="run-your-logic-app"></a>Mantıksal uygulamanızı çalıştırın

Mantık uygulamanızı el ile başlatmak için, tasarımcı araç çubuğunda **Çalıştır'ı**seçin.

* Geçerli seyahat süresi limitinizin altında kalırsa, mantık uygulamanız başka bir şey yapmaz ve yeniden kontrol etmeden önce veya bir sonraki aralığı bekler. 

* Geçerli seyahat süresi sınırınızı aşarsa, geçerli seyahat süresini ve limitinizin üzerindeki dakika sayısını içeren bir e-posta alırsınız. Mantıksal uygulamanızın gönderdiği örnek bir e-posta şöyledir:

![Seyahat süresini gösteren e-posta gönderen örnek](./media/tutorial-build-scheduled-recurring-logic-app-workflow/received-example-email-notification.png)

E-posta gelmezse istenmeyen e-posta klasörüne bakın. E-postanızın istenmeyen posta filtresi bu tür postaları yeniden yönlendirebilir. Mantıksal uygulamanızın düzgün bir şekilde çalışıp çalışmadığından emin değilseniz bkz. [Mantıksal uygulama sorunlarını giderme](../logic-apps/logic-apps-diagnosing-failures.md).

Tebrikler, şimdi zamanlama tabanlı yinelenen bir mantıksal uygulama oluşturup çalıştırdınız. 

**Yineleme** tetikleyicisini kullanan diğer mantık uygulamaları oluşturmak için, bir mantık uygulaması oluşturduktan sonra kullanılabilen bu şablonlara göz atın:

* Size gönderilen günlük anımsatıcıları alın.
* Eski Azure bloblarını silin.
* Azure Depolama kuyruğuna bir ileti gönderin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Örnek mantık uygulamasına artık ihtiyacınız olmadığında, mantık uygulamanızı ve ilgili kaynakları içeren kaynak grubunu silin. 

1. Ana Azure menüsünde **Kaynak grupları**’na gidin ve mantıksal uygulamanızın kaynak grubunu seçin.

1. Kaynak grubu menüsünde, Kaynak grubunu **genel bakış** > **silme'yi**seçin. 

   !["Genel Bakış" > "Kaynak grubunu sil"](./media/tutorial-build-scheduled-recurring-logic-app-workflow/delete-resource-group.png)

1. Kaynak grup adını onay olarak girin ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, belirtilen bir zamanlama (hafta içi sabahları) temelinde trafiği denetleyen ve seyahat süresi belirtilen bir sınırı aştığında eylem uygulayan (e-posta gönderen) bir mantıksal uygulama oluşturdunuz. Şimdi, Azure hizmetlerini, Microsoft hizmetlerini ve diğer SaaS uygulamalarını tümleştirerek posta listesi isteklerini onaya gönderen bir mantıksal uygulamanın nasıl oluşturulacağını öğreneceksiniz.

> [!div class="nextstepaction"]
> [Posta listesi isteklerini yönetme](../logic-apps/tutorial-process-mailing-list-subscriptions-workflow.md)
