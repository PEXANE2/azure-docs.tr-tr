---
title: Azure Machine Learning ile Azure Akış Analizi entegrasyonu
description: Bu makalede, kullanıcı tanımlı bir işlev kullanarak Azure Machine Learning'i entegre eden basit bir Azure Akış Analizi işinin nasıl hızla ayarlanacağı açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/19/2020
ms.custom: seodec18
ms.openlocfilehash: b33aeeee03fa57d87a60fd4c1904d5e4a86dd004
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067084"
---
# <a name="perform-sentiment-analysis-with-azure-stream-analytics-and-azure-machine-learning-studio-classic"></a>Azure Stream Analytics ve Azure Machine Learning Studio (klasik) ile duygu analizi gerçekleştirin

Bu makalede, Azure Machine Learning Studio'ya (klasik) entegre olan basit bir Azure Akış Analizi işinin nasıl hızlı bir şekilde ayarlanacağı açıklanmaktadır. Akışmetin verilerini analiz etmek ve duygu puanını gerçek zamanlı olarak belirlemek için Cortana İstihbarat Galerisi'nden Machine Learning duygu analizi modelini kullanırsınız. Cortana Intelligence Suite'i kullanmak, bir duygu analizi modeli oluşturmanın inceliklerini düşünmeden bu görevi yerine getirmenizi sağlar.

> [!TIP]
> Gelişmiş performans ve güvenilirlik için Azure Machine Learning Studio (klasik) UDF yerine [Azure Machine Learning UDF'leri](machine-learning-udf.md) kullanmanız önerilir.

Bu makalede öğrendiklerinizi bu gibi senaryolara uygulayabilirsiniz:

* Twitter verilerinin akışında gerçek zamanlı duyarlılığı analiz etmek.
* Destek personeli ile müşteri sohbetlerinin kayıtlarını analiz etmek.
* Forumlarda, bloglarda ve videolardaki yorumları değerlendirme. 
* Diğer birçok gerçek zamanlı, tahmine dayalı puanlama senaryoları.

Gerçek bir senaryoda, verileri doğrudan bir Twitter veri akışından alırsınız. Öğreticiyi basitleştirmek için, Akış Analizi işinin Azure Blob depolama alanında bir CSV dosyasından tweet ler alacağı şekilde yazılır. Kendi CSV dosyanızı oluşturabilir veya aşağıdaki resimde gösterildiği gibi örnek bir CSV dosyası kullanabilirsiniz:

![CSV dosyasında gösterilen örnek tweetler](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

Oluşturduğunuz Streaming Analytics işi, blob deposundaki örnek metin verilerine kullanıcı tanımlı işlev (UDF) olarak duygu analizi modelini uygular. Çıktı (duyarlılık analizi sonucu) farklı bir CSV dosyasında aynı blob deposuna yazılır. 

Aşağıdaki şekil bu yapılandırmayı göstermektedir. Belirtildiği gibi, daha gerçekçi bir senaryo için, blob depolamayı Azure Etkinlik Hub'larından gelen Twitter verilerini akışla değiştirebilirsiniz. Ayrıca, toplam duyarlılığın [Microsoft Power BI](https://powerbi.microsoft.com/) gerçek zamanlı görselleştirmesini oluşturabilirsiniz.    

![Stream Analytics Machine Learning entegrasyonuna genel bakış](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce şunlara sahip olduğunuzdan emin olun:

* Etkin bir Azure aboneliği.
* Içinde bazı veriler bulunan bir CSV dosyası. Daha önce gösterilen dosyayı [GitHub'dan](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv)indirebilir veya kendi dosyanızı oluşturabilirsiniz. Bu makale için, dosyayı GitHub'dan kullandığınız varsayılır.

Yüksek düzeyde, bu makalede gösterilen görevleri tamamlamak için aşağıdakileri yaparsınız:

1. Bir Azure depolama hesabı ve blob depolama kapsayıcısı oluşturun ve kapsayıcıya CSV biçimli bir giriş dosyası yükleyin.
3. Cortana İstihbarat Galerisi'nden Azure Machine Learning Studio (klasik) çalışma alanına bir duygu analizi modeli ekleyin ve bu modeli Machine Learning çalışma alanında bir web hizmeti olarak dağıtın.
5. Metin girişine ilişkin duyarlılığı belirlemek için bu web hizmetini işlev olarak çağıran bir Akış Analizi işi oluşturun.
6. Stream Analytics işini başlatın ve çıktıyı kontrol edin.

## <a name="create-a-storage-container-and-upload-the-csv-input-file"></a>Bir depolama kapsayıcısı oluşturun ve CSV giriş dosyasını yükleyin
Bu adım için, GitHub'dan kullanılabilen dosya gibi herhangi bir CSV dosyasını kullanabilirsiniz.

1. Azure portalında, **kaynak** > **Depolama Deposu** > **hesabı**oluştur'u tıklatın.

2. Bir ad`samldemo` sağlayın (örnekte). Ad yalnızca küçük harfleri ve sayıları kullanabilir ve Azure'da benzersiz olmalıdır. 

3. Varolan bir kaynak grubu belirtin ve bir konum belirtin. Konum için, bu öğreticide oluşturulan tüm kaynakların aynı konumu kullanmasını öneririz.

    ![depolama hesabı ayrıntılarını sağlama](./media/stream-analytics-machine-learning-integration-tutorial/create-storage-account1.png)

4. Azure portalında depolama hesabını seçin. Depolama hesabı **çubuğunda, Kapsayıcılar'ı** tıklatın ve ardından blob depolama alanı oluşturmak için ** + &nbsp;Kapsayıcı'yı** tıklatın.

    ![Giriş için blob depolama kabı oluşturma](./media/stream-analytics-machine-learning-integration-tutorial/create-storage-account2.png)

5. Kapsayıcı için bir ad`azuresamldemoblob` (örnekte) sağlayın ve **Access türünün** **Blob**olarak ayarlı olduğunu doğrulayın. İşiniz bittiğinde, **Tamam**’a tıklayın.

    ![blob konteyner ayrıntılarını belirtin](./media/stream-analytics-machine-learning-integration-tutorial/create-storage-account3.png)

6. **Konteynerler** bıçağında, o kap için bıçağı açan yeni kabı seçin.

7. **Karşıya Yükle**'ye tıklayın.

    ![Konteyner için 'Yükle' düğmesi](./media/stream-analytics-machine-learning-integration-tutorial/create-sa-upload-button.png)

8. Upload **blob** bıçak, daha önce indirdiğiniz **sampleinput.csv** dosyasını yükleyin. **Blob türü için,** **Blob'u engelle'yi** seçin ve blok boyutunu bu öğretici için yeterli olan 4 MB'a ayarlayın.

9. Bıçağın altındaki **Yükle** düğmesini tıklatın.

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Cortana İstihbarat Galerisi'nden duygu analizi modelini ekleyin

Örnek veriler bir blob içinde olduğuna göre, Cortana İstihbarat Galerisi'nde duygu analizi modelini etkinleştirebilirsiniz.

1. Cortana İstihbarat Galerisi'ndeki [tahmine dayalı duyarlılık analizi modeli](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) sayfasına gidin.  

2. **Studio'da Aç'ı**tıklatın.  
   
   ![Stream Analytics Machine Learning, açık Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. Çalışma alanına gitmek için oturum açın. Bir konum seçin.

4. Sayfanın altındaki **Çalıştır'ı** tıklatın. İşlem devam ediyor, bu da yaklaşık bir dakika sürüyor.

   ![Machine Learning Studio'da deneme yürütmek](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-run-experiment.png)  

5. İşlem başarıyla çalıştırdıktan sonra, sayfanın altındaki **Web Hizmetini Dağıt'ı** seçin.

   ![bir web hizmeti olarak Machine Learning Studio deneme dağıtmak](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-deploy-web-service.png)  

6. Duygu analizi modelinin kullanıma hazır olduğunu doğrulamak için **Test** düğmesini tıklatın. "Microsoft'u seviyorum" gibi metin girişi sağlayın. 

   ![Machine Learning Studio test deneyi](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test.png)  

    Test işe yarıyorsa, aşağıdaki örneğe benzer bir sonuç görürsünüz:

   ![Machine Learning Studio test sonuçları](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test-results.png)  

7. **Uygulamalar** sütununda, Excel çalışma kitabını indirmek için **Excel 2010 veya önceki çalışma kitabı** bağlantısını tıklatın. Çalışma kitabı, Akış Analizi işini ayarlamak için daha sonra ihtiyacınız olan API anahtarını ve URL'yi içerir.

    ![Stream Analytics Machine Learning, hızlı bakış](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  


## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Machine Learning modelini kullanan bir Akış Analizi işi oluşturun

Artık blob depolamasında CSV dosyasından örnek tweetleri okuyan bir Stream Analytics işi oluşturabilirsiniz. 

### <a name="create-the-job"></a>İşi oluşturma

1. [Azure portalına](https://portal.azure.com)gidin.  

2.  > Bir kaynak**Internet of Things** > **Stream Analytics iş** **oluşturun'u**tıklatın. 

3. İşi `azure-sa-ml-demo`adlandırın, bir abonelik belirtin, varolan bir kaynak grubu belirtin veya yeni bir tane oluşturun ve işin konumunu seçin.

   ![yeni Akış Analizi işi için ayarları belirtin](./media/stream-analytics-machine-learning-integration-tutorial/create-stream-analytics-job-1.png)
   

### <a name="configure-the-job-input"></a>İş girişini yapılandırma
İş, girişini daha önce blob depolamaalanına yüklediğiniz CSV dosyasından alır.

1. İş oluşturulduktan sonra, iş bıçağındaki **İş Topolojisi** altında **Girişler** seçeneğini tıklatın.    

2. **Giriş** bıçağında, **Akış Girişi** >**Blob depolama alanını** tıklatın

3. **Blob Depolama** bıçağını aşağıdaki değerlerle doldurun:

   
   |Alan  |Değer  |
   |---------|---------|
   |**Girdi diğer adı** | Adı `datainput` kullanın ve **aboneliğinizden blob depolama alanını seçin**       |
   |**Depolama hesabı**  |  Daha önce oluşturduğunuz depolama hesabını seçin.  |
   |**Kapsayıcı**  | Daha önce oluşturduğunuz`azuresamldemoblob`kapsayıcıyı seçin ( )        |
   |**Olay serileştirme biçimi**  |  **CSV'yi** seçin       |

   ![Yeni Stream Analytics iş girişi için ayarlar](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-create-sa-input-new-portal.png)

1. **Kaydet**'e tıklayın.

### <a name="configure-the-job-output"></a>İş çıktısını yapılandırma
İş, sonuçları girdi aldığı aynı blob depolama alanına gönderir. 

1. **İş Bıçaklarında İş Topolojisi** altında **Çıktılar** seçeneğini tıklatın.  

2. **Çıktılar** çubuğunda,**Blob depolama ekle'yi** **tıklatın** >ve ardından `datamloutput`takma adla çıktı ekleyin. 

3. **Blob Depolama** bıçağını aşağıdaki değerlerle doldurun:

   |Alan  |Değer  |
   |---------|---------|
   |**Çıktı diğer adı** | Adı `datamloutput` kullanın ve **aboneliğinizden blob depolama alanını seçin**       |
   |**Depolama hesabı**  |  Daha önce oluşturduğunuz depolama hesabını seçin.  |
   |**Kapsayıcı**  | Daha önce oluşturduğunuz`azuresamldemoblob`kapsayıcıyı seçin ( )        |
   |**Olay serileştirme biçimi**  |  **CSV'yi** seçin       |

   ![Yeni Stream Analytics iş çıktısı için ayarlar](./media/stream-analytics-machine-learning-integration-tutorial/create-stream-analytics-output.png) 

4. **Kaydet**'e tıklayın.   


### <a name="add-the-machine-learning-function"></a>Makine Öğrenimi işlevini ekle 
Daha önce bir web hizmeti için bir Machine Learning modeli yayınladı. Bu senaryoda, Akış Analizi işi çalıştığında, girişten her örnek tweet'i duyarlılık analizi için web hizmetine gönderir. Machine Learning web hizmeti bir`positive` `neutral`duygu `negative`(, , veya ) ve tweet olumlu olma olasılığı döndürür. 

Öğreticinin bu bölümünde, Akış Çözümlemesi işinde bir işlev tanımlarsınız. İşlev, web hizmetine bir tweet göndermek ve yanıtı geri almak için çağrılabilir. 

1. Excel çalışma kitabında daha önce indirdiğiniz web hizmeti URL'si ve API anahtarına sahip olduğundan emin olun.

2. İş > **İş** > Bıçaklarınıza gidin +**AzureML** **Ekle** > 

3. **Azure Machine Learning fonksiyon** bıçağını aşağıdaki değerlerle doldurun:

   |Alan  |Değer  |
   |---------|---------|
   | **İşlev takma adı** | Adınızı `sentiment` kullanın ve URL'yi ve anahtarı girme seçeneği sunan **Azure Machine Learning işlev ayarlarını el ile sağlayın'ı** seçin.      |
   | **URL**| Web hizmeti URL'sini yapıştırın.|
   |**Anahtar** | API tuşunu yapıştırın. |
  
   ![Stream Analytics işine Makine Öğrenimi işlevi eklemek için ayarlar](./media/stream-analytics-machine-learning-integration-tutorial/add-machine-learning-function.png)  
    
4. **Kaydet**'e tıklayın.

### <a name="create-a-query-to-transform-the-data"></a>Verileri dönüştürmek için sorgu oluşturma

Akış Analizi, girişi incelemek ve işlemek için bildirimsel, SQL tabanlı bir sorgu kullanır. Bu bölümde, girişten her tweet'i okuyan ve ardından duygu analizi gerçekleştirmek için Machine Learning işlevini çağıran bir sorgu oluşturursunuz. Sorgu daha sonra sonucu tanımladığınız çıktıya (blob depolama) gönderir.

1. İşe genel bakış bıçağına geri dönün.

2.  **İş Topolojisi**altında Sorgu **kutusunu** tıklatın.

3. Aşağıdaki sorguyu girin:

    ```SQL
    WITH sentiment AS (  
    SELECT text, sentiment1(text) as result 
    FROM datainput  
    )  

    SELECT text, result.[Score]  
    INTO datamloutput
    FROM sentiment  
    ```    

    Sorgu, girişteki her tweet'te duyarlılık çözümlemesi gerçekleştirmek için daha önce oluşturduğunuz işlevi (`sentiment`) çağırır. 

4. Sorguyu kaydetmek için **Kaydet**’e tıklayın.


## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Stream Analytics işini başlatıp çıktıyı denetleyin

Artık Stream Analytics işine başlayabilirsiniz.

### <a name="start-the-job"></a>İşi başlatma
1. İşe genel bakış bıçağına geri dönün.

2. Bıçağın üst kısmında **başlat'ı** tıklatın.

3. Başlangıç **işinde,** **Özel'i**seçin ve csv dosyasını blob depolama alanına yüklediğinizden bir gün önce seçin. Bittiğinde, **Başlat'ı**tıklatın.  


### <a name="check-the-output"></a>Çıktıyı kontrol edin
1. **İzleme** kutusunda etkinliği görene kadar işin birkaç dakika çalışmasına izin verin. 

2. Blob depolama alanının içeriğini incelemek için normalde kullandığınız bir aracınız varsa, `azuresamldemoblob` kapsayıcıyı incelemek için bu aracı kullanın. Alternatif olarak, Azure portalında aşağıdaki adımları yapın:

    1. Portalda, `samldemo` depolama hesabını bulun ve hesap içinde `azuresamldemoblob` kapsayıcıyı bulun. Kapsayıcıda iki dosya görürsünüz: örnek tweetleri içeren dosya ve Stream Analytics işi tarafından oluşturulan bir CSV dosyası.
    2. Oluşturulan dosyayı sağ tıklatın ve ardından **İndir'i**seçin. 

   ![Blob depolamadan CSV iş çıktısını indirin](./media/stream-analytics-machine-learning-integration-tutorial/download-output-csv-file.png)  

3. Oluşturulan CSV dosyasını açın. Aşağıdaki örnek gibi bir şey görürsünüz:  
   
   ![Stream Analytics Machine Learning, CSV görünümü](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  


### <a name="view-metrics"></a>Ölçümleri görüntüle
Azure Machine Learning işleviyle ilgili ölçümleri de görüntüleyebilirsiniz. İşle ilgili aşağıdaki ölçümler iş bıçağındaki **İzleme** kutusunda görüntülenir:

* **İşlev İstekleri,** Machine Learning web hizmetine gönderilen istek sayısını gösterir.  
* **İşlev Olayları** istekteki olay sayısını gösterir. Varsayılan olarak, Machine Learning web hizmetine yapılan her istek en fazla 1.000 olay içerir.  


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [REST API ve Makine Öğrenimini Entegre Edin](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)



