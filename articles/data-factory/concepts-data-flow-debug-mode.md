---
title: Azure Data Factory eşleme veri akışı hata ayıklama modu
description: Veri akışları oluştururken etkileşimli bir hata ayıklama oturumu başlatın
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: 945d123c0901722a527e7cc8181c91f09e4e95ec
ms.sourcegitcommit: fe50db9c686d14eec75819f52a8e8d30d8ea725b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69014517"
---
# <a name="mapping-data-flow-debug-mode"></a>Eşleme veri akışı hata ayıklama modu

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

## <a name="overview"></a>Genel Bakış

Azure Data Factory eşleme veri akışının hata ayıklama modu, tasarım yüzeyinin en üstündeki "veri akışı hata ayıklaması" düğmesiyle birlikte değiştirilebilir. Veri akışları tasarlarken, hata ayıklama modunu açmak, veri akışlarınızı oluştururken ve hata ayıkladığınızda veri şekli dönüşümünü etkileşimli bir şekilde izlemenize olanak sağlar. Hata ayıklama oturumu hem veri akışı tasarım oturumlarında hem de veri akışlarının işlem hattı hata ayıklamada yürütülmesi sırasında kullanılabilir.

![Hata ayıklama düğmesi](media/data-flow/debugbutton.png "Hata ayıklama düğmesi")

Hata ayıklama modu açık olduğunda, etkin bir Spark kümesi ile veri akışınızı etkileşimli olarak oluşturacaksınız. Azure Data Factory, hata ayıklamayı kapattıktan sonra oturum kapatılacak. Hata ayıklama oturumunun açık olduğu süre boyunca Azure Databricks tarafından tahakkuk edilen saatlik ücretlerden haberdar olmanız gerekir.

Çoğu durumda, iş mantığınızı doğrulayabilmeniz ve işinizi Azure Data Factory yayımlamadan önce veri dönüştürmelerinizi görüntüleyebilmeniz için veri akışlarınızı hata ayıklama modunda oluşturmak iyi bir uygulamadır. Veri akışınızı bir ardışık düzende test etmek için işlem hattı panelinde "hata ayıkla" düğmesini kullanın.

> [!NOTE]
> Data Factory araç çubuğunda hata ayıklama modu ışığı yeşil olsa da, 60 dakikalık bir yaşam süresi ile 8 çekirdek/saat genel işlem için veri akışı hata ayıklama oranı üzerinden ücretlendirilirsiniz 

## <a name="cluster-status"></a>Küme durumu

Küme hata ayıklamaya hazırlanışında tasarım yüzeyinin en üstündeki küme durumu göstergesi yeşil olur. Kümeniz zaten ısınma olursa yeşil gösterge neredeyse anında görünür. Hata ayıklama moduna girdiğinizde kümeniz zaten çalışmıyorsa, kümenin dönmesi için 5-7 dakika beklemeniz gerekir. Gösterge, başlamaya kadar dönmesini sağlayacak.

Hata ayıklamanız ile işiniz bittiğinde, Azure Databricks kümenizin sonlanabilir olması için hata ayıklama anahtarını kapatın ve artık hata ayıklama etkinliği için faturalandırılırsınız.

## <a name="debug-settings"></a>Hata ayıklama ayarları

Hata ayıklama ayarları, veri akışı tuvali araç çubuğunda "hata ayıklama ayarları" seçeneğine tıklanarak düzenlenebilirler. Burada kaynak dönüşümlerinizin her biri için kullanılacak satır sınırını veya dosya kaynağını seçebilirsiniz. Bu ayarda bulunan satır limitleri yalnızca geçerli hata ayıklama oturumu içindir. Ayrıca, bir SQL DW kaynağı için kullanılacak hazırlama bağlantılı hizmetini de seçebilirsiniz. 

![Hata ayıklama ayarları](media/data-flow/debug-settings.png "Hata ayıklama ayarları")

Veri akışınızda veya başvurulan veri kümelerinde parametrelere sahipseniz, **Parametreler** sekmesini seçerek hata ayıklama sırasında kullanılacak değerleri belirtebilirsiniz.

![Hata ayıklama ayarları parametreleri](media/data-flow/debug-settings2.png "Hata ayıklama ayarları parametreleri")

## <a name="data-preview"></a>Veri önizleme

Hata ayıklama tarihinde, veri Önizleme sekmesi alt panelde açılır. Üzerinde hata ayıklama modu olmadan veri akışı, Inceleme sekmesindeki dönüştürmelerinizin her birinin içindeki ve çıkan yalnızca geçerli meta verileri gösterir. Veri önizleme, yalnızca hata ayıklama ayarlarınızda sınırınız olarak ayarlamış olduğunuz satır sayısını sorgular. Veri önizlemeyi getirmek için **Yenile** ' ye tıklayın.

![Veri önizleme](media/data-flow/datapreview.png "Veri önizleme")

Veri akışında hata ayıklama modunda çalışırken verileriniz havuz dönüşümüne yazılmaz. Bir hata ayıklama oturumu, dönüştürmelerinizi için bir test bandı işlevi sunacak şekilde tasarlanmıştır. Hata ayıklama sırasında havuzlar gerekli değildir ve veri akışınız içinde yok sayılır. Havuzınızdaki verilerin yazılmasını test etmek isterseniz, veri akışını bir Azure Data Factory işlem hattından yürütün ve bir işlem hattından hata ayıklama yürütmesini kullanın.

### <a name="quick-actions"></a>Hızlı eylemler

Veri önizlemesini görtikten sonra, bir sütunda tür atama, kaldırma veya değiştirme yapmak için hızlı bir dönüşüm oluşturabilirsiniz. Sütun başlığına tıklayın ve ardından veri önizleme araç çubuğundan seçeneklerden birini belirleyin.

![Hızlı eylemler](media/data-flow/quick-actions1.png "Hızlı eylemler")

Bir değişikliği seçtiğinizde, veri önizlemesi hemen yenilenir. Yeni bir dönüşüm oluşturmak için sağ üst köşedeki **Onayla** ' ya tıklayın.

![Hızlı eylemler](media/data-flow/quick-actions2.png "Hızlı eylemler")

**Tür dönüştürme** ve **değiştirme** türetilmiş bir sütun dönüştürmesi oluşturacak ve **Remove** bir SELECT dönüştürmesi oluşturacak.

![Hızlı eylemler](media/data-flow/quick-actions3.png "Hızlı eylemler")

> [!NOTE]
> Veri akışınızı düzenlerseniz hızlı bir dönüştürme eklemeden önce veri önizlemeyi yeniden almanız gerekir.

### <a name="data-profiling"></a>Veri profili oluşturma

Veri önizleme sekmesinizdeki sütunları seçmek ve veri önizleme araç çubuğunda **İstatistikler** ' i tıklatmak, her alanla ilgili ayrıntılı istatistiklerle veri kılavuzunuzun en sağında bir grafik açılır. Azure Data Factory görüntülenecek grafik türünün veri örneklemesi temelinde bir belirleme yapılır. Yüksek kardinalite alanları varsayılan olarak NULL/değil NULL grafiklerine göre NULL/olmayan grafik olarak değişir. Ayrıca, dize alanlarının en fazla/uzun uzunluğunu, sayısal alanlardaki minimum/maksimum değerleri, standart dev, yüzdebirlik değeri, sayımlar ve Ortalama değerlerini de görürsünüz.

![Sütun istatistikleri](media/data-flow/stats.png "Sütun istatistikleri")

## <a name="next-steps"></a>Sonraki adımlar

* Veri akışınızı oluşturup hata ayıkladıktan sonra [bir işlem hattınızdan yürütün.](control-flow-execute-data-flow-activity.md)
* İşlem hattınızı bir veri akışı ile sınarken, işlem hattı [hata ayıklama çalıştırma yürütme seçeneğini kullanın.](iterative-development-debugging.md)
