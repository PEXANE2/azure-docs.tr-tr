---
title: 'Öğretici 1: Kredi riskini tahmin edin'
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio'da (klasik) kredi riski değerlendirmesi için tahmine dayalı bir analiz çözümünü nasıl oluşturacağımı gösteren ayrıntılı bir öğretici. Bu öğretici üç bölümlük öğretici serisinin bir parçasıdır.  Çalışma alanı nın nasıl oluşturulup, veri yüklenir ken ve deneme nasıl oluşturulacaklarını gösterir.
keywords: kredi riski, tahmine dayalı analiz çözümü, risk değerlendirmesi
author: sdgilley
ms.author: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: tutorial
ms.date: 02/11/2019
ms.openlocfilehash: 6fd8573c78d80c950bdeb41ec01e2835def3979a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79204282"
---
# <a name="tutorial-1-predict-credit-risk---azure-machine-learning-studio-classic"></a>Öğretici 1: Kredi riskini tahmin edin - Azure Machine Learning Studio (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

[!INCLUDE [Designer notice](../../../includes/designer-notice.md)]

Bu eğitimde, tahmine dayalı bir analitik çözüm geliştirme sürecine uzun bir göz atabilirsiniz. Machine Learning Studio (klasik) basit bir model geliştirmek.  Ardından modeli Azure Machine Learning web hizmeti olarak dağıtırsınız.  Bu dağıtılan model, yeni verileri kullanarak öngörülerde bulunabilir. Bu öğretici **üç bölümlük öğretici serisinin bir parçasıdır.**

Bir kişinin kredi başvurusunda verdiği bilgilere dayanarak kredi riskini tahmin etmeniz gerektiğini varsayalım.  

Kredi risk değerlendirmesi karmaşık bir sorundur, ancak bu öğretici biraz basitleştirir. Microsoft Azure Machine Learning Studio 'yı (klasik) kullanarak nasıl tahmine dayalı bir analiz çözümü oluşturabileceğinize bir örnek olarak kullanırsınız. Bu çözüm için Azure Machine Learning Studio (klasik) ve Machine Learning web hizmetini kullanırsınız.  

Bu üç bölümlü eğitimde, herkese açık kredi riski verileriyle başlarsınız.  Daha sonra geliştirmek ve bir tahmin modeli eğitmek.  Son olarak modeli bir web hizmeti olarak dağıtın.

Öğretici bu bölümünde size: 
 
> [!div class="checklist"]
> * Machine Learning Studio (klasik) çalışma alanı oluşturma
> * Var olan verileri yükleme
> * Deneme oluşturma

Daha sonra [bu denemeyi, modelleri bölüm 2'de eğitmek](tutorial-part2-credit-risk-train.md) ve sonra bunları bölüm [3'te dağıtmak](tutorial-part3-credit-risk-deploy.md)için kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, Machine Learning Studio 'yı (klasik) en az bir kez daha önce kullandığınızı ve makine öğrenimi kavramlarını anladığınızı varsayar. Bununla birlikte, bir uzman olduğunuz da varsayılmaz.

Azure Machine Learning **Studio'yu (klasik)** daha önce hiç kullanmadıysanız, hızlı başlangıç la başlamak, [Azure Machine Learning Studio'da (klasik) ilk veri bilimi denemenizi oluşturun.](create-experiment.md) Hızlı başlangıç ilk kez Machine Learning Studio (klasik) ile götürür. Öğreticide modülleri sürükleyip denemenize bırakma, birbirine bağlama, denemeyi çalıştırma ve sonuçları görme konularında temel bilgiler verilir.


> [!TIP] 
> Bu öğreticide geliştirdiğiniz denemenin çalışma kopyasını Azure [AI Galerisi'nde](https://gallery.azure.ai)bulabilirsiniz. Tutorial **['a](https://gallery.azure.ai/Experiment/Walkthrough-Credit-risk-prediction-1)** gidin - Kredi riskini tahmin edin ve denemenin bir kopyasını Machine Learning Studio (klasik) çalışma alanına indirmek için **Studio'da Aç'ı** tıklatın.
> 


## <a name="create-a-machine-learning-studio-classic-workspace"></a>Machine Learning Studio (klasik) çalışma alanı oluşturma

Machine Learning Studio'yu (klasik) kullanmak için bir Microsoft Azure Machine Learning Studio (klasik) çalışma alanına sahip olmanız gerekir. Bu çalışma alanı, denemeleri oluşturmak, yönetmek ve yayımlamak için ihtiyacınız olan araçları içerir.  

Çalışma alanı oluşturmak için [bkz.](create-workspace.md)

Çalışma alanınız oluşturulduktan sonra Machine Learning Studio'yı (klasik) açın.[https://studio.azureml.net/Home](https://studio.azureml.net/Home) Birden fazla çalışma alanınız varsa, pencerenin sağ üst köşesindeki araç çubuğundaki çalışma alanını seçebilirsiniz.

![Studio'da çalışma alanını seçin (klasik)](./media/tutorial-part1-credit-risk/open-workspace.png)

> [!TIP]
> Çalışma alanının sahibiyseniz, başkalarını çalışma alanına davet ederek üzerinde çalıştığınız denemeleri paylaşabilirsiniz. Bunu **AYARLAR** sayfasındamachine Learning Studio 'da (klasik) yapabilirsiniz. Her kullanıcı için Microsoft hesabına veya kuruluş hesabına ihtiyacınız olabilir.
> 
> **AYARLAR** **sayfasında, KULLANICILARI**tıklatın, ardından pencerenin altındaki **DAHA FAZLA KULLANICI DAVET** ET'i tıklatın.
> 

## <a name="upload-existing-data"></a><a name="upload"></a>Var olan verileri yükleme

Kredi riski için tahmine dayalı bir model geliştirmek için, modeli eğitmek ve sonra test etmek için kullanabileceğiniz verilere ihtiyacınız vardır. Bu eğitim için UC Irvine Machine Learning deposundan "UCI Statlog (Alman Kredi Verileri) Veri Seti"ni kullanacaksınız. Burada bulabilirsiniz:  
<a href="https://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">https://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a>

**german.data**adlı dosyayı kullanacaksınız. Bu dosyayı yerel sabit diskinize indirin.  

**german.data** kümesi, kredi için 1000 geçmiş başvuru sahibi için 20 değişkenlik satırlar içerir. Bu 20 değişken, her kredi başvuru sahibi için tanımlayıcı özellikler sağlayan veri kümesinin *(özellik vektörü)* kümesini temsil eder. Her satırdaki ek bir sütun, 700 başvuru sahibinin düşük kredi riski ve 300'ün yüksek risk olarak tanımlanmasıyla, başvuranın hesaplanan kredi riskini temsil eder.

UCI web sitesi, bu veriler için özellik vektörünün özniteliklerinin bir açıklamasını sağlar. Bu veriler finansal bilgileri, kredi geçmişini, istihdam durumunu ve kişisel bilgileri içerir. Her başvuru sahibi için, düşük veya yüksek kredi riski olup olmadığını gösteren bir ikili derecelendirme verilmiştir. 

Bu verileri, tahmine dayalı bir analitik model eğitmek için kullanırsınız. İşinizi bitirdiğinizde, modeliniz yeni bir birey için bir özellik vektörünü kabul edebilmeli ve bunların düşük veya yüksek kredi riski olup olmadığını tahmin edebilmelidir.  

İşte ilginç bir bükülme.

UCI web sitesindeki veri kümesinin açıklaması, bir kişinin kredi riskini yanlış sınıfa alırsan maliyetinin ne olduğundan bahseder.
Eğer model aslında düşük kredi riski olan biri için yüksek bir kredi riski öngörüyorsa, model yanlış bir sınıflandırma yapmıştır.

Ama ters yanlış sınıflandırma beş kat daha fazla finans kurumu için pahalıdır: model aslında yüksek kredi riski olan biri için düşük bir kredi riski öngörüyorsa.

Yani, bu ikinci tür yanlış sınıflandırmanın maliyetinin diğer yolu yanlış sınıflandırmadan beş kat daha yüksek olması için modelinizi eğitmek istiyorsunuz.

Denemenizdeki modeli eğitirken bunu yapmanın basit bir yolu, yüksek kredi riski olan birini temsil eden girişleri (beş kez) çoğaltmaktır. 

Daha sonra, eğer model bir şeyi aslında yüksek risk olduğunda düşük kredi riski olarak yanlış sınıflatırsa, model aynı yanlış sınıflandırmayı her yineleme için beş kez yapar. Bu, eğitim sonuçlarında bu hatanın maliyetini artıracaktır.


### <a name="convert-the-dataset-format"></a>Veri kümesi biçimini dönüştürme

Özgün veri kümesi boş ayrılmış bir biçim kullanır. Machine Learning Studio (klasik) virgülden ayrılmış bir değer (CSV) dosyasıyla daha iyi çalışır, böylece boşlukları virgülle değiştirerek veri kümesini dönüştürürsünüz.  

Bu verileri dönüştürmenin birçok yolu vardır. Bir yolu aşağıdaki Windows PowerShell komutunu kullanarak:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Başka bir yolu Unix sed komutu kullanarak:  

    sed 's/ /,/g' german.data > german.csv  

Her iki durumda da, denemenizde kullanabileceğiniz **german.csv** adlı bir dosyada verilerin virgülle ayrılmış bir sürümünü oluşturdunuz.

### <a name="upload-the-dataset-to-machine-learning-studio-classic"></a>Veri kümesini Machine Learning Studio'ya yükleyin (klasik)

Veriler CSV formatına dönüştürüldükten sonra, verileri Machine Learning Studio'ya (klasik) yüklemeniz gerekir. 

1. Machine Learning Studio (klasik) giriş[https://studio.azureml.net](https://studio.azureml.net)sayfasını açın ( ). 

2. Pencerenin ![sol](./media/tutorial-part1-credit-risk/menu.png) üst köşesindeki menü Menüsü'nü tıklatın, **Azure Machine Learning'i**tıklatın, **Studio'yu**seçin ve oturum açın.

3. Pencerenin altındaki **+Yenİ'yi** tıklatın.

4. **DATASET'i**seçin.

5. **YEREL DOSYADAN**seçin.

    ![Yerel bir dosyadan veri kümesi ekleme](./media/tutorial-part1-credit-risk/add-dataset.png)

6. Yeni **bir veri seti yükle** iletişim kutusunda Gözat'ı tıklatın ve oluşturduğunuz **german.csv** dosyasını bulun.

7. Veri kümesi için bir ad girin. Bu öğretici için, "UCI Alman Kredi Kartı Verileri" diyoruz.

8. Veri türü için, **üstbilgisiz Genel CSV Dosyası 'nı (.nh.csv)** seçin.

9. İstersenize açıklama ekleyin.

10. **Tamam** onay işaretini tıklatın.  

    ![Veri kümesini yükleme](./media/tutorial-part1-credit-risk/upload-dataset.png)

Bu, verileri bir denemede kullanabileceğiniz bir veri kümesi modülüne yükler.

Studio (klasik) penceresinin solundaki DATASETS sekmesini tıklatarak Studio'ya (klasik) yüklediğiniz veri **kümelerini** yönetebilirsiniz.

![Veri kümelerini yönet](./media/tutorial-part1-credit-risk/dataset-list.png)

Bir denemeye diğer veri türlerini alma hakkında daha fazla bilgi için eğitim [verilerinizi Azure Machine Learning Studio'ya (klasik) aktarın.](import-data.md)

## <a name="create-an-experiment"></a>Deneme oluşturma

Bu öğreticinin bir sonraki adımı, yüklediğiniz veri kümesini kullanan Machine Learning Studio'da (klasik) bir deneme oluşturmaktır.  

1. Studio'da (klasik), pencerenin altındaki **+Yenİ'yi** tıklatın.
1. **DENEY'i**seçin ve ardından "Boş Deneme"yi seçin. 

    ![Yeni bir deneme oluşturma](./media/tutorial-part1-credit-risk/create-new-experiment.png)


1. Tuvalin üst kısmındavarsayılan deneme adını seçin ve anlamlı bir şeyle yeniden adlandırın.

    ![Denemeyi yeniden adlandır](./media/tutorial-part1-credit-risk/rename-experiment.png)

   > [!TIP]
   > **Özellikler** bölmesinde denemenin **Özeti** ve **Açıklaması'nı** doldurmak iyi bir uygulamadır. Bu özellikler, denemeyi daha sonra bakan herkesin hedeflerinizi ve metodolojinizi anlamasını sağlamak için denemeyi belgeleme şansı verir.
   > 
   > ![Deney özellikleri](./media/tutorial-part1-credit-risk/experiment-properties.png)
   > 

1. Deneme tuvalinin solundaki modül paletinde **Kaydedilmiş Veri Kümelerini**genişletin.
1. **Verilerimin** altında oluşturduğunuz veri kümesini bulun ve tuvale sürükleyin. Paletin üzerindeki **Arama** kutusuna adı girerek de veri kümesini bulabilirsiniz.  

    ![Denemeye veri kümesi ekleme](./media/tutorial-part1-credit-risk/add-dataset-to-experiment.png)


### <a name="prepare-the-data"></a>Verileri hazırlama

Verilerin ilk 100 satırını ve tüm veri kümesi için bazı istatistiksel bilgileri görüntüleyebilirsiniz: Veri kümesinin çıkış bağlantı noktasını (alttaki küçük daire) tıklatın ve **Visualize'ı**seçin.  

Veri dosyası sütun başlıkları ile gelmediği için Studio (klasik) genel başlıklar (Col1, Col2, *vb) sağlamıştır.* İyi başlıklar bir model oluşturmak için gerekli değildir, ancak denemedeki verilerle çalışmayı kolaylaştırır. Ayrıca, bu modeli bir web hizmetinde yayımladığınızda, başlıklar hizmetin kullanıcısına sütunları tanımlamaya yardımcı olur.  

[Meta verileri edit][edit-metadata] modüllerini kullanarak sütun başlıkları ekleyebilirsiniz.

Bir veri kümesiyle ilişkili meta verileri değiştirmek için [Meta Verileri Edit][edit-metadata] modülünü kullanırsınız. Bu durumda, sütun başlıkları için daha kolay adlar sağlamak için kullanabilirsiniz. 

Meta [Verileri Edit'i][edit-metadata]kullanmak için, önce hangi sütunları değiştirin (bu durumda, hepsi) belirtirsiniz. Ardından, bu sütunlarda gerçekleştirilecek eylemi belirtirsiniz (bu durumda sütun başlıklarını değiştirirsiniz.)

1. Modül paletinde **Arama** kutusuna "meta veri" yazın. [Meta Verilerini Edit][edit-metadata] modül listesinde görünür.

1. Meta verileri [edit][edit-metadata] modülünü tuvale tıklayıp sürükleyin ve daha önce eklediğiniz veri kümesinin altına bırakın.

1. Veri kümesini [Meta Verileri Edit'e][edit-metadata]bağlayın: veri kümesinin çıkış bağlantı noktasını (veri kümesinin altındaki küçük daire) tıklatın, [Meta verilerini (modülün][edit-metadata] üst kısmındaki küçük daire) giriş noktasına sürükleyin ve ardından fare düğmesini bırakın. Kanvas üzerinde hareket etseniz bile veri kümesi ve modül bağlı kalır.
 
    Deney şimdi şuna benzer:  

    ![Meta Verileri Edit Ekleme](./media/tutorial-part1-credit-risk/experiment-with-edit-metadata-module.png)

    Kırmızı ünlem işareti, bu modülün özelliklerini henüz ayarlamadığınızı gösterir. Bundan sonra o işlemi yapacaksınız.

    > [!TIP]
    > Modüle çift tıklayıp metin girerek bir modüle yorum ekleyebilirsiniz. Bu, modülün denemenizde ne işe yaradığını bir bakışta görmenize yardımcı olabilir. Bu durumda, [Meta veri][edit-metadata] modüllerini çift tıklatın ve yorum "Sütun başlıkları ekle" yazın. Metin kutusunu kapatmak için tuvalin başka bir yerine tıklayın. Yorumu görüntülemek için modüldeki aşağı ok'u tıklatın.
    > 
    > ![Eklenen yorumla Meta veri modüllerini edit](./media/tutorial-part1-credit-risk/edit-metadata-with-comment.png)
    > 

1. [Meta Verileri Düzenley'i][edit-metadata]seçin ve tuvalin sağındaki **Özellikler** bölmesinde **Sütun Seçicisi Başlat'ı**tıklatın.

1. **Sütunları Seç** iletişim kutusunda, **Kullanılabilir Sütunlar'daki** tüm satırları seçin ve **seçili sütunlara**taşımak için >'ı tıklatın.
   İletişim şuna bakmalıdır:

   ![Tüm sütunların seçildiği Sütun Seçici](./media/tutorial-part1-credit-risk/select-columns.png)


1. **Tamam** onay işaretini tıklatın.

1. **Özellikler** bölmesinde, Yeni sütun **adları** parametresini arayın. Bu alanda, virgülle ve sütun sırasına göre ayrılmış veri kümesindeki 21 sütunun ad listesini girin. UCI web sitesindeki veri kümesi belgelerinden sütun adlarını edinebilir veya kolaylık sağlamak için aşağıdaki listeyi kopyalayıp yapıştırabilirsiniz:  

   ```   
   Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   ```

   Özellikler bölmesi şuna benzer:

   ![Meta verileri edit özellikleri](./media/tutorial-part1-credit-risk/edit-metadata-properties.png)

   > [!TIP]
   > Sütun başlıklarını doğrulamak istiyorsanız, denemeyi çalıştırın (deneme tuvalinin altında **ÇALıŞTıR'ı** tıklatın). Çalışma bittiğinde [(Meta Verilerini Edit'te][edit-metadata]yeşil bir onay işareti görünür), [Meta Verileri Edit][edit-metadata] modülünün çıkış bağlantı noktasını tıklatın ve **Visualize'ı**seçin. Herhangi bir modülün çıktısını, deneme yoluyla verilerin ilerlemesini görüntülemek için aynı şekilde görüntüleyebilirsiniz.
   > 
   > 

### <a name="create-training-and-test-datasets"></a>Eğitim oluşturma ve veri kümelerini test edin

Modeli eğitmek için bazı verilere ve test etmek için de bazı verilere ihtiyacınız var.
Yani deneyin bir sonraki adımında, veri kümesini iki ayrı veri kümesine bölersiniz: biri modelimizi eğitmek, diğeri de test etmek için.

Bunu yapmak için [Veri Bölme][split] modüllerini kullanırsınız.  

1. Verileri [Böl][split] modülünü bulun, tuvale sürükleyin ve Meta veri modülünü [edit][edit-metadata] modülüne bağlayın.

1. Varsayılan olarak, bölme oranı 0,5'tir ve **Randomize bölme** parametresi ayarlanır. Bu, verilerin rasgele yarısının [Bölünmüş Veri][split] modülünün bir bağlantı noktasından, yarısının ise diğer bağlantı noktasından çıktı olduğu anlamına gelir. Bu parametreleri ve **Rasgele tohum** parametresini, eğitim ve test verileri arasındaki bölmeyi değiştirmek için ayarlayabilirsiniz. Bu örnekte, onları olduğu gibi bırakırsınız.
   
   > [!TIP]
   > İlk **çıktı veri kümesindeki satırların özellik Fraksiyonu,** verilerin ne kadarının *sol* çıktı bağlantı noktasından çıktı olduğunu belirler. Örneğin, oranı 0,7 olarak ayarlarsanız, verilerin %70'i sol bağlantı noktasından, %30'u da sağ bağlantı noktasından çıkar.  
   > 
   > 

1. [Verileri Böl][split] modülünü çift tıklatın ve "Eğitim/test verileri %50 bölündü" yorumunu girin. 

[Split Data][split] modülünün çıktılarını istediğiniz gibi kullanabilirsiniz, ancak sol çıktıyı eğitim verileri olarak, doğru çıktıyı da test verisi olarak kullanmayı seçelim.  

[Önceki adımda](tutorial-part1-credit-risk.md#upload)belirtildiği gibi, düşük olarak yüksek bir kredi riski yanlış sınıflandırma maliyeti yüksek olarak düşük kredi riski yanlış sınıflandırma maliyeti beş kat daha yüksektir. Bunu hesaba katmak için, bu maliyet işlevini yansıtan yeni bir veri kümesi oluşturursunuz. Yeni veri kümesinde, her yüksek riskli örnek beş kez çoğaltılırken, her düşük riskli örnek çoğaltılamaz.   

Bu çoğaltmayı R kodunu kullanarak yapabilirsiniz:  

1. Execute R [Script][execute-r-script] modüllerini bulun ve deneme tuvaline sürükleyin. 

1. [Split Data][split] modülünün sol çıkış bağlantı noktasını Execute [R Script][execute-r-script] modülünün ilk giriş bağlantı noktasına ("Dataset1") bağlayın.

1. Yürüt R [Script][execute-r-script] modüle çift tıklayın ve yorum girin, "Maliyet ayarlaması ayarlayın".

1. **Özellikler** bölmesinde, **R Script** parametresindeki varsayılan metni silin ve şu komut dosyasını girin:
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![Execute R Script modülünde R komut dosyası](./media/tutorial-part1-credit-risk/execute-r-script.png)

Eğitim ve test verilerinin aynı maliyet ayarlamasına sahip olması için [Bölünmüş Veri][split] modülünün her çıktısı için aynı çoğaltma işlemini yapmanız gerekir. Bunu yapmanın en kolay yolu, az önce yaptığınız [Execute R Script][execute-r-script] modülünü çoğaltmak ve Split [Data][split] modülünün diğer çıkış noktasına bağlamaktır.

1. Yürüt R [Script][execute-r-script] modülünü sağ tıklatın ve **Kopyala'yı**seçin.

1. Deneme tuvaline sağ tıklayın ve **Yapıştır'ı**seçin.

1. Yeni modülü konuma sürükleyin ve split [veri][split] modülünün doğru çıkış bağlantı noktasını bu yeni [Execute R Script][execute-r-script] modülünün ilk giriş noktasına bağlayın. 

1. Tuvalin altında **Çalıştır'ı**tıklatın. 

> [!TIP]
> Execute R Script modülünün kopyası orijinal modülle aynı komut dosyasını içerir. Bir modülü tuvale kopyalayıp yapıştırdığınızda, kopya orijinalin tüm özelliklerini korur.  
> 
>

Deneyimiz şimdi şuna benziyor:

![Bölme modülü ve R komut dosyaları ekleme](./media/tutorial-part1-credit-risk/experiment.png)

Denemelerinizde R komut dosyalarını kullanma hakkında daha fazla bilgi [için](extend-your-experiment-with-r.md)bkz.


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [machine-learning-studio-clean-up](../../../includes/machine-learning-studio-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şu adımları tamamladınız: 
 
> [!div class="checklist"]
> * Machine Learning Studio (klasik) çalışma alanı oluşturma
> * Varolan verileri çalışma alanına yükleme
> * Deneme oluşturma

Artık bu veriler için modelleri eğitmeye ve değerlendirmeye hazırsınız.

> [!div class="nextstepaction"]
> [Öğretici 2 - Modelleri eğitin ve değerlendirin](tutorial-part2-credit-risk-train.md)

<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
