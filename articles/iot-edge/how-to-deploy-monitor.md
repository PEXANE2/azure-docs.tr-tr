---
title: Azure portal Azure IoT Edge için modülleri uygun ölçekte dağıtın
description: Cihazları IOT Edge grupları için otomatik dağıtımlar oluşturmak için Azure portalını kullanma
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/30/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0a20ea4236683e26c51bc75309435c65e24271d7
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76510269"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-portal"></a>Dağıtma ve Azure portalını kullanarak ölçekte IOT Edge modülleri izleme

Aynı anda birçok cihaz için devam eden dağıtımları yönetmek üzere Azure portal **Otomatik dağıtım IoT Edge** oluşturun. IoT Edge için otomatik dağıtımlar IoT Hub [otomatik cihaz yönetimi](/azure/iot-hub/iot-hub-automatic-device-management) özelliğinin bir parçasıdır. Dağıtımlar birden çok modülü birden çok cihaza dağıtmanızı, modüllerin durumunu ve durumunu izlemenizi ve gerektiğinde değişiklik yapmayı sağlayan dinamik işlemlerdir.

Daha fazla bilgi için bkz. [tek cihazlarda veya ölçekte IoT Edge otomatik dağıtımları anlama](module-deployment-monitoring.md).

## <a name="identify-devices-using-tags"></a>Etiketleri kullanarak cihazları belirleyin

Bir dağıtımı oluşturmadan önce değiştirmek istediğiniz hangi cihazların belirtebilmek sahip. Azure IOT Edge kullanarak cihazları tanımlar **etiketleri** cihaz ikizinde. Her cihazda, çözümünüz için anlamlı olacak şekilde tanımladığınız birden fazla etiket olabilir.

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

Cihaz ikizleri ve etiketleri hakkında daha fazla bilgi için bkz: [IOT hub'daki cihaz ikizlerini kavrama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md).

## <a name="create-a-deployment"></a>Bir dağıtım oluşturun

IoT Edge, senaryonuzu özelleştirmek için kullanabileceğiniz iki farklı türde otomatik dağıtım sağlar. Sistem çalışma zamanı modüllerini ve ek modülleri ve yolları içeren standart bir *dağıtım*oluşturabilirsiniz. Her cihaz yalnızca bir dağıtım uygulayabilir. Ya da sistem çalışma zamanını değil yalnızca özel modüller ve rotalar içeren *katmanlı bir dağıtım*oluşturabilirsiniz. Birçok katmanlı dağıtım, bir cihazda standart dağıtımın üzerine birleştirilebilir. İki otomatik dağıtım türünün birlikte nasıl çalıştığı hakkında daha fazla bilgi için bkz. [tek cihazlarda veya ölçekte IoT Edge otomatik dağıtımları anlama](module-deployment-monitoring.md).

Dağıtım ve katmanlı bir dağıtım oluşturma adımları çok benzerdir. Herhangi bir farklılık aşağıdaki adımlarda çağrılır.

1. [Azure portal](https://portal.azure.com)IoT Hub gidin.
1. Sol bölmedeki menüde **otomatik cihaz yönetimi**altında **IoT Edge** ' yi seçin.
1. Üstteki çubukta **dağıtım oluştur** veya **katmanlı dağıtım oluştur**' u seçin.

Bir dağıtımı oluşturmak için beş adım vardır. Aşağıdaki bölümlerde, her birini yol.

### <a name="step-1-name-and-label"></a>1\. Adım: adı ve etiketi

1. Dağıtımınızı en çok 128 küçük harf olan benzersiz bir ad verin. Boşluk ve şu geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.
1. Dağıtımlarınızın izlenmesine yardımcı olmak için anahtar-değer çiftleri olarak Etiketler ekleyebilirsiniz. Örneğin, **Hostplatform** ve **Linux**veya **Version** ve **3.0.1**.
1. Sonraki adıma geçmek için **modüller** ' i seçin.

### <a name="step-2-modules"></a>2\. Adım: modüller

Bir dağıtıma en fazla 20 modül ekleyebilirsiniz. Modül olmadan bir dağıtım oluşturursanız, hedef cihazlardan tüm geçerli modüller kaldırılır.

Dağıtımlarda, IoT Edge Aracısı ve IoT Edge hub modülleri için ayarları yönetebilirsiniz. İki çalışma zamanı modülünü yapılandırmak için **çalışma zamanı ayarları** ' nı seçin. Katmanlı dağıtımda, çalışma zamanı modülleri dahil edilmez, bu nedenle yapılandırılamaz.

Üç tür modül ekleyebilirsiniz:

* IoT Edge modülü
* Market modülü
* Azure Stream Analytics modülü

#### <a name="add-an-iot-edge-module"></a>IoT Edge modülü ekleme

Bir modül olarak özel kod ekleyin veya bir Azure hizmeti modülü el ile eklemek için şu adımları izleyin:

1. Sayfanın **Container Registry kimlik bilgileri** bölümünde, bu dağıtımın modül görüntülerini içeren tüm özel kapsayıcı kayıt defterleri için adları ve kimlik bilgilerini belirtin. IoT Edge Aracısı bir Docker görüntüsü için kapsayıcı kayıt defteri kimlik bilgilerini bulamazsa hata 500 ' i raporlar.
1. Sayfanın **IoT Edge modüller** bölümünde **Ekle**' ye tıklayın.
1. Açılır menüden **IoT Edge modülünü** seçin.
1. Modülünüzü bir **IoT Edge modül adı**verin.
1. İçin **görüntü URI'si** modülünüzde için kapsayıcı görüntüsü girin.
1. Seçmek için açılan menüyü kullanın. bir **yeniden ilke**. Aşağıdaki seçeneklerden birini seçin:
   * **her zaman** -bu, herhangi bir nedenle kapanırsa modül her zaman yeniden başlatılır.
   * **hiçbir** sebeple, herhangi bir nedenle kapanmadıysa modül hiçbir şekilde yeniden başlatmaz.
   * **hata** durumunda-modül kilitlenirse yeniden başlatılır, ancak temiz bir şekilde kapanmaz.
   * **sağlıksız** -sistem durumu kilitlenirse veya sağlıksız bir durum döndürürse modül yeniden başlatılır. Bu sistem durumu işlevi uygulamak için her modül aittir.
1. Seçmek için açılan menüyü kullanın **istenen durum** modülü için. Aşağıdaki seçeneklerden birini seçin:
   * **çalıştırma,** varsayılan seçenektir. Modül hemen dağıtıldıktan sonra çalışan başlar.
   * **durduruldu** -dağıtıldıktan sonra, siz veya başka bir modülle başlamak üzere çağrılana kadar modül boşta kalır.
1. Belirtmek **kapsayıcı oluşturma seçenekleri** geçirilecek kapsayıcıya. Daha fazla bilgi için [docker oluşturma](https://docs.docker.com/engine/reference/commandline/create/).
1. İkizi modülüne etiket veya diğer özellikler eklemek istiyorsanız **module Ikizi Settings** ' i seçin.
1. Bu modülün **ortam değişkenlerini** girin. Ortam değişkenleri bir modüle yapılandırma bilgileri sağlar.
1. Modülünüzü dağıtıma eklemek için **Ekle** ' yi seçin.

#### <a name="add-a-module-from-the-marketplace"></a>Market 'ten bir modül ekleme

Azure Marketi 'nden bir modül eklemek için aşağıdaki adımları izleyin:

1. Sayfanın **IoT Edge modüller** bölümünde **Ekle**' ye tıklayın.
1. Açılan menüden **Market modülü** ' nü seçin.
1. **IoT Edge Module marketi** sayfasından bir modül seçin. Seçtiğiniz modül, aboneliğiniz, kaynak grubunuz ve cihazınız için otomatik olarak yapılandırılır. Daha sonra IoT Edge modüller listenizde görünür. Bazı modüller için ek yapılandırma gerekebilir. Daha fazla bilgi için bkz. [Azure Marketi 'nden modülleri dağıtma](how-to-deploy-modules-portal.md#deploy-modules-from-azure-marketplace).

#### <a name="add-a-stream-analytics-module"></a>Stream Analytics modülü ekleme

Azure Stream Analytics'ten bir modül eklemek için aşağıdaki adımları izleyin:

1. Sayfanın **IoT Edge modüller** bölümünde **Ekle**' ye tıklayın.
1. Açılır menüden **Azure Stream Analytics modülünü** seçin.
1. Sağ bölmede **aboneliğinizi**seçin.
1. IoT **Edge işinizi**seçin.
1. Seçin **Kaydet** modülünüzde dağıtıma eklenecek.

#### <a name="configure-module-settings"></a>Modül ayarlarını yapılandırma

Bir dağıtıma modül ekledikten sonra, bu adı seçerek **güncelleştirme IoT Edge modülü** sayfasını açabilirsiniz. Bu sayfada modül ayarlarını, ortam değişkenlerini, oluşturma seçeneklerini ve ikizi modülünü düzenleyebilirsiniz. Market 'ten bir modül eklediyseniz, bu parametrelerin bazıları zaten doldurulmuş olabilir.

Katmanlı bir dağıtım oluşturuyorsanız, aynı cihazları hedefleyen diğer dağıtımlarda var olan bir modül yapılandırıyorsanız. Modül ikizi diğer sürümlerin üzerine yazmadan güncelleştirmek için, **module Ikizi ayarları** sekmesini açın. modülün istenen özelliklerinde bir alt bölüm için benzersiz ada sahip yeni bir **Modül ikizi özelliği** oluşturun, örneğin `properties.desired.settings`. Yalnızca `properties.desired` alanı içinde Özellikler tanımlarsanız, daha düşük öncelikli dağıtımlarda tanımlanan modül için istenen özelliklerin üzerine yazılır.

![Katmanlı dağıtım için Module ikizi özelliğini ayarla](./media/how-to-deploy-monitor/module-twin-property.png)

Katmanlı dağıtımlarda modül ikizi yapılandırması hakkında daha fazla bilgi için bkz. [katmanlı dağıtım](module-deployment-monitoring.md#layered-deployment).

Yapılandırılmış bir dağıtım için tüm modülleriniz olduktan sonra Ileri ' yi seçin. üçüncü adıma geçmek için **yollar** .

### <a name="step-3-routes"></a>3\. Adım: rotalar

Modüller birbirleri ile dağıtımında iletişim kurma biçimini yolları tanımlayın. Varsayılan olarak, sihirbaz, **yukarı akış** adlı bir yol sağlar ve **/ileti/\* 'den $upstream**olarak tanımlanır. Bu, herhangi bir modülle çıkış yapan tüm iletilerin IoT Hub 'ınıza gönderilmesi anlamına gelir.  

Ekleme veya yolları alınan bilgilerle güncelleştirme [bildirmek yollar](module-composition.md#declare-routes), ardından **sonraki** gözden geçirme bölüme geçmek için.

**İleri ' yi seçin: ölçümler**.

### <a name="step-4-metrics"></a>4\. Adım: ölçümler

Ölçümler, bir cihazın yapılandırma içeriği uygulama sonucu olarak rapor sağlayabileceği çeşitli durumların Özet sayısını sağlar.

1. **Ölçüm adı**için bir ad girin.

1. **Ölçüm ölçütü**için bir sorgu girin. Sorgu IoT Edge hub modülünün ikizi [bildirilen özelliklerini](module-edgeagent-edgehub.md#edgehub-reported-properties)temel alır. Ölçüm, sorgu tarafından döndürülen satır sayısını temsil eder.

   Örneğin:

   ```sql
   SELECT deviceId FROM devices
     WHERE properties.reported.lastDesiredStatus.code = 200
   ```

**İleri ' yi seçin: hedef cihazlar**.

### <a name="step-5-target-devices"></a>5\. Adım: hedef cihazlar

Bu dağıtım alması gereken belirli cihazları hedeflemek için etiketler özelliği cihazlarınızdan kullanın.

Birden çok dağıtım aynı cihazı hedefleyebilir olduğundan, her dağıtım öncelik numarası vermesi gerekir. Herhangi bir çakışma varsa, en yüksek önceliğe sahip dağıtım (daha büyük değerler daha yüksek öncelik gösterir) kazanır. İki dağıtım aynı öncelik numarasına sahip olmak, çoğu oluşturulmuş bir son kazanır.

Birden çok dağıtım aynı cihazı hedefliyorsanız, yalnızca yüksek önceliğe sahip olan bir işlem uygulanır. Birden çok katmanlı dağıtım aynı cihazı hedeflerse, hepsi uygulanır. Ancak, herhangi bir özellik yinelendiğinden, aynı ada sahip iki yol varsa, daha yüksek öncelikli katmanlı dağıtımdan biri geri kalanı geçersiz kılar.

Bir cihazın hedeflediği katmanlı dağıtımlar, uygulanması için temel dağıtımdan daha yüksek önceliğe sahip olmalıdır.

1. Dağıtım için pozitif bir tamsayı girin **öncelik**.
1. Girin bir **hedef koşulu** hangi cihazların bu dağıtım ile hedeflenecek belirlemek için. Bu koşul, Device ikizi etiketlerine veya Device ikizi bildirilen özelliklerine dayalıdır ve ifade biçimiyle eşleşmelidir. Örneğin, `tags.environment='test'` veya `properties.reported.devicemodel='4000x'`.

Ileri ' yi seçin: son adımla gezinmek için **+ Oluştur** ' u seçin.

### <a name="step-6-review-and-create"></a>6\. Adım: Inceleme ve oluşturma

Dağıtım bilgilerinizi gözden geçirin ve ardından **Oluştur**' u seçin.

## <a name="monitor-a-deployment"></a>Bir dağıtımını izleme

Bir dağıtımın ayrıntılarını görüntülemek ve onu çalıştıran cihazları izlemek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com) oturum açın ve IoT Hub gidin.
1. Seçin **IOT Edge**.
1. **IoT Edge dağıtımları** sekmesini seçin.

   ![IOT Edge dağıtımları görüntüle](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Dağıtım listesini inceleyin. Her dağıtım için aşağıdaki ayrıntıları görebilirsiniz:
   * **Kimliği** -dağıtım adı.
   * **Tür** - **dağıtım ya da** **katmanlı dağıtım**türü.
   * **Hedef koşul** -hedeflenen cihazları tanımlamak için kullanılan etiket.
   * **Öncelik** -bir dağıtıma atanan öncelik numarası.
   * **Sistem ölçümlerini** - **hedeflenen** hedefleme koşula uyan IOT hub'da cihaz ikizlerini belirtir ve **uygulanan** sahip cihazların sayısını belirtir Dağıtım içeriğini kendi modül ikizlerini IOT hub'ında uygulanan.
   * **Cihaz ölçümleri** -dağıtımdaki IoT Edge cihazların sayısı, IoT Edge istemci çalışma zamanının başarısını veya hatalarını bildiriyor.
   * **Özel ölçümler** -dağıtım için tanımladığınız tüm ölçümler için dağıtım raporlama verilerinde IoT Edge cihazların sayısı.
   * **Oluşturma zamanı** -dağıtımın oluşturulduğu zaman damgası. Bu zaman damgasından iki dağıtım aynı önceliğe sahip olduğunuzda TIES ayırmak için kullanılır.
1. İzlemek istediğiniz dağıtımı seçin.  
1. Dağıtım ayrıntılarını inceleyin. Dağıtım ayrıntılarını gözden geçirmek için sekmeleri kullanabilirsiniz.

## <a name="modify-a-deployment"></a>Bir dağıtım değiştirme

Bir dağıtım değiştirdiğinizde değişiklikler hemen hedeflenen tüm cihazlara çoğaltın.

Hedef koşul güncelleştirme aşağıdaki güncelleştirmeleri oluşur:

* Ardından, bir cihaz, eski hedef koşul karşılanmadıysa, ancak yeni hedef koşulunu ve bu dağıtım bu cihaz için en yüksek öncelikli ise, bu dağıtım cihaza uygulanır.
* Şu anda bu dağıtım artık çalıştıran bir cihaza hedef koşulu karşılıyorsa, bu dağıtım kaldırır ve sonraki en yüksek öncelikli dağıtımı alır.
* Şu anda bu dağıtım artık çalıştıran bir cihaza hedef koşulu karşılayan ve diğer tüm dağıtımları, hedef koşulu yerine getirmeyen, hiçbir değişiklik cihazda gerçekleşir. Cihaz, geçerli alt modüller kendi geçerli durumunda çalışmaya devam eder ancak artık bu dağıtımın bir parçası olarak yönetilmez. Başka bir dağıtım hedef koşulu karşılayan sonra bu dağıtım kaldırır ve yeni alır.

Bir dağıtım değiştirmek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com) oturum açın ve IoT Hub gidin.
1. Seçin **IOT Edge**.
1. **IoT Edge dağıtımları** sekmesini seçin.

   ![IOT Edge dağıtımları görüntüle](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Değiştirmek istediğiniz dağıtımı seçin.
1. Aşağıdaki sekmelerde güncelleştirmeleri yapın:
   * **Hedef koşul**
   * **Ölçümler** -tanımladığınız ölçümleri değiştirebilir veya silebilir veya yenilerini ekleyebilirsiniz.
   * **Etiketler**
   * **Modüller**
   * **Rotalar**
   * **Dağıtım**

1. **Kaydet**’i seçin.
1. Bağlantısındaki [bir dağıtımını izleme](#monitor-a-deployment) dağıtmadan değişiklikleri izlemek için.

## <a name="delete-a-deployment"></a>Dağıtımı Sil

Bir dağıtımı sildiğinizde, dağıtılan tüm cihazlar bir sonraki en yüksek öncelikli dağıtımı alır. Daha sonra başka bir dağıtım hedef koşulu cihazlarınızı karşılamıyorsa, dağıtım silindiğinde modülleri kaldırılmaz.

1. [Azure Portal](https://portal.azure.com) oturum açın ve IoT Hub gidin.
1. Seçin **IOT Edge**.
1. **IoT Edge dağıtımları** sekmesini seçin.

   ![IOT Edge dağıtımları görüntüle](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Silmek istediğiniz dağıtım seçmek için onay kutusunu kullanın.
1. **Sil**’i seçin.
1. Bir komut istemi Bu eylem bu dağıtımı silin ve tüm cihazlar için önceki duruma geri bildirir.  Bu, düşük önceliğe sahip bir dağıtımın uygulanacağı anlamına gelir. Başka bir dağıtım hedeflendiyse hiçbir modül kaldırılmaz. Cihazınızın tüm modülleri kaldırmak istiyorsanız, sıfır modülleriyle bir dağıtımını oluşturun ve aynı cihazlara dağıtın. Devam etmek için **Evet** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge cihazlara modül dağıtma](module-deployment-monitoring.md)hakkında daha fazla bilgi edinin.
