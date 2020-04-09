---
title: 'Öğretici: Görüntü sınıflandırması için bir etiketleme projesi oluşturma'
titleSuffix: Azure Machine Learning
description: Çok sınıflı görüntü sınıflandırma modellerinde kullanılabilen görüntüleri etiketleme işlemini nasıl yöneteceğinizi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: sgilley
author: sdgilley
ms.reviewer: ranku
ms.date: 04/02/2020
ms.openlocfilehash: fa33861d86ff8bee3e2a34fb3d93032ac6180880
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879724"
---
# <a name="tutorial-create-a-labeling-project-for-multi-class-image-classification"></a>Öğretici: Çok sınıflı görüntü sınıflandırması için bir etiketleme projesi oluşturma 
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu öğretici, makine öğrenimi modelleri oluşturmak için veri olarak kullanılacak etiketleme (etiketleme olarak da adlandırılır) görüntüleri nasıl yöneteceğinizi gösterir. Azure Machine Learning'de veri etiketleme genel önizlemede yer alıyor.

Görüntüleri sınıflandırmak için bir makine öğrenme modeli eğitmek istiyorsanız, doğru etiketlenmiş yüzlerce hatta binlerce görüntüye ihtiyacınız vardır.  Azure Machine Learning, verilerinizi etiketleyerken etki alanı uzmanlarından oluşan özel ekibinizin ilerlemesini yönetmenize yardımcı olur.
 
Bu eğitimde, kedi ve köpek görüntüleri kullanacağız.  Her görüntü bir kedi ya da bir köpek olduğundan, bu *çok sınıflı* bir etiketleme projesidir. Şunları öğrenirsiniz:

> [!div class="checklist"]
>
> * Bir Azure depolama hesabı oluşturun ve hesaba resim yükleyin.
> * Azure Machine Learning çalışma alanı oluşturun.
> * Çok sınıflı bir görüntü etiketleme projesi oluşturun.
> * Verilerinizi etiketle.  Siz veya etiketleyicileriniz bu görevi gerçekleştirebilirsiniz.
> * Verileri gözden geçirerek ve dışa aktararak projeyi tamamlayın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://aka.ms/AMLFree)oluşturun.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Azure Machine Learning çalışma alanı, makine öğrenimi modellerini denemek, eğitmek ve dağıtmak için bulutta kullandığınız temel bir kaynaktır. Azure aboneliğinizi ve kaynak grubunuzu hizmette kolayca tüketilen bir nesneye bağlar.

Azure kaynaklarınızı yönetmek için web tabanlı bir konsol olan Azure portalı üzerinden bir çalışma alanı oluşturursunuz.

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal.md)]

## <a name="start-a-labeling-project"></a>Etiketleme projesi başlatma

Daha sonra, tüm beceri düzeylerindeki veri bilimi uygulayıcıları için veri bilimi senaryoları gerçekleştirmek için makine öğrenimi araçlarını içeren birleştirilmiş arayüz olan Azure Machine Learning stüdyosunda veri etiketleme projesini yöneteceksiniz. Stüdyo Internet Explorer tarayıcılarında desteklenmez.

1. [Azure Machine Learning stüdyosunda](https://ml.azure.com)oturum açın.

1. Aboneliğinizi ve oluşturduğunuz çalışma alanını seçin.

### <a name="create-a-datastore"></a><a name="create-datastore"></a>Veri deposu oluşturma

Azure Machine Learning veri depoları, abonelik kimliğiniz ve belirteç yetkilendirmeniz gibi bağlantı bilgilerini depolamak için kullanılır. Burada, bu öğreticinin resimlerini içeren depolama hesabına bağlanmak için bir veri deposu kullanıyorsunuz.

1. Çalışma alanınızın sol tarafında **Veri Depoları'nı**seçin.

1. + **Yeni datastore'u**seçin.

1. Formu şu ayarlarla doldurun:

    Alan|Açıklama 
    ---|---
    Datastore adı | Veri deposuna bir ad verin.  Burada **labeling_tutorial.**
    Datastore türü | Depolama alanının türünü seçin.  Burada, görüntüler için tercih edilen depolama alanı olan **Azure Blob Depolama'yı**kullanıyoruz.
    Hesap seçim yöntemi | **El ile Gir'i**seçin.
    URL'si | `https://azureopendatastorage.blob.core.windows.net/openimagescontainer`
    Kimlik doğrulaması türü | **SAS belirteci'ni**seçin.
    Hesap anahtarı | `?sv=2019-02-02&ss=bfqt&srt=sco&sp=rl&se=2025-03-25T04:51:17Z&st=2020-03-24T20:51:17Z&spr=https&sig=7D7SdkQidGT6pURQ9R4SUzWGxZ%2BHlNPCstoSRRVg8OY%3D`

1. Veri deposunu oluşturmak için **Oluştur'u** seçin.

### <a name="add-labelers-to-workspace"></a>Çalışma alanına etiket çiler ekleme

Çalışma alanınızı, projelerinizin herhangi biri için verileri etiketleyecek tüm kişileri içerecek şekilde ayarlayın.  Daha sonra bu etiketleyicileri özel etiketleme projenize eklersiniz.

1. Sol tarafta Veri **etiketlemesini**seçin.

1. Sayfanın üst kısmında **Etiketçiler'i**seçin.

1. Etiketleyicinin e-posta adresini eklemek için **etiketleyici ekle'yi** seçin.

1. Bitirene kadar daha fazla etiket eklemeye devam edin.

### <a name="create-a-labeling-project"></a>Etiketleme projesi oluşturma

Artık etiketleyici listenize ve etiketlemek istediğiniz verilere erişmeye sahip olduğunuza göre, etiketleme projenizi oluşturun.

1. Sayfanın üst kısmında **Projeler'i**seçin.

1. Seçin **+ Proje ekle**.

    ![Proje oluşturma](media/tutorial-labeling/create-project.png)

### <a name="project-details"></a>Proje ayrıntıları

1. **Proje ayrıntıları** formu için aşağıdaki girişi kullanın:

    Alan|Açıklama 
    ---|---
    Proje adı | Projenize bir ad verin.  Burada **öğretici-kediler-n-köpekler**kullanacağız.
    Görev türünü etiketleme | **Resim Sınıflandırma Çok sınıf seçin.**  
    
    Projeyi oluşturmaya devam etmek için **İleri'yi** seçin.

### <a name="select-or-create-a-dataset"></a>Bir veri kümesi seçme veya oluşturma

1.   Bir veri kümesi formu **seçin veya oluşturun,** ikinci seçeneği seçin, **bir veri kümesi oluşturun,** ardından **veri deposundan**bağlantıyı seçin.

1. **Datastore formundan veri kümesi oluştur** için aşağıdaki girişi kullanın:

    1. Temel **bilgi** formunda, bir ad ekleyin, burada **resimler-for-tutorial**kullanacağız.  İsterseniz bir açıklama ekleyin.  Ardından **İleri'yi**seçin.
    1. **Datastore seçim** formunda, **daha önce oluşturulmuş veri deponuzu** **(örneğin tutorial_images (Azure Blob Depolama)** seçmek için açılır dosyayı kullanın
    1. Ardından, **Datastore seçim** formunda hala, **Gözat'ı** seçin ve **ardından MultiClass - DogsCats'i**seçin.  Yol olarak **/MultiClass - DogsCats'i** kullanmak için **Kaydet'i** seçin.
    1. Ayrıntıları onaylamak için **İleri'yi** seçin ve ardından veri kümesini oluşturmak için **Oluşturun'ı** oluşturun.
    1. Listedeki veri kümesi adının yanındaki daireyi seçin( örneğin **resimler-öğretici için.**

1. Projeyi oluşturmaya devam etmek için **İleri'yi** seçin.

### <a name="label-classes"></a>Etiket sınıfları

1. Etiket **sınıfları** formunda, bir etiket adı yazın ve bir sonraki etiketi yazmak için **+Etiket Ekle'yi** seçin.  Bu proje için, etiketler **Cat**, **Köpek**, ve **Belirsiz**.

1. Tüm etiketleri eklediğinde **İleri'yi** seçin.

### <a name="labeling-instructions"></a>Yönergeleri etiketleme

1. Etiketleme **talimatları** formunda, etiketleyicileriniz için ayrıntılı yönergeler sağlayan bir web sitesine bağlantı sağlayabilirsiniz.  Bu öğretici için boş bırakacağız.

1. Ayrıca, doğrudan forma görevin kısa bir açıklamasını ekleyebilirsiniz.  Tip **Etiketleme öğretici - Kediler & Köpekler.**

1. **Sonraki'ni**seçin.

1. ML **destekli etiketleme** formunda onay kutusunu işaretsiz bırakın. ML destekli etiketleme, bu öğreticide kullanacağından daha fazla veri gerektirir.

1. **Create project** (Proje oluştur) öğesini seçin.

Bu sayfa otomatik olarak yenilenmez. Duraklattıktan sonra, projenin durumu **Created**olarak değişene kadar sayfayı el ile yenileyin.

### <a name="add-labelers-to-your-project"></a>Projenize etiketleyici ekleme

Bu projeye etiketleyicilerinizin bazılarını veya tümlerini ekleyin.

1. Projeyi açmak için proje adını seçin.  

1. Sayfanın üst kısmında **Takımlar'ı**seçin.

1. Varsayılan **Takım** bağlantısını labeling_tutorial seçin.

1. Şimdi bu projeye katılmak istediğiniz etiketleyicileri eklemek için **etiketat'ı** kullanın. 

1. Daha önce oluşturduğunuz etiketçiler listesinden seçim alın.  Kullanmak istediğiniz tüm etiketçileri seçtikten sonra, varsayılan proje ekibinize eklemek için **etiket atay'ı** seçin.

## <a name="start-labeling"></a>Etiketlemeyi başlat

Azure kaynaklarınızı şimdi ayarladınız ve bir veri etiketleme projesi yapılandırıldınız. Verilerinize etiket ekleme nin zamanı.

### <a name="notify-labelers"></a>Etiketleyicilere bildirin

Etiketlemeniz gereken çok sayıda görüntü varsa, umarım görevi tamamlamak için çok sayıda etiketleyiciniz de vardır.  Artık verilere erişebilmeleri ve etiketlemeye başlayabilmeleri için onlara yönergeler göndermek isteyeceksiniz.

1. [Machine Learning stüdyosunda,](https://ml.azure.com)projenizi bulmak için sol tarafta **veri etiketlemesini** seçin.  

1. Proje adı bağlantısını seçin.

1. Sayfanın üst kısmında **Ayrıntılar'ı**seçin.  Projenizin bir özetini görüyorsunuz.

    ![Proje ayrıntıları](media/tutorial-labeling/project-details.png)

1. Etiketleyicilerinize göndermek için **Etiketleme portalı URL** bağlantısını kopyalayın.

1. Şimdi etiketleme ekibinizi bulmak için en üstteki **Takım'ı** seçin.  

1. Takım adı bağlantısını seçin.

1. Sayfanın üst kısmında, e-postanızı başlatmak için **E-posta ekibini** seçin.  Az önce kopyaladığınız etiketleme portalı URL'ye yapıştırın.  

Bir etiketleyici portal URL'sine her gittiğinde, sıra boşalana kadar etiketlenecek daha fazla resim sunulur.  

### <a name="tag-the-images"></a>Görüntüleri etiketleme

Öğreticinin bu bölümünde, rolleri *proje yöneticisinden* bir *etiketleyicininkine*değiştirirsiniz.  Ekibe gönderdiğiniz URL'yi kullanın.  Bu URL sizi projenizin etiketleme portalına getirir.  Talimatları ekleseydiniz, sayfaya geldiğinizde onları burada görürdün.

1. Sayfanın üst kısmında, etiketlemeye başlamak için **Görevler'i** seçin.

1. Tek bir kerede etiketlemek istediğiniz resim sayısını görüntülemek için sağtarafta küçük resim resmi seçin. Devam etmeden önce tüm bu görüntüleri etiketlemeniz gerekir. Yalnızca etiketlenmemiş yeni bir veri sayfanız olduğunda mizanpajları değiştirin. Düzenler arasında geçiş yapmak, sayfanın devam eden etiketleme çalışmasını temizler.

1. Bir veya daha fazla resim seçin ve ardından seçime uygulamak için bir etiket seçin. Etiket resmin altında görünür.  Sayfadaki tüm resimleri seçmeye ve etiketlemeye devam edin.  Görüntülenen tüm görüntüleri aynı anda seçmek için **Tümünü Seç'i**seçin. Etiket uygulamak için en az bir resim seçin.


    > [!TIP]
    > Klavyenizdeki sayı tuşlarını kullanarak ilk dokuz etiketi seçebilirsiniz.

1. Sayfadaki tüm resimler etiketlendikten sonra, bu etiketleri göndermek için **Gönder'i** seçin.

    ![Resimleri etiketleme](media/tutorial-labeling/catsndogs.gif)

1. Eldeki veriler için etiketler gönderdikten sonra Azure, sayfayı iş kuyruğundan yeni bir resim kümesiyle yeniler.

## <a name="complete-the-project"></a>Projeyi tamamlayın

Şimdi rolleri etiketleme projesi için *proje yöneticisine* geri değiştirirsiniz.

Bir yönetici olarak, etiketleyicinizin çalışmasını gözden geçirmek isteyebilirsiniz.  

### <a name="review-labeled-data"></a>Etiketli verileri gözden geçirme

1. [Machine Learning stüdyosunda,](https://ml.azure.com)projenizi bulmak için sol tarafta **veri etiketlemesini** seçin.  

1. Proje adı bağlantısını seçin.

1. Pano, projenizin ilerlemesini gösterir.

1. Sayfanın üst kısmında **Veri'yi**seçin.

1. Sol tarafta, etiketli resimlerinizi görmek için **Etiketli verileri** seçin.  

1. Bir etiketle aynı fikirde değilseniz, resmi seçin ve sayfanın alt kısmında **Reddet'i** seçin.  Etiketler kaldırılır ve görüntü etiketlenmemiş görüntüler kuyruğuna geri konur.

### <a name="export-labeled-data"></a>Etiketli verileri dışa aktarma

Makine Öğrenimi denemesi için etiket verilerini istediğiniz zaman dışa aktarabilirsiniz. Kullanıcılar genellikle tüm görüntülerin etiketlenmesini beklemek yerine birden çok kez dışa aktarıp farklı modeller eğiterler.

Resim etiketleri [COCO biçiminde](http://cocodataset.org/#format-data) veya Azure Machine Learning veri kümesi olarak dışa aktarılabilir. Veri seti biçimi, Azure Machine Learning'de eğitim için kullanımı kolaylaştırır.  

1. [Machine Learning stüdyosunda,](https://ml.azure.com)projenizi bulmak için sol tarafta **veri etiketlemesini** seçin.  

1. Proje adı bağlantısını seçin.

1. **Dışa Aktar'ı** seçin ve **Azure ML Dataset olarak Dışa Aktar'ı**seçin. 

    Dışa aktarma durumu **Dışa Aktarma** düğmesinin hemen altında görünür. 

1. Etiketler başarıyla dışa aktarıladıktan sonra, sonuçları görüntülemek için sol taraftaki **Veri Kümeleri'ni** seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme


[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, görüntüleri etiketlediniz.  Şimdi etiketli verilerinizi kullanın:

> [!div class="nextstepaction"]
> [Tren bir makine öğrenme görüntü tanıma modeli.](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/labeled-datasets/labeled-datasets.ipynb)
