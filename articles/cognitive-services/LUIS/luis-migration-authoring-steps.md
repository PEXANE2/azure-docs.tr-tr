---
title: Azure yazma kaynağına geçiş
titleSuffix: Azure Cognitive Services
description: Azure yazma kaynağına geçirin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: diberry
ms.openlocfilehash: f5197ae79670e4543c58224a33838706edae6218
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78194656"
---
# <a name="steps-to-migrate-to-the-azure-authoring-resource"></a>Azure yazma kaynağına geçiş adımları

Dil Anlama (LUIS) portalından, Azure yazma kaynağını kullanmak için sahip olduğunuz tüm uygulamaları geçirin.

## <a name="prerequisites"></a>Ön koşullar

* **İsteğe bağlı olarak,** her uygulamayı dışa aktararak veya dışa aktarma [API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40)kullanarak luis portalının uygulamalar listesindeki uygulamaları yedekleyin.
* **İsteğe bağlı olarak,** her uygulamanın ortak çalışan listesini kaydedin. Tüm ortak çalışanlara geçiş sürecinin bir parçası olarak bir e-posta gönderilebilir.
* **Gerekli**, bir [Azure aboneliği](https://azure.microsoft.com/free/)olması gerekir. Abonelik işleminin bir bölümü fatura bilgilerini gerektirir. Ancak, LUIS kullandığınızda Ücretsiz (F0) fiyatlandırma katmanlarını kullanabilirsiniz. Kullanımınız arttıkça, sonunda ücretli bir katmana ihtiyacınız olduğunu görebilirsiniz.

Azure aboneliğiniz yoksa [kaydolun.](https://azure.microsoft.com/free/)

## <a name="access-the-migration-process"></a>Geçiş işlemine erişin

Haftalık olarak, uygulamalarınızı geçirmeniz istenir. Bu pencereyi geçiş yapmadan iptal edebilirsiniz. Bir sonraki zamanlanan dönemden önce geçiş yapmak istiyorsanız, geçiş işlemini LUIS portalının üst araç çubuğundaki **Azure** simgesinden başlatabilirsiniz.

> [!div class="mx-imgBorder"]
> ![Geçiş simgesi](./media/migrate-authoring-key/migration-button.png)

## <a name="app-owner-begins-the-migration-process"></a>Uygulama sahibi geçiş işlemini başlatın

Herhangi bir LUIS uygulamasının sahibiyseniz geçiş işlemi kullanılabilir.

1. [LUIS portalında](https://www.luis.ai) oturum açın ve kullanım koşullarını kabul edin.
1. Geçiş açılır penceresi geçişe devam etmenizi veya daha sonra geçiş yapmanızı sağlar. **Şimdi Geçir'i**seçin. Daha sonra geçiş yapmayı seçerseniz, Azure'daki yeni yazma anahtarına geçiş yapmak için 9 ayınız var.

    ![Geçiş işlemindeki ilk açılır pencere, şimdi geçir'i seçin.](./media/migrate-authoring-key/migrate-now.png)

1. İsteğe bağlı olarak, uygulamalarınızın ortak çalışanları varsa, onlara geçiş hakkında bildiren **bir e-posta göndermeniz** istenir. Bu isteğe bağlı bir adımdır.

    Hesabınızı Azure'a geçtikten sonra, uygulamalarınız artık ortak çalışanlar tarafından kullanılamaz.

    Her ortak çalışan ve uygulama için varsayılan e-posta uygulaması hafif biçimlendirilmiş bir e-postayla açılır. E-postayı göndermeden önce edinebilirsiniz.

    E-posta şablonu tam uygulama kimliği ve uygulama adını içerir.

    ```html
    Dear Sir/Madam,

    I will be migrating my LUIS account to Azure. Consequently, you will no longer have access to the following app:

    App Id: <app-ID-omitted>
    App name: Human Resources

    Thank you
    ```

1. Varolan bir yazarlık kaynağını kullanmayı veya yeni bir yazarlık kaynağı oluşturmayı seçerek bir LUIS yazma kaynağı oluşturmayı seçin.

    > [!div class="mx-imgBorder"]
    > ![Yazarlık kaynağı oluşturma](./media/migrate-authoring-key/choose-existing-authoring-resource.png)

1. Sonraki pencerede kaynak anahtar bilgilerinizi girin. Bilgileri girdikten sonra **kaynak oluştur'u**seçin. Bölge başına abonelik başına 10 ücretsiz yazma kaynağınız olabilir.

    ![Yazarlık kaynağı oluşturma](./media/migrate-authoring-key/choose-authoring-resource-form.png)

    **Yeni bir yazarlık kaynağı oluştururken**aşağıdaki bilgileri sağlayın:

    * **Kaynak adı** - seçtiğiniz özel bir ad, yazma ve tahmin bitiş noktası sorguları için URL'nin bir parçası olarak kullanılır.
    * **Kiracı** - Azure aboneliğinizin ilişkili olduğu kiracı.
    * **Abonelik adı** - kaynak için faturalandırılacak abonelik.
    * **Kaynak grubu** - seçtiğiniz veya oluşturduğunuz özel bir kaynak grubu adı. Kaynak grupları, Azure kaynaklarını erişim ve yönetim için gruplandırmanıza olanak tanır.
    * **Konum** - konum seçimi **kaynak grubu** seçimini temel alır.
    * **Fiyatlandırma katmanı** - fiyatlandırma katmanı saniye ve ay başına maksimum işlemi belirler.

1. Yazarlık kaynağınızı doğrulayın ve **şimdi geçirin.**

    ![Yazarlık kaynağı oluşturma](./media/migrate-authoring-key/choose-authoring-resource-and-migrate.png)

1. Yazarlık kaynağı oluşturulduğunda, başarı iletisi gösterilir. Açılan pencereyi kapatmak için **Kapat'ı** seçin.

    ![Yazarlık kaynağınız başarıyla oluşturuldu.](./media/migrate-authoring-key/migration-success.png)

    **Uygulamalarım** listesi, yeni yazma kaynağına geçirilen uygulamaları gösterir.

    LUIS portalında uygulamalarınızı düzenlemeye devam etmek için yazar kaynağın anahtarını bilmeniz gerekmez. Uygulamalarınızı programlı bir şekilde yeniden yapmayı planlıyorsanız, yazma anahtar değerlerine ihtiyacınız var. Bu değerler, LUIS portalındaki **> Azure kaynaklarını yönet** sayfasında görüntülenir ve kaynağın **Keys** sayfasındaki Azure portalında da kullanılabilir.

1. Uygulamalarınıza erişmeden önce, yazabileceğiniz uygulamaları görmek için abonelik ve LUIS yazma kaynağını seçin.

    ![Yazabileceğiniz uygulamaları görmek için abonelik ve LUIS yazma kaynağını seçin.](./media/migrate-authoring-key/app-list-by-subscription-and-resource.png)


## <a name="app-contributor-begins-the-migration-process"></a>Uygulama katılımcısı geçiş işlemini başlatın

Geçiş için uygulama sahibiyle aynı adımları izleyin. İşlem, tür `LUIS.Authoring`yeni bir yazma kaynağı oluşturur.

Başkaları tarafından sahip olunan geçiş uygulamalarına katkıda bulunan kişi olarak eklenebilir şekilde hesabınızı geçirmeniz gerekir.

## <a name="after-the-migration-process-add-contributors-to-your-authoring-resource"></a>Geçiş işleminden sonra, yazar kaynağınıza katkıda bulunanları ekleyin

[!INCLUDE [Manage contributors for the Azure authoring resource for language understanding](./includes/manage-contributors-authoring-resource.md)]

[Katkıda bulunanları nasıl ekleyeceğinizi](luis-how-to-collaborate.md)öğrenin.

## <a name="troubleshooting-errors-with-the-migration-process"></a>Geçiş işlemiyle ilgili hataları giderme

Geçiş işlemi `MissingSubscriptionRegistration` sırasında luis portalında kırmızı bildirim çubuğu içeren bir hata alırsanız, [Azure portalında](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) veya [Azure CLI'de](luis-how-to-azure-subscription.md#create-resources-in-azure-cli)bir Bilişsel Hizmet kaynağı oluşturun. [Bu hatanın nedenleri](../../azure-resource-manager/templates/error-register-resource-provider.md#cause)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar


* Yazma ve çalışma zamanı tuşları hakkındaki [kavramları](luis-concept-keys.md) gözden geçirme
* [Anahtarları nasıl ataysüreceğini](luis-how-to-azure-subscription.md) ve [katkıda bulunanları](luis-how-to-collaborate.md) eklemeyi gözden geçirin
