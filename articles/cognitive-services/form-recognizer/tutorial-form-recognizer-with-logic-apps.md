---
title: "Öğretici: Faturaları analiz etmek için Azure Logic Apps ile Form Tanıyıcı'yı kullanın - Form Tanıyın"
titleSuffix: Azure Cognitive Services
description: Bu eğitimde, bir modeli eğitim ve örnek verileri kullanarak test etme işlemini otomatikleştiren bir iş akışı oluşturmak için Azure Logic Apps ile Form Tanıyır'ı kullanırsınız.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: tutorial
ms.date: 01/27/2020
ms.author: nitinme
ms.openlocfilehash: d71d9c7e6570e562fe4c692ede1d07b70c923cb6
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77118273"
---
# <a name="tutorial-use-form-recognizer-with-azure-logic-apps-to-analyze-invoices"></a>Öğretici: Faturaları analiz etmek için Azure Logic Apps ile Form Tanıyıcı'yı kullanın

Bu eğitimde, Azure Mantıksal Apps'ta, faturalardan veri ayıklamak için Azure Bilişsel Hizmetler paketinin bir parçası olan Form Ayırıcı'yı kullanan bir iş akışı oluşturursunuz. Önce örnek bir veri kümesini kullanarak bir Form Tanıyıcı modelini eğitin ve ardından modeli başka bir veri kümesinde sınarsınız.

Bu öğreticinin kapsadığı şey şu:

> [!div class="checklist"]
> * Form Tanıyıcı için erişim isteği
> * Azure Depolama blob kapsayıcısı oluşturma
> * Azure blob kapsayıcısına örnek verileri yükleme
> * Azure Mantık Uygulaması Oluşturma
> * Form Tanıyıcı kaynağını kullanacak şekilde mantık uygulamasını yapılandırma
> * Mantık uygulamasını çalıştırarak iş akışını test edin

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun.](https://azure.microsoft.com/free/)

## <a name="understand-the-invoice-to-be-analyzed"></a>Analiz edilecek faturayı anlama

Modeli eğitmek ve test etmek için kullanacağınız örnek veri kümesi [GitHub'dan](https://go.microsoft.com/fwlink/?linkid=2090451).zip dosyası olarak kullanılabilir. .zip dosyasını indirin ve ayıklayın ve **/Train** klasörünün altında bir fatura PDF dosyasını açın. Fatura numarası, fatura tarihi ve benzeri bir tablo ya da tablo olduğuna dikkat edin. 

> [!div class="mx-imgBorder"]
> ![Örnek fatura](media/tutorial-form-recognizer-with-logic-apps/sample-receipt.png)

Bu eğitimde, bu gibi tablolardaki bilgileri JSON biçimine çıkarmak için Azure Logic Apps iş akışını nasıl kullanacağınızı öğreneceksiniz.

## <a name="create-an-azure-storage-blob-container"></a>Azure Depolama blob kapsayıcısı oluşturma

Modeli eğitmek için gereken örnek verileri yüklemek için bu kapsayıcıyı kullanırsınız.

1. Depolama hesabı oluşturmak için [Azure Depolama hesabı oluştur'daki](../../storage/common/storage-account-create.md) yönergeleri izleyin. Depolama hesabı adı olarak **formrecostorage'ı** kullanın.
1. Azure Depolama hesabı içinde bir kapsayıcı oluşturmak için [Bir Azure blob kapsayıcısı oluştur'daki](../../storage/blobs/storage-quickstart-blobs-portal.md) yönergeleri izleyin. Konteyner adı olarak **formrecocontainer** kullanın. Ortak erişim düzeyini Kapsayıcı'ya ayarladığınızdan emin olun **(kapsayıcılar ve lekeler için anonim okuma erişimi)**.

    > [!div class="mx-imgBorder"]
    > ![Blob kapsayıcısı oluşturma](media/tutorial-form-recognizer-with-logic-apps/create-blob-container.png)

## <a name="upload-sample-data-to-the-azure-blob-container"></a>Azure blob kapsayıcısına örnek verileri yükleme

GitHub'da bulunan örnek verileri [indirin.](https://go.microsoft.com/fwlink/?linkid=2090451) Verileri yerel bir klasöre ayıklayın ve **/Train** klasörünün içeriğini daha önce oluşturduğunuz **formrecocontainer'a** yükleyin. Bir kapsayıcıya veri yüklemek için [bir blok blob yükle](../../storage/blobs/storage-quickstart-blobs-portal.md#upload-a-block-blob) yönergeleri izleyin.

Kapsayıcının URL'sini kopyalayın. Bu URL'ye daha sonra öğreticide ihtiyacınız olacak. Depolama hesabını ve kapsayıcıyı bu öğreticide listelenen adlarla oluşturduysanız, URL *https\/olacaktır: /formrecostorage.blob.core.windows.net/formrecocontainer/*.

## <a name="create-a-form-recognizer-resource"></a>Form Tanıyıcı kaynağı oluşturma

[!INCLUDE [create resource](./includes/create-resource.md)]

## <a name="create-your-logic-app"></a>Mantıksal uygulamanızı oluşturma

Görevleri ve iş akışlarını otomatikleştirmek ve düzenlemek için Azure Logic Apps'ı kullanabilirsiniz. Bu öğreticide, e-posta eki olarak çözümlemek istediğiniz bir fatura alarak tetiklenen bir mantık uygulaması oluşturursunuz. Bu iş akışında, aşağıdaki görevleri gerçekleştirin:
* Fatura ekli bir e-posta aldığınızda mantık uygulamasını otomatik olarak tetikleecek şekilde yapılandırın.
* Azure blob depolamasına yüklediğiniz örnek verileri kullanarak bir modeli eğitmek için bir Form Recognizer **Train Model** işlemi kullanacak şekilde mantık uygulamasını yapılandırın.
* Zaten eğittiğiniz modeli kullanmak için form tanıma **çözümformu** işlemini kullanacak şekilde mantık uygulamasını yapılandırın. Bu bileşen, bu mantık uygulamasına sağladığınız faturayı, daha önce eğittiği modele göre analiz eder.

İş akışınızı ayarlamak için aşağıdaki adımları izleyin.

1. Ana Azure menüsünden **kaynak** > **Tümleştirme** > **Mantığı Uygulaması**oluştur'u seçin.

1. **Mantıksal uygulama oluştur** bölümünde, mantıksal uygulamanızın ayrıntılarını burada gösterildiği gibi sağlayın. İşin bittikten sonra **Oluştur'u**seçin.

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | **Adı** | <*mantık-uygulama adı*> | Yalnızca harfler, sayılar,`-`tireler ( ), alt çizer (`_`), parantez`(`( `)`, ),`.`ve dönemleri içerebilen mantık uygulama adınız ( ). Bu örnekte "My-First-Logic-App" kullan›l›r. |
   | **Abonelik** | <*Azure abonelik adı*> | Azure abonelik adınız |
   | **Kaynak grubu** | <*Azure-kaynak grubu adı*> | İlgili kaynakları düzenlemek için kullanılan [Azure kaynak grubunun](./../../azure-resource-manager/management/overview.md) adı. Bu örnekte "My-First-LA-RG" kullanışıktır. |
   | **Konum** | <*Azure bölgesi*> | Mantık uygulama bilgilerinizi depolayabilmek için bölge. Bu örnekte "Batı ABD" kullanılıyor. |
   | **Log Analytics** | Kapalı | Tanılama günlüğüne kaydetme ayarını **Kapalı** durumda bırakın. |
   ||||

1. Azure uygulamanızı dağıttıktan sonra, Azure araç çubuğunda, dağıtılan mantık uygulamanız için **Bildirimler** > **Git'i seçin.** Veya, arama kutusuna adını yazarak mantık uygulamanızı bulabilir ve seçebilirsiniz.

   Logic Apps Tasarımcısı açılır ve bir tanıtım videosu ile sık kullanılan tetikleyicilerin bulunduğu bir sayfa görüntülenir. **Şablonlar** altında **Boş Mantıksal Uygulama**'yı seçin.

   > [!div class="mx-imgBorder"]
   > ![Mantık uygulaması için boş şablon seçin](./../../logic-apps/media/quickstart-create-first-logic-app-workflow/choose-logic-app-template.png)

### <a name="configure-the-logic-app-to-trigger-the-workflow-when-an-email-arrives"></a>Bir e-posta geldiğinde iş akışını tetiklemek için mantık uygulamasını yapılandırın

Bu öğreticide, ekli bir faturayla bir e-posta geldiğinde iş akışını tetiklersiniz. Bu öğretici, e-posta hizmeti olarak Office 365'i kullanır, ancak kullanmak istediğiniz diğer e-posta sağlayıcılarını kullanabilirsiniz.

1. Sekmelerden Tümü'nü seçin, **Office 365 Outlook'u**seçin ve ardından **Tetikleyiciler'in**altında **yeni bir e-posta geldiğinde**seçin.

    ![Gelen bir e-posta aracılığıyla mantık uygulamasını tetikleme](media/tutorial-form-recognizer-with-logic-apps/logic-app-email-trigger.png)

1. Office **365 Outlook** kutusunda **Oturum Aç'ı**tıklatın ve Office 365 hesabına giriş yapmak için ayrıntıları girin.

1. Sonraki iletişim kutusunda aşağıdaki adımları gerçekleştirin.
    1. Yeni e-postalar için izlenmesi gereken klasörü seçin.
    1. **Has ekleri için** **Evet'i**seçin. Bu, iş akışını yalnızca ekleri olan e-postaların tetiklemesini sağlar.
    1. **Ekler ekle**için **Evet'i**seçin. Bu, ekin içeriğinin akış aşağı işlemede kullanılmasını sağlar.

        > [!div class="mx-imgBorder"]
        > ![Mantık uygulaması e-posta tetikleyicisi yapılandırma](media/tutorial-form-recognizer-with-logic-apps/logic-app-specify-email-folder.png)

1. Üstteki araç çubuğundan **Kaydet'i** tıklatın.

### <a name="configure-the-logic-app-to-use-form-recognizer-train-model-operation"></a>Form Recognizer Train Model işlemini kullanacak mantık uygulamasını yapılandırın

Faturaları çözümlemek için Form Tanıma hizmetini kullanabilmeniz için, modelin analiz edebileceği ve öğrenebileceği bazı örnek fatura verileri sağlayarak bir model yetiştirmeniz gerekir.

1. **Yeni adım**seçin ve altında bir **eylem seçin**, **Form Tanıyın**için arama . Görünen sonuçlardan Form **Tanıyıcı'yı**seçin ve ardından Form Tanıyıcı için kullanılabilen eylemlerin altında **Tren Modeli'ni**seçin.

    > [!div class="mx-imgBorder"]
    > ![Form Tanıyıcı Modelini Eğitin](media/tutorial-form-recognizer-with-logic-apps/logic-app-form-reco-train-model.png)

1. Form Recognizer iletişim kutusunda, bağlantı için bir ad girin ve bitiş noktası URL'sini ve Form Tanıyıcısı kaynağı için aldığınız anahtarı girin.

    > [!div class="mx-imgBorder"]
    > ![Form Tanıyıcı için bağlantı adı](media/tutorial-form-recognizer-with-logic-apps/logic-app-form-reco-create-connection.png)

    **Oluştur'u**tıklatın.

1. Kaynak **için** **Tren Modeli** iletişim kutusuna, örnek verileri yüklediğiniz kapsayıcının URL'sini girin.

    > [!div class="mx-imgBorder"]
    > ![Örnek faturalar için depolama kapsayıcısı](media/tutorial-form-recognizer-with-logic-apps/source-for-train-model.png)

1. Üstteki araç çubuğundan **Kaydet'i** tıklatın.

### <a name="configure-the-logic-app-to-use-the-form-recognizer-analyze-form-operation"></a>Form Recognizer Analyze Form işlemini kullanacak mantık uygulamasını yapılandırın

Bu bölümde, İş akışına **Çözümformu** işlemini eklersiniz. Bu işlem, mantık uygulamasına sağlanan yeni bir faturayı çözümlemek için zaten eğitilmiş modeli kullanır.

1. **Yeni adım**seçin ve altında bir **eylem seçin**, **Form Tanıyın**için arama . Görünen sonuçlardan Form **Tanıyıcı'yı**seçin ve ardından Form Tanıyıcı için kullanılabilen eylemlerin altında **Formu Analiz Et'i**seçin.

    > [!div class="mx-imgBorder"]
    > ![Form Tanıyıcı Modelini Çözümle](media/tutorial-form-recognizer-with-logic-apps/logic-app-form-reco-analyze-model.png)

1. Formu **Çözümle** iletişim kutusunda aşağıdaki adımları yapın:

    1. Model **Kimliği** metin kutusunu tıklatın ve açılan iletişim kutusunda Dinamik **İçerik** sekmesialtında **modelId'i**seçin. Bunu yaparak, akış uygulamasını son bölümde eğittiğiniz modelin model kimliğiyle birlikte sağlarsınız.

        > [!div class="mx-imgBorder"]
        > ![Form Tanıyıcı için ModelID'yi kullanma](media/tutorial-form-recognizer-with-logic-apps/analyze-form-model-id.png)

    2. **Belge** metin kutusunu tıklatın ve açılan iletişim kutusunda Dinamik **İçerik** sekmesinin altında **Ekler İçeriği'ni**seçin. Bu, akışı, iş akışını tetikleyen e-postaya eklenen örnek fatura dosyasını kullanacak şekilde yapılandırır.

        > [!div class="mx-imgBorder"]
        > ![Faturaları analiz etmek için e-posta eki kullanma](media/tutorial-form-recognizer-with-logic-apps/analyze-form-input-data.png)

1. Üstteki araç çubuğundan **Kaydet'i** tıklatın.

### <a name="extract-the-table-information-from-the-invoice"></a>Tablo bilgilerini faturadan ayıklama

Bu bölümde, faturalar içindeki tablodaki bilgileri ayıklamak için mantık uygulamasını yapılandırırsınız.

1. **Eylem Ekle'yi**seçin ve **eylem seçin,** Bir Eylemi Seçin, **Oluştur'u** arayın ve kullanılabilen eylemlerin altında yeniden **Oluştur'u** seçin.
    ![Tablo bilgilerini faturadan ayıklama](media/tutorial-form-recognizer-with-logic-apps/extract-table.png)

1. **Oluştur** iletişim kutusunda, **Girişler** metin kutusunu tıklatın ve açılan iletişim kutusundan **tabloları**seçin.

    > [!div class="mx-imgBorder"]
    > ![Tablo bilgilerini faturadan ayıklama](media/tutorial-form-recognizer-with-logic-apps/select-tables.png)

1. **Kaydet**'e tıklayın.

## <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

Mantık uygulamasını test etmek için, [GitHub'dan](https://go.microsoft.com/fwlink/?linkid=2090451)indirdiğiniz örnek veri kümesinin **/Test** klasöründeki örnek faturaları kullanın. Şu adımları uygulayın:

1. Uygulamanız için Azure Logic Apps tasarımcısından, en üstteki araç çubuğundan **Çalıştır'ı** seçin. İş akışı artık etkin ve fatura ekli bir e-posta almak için bekler.
1. Mantık uygulamasını oluştururken sağladığınız e-posta adresine ekli örnek bir fatura içeren bir e-posta gönderin. Mantık uygulamasını yapılandırırken e-postanın sağladığınız klasöre teslim olduğundan emin olun.
1. E-posta klasöre teslim edilir gelmez, Logic Apps Tasarımcısı her aşamanın ilerlemesini içeren bir ekran gösterir. Aşağıdaki ekran görüntüsünde, ek içeren bir e-postanın alındığını ve iş akışının devam ettiğini görürsünüz.

    > [!div class="mx-imgBorder"]
    > ![E-posta göndererek iş akışını başlatma](media/tutorial-form-recognizer-with-logic-apps/logic-apps-email-arrived-progress.png)

1. İş akışının tüm aşamaları tamamlandıktan sonra, Logic Apps Designer her aşamaya karşı yeşil bir onay kutusu gösterir. Tasarımcı penceresinde, **her 2 için**'yi seçin ve ardından **Oluştur'u**seçin.

    > [!div class="mx-imgBorder"]
    > ![İş akışı tamamlandı](media/tutorial-form-recognizer-with-logic-apps/logic-apps-verify-output.png)

    **ÇıKTıLAR** kutusundan çıktıyı kopyalayın ve herhangi bir metin düzenleyicisine yapıştırın.

1. JSON çıktısını e-postada ek olarak gönderdiğiniz örnek faturayla karşılaştırın. JSON verilerinin fatura içindeki tablodaki verilere karşılık geldiğini doğrulayın.

    ```json
    [
      {
        "id": "table_0",
        "columns": [
          {
            "header": [
              {
                "text": "Invoice Number",
                "boundingBox": [
                  38.5,
                  585.2,
                  113.4,
                  585.2,
                  113.4,
                  575.8,
                  38.5,
                  575.8
                ]
              }
            ],
            "entries": [
              [
                {
                  "text": "7689302",
                  "boundingBox": [
                    38.5,
                    549.8,
                    77.3,
                    549.8,
                    77.3,
                    536.2,
                    38.5,
                    536.2
                  ],
                  "confidence": 1
                }
              ]
            ]
          },
          {
            "header": [
              {
                "text": "Invoice Date",
                "boundingBox": [
                  139.7,
                  585.2,
                  198.5,
                  585.2,
                  198.5,
                  575.8,
                  139.7,
                  575.8
                ]
              }
            ],
            "entries": [
              [
                {
                  "text": "3/09/2015",
                  "boundingBox": [
                    139.7,
                    548.1,
                    184,
                    548.1,
                    184,
                    536.2,
                    139.7,
                    536.2
                  ],
                  "confidence": 1
                }
              ]
            ]
          },
          {
            "header": [
              {
                "text": "Invoice Due Date",
                "boundingBox": [
                  240.5,
                  585.2,
                  321,
                  585.2,
                  321,
                  575.8,
                  240.5,
                  575.8
                ]
              }
            ],
            "entries": [
              [
                {
                  "text": "6/29/2016",
                  "boundingBox": [
                    240.5,
                    549,
                    284.8,
                    549,
                    284.8,
                    536.2,
                    240.5,
                    536.2
                  ],
                  "confidence": 1
                }
              ]
            ]
          },
          {
            "header": [
              {
                "text": "Charges",
                "boundingBox": [
                  341.3,
                  585.2,
                  381.2,
                  585.2,
                  381.2,
                  575.8,
                  341.3,
                  575.8
                ]
              }
            ],
            "entries": [
              [
                {
                  "text": "$22,123.24",
                  "boundingBox": [
                    380.6,
                    548.5,
                    430.5,
                    548.5,
                    430.5,
                    536.2,
                    380.6,
                    536.2
                  ],
                  "confidence": 1
                }
              ]
            ]
          },
          {
            "header": [
              {
                "text": "VAT ID",
                "boundingBox": [
                  442.1,
                  590,
                  474.8,
                  590,
                  474.8,
                  575.8,
                  442.1,
                  575.8
                ]
              }
            ],
            "entries": [
              [
                {
                  "text": "QR",
                  "boundingBox": [
                    447.7,
                    549.8,
                    462.6,
                    549.8,
                    462.6,
                    536.2,
                    447.7,
                    536.2
                  ],
                  "confidence": 1
                }
              ]
            ]
          }
        ]
      }
    ]
    ```
    Bu öğreticiyi başarıyla tamamladınız!

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir modeli eğitmek ve faturanın içeriğini ayıklamak için Form Tanıyıcı'yı kullanmak üzere bir Azure Logic Apps iş akışı ayarlayın. Ardından, kendi formlarınızla benzer bir senaryo oluşturabilmek için bir eğitim veri kümesi oluşturmayı öğrenin.

> [!div class="nextstepaction"]
> [Eğitim verileri kümesi oluşturma](build-training-data-set.md)