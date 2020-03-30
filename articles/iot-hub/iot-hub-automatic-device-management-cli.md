---
title: Azure IoT Hub (CLI) ile ölçekte otomatik cihaz yönetimi | Microsoft Dokümanlar
description: Birden fazla IoT aygıtını veya modüllerini yönetmek için Azure IoT Hub otomatik yapılandırmalarını kullanma
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: chrisgre
ms.openlocfilehash: 748f3e09fd03a6f37954c8dfaf4b6ae9144384bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235612"
---
# <a name="automatic-iot-device-and-module-management-using-the-azure-cli"></a>Azure CLI'yi kullanarak otomatik IoT aygıtı ve modül yönetimi

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-hub-auto-device-config-selector.md)]

Azure IoT Hub'daki otomatik aygıt yönetimi, büyük aygıt filolarını yönetmenin yinelenen ve karmaşık görevlerinin çoğunu otomatikleştirir. Otomatik aygıt yönetimi yle, özelliklerine göre bir aygıt kümesini hedefleyebilir, istenen yapılandırmayı tanımlayabilir ve ardından IoT Hub'ın aygıtları kapsama girdiklerinde güncelleştirmesine izin verebilirsiniz. Bu güncelleştirme, tamamlama ve uyumluluğu özetlemenize, birleştirme ve çakışmaları işlemenize ve yapılandırmaları aşamalı bir yaklaşımla kullanıma çıkarmanıza olanak tanıyan _otomatik aygıt yapılandırması_ veya _otomatik modül yapılandırması_kullanılarak yapılır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Otomatik cihaz yönetimi, istenilen özelliklere sahip bir dizi aygıt ikizini veya modül ikizini güncelleyerek ve bildirilen ikiz özelliklere dayalı bir özeti bildirerek çalışır.  Üç bölümden biri olan *yapılandırma* adı verilen yeni bir sınıf ve JSON belgesi sunar:

* **Hedef koşul** güncellenecek cihaz ikizleri veya modül ikizlerkapsamını tanımlar. Hedef koşul, aygıt ikiz etiketleri ve/veya bildirilen özelliklerdeki sorgu olarak belirtilir.

* **Hedef içerik,** hedeflenen cihaz ikizleri veya modül ikizleri eklenecek veya güncellenecek istenen özellikleri tanımlar. İçerik, değiştirilecek istenilen özelliklerin bölümüne giden bir yol içerir.

* **Ölçümler,** **Başarı**, **Devam Eden**ve **Hata**gibi çeşitli yapılandırma durumlarının özet sayılarını tanımlar. Özel ölçümler, bildirilen ikiz özelliklerdeki sorgular olarak belirtilir.  Sistem ölçümleri, hedeflenen ikiz sayısı ve başarıyla güncelleştirilen ikiz sayısı gibi ikiz güncelleştirme durumunu ölçen varsayılan ölçümlerdir.

Otomatik yapılandırmalar, yapılandırma oluşturulduktan kısa bir süre sonra ilk kez ve beş dakikalık aralıklarla çalışır. Ölçümler sorguları, otomatik yapılandırma her çalıştığında çalışır.

## <a name="cli-prerequisites"></a>CLI ön koşulları

* Azure aboneliğinizde bir [IoT hub'ı.](../iot-hub/iot-hub-create-using-cli.md) 

* Ortamınızda [Azure CLI.](https://docs.microsoft.com/cli/azure/install-azure-cli) Azure CLI sürümünüz en az 2.0.70 veya üzeri olmalıdır. Doğrulamak için `az –-version` kullanın. Bu sürüm, az uzantı komutlarını destekler ve Knack komut çerçevesini kullanıma sunar. 

* [Azure CLI için IoT uzantısı.](https://github.com/Azure/azure-cli)

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

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

## <a name="define-the-target-content-and-metrics"></a>Hedef içeriği ve ölçümleri tanımlama

Hedef içerik ve metrik sorgular, aygıt ikizi veya modül ikizi istenen özellikleri tanımlayan JSON belgeleri olarak belirtilir ve ölçmek için özellikleri bildirir.  Azure CLI kullanarak otomatik yapılandırma oluşturmak için hedef içeriği ve ölçümleri yerel olarak .txt dosyaları olarak kaydedin. Yapılandırmayı cihazınıza uygulamak için komutu çalıştırdığınızda daha sonraki bir bölümde dosya yollarını kullanırsınız.

Otomatik aygıt yapılandırması için temel bir hedef içerik örneği aşağıda veda edebilirsiniz:

```json
{
  "content": {
    "deviceContent": {
      "properties.desired.chillerWaterSettings": {
        "temperature": 38,
        "pressure": 78
      }
    }
}
```

Otomatik modül yapılandırmaları çok benzer şekilde, `moduleContent` ancak `deviceContent`yerine hedef .

```json
{
  "content": {
    "moduleContent": {
      "properties.desired.chillerWaterSettings": {
        "temperature": 38,
        "pressure": 78
      }
    }
}
```

Aşağıda metrik sorgu örnekleri verilmiştir:

```json
{
  "queries": {
    "Compliant": "select deviceId from devices where configurations.[[chillerdevicesettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='current'",
    "Error": "select deviceId from devices where configurations.[[chillerdevicesettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='error'",
    "Pending": "select deviceId from devices where configurations.[[chillerdevicesettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='pending'"
  }
}
```

Modüller için metrik sorgular da aygıtlar için sorguları `moduleId` `devices.modules`benzer, ancak seçim için seçin. Örnek: 

```json
{
  "queries": {
    "Compliant": "select deviceId, moduleId from devices.module where configurations.[[chillermodulesettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='current'"
  }
}
```

## <a name="create-a-configuration"></a>Yapılandırma oluşturma

Hedef içerik ve ölçümlerden oluşan bir yapılandırma oluşturarak hedef aygıtları yapılandırabilirsiniz. 

Yapılandırma oluşturmak için aşağıdaki komutu kullanın:

```azurecli
   az iot hub configuration create --config-id [configuration id] \
     --labels [labels] --content [file path] --hub-name [hub name] \
     --target-condition [target query] --priority [int] \
     --metrics [metric queries]
```

* --**config-id** - IoT hub'ında oluşturulacak yapılandırmanın adı. Yapılandırmanıza en fazla 128 küçük harf içeren benzersiz bir ad verin. Boşluklardan ve aşağıdaki geçersiz karakterlerden kaçının: `& ^ [ ] { } \ | " < > /`.

* --**etiketler** - Yapılandırmanızı izlemenize yardımcı olmak için etiket ekleyin. Etiketler, dağıtımınızı açıklayan Ad, Değer çiftleridir. Örneğin `HostPlatform, Linux` veya `Version, 3.0.1` olabilir.

* --**içerik** - Satır içi JSON veya hedef içeriğe dosya yolu ikiz istenilen özellikleri olarak ayarlanacak. 

* --**hub-name** - Yapılandırmanın oluşturulacağı IoT hub'ının adı. Hub geçerli abonelikte olmalıdır. Komutile istenilen aboneye geçme`az account set -s [subscription name]`

* --**hedef koşulu** - Bu yapılandırma ile hangi cihazların veya modüllerin hedefleneceğini belirlemek için bir hedef koşul girin.Otomatik aygıt yapılandırması için koşul, aygıt ikiz etiketlerini veya aygıt ikizi istenen özelliklere dayanır ve ifade biçimiyle eşleşmelidir.Örneğin `tags.environment='test'` veya `properties.desired.devicemodel='4000x'` olabilir.Otomatik modül yapılandırması için, durum modül ikiz etiketleri veya modül ikiz istenilen özelliklere dayanmaktadır... Örneğin `from devices.modules where tags.environment='test'` veya `from devices.modules where properties.reported.chillerProperties.model='4000x'` olabilir.

* --**öncelik** - Pozitif tamsayı. İki veya daha fazla yapılandırmanın aynı aygıta veya modüle hedef olması durumunda, Öncelik için en yüksek sayısal değere sahip yapılandırma geçerli olacaktır.

* --**ölçümler** - Dosya yolu metrik sorgular için. Ölçümler, bir aygıtın veya modülün yapılandırma içeriğini uyguladıktan sonra geri bildirebileceği çeşitli durumların özet sayımlarını sağlar. Örneğin, bekleyen ayarlar değişiklikleri için bir metrik, hatalar için bir metrik ve başarılı ayarlar için bir metrik oluşturabilirsiniz. 

## <a name="monitor-a-configuration"></a>Yapılandırmayı izleme

Yapılandırmanın içeriğini görüntülemek için aşağıdaki komutu kullanın:

```azurecli
az iot hub configuration show --config-id [configuration id] \
  --hub-name [hub name]
```

* --**config-id** - IoT hub'ında bulunan yapılandırmanın adı.

* --**hub-name** - Yapılandırmanın bulunduğu IoT hub'ının adı. Hub geçerli abonelikte olmalıdır. Komutile istenilen aboneye geçme`az account set -s [subscription name]`

Komut penceresindeki yapılandırmayı inceleyin.**Ölçümler** özelliği, her merkez tarafından değerlendirilen her metrik için bir sayım listeler:

* **targetedCount** - IoT Hub'daki aygıt ikizlerinin veya modül ikizlerinin sayısını belirleyen ve hedefleme koşuluyla eşleşen bir sistem ölçümü.

* **appliedCount** - Sistem metrik, hedef içeriğe uygulanan aygıt veya modül sayısını belirtir.

* **Özel ölçümleriniz** - Tanımladığınız tüm ölçümler kullanıcı ölçümleridir.

Aşağıdaki komutu kullanarak ölçümlerin her biri için aygıt kimliklerinin, modül kimliklerinin veya nesnelerin listesini gösterebilirsiniz:

```azurecli
az iot hub configuration show-metric --config-id [configuration id] \
   --metric-id [metric id] --hub-name [hub name] --metric-type [type] 
```

* --**config-id** - IoT hub'ında bulunan dağıtımın adı.

* --**metric-id** - Örneğin, `appliedCount`cihaz kimlikleri veya modül kimliklerinin listesini görmek istediğiniz metrik adı.

* --**hub-name** - Dağıtımın bulunduğu IoT hub'ının adı. Hub geçerli abonelikte olmalıdır. Komutu `az account set -s [subscription name]`ile istenilen aboneye geçin.

* --**metrik türü** - Metrik `system` türü `user`olabilir veya .  Sistem ölçümleri `targetedCount` `appliedCount`ve . Diğer tüm ölçümler kullanıcı ölçümleridir.

## <a name="modify-a-configuration"></a>Yapılandırmayı değiştirme

Bir yapılandırmayı değiştirdiğinizde, değişiklikler hemen tüm hedeflenen aygıtlara çoğalır. 

Hedef koşulu güncelleştirirseniz, aşağıdaki güncelleştirmeler oluşur:

* Bir ikiz eski hedef koşulu karşılamadı, ancak yeni hedef koşulu karşılar ve bu yapılandırma bu ikiz için en yüksek öncelik, o zaman bu yapılandırma uygulanır. 

* Şu anda bu yapılandırmayı çalıştıran bir ikiz artık hedef koşula uygun değilse, yapılandırmadaki ayarlar kaldırılır ve ikiz sonraki en yüksek öncelikli yapılandırma tarafından değiştirilir. 

* Şu anda bu yapılandırmayı çalıştıran bir ikiz artık hedef koşulu karşılamıyorsa ve diğer yapılandırmaların hedef koşulunu karşılamıyorsa, yapılandırmadaki ayarlar kaldırılır ve ikizüzerinde başka değişiklik yapılmaz. 

Yapılandırmayı güncelleştirmek için aşağıdaki komutu kullanın:

```azurecli
az iot hub configuration update --config-id [configuration id] \
   --hub-name [hub name] --set [property1.property2='value']
```

* --**config-id** - IoT hub'ında bulunan yapılandırmanın adı.

* --**hub-name** - Yapılandırmanın bulunduğu IoT hub'ının adı. Hub geçerli abonelikte olmalıdır. Komutu `az account set -s [subscription name]`ile istenilen aboneye geçin.

* --**set** - Yapılandırmadaki bir özelliği güncelleştirin. Aşağıdaki özellikleri güncelleştirebilirsiniz:

    * targetCondition - örneğin`targetCondition=tags.location.state='Oregon'`

    * Etiket 

    * Öncelik

## <a name="delete-a-configuration"></a>Yapılandırmayı silme

Bir yapılandırmayı sildiğinizde, herhangi bir aygıt ikizleri veya modül ikizleri bir sonraki en yüksek öncelikli yapılandırmalarını alırlar. İkizler başka bir yapılandırmanın hedef koşuluna uymuyorsa, başka bir ayar uygulanmaz. 

Yapılandırmayı silmek için aşağıdaki komutu kullanın:

```azurecli
az iot hub configuration delete --config-id [configuration id] \
   --hub-name [hub name] 
```

* --**config-id** - IoT hub'ında bulunan yapılandırmanın adı.

* --**hub-name** - Yapılandırmanın bulunduğu IoT hub'ının adı. Hub geçerli abonelikte olmalıdır. Komutu `az account set -s [subscription name]`ile istenilen aboneye geçin.

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
