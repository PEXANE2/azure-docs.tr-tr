---
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 11/09/2018
ms.author: juliako
ms.openlocfilehash: 065cb4daa9501ee658d364dad43b9e03798e4083
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188785"
---
İş, algılanan ve izlenen yüzler hakkında meta veriler içeren bir JSON çıktı dosyası üretir. Meta veriler, yüzlerin konumunu gösteren koordinatların yanı sıra o kişinin izini gösteren bir yüz kimliği numarasını içerir. Face ID numaraları, ön yüzün kaybolduğu veya çerçevede çakıştıldığında olduğu durumlarda sıfırlanma yatkınlığı yla karşı karşıya dır ve bu da bazı kişilerin birden fazla kimlik atanmasına neden olur.

JSON çıktısı aşağıdaki öğeleri içerir:

### <a name="root-json-elements"></a>Kök JSON elemanları

| Öğe | Açıklama |
| --- | --- |
| version |Bu, Video API sürümüanlamına gelir. |
| Zaman ölçeği |Videonun saniyesinde "Keneler". |
| uzaklık |Bu zaman damgaları için zaman mahsup edilir. Video API'lerinin sürüm 1.0'ında bu her zaman 0 olacaktır. Desteklediğimiz gelecek senaryolarda bu değer değişebilir. |
| genişlik, yükseklik |Çıkış video çerçevesinin piksel genişliği ve en yüksek katı.|
| Framerate |Videodaki saniye başına kare hızı. |
| [Parça](#fragments-json-elements) |Meta veriler, parça adı verilen farklı bölümlere ayrılır. Her parçada başlangıç, süre, aralık sayısı ve olaylar vardır. |

### <a name="fragments-json-elements"></a>Parçaları JSON elemanları

|Öğe|Açıklama|
|---|---|
| start |"Keneler"deki ilk olayın başlangıç saati. |
| süre |Parçanın uzunluğu, "keneler"de. |
| dizin | (Yalnızca Azure Media Redactor için geçerlidir) geçerli olayın kare dizini tanımlar. |
| interval |Parça içindeki her olay girişinin aralığı , "kene" olarak adlandırılır. |
| etkinlikler |Her olay, bu süre içinde algılanan ve izlenen yüzleri içerir. Bu bir dizi olay. Dış dizi bir zaman aralığını temsil eder. İç dizi belirtilen noktada gerçekleşen 0 veya daha fazla olaydan oluşur. Boş bir parantez [] hiçbir yüz algılanmadı anlamına gelir. |
| id |İzlenen yüzün kimliği. Bir yüz fark edilmediğinde bu sayı yanlışlıkla değişebilir. Belirli bir birey genel video boyunca aynı kİmliğe sahip olmalıdır, ancak algılama algoritmasındaki sınırlamalar (oklüzyon, vb.) nedeniyle bu garanti edilemez. |
| x, y |0.0 ile 1.0 arasında normalleştirilmiş bir ölçekte yüz sınırlayıcı kutunun sol üst X ve Y koordinatları. <br/>-X ve Y koordinatları her zaman manzaraya göredir, bu nedenle bir portre videonuz varsa (veya iOS durumunda baş aşağı), koordinatları buna göre aktarmanız gerekir. |
| genişlik, yükseklik |0,0 ile 1,0 arasında normalleştirilmiş bir ölçekte yüz sınırlayıcı kutunun genişliği ve yüksekliği. |
| facesAlgılanan |Bu, JSON sonuçlarının sonunda bulunur ve algoritmanın video sırasında algılayan yüz sayısını özetler. Bir yüz fark edilmezse (örn. yüz ekrandan çıkarsa, uzağa bakarsa) yanlışlıkla sıfırlanabildiği için, bu sayı her zaman videodaki gerçek yüz sayısına eşit olmayabilir. |
