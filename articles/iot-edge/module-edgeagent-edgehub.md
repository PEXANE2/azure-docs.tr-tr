---
title: Aracı ve hub modülü ikizlerin özellikleri - Azure IoT Edge
description: edgeAgent ve edgeHub modülü ikizleri için belirli özellikleri ve değerlerini gözden geçirin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/17/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: f2d6603c264c9da3f2700f460a8c61b24681fac6
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546198"
---
# <a name="properties-of-the-iot-edge-agent-and-iot-edge-hub-module-twins"></a>IoT Edge aracısının ve IoT Edge hub modülü ikizlerinin özellikleri

IoT Edge aracısı ve IoT Edge hub'ı, IoT Edge çalışma süresini oluşturan iki modüldür. Her çalışma zamanı modülünün sorumlulukları hakkında daha fazla bilgi için azure [IoT Edge çalışma süresini ve mimarisini anlayın.](iot-edge-runtime.md)

Bu makalede, çalışma zamanı modülü ikiz istenilen özellikleri ve bildirilen özelliklerini sağlar. IoT Edge cihazlarında modüllerin nasıl dağıtılanması hakkında daha fazla bilgi için [bkz.](module-composition.md)

Bir modül ikiz içerir:

* **İstenilen özellikler**. Çözüm arka uç istenilen özellikleri ayarlayabilirsiniz ve modül bunları okuyabilir. Modül ayrıca istenilen özelliklerdeki değişikliklerin bildirimlerini de alabilir. İstenilen özellikler, modül yapılandırmasını veya koşullarını eşitlemek için bildirilen özelliklerle birlikte kullanılır.

* **Bildirilen özellikler.** Modül bildirilen özellikleri ayarlayabilir ve çözüm arka uç bunları okuyabilir ve sorgulayabilir. Bildirilen özellikler, modül yapılandırmasını veya koşullarını eşitlemek için istenilen özelliklerle birlikte kullanılır.

## <a name="edgeagent-desired-properties"></a>EdgeAgent istenilen özellikler

IoT Edge aracısının modül ikizi çağrılır `$edgeAgent` ve bir aygıtta çalışan IoT Edge aracısı ile IoT Hub arasındaki iletişimi koordine eder. İstenilen özellikler, tek bir aygıtın veya ölçekdüzeyinde dağıtımın bir parçası olarak belirli bir aygıta dağıtım bildirimi uygulanırken ayarlanır.

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| şemaVersiyon | "1.0" olmalı. | Evet |
| runtime.type | "Docker" olmalı. | Evet |
| runtime.settings.minDockerVersion | Bu dağıtım bildiriminin gerektirdiği minimum Docker sürümüne ayarlayın | Evet |
| runtime.settings.loggingOptions | IoT Edge aracı kapsayıcısı için günlüğe kaydetme seçeneklerini içeren stringified JSON. [Docker günlüğe kaydetme seçenekleri](https://docs.docker.com/engine/admin/logging/overview/) | Hayır |
| runtime.settings.registryCredentials<br>. {registryId}.kullanıcı adı | Kapsayıcı kayıt defterinin kullanıcı adı. Azure Kapsayıcı Kayıt Defteri için kullanıcı adı genellikle kayıt defteri adıdır.<br><br> Kayıt defteri kimlik bilgileri herhangi bir özel modül görüntüleri için gereklidir. | Hayır |
| runtime.settings.registryCredentials<br>. {registryId}.password | Kapsayıcı kayıt defterinin parolası. | Hayır |
| runtime.settings.registryCredentials<br>. {registryId}.adresi | Konteyner kayıt defterinin adresi. Azure Kapsayıcı Kayıt Defteri için adres genellikle *{kayıt defteri adı}.azurecr.io'dur.* | Hayır |  
| systemModules.edgeAgent.type | "Docker" olmalı. | Evet |
| systemModules.edgeAgent.settings.image | IoT Edge aracısının görüntüsünün URI'si. Şu anda, IoT Edge aracısı kendisini güncelleştiremez. | Evet |
| systemModules.edgeAgent.settings<br>.createOptions | IoT Edge aracı kapsayıcısının oluşturulması için seçenekleri içeren dizilmiş JSON. [Docker oluşturma seçenekleri](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Hayır |
| systemModules.edgeAgent.configuration.id | Bu modülü dağıtan dağıtımın kimliği. | IoT Hub, bildirim dağıtım kullanılarak uygulandığında bu özelliği ayarlar. Dağıtım bildiriminin bir parçası değil. |
| systemModules.edgeHub.type | "Docker" olmalı. | Evet |
| systemModules.edgeHub.status | "Çalışıyor" olmalı | Evet |
| systemModules.edgeHub.restartPolicy | "Her zaman" olmalı. | Evet |
| systemModules.edgeHub.settings.image | IoT Edge hub'ının görüntüsünün URI'si. | Evet |
| systemModules.edgeHub.settings<br>.createOptions | IoT Edge hub kapsayıcısının oluşturulması için seçenekleri içeren dizilmiş JSON. [Docker oluşturma seçenekleri](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Hayır |
| systemModules.edgeHub.configuration.id | Bu modülü dağıtan dağıtımın kimliği. | IoT Hub, bildirim dağıtım kullanılarak uygulandığında bu özelliği ayarlar. Dağıtım bildiriminin bir parçası değil. |
| Modül. {moduleId}.version | Bu modülün sürümünü temsil eden kullanıcı tanımlı bir dize. | Evet |
| Modül. {moduleId}.type | "Docker" olmalı. | Evet |
| Modül. {moduleId}.status | {"çalışan" \| "durduruldu"} | Evet |
| Modül. {moduleId}.restartPolicy | {"never" \| "on-failure" \| "on-sağlıksız" \| "her zaman"} | Evet |
| Modül. {moduleId}.imagePullPolicy | {"on-create" \| "never"} | Hayır |
| Modül. {moduleId}.env | Modüle geçirilen ortam değişkenlerinin listesi. Biçimi alır`"<name>": {"value": "<value>"}` | Hayır |
| Modül. {moduleId}.settings.image | Uri modül görüntüsüne. | Evet |
| Modül. {moduleId}.settings.createOptions | Modül kapsayıcısının oluşturulması için seçenekleri içeren dizilmiş Bir JSON. [Docker oluşturma seçenekleri](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate) | Hayır |
| Modül. {moduleId}.configuration.id | Bu modülü dağıtan dağıtımın kimliği. | IoT Hub, bildirim dağıtım kullanılarak uygulandığında bu özelliği ayarlar. Dağıtım bildiriminin bir parçası değil. |

## <a name="edgeagent-reported-properties"></a>EdgeAgent bildirilen özellikleri

IoT Edge aracısı bildirilen özellikleri üç ana bilgi parçaları içerir:

1. Son görülen istenilen özelliklerin uygulama durumu;
2. IoT Edge aracısı tarafından bildirilen, aygıtta şu anda çalışan modüllerin durumu; Ve
3. Aygıtta şu anda çalışan istenen özelliklerin bir kopyası.

Geçerli istenen özelliklerin kopyası, aygıtın en son dağıtımı uygulayıp uygulamadığını veya önceki bir dağıtım bildirimini hala çalıştırıp çalıştırmadığını söylemek için yararlıdır.

> [!NOTE]
> IoT Edge aracısının bildirilen özellikleri, ölçekteki dağıtımların durumunu araştırmak için [IoT Hub sorgu diliyle](../iot-hub/iot-hub-devguide-query-language.md) sorgulanabileceğinden yararlıdır. Durum için IoT Edge aracı özelliklerini nasıl kullanacağıhakkında daha fazla bilgi için [bkz.](module-deployment-monitoring.md)

Aşağıdaki tablo, istenen özelliklerden kopyalanan bilgileri içermez.

| Özellik | Açıklama |
| -------- | ----------- |
| lastDesiredVersion | Bu tamsayı, IoT Edge aracısı tarafından işlenen istenen özelliklerin son sürümünü ifade eder. |
| lastDesiredStatus.code | Bu durum kodu, IoT Edge aracısı tarafından görülen son istenen özellikleri ifade eder. İzin verilen `200` değerler: `400` Başarı, `412` Geçersiz yapılandırma, Geçersiz `417` şema sürümü, `500` istenen özellikler boş, Başarısız |
| lastDesiredStatus.description | Durumun metin açıklaması |
| deviceSağlık | `healthy`tüm modüllerin çalışma süresi durumu `running` ya `stopped` `unhealthy` da , aksi takdirde |
| configurationHealth. {deploymentId}.health | `healthy`dağıtım {deploymentId} tarafından ayarlanan tüm modüllerin çalışma `running` süresi `stopped` `unhealthy` durumu veya |
| runtime.platform.OS | Aygıtta çalışan işletim sistemi raporlama |
| runtime.platform.architecture | Aygıttaki CPU mimarisinin bildirin |
| systemModules.edgeAgent.runtimeDurum | IoT Edge aracısının bildirilen durumu: \| {"çalışan" "sağlıksız"} |
| systemModules.edgeAgent.statusDescription | IoT Edge aracısının bildirilen durumunun metin açıklaması. |
| systemModules.edgeHub.runtimeDurum | IoT Edge hub'ının durumu: \| { \| "çalışan" \| "durduruldu" \| "başarısız" "geri tepme" "sağlıksız" } |
| systemModules.edgeHub.statusDescription | Sağlıksızsa IoT Edge hub'ının durumunun metin açıklaması. |
| systemModules.edgeHub.exitCode | Konteyner çıkarsa IoT Edge hub konteyneri tarafından bildirilen çıkış kodu |
| systemModules.edgeHub.startTimeUtc | IoT Edge hub'ın en son başlatıldığı saat |
| systemModules.edgeHub.lastExitTimeUtc | IoT Edge hub'ın en son çıktığı saat |
| systemModules.edgeHub.lastRestartTimeUtc | IoT Edge hub'ın en son yeniden başlatıldığı saat |
| systemModules.edgeHub.restartCount | Bu modülün yeniden başlatma ilkesinin bir parçası olarak kaç kez yeniden başlatıldı. |
| Modül. {moduleId}.runtimeDurum | \| Modülün durumu: { "running" \| "durduruldu" \| "başarısız" \| "geri tepme" "sağlıksız" } |
| Modül. {moduleId}.statusDescription | Sağlıksız ise modülün durumunun metin açıklaması. |
| Modül. {moduleId}.exitCode | Konteyner çıkarsa modül konteyneri tarafından bildirilen çıkış kodu |
| Modül. {moduleId}.startTimeUtc | Modülün en son başlatıldıı saat |
| Modül. {moduleId}.lastExitTimeUtc | Modülün en son çıktığı saat |
| Modül. {moduleId}.lastRestartTimeUtc | Modülün en son yeniden başlatıldıı saat |
| Modül. {moduleId}.restartCount | Bu modülün yeniden başlatma ilkesinin bir parçası olarak kaç kez yeniden başlatıldı. |

## <a name="edgehub-desired-properties"></a>EdgeHub istenilen özellikler

IoT Edge hub'ı için `$edgeHub` modül ikizi çağrılır ve bir aygıtta çalışan IoT Edge hub'ı ile IoT Hub arasındaki iletişimi koordine eder. İstenilen özellikler, tek bir aygıtın veya ölçekdüzeyinde dağıtımın bir parçası olarak belirli bir aygıta dağıtım bildirimi uygulanırken ayarlanır.

| Özellik | Açıklama | Dağıtım bildiriminde gerekli |
| -------- | ----------- | -------- |
| şemaVersiyon | "1.0" olmalı. | Evet |
| Yol. {routeName} | IoT Edge hub rotasını temsil eden bir dize. Daha fazla bilgi [için, rotaları Bildir'e](module-composition.md#declare-routes)bakın. | Öğe `routes` var olabilir ama boş. |
| storeAndForwardConfiguration.timeToLiveSecs | IoT Edge hub'ınsaniye ler içinde zaman, ioT Hub veya yerel bir modül olsun, yönlendirme uç noktalarından bağlantısı kesilirse iletileri tutar. Değer herhangi bir pozitif tamsayı olabilir. | Evet |

## <a name="edgehub-reported-properties"></a>EdgeHub bildirilen özellikleri

| Özellik | Açıklama |
| -------- | ----------- |
| lastDesiredVersion | Bu tamsayı, IoT Edge hub'ı tarafından işlenen istenen özelliklerin son sürümünü ifade eder. |
| lastDesiredStatus.code | IoT Edge hub'ı tarafından görülen son istenen özelliklere atıfta bulunan durum kodu. İzin verilen `200` değerler: `400` Başarı, `500` Geçersiz yapılandırma, Başarısız |
| lastDesiredStatus.description | Durumun metin açıklaması. |
| Istemci. {aygıt veya moduleId}.status | Bu aygıtın veya modülün bağlantı durumu. Olası değerler {"bağlı" \| "bağlantısız"}. Yalnızca modül kimlikleri bağlantısı kesilebilir. IoT Edge hub'ına bağlanan aşağı akım aygıtları yalnızca bağlı yken görünür. |
| Istemci. {aygıt veya moduleId}.lastConnectTime | Cihaz veya modül en son bağlanıncı. |
| Istemci. {aygıt veya moduleId}.lastDisconnectTime | En son aygıt veya modül bağlantısı kesildiğinde. |

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım bildirimlerini oluşturmak için bu özelliklerinasıl kullanacağınızı öğrenmek için [bkz.](module-composition.md)
