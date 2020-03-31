---
title: Quickstart - Visual Studio Code'da Azure Akışı Analizi işi oluşturun
description: Bu hızlı başlangıç, bir Akış Analizi işi oluşturarak, giriş ve çıktıları yapılandırarak ve Visual Studio Code ile bir sorgu tanımlayarak nasıl başlayacağınızı gösterir.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 01/18/2020
ms.topic: quickstart
ms.custom: mvc
ms.openlocfilehash: e3e878b4f548da64ab04eb079d51b0846cf35c57
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76313884"
---
# <a name="quickstart-create-an-azure-stream-analytics-job-in-visual-studio-code-preview"></a>Hızlı başlangıç: Visual Studio Code'da Bir Azure Akışı Analizi işi oluşturun (önizleme)

Bu hızlı başlangıç, Visual Studio Code için Azure Akış Analiz Araçları uzantısını kullanarak bir Azure Akışı Analizi işini nasıl oluşturup çalıştırabileceğinizi gösterir. Örnek iş, bir Azure IoT Hub aygıtından veri akışı okur. 27° üzerindeyken ortalama sıcaklığı hesaplayan ve ortaya çıkan çıktı olaylarını blob depolamasındaki yeni bir dosyaya yazan bir iş tanımlarsınız.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/)oluşturun.

* [Azure portalında](https://portal.azure.com/)oturum açın.

* [Visual Studio Kodunu](https://code.visualstudio.com/)Yükleyin.

## <a name="install-the-azure-stream-analytics-tools-extension"></a>Azure Akış Analiz Araçları uzantısını yükleme

1. Visual Studio Code'u açın.

2. Sol bölmedeki **Uzantılardan** Akış **Analizi'ni** arayın ve **Azure Akışı Analitik Araçları** uzantısında **Yükle'yi** seçin.

3. Uzantı yüklendikten sonra **Azure Akışı Analizi Araçları'nın** Etkin **Uzantılar'da**görünür olduğunu doğrulayın.

   ![Visual Studio Code'da etkin leştirilmiş uzantılar altında Azure Akışı Analiz Araçları](./media/quick-create-vs-code/enabled-extensions.png)

## <a name="activate-the-azure-stream-analytics-tools-extension"></a>Azure Akış Analiz Araçları uzantısını etkinleştirme

1. Visual Studio Code etkinlik çubuğundaki **Azure** simgesini seçin. Yan çubuktaki **Akış Analizi** altında **Azure'da Oturum Aç'ı'nı**seçin.

   ![Visual Studio Code'da Azure'da oturum açma](./media/quick-create-vs-code/azure-sign-in.png)

2. Oturum açKen, Azure hesap adınız Visual Studio Code penceresinin sol alt köşesindeki durum çubuğunda görünür.

> [!NOTE]
> Azure Akış Analiz Araçları uzantısı, oturum açmazsanız bir sonraki seferde sizi otomatik olarak imzalar. Hesabınızda iki faktörlü kimlik doğrulaması varsa, PIN kullanmak yerine telefon kimlik doğrulaması kullanmanızı öneririz.
> Kaynakları listelemeyle ilgili sorunlarınız varsa, oturum açma ve yeniden oturum açma genellikle yardımcı olur. Oturum unu imzalamak için `Azure: Sign Out`komutu girin.

## <a name="prepare-the-input-data"></a>Girdi verilerini hazırlama

Akış Analizi işini tanımlamadan önce, daha sonra iş girişi olarak yapılandırılan verileri hazırlamanız gerekir. İşin gerektirdiği giriş verilerini hazırlamak için aşağıdaki adımları tamamlayın:

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. IoT >  **Hub'ın kaynak** > **Internet'ini**seçin.**IoT Hub**

3. **IoT Hub** bölmesine aşağıdaki bilgileri girin:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Abonelik  | \<Aboneliğiniz\> |  Kullanmak istediğiniz Azure aboneliğini seçin. |
   |Kaynak Grubu   |   asaquickstart-resourcegroup  |   **Yeni Oluştur**’u seçin ve hesabınız için yeni bir kaynak grubu adı girin. |
   |Bölge  |  \<Kullanıcılarınıza en yakın bölgeyi seçin\> | IoT hub'ınızı barındırabileceğiniz bir coğrafi konum seçin. Kullanıcılarınıza en yakın konumu kullanın. |
   |IoT Hub Adı  | MyASAIoTHub  |   IoT hub'ınız için bir ad seçin.   |

   ![IoT hub oluşturma](./media/quick-create-vs-code/create-iot-hub.png)

4. **Sonraki'ni seçin: Boyut ve ölçek ayarlayın.**

5. Fiyatlandırma ve **ölçek katmanı**için seçim yapın. Bu hızlı başlangıç için, aboneliğinizde hala mevcutsa **F1 - Free** katmanını seçin. Boş katman kullanılamıyorsa, kullanılabilir en düşük katmanı seçin. Daha fazla bilgi için [Azure IoT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub/)için bkz.

   ![IoT hub'ınızı boyutlandırın ve ölçeklendirin](./media/quick-create-vs-code/iot-hub-size-and-scale.png)

6. **İncele ve oluştur**’u seçin. IoT hub bilgilerinizi gözden geçirin ve **Oluştur'u**seçin. IoT hub’ınızın oluşturulması birkaç dakika sürebilir. **Bildirimler** bölmesindeki ilerlemeyi izleyebilirsiniz.

7. IoT hub'ınızın gezinti menüsünde, **IoT aygıtlarının**altına **ekle'yi** seçin. **Aygıt Kimliği**için bir kimlik ekleyin ve **Kaydet'i**seçin.

   ![IoT hub'ınıza aygıt ekleme](./media/quick-create-vs-code/add-device-iot-hub.png)

8. Aygıt oluşturulduktan sonra aygıtı **IoT aygıtlar** listesinden açın. **Bağlantı dizesinde (birincil anahtar)** dizekopyalayın ve daha sonra kullanmak üzere bir not defterine kaydedin.

   ![Copy IoT hub cihaz bağlantı dizesi](./media/quick-create-vs-code/save-iot-device-connection-string.png)

## <a name="run-the-iot-simulator"></a>IoT simülatörünü çalıştırın

1. [Raspberry Pi Azure IoT Online Simulator'u](https://azure-samples.github.io/raspberry-pi-web-simulator/) yeni bir tarayıcı sekmesinde veya penceresinde açın.

2. Satır 15'teki yer tutucuyu daha önce kaydettiğiniz IoT hub aygıt bağlantı dizesiyle değiştirin.

3. **Çalıştır**'ı seçin. Çıktı, IoT hub'ınıza gönderilen sensör verilerini ve iletilerini göstermelidir.

   ![Çıktı ile Raspberry Pi Azure IoT Online Simulator](./media/quick-create-vs-code/ras-pi-connection-string.png)

## <a name="create-blob-storage"></a>Blob depolama oluşturma

1. Azure portalının sol üst köşesinden **kaynak** > **Depolama Deposu** > **hesabı**oluştur'u seçin.

2. Depolama **hesabı oluştur** bölmesine bir depolama hesabı adı, konum ve kaynak grubu girin. Oluşturduğunuz IoT hub'ı yla aynı konum ve kaynak grubunu seçin. Ardından hesabı oluşturmak için **Gözden Geçir + oluştur'u** seçin.

   ![Depolama hesabı oluştur](./media/quick-create-vs-code/create-storage-account.png)

3. Depolama hesabınız oluşturulduktan **sonra, Genel Bakış** bölmesindeki **Blobs** döşemesini seçin.

   ![Depolama hesabına genel bakış](./media/quick-create-vs-code/blob-storage.png)

4. **Blob Servis** sayfasından **Kapsayıcı'yı** seçin ve **kapsayıcınız**1 gibi bir ad sağlayın. **Ortak erişim düzeyini** Özel olarak bırakın **(anonim erişim yok)** ve **Tamam'ı**seçin.

   ![Blob kapsayıcısı oluşturma](./media/quick-create-vs-code/create-blob-container.png)

## <a name="create-a-stream-analytics-project"></a>Stream Analytics projesi oluşturma

1. Visual Studio Code'da komut paletini açmak için **Ctrl+Shift+P'yi** seçin. Sonra **ASA** girin ve **ASA seçin: Yeni Proje oluşturun.**

   ![Yeni bir proje oluşturma](./media/quick-create-vs-code/create-new-project.png)

2. **MyASAproj**gibi proje adınızı girin ve projeniz için bir klasör seçin.

    ![Proje adı oluşturma](./media/quick-create-vs-code/create-project-name.png)

3. Yeni proje çalışma alanınıza eklenir. Bir Akış Analizi projesi üç klasörden oluşur: **Girişler,** Çıktılar ve **Fonksiyonlar.** **Functions** Ayrıca sorgu komut dosyası **(*.asaql)**, **Bir JobConfig.json** dosyası ve bir **asaproj.json** yapılandırma dosyası vardır.

    **asaproj.json** yapılandırma dosyası, Akış Analizi işini Azure'a göndermek için gereken girdileri, çıktıları ve iş yapılandırma dosya bilgilerini içerir.

    ![Visual Studio Code'daki Stream Analytics proje dosyaları](./media/quick-create-vs-code/asa-project-files.png)

> [!Note]
> Komut paletinden giriş ve çıktı eklerken, ilgili yollar otomatik olarak **asaproj.json'a** eklenir. Doğrudan diske giriş veya çıkışları ekler veya kaldırırsanız, bunları el ile eklemeniz veya **asaproj.json'dan**kaldırmanız gerekir. Giriş ve çıktıları tek bir yere koymayı ve her **asaproj.json** dosyasındaki yolları belirterek bunları farklı işlerde referans almayı seçebilirsiniz.

## <a name="define-the-transformation-query"></a>Dönüşüm sorgusunu tanımlama

1. Proje klasörünüzden **myASAproj.asaql'ı** açın.

2. Aşağıdaki sorguyu ekleyin:

   ```sql
   SELECT *
   INTO Output
   FROM Input
   HAVING Temperature > 27
   ```

## <a name="test-the-query-locally-with-sample-data"></a>Sorguyu örnek verilerle yerel olarak test edin

Sorguyu bulutta çalıştırmadan önce, sorgu mantığını doğrulamak için sorguyu yerel olarak yerel bir örnek veri dosyasıyla veya canlı girişten yakalanan verilerle sınayabilirsiniz.

Daha fazla ayrıntı için [örnek verilerle test sorgularında](visual-studio-code-local-run.md) yönergeleri yerel olarak izleyin.

 ![Visual Studio Code'da örnek verilerle test](./media/vscode-local-run/localrun-localinput.gif)

## <a name="define-a-live-input"></a>Canlı giriş tanımlama

1. Akış Analizi projenizdeki **Girişler** klasörüne sağ tıklayın. Ardından **ASA:** Bağlam menüsünden Giriş Ekle'yi seçin.

    ![Girişler klasöründen giriş ekleme](./media/quick-create-vs-code/add-input-from-inputs-folder.png)

    Veya komut paletini açmak için **Ctrl+Shift+P'yi** seçin ve **ASA girin: Giriş ekleyin.**

   ![Visual Studio Koduna Akış Analizi girişi ekleme](./media/quick-create-vs-code/add-input.png)

2. Giriş türü için **IoT Hub'ı** seçin.

   ![Giriş seçeneği olarak IoT hub'ını seçin](./media/quick-create-vs-code/iot-hub.png)

3. Komut paletinden girişi eklediyseniz, girişi kullanacak Stream Analytics sorgu komut dosyasını seçin. Otomatik olarak **myASAproj.asaql**için dosya yolu ile doldurulmalıdır.

   ![Visual Studio Code'da bir Akış Analizi komut dosyası seçin](./media/quick-create-vs-code/asa-script.png)

4. Açılan menüden **Azure Aboneliklerinizden Seç'i** seçin.

    ![Aboneliklerden seçim](./media/quick-create-vs-code/add-input-select-subscription.png)

5. Yeni oluşturulan **IoTHub1.json** dosyasını aşağıdaki değerlerle edin. Burada belirtilmeyen alanlar için varsayılan değerleri tutun.

   |Ayar|Önerilen değer|Açıklama|
   |-------|---------------|-----------|
   |Adı|Giriş|İş girişini tanımlamak için bir ad girin.|
   |IotHubNamespace|MyASAIoTHub|IoT hub'ınızın adını seçin veya girin. IoT hub adları, aynı abonelikte oluşturulduklarında otomatik olarak algılanır.|
   |PaylaşılanAccessPolitikasıAdı|iothubowner| |

   CodeLens özelliğini kullanarak bir dize girmenize, açılır listeden seçim yapmaya veya doğrudan dosyadaki metni değiştirmenize yardımcı olabilirsiniz. Aşağıdaki ekran görüntüsü, **Aboneliklerinizden Select'i** örnek olarak gösterir. Kimlik bilgileri otomatik olarak listelenir ve yerel kimlik bilgisi yöneticisine kaydedilir.

   ![Visual Studio Kodu'nda girişi yapılandırma](./media/quick-create-vs-code/configure-input.png)

   ![Visual Studio Kodu'nda giriş değerini yapılandırma](./media/quick-create-vs-code/configure-input-value.png)

## <a name="preview-input"></a>Girişi önizleme

Üst hattan **IoTHub1.json'da** **Verileri Önizleme'yi** seçin. Bazı giriş verileri IoT hub'ından getirilir ve önizleme penceresinde gösterilir. Bu işlem biraz zaman alabilir.

 ![Canlı girişi önizleme](./media/quick-create-vs-code/preview-live-input.png)

## <a name="define-an-output"></a>Çıktı tanımlama

1. Komut paletini açmak için **Ctrl+Shift+P'yi** seçin. Sonra, **ASA girin: Çıktı ekle**.

   ![Visual Studio Koduna Akış Analizi çıktısı ekleme](./media/quick-create-vs-code/add-output.png)

2. Lavabo türü için **Blob Depolama'yı** seçin.

3. Bu girişi kullanacak Stream Analytics sorgu komut dosyasını seçin.

4. Çıktı dosya adını **BlobStorage**olarak girin.

5. Aşağıdaki değerleri kullanarak **BlobStorage'ı** düzenleme. Burada belirtilmeyen alanlar için varsayılan değerleri tutun. Açılan listeden seçim seçmenize veya bir dize girmenize yardımcı olmak için CodeLens özelliğini kullanın.

   |Ayar|Önerilen değer|Açıklama|
   |-------|---------------|-----------|
   |Adı|Çıktı| İş çıktısını tanımlamak için bir ad girin.|
   |Depolama Hesabı|asaquickstartstorage|Depolama hesabınızın adını seçin veya girin. Depolama hesabı adları, aynı abonelikte oluşturulduklarında otomatik olarak algılanır.|
   |Kapsayıcı|kapsayıcı1|Depolama hesabınızda oluşturduğunuz mevcut kapsayıcıyı seçin.|
   |Yol Deseni|çıkış|Kapsayıcıda oluşturulacak dosya yolunun adını girin.|

   ![Visual Studio Code'da çıktıyı yapılandırma](./media/quick-create-vs-code/configure-output.png)

## <a name="compile-the-script"></a>Komut dosyasını derleme

Komut dosyası derlemesi sözdizimini denetler ve otomatik dağıtım için Azure Kaynak Yöneticisi şablonlarını oluşturur.

Komut dosyası derlemesini tetiklemenin iki yolu vardır:

- Çalışma alanından komut dosyasını seçin ve ardından komut paletinden derle.

   ![Komut dosyasını derlemek için Visual Studio Code komut paletini kullanın](./media/quick-create-vs-code/compile-script1.png)

- Komut dosyasına sağ tıklayın ve **ASA'yı seçin: Scripti Derle**.

    ![Derlemek için Stream Analytics komut dosyasına sağ tıklayın](./media/quick-create-vs-code/compile-script2.png)

Derlemeden sonra, oluşturulan iki Azure Kaynak Yöneticisi şablonunu projenizin **Dağıt** klasöründe bulabilirsiniz. Bu iki dosya otomatik dağıtım için kullanılır.

![Dosya Gezgini'nde Akış Analizi dağıtım şablonları](./media/quick-create-vs-code/deployment-templates.png)

## <a name="submit-a-stream-analytics-job-to-azure"></a>Azure'a Akış Analizi işi gönderme

1. Sorgu komut dosyanızın komut dosyası nın komut dosyası penceresinde **Azure'a Gönder'i**seçin.

   ![Komut dosyası düzenleyicisindeki abonelikmetninizden seçim](./media/quick-create-vs-code/submit-job.png)

2. Aboneliğinizi açılan listeden seçin.

3. **İş seç'i**seçin. Ardından **Yeni İş Oluştur'u**seçin.

4. İş adınızı girin, **myASAjob.** Ardından kaynak grubunu ve konumu seçmek için yönergeleri izleyin.

5. **Azure'a Gönder'i**seçin. Çıkış penceresinde günlükleri bulabilirsiniz. 

6. İşiniz oluşturulduğunda, akışı Akış **Analizi Gezgini'nde**görebilirsiniz.

    ![Stream Analytics Explorer'da listelenen iş](./media/quick-create-vs-code/list-job.png)

## <a name="start-the-stream-analytics-job-and-check-output"></a>Stream Analytics işini başlatıp çıktıyı denetleyin

1. Visual Studio Code'da **Stream Analytics Explorer'ı** açın ve işinizi bulun, **myASAJob**.

2. İş adını sağ tıklatın. Ardından, bağlam menüsünden **Başlat'ı** seçin.

   ![Visual Studio Code'da Stream Analytics işini başlatın](./media/quick-create-vs-code/start-asa-job-vs-code.png)

3. İşi başlatmak için açılan pencerede **Şimdi'i** seçin.

4. İş durumunun **Running**olarak değiştiğini unutmayın. Giriş ve çıktı olay ölçümlerini görmek için iş adını sağ tıklatın ve **Portal'da İş Görünümü Aç'ı** seçin. Bu eylem birkaç dakika sürebilir.

5. Sonuçları görüntülemek için, Görsel Stüdyo Kodu uzantısında veya Azure portalında blob depolama alanını açın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, kaynak grubunu, akış işini ve ilgili tüm kaynakları silin. İşi silerken, işin tükettiği akış birimlerinin faturalanması önler. 

İşi gelecekte kullanmayı planlıyorsanız, işi durdurabilir ve daha sonra yeniden başlatabilirsiniz. Bu işi bir daha kullanmayacaksanız, bu hızlı başlangıçta oluşturduğunuz tüm kaynakları silmek için aşağıdaki adımları kullanın:

1. Azure portalındaki sol menüden **Kaynak gruplarını** seçin ve ardından oluşturduğunuz kaynağın adını seçin.  

2. Kaynak grubunuzun sayfasında **Sil**’i seçin. Metin kutusuna silmek için kaynağın adını girin ve sonra **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Visual Studio Code'u kullanarak basit bir Stream Analytics işini dağıttınız. [Azure portalı,](stream-analytics-quick-create-portal.md) [PowerShell](stream-analytics-quick-create-powershell.md)ve [Visual Studio'yu](stream-analytics-quick-create-vs.md)kullanarak Stream Analytics işlerini de dağıtabilirsiniz.

Visual Studio Code için Azure Akış Analiz Araçları hakkında bilgi edinmek için aşağıdaki makalelere devam edin:

* [Visual Studio Code ile Azure Akış Analizi işlerini canlı girdiye karşı yerel olarak test edin](visual-studio-code-local-run-live-input.md)

* [Azure Akış Analizi işlerini görüntülemek için Visual Studio Kodunu kullanın](visual-studio-code-explore-jobs.md)

* [npm paketini kullanarak CI/CD ardışık hatlarını ayarlama](setup-cicd-vs-code.md)
