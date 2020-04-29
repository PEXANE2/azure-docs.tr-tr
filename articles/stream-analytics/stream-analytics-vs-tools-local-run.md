---
title: Visual Studio 'da yerel olarak Azure Stream Analytics sorguları test etme
description: Bu makalede, sorguların Visual Studio için Azure Stream Analytics araçları ile yerel olarak nasıl test edileceğini açıklar.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 07/10/2018
ms.openlocfilehash: 34c8555356d5c0142d7b677c8119fe66806ab064
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76834918"
---
# <a name="test-stream-analytics-queries-locally-with-visual-studio"></a>Visual Studio ile yerel olarak Stream Analytics sorguları test etme

Stream Analytics işlerinizi örnek verilerle veya [canlı verilerle](stream-analytics-live-data-local-testing.md)yerel olarak test etmek Için, Visual Studio için Azure Stream Analytics araçları 'nı kullanabilirsiniz. 

Visual Studio kullanarak Stream Analytics işi oluşturmayı öğrenmek için bu [hızlı](stream-analytics-quick-create-vs.md) başlangıcı kullanın.

## <a name="test-your-query"></a>Sorgunuzu test etme

Azure Stream Analytics projenizde betiği düzenleyicide açmak için **Script. asaql** öğesine çift tıklayın. Sözdizimi hataları olup olmadığını görmek için sorguyu derleyebilirsiniz. Sorgu Düzenleyicisi IntelliSense, söz dizimi renklendirmesi ve bir hata işaretleyicisi destekler.

![Sorgu düzenleyicisi](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="add-local-input"></a>Yerel giriş Ekle

Sorgunuzu yerel statik verilere karşı doğrulamak için, girişe sağ tıklayın ve **yerel giriş Ekle**' yi seçin.
   
![Yerel giriş Ekle](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-01.png)
   
Açılır pencerede, yerel yolunuzda örnek veriler ' i seçin ve **kaydedin**.
   
![Yerel giriş Ekle](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-02.png)
   
Giriş klasörünüze **local_EntryStream. JSON** adlı bir dosya otomatik olarak eklenir.
   
![Yerel giriş klasörü dosya listesi](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-03.png)
   
Sorgu Düzenleyicisi 'nde **yerel olarak çalıştır** ' ı seçin. İsterseniz F5 'e de basabilirsiniz.
   
![Yerel olarak çalıştır](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-01.png)
   
Çıktı, doğrudan Visual Studio 'dan bir tablo biçiminde görüntülenebilir.

![Tablo biçiminde çıkış](./media/stream-analytics-vs-tools-local-run/stream-analytics-for-vs-local-result.png)

Çıkış yolunu konsol çıktısından bulabilirsiniz. Sonuç klasörünü açmak için herhangi bir tuşa basın.
   
![Yerel çalıştırma](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-02.png)
   
Yerel klasördeki sonuçları denetleyin.
   
![Yerel klasör sonucu](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-03.png)
   

### <a name="sample-input"></a>Örnek giriş
Ayrıca, giriş kaynaklarınızdan yerel bir dosyaya örnek giriş verileri toplayabilirsiniz. Giriş yapılandırma dosyasına sağ tıklayın ve **örnek veriler**' i seçin. 

![Örnek Veri](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-01.png)

Yalnızca Event Hubs veya IoT Hub 'larından veri akışı örneği oluşturabilirsiniz. Diğer giriş kaynakları desteklenmez. Açılan iletişim kutusunda, örnek verileri kaydetmek için yerel yolu girin ve **örnek**' i seçin.

![Örnek veri yapılandırması](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-02.png)
 
İlerleme durumunu **Çıkış** penceresinde görebilirsiniz. 

![Örnek veri çıktısı](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-03.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Visual Studio 'Yu kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Azure Stream Analytics işleri görüntülemek için Visual Studio 'Yu kullanma](stream-analytics-vs-tools.md)
* [Visual Studio için Azure Stream Analytics araçları 'nı kullanarak canlı verileri yerel olarak test etme (Önizleme)](stream-analytics-live-data-local-testing.md)
* [Öğretici: Azure DevOps kullanarak CI/CD ile Azure Stream Analytics işi dağıtma](stream-analytics-tools-visual-studio-cicd-vsts.md)
* [Stream Analytics araçlarıyla sürekli tümleştirme ve geliştirme](stream-analytics-tools-for-visual-studio-cicd.md)
