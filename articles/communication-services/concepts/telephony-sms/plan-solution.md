---
title: Azure Iletişim Hizmetleri telefon ve SMS çözümünüzü planlayın
titleSuffix: An Azure Communication Services concept document
description: Telefon numaraları ve telefon kullanım alanınızı etkin bir şekilde nasıl planlayacağınızı öğrenin.
author: stkozak
manager: rampras
services: azure-communication-services
ms.author: stkozak
ms.date: 06/23/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 39f88ab8b735438f60d8e20513ea5cbda43d41ee
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90947923"
---
# <a name="plan-your-telephony-and-sms-solution"></a>Telefon ve SMS çözümünüzü planlayın

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

Bu belgede, Azure Iletişim hizmetlerinin sunabileceği çeşitli telefon planları ve sayı türleri açıklanmaktadır. Iletişim Hizmetleri aracılığıyla kullanılabilir hale getirilen bir ses hizmeti sağlayıcısı, telefon numarası türleri, planlar ve yetenekler seçmenize yardımcı olmak için karar akışlarına kılavuzluk ederiz.

## <a name="about-phone-numbers-in-azure-communications-services"></a>Azure Iletişim hizmetlerinde telefon numaraları hakkında

Azure Iletişim Hizmetleri telefon görüşmeleri ve SMS iletileri yerleştirmek ve almak için telefon numaralarını kullanmanıza olanak sağlar. Bu telefon numaraları, hizmetiniz tarafından verilen giden aramalarda arayan KIMLIĞINI yapılandırmak için kullanılabilir.
  
Iletişim Hizmetleri çözümünüze içeri aktarmak için mevcut bir telefon numaranız yoksa, başlamak için en kolay yol, birkaç dakika içinde Azure Iletişim hizmetlerinden yeni bir telefon numarası edinmenin bir yoludur.

Çözümünüzde kullanmaya devam etmek istediğiniz bir telefon numaranız varsa (örneğin, 1-800 – ŞIRKET), mevcut sağlayıcıdaki telefon numarasını Iletişim hizmetlerine gönderebilirsiniz.

Aşağıdaki diyagram, kullanılabilir seçeneklerde gezinmenize yardımcı olur:

:::image type="content" source="../media/plan-solution/decision-tree-basic.png" alt-text="Telefon numaralarınıza ilişkin bir kararın nasıl yapılacağını gösteren diyagram.":::

Şimdi, Iletişim Hizmetleri aracılığıyla kullanılabilen telefon numarası türlerini ve yeteneklerini gözden geçirelim. 

## <a name="microsoft-direct-offer-of-phone-numbers-and-capabilities"></a>Microsoft doğrudan telefon numarası ve özellik teklifi

Azure Iletişim Hizmetleri, geliştiriciler için mükemmel esneklik sağlar. Çoğu telefon numarası için "a la carte" plan kümesini yapılandırmanıza izin veririz. Bazı geliştiricilerin yalnızca bir gelen çağrı planına ihtiyacı vardır; Bazıları gelen çağrı ve giden SMS planlarını kabul edebilir. Bu planlar, Iletişim hizmetlerinde telefon numaralarınızı kira ve/veya bağlantı noktası olarak seçilebilir.

Kullanılabilir planlar, içinde çalıştırdığınız ülke ve telefon numarası türüne bağlıdır. Aşağıdaki diyagram karar akışını temsil eder: kullanılabilir planlar, içinde çalıştırdığınız ülke ve telefon numarası türüne bağlıdır.

<!-- Tami/team have rejected this multiple times despite updates, says it needs to be higher res - need to work with her to get approval for this image. Commenting out to move our staging forward. :::image type="content" source="../../media/example-decision-flow.png" alt-text="Example for the decision flow"::: -->

Telefon numarası türünü seçmeden önce uluslararası telefon numaralandırma planını gözden geçirelim.

### <a name="optional-reading-international-public-telecommunication-numbering-plan-e164"></a>İsteğe bağlı okuma. Uluslararası ortak iletişim numaralandırma planı (E. 164)

> [!NOTE]
> Azure Iletişim Hizmetleri doğrudan teklifi tarafından sunulan numara türlerini ve yeteneklerini daha iyi anlamak için E. 164 telefon numaralandırma planına alışkın olsanız bile bu bilgilerin gözden geçirilmesini öneririz.

Uluslararası ortak iletişim numaralandırma planı, International Telekomünikasyon Birliği (ITU) önerisi E. 164 içinde tanımlanmıştır. Uyumlu sayılar en fazla 15 basamakla sınırlıdır.

Telefon numarası şunlardan oluşur

-   Ön ek "+"
-   Uluslararası bir arama ön eki veya ülke/bölge kodu (bir, iki veya üç basamak) 
-   *(Isteğe bağlı)* Genellikle alan kodu olarak adlandırılan ulusal bir hedef kodu veya numaralandırma planı. Bu kodun uzunluğu ülkeye göre değişir. Birleşik Devletler, üç haneye sahiptir. Avustralya ve Yeni Zelanda 'da tek bir rakam vardır. Almanya, Japonya, Meksika ve diğer ülkelerde alan kodlarının değişken uzunlukları vardır. Örneğin, Almanya 'da, alan kodu iki ila beş basamaktan herhangi bir yerde olabilir, ancak Japonya 'da, bir ila beş basamağa kadar herhangi bir yerde olabilir.
-   Abone numarası

> [!NOTE]
> Yukarıdaki sınıflandırma, ITU E. 164 standardına tam olarak uyumlu değildir ve basitleştirilmiş bir açıklama sağlamaya yöneliktir. Örneğin, abone numarası standart içinde alt bölünür. Uluslararası numaralandırma planı hakkında daha fazla bilgi edinmek istiyorsanız, [ITU e. 164 Standard](https://www.itu.int/rec/T-REC-E.164) , başlamak için mükemmel bir yerdir.  

Numaralandırma planını daha iyi anlamanıza yardımcı olacak bazı örnekler aşağıda verilmiştir:

ABD 'de bölgesel telefon numarası:

:::image type="content" source="../media/plan-solution/regional-us.png" alt-text="ABD 'de bölgesel telefon numarası örneği":::

Kanada 'da bölgesel telefon numarası:

:::image type="content" source="../media/plan-solution/regional-canada.png" alt-text="Kanada 'da bölgesel telefon numarası örneği":::

Kuzey Amerika bölgesindeki ücretsiz bir sayı:

:::image type="content" source="../media/plan-solution/tollfree-us.png" alt-text="Kuzey Amerika ücretsiz telefon numarası örneği":::

UK 'teki cep telefonu numarası:

:::image type="content" source="../media/plan-solution/mobile-uk.png" alt-text="UK 'teki cep telefonu numarası örneği":::

Daha sonra, Azure Iletişim hizmetlerinde bulunan belirli telefon numarası türlerini gözden geçirelim.

## <a name="phone-number-types-in-azure-communication-services"></a>Azure Iletişim hizmetlerindeki telefon numarası türleri

Microsoft, uygun olduğunda ülke içi kısa kodlar ve cep telefonu numaraları için bölgesel ve ücretsiz planlar sunar.

Aşağıdaki tabloda bu telefon numarası türleri özetlenmektedir: 

| Telefon numarası türü | Örnek                              | Ülke kullanılabilirliği    | Yaygın kullanım durumu                                                                                                     |
| ----------------- | ------------------------------------ | ----------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Bölgesel          | + 1 (coğrafi alan kodu) XXX XX XX  | ABD, Kanada, Porto Riko | Uygulamalarınızda kullanıcılara telefon numarası atama veya etkileşimli sesli yanıt (ıVR) sistemleri/bota atama |
| Ücretsiz         | + 1 (ücretsiz alan *kodu*) xxx xx xx | ABD, Kanada, Porto Riko | Etkileşimli sesli yanıt (ıVR) sistemleri/botlar, SMS uygulamalarına atama                                        |

## <a name="plans"></a>Planlar 

Telefon numaralarınız için etkinleştirebilecek yeteneklere göz atalım. Bu yetenekler, yasal gereksinimler nedeniyle ülkeye göre farklılık gösterir. Azure Iletişim Hizmetleri aşağıdaki özellikleri sunar:

- **Tek yönlü gıden SMS**, bildirim ve iki öğeli kimlik doğrulama senaryoları için kullanışlıdır.
- **İki yönlü gelen ve gıden SMS**, SMS 'nin bir planın parçası olarak gönderilmesini ve alınmasını sağlayan önceden tanımlanmış bir paket.
- **PSTN çağrısı**, bir gelen çağırma seçebilir ve giden çağrıları yerleştirmek IÇIN çağıran kimliğini kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="quickstarts"></a>Hızlı Başlangıçlar

- [Telefon numarası alın](../../quickstarts/telephony-sms/get-phone-number.md)
- [Bir çağrı yerleştir](../../quickstarts/voice-video-calling/calling-client-samples.md)
- [SMS gönderme](../../quickstarts/telephony-sms/send.md)

### <a name="conceptual-documentation"></a>Kavramsal belgeler

- [Ses ve video kavramları](../voice-video-calling/about-call-types.md)
- [Çağrı akışları ve SMS akışları](../call-flows.md)
- [Fiyatlandırma](../pricing.md)
