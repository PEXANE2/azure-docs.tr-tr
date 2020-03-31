---
title: Azure portalında modülleri ölçekte dağıtma - Azure IoT Edge
description: IoT Edge aygıt grupları için otomatik dağıtımlar oluşturmak için Azure portalını kullanın
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/30/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0a20ea4236683e26c51bc75309435c65e24271d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271440"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-portal"></a>Azure portalını kullanarak IoT Edge modüllerini ölçekte dağıtın ve izleyin

Birçok cihaz için aynı anda devam eden dağıtımları yönetmek için Azure portalında **bir IoT Edge otomatik dağıtımı** oluşturun. IoT Edge için otomatik dağıtımlar, IoT Hub'ın [otomatik aygıt yönetimi](/azure/iot-hub/iot-hub-automatic-device-management) özelliğinin bir parçasıdır. Dağıtımlar, birden çok cihaza birden fazla modül dağıtmanızı, modüllerin durumunu ve sistem durumunu izlemenizi ve gerektiğinde değişiklik yapmanızı sağlayan dinamik işlemlerdir.

Daha fazla bilgi için bkz: [Tek aygıtlar için veya ölçekte IoT Edge otomatik dağıtımları anlayın.](module-deployment-monitoring.md)

## <a name="identify-devices-using-tags"></a>Etiketleri kullanarak cihazları tanımlama

Dağıtım oluşturmadan önce, hangi aygıtları etkilemek istediğinizi belirtemeniz gerekir. Azure IoT Edge, aygıt ikizinde **etiketleri** kullanan aygıtları tanımlar. Her aygıtın, çözümünüz için anlamlı olan herhangi bir şekilde tanımladığınız birden çok etiketi olabilir.

Örneğin, akıllı binalardan oluşan bir kampüsü yönetiyorsanız, bir aygıta konum, oda türü ve ortam etiketleri ekleyebilirsiniz:

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

Aygıt ikizleri ve etiketleri hakkında daha fazla bilgi [için, IoT Hub'da aygıt ikizlerini anlayın ve kullanın.](../iot-hub/iot-hub-devguide-device-twins.md)

## <a name="create-a-deployment"></a>Dağıtım oluşturma

IoT Edge, senaryonuzu özelleştirmek için kullanabileceğiniz iki farklı türde otomatik dağıtım sağlar. Bu sistem çalışma zamanı modülleri ve herhangi bir ek modüller ve rotalar içeren standart bir *dağıtım*oluşturabilirsiniz. Her aygıt yalnızca bir dağıtım uygulayabilir. Veya sistem çalışma süresini değil, yalnızca özel modülleri ve yolları içeren katmanlı bir *dağıtım*oluşturabilirsiniz. Birçok katmanlı dağıtım, standart bir dağıtımın üstünde bir aygıtta birleştirilebilir. İki tür otomatik dağıtımın birlikte nasıl çalıştığı hakkında daha fazla bilgi için [bkz.](module-deployment-monitoring.md)

Dağıtım ve katmanlı dağıtım oluşturma adımları çok benzer. Herhangi bir fark aşağıdaki adımlarda çağrılır.

1. Azure [portalında,](https://portal.azure.com)IoT Hub'ınıza gidin.
1. Sol bölmedeki menüde Otomatik **Aygıt Yönetimi**altında **IoT Edge'i** seçin.
1. Üst çubuğunda **Dağıtım Oluştur** veya Katmanlı **Dağıtım Oluştur'u**seçin.

Dağıtım oluşturmak için beş adım vardır. Aşağıdaki bölümlerher biri ile yürümek.

### <a name="step-1-name-and-label"></a>Adım 1: Ad ve etiket

1. Dağıtımınıza en fazla 128 küçük harf içeren benzersiz bir ad verin. Boşluklardan ve aşağıdaki geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.
1. Dağıtımlarınızı izlemenize yardımcı olmak için anahtar değer çiftleri olarak etiketler ekleyebilirsiniz. Örneğin, **HostPlatform** ve **Linux**veya **Sürüm** ve **3.0.1**.
1. İleri'yi seçin: İkinci adıma geçmek için **modüller.**

### <a name="step-2-modules"></a>Adım 2: Modüller

Dağıtıma en fazla 20 modül ekleyebilirsiniz. Modülolmadan bir dağıtım oluşturursanız, geçerli modülleri hedef aygıtlardan kaldırır.

Dağıtımlarda, IoT Edge aracısı ve IoT Edge hub modüllerinin ayarlarını yönetebilirsiniz. İki çalışma zamanı modülünü yapılandırmak için **Çalışma Zamanı Ayarları'nı** seçin. Katmanlı dağıtımda, çalışma zamanı modülleri dahil edilmeyin, bu nedenle yapılandırılamaz.

Üç tür modül ekleyebilirsiniz:

* IoT Edge Modülü
* Pazar Yeri Modülü
* Azure Akışı Analitik Modülü

#### <a name="add-an-iot-edge-module"></a>IoT Edge modülü ekleme

Modül olarak özel kod eklemek veya bir Azure hizmet modülü el ile eklemek için aşağıdaki adımları izleyin:

1. Sayfanın **Kapsayıcı Kayıt Kimlik Bilgileri** bölümünde, bu dağıtım için modül görüntülerini içeren özel kapsayıcı kayıtlarının adlarını ve kimlik bilgilerini sağlayın. IoT Edge Agent, Docker görüntüsü için konteyner kayıt defteri kimlik bilgisini bulamazsa hata 500'i bildirir.
1. Sayfanın **IoT Kenar Modülleri** bölümünde **Ekle'yi**tıklatın.
1. Açılan menüden **IoT Edge Modülü'nü** seçin.
1. Modülünüze **bir IoT Edge Modül Adı**verin.
1. Image **URI** alanı için, modülünüz için kapsayıcı görüntüsünü girin.
1. **Yeniden Başlat ilkesini**seçmek için açılır menüyü kullanın. Aşağıdaki seçeneklerden birini seçin:
   * **her zaman** - Modül herhangi bir nedenle kapanırsa her zaman yeniden başlatılır.
   * **asla** - Modül herhangi bir nedenle kapanırsa asla yeniden başlatılmazsa.
   * **on-failure** - Modül çökerse yeniden başlatılır, ancak temiz bir şekilde kapanırsa değil.
   * **sağlıksız** - Modül çökerse veya sağlıksız bir durum döndürürse yeniden başlatılır. Sağlık durumu işlevini uygulamak her modüle kalmış.
1. Modül için **İstenilen Durum'u** seçmek için açılır menüyü kullanın. Aşağıdaki seçeneklerden birini seçin:
   * **çalışan** - Çalışan varsayılan seçenektir. Modül, dağıtıldıktan hemen sonra çalışmaya başlar.
   * **durduruldu** - Konuşlandırıldıktan sonra, modül siz veya başka bir modül tarafından başlatılınana kadar boşta kalır.
1. **Kapsayıcıya** geçirilmesi gereken kapsayıcı oluşturma seçeneklerini belirtin. Daha fazla bilgi için [docker'ın oluşturmaya bakınız.](https://docs.docker.com/engine/reference/commandline/create/)
1. Modül ikizine etiket veya başka özellikler eklemek istiyorsanız **Modül İkiz Ayarları'nı** seçin.
1. Bu modül için **Çevre Değişkenleri** girin. Ortam değişkenleri bir modüle yapılandırma bilgileri sağlar.
1. Modülünüzü dağıtıma eklemek için **Ekle'yi** seçin.

#### <a name="add-a-module-from-the-marketplace"></a>Marketten modül ekleme

Azure Marketi'nden bir modül eklemek için aşağıdaki adımları izleyin:

1. Sayfanın **IoT Kenar Modülleri** bölümünde **Ekle'yi**tıklatın.
1. Açılan menüden **Market Modülü'nü** seçin.
1. **IoT Edge Modül Pazarı** sayfasından bir modül seçin. Seçtiğiniz modül aboneliğiniz, kaynak grubunuz ve aygıtınız için otomatik olarak yapılandırılır. Daha sonra IoT Edge modülleri listenizde görünür. Bazı modüller ek yapılandırma gerektirebilir. Daha fazla bilgi için Azure [pazar yerinden modülleri dağıt'a](how-to-deploy-modules-portal.md#deploy-modules-from-azure-marketplace)bakın.

#### <a name="add-a-stream-analytics-module"></a>Akış Analizi modülü ekleme

Azure Akış Analizi'nden bir modül eklemek için aşağıdaki adımları izleyin:

1. Sayfanın **IoT Kenar Modülleri** bölümünde **Ekle'yi**tıklatın.
1. Açılan menüden **Azure Akışı Analizi modülünü** seçin.
1. Sağ bölmede **Aboneliğinizi**seçin.
1. IoT **Edge işinizi**seçin.
1. Modülünüzü dağıtıma eklemek için **Kaydet'i** seçin.

#### <a name="configure-module-settings"></a>Modül ayarlarını yapılandırma

Dağıtıma bir modül ekledikten sonra, **IoT Edge Modülünü Güncelleştir** sayfasını açmak için adını seçebilirsiniz. Bu sayfada, modül ayarlarını, ortam değişkenlerini, seçenekler oluşturabilir ve modül ikizlerini ayarlayabilirsiniz. Pazardan bir modül eklediyseniz, bu parametrelerin bazıları zaten doldurulmuş olabilir.

Katmanlı bir dağıtım oluşturuyorsanız, aynı aygıtları hedefleyen diğer dağıtımlarda bulunan bir modülü yapılandırıyor olabilirsiniz. Modül ikizini diğer sürümlere abartmadan güncellemek **için, Modül İkiz Ayarları** sekmesini açın. Örneğin, modül ikizinin istediği özellikler içinde bir alt bölüm için benzersiz bir adla yeni bir **Modül İkiz Özelliği** `properties.desired.settings`oluşturun. Yalnızca `properties.desired` alan içindeki özellikleri tanımlarsanız, daha düşük öncelikli dağıtımlarda tanımlanan modül için istenen özelliklerin üzerine yazılır.

![Katmanlı dağıtım için modül ikiz özelliğini ayarlama](./media/how-to-deploy-monitor/module-twin-property.png)

Katmanlı dağıtımlarda modül ikiz yapılandırması hakkında daha fazla bilgi için katmanlı [dağıtıma](module-deployment-monitoring.md#layered-deployment)bakın.

Dağıtım için tüm modülleri yapılandırdıktan **sonra, Sonraki:** Üçüncü adıma geçmek için rotalar'ı seçin.

### <a name="step-3-routes"></a>Adım 3: Rotalar

Rotalar, modüllerin dağıtım sırasında birbirleriyle nasıl iletişim kurduğunu tanımlar. Varsayılan olarak sihirbaz size **upstream** olarak adlandırılan ve **FROM\* /messages/ INTO $upstream**olarak tanımlanan bir rota verir, bu da herhangi bir modül tarafından çıkan iletilerin IoT hub'ınıza gönderildiği anlamına gelir.  

Bildirrotaları'ndan gelen [Declare routes](module-composition.md#declare-routes)bilgilerle rotaları ekleyin veya güncelleştirin , ardından inceleme bölümüne devam etmek için **İleri'yi** seçin.

**Sonraki'ni seçin: Ölçümler**.

### <a name="step-4-metrics"></a>Adım 4: Ölçümler

Ölçümler, yapılandırma içeriğinin uygulanması sonucunda bir aygıtın geri bildirebileceği çeşitli durumların özet sayımlarını sağlar.

1. **Metrik Ad**için bir ad girin.

1. **Metrik Ölçütler**için bir sorgu girin. Sorgu IoT Edge hub modülü ikiz [bildirilen özellikleri](module-edgeagent-edgehub.md#edgehub-reported-properties)dayanmaktadır. Metrik, sorgu tarafından döndürülen satır sayısını gösterir.

   Örnek:

   ```sql
   SELECT deviceId FROM devices
     WHERE properties.reported.lastDesiredStatus.code = 200
   ```

**Sonraki'ni seçin: Hedef Aygıtlar**.

### <a name="step-5-target-devices"></a>Adım 5: Hedef cihazlar

Bu dağıtımı alması gereken belirli aygıtları hedeflemek için aygıtlarınızdaki etiketler özelliğini kullanın.

Birden çok dağıtım aynı aygıtı hedeflenediğinden, her dağıtıma bir öncelik numarası vermelisiniz. Bir çakışma varsa, en yüksek önceliğe (daha büyük değerler daha yüksek önceliği gösterir) dağıtım kazanır. İki dağıtım aynı öncelik numarasına sahipse, en son oluşturulan kişi kazanır.

Birden çok dağıtım aynı aygıtı hedefliyorsa, yalnızca daha yüksek önceliğe sahip olan aygıt uygulanır. Birden çok katmanlı dağıtımlar aynı aygıtı hedefliyorsa, bunların tümü uygulanır. Ancak, aynı ada sahip iki yol varsa, herhangi bir özellik çoğaltılırsa, daha yüksek öncelikli katmanlı dağıtımdan biri geri kalanının üzerine yazar.

Bir aygıtı hedefleyen katmanlı dağıtımın uygulanabilmesi için temel dağıtımdan daha yüksek bir önceliğe sahip olması gerekir.

1. Dağıtım **Önceliği**için pozitif bir tamsayı girin.
1. Bu dağıtımda hangi aygıtların hedefalınacağını belirlemek için bir **Hedef koşulu** girin.Koşul, aygıt ikiz etiketlerine veya aygıt ikizi bildirilen özelliklere dayanır ve ifade biçimiyle eşleşmelidir.Örneğin `tags.environment='test'` veya `properties.reported.devicemodel='4000x'` olabilir.

Sonraki'ni seçin: Son adıma geçmek için **Gözden Geçir + Oluştur.**

### <a name="step-6-review-and-create"></a>Adım 6: Gözden geçirin ve oluşturun

Dağıtım bilgilerinizi gözden geçirin ve ardından **Oluştur'u**seçin.

## <a name="monitor-a-deployment"></a>Dağıtımı izleme

Dağıtımın ayrıntılarını görüntülemek ve çalıştıran aygıtları izlemek için aşağıdaki adımları kullanın:

1. [Azure portalında](https://portal.azure.com) oturum açın ve IoT Hub'ınıza gidin.
1. **IoT Edge'i**seçin.
1. **IoT Edge dağıtımları** sekmesini seçin.

   ![IoT Edge dağıtımlarını görüntüleme](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Dağıtım listesini inceleyin.Her dağıtım için aşağıdaki ayrıntıları görüntüleyebilirsiniz:
   * **ID** - dağıtımAdı.
   * **Türü** - **dağıtım** türü, dağıtım veya **Katmanlı Dağıtım.**
   * **Hedef Koşul** - hedeflenen aygıtları tanımlamak için kullanılan etiket.
   * **Öncelik** - dağıtıma atanan öncelik numarası.
   * **Sistem ölçümleri** - **Hedeflenen,** IoT Hub'da hedefleme koşuluyla eşleşen aygıt ikizlerinin sayısını belirtir ve **Applied,** IoT Hub'da dağıtım içeriğine uygulanan aygıt sayısını belirtir.
   * **Aygıt Ölçümleri** - Dağıtımdaki IoT Edge aygıtlarının sayısı başarıyı veya IoT Edge istemciçalışma zamanındaki hataları raporeder.
   * **Özel Ölçümler** - Dağıtım için tanımladığınız ölçümler için dağıtım raporlama verilerindeki IoT Edge aygıtlarının sayısı.
   * **Oluşturma Zamanı** - dağıtımın oluşturulduğu zamana ait zaman damgası. Bu zaman damgası, iki dağıtım aynı önceliğe sahip olduğunda ilişkileri kırmak için kullanılır.
1. İzlemek istediğiniz dağıtımı seçin.  
1. Dağıtım ayrıntılarını inceleyin. Dağıtım ayrıntılarını gözden geçirmek için sekmeleri kullanabilirsiniz.

## <a name="modify-a-deployment"></a>Dağıtımı değiştirme

Bir dağıtımı değiştirdiğinizde, değişiklikler hemen tüm hedeflenen aygıtlara çoğalır.

Hedef koşulu güncelleştirirseniz, aşağıdaki güncelleştirmeler oluşur:

* Bir aygıt eski hedef koşulu karşılamadı, ancak yeni hedef koşulu karşılıyorsa ve bu dağıtım bu aygıt için en yüksek öncelikse, bu dağıtım aygıta uygulanır.
* Şu anda bu dağıtımı çalıştıran bir aygıt artık hedef koşulu karşılamazsa, bu dağıtımı kaldırır ve bir sonraki en yüksek öncelikli dağıtımı alır.
* Şu anda bu dağıtımı çalıştıran bir aygıt artık hedef koşulu karşılamıyorsa ve diğer dağıtımların hedef koşulunu karşılamıyorsa, aygıtta değişiklik olmaz. Aygıt geçerli modüllerini geçerli durumunda çalıştırmaya devam eder, ancak artık bu dağıtımın bir parçası olarak yönetilmez. Başka bir dağıtımın hedef koşulunu karşıladıktan sonra, bu dağıtımı yükler ve yenisini alır.

Dağıtımı değiştirmek için aşağıdaki adımları kullanın:

1. [Azure portalında](https://portal.azure.com) oturum açın ve IoT Hub'ınıza gidin.
1. **IoT Edge'i**seçin.
1. **IoT Edge Dağıtımlar** sekmesini seçin.

   ![IoT Edge dağıtımlarını görüntüleme](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Değiştirmek istediğiniz dağıtımı seçin.
1. Aşağıdaki sekmelerde güncelleştirmeler yapın:
   * **Hedef Durumu**
   * **Ölçümler** - tanımladığınız ölçümleri değiştirebilir veya silebilir veya yenilerini ekleyebilirsiniz.
   * **Etiketler**
   * **Modules**
   * **Yollar**
   * **Dağıtım**

1. **Kaydet'i**seçin.
1. Değişikliklerin çıkışını izlemek için [dağıtımı İzle'deki](#monitor-a-deployment) adımları izleyin.

## <a name="delete-a-deployment"></a>Dağıtımı silme

Bir dağıtımı sildiğinizde, dağıtılan aygıtlar bir sonraki en yüksek öncelikli dağıtımlarını alırlar. Aygıtlarınız başka bir dağıtımın hedef koşuluna uymuyorsa, dağıtım silindiğinde modüller kaldırılmaz.

1. [Azure portalında](https://portal.azure.com) oturum açın ve IoT Hub'ınıza gidin.
1. **IoT Edge'i**seçin.
1. **IoT Edge Dağıtımlar** sekmesini seçin.

   ![IoT Edge dağıtımlarını görüntüleme](./media/how-to-deploy-monitor/iot-edge-deployments.png)

1. Silmek istediğiniz dağıtımı seçmek için onay kutusunu kullanın.
1. **Sil**’i seçin.
1. Bir istem, bu eylemin bu dağıtımı siler ve tüm aygıtlar için önceki duruma geri döner.Bu, daha düşük önceliğe sahip bir dağıtımın uygulanacağı anlamına gelir.Başka bir dağıtım hedeflenmezse, hiçbir modül kaldırılmaz. Cihazınızdaki tüm modülleri kaldırmak istiyorsanız, sıfır modüliçeren bir dağıtım oluşturun ve aynı aygıtlara dağıtın.Devam etmek için **Evet'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Modülleri IoT Edge aygıtlarına dağıtma](module-deployment-monitoring.md)hakkında daha fazla bilgi edinin.
