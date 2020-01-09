---
title: Cihaz grupları - Azure IOT Edge için otomatik dağıtım | Microsoft Docs
description: Otomatik dağıtımlar, paylaşılan etiketlere göre cihaz grupları yönetmek için Azure IOT Edge'de kullanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/12/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 13390de8d3008907a0b55bf3a61c931dfdcd84e6
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552364"
---
# <a name="understand-iot-edge-automatic-deployments-for-single-devices-or-at-scale"></a>IOT Edge otomatik dağıtımlar tek tek cihazlarda veya uygun ölçekte anlama

Otomatik dağıtımlar ve katmanlı dağıtım, çok sayıda IoT Edge cihazda modülleri yönetmenize ve yapılandırmanıza yardımcı olur. 

Azure IoT Edge, modülleri IoT Edge cihazlarda çalışacak şekilde yapılandırmanın iki yolunu sağlar. İlk yöntem, modülleri cihaz başına temelinde dağıtmaktır. Bir dağıtım bildirimi oluşturun ve ardından ada göre belirli bir cihaza uygularsınız. İkinci yöntem, modülleri bir tanımlı koşullar kümesini karşılayan herhangi bir kayıtlı cihaza otomatik olarak dağıtmaktır. Bir dağıtım bildirimi oluşturup, cihaz ikizi [Etiketler](../iot-edge/how-to-deploy-monitor.md#identify-devices-using-tags) temelinde hangi cihazların uygulanacağını tanımlarsınız. 

Bu makalede yapılandırmasına odaklanılmıştır ve cihazların filolarına aşamaları izleme için IOT Edge otomatik dağıtımlar anılan. Genel dağıtım adımları aşağıdaki gibidir: 

1. Bir işleç modülleri, hem de hedef cihazlar kümesini tanımlayan bir dağıtım tanımlar. Her dağıtımda bu bilgileri yansıtan bir dağıtım bildirimi vardır. 
2. IOT Hub hizmeti, tüm hedeflenen cihazların istenen modülleriyle yapılandırılacakları ile iletişim kurar. 
3. IOT Hub hizmeti, IOT Edge cihazları alır ve işleci için kullanılabilir hale getirir.  Örneğin, bir operatör, bir sınır cihazının başarıyla yapılandırılmadığını veya çalışma zamanı sırasında bir modülün başarısız olup olmadığını görebilir. 
4. Herhangi bir zamanda hedefleme koşullara uyan IOT Edge cihazları yeni dağıtım için yapılandırılmış. 
 
Bu makalede, yapılandırma ve dağıtım izleme alan her bir bileşeni açıklar. Oluşturma ve dağıtımı güncelleştirme yönergeleri için bkz [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md).

## <a name="deployment"></a>Kurulum

IOT Edge otomatik dağıtım, IOT Edge modül görüntüleri hedeflenen bir IOT Edge cihazlarının örnekler olarak çalıştırmak için atar. Karşılık gelen başlatma parametreleri ile modüllerin listesini dahil etmek için bir IOT Edge dağıtımı bildirimi yapılandırarak çalışır. Bir dağıtım, tek bir cihaza (cihaz KIMLIĞINE göre) veya bir cihaz grubuna (etiketlere göre) atanabilir. Bir IoT Edge cihaz bir dağıtım bildirimi aldıktan sonra, kapsayıcı görüntülerini ilgili kapsayıcı depolarından indirir ve yükler ve bunları uygun şekilde yapılandırır. Dağıtım oluşturulduktan sonra, bir operatör hedeflenen cihazların doğru yapılandırılıp yapılandırılmadığını görmek için dağıtım durumunu izleyebilir.

IOT Edge cihazları yalnızca bir dağıtım ile yapılandırılabilir. Dağıtımı almadan önce aşağıdaki önkoşulların cihazda olmalıdır:

* Temel işletim sistemi
* Moby ya da Docker gibi bir kapsayıcı yönetim sistemi
* IOT Edge çalışma zamanı sağlama 

### <a name="deployment-manifest"></a>Dağıtım bildirimi

Bir dağıtım bildirimi hedeflenen IOT Edge cihazlarında yapılandırılması için modülleri açıklayan bir JSON belgesidir. Gerekli sistem modüllerine (özellikle IOT Edge aracısı ve IOT Edge hub'ı) dahil olmak üzere tüm modüller için yapılandırma meta verilerini içeriyor.  

Her bir modül için yapılandırma meta verilerini içerir: 

* Sürüm 
* Tür 
* Durum (örneğin, çalışıyor veya durduruldu) 
* Yeniden başlatma ilkesi 
* Görüntü ve kapsayıcı kayıt defteri
* Giriş ve çıkış veri yolları 

Bir özel kapsayıcı kayıt defteri modülü görüntüsü depolanırsa, IOT Edge aracısı kayıt defteri kimlik bilgilerini tutar. 

### <a name="target-condition"></a>Hedef koşul

Hedef koşul, dağıtımın kullanım ömrü boyunca sürekli olarak değerlendirilir. Gereksinimleri karşılayan yeni cihazları dahil edin ve artık yapan herhangi bir mevcut cihaza kaldırılır. Dağıtım Hizmeti herhangi bir hedef koşulu değişiklik algılarsa yeniden başlatılır. 

Örneğin, bir A hedef koşulu tags.environment dağıtımınız = 'prod'. Dağıtımı devre dışı yaslanıp, 10 üretim cihaz bulunur. Modüller, bu 10 cihazları başarıyla yüklenir. IOT Edge aracı durumu, toplam cihaz sayısı 10, 10 başarılı yanıtlar, 0 hata yanıtları ve 0 bekleyen yanıtlar gösterilir. Şimdi tags.environment ile beş daha fazla cihaz Ekle 'prod' =. Hizmet değişikliği algılar ve beş yeni cihazlara dağıtmak çalıştığında, IOT Edge aracı durumu 15 toplam cihaz sayısı, 10 başarılı yanıtlar, 0 hata yanıtları ve 5 bekleyen yanıtlar olur.

Herhangi bir Boolean koşul, hedef cihazları seçmek için cihaz ikizlerini etiketleri veya DeviceID kullanın. Etiketleri koşul kullanmak istiyorsanız, "tags" eklemeniz gerekir:{} cihaz ikizi özelliklerini aynı düzeyde altında bölümünde. [Cihaz ikizindeki etiket hakkında daha fazla bilgi edinin](../iot-hub/iot-hub-devguide-device-twins.md)

Hedef koşul örnekleri:

* DeviceID = 'linuxprod1'
* Tags.Environment = 'prod'
* Tags.Environment = 'prod' AND tags.location = 'westus'
* Tags.Environment = 'prod' veya tags.location = 'westus'
* Tags.operator 'John' = ve tags.environment = 'prod' değil DeviceID = 'linuxprod1'

Hedef koşul oluştururken bazı kısıtlar şunlardır:

* Cihaz ikizinde etiketleri veya DeviceID kullanarak bir hedef koşulu yalnızca oluşturabilirsiniz.
* Çift tırnak işareti, herhangi bir bölümünü hedef koşulu izin verilmez. Tek tırnak işareti kullanın.
* Tek tırnak, hedef koşulu değerlerini temsil eder. Bu nedenle, cihaz adı bir parçası ise tek tırnak işareti ile başka bir tek tırnak işareti kaçış gerekir. Örneğin, bir cihazı hedeflemeniz adlı `operator'sDevice`, yazma `deviceId='operator''sDevice'`.
* Sayı, harf ve şu karakterleri hedef koşulu değerlerde izin verilir: `-:.+%_#*?!(),=@;$`.

### <a name="priority"></a>Öncelik

Öncelikli bir dağıtım hedeflenen cihaza göre diğer dağıtımlar uygulanması gerektiğini tanımlar. Dağıtım ile daha büyük sayılar daha yüksek öncelikli belirten pozitif bir tamsayı önceliktir. IOT Edge cihazı birden fazla dağıtım tarafından hedeflendiğinde, en yüksek önceliğe sahip dağıtım uygulanır.  Düşük önceliklere sahip dağıtımlar uygulanmaz ve birleştirilirler.  Bir cihaz eşit önceliğe sahip iki veya daha fazla dağıtıma hedefleniyorsa, en son oluşturulan dağıtım (oluşturma zaman damgasıyla belirlenir) geçerlidir.

### <a name="labels"></a>Etiketler 

Etiketler, dağıtımları filtrelemek ve gruplamak için kullanabileceğiniz dize anahtar/değer çiftleridir. Bir dağıtımda birden fazla etiket olabilir. Etiketler isteğe bağlıdır ve IoT Edge cihazlarının gerçek yapılandırmasını etkilemez. 

### <a name="metrics"></a>Ölçümler

Varsayılan olarak, tüm dağıtımlar dört ölçüm üzerinde rapor alır:

* **Hedeflenen** , dağıtım hedefleme durumuyla eşleşen IoT Edge cihazları gösterir.
* **Uygulanan** , daha yüksek öncelikli bir dağıtım tarafından hedeflenilmemiş hedeflenen IoT Edge cihazları gösterir.
* **Raporlama başarısı** , yeniden hizmet olarak bildirilen ve modüllerin başarıyla dağıtıldığını bildiren IoT Edge cihazları gösterir. 
* **Raporlama hatası** , bir veya daha fazla modülün başarıyla dağıtılmadığını hizmete geri rapor veren IoT Edge cihazları gösterir. Daha fazla hata araştırmak, bu cihazlar için uzaktan bağlanma ve günlük dosyalarını görüntülemek için.

Ayrıca, dağıtımı izlemeye ve yönetmeye yardımcı olması için kendi özel ölçümlerinizi de tanımlayabilirsiniz. 

Ölçümler, cihazların bir dağıtım yapılandırması uygulamanın bir sonucu olarak yeniden rapor edebileceği çeşitli durumların Özet sayısını sağlar. Ölçümler, son istenen durum veya son bağlantı zamanı gibi [ikizi bildirilen özellikleri, Edgehub modülünü](module-edgeagent-edgehub.md#edgehub-reported-properties)sorgulayabilir. Örneğin: 

```sql
SELECT deviceId FROM devices
  WHERE properties.reported.lastDesiredStatus.code = 200
```

Kendi ölçümlerinizi eklemek isteğe bağlıdır ve IoT Edge cihazlarının gerçek yapılandırmasını etkilemez. 

## <a name="layered-deployment"></a>Katmanlı dağıtım

Katmanlı dağıtımlar, oluşturulması gereken benzersiz dağıtım sayısını azaltmak için birlikte birleştirilebilecek otomatik dağıtımlardır. Katmanlı dağıtımlar, aynı modüllerin birçok otomatik dağıtımda farklı birleşimlerde yeniden kullanıldığı senaryolarda faydalıdır. 

Katmanlı dağıtımlar, herhangi bir otomatik dağıtımla aynı temel bileşenlere sahiptir. Cihazları, cihaz ikklerindeki etiketlere göre hedefleyin ve Etiketler, ölçümler ve durum raporlama etrafında aynı işlevleri sağlar. Katmanlı dağıtımlar da bunlara atanmış öncelikler vardır, ancak bir cihaza hangi dağıtımın uygulanacağını belirleyen önceliği kullanmak yerine, bir cihaza birden çok dağıtımın nasıl derecelendirilir olduğunu belirler. Örneğin, iki katmanlı dağıtımda aynı ada sahip bir modül veya bir yol varsa, düşük önceliğin üzerine yazıldığında, yüksek önceliğe sahip katmanlı dağıtım uygulanır. 

Sistem çalışma zamanı modülleri, edgeAgent ve edgeHub, katmanlı dağıtımın bir parçası olarak yapılandırılmamıştır. Katmanlı bir dağıtım tarafından hedeflenen tüm IoT Edge cihazlara, önce katmanlı dağıtımların eklenebileceği temeli sağlamak üzere standart bir Otomatik dağıtıma uygulanmış olması gerekir. 

IoT Edge bir cihaz yalnızca bir standart otomatik dağıtım uygulayabilir, ancak birden çok katmanlı otomatik dağıtımlar uygulayabilir. Bir cihazın hedeflediği katmanlı dağıtımlar, bu cihaz için otomatik dağıtımdan daha yüksek önceliğe sahip olmalıdır. 

Örneğin, binaları yöneten bir şirketin aşağıdaki senaryosunu göz önünde bulundurun. Güvenlik kameraları, hareket sensörleri ve yükseltme 'lerden veri toplamak için IoT Edge modüller geliştirirler. Ancak, tüm binaları üç modülü de kullanamaz. Standart otomatik dağıtımlar sayesinde şirketin, binalarının ihtiyaç duyduğu tüm modül birleşimleri için bireysel dağıtımlar oluşturması gerekir. 

![Standart otomatik dağıtımların her modül birleşimine uygun olması gerekir](./media/module-deployment-monitoring/standard-deployment.png)

Ancak, şirket katmanlı Otomatik dağıtıma geçtiğinde, yönetmek üzere daha az dağıtım olan binaları için aynı modül kombinasyonlarını oluşturamazlar. Her modülün kendi katmanlı bir dağıtımı vardır ve cihaz etiketleri her oluşturmaya hangi modüllerin ekleneceğini belirler. 

![Katmanlı otomatik dağıtım, aynı modüllerin farklı yollarla birleştirileceği senaryoları basitleştirir](./media/module-deployment-monitoring/layered-deployment.png)

### <a name="module-twin-configuration"></a>Module ikizi yapılandırması

Katmanlı dağıtımlarla çalışırken, kasıtlı olarak veya başka bir şekilde, bir cihazı hedefleyen aynı modüle sahip iki dağıtıma sahip olabilirsiniz. Bu durumlarda, daha yüksek öncelikli dağıtımın ikizi modülünün üzerine yazıp yazmayacağını veya sonuna ekleme yapıp etmeyeceğine karar verebilirsiniz. Örneğin, 100 farklı cihaza aynı modülü uygulayan bir dağıtıma sahip olabilirsiniz. Ancak, bu cihazların 10 ' u güvenli tesislerde ve proxy sunucularıyla iletişim kurmak için ek yapılandırma gerekir. Bu 10 cihazın, temel dağıtımdan var olan modül ikizi bilgilerinin üzerine yazmadan güvenli bir şekilde iletişim kurmasına olanak tanıyan modül ikizi özellikleri eklemek için katmanlı bir dağıtım kullanabilirsiniz. 

Dağıtım bildiriminde Module ikizi istenen özellikleri ekleyebilirsiniz. Standart dağıtımda, bir katman ikizi Module, katmanlı bir dağıtımda istenen özelliklerin yeni bir alt kümesini **bildirebilirsiniz.** 

Örneğin, standart bir dağıtımda, sanal sıcaklık algılayıcı modülünü, verileri 5 saniyelik aralıklarla veri göndermesini söyleyen aşağıdaki istenen özelliklerle ekleyebilirsiniz:

```json
"SimulatedTemperatureSensor": {
  "properties.desired": {
    "SendData": true,
    "SendInterval": 5
  }
}
```

Aynı cihazları veya aynı cihazların bir alt kümesini hedefleyen katmanlı bir dağıtımda, sanal algılayıcının 1000 ileti göndermesini ve sonra durdurmasını belirten ek bir özellik eklemek isteyebilirsiniz. Varolan özelliklerin üzerine yazmak istemezsiniz, bu nedenle, istenen özellikler içinde yeni özelliği içeren `layeredProperties` adlı yeni bir bölüm oluşturursunuz:

```json
"SimulatedTemperatureSensor": {
  "properties.desired.layeredProperties": {
    "StopAfterCount": 1000
  }
}
```

Her iki dağıtıma de uygulanmış bir cihaz, sanal sıcaklık algılayıcısı için ikizi modülünde aşağıdaki işlemi yansıtır: 

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

İkizi modülünün `properties.desired` alanını katmanlı bir dağıtımda ayarlarsanız, daha düşük öncelikli dağıtımlarda Bu modülün istenen özelliklerinin üzerine yazar. 

## <a name="phased-rollout"></a>Aşamalı dağıtımı 

Aşamalı yapabildiği değişiklikleri operatörün insanın ufkunu genişleten bir IOT Edge cihazları kümesine dağıtır, genel bir işlemdir. Aşamalı olarak geniş ölçek bozucu değişiklikler yapma riskini azaltmak için değişiklik olmaktır. Otomatik dağıtımlar, IoT Edge cihazları arasında aşamalı piyasaya çıkarma yönetimine yardımcı olur. 

Aşamalı aşağıdaki aşamaları ve adımları çalıştırılır: 

1. Bunları sağlama ve cihaz ikizi etiketi gibi ayarlayarak IOT Edge cihazları, bir test ortamı kurmak `tag.environment='test'`. Test ortamı, dağıtımın sonunda hedeflenecek üretim ortamını yansıtmalıdır. 
2. İstenen modülleri ve yapılandırmaları da dahil olmak üzere bir dağıtım oluşturun. IOT Edge cihazı ortam test hedefleme koşul hedeflemelidir.   
3. Yeni modül yapılandırması test ortamında doğrulayın.
4. Dağıtım hedefleme koşula yeni bir etiket ekleyerek bir alt kümesini üretim IOT Edge cihazları içerecek şekilde güncelleştirin. Ayrıca, dağıtım önceliğini şu anda bu cihazları hedefleyen diğer dağıtımlar daha yüksek olduğundan emin olun 
5. Dağıtım durumu görüntüleyerek dağıtım hedeflenen IOT Cihazlarında başarılı olduğunu doğrulayın.
6. Kalan tüm üretim IOT Edge cihazları hedeflemek için dağıtım güncelleştirin.

## <a name="rollback"></a>Geri alma

Bir hata veya yanlış yapılandırmalarını geri alırsanız, dağıtımları alınabilir. Bir dağıtım, bir IoT Edge cihazının mutlak modül yapılandırmasını tanımladığından, hedef tüm modülleri kaldırsa bile, ek bir dağıtımın aynı cihaza daha düşük bir önceliğe de hedeflenmiş olması gerekir.  

Dağıtım silindiğinde, modüller hedeflenen cihazlardan kaldırılmaz. Boş bir dağıtım olsa bile, cihazlar için yeni bir yapılandırma tanımlayan başka bir dağıtım olması gerekir. 

Geri alma işlemleri, aşağıdaki sırayla gerçekleştirin: 

1. İkinci bir dağıtım aynı cihazı kümesinin hedeflenir onaylayın. Geri alma amacı, tüm modülleri kaldırmak için ise, ikinci dağıtımı modüllerin içermemelidir. 
2. Değiştirin veya hedef koşul ifadesi, böylece cihazlar artık hedefleme koşula uyan geri almak istediğiniz dağıtımın kaldırın.
3. Dağıtım durumunu görüntüleyerek geri alma başarılı olduğunu doğrulayın.
   * Toplu geri dağıtım durumu geri alındı cihazlar için artık göstermelidir.
   * İkinci dağıtımı artık geri alındı cihazlar için dağıtım durumunu içermelidir.


## <a name="next-steps"></a>Sonraki adımlar

* Oluşturmak, güncelleştirmek veya bir dağıtımda silmek için adımlarında yol [dağıtma ve izleme uygun ölçekte IOT Edge modülleri](how-to-deploy-monitor.md).
* Gibi diğer IOT Edge kavramları hakkında daha fazla bilgi [IOT Edge çalışma zamanı](iot-edge-runtime.md) ve [IOT Edge modülleri](iot-edge-modules.md).

