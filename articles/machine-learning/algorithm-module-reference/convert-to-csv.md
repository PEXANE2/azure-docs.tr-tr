---
title: "CSV'ye dönüştürün: Modül Başvurusu"
titleSuffix: Azure Machine Learning
description: Bir veri kümesini İndirilebilen, dışa aktarılabilen veya R veya Python komut dosyası modülleriyle paylaşılabilen bir CSV biçimine dönüştürmek için Azure Machine Learning'de CSV'ye Dönüştür modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: fc2a043e8f1565cf5fe45ba0b072ad015076635e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477689"
---
# <a name="convert-to-csv-module"></a>CSV modülüne dönüştürün

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Bir veri kümesini, R veya Python komut dosyası modülleriyle indirilebilen, dışa aktarılabilen veya paylaşılabilen bir CSV biçimine dönüştürmek için bu modülü kullanın.

### <a name="more-about-the-csv-format"></a>CSV formatı hakkında daha fazla şey 

"Virgülden ayrılmış değerler" anlamına gelen CSV biçimi, birçok harici makine öğrenme aracı tarafından kullanılan bir dosya biçimidir. CSV, R veya Python gibi açık kaynak dilleriyle çalışırken ortak bir değişim biçimidir.

Çalışmalarınızın çoğunu Azure Machine Learning'de yapsanız bile, veri kümenizi harici araçlarda kullanmak üzere CSV'ye dönüştürmeyi kullanışlı bulabileceğiniz zamanlar vardır. Örnek:

+ Excel ile açmak için CSV dosyasını indirin veya ilişkisel bir veritabanına aktarın.  
+ CSV dosyasını bulut depolamaalanına kaydedin ve görselleştirmeler oluşturmak için Power BI'den dosyaya bağlanın.  
+ Verileri R ve Python'da kullanılmak üzere hazırlamak için CSV biçimini kullanın. 

Bir veri kümesini CSV'ye dönüştürdüğünüzde, csv Azure ML çalışma alanınızda kaydedilir. Dosyayı doğrudan açmak ve kullanmak için bir Azure depolama yardımcı programı kullanabilirsiniz. Ayrıca CSV modülüne Dönüştür'ü seçerek tasarımcıdaki **CSV'ye** erişebilir, ardından çıktıyı görüntülemek için sağ paneldeki **Çıktılar** sekmesinin altındaki histogram simgesini seçebilirsiniz. CSV'yi Sonuçlar klasöründen yerel bir dizine indirebilirsiniz.  

## <a name="how-to-configure-convert-to-csv"></a>Convert'i CSV'ye yapılandırma


1.  Convert to CSV modüllerini boru hattınıza ekleyin. Bu modülü tasarımcıda **Veri Dönüşümü** grubunda bulabilirsiniz. 

2. Veri kümesini oluşturan herhangi bir modüle bağlayın.   
  
3.  Boru hattını gönderin.

### <a name="results"></a>Sonuçlar
  

**CSV'ye Dönüştür'ün**sağ panelindeki **Çıktılar** sekmesini seçin ve Bağlantı **Noktası çıkışlarının**altındaki bu simgelerden birini seçin.  

+ **Veri kümesini kaydet**: Simgeyi seçin ve CSV dosyasını ayrı bir veri kümesi olarak Azure ML çalışma alanına kaydedin. Veri kümesini Modül ağacı olarak Benim Veri **Kümeleri mitlerinin** altında bulabilirsiniz.

 + **Çıktıyı Görüntüle**: Göz simgesini seçin ve **Results_dataset** klasörüne göz atmak için yönergeyi izleyin ve data.csv dosyasını indirin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 