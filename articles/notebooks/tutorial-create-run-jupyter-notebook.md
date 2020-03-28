---
title: Öğretici - Jupyter dizüstü bilgisayar oluşturma ve çalıştırma - Azure Dizüstü Bilgisayarlar Önizleme
description: Azure Notebook Önizleme'de veri biliminde doğrusal gerileme sürecini gösteren bir Jupyter dizüstü bilgisayar oluşturma ve çalıştırma yı öğrenin.
ms.topic: tutorial
ms.date: 01/11/2019
ms.openlocfilehash: 8a1c13f41ef1588b040b3540b852d83764c6ce79
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75660826"
---
# <a name="tutorial-create-and-run-a-jupyter-notebook-with-python"></a>Öğretici: Python ile bir Jupyter dizüstü bilgisayar oluşturun ve çalıştırın

Bu öğretici, basit doğrusal gerilemeyi gösteren eksiksiz bir Jupyter dizüstü bilgisayar oluşturmak için Azure Not Defterlerini kullanma sürecinde size yol gösterir. Bu öğretici süresince, farklı hücreler oluşturmayı, hücreleri çalıştırmayı ve not defterini slayt gösterisi olarak sunmayı içeren Jupyter dizüstü bilgisayar ui'sini tanıtın.

Tamamlanan not defteri [GitHub - Azure Not Defteri Örnekleri'nde](https://github.com/Microsoft/AzureNotebooks/tree/master/Samples/Linear%20Regression%20-%20Cricket%20Chirps)bulunabilir. Ancak bu öğretici, yeni bir proje ve boş bir not defteriyle başlar, böylece adım adım oluşturmayı deneyimleyebilirsiniz.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bazı örnek verileri içeren bir proje not defteri oluşturma
> * Çeşitli hücre türleri oluşturmak için not defteri arabirimini kullanın
> * Not defterini çalıştırma
> * Not defterini kaydetme
> * Visual Studio Code'da not defterini hata ayıklama

## <a name="create-the-project"></a>Proje oluşturma

1. Azure [Not Defterleri'ne](https://notebooks.azure.com) gidin ve oturum açın. (Ayrıntılar için [Bkz. Hızlı Başlangıç - Azure Not Defterlerinde Oturum Aç).](quickstart-sign-in-azure-notebooks.md)

1. Herkese açık profil sayfanızdan, sayfanın üst kısmında **Projelerim'i** seçin:

    ![Tarayıcı penceresinin üst kısmındaki Projelerim bağlantısı](media/quickstarts/my-projects-link.png)

1. **Projelerim** sayfasında + **Yeni Proje 'yi** (klavye kısayolu: n); düğmesi yalnızca tarayıcı **+** penceresi darsa gibi görünebilir:

    ![Projelerim sayfasında yeni Proje komutu](media/quickstarts/new-project-command.png)

1. Aşağıdaki ayrıntıları görünen, giren veya ayarlayan **Yeni Proje Oluştur** açılır penceresinde Oluştur seçeneğini **belirleyin:**

   - **Proje adı**: Doğrusal Regresyon Örneği - Kriket Chirps
   - **Proje KIMLIĞI**: doğrusal-regresyon-örnek
   - **Kamu projesi**: (temizlendi)
   - **bir README.md oluşturun**: (temizlendi)

1. Birkaç dakika sonra Azure Dizüstü Bilgisayarlar sizi yeni projeye yönlendirer.

## <a name="create-the-data-file"></a>Veri dosyasını oluşturma

Not defterinde oluşturduğunuz doğrusal regresyon *modeli, projenizdeki cricket_chirps.csv*adlı bir dosyadan veri çeker. Bu dosyayı [GitHub - Azure Not Örnekleri'nden](https://github.com/Microsoft/AzureNotebooks/tree/master/Samples/Linear%20Regression%20-%20Cricket%20Chirps)kopyalayarak veya verileri doğrudan girerek oluşturabilirsiniz. Aşağıdaki bölümlerde her iki yaklaşım da açıklayınız.

### <a name="upload-the-data-file"></a>Veri dosyasını yükleme

1. Azure Not Defterleri'ndeki proje panonuzda**URL'den** **Yükle'yi** > seçin
1. Açılan pencerede, Dosya URL'sindeki aşağıdaki URL'yi ve **Dosya Adı'nda** *cricket_chirps.csv'yi* girin, ardından **Bitti'yi**seçin. **File URL**

    ```url
    https://raw.githubusercontent.com/Microsoft/AzureNotebooks/master/Samples/Linear%20Regression%20-%20Cricket%20Chirps/cricket_chirps.csv
    ```

1. *cricket_chirps.csv* dosyası artık projenizin dosya listesinde görünmelidir:

    ![Proje dosya listesinde gösterilen yeni oluşturulan CSV dosyası](media/tutorial/csv-file-in-project.png)

### <a name="create-a-file-from-scratch"></a>Sıfırdan dosya oluşturma

1. Azure Not Defterleri'ndeki proje panonuzda + Yeni > Boş**Dosya'yı** seçin **+ New**
1. Projenin dosya listesinde bir alan görüntülenir. *cricket_chirps.csv* girin ve Enter tuşuna basın.
1. *Cricket_chirps.csv'ye* sağ tıklayın ve **Dosyayı Edit'i**seçin.
1. Görünen düzenleyiciye aşağıdaki verileri girin:

    ```csv
    Chirps/Minute,Temperature
    20,88.6
    16,71.6
    19.8,93.3
    18.4,84.3
    17.1,80.6
    15.5,75.2
    14.7,69.7
    17.1,82
    15.4,69.4
    16.2,83.3
    15,79.6
    17.2,82.6
    16,80.6
    17,83.5
    14.4,76.3
    ```

1. Dosyayı kaydetmek ve proje panosuna dönmek için **Dosyayı Kaydet'i** seçin.

## <a name="install-project-level-packages"></a>Proje düzeyi paketlerini yükleme

Not defterinde, gerekli paketleri yüklemek `!pip install` için her zaman kod hücresindeki gibi komutları kullanabilirsiniz. Ancak, bu tür komutlar not defterinin kod hücrelerini her çalıştırdığınızda çalıştırılır ve önemli ölçüde zaman alabilir. Bu nedenle, bunun yerine bir `requirements.txt` dosya kullanarak proje düzeyinde paketleri yükleyebilirsiniz.

1. Aşağıdaki içeriklerle birlikte bir `requirements.txt` dosya oluşturmak için [sıfırdan dosya oluştur'da](#create-a-file-from-scratch) açıklanan işlemi kullanın:

    ```text
    matplotlib==3.0.0
    numpy==1.15.0
    pandas==0.23.4
    scikit-learn==0.20.0
    ```

    İsterseniz yerel `requirements.txt` bilgisayarınızdan da bir dosya yükleyebilirsiniz, [veri dosyasını yükleyin'de](#upload-the-data-file)açıklandığı gibi.

1. Proje panosunda **Proje Ayarları'nı**seçin.
1. Görünen açılır pencerede **Çevre** sekmesini seçin ve **ardından +Ekle'yi**seçin.
1. **Çevre Kurulum Adımları**altındaki ilk açılır denetimde (işlem) **Requirements.txt'yi**seçin.
1. İkinci açılır açma denetiminde (dosya adı), *requirements.txt'yi* (oluşturduğunuz dosya) seçin.
1. Üçüncü açılır-aşağı denetiminde (Python sürümü), **Python Sürüm 3.6'yı**seçin.
1. **Kaydet'i**seçin.

![Gereksinimleri belirten Proje Ayarları Ortamı sekmesi.txt dosyası](media/tutorial/tutorial-requirements-txt.png)

Bu kurulum adımı nın devreye sokulduğunda, projede çalıştırdığınız tüm not defteri bu paketlerin yüklü olduğu bir ortamda çalışır.

## <a name="create-and-run-a-notebook"></a>Not defteri oluşturma ve çalıştırma

Veri dosyası hazır ve proje ortamı ayarlı, artık not defteri oluşturabilir ve açabilirsiniz.

1. Proje panosunda **+ Yeni** > **Not Defteri'ni**seçin.
1. Açılır pencerede, *Doğrusal Regresyon Örneği girin - Madde Adı için Kriket Chirps.ipynb* , dil için **Python 3.6** seçin, sonra **Yeni**seçin . **Item Name**
1. Yeni not defteri dosya listesinde göründükten sonra, not defterini başlatmak için defteri seçin. Yeni bir tarayıcı sekmesi otomatik olarak açılır.
1. Ortam ayarlarında *gereksinimler.txt* dosyanız olduğundan, "Kabınızın hazırlanmasını bekliyoruz" iletisini görürsünüz. İletiyi kapatmak ve not defterinde çalışmaya devam etmek için **Tamam'ı** seçebilirsiniz; ancak, ortam tam olarak ayarlanana kadar kod hücrelerini çalıştıramazsınız.
1. Not defteri, Jupyter arabiriminde varsayılan olarak tek bir boş kod hücresiyle açılır.

    [![Azure Not Defterleri'nde yeni bir dizüstü bilgisayarın ilk görünümü](media/tutorial/tutorial-new-notebook.png)](media/tutorial/tutorial-new-notebook.png#lightbox)

## <a name="tour-the-notebook-interface"></a>Dizüstü bilgisayar arabirimini gezin

Not defteri çalışırken, kod ve Markdown hücreleri ekleyebilir, bu hücreleri çalıştırabilir ve not defterinin çalışmasını yönetebilirsiniz. İlk olarak, ancak, arayüzü kendinizi tanımak için birkaç dakika nızı ayırdığınız değer. Tam dokümantasyon için **Yardım** > **Not Defteri Yardım** menüsü komutunu seçin.

Pencerenin üst kısmında aşağıdaki öğeleri görürsünüz:

(A) Tıklatarak düzenleme yapabileceğiniz not defterinizin adı.
(B) Tarayıcınızda yeni sekmeler açan ilgili projeye ve proje panonuza gitmek için düğmeler.
(C) Not defteriyle çalışmak için komutları içeren bir menü.
(D) ortak işlemler için kısayolları olan bir araç çubuğu.
(E) hücreleri içeren düzenleme tuval.
(F) not defterine güvenilip güvenilmediğinin göstergesi (varsayılan **güvenilmez).**
(G) bir etkinlik göstergesi ile birlikte dizüstü çalıştırmak için kullanılan çekirdek.

[![Jupyter arabiriminin birincil arabirimi alanları](media/tutorial/tutorial-notebook-ui.png)](media/tutorial/tutorial-notebook-ui.png#lightbox)

Jupyter birincil Kullanıcı Arası Bira Birimi elemanlarının yerleşik bir tur sağlar. **Yardım** > **Kullanıcı Arabirimi Turu** komutunu seçerek ve açılır pencereleri tıklatarak turu başlatın.

Menü komutları grupları aşağıdaki gibidir:

| Menü | Açıklama |
| --- | --- |
| Dosya | Not defteri oluşturma ve kopyalama, yazdırma önizlemesi gösterme ve not defterini çeşitli biçimlerde indirme komutları da dahil olmak üzere not defteri dosyasını yönetme komutları. |
| Düzenle | Hücreleri kesmek, kopyalamak ve yapıştırmak, değerleri bulmak ve değiştirmek, hücre eklerini yönetmek ve görüntüleri eklemek için tipik komutlar.  |
| Görünüm | Jupyter UI'nin farklı bölümlerinin görünürlüğünü kontrol etme komutları. |
| Ekle | Geçerli hücrenin üstüne veya altına yeni bir hücre eklemek için komutlar. Not defteri oluştururken bu komutları sık sık kullanırsınız. |
| Cep | Çeşitli **Çalıştır** komutları farklı kombinasyonlarda bir veya daha fazla hücre çalıştırır. **Hücre Türü** komutları **Kod**, **Markdown**ve Raw **NBConvert** (düz metin) arasındaki hücre türünü değiştirir. **Geçerli Çıktılar** ve **Tüm Çıktılar** komutları, run kodundan çıktının nasıl gösterildiğini denetler ve tüm çıktıları temizlemek için bir komut içerir. |
| Çekirdek | Not defterini çalıştırmak için kullanılan dili veya Python sürümünü değiştirmek için **Kodun** çekirdekte nasıl çalıştırıldığını yönetme komutları. |
| Veri | Proje den veya oturumdan dosya yükleme ve indirme komutları. Bkz. [Proje veri dosyalarıyla Çalışma](work-with-project-data-files.md) |
| Pencere öğeleri | Görselleştirme, haritalama ve çizim için ek özellikler sağlayan [Jupyter Widget'ları](https://ipywidgets.readthedocs.io/en/stable/examples/Widget%20Basics.html)yönetme komutları.|
| Yardım | Jupyter arabirimi için yardım ve belge sağlayan komutlar. |

Araç çubuğundaki komutların çoğunda eşdeğer menü komutları vardır. Bir istisna, [Share ve present dizüstü bilgisayarlarda](present-jupyter-notebooks-slideshow.md)tartışılan RISE Slayt **Gösterisini Girin/Edin.**

Not defterini izleyen bölümlerde doldururken bu komutlardan birkaçını kullanırsınız.

## <a name="create-a-markdown-cell"></a>Markdown hücresi oluşturma

1. Not defteri tuvalinde gösterilen ilk boş hücreye tıklayın. Varsayılan olarak, bir hücre **kod** türüdür, bu da seçili çekirdek (Python, R veya F#) için çalıştırılabilir kodu içerecek şekilde tasarlandığı anlamına gelir. Geçerli tür, araç çubuğundaki tür açılır durumda gösterilir:

    ![Hücre tipi araç çubuğu açılır](media/tutorial/tutorial-cell-type-drop-down.png)

1. Araç çubuğu açılır bırakmasını kullanarak hücre türünü **Markdown** olarak değiştirin; alternatif olarak, **Hücre** > **Hücre Türü** > **İşaretaşağı** menü komutunu kullanın:

    ![Hücre türü menü komutu](media/tutorial/tutorial-cell-type-menu.png)

1. Düzenlemeye başlamak için hücreye tıklayın ve ardından aşağıdaki İşaretle'yi girin:

    ```markdown
    # Example Linear Regression

    This notebook contains a walkthrough of a simple linear regression. The data, obtained from [college.cengage.com](https://college.cengage.com/mathematics/brase/understandable_statistics/7e/students/datasets/slr/frames/frame.html), relates the rate of cricket chirps to temperature from *The Song of Insects*, by Dr. G. W. Pierce, Harvard College Press.

    In this example we're using the count of chirps per minute as the independent varible to then predict the dependent variable, temperature. In short, we're using a little data science to make ourselves a cricket thermometer. (You could also reverse the data and use temperature to predict the number of chirps, but it's more fun to use crickets as the thermometer itself!)

    The methods shown in this example follow what's presented in the Udemy course, [Machine Learning A to Z](https://www.udemy.com/machinelearning/learn/v4/).

    A lovely aspect of Notebooks is that you can use Markdown cells to explain what the code is doing rather than code comments. There are several benefits to doing so:

    - Markdown allows for richer text formatting, like *italics*, **bold**, `inline code`, hyperlinks, and headers.
    - Markdown cells automatically word wrap whereas code cells do not. Code comments typically use explicit line breaks for formatting, but that's not necessary in Markdown.
    - Using Markdown cells makes it easier to run the Notebook as a slide show.
    - Markdown cells help you remove lengthy comments from the code, making the code easier to scan.

    When you run a code cell, Jupyter executes the code; when you run a Markdown cell, Jupyter renders all the formatting into text that's suitable for presentation.
    ```

1. Tarayıcı için Markdown'u HTML'ye dönüştürmek için araç çubuğundaki **Çalıştır** komutunu seçin veya **Hücre** > **Hücreleri Komutunu** kullanın. Biçimlendirme için Markdown kodu ve bağlantılar artık bir tarayıcıda beklediğiniz gibi görünür.

1. Not defterindeki son hücreyi çalıştırdığınızda, Jupyter çalıştırdığınız hücrenin altında otomatik olarak yeni bir hücre oluşturur. Bu bölümdeki adımları aşağıdaki Markdown ile yineleyerek bu hücreye daha fazla Markdown koyun:

    ```markdown
    ## Install packages using pip or conda

    Because the code in your notebook likely uses some Python packages, you need to make sure the Notebook environment contains those packages. You can do this directly within the notebook in a code block that contains the appropriate pip or conda commands prefixed by `!`:

    \```
    !pip install <pkg name>

    !conda install <pkg name> -y
    \```
    ```

1. İşaretle'yi yeniden yeniden yapmak için, işlenen hücrede çift tıklatın. Değişiklik yaptıktan sonra HTML'yi yeniden işlemek için hücreyi çalıştırın.

## <a name="create-a-code-cell-with-commands"></a>Komutları içeren bir kod hücresi oluşturma

Önceki Markdown hücresinin açıkladığı gibi, komutları doğrudan not defterine ekleyebilirsiniz. Paketleri yüklemek, kıvırma veya wget'ı çalıştırmak için komutları kullanabilirsiniz, verileri veya başka bir şeyi alabilirsiniz. Jupyter dizüstü bilgisayarlar etkili bir Linux sanal makine içinde çalıştırmak, böylece tam Linux komutu ile çalışmak için ayarlanmış olması.

1. Önceki Markdown hücresinde **Çalıştır'ı** kullandıktan sonra ortaya çıkan kod hücresine aşağıdaki komutları girin. Yeni bir hücre görmüyorsanız,**Aşağıdaki Ekle Hücresini** **Ekle** > ile bir **+** hücre oluşturun veya araç çubuğundaki düğmeyi kullanın.

    ```bash
    !pip install numpy
    !pip install matplotlib
    !pip install pandas
    !pip install sklearn
    ```

1. Hücreyi çalıştırmadan önce, araç **+** çubuğundaki düğmeyle yeni bir hücre oluşturun, işaretle işaretleyin ve aşağıdaki açıklamayı girin:

    ```markdown
    Note that when you run a code block that contains install commands, and also those with `import` statements, it make take the notebooks a little time to complete the task. To the left of the code block you see `In [*]` to indicate that execution is happening. The Notebook's kernel on the upper right also shows a filled-in circle to indicate "busy."
    ```

1. Not defterindeki tüm hücreleri çalıştırmak için **Tüm** > hücre**çalıştır** komutunu seçin. Markdown hücrelerinin HTML olarak işlendiğine ve komutun çekirdekte çalışmasına dikkat edin ve Markdown'da açıklandığı gibi çekirdek göstergesine dikkat edin:

    ![Not defteri çekirdeği için meşgul göstergesi](media/tutorial/tutorial-kernel-busy.png)

1. Ayrıca tüm `pip install` komutların çalışması biraz zaman alır ve bu paketleri proje ortamına zaten yüklediğiniz için (ve varsayılan olarak Azure Not Defterlerine de dahil oldukları için), "Gereksinim zaten karşılandı" yazan birçok ileti görürsünüz. Tüm bu çıktı görsel olarak dikkat dağıtıcı olabilir, bu nedenle bu hücreyi seçin (tek bir tıklama kullanarak), sonra çıktıyı gizlemek için **Hücre** > **Hücre Çıkışları** > **Toggle** kullanın. Çıktıyı tamamen kaldırmak için aynı alt menüdeki **Clear** komutunu da kullanabilirsiniz.

    **Toggle** komutu yalnızca hücreden en son çıktıyı gizler; hücreyi yeniden çalıştırıyorsanız, çıktı yeniden görüntülenir.

1. Paketler proje ortamına yüklü olduğundan, `! pip install` komutları kullanarak `#`yorum yapın; bu şekilde not defterinde öğretim malzemesi olarak kalabilirler, ancak çalıştırmak için zaman almazlar ve gereksiz çıktı üretmezler. Bu durumda, yorumlanan komutları not defterinde tutmak da not defterinin bağımlılıklarını gösterir.

    ```bash
    # !pip install numpy
    # !pip install matplotlib
    # !pip install pandas
    # !pip install sklearn
    ```

## <a name="create-the-remaining-cells"></a>Kalan hücreleri oluşturma

Not defterinin geri kalanını doldurmak için, bir sonraki bir dizi Markdown ve kod hücresi oluşturursunuz. Aşağıda listelenen her hücre için önce yeni hücreyi oluşturun, sonra türü ayarlayın, ardından içeriğe yapıştırın.

Her hücreyi oluşturduktan sonra not defterini çalıştırmak için beklenebilir, ancak oluşturduğunuz da her hücreyi çalıştırmak ilginçtir. Tüm hücreler çıktı göstermez; herhangi bir hata görmüyorsanız, hücrenin normal olarak çalıştırdığını varsayınız.

Her kod hücresi önceki hücrelerde çalıştırılabilen koda bağlıdır ve hücrelerden birini çalıştırmayı ihmal ederseniz, daha sonraki hücreler hatalara neden olabilir. Bir hücreyi çalıştırmayı unuttuğunuz bir sözcük bulursanız, geçerli **hücreyi** > çalıştırmadan önce**Tüm Hücreyi Çalıştır'ı** kullanmayı deneyin.

Beklenmeyen sonuçlar görürseniz (ki büyük olasılıkla göreceksiniz!), her hücrenin gerektiğinde "Kod" veya "İşaretle" olarak ayarlanıp ayarlandığını denetleyin. Örneğin, Kod hücresine Markdown'u girdiğinizde genellikle "Geçersiz sözdizimi" hatası oluşur.

1. İşaretleç hücresi:

    ```markdown
    ## Import packages and prepare the dataset

    In this example we're using numpy, pandas, and matplotlib. Data is in the file cricket_chirps.csv. Because this file is in the same project as this present Notebook, we can just load it using a relative pathname.
    ```

1. Kod hücresi; çalıştırıldığında, tablo içeriğini çıktı olarak gösterir. İfadeyi yorumlayarak çıktıyı `print` bastırabilirsiniz.

    ```python
    import numpy as np
    import pandas as pd

    dataset = pd.read_csv('cricket_chirps.csv')
    print(dataset)
    X = dataset.iloc[:, :-1].values  # Matrix of independent variables -- remove the last column in this data set
    y = dataset.iloc[:, 1].values    # Matrix of dependent variables -- just the last column (1 == 2nd column)
    ```

    > [!Note]
    > Bu koddan "numpy.dtype boyutu değiştirildi" ile ilgili uyarılar görebilirsiniz; uyarılar güvenle yoksayılabilir.

1. İşaretleç hücresi:

    ```markdown
    Next, split the dataset into a Training set (2/3rds) and Test set (1/3rd). We don't need to do any feature scaling because there is only one column of independent variables, and packages typically do scaling for you.
    ```

1. Kod hücresi; çalıştırıldığında, bu hücrenin çıktısı yoktur.

    ```python
    from sklearn.model_selection import train_test_split

    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 1/3, random_state = 0)
    ```

1. İşaretleç hücresi:

    ```markdown
    ## Fit the data to the training set

    "Fitting" the data to a training set means making the line that describes the relationship between the independent and the dependent variables. With a simple data set like we're using here, you can visualize the line on a simple x-y plot: the x-axis is the independent variable (chirp count in this example), and the y-axis is the independent variable (temperature). Fitting the data means plotting all the points in the training set, then drawing the best-fit line through that data.

    With two independent variables you can imagine a three-dimensional plot with a line fitted to the data. At three or more independent variables, however, it's no longer easy to visualize the fit, but you get the idea. In the end, it's all just mathematics, which a computer can handle easily without having to form a mental picture!

    The regressor's `fit` method here creates the line, which algebraically is of the form `y = x*b1 + b0`, where b1 is the coefficient or slope of the line (which you can get to through `regressor.coef_`), and b0 is the intercept of the line at x=0 (which you can get to through `regressor.intercept_`).
    ```

1. Kod hücresi; çalıştırıldığında, bu hücre çıktıyı gösterir. `LinearRegression(copy_X=True, fit_intercept=True, n_jobs=None,normalize=False)`

    ```python
    from sklearn.linear_model import LinearRegression

    regressor = LinearRegression()    # This object is the regressor, that does the regression
    regressor.fit(X_train, y_train)   # Provide training data so the machine can learn to predict using a learned model.
    ```

1. İşaretleç hücresi:

    ```markdown
    ## Predict the results

    With the regressor in hand, we can predict the test set results using its `predict` method. That method takes a vector of independent variables for which you want predictions.

    Because the regressor is fit to the data by virtue of `coef_` and `intercept_`, a prediction is the result of `coef_ * x + intercept_`. (Indeed, `predict(0)` returns `intercept_` and `predict(1)` returns `intercept_ + coef_`.)

    In the code, the `y_test` matrix (from when we split the set) contains the real observations. `y_pred` assigned here contains the predictions for the same `X_test` inputs. It's not expected that the test or training points exactly fit the regression; the regression is trying to find the model that we can use to make predictions with new observations of the independent variables.
    ```

1. Kod hücresi; çalıştırıldığında, bu hücre `[79.49588055 75.98873911 77.87719989 80.03544077 75.17939878]`gibi sonuçları gösterir.

    ```python
    y_pred = regressor.predict(X_test)
    print(y_pred)
    ```

1. İşaretleç hücresi:

    ```markdown
    It's interesting to think that all the "predictions" we use in daily life, like weather forecasts, are just regression models of some sort working with various data sets. Those models are much more complicated than what's shown here, but the idea is the same.

    Knowing how predictions work help us understand that the actual observations we would collect in the moment will always be somewhat off from the predictions: the predictions fit exactly to the model, whereas the observations typically won't.

    Of course, such systems feed new observations back into the dataset to continually improve the model, meaning that predictions should get more accurate over time.

    The challenge is determining what data to actually use. For example, with weather, how far back in time do you go? How have weather patterns been changing decade by decade? In any case, something like weather predictions will be doing things hour by hour, day by day, for things like temperature, precipitation, winds, cloud cover, etc. Radar and other observations are of course fed into the model and the predictions are reduced to mathematics.
    ```

1. İşaretleç hücresi:

    ```markdown
    ## Visualize the results

    The following code generates a plot: green dots are training data, red dots are test data, blue dots are predictions. Gray line is the regression itself. You see that all the blue dots are exactly on the line, as they should be, because the predictions exactly fit the model (the line).
    ```

1. Kod hücresi; çalıştırıldığında, bu hücre bir grafik çizimi üretir. Çizimi ilk kez görmüyorsanız (ve bunun yerine "Şekil boyutu 640x480'i 1 Eksenli" olarak görüyorsanız), hücreyi yeniden çalıştırın.

    ```python
    import matplotlib.pyplot as plt

    plt.scatter(X_train, y_train, color = 'green')
    plt.scatter(X_test, y_test, color = 'red')   
    plt.scatter(X_test, y_pred, color = 'blue')  # The predicted temperatures of the same X_test input.
    plt.plot(X_train, regressor.predict(X_train), color = 'gray')
    plt.title('Temperature based on chirp count')
    plt.xlabel('Chirps/minute')
    plt.ylabel('Temperature')
    plt.show()
    ```

    ![Matplotlib kodundan çizim çıktısı](media/tutorial/tutorial-plot-output.png)

1. İşaretleç hücresi:

    ```markdown
    ## Closing comments

    At the end of the day, when you create a model, you use training data. Then you start feeding test data (real observations) to see how well the model actually works. You may find that the model is a little inaccurate over time, in which case you retrain the model with some new data. Retraining the model means you're creating a new fit line that's used for predictions.

    This regression can be used to examine the variability of the relationship between temperature and chirp count. Of course, if the model proves too inaccurate (that is, the predictions aren't very good), then it suggests that we might need to introduce more independent variables like humidity, time of year, latitude, amount of moonlight, and so on. If you have such data, you can do separate lines regressions for each independent variable, and then do multiple regressions with combinations of independent variables. 

    Again, once you are working with more than one or two independent variables, it's much easier to use machine learning to crunch the numbers than to try to visualize it graphically.
    ```

## <a name="clear-outputs-and-rerun-all-cells"></a>Çıkışları temizleyin ve tüm hücreleri yeniden çalıştırın

Tüm not defterini doldurmak için önceki bölümdeki adımları izleyerek, doğrusal regresyon hakkında tam bir öğretici bağlamında her iki çalışma kodu parçasını da oluşturdunuz. Bu doğrudan kod ve metin birleşimi dizüstü bilgisayarların en büyük avantajlarından biridir!

Şimdi tüm dizüstü yeniden çalıştırmayı deneyin:

1. **Çekirdek** > **Yeniden Başlat & Net Çıktı'yı**seçerek çekirdeğin tüm oturum verilerini ve tüm hücre çıktısını temizleyin. Bu komut, kod hücreleri arasında garip bağımlılıklar oluşturmadığınızdan emin olmak için bir not defterini tamamladığınızda çalıştırılan iyi bir komuttur.

1. **Hücre** > **Tümlerini Çalıştır'ı**kullanarak not defterini yeniden çalıştırın. Kod çalışırken çekirdek göstergesinin doldurulduğuna dikkat edin.

    Çok uzun süre çalışan veya başka bir şekilde sıkışmış herhangi bir kod varsa, **Çekirdek** > **Kesme** komutunu kullanarak çekirdek durdurabilirsiniz.

1. Sonuçları incelemek için not defterinde ilerleyin. (Yine çizim görünmüyorsa, bu hücreyi yeniden çalıştırın.)

## <a name="save-halt-and-close-the-notebook"></a>Not defterini kaydedin, durdurun ve kapatın

Bir not defterini düzenlerken, **Dosya** > **Kaydet ve Denetim Noktası** komutu veya araç çubuğundaki kaydet düğmesiyle geçerli durumunu kaydedebilirsiniz. "Denetim noktası", oturum sırasında istediğiniz zaman geri dönebileceğiniz bir anlık görüntü oluşturur. Denetim noktaları bir dizi deneysel değişiklik yapmanızı sağlar ve bu değişiklikler işe yaramazsa, **Dosya** > **Geri Dönüşü denetim noktasına** dosya komutunu kullanarak bir denetim noktasına geri dönebilirsiniz. Alternatif bir yaklaşım, fazladan hücreler oluşturmak ve çalıştırmak istemediğiniz kodları yorumlamaktır; her iki şekilde de çalışır.

**Ayrıca,** > not defterinin geçerli durumunun bir kopyasını projenizdeki yeni bir dosyaya dönüştürmek için istediğiniz zaman Dosya**Yap** komutunu da kullanabilirsiniz. Bu kopya yeni bir tarayıcı sekmesinde otomatik olarak açılır.

Not defteriyle yaptığınız da, not defterini kapatıp çalıştıran çekirdeği kapatan **Dosya** > **Kapat ve durdur** komutunu kullanın. Azure Notebook'lar daha sonra tarayıcı sekmesini otomatik olarak kapatır.

## <a name="debug-notebooks-using-visual-studio-code"></a>Visual Studio Code kullanarak hata ayıklama defterleri

Not defterinizdeki kod hücreleri beklediğiniz şekilde davranış yoksa, kod hataları nız veya başka hatalarınız olabilir. Ancak, değişkenlerin `print` değerini göstermek için deyimleri kullanmak dışında, tipik bir Jupyter ortamı herhangi bir hata ayıklama olanakları sunmaz.

Neyse ki, not defterinin *.ipynb* dosyasını indirebilir, python uzantısını kullanarak Visual Studio Code'da açabilirsiniz. Uzantı, doğrudan tek bir kod dosyası olarak bir not defteri içeri aktarır ve yorumlarda Markdown hücrelerini korur. Not defterini aldıktan sonra, kodunuzu geçmek, kesme noktaları ayarlamak, durumu incelemek vb. için Visual Studio Code hata ayıklayıcısını kullanabilirsiniz. Kodunuzda düzeltmeyaptıktan sonra *.ipynb* dosyasını Visual Studio Code'tan dışa aktarır ve Azure Not Defterleri'ne geri yüklersiniz.

Daha fazla bilgi için Visual Studio Code belgelerinde [bir Jupyter not defterini debug](https://code.visualstudio.com/docs/python/jupyter-support#debug-a-jupyter-notebook) olarak görün.

Ayrıca Visual Studio Code - Jupyter dizüstü bilgisayarlar için Visual Studio Code ek özellikleri için [Jupyter desteğine](https://code.visualstudio.com/docs/python/jupyter-support) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Örnek not defterlerini keşfedin](azure-notebooks-samples.md)

Nasıl-makaleler:

- [Projeleri oluşturma ve kopyalama](create-clone-jupyter-notebooks.md)
- [Projeleri yapılandırma ve yönetme](configure-manage-azure-notebooks-projects.md)
- [Paketleri dizüstü bilgisayarın içinden yükleme](install-packages-jupyter-notebook.md)
- [Slayt gösterisi yapma](present-jupyter-notebooks-slideshow.md)
- [Veri dosyalarıyla çalışma](work-with-project-data-files.md)
- [Veri kaynaklarına erişme](access-data-resources-jupyter-notebooks.md)
- [Azure Machine Learning’i kullanma](use-machine-learning-services-jupyter-notebooks.md)
