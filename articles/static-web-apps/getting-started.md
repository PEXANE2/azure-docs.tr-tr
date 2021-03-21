---
title: 'Hızlı başlangıç: Azure statik Web Apps ilk statik sitenizi oluşturma'
description: Statik bir siteyi Azure statik Web Apps dağıtmayı öğrenin.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: quickstart
ms.date: 08/13/2020
ms.author: cshoe
ms.openlocfilehash: 2d7a2dcbbd0b6e9a10ca8af93798bfddbee94ee3
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102182664"
---
# <a name="quickstart-building-your-first-static-site-with-azure-static-web-apps"></a>Hızlı başlangıç: Azure statik Web Apps ilk statik sitenizi oluşturma

Azure statik Web Apps bir GitHub deposundan uygulama oluşturarak bir Web sitesini üretim ortamına yayınlar. Bu hızlı başlangıçta, Azure statik Web uygulamalarına Visual Studio Code uzantısını kullanarak bir Web uygulaması dağıtırsınız.

Azure aboneliğiniz yoksa [ücretsiz bir deneme hesabı oluşturun](https://azure.microsoft.com/free).

## <a name="prerequisites"></a>Önkoşullar

- [GitHub](https://github.com) hesabı
- [Azure](https://portal.azure.com) hesabı
- [Visual Studio Code](https://code.visualstudio.com)
- [Visual Studio Code için Azure Static Web Apps uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestaticwebapps)
- [Git'i yükleyin](https://www.git-scm.com/downloads)

[!INCLUDE [create repository from template](../../includes/static-web-apps-get-started-create-repo.md)]

[!INCLUDE [clone the repository](../../includes/static-web-apps-get-started-clone-repo.md)]

Daha sonra, Visual Studio Code açın ve **dosya > klasöre** giderek, yeni bir düzenleyicide makinenize Klonladığınız depoyu açın.

## <a name="create-a-static-web-app"></a>Statik web uygulaması oluşturma

1. Visual Studio Code'un etkinlik çubuğundan Azure logosunu seçerek Azure uzantıları penceresini açın.

    :::image type="content" source="media/getting-started/extension-azure-logo.png" alt-text="Azure Logosu":::

    > [!NOTE]
    > Azure ve GitHub'da oturum açmanız gerekir. Daha önce Visual Studio Code'da Azure ve GitHub oturumu açmadıysanız oluşturma işlemi sırasında uzantı ikisinde de oturum açmanızı isteyecektir.

1. Farenizi _Static Web Apps_ etiketinin üzerine getirip **artı işaretini** seçin.

    :::image type="content" source="media/getting-started/extension-create-button.png" alt-text="Uygulama adı":::

1. Pageç, düzenleyicinin en üstünde açılır ve uygulamanızı ad vermenizi ister.

    **my-first-static-web-app** yazıp **Enter** tuşuna basın.

    :::image type="content" source="media/getting-started/extension-create-app.png" alt-text="Statik web uygulaması oluşturma":::

1. **main** dalını seçip **Enter** tuşuna basın.

    :::image type="content" source="media/getting-started/extension-branch.png" alt-text="Dal adı":::

1. **/** Uygulama kodu için konum olarak öğesini seçin ve **ENTER** tuşuna basın.

    :::image type="content" source="media/getting-started/extension-app-location.png" alt-text="Uygulama kodu konumu":::

1. Uzantı, uygulamanızdaki API konumunu arar. Bu makalede API kullanılmaz.

    **Şimdilik atla**'yı seçip **Enter** tuşuna basın.

    :::image type="content" source="media/getting-started/extension-api-location.png" alt-text="API konumu":::

1. Uygulamanızda üretim ortamı dosyalarının yer aldığı konumu seçin.

    # <a name="no-framework"></a>[Çerçeve yok](#tab/vanilla-javascript)

    Kutuyu temizleyin ve **ENTER** tuşuna basın.

    :::image type="content" source="media/getting-started/extension-artifact-no-framework.png" alt-text="Uygulama dosyaları yolu":::

    # <a name="angular"></a>[Angular](#tab/angular)

    **Dist/angular-temel** yazın ve **ENTER** tuşuna basın.

    :::image type="content" source="media/getting-started/extension-artifact-angular.png" alt-text="Angular uygulamasındaki dosyaların yolu":::

    # <a name="react"></a>[React](#tab/react)

    **build** yazın ve **Enter** tuşuna basın.

    :::image type="content" source="media/getting-started/extension-artifact-react.png" alt-text="React uygulamasındaki dosyaların yolu":::

    # <a name="vue"></a>[Vue](#tab/vue)

    **dist** yazın ve **Enter** tuşuna basın.

    :::image type="content" source="media/getting-started/extension-artifact-vue.png" alt-text="Vue uygulamasındaki dosyaların yolu":::

    ---

1. Size en yakın konumu seçin ve **Enter** tuşuna basın.

    :::image type="content" source="media/getting-started/extension-location.png" alt-text="Kaynak konumu":::

1. Uygulama oluşturulduktan sonra Visual Studio Code'da bir onay bildirimi gösterilir.

    :::image type="content" source="media/getting-started/extension-confirmation.png" alt-text="Oluşturma onayı":::

1. Visual Studio Code Gezgin penceresinde, abonelik adınızın bulunduğu düğüme gidin ve genişletin. Dağıtımın tamamlanması birkaç dakika sürebilir. Ardından, statik Web Apps bölümüne dönüp uygulamanızın adını seçin ve ardından, Azure portal uygulamayı görüntülemek için ilk-statik-Web-uygulama ' ya sağ tıklayın ve portalda aç ' ı seçin.

    :::image type="content" source="media/getting-started/extension-open-in-portal.png" alt-text="Portalda aç":::

[!INCLUDE [view website](../../includes/static-web-apps-get-started-view-website.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekecekseniz, Azure statik Web Apps örneğini uzantı aracılığıyla silebilirsiniz.

Visual Studio Code Gezgin penceresinde _statik Web Apps_ bölümüne dönüp, **ilk-statik-Web-uygulaması** ' na sağ tıklayın ve **Sil**' i seçin.

:::image type="content" source="media/getting-started/extension-delete.png" alt-text="Uygulamayı Sil":::

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [API ekleme](add-api.md)
