---
title: Visual Studio'da Azure Akış Analizi sorgularını yerel olarak test edin
description: Bu makalede, Visual Studio için Azure Akış Analizi Araçları ile sorguların yerel olarak nasıl test edilecek olduğu açıklanmaktadır.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 07/10/2018
ms.openlocfilehash: 34c8555356d5c0142d7b677c8119fe66806ab064
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76834918"
---
# <a name="test-stream-analytics-queries-locally-with-visual-studio"></a>Visual Studio ile Test Stream Analytics sorgularını yerel olarak sorgulayın

Visual Studio için Azure Akış Analizi araçlarını kullanarak Stream Analytics işlerinizi örnek verilerle veya [canlı verilerle](stream-analytics-live-data-local-testing.md)yerel olarak test edebilirsiniz. 

Visual Studio'u kullanarak Bir Stream Analytics işi oluşturmayı öğrenmek için bu [Quickstart'ı](stream-analytics-quick-create-vs.md) kullanın.

## <a name="test-your-query"></a>Sorgunuzu test etme

Azure Akışı Analizi projenizde, komut dosyasını düzenleyicide açmak için **Script.asaql'ı** çift tıklatın. Sözdizimi hataları olup olmadığını görmek için sorguyu derleyebilirsiniz. Sorgu düzenleyicisi IntelliSense, sözdizimi boyama ve bir hata işaretçisi destekler.

![Sorgu düzenleyicisi](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="add-local-input"></a>Yerel giriş ekleme

Sorgunuzu yerel statik verilere göre doğrulamak için girişi sağ tıklatın ve **yerel giriş ekle'yi**seçin.
   
![Yerel giriş ekleme](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-01.png)
   
Açılan pencerede, yerel yolunızdan örnek verileri seçin ve **Kaydet.**
   
![Yerel giriş ekleme](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-02.png)
   
giriş klasörünüze otomatik olarak **local_EntryStream.json** adlı bir dosya eklenir.
   
![Yerel giriş klasörü dosya listesi](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-add-local-input-03.png)
   
Sorgu düzenleyicisinde **Yerel Olarak Çalıştır'ı** seçin. Ya da F5 tuşuna basabilirsiniz.
   
![Yerel Olarak Çalıştır](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-01.png)
   
Çıktı doğrudan Visual Studio'dan tablo biçiminde görüntülenebilir.

![Tablo biçiminde çıktı](./media/stream-analytics-vs-tools-local-run/stream-analytics-for-vs-local-result.png)

Çıkış yolunu konsol çıkışından bulabilirsiniz. Sonuç klasörünü açmak için herhangi bir tuşa basın.
   
![Yerel çalıştırma](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-02.png)
   
Sonuçları yerel klasörde denetleyin.
   
![Yerel klasör sonucu](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-local-run-03.png)
   

### <a name="sample-input"></a>Örnek giriş
Ayrıca, giriş kaynaklarınızdan yerel bir dosyaya örnek giriş verileri de toplayabilirsiniz. Giriş yapılandırma dosyasına sağ tıklayın ve **Örnek Verileri**seçin. 

![Örnek Veri](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-01.png)

Yalnızca Olay Hub'larından veya IoT Hub'larından veri akışı örneği alabilirsiniz. Diğer giriş kaynakları desteklenmez. Açılan iletişim kutusunda, örnek verileri kaydetmek için yerel yolu doldurun ve **Örnek'i**seçin.

![Örnek veri yapılandırması](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-02.png)
 
İlerlemeyi **Çıkış** penceresinde görebilirsiniz. 

![Örnek veri çıktısı](./media/stream-analytics-vs-tools-local-run/stream-analytics-tools-for-vs-sample-data-03.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Quickstart: Visual Studio'u kullanarak Bir Akış Analizi işi oluşturun](stream-analytics-quick-create-vs.md)
* [Azure Akış Analizi işlerini görüntülemek için Visual Studio'yı kullanın](stream-analytics-vs-tools.md)
* [Visual Studio için Azure Akış Analizi araçlarını kullanarak canlı verileri yerel olarak test edin (Önizleme)](stream-analytics-live-data-local-testing.md)
* [Öğretici: Azure DevOps'leri kullanarak CI/CD ile Azure Akışı Analizi işini dağıtma](stream-analytics-tools-visual-studio-cicd-vsts.md)
* [Stream Analytics araçlarıyla sürekli tümleştirme ve geliştirme](stream-analytics-tools-for-visual-studio-cicd.md)
