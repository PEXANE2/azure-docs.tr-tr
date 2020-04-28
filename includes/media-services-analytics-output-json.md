---
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 11/09/2018
ms.author: juliako
ms.openlocfilehash: 065cb4daa9501ee658d364dad43b9e03798e4083
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188785"
---
İş, algılanan ve izlenen yüzler hakkında meta veriler içeren bir JSON çıkış dosyası oluşturur. Meta veriler, yüzlerin konumunu gösteren koordinatları ve bu kişinin izlenmesini belirten bir yüz KIMLIK numarası içerir. Yüz KIMLIĞI numaraları, en alt yüz kayıp veya çerçevede örtüşdüğünde, bazı kişilerin birden çok kimliğe atanabileceği durumlar altında sıfırlanır.

JSON çıktısı aşağıdaki öğeleri içerir:

### <a name="root-json-elements"></a>Kök JSON öğeleri

| Öğe | Açıklama |
| --- | --- |
| version |Bu, video API 'sinin sürümünü ifade eder. |
| zaman |Videonun saniye başına "ticks" değeri. |
| uzaklık |Bu, zaman damgaları için zaman kaymadır. Video API 'Lerinin 1,0 sürümünde bu her zaman 0 olur. Daha sonra destekduğumuz senaryolarda bu değer değişebilir. |
| Genişlik, daha fazla |Çıkış video çerçevesinin piksel cinsinden genişliği ve uzunluğu.|
| kare hızı değerini |Videodaki saniye başına kare hızı. |
| [Eşleştir](#fragments-json-elements) |Meta veriler parçalar adlı farklı kesimlere bölünmüştür. Her parçada başlangıç, süre, aralık sayısı ve olaylar vardır. |

### <a name="fragments-json-elements"></a>Parçacık JSON öğeleri

|Öğe|Açıklama|
|---|---|
| start |"Ticks" içindeki ilk olayın başlangıç saati. |
| süre |Parçanın uzunluğu, "ticks." |
| dizin | (Yalnızca Azure Media Redactor için geçerlidir) geçerli olayın çerçeve dizinini tanımlar. |
| interval |"Ticks" içinde, parça içindeki her bir olay girişinin aralığı. |
| etkinlikler |Her olay, algılanan ve bu süre içinde izlenen yüzeyleri içerir. Bu, bir dizi olaydır. Dış dizi bir zaman aralığını temsil eder. İç dizi belirtilen noktada gerçekleşen 0 veya daha fazla olaydan oluşur. Boş köşeli ayraç [] hiçbir yüz algılanmadığı anlamına gelir. |
| id |İzlenmekte olan yüzün KIMLIĞI. Bir yüz algılanmadan bu numara istenmeden değişebilir. Belirli bir bireyin, genel videonun tamamında aynı KIMLIĞE sahip olmalıdır, ancak algılama algoritmasındaki sınırlamalar (occlusiyon vb.) nedeniyle bu garanti edilemez. |
| x, y |0,0 ile 1,0 arasında normalleştirilmiş ölçekte yüz sınırlayıcı kutusunun sol üst X ve Y koordinatları. <br/>-X ve Y koordinatları yatay olarak her zaman görelidir. bu nedenle, dikey bir videonuz (veya iOS söz konusu olduğunda) varsa koordinatları uygun şekilde yeniden ayarlamanız gerekir. |
| Genişlik, yükseklik |0,0 ile 1,0 arasında normalleştirilmiş ölçekte yüz sınırlayıcı kutunun genişliği ve yüksekliği. |
| Facesalgılandı |Bu, JSON sonuçlarının sonunda bulunur ve bu, video sırasında algoritmanın algıladığı yüzlerin sayısını özetler. Bir yüz algılanmadığı durumlarda kimlikler yanlışlıkla sıfırlanabileceğinden (ör. yüz ekran, uzaklaşma), bu numara her zaman videodaki gerçek sayıda yüzdeke eşit olamaz. |
