---
title: 'Hızlı başlangıç: Azure statik Web Apps ilk statik Web uygulamanızı oluşturma'
description: Tercih ettiğiniz ön uç çerçevesiyle Azure statik Web Apps örneği oluşturmayı öğrenin.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: bbc06b657525880f22bd5fb38e902f906d438c9c
ms.sourcegitcommit: 37afde27ac137ab2e675b2b0492559287822fded
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88565919"
---
# <a name="quickstart-building-your-first-static-web-app"></a>Hızlı başlangıç: ilk statik Web uygulamanızı oluşturma

Azure Statik Web Uygulamaları, uygulamaları bir GitHub deposundan derleyerek web sitelerini bir üretim ortamında yayımlar. Bu hızlı başlangıçta, bir GitHub deposundan tercih ettiğiniz ön uç çerçevesini kullanarak bir Web uygulaması oluşturacaksınız.

Azure aboneliğiniz yoksa [ücretsiz bir deneme hesabı oluşturun](https://azure.microsoft.com/free).

## <a name="prerequisites"></a>Ön koşullar

- [GitHub](https://github.com) hesabı
- [Azure](https://portal.azure.com) hesabı

## <a name="create-a-repository"></a>Depo oluşturma

Bu makalede yeni bir depo oluşturmanızı kolaylaştırmak için GitHub şablon depoları kullanılmaktadır. Şablonlar özelliği, farklı ön uç çerçevelerle oluşturulmuş başlangıç uygulamalarıdır.

# <a name="angular"></a>[Angular](#tab/angular)

- GitHub 'da oturum açtığınızdan emin olun ve yeni bir depo oluşturmak için aşağıdaki konuma gidin
  - https://github.com/staticwebdev/angular-basic/generate
- Deponuzu **-First-static-Web-App** olarak adlandırın

# <a name="react"></a>[React](#tab/react)

- GitHub 'da oturum açtığınızdan emin olun ve yeni bir depo oluşturmak için aşağıdaki konuma gidin
  - https://github.com/staticwebdev/react-basic/generate
- Deponuzu **-First-static-Web-App** olarak adlandırın

# <a name="vue"></a>[Vue](#tab/vue)

- GitHub 'da oturum açtığınızdan emin olun ve yeni bir depo oluşturmak için aşağıdaki konuma gidin
  - https://github.com/staticwebdev/vue-basic/generate
- Deponuzu **-First-static-Web-App** olarak adlandırın

# <a name="no-framework"></a>[Çerçeve yok](#tab/vanilla-javascript)

- GitHub 'da oturum açtığınızdan emin olun ve yeni bir depo oluşturmak için aşağıdaki konuma gidin
  - https://github.com/staticwebdev/vanilla-basic/generate
- Deponuzu **-First-static-Web-App** olarak adlandırın

> [!NOTE]
> Azure statik Web Apps, bir Web uygulaması oluşturmak için en az bir HTML dosyası gerektirir. Bu adımda oluşturduğunuz depo, tek bir _index.html_ dosyası içerir.

---

**Şablondan depo oluştur** düğmesine tıklayın.

:::image type="content" source="media/getting-started/create-template.png" alt-text="Şablondan depo oluştur":::

## <a name="create-a-static-web-app"></a>Statik Web uygulaması oluşturma

Artık depo oluşturulduktan sonra, Azure portal statik bir Web uygulaması oluşturabilirsiniz.

- [Azure Portal](https://portal.azure.com) gidin
- **Kaynak oluştur** seçeneğine tıklayın
- **Statik Web Uygulamaları** için arama yapın
- **Statik Web Uygulamaları (Önizleme)** seçeneğine tıklayın
- **Oluştur** seçeneğine tıklayın

### <a name="basics"></a>Temel bilgiler

Yeni uygulamanızı yapılandırıp bir GitHub deposuna bağlayarak başlayın.

:::image type="content" source="media/getting-started/basics-tab.png" alt-text="Temel bilgiler sekmesi":::

- _Azure aboneliğinizi_ seçin
- Yeni bir _kaynak grubu_ seçin veya oluşturun
- Uygulamanın **-First-static-Web-App**' i adlandırın.
  - Geçerli karakterler şunlardır: `a-z` (büyük/küçük harf duyarsız), `0-9` ve `-`.
- Size en yakın _bölgeyi_ seçin
- **Ücretsiz** _SKU 'yu_ seçin
- **GitHub ile Oturum Aç** düğmesine tıklayın ve GitHub ile kimliğinizi doğrulayın

GitHub ile oturum açtıktan sonra depo bilgilerini girin.

:::image type="content" source="media/getting-started/repository-details.png" alt-text="Depo ayrıntıları":::

- Tercih ettiğiniz _kuruluşu_ seçin
- _Depo_ açılır listesinden **My-First-Web-static-App** ' i seçin
- _Dal_ açılan listesinden **ana öğe** seçin
- Derleme yapılandırmasını düzenlemek için **İleri: Derleme >** düğmesine tıklayın

:::image type="content" source="media/getting-started/next-build-button.png" alt-text="Sonraki derleme düğmesi":::

> [!NOTE]
>  Herhangi bir depo görmüyorsanız, GitHub 'da Azure statik Web Apps yetkilendirmeniz gerekebilir. [GitHub giriş sayfasına](https://github.com) gidin ve açılan menüyü açmak için hesap yansımanıza tıklayın. **Ayarlar**' a ve ardından **uygulamalar > yetkili OAuth uygulamaları Azure statik Web Apps >** ve son olarak **izin ver**' i seçin. Kuruluş depolarında, izinleri vermek için kuruluşun sahibi olmanız gerekir.

### <a name="build"></a>Oluşturma

Ardından, tercih ettiğiniz ön uç çerçevesine özgü yapılandırma ayrıntılarını ekleyin.

# <a name="angular"></a>[Angular](#tab/angular)

- **/** _Uygulama konumu_ kutusuna girin
- _API konum_ kutusundan varsayılan değeri temizle
- _Uygulama yapıt konumu_ kutusuna **dağ/angular-temel** girin

# <a name="react"></a>[React](#tab/react)

- **/** _Uygulama konumu_ kutusuna girin
- _API konum_ kutusundan varsayılan değeri temizle
- _Uygulama yapıtı konum_ kutusunda **derlemeyi** girin

# <a name="vue"></a>[Vue](#tab/vue)

- **/** _Uygulama konumu_ kutusuna girin
- _API konum_ kutusundan varsayılan değeri temizle
- _Uygulama yapıtı konum_ kutusuna **Dist** girin

# <a name="no-framework"></a>[Çerçeve yok](#tab/vanilla-javascript)

- **/** _Uygulama konumu_ kutusuna girin
- _API konum_ kutusundan varsayılan değeri temizle
- _Uygulama yapıtı konum_ kutusundan varsayılan değeri temizle

---

**Gözden geçir ve oluştur** düğmesine tıklayın.

:::image type="content" source="media/getting-started/review-create.png" alt-text="İnceleme oluştur düğmesi":::

Uygulamayı oluşturduktan sonra bu değerleri değiştirmek için [iş akışı dosyasını](github-actions-workflow.md)düzenleyebilirsiniz.

### <a name="review--create"></a>Gözden geçirme ve oluşturma

İstek doğruladıktan sonra, uygulamayı oluşturmaya devam edebilirsiniz.

**Oluştur** düğmesine tıklayın

:::image type="content" source="media/getting-started/create-button.png" alt-text="Oluştur düğmesi":::

Kaynak oluşturulduktan sonra **Kaynağa Git** düğmesine tıklayın

:::image type="content" source="media/getting-started/resource-button.png" alt-text="Kaynağa git düğmesi":::

## <a name="view-the-website"></a>Web sitesini görüntüleme

Statik uygulama dağıtmanın iki yönü vardır. Bunların ilki uygulamanızı oluşturan temel alınan Azure kaynaklarının sağlanmasıdır. İkincisi de, uygulamanızı derleyip yayımlayan GitHub Actions iş akışıdır.

Yeni statik sitenize gidebilmeniz için önce dağıtım derlemesinin çalıştırılması gerekir.

Statik Web Apps genel bakış penceresinde web uygulamanızla etkileşime başlamanıza yardımcı olacak bir dizi bağlantı görüntülenir.

:::image type="content" source="media/getting-started/overview-window.png" alt-text="Genel bakış penceresi":::

1. "GitHub eylemlerinizin durumunu denetlemek için buraya tıklayın" ifadesini içeren başlığa tıkladığınızda sizi deponuza karşı çalışan GitHub eylemlerine götürür. Dağıtım işinin tamamlandığını doğruladıktan sonra, oluşturulan URL aracılığıyla Web sitenize gidebilirsiniz.

2. GitHub eylemleri iş akışı tamamlandıktan sonra, Web sitesini yeni sekmede açmak için _URL_ bağlantısına tıklayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam edemeyecekinizde, Azure statik Web Apps örneğini aşağıdaki adımlarla silebilirsiniz:

1. [Azure portalı](https://portal.azure.com) açın
1. En üst arama çubuğundan **My-First-Web-static-App** araması yapın
1. Uygulama adına tıklayın
1. **Sil** düğmesine tıklayın
1. Silme eylemini onaylamak için **Evet** ' e tıklayın

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [API ekleme](add-api.md)
