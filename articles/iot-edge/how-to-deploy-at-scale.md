---
title: Azure portal Azure IoT Edge için modülleri uygun ölçekte dağıtın
description: IoT Edge cihaz grupları için otomatik dağıtımlar oluşturmak üzere Azure portal kullanın
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 4/21/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: e55d3f704c76d2783c3e442a90c829448129a4d0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82134422"
---
# <a name="deploy-iot-edge-modules-at-scale-using-the-azure-portal"></a>Azure portal kullanarak ölçekli IoT Edge modüllerini dağıtın

Aynı anda birçok cihaz için devam eden dağıtımları yönetmek üzere Azure portal **Otomatik dağıtım IoT Edge** oluşturun. IoT Edge için otomatik dağıtımlar IoT Hub [otomatik cihaz yönetimi](/azure/iot-hub/iot-hub-automatic-device-management) özelliğinin bir parçasıdır. Dağıtımlar birden çok modülü birden çok cihaza dağıtmanızı, modüllerin durumunu ve durumunu izlemenizi ve gerektiğinde değişiklik yapmayı sağlayan dinamik işlemlerdir.

Daha fazla bilgi için bkz. [tek cihazlarda veya ölçekte IoT Edge otomatik dağıtımları anlama](module-deployment-monitoring.md).

## <a name="identify-devices-using-tags"></a>Etiketleri kullanarak cihazları tanımla

Bir dağıtım oluşturabilmeniz için önce hangi cihazların etkilenmesini istediğinizi belirleyebilmelisiniz. Azure IoT Edge cihaz ikizi **etiketleri** kullanarak cihazları tanımlar. Her cihazda, çözümünüz için anlamlı olacak şekilde tanımladığınız birden fazla etiket olabilir.

Örneğin, akıllı binaları bir kampüs yönetiyorsanız, bir cihaza konum, oda türü ve ortam etiketleri ekleyebilirsiniz:

```json
"tags":{
  "location":{
    "building": "20",
    "floor": "2"
  },
  "roomtype": "conference",
  "environment": "prod"
}
```

Cihaz ikgörüti ve etiketleri hakkında daha fazla bilgi için bkz. [IoT Hub 'da cihaz TWINS 'ı anlama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md).

## <a name="create-a-deployment"></a>Dağıtım oluşturma

IoT Edge, senaryonuzu özelleştirmek için kullanabileceğiniz iki farklı türde otomatik dağıtım sağlar. Sistem çalışma zamanı modüllerini ve ek modülleri ve yolları içeren standart bir *dağıtım*oluşturabilirsiniz. Her cihaz yalnızca bir dağıtım uygulayabilir. Ya da sistem çalışma zamanını değil yalnızca özel modüller ve rotalar içeren *katmanlı bir dağıtım*oluşturabilirsiniz. Birçok katmanlı dağıtım, bir cihazda standart dağıtımın üzerine birleştirilebilir. İki otomatik dağıtım türünün birlikte nasıl çalıştığı hakkında daha fazla bilgi için bkz. [tek cihazlarda veya ölçekte IoT Edge otomatik dağıtımları anlama](module-deployment-monitoring.md).

Dağıtım ve katmanlı bir dağıtım oluşturma adımları çok benzerdir. Herhangi bir farklılık aşağıdaki adımlarda çağrılır.

1. [Azure portal](https://portal.azure.com)IoT Hub gidin.
1. Sol bölmedeki menüde **otomatik cihaz yönetimi**altında **IoT Edge** ' yi seçin.
1. Üstteki çubukta **dağıtım oluştur** veya **katmanlı dağıtım oluştur**' u seçin.

Dağıtım oluşturmak için beş adım vardır. Aşağıdaki bölümler her bir adım adım yol gösterir.

### <a name="step-1-name-and-label"></a>1. Adım: adı ve etiketi

1. Dağıtımınıza 128 küçük harflerden oluşan benzersiz bir ad verin. Boşluklardan ve aşağıdaki geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.
1. Dağıtımlarınızın izlenmesine yardımcı olmak için anahtar-değer çiftleri olarak Etiketler ekleyebilirsiniz. Örneğin, **Hostplatform** ve **Linux**veya **Version** ve **3.0.1**.
1. Sonraki adıma geçmek için **modüller** ' i seçin.

### <a name="step-2-modules"></a>2. Adım: modüller

Bir dağıtıma en fazla 20 modül ekleyebilirsiniz. Modül olmadan bir dağıtım oluşturursanız, hedef cihazlardan tüm geçerli modüller kaldırılır.

Dağıtımlarda, IoT Edge Aracısı ve IoT Edge hub modülleri için ayarları yönetebilirsiniz. İki çalışma zamanı modülünü yapılandırmak için **çalışma zamanı ayarları** ' nı seçin. Katmanlı dağıtımda, çalışma zamanı modülleri dahil edilmez, bu nedenle yapılandırılamaz.

Üç tür modül ekleyebilirsiniz:

* IoT Edge modülü
* Market modülü
* Azure Stream Analytics modülü

#### <a name="add-an-iot-edge-module"></a>IoT Edge modülü ekleme

Özel kodu bir modül olarak eklemek veya el ile bir Azure hizmet modülünü eklemek için aşağıdaki adımları izleyin:

1. Sayfanın **Container Registry kimlik bilgileri** bölümünde, bu dağıtımın modül görüntülerini içeren tüm özel kapsayıcı kayıt defterleri için adları ve kimlik bilgilerini belirtin. IoT Edge Aracısı bir Docker görüntüsü için kapsayıcı kayıt defteri kimlik bilgilerini bulamazsa hata 500 ' i raporlar.
1. Sayfanın **IoT Edge modüller** bölümünde **Ekle**' ye tıklayın.
1. Açılır menüden **IoT Edge modülünü** seçin.
1. Modülünüzü bir **IoT Edge modül adı**verin.
1. **Görüntü URI 'si** alanı için modülünüzün kapsayıcı görüntüsünü girin.
1. Bir **yeniden başlatma ilkesi**seçmek için açılan menüyü kullanın. Aşağıdaki seçeneklerden birini seçin:
   * **her zaman** -bu, herhangi bir nedenle kapanırsa modül her zaman yeniden başlatılır.
   * **hiçbir** sebeple, herhangi bir nedenle kapanmadıysa modül hiçbir şekilde yeniden başlatmaz.
   * **hata** durumunda-modül kilitlenirse yeniden başlatılır, ancak temiz bir şekilde kapanmaz.
   * **sağlıksız** -sistem durumu kilitlenirse veya sağlıksız bir durum döndürürse modül yeniden başlatılır. Sistem durumu işlevini uygulamak için her modüle kadar.
1. Modülün **Istenen durumunu** seçmek için açılan menüyü kullanın. Aşağıdaki seçeneklerden birini seçin:
   * **çalıştırma,** varsayılan seçenektir. Modül dağıtıldıktan hemen sonra çalışmaya başlayacaktır.
   * **durduruldu** -dağıtıldıktan sonra, siz veya başka bir modülle başlamak üzere çağrılana kadar modül boşta kalır.
1. Kapsayıcıya geçirilmesi gereken herhangi bir **kapsayıcı oluşturma seçeneğini** belirtin. Daha fazla bilgi için bkz. [Docker Create](https://docs.docker.com/engine/reference/commandline/create/).
1. İkizi modülüne etiket veya diğer özellikler eklemek istiyorsanız **module Ikizi Settings** ' i seçin.
1. Bu modülün **ortam değişkenlerini** girin. Ortam değişkenleri bir modüle yapılandırma bilgileri sağlar.
1. Modülünüzü dağıtıma eklemek için **Ekle** ' yi seçin.

#### <a name="add-a-module-from-the-marketplace"></a>Market 'ten bir modül ekleme

Azure Marketi 'nden bir modül eklemek için aşağıdaki adımları izleyin:

1. Sayfanın **IoT Edge modüller** bölümünde **Ekle**' ye tıklayın.
1. Açılan menüden **Market modülü** ' nü seçin.
1. **IoT Edge Module marketi** sayfasından bir modül seçin. Seçtiğiniz modül, aboneliğiniz, kaynak grubunuz ve cihazınız için otomatik olarak yapılandırılır. Daha sonra IoT Edge modüller listenizde görünür. Bazı modüller için ek yapılandırma gerekebilir. Daha fazla bilgi için bkz. [Azure Marketi 'nden modülleri dağıtma](how-to-deploy-modules-portal.md#deploy-modules-from-azure-marketplace).

#### <a name="add-a-stream-analytics-module"></a>Stream Analytics modülü ekleme

Azure Stream Analytics bir modül eklemek için aşağıdaki adımları izleyin:

1. Sayfanın **IoT Edge modüller** bölümünde **Ekle**' ye tıklayın.
1. Açılır menüden **Azure Stream Analytics modülünü** seçin.
1. Sağ bölmede **aboneliğinizi**seçin.
1. IoT **Edge işinizi**seçin.
1. Modülünüzü dağıtıma eklemek için **Kaydet** ' i seçin.

#### <a name="configure-module-settings"></a>Modül ayarlarını yapılandırma

Bir dağıtıma modül ekledikten sonra, bu adı seçerek **güncelleştirme IoT Edge modülü** sayfasını açabilirsiniz. Bu sayfada modül ayarlarını, ortam değişkenlerini, oluşturma seçeneklerini ve ikizi modülünü düzenleyebilirsiniz. Market 'ten bir modül eklediyseniz, bu parametrelerin bazıları zaten doldurulmuş olabilir.

Katmanlı bir dağıtım oluşturuyorsanız, aynı cihazları hedefleyen diğer dağıtımlarda var olan bir modül yapılandırıyorsanız. Modül ikizi diğer sürümlerin üzerine yazmadan güncelleştirmek için, **module Ikizi ayarları** sekmesini açın. modülün istenen özelliklerinde bir alt bölüm için benzersiz ada sahip yeni bir **Modül ikizi özelliği** oluşturun. Örneğin `properties.desired.settings`,. Yalnızca `properties.desired` alanın içinde Özellikler tanımlarsanız, daha düşük öncelikli dağıtımlarda tanımlanan modül için istenen özelliklerin üzerine yazılır.

![Katmanlı dağıtım için Module ikizi özelliğini ayarla](./media/how-to-deploy-monitor/module-twin-property.png)

Katmanlı dağıtımlarda modül ikizi yapılandırması hakkında daha fazla bilgi için bkz. [katmanlı dağıtım](module-deployment-monitoring.md#layered-deployment).

Yapılandırılmış bir dağıtım için tüm modülleriniz olduktan sonra Ileri ' yi seçin. üçüncü adıma geçmek için **yollar** .

### <a name="step-3-routes"></a>3. Adım: rotalar

Rotalar, modüllerin bir dağıtım içindeki birbirleriyle nasıl iletişim kuracağını tanımlar. Varsayılan olarak, sihirbaz, **yukarı akış** adlı bir yol sağlar ve **/ileti/\* **olarak tanımlanır $upstream. Bu, herhangi bir modülle çıkış iletilerinin IoT Hub 'ınıza gönderildiği anlamına gelir.  

Bilgileri [Declare rotalarındaki](module-composition.md#declare-routes)bilgilerle ekleyin veya güncelleştirin, ardından inceleme bölümüne devam etmek için **İleri** ' yi seçin.

**İleri ' yi seçin: ölçümler**.

### <a name="step-4-metrics"></a>4. Adım: ölçümler

Ölçümler, bir cihazın yapılandırma içeriği uygulama sonucu olarak rapor sağlayabileceği çeşitli durumların Özet sayısını sağlar.

1. **Ölçüm adı**için bir ad girin.

1. **Ölçüm ölçütü**için bir sorgu girin. Sorgu IoT Edge hub modülünün ikizi [bildirilen özelliklerini](module-edgeagent-edgehub.md#edgehub-reported-properties)temel alır. Ölçüm, sorgu tarafından döndürülen satır sayısını temsil eder.

   Örneğin:

   ```sql
   SELECT deviceId FROM devices
     WHERE properties.reported.lastDesiredStatus.code = 200
   ```

**İleri ' yi seçin: hedef cihazlar**.

### <a name="step-5-target-devices"></a>5. Adım: hedef cihazlar

Bu dağıtımı alması gereken belirli cihazları hedeflemek için cihazlarınızdaki Etiketler özelliğini kullanın.

Birden çok dağıtım aynı cihazı hedefleyebilir, bu da her dağıtıma bir öncelik numarası vermeniz gerekir. Herhangi bir çakışma varsa, en yüksek önceliğe sahip dağıtım (daha büyük değerler daha yüksek öncelik gösterir) kazanır. İki dağıtımda aynı öncelik numarası varsa, en son WINS 'nin oluşturulduğu bir değer vardır.

Birden çok dağıtım aynı cihazı hedefliyorsanız, yalnızca yüksek önceliğe sahip olan bir işlem uygulanır. Birden çok katmanlı dağıtım aynı cihazı hedeflerse, hepsi uygulanır. Ancak, herhangi bir özellik yinelendiğinden, aynı ada sahip iki yol varsa, daha yüksek öncelikli katmanlı dağıtımdan biri geri kalanı geçersiz kılar.

Bir cihazın hedeflediği katmanlı dağıtımlar, uygulanması için temel dağıtımdan daha yüksek önceliğe sahip olmalıdır.

1. Dağıtım **önceliği**için pozitif bir tamsayı girin.
1. Bu dağıtıma hangi cihazların hedeflenceğini belirleyen bir **hedef koşul** girin.Bu koşul, Device ikizi etiketlerine veya Device ikizi bildirilen özelliklerine dayalıdır ve ifade biçimiyle eşleşmelidir.Örneğin `tags.environment='test'` veya `properties.reported.devicemodel='4000x'` olabilir.

Ileri ' yi seçin: son adımla gezinmek için **+ Oluştur** ' u seçin.

### <a name="step-6-review-and-create"></a>6. Adım: Inceleme ve oluşturma

Dağıtım bilgilerinizi gözden geçirin ve ardından **Oluştur**' u seçin.

Dağıtımınızı izlemek için bkz. [IoT Edge dağıtımlarını izleme](how-to-monitor-iot-edge-deployments.md).

## <a name="modify-a-deployment"></a>Bir dağıtımı değiştirme

Bir dağıtımı değiştirdiğinizde, değişiklikler hedeflenen tüm cihazlara hemen çoğaltılır. Mevcut bir dağıtım için aşağıdaki ayarları ve özellikleri değiştirebilirsiniz:

* Hedef koşullar
* Özel ölçümler
* Etiketler
* Etiketler
* İstenen özellikler

### <a name="modify-target-conditions-custom-metrics-and-labels"></a>Hedef koşulları, özel ölçümleri ve etiketleri değiştirme

1. IoT Hub 'ınızda sol bölme menüsünden **IoT Edge** ' yi seçin.
1. **IoT Edge dağıtımları** sekmesini seçin ve ardından yapılandırmak istediğiniz dağıtımı seçin.
1. **Hedef koşul** sekmesini seçin. **hedef koşulu** hedeflenen cihazları hedefleyecek şekilde değiştirin. **Önceliği**de ayarlayabilirsiniz.  **Kaydet**’i seçin.

    Hedef koşulu güncelleştirirseniz, aşağıdaki güncelleştirmeler gerçekleşir:

    * Bir cihaz eski hedef koşulunu karşılamıyorsa, ancak yeni hedef koşulunu karşılıyorsa ve bu dağıtım bu cihaz için en yüksek önceliktir, bu dağıtım cihaza uygulanır.
    * Şu anda bu dağıtımı çalıştıran bir cihaz artık hedef koşulu karşılamıyorsa, bu dağıtımı kaldırır ve sonraki en yüksek öncelikli dağıtımı alır.
    * Şu anda bu dağıtımı çalıştıran bir cihaz artık hedef koşulunu karşılamıyorsa ve diğer dağıtımların hedef koşulunu karşılamıyorsa, cihazda hiçbir değişiklik gerçekleşmez. Cihaz geçerli durumunda mevcut modüllerini çalıştırmaya devam eder, ancak artık bu dağıtımın bir parçası olarak yönetilmez. Başka bir dağıtımın hedef koşulunu karşılıyorsa, bu dağıtımı kaldırır ve yenisini alır.

1. **Ölçümler** sekmesini seçin ve **ölçümleri Düzenle** düğmesine tıklayın. Örnek sözdizimini kılavuz olarak kullanarak özel ölçümleri ekleyin veya değiştirin. **Kaydet**’i seçin.

    ![Bir dağıtımda özel ölçümleri düzenleme](./media/how-to-deploy-monitor/metric-list.png)

1. **Etiketler** sekmesini seçin ve istediğiniz değişiklikleri yapın ve **Kaydet**' i seçin.

## <a name="delete-a-deployment"></a>Bir dağıtımı silme

Bir dağıtımı sildiğinizde, dağıtılan tüm cihazlar bir sonraki en yüksek öncelikli dağıtımı alır. Cihazlarınız başka bir dağıtımın hedef koşulunu karşılamıyorsa, dağıtım silindiğinde modüller kaldırılmaz.

1. [Azure Portal](https://portal.azure.com) oturum açın ve IoT Hub gidin.
1. **IoT Edge**seçin.
1. **IoT Edge dağıtımları** sekmesini seçin.

   ![IoT Edge dağıtımlarını görüntüle](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Silmek istediğiniz dağıtımı seçmek için onay kutusunu kullanın.
1. **Sil**’i seçin.
1. Bir istem, bu işlemin bu dağıtımı silineceğini ve tüm cihazlar için önceki duruma geri dönmeyeceğini size bildirir.Düşük önceliğe sahip bir dağıtım uygulanacaktır.Başka bir dağıtım hedeflendiyse hiçbir modül kaldırılmaz. Cihazınızdan tüm modülleri kaldırmak istiyorsanız, sıfır modüllerle bir dağıtım oluşturun ve aynı cihazlara dağıtın.Devam etmek için **Evet** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge cihazlara modül dağıtma](module-deployment-monitoring.md)hakkında daha fazla bilgi edinin.
