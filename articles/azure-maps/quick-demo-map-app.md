---
title: 'Hızlı başlangıç: Azure Maps ile etkileşimli harita arama'
description: Etkileşimli ve aranabilir haritalar oluşturmayı öğrenin. Azure haritalar hesabı oluşturma, birincil anahtar alma ve Web SDK 'sını kullanarak harita uygulamaları ayarlama bölümüne bakın.
author: anastasia-ms
ms.author: v-stharr
ms.date: 7/10/2020
ms.topic: quickstart
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: fa5cab35416f2629d39ec04803f90fae1a2e5f6d
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88031056"
---
# <a name="quickstart-create-an-interactive-search-map-with-azure-maps"></a>Hızlı başlangıç: Azure Maps ile etkileşimli arama haritası oluşturma

Bu makalede, kullanıcılara etkileşimli bir arama deneyimi sağlayan bir harita oluşturmak için Azure haritalar 'ın nasıl kullanılacağı gösterilmektedir. Bu temel adımlarda size kılavuzluk eder:

* Kendi Azure haritalar hesabınızı oluşturun.
* Demo Web uygulamasında kullanmak üzere birincil anahtarınızı alın.
* Demo harita uygulamasını indirip açın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* [Azure Portal](https://portal.azure.com) oturum açın.

<a id="createaccount"></a>

## <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma

Aşağıdaki adımlarla yeni bir Azure Maps hesabı oluşturun:

1. [Azure portalının](https://portal.azure.com) sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. Market 'te *Ara* kutusuna **Azure Maps**yazın.
3. *Sonuçlardan* **Azure haritaları**' nı seçin. Haritanın altında görüntülenen **Oluştur** düğmesine tıklayın.
4. **Haritalar Hesabı Oluştur** sayfasında aşağıdaki değerleri girin:
    * Bu hesap için kullanmak istediğiniz *Abonelik*.
    * Bu hesap için *Kaynak grubu* adı. Kaynak grubu için *Yeni oluştur* veya *Mevcut olanı kullan* seçeneğini belirleyebilirsiniz.
    * Yeni hesabınıza verilen *Ad*.
    * Bu hesabın *fiyatlandırma katmanı* .
    * *Lisans*’ı ve *Gizlilik Bildirimi*’ni okuyun ve onay kutusunu işaretleyerek koşulları kabul edin.
    * **Oluştur** düğmesine tıklayın.

    :::image type="content" source="./media/quick-demo-map-app/create-account.png" alt-text="Portalda Haritalar hesabı oluşturma":::

<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>Hesabınızın birincil anahtarını alma

Haritalar hesabınız başarıyla oluşturulduktan sonra, Haritalar API 'Lerini sorgulamanızı sağlayan birincil anahtarı alın.

1. Portalda Haritalar hesabınızı açın.
2. Ayarlar bölümünde **kimlik doğrulaması**' nı seçin.
3. **Birincil Anahtar**’ı panonuza kopyalayın. Bu öğreticinin ilerleyen kısmında kullanmak üzere bunu yerel olarak kaydedin.

>[!NOTE]
> Birincil anahtar yerine abonelik anahtarını kullanırsanız, haritanız düzgün şekilde işlenmez. Ayrıca, güvenlik nedeniyle, birincil ve ikincil anahtarlarınız arasında döndürme yapmanız önerilir. Anahtarları döndürmek için uygulamanızı ikincil anahtarı kullanacak şekilde güncelleştirin, dağıtın ve ardından birincil anahtarın yanındaki döngüye/Yenile düğmesine basarak yeni bir birincil anahtar oluşturun. Eski birincil anahtar devre dışı bırakılacak. Anahtar döndürme hakkında daha fazla bilgi için bkz. [anahtar döndürme ve denetleme ile Azure Key Vault ayarlama](https://docs.microsoft.com/azure/key-vault/secrets/key-rotation-log-monitoring)

:::image type="content" source="./media/quick-demo-map-app/get-key.png" alt-text="Azure portal birincil anahtar Azure Haritalar anahtarını al":::

## <a name="download-the-demo-application"></a>Demo uygulamayı indirme

1. [interactiveSearch.html](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/interactiveSearch.html)'ye gidin. Dosyanın içeriğini kopyalayın.
2. Bu dosyanın içeriğini **AzureMapDemo.html**olarak yerel olarak kaydedin. Bir metin düzenleyicisinde açın.
3. Dizeyi arayın `<Your Azure Maps Key>` . Önceki bölümdeki **birincil anahtar** değeriyle değiştirin.

## <a name="open-the-demo-application"></a>Demo uygulamasını açın

1. **AzureMapDemo.html** dosyasını istediğiniz bir tarayıcıda açın.
2. Los Angeles şehrinde gösterilen Haritayı gözlemleyin. Yakınlaştırma düzeyine bağlı olarak haritanın daha fazla veya daha az bilgiyle nasıl işlendiğini görmek için yakınlaştırma ve uzaklaştırma yapın.
3. Haritanın varsayılan merkezini değiştirin. **AzureMapDemo.html** dosyasında, **center** adlı değişkeni arayın. Bu değişkenin boylam ve enlem çiftini, yeni **[-74.0060, 40.7128]** değerleriyle değiştirin. Dosyayı kaydedin ve tarayıcınızı yenileyin.
4. Etkileşimli arama deneyimini deneyin. Demo Web uygulamasının sol üst köşesindeki arama kutusunda **Restoran**' ı arayın.
5. Farenizi arama kutusunun altında görüntülenen adresler ve konumlar listesinin üzerine taşıyın. Haritadaki karşılık gelen PIN 'in bu konumla ilgili bilgileri nasıl göründüğüne dikkat edin. Özel işletmelerin gizliliğini korumak için kurgusal ad ve adresler gösterilir.

    :::image type="content" source="./media/quick-demo-map-app/interactive-search.png" alt-text="Etkileşimli harita arama Web uygulaması":::


## <a name="clean-up-resources"></a>Kaynakları temizleme

>[!WARNING]
>[Sonraki adımlar](#next-steps) bölümünde listelenen öğreticiler, Azure haritalar 'ı hesabınızla birlikte nasıl kullanacağınızı ve yapılandıracağınızı ayrıntılandırır. Öğreticilere devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin.

Öğreticilere devam etmeyi planlamıyorsanız, kaynakları temizlemek için şu adımları uygulayın:

1. **AzureMapDemo.html** Web uygulamasını çalıştıran tarayıcıyı kapatın.
2. Azure portal sayfasına gidin. Ana Portal sayfasından **tüm kaynaklar** ' ı seçin. Ya da sol üst köşedeki menü simgesine tıklayın. **Tüm kaynaklar**’ı seçin.
3. Azure haritalar hesabınıza tıklayın. Sayfanın üst kısmında **Sil**' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Maps hesabınızı oluşturdunuz ve bir demo uygulaması oluşturdunuz. Azure haritalar hakkında daha fazla bilgi edinmek için aşağıdaki öğreticilere göz atın:

> [!div class="nextstepaction"]
> [Azure haritalar ile yakın ilgi çekici noktaları arayın](tutorial-search-location.md)

Daha fazla kod örneği ve etkileşimli kodlama deneyimi için aşağıdaki kılavuzlara bakın:

> [!div class="nextstepaction"]
> [Azure haritalar arama hizmeti ile adres bulma](how-to-search-for-address.md)

> [!div class="nextstepaction"]
> [Azure haritalar 'ı kullanın Harita Denetimi](how-to-use-map-control.md)
