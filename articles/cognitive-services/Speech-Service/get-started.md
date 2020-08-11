---
title: Konuşma tanıma hizmetini ücretsiz olarak deneyin
titleSuffix: Azure Cognitive Services
description: Konuşma hizmetini kullanmaya başlamak kolay ve ekonomik bir işlemdir. Hizmetin neler yapabileceğini keşfedebilmeniz ve gereksinimlerinize uygun olup olmadığına karar verebilmeniz için ücretsiz olarak iki seçenek mevcuttur.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: erhopf
ms.custom: seodec18, seo-javascript-october2019
ms.openlocfilehash: 5dc1f8ed31c8d7199a31643a76e611d828da77ac
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88056474"
---
# <a name="try-the-speech-service-for-free"></a>Konuşma tanıma hizmetini ücretsiz olarak deneyin

Bu makalede, hizmetin neler yapabileceğini keşfedebilmeniz ve gereksinimlerinize uygun olup olmadığına karar vermek için konuşma hizmetini ücretsiz olarak test etmek için bir seçenek belirleyin. Durumunuza ve kullanım örneğine bağlı olarak aşağıdaki iki seçenekten birini belirleyin:

- [1. seçenek](#no-card): kredi kartı bilgilerini (mevcut bir Azure hesabınız olması gerekir) sağlamadan hemen **ücretsiz deneme** API anahtarları alın. **Ücretsiz deneme süresi** 30 gün sürer ve veriler sonda silinir. Bu seçenek, hizmet ile hızlı deneme için idealdir.
- [2. seçenek](#new-resource): **ücretsiz bir abonelik** (kredi kartı bilgileri gerekir) kullanarak Azure 'da yeni bir konuşma kaynağı oluşturun. **Ücretsiz abonelik** , genellikle ücretli bir aboneliğe göre daha sıkı bir ücret sınırlarına sahiptir. Hizmeti test etmek istiyorsanız bu seçenek en iyi seçenektir, ancak gelecekte ücretli bir aboneliğe yükseltmeyi ve veri kaybetmek istemediğinizi de planlayın.

## <a name="try-the-speech-service-without-credit-card-info"></a><a id="no-card"></a>Konuşma hizmeti 'ni kredi kartı bilgisi olmadan deneyin

30 günlük ücretsiz denemeyi etkinleştirmek ve API anahtarları almak için aşağıdaki adımları izleyin. Deneme süreniz, aşağıdaki adımlar tamamlandıktan hemen sonra başlar.

1. Bilişsel [hizmeti 'Ni deneyin](https://azure.microsoft.com/try/cognitive-services/)' e gidin.
1. **Konuşma API 'leri** sekmesini seçin.
1. **API anahtarını al**' ı seçin.

Faturalandırma seçimlerinizle karşılaşırsınız. Ücretsiz seçeneğini belirleyin ve ardından kullanıcı sözleşmesini okuyun ve onaylayın. Konuşma hizmetini 30 gün boyunca ücretsiz olarak denemek için kullanabileceğiniz anahtarlarla karşılaşırsınız.

## <a name="try-the-speech-service-by-creating-an-azure-resource"></a><a id="new-resource"></a>Azure kaynağı oluşturarak konuşma hizmetini deneyin

Aşağıdaki adımlar için hem Microsoft hesabı hem de bir Azure hesabınızın olması gerekir. Bir Microsoft hesabı yoksa, [Microsoft hesabı portalında](https://account.microsoft.com/account)ücretsiz bir ücret karşılığında kaydolabilirsiniz. **Microsoft hesabıyla oturum açın** ' yi seçin ve ardından oturum açmak Istendiğinde **Microsoft hesabı oluştur**' u seçin. Yeni Microsoft hesabı oluşturup doğrulamak için adımları izleyin.

Microsoft hesabı aldıktan sonra [Azure kaydolma sayfasına](https://azure.microsoft.com/free/ai/)gidin, **ücretsiz Başlat**' ı seçin ve Microsoft hesabı kullanarak yeni bir Azure hesabı oluşturun.

> [!NOTE]
> Konuşma hizmetinde iki hizmet katmanı vardır: ücretsiz ve abonelik, farklı sınırlamalara ve avantajlara sahiptir. Ücretsiz bir Azure hesabı için kaydolduğunuzda, hizmet kredisi $200 ile birlikte gelir ve ücretli bir konuşma hizmeti aboneliğine uygulayabileceğiniz, 30 güne kadar geçerli olur.
>
> Ücretsiz, düşük hacimli konuşma hizmeti katmanını kullanıyorsanız, ücretsiz deneme veya hizmet kredinizin süresi dolduktan sonra bile bu ücretsiz aboneliği koruyabilirsiniz.
>
> Daha fazla bilgi için bkz. bilişsel [Hizmetler fiyatlandırması-konuşma hizmeti](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="create-the-resource"></a>Kaynağı oluşturma

Azure hesabınıza bir konuşma hizmeti kaynağı (ücretsiz veya ücretli katman) eklemek için:

1. Microsoft hesabınızı kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.

1. Portalın sol üst kısmında **kaynak oluştur** ' u seçin. **Kaynak oluştur**' u görmüyorsanız, sol üstteki daraltılan menüyü seçerek her zaman bulabilirsiniz:

   ![daraltılmış gezinti düğmesi](media/index/collapsed-nav.png)

1. **Yeni** pencerede, arama kutusuna "konuşma" YAZıN ve ENTER tuşuna basın.

1. Arama sonuçlarında **konuşma**' yı seçin.

   ![Konuşma Arama sonuçları](media/index/speech-search.png)

1. **Oluştur**' u seçin, sonra:

   - Yeni kaynağınız için benzersiz bir ad verin. Ad, aynı hizmete bağlı birden çok abonelik arasında ayrım yapmanıza yardımcı olur.
   - Ücretlerin nasıl faturalandırıldığını belirlemek için yeni kaynağın ilişkilendirildiği Azure aboneliğini seçin.
   - Kaynağın kullanılacağı [bölgeyi](regions.md) seçin.
   - Ücretsiz (F0) ya da ücretli (S0) fiyatlandırma katmanını seçin. Her katmana yönelik fiyatlandırma ve kullanım kotaları hakkında tam bilgi için, **tam fiyatlandırma ayrıntılarını görüntüle**' yi seçin.
   - Bu konuşma aboneliği için yeni bir kaynak grubu oluşturun veya aboneliği var olan bir kaynak grubuna atayın. Kaynak grupları, çeşitli Azure aboneliklerinizi düzenli halde tutmanıza yardımcı olur.
   - **Oluştur**’u seçin. Bu işlem sizi dağıtıma genel bakış ve dağıtım ilerleme durumu iletilerini görüntüleyecek şekilde götürür.

> [!NOTE]
> Bir veya birden çok bölgede sınırsız sayıda standart katmanlı abonelik oluşturabilirsiniz. Ancak, yalnızca bir serbest katmanlı abonelik oluşturabilirsiniz. Ücretsiz katmanda 7 gün boyunca kullanılmamış olarak kalan model dağıtımları otomatik olarak kullanımdan kaldırılacaktır.

Yeni konuşma kaynağınızın dağıtılması birkaç dakika sürer. Dağıtım tamamlandıktan sonra **Kaynağa Git** ' i seçin ve sol gezinti bölmesinde konuşma hizmeti abonelik anahtarlarınızı göstermek için **anahtarlar** ' ı seçin. Her abonelik iki anahtara sahiptir; uygulamanızda herhangi bir anahtarı kullanabilirsiniz. Bir anahtarı kod düzenleyicinize veya başka bir konuma hızlıca kopyalamak/yapıştırmak için, her bir anahtarın yanındaki Kopyala düğmesini seçin, pencereler arasında Pano içeriğini istenen konuma yapıştırın.

> [!IMPORTANT]
> Bu abonelik anahtarları bilişsel hizmet API 'nize erişmek için kullanılır. Anahtarlarınızı paylaşmayın. Bunları güvenli bir şekilde depolayın (örneğin, Azure Key Vault kullanarak). Ayrıca, bu anahtarların düzenli olarak yeniden oluşturulması önerilir. API çağrısı yapmak için yalnızca bir anahtar gereklidir. İlk anahtarı yeniden oluştururken, hizmete devam eden erişim için ikinci anahtarı kullanabilirsiniz.

## <a name="switch-to-a-new-subscription"></a>Yeni bir aboneliğe geç

Bir abonelikten diğerine geçiş yapmak için, örneğin ücretsiz deneme süreniz sona erdiğinde veya uygulamanızı yayımladığınızda, kodunuzdaki bölge ve abonelik anahtarını yeni Azure kaynağının bölge ve abonelik anahtarıyla değiştirin.

## <a name="about-regions"></a>Bölgeler hakkında

- Uygulamanız bir [konuşma SDK 'sı](speech-sdk.md)kullanıyorsa, `westus` bir konuşma yapılandırması oluştururken gibi bölge kodunu sağlarsınız.
- Uygulamanız konuşma hizmeti 'nin [REST API 'lerinden](rest-apis.md)birini kullanıyorsa bölge, istek yaparken kullandığınız uç nokta URI 'sinin bir parçasıdır.
- Bölge için oluşturulan anahtarlar yalnızca o bölgede geçerlidir. Bunları diğer bölgelerle birlikte kullanmaya çalışmak, kimlik doğrulama hatalarına neden olur.

## <a name="next-steps"></a>Sonraki adımlar

10 dakikalık hızlı başlangıçlardan birini tamamlayıp SDK örneklerimize göz atın:

> [!div class="nextstepaction"]
> [Hızlı başlangıç: konuşma CLI](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programmer-tool-spx) 
>  ile konuşmayı tanıma [Konuşma SDK örnekleri](speech-sdk.md#sample-source-code)
