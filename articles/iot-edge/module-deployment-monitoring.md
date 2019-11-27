---
title: Cihaz grupları - Azure IOT Edge için otomatik dağıtım | Microsoft Docs
description: Otomatik dağıtımlar, paylaşılan etiketlere göre cihaz grupları yönetmek için Azure IOT Edge'de kullanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 09/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: eb45f2b929c08ce77c83af450726a00dd6af458e
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74456736"
---
# <a name="understand-iot-edge-automatic-deployments-for-single-devices-or-at-scale"></a>IOT Edge otomatik dağıtımlar tek tek cihazlarda veya uygun ölçekte anlama

Azure IoT Edge cihazlar diğer IoT cihazları türlerine benzer bir [cihaz yaşam döngüsünü](../iot-hub/iot-hub-device-management-overview.md) izler:

1. Bir cihazı bir işletim sistemi ile Imaging ve [IoT Edge çalışma zamanını](iot-edge-runtime.md)yükleyerek yeni IoT Edge cihazları sağlayın.
2. Cihazları [IoT Edge modülleri](iot-edge-modules.md)çalıştıracak şekilde yapılandırın ve sonra durumlarını izleyin. 
3. Son olarak, değiştirilen veya kalkacak cihazları devre dışı bırakın.  

Azure IoT Edge, modülleri IoT Edge cihazlarda çalışacak şekilde yapılandırmak için iki yol sunar: tek bir cihazdaki geliştirme ve hızlı yinelemeler (bu yöntemi Azure IoT Edge [öğreticilerde](tutorial-deploy-function.md)kullandınız) ve bir tane de IoT Edge cihazlarının büyük bir bölümünü yönetmek için kullanılır. Bu yaklaşımların her ikisi de Azure portalında hem de programsal olarak kullanılabilir. Grupları veya çok sayıda cihazı hedeflemek için, bir cihaz ikizi [etiketlerini](../iot-edge/how-to-deploy-monitor.md#identify-devices-using-tags) kullanmak üzere modüllerinizi dağıtmak istediğiniz cihazları belirtebilirsiniz. Aşağıdaki adımlarda tanımlanan etiketler özelliği aracılığıyla bir Washington eyaletinde cihaz grubuna bir dağıtım bahsedeceğiz. 

Bu makalede yapılandırmasına odaklanılmıştır ve cihazların filolarına aşamaları izleme için IOT Edge otomatik dağıtımlar anılan. Genel dağıtım adımları aşağıdaki gibidir: 

1. Bir işleç modülleri, hem de hedef cihazlar kümesini tanımlayan bir dağıtım tanımlar. Her dağıtımda bu bilgileri yansıtan bir dağıtım bildirimi vardır. 
2. IOT Hub hizmeti, tüm hedeflenen cihazların istenen modülleriyle yapılandırılacakları ile iletişim kurar. 
3. IOT Hub hizmeti, IOT Edge cihazları alır ve işleci için kullanılabilir hale getirir.  Örneğin, bir operatör, bir sınır cihazının başarıyla yapılandırılmadığını veya çalışma zamanı sırasında bir modülün başarısız olup olmadığını görebilir. 
4. Herhangi bir zamanda hedefleme koşullara uyan IOT Edge cihazları yeni dağıtım için yapılandırılmış. Örneğin, Washington Eyaleti 'nde tüm IoT Edge cihazlarını hedefleyen bir dağıtım, kaynak ve Washington durum cihaz grubuna eklendikten sonra otomatik olarak yeni bir IoT Edge cihazı yapılandırır. 
 
Bu makalede, yapılandırma ve dağıtım izleme alan her bir bileşeni açıklar. Dağıtım oluşturma ve güncelleştirme hakkında yönergeler için bkz. [IoT Edge modüllerini ölçekli olarak dağıtma ve izleme](how-to-deploy-monitor.md).

## <a name="deployment"></a>Dağıtım

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

Herhangi bir Boolean koşul, hedef cihazları seçmek için cihaz ikizlerini etiketleri veya DeviceID kullanın. Koşulu etiketleriyle birlikte kullanmak istiyorsanız, Device ikizi 'ın özellikler ile aynı düzey altında "Etiketler":{} bölümüne eklemeniz gerekir. [Cihaz ikizi Etiketler hakkında daha fazla bilgi edinin](../iot-hub/iot-hub-devguide-device-twins.md)

Hedef koşul örnekleri:

* DeviceID = 'linuxprod1'
* Tags.Environment = 'prod'
* Tags.Environment = 'prod' AND tags.location = 'westus'
* Tags.Environment = 'prod' veya tags.location = 'westus'
* Tags.operator 'John' = ve tags.environment = 'prod' değil DeviceID = 'linuxprod1'

Hedef koşul oluştururken bazı kısıtlar şunlardır:

* Cihaz ikizinde etiketleri veya DeviceID kullanarak bir hedef koşulu yalnızca oluşturabilirsiniz.
* Çift tırnak işareti, herhangi bir bölümünü hedef koşulu izin verilmez. Tek tırnak işareti kullanın.
* Tek tırnak, hedef koşulu değerlerini temsil eder. Bu nedenle, cihaz adı bir parçası ise tek tırnak işareti ile başka bir tek tırnak işareti kaçış gerekir. Örneğin, `operator'sDevice`adlı bir cihazı hedeflemek için `deviceId='operator''sDevice'`yazın.
* Hedef koşul değerlerinde sayılar, harfler ve aşağıdaki karakterlere izin verilir: `-:.+%_#*?!(),=@;$`.

### <a name="priority"></a>Öncelik

Öncelikli bir dağıtım hedeflenen cihaza göre diğer dağıtımlar uygulanması gerektiğini tanımlar. Dağıtım ile daha büyük sayılar daha yüksek öncelikli belirten pozitif bir tamsayı önceliktir. IOT Edge cihazı birden fazla dağıtım tarafından hedeflendiğinde, en yüksek önceliğe sahip dağıtım uygulanır.  Düşük önceliklere sahip dağıtımlar uygulanmaz ve birleştirilirler.  Bir cihaz eşit önceliğe sahip iki veya daha fazla dağıtıma hedefleniyorsa, en son oluşturulan dağıtım (oluşturma zaman damgasıyla belirlenir) geçerlidir.

### <a name="labels"></a>Etiketler 

Etiketleri filtresi ve dağıtım grubu için kullanabileceğiniz dize anahtar/değer çiftleridir. Bir dağıtımda birden fazla etiket olabilir. Etiket isteğe bağlıdır ve herhangi bir etkisi IOT Edge cihazları gerçek yapılandırmasını yapın. 

### <a name="deployment-status"></a>Dağıtım durumu

Bir dağıtım için hedeflenen tüm IOT Edge cihazı başarıyla uygulanmış olup olmadığını belirlemek için de izlenebilir.  Hedeflenen bir sınır aygıtı aşağıdaki durum kategorilerindeki bir veya daha fazla şekilde görünür: 

* **Hedef** , dağıtım hedefleme durumuyla eşleşen IoT Edge cihazları gösterir.
* **Gerçek** , daha yüksek bir öncelik dağıtımı tarafından hedeflenilmemiş hedeflenen IoT Edge cihazları gösterir.
* **Sağlıklı** , modüllerin başarıyla dağıtıldığını hizmete geri rapor veren IoT Edge cihazları gösterir. 
* **Sağlıksız** durum, cihazların bir veya modüllerin başarıyla dağıtılmadığını hizmete geri bildirdiği IoT Edge gösterir. Daha fazla hata araştırmak, bu cihazlar için uzaktan bağlanma ve günlük dosyalarını görüntülemek için.
* **Bilinmiyor** , bu dağıtım ile ilgili herhangi bir durumu bildirmeyen IoT Edge cihazları gösterir. Daha fazla araştırmak için hizmet bilgileri ve günlük dosyalarını görüntüleyin.

## <a name="phased-rollout"></a>Aşamalı dağıtımı 

Aşamalı yapabildiği değişiklikleri operatörün insanın ufkunu genişleten bir IOT Edge cihazları kümesine dağıtır, genel bir işlemdir. Aşamalı olarak geniş ölçek bozucu değişiklikler yapma riskini azaltmak için değişiklik olmaktır.  

Aşamalı aşağıdaki aşamaları ve adımları çalıştırılır: 

1. IoT Edge cihazların bir test ortamı oluşturun ve `tag.environment='test'`gibi bir cihaz ikizi etiketi ayarlayarak. Test ortamı, dağıtımın sonunda hedeflenecek üretim ortamını yansıtmalıdır. 
2. İstenen modülleri ve yapılandırmaları da dahil olmak üzere bir dağıtım oluşturun. IOT Edge cihazı ortam test hedefleme koşul hedeflemelidir.   
3. Yeni modül yapılandırması test ortamında doğrulayın.
4. Dağıtım hedefleme koşula yeni bir etiket ekleyerek bir alt kümesini üretim IOT Edge cihazları içerecek şekilde güncelleştirin. Ayrıca, dağıtım önceliğini şu anda bu cihazları hedefleyen diğer dağıtımlar daha yüksek olduğundan emin olun 
5. Dağıtım durumu görüntüleyerek dağıtım hedeflenen IOT Cihazlarında başarılı olduğunu doğrulayın.
6. Kalan tüm üretim IOT Edge cihazları hedeflemek için dağıtım güncelleştirin.

## <a name="rollback"></a>Geri alma

Bir hata veya yanlış yapılandırmalarını geri alırsanız, dağıtımları alınabilir.  Bir dağıtım, bir IoT Edge cihazının mutlak modül yapılandırmasını tanımladığından, hedef tüm modülleri kaldırsa bile, ek bir dağıtımın aynı cihaza daha düşük bir önceliğe de hedeflenmiş olması gerekir.  

Geri alma işlemleri, aşağıdaki sırayla gerçekleştirin: 

1. İkinci bir dağıtım aynı cihazı kümesinin hedeflenir onaylayın. Geri alma amacı, tüm modülleri kaldırmak için ise, ikinci dağıtımı modüllerin içermemelidir. 
2. Değiştirin veya hedef koşul ifadesi, böylece cihazlar artık hedefleme koşula uyan geri almak istediğiniz dağıtımın kaldırın.
3. Dağıtım durumunu görüntüleyerek geri alma başarılı olduğunu doğrulayın.
   * Toplu geri dağıtım durumu geri alındı cihazlar için artık göstermelidir.
   * İkinci dağıtımı artık geri alındı cihazlar için dağıtım durumunu içermelidir.


## <a name="next-steps"></a>Sonraki adımlar

* Dağıtım [ve izleme IoT Edge modüllerindeki](how-to-deploy-monitor.md)bir dağıtımı oluşturma, güncelleştirme veya silme adımlarını gözden geçir.
* [IoT Edge çalışma zamanı](iot-edge-runtime.md) ve [IoT Edge modülleri](iot-edge-modules.md)gibi diğer IoT Edge kavramları hakkında daha fazla bilgi edinin.

