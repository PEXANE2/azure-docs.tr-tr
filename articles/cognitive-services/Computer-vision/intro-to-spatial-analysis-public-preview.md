---
title: Uzamsal Analize genel bakış
titleSuffix: Azure Cognitive Services
description: Bu belgede Görüntü İşleme uzamsal analiz kapsayıcısının temel kavramları ve özellikleri açıklanmaktadır.
services: cognitive-services
author: nitinme
manager: nitinme
ms.author: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/01/2021
ms.openlocfilehash: ad05dd59c925242baf5c2b0e36c1f51bc4fec5d4
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100575360"
---
# <a name="overview-of-computer-vision-spatial-analysis"></a>Görüntü İşleme uzamsal analizine genel bakış

Görüntü İşleme uzamsal analizler, kuruluşların belirli bir alanda bulunan hareketleri ve iletişim durumunu anlamak için fiziksel alanlarının değerlerini en üst düzeye çıkarmasına yardımcı olan Azure bilişsel hizmetler 'in yeni bir özelliğidir Görüntü İşleme. CCTV veya gözetim kameralarından video almanıza, video akışlarından Öngörüler çıkarmak için AI işlemlerini çalıştırmanıza ve diğer sistemler tarafından kullanılacak olayları oluşturmanıza olanak tanır. Bir kamera akışından gelen girişler sayesinde bir AI işlemi, yüz maskesi ve sosyal distancing yönergeleri ile bir boşluk veya ölçüm uyumluluğu girme gibi şeyler yapabilir.

## <a name="the-basics-of-spatial-analysis"></a>Uzamsal analizin temelleri

Günümüzde uzamsal çözümlemenin temel işlemleri, video al, videodaki kişileri algılayan, kişileri bir süre içinde hareket ettikleri sırada izleyen bir işlem hattı üzerine oluşturulmuştur ve insanlar ilgi alanları ile etkileşime geçtiğinde olaylar oluşturur.

## <a name="spatial-analysis-terms"></a>Uzamsal analiz koşulları

| Süre | Tanım |
|------|------------|
| İnsanların algılanması | Bu bileşen "Bu görüntüdeki kişiler nerede?" sorusunu yanıtlar. Bir görüntüde insanlar bulur ve kişilerin izleme bileşenine her birinin konumunu gösteren bir sınırlayıcı kutu geçirir. |
| Kişi Izleme | Bu bileşen, insanların bir kameranın önünde dolaştığından zaman içinde algılamaları bağlar. Bu, insanların genellikle kişilerin bu şekilde yaptığı genel görünüşler hakkında bilgi ve temel bilgiler hakkında zamana bağlı mantık kullanır. Birden çok kamera genelinde kişileri izlemez. Bir kişi, bir kameradan yaklaşık bir dakikadan uzun bir süre boyunca görünüm alanı içeriyorsa ve ardından kamera görünümünü yeniden girerse, sistem bunu yeni bir kişi olarak alır. İnsanlar Izleme, kameraları genelinde kişileri benzersiz bir şekilde tanımlamaz. Yüz tanıma veya yürüyüş izlemeyi kullanmaz. |
| Yüz maskesini algılama | Bu bileşen bir kişinin, kameranın görünüm alanındaki yüzünün konumunu algılar ve bir yüz maskesinin varlığını tanımlar. Bunu yapmak için, AI işlemi videonun görüntülerini tarar; bir yüz algılandığında hizmetin yüz etrafında bir sınırlayıcı kutu sağladığını burada bulabilirsiniz. Nesne algılama yeteneklerini kullanarak, sınırlayıcı kutudaki yüz maskeleri varlığını belirler. Yüz maskesini algılama, yüz özniteliklerini tahmin eden veya sınıflandırarak ya da yüz tanımayı gerçekleştirerek başka bir yüzden bir yüzü ayırt etme işlemini kapsamaz. |
| Ilgilendiğiniz bölge | Bu, yapılandırmanın bir parçası olarak giriş videosunda tanımlanan bir bölge veya satırdır. Bir kişi, videonun bölgesiyle etkileşime geçtiğinde sistem bir olay oluşturur. Örneğin, PersonCrossingLine işlemi için videoda bir satır tanımlanmıştır. Bir kişi bu satırı aştığında bir olay oluşturulur. |
| Olay | Bir olay, uzamsal çözümlemenin birincil çıktısından oluşur. Her işlem belirli bir olayı düzenli aralıklarla (örn.) yayar. dakikada bir kez veya belirli bir tetikleyici gerçekleştiğinde. Olay, giriş videosunda ne olduğu ile ilgili bilgiler içerir, ancak herhangi bir görüntü veya video içermez. Örneğin, PeopleCount işlemi, kişi sayısı (tetikleyici) veya her dakikada bir (düzenli olarak) her değiştiğinde güncelleştirilmiş sayımı içeren bir olay yayabilir. |

## <a name="responsible-use-of-spatial-analysis-technology"></a>Uzamsal analiz teknolojisinin sorumlu kullanımı

Uzamsal analiz teknolojisinin sorumlu olduğunu nasıl kullanacağınızı öğrenmek için bkz. [asetat notunun](/legal/cognitive-services/computer-vision/transparency-note-spatial-analysis?context=%2fazure%2fcognitive-services%2fComputer-vision%2fcontext%2fcontext). Microsoft 'un saydamlık notları, AI teknolojimizin nasıl çalıştığını anlamanıza yardımcı olmak üzere tasarlanmıştır. sistem sahipleri, bu durum sistem performansını ve davranışını etkilemesinin yanı sıra teknoloji, kişiler ve ortam dahil olmak üzere tüm sistem hakkında düşünmesinin önemini de kolaylaştırabilir.

## <a name="spatial-analysis-gating-for-public-preview"></a>Genel önizleme için uzamsal analiz geçişi

Uzamsal analizler için tasarlanan senaryolar için kullanıldığından emin olmak için, bu teknolojiyi bir uygulama işlemi aracılığıyla müşteriler için kullanılabilir hale getiririz. Uzamsal Analize erişim sağlamak için, çevrimiçi alma formumuzu doldurarak başlamanız gerekir. [Uygulamanızı buradan başlatın](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u).

Uzamsal analiz Genel önizlemesine erişim, Microsoft 'un bu geçişli önizleme sırasında sınırlı sayıda müşteriyi desteklemeye yönelik uygunluk ölçütlerinizle, dileyici sürecimize ve kullanılabilirliğine bağlı olarak Microsoft 'un kendi takdirine tabidir. Genel önizlemede, Microsoft ile önemli bir ilişkiye sahip olan müşterilere bakıyoruz, önerilen kullanım durumlarında bizimle çalışmaya ilgileniyor ve sorumlu AI taahhütleriyle birlikte tutulması gereken ek senaryolar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uzamsal analiz kapsayıcısı ile çalışmaya başlama](spatial-analysis-container.md)