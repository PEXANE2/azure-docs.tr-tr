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
ms.date: 07/05/2019
ms.author: erhopf
ms.custom: seodec18, seo-javascript-october2019
ms.openlocfilehash: 3ddba414ef1801b812d157ad734847099a8a9f25
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74806193"
---
# <a name="try-the-speech-service-for-free"></a>Konuşma tanıma hizmetini ücretsiz olarak deneyin

Konuşma hizmetinin kullanılması kolay ve ekonomik bir işlemdir. Hizmetin neler yapabileceğini keşfedebilmeniz ve gereksinimlerinize uygun olup olmadığına karar vermek için ücretsiz olarak iki seçenek mevcuttur:

- Kredi kartı bilgileri sağlamadan ücretsiz deneme sürümü alın (mevcut bir Azure hesabınız olması gerekir)
- Deneme süresi boyunca ücretsiz olarak yeni bir Azure hesabı oluşturun (kredi kartı bilgileri gerekir)

Bu makalede, gereksinimlerinize en uygun olan bu seçeneklerden birini seçersiniz.

> [!NOTE]
> Konuşma hizmetinde iki hizmet katmanı vardır: ücretsiz ve abonelik, farklı sınırlamalara ve avantajlara sahiptir. Ücretsiz bir Azure hesabı için kaydolduğunuzda, hizmet kredisi $200 ile birlikte gelir ve ücretli bir konuşma hizmeti aboneliğine uygulayabileceğiniz, 30 güne kadar geçerli olur.
>
> Ücretsiz, düşük hacimli konuşma hizmeti katmanını kullanıyorsanız, ücretsiz deneme veya hizmet kredinizin süresi dolduktan sonra bile bu ücretsiz aboneliği koruyabilirsiniz.
>
> Daha fazla bilgi için bkz. bilişsel [Hizmetler fiyatlandırması-konuşma hizmeti](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="try-the-speech-service-without-credit-card-info"></a>Konuşma hizmeti 'ni kredi kartı bilgisi olmadan deneyin

Konuşma hizmetini sonraki bölümdeki yönergeleri kullanarak denemeyi öneririz, ancak Aşağıdakiler geçerliyse bu bölümün yönergelerini tercih edebilirsiniz:

- Zaten etkin bir Azure hesabınız var.
- Konuşma hizmetini yeni bir Azure hesabı oluşturmadan değerlendirmek istiyorsunuz.
- Kredi kartı gerekmez ve deneme süresinden sonra hiç veri kaydedilmez.

> [!NOTE]
> Deneme süreniz, aşağıdaki adımlar tamamlandıktan hemen sonra başlar.

1. Bilişsel [hizmeti 'Ni deneyin](https://azure.microsoft.com/try/cognitive-services/)' e gidin.
1. Seçin **konuşma API'leri** sekmesi.
1. **API anahtarını al**' ı seçin.

Faturalandırma seçimlerinizle karşılaşırsınız. Ücretsiz seçeneğini belirleyin ve ardından kullanıcı sözleşmesini okuyun ve onaylayın. Konuşma hizmetini sınırlı bir süre için denemek üzere kullanabileceğiniz anahtarlarla karşılaşırsınız.

## <a name="try-the-speech-service-using-a-new-azure-account"></a>Yeni bir Azure hesabı kullanarak konuşma hizmetini deneyin

Yeni bir Azure hesabına kaydolmak için bir Microsoft hesabı gerekir. Bir Microsoft hesabı yoksa, [Microsoft hesabı portalında](https://account.microsoft.com/account)ücretsiz bir ücret karşılığında kaydolabilirsiniz. **Microsoft hesabıyla oturum açın** ' yi seçin ve ardından oturum açmak Istendiğinde **Microsoft hesabı oluştur**' u seçin. Oluşturun ve yeni Microsoft hesabınızı doğrulamak için adımları izleyin.

Microsoft hesabı aldıktan sonra [Azure kaydolma sayfasına](https://azure.microsoft.com/free/ai/)gidin, **ücretsiz Başlat**' ı seçin ve Microsoft hesabı kullanarak yeni bir Azure hesabı oluşturun.

### <a name="create-a-speech-resource-in-azure"></a>Azure'da bir konuşma kaynak oluşturma

> [!NOTE]
> Bir veya birden çok bölgede standart katman abonelikleri sınırsız sayıda oluşturabilirsiniz. Ancak, yalnızca bir ücretsiz katman abonelik oluşturabilir. Ücretsiz katmanda 7 gün boyunca kullanılmamış olarak kalan model dağıtımları otomatik olarak kullanımdan kaldırılacaktır.

Azure hesabınızda bir konuşma hizmeti kaynak (ücretsiz veya Ücretli katman) eklemek için:

1. Oturum [Azure portalında](https://portal.azure.com/) Microsoft hesabınızı kullanarak.

1. Seçin **kaynak Oluştur** , portalın sol üst köşesindeki. **Kaynak oluştur**' u görmüyorsanız, sol üstteki daraltılan menüyü seçerek her zaman bulabilirsiniz:

   ![daraltılmış gezinti düğmesi](media/index/collapsed-nav.png)

1. **Yeni** pencerede, arama kutusuna "konuşma" YAZıN ve ENTER tuşuna basın.

1. Arama sonuçlarında seçin **konuşma**.

   ![Konuşma Arama sonuçları](media/index/speech-search.png)

1. **Oluştur**' u seçin, sonra:

   - Yeni kaynağınız için benzersiz bir ad verin. Adı aynı hizmetin birden fazla aboneliğe arasında ayırt etmenize yardımcı olur.
   - Yeni kaynak ücretleri nasıl faturalandırılır belirlemek için ilişkili olduğu Azure aboneliğini seçin.
   - Kaynağın kullanılacağı [bölgeyi](regions.md) seçin.
   - Ücretsiz (F0) ya da ücretli (S0) fiyatlandırma katmanını seçin. Her katmana yönelik fiyatlandırma ve kullanım kotaları hakkında tam bilgi için, **tam fiyatlandırma ayrıntılarını görüntüle**' yi seçin.
   - Bu konuşma abonelik için yeni bir kaynak grubu oluşturun veya mevcut bir kaynak grubu aboneliği atayın. Kaynak düzenlenir, çeşitli Azure aboneliklerinizi tutmak Yardım gruplandırır.
   - **Oluştur**'u seçin. Bu işlem sizi dağıtıma genel bakış ve dağıtım ilerleme durumu iletilerini görüntüleyecek şekilde götürür.

Yeni konuşma kaynağınızın dağıtılması birkaç dakika sürer. Dağıtım tamamlandıktan sonra **Kaynağa Git** ' i seçin ve sol gezinti bölmesinde konuşma hizmeti abonelik anahtarlarınızı göstermek için **anahtarlar** ' ı seçin. Her aboneliğin, iki anahtarı vardır; uygulamanızda iki anahtarı kullanabilirsiniz. Bir anahtarı kod düzenleyicinize veya başka bir konuma hızlıca kopyalamak/yapıştırmak için, her bir anahtarın yanındaki Kopyala düğmesini seçin, pencereler arasında Pano içeriğini istenen konuma yapıştırın.

> [!IMPORTANT]
> Bu abonelik anahtarları bilişsel hizmet API 'nize erişmek için kullanılır. Anahtarlarınızı paylaşmayın. Bunları güvenli bir şekilde depolayın (örneğin, Azure Key Vault kullanarak). Ayrıca, bu anahtarların düzenli olarak yeniden oluşturulması önerilir. API çağrısı yapmak için yalnızca bir anahtar gereklidir. İlk anahtarı yeniden oluştururken, hizmete devam eden erişim için ikinci anahtarı kullanabilirsiniz.

## <a name="switch-to-a-new-subscription"></a>Yeni bir aboneliğe geçme

Bir abonelikten diğerine geçmek için örneğin, ücretsiz deneme süresinin sona erdiği veya uygulamanızı yayımladığınızda değiştirin kodunuzu bölge ve abonelik anahtarı ile yeni bir Azure kaynak bölge ve abonelik anahtarı.

## <a name="about-regions"></a>Bölgeler hakkında

- Uygulamanız kullanıyorsa bir [Speech SDK'sı](speech-sdk.md), bölge kodu gibi sağladığınız `westus`, konuşma yapılandırma oluştururken.
- Uygulamanızı konuşma hizmetin birini kullanıp kullanmadığını [REST API'leri](rest-apis.md), bölge uç noktası URI'si istekleri yaparken kullandığınız bir parçasıdır.
- Bir bölgede oluşturulan anahtarları yalnızca bu bölgede geçerlidir. Diğer bölgeler ile kullanılmaya çalışılırsa, kimlik doğrulama hataları neden olur.

## <a name="next-steps"></a>Sonraki adımlar

10 dakikalık başlangıçtan biriyle tamamlamak veya SDK örneklerimizi denetleyin:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: C# ' de Konuşma tanıma](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
> [Speech SDK'sı örnekleri](speech-sdk.md#get-the-samples)
