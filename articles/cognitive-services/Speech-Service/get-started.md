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
ms.date: 02/26/2020
ms.author: erhopf
ms.custom: seodec18, seo-javascript-october2019
ms.openlocfilehash: f74e3ea3d20ad2666b434e009cf62add6f88d200
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77913676"
---
# <a name="try-the-speech-service-for-free"></a>Konuşma tanıma hizmetini ücretsiz olarak deneyin

Bu makalede, hizmetin neler yapabileceğini keşfedebilmeniz ve gereksinimlerinize uygun olup olmadığına karar vermek için konuşma hizmetini ücretsiz olarak test etmek için bir seçenek belirleyin. Durumunuza ve kullanım örneğine bağlı olarak aşağıdaki iki seçenekten birini belirleyin:

- [1. seçenek](#no-card): kredi kartı bilgilerini (mevcut bir Azure hesabınız olması gerekir) sağlamadan hemen **ücretsiz deneme** API anahtarları alın. **Ücretsiz deneme süresi** 30 gün sürer ve veriler sonda silinir. Bu seçenek, hizmet ile hızlı deneme için idealdir.
- [2. seçenek](#new-resource): **ücretsiz bir abonelik** (kredi kartı bilgileri gerekir) kullanarak Azure 'da yeni bir konuşma kaynağı oluşturun. **Ücretsiz abonelik** , genellikle ücretli bir aboneliğe göre daha sıkı bir ücret sınırlarına sahiptir. Hizmeti test etmek istiyorsanız bu seçenek en iyi seçenektir, ancak gelecekte ücretli bir aboneliğe yükseltmeyi ve veri kaybetmek istemediğinizi de planlayın.

## <a id="no-card"></a>Konuşma hizmeti 'ni kredi kartı bilgisi olmadan deneyin

30 günlük ücretsiz denemeyi etkinleştirmek ve API anahtarları almak için aşağıdaki adımları izleyin. Deneme süreniz, aşağıdaki adımlar tamamlandıktan hemen sonra başlar.

1. Bilişsel [hizmeti 'Ni deneyin](https://azure.microsoft.com/try/cognitive-services/)' e gidin.
1. **Konuşma API 'leri** sekmesini seçin.
1. **API anahtarını al**' ı seçin.

Faturalandırma seçimlerinizle karşılaşırsınız. Ücretsiz seçeneğini belirleyin ve ardından kullanıcı sözleşmesini okuyun ve onaylayın. Konuşma hizmetini 30 gün boyunca ücretsiz olarak denemek için kullanabileceğiniz anahtarlarla karşılaşırsınız.

## <a id="new-resource"></a>Azure kaynağı oluşturarak konuşma hizmetini deneyin

Aşağıdaki adımlar için hem Microsoft hesabı hem de bir Azure hesabınızın olması gerekir. Bir Microsoft hesabı yoksa, [Microsoft hesabı portalında](https://account.microsoft.com/account)ücretsiz bir ücret karşılığında kaydolabilirsiniz. **Microsoft hesabıyla oturum açın** ' yi seçin ve ardından oturum açmak Istendiğinde **Microsoft hesabı oluştur**' u seçin. Oluşturun ve yeni Microsoft hesabınızı doğrulamak için adımları izleyin.

Microsoft hesabı aldıktan sonra [Azure kaydolma sayfasına](https://azure.microsoft.com/free/ai/)gidin, **ücretsiz Başlat**' ı seçin ve Microsoft hesabı kullanarak yeni bir Azure hesabı oluşturun.

> [!NOTE]
> Konuşma hizmetinde iki hizmet katmanı vardır: ücretsiz ve abonelik, farklı sınırlamalara ve avantajlara sahiptir. Ücretsiz bir Azure hesabı için kaydolduğunuzda, hizmet kredisi $200 ile birlikte gelir ve ücretli bir konuşma hizmeti aboneliğine uygulayabileceğiniz, 30 güne kadar geçerli olur.
>
> Ücretsiz, düşük hacimli konuşma hizmeti katmanını kullanıyorsanız, ücretsiz deneme veya hizmet kredinizin süresi dolduktan sonra bile bu ücretsiz aboneliği koruyabilirsiniz.
>
> Daha fazla bilgi için bkz. bilişsel [Hizmetler fiyatlandırması-konuşma hizmeti](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="create-the-resource"></a>Kaynağı oluşturma

Azure hesabınızda bir konuşma hizmeti kaynak (ücretsiz veya Ücretli katman) eklemek için:

1. Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com/) oturum açın.

1. Portalın sol üst kısmında **kaynak oluştur** ' u seçin. **Kaynak oluştur**' u görmüyorsanız, sol üstteki daraltılan menüyü seçerek her zaman bulabilirsiniz:

   ![daraltılmış gezinti düğmesi](media/index/collapsed-nav.png)

1. **Yeni** pencerede, arama kutusuna "konuşma" YAZıN ve ENTER tuşuna basın.

1. Arama sonuçlarında **konuşma**' yı seçin.

   ![Konuşma Arama sonuçları](media/index/speech-search.png)

1. **Oluştur**' u seçin, sonra:

   - Yeni kaynağınız için benzersiz bir ad verin. Ad, aynı hizmete bağlı birden çok abonelik arasında ayrım yapmanıza yardımcı olur.
   - Yeni kaynak ücretleri nasıl faturalandırılır belirlemek için ilişkili olduğu Azure aboneliğini seçin.
   - Kaynağın kullanılacağı [bölgeyi](regions.md) seçin.
   - Ücretsiz (F0) ya da ücretli (S0) fiyatlandırma katmanını seçin. Her katmana yönelik fiyatlandırma ve kullanım kotaları hakkında tam bilgi için, **tam fiyatlandırma ayrıntılarını görüntüle**' yi seçin.
   - Bu konuşma abonelik için yeni bir kaynak grubu oluşturun veya mevcut bir kaynak grubu aboneliği atayın. Kaynak düzenlenir, çeşitli Azure aboneliklerinizi tutmak Yardım gruplandırır.
   - **Oluştur**’u seçin. Bu işlem sizi dağıtıma genel bakış ve dağıtım ilerleme durumu iletilerini görüntüleyecek şekilde götürür.

> [!NOTE]
> Bir veya birden çok bölgede standart katman abonelikleri sınırsız sayıda oluşturabilirsiniz. Ancak, yalnızca bir ücretsiz katman abonelik oluşturabilir. Ücretsiz katmanda 7 gün boyunca kullanılmamış olarak kalan model dağıtımları otomatik olarak kullanımdan kaldırılacaktır.

Yeni konuşma kaynağınızın dağıtılması birkaç dakika sürer. Dağıtım tamamlandıktan sonra **Kaynağa Git** ' i seçin ve sol gezinti bölmesinde konuşma hizmeti abonelik anahtarlarınızı göstermek için **anahtarlar** ' ı seçin. Her aboneliğin, iki anahtarı vardır; uygulamanızda iki anahtarı kullanabilirsiniz. Bir anahtarı kod düzenleyicinize veya başka bir konuma hızlıca kopyalamak/yapıştırmak için, her bir anahtarın yanındaki Kopyala düğmesini seçin, pencereler arasında Pano içeriğini istenen konuma yapıştırın.

> [!IMPORTANT]
> Bu abonelik anahtarları bilişsel hizmet API 'nize erişmek için kullanılır. Anahtarlarınızı paylaşmayın. Bunları güvenli bir şekilde depolayın (örneğin, Azure Key Vault kullanarak). Ayrıca, bu anahtarların düzenli olarak yeniden oluşturulması önerilir. API çağrısı yapmak için yalnızca bir anahtar gereklidir. İlk anahtarı yeniden oluştururken, hizmete devam eden erişim için ikinci anahtarı kullanabilirsiniz.

## <a name="switch-to-a-new-subscription"></a>Yeni bir aboneliğe geçme

Bir abonelikten diğerine geçmek için örneğin, ücretsiz deneme süresinin sona erdiği veya uygulamanızı yayımladığınızda değiştirin kodunuzu bölge ve abonelik anahtarı ile yeni bir Azure kaynak bölge ve abonelik anahtarı.

## <a name="about-regions"></a>Bölgeler hakkında

- Uygulamanız bir [konuşma SDK 'sı](speech-sdk.md)kullanıyorsa, bir konuşma yapılandırması oluştururken `westus`gibi bölge kodunu sağlarsınız.
- Uygulamanız konuşma hizmeti 'nin [REST API 'lerinden](rest-apis.md)birini kullanıyorsa bölge, istek yaparken kullandığınız uç nokta URI 'sinin bir parçasıdır.
- Bir bölgede oluşturulan anahtarları yalnızca bu bölgede geçerlidir. Diğer bölgeler ile kullanılmaya çalışılırsa, kimlik doğrulama hataları neden olur.

## <a name="next-steps"></a>Sonraki adımlar

10 dakikalık başlangıçtan biriyle tamamlamak veya SDK örneklerimizi denetleyin:

> [!div class="nextstepaction"]
> [Hızlı başlangıç: C# ](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
> [konuşma SDK örneklerinde](speech-sdk.md#get-the-samples) konuşmayı tanıma
