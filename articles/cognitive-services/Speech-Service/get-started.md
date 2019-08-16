---
title: Konuşma hizmetini ücretsiz deneyin
titleSuffix: Azure Cognitive Services
description: Konuşma hizmetini kullanmaya başlamak kolay ve ekonomik bir işlemdir. 30 günlük ücretsiz deneme, olanak tanır. hangi hizmet yapmak ve uygulamanızın ihtiyaçları için doğru olup olmadığını karar verebilirsiniz keşfedin.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 41d12013ec7eaa4e2aae59e1b366cc511a41f749
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69535097"
---
# <a name="try-speech-services-for-free"></a>Konuşma Hizmetleri ücretsiz olarak deneyin

Konuşma hizmetlerini kullanmaya başlamak kolay ve ekonomik bir işlemdir. 30 günlük ücretsiz deneme, olanak tanır. hangi hizmet yapmak ve uygulamanızın ihtiyaçları için doğru olup olmadığını karar verebilirsiniz keşfedin.

Daha fazla zaman gerekiyorsa, bir Microsoft Azure hesabına kaydolun — hizmet kredisi, 30 güne kadar ücretli bir konuşma Hizmetleri aboneliğine uygulayabileceğiniz $200 ile gelir.

Son olarak, konuşma Hizmetleri, uygulama geliştirmeye uygun olan ücretsiz ve düşük hacimli bir katman sunar. Hizmet kredinizin süresi dolduktan sonra bile bu ücretsiz aboneliği tutabilirsiniz.

## <a name="free-trial"></a>Ücretsiz deneme sürümü

30 günlük ücretsiz deneme standart fiyatlandırma katmanı sınırlı bir süre için erişmenizi sağlar.

30 günlük ücretsiz deneme için kaydolmak için:

1. Git [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/).

1. Seçin **konuşma API'leri** sekmesi.

   ![Konuşma Hizmetleri sekmesi](media/index/try-speech-api-free-trial1.png)

1. **Konuşma Hizmetleri**altında, **API anahtarı al**' ı seçin.

   ![API anahtarı](media/index/try-speech-api-free-trial2.png)

1. Koşulları kabul edin ve aşağı açılan menüden bölgeniz seçin.

   ![Koşulları kabul edin](media/index/try-speech-api-free-trial3.png)

1. Microsoft, Facebook, LinkedIn ve GitHub hesabınızı kullanarak oturum açın.

    Ücretsiz bir Microsoft hesabı için kaydolabilirsiniz [Microsoft hesap portalı](https://account.microsoft.com/account). Başlamak için **Microsoft hesabıyla oturum açın** ' yi seçin ve ardından oturum açmanız istendiğinde oluştur ' u seçin **.** Oluşturun ve yeni Microsoft hesabınızı doğrulamak için adımları izleyin.

Bilişsel hizmetler için deneyin'da oturum açtıktan sonra ücretsiz deneme sürümünüzü başlar. Görüntülenen Web sayfasının şu anda deneme abonelikleri olan tüm Azure Bilişsel hizmetler hizmetler listelenir. İki abonelik anahtarı **konuşma Hizmetleri**' nin yanında listelenmiştir. İki anahtarı uygulamalarınızda kullanabilirsiniz.

> [!NOTE]
> Tüm ücretsiz deneme Abonelikleri, Batı ABD bölgesinde içindir. İstekte bulunduğunuzda kullandığınızdan emin olun `westus` uç noktası.

## <a name="new-azure-account"></a>Yeni Azure hesabı

Yeni Azure hesapları 30 gün boyunca kullanılabilir 200 ABD Doları değerinde bir hizmet kredisi alırsınız. Bu kredisi, konuşma hizmetlerini daha ayrıntılı incelemek veya uygulama geliştirmeye başlamak için kullanabilirsiniz.

Yeni bir Azure hesabına kaydolmak için [Azure kaydolma sayfasına](https://azure.microsoft.com/free/ai/)gidin, **ücretsiz Başlat ' ı** seçin ve Microsoft hesabı kullanarak yeni bir Azure hesabı oluşturun.

Ücretsiz bir Microsoft hesabı için kaydolabilirsiniz [Microsoft hesap portalı](https://account.microsoft.com/account). Başlamak için **Microsoft hesabıyla oturum açın** ' yi seçin ve ardından oturum açmanız istendiğinde oluştur ' u seçin **.** Oluşturun ve yeni Microsoft hesabınızı doğrulamak için adımları izleyin.

Azure hesabınızı oluşturduktan sonra, konuşma Hizmetleri için bir abonelik başlatmak üzere sonraki bölümdeki adımları izleyin.

## <a name="create-a-speech-resource-in-azure"></a>Azure'da bir konuşma kaynak oluşturma

Azure hesabınıza bir konuşma Hizmetleri kaynağı (ücretsiz veya ücretli katman) eklemek için:

1. Oturum [Azure portalında](https://portal.azure.com/) Microsoft hesabınızı kullanarak.

1. Seçin **kaynak Oluştur** , portalın sol üst köşesindeki.

    ![Kaynak oluşturma](media/index/try-speech-api-create-speech1.png)

1. İçinde **yeni** penceresinde, arama **konuşma**.

1. Arama sonuçlarında seçin **konuşma**.

    ![Konuşma seçin](media/index/try-speech-api-create-speech2.png)

1. Altında **konuşma**seçin **Oluştur** düğmesi.

    ![Oluştur düğmesine seçin](media/index/try-speech-api-create-speech3.png)

1. Altında **Oluştur**, girin:

   * Yeni kaynak için bir ad. Adı aynı hizmetin birden fazla aboneliğe arasında ayırt etmenize yardımcı olur.
   * Yeni kaynak ücretleri nasıl faturalandırılır belirlemek için ilişkili olduğu Azure aboneliğini seçin.
   * Kaynağın kullanılacağı [bölgeyi](regions.md) seçin.
   * Ya da ücretsiz veya Ücretli fiyatlandırma katmanı seçin. Her bir katmanın fiyatlandırma ve kullanım kotalarıyla ilgili tüm bilgiler için **tam fiyatlandırma ayrıntılarını görüntüle** ' yi seçin.
   * Bu konuşma abonelik için yeni bir kaynak grubu oluşturun veya mevcut bir kaynak grubu aboneliği atayın. Kaynak düzenlenir, çeşitli Azure aboneliklerinizi tutmak Yardım gruplandırır.
   * Aboneliğinize gelecekte kolay erişim için seçin **panoya Sabitle** onay kutusu.
   * Seçin **oluşturun.**

     ![Oluştur düğmesine seçin](media/index/try-speech-api-create-speech4.png)

     Oluşturmak ve yeni konuşma kaynağınızı dağıtmak için bir dakika sürer. Seçin **hızlı** yeni kaynağınız hakkındaki bilgileri görmek için.

     ![Hızlı Başlangıç paneli](media/index/try-speech-api-create-speech5.png)

1. **Hızlı başlangıç**altında, abonelik anahtarlarınızı göstermek için adım 1 ' ın altındaki **anahtarlar** bağlantısını seçin. Her aboneliğin, iki anahtarı vardır; uygulamanızda iki anahtarı kullanabilirsiniz. Kodunuzu yapıştırma panoya kopyalamak için her anahtar yanındaki düğmeyi seçin.

> [!NOTE]
> Bir veya birden çok bölgede standart katman abonelikleri sınırsız sayıda oluşturabilirsiniz. Ancak, yalnızca bir ücretsiz katman abonelik oluşturabilir. Ücretsiz katmanındaki 7 gün için kullanılmaması modeli dağıtımlarını otomatik olarak kullanımdan olacaktır.

## <a name="switch-to-a-new-subscription"></a>Yeni bir aboneliğe geçme

Bir abonelikten diğerine geçmek için örneğin, ücretsiz deneme süresinin sona erdiği veya uygulamanızı yayımladığınızda değiştirin kodunuzu bölge ve abonelik anahtarı ile yeni bir Azure kaynak bölge ve abonelik anahtarı.

> [!NOTE]
> Ücretsiz deneme anahtarlarının yer olan Batı ABD'de oluşturulur (`westus`) bölge. Dilerseniz Azure Panosu oluşturulan bir aboneliği bazı başka bir bölgede olabilir.

* Uygulamanız kullanıyorsa bir [Speech SDK'sı](speech-sdk.md), bölge kodu gibi sağladığınız `westus`, konuşma yapılandırma oluştururken.
* Uygulamanız, konuşma Hizmetleri 'nin [REST API 'lerinden](rest-apis.md)birini kullanıyorsa bölge, istek yaparken kullandığınız uç nokta URI 'sinin bir parçasıdır.

Bir bölgede oluşturulan anahtarları yalnızca bu bölgede geçerlidir. Diğer bölgeler ile kullanılmaya çalışılırsa, kimlik doğrulama hataları neden olur.

## <a name="next-steps"></a>Sonraki adımlar

10 dakikalık başlangıçtan biriyle tamamlamak veya SDK örneklerimizi denetleyin:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: C# ](quickstart-csharp-dotnet-windows.md) 
>  [Konuşma SDK örneklerindeki](speech-sdk.md#get-the-samples) konuşmayı tanıma
