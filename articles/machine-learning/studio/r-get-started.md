---
title: R kullanmaya başlama
titleSuffix: ML Studio (classic) - Azure
description: Bir tahmin çözümü oluşturmak için Azure Machine Learning Studio (klasik) ile R dilini kullanmaya başlamak için bu R programlama öğreticisini kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/01/2019
ms.openlocfilehash: 1b347707b3c656bd692a29f0fd748c1503be4fb8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218004"
---
# <a name="getting-started-with-the-r-programming-language-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio'da R programlama dili yle başlarken (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

<!-- Stephen F Elston, Ph.D. -->

## <a name="introduction"></a>Giriş

Bu öğretici, R programlama dilini kullanarak Azure Machine Learning Studio'yu (klasik) genişletmeye başlamanıza yardımcı olur. Studio (klasik) içinde R kodu oluşturmak, test etmek ve çalıştırmak için bu R programlama öğretici izleyin. Öğretici ile çalışırken, Studio'da R dilini (klasik) kullanarak tam bir tahmin çözümü oluşturacaksınız.  

Azure Machine Learning Studio (klasik) birçok güçlü makine öğrenimi ve veri işleme modülleri içerir. Güçlü R dili, analitiğin lingua franca'sı olarak tanımlanmıştır. Ne mutlu ki, Studio'da analitik ve veri manipülasyonu (klasik) R kullanılarak genişletilebilir. Bu kombinasyon, Studio'nun (klasik) esnekliğini ve r'nin derin analitiğini sağlar.

### <a name="forecasting-and-the-dataset"></a>Tahmin ve veri kümesi

Tahmin yaygın olarak kullanılan ve oldukça yararlı bir analitik yöntemdir. Yaygın kullanımlar mevsimsel maddelerin satışlarını tahmin etmek, optimum stok düzeylerini belirlemekten makroekonomik değişkenleri tahmin etmeye kadar değişmektedir. Tahmin genellikle zaman serisi modelleri ile yapılır.

Zaman serisi verileri, değerlerin zaman dizini olduğu verilerdir. Zaman endeksi düzenli olabilir, örneğin her ay veya her dakika, ya da düzensiz. Zaman serisi modeli zaman serisi verilerine dayanır. R programlama dili esnek bir çerçeve ve zaman serisi verileri için kapsamlı bir analiz içerir.

Bu kılavuzda Kaliforniya süt ürünleri üretimi ve fiyatlandırma verileri ile birlikte çalışacağız. Bu veriler, çeşitli süt ürünlerinin üretimi ve bir kriter emtia olan süt yağının fiyatı hakkında aylık bilgiler içermektedir.

Bu makalede kullanılan veriler, R komut dosyaları ile birlikte [MachineLearningSamples-Notebooks/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples)adresinden indirilebilir. Dosyadaki `cadairydata.csv` veriler ilk olarak Wisconsin Üniversitesi'nden edinilebilen [https://dairymarkets.com](https://dairymarkets.com)bilgilerden sentezlendi.

### <a name="organization"></a>Kuruluş

Azure Machine Learning Studio (klasik) ortamında analitik ve veri işleme R kodunu oluşturmayı, test etmeyi ve çalıştırmayı öğrendikçe birkaç adımda ilerleyeceğiz.  

* Önce Azure Machine Learning Studio (klasik) ortamında R dilini kullanmanın temellerini inceleyeceğiz.
* Daha sonra Azure Machine Learning Studio (klasik) ortamında veri, R kodu ve grafikler için G/Ç'nin çeşitli yönlerini tartışmaya devam ediyoruz.
* Daha sonra veri temizleme ve dönüştürme için kod oluşturarak tahmin çözümümüzün ilk bölümünü oluşturacağız.
* Hazırladığımız verilerle, veri setimizdeki birkaç değişken arasındaki korelasyonların analizini gerçekleştireceğiz.
* Son olarak, süt üretimi için mevsimsel zaman serisi tahmin modeli oluşturacaktır.

## <a name="interact-with-r-language-in-machine-learning-studio-classic"></a><a id="mlstudio"></a>Machine Learning Studio'da R dili ile etkileşim (klasik)

Bu bölümde, Machine Learning Studio (klasik) ortamında R programlama dili ile etkileşim bazı temelleri ile götürür. R dili, Azure Machine Learning Studio (klasik) ortamında özelleştirilmiş analitik ve veri işleme modülleri oluşturmak için güçlü bir araç sağlar.

RStudio'u küçük ölçekte R kodunu geliştirmek, test etmek ve hata ayıklamak için kullanacağım. Bu kod daha sonra, Azure Machine Learning Studio'da (klasik) çalışmaya hazır bir [Execute R Script][execute-r-script] modülüne kesilip yapıştırılır.  

### <a name="the-execute-r-script-module"></a>Execute R Script modülü

Machine Learning Studio (klasik) içinde, R komut dosyaları [Execute R Script][execute-r-script] modülü içinde çalıştırılır. Machine Learning Studio'daki [Execute R Script][execute-r-script] modülünün bir örneği (klasik) Şekil 1'de gösterilmiştir.

 ![R programlama dili: Machine Learning Studio'da seçilen Execute R Script modülü (klasik)](./media/r-quickstart/fig1.png)

*Şekil 1. Machine Learning Studio (klasik) ortamı execute R Script modülseçilir.*

Şekil 1'e atıfta bulunarak, [Execute R Script][execute-r-script] modülüyle çalışmak için Machine Learning Studio (klasik) ortamının bazı önemli bölümlerine bakalım.

* Deneydeki modüller orta bölmede gösterilir.
* Sağ bölmenin üst kısmında R komut dosyalarınızı görüntülemek ve düzeltmek için bir pencere içerir.  
* Sağ bölmenin alt kısmı [Execute R Script'in][execute-r-script]bazı özelliklerini gösterir. Bu bölmenin uygun noktalarını seçerek hata ve çıkış günlüklerini görüntüleyebilirsiniz.

Biz, tabii ki, bu makalenin geri kalanında daha ayrıntılı olarak [Execute R Script][execute-r-script] tartışıyor olacak.

Karmaşık R işlevleri ile çalışırken, rstudio'da izlemenizi, test etmemi ve hata ayıklamanızı öneririm. Herhangi bir yazılım geliştirmede olduğu gibi, kodunuzu artımlı olarak genişletin ve küçük basit test örneklerinde test edin. Daha sonra işlevlerinizi Execute [R Script][execute-r-script] modülünün R komut dosyası penceresine kesip yapıştırın. Bu yaklaşım, hem RStudio tümleşik geliştirme ortamını (IDE) hem de Azure Machine Learning Studio'nun (klasik) gücünü kullanmanızı sağlar.  

#### <a name="execute-r-code"></a>R kodunu çalıştırma

[Çalıştır R Script][execute-r-script] modülündeki herhangi bir R **kodu, Çalıştır** düğmesini seçerek denemeyi çalıştırdığınızda çalıştırılacaktır. Yürütme tamamlandığında, [Yürüt R Script][execute-r-script] simgesinde bir onay işareti görünür.

#### <a name="defensive-r-coding-for-azure-machine-learning"></a>Azure Machine Learning için Savunma R kodlaması

Örneğin, Azure Machine Learning Studio 'yı (klasik) kullanarak bir web hizmeti için R kodu geliştiriyorsanız, kodunuzu beklenmeyen bir veri girişi ve özel durumlarla nasıl başa çıkacağını kesinlikle planlamanız gerekir. Netliği korumak için, gösterilen kod örneklerinin çoğunda denetleme veya özel durum işleme yoluna çok fazla dahil olmadım. Ancak, devam ederken size R'nin özel durum işleme yeteneğini kullanarak işlevlere birkaç örnek vereceğim.  

Eğer R istisna işleme daha eksiksiz bir tedavi gerekiyorsa, size Wickham tarafından kitabın uygulanabilir bölümleri daha fazla okuma aşağıda listelenen okumanızı [öneririz.](#appendixb)

#### <a name="debug-and-test-r-in-machine-learning-studio-classic"></a>Machine Learning Studio'da Hata Ayıklama ve Test R (klasik)

Yinelemek için, rstudio küçük bir ölçekte test ve hata ayıklama öneririz. Ancak, [Yürüt R Script][execute-r-script] kendisinde R kodu sorunlarını izlemek gerekir durumlar vardır. Buna ek olarak, Machine Learning Studio (klasik) sonuçlarınızı kontrol etmek için iyi bir uygulamadır.

R kodunuzu ve Azure Machine Learning Studio (klasik) platformunda uygulama çıktısı öncelikle output.log'da bulunur. Bazı ek bilgiler error.log'da görülecektir.  

R kodunuzu çalıştırırken Machine Learning Studio'da (klasik) bir hata oluşursa, ilk işlem yolunuz error.log'a bakmak olmalıdır. Bu dosya, hatanızı anlamanıza ve düzeltmenize yardımcı olacak yararlı hata iletileri içerebilir. error.log'u görüntülemek için, hatayı içeren Yürüt [R Komut Dosyası'nın][execute-r-script] **özellikler bölmesindeki** hata **günlüğünü** görüntüle'yi seçin.

Örneğin, bir [Execute R Script][execute-r-script] modülünde tanımlanmamış bir değişken y ile aşağıdaki R kodunu çalıştırdım:

```R
x <- 1.0
z <- x + y
```

Bu kod yürütülmese başarısız olur ve hata durumuna neden olur. **Özellikler bölmesinde** **Görünüm hata günlüğü** seçilmesi Şekil 2'de gösterilen ekranı üretir.

  ![Hata iletisi açılır](./media/r-quickstart/fig2.png)

*Şekil 2. Hata iletisi açılır.*

Görünüşe göre R hata mesajını görmek için çıktı.log'a bakmamız gerekiyor. Yürüt [R Komut Dosyasını][execute-r-script] seçin ve ardından sağdaki **özellikler bölmesindeki** Görünüm **çıktısı.log** öğesini seçin. Yeni bir tarayıcı penceresi açılır ve aşağıdakileri görüyorum.

    [Critical]     Error: Error 0063: The following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found


    object 'y' not found
    ----------- End of error message from R -----------

Bu hata iletisi hiçbir sürpriz içerir ve açıkça sorunu tanımlar.

R'deki herhangi bir nesnenin değerini incelemek için, bu değerleri output.log dosyasına yazdırabilirsiniz. Nesne değerlerini inceleme kuralları, etkileşimli Bir R oturumundakiyle temelde aynıdır. Örneğin, bir satıra bir değişken adı yazarsanız, nesnenin değeri output.log dosyasına yazdırılır.  

#### <a name="packages-in-machine-learning-studio-classic"></a>Machine Learning Studio Paketleri (klasik)

Studio 350'den fazla önceden yüklenmiş R dil paketleri ile birlikte gelir. Önceden yüklenmiş paketlerin listesini almak için [Yürüt R Script][execute-r-script] modülünde aşağıdaki kodu kullanabilirsiniz.

```R
data.set <- data.frame(installed.packages())
maml.mapOutputPort("data.set")
```

Şu anda bu kodun son satırını anlamıyorsanız, okumaya devam edin. Bu makalenin geri kalanında biz kapsamlı Studio (klasik) ortamda R kullanarak tartışacağız.

### <a name="introduction-to-rstudio"></a>RStudio'ya Giriş

RStudio R için yaygın olarak kullanılan bir IDE olduğunu. Bu kılavuzda kullanılan R kodunun bazılarını düzenlemek, test etmek ve hata ayıklamak için RStudio'yu kullanacağım. R kodu test edilip hazır olduğunda, RStudio editöründen Machine Learning Studio (klasik) [Execute R Script][execute-r-script] modülüne kesip yapıştırabilirsiniz.  

Masaüstü makinenizde R programlama dili yüklü yoksa, şimdi yapmanızı öneririm. Açık kaynak R dili ücretsiz indirme Kapsamlı R Arşiv Ağı (CRAN) mevcuttur. [https://www.r-project.org/](https://www.r-project.org/) Windows, Mac OS ve Linux/UNIX için indirmeler mevcuttur. Yakındaki bir ayna seçin ve indirme yönergeleriizleyin. Buna ek olarak, CRAN yararlı analitik ve veri işleme paketleri bir zenginlik içerir.

RStudio'da yeniyseniz, masaüstü sürümünü indirip yüklemeniz gerekir. Windows, Mac OS ve Linux/UNIX için RStudio indirmelerini http://www.rstudio.com/products/RStudio/. Masaüstü makinenize RStudio yüklemek için verilen yönergeleri izleyin.  

RStudio için bir öğretici giriş [RStudio IDE kullanarak](https://support.rstudio.com/hc/sections/200107586-Using-RStudio)mevcuttur.

Ben Kılavuzu [RStudio belgeleri](#appendixa) aşağıda RStudio kullanarak bazı ek bilgiler sağlar.  

## <a name="get-data-in-and-out-of-the-execute-r-script-module"></a><a id="scriptmodule"></a>Execute R Script modülüne veri girme ve çıkarma

Bu [bölümde, Execute R Script][execute-r-script] modülüne verileri nasıl girip çıkaracağınızı tartışacağız. [Execute R Script][execute-r-script] modülüne okunan ve çıkan çeşitli veri türlerinin nasıl işleyeceğini gözden geçireceğiz.

Bu bölümün tam kodu [MachineLearningSamples-Notebooks/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples)bulunmaktadır.

### <a name="load-and-check-data-in-machine-learning-studio-classic"></a>Machine Learning Studio'da verileri yükleyin ve kontrol edin (klasik)

#### <a name="load-the-dataset"></a><a id="loading"></a>Veri kümesini yükleme

**Csdairydata.csv** dosyasını Azure Machine Learning Studio'ya (klasik) yükleyerek başlayacağız.

1. Azure Machine Learning Studio (klasik) ortamınızı başlatın.
1. Ekranınızın sol alt noktasında **+ Yenİ'yi** seçin ve **Dataset'i**seçin.
1. **Yerel Dosyadan**seçin ve ardından dosyayı seçmek için **Gözatın.**
1. Veri kümesi nin türü olarak **üstbilgili (.csv) Genel CSV dosyasını** seçtiğinizden emin olun.
1. Onay işaretini seçin.
1. Veri kümesi yüklendikten **sonra, Veri Kümeleri** sekmesini seçerek yeni veri kümesini görmeniz gerekir.  

#### <a name="create-an-experiment"></a>Deneme oluşturma

Artık Machine Learning Studio'da (klasik) bazı verilere sahip olduğumuza göre, analizi yapmak için bir deney oluşturmamız gerekiyor.  

1. Sol altta **+ Yenİ'yi** seçin ve **Deney'i**seçin, ardından **Boş Deney'i**seçin.
1. Sayfanın üst kısmında **... başlığında oluşturulan Deneme'yi** seçerek ve değiştirerek denemenizi adlandırabilirsiniz. Örneğin, **CA Süt Analizi**olarak değiştirerek.
1. Deneme sayfasının solunda, **Kaydedilen Veri Kümelerini**ve ardından **Veri Kümelerimi**genişletin. Daha önce yüklediğiniz **cadairydata.csv'yi** görmelisiniz.
1. **Csdairydata.csv veri kümesini** deneyin üzerine sürükleyin ve bırakın.
1. Sol bölmenin üst kısmındaki **Arama deneme öğeleri** [kutusunda, R Komut Dosyasını Çalıştır][execute-r-script]yazın. Modülün arama listesinde göründüğünü görürsünüz.
1. Execute R [Script][execute-r-script] modülünü paletinizin üzerine sürükleyin ve bırakın.  
1. **Csdairydata.csv veri kümesinin** çıktısını [Execute R Script'in][execute-r-script]en sol girişine **(Dataset1)** bağlayın.
1. **'Kaydet'i seçmeyi unutmayın!**  

Bu noktada deneyiniz Şekil 3'e benzer.

![Dataset ve Execute R Script modülü ile CA Süt Analizi deneyi](./media/r-quickstart/fig3.png)

*Şekil 3. Dataset ve Execute R Script modülü ile CA Süt Analizi denemesi.*

#### <a name="check-on-the-data"></a>Verileri kontrol edin

Deneyimize yüklediğimiz verilere bir göz atalım. Denemede, **cadairydata.csv veri kümesinin** çıktısını seçin ve **visualize'ı**seçin. Şekil 4 gibi bir şey görmelisin.  

![cadairydata.csv veri kümesinin özeti](./media/r-quickstart/fig4.png)

*Şekil 4. Cadairydata.csv veri kümesinin özeti.*

Bu görünümde yararlı bilgiler bir sürü bakın. Bu veri kümesinin ilk birkaç satırını görebiliriz. Bir sütun seçersek, İstatistikler bölümü sütun hakkında daha fazla bilgi gösterir. Örneğin, Özellik Türü satırı bize sütuna atanan veri türlerini Azure Machine Learning Studio (klasik) gösterir. Biz herhangi bir ciddi iş yapmaya başlamadan önce bu gibi hızlı bir görünüm olması iyi bir akıl sağlığı kontrol etmektir.

### <a name="first-r-script"></a>İlk R komut dosyası

Azure Machine Learning Studio (klasik) içinde denemeler yapmak için basit bir ilk R komut dosyası oluşturalım. Ben oluşturduk ve RStudio aşağıdaki komut test etti.  

```R
## Only one of the following two lines should be used
## If running in Machine Learning Studio (classic), use the first line with maml.mapInputPort()
## If in RStudio, use the second line with read.csv()
cadairydata <- maml.mapInputPort(1)
# cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
str(cadairydata)
pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
## The following line should be executed only when running in
## Azure Machine Learning Studio (classic)
maml.mapOutputPort('cadairydata')
```

Şimdi bu komut dosyasını Azure Machine Learning Studio'ya (klasik) aktarmam gerekiyor. Kesip yapıştırabilirim. Ancak, bu durumda, bir zip dosyası üzerinden benim R komut aktarımı olacaktır.

### <a name="data-input-to-the-execute-r-script-module"></a>Execute R Script modülüne veri girişi

[Execute R Script][execute-r-script] modülüne girişlere bir göz atalım. Bu örnekte, Kaliforniya süt ürünleri verilerini [Execute R Script][execute-r-script] modülünde okuyacağız.  

[Execute R Script][execute-r-script] modülü için üç olası giriş vardır. Uygulamanıza bağlı olarak bu girdilerden herhangi birini veya tümlerini kullanabilirsiniz. Ayrıca hiç giriş alan bir R komut dosyası kullanmak da son derece makuldür.  

Soldan sağa giden bu girdilerin her birine bakalım. İmlecinizi girişin üzerine yerleştirerek ve araç ipucunu okuyarak girişlerin her birinin adlarını görebilirsiniz.  

#### <a name="script-bundle"></a>Komut Dosyası Paketi

Komut Dosyası Paketi girişi, bir zip dosyasının içeriğini [Execute R Script][execute-r-script] modülüne geçirmenizi sağlar. Zip dosyasının içeriğini R kodunuza okumak için aşağıdaki komutlardan birini kullanabilirsiniz.

```R
source("src/yourfile.R") # Reads a zipped R script
load("src/yourData.rdata") # Reads a zipped R data file
```

> [!NOTE]
> Azure Machine Learning Studio (klasik), zip'teki dosyaları src/ dizinde gibi ele alırsa, bu nedenle dosya adlarınızı bu dizin adı ile öne eklemeniz gerekir. `yourfile.R` Örneğin, zip dosyaları içeriyorsa `yourData.rdata` ve zip kökünde, bu adres `src/yourfile.R` `src/yourData.rdata` e-posta `load`ve ne zaman ve . `source`

[Veri kümesini](#loading)Yükleyin'de veri kümelerini yüklemeyi zaten tartıştık. Önceki bölümde gösterilen R komut dosyasını oluşturduktan ve test ettikten sonra aşağıdakileri yapın:

1. R komut dosyasını bir . R dosyası. Senaryo dosyama "basit çizim" diyorum. R". İşte içindekiler.

   ```R
   ## Only one of the following two lines should be used
   ## If running in Machine Learning Studio (classic), use the first line with maml.mapInputPort()
   ## If in RStudio, use the second line with read.csv()
   cadairydata <- maml.mapInputPort(1)
   # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
   str(cadairydata)
   pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
   ## The following line should be executed only when running in
   ## Azure Machine Learning Studio (classic)
   maml.mapOutputPort('cadairydata')
   ```

1. Bir zip dosyası oluşturun ve komut dosyanızı bu zip dosyasına kopyalayın. Windows'da, dosyayı sağ tıklayabilir ve **Gönder'i**ve ardından **Sıkıştırılmış klasörü**seçebilirsiniz. Bu "simpleplot içeren yeni bir zip dosyası oluşturacaktır. R" dosyası.

1. Dosyanızı Azure Machine Learning Studio'daki (klasik) **veri kümelerine** ekleyin ve türünü **zip**olarak belirtin. Artık veri kümelerinizde zip dosyasını görmeniz gerekir.

1. Sürükleyin ve **ML Studio (klasik) tuval**üzerine veri **kümelerinden** zip dosyası bırakın.

1. **Zip veri** simgesinin çıktısını [Execute R Script][execute-r-script] modülünün Komut Dosyası **Paketi** girişine bağlayın.

1. Execute `source()` [R Script][execute-r-script] modülüiçin kod penceresine zip dosya adınız olan işlevi yazın. Benim durumumda `source("src/simpleplot.R")`yazdım.  

1. **Kaydet'i**seçtiğinizden emin olun.

Bu adımlar tamamlandıktan sonra, [Yürütme R Script][execute-r-script] modülü deneme çalıştırıldığında zip dosyasında R komut dosyası yürütecektir. Bu noktada deneyiniz Şekil 5'e benzer.

![Sıkıştırılmış R komut dosyalarını kullanarak deneme](./media/r-quickstart/fig6.png)

*Şekil 5. Sıkıştırılmış R komut dosyalarını kullanarak deneme.*

#### <a name="dataset1"></a>Veri kümesi1

Dataset1 girişini kullanarak dikdörtgen bir veri tablosunu R kodunuza geçirebilirsiniz. Basit komut dosyamızda `maml.mapInputPort(1)` işlev, bağlantı noktası 1'deki verileri okur. Bu veriler daha sonra kodunuzdaki bir veri çerçevesi değişken adına atanır. Basit komut dosyamızda ilk kod satırı atamayı gerçekleştirir.

```R
cadairydata <- maml.mapInputPort(1)
```

**Çalıştır** düğmesini seçerek denemenizi gerçekleştirin. Yürütme bittiğinde, [Yürüt R Script][execute-r-script] modülünü seçin ve ardından özellikler bölmesindeki çıkış **günlüğünü** görüntüle'yi seçin. Tarayıcınızda output.log dosyasının içeriğini gösteren yeni bir sayfa görünmelidir. Aşağı kaydırdığınızda aşağıdaki gibi bir şey görmeniz gerekir.

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

Sayfanın daha aşağısında sütunlar hakkında daha ayrıntılı bilgiler yer alıyor ve bu bilgiler aşağıdakigibi görünecektir.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput]
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput]
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month            : chr  "Jan" "Feb" "Mar" "Apr" ...
    [ModuleOutput]
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput]
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput]
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput]
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...

Bu sonuçlar çoğunlukla beklendiği gibi, 228 gözlemler ve veri çerçevesinde 9 sütun ile. Sütun adlarını, R veri türünü ve her sütunun bir örneğini görebiliriz.

> [!NOTE]
> Aynı yazdırılmış [çıktı, Execute R Script][execute-r-script] modülünün R Aygıt çıkışından rahatlıkla elde edilebilir. Sonraki bölümde Execute R [Script][execute-r-script] modülünün çıktılarını tartışacağız.  

#### <a name="dataset2"></a>Dataset2

Dataset2 girişinin davranışı Dataset1 ile aynıdır. Bu girişi kullanarak R kodunuza ikinci bir dikdörtgen veri tablosu geçirebilirsiniz. Bu `maml.mapInputPort(2)`verileri aktarmak için bağımsız değişken 2 işlevi kullanılır.  

### <a name="execute-r-script-outputs"></a>R Script çıktılarını çalıştırma

#### <a name="output-a-dataframe"></a>Veri çerçevesi çıktısı

`maml.mapOutputPort()` İşlevi kullanarak Sonuç Dataset1 bağlantı noktası üzerinden Bir R veri çerçevesinin içeriğini dikdörtgen tablo olarak çıktıyapabilirsiniz. Bizim basit R komut dosyasında bu aşağıdaki satırda gerçekleştirilir.

```
maml.mapOutputPort('cadairydata')
```

Denemeyi çalıştırdıktan sonra, Sonuç Veri Kümesi1 çıkış bağlantı noktasını seçin ve ardından **Visualize'ı**seçin. Şekil 6 gibi bir şey görmelisin.

![Kaliforniya süt ürünleri verilerinin çıktısının görselleştirilmesi](./media/r-quickstart/fig7.png)

*Şekil 6. Kaliforniya süt ürünleri verilerinin çıktısının görselleştirilmesi.*

Bu çıktı, tam olarak beklediğimiz gibi, giriş ile aynı görünüyor.  

### <a name="r-device-output"></a>R Cihaz çıkışı

[Execute R Script][execute-r-script] modülünün Aygıt çıktısı iletiler ve grafik çıktısı içerir. R'den gelen standart çıktı ve standart hata iletileri R Device çıkış bağlantı noktasına gönderilir.  

R Aygıt çıktısını görüntülemek için bağlantı noktasını seçin ve ardından **Visualize'da**. Şekil 7'deki R komut dosyasından standart çıktı ve standart hatayı görüyoruz.

![R Aygıt bağlantı noktasından standart çıktı ve standart hata](./media/r-quickstart/fig8.png)

*Şekil 7. R Device bağlantı noktasından standart çıktı ve standart hata.*

Aşağı doğru kaydırılırken Şekil 8'deki R komut dosyamızdan grafik çıktısını görüruz.  

![R Device bağlantı noktasından grafik çıkışı](./media/r-quickstart/fig9.png)

*Şekil 8. R Device bağlantı noktasından grafik çıkışı.*  

## <a name="data-filtering-and-transformation"></a><a id="filtering"></a>Veri filtreleme ve dönüştürme

Bu bölümde, Kaliforniya süt ürünleri verileri üzerinde bazı temel veri filtreleme ve dönüştürme işlemleri gerçekleştireceğiz. Bu bölümün sonunda, analitik bir model oluşturmak için uygun bir biçimde veri olacaktır.  

Daha spesifik olarak, bu bölümde birkaç yaygın veri temizleme ve dönüştürme görevi gerçekleştireceğiz: tür dönüşümü, veri çerçevelerine filtreleme, yeni hesaplanmış sütunekleme ve değer dönüşümleri. Bu arka plan, gerçek dünya sorunlarında karşılaşılan birçok varyasyonla başa çıkmanıza yardımcı olmalıdır.

Bu bölümün tam R kodu [MachineLearningSamples-Notebooks/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples)mevcuttur.

### <a name="type-transformations"></a>Tür dönüşümleri

Artık California süt ürünleri verilerini [Execute R Script][execute-r-script] modülündeki R koduna okuyabildiğimize göre, sütunlarda bulunan verilerin amaçlanan tür ve biçime sahip olduğundan emin olmalıyız.  

R dinamik olarak yazılan bir dildir, bu da veri türlerinin gerektiğinde birinden diğerine zorlandığı anlamına gelir. R atomik veri türleri sayısal, mantıksal ve karakter içerir. Faktör türü, kategorik verileri sıkıştırmak için kullanılır. Aşağıdaki ileri [okuma](#appendixb) referanslar veri türleri hakkında çok daha fazla bilgi bulabilirsiniz.

Tabular veriler harici bir kaynaktan R'ye okunduğunda, sütunlarda ortaya çıkan türleri denetlemek her zaman iyi bir fikirdir. Bir tür karakter sütunu isteyebilirsiniz, ancak çoğu durumda bu faktör veya tam tersi olarak gösterecektir. Diğer durumlarda sayısal olması gerektiğini düşündüğünüz bir sütun, kayan nokta sayısı olarak 1,23 yerine '1,23' gibi karakter verileriyle temsil edilir.  

Neyse ki, eşleme mümkün olduğu sürece, bir türü diğerine dönüştürmek kolaydır. Örneğin, 'Nevada'yı sayısal bir değere dönüştüremezsiniz, ancak bunu bir faktöre (kategorik değişken) dönüştürebilirsiniz. Başka bir örnek olarak, sayısal 1'i '1' karakterine veya bir faktöre dönüştürebilirsiniz.  

Bu dönüşümlerden herhangi biri için sözdizimi basittir: `as.datatype()`. Bu tür dönüştürme işlevleri aşağıdakileri içerir.

* `as.numeric()`
* `as.character()`
* `as.logical()`
* `as.factor()`

Önceki bölüme koyduğumuz sütunların veri türlerine bakıldığında: tür karakteri olan 'Ay' etiketli sütun dışında tüm sütunlar sayısal dır. Bunu bir faktöre dönüştürelim ve sonuçları test edelim.  

Scatterplot matrisini oluşturan satırı sildim ve 'Ay' sütununa bir faktöre dönüştüren bir satır ekledim. Benim deneme de sadece kesip [R Komut Dosyası][execute-r-script] Modülü kod penceresine R kodu yapıştırın. Zip dosyasını güncelleştirip Azure Machine Learning Studio'ya (klasik) yükleyebilirsiniz, ancak bu birkaç adım dan oluşur.  

```R
## Only one of the following two lines should be used
## If running in Machine Learning Studio (classic), use the first line with maml.mapInputPort()
## If in RStudio, use the second line with read.csv()
cadairydata <- maml.mapInputPort(1)
# cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
## Ensure the coding is consistent and convert column to a factor
cadairydata$Month <- as.factor(cadairydata$Month)
str(cadairydata) # Check the result
## The following line should be executed only when running in
## Azure Machine Learning Studio (classic)
maml.mapOutputPort('cadairydata')
```

Bu kodu yürütelim ve R komut dosyasının çıkış günlüğüne bakalım. Günlükteki ilgili veriler Şekil 9'da gösterilmiştir.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 14 levels "Apr","April",..: 6 5 9 1 11 8 7 3 14 13 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Şekil 9. Bir faktör değişkeni ile veri çerçevesinin özeti.*

Ay için türü şimdi '**Faktör w / 14 düzeyleri**'. Yıl içinde sadece 12 ay olduğu için bu bir sorundur. Ayrıca, Sonuç Veri Kümesi bağlantı noktasının **Visualize'daki** türünün '**Kategorik**' olup olmadığını da kontrol edebilirsiniz.

Sorun şu ki, 'Ay' sütunu sistematik olarak kodlanmamıştır. Bazı durumlarda bir ay Nisan denir ve bazılarında Nisan olarak kısaltılır. Dizeyi 3 karaktere kırparak bu sorunu çözebiliriz. Kod satırı şimdi aşağıdaki gibi görünür:

```R
## Ensure the coding is consistent and convert column to a factor
cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))
```

Denemeyi yeniden çalıştırın ve çıktı günlüğünü görüntüleyin. Beklenen sonuçlar Şekil 10'da gösterilmiştir.  

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Şekil 10. Doğru faktör düzeyleri sayısına sahip veri çerçevesinin özeti.*

Faktör değişkenimiz artık istenilen 12 seviyesine sahip.

### <a name="basic-data-frame-filtering"></a>Temel veri çerçevesi filtreleme

R veri çerçeveleri güçlü filtreleme özelliklerini destekler. Veri kümeleri, satır veya sütunlarda mantıksal filtreler kullanılarak alt ayarlanabilir. Çoğu durumda, karmaşık filtre ölçütleri gerekir. Aşağıda [daha fazla okuma](#appendixb) da referanslar veri çerçeveleri filtreleme geniş örnekler içerir.  

Veri setimizde yapmamız gereken bir filtreleme parçası var. Cadairydata veri çerçevesindeki sütunlara bakarsanız, iki gereksiz sütun görürsünüz. İlk sütun da çok yararlı olmayan bir satır numarası tutar. İkinci sütun, Year.Month, gereksiz bilgiler içerir. Aşağıdaki R kodunu kullanarak bu sütunları kolayca dışlayabiliriz.

> [!NOTE]
> Şu andan itibaren bu bölümde, ben sadece ben Yürüt [R Script][execute-r-script] modülü ekleyerek ek kod gösterecektir. Ben **işlevden** `str()` önce her yeni satır ekleyeceğiz. Azure Machine Learning Studio'daki (klasik) sonuçlarımı doğrulamak için bu işlevi kullanıyorum.

[Execute R Script][execute-r-script] modülündeki R koduma aşağıdaki satırı ekliyorum.

```R
# Remove two columns we do not need
cadairydata <- cadairydata[, c(-1, -2)]
```

Denemenizde bu kodu çalıştırın ve çıktı günlüğünden sonucu kontrol edin. Bu sonuçlar Şekil 11'de gösterilmiştir.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  7 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Şekil 11. İki sütun kaldırılmış veri çerçevesinin özeti.*

İyi haber! Beklenen sonuçları alıyoruz.

### <a name="add-a-new-column"></a>Yeni bir sütun ekleme

Zaman serisi modelleri oluşturmak için zaman serisinin başlangıcından bu yana ayları içeren bir sütunolması uygun olacaktır. Yeni bir sütun 'Month.Count' oluşturacağız.

Kodu düzenlemeye yardımcı olmak için ilk basit `num.month()`fonksiyonumuzu oluşturacağız. Daha sonra bu işlevi veri çerçevesinde yeni bir sütun oluşturmak için uygulayacağız. Yeni kod aşağıdaki gibidir.

```R
## Create a new column with the month count
## Function to find the number of months from the first
## month of the time series
num.month <- function(Year, Month) {
  ## Find the starting year
  min.year  <- min(Year)

  ## Compute the number of months from the start of the time series
  12 * (Year - min.year) + Month - 1
}

## Compute the new column for the dataframe
cadairydata$Month.Count <- num.month(cadairydata$Year, cadairydata$Month.Number)
```

Şimdi güncelleştirilmiş denemeyi çalıştırın ve sonuçları görüntülemek için çıktı günlüğünü kullanın. Bu sonuçlar Şekil 12'de gösterilmiştir.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Şekil 12. Ek sütunile veri çerçevesinin özeti.*

Görünüşe göre her şey işe yarıyor. Veri çerçevemizde beklenen değerlere sahip yeni sütunumuz var.

### <a name="value-transformations"></a>Değer dönüşümleri

Bu bölümde, veri çerçevemizin bazı sütunlarında değerler üzerinde bazı basit dönüşümler gerçekleştireceğiz. R dili neredeyse rasgele değer dönüşümlerini destekler. Aşağıda [daha fazla okuma](#appendixb) referansları geniş örnekler içerir.

Veri çerçevemizin özetlerinde yer alan değerlere bakarsanız burada garip bir şey görmeniz gerekir. Kaliforniya'da sütten daha fazla dondurma mı üretiliyor? Hayır, tabii ki hayır, bu hiç mantıklı değil, bu gerçek bazı larımız için üzücü olabilir. Birimler farklı. Fiyat ABD sterlini birimleri, süt 1 M ABD sterlini birimleri, dondurma 1.000 ABD galon birimleri ve süzme peynir birimleri 1.000 ABD pound bulunmaktadır. Dondurma galon başına yaklaşık 6,5 kilo ağırlığında varsayarsak, biz kolayca 1.000 £ eşit birimlerde böylece bu değerleri dönüştürmek için çarpma yapabilirsiniz.

Tahmin modelimiz için, bu verilerin eğilim ve mevsimsel ayarlaması için çarpan bir model kullanıyoruz. Günlük dönüşümü, bu işlemi basitleştirerek doğrusal bir model kullanmamıza olanak tanır. Günlük dönüşümlerini çarpanın uygulandığı aynı fonksiyonda uygulayabiliriz.

Aşağıdaki kodda, yeni bir işlev `log.transform()`tanımlıyorum ve sayısal değerleri içeren satırlara uyguluyorum. R `Map()` işlevi, `log.transform()` işlevi veri çerçevesinin seçili sütunlarına uygulamak için kullanılır. `Map()``apply()` benzerdir, ancak işleve birden fazla bağımsız değişken listesine izin verir. Çarpanların listesinin işleve ikinci bağımsız `log.transform()` değişkeni sağladığını unutmayın. İşlev, `na.omit()` veri çerçevesinde eksik veya tanımlanmamış değerlere sahip olmadığımızı sağlamak için bir temizleme parçası olarak kullanılır.

```R
log.transform <- function(invec, multiplier = 1) {
  ## Function for the transformation, which is the log
  ## of the input value times a multiplier

  warningmessages <- c("ERROR: Non-numeric argument encountered in function log.transform",
                       "ERROR: Arguments to function log.transform must be greate than zero",
                       "ERROR: Aggurment multiplier to funcition log.transform must be a scaler",
                       "ERROR: Invalid time seies value encountered in function log.transform"
                       )

  ## Check the input arguments
  if(!is.numeric(invec) | !is.numeric(multiplier)) {warning(warningmessages[1]); return(NA)}  
  if(any(invec < 0.0) | any(multiplier < 0.0)) {warning(warningmessages[2]); return(NA)}
  if(length(multiplier) != 1) {{warning(warningmessages[3]); return(NA)}}

  ## Wrap the multiplication in tryCatch
  ## If there is an exception, print the warningmessage to
  ## standard error and return NA
  tryCatch(log(multiplier * invec),
           error = function(e){warning(warningmessages[4]); NA})
}


## Apply the transformation function to the 4 columns
## of the dataframe with production data
multipliers  <- list(1.0, 6.5, 1000.0, 1000.0)
cadairydata[, 4:7] <- Map(log.transform, cadairydata[, 4:7], multipliers)

## Get rid of any rows with NA values
cadairydata <- na.omit(cadairydata)  
```

İşlevde `log.transform()` biraz oluyor. Bu kodun çoğu bağımsız değişkenlerle ilgili olası sorunları denetlemekte veya hesaplamalar sırasında ortaya çıkabilecek özel durumlarla başa çıkmaktadır. Hesaplamaları yalnızca birkaç satır yapar.

Savunma programlamanın amacı, işlemenin devam etmesini engelleyen tek bir işlevin başarısız olmasını önlemektir. Uzun süren bir analiz ani bir başarısızlık kullanıcılar için oldukça sinir bozucu olabilir. Bu durumu önlemek için, zararı akış aşağı işlemeyle sınırlandıracak varsayılan iade değerleri seçilmelidir. Kullanıcıları bir şeylerin ters gittiği konusunda uyarmak için bir ileti de üretilir.

Eğer R savunma programlama için kullanılan değilseniz, tüm bu kod biraz ezici görünebilir. Sana ana basamakları ilerlerken:

1. Dört iletiden oluşan bir vektör tanımlanır. Bu iletiler, bu kodla oluşabilecek olası hatalar ve özel durumlar hakkında bilgi iletmek için kullanılır.
2. Her dava için NA değeri iade ediyorum. Daha az yan etkisi olabilir birçok olasılık vardır. Sıfırlardan oluşan bir vektör veya örneğin orijinal giriş vektörü döndürebilirim.
3. Denetimler işlevin bağımsız değişkenleri üzerinde çalıştırılır. Her durumda, bir hata algılanırsa, varsayılan değer döndürülür ve `warning()` işlev tarafından bir ileti üretilir. Ben ikinci `warning()` yürütme `stop()` sona erecek gibi yerine kullanıyorum, tam olarak ne önlemek için çalışıyorum. Bu durumda işlevsel bir yaklaşım karmaşık ve belirsiz görünüyordu, bir prosedür tarzında bu kodu yazdım unutmayın.
4. Günlük hesaplamaları, özel durumların `tryCatch()` işleme ani bir durma ya yıltırması için sarılır. R `tryCatch()` fonksiyonları tarafından yükseltilen çoğu hata olmadan sadece bunu yapar bir durdurma sinyali, neden.

Denemenizde bu R kodunu çalıştırın ve output.log dosyasındaki yazdırılan çıktıya bir göz atın. Şimdi şekil 13'te gösterildiği gibi, günlükteki dört sütunun dönüştürülmüş değerlerini görürsünüz.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Şekil 13. Veri çerçevesinde dönüştürülen değerlerin özeti.*

Değerlerin değiştiğini görüyoruz. Süt üretimi artık diğer tüm süt ürünleri üretimini büyük ölçüde aşıyor, biz şimdi bir günlük ölçek arıyoruz hatırlatarak.

Bu noktada verilerimiz temizlenir ve bazı modelleme için hazırız. [Execute R Script][execute-r-script] modülümüzün Sonuç Veri Seti çıktısının görselleştirme özetine baktığınızda, 'Ay' sütununun yine istediğimiz gibi 12 benzersiz değere sahip 'Kategorik' olduğunu göreceksiniz.

## <a name="time-series-objects-and-correlation-analysis"></a><a id="timeseries"></a>Zaman serisi nesneleri ve korelasyon analizi

Bu bölümde birkaç temel R zaman serisi nesnesini inceleyeceğiz ve bazı değişkenler arasındaki bağıntıları analiz edeceğiz. Amacımız, birkaç gecikmede çift yönlü korelasyon bilgilerini içeren bir veri çerçevesi çıkarmaktır.

Bu bölümün tam R kodu [MachineLearningSamples-Notebooks/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples)bulunmaktadır.

### <a name="time-series-objects-in-r"></a>R'deki zaman serisi nesneleri

Daha önce de belirtildiği gibi, zaman serileri zamana göre dizilenmiş bir veri değerleri dizisidir. R zaman serisi nesneleri, zaman dizini oluşturmak ve yönetmek için kullanılır. Zaman serisi nesneleri kullanmanın çeşitli avantajları vardır. Zaman serisi nesneleri, nesnenin içinde kapsüllenmiş zaman serisi dizi değerlerini yönetmenin birçok ayrıntısından sizi kurtarır. Buna ek olarak, zaman serisi nesneleri çizim, baskı, modelleme, vb için birçok zaman serisi yöntemleri kullanmanıza olanak sağlar.

POSIXct zaman serisi sınıfı yaygın olarak kullanılır ve nispeten basittir. Bu zaman serisi sınıf çağın başlangıcından itibaren zaman ölçer, Ocak 1, 1970. Bu örnekte POSIXct zaman serisi nesneleri kullanacağız. Diğer yaygın olarak kullanılan R zaman serisi nesne sınıfları hayvanat bahçesi ve xts, genişletilebilir zaman serisi içerir.

### <a name="time-series-object-example"></a>Zaman serisi nesne örneği

Örneğimizle başlayalım. Yeni **bir** [Execute R Script][execute-r-script] modülünü sürükleyin ve deneyinize bırakın. Varolan [Execute R Script][execute-r-script] modülünün Sonuç Dataset1 çıkış bağlantı noktasını yeni [Execute R Script][execute-r-script] modülünün Dataset1 giriş bağlantı noktasına bağlayın.

Ben ilk örnekler için yaptığı gibi, biz örnek ile ilerleme olarak, bazı noktalarda ben her adımda R kodunun sadece artımlı ek satırları gösterecektir.  

#### <a name="reading-the-dataframe"></a>Veri çerçevesini okuma

İlk adım olarak, bir veri çerçevesi içinde okuyalım ve beklenen sonuçları aldığımızdan emin olalım. Aşağıdaki kod işi yapmalıdır.

```R
# Comment the following if using RStudio
cadairydata <- maml.mapInputPort(1)
str(cadairydata) # Check the results
```

Şimdi, deneyi yürüt. Yeni Execute R Script şeklinin günlüğü Şekil 14 gibi görünmelidir.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...

*Şekil 14. Execute R Script modülündeki veri çerçevesinin özeti.*

Bu veriler beklenen tür ve biçimdir. 'Ay' sütununun tür faktörü olduğunu ve beklenen düzey sayısına sahip olduğunu unutmayın.

#### <a name="creating-a-time-series-object"></a>Zaman serisi nesnesi oluşturma

Veri çerçevemize bir zaman serisi nesnesi eklememiz gerekiyor. Geçerli kodu, posiXct sınıfının yeni bir sütunu ekleyen aşağıdakilerle değiştirin.

```R
# Comment the following if using RStudio
cadairydata <- maml.mapInputPort(1)

## Create a new column as a POSIXct object
Sys.setenv(TZ = "PST8PDT")
cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

str(cadairydata) # Check the results
```

Şimdi, kütüğü kontrol et. Şekil 15'e benzemeli.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

*Şekil 15. Bir zaman serisi nesnesi ile veri çerçevesinin özeti.*

Özetten yeni sütunun aslında POSIXct sınıfı olduğunu görebiliriz.

### <a name="exploring-and-transforming-the-data"></a>Verileri keşfetme ve dönüştürme

Bu veri kümesindeki bazı değişkenleri inceleyelim. Scatterplot matrisi hızlı bir görünüm üretmek için iyi bir yoldur. Önceki R kodundaki `str()` işlevi aşağıdaki satırla değiştiriyorum.

```R
pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")
```

Bu kodu çalıştırın ve ne olacağını görün. R Aygıt bağlantı noktasında üretilen çizim Şekil 16'ya benzer olmalıdır.

![Seçili değişkenlerin dağılım grafiği matrisi](./media/r-quickstart/fig17.png)

*Şekil 16. Seçili değişkenlerin dağılım grafiği matrisi.*

Bu değişkenler arasındaki ilişkilerde bazı garip görünümlü bir yapı vardır. Belki de bu, verilerdeki eğilimlerden ve değişkenleri standartlaştıramadığımız gerçeğinden kaynaklanmaktadır.

### <a name="correlation-analysis"></a>Bağıntı analizi

Korelasyon analizini gerçekleştirmek için hem trendi de-trend ihale getirmemiz hem de değişkenleri standartlaştırmamız gerekir. Değişkenleri hem merkezhem de ölçeklendiren R `scale()` fonksiyonunu kullanabiliriz. Bu işlev daha hızlı çalışabilir. Ancak, size R savunma programlama bir örnek göstermek istiyorum.

Aşağıda `ts.detrend()` gösterilen işlev bu işlemlerin her ikisini de gerçekleştirir. Aşağıdaki iki kod satırı verileri eritin ve sonra değerleri standartlaştırın.

```R
ts.detrend <- function(ts, Time, min.length = 3){
  ## Function to de-trend and standardize a time series

  ## Define some messages if they are NULL  
  messages <- c('ERROR: ts.detrend requires arguments ts and Time to have the same length',
                'ERROR: ts.detrend requires argument ts to be of type numeric',
                paste('WARNING: ts.detrend has encountered a time series with length less than', as.character(min.length)),
                'ERROR: ts.detrend has encountered a Time argument not of class POSIXct',
                'ERROR: Detrend regression has failed in ts.detrend',
                'ERROR: Exception occurred in ts.detrend while standardizing time series in function ts.detrend'
  )
  # Create a vector of zeros to return as a default in some cases
  zerovec  <- rep(length(ts), 0.0)

  # The input arguments are not of the same length, return ts and quit
  if(length(Time) != length(ts)) {warning(messages[1]); return(ts)}

  # If the ts is not numeric, just return a zero vector and quit
  if(!is.numeric(ts)) {warning(messages[2]); return(zerovec)}

  # If the ts is too short, just return it and quit
  if((ts.length <- length(ts)) < min.length) {warning(messages[3]); return(ts)}

  ## Check that the Time variable is of class POSIXct
  if(class(cadairydata$Time)[[1]] != "POSIXct") {warning(messages[4]); return(ts)}

  ## De-trend the time series by using a linear model
  ts.frame  <- data.frame(ts = ts, Time = Time)
  tryCatch({ts <- ts - fitted(lm(ts ~ Time, data = ts.frame))},
           error = function(e){warning(messages[5]); zerovec})

  tryCatch( {stdev <- sqrt(sum((ts - mean(ts))^2))/(ts.length - 1)
             ts <- ts/stdev},
            error = function(e){warning(messages[6]); zerovec})

  ts
}  
## Apply the detrend.ts function to the variables of interest
df.detrend <- data.frame(lapply(cadairydata[, 4:7], ts.detrend, cadairydata$Time))

## Plot the results to look at the relationships
pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = df.detrend, main = "Pairwise Scatterplots of detrended standardized time series")
```

İşlevde `ts.detrend()` biraz oluyor. Bu kodun çoğu bağımsız değişkenlerle ilgili olası sorunları denetlemekte veya hesaplamalar sırasında ortaya çıkabilecek özel durumlarla başa çıkmaktadır. Hesaplamaları yalnızca birkaç satır yapar.

Değer dönüşümlerinde savunma programlama nın bir örneğini tartıştık. Her iki hesaplama bloğu da `tryCatch()`. Bazı hatalar için orijinal giriş vektörü döndürmek mantıklı, ve diğer durumlarda, sıfırların bir vektör ü veyorum.  

Trendi yitirme için kullanılan doğrusal regresyonun bir zaman serisi gerilemesi olduğunu unutmayın. Predictor değişkeni bir zaman serisi nesnesidir.  

Tanımlandıktan sonra `ts.detrend()` veri çerçevemizdeki ilgi değişkenlerine uygularız. Veri çerçevesi `lapply()` tarafından oluşturulan sonucu ortaya çıkan listeyi `as.data.frame()`kullanarak zorlamamız gerekir. Savunma yönleri `ts.detrend()`nedeniyle, değişkenlerden birinin işlenememesi diğerlerinin doğru işlenmesini engellemeyececektir.  

Kodun son satırı çift ekilek bir dağılım çizimi oluşturur. R kodu çalıştırdıktan sonra dağılım çiziminin sonuçları Şekil 17'de gösterilmiştir.

![De-trended ve standart laştırılmış zaman serisinin çift yönlü dağılım konusu](./media/r-quickstart/fig18.png)

*Şekil 17. De-trended ve standart zaman serisi nin çift yönlü scatterplot.*

Bu sonuçları Şekil 16'da gösterilenlerle karşılaştırabilirsiniz. Eğilim kaldırıldı ve değişkenler standartlaştırıldı, bu değişkenler arasındaki ilişkilerde çok daha az yapı görüyoruz.

R ccf nesneleri olarak korelasyonhesaplamak için kod aşağıdaki gibidir.

```R
## A function to compute pairwise correlations from a
## list of time series value vectors
pair.cor <- function(pair.ind, ts.list, lag.max = 1, plot = FALSE){
  ccf(ts.list[[pair.ind[1]]], ts.list[[pair.ind[2]]], lag.max = lag.max, plot = plot)
}

## A list of the pairwise indices
corpairs <- list(c(1,2), c(1,3), c(1,4), c(2,3), c(2,4), c(3,4))

## Compute the list of ccf objects
cadairycorrelations <- lapply(corpairs, pair.cor, df.detrend)  

cadairycorrelations
```

Bu kodun çalıştırılmesi Şekil 18'de gösterilen günlüğü üretir.

    [ModuleOutput] Loading objects:
    [ModuleOutput]   port1
    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] [[1]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.148 0.358 0.317 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[2]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.395 -0.186 -0.238 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[3]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.059 -0.089 -0.127 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[4]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.140 0.294 0.293 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[5]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.002 -0.074 -0.124 

*Şekil 18. Çift yönlü korelasyon analizinden ccf nesnelerinin listesi.*

Her gecikme için bir korelasyon değeri vardır. Bu korelasyon değerlerinin hiçbiri önemli olacak kadar büyük değildir. Bu nedenle, her değişkeni bağımsız olarak modelleebileceğimiz sonucuna varabiliriz.

### <a name="output-a-dataframe"></a>Veri çerçevesi çıktısı
R ccf nesnelerinin bir listesi olarak çift yönlü bağıntıları hesapladık. Sonuç Veri Kümesi çıkış bağlantı noktası gerçekten bir veri çerçevesi gerektirdiğinden, bu biraz sorun sunar. Ayrıca, ccf nesnesi kendisi bir liste ve biz bu listenin ilk öğesi sadece değerleri istiyorum, çeşitli gecikmeler de korelasyonlar.

Aşağıdaki kod, kendilerini listeler ccf nesneleri listesinden gecikme değerlerini ayıklar.

```R
df.correlations <- data.frame(do.call(rbind, lapply(cadairycorrelations, '[[', 1)))

c.names <- c("correlation pair", "-1 lag", "0 lag", "+1 lag")
r.names  <- c("Corr Cot Cheese - Ice Cream",
              "Corr Cot Cheese - Milk Prod",
              "Corr Cot Cheese - Fat Price",
              "Corr Ice Cream - Mik Prod",
              "Corr Ice Cream - Fat Price",
              "Corr Milk Prod - Fat Price")

## Build a dataframe with the row names column and the
## correlation data frame and assign the column names
outframe <- cbind(r.names, df.correlations)
colnames(outframe) <- c.names
outframe


## WARNING!
## The following line works only in Azure Machine Learning Studio (classic)
## When running in RStudio, this code will result in an error
#maml.mapOutputPort('outframe')
```

Kodun ilk satırı biraz zor ve bazı açıklamalar bunu anlamanıza yardımcı olabilir. İçten dışa doğru çalışarak şunlar alabiliyoruz:

1. '**[[**' bağımsız değişkeni ile işleç '**1**' ccf nesne si listesinin ilk öğesinden gecikmelerde korelasyon vektörünü seçer.
2. İşlev, `do.call()` `rbind()` işlevi liste döndürür öğeleri `lapply()`üzerinde uygular.
3. İşlev, `data.frame()` bir `do.call()` veri çerçevesi tarafından üretilen sonucu zorlar.

Satır adlarının veri çerçevesinin bir sütununda olduğunu unutmayın. Bunu yapmak, [Yürüt R Komut Dosyası'ndan][execute-r-script]çıktı olduklarında satır adlarını korur.

Kodun çalıştırılmasını, Sonuç Veri Kümesi bağlantı noktasındaki çıktıyı **görselleştirdiğimde** Şekil 19'da gösterilen çıktıyı üretir. Satır adları amaçlandığı gibi ilk sütunda dır.

![Korelasyon analizinden elde edilen sonuçlar](./media/r-quickstart/fig20.png)

*Şekil 19. Korelasyon analizinden elde edilen sonuçlar.*

## <a name="time-series-example-seasonal-forecasting"></a><a id="seasonalforecasting"></a>Zaman serisi örnek: mevsimsel tahmin

Verilerimiz şu anda analiz için uygun bir formda ve değişkenler arasında anlamlı bir korelasyon olmadığını belirledik. Devam edelim ve bir zaman serisi tahmin modeli oluşturalım. Bu modeli kullanarak 2013 yılının 12 ayı için Kaliforniya süt üretimini tahmin edeceğiz.

Tahmin modelimiz iki bileşenden, bir trend bileşenine ve mevsimsel bileşene sahip olacaktır. Tam tahmin bu iki bileşenin ürünüdür. Bu model türü çarpan model olarak bilinir. Alternatif bir katkı modelidir. İlgi değişkenlerine bir günlük dönüşümü uyguladık, bu da bu analizi uygulanabilir kılıyor.

Bu bölümün tam R kodu [MachineLearningSamples-Notebooks/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples)bulunmaktadır.

### <a name="creating-the-dataframe-for-analysis"></a>Analiz için veri çerçevesi oluşturma

Denemenize **yeni** bir [Execute R Script][execute-r-script] modülü ekleyerek başlayın. Mevcut [Execute R Script][execute-r-script] modülünün Sonuç Veri **Seti** çıktısını yeni modülün **Dataset1** girişine bağlayın. Sonuç Şekil 20 gibi bir şey görünmelidir.

![Yeni Execute R Script modülü ile deneme eklendi](./media/r-quickstart/fig21.png)

*Şekil 20. Yeni Execute R Script modülü ile deneme eklendi.*

Yeni tamamladığımız korelasyon analizinde olduğu gibi, POZİXct zaman serisi nesnesi olan bir sütun eklememiz gerekir. Aşağıdaki kod bunu yapar.

```R
# If running in Machine Learning Studio (classic), uncomment the first line with maml.mapInputPort()
cadairydata <- maml.mapInputPort(1)

## Create a new column as a POSIXct object
Sys.setenv(TZ = "PST8PDT")
cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

str(cadairydata)
```

Bu kodu çalıştırın ve günlük bak. Sonuç Şekil 21 gibi görünmelidir.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

*Şekil 21. Veri çerçevesinin bir özeti.*

Bu sonuçla analizlerimize başlamaya hazırız.

### <a name="create-a-training-dataset"></a>Eğitim veri kümesi oluşturma

Veri çerçevesi oluşturulduile birlikte bir eğitim veri kümesi oluşturmamız gerekir. Bu veriler, test veri setimiz olan 2013 yılının son 12 yılı hariç tüm gözlemleri içerecektir. Aşağıdaki kod veri çerçevesini alt kümeler ve süt ürünleri üretiminin ve fiyat değişkenlerinin çizimlerini oluşturur. Daha sonra dört üretim ve fiyat değişkenleri arsalar oluşturun. Anonim bir işlev, çizim için bazı açılımları tanımlamak ve sonra `Map()`diğer iki bağımsız değişkenin listesi üzerinde ' ile yinelemek için kullanılır. Eğer bir for döngü burada iyi çalışmış olacağını düşünüyorsanız, doğru. Ama R işlevsel bir dil olduğu için size işlevsel bir yaklaşım gösteriyorum.

```R
cadairytrain <- cadairydata[1:216, ]

Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
               "Log CA Ice Cream Production, 1000s lb",
               "Log CA Milk Production 1000s lb",
               "Log North CA Milk Milk Fat Price per 1000 lb")

Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)
```

Kodun çalıştırıl, Şekil 22'de gösterilen R Aygıtı çıkışından zaman serisi çizimler imal eder. Zaman ekseninin tarih birimleri içinde olduğunu unutmayın, zaman serisi çizim yönteminin güzel bir yararı.

![California süt üretim ve fiyat verilerinin zaman serisi arsalar ilk](./media/r-quickstart/unnamed-chunk-161.png)

![California süt ürünleri üretimi ve fiyat verilerinin zaman serisi arsalar Ikinci](./media/r-quickstart/unnamed-chunk-162.png)

![California süt ürünleri üretimi ve fiyat verilerinin zaman serisi arsalar Üçüncü](./media/r-quickstart/unnamed-chunk-163.png)

![Kaliforniya süt ürünleri üretimi ve fiyat verilerinin zaman serisi arsalar dördüncü](./media/r-quickstart/unnamed-chunk-164.png)

*Şekil 22. California süt ürünleri üretimi ve fiyat verilerinin zaman serisi çizimleri.*

### <a name="a-trend-model"></a>Bir trend modeli

Bir zaman serisi nesnesi oluşturduktan ve verilere göz attıktan sonra, Kaliforniya süt üretim verileri için bir trend modeli oluşturmaya başlayalım. Bunu bir zaman serisi gerilemesi ile yapabiliriz. Ancak, eğitim verilerinde gözlenen eğilimi doğru bir şekilde modellemek için bir eğimden daha fazlasına ve kesişmeye ihtiyacımız olacağı arsadan açıktır.

Verilerin küçük ölçekli göz önüne alındığında, Ben RStudio eğilim modeli inşa edecek ve daha sonra kesip Azure Machine Learning Studio (klasik) içine ortaya çıkan modeli yapıştırın. RStudio, bu tür etkileşimli analizler için etkileşimli bir ortam sağlar.

İlk denemeolarak, 3'e kadar güçiçeren polinomrere regresyon deneyeceğim. Bu tür modellerin aşırı takılması gibi gerçek bir tehlike vardır. Bu nedenle, yüksek sıralı terimlerönlemek için en iyisidir. İşlev `I()` içeriğinin yorumlanmasını engeller (içeriğini 'olduğu gibi' yorumlar) ve bir regresyon denkleminde kelimenin tam anlamıyla yorumlanmış bir işlev yazmanızı sağlar.

```R
milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
summary(milk.lm)
```

Bu aşağıdakileri oluşturur.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3),
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12667 -0.02730  0.00236  0.02943  0.10586
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.33e+00   1.45e-01   43.60   <2e-16 ***
    ## Time              1.63e-09   1.72e-10    9.47   <2e-16 ***
    ## I(Month.Count^2) -1.71e-06   4.89e-06   -0.35    0.726
    ## I(Month.Count^3) -3.24e-08   1.49e-08   -2.17    0.031 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0418 on 212 degrees of freedom
    ## Multiple R-squared:  0.941,    Adjusted R-squared:  0.94
    ## F-statistic: 1.12e+03 on 3 and 212 DF,  p-value: <2e-16

Bu çıktıdaki`Pr(>|t|)`P değerlerinden kareli terimin önemli olmayabileceğini görebiliriz. Ben kare `update()` terimi bırakarak bu modeli değiştirmek için işlevi kullanacağız.

```R
milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
summary(milk.lm)
```

Bu aşağıdakileri oluşturur.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12597 -0.02659  0.00185  0.02963  0.10696
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.38e+00   4.07e-02   156.6   <2e-16 ***
    ## Time              1.57e-09   4.32e-11    36.3   <2e-16 ***
    ## I(Month.Count^3) -3.76e-08   2.50e-09   -15.1   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0417 on 213 degrees of freedom
    ## Multiple R-squared:  0.941,  Adjusted R-squared:  0.94
    ## F-statistic: 1.69e+03 on 2 and 213 DF,  p-value: <2e-16

Bu daha iyi görünüyor. Tüm terimler önemlidir. Ancak, 2e-16 değeri varsayılan bir değerdir ve çok ciddiye alınmamalıdır.  

Akıl sağlığı testi olarak, gösterilen eğilim eğrisi ile Kaliforniya süt ürünleri üretim verilerinin bir zaman serisi arsa yapalım. Modeli oluşturmak ve bir çizim yapmak için Azure Machine Learning Studio (klasik) [Execute R Script][execute-r-script] modeline (RStudio değil) aşağıdaki kodu ekledim. Sonuç Şekil 23'te gösterilmiştir.

```R
milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)
```

![Trend modeli ile Kaliforniya süt üretim verileri gösterilmiştir](./media/r-quickstart/unnamed-chunk-18.png)

*Şekil 23. Trend modeli ile Kaliforniya süt üretim verileri gösterilmiştir.*

Trend modeli oldukça iyi veri uyuyor gibi görünüyor. Ayrıca, model eğrisinde garip kıpırdanmalar gibi aşırı uyum kanıtı gibi görünmüyor.  

### <a name="seasonal-model"></a>Mevsimlik model

Elinde bir trend modeli ile, biz itmek ve mevsimsel etkileri dahil etmek gerekir. Ay-ay etkisini yakalamak için doğrusal modelde yılın ayını kukla değişken olarak kullanacağız. Bir modele faktör değişkenleri tanıttığınızda, durdurma nın hesaplanmaması gerektiğini unutmayın. Bunu yapmazsanız, formül aşırı belirtilir ve R istenilen faktörlerden birini düşürür, ancak kesme terimini korur.

Tatmin edici bir trend modeline sahip `update()` olduğumuziçin, mevcut modele yeni terimler eklemek için işlevi kullanabiliriz. Güncelleme formülündeki -1 kesme terimini düşürür. Şu an için RStudio devam:

```R
milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
summary(milk.lm2)
```

Bu aşağıdakileri oluşturur.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3) + Month - 1,
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.06879 -0.01693  0.00346  0.01543  0.08726
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## Time              1.57e-09   2.72e-11    57.7   <2e-16 ***
    ## I(Month.Count^3) -3.74e-08   1.57e-09   -23.8   <2e-16 ***
    ## MonthApr          6.40e+00   2.63e-02   243.3   <2e-16 ***
    ## MonthAug          6.38e+00   2.63e-02   242.2   <2e-16 ***
    ## MonthDec          6.38e+00   2.64e-02   241.9   <2e-16 ***
    ## MonthFeb          6.31e+00   2.63e-02   240.1   <2e-16 ***
    ## MonthJan          6.39e+00   2.63e-02   243.1   <2e-16 ***
    ## MonthJul          6.39e+00   2.63e-02   242.6   <2e-16 ***
    ## MonthJun          6.38e+00   2.63e-02   242.4   <2e-16 ***
    ## MonthMar          6.42e+00   2.63e-02   244.2   <2e-16 ***
    ## MonthMay          6.43e+00   2.63e-02   244.3   <2e-16 ***
    ## MonthNov          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## MonthOct          6.37e+00   2.63e-02   241.8   <2e-16 ***
    ## MonthSep          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0263 on 202 degrees of freedom
    ## Multiple R-squared:     1,    Adjusted R-squared:     1
    ## F-statistic: 1.42e+06 on 14 and 202 DF,  p-value: <2e-16

Modelin artık bir kesişme terimi olmadığını ve 12 önemli ay faktörü olduğunu görüyoruz. Tam olarak görmek istediğimiz şey buydu.

Mevsimsel modelin ne kadar iyi çalıştığını görmek için Kaliforniya süt ürünleri üretim verilerinin başka bir zaman serisi arsa yapalım. Modeli oluşturmak ve bir çizim yapmak için Azure Machine Learning Studio 'ya (klasik) [Execute R Script'e][execute-r-script] aşağıdaki kodu ekledim.

```R
milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)
```

Bu kodu Azure Machine Learning Studio'da (klasik) çalıştırmak Şekil 24'te gösterilen çizimi üretir.

![Mevsimsel etkileri de dahil olmak üzere model ile Kaliforniya süt üretimi](./media/r-quickstart/unnamed-chunk-20.png)

*Şekil 24. Mevsimsel etkileri de dahil olmak üzere model ile Kaliforniya süt üretimi.*

Şekil 24'te gösterilen verilere uyum oldukça cesaret vericidir. Hem eğilim hem de mevsimsel etki (aylık değişim) makul görünüyor.

Modelimizi bir başka kontrol edince, artıklara bir göz atalım. Aşağıdaki kod, iki modelden öngörülen değerleri hesaplar, mevsimsel model için artıkları hesaplar ve sonra eğitim verileri için bu artıkları çizer.

```R
## Compute predictions from our models
predict1  <- predict(milk.lm, cadairydata)
predict2  <- predict(milk.lm2, cadairydata)

## Compute and plot the residuals
residuals <- cadairydata$Milk.Prod - predict2
plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")
```

Artık çizim Şekil 25'te gösterilmiştir.

![Eğitim verileri için mevsimsel modelin artıkları](./media/r-quickstart/unnamed-chunk-21.png)

*Şekil 25. Eğitim verileri için mevsimsel modelartıkları.*

Bu artıklar makul görünüyor. 2008-2009 durgunluğundan başka özel bir yapı yoktur, ki modelimiz bunu özellikle iyi hesaba katmaz.

Şekil 25'te gösterilen çizim, artıklardaki zamana bağlı desenleri algılamak için yararlıdır. Kullandığım artıkları hesaplamanın ve çizmenin açık yaklaşımı, artıkları arsanın üzerine zaman sırasına göre yerleştirir. Diğer taraftan, ben çizilmiş `milk.lm$residuals`olsaydı, arsa zaman sırasına göre olmazdı.

Ayrıca tanılama `plot.lm()` çizimleri bir dizi üretmek için kullanabilirsiniz.

```R
## Show the diagnostic plots for the model
plot(milk.lm2, ask = FALSE)
```

Bu kod Şekil 26'da gösterilen bir dizi tanılama çizimi üretir.

![Mevsimsel model için tanılama çizimlerinden ilki](./media/r-quickstart/unnamed-chunk-221.png)

![Mevsimsel model için tanılama çizimlerinin ikincisi](./media/r-quickstart/unnamed-chunk-222.png)

![Mevsimsel model için tanılama çizimlerinin üçte biri](./media/r-quickstart/unnamed-chunk-223.png)

![Mevsimsel model için tanılama çizimlerinin dördüncüsü](./media/r-quickstart/unnamed-chunk-224.png)

*Şekil 26. Mevsimsel model için tanılama çizimleri.*

Bu arsalar tespit birkaç son derece etkili noktaları vardır, ama hiçbir şey büyük endişe neden olur. Ayrıca, Normal Q-Q çiziminden artıkların normal olarak dağıtılana yakın olduğunu görebiliriz, bu da doğrusal modeller için önemli bir varsayımdır.

### <a name="forecasting-and-model-evaluation"></a>Tahmin ve model değerlendirmesi

Örneğimizi tamamlamak için yapılacak bir şey daha var. Tahminleri hesaplamalı ve hatayı gerçek verilere göre ölçmemiz gerekir. Tahminimiz 2013 yılının 12 ayı için olacak. Bu tahmin için, eğitim veri setimizin bir parçası olmayan gerçek verilere bir hata ölçümü hesaplayabiliriz. Ayrıca, 18 yıllık eğitim verilerindeki performansı 12 aylık test verileriyle karşılaştırabiliriz.  

Zaman serisi modellerinin performansını ölçmek için bir dizi ölçüm kullanılır. Bizim durumumuzda kök ortalama kare (RMS) hata sını kullanacağız. Aşağıdaki işlev iki seri arasındaki RMS hatasını hesaplar.  

```R
RMS.error <- function(series1, series2, is.log = TRUE, min.length = 2){
  ## Function to compute the RMS error or difference between two
  ## series or vectors

  messages <- c("ERROR: Input arguments to function RMS.error of wrong type encountered",
                "ERROR: Input vector to function RMS.error is too short",
                "ERROR: Input vectors to function RMS.error must be of same length",
                "WARNING: Funtion rms.error has received invald input time series.")

  ## Check the arguments
  if(!is.numeric(series1) | !is.numeric(series2) | !is.logical(is.log) | !is.numeric(min.length)) {
    warning(messages[1])
    return(NA)}

  if(length(series1) < min.length) {
    warning(messages[2])
    return(NA)}

  if((length(series1) != length(series2))) {
       warning(messages[3])
    return(NA)}

  ## If is.log is TRUE exponentiate the values, else just copy
  if(is.log) {
    tryCatch( {
      temp1 <- exp(series1)
      temp2 <- exp(series2) },
      error = function(e){warning(messages[4]); NA}
    )
  } else {
    temp1 <- series1
    temp2 <- series2
  }

 ## Compute predictions from our models
predict1  <- predict(milk.lm, cadairydata)
predict2  <- predict(milk.lm2, cadairydata)

## Compute the RMS error in a dataframe
  tryCatch( {
    sqrt(sum((temp1 - temp2)^2) / length(temp1))},
    error = function(e){warning(messages[4]); NA})
}
```

"Değer `log.transform()` dönüşümleri" bölümünde tartıştığımız işlevde olduğu gibi, bu işlevde de oldukça çok sayıda hata denetimi ve özel durum kurtarma kodu vardır. Kullanılan ilkeler aynıdır. İş iki yerde `tryCatch()`yapılır. İlk olarak, değerlerin günlükleri ile çalışıyoruz beri zaman serisi üstel vardır. İkinci olarak, gerçek RMS hatası hesaplanır.  

RMS hatasını ölçmek için bir işlevle donatılmış, RMS hatalarını içeren bir veri çerçevesi oluşturup çıkaralım. Biz sadece trend modeli ve mevsimsel faktörler ile tam model için terimler içerecektir. Aşağıdaki kod, oluşturduğumuz iki doğrusal modeli kullanarak işi yapar.

```R
## Compute the RMS error in a dataframe
## Include the row names in the first column so they will
## appear in the output of the Execute R Script
RMS.df  <-  data.frame(
rowNames = c("Trend Model", "Seasonal Model"),
  Traing = c(
  RMS.error(predict1[1:216], cadairydata$Milk.Prod[1:216]),
  RMS.error(predict2[1:216], cadairydata$Milk.Prod[1:216])),
  Forecast = c(
    RMS.error(predict1[217:228], cadairydata$Milk.Prod[217:228]),
    RMS.error(predict2[217:228], cadairydata$Milk.Prod[217:228]))
)
RMS.df

## The following line should be executed only when running in
## Azure Machine Learning Studio (classic)
maml.mapOutputPort('RMS.df')
```

Bu kodun çalıştırılmasını Sonuç Veri kümesi çıkış bağlantı noktasında Şekil 27'de gösterilen çıktı üretir.

![Modeller için RMS hatalarının karşılaştırılması](./media/r-quickstart/fig26.png)

*Şekil 27. Modeller için RMS hatalarının karşılaştırılması.*

Bu sonuçlardan, modele mevsimsel faktörlerin eklenmesinin RMS hatasını önemli ölçüde azalttığını görüyoruz. Çok şaşırtıcı değil, eğitim verileri için RMS hata tahmin için biraz daha azdır.

## <a name="guide-to-rstudio-documentation"></a><a id="appendixa"></a>RStudio dokümantasyon kılavuzu

RStudio oldukça iyi belgelenmiştir. Burada başlamak için RStudio belgelerin önemli bölümlerine bazı bağlantılar vardır.

* **Proje oluşturma** - RStudio'u kullanarak R kodunuzu organize edebilir ve proje olarak yönetebilirsiniz. Ayrıntılar için [Projeleri Kullanma'ya](https://support.rstudio.com/hc/articles/200526207-Using-Projects) bakın. Bu yönergeleri izlemenizi ve bu makaledeki R kodu örnekleri için bir proje oluşturmanızı öneririm.  
* **R kodunu düzenleme ve yürütme** - RStudio, R kodunu düzenlemek ve yürütmek için entegre bir ortam sağlar. Ayrıntılar için [Düzenleme ve Yürütme Kodu'na](https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code) bakın.
* **Hata ayıklama** - RStudio güçlü hata ayıklama yetenekleri içerir. Bu özellikler hakkında daha fazla bilgi için [RStudio ile Hata Ayıklama](https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio) bölümüne bakın. Kesme noktası sorun giderme özellikleri hakkında bilgi için [Kesme Noktası Sorun Giderme'ye](https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting)bakın.

## <a name="further-reading"></a><a id="appendixb"></a>Daha fazla bilgi

Bu R programlama öğreticisi Azure Machine Learning Studio (klasik) ile R dilini kullanmak için gerekenlerin temellerini kapsar. R'ye aşina değilseniz, CRAN'da iki tanıtım mevcuttur:

* Emmanuel Paradis tarafından [Yeni Başlayanlar için R](https://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf) başlamak için iyi bir yerdir.  
* W. N tarafından [R'ye Giriş.](https://cran.r-project.org/doc/manuals/R-intro.html) Venables et. al. biraz daha derine gider.

R'de başlamana yardımcı olabilecek birçok kitap vardır. Burada yararlı bulmak birkaçı şunlardır:

* R Programlama Sanatı: Norman Matloff tarafından **İstatistiksel Yazılım Tasarımı A Tour** R programlama için mükemmel bir giriştir.  
* Paul Teetor tarafından **R Cookbook** R kullanarak bir sorun ve çözüm yaklaşımı sağlar.  
* **R in Action** robert Kabacoff tarafından başka bir yararlı tanıtım kitabıdır. Arkadaşı [Hızlı R web sitesi](https://www.statmethods.net/) yararlı bir kaynaktır.
* Patrick Burns tarafından **R Inferno** r programlama karşılaşılan zor ve zor konular bir dizi ile ilgilenen şaşırtıcı derecede mizahi bir kitaptır. Kitap [R Inferno](https://www.burns-stat.com/documents/books/the-r-inferno/)ücretsiz olarak kullanılabilir.
* Eğer R gelişmiş konulara derin bir dalış istiyorsanız, Hadley Wickham tarafından kitap **Advanced R** bir göz atın. Bu kitabın online sürümü ücretsiz olarak [http://adv-r.had.co.nz/](http://adv-r.had.co.nz/)kullanılabilir.

R zaman serisi paketlerinin kataloğu [CRAN Görev Görünümü: Zaman Serisi Analizi'nde](https://cran.r-project.org/web/views/TimeSeries.html)bulunabilir. Belirli zaman serisi nesne paketleri hakkında bilgi için, bu paket için belgelere başvurmanız gerekir.

Paul Cowpertwait ve Andrew Metcalfe tarafından R ile kitap **Tanıtım Zaman Serisi** zaman serisi analizi için R kullanarak bir giriş sağlar. Daha birçok teorik metin R örnekleri sağlar.

İşte bazı büyük internet kaynakları şunlardır:

* DataCamp video dersleri ve kodlama egzersizleri ile tarayıcınızın konforunda R öğretir. En son R teknikleri ve paketleri interaktif öğreticiler vardır. Ücretsiz [interaktif R öğretici](https://www.datacamp.com/courses/introduction-to-r)alın.
* [Programiz'den R Programlama, Kesin Kılavuz öğrenin.](https://www.programiz.com/r-programming)
* Clarkson Üniversitesi Kelly Black tarafından hızlı bir [R Tutorial.](https://www.cyclismo.org/tutorial/R/)
* [Veri becerilerinizi geliştirmek için Top R dil kaynaklarında](https://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html)listelenen 60'ın üzerinde R kaynağı vardır.

<!-- Module References -->
[execute-r-script]: /azure/machine-learning/studio-module-reference/execute-r-script
