---
title: Azure Data Factory veri akışlarını eşleme | Microsoft Docs
description: Azure Data Factory veri akışlarını eşleştirmeye genel bakış
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/7/2019
ms.openlocfilehash: 8d0ad794caee8a06c8d403a981037d6560fb3f43
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72030180"
---
# <a name="what-are-mapping-data-flows"></a>Veri Akışlarını Eşleme nedir?

Veri akışlarını eşleme, Azure Data Factory ' de görsel olarak tasarlanan veri dönüştürmelerdir. Veri akışları, veri mühendislerinin kod yazmadan grafik veri dönüştürme mantığı geliştirmesini sağlar. Elde edilen veri akışları, ölçeklendirilen Spark kümelerini kullanarak Azure Data Factory işlem hatları içinde etkinlik olarak yürütülür. Veri akışı etkinlikleri mevcut Data Factory zamanlama, denetim, akış ve izleme özellikleri aracılığıyla çalıştırılabilir.

Veri akışlarını eşleme, hiçbir kodlamaya gerek olmadan tam bir görsel deneyim sağlar. Veri akışlarınız, ölçeği genişletilmiş veri işleme için kendi yürütme kümenizde yürütülür. Azure Data Factory, tüm kod çevirisi, yol iyileştirmesi ve veri akışı işlerinizin yürütülmesini işler.

## <a name="getting-started"></a>Başlangıç

Bir veri akışı oluşturmak için, fabrika kaynakları altındaki artı işaretine tıklayın. 

![Yeni veri akışı](media/data-flow/newdataflow2.png "Yeni veri akışı")

Bu sizi, dönüşüm mantığınızı oluşturabileceğiniz veri akışı tuvaline götürür. Kaynak dönüşümünüzü yapılandırmaya başlamak için ' Kaynak Ekle ' kutusuna tıklayın. Daha fazla bilgi için bkz. [kaynak dönüştürme](data-flow-source.md).

## <a name="data-flow-canvas"></a>Veri akışı tuvali

Veri akışı tuvali üç parçaya ayrılmıştır: üst çubuk, grafik ve yapılandırma paneli. 

![Tuval](media/data-flow/canvas1.png "tuvali")

### <a name="graph"></a>Graph

Grafik, dönüşüm akışını görüntüler. Bir veya daha fazla havuza akan kaynak verilerinin kökenini gösterir. Yeni bir kaynak eklemek için ' Kaynak Ekle ' kutusuna tıklayın. Yeni bir dönüşüm eklemek için, var olan bir dönüşümün sağ alt kısmındaki artı işaretine tıklayın.

![Tuval](media/data-flow/canvas2.png "tuvali")

### <a name="configuration-panel"></a>Yapılandırma bölmesi

Yapılandırma panelinde, şu anda seçili olan dönüştürmeye özgü ayarlar gösterilir veya hiçbir dönüşüm seçilmezse, veri akışı. Genel veri akışı yapılandırmasında, **genel** sekmesinin altındaki adı ve açıklamayı düzenleyebilir veya **Parametreler** sekmesi aracılığıyla parametreler ekleyebilirsiniz. Daha fazla bilgi için bkz. [veri akışı parametrelerini eşleme](parameters-data-flow.md).

Her dönüşümde en az dört Yapılandırma sekmesi vardır:

#### <a name="transformation-settings"></a>Dönüştürme ayarları

Her bir dönüşümün yapılandırma bölmesindeki ilk sekme, bu dönüştürmeye özgü ayarları içerir. Daha fazla bilgi için lütfen bu dönüşümün belge sayfasına bakın.

![Kaynak ayarları sekmesi](media/data-flow/source1.png "kaynak ayarları sekmesi")

#### <a name="optimize"></a>En iyi duruma getirme

_Optimizasyon_ sekmesi, bölümleme düzenlerini yapılandırma ayarlarını içerir.

![](media/data-flow/optimize1.png "İyileştirmeden") iyileştirme

Varsayılan ayar, Spark üzerinde çalışan veri akışları için yerel bölümlendirme şemasını kullanmak Azure Data Factory bildiren "geçerli bölümleme kullan" dır. Çoğu senaryoda, bu ayar önerilen yaklaşımdır.

Bölümlemeyi ayarlamak isteyebileceğiniz örnekler vardır. Örneğin, dönüşümlerinizi Gölü tek bir dosyaya çıkarmak istiyorsanız, bir havuz dönüşümünde "tek bölüm" seçeneğini belirleyin.

Bölümleme düzenlerini denetlemek isteyebileceğiniz başka bir durum da performansı iyileştiriliyor. Bölümlemenin ayarlanması, verilerin işlem düğümleri genelinde dağıtılması üzerinde denetim sağlar ve genel veri akışı performansından hem olumlu hem de olumsuz etkileri olabilir. Daha fazla bilgi için bkz. [veri akışı performans Kılavuzu](concepts-data-flow-performance.md).

Herhangi bir dönüşümdeki Bölümlendirmeyi değiştirmek için, optimizasyon sekmesine tıklayın ve "bölümlendirme ayarla" radyo düğmesini seçin. Daha sonra bölümlemeye yönelik bir dizi seçenek sunulur. Bölümlemenin en iyi yöntemi, veri birimleriniz, aday anahtarlarınız, null değerleri ve kardinalite temelinde farklılık gösterir. En iyi uygulama, varsayılan bölümlendirme ile başlamalı ve farklı bölümlendirme seçeneklerini denemelerdir. İşlem hattı hata ayıklama çalıştırmalarını kullanarak test edebilir ve Izleme görünümündeki her bir dönüşüm gruplandırmasında yürütme süresini ve bölüm kullanımını görüntüleyebilirsiniz. Daha fazla bilgi için bkz. [veri akışlarını izleme](concepts-data-flow-monitoring.md).

Kullanılabilir bölümleme seçenekleri aşağıda verilmiştir.

##### <a name="round-robin"></a>Hepsini Bir Kez Deneme 

Hepsini bir kez deneme, verileri bölümler arasında eşit olarak dağıtan basit bir bölümdür. Katı ve akıllı bölümlendirme stratejisi uygulamak için iyi bir ana aday olmadığında hepsini bir kez deneme kullanın. Fiziksel bölüm sayısını ayarlayabilirsiniz.

##### <a name="hash"></a>Karma

Azure Data Factory, benzer değerlere sahip satırlar aynı bölüme düşecek şekilde Tekdüzen bölümler oluşturmak için bir sütun karması oluşturacak. Karma seçeneğini kullanırken, olası bölüm eğsınaması için test edin. Fiziksel bölüm sayısını ayarlayabilirsiniz.

##### <a name="dynamic-range"></a>Dinamik Aralık

Dinamik Aralık, sağladığınız sütunlara veya ifadelere göre Spark dinamik aralıklarını kullanacaktır. Fiziksel bölüm sayısını ayarlayabilirsiniz. 

##### <a name="fixed-range"></a>Sabit Aralık

Bölümlenmiş veri sütunlarınızın içindeki değerler için sabit bir Aralık sağlayan bir ifade oluşturun. Bölüm eğriliğini önlemek için bu seçeneği kullanmadan önce verilerinizin iyi şekilde anlaşılmasına sahip olmanız gerekir. İfade için girdiğiniz değerler, bölüm işlevinin bir parçası olarak kullanılır. Fiziksel bölüm sayısını ayarlayabilirsiniz.

##### <a name="key"></a>Anahtar

Verilerinizin önem düzeyini iyi anlamak istiyorsanız, anahtar bölümleme iyi bir bölüm stratejisi olabilir. Anahtar bölümleme, sütuninizdeki her benzersiz değer için bölümler oluşturacaktır. Bu sayı, verilerdeki benzersiz değerleri temel alacak olduğundan bölüm sayısını ayarlayamazsınız.

#### <a name="inspect"></a>Bilgiyi

_İnceleme_ sekmesi, dönüştürmakta olduğunuz veri akışının meta verilerine bir görünüm sağlar. Sütun sayılarını, sütun değiştirildiğini, eklenen sütunları, veri türlerini, sütun sıralamasını ve sütun başvurularını görebilirsiniz. İnceleme, meta verilerinizin salt okunurdur görünümüdür. Inceleme bölmesinde meta verileri görmek için hata ayıklama modunun etkin olması gerekmez.

![](media/data-flow/inspect1.png "İncelemeyi") İncele

Dönüşümlerinizi kullanarak verilerinizin şeklini değiştirirken, Inceleme bölmesi aracılığıyla meta veri değişiklikleri akışını görürsünüz. Kaynak dönüşümünüze tanımlı bir şema yoksa, veriler Inceleme bölmesinde görünmez. Şema DRFT senaryolarında meta verilerin bulunmaması yaygındır.

#### <a name="data-preview"></a>Veri önizleme

Hata ayıklama modu açık ise, _veri önizleme_ sekmesi her dönüşümde verilerin etkileşimli bir anlık görüntüsünü sunar. Daha fazla bilgi için bkz. [hata ayıklama modunda veri önizlemesi](concepts-data-flow-debug-mode.md#data-preview).

### <a name="top-bar"></a>Üst çubuk

Üst çubuk, kaydetme ve doğrulama gibi tüm veri akışını etkileyen eylemler içerir. Grafik ve yapılandırma modları arasında, **grafiği göster** ve **grafiği gizle** düğmelerini kullanarak da geçiş yapabilirsiniz.

![Grafik](media/data-flow/hideg.png "gizleme grafiği") gizle

Grafınızı gizlerseniz, **önceki** ve **sonraki** düğmelerini kullanarak dönüştürme düğümleriniz arasında geçiş yapabilirsiniz.

![](media/data-flow/showhide.png "Gezinme") git

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak dönüştürme](data-flow-source.md) oluşturmayı öğrenin
* Veri akışlarınızı [hata ayıklama modunda](concepts-data-flow-debug-mode.md) nasıl oluşturacağınızı öğrenin
