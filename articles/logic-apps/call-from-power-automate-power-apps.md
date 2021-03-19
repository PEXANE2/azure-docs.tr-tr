---
title: Power Automate ve Power Apps’ten mantıksal uygulama çağırma
description: Mantıksal uygulamaları bağlayıcı olarak dışa aktararak Microsoft Power otomatikleştirmede Logic Apps 'i çağırın.
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, logicappspm
ms.topic: conceptual
ms.date: 10/05/2020
ms.openlocfilehash: b402dab4c6e94a7634e11f0330b5379315e43abf
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91762507"
---
# <a name="call-logic-apps-from-power-automate-and-power-apps"></a>Power Automate ve Power Apps’ten mantıksal uygulama çağırma

Mantıksal uygulamalarınızı Microsoft Power otomatikleştirmek ve Microsoft Power Apps 'ten çağırmak için, mantıksal uygulamalarınızı bağlayıcılar olarak dışarı aktarabilirsiniz. Bir mantıksal uygulamayı bir Power otomatikleştir veya Power Apps ortamında özel bağlayıcı olarak kullanıma suntığınızda, daha sonra mantıksal uygulamanızı burada bulunan akışlardan çağırabilirsiniz.

Akışınızı Power otomatikleştirebileceğiniz veya gücünden Logic Apps geçirmek istiyorsanız, bkz. [akış akışlarını Power otomatikleştirmede dışa aktarma ve Azure Logic Apps dağıtma](export-from-microsoft-flow-logic-app-template.md).

> [!NOTE]
> Azure Logic Apps ' de tüm güç otomatikleştirme bağlayıcıları kullanılamaz. Yalnızca Azure Logic Apps içindeki eşit bağlayıcılara sahip güç otomatikleştirme akışlarını geçirebilirsiniz. Örneğin, düğme tetikleyicisi, onay Bağlayıcısı ve bildirim Bağlayıcısı, Power otomatikleştirmek için özeldir. Şu anda, mantıksal uygulama şablonları olarak dışa aktarma ve dağıtım için Power otomatikleştirmede OpenAPI tabanlı akışlar desteklenmez.
>
> * Hangi Power otomatikleştirmek bağlayıcılarının Logic Apps eşdeğerleri olmadığını öğrenmek için bkz. [Power otomatikleştiren bağlayıcılar](/connectors/connector-reference/connector-reference-powerautomate-connectors).
>
> * Hangi Logic Apps bağlayıcılarının güç otomatikleştirme eşdeğerleri olmadığını öğrenmek için bkz. [Logic Apps bağlayıcılar](/connectors/connector-reference/connector-reference-powerautomate-connectors).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Power otomatikleştirir veya Power Apps lisansı.

* Dışarı aktarmak için bir istek tetikleyicisi içeren bir mantıksal uygulama.

* Mantıksal uygulamanızı çağırmak istediğiniz güç otomatikleştirme veya güç uygulamalarında bir akış.

## <a name="export-your-logic-app-as-a-custom-connector"></a>Mantıksal uygulamanızı özel bağlayıcı olarak dışarı aktarma

Mantıksal uygulamanızı Power otomatikleştirin veya Power Apps 'ten çağırabilmeniz için önce mantıksal uygulamanızı özel bir bağlayıcı olarak dışarı aktarmanız gerekir.

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure portal arama kutusuna girin `Logic Apps` . Sonuçlarda, **Hizmetler**' in altında **Logic Apps**' yi seçin.

1. Dışarı aktarmak istediğiniz mantıksal uygulamayı seçin.

1. Mantıksal uygulamanızın menüsünde, **dışarı aktar**' ı seçin.

    :::image type="content" source="./media/call-logic-apps-from-power-automate-power-apps/export-logic-app.png" alt-text="Azure portal ' deki Logic App 'in sayfasının ekran görüntüsü, ' Export ' düğmesi seçili olan menüyü gösterir.":::

1. **Dışarı aktarma** bölmesinde, **ad** için, mantıksal uygulamanıza özel bağlayıcı için bir ad girin. **Ortam** listesinden, mantıksal uygulamanızı çağırmak Istediğiniz güç otomatikleştir veya Power Apps ortamını seçin. İşiniz bittiğinde **Tamam**’ı seçin.

    :::image type="content" source="./media/call-logic-apps-from-power-automate-power-apps/export-logic-app2.png" alt-text="Özel bağlayıcı adı ve ortamı için gerekli alanları gösteren mantıksal uygulamanın dışa aktarma bölmesinin ekran görüntüsü.":::

1. Mantıksal uygulamanızın başarıyla verildiğini onaylamak için bildirimler bölmesini kontrol edin.

### <a name="exporting-errors"></a>Hataları dışarı aktarma

Mantıksal uygulamanızı özel bağlayıcı olarak ve önerilen çözümlerini dışarı aktardığınızda oluşabilecek hatalar şunlardır:

* **Ortamlar alınamadı. Hesabınızın güç otomatikleştirme için yapılandırıldığından emin olun, sonra yeniden deneyin.**: Azure hesabınızda bir güç otomatikleştirme planına sahip olup olmadığını denetleyin.

* **Geçerli mantıksal uygulama verilemiyor. Dışarı aktarmak için, istek tetikleyicisi olan bir mantıksal uygulama seçin.**: mantıksal uygulamanızın bir [istek tetikleyicisi](./logic-apps-workflow-actions-triggers.md#request-trigger)ile başladığından emin olun.

## <a name="connect-to-your-logic-app-from-power-automate"></a>Power otomatikleştirmede mantıksal uygulamanıza bağlanma

Güç otomatikleştirme akışınız ile verdiğiniz mantıksal uygulamaya bağlanmak için:

1. [Power Automate](https://flow.microsoft.com)’te oturum açın.

1. **Power otomatikleştir** giriş sayfası menüsünde **Akışlarım**' ı seçin.

1. **Akışlar** sayfasında, mantıksal uygulamanıza bağlamak istediğiniz akışı seçin.

1. Flow sayfanızın menüsünden **Düzenle**' yi seçin.

1. Akış Düzenleyicisi 'nde **yeni&#43; adımı**' nı seçin.

1. **Eylem seçin** altında, ara kutusuna mantıksal uygulama bağlayıcınızın adını girin. İsteğe bağlı olarak, ortamınızda yalnızca özel bağlayıcıları göstermek için, **özel** sekmesini seçerek sonuçları filtreleyin.

    :::image type="content" source="./media/call-logic-apps-from-power-automate-power-apps/power-automate-custom-connector-action.png" alt-text="Özel bağlayıcı ve kullanılabilir eylemler için yeni bir adım eklendiğini gösteren Power otomatikleştiren akış Düzenleyicisi 'nin ekran görüntüsü.":::

1. Mantıksal uygulama bağlayıcınızla almak istediğiniz eylemi seçin. 

1. İşlemin mantıksal uygulama bağlayıcısına geçirme bilgilerini sağlayın.

1. Değişikliklerinizi kaydetmek için, Power otomatikleştir Düzenleyicisi menüsünde **Kaydet**' i seçin.

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Logic Apps hizmetinde, verdiğiniz mantıksal uygulamayı bulun.

1. Mantıksal uygulamanızın güç otomatikleştirme akışınızda beklediğiniz şekilde çalıştığından emin olun.

## <a name="delete-logic-app-connector-from-power-automate"></a>Mantıksal uygulama bağlayıcısını Power otomatikleştirmede silme

1. [Power Automate](https://flow.microsoft.com)’te oturum açın.

1. **Power otomatikleştir** giriş sayfasında, menüdeki **veri** &gt; **özel bağlayıcıları** ' nı seçin.

1. Listeden özel bağlayıcınızı bulun ve üç nokta (**...**) düğmesini Sil ' i seçin &gt; .

    :::image type="content" source="./media/call-logic-apps-from-power-automate-power-apps/delete-custom-connector.png" alt-text="Mantıksal uygulamanın özel bağlayıcı yönetim düğmelerini gösteren Power otomatikleştirmek ' Özel Bağlayıcılar ' sayfasının ekran görüntüsü.":::

1. Silmeyi onaylamak için **Tamam**' ı seçin.

## <a name="connect-to-your-logic-app-from-power-apps"></a>Power Apps 'ten mantıksal uygulamanıza bağlanma

Power Apps akışınızla birlikte verdiğiniz mantıksal uygulamaya bağlanmak için:

1. [Power Apps](https://powerapps.microsoft.com/)'te oturum açın.

1. **Power Apps** giriş sayfasında, menüdeki **akışlar** ' ı seçin.

1. **Akışlar** sayfasında, mantıksal uygulamanıza bağlamak istediğiniz akışı seçin.

1. Akışınızın sayfasında, akışın menüsünde **Düzenle** ' yi seçin.

1. Akış Düzenleyicisi 'nde **&#43; yeni adım** düğmesini seçin.

1. Yeni adımda **bir eylem seçin** altında, arama kutusuna mantıksal uygulama bağlayıcınızın adını girin. İsteğe bağlı olarak, ortamınızda yalnızca özel bağlayıcılar görmek için sonuçları **özel** sekmeye filtreleyin.

    :::image type="content" source="./media/call-logic-apps-from-power-automate-power-apps/power-apps-custom-connector-action.png" alt-text="Özel bağlayıcı ve kullanılabilir eylemler için yeni bir adım eklendiğini gösteren Power Apps akış Düzenleyicisi 'nin ekran görüntüsü.":::

1. Bağlayıcıyla almak istediğiniz eylemi seçin. 

1. Mantıksal uygulama bağlayıcısına eyleminizi hangi bilgilerin geçirdikli bir şekilde yapılandırın.

1. Değişikliklerinizi kaydetmek için Power Apps Düzenleyicisi menüsünde **Kaydet** ' i seçin. 

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Logic Apps hizmetinde, verdiğiniz mantıksal uygulamayı bulun.

1. Mantıksal uygulamanızın, Power Apps akışınıza yönelik olarak çalıştığını doğrulayın.

## <a name="delete-logic-app-connector-from-power-apps"></a>Logic App bağlayıcısını Power Apps 'ten silme

1. [Power Apps](https://powerapps.microsoft.com)'te oturum açın.

1. **Power Apps** giriş sayfasında, menüdeki **veri** &gt; **özel bağlayıcıları** ' nı seçin.

1. Listeden özel bağlayıcınızı bulun ve üç nokta (**...**) düğmesini Sil ' i seçin &gt; .

    :::image type="content" source="./media/call-logic-apps-from-power-automate-power-apps/delete-custom-connector.png" alt-text="Mantıksal uygulamanın özel bağlayıcı yönetim düğmelerini gösteren, Power Apps ' Özel Bağlayıcılar ' sayfasının ekran görüntüsü.":::

1. Silmeyi onaylamak için **Tamam**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps Için bağlayıcılar](../connectors/apis-list.md) hakkında daha fazla bilgi edinin
* [Azure Logic Apps](../logic-apps/logic-apps-overview.md) hakkında daha fazla bilgi edinin
