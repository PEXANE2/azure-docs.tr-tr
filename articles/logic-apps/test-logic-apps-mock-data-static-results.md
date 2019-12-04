---
title: Mantıksal uygulamaları sahte verilerle test etme
description: Üretim ortamlarını etkilemeden sahte verilerle mantıksal uygulamaları test etmek için statik sonuçlar ayarlayın
services: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 05/13/2019
ms.openlocfilehash: b71aae91f4a065b70537a300aa0bd7016edfd4b4
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74790269"
---
# <a name="test-logic-apps-with-mock-data-by-setting-up-static-results"></a>Statik sonuçlar ayarlayarak sahte verilerle mantıksal uygulamaları test edin

Logic Apps 'i sınarken, çeşitli nedenlerle uygulamalar, hizmetler ve sistemler için gerçekten çağrı yapma veya erişme izniniz olmayabilir. Genellikle bu senaryolarda, farklı koşul yolları çalıştırmanız, hatalara zorlamak, belirli ileti yanıt gövdelerini sağlamanız veya hatta bazı adımları atlamayı denemeniz gerekebilir. Mantıksal uygulamanızdaki bir eylem için statik sonuçlar ayarlayarak, çıkış verilerini bu eylemden yapılandırabilirsiniz. Bir eylem üzerinde statik sonuçların etkinleştirilmesi eylemi çalıştırmaz, ancak bunun yerine sahte verileri döndürür.

Örneğin, Outlook 365 posta gönder eylemi için statik sonuçlar ayarlarsanız, Logic Apps altyapısı, Outlook 'U çağırmak ve e-posta göndermek yerine yalnızca statik sonuçlar olarak belirttiğiniz sahte verileri döndürür.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgi

* Statik sonuçları ayarlamak istediğiniz mantıksal uygulama

<a name="set-up-static-results"></a>

## <a name="set-up-static-results"></a>Statik sonuçları ayarla

1. Henüz yapmadıysanız, [Azure portal](https://portal.azure.com)Logic Apps tasarımcısında mantıksal uygulamanızı açın.

1. Statik sonuçları ayarlamak istediğiniz eylemde, şu adımları izleyin: 

   1. Eylemin sağ üst köşesinde üç nokta ( *...* ) düğmesini seçin ve **statik sonuç**' i seçin, örneğin:

      !["Statik sonuç" > "statik sonucu etkinleştir" i seçin](./media/test-logic-apps-mock-data-static-results/select-static-result.png)

   1. **Statik sonucu etkinleştir**' i seçin. Gerekli (*) Özellikler için, eylemin yanıtı için döndürmek istediğiniz sahte çıkış değerlerini belirtin.

      Örneğin, HTTP eylemi için gereken özellikler şunlardır:

      | Özellik | Açıklama |
      |----------|-------------|
      | **Durum** | Döndürülecek eylemin durumu |
      | **Durum Kodu** | Döndürülecek belirli durum kodu |
      | **Üst Bilgiler** | Döndürülecek üst bilgi içeriği |
      |||

      !["Statik sonucu etkinleştir" i seçin](./media/test-logic-apps-mock-data-static-results/enable-static-result.png)

      JavaScript Nesne Gösterimi (JSON) biçiminde sahte verileri girmek için **JSON moduna geç** ' i (!["JSON moduna geç"](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button.png)) seçin.

   1. İsteğe bağlı özellikler için **isteğe bağlı alanları Seç** listesini açın ve sonra da, kullanmak istediğiniz özellikleri seçin.

      ![İsteğe bağlı özellikleri seçin](./media/test-logic-apps-mock-data-static-results/optional-properties.png)

1. Kaydetmeye hazırsanız **bitti**' yi seçin.

   Eylemin sağ üst köşesinde bulunan başlık çubuğu artık, statik sonuçları etkinleştirdiğini belirten bir test Beaker simgesi (statik sonuçlar için![simgesi](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)) gösterir.

   ![Etkin statik sonuçları gösteren simge](./media/test-logic-apps-mock-data-static-results/static-results-enabled.png)

   Sahte verileri kullanan önceki çalıştırmaları bulmak için, bu konunun ilerleyen kısımlarında [statik sonuçlar kullanan çalıştırmaları bulma](#find-runs-mock-data) bölümüne bakın.

<a name="reuse-sample-outputs"></a>

## <a name="reuse-previous-outputs"></a>Önceki çıktıları yeniden kullan

Mantıksal uygulamanızın çıkış içeren önceki bir çalıştırması varsa, sahte çıktılar olarak yeniden kullanabilirsiniz. çıkışları bu çalıştırdan kopyalayabilir ve yapıştırabilirsiniz.

1. Henüz yapmadıysanız, [Azure portal](https://portal.azure.com)Logic Apps tasarımcısında mantıksal uygulamanızı açın.

1. Mantıksal uygulamanızın ana menüsünde **genel bakış**' ı seçin.

1. **Çalıştırma geçmişi** bölümünde, istediğiniz mantıksal uygulama çalıştırmasını seçin.

1. Mantıksal uygulamanızın iş akışında istediğiniz çıkışları içeren eylemi bulun ve genişletin.

1. **Ham çıkışları göster** bağlantısını seçin.

1. Tam JavaScript Nesne Gösterimi (JSON) nesnesini ya da kullanmak istediğiniz belirli alt bölümü (örneğin, çıktılar bölümü ya da yalnızca üstbilgiler bölümü) kopyalayın.

1. [Statik sonuçları ayarla](#set-up-static-results)bölümünde eyleminiz için **statik sonuç** kutusunu açma adımlarını izleyin.

1. **Statik sonuç** kutusu açıldıktan sonra, iki adımdan birini seçin:

   * Bir JSON nesnesini tamamen yapıştırmak için **JSON moduna geç** ' i seçin (!["JSON moduna geç" ' i seçin](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button.png)):

     ![Tüm nesne için "JSON moduna geç" i seçin](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button-complete.png)

   * Yalnızca bir JSON bölümünü yapıştırmak için bu bölümün etiketinin yanındaki bu bölüm için **JSON moduna geç** ' i seçin, örneğin:

     ![Çıktılar için "JSON moduna geç" seçeneğini belirleyin](./media/test-logic-apps-mock-data-static-results/switch-to-json-mode-button-outputs.png)

1. JSON düzenleyicisinde, daha önce kopyalanmış JSON dosyanızı yapıştırın.

   ![JSON modu](./media/test-logic-apps-mock-data-static-results/json-editing-mode.png)

1. İşiniz bittiğinde **Bitti**'yi seçin. Ya da tasarımcıya dönmek için **Düzenleyici modunu değiştir** ' i (!["Düzenleyici modunu değiştir"](./media/test-logic-apps-mock-data-static-results/switch-editor-mode-button.png)) seçin.

<a name="find-runs-mock-data"></a>

## <a name="find-runs-that-use-static-results"></a>Statik sonuçlar kullanan çalıştırmaları bulun

Mantıksal uygulamanızın çalışma geçmişi, eylemlerin statik sonuçlar kullanacağı çalıştırmaları tanımlar. Bu çalışmaları bulmak için şu adımları izleyin:

1. Mantıksal uygulamanızın ana menüsünde **genel bakış**' ı seçin. 

1. Sağ bölmede, **çalışma geçmişi**altında, **statik sonuçlar** sütununu bulun. 

   Sonuçları olan eylemleri içeren herhangi bir çalışan, **statik sonuçlar** sütununun **etkin**olarak ayarlanmış olduğunu, örneğin:

   ![Çalışma geçmişi-statik sonuçlar sütunu](./media/test-logic-apps-mock-data-static-results/run-history.png)

1. Statik sonuçlar kullanan eylemleri görüntülemek için, **statik sonuçlar** sütununun **etkin**olarak ayarlandığı, istediğiniz çalıştırmayı seçin.

   Statik sonuçlar kullanan eylemler, test Beaker (statik sonuçlar için![simgesi](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)) simgesini gösterir, örneğin:

   ![Çalıştırma geçmişi-statik sonuçlar kullanan eylemler](./media/test-logic-apps-mock-data-static-results/static-results-enabled-run-details.png)

## <a name="disable-static-results"></a>Statik sonuçları devre dışı bırak

Statik sonuçları kapatmak, son kurulumdan değerleri oluşturmaz. Bu nedenle, bir sonraki sefer statik sonuçları açtığınızda önceki değerlerinizi kullanmaya devam edebilirsiniz.

1. Statik çıkışları devre dışı bırakmak istediğiniz eylemi bulun. Eylemin sağ üst köşesinde, test Beaker simgesini seçin (statik sonuçlar için![simgesini](./media/test-logic-apps-mock-data-static-results/static-results-test-beaker-icon.png)).

   ![Statik sonuçları devre dışı bırak](./media/test-logic-apps-mock-data-static-results/disable-static-results.png)

1. **Statik sonucu devre dışı bırak** > **bitti**' yi seçin.

   ![Statik sonuçları devre dışı bırak](./media/test-logic-apps-mock-data-static-results/disable-static-results-button.png)

## <a name="reference"></a>Başvuru

Temel alınan iş akışı tanımlarınızda Bu ayar hakkında daha fazla bilgi için bkz. [statik sonuçlar-Iş akışı tanımlama dili ve Runtimeconfiguration Için şema başvurusu](../logic-apps/logic-apps-workflow-definition-language.md#static-results) [. Staticresult-çalışma zamanı yapılandırma ayarları](../logic-apps/logic-apps-workflow-actions-triggers.md#runtime-configuration-settings)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps](../logic-apps/logic-apps-overview.md) hakkında daha fazla bilgi edinin