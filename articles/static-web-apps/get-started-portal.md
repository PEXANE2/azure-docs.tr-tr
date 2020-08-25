---
title: 'Hızlı başlangıç: Azure portal kullanarak Azure statik Web Apps ile ilk statik Web uygulamanızı oluşturma'
description: Azure portal ile bir Azure statik Web Apps örneği oluşturmayı öğrenin.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: quickstart
ms.date: 08/13/2020
ms.author: cshoe
ms.openlocfilehash: e0b78c5e053c5668fbebd8ebaac91a90aa2b364f
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88752930"
---
# <a name="quickstart-building-your-first-static-web-app-in-the-azure-portal"></a>Hızlı başlangıç: Azure portal ilk statik Web uygulamanızı oluşturma

Azure statik Web Apps bir GitHub deposundan uygulama oluşturarak bir Web sitesini üretim ortamına yayınlar. Bu hızlı başlangıçta, portalı kullanarak bir Web uygulamasını Azure statik Web uygulamalarına dağıtırsınız.

Azure aboneliğiniz yoksa [ücretsiz bir deneme hesabı oluşturun](https://azure.microsoft.com/free).

## <a name="prerequisites"></a>Ön koşullar

- [GitHub](https://github.com) hesabı
- [Azure](https://portal.azure.com) hesabı

[!INCLUDE [create repository from template](../../includes/static-web-apps-get-started-create-repo.md)]

## <a name="create-a-static-web-app"></a>Statik Web uygulaması oluşturma

Artık depo oluşturulduktan sonra, Azure portal statik bir Web uygulaması oluşturabilirsiniz.

1. [Azure Portal](https://portal.azure.com) gidin
1. **Kaynak oluştur** ' u seçin
1. **Statik Web Uygulamaları** için arama yapın
1. **Statik Web Apps seçin (Önizleme)**
1. **Oluştur**’u seçin

_Temel bilgiler_ sekmesinde, yeni uygulamanızı yapılandırıp bir GitHub deposuna bağlayarak başlayın.

:::image type="content" source="media/getting-started-portal/basics-tab.png" alt-text="Temel bilgiler sekmesi":::

1. _Azure aboneliğinizi_ seçin
1. Yeni bir _kaynak grubu_ seçin veya oluşturun
1. Uygulamanın **-First-static-Web-App**' i adlandırın.
      1. Geçerli karakterler şunlardır: `a-z` (büyük/küçük harf duyarsız), `0-9` ve `-`.
1. Size en yakın _bölgeyi_ seçin
1. **Ücretsiz** _SKU 'yu_ seçin
1. GitHub ile **oturum açma** seçeneğini belirleyin ve GitHub ile kimlik doğrulaması yapın

GitHub ile oturum açtıktan sonra depo bilgilerini girin.

:::image type="content" source="media/getting-started-portal/repository-details.png" alt-text="Depo ayrıntıları":::

1. Tercih ettiğiniz _kuruluşu_ seçin
1. _Depo_ açılır listesinden **My-First-Web-static-App** ' i seçin
1. _Dal_ açılan listesinden **ana öğe** seçin
1. Derleme yapılandırmasını düzenlemek için **Sonraki: build >** düğmesini seçin

:::image type="content" source="media/getting-started-portal/next-build-button.png" alt-text="Sonraki derleme düğmesi":::

> [!NOTE]
> Herhangi bir depo görmüyorsanız, GitHub 'da Azure statik Web Apps yetkilendirmeniz gerekebilir. GitHub deponuza gidin ve **yetkili OAuth uygulamaları > ayarlar > uygulamalar**' a gidip **Azure statik Web Apps**' yı seçin ve ardından **ver**' i seçin. Kuruluş depolarında, izinleri vermek için kuruluşun sahibi olmanız gerekir.

1. _Derleme_ sekmesinde, tercih ettiğiniz ön uç çerçevesine özgü yapılandırma ayrıntılarını ekleyin.

    # <a name="no-framework"></a>[Çerçeve yok](#tab/vanilla-javascript)

    - _Uygulama konumu_ kutusunda varsayılan değeri temizle
    - _API konum_ kutusundan varsayılan değeri temizle
    - _Uygulama yapıtı konum_ kutusundan varsayılan değeri temizle

    # <a name="angular"></a>[Angular](#tab/angular)

    - _Uygulama konumu_ kutusunda varsayılan değeri temizle
    - _API konum_ kutusundan varsayılan değeri temizle
    - _Uygulama yapıt konumu_ kutusuna **Dist/angular-temel** yazın

    # <a name="react"></a>[React](#tab/react)

    - _Uygulama konumu_ kutusunun varsayılan değerini temizle
    - _API konum_ kutusundan varsayılan değeri temizle
    - _Uygulama yapıtı konum_ kutusunda **Oluştur** yazın

    # <a name="vue"></a>[Vue](#tab/vue)

    - _Uygulama konumu_ kutusunun varsayılan değerini temizle
    - _API konum_ kutusundan varsayılan değeri temizle
    - _Uygulama yapıtı konum_ kutusuna **Dist** yazın

    ---

1. **Gözden geçir + oluştur**’u seçin.

    :::image type="content" source="media/getting-started-portal/review-create.png" alt-text="İnceleme oluştur düğmesi":::

    > [!NOTE]
    > Uygulamayı oluşturduktan sonra bu değerleri değiştirmek için [iş akışı dosyasını](github-actions-workflow.md) düzenleyebilirsiniz.

1. **Oluştur**’u seçin.

    :::image type="content" source="media/getting-started-portal/create-button.png" alt-text="Oluştur düğmesi":::

1. **Kaynağa git**’i seçin.

    :::image type="content" source="media/getting-started-portal/resource-button.png" alt-text="Kaynağa git düğmesi":::

[!INCLUDE [view website](../../includes/static-web-apps-get-started-view-website.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam edemeyecekinizde, Azure statik Web Apps örneğini aşağıdaki adımlarla silebilirsiniz:

1. [Azure portalı](https://portal.azure.com) açın
1. En üst arama çubuğundan **My-First-Web-static-App** araması yapın
1. Uygulama adı üzerinde seçin
1. **Sil** düğmesini seçin
1. Silme eylemini onaylamak için **Evet** ' i seçin

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [API ekleme](add-api.md)
