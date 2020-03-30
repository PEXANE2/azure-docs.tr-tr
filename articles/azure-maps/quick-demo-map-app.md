---
title: 'Quickstart: Azure Haritalar ile etkileşimli harita arama | Microsoft Azure Haritaları'
description: Microsoft Azure Maps web SDK'yı kullanarak etkileşimli harita araması için demo web uygulamasını nasıl oluşturabilirsiniz öğrenin.
author: philmea
ms.author: philmea
ms.date: 1/14/2020
ms.topic: quickstart
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 13dc5f6c7175e1ed568199abcbaa4c5d9a20fa7f
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80334434"
---
# <a name="quickstart-create-an-interactive-search-map-by-using-azure-maps"></a>Hızlı başlangıç: Azure Haritalar'ı kullanarak etkileşimli bir arama haritası oluşturma

Bu makalede, Azure Haritalar’ın kullanıcılara etkileşimli arama deneyimi sunan bir harita oluşturmaya yönelik özellikler gösterilmektedir. Bu temel adımlar üzerinden size yol olur:
* Kendi Azure Haritalar hesabınızı oluşturun.
* Demo web uygulamasında kullanmak için hesap anahtarınızı alın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com)oturum açın.

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
    * Bu hesabın *Fiyatlandırma katmanı.*
    * *Lisans*’ı ve *Gizlilik Bildirimi*’ni okuyun ve onay kutusunu işaretleyerek koşulları kabul edin.
    * **Oluştur** düğmesine tıklayın.

![Portalda Haritalar hesabı oluşturma](./media/quick-demo-map-app/create-account.png)

<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>Hesabınızın birincil anahtarını alma

Haritalar hesabınız başarıyla oluşturulduktan sonra, Haritalar API’lerini sorgulamanıza olanak sağlayan anahtarı alın. Azure Haritalar hizmetlerini ararken hesabınızın birincil anahtarını abonelik anahtarı olarak kullanmanızı öneririz.

1. Portalda Haritalar hesabınızı açın.
2. Ayarlar bölümünde Kimlik **Doğrulama'yı**seçin.
3. **Birincil Anahtar**’ı panonuza kopyalayın. Bu öğreticinin ilerleyen kısmında kullanmak üzere bunu yerel olarak kaydedin.

![Azure portalında Birincil Anahtar Azure Haritaları anahtarını alın](./media/quick-demo-map-app/get-key.png)

## <a name="download-the-application"></a>Uygulamayı indirme

1. [InteractiveSearch.html'e](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/interactiveSearch.html) gidin ve GitHub Kullanıcı Arabirimi içindeki içeriği görüntülemek için tıklatın. **Ham** düğmesine sağ tıklayın ve dosyayı indirmek için dosyanın içeriğini kopyalayın veya 'Olarak kaydedin'
2. Bu dosyanın içeriğini **AzureMapDemo.html**olarak yerel olarak kaydedin. Metin düzenleyicisinde açın.
3. Dizeyi `<Your Azure Maps Key>`arayın. Önceki bölümden **Birincil Anahtar** değeri ile değiştirin.

## <a name="open-the-application"></a>Uygulamayı açma

1. **AzureMapDemo.html** dosyasını istediğiniz bir tarayıcıda açın.
2. Los Angeles şehrinin haritasını gözlemleyin. Yakınlaştırma düzeyine bağlı olarak haritanın daha fazla veya daha az bilgiyle nasıl işlendiğini görmek için yakınlaştırma ve uzaklaştırma yapın. 
3. Haritanın varsayılan merkezini değiştirin. **AzureMapDemo.html** dosyasında, **center** adlı değişkeni arayın. Bu değişkenin boylam ve enlem çiftini, yeni **[-74.0060, 40.7128]** değerleriyle değiştirin. Dosyayı kaydedin ve tarayıcınızı yenileyin.
4. Etkileşimli arama deneyimini deneyin. Demo web uygulamasının sol üst köşesindeki arama **kutusunda, restoran**arayın.
5. Farenizi arama kutusunun altında görünen adresler ve konumlar listesinin üzerinde taşıyın. Haritadaki ilgili pinin bu konumla ilgili bilgileri nasıl ortaya çıkarabildiğini fark edin. Özel işletmelerin gizliliğini korumak için kurgusal ad ve adresler gösterilir.

    ![Etkileşimli harita arama web uygulaması](./media/quick-demo-map-app/interactive-search.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiler, Azure Haritalar'ı hesabınızla nasıl kullanacağınızı ve yapılandıracağımız hakkında ayrıntılı bilgi ve rekatlar sunar. Öğreticilere devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi düşünmüyorsanız, kaynakları temizlemek için aşağıdaki adımları izleyin:

1. **AzureMapDemo.html** web uygulamasını çalıştıran tarayıcıyı kapatın.
2. Azure portalının sol menüsünden **Tüm kaynakları**seçin. Ardından Azure Haritalar hesabınızı seçin. **Tüm kaynaklar** bıçağının üst kısmında **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Haritalar hesabınızı oluşturdunuz ve bir demo uygulaması oluşturdunuz. Azure Haritalar hakkında bilgi edinmek için aşağıdaki öğreticilere göz atın:

> [!div class="nextstepaction"]
> [Azure Haritalar'ı kullanarak yakındaki ilgi çekici noktaları arayın](tutorial-search-location.md)

Daha fazla kod örneği ve etkileşimli kodlama deneyimi için aşağıdaki kılavuzlara bakın:

> [!div class="nextstepaction"]
> [Azure Haritalar arama hizmetini kullanarak bir adres bulma](how-to-search-for-address.md)

> [!div class="nextstepaction"]
> [Azure Haritalar Harita Denetimini Kullanma](how-to-use-map-control.md)
