---
title: Azure IoT Hub ile ölçekte otomatik cihaz yönetimi | Microsoft Dokümanlar
description: Birden fazla IoT aygıtını ve modüllerini yönetmek için Azure IoT Hub otomatik yapılandırmalarını kullanın
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: chrisgre
ms.openlocfilehash: 75c6b7d89e7ae540e7428afde127281aa3f15fc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271310"
---
# <a name="automatic-iot-device-and-module-management-using-the-azure-portal"></a>Azure portalını kullanarak otomatik IoT aygıt ve modül yönetimi

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-hub-auto-device-config-selector.md)]

Azure IoT Hub'daki otomatik aygıt yönetimi, büyük aygıt filolarını yönetmenin yinelenen ve karmaşık görevlerinin çoğunu otomatikleştirir. Otomatik aygıt yönetimi yle, özelliklerine göre bir aygıt kümesini hedefleyebilir, istenen yapılandırmayı tanımlayabilir ve ardından IoT Hub'ın aygıtları kapsama girdiklerinde güncelleştirmesine izin verebilirsiniz. Bu güncelleştirme, tamamlama ve uyumluluğu özetlemenize, birleştirme ve çakışmaları işlemenize ve yapılandırmaları aşamalı bir yaklaşımla kullanıma çıkarmanıza olanak tanıyan _otomatik aygıt yapılandırması_ veya _otomatik modül yapılandırması_kullanılarak yapılır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Otomatik cihaz yönetimi, istenilen özelliklere sahip bir dizi aygıt ikizini veya modül ikizini güncelleyerek ve bildirilen ikiz özelliklere dayalı bir özeti bildirerek çalışır.  Üç bölümden biri olan *yapılandırma* adı verilen yeni bir sınıf ve JSON belgesi sunar:

* **Hedef koşul** güncellenecek cihaz ikizleri veya modül ikizlerkapsamını tanımlar. Hedef koşul, çift etiketler ve/veya bildirilen özelliklerdeki bir sorgu olarak belirtilir.

* **Hedef içerik,** hedeflenen cihaz ikizleri veya modül ikizleri eklenecek veya güncellenecek istenen özellikleri tanımlar. İçerik, değiştirilecek istenilen özelliklerin bölümüne giden bir yol içerir.

* **Ölçümler,** **Başarı**, **Devam Eden**ve **Hata**gibi çeşitli yapılandırma durumlarının özet sayılarını tanımlar. Özel ölçümler, bildirilen ikiz özelliklerdeki sorgular olarak belirtilir.  Sistem ölçümleri, hedeflenen ikiz sayısı ve başarıyla güncelleştirilen ikiz sayısı gibi ikiz güncelleştirme durumunu ölçen varsayılan ölçümlerdir.

Otomatik yapılandırmalar, yapılandırma oluşturulduktan kısa bir süre sonra ilk kez ve beş dakikalık aralıklarla çalışır. Ölçümler sorguları, otomatik yapılandırma her çalıştığında çalışır.

## <a name="implement-twins"></a>İkizler iuygulayın

Otomatik aygıt yapılandırmaları, bulut ve aygıtlar arasında durumu eşitlemek için aygıt ikizlerinin kullanılmasını gerektirir.  Daha fazla bilgi için [Bkz. IoT Hub'da aygıt ikizlerini anlayın ve kullanın.](iot-hub-devguide-device-twins.md)

Otomatik modül yapılandırmaları bulut ve modüller arasında durum senkronize etmek için modül ikizler için kullanımını gerektirir. Daha fazla bilgi [için, IoT Hub'daki modül ikizlerini anlayın ve kullanın.](iot-hub-devguide-module-twins.md)

## <a name="use-tags-to-target-twins"></a>İkizleri hedeflemek için etiketleri kullanma

Yapılandırma oluşturmadan önce, hangi aygıtları veya modülleri etkilemek istediğinizi belirtmeniz gerekir. Azure IoT Hub aygıtları ve aygıt ikizinde etiketleri kullanarak tanımlar ve modül ikizindeki etiketleri kullanarak modülleri tanımlar. Her cihaz veya modülbirden fazla etikete sahip olabilir ve bunları çözümünüz için anlamlı herhangi bir şekilde tanımlayabilirsiniz. Örneğin, aygıtları farklı konumlarda yönetiyorsanız, aygıt ikizine aşağıdaki etiketleri ekleyin:

```json
"tags": {
    "location": {
        "state": "Washington",
        "city": "Tacoma"
    }
},
```

## <a name="create-a-configuration"></a>Yapılandırma oluşturma

1. Azure [portalında,](https://portal.azure.com)IoT hub'ınıza gidin. 

2. **IoT aygıt yapılandırması**seçin.

3. **Aygıt yapılandırması ekle** veya modül **yapılandırması ekle'yi**seçin.

   ![Aygıt yapılandırması veya modül yapılandırması ekleme](./media/iot-hub-automatic-device-management/create-automatic-configuration.png)

Yapılandırma oluşturmak için beş adım vardır. Aşağıdaki bölümlerher biri ile yürümek. 

### <a name="name-and-label"></a>Ad ve Etiket

1. Yapılandırmanıza en fazla 128 küçük harf içeren benzersiz bir ad verin. Boşluklardan ve aşağıdaki geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.

2. Yapılandırmalarınızı izlemenize yardımcı olmak için etiketler ekleyin. Etiketler, yapılandırmanızı açıklayan **Ad**, **Değer** çiftleridir. Örneğin `HostPlatform, Linux` veya `Version, 3.0.1` olabilir.

3. Bir sonraki adıma geçmek için **İleri'yi** seçin. 

### <a name="specify-settings"></a>Ayarları Belirt

Bu bölümde, hedeflenen cihazveya modül ikizleri ayarlanacak içerik tanımlanır. Her ayar kümesi için iki giriş vardır. İlki, ayarlanacak ikiz istenilen özellikler içinde JSON bölümüne giden yol olan ikiz yoldur.  İkincisi, bu bölüme eklenecek JSON içeriğidir. 

Örneğin, ikiz yolu aşağıdaki JSON içeriğine `properties.desired.chiller-water` ayarlayabilir ve sonra sağlayabilirsiniz: 

```json
{
  "temperature": 66,
  "pressure": 28
}
```

![İkiz yolu ve içeriği ayarlama](./media/iot-hub-automatic-device-management/module-config-twin-settings.png)


Ayrıca, tüm ikiz yolu belirterek ve hiçbir parantez ile değer sağlayarak tek tek ayarları ayarlayabilirsiniz. Örneğin, ikiz yol `properties.desired.chiller-water.temperature`ile, içeriği `66`. Sonra basınç özelliği için yeni bir ikiz ayarı oluşturun. 

İki veya daha fazla yapılandırma aynı ikiz yolu hedefliyorsa, en yüksek öncelikli yapılandırmadaki içerik uygulanır (öncelik adım 4'te tanımlanır).

Varolan bir özelliği kaldırmak istiyorsanız, özellik `null`değerini ' e belirtin.

**Aygıt İkiz Ayar Ekle'yi** veya **Modül İkiz Ayar Ekle'yi**seçerek ek ayarlar ekleyebilirsiniz.

### <a name="specify-metrics-optional"></a>Ölçümleri Belirt (isteğe bağlı)

Ölçümler, bir aygıtın veya modülün yapılandırma içeriğini uyguladıktan sonra geri bildirebileceği çeşitli durumların özet sayımlarını sağlar. Örneğin, bekleyen ayarlar değişiklikleri için bir metrik, hatalar için bir metrik ve başarılı ayarlar için bir metrik oluşturabilirsiniz.

Her yapılandırmada en fazla beş özel ölçüm olabilir. 

1. **Metrik Ad**için bir ad girin.

2. **Metrik Ölçütler**için bir sorgu girin.  Sorgu, aygıt ikizi bildirilen özelliklerine dayanır.  Metrik, sorgu tarafından döndürülen satır sayısını gösterir.

Örnek:

```sql
SELECT deviceId FROM devices 
  WHERE properties.reported.chillerWaterSettings.status='pending'
```

Yapılandırmanın uygulandığı bir yan tümce ekleyebilirsiniz, örneğin: 

```sql
/* Include the double brackets. */
SELECT deviceId FROM devices 
  WHERE configurations.[[yourconfigname]].status='Applied'
```

Yapılandırılmış modülleri rapor etmek için bir metrik oluşturuyorsanız, 'den `moduleId` `devices.modules`birini seçin. Örnek:

```sql
SELECT deviceId, moduleId FROM devices.modules
  WHERE properties.reported.lastDesiredStatus.code = 200
```

### <a name="target-devices"></a>Hedef Cihazlar

Bu yapılandırmayı alması gereken belirli aygıtları veya modülleri hedeflemek için ikizlerinizden gelen etiket özelliğini kullanın. İkiz bildirilen özellikleri de hedefleyebilirsiniz.

Otomatik aygıt yapılandırmaları yalnızca aygıt çift etiketlerini hedefleyebilir ve otomatik modül yapılandırmaları yalnızca modül çift etiketlerini hedefleyebilir. 

Birden çok yapılandırma aynı aygıtı veya modülü hedefleyediğinden, her yapılandırmanın bir öncelik numarasına ihtiyacı vardır. Bir çakışma olursa, en yüksek önceliğe sahip yapılandırma kazanır. 

1. Yapılandırma **Önceliği**için pozitif bir tamsayı girin. En yüksek sayısal değer en yüksek öncelik olarak kabul edilir. İki yapılandırma aynı öncelik numarasına sahipse, en son oluşturulan yapılandırma kazanır. 

2. Bu yapılandırmayla hangi aygıtların veya modüllerin hedefleneceğini belirlemek için bir **Hedef koşulu** girin. Koşul, ikiz etiketlere veya ikiz bildirilen özelliklere dayanır ve ifade biçimiyle eşleşmelidir. 

   Otomatik aygıt yapılandırması için, hedefe yönelik etiketi veya bildirilen özelliği belirtebilirsiniz. Örneğin `tags.environment='test'` veya `properties.reported.chillerProperties.model='4000x'` olabilir. Tüm aygıtları hedeflemeyi belirtebilirsiniz. `*` 
   
   Otomatik modül yapılandırması için, IoT hub'ına kayıtlı modüllerden etiketleri veya bildirilen özellikleri belirtmek için bir sorgu kullanın. Örneğin `from devices.modules where tags.environment='test'` veya `from devices.modules where properties.reported.chillerProperties.model='4000x'` olabilir. Joker karakter tüm modülleri hedeflemek için kullanılamaz. 

3. Son adıma geçmek için **İleri'yi** seçin.

### <a name="review-configuration"></a>Yapılandırmayı İncele

Yapılandırma bilgilerinizi gözden geçirin ve **ardından Gönder'i**seçin.

## <a name="monitor-a-configuration"></a>Yapılandırmayı izleme

Yapılandırmanın ayrıntılarını görüntülemek ve çalıştıran aygıtları izlemek için aşağıdaki adımları kullanın:

1. Azure [portalında,](https://portal.azure.com)IoT hub'ınıza gidin. 

2. **IoT aygıt yapılandırması**seçin.

3. Yapılandırma listesini inceleyin. Her yapılandırma için aşağıdaki ayrıntıları görüntüleyebilirsiniz:

   * **ID** - yapılandırmanın adı.

   * **Hedef koşul** - hedeflenen aygıtları veya modülleri tanımlamak için kullanılan sorgu.

   * **Öncelik** - yapılandırmaya atanan öncelik numarası.

   * **Oluşturma zamanı** - yapılandırmanın oluşturulduğu zamana ait zaman damgası. Bu zaman damgası, iki yapılandırma aynı önceliğe sahip olduğunda bağları kırmak için kullanılır. 

   * **Sistem ölçümleri** - IoT Hub tarafından hesaplanan ve geliştiriciler tarafından özelleştirilemeyen ölçümler. Hedeflenen aygıt ikizlerinin sayısını, hedef koşulla eşleşen olarak belirtir. Ayrı, daha yüksek öncelikli yapılandırmanın da değişiklik yapmış olması durumunda kısmi değişiklikler içerebilen yapılandırma tarafından değiştirilen aygıt ikizlerinin sayısını belirtir. 

   * **Özel ölçümler** - geliştirici tarafından ikiz bildirilen özelliklere karşı sorguolarak belirtilen ölçümler.  Yapılandırma başına en fazla beş özel ölçüm tanımlanabilir. 
   
4. İzlemek istediğiniz yapılandırmayı seçin.  

5. Yapılandırma ayrıntılarını inceleyin. Yapılandırmayı alan aygıtlarla ilgili belirli ayrıntıları görüntülemek için sekmeleri kullanabilirsiniz.

   * **Hedef Koşul** - hedef koşulla eşleşen aygıtlar veya modüller. 

   * **Ölçümler** - sistem ölçümleri ve özel ölçümlerlistesi.  Açılan yolda metrik seçilip ardından Cihazları Görüntüle veya **Modülleri**Görüntüle'yi seçerek her **View Devices** metrik için sayılan aygıtların veya modüllerin listesini görüntüleyebilirsiniz.

   * **Aygıt İkiz Ayarları** veya **Modül İkiz Ayarları** - yapılandırma tarafından ayarlanan ikiz ayarlar. 

   * **Yapılandırma Etiketleri** - yapılandırmayı tanımlamak için kullanılan anahtar değer çiftleri.  Etiketlerin işlevsellik üzerinde hiçbir etkisi yoktur. 

## <a name="modify-a-configuration"></a>Yapılandırmayı değiştirme

Bir yapılandırmayı değiştirdiğinizde, değişiklikler hemen tüm hedeflenen aygıtlara veya modüllere çoğalır. 

Hedef koşulu güncelleştirirseniz, aşağıdaki güncelleştirmeler oluşur:

* Bir ikiz eski hedef koşulu karşılamadı, ancak yeni hedef koşulu karşılar ve bu yapılandırma bu ikiz için en yüksek öncelik, o zaman bu yapılandırma uygulanır. 

* Şu anda bu yapılandırmayı çalıştıran bir ikiz artık hedef koşula uygun değilse, yapılandırmadaki ayarlar kaldırılır ve ikiz sonraki en yüksek öncelikli yapılandırma tarafından değiştirilir. 

* Şu anda bu yapılandırmayı çalıştıran bir ikiz artık hedef koşulu karşılamıyorsa ve diğer yapılandırmaların hedef koşulunu karşılamıyorsa, yapılandırmadaki ayarlar kaldırılır ve ikizüzerinde başka değişiklik yapılmaz. 

Yapılandırmayı değiştirmek için aşağıdaki adımları kullanın: 

1. Azure [portalında,](https://portal.azure.com)IoT hub'ınıza gidin. 

2. **IoT aygıt yapılandırması**seçin. 

3. Değiştirmek istediğiniz yapılandırmayı seçin. 

4. Aşağıdaki alanlarda güncelleştirmeler yapın: 

   * Hedef koşul 
   * Etiketler 
   * Öncelik 
   * Ölçümler

4. **Kaydet'i**seçin.

5. Değişikliklerin kullanıma sunulmasını izlemek için [bir yapılandırmayı İzle'deki](#monitor-a-configuration) adımları izleyin. 

## <a name="delete-a-configuration"></a>Yapılandırmayı silme

Bir yapılandırmayı sildiğinizde, herhangi bir aygıt ikizi bir sonraki en yüksek öncelikli yapılandırmalarını alırlar. Aygıt ikizleri başka bir yapılandırmanın hedef koşuluna uymuyorsa, başka bir ayar uygulanmaz. 

1. Azure [portalında,](https://portal.azure.com)IoT hub'ınıza gidin. 

2. **IoT aygıt yapılandırması**seçin. 

3. Silmek istediğiniz yapılandırmayı seçmek için onay kutusunu kullanın. 

4. **Sil**’i seçin.

5. Bir istem onaylamanızı ister.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, IoT aygıtlarını ölçekte yapılandırmayı ve izlemeyi öğrendiniz. Azure IoT Hub'ı yönetme hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [IoT Hub cihaz kimliklerinizi toplu olarak yönetme](iot-hub-bulk-identity-mgmt.md)
* [IoT Hub ölçümleri](iot-hub-metrics.md)
* [İşlemleri izleme](iot-hub-operations-monitoring.md)

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [IoT Hub geliştirici kılavuzu](iot-hub-devguide.md)
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/tutorial-simulate-device-linux.md)

Sıfır dokunmayı, tam zamanında sağlamayı etkinleştirmek için IoT Hub Aygıt Sağlama Hizmetini kullanmak için bkz: 

* [Azure IoT Hub Cihazı Sağlama Hizmeti](/azure/iot-dps)
