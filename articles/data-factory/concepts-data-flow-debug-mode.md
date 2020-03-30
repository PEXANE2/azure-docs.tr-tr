---
title: Veri akışı Hata Ayıklama Modu'nu eşleme
description: Veri akışları inşa ederken etkileşimli hata ayıklama oturumu başlatma
ms.author: makromer
author: kromerm
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/06/2019
ms.openlocfilehash: 18848695327a374f12fbe5a34d03366b050d8b65
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74928356"
---
# <a name="mapping-data-flow-debug-mode"></a>Veri akışı Hata Ayıklama Modu'nu eşleme

## <a name="overview"></a>Genel Bakış

Azure Veri Fabrikası veri akışının hata ayıklama modu, veri akışlarınızı oluştururken ve hata ayıklarken veri şeklinin dönüşümünüzü etkileşimli olarak izlemenize olanak tanır. Hata ayıklama oturumu hem Veri Akışı tasarım oturumlarında hem de veri akışlarının ardışık defveri ayıklama yürütülmesi sırasında kullanılabilir. Hata ayıklama modunu açmak için tasarım yüzeyinin üst kısmındaki "Veri Akışı Hata Ayıklama" düğmesini kullanın.

![Hata ayıklama kaydırıcısı](media/data-flow/debugbutton.png "Hata ayıklama kaydırıcısı")

Kaydırıcıyı açtıktan sonra, kullanmak istediğiniz tümleştirme çalışma zamanı yapılandırmasını seçmeniz istenir. AutoResolveIntegrationRuntime seçilirse, 60 dakikalık bir süre yaşamak için genel hesaplama sekiz çekirdekli bir küme döndürülür. Veri akışı tümleştirme çalışma süreleri hakkında daha fazla bilgi için [Bkz. Veri akışı performansı.](concepts-data-flow-performance.md#increasing-compute-size-in-azure-integration-runtime)

![Hata Ayıklama IR seçimi](media/data-flow/debugbutton2.png "Hata Ayıklama IR seçimi")

Hata Ayıklama modu açıkken, etkin bir Spark kümesiyle veri akışınızı etkileşimli olarak oluşturursunuz. Azure Veri Fabrikası'nda hata ayıklamayı kapattığınızda oturum kapanır. Hata ayıklama oturumuaçık olduğu süre boyunca Azure Databricks tarafından yapılan saatlik ücretlerden haberdar olmalısınız.

Çoğu durumda, çalışmanızı Azure Veri Fabrikası'nda yayınlamadan önce iş mantığınızı doğrulayabilmeniz ve veri dönüşümlerinizi görüntüleyebilmeniz için Veri Akışlarınızı hata ayıklama modunda oluşturmak iyi bir uygulamadır. Veri akışınızı bir ardışık ardışık alanda test etmek için boru hattı panelindeki "Hata Ayıklama" düğmesini kullanın.

## <a name="cluster-status"></a>Küme durumu

Tasarım yüzeyinin üst kısmındaki küme durum göstergesi, küme hata ayıklama için hazır olduğunda yeşile döner. Kümeniz zaten sıcaksa, yeşil gösterge neredeyse anında görünür. Hata ayıklama moduna girdiğinizde kümeniz zaten çalışmıyorsa, kümenin dönmesi için 5-7 dakika beklemeniz gerekir. Gösterge hazır olana kadar dönecektir.

Hata ayıklama işiniz bittiğinde, Hata Ayıklama düğmesini kapatın, böylece Azure Veri Tuğlaları kümeniz sonlandırılabilir ve hata ayıklama etkinliği için faturalandırılmanıza neden olun.

## <a name="debug-settings"></a>Hata ayıklama ayarları

Hata ayıklama ayarları, Veri Akışı tuvalaraç çubuğundaki "Hata Ayıklama Ayarları"na tıklayarak düzenlenebilir. Kaynak dönüşümlerinizin her biri için kullanılacak satır sınırını veya dosya kaynağını buradan seçebilirsiniz. Bu ayarda satır sınırları yalnızca geçerli hata ayıklama oturumu içindir. Bir SQL DW kaynağı için kullanılacak hazırlama bağlantılı hizmeti de seçebilirsiniz. 

![Hata ayıklama ayarları](media/data-flow/debug-settings.png "Hata ayıklama ayarları")

Veri Akışınızda veya başvurulan veri kümelerinden herhangi birinde parametreler varsa, **Parametreler** sekmesini seçerek hata ayıklama sırasında hangi değerlerin kullanılacağını belirtebilirsiniz.

![Hata ayıklama ayarları parametreleri](media/data-flow/debug-settings2.png "Hata ayıklama ayarları parametreleri")

## <a name="data-preview"></a>Veri önizlemesi

Hata ayıklama açıkken, Veri Önizleme sekmesi alt taki panelde yanar. Hata ayıklama modu olmadan, Veri Akışı, Denetim sekmesindeki dönüşümlerinizin her birine yalnızca geçerli meta verileri gösterir. Veri önizlemesi yalnızca hata ayıklama ayarlarınızda sınırınız olarak ayarladığınız satır sayısını sorgular. Veri önizlemesini almak için **Yenile'yi** tıklatın.

![Veri önizleme](media/data-flow/datapreview.png "Veri önizlemesi")

> [!NOTE]
> Dosya kaynakları yalnızca okuduğunuz satırları değil, gördüğünüz satırları sınırlar. Çok büyük veri kümeleri için, bu dosyanın küçük bir bölümünü almanız ve sınama nız için kullanmanız önerilir. Dosya veri kümesi türü olan her kaynak için Hata Ayıklama Ayarları'nda geçici bir dosya seçebilirsiniz.

Veri Akışında Hata Ayıklama Modunda çalışırken, verileriniz Lavabo dönüşümüne yazılmayacaktır. Hata Ayıklama oturumu, dönüşümleriniz için bir test donanımı görevi görmek üzere tasarlanmıştır. Hata ayıklama sırasında lavabolar gerekli değildir ve veri akışınızda yoksayılır. Verileri Lavabonuzda yazmayı test etmek istiyorsanız, Bir Azure Veri Fabrikası Ardışık Durumundan Veri Akışını çalıştırın ve bir ardışık kaynaktan Hata Ayıklama yürütmesini kullanın.

### <a name="testing-join-conditions"></a>Test birleştirme koşulları

Birim testi Joins, Exists veya Lookup dönüşümleri olduğunda, testiniz için bilinen küçük bir veri kümesi kullandığınızdan emin olun. Testiniz için kullanılacak geçici bir dosya ayarlamak için yukarıdaki Hata Ayıklama Ayarları seçeneğini kullanabilirsiniz. Bu gereklidir, çünkü satırları büyük bir veri kümesinden sınırlandırırken veya örnekleme yaparken, hangi satırların ve hangi tuşların sınama için akışa okunacağını tahmin edemezsiniz. Sonuç belirleyici değildir, yani birleştirme koşullarınız başarısız olabilir.

### <a name="quick-actions"></a>Hızlı eylemler

Veri önizlemesini gördüğünüzde, sütunda dakti-yazar olarak, kaldırmak ta veya değişiklik yapmak için hızlı bir dönüşüm oluşturabilirsiniz. Sütun üstbilgisini tıklatın ve ardından veri önizleme araç çubuğundan seçeneklerden birini seçin.

![Hızlı eylemler](media/data-flow/quick-actions1.png "Hızlı eylemler")

Bir değişiklik seçtiğinizde, veri önizlemesi hemen yenilenir. Yeni bir dönüşüm oluşturmak için sağ üst köşede **Onayla'yı** tıklatın.

![Hızlı eylemler](media/data-flow/quick-actions2.png "Hızlı eylemler")

**Typecast** ve **Değiştir** türetilmiş sütun dönüşümü oluşturur ve **Kaldır** bir Select dönüşümü oluşturur.

![Hızlı eylemler](media/data-flow/quick-actions3.png "Hızlı eylemler")

> [!NOTE]
> Veri Akışınızı yeniden leştirirseniz, hızlı bir dönüşüm eklemeden önce veri önizlemesini yeniden getirmeniz gerekir.

### <a name="data-profiling"></a>Veri profilleme

Veri önizleme sekmenizde bir sütun **Statistics** seçmek ve veri önizleme araç çubuğundaki İstatistikler'i tıklatmak, her alanla ilgili ayrıntılı istatistikleriçeren veri ızgaranızın en sağında bir grafik açılır. Azure Veri Fabrikası, hangi grafik türünün görüntülenenene ilişkin veri örneklemesine dayalı bir belirleme yapar. Yüksek önem düzeyi alanları null/NOT NULL grafikleri varsayılan olurken, düşük önem ediliğe sahip kategorik ve sayısal veriler veri değeri sıklığını gösteren çubuk grafikleri görüntüler. Ayrıca dize alanlarının max/len uzunluğunu, sayısal alanlardaki min/max değerlerini, standart dev'i, yüzdelik artışlarını, sayımları ve ortalamayı görürsünüz.

![Sütun istatistikleri](media/data-flow/stats.png "Sütun istatistikleri")

## <a name="next-steps"></a>Sonraki adımlar

* Veri akışınızı oluşturmayı ve hata ayıklamayı bitirdikten sonra, [bir ardışık kaynaktan çalıştırın.](control-flow-execute-data-flow-activity.md)
* Ardışık ardışık hattınızı bir veri akışıyla sınarken, ardışık hatlar hata [ayıklama yürütme seçeneğini kullanın.](iterative-development-debugging.md)
