---
title: Azure Machine Learning ile tümleştirme Azure Stream Analytics
description: Bu makalede, Kullanıcı tanımlı bir işlev kullanarak Azure Machine Learning tümleştiren basit bir Azure Stream Analytics işinin nasıl hızlı bir şekilde ayarlanacağı açıklanır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 08/12/2020
ms.custom: seodec18
ms.openlocfilehash: 26a1208131f1d9d3df7dccd8e27bda37992f043f
ms.sourcegitcommit: 3bf69c5a5be48c2c7a979373895b4fae3f746757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88236689"
---
# <a name="do-sentiment-analysis-with-azure-stream-analytics-and-azure-machine-learning-studio-classic"></a>Azure Stream Analytics ve Azure Machine Learning Studio (klasik) ile yaklaşım analizini yapın

Bu makalede, yaklaşım analizi için Azure Machine Learning Studio (klasik) kullanan basit bir Azure Stream Analytics işinin nasıl ayarlanacağı gösterilmektedir. Akış metin verilerini analiz etmek ve yaklaşım Puanını öğrenmek için Cortana Intelligence Gallery Machine Learning bir yaklaşım Analizi modeli kullanırsınız.

> [!TIP]
> Daha iyi performans ve güvenilirlik için Azure Machine Learning Studio (klasik) UDF yerine [Azure Machine Learning UDF 'leri](machine-learning-udf.md) kullanmanız önemle önerilir.

Bu makaleden öğrendiklerinizi, bunlar gibi senaryolar için uygulayabilirsiniz:

* Veri akışı Twitter verilerinde gerçek zamanlı yaklaşım çözümleniyor.
* Destek personeli ile müşteri sohbetleri kayıtlarını analiz etme.
* Forumlar, blogların ve videoların açıklamalarını değerlendirme.
* Diğer birçok gerçek zamanlı, tahmine dayalı puan senaryosu.

Oluşturduğunuz Akış Analizi işi, blob deposundan örnek metin verileri üzerinde Kullanıcı tanımlı bir işlev (UDF) olarak, yaklaşım Analizi modelini uygular. Çıktı (yaklaşım analizinin sonucu), farklı bir CSV dosyasındaki aynı blob deposuna yazılır. 

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunlara sahip olduğunuzdan emin olun:

* Etkin bir Azure aboneliği.

* İçindeki bazı Twitter verilerine sahip bir CSV dosyası. [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv)'dan örnek bir dosya indirebilir veya kendi dosyanızı oluşturabilirsiniz. Gerçek dünyada bir senaryoda, verileri doğrudan Twitter veri akışından alırsınız.

## <a name="create-a-storage-container-and-upload-the-csv-input-file"></a>Bir depolama kapsayıcısı oluşturun ve CSV giriş dosyasını karşıya yükleyin

Bu adımda, depolama kapsayıcınıza bir CSV dosyası yüklersiniz.

1. Azure Portal, **kaynak**  >  **depolama**  >  **depolama hesabı**oluştur ' u seçin.

2. *Temel bilgiler* sekmesini aşağıdaki ayrıntılarla doldurun ve kalan alanlar için varsayılan değerleri bırakın:

   |Alan  |Değer  |
   |---------|---------|
   |Abonelik|Aboneliğinizi seçin.|
   |Kaynak grubu|Kaynak grubunuzu seçin.|
   |Depolama hesabı adı|Depolama hesabınız için bir ad girin. AD Azure genelinde benzersiz olmalıdır.|
   |Konum|Konum seçin. Tüm kaynaklar aynı konumu kullanmalıdır.|
   |Hesap türü|BlobStorage|

   ![depolama hesabı ayrıntılarını sağlayın](./media/stream-analytics-machine-learning-integration-tutorial/create-storage-account1.png)

3. **Gözden geçir + oluştur**' u seçin. Ardından, depolama hesabınızı dağıtmak için **Oluştur** ' u seçin.

4. Dağıtım tamamlandığında depolama hesabınıza gidin. **Blob hizmeti** bölümünden **Kapsayıcılar**’ı seçin. Ardından yeni bir kapsayıcı oluşturmak için **+ kapsayıcı** ' yı seçin.

   ![Giriş için BLOB depolama kapsayıcısı oluştur](./media/stream-analytics-machine-learning-integration-tutorial/create-storage-account2.png)

5. Kapsayıcı için bir ad sağlayın ve **genel erişim düzeyinin** **özel**olarak ayarlandığını doğrulayın. İşiniz bittiğinde **Oluştur**'u seçin.

   ![blob kapsayıcısı ayrıntılarını belirtin](./media/stream-analytics-machine-learning-integration-tutorial/create-storage-account3.png)

6. Yeni oluşturulan kapsayıcıya gidin ve **karşıya yükle**' yi seçin. Daha önce indirdiğiniz **sampleinput.csv** dosyasını karşıya yükleyin.

   ![Bir kapsayıcı için ' karşıya yükle ' düğmesi](./media/stream-analytics-machine-learning-integration-tutorial/create-sa-upload-button.png)

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Cortana Intelligence Gallery yaklaşım Analytics modelini ekleme

Örnek veriler bir blob 'da olduğuna göre, Cortana Intelligence Gallery ' de yaklaşım Analizi modelini etkinleştirebilirsiniz.

1. Cortana Intelligence Gallery, tahmine [dayalı yaklaşım Analizi modeli](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) sayfasına gidin.  

2. **Studio 'Da aç (klasik)** seçeneğini belirleyin.  
   
   ![Stream Analytics Machine Learning, Machine Learning Studio açın](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. Çalışma alanına gitmek için oturum açın. Bir konum seçin.

4. Sayfanın alt kısmındaki **Çalıştır** ' ı seçin. İşlem çalışır ve yaklaşık bir dakika sürer.

   ![deneme Machine Learning Studio Çalıştır](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-run-experiment.png)  

5. İşlem başarıyla çalıştırıldıktan sonra sayfanın alt kısmındaki **Web Hizmeti Dağıt** ' ı seçin.

   ![deneme Machine Learning Studio Web hizmeti olarak dağıtma](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-deploy-web-service.png)  

6. Yaklaşım Analizi modelinin kullanıma hazırlandığını doğrulamak için **Test** düğmesini seçin. "Microsoft seviyorum" gibi metin girişi sağlayın.

   ![Machine Learning Studio test denemesi](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test.png)  

   Test çalışırsa, aşağıdaki örneğe benzer bir sonuç görürsünüz:

   ![Machine Learning Studio test sonuçları](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test-results.png)  

7. **Uygulamalar** sütununda Excel **2010 veya önceki bir çalışma kitabı** bağlantısını seçerek Excel çalışma kitabını indirin. Çalışma kitabı, daha sonra Stream Analytics işini ayarlamak için gerekli olan API anahtarını ve URL 'YI içerir.

    ![Stream Analytics Machine Learning, Hızlı Bakış](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  

## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Machine Learning modelini kullanan bir Stream Analytics işi oluşturma

Artık, BLOB depolama alanındaki CSV dosyasından örnek çoklu alanı içeren bir Stream Analytics iş oluşturabilirsiniz.

### <a name="create-the-job"></a>İşi oluşturma

[Azure Portal](https://portal.azure.com) gidin ve bir Stream Analytics iş oluşturun. Bu işleme alışkın değilseniz, bkz. [Azure Portal kullanarak Stream Analytics Işi oluşturma](stream-analytics-quick-create-portal.md).

### <a name="configure-the-job-input"></a>İş girişini yapılandırma

İş, daha önce karşıya yüklediğiniz CSV dosyasındaki girişini BLOB depolama alanına alır.

1. Stream Analytics işinize gidin. **Iş topolojisi**altında **girişler** seçeneğini belirleyin. **Akış girişi**  > **BLOB depolaması**Ekle ' yi seçin.

2. **BLOB depolama** ayrıntılarını aşağıdaki değerlerle doldurun:

   |Alan  |Değer  |
   |---------|---------|
   |Girdi diğer adı|Girişe bir ad verin. Sorgunuzu yazdığınızda bu diğer adı unutmayın.|
   |Abonelik|Aboneliğinizi seçin.|
   |Depolama hesabı|Önceki adımda yaptığınız depolama hesabını seçin.|
   |Kapsayıcı|Önceki adımda oluşturduğunuz kapsayıcıyı seçin.|
   |Olay serileştirme biçimi|CSV|

3. **Kaydet**’i seçin.

### <a name="configure-the-job-output"></a>İş çıktısını yapılandırma

İş, sonuçları girdi aldığı BLOB depolama alanına gönderir.

1. Stream Analytics işinize gidin. **Iş topolojisi**altında **çıktılar** seçeneğini belirleyin. **Add**  >  **BLOB depolama**Ekle ' yi seçin.

2. **BLOB depolama** formunu şu değerlerle doldurun:

   |Alan  |Değer  |
   |---------|---------|
   |Girdi diğer adı|Girişe bir ad verin. Sorgunuzu yazdığınızda bu diğer adı unutmayın.|
   |Abonelik|Aboneliğinizi seçin.|
   |Depolama hesabı|Önceki adımda yaptığınız depolama hesabını seçin.|
   |Kapsayıcı|Önceki adımda oluşturduğunuz kapsayıcıyı seçin.|
   |Olay serileştirme biçimi|CSV|

3. **Kaydet**’i seçin.

### <a name="add-the-machine-learning-function"></a>Machine Learning işlevini ekleme

Daha önce bir Web hizmetine Machine Learning modeli yayımladınız. Bu senaryoda, Akış Analizi işi çalıştırıldığında, her örnek tweet, yaklaşım analizi için girişten Web hizmetine gönderilir. Machine Learning Web hizmeti, bir yaklaşım ( `positive` , `neutral` , veya `negative` ) ve Tweet 'in pozitif olma olasılığını döndürür.

Bu bölümde, Stream çözümleme işinde bir işlev tanımlarsınız. İşlevi, Web hizmetine bir Tweet göndermek ve yanıtı geri almak için çağrılabilir.

1. Excel çalışma kitabında daha önce indirdiğiniz Web hizmeti URL 'SI ve API anahtarına sahip olduğunuzdan emin olun.

2. Stream Analytics işinize gidin. Sonra **işlevler**  >  **+**  >  **Azure ML Studio** Ekle ' yi seçin

3. **Azure Machine Learning işlev** formunu aşağıdaki değerlerle doldurun:

   |Alan  |Değer  |
   |---------|---------|
   | İşlev diğer adı | Adı kullanın `sentiment` ve **Azure Machine Learning işlev ayarlarını el ile sağla**' yı seçin. Bu, size URL ve anahtar girme seçeneği sunar.      |
   | URL| Web hizmeti URL 'sini yapıştırın.|
   |Anahtar | API anahtarını yapıştırın. |

4. **Kaydet**' i seçin.

### <a name="create-a-query-to-transform-the-data"></a>Verileri dönüştürmek için bir sorgu oluşturma

Stream Analytics girişi incelemek ve işlemek için bildirime dayalı SQL tabanlı bir sorgu kullanır. Bu bölümde, girişten her Tweet okuyan ve sonra yaklaşım analizini gerçekleştirmek için Machine Learning işlevini çağıran bir sorgu oluşturacaksınız. Sorgu daha sonra sonucu tanımladığınız çıktıya (BLOB depolama) gönderir.

1. Stream Analytics işe genel bakış ' a dönün.

2. **Iş topolojisi**altında **sorgu**' yı seçin.

3. Aşağıdaki sorguyu girin:

    ```SQL
    WITH sentiment AS (  
    SELECT text, sentiment1(text) as result 
    FROM <input>  
    )  

    SELECT text, result.[Score]  
    INTO <output>
    FROM sentiment  
    ```    

   Sorgu, `sentiment` girişte bulunan her bir Tweet için yaklaşım analizini gerçekleştirmek üzere daha önce oluşturduğunuz işlevi () çağırır.

4. sorguyu kaydetmek için **Kaydet** ' i seçin.

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Stream Analytics işini başlatıp çıktıyı denetleyin

Artık Stream Analytics işini başlatabilirsiniz.

### <a name="start-the-job"></a>İşi başlatma

1. Stream Analytics işe genel bakış ' a dönün.

2. sayfanın en üstünde **Başlat** ' ı seçin.

3. **Başlangıç işi**' nde, **özel**' i seçin ve ardından CSV dosyasını blob depolamaya yüklemeden önce bir gün önce bir gün seçin. İşiniz bittiğinde **Başlat**’ı seçin.  

### <a name="check-the-output"></a>Çıktıyı denetleyin

1. **İzleme** kutusunda etkinlik görene kadar birkaç dakika boyunca işe çalışmasına izin verin.

2. Blob depolamanın içeriğini incelemek için normalde kullandığınız bir aracınız varsa, kapsayıcıyı incelemek için bu aracı kullanın. Alternatif olarak, Azure portal aşağıdaki adımları uygulayın:

      1. Azure portal, depolama hesabınızı bulun ve hesap içinde kapsayıcıyı bulun. Kapsayıcıda iki dosya görürsünüz: örnek arası ve Stream Analytics işi tarafından oluşturulan bir CSV dosyası içeren dosya.
      2. Oluşturulan dosyaya sağ tıklayın ve ardından **İndir**' i seçin.

3. Oluşturulan CSV dosyasını açın. Aşağıdaki örneğe benzer bir şey görürsünüz:  

   ![Stream Analytics Machine Learning, CSV görünümü](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  

### <a name="view-metrics"></a>Ölçümleri görüntüle

Ayrıca, işlevle ilgili ölçümleri de görüntüleyebilirsiniz Azure Machine Learning. İşle ilgili aşağıdaki ölçümler, işe genel bakış ' ın **izleme** kutusunda görüntülenir:

* **Işlev istekleri** bir Machine Learning Web hizmetine gönderilen istek sayısını gösterir.  
* **Işlev olayları** istekteki olay sayısını gösterir. Varsayılan olarak, bir Machine Learning Web hizmetine yapılan her istek en fazla 1.000 olay içerir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [REST API ve Machine Learning tümleştirin](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
