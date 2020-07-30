---
title: Yenilikler IoT Tak ve Kullan önizlemeyi yenileme | Microsoft Docs
description: IoT Tak ve Kullan önizleme yenileme sürümü ile yenilikleri öğrenin.
author: rido-min
ms.author: rmpablos
ms.date: 07/06/2020
ms.topic: overview
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: eliotgra
ms.openlocfilehash: f212143d90b6a6bf9c8f1f597c276b9ab4100617
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87406269"
---
# <a name="iot-plug-and-play-preview-refresh"></a>IoT Tak ve Kullan önizlemeyi yenileme

Bu makalede, Tak ve Kullan IoT 'de SDK 'LARı, kitaplıkları, araçları ve Hizmetleri, Temmuz 2020 ' de önizleme yenileme yayını ' nda bulunan temel değişiklikler açıklanmaktadır. Önceki IoT Tak ve Kullan önizleme sürümü Ağustos 2019 ' de.

## <a name="digital-twins-definition-language-dtdl"></a>Dijital TWINS tanım dili (DTDL)

Bu sürüm [dtdl v2](https://github.com/Azure/opendigitaltwins-dtdl) desteği ekler ve [dtdl v1](https://github.com/Azure/opendigitaltwins-dtdl/tree/master/DTDL/v1-preview)'yi kullanımdan kaldırır.

Aşağıdaki listede DTDL v1 ve DTDL v2 arasındaki temel farklılıklar gösterilmektedir. DTDL v2 'de:

- Model kimlikleri yerine öneki vardır `dtmi` `urn` . Digital ikizi model tanımlayıcıları (DTMı) `urn` DTDL v1 'de kullanılan önekli dijital Ikizi kimliklerini değiştirir. Sürüm artık daha önce bir `;` . Örneğin, daha önce kullandığınız yerde `urn:CompanyName:Model:1` , artık kullanıyorsunuz `dtmi:CompanyName:Model;1` .
- `@context`Yerine olarak ayarlayın `dtmi:dtdl:context;2` `http://azureiot.com/v1/contexts/IoTModel.json` .
- Bir cihazı modellemek için, **Capabilitymodel** türleri yerine **arabirim** türünü kullanın.
- **Bileşenler** **arabirim** örneklerinin yerini alır. **İlişki** ve **bileşenleri** , bir **arabirim**içeriğinin bir parçası olarak tanımlayabilirsiniz.
- **Arabirim** , başka bir **arabirimden**devralınabilir.
- _Genişletilebilir anlam türlerini_kullanarak dtdl 'yi genişletebilirsiniz. Bu Genişletilebilir tür sistemi, DTDL v1 içindeki sıcaklık ve nem gibi sabit kodlanmış semantik türlerden daha fazla esneklik sunar.
- **Displayunit** özelliği kaldırılmıştır.
- Bir adda öndeki veya sondaki alt çizgileri kullanamazsınız. Bir adın önde gelen alt çizgileri sistem kullanımı için ayrılmıştır.

DTDL v1 ile çalışmak için SDK 'nın önceki sürümünü ve Azure IoT Gezgini 0.10. x ' i kullanmanız gerekir. DTDL v2 ile çalışmak için SDK 'nın en son sürümüne ve Azure IoT Explorer 0.11. x ' e ihtiyacınız vardır.

## <a name="no-component-and-multiple-component-implementations"></a>Bileşen ve birden çok bileşen uygulaması yok

Birkaç telemetri, komut veya özellik kullanan basit aygıtlar, bileşenleri kullanılmadan tek bir arabirimde açıklanabilir. Varolan cihaz uygulamasında herhangi bir değişiklik yapılmadan **model kimliğini** duyurarak mevcut herhangi bir cihaz bir IoT Tak ve kullan olabilir.

Daha karmaşık cihazlar karmaşıklığı yönetmek ve cihazlarda yeniden kullanımını etkinleştirmek için farklı arabirimlerde telemetri, komut ve özellikleri gruplandırabilir. Bu cihazların [ıot Tak ve kullan önizleme iletisi](concepts-convention.md)kurallarında tanımlanan basit kuralların bir kümesini izlemesi için güncelleştirilmeleri gerekir.

## <a name="registration-and-discovery"></a>Kayıt ve bulma

Bu sürümde, cihazlar her bağlantıda IoT Hub **model kimliğini** duyurur. IoT Hub, Device ikizi özelliğini kullanarak **model kimliğini** almak için arka uç çözümlerini ETKINLEŞTIREN **model kimliğini** önbelleğe alır `modelId` . **Model kimliği** , dijital ikizi içinden de alınabilir `$metadata.$model` .

## <a name="microsoft-defined-interfaces"></a>Microsoft tanımlı arabirimler

Aşağıdaki Microsoft tarafından tanımlanan arabirimler kullanım dışıdır ve yeni model deposunda yayımlanmaz:

- **urn: azureiot: ModelDiscovery: DigitalTwin: 1**
- **urn: azureiot: ModelDiscovery: ModelInformation: 1**

Şu arabirim yeni model deposunda yayımlandı: `dtmi:azure:DeviceManagement:DeviceInformation;1` URL 'de kullanılabilir [ https://repo.azureiotrepository.com/Models/dtmi:azure:DeviceManagement:DeviceInformation ; 1? api-Version = 2020-05 -01-Preview](https://repo.azureiotrepository.com/Models/dtmi:azure:DeviceManagement:DeviceInformation;1?api-version=2020-05-01-preview).

## <a name="digitaltwinchangeevents"></a>DigitalTwinChangeEvents

**Digitaltwınchangeevents** [olay kaynağının](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events) olay yapısı **JSON-Patch** biçimini kullanacak şekilde değiştirildi. Bu değişiklik, geri uyumluluk desteği olmayan bir son değişiklik değildir.

## <a name="message-routing"></a>İleti yönlendirme

Telemetri iletilerinin [SystemProperties](../iot-hub/iot-hub-devguide-messages-construct.md) koleksiyonu içinde aşağıdaki değişiklikleri vardır.

Artık cihaz tarafından kaydedilen **model kimliği** olan bir **dt-DataSchema** özelliği içerir.

**Dt-Subject** özelliği, telemetri iletisi gönderen bileşeni temsil eder.

**Iothub-Interface-Name** özelliği kullanım dışıdır.

## <a name="device-and-service-sdks"></a>Cihaz ve hizmet SDK’ları

SDK 'ların önceki önizleme sürümleriyle geriye dönük bir uyumluluk yoktur. Bir SDK 'nın en son önizleme sürümüne geçtiğinizde kodunuzu değiştirmeniz gerekir.

Kural tabanlı yaklaşımla, ayrı cihaz istemci SDK 'larına gerek yoktur. Bu önizleme sürümünden, mevcut **Digitaltwınclient** kitaplıkları tüm dillerde kullanım dışıdır. Bunun yerine, IoT Hub cihaz istemci SDK 'Ları, **model kimliğini**duyurmak için bir seçenek içerecek şekilde güncelleştirilmiştir.

Bileşenleri kullanmayan cihazlarda minimum kod değişikliği yapılması gerekir. yalnızca **model kimliği**duyuruldu. Birden çok bileşen kullanan daha karmaşık cihazlar, [kuralları](concepts-convention.md)uygulamak için bazı yeniden kullanılabilir işlevler gerektirebilir. Cihaz örnekleri, cihaz uygulamanızda yeniden kullanabileceğiniz bir işlevler kümesi içerir.

### <a name="service-sdks"></a>Hizmet SDK'ları

Service SDK [Node.js](https://github.com/Azure/azure-iot-sdk-node/blob/digitaltwins-preview/digitaltwins/service/readme.md) ve [Python](https://github.com/Azure/azure-iot-sdk-python/blob/digitaltwins-preview/azure-iot-hub/README.md)'da kullanılabilir.

## <a name="vs-code-extension"></a>VS Code uzantısı

[Azure IoT cihaz çalışma ekranı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) uzantısı, dtdl v1 için yazma desteği, model deposunun önceki sürümüyle tümleştirme ve kod oluşturma desteği sağlar.

VS Code 'de DTDL v2 yazma desteği gerekiyorsa, yeni [dtdl uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-dtdl) vs Code ' ye yükleyebilirsiniz. Uzantı, model deposu veya kod oluşturma ile tümleştirme sağlamaz. Depodaki modelleri yönetmek artık bir [Web Kullanıcı arabirimi](https://aka.ms/iotmodelrepo)kullanılarak yapılır.

## <a name="digital-twin-service-side-rest-apis"></a>Digital ikizi hizmet tarafı REST API 'Leri

[Digital ikizi hizmet tarafı REST API 'leri](https://docs.microsoft.com/rest/api/iothub/service/digitaltwin) ve yükleri değişmiştir. Desteklenen API 'Ler şunlardır:

- Dijital ikizi alın.
- Bir komut çağırın.
- **JSON Patch** yükünü kullanarak dijital ikizi güncelleştirin.

Mevcut REST API 'Leri bu sürümde desteklenmeye devam eder.

## <a name="model-repository"></a>Model deposu

Artık hem genel yayımlanan modelleri hem de özel RBAC korumalı şirket modellerini içeren tek bir model deposu vardır. Tüm modeller benzersiz bir tanımlayıcıya sahiptir ve oluşturulduktan sonra sabittir.

Önceki sürümden mevcut şirket modeli depoları bu sürümde desteklenmez. Eski DTDL v1 modellerini yönetmek için [Azure Sertifikalı IoT](https://preview.catalog.azureiotsolutions.com/products) Web sitesini kullanmaya devam edebilirsiniz. Ancak, artık bu Web sitesini kullanarak cihazları kaydedebilir, test edebilir ve bunları onaylayamaz.

Azure CLı için Azure IoT uzantısı, yeni model deposunu desteklemez. `az iot pnp`Komutlar yalnızca önceki sürümden model depoları çalışır.

## <a name="azure-iot-central"></a>Azure IoT Central

Azure IoT Central Şu anda IoT Tak ve Kullan önizleme yenileme sürümünü destekleyecek şekilde güncelleştiriliyor.
