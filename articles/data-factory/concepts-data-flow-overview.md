---
title: Veri akışlarını eşleme
description: Azure Veri Fabrikası'nda veri akışlarını eşleme genel bakış
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/28/2020
ms.openlocfilehash: 39d1f15b771168b618bfbc4951f2036a8b95b027
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418465"
---
# <a name="what-are-mapping-data-flows"></a>Veri akışlarını eşleme nedir?

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Veri akışlarını haritalama, Azure Veri Fabrikası'nda görsel olarak tasarlanmış veri dönüşümleridir. Veri akışları, veri mühendislerinin kod yazmadan grafiksel veri dönüştürme mantığı geliştirmelerine olanak sağlar. Ortaya çıkan veri akışları, ölçeklenmiş Apache Spark kümelerini kullanan Azure Veri Fabrikası ardışık hatlarındaki etkinlikler olarak yürütülür. Veri akışı etkinlikleri, mevcut Veri Fabrikası zamanlama, kontrol, akış ve izleme yetenekleri aracılığıyla devreye alınabilir.

Veri akışlarının eşlenmesi, kodlama gerektirmeden tamamen görsel bir deneyim sağlar. Veri akışlarınız, ölçeklenmiş veri işleme için yürütme kümenizde çalışır. Azure Veri Fabrikası, veri akışı işlerinizi tüm kod çevirisini, yol optimizasyonunu ve yürütülmesini işler.

## <a name="getting-started"></a>Başlarken

Veri akışı oluşturmak **için, Fabrika Kaynakları**altında artı işaretini seçin ve ardından **Veri Akışı'nı**seçin. 

![Yeni veri akışı](media/data-flow/newdataflow2.png "yeni veri akışı")

Bu eylem, dönüşüm mantığınızı oluşturabileceğiniz veri akışı tuvaline götürür. Kaynak dönüşümünüzü yapılandırmaya başlamak için **kaynak ekle'yi** seçin. Daha fazla bilgi için [Kaynak dönüşümüne](data-flow-source.md)bakın.

## <a name="data-flow-canvas"></a>Veri akışı tuvali

Veri akışı tuvali üç bölüme ayrılır: üst çubuk, grafik ve yapılandırma paneli. 

![Tuval](media/data-flow/canvas1.png "Tuval")

### <a name="graph"></a>Graf

Grafik dönüşüm akışını görüntüler. Bir veya daha fazla lavaboya akarken kaynak veri soyu gösterir. Yeni bir kaynak eklemek için **kaynak ekle'yi**seçin. Yeni bir dönüşüm eklemek için, varolan dönüşümün sağ alt ağındaki artı işaretini seçin.

![Tuval](media/data-flow/canvas2.png "Tuval")

### <a name="azure-integration-runtime-data-flow-properties"></a>Azure tümleştirme çalışma zamanı veri akış özellikleri

![Hata ayıklama düğmesi](media/data-flow/debugbutton.png "Hata ayıklama düğmesi")

ADF'deki veri akışlarıyla çalışmaya başladığınızda, tarayıcı web gücü üzerindeki veri aşışları için "Hata Ayıklama" anahtarını açmak istersiniz. Bu, etkileşimli hata ayıklama, veri önizlemeleri ve ardışık işlem ayıklama yürütmeleri için kullanılacak bir Spark kümesini döndürür. Özel bir [Azure Tümleştirme Çalışma Zamanı](concepts-integration-runtime.md)seçerek kullanılan kümenin boyutunu ayarlayabilirsiniz. Hata ayıklama oturumu, son veri önizlemenizden veya son hata ayıklama ardışık uygulamadan sonra 60 dakikaya kadar canlı kalır.

Veri akışı etkinlikleri ile ardışık hale getirirken, ADF "Açık Çalıştır" özelliğindeki [etkinlikle](control-flow-execute-data-flow-activity.md) ilişkili Azure Tümleştirme Çalışma Zamanını kullanır.

Varsayılan Azure Tümleştirme Çalışma Süresi, verileri önizlemenize ve hata ayıklama ardışık hatlarını en az maliyetlerle hızlı bir şekilde yürütmenize olanak tanıyan küçük bir 4 çekirdekli tek çalışan düğüm kümesidir. Büyük veri kümelerine karşı işlemler gerçekleştirin' i daha büyük bir Azure IR yapılandırması ayarlayın.

Azure IR veri akışı özelliklerinde bir TTL ayarlayarak ADF'ye küme kaynakları (VM) havuzunu korumasını talimatı verebilirsiniz. Bu eylem, sonraki etkinliklerde daha hızlı iş yürütme ile sonuçlanır.

#### <a name="azure-integration-runtime-and-data-flow-strategies"></a>Azure tümleştirme çalışma zamanı ve veri akışı stratejileri

##### <a name="execute-data-flows-in-parallel"></a>Veri akışlarını paralel olarak yürütme

Bir ardışık ardışık ardışık alanda veri akışlarını paralel olarak yürütürseniz, ADF her etkinlik için bağlı olan Azure Tümleştirme Çalışma Zamanı'nızdaki ayarlara bağlı olarak her etkinlik yürütmesi için ayrı Spark kümeleri oluşturur. ADF ardışık hatlarında paralel yürütmeler tasarlamak için, veri akışı etkinliklerinizi UI'de öncelik kısıtlaması olmadan ekleyin.

Bu üç seçenekten, bu seçenek büyük olasılıkla en kısa sürede yürütülür. Ancak, her paralel veri akışı ayrı kümelerde aynı anda yürütülür, bu nedenle olayların sıralanması deterministik değildir.

Veri akışı etkinliklerinizi ardışık hatlar içinde paralel olarak yürütüyorsanız, TTL kullanmamanız önerilir. Bu eylem, aynı Azure Tümleştirme Çalışma Zamanı'nı kullanarak aynı anda veri akışınızın paralel yürütülmesinin veri fabrikanız için birden çok sıcak havuz örneklerine yol açmasıdır.

##### <a name="overload-single-data-flow"></a>Tek veri akışını aşırı yükleme

Tüm mantığınızı tek bir veri akışının içine koyarsanız, ADF aynı iş yürütme bağlamını tek bir Spark küme örneğinde yürütür.

İş kurallarınız ve iş mantığınız birbirine karışabileceğinden, bu seçeneği nizlemesi ve sorun giderilmesi daha zor olabilir. Bu seçenek aynı zamanda çok yeniden kullanılabilirlik sağlamaz.

##### <a name="execute-data-flows-serially"></a>Veri akışlarını seri olarak yürütme

Veri akışı etkinliklerinizi ardışık işlem de seri olarak yürütürseniz ve Azure IR yapılandırmasında bir TTL ayarladıysanız, ADF bilgi işlem kaynaklarını (VM' ler) yeniden kullanır ve böylece sonraki yürütme süreleri daha hızlı olur. Her yürütme için hala yeni bir Kıvılcım bağlamı alırsınız.

Bu üç seçenekten, bu eylemin uçtan uca yürütülmesi en uzun zaman alır. Ancak, her veri akışı adımında mantıksal işlemlerin temiz bir ayrımını sağlar.

### <a name="configuration-panel"></a>Yapılandırma paneli

Yapılandırma paneli, seçili dönüşüme özgü ayarları gösterir. Dönüşüm seçili değilse, veri akışını gösterir. Genel veri akışı yapılandırmasında, **Genel** sekmesi altında adı ve açıklamayı düzenleme yapabilir veya **Parametreler** sekmesi aracılığıyla parametreler ekleyebilirsiniz. Daha fazla bilgi için [bkz.](parameters-data-flow.md)

Her dönüşüm en az dört yapılandırma sekmesi içerir.

#### <a name="transformation-settings"></a>Dönüşüm ayarları

Her dönüşümün yapılandırma bölmesindeki ilk sekme, bu dönüşüme özgü ayarları içerir. Daha fazla bilgi için dönüşümün dokümantasyon sayfasına bakın.

![Kaynak ayarları sekmesi](media/data-flow/source1.png "Kaynak ayarları sekmesi")

#### <a name="optimize"></a>İyileştirme

**Optimize sekmesi,** bölümleme düzenlerini yapılandırmak için ayarlar içerir.

![Optimize](media/data-flow/optimize1.png "İyileştirme")

Varsayılan ayar, Azure Veri Fabrikası'na Spark'ta çalışan veri akışlarına özgü bölümleme düzenini kullanmasını sağlayan **geçerli bölümleme**kullanın. Çoğu senaryoda, bu ayarı öneririz.

Bölümlemayı ayarlamak isteyebileceğiniz durumlar vardır. Örneğin, dönüşümlerinizi göldeki tek bir dosyaya çıktırmak istiyorsanız, lavabo dönüşümünde **Tek bölüm'ü** seçin.

Bölümleme düzenlerini denetlemek isteyebileceğiniz başka bir durum da performansı en iyi duruma getirmektir. Bölümlemenin ayarlanması, verilerinizin bilgi işlem düğümleri ve genel veri akışı performansınız üzerinde hem olumlu hem de olumsuz etkileri olabilecek veri yerelliği optimizasyonları arasında dağıtımı üzerinde denetim sağlar. Daha fazla bilgi için [Veri akışı performans kılavuzuna](concepts-data-flow-performance.md)bakın.

Herhangi bir dönüşümde bölümlemeyi değiştirmek için **Optimize sekmesini** seçin ve **Bölümleme Radyoyu Ayarla** düğmesini seçin. Bölümleme için bir dizi seçenek sunulur. En iyi bölümleme yöntemi, veri birimlerinize, aday anahtarlarınıza, null değerlerinize ve önemli ciddiyete bağlı olarak değişir. 

En iyi yöntem, varsayılan bölümleme ile başlamak ve sonra farklı bölümleme seçenekleri denemektir. Denetim hattı hata ayıklama çalıştırmalarını kullanarak sınayabilir ve izleme görünümünden her dönüşüm grubunda yürütme süresini ve bölüm kullanımını görüntüleyebilirsiniz. Daha fazla bilgi için [bkz.](concepts-data-flow-monitoring.md)

Aşağıdaki bölümleme seçenekleri kullanılabilir.

##### <a name="round-robin"></a>Yuvarlak robin 

Round robin, verileri bölümler arasında eşit olarak dağıtan basit bir bölümdür. Sağlam, akıllı bir bölümleme stratejisi uygulamak için iyi anahtar adayları yoksa round-robin kullanın. Fiziksel bölüm sayısını ayarlayabilirsiniz.

##### <a name="hash"></a>Karma

Azure Veri Fabrikası, benzer değerlere sahip satırların aynı bölüme düşmesi gibi tek düze bölümler üretmek için bir sütun karması üretir. Karma seçeneğini kullandığınızda, olası bölüm eğriliği için sınama. Fiziksel bölüm sayısını ayarlayabilirsiniz.

##### <a name="dynamic-range"></a>Dinamik aralık

Dinamik aralık, sağladığınız sütunları veya ifadeleri temel alan Spark dinamik aralıklarını kullanır. Fiziksel bölüm sayısını ayarlayabilirsiniz. 

##### <a name="fixed-range"></a>Sabit aralık

Bölümlenmiş veri sütunlarınızdaki değerler için sabit aralık sağlayan bir ifade oluşturun. Bölüm eğriliğini önlemek için, bu seçeneği kullanmadan önce verilerinizi iyi anlamanız gerekir. İfade için girdiğiniz değerler bir bölüm işlevinin parçası olarak kullanılır. Fiziksel bölüm sayısını ayarlayabilirsiniz.

##### <a name="key"></a>Anahtar

Verilerinizin ciddiyetini iyi anlıyorsanız, anahtar bölümleme iyi bir strateji olabilir. Anahtar bölümleme, sütununuzdaki her benzersiz değer için bölümler oluşturur. Sayı verilerdeki benzersiz değerleri temel aldığı için bölüm sayısını ayarlayamazsınız.

#### <a name="inspect"></a>Incelemek

**Denetle** sekmesi, dönüştürdüğünüz veri akışının meta verilerine bir görünüm sağlar. Sütun sayımlarını, sütunların değiştiğini, eklenen sütunları, veri türlerini, sütun sırasını ve sütun başvurularını görebilirsiniz. **Denetle,** meta verilerinizin salt okunur görünümüdür. **Denetle** bölmesinde meta verileri görmek için hata ayıklama modunun etkinleştirilmiş olması gerekmez.

![Incelemek](media/data-flow/inspect1.png "Incelemek")

Dönüşümler yoluyla verilerinizin şeklini değiştirerek, **Denetle** bölmesinde meta veri değişikliklerinin akışını görürsünüz. Kaynak dönüşümunuzda tanımlı bir şema yoksa, meta veriler **Inspect** bölmesinde görünmez. Meta veri eksikliği şema sürüklenme senaryolarında yaygındır.

#### <a name="data-preview"></a>Veri önizlemesi

Hata ayıklama modu açıksa, **Veri Önizleme** sekmesi her dönüşümde verilerin etkileşimli bir anlık görüntüsünü sağlar. Daha fazla bilgi için [hata ayıklama modunda Veri önizlemebölümüne](concepts-data-flow-debug-mode.md#data-preview)bakın.

### <a name="top-bar"></a>Üst çubuk

Üst çubuk, kaydetme ve doğrulama gibi tüm veri akışını etkileyen eylemler içerir. Grafiği **Göster** ve **Grafiği Gizle** düğmelerini kullanarak grafik ve yapılandırma modları arasında geçiş yapabilirsiniz.

![Grafiği gizle](media/data-flow/hideg.png "Grafiği gizle")

Grafiğinizi gizlerseniz, **Önceki** ve **Sonraki** düğmeleri aracılığıyla dönüşüm düğümlerinize yanal olarak göz atabilirsiniz.

![Önceki ve sonraki düğmeler](media/data-flow/showhide.png "önceki ve sonraki düğmeler")

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak dönüşümü](data-flow-source.md)oluşturmayı öğrenin.
* Veri akışınızı [hata ayıklama modunda](concepts-data-flow-debug-mode.md)nasıl oluşturabilirsiniz öğrenin.
