---
title: İş ortaklarından algılayıcı verileri al
description: Bu makalede, iş ortaklarından algılayıcı verilerinin nasıl alınacağı açıklanır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: ece310a248140b7913ffcc9f7146d382ee44fb5d
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74851307"
---
# <a name="get-sensor-data-from-sensor-partners"></a>Algılayıcı iş ortaklarından algılayıcı verileri al

Azure Farm, IoT cihazlarınızdan ve sensörlerden veri hub 'ına akış verileri almanıza yardımcı olur. Şu anda, aşağıdaki algılayıcı cihaz iş ortakları desteklenir.

  ![Farmtempts iş ortakları](./media/get-sensor-data-from-sensor-partner/partner-information-1.png)

Cihaz verilerinin Azure Farmile tümleştirilmesi, grubunuzda veri hub 'ına dağıtılan IoT sensörlerinden veri almanıza yardımcı olur. Veriler, kullanılabilir olduğunda, Farmtts Hızlandırıcısı aracılığıyla görselleştirilebilir. Veriler, veri Fusion ve makine öğrenimi/yapay zekası (ML/AI) modeli için, Farmtts kullanılarak kullanılabilir.

Algılayıcı veri akışını başlatmak için aşağıdakilerden emin olun:

-  Azure Market 'te Farmtts yüklediniz.
-  Grubunuza yüklemek istediğiniz sensörlerden ve cihazlara karar verdiniz.
-  Soil nemi sensörlerinden birini kullanmayı planlıyorsanız, algılayıcı sayısına ve tam olarak yerleştirmeniz gereken yere yönelik bir öneri almak için farmtts SOIL nemi algılayıcı yerleştirme haritasını kullanın. Daha fazla bilgi için bkz. [haritalar oluşturma](generate-maps.md).
- Cihaz veya Sensörlerinizi grubunuzdaki iş ortağınızdan satın alıp dağıtırsınız. Algılayıcı verilerine cihaz iş ortaklarınızın çözümü aracılığıyla erişebildiğinizden emin olun.

## <a name="enable-device-integration-with-farmbeats"></a>Cihaz tümleştirmesini Farmtempts ile etkinleştirme 

Algılayıcı verilerinin akışını başlattıktan sonra, verileri Farmtts sisteminize alma sürecini başlatabilirsiniz. Farmto 'Lar ile tümleştirmeyi sağlamak için cihaz sağlayıcınıza aşağıdaki bilgileri sağlayın:

 - API uç noktası
 - Kiracı Kimliği
 - İstemci Kimliği
 - Gizli anahtar
 - EventHub bağlantı dizesi

Önceki bilgiler, sistem tümleştiricisi tarafından size sunulmaktadır. Cihaz tümleştirmelerini etkinleştirdiğinizde karşılaşabileceğiniz herhangi bir sorun için sistem tümleştirmelerinizi ile iletişime geçin.

Alternatif olarak, Azure Cloud Shell ' den bu betiği çalıştırarak kimlik bilgilerini oluşturabilirsiniz. Şu adımları izleyin.

1. [ZIP dosyasını](https://aka.ms/farmbeatspartnerscript)indirin ve yerel sürücünüze ayıklayın. ZIP dosyasının içinde iki dosya bulunur.
2. https://portal.azure.com/ oturum açın ve Cloud Shell açın. Bu seçenek, Azure portal sağ üst köşesindeki araç çubuğunda bulunur.

    ![Azure portal araç çubuğu](./media/get-drone-imagery-from-drone-partner/navigation-bar-1.png)

3. Ortamın **PowerShell**olarak ayarlandığından emin olun. Varsayılan olarak, Bash olarak ayarlanır.

    ![PowerShell araç çubuğu ayarı](./media/get-sensor-data-from-sensor-partner/power-shell-new-1.png)

4. Cloud Shell örneğindeki 1. adımdan iki dosyayı karşıya yükleyin.

    ![Araç çubuğu düğmesini karşıya yükle](./media/get-sensor-data-from-sensor-partner/power-shell-two-1.png)

5. Dosyaların karşıya yüklendiği dizine gidin. Varsayılan olarak, Kullanıcı adı altında ana dizine yüklenir.
6. Şu betiği çalıştırın:

    ```azurepowershell-interactive 

    ./generateCredentials.ps1   

    ```
7. **API uç noktası**, **kiracı KIMLIĞI**, **Istemci kimliği**, **istemci gizli anahtarı**ve **EventHub bağlantı dizesi**için değerleri yakalamak üzere ekran yönergelerini izleyin. EventHub bağlantı dizesi, Swagger içindeki API yanıtının bir parçası olarak kullanılabilir.

### <a name="integrate-device-data-by-using-the-generated-credentials"></a>Oluşturulan kimlik bilgilerini kullanarak cihaz verilerini tümleştirme

Önceki bölümde oluşturduğunuz kimlik bilgileri kümesini kullanarak Farmtts 'leri bağlamak için cihaz iş ortağı portalına gidin:

 - API uç noktası
 - EventHub bağlantı dizesi
 - İstemci Kimliği
 - Gizli anahtar
 - Kiracı Kimliği

 Cihaz sağlayıcısı başarılı bir tümleştirmeyi onaylar. Onay sonrasında, tüm cihazları ve algılayıcıları Azure Farmtts üzerinde görüntüleyebilirsiniz.

## <a name="view-devices-and-sensors"></a>Cihazları ve algılayıcıları görüntüleme

Grubunuza yönelik cihazları ve algılayıcıları görüntülemek için aşağıdaki bölümü kullanın.

### <a name="view-devices"></a>Cihazları görüntüleme

Şu anda, Farmtempts aşağıdaki cihazları destekler:

- **Düğüm**: bir veya daha fazla sensörün eklendiği cihaz.
- **Ağ geçidi**: bir veya daha fazla düğümün eklendiği cihaz.

Şu adımları izleyin.

1. Giriş sayfasında, menüden **cihazlar** ' ı seçin.
  **Cihazlar** sayfası cihaz türünü, modeli, durumu, yerleştirildiği grubu ve meta verilerin son güncelleştirilme tarihini görüntüler. Varsayılan olarak, Grup sütunu *null*olarak ayarlanır. Bir gruba bir cihaz atamayı seçebilirsiniz. Daha fazla bilgi için bkz. [cihaz atama](#assign-devices).
2. Cihaza bağlı cihaz özelliklerini, telemetri ve alt cihazları görüntülemek için cihazı seçin.

    ![Cihazlar sayfası](./media/get-sensor-data-from-sensor-partner/view-devices-1.png)

### <a name="view-sensors"></a>Algılayıcıları görüntüle

Şu adımları izleyin.

1. Giriş sayfasında, menüden **Algılayıcılar** ' ı seçin.
  **Algılayıcılar** sayfasında, algılayıcı türü, bağlandığı grup, ana cihaz, bağlantı noktası adı, bağlantı noktası türü ve son güncel durum hakkındaki ayrıntılar gösterilir.
2. Sensörden algılayıcı özelliklerini, etkin uyarıları ve telemetrisini görüntülemek için sensör seçin.

    ![Algılayıcılar sayfası](./media/get-sensor-data-from-sensor-partner/view-sensors-1.png)

## <a name="assign-devices"></a>Cihazları ata  

Algılayıcı verileri akışını gerçekleştirdikten sonra, algılayıcıyı dağıttığınız gruba atayabilirsiniz.

1. Giriş sayfasında, menüden **gruplar** ' ı seçin. **Gruplar** listesi sayfası görüntülenir.
2. Cihazı atamak istediğiniz grubu seçin ve cihaz **Ekle**' yi seçin.
3. **Cihaz Ekle** penceresi görüntülenir. Gruba atamak istediğiniz cihazı seçin.

    ![Cihaz Ekle penceresi](./media/get-sensor-data-from-sensor-partner/add-devices-1.png)

4. **Cihaz Ekle**' yi seçin. Alternatif olarak, **cihazlar** menüsüne gidin, bir gruba atamak istediğiniz cihazları seçin ve **cihazları ilişkilendir**' i seçin.
5. **Cihazları ilişkilendir** penceresinde, açılan listeden grubu seçin ve grubu seçili tüm cihazlarla Ilişkilendirmek Için **Tümüne Uygula** ' yı seçin.

    ![Cihazları ilişkilendir penceresi](./media/get-sensor-data-from-sensor-partner/associate-devices-1.png)

6. Her bir cihazı farklı bir grupla ilişkilendirmek için **gruba ata** sütunundaki açılan oku seçin ve her bir cihaz satırı için bir grup seçin.
7. Cihaz atamasını tamamlamaya yönelik **ata** ' yı seçin.

### <a name="visualize-sensor-data"></a>Algılayıcı verilerini görselleştirin

Şu adımları izleyin.

1. Giriş sayfasında, **gruplar** sayfasını görüntülemek Için menüden **gruplar** ' ı seçin.
2. Algılayıcı verilerini görmek istediğiniz **grubu** seçin.
3. **Grup** panosunda telemetri verilerini görüntüleyebilirsiniz. Belirli bir tarih aralığını görmek için canlı Telemetriyi görüntüleyebilir veya **özel Aralık** kullanabilirsiniz.

    ![Grup panosu](./media/get-sensor-data-from-sensor-partner/telemetry-data-1.png)

## <a name="delete-a-sensor"></a>Algılayıcıyı silme

Şu adımları izleyin.

1. Giriş sayfasında, **sensör** sayfasını görüntülemek Için menüden **Algılayıcılar** ' ı seçin.
2. Silmek istediğiniz cihazı seçin ve onay penceresinde **Sil** ' i seçin.

    ![Sil düğmesi](./media/get-sensor-data-from-sensor-partner/delete-sensors-1.png)

Bir onay iletisi, sensör başarıyla silindiğini gösterir.

## <a name="delete-devices"></a>Cihazları Sil

Şu adımları izleyin.

1. Giriş sayfasında, **cihazlar** sayfasını görüntülemek Için menüdeki **cihazlar** ' ı seçin.
2. Silmek istediğiniz cihazı seçin ve onay penceresinde **Sil** ' i seçin.

    ![Sil düğmesi](./media/get-sensor-data-from-sensor-partner/delete-device-1.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Farmtts örneğiniz için sensör verileri akar. Şimdi, gruplar için [haritalar oluşturmayı](generate-maps.md#generate-maps) öğrenin.
