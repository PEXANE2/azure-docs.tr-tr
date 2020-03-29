---
title: Mantıksal uygulamaları sahte verilerle test etme
description: Üretim ortamlarını etkilemeden sahte verilerle mantık uygulamalarını test etmek için statik sonuçlar ayarlama
services: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 05/13/2019
ms.openlocfilehash: b71aae91f4a065b70537a300aa0bd7016edfd4b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74790269"
---
# <a name="test-logic-apps-with-mock-data-by-setting-up-static-results"></a>Statik sonuçlar ayarlayarak sahte verilerle mantık uygulamalarını test edin

Mantıksal uygulamalarınızı sınarken, çeşitli nedenlerle uygulamaları, hizmetleri ve sistemleri gerçekten aramaya veya erişime hazır olmayabilirsiniz. Genellikle bu senaryolarda, farklı durum yolları çalıştırmak, hataları zorlamak, belirli ileti yanıt gövdeleri sağlamak, hatta bazı adımları atlamayı denemek zorunda kalabilirsiniz. Mantık uygulamanızdaki bir eylem için statik sonuçlar ayarlayarak, bu eylemden çıktı verileriyle alay edebilirsiniz. Bir eylemde statik sonuçları etkinleştirmek eylemi çalıştırmaz, ancak bunun yerine sahte verileri döndürür.

Örneğin, Outlook 365 posta gönderimi için statik sonuçlar ayarlarsanız, Logic Apps altyapısı Outlook'u arayıp e-posta göndermek yerine statik sonuç olarak belirttiğiniz sahte verileri döndürür.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Statik sonuçlar ayarlamak istediğiniz mantık uygulaması

<a name="set-up-static-results"></a>

## <a name="set-up-static-results"></a>Statik sonuçları ayarlama

1. Azure [portalında](https://portal.azure.com)henüz yapmadıysanız, mantık uygulamanızı Logic Apps Designer'da açın.

1. Statik sonuçlar ayarlamak istediğiniz eylemde aşağıdaki adımları izleyin: 

   1. Eylemin sağ üst köşesinde, elipsleri (*...*) düğmesini seçin ve **Statik sonucu**seçin , örneğin:

      !["Statik sonuç" > "Statik Sonucu Etkinleştir" seçeneğini belirleyin](./media/test-logic-apps-mock-data-static-results/select-static-result.png)

   1. **Statik Sonucu Etkinleştir'i**seçin. Gerekli (*) özellikler için, eylemin yanıtı için döndürmek istediğiniz sahte çıktı değerlerini belirtin.

      Örneğin, HTTP eylemi için gerekli özellikler şunlardır:

      | Özellik | Açıklama |
      |----------|-------------|
      | **Durum** | İade etmek için eylemin durumu |
      | **Durum Kodu** | Döndürülecek özel durum kodu |
      | **Üst Bilgiler** | Döndürülecek üstbilgi içeriği |
      |||

      !["Statik Sonucu Etkinleştir" seçeneğini belirleyin](./media/test-logic-apps-mock-data-static-results/enable-static-result.png)

      JavaScript Nesne Gösterimi (JSON) biçiminde sahte veri girmek **için JSON Moduna Geç** 'i seçin (!["JSON Moduna Geç"](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button.png)seçeneğini belirleyin.

   1. İsteğe bağlı özellikler **için, isteğe bağlı alanları seç** listesini açın ve alay etmek istediğiniz özellikleri seçin.

      ![İsteğe bağlı özellikleri seçin](./media/test-logic-apps-mock-data-static-results/optional-properties.png)

1. Kaydetmeye hazır **olduğunuzda, Bitti'yi**seçin.

   Eylemin sağ üst köşesinde, başlık çubuğu artık statik sonuçları etkinleştirdiğinizi![gösteren bir](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)test beher simgesi (statik sonuçlar için simge) gösterir.

   ![Etkin statik sonuçları gösteren simge](./media/test-logic-apps-mock-data-static-results/static-results-enabled.png)

   Sahte verileri kullanan önceki çalıştırmaları bulmak için, bu konunun ilerleyen saatlerinde [statik sonuçlar kullanan çalıştıran bul'a](#find-runs-mock-data) bakın.

<a name="reuse-sample-outputs"></a>

## <a name="reuse-previous-outputs"></a>Önceki çıktıları yeniden kullanma

Mantık uygulamanızın sahte çıktılar olarak yeniden kullanabileceğiniz çıktılarla birlikte önceki bir çalışması varsa, bu çalıştırmadan çıkan çıktıları kopyalayıp yapıştırabilirsiniz.

1. Azure [portalında](https://portal.azure.com)henüz yapmadıysanız, mantık uygulamanızı Logic Apps Designer'da açın.

1. Mantık uygulamanızın ana menüsünde **Genel Bakış'ı**seçin.

1. Geçmiş **Çalıştır** bölümünde, istediğiniz mantık uygulamasını seçin.

1. Mantık uygulamanızın iş akışında, istediğiniz çıktılara sahip eylemi bulun ve genişletin.

1. Ham **çıktıları göster** bağlantısını seçin.

1. JavaScript Nesne Gösterimi (JSON) nesnesinin tamamını veya kullanmak istediğiniz belirli alt bölümü (örneğin, çıktılar bölümünü, hatta yalnızca üstbilgi bölümünü kopyalayın.

1. **Statik sonuçları** ayarla'daki eyleminiz için [Set up static results](#set-up-static-results)Statik sonuç kutusunu açmak için adımları izleyin.

1. Statik **sonuç** kutusu açıldıktan sonra aşağıdakilerden birini seçin:

   * Tam bir JSON nesnesini yapıştırmak **için JSON Moduna Geç** 'i seçin (!["JSON Moduna Geç"](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button.png)seçeneğini belirleyin:

     ![Tam nesne için "JSON Moduna Geç" seçeneğini belirleyin](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button-complete.png)

   * Bu bölümün etiketinin yanındaki sadece bir JSON bölümünü yapıştırmak için, örneğin, bu bölüm için **JSON Moduna Geç'i** seçin:

     ![Çıktılar için "JSON Moduna Geç" seçeneğini belirleyin](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button-outputs.png)

1. JSON editöründe, daha önce kopyalanmış JSON'unuzu yapıştırın.

   ![JSON modu](./media/test-logic-apps-mock-data-static-results/json-editing-mode.png)

1. İşiniz bittiğinde **Bitti**'yi seçin. Veya tasarımcıya dönmek için **Düzenleyici Modunu Değiştir** 'i seçin (!["Düzenleyici Modunu Değiştir"i](./media/test-logic-apps-mock-data-static-results/switch-editor-mode-button.png)seçin.

<a name="find-runs-mock-data"></a>

## <a name="find-runs-that-use-static-results"></a>Statik sonuçlar kullanan çalıştırmaları bulma

Mantık uygulamanızın çalışma geçmişi, eylemlerin statik sonuçları kullandığı çalıştırmaları tanımlar. Bu çalıştırmaları bulmak için aşağıdaki adımları izleyin:

1. Mantık uygulamanızın ana menüsünde **Genel Bakış'ı**seçin. 

1. Sağ bölmede, **Çalışır geçmişi**altında, **Statik Sonuçlar** sütununa bulabilirsiniz. 

   Sonuçlarla eylemleri içeren herhangi bir çalıştırma, Statik **Sonuçlar** sütununa **Etkin**olarak ayarlanmış ,örneğin:

   ![Çalışma geçmişi - statik sonuçlar sütunu](./media/test-logic-apps-mock-data-static-results/run-history.png)

1. Statik sonuçları kullanan eylemleri görüntülemek için Statik **Sonuçlar** sütununun **Etkin**olarak ayarlandığı çalıştır'ı seçin.

   Statik sonuçları kullanan eylemler, örneğin test![kabını](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)(statik sonuçlar için simge) simgesini gösterir:

   ![Geçmiş çalıştırma - statik sonuçlar kullanan eylemler](./media/test-logic-apps-mock-data-static-results/static-results-enabled-run-details.png)

## <a name="disable-static-results"></a>Statik sonuçları devre dışı

Statik sonuçları kapatmak, son kurulumunuzdaki değerleri atmaz. Bu nedenle, statik sonuçları bir dahaki sefere açtığınızda, önceki değerlerinizi kullanmaya devam edebilirsiniz.

1. Statik çıktıları devre dışı kakmak istediğiniz eylemi bulun. Eylemin sağ üst köşesinde, test beher simgesini seçin![( Statik](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)sonuçlar için simge).

   ![Statik sonuçları devre dışı](./media/test-logic-apps-mock-data-static-results/disable-static-results.png)

1. Statik Sonucu > **Devre Dışı Devre Tonu'nun 'u**seçin. **Disable Static Result**

   ![Statik sonuçları devre dışı](./media/test-logic-apps-mock-data-static-results/disable-static-results-button.png)

## <a name="reference"></a>Başvuru

Temel iş akışı tanımlarınızda bu ayar hakkında daha fazla bilgi için statik [sonuçlar - İş Akışı Tanımı Dili için Şema başvurusu](../logic-apps/logic-apps-workflow-definition-language.md#static-results) ve [runtimeConfiguration.staticResult - Runtime yapılandırma ayarlarına](../logic-apps/logic-apps-workflow-actions-triggers.md#runtime-configuration-settings) bakın

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Mantık Uygulamaları](../logic-apps/logic-apps-overview.md) hakkında daha fazla bilgi edinin