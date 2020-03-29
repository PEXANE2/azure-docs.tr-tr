---
title: Aygıt grupları için otomatik dağıtım - Azure IoT Edge | Microsoft Dokümanlar
description: Paylaşılan etiketlere dayalı aygıt gruplarını yönetmek için Azure IoT Edge'de otomatik dağıtımları kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/30/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 8aaac6100ba980301ff3e85a3ac3959bfee89b49
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76895972"
---
# <a name="understand-iot-edge-automatic-deployments-for-single-devices-or-at-scale"></a>Tek cihazlar için veya ölçekte IoT Edge otomatik dağıtımları anlayın

Otomatik dağıtımlar ve katmanlı dağıtım, çok sayıda IoT Edge aygıtındaki modülleri yönetmenize ve yapılandırmanıza yardımcı olur.

Azure IoT Edge, modülleri IoT Edge aygıtlarında çalışacak şekilde yapılandırmanın iki yolunu sağlar. İlk yöntem, modülleri cihaz başına dağıtmaktır. Bir dağıtım bildirimi oluşturun ve ardından belirli bir aygıta ada göre uygularsınız. İkinci yöntem, modülleri tanımlanan koşullar kümesini karşılayan kayıtlı herhangi bir aygıta otomatik olarak dağıtmaktır. Bir dağıtım bildirimi oluşturur sunuz ve ardından aygıt ikizindeki [etiketlere](../iot-edge/how-to-deploy-monitor.md#identify-devices-using-tags) göre hangi aygıtlara uygulandığını tanımlarsınız.

Bu makalede, topluca *IoT Edge otomatik dağıtımları*olarak adlandırılan aygıt filolarının yapılandırılması ve izlenmesi üzerinde duruluyor.Temel dağıtım adımları aşağıdaki gibidir:

1. İşleç, bir modül kümesini ve hedef aygıtları açıklayan bir dağıtım tanımlar.Her dağıtım, bu bilgileri yansıtan bir dağıtım bildirimine sahiptir.
2. IoT Hub hizmeti, bildirilen modüllerle yapılandırmak için tüm hedeflenen aygıtlarla iletişim kurar.
3. IoT Hub hizmeti, IoT Edge aygıtlarından durumu alır ve operatör tarafından kullanılabilir hale getirir.Örneğin, bir operatör Edge aygıtının başarıyla yapılandırılmadığını veya bir modülün çalışma zamanında arızalı olup olmadığını görebilir.
4. İstediğinzaman, hedefleme koşullarını karşılayan yeni IoT Edge aygıtları dağıtım için yapılandırılır.

Bu makalede, bir dağıtımı yapılandırma ve izlemeyle ilgili her bileşen açıklanmaktadır. Dağıtım oluşturma ve güncelleme için Bkz. [Dağıtım ve Ölçekte IoT Edge modüllerini izleyin.](how-to-deploy-monitor.md)

## <a name="deployment"></a>Dağıtım

IoT Edge otomatik dağıtım, hedeflenen IoT Edge aygıtları kümesinde örnek olarak çalışması için IoT Edge modül görüntülerini atar. Bir IoT Edge dağıtım bildirimini, ilgili başlatma parametrelerine sahip modüllerin listesini içerecek şekilde yapılandırarak çalışır.Dağıtım, tek bir aygıta (Aygıt Kimliği'ne dayalı olarak) veya bir aygıt grubuna (etiketlere dayalı) atanabilir.Bir IoT Edge aygıtı bir dağıtım bildirimi aldığında, kapsayıcı görüntülerini ilgili kapsayıcı depolarından karşıdan yükler ve buna göre yapılandırır.Dağıtım oluşturulduktan sonra, bir operatör hedeflenen aygıtların doğru şekilde yapılandırılıp yapılandırılmadığını görmek için dağıtım durumunu izleyebilir.

Yalnızca IoT Edge aygıtları dağıtımla yapılandırılabilir. Dağıtıma geçebilmek için aşağıdaki ön koşullar aygıtta olmalıdır:

* Temel işletim sistemi
* Moby veya Docker gibi bir konteyner yönetim sistemi
* IoT Edge çalışma zamanının sağlanması

### <a name="deployment-manifest"></a>Dağıtım bildirimi

Dağıtım bildirimi, hedeflenen IoT Edge aygıtlarında yapılandırılacak modülleri açıklayan bir JSON belgesidir. Gerekli sistem modülleri (özellikle IoT Edge aracısı ve IoT Edge hub' ı) dahil olmak üzere tüm modüller için yapılandırma meta verilerini içerir.  

Her modül için yapılandırma meta verileri şunları içerir:

* Sürüm
* Tür
* Durum (örneğin, çalışan veya durdurulmuş)
* Yeniden başlatma ilkesi
* Resim ve konteyner kayıt defteri
* Veri giriş ve çıktı yolları

Modül görüntüsü özel bir kapsayıcı kayıt defterinde depolanırsa, IoT Edge aracısı kayıt defteri kimlik bilgilerini tutar.

### <a name="target-condition"></a>Hedef koşul

Hedef koşul, dağıtım ömrü boyunca sürekli olarak değerlendirilir. Gereksinimleri karşılayan tüm yeni aygıtlar dahildir ve artık olmayan varolan aygıtlar kaldırılır. Hizmet herhangi bir hedef koşul değişikliği algılarsa dağıtım yeniden etkinleştirilir.

Örneğin, hedef koşul etiketleri.environment = 'prod' olan bir dağıtım var. Dağıtımı başlattığınızda, 10 üretim aygıtı vardır. Modüller bu 10 cihaza başarıyla yüklenir. IoT Edge aracısı durumu 10 toplam aygıt, 10 başarılı yanıt, 0 hata yanıtları ve 0 bekleyen yanıtgösterir. Şimdi tags.environment = 'prod' ile beş aygıt daha ekleyin. Hizmet değişikliği algılar ve IoT Edge aracısı durumu, beş yeni aygıta dağıtılırken 15 toplam aygıt, 10 başarılı yanıt, 0 hata yanıtı ve 5 bekleyen yanıt olur.

Hedef aygıtları seçmek için aygıt ikizi etiketleri, aygıt ikizi bildirilen özellikler veya deviceId'deki herhangi bir Boolean koşulunu kullanın. Koşul'u etiketlerle kullanmak istiyorsanız, "etiketler" eklemeniz gerekir:{} aygıttaki ikiz bölüm özellikleriyle aynı düzeydedir. [Aygıt ikizindeki etiketler hakkında daha fazla bilgi edinin](../iot-hub/iot-hub-devguide-device-twins.md)

Hedef koşullara örnekler:

* deviceId ='linuxprod1'
* tags.environment ='prod'
* tags.environment = 'prod' VE tags.location = 'westus'
* tags.environment = 'prod' OR tags.location = 'westus'
* tags.operator = 'John' VE tags.environment = 'prod' NOT deviceId = 'linuxprod1'
* properties.reported.devicemodel = '4000x'

Bir hedef koşul oluşturuyorsanız bu kısıtlamaları göz önünde bulundurun:

* Aygıt ikizinde, yalnızca etiketleri, bildirilen özellikleri veya deviceId'i kullanarak bir hedef koşulu oluşturabilirsiniz.
* Hedef koşulun herhangi bir bölümünde çift tırnak işaretine izin verilmez. Tek tırnak işaretleri kullanın.
* Tek tırnak işaretleri hedef koşulun değerlerini temsil ediyor. Bu nedenle, aygıt adının bir parçasıysa, tek bir tekliften başka bir teklifle kaçmalısınız. Örneğin, adlı `operator'sDevice`bir aygıtı hedeflemek için , yazın. `deviceId='operator''sDevice'`
* Sayılar, harfler ve aşağıdaki karakterler hedef koşul değerlerinde izin verilir: `-:.+%_#*?!(),=@;$`.

### <a name="priority"></a>Öncelik

Öncelik, diğer dağıtımlara göre hedeflenen aygıta dağıtım uygulanıp uygulanmayacağını tanımlar. Dağıtım önceliği, daha büyük sayıların daha yüksek önceliği gösteren pozitif bir tamsayıdır. Bir IoT Edge aygıtı birden fazla dağıtım tarafından hedeflenirse, en yüksek önceliğe sahip dağıtım geçerlidir.Daha düşük önceliklere sahip dağıtımlar uygulanmaz ve birleştirilmeyecektir.Bir aygıt eşit önceliğe sahip iki veya daha fazla dağıtımla hedeflenirse, en son oluşturulan dağıtım (oluşturma zaman damgası tarafından belirlenir) uygulanır.

### <a name="labels"></a>Etiketler

Etiketler, dağıtımları filtrelemek ve gruplandırmak için kullanabileceğiniz dize anahtar/değer çiftleridir.Dağıtımda birden çok etiket olabilir. Etiketler isteğe bağlıdır ve IoT Edge aygıtlarının gerçek yapılandırmasını etkilemez.

### <a name="metrics"></a>Ölçümler

Varsayılan olarak, tüm dağıtımlar dört ölçüm üzerinde rapor:

* **Hedeflenen,** Dağıtım hedefleme koşuluyla eşleşen IoT Edge aygıtlarını gösterir.
* **Uygulanan,** daha yüksek öncelikli başka bir dağıtım tarafından hedeflenmeyen hedeflenen IoT Edge aygıtlarını gösterir.
* **Raporlama Başarısı,** modüllerin başarıyla dağıtıldığını bildiren IoT Edge aygıtlarını gösterir.
* **Raporlama Hatası,** bir veya daha fazla modülün başarıyla dağıtılmadığı bildirilen IoT Edge aygıtlarını gösterir. Hatayı daha fazla araştırmak için, bu aygıtlara uzaktan bağlanın ve günlük dosyalarını görüntüleyin.

Ayrıca, dağıtımı izlemeye ve yönetmeye yardımcı olmak için kendi özel ölçümlerinizi tanımlayabilirsiniz.

Ölçümler, aygıtların dağıtım yapılandırması uygulaması sonucunda geri bildirebileceği çeşitli durumların özet sayımlarını sağlar. Ölçümler [edgeHub modülü ikiz bildirilen özellikleri](module-edgeagent-edgehub.md#edgehub-reported-properties)sorgulayabilirsiniz , *lastDesiredStatus* veya *lastConnectTime*gibi . Örnek:

```sql
SELECT deviceId FROM devices
  WHERE properties.reported.lastDesiredStatus.code = 200
```

Kendi ölçümlerinizi eklemek isteğe bağlıdır ve IoT Edge aygıtlarının gerçek yapılandırmasını etkilemez.

## <a name="layered-deployment"></a>Katmanlı dağıtım

Katmanlı dağıtımlar, oluşturulması gereken benzersiz dağıtım sayısını azaltmak için biraraya getirilebilen otomatik dağıtımlardır. Katmanlı dağıtımlar, aynı modüllerin birçok otomatik dağıtımda farklı kombinasyonlarda yeniden kullanıldığı senaryolarda yararlıdır.

Katmanlı dağıtımlar, tüm otomatik dağıtımlarla aynı temel bileşenlere sahiptir. Aygıtikizleri üzerindeki etiketlere göre cihazları hedefler ve etiketler, ölçümler ve durum raporlaması etrafında aynı işlevselliği sağlarlar. Katmanlı dağıtımların öncelikleri de vardır, ancak aygıta hangi dağıtımın uygulandığını belirlemek için önceliği kullanmak yerine, öncelik bir aygıtta birden çok dağıtımın nasıl sıralanır olduğunu belirler. Örneğin, iki katmanlı dağıtımda aynı ada sahip bir modül veya rota varsa, daha yüksek önceliğe sahip katmanlı dağıtım, alt önceliğe göre yazılırken uygulanır.

Sistem çalışma zamanı modülleri, edgeAgent ve edgeHub, katmanlı dağıtımın bir parçası olarak yapılandırılmamıştır. Katmanlı dağıtım tarafından hedeflenen herhangi bir IoT Edge aygıtının önce standart bir otomatik dağıtıma ihtiyacı vardır. Otomatik dağıtım, katmanlı dağıtımların eklenebileceği tabanı sağlar.

Bir IoT Edge aygıtı bir ve yalnızca bir standart otomatik dağıtım uygulayabilir, ancak birden çok katmanlı otomatik dağıtım uygulayabilir. Bir aygıtı hedefleyen katmanlı dağıtımların, o aygıtın otomatik dağıtımından daha yüksek bir önceliğe sahip olması gerekir.

Örneğin, binaları yöneten bir şirketin aşağıdaki senaryosunu göz önünde bulundurun. Güvenlik kameralarından, hareket sensörlerinden ve asansörlerden veri toplamak için IoT Edge modülleri geliştirdiler. Ancak, tüm binaları her üç modülleri kullanabilirsiniz. Standart otomatik dağıtımlarla, şirketin binalarının ihtiyaç duyduğu tüm modül kombinasyonları için ayrı ayrı dağıtımlar oluşturması gerekir.

![Standart otomatik dağıtımlar her modül kombinasyonuna uyum sağlamalı](./media/module-deployment-monitoring/standard-deployment.png)

Ancak, şirket katmanlı otomatik dağıtımlara geçtikten sonra, yönetebilecekleri daha az dağıtım olan binaları için aynı modül kombinasyonlarını oluşturabileceklerini fark ederler. Her modülün kendi katmanlı dağıtımı vardır ve aygıt etiketleri her binaya hangi modüllerin eklendiğine yönelik tanımlar.

![Katmanlı otomatik dağıtımlar, aynı modüllerin farklı şekillerde birleştirildiği senaryoları basitleştirir](./media/module-deployment-monitoring/layered-deployment.png)

### <a name="module-twin-configuration"></a>Modül ikiz yapılandırması

Katmanlı dağıtımlarla çalışırken, kasıtlı olarak veya başka bir şekilde, aynı modüle sahip bir aygıtı hedefleyen iki dağıtıma sahip olabilirsiniz. Bu gibi durumlarda, daha yüksek öncelikli dağıtım ın modülün üzerine mi yoksa buna ek mi yazılması gerektiğine karar verebilirsiniz. Örneğin, aynı modülü 100 farklı aygıta uygulayan bir dağıtımınız olabilir. Ancak, bu aygıtlardan 10'u güvenli tesislerdedir ve proxy sunucuları aracılığıyla iletişim kurmak için ek yapılandırmaya ihtiyaç duyar. Bu 10 aygıtın temel dağıtımdan varolan modül ikiz bilgilerini üzerine yazmadan güvenli bir şekilde iletişim kurmasını sağlayan modül ikiz özellikleri eklemek için katmanlı bir dağıtım kullanabilirsiniz.

Dağıtım bildiriminde modül ikiz istenilen özellikleri ekleyebilirsiniz. Standart bir **dağıtımda,** modül ikizinin istenilen bölümüne özellikler eklerseniz, katmanlı bir dağıtımda istenen özelliklerin yeni bir alt kümesini bildirebilirsiniz.

Örneğin, standart bir dağıtımda, 5 saniyelik aralıklarla veri göndermesini söyleyen aşağıdaki istenilen özelliklere sahip simüle edilmiş sıcaklık sensörü modülünü ekleyebilirsiniz:

```json
"SimulatedTemperatureSensor": {
  "properties.desired": {
    "SendData": true,
    "SendInterval": 5
  }
}
```

Aynı aygıtların bazılarını veya tümünü hedefleyen katmanlı bir dağıtımda, benzetilen sensöre 1000 ileti göndermesini ve ardından durmasını söyleyen bir özellik ekleyebilirsiniz. Varolan özelliklerin üzerine yazmak istemezsiniz, bu nedenle istenen özellikler içinde `layeredProperties`yeni bir bölüm oluşturursunuz, yeni özelliği içerir:

```json
"SimulatedTemperatureSensor": {
  "properties.desired.layeredProperties": {
    "StopAfterCount": 1000
  }
}
```

Her iki dağıtım uygulanan bir aygıt, simüle edilmiş sıcaklık sensörü için modül ikizinde aşağıdaki özellikleri yansıtır:

```json
"properties": {
  "desired": {
    "SendData": true,
    "SendInterval": 5,
    "layeredProperties": {
      "StopAfterCount": 1000
    }
  }
}
```

Modül ikizinin `properties.desired` alanını katmanlı bir dağıtımda ayarlarsanız, daha düşük öncelikli dağıtımlarda bu modül için istenen özelliklerin üzerine yazılır.

## <a name="phased-rollout"></a>Aşamalı kullanıma sunulması

Aşamalı kullanıma alma, bir işleç tarafından ioT Edge aygıtlarının genişletilmesi kümesine değişiklikler dağıttığı genel bir işlemdir. Amaç, geniş ölçekli kırma değişiklikleri yapma riskini azaltmak için kademeli olarak değişiklik yapmaktır. Otomatik dağıtımlar, bir IoT Edge aygıtı filosunda aşamalı kullanıma sunulmasının yönetilmesine yardımcı olur.

Aşamalı bir rollout aşağıdaki aşamalar ve adımlar yürütülür:

1. IoT Edge aygıtlarının bir test ortamı nı oluşturarak ve `tag.environment='test'`bir aygıt ikiz etiketi ni ayarlayarak .Sınama ortamı, dağıtımın sonunda hedefalacağı üretim ortamını yansıtmalıdır.
2. İstenilen modülleri ve yapılandırmaları içeren bir dağıtım oluşturun. Hedefleme koşulu, test IoT Edge aygıt ortamını hedeflemelidir.
3. Test ortamındaki yeni modül yapılandırmasını doğrulayın.
4. Hedefleme durumuna yeni bir etiket ekleyerek üretim IoT Edge aygıtlarının bir alt kümesini içerecek şekilde dağıtımı güncelleştirin. Ayrıca, dağıtım önceliğinin şu anda bu aygıtlara hedeflenen diğer dağıtımlardan daha yüksek olduğundan emin olun
5. Dağıtım durumunu görüntüleyerek dağıtımın hedeflenen IoT Aygıtlarında başarılı olduğunu doğrulayın.
6. Kalan tüm üretim IoT Edge aygıtlarını hedeflemek için dağıtımı güncelleştirin.

## <a name="rollback"></a>Geri alma

Hatalar veya yanlış yapılandırmalar alırsanız dağıtımlar geri alınabilir.Dağıtım, bir IoT Edge aygıtıiçin mutlak modül yapılandırmasını tanımladığından, hedef tüm modülleri kaldırmak olsa bile, ek bir dağıtımın aynı aygıta daha düşük bir öncelikle hedefleştirilmesi gerekir.  

Dağıtımın silmesi, modülleri hedeflenen aygıtlardan kaldırmaz. Boş bir dağıtım olsa bile aygıtlar için yeni bir yapılandırma tanımlayan başka bir dağıtım olmalıdır.

Aşağıdaki sırayla geri alma gerçekleştirin:

1. İkinci bir dağıtımın da aynı aygıt kümesini hedeflediğini doğrulayın. Geri alma nın amacı tüm modülleri kaldırmaksa, ikinci dağıtım herhangi bir modül içermemelidir.
2. Aygıtların artık hedefleme koşuluna uymaması için geri almak istediğiniz dağıtımın hedef koşulu ifadesini değiştirin veya kaldırın.
3. Geri alma durumunu görüntüleyerek geri alma nın başarılı olduğunu doğrulayın.
   * Geri alma dağıtımı artık geri alındı aygıtlar için durum göstermemelidir.
   * İkinci dağıtım şimdi geri alındı aygıtlar için dağıtım durumunu içermelidir.

## <a name="next-steps"></a>Sonraki adımlar

* Dağıtım'da bir dağıtım oluşturmak, güncelleştirmek veya silmek [ve IoT Edge modüllerini ölçekte izlemek](how-to-deploy-monitor.md)için adımları gözden geçirin.
* [IoT Edge çalışma zamanı](iot-edge-runtime.md) ve [IoT Edge modülleri](iot-edge-modules.md)gibi diğer IoT Edge kavramları hakkında daha fazla bilgi edinin.
