---
title: İş ortaklarından sensör verileri alma
description: Bu makalede, ortaklardan sensör verilerinin nasıl alınılabildiğini açıklanmaktadır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 113ab07af8ada16c0779da510c5f5b1f1f5a290b
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398228"
---
# <a name="get-sensor-data-from-sensor-partners"></a>Sensör ortaklarından sensör verileri alma

Azure FarmBeats, IoT aygıtlarınızdan ve sensörlerinizden gelen akış verilerini Datahub'a getirmenize yardımcı olur. Şu anda, aşağıdaki sensör cihaz ortakları desteklenir.

  ![FarmBeats ortakları](./media/get-sensor-data-from-sensor-partner/partner-information-2.png)

Aygıt verilerini Azure FarmBeats ile tümleştirmek, çiftliğinizde dağıtılan IoT sensörlerinden yer verilerini veri merkezine ulaştırmanıza yardımcı olur. Veriler, kullanılabilir hale geldikten sonra FarmBeats hızlandırıcısı aracılığıyla görselleştirilebilir. Veriler FarmBeats kullanılarak veri füzyonu ve makine öğrenimi/yapay zeka (ML/AI) modeli yapımında kullanılabilir.

Sensör veri akışını başlatmak için aşağıdakileri sağlayın:

-  FarmBeat'leri Azure Marketi'nde yükledin.
-  Çiftliğinize yüklemek istediğiniz sensörlere ve cihazlara karar verdiniz.
-  Toprak nem sensörleri kullanarak planlıyorsanız, sensörlerin sayısı ve tam olarak nereye yerleştirmelisiniz bir tavsiye almak için FarmBeats Toprak Nem Sensörü Yerleştirme haritasıkullanın. Daha fazla bilgi için [bkz.](generate-maps-in-azure-farmbeats.md)
- Çiftliğinizdeki cihaz ortağınızdan aygıtlar veya sensörler satın alır ve dağıtAbilirsiniz. Sensör verilerine aygıt ortaklarınızın çözümü yle erişebildiğinizden emin olun.

## <a name="enable-device-integration-with-farmbeats"></a>FarmBeats ile cihaz tümleştirmesini etkinleştirme

Sensör verilerinin akışını başlattıktan sonra, farmbeats sisteminize veri alma işlemine başlayabilirsiniz. FarmBeats'e tümleştirmeyi etkinleştirmek için aygıt sağlayıcınıza aşağıdaki bilgileri sağlayın:

 - API uç noktası
 - Kiracı Kimliği
 - İstemci Kimliği
 - Gizli anahtar
 - EventHub bağlantı dizesi

Yukarıdaki bilgileri oluşturmak için aşağıdaki adımları izleyin:

> [!NOTE]
> FarmBeats'in dağıtıldığı Azure aboneliğine erişmek için bu adımların Azure'da tamamlanması gerekir.

1. https://portal.azure.com/ adresinde oturum açın.

2. **FarmBeats sürüm 1.2.7 veya sonraki sürümdeyseniz, a, b ve c adımlarını atlayın ve adım 3'e gidin.** FarmBeats UI'nin sağ üst köşesindeki **Ayarlar** simgesini seçerek FarmBeats sürümünü kontrol edebilirsiniz.

      a.  Azure **Active Directory** > **Uygulama Kayıtları'na** gidin

      b. FarmBeats dağıtımınızın bir parçası olarak oluşturulan **Uygulama Kaydı'nı** seçin. FarmBeats datahub'ınızla aynı ada sahip olacaktır.

      c. **Bir API'yi ortaya >** seçin bir istemci uygulaması **ekleyin** ve **04b07795-8ddb-461a-bbee-02f9e1bf7b46** girin ve Yetki Kapsamı'nı kontrol **edin.** Bu, aşağıdaki adımları gerçekleştirmek için Azure CLI'ye (Bulut Kabuğu) erişim sağlayacaktır:

3. Cloud Shell'i açın. Bu seçenek, Azure portalının sağ üst köşesindeki araç çubuğunda kullanılabilir.

    ![Azure portal araç çubuğu](./media/get-drone-imagery-from-drone-partner/navigation-bar-1.png)

4. Ortamın **PowerShell**olarak ayarlandığından emin olun. Varsayılan olarak, Bash olarak ayarlanır.

    ![PowerShell araç çubuğu ayarı](./media/get-sensor-data-from-sensor-partner/power-shell-new-1.png)

5. Ev dizine git.

    ```azurepowershell-interactive 
    cd  
    ```

6. Şu komutu çalıştırın. Bu, ev dizininize bir komut dosyası indirir.

    ```azurepowershell-interactive 

    wget –q https://aka.ms/farmbeatspartnerscriptv3 -O ./generatePartnerCredentials.ps1

    ```

7. Aşağıdaki komut dosyasını çalıştırın. Komut dosyası, **Azure Etkin Dizin** > **Genel Bakış** sayfasından elde edilebilen Kiracı Kimliğini sorar.

    ```azurepowershell-interactive 

    ./generatePartnerCredentials.ps1   

    ```

8. **API Endpoint,** **Kiracı Kimliği,** **İstemci Kimliği, İstemci** **Gizli**ve **EventHub Bağlantı Dizesi**değerlerini yakalamak için ekrandaki yönergeleri izleyin.

### <a name="integrate-device-data-by-using-the-generated-credentials"></a>Oluşturulan kimlik bilgilerini kullanarak aygıt verilerini tümleştirme

Şimdi önceki bölümden oluşturulan aşağıdaki bilgilere sahipsiniz.
 - API uç noktası
 - EventHub bağlantı dizesi
 - İstemci Kimliği
 - Gizli anahtar
 - Kiracı Kimliği

FarmBeats'i bağlamak için bunu cihaz ortağınıza sağlamanız gerekir. Aynı şeyi yapmak için aygıt ortağı portalına gidin. Örneğin, Davis Instruments, Teralytic veya Pessl Instruments (Metos.at) cihazları kullanıyorsanız, aşağıda belirtildiği gibi ilgili sayfalara gidin:

1. [Davis Enstrümanlar](https://weatherlink.github.io/azure-farmbeats/setup)

2. [Teralitik](https://app.teralytic.com/)

3. [Pessl Aletleri](https://ng.fieldclimate.com/user-api-services)

Aygıt sağlayıcısı başarılı bir tümleştirmeyi onaylar. Onay aldıktan sonra Azure FarmBeats'teki tüm aygıtları ve sensörleri görüntüleyebilirsiniz.

## <a name="view-devices-and-sensors"></a>Cihazları ve sensörleri görüntüleme

Çiftliğinizin cihazlarını ve sensörlerini görüntülemek için aşağıdaki bölümü kullanın.

### <a name="view-devices"></a>Cihazları görüntüleme

Şu anda FarmBeats aşağıdaki cihazları destekler:

- **Düğüm**: Bir veya daha fazla sensörün bağlı olduğu cihaz.
- **Ağ Geçidi**: Bir veya daha fazla düğümün bağlı olduğu aygıt.

Şu adımları uygulayın:

1. Giriş sayfasında, menüden **Cihazlar'ı** seçin.
  **Aygıtlar** sayfası aygıt türünü, modelini, durumunu, yerleştirildiği çiftliği ve meta veriler için son güncelleştirilen tarihi görüntüler. Varsayılan olarak, çiftlik *sütunu NULL*olarak ayarlanır. Bir çiftliğe aygıt atamayı seçebilirsiniz. Daha fazla bilgi için [aygıtları atay'a](#assign-devices)bakın.
2. Aygıta bağlı aygıt özelliklerini, telemetriyi ve alt aygıtları görüntülemek için aygıtı seçin.

    ![Cihazlar sayfası](./media/get-sensor-data-from-sensor-partner/view-devices-1.png)

### <a name="view-sensors"></a>Sensörleri görüntüleme

Şu adımları uygulayın:

1. Ana sayfada menüden **Sensörler'i** seçin.
  **Sensörler** sayfası, sensörün türü, bağlı olduğu çiftlik, ana aygıt, bağlantı noktası adı, bağlantı noktası türü ve güncellenen son durum hakkında ayrıntıları görüntüler.
2. Sensör özelliklerini, aktif uyarıları ve sensörün telemetrisini görüntülemek için sensörü seçin.

    ![Sensörler sayfası](./media/get-sensor-data-from-sensor-partner/view-sensors-1.png)

## <a name="assign-devices"></a>Aygıtları atama  

Sensör verileri aktıktan sonra, sensörleri dağıttığınız çiftliğe atayabilirsiniz.

1. Ana sayfada, menüden **Çiftlikler'i** seçin. **Çiftlikler** listesi sayfası görüntülenir.
2. Aygıtı atamak istediğiniz çiftliği seçin ve Aygıt **Ekle'yi**seçin.
3. **Aygıt Ekle** penceresi görüntülenir. Çiftliğe atamak istediğiniz aygıtı seçin.

    ![Aygıt ekle penceresi](./media/get-sensor-data-from-sensor-partner/add-devices-1.png)

4. **Aygıt Ekle'yi**seçin. Alternatif olarak, **Aygıtlar** menüsüne gidin, bir çiftliğe atamak istediğiniz aygıtları seçin ve **Aygıtları Ilişkilendir'i**seçin.
5. Ortak **Aygıtlar** penceresinde, açılan listeden çiftliği seçin ve seçili tüm aygıtlara çiftlik ilişkilendirmek **için Herkese Uygula'yı** seçin.

    ![Ilişkilendirme Aygıtları penceresi](./media/get-sensor-data-from-sensor-partner/associate-devices-1.png)

6. Her aygıtı farklı bir çiftliğe ilişkilendirmek **için, Farm** sütununa atay sütunundaki açılır ok'u seçin ve her aygıt satırı için bir çiftlik seçin.

7. Aygıt atamasını tamamlamak için **Atla'yı** seçin.

### <a name="visualize-sensor-data"></a>Algılayıcı verilerini görselleştirme

Şu adımları uygulayın:

1. Ana sayfada, **Çiftlikler** sayfasını görüntülemek için menüden **Çiftlikler'i** seçin.
2. Sensör verilerini görmek istediğiniz **Çiftlik'i** seçin.
3. **Farm** panosunda, telemetri verilerini görüntüleyebilirsiniz. Canlı telemetrigörüntüleyebilir veya belirli bir tarih aralığını görmek için **Özel Aralık'ı** kullanabilirsiniz.

    ![Çiftlik panosu](./media/get-sensor-data-from-sensor-partner/telemetry-data-1.png)

## <a name="delete-a-sensor"></a>Sensörü silme

Şu adımları uygulayın:

1. Ana sayfada, **Sensörler** sayfasını görüntülemek için menüden **Sensörler'i** seçin.
2. Silmek istediğiniz aygıtı seçin ve onay penceresinde **Sil'i** seçin.

    ![Sil düğmesi](./media/get-sensor-data-from-sensor-partner/delete-sensors-1.png)

Onay iletisi sensörün başarıyla silindiğini gösterir.

## <a name="delete-devices"></a>Aygıtları silme

Şu adımları uygulayın:

1. Ana sayfada, **Cihazlar** sayfasını görüntülemek için menüden **Cihazlar'ı** seçin.
2. Silmek istediğiniz aygıtı seçin ve onay penceresinde **Sil'i** seçin.

    ![Sil düğmesi](./media/get-sensor-data-from-sensor-partner/delete-device-1.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure FarmBeats örneğinize akan sensör verilerine sahipsiniz. Şimdi, çiftlikleriniz için haritaları nasıl [oluşturacağınızı](generate-maps-in-azure-farmbeats.md#generate-maps) öğrenin.
