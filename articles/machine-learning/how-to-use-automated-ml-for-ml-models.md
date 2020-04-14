---
title: Dağıtmak & modelleri oluşturmak için autoML'yi kullanın
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile otomatik makine öğrenimi modelleri oluşturun, gözden geçirin ve dağıtın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: nibaccam
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 03/10/2020
ms.openlocfilehash: 0d6fa02578814c4c5d034be05cbc63093d70603b
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81257241"
---
# <a name="create-review-and-deploy-automated-machine-learning-models-with-azure-machine-learning"></a>Azure Machine Learning ile otomatik makine öğrenimi modelleri oluşturun, gözden geçirin ve dağıtın
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Bu makalede, Azure Machine Learning'in stüdyo arabiriminde tek bir kod satırı olmadan otomatik makine öğrenimi modellerini nasıl oluşturabileceğinizi, keşfeder ve dağıtabileceğinizi öğrenirsiniz. Otomatik makine öğrenimi, belirli verileriniz için kullanılacak en iyi makine öğrenimi algoritmasının sizin için seçildiği bir süreçtir. Bu işlem, hızlı bir şekilde makine öğrenme modelleri oluşturmanıza olanak sağlar. [Otomatik makine öğrenimi hakkında daha fazla bilgi edinin.](concept-automated-ml.md)
 
Uçtan uca bir örnek için, [Azure Machine Learning'in otomatik ML arabirimi yle bir sınıflandırma modeli oluşturmak için öğreticiyi](tutorial-first-experiment-automated-ml.md)deneyin. 

Python kod tabanlı bir deneyim için, Azure Machine Learning SDK ile [otomatik makine öğrenimi denemelerinizi yapılandırın.](how-to-configure-auto-train.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

* Bir tür **Enterprise sürümü**içeren bir Azure Machine Learning çalışma alanı. Bkz. [Azure Makine Öğrenimi çalışma alanı oluşturun.](how-to-manage-workspace.md)  Varolan bir çalışma alanını Enterprise sürümüne yükseltmek için [bkz.](how-to-manage-workspace.md#upgrade)

## <a name="get-started"></a>Kullanmaya başlayın

1. Azure Machine Learning'de https://ml.azure.comoturum açın. 

1. Aboneliğinizi ve çalışma alanınızı seçin. 

1. Sol bölmeye gidin. **Yazar** bölümünün altında **Otomatik ML'yi** seçin.

[![Azure Machine Learning stüdyo navigasyon bölmesi](media/how-to-use-automated-ml-for-ml-models/nav-pane.png)](media/how-to-use-automated-ml-for-ml-models/nav-pane-expanded.png)

 Bu herhangi bir deneme yapıyor ilk kez ise, boş bir liste ve belgelere bağlantılar görürsünüz. 

Aksi takdirde, SDK ile oluşturulanlar da dahil olmak üzere en son otomatik makine öğrenimi denemelerinizin bir listesini görürsünüz. 

## <a name="create-and-run-experiment"></a>Deneme oluşturma ve çalıştırma

1. + **Yeni otomatik ML çalıştır'ı** seçin ve formu doldurun.

1. Depolama kapsayıcınızdan bir veri kümesi seçin veya yeni bir veri kümesi oluşturun. Veri kümeleri yerel dosyalardan, web url'lerinden, veri depolarından veya Azure açık veri kümelerinden oluşturulabilir. 

    >[!Important]
    > Eğitim verileri için gereksinimler:
    >* Veriler tabular formda olmalıdır.
    >* Tahmin etmek istediğiniz değer (hedef sütun) verilerde bulunmalıdır.

    1. Yerel bilgisayarınızdaki bir dosyadan yeni bir veri kümesi oluşturmak için **Gözat'ı** seçin ve ardından dosyayı seçin. 

    1. Veri setinize benzersiz bir ad verin ve isteğe bağlı bir açıklama sağlayın. 

    1. **Datastore ve dosya seçim formunu**açmak için **İleri'yi** seçin. Bu formda veri kümenizi nereye yükleyebileceğinizi seçin; çalışma alanınız ile otomatik olarak oluşturulan varsayılan depolama kapsayıcısı veya deneme için kullanmak istediğiniz bir depolama kapsayıcısı seçin. 

    1. Doğruluk için **Ayarlar ve önizleme** formunu inceleyin. Form, dosya türüne göre akıllıca doldurulur. 

        Alan| Açıklama
        ----|----
        Dosya biçimi| Bir dosyada depolanan verilerin düzenini ve türünü tanımlar.
        Sınırlayıcı| Düz metinde veya diğer veri akışlarında ayrı, bağımsız bölgeler arasındaki sınırı belirtmek için bir veya daha fazla karakter.
        Encoding| Veri kümenizi okumak için kullanılacak şema tablosunun hangi bitini tanımlar.
        Sütun başlıkları| Varsa veri kümesinin üstbilgilerinin nasıl işleneceklerini gösterir.
        Satırları atla | Veri kümesinde kaç satırAtıldı gösterir.
    
        **Sonraki'ni**seçin.

    1. **Şema** formu, **Ayarlar ve önizleme** formundaki seçimlere göre akıllıca doldurulur. Burada her sütun için veri türünü yapılandırın, sütun adlarını gözden geçirin ve denemeniz için hangi sütunların **eklenmeyeceğini** seçin. 
            
        **İleri'yi seçin.**

    1. **Ayrıntıları Onayla** formu, **temel bilgi** ve ayarlar ve **önizleme** formlarında daha önce doldurulan bilgilerin bir özetidir. Ayrıca, profil oluşturma özellikli bir bilgi işlem kullanarak veri kümeniz için bir veri profili oluşturma seçeneğiniz de var. [Veri profilleme](#profile)hakkında daha fazla bilgi edinin.

        **Sonraki'ni**seçin.
1. Yeni oluşturulan veri kümenizi göründükten sonra seçin. Ayrıca, veri kümesinin ve örnek istatistiklerin önizlemesini de görüntüleyebilirsiniz. 

1. **Yapılandırılan çalıştır** formuna benzersiz bir deneme adı girin.

1. Hedef sütun seçin; bu, üzerinde öngörüler yapmak istediğiniz sütundur.

1. Veri profiloluşturma ve eğitim işi için bir bilgi işlem seçin. Açılan listede varolan işlemlerinizin listesi kullanılabilir. Yeni bir bilgi işlem oluşturmak için, adım 7'deki yönergeleri izleyin.

1. Bu deneme için bilgi işlem bağlamınızı yapılandırmak için **yeni bir bilgi işlem oluştur'u** seçin.

    Alan|Açıklama
    ---|---
    İşlem adı| Bilgi işlem bağlamınızı tanımlayan benzersiz bir ad girin.
    Sanal makine boyutu| İşleminiz için sanal makine boyutunu seçin.
    Min / Max düğümleri (Gelişmiş Ayarlar' da)| Profil verileri için 1 veya daha fazla düğüm belirtmeniz gerekir. İşleminiz için en fazla düğüm sayısını girin. Varsayılan, Bir AML İşlemi için 6 düğümdür.
    
    **Oluştur'u**seçin. Yeni bir işlem oluşturulması birkaç dakika sürebilir.

    >[!NOTE]
    > Bilgi işlem adınız, seçtiğiniz/oluşturduğunuz bilgiişlemin *profil oluşturma etkin*olup olmadığını gösterir. (Daha fazla ayrıntı için bölüm [verilerinin profilini çıkarma](#profile) bölümüne bakın).

    **Sonraki'ni**seçin.

1. Görev **türü ve ayarları** formunda görev türünü seçin: sınıflandırma, regresyon veya tahmin. Daha fazla bilgi için [görev türlerinin nasıl tanımlanabildiğini](how-to-define-task-type.md) görün.

    1. Sınıflandırma için, metin featurizations için kullanılan derin öğrenme etkinleştirebilirsiniz.

    1. Tahmin için:
        1. Zaman sütununu seçin: Bu sütun kullanılacak zaman verilerini içerir.

        1. Tahmin ufkunu seçin: Modelin geleceğe tahmin edebilmesi için kaç saat birimi (dakika/saat/gün/hafta/ay/yıl) gösterin. Daha fazla model gelecekte tahmin etmek için gerekli ise, daha az doğru olacak. [Tahmin ve tahmin ufku hakkında daha fazla bilgi edinin.](how-to-auto-train-forecast.md)

1. (İsteğe bağlı) Ek yapılandırma ayarlarını görüntüleyin: Eğitim işini daha iyi denetlemek için kullanabileceğiniz ek ayarlar. Aksi takdirde, varsayılanlar deneme seçimi ve verilere göre uygulanır. 

    Ek yapılandırmalar|Açıklama
    ------|------
    Birincil metrik| Modelinizi puanlamak için kullanılan ana metrik. [Model ölçümleri hakkında daha fazla bilgi edinin.](how-to-configure-auto-train.md#explore-model-metrics)
    Otomatik featurization| Otomatik makine öğrenimi tarafından yapılan ön işlemeyi etkinleştirmek veya devre dışı kalarak seçin. Ön işleme, sentetik özellikler oluşturmak için otomatik veri temizleme, hazırlama ve dönüştürmeyi içerir. Görev türü tahmin edilen zaman serisi için desteklenmez. [Ön işleme hakkında daha fazla bilgi edinin.](#featurization) 
    En iyi modeli açıklayın | Önerilen en iyi modelin açıklanabilirliğini göstermek için etkinleştirmek veya devre dışı kılabilir
    Engellenen algoritma| Eğitim işinden hariç tutmak istediğiniz algoritmaları seçin.
    Çıkış kriteri| Bu kriterlerden herhangi biri karşılandığında, eğitim işi durdurulur. <br> *Eğitim iş süresi (saat)*: Ne kadar süre eğitim iş çalışmasına izin vermek. <br> *Metrik puan eşiği*: Tüm boru hatları için minimum metrik puan. Bu, ulaşmak istediğiniz tanımlı bir hedef ölçüme sahipseniz, eğitim işinde gerekenden daha fazla zaman harcamamanızı sağlar.
    Doğrulama| Eğitim işinde kullanmak üzere çapraz doğrulama seçeneklerinden birini seçin. [Çapraz doğrulama hakkında daha fazla bilgi edinin.](how-to-configure-auto-train.md)
    Eşzamanlılık| *Maksimum eşzamanlı yinelemeler*: Eğitim işinde test etmek için maksimum ardışık hat lar (yinelemeler) sayısı. İş, belirtilen yineleme sayısından fazla çalışmaz.

1. (İsteğe bağlı) Featurization ayarlarını görüntüleyin: **Ek yapılandırma ayarları** formunda Otomatik **featurization'ı** etkinleştirmeyi seçerseniz, bu form, bu başarısıralamalarını gerçekleştirmek için hangi sütunları belirteceğiniz ve eksik değer imputasyonları için hangi istatistiksel değeri kullanacağınızı seçtiğiniz yerdir.

<a name="profile"></a>

## <a name="data-profiling--summary-stats"></a>Özet istatistikleri & veri profilleme

Veri setinizin ML'ye hazır olup olmadığını doğrulamak için veri setinizde çok çeşitli özet istatistikleri alabilirsiniz. Sayısal olmayan sütunlar için, yalnızca min, max ve hata sayısı gibi temel istatistikleri içerirler. Sayısal sütunlar için, istatistiksel momentlerini ve tahmini niceliklerini de gözden geçirebilirsiniz. Özellikle, veri profilimiz şunları içerir:

>[!NOTE]
> Alakasız türleri olan özellikler için boş girişler görüntülenir.

Istatistik|Açıklama
------|------
Özellik| Özetlenen sütunun adı.
Profil| Çıkarılan türe göre satır içi görselleştirme. Örneğin, dizeleri, booleans ve tarihler değer sayımları olacak, ondalık sayılar (sayısal) histogramlar yaklaşık varken. Bu, verilerin dağılımı hakkında hızlı bir anlayış elde etmenizi sağlar.
Tür dağılımı| Sütun içindeki türlerin satır içi değer sayısı. Nulls kendi türüdür, bu nedenle bu görselleştirme tek veya eksik değerleri algılamak için yararlıdır.
Tür|Sütunun çıkarılan türü. Olası değerler şunlardır: dizeleri, booleans, tarihler ve ondalık sayılar.
Min| Sütunun minimum değeri. Türünde doğal bir sıralama olmayan özellikler için boş girişler görünür (örn. booleans).
Maks| Sütunun maksimum değeri. 
Sayı| Sütundaki toplam eksik ve eksik olmayan giriş sayısı.
Eksik sayı değil| Sütunda eksik olmayan giriş sayısı. Boş dizeleri ve hataları değer olarak kabul edilir, bu nedenle "eksik değil sayısına" katkıda bulunmaz.
Quantiles| Verilerin dağılımını bir anlamda sağlamak için her nicelikte yaklaşık değerler.
Ortalama| Sütunun aritmetik ortalaması veya ortalaması.
Standart sapma| Bu sütunun verilerinin dağılım veya varyasyon miktarını ölçün.
Varyans| Bu sütunun verilerinin ortalama değerinden ne kadar yayıldığının ölçüsüdür. 
Çarpıklık| Bu sütunun verilerinin normal bir dağılımdan ne kadar farklı olduğunu ölçün.
Basık| Bu sütunun verilerinin ne kadar yoğun şekilde takip edildiğinin ölçüsü normal bir dağılımla karşılaştırılır.

<a name="featurization"></a>

## <a name="advanced-featurization-options"></a>Gelişmiş featurization seçenekleri

Otomatik makine öğrenimi, verilerinizle ilgili olası sorunları belirlemenize ve yönetmenize yardımcı olmak için otomatik olarak ön işleme ve veri korkulukları sunar. 

### <a name="preprocessing"></a>Ön

> [!NOTE]
> Otomatik ML oluşturulan modellerinizi bir [ONNX modeline](concept-onnx.md)dışa aktarmayı planlıyorsanız, yalnızca * ile gösterilen featurization seçenekleri ONNX biçiminde desteklenir. [Modelleri ONNX'e dönüştürme](concept-automated-ml.md#use-with-onnx)hakkında daha fazla bilgi edinin. 

|Ön&nbsp;işleme adımları| Açıklama |
| ------------- | ------------- |
|Yüksek kardinallik bırak veya varyans özellikleri yok* |Tüm değerler eksik, tüm satırlarda aynı değere sahip veya son derece yüksek öneme sahip (örneğin, kalıplar, t.c.'ler veya GUI'ler) özellikler de dahil olmak üzere bunları eğitim ve doğrulama kümelerinden bırakın.|
|Eksik değerleri impute* |Sayısal özellikler için, sütundaki değerlerin ortalaması ile impute.<br/><br/>Kategorik özellikler için, en sık değere sahip impute.|
|Ek özellikler oluşturun* |DateTime özellikleri için: Yıl, Ay, Gün, haftanın günü, yılın günü, Üç Aylık Dönem, Yılın Haftası, Saat, Dakika, İkinci.<br/><br/>Metin özellikleri için: Tek gram, çift gram ve üç karakterli grama dayalı terim sıklığı.|
|Dönüştürün ve kodlayın *|Birkaç benzersiz değere sahip sayısal özellikler kategorik özelliklere dönüştürülür.<br/><br/>Düşük kardinallik kategorik için tek sıcak kodlama yapılır; yüksek kardinallik, tek-sıcak-karma kodlama için.|
|Kelime gömme|Metin belirteçlerinin vektörlerini önceden eğitilmiş bir model kullanarak cümle vektörlerine dönüştüren metin featurizer. Her sözcüğün bir belgeye gömme vektörü, bir belge özellik vektörü oluşturmak için bir araya toplanır.|
|Hedef kodlamalar|Kategorik özellikler için, her kategoriyi regresyon sorunları için ortalama hedef değerine ve sınıflandırma sorunları için her sınıf için sınıf olasılığına göre eşler. Seyrek veri kategorilerinin neden olduğu haritalama ve gürültünün montajı üzerinde azaltmak için frekans tabanlı ağırlıklandırma ve k-kat çapraz doğrulama uygulanır.|
|Metin hedef kodlama|Metin girişi için, her sınıfın olasılığını oluşturmak için sözcüklerden oluşan bir yığın doğrusal model kullanılır.|
|Kanıt Ağırlığı (WoE)|Kategorik sütunların hedef sütunla korelasyonunun bir ölçüsü olarak WoE'yi hesaplar. Sınıf içi vs sınıf dışı olasılıkların oranının günlüğü olarak hesaplanır. Bu adım, sınıf başına bir sayısal özellik sütunu çıkarır ve eksik değerleri ve aykırı tedaviyi açıkça ortaya koyma gereksinimini ortadan kaldırır.|
|Küme Mesafesi|Tüm sayısal sütunlarda k anlamına gelir kümeleme modeli eğitir.  Çıktılar k yeni özellikler, küme başına yeni bir sayısal özellik, her bir kümenin centroid için her örnek uzaklığı içeren.|

### <a name="data-guardrails"></a>Veri korkulukları

Otomatik featurization etkinleştirildiğinde veya doğrulama otomatik olarak ayarlandığında veri korkulukları uygulanır. Veri korkulukları, verilerinizle ilgili olası sorunları (örn. eksik değerler, sınıf dengesizliği) belirlemenize ve geliştirilmiş sonuçlar için düzeltici eylemlerde yardımcı olur. 

Kullanıcılar, otomatik ml çalışmasının Veri **korkulukları** sekmesindeki stüdyodaki veri korkuluklarını ```show_output=True``` inceleyebilir veya Python SDK'yı kullanarak deneme gönderirken ayarlayabilir. 

#### <a name="data-guardrail-states"></a>Veri Koruma Durumları

Veri korkulukları üç eyaletten birini görüntüler: **Geçti**, **Bitti**, veya **Uyarı .**

Durum| Açıklama
----|----
Geçirilen| Veri sorunları algılanmadı ve kullanıcı eylemi gerekmez. 
Bitti| Verilerinize değişiklikler uygulandı. Kullanıcıların, değişikliklerin beklenen sonuçlarla uyumlu olmasını sağlamak için Otomatik ML'nin gerçekleştirdiği düzeltici eylemleri gözden geçirmelerini öneririz. 
Uyarılır| Giderilemeyen bir veri sorunu algılandı. Kullanıcıları sorunu düzeltmeye ve düzeltmeye teşvik ediyoruz. 

>[!NOTE]
> Otomatik ML denemelerinin önceki sürümlerinde dördüncü bir durum görüntülenir: **Sabit.** Yeni denemeler bu durumu görüntülemez ve **Sabit** durumu görüntüleyen tüm korkuluklar artık **Bitti'yi**görüntüler.   

Aşağıdaki tabloda, şu anda desteklenen veri korkulukları ve kullanıcıların denemelerini gönderirken karşılaşabilecekleri ilişkili durumları açıklanmaktadır.

Otokorkuluk|Durum|Tetikleme&nbsp;koşulu&nbsp;
---|---|---
Eksik özellik değerleri imputation |**Geçirilen** <br><br><br> **Bitti**| Eğitim verilerinizde eksik özellik değerleri algılanmadı. [Eksik değer imputation](https://docs.microsoft.com/azure/machine-learning/how-to-use-automated-ml-for-ml-models#advanced-featurization-options) hakkında daha fazla bilgi edinin. <br><br> Eğitim verilerinizde eksik özellik değerleri algılandı ve imputed.
Yüksek kardinallik özelliği işleme |**Geçirilen** <br><br><br> **Bitti**| Girişleriniz analiz edildi ve yüksek kardinallik özelliği algılanmadı. [Yüksek kardinallik özelliği algılama](https://docs.microsoft.com/azure/machine-learning/how-to-use-automated-ml-for-ml-models#advanced-featurization-options) hakkında daha fazla bilgi edinin. <br><br> Girişlerinizde yüksek kardinallik özellikleri algılandı ve işlendi.
Doğrulama bölünmüş işleme |**Bitti**| *Doğrulama yapılandırması 'otomatik' olarak ayarlandı ve eğitim verileri 20.000'den **az** satır içeriyordu.* <br> Eğitilen modelin her yinelemesi çapraz doğrulama yoluyla doğrulandı. Doğrulama verileri hakkında daha fazla bilgi [edinin.](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train#train-and-validation-data) <br><br> *Doğrulama yapılandırması 'otomatik' olarak ayarlandı ve eğitim verileri 20.000'den **fazla** satır içeriyordu.* <br> Giriş verileri bir eğitim veri kümesine ve modelin doğrulanması için bir doğrulama veri kümesine ayrılmıştır.
Sınıf dengeleme algılama |**Geçirilen** <br><br><br><br> **Uyarılır** | Girişleriniz analiz edildi ve tüm sınıflar eğitim verilerinizde dengelendi. Her sınıf, örneklerin sayısı ve oranıyla ölçüldüğünde, veri kümesinde iyi bir gösterime sahipse, veri kümesi dengeli olarak kabul edilir. <br><br><br> Girişlerinizde dengesiz sınıflar tespit edildi. Model önyargısını düzeltmek için dengeleme sorunu giderin. Dengesiz veriler hakkında daha fazla bilgi [edinin.](https://docs.microsoft.com/azure/machine-learning/concept-manage-ml-pitfalls#identify-models-with-imbalanced-data)
Bellek sorunları algılama |**Geçirilen** <br><br><br><br> **Bitti** |<br> Seçili {horizon, lag, rolling window} değeri(ler) analiz edildi ve bellek dışı olası sorunlar algılanmadı. Zaman serisi tahmin [yapılandırmaları](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#configure-and-run-experiment) hakkında daha fazla bilgi edinin. <br><br><br>Seçili {horizon, lag, rolling window} değerleri analiz edildi ve denemenizin belleği bitme potansiyeline sahip olabilir. Gecikme veya yuvarlanma penceresi yapılandırmaları kapatıldı.
Frekans algılama |**Geçirilen** <br><br><br><br> **Bitti** |<br> Zaman serisi analiz edildi ve tüm veri noktaları algılanan frekansla hizalandı. <br> <br> Zaman serisi analiz edildi ve algılanan frekansla uyuşmayan veri noktaları algılandı. Bu veri noktaları veri kümesinden kaldırıldı. Zaman serisi tahminleri için veri hazırlama hakkında daha fazla bilgi [edinin.](https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-forecast#preparing-data)

## <a name="run-experiment-and-view-results"></a>Deneme çalıştırma ve sonuçları görüntüleme

Denemenizi çalıştırmak için **Finish'i** seçin. Denemeyi hazırlama işlemi 10 dakika kadar sürebilir. Eğitim işlerinde her işlem hattının çalıştırılmasını tamamlamak fazladan 2-3 dakika daha sürebilir.

### <a name="view-experiment-details"></a>Deneme ayrıntılarını görüntüleme

**Ayrıntıyı Çalıştır** ekranı **Ayrıntılar** sekmesine açılır. Bu ekranda, çalışma numarasının yanındaki üst teki durum çubuğu da dahil olmak üzere deneme çalışmasının bir özetini gösterir. 

**Modeller** sekmesi oluşturulan ve ölçüm puanına göre sıralanan model listesini içerir. Varsayılan olarak, seçilen ölçüme göre en yüksek puana sahip olan model listenin en üstünde yer alır. Eğitim işi daha fazla model denedikçe, bu modeller listeye eklenir. Bunu kullanarak, şimdiye kadar oluşturulan modeller için ölçümlerin hızlı bir karşılaştırmasını elde edebilirsiniz.

[![Ayrıntılar panosunu çalıştır](media/how-to-use-automated-ml-for-ml-models/run-details.png)](media/how-to-use-automated-ml-for-ml-models/run-details-expanded.png#lightbox)

### <a name="view-training-run-details"></a>Eğitim çalıştırma ayrıntılarını görüntüleme

**Model ayrıntıları** sekmesindeki çalışma ölçümleri veya **Görselleştirmeler** sekmesindeki performans grafikleri gibi eğitim çalıştırma ayrıntılarını görmek için tamamlanan modellerden herhangi birini inceince ayrıntılı bilgi [edinin.](how-to-understand-automated-ml.md)

[![Yineleme ayrıntıları](media/how-to-use-automated-ml-for-ml-models/iteration-details.png)](media/how-to-use-automated-ml-for-ml-models/iteration-details-expanded.png)

## <a name="deploy-your-model"></a>Modelinizi dağıtın

Elinizdeen iyi modele sahip olduktan sonra, yeni veriler üzerinde tahmin etmek için bir web hizmeti olarak dağıtmanın zamanı gelmiştir.

Otomatik ML, kodu yazmadan modeli dağıtmanıza yardımcı olur:

1. Dağıtım için birkaç seçeneğiniz var. 

    + Seçenek 1: En iyi modeli dağıtmak için (tanımladığınız metrik ölçütlere göre), **Ayrıntılar** sekmesinde **en iyi modeli dağıt** düğmesini seçin.

    + Seçenek 2: Bu denemeden belirli bir model yinelemesini dağıtmak için, **Model ayrıntıları** sekmesini açmak için modeli ayrıntıya inin ve Model'i **Dağıt'ı**seçin.

1. **Dağıl model** bölmesini doldur.

    Alan| Değer
    ----|----
    Adı| Dağıtımınız için benzersiz bir ad girin.
    Açıklama| Bu dağıtımın ne için olduğunu daha iyi belirlemek için bir açıklama girin.
    İşlem türü| Dağıtmak istediğiniz bitiş noktası türünü seçin: *Azure Kubernetes Hizmeti (AKS)* veya *Azure Kapsayıcı Örneği (ACI)*.
    İşlem adı| *Yalnızca AKS için geçerlidir:* Dağıtmak istediğiniz AKS kümesinin adını seçin.
    Kimlik doğrulamayı etkinleştirme | Belirteç tabanlı veya anahtar tabanlı kimlik doğrulamasına izin vermek için seçin.
    Özel dağıtım varlıklarını kullanma| Kendi puanlama komut dosyası ve ortam dosyanızı yüklemek istiyorsanız bu özelliği etkinleştirin. [Komut dosyalarını puanlama hakkında daha fazla bilgi edinin.](how-to-deploy-and-where.md#script)

    >[!Important]
    > Dosya adları 32 karakterin altında olmalı ve alfasayısal larla başlayıp bitmelidir. Tireler, alt çizerler, nokta ve alfanümerik arasında içerebilir. Boşluklara izin verilmez.

    *Gelişmiş* menü, [veri toplama](how-to-enable-app-insights.md) ve kaynak kullanım ayarları gibi varsayılan dağıtım özellikleri sunar. Bu varsayılanları geçersiz kılmak istiyorsanız, bunu bu menüde yapın.

1. **Dağıt**'ı seçin. Dağıtımın tamamlanması yaklaşık 20 dakika sürebilir.

Şimdi tahminler oluşturmak için operasyonel bir web hizmeti var! Azure [Machine Learning desteğinde yerleşik Power BI'den](how-to-consume-web-service.md#consume-the-service-from-power-bi)hizmeti sorgulayarak öngörüleri test edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir web hizmetini nasıl tükettiğinızı öğrenin.](https://docs.microsoft.com/azure/machine-learning/how-to-consume-web-service)
* [Otomatik makine öğrenimi sonuçlarını anlayın.](how-to-understand-automated-ml.md)
* [Otomatik makine öğrenimi](concept-automated-ml.md) ve Azure Machine Learning hakkında daha fazla bilgi edinin.
