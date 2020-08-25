---
title: 'Hızlı başlangıç: Azure statik Web Apps ilk statik Web uygulamanızı oluşturma'
description: Azure statik Web Apps web sitesi oluşturmayı öğrenin.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: quickstart
ms.date: 08/13/2020
ms.author: cshoe
ms.openlocfilehash: db3836e6171d187539b8615efcb5ab782c368020
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88752455"
---
# <a name="quickstart-building-your-first-static-web-app"></a>Hızlı başlangıç: ilk statik Web uygulamanızı oluşturma

Azure statik Web Apps bir GitHub deposundan uygulama oluşturarak bir Web sitesini üretim ortamına yayınlar. Bu hızlı başlangıçta, Azure statik Web uygulamalarına Visual Studio Code uzantısını kullanarak bir Web uygulaması dağıtırsınız.

Azure aboneliğiniz yoksa [ücretsiz bir deneme hesabı oluşturun](https://azure.microsoft.com/free).

## <a name="prerequisites"></a>Ön koşullar

- [GitHub](https://github.com) hesabı
- [Azure](https://portal.azure.com) hesabı
- [Visual Studio Code](https://code.visualstudio.com)
- [Visual Studio Code için Azure statik Web Apps uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestaticwebapps)

[!INCLUDE [create repository from template](../../includes/static-web-apps-get-started-create-repo.md)]

[!INCLUDE [clone the repository](../../includes/static-web-apps-get-started-clone-repo.md)]

Daha sonra, Visual Studio Code açın ve **dosya > klasöre** giderek, yeni bir düzenleyicide makinenize Klonladığınız depoyu açın.

## <a name="create-a-static-web-app"></a>Statik Web uygulaması oluşturma

1. Visual Studio Code içinde, Azure uzantıları penceresini açmak için etkinlik çubuğundan Azure logosu ' nu seçin.

    :::image type="content" source="media/getting-started/extension-azure-logo.png" alt-text="Azure logosu":::

    > [!NOTE]
    > Azure ve GitHub oturum açma gereklidir. Azure 'da önceden oturum açmadıysanız ve Visual Studio Code 'den GitHub 'da, uzantı oluşturma işlemi sırasında her ikisinde de oturum açmanızı ister.

1. Farenizi _statik Web Apps_ etiketinin üzerine getirin ve **artı işaretini**seçin.

    :::image type="content" source="media/getting-started/extension-create-button.png" alt-text="Uygulama adı":::

1. Pageç, düzenleyicinin en üstünde açılır ve uygulamanızı ad vermenizi ister.

    **My-First-static-Web-App** yazın ve **ENTER**tuşuna basın.

    :::image type="content" source="media/getting-started/extension-create-app.png" alt-text="Statik Web uygulaması oluşturma":::

1. **Ana** dalı seçin ve **ENTER**tuşuna basın.

    :::image type="content" source="media/getting-started/extension-branch.png" alt-text="Dal adı":::

1. **/** Uygulama kodu için konum olarak öğesini seçin ve **ENTER**tuşuna basın.

    :::image type="content" source="media/getting-started/extension-app-location.png" alt-text="Uygulama kodu konumu":::

1. Uzantı, uygulamanızdaki API 'nin konumunu arıyor. Bu makale bir API uygulamaz.

    **Şimdilik atla** ' yı seçin ve **ENTER**tuşuna basın.

    :::image type="content" source="media/getting-started/extension-api-location.png" alt-text="API konumu":::

1. Uygulamanızdaki üretim için dosyaların oluşturulduğu konumu seçin.

    # <a name="no-framework"></a>[Çerçeve yok](#tab/vanilla-javascript)

    Kutuyu temizleyin ve **ENTER**tuşuna basın.

    :::image type="content" source="media/getting-started/extension-artifact-no-framework.png" alt-text="Uygulama dosyaları yolu":::

    # <a name="angular"></a>[Angular](#tab/angular)

    **Dist/angular-temel** yazın ve **ENTER**tuşuna basın.

    :::image type="content" source="media/getting-started/extension-artifact-angular.png" alt-text="Angular uygulama dosyaları yolu":::

    # <a name="react"></a>[React](#tab/react)

    **Oluştur** yazın ve **ENTER**tuşuna basın.

    :::image type="content" source="media/getting-started/extension-artifact-react.png" alt-text="Uygulama dosyaları yoluna tepki verme":::

    # <a name="vue"></a>[Vue](#tab/vue)

    **Dist** yazın ve **ENTER**tuşuna basın.

    :::image type="content" source="media/getting-started/extension-artifact-vue.png" alt-text="Vue uygulama dosyaları yolu":::

    ---

1. Size en yakın konumu seçin ve **ENTER**tuşuna basın.

    :::image type="content" source="media/getting-started/extension-location.png" alt-text="Kaynak konumu":::

1. Uygulama oluşturulduktan sonra Visual Studio Code bir onay bildirimi gösterilir.

    :::image type="content" source="media/getting-started/extension-confirmation.png" alt-text="Oluşturulma onayı":::

1. Visual Studio Code Gezgin penceresinde, _statik Web Apps_ bölümüne dönüp **üretime** sağ tıklayıp **portalda aç** ' ı seçerek Azure Portal uygulamayı görüntüleyin.

    :::image type="content" source="media/getting-started/extension-open-in-portal.png" alt-text="Portalı aç":::

[!INCLUDE [view website](../../includes/static-web-apps-get-started-view-website.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekecekseniz, Azure statik Web Apps örneğini uzantı aracılığıyla silebilirsiniz.

Visual Studio Code Gezgin penceresinde _statik Web Apps_ bölümüne dönüp, **ilk-statik-Web-uygulaması** ' na sağ tıklayın ve **Sil**' i seçin.

:::image type="content" source="media/getting-started/extension-delete.png" alt-text="Uygulamayı Sil":::

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [API ekleme](add-api.md)
