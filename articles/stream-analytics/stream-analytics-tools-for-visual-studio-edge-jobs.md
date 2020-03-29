---
title: Visual Studio'da Azure Stream Analytics Edge işleri
description: Bu makalede, Visual Studio için Stream Analytics araçlarını kullanarak IoT Edge işlerinde Akış Analizi'nizi nasıl yazılanın, hata ayıklamanız ve oluşturabilirsiniz.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 07f109b3d5539f7cd87a12fb42a36803573c2bdf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75354562"
---
# <a name="develop-stream-analytics-edge-jobs-using-visual-studio-tools"></a>Visual Studio araçlarını kullanarak Stream Analytics Edge işlerini geliştirin

Bu eğitimde, Visual Studio için Stream Analytics araçlarını nasıl kullanacağınızı öğreneceksiniz. Akış Analitiği Kenarı işlerinizi nasıl yazabileceğinizi, hata ayıklamanızı ve oluşturabileceğinizi öğrenirsiniz. İşi oluşturup test ettikten sonra, aygıtlarınıza dağıtmak için Azure portalına gidebilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşullara ihtiyacınız vardır:

* Install [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/), [Visual Studio 2015](https://www.visualstudio.com/vs/older-downloads/), veya Visual [Studio 2013 Güncelleme 4](https://www.microsoft.com/download/details.aspx?id=45326). Enterprise (Ultimate/Premium), Professional ve Community sürümleri desteklenir. Express sürümü desteklenmez.  

* Visual Studio için Stream Analytics araçlarını yüklemek için [yükleme yönergelerini](stream-analytics-tools-for-visual-studio-edge-jobs.md) izleyin.
 
## <a name="create-a-stream-analytics-edge-project"></a>Akış Analizi Edge projesi oluşturun 

Visual Studio'dan **Dosya** > **Yeni** > **Projesi'ni**seçin. **Azure Akış Analizi** > **Akış Analizi Edge** > **Azure Akışı Analitik Edge Uygulamasını**genişletmek > soldaki **Şablonlar** listesine gidin. Projeniz için bir Ad, Konum ve Çözüm adı sağlayın ve **Tamam'ı**seçin.

![Visual Studio'da Yeni Stream Analytics Edge projesi](./media/stream-analytics-tools-for-visual-studio-edge-jobs/new-stream-analytics-edge-project.png)

Proje oluşturulduktan sonra klasör hiyerarşisini görüntülemek için **Çözüm Gezgini'ne** gidin.

![Stream Analytics Edge işinin çözüm kaşifi görünümü](./media/stream-analytics-tools-for-visual-studio-edge-jobs/edge-project-in-solution-explorer.png)

 
## <a name="choose-the-correct-subscription"></a>Doğru aboneliği seçin

1. Visual Studio **View** menüsünden **Server Explorer'ı**seçin.  

2. **Azure** > Microsoft **Azure Aboneliği >'ne Bağlan'ı** seçin ve ardından Azure hesabınızla oturum açın' a sağ tıklayın.

## <a name="define-inputs"></a>Girişleri tanımlama

1. Çözüm **Gezgini'nden,** **Giriş düğümlerini** genişleterek **EdgeInput.json**adlı bir giriş görmeniz gerekir. Ayarlarını görüntülemek için çift tıklatın.  

2. Kaynak Türünü **Veri Akışına**Ayarlama. Ardından Kaynak'ı Edge Hub'ına, Olay Serileştirme Biçimini **Json'a**ve KODLAMAyı **UTF8'e**ayarlayın. **Edge Hub** İsteğe bağlı olarak, **Giriş Diğer Adını**yeniden adlandırabilirsiniz, bu örnekte olduğu gibi bırakalım. Giriş takma adını yeniden adlandırmanız durumunda, sorguyu tanımlarken belirttiğiniz adı kullanın. Ayarları kaydetmek için **Kaydet**’i seçin.  
   ![Stream Analytics iş giriş yapılandırması](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-input-configuration.png)
 


## <a name="define-outputs"></a>Çıktıları tanımlama

1. Çözüm **Gezgini'** nden, **Outputs** düğümlerini genişleterek **EdgeOutput.json**adlı bir çıktı görmeniz gerekir. Ayarlarını görüntülemek için çift tıklatın.  

2. **Kenar Hub'ı**seçmek için Lavabo'u ayarladığımdan emin olun, Olay Serileştirme Biçimini **Json'a**ayarlayın, Kodlamayı **UTF8'e**ayarlayın ve Biçim **Dizisini**ayarlayın. İsteğe bağlı olarak, **Çıktı Diğer Adını**yeniden adlandırabilirsiniz, bu örnekte olduğu gibi bırakalım. Çıktı diğer adını yeniden adlandırmanız durumunda, sorguyu tanımlarken belirttiğiniz adı kullanın. Ayarları kaydetmek için **Kaydet**’i seçin. 
   ![Stream Analytics iş çıktısı yapılandırması](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-output-configuration.png)
 
## <a name="define-the-transformation-query"></a>Dönüşüm sorgusunu tanımlama

Stream Analytics IoT Edge ortamlarında dağıtılan Akış Analizi [işleri, Akış Analitiği Sorgu Dili referansının](https://msdn.microsoft.com/azure/stream-analytics/reference/stream-analytics-query-language-reference?f=255&MSPPError=-2147217396)çoğunu destekler. Ancak, Stream Analytics Edge işleri için aşağıdaki işlemler henüz desteklenmemektedir: 


|**Kategori**  | **Komut**  |
|---------|---------|
|Diğer operatörler | <ul><li>BÖLÜM E GÖRE</li><li>ZAMAN DAMGASı ÜZERINDE</li><li>JavaScript UDF</li><li>Kullanıcı tanımlı agregalar (UDA)</li><li>GetMetadataPropertyValue</li><li>Tek bir adımda 14'ten fazla toplam kullanma</li></ul>   |

Portalda bir Stream Analytics Edge işi oluşturduğunuzda, derleyici desteklenen bir operatör kullanmıyorsanız sizi otomatik olarak uyarır.

Visual Studio'nuzdan, sorgu düzenleyicisinde aşağıdaki dönüşüm sorgusunu **(script.asaql dosyası)** tanımlayın

```sql
SELECT * INTO EdgeOutput
FROM EdgeInput 
```

## <a name="test-the-job-locally"></a>İşi yerel olarak test edin

Sorguyu yerel olarak sınamak için örnek verileri yüklemeniz gerekir. Kayıt verilerini [GitHub deposundan](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/Registration.json) indirerek örnek verileri alabilir ve yerel bilgisayarınıza kaydedebilirsiniz. 

1. Örnek verileri yüklemek için **EdgeInput.json** dosyasına sağ tıklayın ve **Yerel Giriş Ekle'yi** seçin  

2. Açılan pencerede > Yerel yolunuzdaki örnek verilere **göz atın** > **Kaydet'i**seçin.
   ![Visual Studio'da yerel giriş yapılandırması](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-local-input-configuration.png)
 
3. **local_EdgeInput.json** adlı bir dosya giriş klasörünüze otomatik olarak eklenir.  
4. Yerel olarak çalıştırabilir veya Azure'a gönderebilirsiniz. Sorguyu sınamak için **Yerel Olarak Çalıştır'ı**seçin.  
   ![Visual Studio'da Stream Analytics iş çalıştırma seçenekleri](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-visual-stuidio-run-options.png)
 
5. Komut istemi penceresi işin durumunu gösterir. İş başarılı bir şekilde çalıştığında, proje klasörü yolunuz "Visual Studio 2015\Projects\MyASAEdgejob\MyASAEdgejob\ASALocalRun\2018-02-23-11-31-42" adlı proje klasörü yolunda "2018-02-23-23-11-42" gibi görünen bir klasör oluşturur. Sonuçları yerel klasörde görüntülemek için klasör yoluna gidin:

   Ayrıca Azure portalında oturum açabilir ve işin oluşturulduğunu doğrulayabilirsiniz. 

   ![Akış Analizi iş sonuç klasörü](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-job-result-folder.png)

## <a name="submit-the-job-to-azure"></a>İşi Azure'a gönderme

1. İşi Azure'a göndermeden önce Azure Aboneliğinize bağlanmanız gerekir. **Sunucu Gezgini'ni** aç >**Azure Aboneliği'ne** **Azure** > Connect'e sağ tıklayın > Azure aboneliğinizde oturum açın.  

2. İşi Azure'a göndermek için sorgu düzenleyicisine gidin > **Azure'a Gönder'i**seçin.  

3. Açılır pencere açılır. Varolan bir Akış Analitiği Kenarı işini güncelleştirmeyi veya yeni bir iş oluşturmayı seçin. Varolan bir işi güncelleştirdiğinizde, tüm iş yapılandırmasının yerini alır, bu senaryoda yeni bir iş yayımlarsınız. **MyASAEdgeJob** gibi bir iş için bir ad girin > **Yeni Bir Azure Akışı Analizi İşi Oluştur'u** seçin > gerekli **Abonelik,** **Kaynak Grubu**ve **Konum** > Seçin **Gönder'i**seçin.

   ![Visual Studio'dan Akış Analizi işini Azure'a gönderin](./media/stream-analytics-tools-for-visual-studio-edge-jobs/submit-stream-analytics-job-to-azure.png)
 
   Şimdi Stream Analytics Edge işiniz oluşturuldu. Aygıtlarınıza nasıl dağıtılamayı öğrenmek için [IoT Edge öğreticisindeki Çalıştır işlerine](stream-analytics-edge.md) başvurabilirsiniz. 

## <a name="manage-the-job"></a>İşi yönetme 

İş durumunu ve iş diyagramını Sunucu Gezgini'nden görüntüleyebilirsiniz. **Sunucu Gezgini'ndeki** **Stream Analytics'ten** aboneliği ve Stream Analytics Edge işini dağıttığınız kaynak grubunu genişletin. **Oluşturulan**durumla MyASAEdgejob'u görüntüleyebilirsiniz. İş düğümünüzü genişletin ve iş görünümünü açmak için üzerine çift tıklayın.

![Sunucu gezgini iş yönetimi seçenekleri](./media/stream-analytics-tools-for-visual-studio-edge-jobs/server-explorer-options.png)
 
İş görünümü penceresi, işi yenileme, işi silme ve işi Azure portalından açma gibi işlemler sağlar.

![Visual Studio'da iş diyagramı ve diğer seçenekler](./media/stream-analytics-tools-for-visual-studio-edge-jobs/job-diagram-and-other-options.png) 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure IoT Edge hakkında daha fazla bilgi](../iot-edge/about-iot-edge.md)
* [ASA Üzerinde IoT Edge öğretici](../iot-edge/tutorial-deploy-stream-analytics.md)
* [Bu anketi kullanarak takıma geri bildirim gönderme](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) 
