---
title: 'Hızlı başlangıç: Azure Maps ile etkileşimli harita arama'
description: Azure hızlı başlangıç-Azure haritaları kullanarak bir demo etkileşimli harita araması oluşturma
author: walsehgal
ms.author: v-musehg
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 7553499dc9aba8d20bee126204b6d19f2386d295
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75408407"
---
# <a name="quickstart-create-an-interactive-search-map-by-using-azure-maps"></a>Hızlı başlangıç: Azure haritalar kullanarak etkileşimli arama haritası oluşturma

Bu makalede, Azure Haritalar’ın kullanıcılara etkileşimli arama deneyimi sunan bir harita oluşturmaya yönelik özellikler gösterilmektedir. Bu temel adımlarda size kılavuzluk eder:
* Kendi Azure haritalar hesabınızı oluşturun.
* Tanıtım Web uygulamasında kullanılacak hesap anahtarınızı alın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com)’ında oturum açın.

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-maps"></a>Azure Maps hesabı oluşturma

Aşağıdaki adımları uygulayarak yeni bir Haritalar hesabı oluşturun:

1. [Azure portalının](https://portal.azure.com) sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. *Market’te Ara* kutusuna **Haritalar** yazın.
3. *Sonuçlar* içinden **Haritalar**’ı seçin. Haritanın altında görüntülenen **Oluştur** düğmesine tıklayın.
4. **Haritalar Hesabı Oluştur** sayfasında aşağıdaki değerleri girin:
    * Bu hesap için kullanmak istediğiniz *Abonelik*.
    * Bu hesap için *Kaynak grubu* adı. Kaynak grubu için *Yeni oluştur* veya *Mevcut olanı kullan* seçeneğini belirleyebilirsiniz.
    * Yeni hesabınıza verilen *Ad*.
    * Bu hesabın *fiyatlandırma katmanı* .
    * *Lisans*’ı ve *Gizlilik Bildirimi*’ni okuyun ve onay kutusunu işaretleyerek koşulları kabul edin.
    * **Oluştur** düğmesine tıklayın.

![Portalda Haritalar hesabı oluşturma](./media/quick-demo-map-app/create-account.png)

<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>Hesabınızın birincil anahtarını alma

Haritalar hesabınız başarıyla oluşturulduktan sonra, Haritalar API’lerini sorgulamanıza olanak sağlayan anahtarı alın. Azure haritalar hizmetlerini çağırırken hesabınızın birincil anahtarını abonelik anahtarı olarak kullanmanızı öneririz.

1. Portalda Haritalar hesabınızı açın.
2. Ayarlar bölümünde **kimlik doğrulaması**' nı seçin.
3. **Birincil Anahtar**’ı panonuza kopyalayın. Bu öğreticinin ilerleyen kısmında kullanmak üzere bunu yerel olarak kaydedin.

![Portalda Birincil Anahtar’ı alma](./media/quick-demo-map-app/get-key.png)

## <a name="download-the-application"></a>Uygulamayı indirme

1. [interactiveSearch.html](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/interactiveSearch.html) dosyasının içeriklerini indirin veya kopyalayın.
2. Bu dosyanın içeriğini **AzureMapDemo. html**olarak yerel olarak kaydedin. Bir metin düzenleyicisinde açın.
3. `<Your Azure Maps Key>`dize için arama yapın. Önceki bölümdeki **birincil anahtar** değeriyle değiştirin.

## <a name="open-the-application"></a>Uygulamayı açma

1. **AzureMapDemo.html** dosyasını istediğiniz bir tarayıcıda açın.
2. Los Angeles şehrinde gösterilen Haritayı gözlemleyin. Yakınlaştırma düzeyine bağlı olarak haritanın daha fazla veya daha az bilgiyle nasıl işlendiğini görmek için yakınlaştırma ve uzaklaştırma yapın. 
3. Haritanın varsayılan merkezini değiştirin. **AzureMapDemo.html** dosyasında, **center** adlı değişkeni arayın. Bu değişkenin boylam ve enlem çiftini, yeni **[-74.0060, 40.7128]** değerleriyle değiştirin. Dosyayı kaydedin ve tarayıcınızı yenileyin.
4. Etkileşimli arama deneyimini deneyin. Demo Web uygulamasının sol üst köşesindeki arama kutusunda **Restoran**' ı arayın.
5. Farenizi arama kutusunun altında görüntülenen adresler ve konumlar listesinin üzerine taşıyın. Haritadaki karşılık gelen PIN 'in bu konumla ilgili bilgileri nasıl göründüğüne dikkat edin. Özel işletmelerin gizliliğini korumak için kurgusal ad ve adresler gösterilir.

    ![Etkileşimli arama Web uygulaması](./media/quick-demo-map-app/interactive-search.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiler, Azure haritalar 'ı hesabınızla nasıl kullanacağınızı ve yapılandıracağınızı ayrıntılandırır. Öğreticilere devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, kaynakları temizlemek için şu adımları uygulayın:

1. **AzureMapDemo. html** Web uygulamasını çalıştıran tarayıcıyı kapatın.
2. Azure portal sol menüden **tüm kaynaklar**' ı seçin. Ardından Azure haritalar hesabınızı seçin. **Tüm kaynaklar** dikey penceresinin en üstünde **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Maps hesabınızı oluşturdunuz ve bir demo uygulaması oluşturdunuz. Azure haritalar hakkında bilgi edinmek için aşağıdaki öğreticilere göz atın:

> [!div class="nextstepaction"]
> [Azure haritalar 'ı kullanarak yakın ilgi çekici noktaları arayın](tutorial-search-location.md)

Daha fazla kod örneği ve etkileşimli kodlama deneyimi için aşağıdaki kılavuzlara bakın:

> [!div class="nextstepaction"]
> [Azure haritalar arama hizmetini kullanarak bir adres bulma](how-to-search-for-address.md)

> [!div class="nextstepaction"]
> [Azure haritalar 'ı kullanın Harita Denetimi](how-to-use-map-control.md)
