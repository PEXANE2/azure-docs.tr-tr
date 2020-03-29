---
title: Konuşma tanıma hizmetini ücretsiz olarak deneyin
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti ile başlamak kolay ve uygun fiyatlı. Hizmetin neler yapabileceğini keşfedebilmeniz ve ihtiyaçlarınız için doğru olup olmadığına karar verebilmeniz için ücretsiz olarak iki seçenek mevcuttur.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: erhopf
ms.custom: seodec18, seo-javascript-october2019
ms.openlocfilehash: f74e3ea3d20ad2666b434e009cf62add6f88d200
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219670"
---
# <a name="try-the-speech-service-for-free"></a>Konuşma tanıma hizmetini ücretsiz olarak deneyin

Bu makalede, hizmetin neler yapabileceğini keşfetmek ve gereksinimleriniz için doğru olup olmadığına karar vermek için Konuşma hizmetini ücretsiz olarak kolayca sınamak için bir seçenek seçebilirsiniz. Durumunuza ve kullanım durumunuza bağlı olarak aşağıdaki iki seçenekten birini seçin:

- [Seçenek 1](#no-card): Kredi kartı bilgileri vermeden hemen **ücretsiz deneme** API anahtarları alın (mevcut bir Azure hesabınız olması gerekir). **Ücretsiz deneme** 30 gün sürer ve veriler sonunda silinir. Bu seçenek, hizmetle hızlı deneme için en iyisidir.
- [Seçenek 2](#new-resource): **Ücretsiz abonelik** (kredi kartı bilgileri gereklidir) kullanarak ücretsiz olarak Azure'da yeni bir Konuşma kaynağı oluşturun. **Ücretsiz abonelik,** ücretli abonelikten daha katı ücret limitlerine sahiptir. Hizmeti sınamak, ancak gelecekte ücretli bir aboneye yükseltmeyi planlıyorsanız ve veri kaybetmek istemiyorsanız, bu seçenek en iyisidir.

## <a name="try-the-speech-service-without-credit-card-info"></a><a id="no-card"></a>Kredi kartı bilgisi olmadan Konuşma hizmetini deneyin

30 günlük ücretsiz deneme sürümünü etkinleştirmek ve API tuşlarını almak için aşağıdaki adımları tamamlayın. Deneme süreniz, aşağıdaki adımlar tamamlandıktan hemen sonra başlar.

1. Bilişsel [Hizmet deneyin](https://azure.microsoft.com/try/cognitive-services/)gidin.
1. Konuşma **API'leri** sekmesini seçin.
1. **API Anahtarı Al'ı**seçin.

Size fatura landırma seçenekleri sunulacaktır. Ücretsiz seçeneği seçin ve ardından kullanıcı sözleşmesini okuyun ve onaylayın. Konuşma hizmetini 30 gün boyunca ücretsiz olarak denemek için kullanabileceğiniz anahtarlarla birlikte sunulur.

## <a name="try-the-speech-service-by-creating-an-azure-resource"></a><a id="new-resource"></a>Azure kaynağı oluşturarak Konuşma hizmetini deneyin

Aşağıdaki adımlar için hem Microsoft hesabına hem de Azure hesabına ihtiyacınız var. Microsoft hesabınız yoksa, [Microsoft hesap portalından](https://account.microsoft.com/account)ücretsiz olarak kaydolabilirsiniz. **Microsoft ile Oturum Aç'ı** seçin ve oturum açmam istendiğinde Microsoft hesabı **oluştur'u**seçin. Yeni Microsoft hesabınızı oluşturmak ve doğrulamak için adımları izleyin.

Bir Microsoft hesabınız olduktan sonra Azure [kaydolma sayfasına](https://azure.microsoft.com/free/ai/)gidin , **ücretsiz Başlat'ı**seçin ve Microsoft hesabı kullanarak yeni bir Azure hesabı oluşturun.

> [!NOTE]
> Konuşma hizmetinin iki hizmet katmanı vardır: farklı sınırlamaları ve avantajları olan ücretsiz ve abonelik. Ücretsiz bir Azure hesabına kaydolduğunuzda, 30 güne kadar geçerli olan ücretli konuşma hizmeti aboneliği ne kadar uygulayabileceğiniz 200 TL'lik hizmet kredisi yle birlikte gelir.
>
> Ücretsiz, düşük hacimli Konuşma hizmeti katmanını kullanırsanız, ücretsiz deneme veya hizmet krediniz indikten sonra bile bu ücretsiz aboneliği koruyabilirsiniz.
>
> Daha fazla bilgi için Bilişsel [Hizmetler fiyatlandırma - Konuşma hizmetine](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)bakın.

### <a name="create-the-resource"></a>Kaynağı oluşturma

Azure hesabınıza bir Konuşma hizmeti kaynağı (ücretsiz veya ücretli katman) eklemek için:

1. Microsoft hesabınızı kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.

1. Portalın sol üst kısmında **bir kaynak oluştur'u** seçin. **Kaynak Oluştur'u**görmüyorsanız, sol üstteki daraltılmış menüyü seçerek her zaman bulabilirsiniz:

   ![daraltılmış navigasyon düğmesi](media/index/collapsed-nav.png)

1. **Yeni** pencerede, arama kutusuna "konuşma" yazın ve ENTER tuşuna basın.

1. Arama sonuçlarında **Konuşma'yı**seçin.

   ![konuşma arama sonuçları](media/index/speech-search.png)

1. **Oluştur'u**seçin, sonra:

   - Yeni kaynağınız için benzersiz bir ad verin. Ad, aynı hizmete bağlı birden çok abonelik arasında ayrım yapmaya yardımcı olur.
   - Ücretlerin nasıl faturalandırıldırıldırıldığını belirlemek için yeni kaynağın ilişkili olduğu Azure aboneliğini seçin.
   - Kaynağın kullanılacağı [bölgeyi](regions.md) seçin.
   - Ücretsiz (F0) veya ücretli (S0) fiyatlandırma katmanı seçin. Her katman için fiyatlandırma ve kullanım kotaları hakkında tam bilgi **için, tam fiyatlandırma ayrıntılarını görüntüle'yi**seçin.
   - Bu Konuşma aboneliği için yeni bir kaynak grubu oluşturun veya aboneliği varolan bir kaynak grubuna atayın. Kaynak grupları, çeşitli Azure aboneliklerinizi düzenli tutmanıza yardımcı olur.
   - **Oluştur'u**seçin. Bu, sizi dağıtıma genel bakışa götürür ve dağıtım ilerleme iletilerini görüntüler.

> [!NOTE]
> Bir veya birden çok bölgede sınırsız sayıda standart katman aboneliği oluşturabilirsiniz. Ancak, yalnızca bir serbest katmanlı abonelik oluşturabilirsiniz. 7 gün boyunca kullanılmayan ücretsiz katmandaki model dağıtımları otomatik olarak devre dışı kalır.

Yeni Konuşma kaynağınızı dağıtmak birkaç dakika nızı alır. Dağıtım tamamlandıktan sonra **kaynağa Git'i** seçin ve sol gezinti bölmesinde Konuşma hizmeti abonelik anahtarlarınızı görüntülemek için **Tuşlar'ı** seçin. Her aboneliğin iki anahtarı vardır; uygulamanızda her iki anahtarı da kullanabilirsiniz. Kod düzenleyicinize veya başka bir konuma bir anahtarı hızla kopyalamak/yapıştırmak için, her anahtarın yanındaki kopyala düğmesini seçin, pano içeriğini istenilen konuma yapıştırmak için pencereleri değiştirin.

> [!IMPORTANT]
> Bu abonelik anahtarları Bilişsel Hizmet API'nıza erişmek için kullanılır. Anahtarlarınızı paylaşmayın. Örneğin Azure Key Vault'u kullanarak güvenli bir şekilde saklayın. Ayrıca bu anahtarları düzenli olarak yenilemenizi öneririz. API araması yapmak için yalnızca bir anahtar gereklidir. İlk anahtarı yenilerken, hizmete sürekli erişim için ikinci anahtarı kullanabilirsiniz.

## <a name="switch-to-a-new-subscription"></a>Yeni bir abonelmeye geçiş

Örneğin, ücretsiz deneme süreniz sona erdiğinde veya uygulamanızı yayımladığınızda bir abonelikten diğerine geçmek için, kodunuzdaki bölge ve abonelik anahtarını yeni Azure kaynağının bölge ve abonelik anahtarıyla değiştirin.

## <a name="about-regions"></a>Bölgeler hakkında

- Uygulamanız bir [Konuşma SDK](speech-sdk.md)kullanıyorsa, konuşma `westus`yapılandırması oluştururken bölge kodunu sağlarsınız.
- Uygulamanız Konuşma hizmetinin [REST API'lerinden](rest-apis.md)birini kullanıyorsa, bölge istekte bulunmak ta iken kullandığınız bitiş noktası URI'nin bir parçasıdır.
- Bir bölge için oluşturulan anahtarlar yalnızca o bölgede geçerlidir. Bunları diğer bölgelerle kullanmaya çalışmak kimlik doğrulama hatalarına neden olur.

## <a name="next-steps"></a>Sonraki adımlar

10 dakikalık hızlı başlangıçlarımızdan birini tamamlayın veya SDK örneklerimize göz atın:

> [!div class="nextstepaction"]
> [Hızlı başlangıç: C#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
> [Speech SDK örneklerindeki](speech-sdk.md#get-the-samples) konuşmayı tanıyın
