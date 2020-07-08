---
title: Whitenoıse paketiyle değişiklik gizliliği uygulama
titleSuffix: Azure Machine Learning
description: Değişiklik gizliliğinin ne olduğunu ve duman paketinin, veri gizliliğini koruyan fark özel sistemlerini uygulamanıza nasıl yardımcı olabileceğini öğrenin.
author: luisquintanilla
ms.author: luquinta
ms.date: 05/03/2020
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.openlocfilehash: aa4fe715c18e582448ee7f642a6a75947356ab61
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84982671"
---
# <a name="preserve-data-privacy-by-using-differential-privacy-and-the-whitenoise-package"></a>Değişiklik gizliliği ve Whitenoıse paketini kullanarak veri gizliliğini koruma

Değişiklik gizliliğinin ne olduğunu ve duman paketinin farklı bir özel sistem uygulamanıza nasıl yardımcı olabileceğini öğrenin.

Kuruluşun analizler için topladığı ve kullandığı veri miktarı arttıkça, gizlilik ve güvenlikle ilgili endişeler de vardır. Çözümlemeler için veri gerekir. Genellikle, modelleri eğitmek için kullanılan veriler daha doğru olur. Bu çözümlemeler için kişisel bilgiler kullanıldığında, verilerin kullanımı boyunca özel olarak kalması özellikle önemlidir.

> [!NOTE]
> Araç takımını yeniden adlandırdığımızda, gelecek haftalarda yeni adı tanıtılacağını lütfen unutmayın. 

## <a name="how-differential-privacy-works"></a>Değişiklik gizliliği nasıl kullanılır?

Değişiklik gizliliği, kişilerin verilerinin güvende ve özel olmasını sağlamaya yardımcı olan bir sistem ve uygulamalar kümesidir.

> [!div class="mx-imgBorder"]
> ![Değişiklik gizlilik süreci](./media/concept-differential-privacy/differential-privacy-process.jpg)

Geleneksel senaryolarda, ham veriler dosya ve veritabanlarında depolanır. Kullanıcılar verileri çözümlerse genellikle ham verileri kullanır. Bu sorun, bir kişinin gizliliğine infringe olabileceğinden bir kaygıdır. Değişiklik gizliliği, kullanıcıların herhangi bir veri noktasını tanımlayamaması için verilere "gürültü" veya rasgelelik ekleyerek bu sorunla başa geçmeye çalışır. En azından, bu tür bir sistem, plausıble deniability sağlar.

Farklı şekilde özel sistemlerde, veriler **sorgu**olarak adlandırılan istekler aracılığıyla paylaşılır. Bir Kullanıcı veriler için bir sorgu gönderdiğinde, **Gizlilik mekanizmaları** olarak bilinen işlemler istenen verilere gürültü ekler. Gizlilik mekanizmaları ham veriler yerine *verilerin yaklaşık bir kısmını* döndürür. Bu Gizlilik-koruma sonucu bir **raporda**görüntülenir. Raporlar, hesaplanan gerçek veriler ve verilerin nasıl oluşturulduğuna ilişkin bir açıklama olmak üzere iki bölümden oluşur.

## <a name="differential-privacy-metrics"></a>Değişiklik gizlilik ölçümleri

Değişiklik gizliliği, kullanıcının sonunda hassas verileri açığa çıkarmak için sınırsız sayıda rapor üretmesine karşı korunmaya çalışır. **Epsilon** olarak bilinen bir değer, ne gürültülü ne de özel bir rapor olduğunu ölçer. Epsilon, gürültü veya gizliliğe ters bir ilişkiye sahiptir. Epsilon alt, verilerin daha gürültülü (ve özel) olması gerekir.

Epsilon değerleri negatif değil. Aşağıdaki değerler 1 tam plausible deniability sağlar. 1 ' den fazla şey gerçek verilerin açığa çıkması riski ile gelir. Farklı bir özel sistem uygularken, 0 ile 1 arasında Epsilon değeri olan raporlar oluşturmak istersiniz.

Daha doğrudan Epsilon ile bağıntılı bir değer **Delta**. Delta, raporun tam olarak özel olmadığı olasılığının bir ölçüsüdür. Delta ne kadar yüksekse, Epsilon daha yüksektir. Bu değerler bağıntılı olduğundan, Epsilon daha sık kullanılır.

## <a name="privacy-budget"></a>Gizlilik bütçesi

Birden çok sorguya izin verilen sistemlerde gizliliği sağlamak için, değişiklik gizliliği bir hız sınırı tanımlar. Bu sınır **Gizlilik bütçesi**olarak bilinir. Gizlilik bütçelerine, yeniden tanımlama riskini sınırlamak için genellikle 1 ile 3 arasında bir Epsilon miktarı ayrılır. Raporlar oluşturulduğunda, gizlilik bütçeleri tek tek raporların Epsilon değerini ve tüm raporların toplamasını izler. Bir gizlilik bütçesi harcandıktan veya boşaldığında, kullanıcılar artık verilere erişemez.  

## <a name="reliability-of-data"></a>Verilerin güvenilirliği

Gizlilik Koruması hedef olmalıdır, ancak verilerin kullanılabilirliği ve güvenilirliği olduğunda bir zorunluluğunu getirir vardır. Veri Analizi 'nde doğruluk, örnekleme hataları tarafından sunulan belirsizlik ölçüsü olarak düşünülebilir. Bu belirsizlik, belirli sınırlar dahilinde düşecek şekilde eğilimlidir. Bunun yerine bir değişiklik gizliliği perspektifinden **doğruluk** , verilerin güvenilirliğini ölçer ve bu, gizlilik mekanizmalarının sunduğu belirsizlik tarafından etkilendi. Kısacası, daha yüksek bir gürültü veya gizlilik düzeyi, daha düşük bir Epsilon, doğruluk ve güvenilirliğe sahip verileri dönüştürür. Veriler çok gizli olsa da, güvenilir olmadığından büyük olasılıkla kullanılma olasılığı düşüktür.

## <a name="implementing-differentially-private-systems"></a>Farklı, özel sistemleri uygulama

Farklı şekilde özel sistemlerin uygulanması zordur. Whitenoıse, küresel farklı özel sistemler oluşturmak için farklı bileşenler içeren açık kaynaklı bir projem. Aşağıdaki en üst düzey bileşenlerden biri oluşmuştur:

- Çekirdek
- Sistem

### <a name="core"></a>Çekirdek

Çekirdek kitaplık, farklı bir özel sistem uygulamak için aşağıdaki Gizlilik mekanizmalarını içerir:

|Bileşen  |Açıklama  |
|---------|---------|
|Analiz     | Rastgele hesaplamaların grafik açıklaması. |
|Doğrulayıcı     | Bir çözümlemenin farklı bir şekilde özel olması için gerekli koşulları denetlemeye ve türetmede kullanılabilecek bir araç kümesi içeren bir Rust kitaplığı.          |
|Çalışma Zamanı     | Çözümlemenin yürütüleceği ortam. Başvuru çalışma zamanı Rust dilinde yazılmıştır, ancak çalışma zamanları, veri gereksinimlerinize bağlı olarak SQL ve Spark gibi herhangi bir hesaplama çerçevesi kullanılarak yazılabilir.        |
|Bağlamalar     | Analizler oluşturmak için dil bağlamaları ve yardımcı kitaplıkları. Şu anda, Python bağlamaları sağlıyor. |

### <a name="system"></a>Sistem

Sistem kitaplığı, tablolu ve ilişkisel verilerle çalışmaya yönelik aşağıdaki araçları ve hizmetleri sağlar:

|Bileşen  |Açıklama  |
|---------|---------|
|Veri Erişimi     | SQL sorgularını karşılar ve işleyen ve rapor üreten kitaplık. Bu kitaplık Python 'da uygulanır ve aşağıdaki ODBC ve DBAPı veri kaynaklarını destekler:<ul><li>PostgreSQL</li><li>SQL Server</li><li>Spark</li><li>Preston</li><li>Pandas</li></ul>|
|Hizmet     | Paylaşılan veri kaynaklarına karşı istekleri veya sorguları sunan bir REST uç noktası sağlayan yürütme hizmeti. Hizmet, heterojen istekler olarak da bilinen farklı Delta ve Epsilon değerleri içeren isteklerde çalışan değişiklik gizlilik modüllerinin bulunmasına olanak tanımak üzere tasarlanmıştır. Bu başvuru, bağıntılı verilere ilişkin sorgulardan daha fazla etki için uygulama hesaplar. |
|İse     | Gizlilik ihlallerini, doğruluğu ve farkı denetleyen stochastik değerlendirici. Değerlendirici aşağıdaki testleri destekler: <ul><li>Gizlilik testi-bir raporun değişiklik gizliliği koşullarına uygun olup olmadığını belirler.</li><li>Doğruluk testi-raporların güvenilirliğini %95 Güvenirlik düzeyi verilen üst ve alt sınırların içinde olup olmadığını ölçer.</li><li>Yardımcı program testi-bir raporun güven sınırlarının, hala gizliliği en üst düzeye çıkarırken verilere yeterince yakın olup olmadığını belirler.</li><li>Sapma testi-dengesizolmamasını sağlamak için yinelenen sorgular için raporların dağıtımını ölçer</li></ul> |

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning [veri gizliliğini koruyun](how-to-differential-privacy.md) .

İzin alma bileşenleri hakkında daha fazla bilgi edinmek için, bir [Çekirdek paketi](https://github.com/opendifferentialprivacy/whitenoise-core)olan GitHub depolarına göz atın, [Sistem paketi](https://github.com/opendifferentialprivacy/whitenoise-system) ve whitenoıse [örnekleri](https://github.com/opendifferentialprivacy/whitenoise-samples).