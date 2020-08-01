---
title: Veri akışlarını eşleme
description: Azure Data Factory veri akışlarını eşleştirmeye genel bakış
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/09/2020
ms.openlocfilehash: 850879675d4554329f24c86f2ac28660b303084c
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475575"
---
# <a name="what-are-mapping-data-flows"></a>Veri akışlarını eşleme nedir?

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Veri akışlarını eşleme, Azure Data Factory ' de görsel olarak tasarlanan veri dönüştürmelerdir. Veri akışları, veri mühendislerinin kod yazmadan grafik veri dönüştürme mantığı geliştirmesini sağlar. Elde edilen veri akışları, ölçeği genişletilmiş Apache Spark kümelerini kullanan Azure Data Factory işlem hatları içinde etkinlik olarak yürütülür. Veri akışı etkinlikleri, mevcut Data Factory zamanlama, denetim, akış ve izleme özellikleri aracılığıyla erişebilir.

Veri akışlarını eşleme, hiçbir kodlamaya gerek olmadan tamamen görsel bir deneyim sağlar. Veri akışlarınız, ölçeği genişletilmiş veri işleme için yürütme kümenizde çalışır. Azure Data Factory, tüm kod çevirisi, yol iyileştirmesi ve veri akışı işlerinizin yürütülmesini işler.

![Mimari](media/data-flow/adf-data-flows.png "Mimari")

## <a name="getting-started"></a>Başlarken

Bir veri akışı oluşturmak için, **fabrika kaynakları**altındaki artı işaretini ve ardından **veri akışı**' nı seçin. 

![Yeni veri akışı](media/data-flow/newdataflow2.png "Yeni veri akışı")

Bu eylem sizi, dönüşüm mantığınızı oluşturabileceğiniz veri akışı tuvaline götürür. Kaynak dönüştürmeyi yapılandırmaya başlamak için **Kaynak Ekle** ' yi seçin. Daha fazla bilgi için bkz. [kaynak dönüştürme](data-flow-source.md).

## <a name="data-flow-canvas"></a>Veri akışı tuvali

Veri akışı tuvali üç parçaya ayrılmıştır: üst çubuk, grafik ve yapılandırma paneli. 

![Tuval](media/data-flow/canvas1.png "Tuval")

### <a name="graph"></a>Graf

Grafik, dönüşüm akışını görüntüler. Bir veya daha fazla havuza akan kaynak verilerinin kökenini gösterir. Yeni bir kaynak eklemek için **Kaynak Ekle**' yi seçin. Yeni bir dönüşüm eklemek için, varolan bir dönüşümün sağ alt köşesindeki artı işaretini seçin.

![Tuval](media/data-flow/canvas2.png "Tuval")

### <a name="azure-integration-runtime-data-flow-properties"></a>Azure tümleştirme çalışma zamanı veri akışı özellikleri

![Hata ayıklama düğmesi](media/data-flow/debugbutton.png "Hata ayıklama düğmesi")

ADF 'de veri akışları ile çalışmaya başladığınızda, tarayıcı kullanıcı arabiriminin en üstündeki veri akışları için "hata ayıklama" anahtarını açmak istersiniz. Bu, etkileşimli hata ayıklama, veri önizlemeleri ve ardışık düzen hata ayıklama yürütmeleri için kullanılacak bir Spark kümesi sağlar. Kullanılan kümenin boyutunu özel bir [Azure Integration Runtime](concepts-integration-runtime.md)seçerek ayarlayabilirsiniz. Hata ayıklama oturumu, son veri önizlemeniz veya son hata ayıklama işlem hattı yürütmesinden sonra 60 dakika boyunca etkin kalır.

Veri akışı etkinlikleriyle işlem hatlarınızı çalıştırdığınızda ADF, "Çalıştır" özelliğindeki [etkinlikle](control-flow-execute-data-flow-activity.md) ilişkili Azure Integration Runtime kullanır.

Varsayılan Azure Integration Runtime, verileri önizlemenize ve hata ayıklama işlem hatlarını en az maliyetle hızlı bir şekilde yürütebilmenizi sağlayan küçük 4 çekirdekli tek çalışan düğümü kümesidir. Büyük veri kümelerine yönelik işlemler gerçekleştiriyorsanız daha büyük bir Azure IR yapılandırma ayarlayın.

Azure IR veri akışı özelliklerinde bir TTL ayarlayarak, ADF 'yi bir küme kaynakları havuzunu (VM 'Ler) tutmaya söyleyebilirsiniz. Bu eylem, sonraki etkinliklerde daha hızlı iş yürütmeye neden olur.

#### <a name="azure-integration-runtime-and-data-flow-strategies"></a>Azure tümleştirme çalışma zamanı ve veri akışı stratejileri

##### <a name="execute-data-flows-in-parallel"></a>Veri akışlarını paralel olarak yürütme

Bir işlem hattındaki veri akışlarını paralel olarak çalıştırırsanız, ADF her bir etkinliğe bağlı Azure Integration Runtime ayarlara bağlı olarak her etkinlik yürütmesi için ayrı Spark kümeleri alır. ADF işlem hatlarında paralel yürütmeler tasarlamak için, veri akışı etkinliklerinizi Kullanıcı arabiriminde öncelik kısıtlamaları olmadan ekleyin.

Bu üç seçenekten Bu seçenek büyük olasılıkla en kısa sürede yürütülür. Ancak, her paralel veri akışı ayrı kümelerde aynı anda yürütülür, bu nedenle olayların sıralaması belirleyici değildir.

Veri akışı etkinliklerinizi işlem hatlarınız içinde paralel olarak yürütüyorsunuz, TTL kullanılması önerilmez. Bu eylem, veri akışınız için Azure Integration Runtime aynı anda paralel yürütmelerin, Data Factory 'niz için birden çok ısınma havuzu örneği ile sonuçlanmasına neden olur.

##### <a name="overload-single-data-flow"></a>Tek veri akışını aşırı yükleme

Tüm mantığınızı tek bir veri akışı içinde yerleştirirseniz, ADF aynı iş yürütme bağlamını tek bir Spark küme örneği üzerinde yürütür.

İş kurallarınız ve iş mantığınızın bir araya gelmemesi nedeniyle, bu seçeneğin izlenmesi ve sorun gidermesi daha zor olabilir. Bu seçenek ayrıca daha fazla yeniden kullanılabilirlik sağlamaz.

##### <a name="execute-data-flows-sequentially"></a>Veri akışlarını ardışık olarak Yürüt

Veri akışı etkinliklerinizi ardışık düzende yürütüşuyorsanız ve Azure IR yapılandırmasında bir TTL belirlediyseniz, ADF daha hızlı yürütme süreleri elde eden işlem kaynaklarını (VM 'Ler) yeniden kullanacaktır. Her yürütme için yeni bir Spark bağlamı almaya devam edersiniz.

Bu üç seçenekten, bu eylem büyük olasılıkla uçtan uca yürütülmeye en uzun zaman alır. Ancak, her bir veri akışı adımında mantıksal işlemlerin temiz bir şekilde ayrılmasını sağlar.

### <a name="configuration-panel"></a>Yapılandırma bölmesi

Yapılandırma panelinde seçili olan dönüştürmeye özgü ayarlar gösterilir. Hiçbir dönüşüm seçilmezse, veri akışını gösterir. Genel veri akışı yapılandırmasında, **genel** sekmesinin altındaki adı ve açıklamayı düzenleyebilir veya **Parametreler** sekmesi aracılığıyla parametreler ekleyebilirsiniz. Daha fazla bilgi için bkz. [veri akışı parametrelerini eşleme](parameters-data-flow.md).

Her dönüşümde en az dört Yapılandırma sekmesi bulunur.

#### <a name="transformation-settings"></a>Dönüştürme ayarları

Her bir dönüşümün yapılandırma bölmesindeki ilk sekme, bu dönüştürmeye özgü ayarları içerir. Daha fazla bilgi için bkz. bu dönüşümün belge sayfası.

![Kaynak ayarları sekmesi](media/data-flow/source1.png "Kaynak ayarları sekmesi")

#### <a name="optimize"></a>İyileştirme

**Optimizasyon** sekmesi, bölümleme düzenlerini yapılandırma ayarlarını içerir. Veri akışlarınızı iyileştirme hakkında daha fazla bilgi edinmek için bkz. [eşleme veri akışı performans Kılavuzu](concepts-data-flow-performance.md).

![İyileştirme](media/data-flow/optimize.png "İyileştirme")

#### <a name="inspect"></a>Bilgiyi

**İnceleme** sekmesi, dönüştürmakta olduğunuz veri akışının meta verilerine bir görünüm sağlar. Sütun sayılarını, değişen sütunları, eklenen sütunları, veri türlerini, sütun sırasını ve sütun başvurularını görebilirsiniz. **İnceleme** , meta verilerinizin salt okunurdur görünümüdür. **İnceleme** bölmesinde meta verileri görmek için hata ayıklama modunun etkin olması gerekmez.

![Bilgiyi](media/data-flow/inspect1.png "Bilgiyi")

Dönüşümlerinizi kullanarak verilerinizin şeklini değiştirirken, **İnceleme** bölmesinde meta veri değişiklikleri akışını görürsünüz. Kaynak dönüşümünüze tanımlı bir şema yoksa, veriler **İnceleme** bölmesinde görünmez. Şema DRFT senaryolarında meta verilerin bulunmaması yaygındır.

#### <a name="data-preview"></a>Veri önizlemesi

Hata ayıklama modu açık ise, **veri önizleme** sekmesi her dönüşümde verilerin etkileşimli bir anlık görüntüsünü sunar. Daha fazla bilgi için bkz. [hata ayıklama modunda veri önizlemesi](concepts-data-flow-debug-mode.md#data-preview).

### <a name="top-bar"></a>Üst çubuk

Üst çubuk, kaydetme ve doğrulama gibi tüm veri akışını etkileyen eylemler içerir. Grafiği **göster** ve **grafiği gizle** düğmelerini kullanarak grafik ve yapılandırma modları arasında da geçiş yapabilirsiniz.

![Grafiği gizle](media/data-flow/hideg.png "Grafiği gizle")

Grafınızı gizlerseniz, **önceki** ve **sonraki** düğmelerini kullanarak dönüştürme düğümleriniz arasında daha da gezinebilirsiniz.

![Önceki ve sonraki düğmeler](media/data-flow/showhide.png "önceki ve sonraki düğmeler")

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak dönüştürme](data-flow-source.md)oluşturmayı öğrenin.
* Veri akışlarınızı [hata ayıklama modunda](concepts-data-flow-debug-mode.md)oluşturmayı öğrenin.
