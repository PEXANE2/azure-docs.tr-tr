---
title: Özel komutlar uygulaması yazarken hata ayıklama hataları (Önizleme)
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlar uygulaması yazarken hata ayıklamanın nasıl yapılacağını öğreneceksiniz.
services: cognitive-services
author: xiaojul
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: xiaojul
ms.openlocfilehash: b0a26b4d3f0f59f8e05c5990bbc64ee55f12f124
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307925"
---
# <a name="debug-errors-when-authoring-a-custom-commands-application"></a>Özel bir komut uygulaması yazarken hata ayıklama hataları

Bu makalede, özel komutlar uygulaması oluştururken hata ayıkladığınızda hata ayıklamanın nasıl yapılacağı açıklanır. 

## <a name="errors-when-creating-an-application"></a>Uygulama oluştururken hatalar
Özel komutlar Ayrıca, özel komutlar uygulaması oluştururken [Luo](https://www.luis.ai/) 'da bir uygulama oluşturur. 

[Lusıs, yazma kaynağı başına 500 uygulama kısıtlar](https://docs.microsoft.com/azure/cognitive-services/luis/luis-limits). Zaten 500 uygulamasına sahip bir yazma kaynağı kullanıyorsanız, LUSıS uygulamasının oluşturulması başarısız olabilir. 

Seçili LUSıS yazma kaynağının 500 ' den küçük olduğundan emin olun. Aksi takdirde, yeni bir LUSıS yazma kaynağı oluşturabilir, başka birine geçebilir veya LUSıS uygulamalarınızı temizlemeyi deneyebilirsiniz.  

## <a name="errors-when-deleting-an-application"></a>Uygulama silinirken hatalar oluştu
### <a name="cant-delete-luis-application"></a>LUSıS uygulaması silinemiyor
Özel komutlar uygulaması silinirken, özel komutlar özel komutlar uygulamasıyla ilişkili LUO uygulamasını silmeye de çalışabilir.

LUSıS uygulamasını silme işlemi başarısız olduysa, lütfen el ile silmek için [lusıs](https://www.luis.ai/) hesabınıza gidin.

### <a name="toomanyrequests"></a>TooManyRequests
Tek seferde büyük miktarda uygulamayı silmeye çalıştığınızda, muhtemelen ' TooManyRequests ' hataları görebilirsiniz. Bu, silme isteklerinizin Azure tarafından kısıtlandığı anlamına gelir. 

Lütfen sayfanızı yenileyin ve daha az uygulamayı silmeyi deneyin.

## <a name="errors-when-modifying-an-application"></a>Bir uygulamayı değiştirirken oluşan hatalar

### <a name="cant-delete-a-parameter-or-a-web-endpoint"></a>Bir parametre veya Web uç noktası silinemiyor
Kullanılırken bir parametreyi silmeye izin verilmiyor. Lütfen herhangi bir konuşma yanıtlarındaki parametre başvurusunu, örnek cümleleri, koşulları ve eylemleri kaldırın ve yeniden deneyin.

### <a name="cant-delete-a-web-endpoint"></a>Web uç noktası silinemiyor
Kullanılırken bir Web uç noktası silme izniniz yok. Lütfen bir Web uç noktası kaldırılmadan önce bu Web uç noktasını kullanan tüm **çağrı Web uç noktası** eylemlerini kaldırın.

## <a name="errors-when-training-an-application"></a>Bir uygulamayı eğitmek için hatalar
### <a name="build-in-intents"></a>Amaçlar halinde derleme
LUSıS 'in derleme Evet/Hayır amacı vardır. Yalnızca "Yes", "Hayır" ile örnek cümleler olması eğitimin başarısız olmasına neden olur. 

| Sözcükle | Farklılıklar | 
| ------- | --------- | 
| Yes | Emin olun, tamam |
| No | Nope, değil | 

### <a name="common-sample-sentences"></a>Ortak örnek cümleler
Özel komutlar, farklı komutlar arasında paylaşılan ortak örnek cümlelere izin vermez. Bir komutta bazı örnek cümleler zaten başka bir komutta tanımlanmışsa, bir uygulamanın eğitimi başarısız olabilir. 

Lütfen farklı komutlar arasında paylaşılan ortak örnek cümleler olmadığından emin olun. 

Örnek cümlelerinizi farklı komutlarda dengelemenin en iyi uygulaması için lütfen [lusıs en iyi uygulaması](https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-best-practices)' na bakın.

### <a name="empty-sample-sentences"></a>Boş örnek cümleler
Her komut için en az 1 örnek cümle olması gerekir.

### <a name="undefined-parameter-in-sample-sentences"></a>Örnek tümcelerde Tanımsız parametre
Örnek cümlelerde bir veya daha fazla parametre kullanıldı, ancak tanımlanmamış.

### <a name="training-takes-too-long"></a>Eğitim çok uzun sürüyor
LUSıS eğitiminin daha az örnekle hızla öğrenme amacı vardır. Çok sayıda örnek tümce eklemeyin. 

Birçok örnek cümle varsa, bir parametre tanımlayın, bunları bir düzende soyut yapın ve örnek cümlelere ekleyin.

Örneğin, aşağıdaki örnek tümceler için {araç} parametresini tanımlayabilir ve yalnızca "Book a {araç}" öğesini örnek cümlelere ekleyebilirsiniz.

| Örnek cümleler | Desen | 
| ------- | ------- | 
| Araba kitabı | Bir {araç} kitabı | 
| Uçuş kitabı | Bir {araç} kitabı |
| Bir TAXI defteri | Bir {araç} kitabı |

LUSıS eğitiminin en iyi uygulaması için lütfen [lubu en iyi uygulama](https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-best-practices)bölümüne bakın.

## <a name="cant-update-luis-key"></a>LUSıS anahtarı güncelleştirilemiyor
### <a name="reassign-to-e0-authoring-resource"></a>E0 Authoring Resource öğesine yeniden ata
LUSıS, E0 yazma kaynağına LUSıS uygulamasının atanmasını desteklemez.

Authoring Resource F0 ' den E0 ' ye değiştirmeniz veya farklı bir E0 kaynağına değiştirmeniz gerekiyorsa lütfen uygulamayı yeniden oluşturun.

### <a name="save-button-is-disabled"></a>Kaydet düğmesi devre dışı
Uygulamanıza hiçbir zaman bir LUSıS tahmin kaynağı atamadıysanız, bir tahmin kaynağı eklemeden yazma kaynağınızı değiştirmeye çalıştığınızda Kaydet düğmesi devre dışı bırakılır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da örneklere bakın](https://aka.ms/speech/cc-samples)
