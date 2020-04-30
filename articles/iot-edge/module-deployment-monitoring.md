---
title: Cihaz grupları için otomatik dağıtım-Azure IoT Edge | Microsoft Docs
description: Paylaşılan etiketlere göre cihaz gruplarını yönetmek için Azure IoT Edge otomatik dağıtımları kullanın
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/30/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 7540c5a82220eef61b8f1cf470697315496cd6bf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82127599"
---
# <a name="understand-iot-edge-automatic-deployments-for-single-devices-or-at-scale"></a>Tek cihazların veya ölçekteki otomatik dağıtımları IoT Edge anlayın

Otomatik dağıtımlar ve katmanlı dağıtım, çok sayıda IoT Edge cihazda modülleri yönetmenize ve yapılandırmanıza yardımcı olur.

Azure IoT Edge, modülleri IoT Edge cihazlarda çalışacak şekilde yapılandırmanın iki yolunu sağlar. İlk yöntem, modülleri cihaz başına temelinde dağıtmaktır. Bir dağıtım bildirimi oluşturun ve ardından ada göre belirli bir cihaza uygularsınız. İkinci yöntem, modülleri bir tanımlı koşullar kümesini karşılayan herhangi bir kayıtlı cihaza otomatik olarak dağıtmaktır. Bir dağıtım bildirimi oluşturup, cihaz ikizi [Etiketler](../iot-edge/how-to-deploy-at-scale.md#identify-devices-using-tags) temelinde hangi cihazların uygulanacağını tanımlarsınız.

Bu makale, toplu olarak *IoT Edge otomatik dağıtımlar*olarak adlandırılan cihazları yapılandırma ve izleme konusunda odaklanır.Temel dağıtım adımları aşağıdaki gibidir:

1. İşleci bir modül kümesini ve hedef cihazları açıklayan bir dağıtımı tanımlar.Her dağıtımda bu bilgileri yansıtan bir dağıtım bildirimi vardır.
2. IoT Hub hizmeti, bunları belirtilen modüllerle yapılandırmak için hedeflenen tüm cihazlarla iletişim kurar.
3. IoT Hub hizmeti, IoT Edge cihazlarından durumu alır ve işleç için kullanılabilir hale getirir.Örneğin, bir operatör, bir sınır cihazının ne zaman başarıyla yapılandırılmadığı veya bir modülün çalışma zamanında başarısız olup olmadığını görebilir.
4. Herhangi bir zamanda, hedeflenen koşullara uyan yeni IoT Edge cihazlar dağıtım için yapılandırılır.

Bu makalede, bir dağıtımı yapılandırma ve izleme ile ilgili her bir bileşen açıklanmaktadır. Dağıtım oluşturma ve güncelleştirme hakkında yönergeler için bkz. [IoT Edge modüllerini ölçekli olarak dağıtma ve izleme](how-to-deploy-at-scale.md).

## <a name="deployment"></a>Dağıtım

IoT Edge otomatik dağıtım, IoT Edge modül görüntülerini hedeflenen bir IoT Edge cihaz kümesi üzerinde örnek olarak çalışacak şekilde atar. Bir IoT Edge dağıtım bildirimini, karşılık gelen başlatma parametrelerine sahip modüllerin listesini içerecek şekilde yapılandırarak işe yarar.Bir dağıtım, tek bir cihaza (cihaz KIMLIĞINE göre) veya bir cihaz grubuna (etiketlere göre) atanabilir.Bir IoT Edge cihaz bir dağıtım bildirimi aldıktan sonra, kapsayıcı görüntülerini ilgili kapsayıcı depolarından indirir ve yükler ve bunları uygun şekilde yapılandırır.Dağıtım oluşturulduktan sonra, bir operatör hedeflenen cihazların doğru yapılandırılıp yapılandırılmadığını görmek için dağıtım durumunu izleyebilir.

Yalnızca IoT Edge cihazları bir dağıtım ile yapılandırılabilir. Dağıtımı alabilmesi için önce aşağıdaki önkoşulların cihazda olması gerekir:

* Temel işletim sistemi
* Moby veya Docker gibi bir kapsayıcı yönetim sistemi
* IoT Edge çalışma zamanının sağlanması

### <a name="deployment-manifest"></a>Dağıtım bildirimi

Dağıtım bildirimi, hedeflenen IoT Edge cihazlarda yapılandırılacak modülleri açıklayan bir JSON belgesidir. Gerekli sistem modülleri (özellikle de IoT Edge Aracısı ve IoT Edge hub) dahil olmak üzere tüm modüllerle ilgili yapılandırma meta verilerini içerir.  

Her modülün yapılandırma meta verileri şunları içerir:

* Sürüm
* Tür
* Durum (örneğin, çalışıyor veya durduruldu)
* Yeniden başlatma ilkesi
* Görüntü ve kapsayıcı kayıt defteri
* Veri girişi ve çıkışı için rotalar

Modül görüntüsü bir özel kapsayıcı kayıt defterinde depolanıyorsa, IoT Edge Aracısı kayıt defteri kimlik bilgilerini barındırır.

### <a name="target-condition"></a>Hedef koşul

Hedef koşul, dağıtımın kullanım ömrü boyunca sürekli olarak değerlendirilir. Gereksinimleri karşılayan tüm yeni cihazlar dahil edilir ve artık olmayan mevcut cihazlar kaldırılır. Hizmet herhangi bir hedef koşul değişikliğini algıladığında dağıtım yeniden etkinleştirilir.

Örneğin, hedef koşul etiketleriyle bir dağıtımınız vardır. ortam = ' prod '. Dağıtımı başlattığınızda 10 üretim cihazı vardır. Modüller bu 10 cihaza başarıyla yüklenir. IoT Edge Aracısı durumu 10 toplam cihaz, 10 başarılı yanıt, 0 hata yanıtı ve 0 bekleyen yanıt gösterir. Artık etiketlere sahip beş cihaz ekleyin. ortam = ' prod '. Hizmet değişikliği algılar ve IoT Edge Aracı durumu 15 toplam cihaz, 10 başarılı yanıt, 0 hata yanıtı ve beş yeni cihaza dağıtırken 5 bekleyen yanıt olur.

Hedef cihazları seçmek için Device ikizi etiketlerindeki herhangi bir Boole koşulunu, Device ikizi bildirilen özellikleri veya DeviceID 'yi kullanın. Koşulu etiketleriyle birlikte kullanmak istiyorsanız, Device ikizi 'ın özellikler ile aynı düzey altında "Etiketler"{} : bölümüne eklemeniz gerekir. [Cihaz ikizi Etiketler hakkında daha fazla bilgi edinin](../iot-hub/iot-hub-devguide-device-twins.md)

Hedef koşulları örnekleri:

* DeviceID = ' linuxprod1 '
* Tags. Environment = ' prod '
* Tags. Environment = ' prod ' ve Tags. Location = ' westus '
* Tags. Environment = ' prod ' veya Tags. Location = ' westus '
* Tags. operator = ' John ' ve Tags. Environment = ' prod ' NOT DeviceID = ' linuxprod1 '
* Properties. bildirilen. devicemodel = ' 4000x '

Bir hedef koşul oluştururken bu kısıtlamaları göz önünde bulundurun:

* Device ikizi 'da, yalnızca etiketleri, bildirilen özellikleri veya DeviceID 'yi kullanarak bir hedef koşul oluşturabilirsiniz.
* Hedef koşulun herhangi bir bölümünde çift tırnak işaretlerine izin verilmez. Tek tırnakları kullanın.
* Tek tırnak işaretleri, hedef koşulun değerlerini temsil eder. Bu nedenle, cihaz adının bir parçası ise, tek tırnağa başka bir teklifle kaçış yapmanız gerekir. Örneğin, adlı `operator'sDevice`bir cihazı hedeflemek için yazın `deviceId='operator''sDevice'`.
* Hedef koşul değerlerinde sayılar, harfler ve aşağıdaki karakterlere izin verilir: `-:.+%_#*?!(),=@;$`.

### <a name="priority"></a>Öncelik

Öncelik, bir dağıtımın, diğer dağıtımlara göre hedeflenen bir cihaza uygulanıp uygulanmayacağını tanımlar. Dağıtım önceliği, daha büyük olan ve daha yüksek önceliğe sahip pozitif bir tamsayıdır. IoT Edge bir cihaz birden fazla dağıtım tarafından hedeflenirse, en yüksek önceliğe sahip dağıtım geçerlidir.Düşük önceliklere sahip dağıtımlar uygulanmaz ve birleştirilirler.Bir cihaz eşit önceliğe sahip iki veya daha fazla dağıtıma hedefleniyorsa, en son oluşturulan dağıtım (oluşturma zaman damgasıyla belirlenir) geçerlidir.

### <a name="labels"></a>Etiketler

Etiketler, dağıtımları filtrelemek ve gruplamak için kullanabileceğiniz dize anahtar/değer çiftleridir.Bir dağıtımda birden fazla etiket olabilir. Etiketler isteğe bağlıdır ve IoT Edge cihazlarının gerçek yapılandırmasını etkilemez.

### <a name="metrics"></a>Ölçümler

Varsayılan olarak, tüm dağıtımlar dört ölçüm üzerinde rapor alır:

* **Hedeflenen** , dağıtım hedefleme durumuyla eşleşen IoT Edge cihazları gösterir.
* **Uygulanan** , daha yüksek öncelikli bir dağıtım tarafından hedeflenilmemiş hedeflenen IoT Edge cihazları gösterir.
* **Raporlama başarısı** , modüllerin başarıyla dağıtıldığını bildiren IoT Edge cihazları gösterir.
* **Raporlama hatası** , bir veya daha fazla modülün başarıyla dağıtılmadığını bildiren IoT Edge cihazları gösterir. Hatayı daha fazla araştırmak için, bu cihazlara uzaktan bağlanın ve günlük dosyalarını görüntüleyin.

Ayrıca, dağıtımı izlemeye ve yönetmeye yardımcı olması için kendi özel ölçümlerinizi de tanımlayabilirsiniz.

Ölçümler, cihazların bir dağıtım yapılandırması uygulamanın bir sonucu olarak yeniden rapor edebileceği çeşitli durumların Özet sayısını sağlar. Ölçümler, [ikizi bildirilen özelliklerini](module-edgeagent-edgehub.md#edgehub-reported-properties)( *Lastdesiredstatus* veya *lastconnecttime*gibi) sorgulayabilirler. Örneğin:

```sql
SELECT deviceId FROM devices
  WHERE properties.reported.lastDesiredStatus.code = 200
```

Kendi ölçümlerinizi eklemek isteğe bağlıdır ve IoT Edge cihazlarının gerçek yapılandırmasını etkilemez.

## <a name="layered-deployment"></a>Katmanlı dağıtım

Katmanlı dağıtımlar, oluşturulması gereken benzersiz dağıtım sayısını azaltmak için birlikte birleştirilebilecek otomatik dağıtımlardır. Katmanlı dağıtımlar, aynı modüllerin birçok otomatik dağıtımda farklı birleşimlerde yeniden kullanıldığı senaryolarda faydalıdır.

Katmanlı dağıtımlar, herhangi bir otomatik dağıtımla aynı temel bileşenlere sahiptir. Cihazları, cihaz ikklerindeki etiketlere göre hedefleyin ve Etiketler, ölçümler ve durum raporlama etrafında aynı işlevleri sağlar. Katmanlı dağıtımlar da bunlara atanmış öncelikler vardır, ancak bir cihaza hangi dağıtımın uygulanacağını belirleyen önceliği kullanmak yerine, bir cihaza birden çok dağıtımın nasıl derecelendirilir olduğunu belirler. Örneğin, iki katmanlı dağıtımda aynı ada sahip bir modül veya bir yol varsa, düşük önceliğin üzerine yazıldığında, yüksek önceliğe sahip katmanlı dağıtım uygulanır.

Sistem çalışma zamanı modülleri, edgeAgent ve edgeHub, katmanlı dağıtımın bir parçası olarak yapılandırılmamıştır. Katmanlı bir dağıtım tarafından hedeflenen tüm IoT Edge cihazlara, önce standart bir Otomatik dağıtıma uygulanmış olması gerekir. Otomatik dağıtım, katmanlı dağıtımların eklenebileceği temeli sağlar.

IoT Edge bir cihaz yalnızca bir standart otomatik dağıtım uygulayabilir, ancak birden çok katmanlı otomatik dağıtımlar uygulayabilir. Bir cihazın hedeflediği katmanlı dağıtımlar, bu cihaz için otomatik dağıtımdan daha yüksek önceliğe sahip olmalıdır.

Örneğin, binaları yöneten bir şirketin aşağıdaki senaryosunu göz önünde bulundurun. Güvenlik kameraları, hareket sensörleri ve yükseltme 'lerden veri toplamak için IoT Edge modüller geliştirirler. Ancak, tüm binaları üç modülü de kullanamaz. Standart otomatik dağıtımlar sayesinde şirketin, binalarının ihtiyaç duyduğu tüm modül birleşimleri için bireysel dağıtımlar oluşturması gerekir.

![Standart otomatik dağıtımların her modül birleşimine uygun olması gerekir](./media/module-deployment-monitoring/standard-deployment.png)

Ancak, şirket katmanlı Otomatik dağıtıma geçtiğinde, yönetmek üzere daha az dağıtım olan binaları için aynı modül kombinasyonlarını oluşturamazlar. Her modülün kendi katmanlı bir dağıtımı vardır ve cihaz etiketleri her oluşturmaya hangi modüllerin ekleneceğini belirler.

![Katmanlı otomatik dağıtımlar, aynı modüllerin farklı yollarla birleştirileceği senaryoları basitleştirir](./media/module-deployment-monitoring/layered-deployment.png)

### <a name="module-twin-configuration"></a>Module ikizi yapılandırması

Katmanlı dağıtımlarla çalışırken, kasıtlı olarak veya başka bir şekilde, bir cihazı hedefleyen aynı modüle sahip iki dağıtıma sahip olabilirsiniz. Bu durumlarda, daha yüksek öncelikli dağıtımın ikizi modülünün üzerine yazıp yazmayacağını veya sonuna ekleme yapıp etmeyeceğine karar verebilirsiniz. Örneğin, 100 farklı cihaza aynı modülü uygulayan bir dağıtıma sahip olabilirsiniz. Ancak, bu cihazların 10 ' u güvenli tesislerde ve proxy sunucularıyla iletişim kurmak için ek yapılandırma gerekir. Bu 10 cihazın, temel dağıtımdan var olan modül ikizi bilgilerinin üzerine yazmadan güvenli bir şekilde iletişim kurmasına olanak tanıyan modül ikizi özellikleri eklemek için katmanlı bir dağıtım kullanabilirsiniz.

Dağıtım bildiriminde Module ikizi istenen özellikleri ekleyebilirsiniz. Standart dağıtımda, bir katman ikizi Module, katmanlı bir dağıtımda istenen özelliklerin yeni bir alt kümesini **bildirebilirsiniz.**

Örneğin, standart bir dağıtımda, sanal sıcaklık algılayıcısı modülünü 5 saniyelik aralıklarla veri göndermesini söyleyen aşağıdaki istenen özelliklerle ekleyebilirsiniz:

```json
"SimulatedTemperatureSensor": {
  "properties.desired": {
    "SendData": true,
    "SendInterval": 5
  }
}
```

Aynı cihazların bazılarını veya tümünü hedefleyen katmanlı bir dağıtımda, sanal algılayıcının 1000 ileti göndermesini ve sonra durdurmasını söyleyen bir özellik ekleyebilirsiniz. Var olan özelliklerin üzerine yazmak istemezsiniz, bu nedenle, istenen özellikler `layeredProperties`içinde yeni özelliği içeren yeni bir bölüm oluşturursunuz:

```json
"SimulatedTemperatureSensor": {
  "properties.desired.layeredProperties": {
    "StopAfterCount": 1000
  }
}
```

Her iki dağıtımda de uygulanmış bir cihaz, sanal sıcaklık algılayıcısı için ikizi modülünde aşağıdaki özellikleri yansıtır:

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

## <a name="phased-rollout"></a>Aşamalı dağıtım

Aşamalı bir dağıtım, bir işlecin bir dizi IoT Edge cihaza değişiklikleri dağıttığı genel bir işlemdir. Amaç, geniş ölçekli büyük değişiklikler yapma riskini azaltmak için değişiklikler yavaş yavaş yapılır. Otomatik dağıtımlar, IoT Edge cihazları arasında aşamalı piyasaya çıkarma yönetimine yardımcı olur.

Aşamalı bir dağıtım aşağıdaki aşamalar ve adımlarda yürütülür:

1. IoT Edge cihazların bir test ortamı oluşturun ve gibi `tag.environment='test'`bir cihaz ikizi etiketi ayarlayarak.Test ortamı, dağıtımın sonunda hedeflenecek üretim ortamını yansıtmalıdır.
2. İstenen modülleri ve konfigürasyonları içeren bir dağıtım oluşturun. Hedefleme koşulu, test IoT Edge cihaz ortamını hedeflemelidir.
3. Test ortamında yeni modül yapılandırmasını doğrulayın.
4. Hedefleme koşuluna yeni bir etiket ekleyerek dağıtımı, üretim IoT Edge cihazlarının bir alt kümesini içerecek şekilde güncelleştirin. Ayrıca, dağıtım önceliğin o cihazları hedefleyen diğer dağıtımlardan daha yüksek olduğundan emin olun
5. Dağıtım durumunu görüntüleyerek, hedeflenen IoT cihazlarında dağıtımın başarılı olduğunu doğrulayın.
6. Dağıtımı, kalan tüm üretim IoT Edge cihazlarını hedefleyecek şekilde güncelleştirin.

## <a name="rollback"></a>Etkin

Hatalar veya yanlış yapılandırma alırsanız dağıtımlar geri alınabilir.Bir dağıtım, bir IoT Edge cihazının mutlak modül yapılandırmasını tanımladığından, hedef tüm modülleri kaldırsa bile, ek bir dağıtımın aynı cihaza daha düşük bir önceliğe de hedeflenmiş olması gerekir.  

Dağıtım silindiğinde, modüller hedeflenen cihazlardan kaldırılmaz. Boş bir dağıtım olsa bile, cihazlar için yeni bir yapılandırma tanımlayan başka bir dağıtım olması gerekir.

Aşağıdaki sırada geri alma işlemleri gerçekleştirin:

1. İkinci bir dağıtımın aynı cihaz kümesine de hedeflenmiş olduğunu onaylayın. Geri almanın hedefi tüm modülleri kaldırmaktır, ikinci dağıtımda herhangi bir modül içermemelidir.
2. Cihazların hedefleme koşulunu artık karşılamadığında, geri almak istediğiniz dağıtımın hedef koşul ifadesini değiştirin veya kaldırın.
3. Dağıtım durumunu görüntüleyerek geri almanın başarılı olduğunu doğrulayın.
   * Geri alınan dağıtım artık geri alınmış cihazların durumunu göstermemelidir.
   * İkinci dağıtım artık geri alınan cihazların dağıtım durumunu içermelidir.

## <a name="next-steps"></a>Sonraki adımlar

* Dağıtım [ve izleme IoT Edge modüllerindeki](how-to-deploy-at-scale.md)bir dağıtımı oluşturma, güncelleştirme veya silme adımlarını gözden geçir.
* [IoT Edge çalışma zamanı](iot-edge-runtime.md) ve [IoT Edge modülleri](iot-edge-modules.md)gibi diğer IoT Edge kavramları hakkında daha fazla bilgi edinin.
