---
title: Araştırma-kişiselleştirici
titleSuffix: Azure Cognitive Services
description: Araştırmayla, kişiselleştirmede Kullanıcı davranışı değiştikçe bile iyi sonuçlar sunmaya devam edebiliyor. Bir araştırma ayarı seçmek, modeli geliştirmek için ile araştırılacak kullanıcı etkileşimlerinin oranı hakkında bir iş karardır.
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 10/23/2019
ms.openlocfilehash: 00b9cc881d9414e957d7503bb6156095bb821c06
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87128157"
---
# <a name="exploration-and-exploitation"></a>Araştırma ve yararlanma

Araştırmayla, kişiselleştirmede Kullanıcı davranışı değiştikçe bile iyi sonuçlar sunmaya devam edebiliyor.

Kişiselleştirici bir derecelendirme çağrısı aldığında, bir Rewarterctionıd döndürür:
* , Geçerli makine öğrenimi modeline bağlı olarak en olası kullanıcı davranışını eşleştirmek için yararlanma kullanır.
* En yüksek olasılığa sahip eylemle eşleşmeyen araştırma kullanır.

Kişiselleştirici Şu anda, araştırmak için *Epsilon doyumsuz* adlı bir algoritma kullanıyor. 

## <a name="choosing-an-exploration-setting"></a>Araştırma ayarı seçme

Kişiselleştirici için Azure portal **yapılandırma** sayfasında araştırma için kullanılacak trafik yüzdesini yapılandırırsınız. Bu ayar, araştırmayı gerçekleştiren derece çağrılarının yüzdesini belirler. 

Kişiselleştirici, her bir derecelendirme çağrısında bu olasılığa göz atın veya bu olasılığa karşı faydalanıp yararlanamaz. Bu, belirli kullanıcı kimlikleri üzerinde bir işlemi kilitleyen bazı A/B çerçevelerinden farklı bir davranıştır.

## <a name="best-practices-for-choosing-an-exploration-setting"></a>Araştırma ayarı seçmek için en iyi uygulamalar

Bir araştırma ayarı seçmek, modeli geliştirmek için ile araştırılacak kullanıcı etkileşimlerinin oranı hakkında bir iş karardır. 

Sıfır ayarı, Kişiselleştiriciye ait avantajların çoğunu Ifade eder. Bu ayar ile, kişiselleştirici daha iyi kullanıcı etkileşimleri bulma için Kullanıcı etkileşimi kullanmaz. Bu, stagof, Drın ve son olarak daha düşük performans modeli oluşturulmasına yol açar.

Çok yüksek bir ayar, Kullanıcı davranışından öğrenmesinin avantajlarından yararlanır. %100 olarak ayarlandığında, sabit bir rastgele seçim yapılır ve kullanıcıların öğrenildiği tüm davranışlar sonucu etkilemez.

Kişiselleştirmenin araştırıp mi yararlandığına göre uygulama davranışını değiştirmemelidir. Bu, son olarak olası performansı azaltabilecek bir şekilde öğrenmeye yol açacaktı.

## <a name="next-steps"></a>Sonraki adımlar

[Pekiştirmeye dayalı öğrenme](concepts-reinforcement-learning.md) 