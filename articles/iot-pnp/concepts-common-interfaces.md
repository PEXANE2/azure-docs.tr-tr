---
title: Ortak arayüzler - IoT Tak ve Çalıştır Önizleme | Microsoft Dokümanlar
description: IoT Tak ve Çalıştır geliştiricileri için ortak arabirimlerin açıklaması
author: ChrisGMsft
ms.author: chrisgre
ms.date: 12/26/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: c6ac90f917b9afc37b3a39d8da679fbcad091778
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80234699"
---
# <a name="iot-plug-and-play-preview-common-interfaces"></a>IoT Tak ve Çalıştır Preview ortak arayüzleri

Tüm IoT Tak ve Çalıştır aygıtlarının bazı ortak arabirimleri uygulaması beklenmektedir. Ortak arabirimler, tutarlı işlevsellik sağladıkları için IoT çözümlerine yarar sağlar. [Sertifikasyon,](tutorial-build-device-certification.md) cihazınızın birkaç ortak arabirim uygulamasını gerektirir. Ortak model deposundan ortak arabirim tanımlarını alabilirsiniz.

## <a name="summary-of-common-interfaces"></a>Ortak arabirimlerin özeti

| Adı | Kimlik | Açıklama | Azure IoT SDK tarafından uygulandı | Yetenek modelinde beyan edilmelidir |
| -------- | -------- | -------- | -------- | -------- | -------- |
| Model Bilgileri | vazo:azureiot:ModelDiscovery:ModelInformation:1 | Aygıtların yetenek modeli kimliğini ve arabirimlerini beyan etmesi için. Tüm IoT Tak ve Çalıştır aygıtları için gereklidir. | Evet | Hayır |
| Dijital İkiz İstemci SDK Bilgileri | urn:azureiot:İstemci:SDKInformation:1 | Aygıtı Azure'a bağlamak için istemci SDK. [Belgelendirme](tutorial-build-device-certification.md) için gerekli | Evet | Hayır |
| Cihaz bilgileri | urn:azureiot:DeviceManagement:DeviceInformation:1 | Aygıt la ilgili donanım ve işletim sistemi bilgileri. [Belgelendirme](tutorial-build-device-certification.md) için gerekli | Hayır | Evet |
| Model Tanımı | vazo:azureiot:ModelDiscovery:ModelDefinition:1 | Aygıtların yetenek modeli ve arabirimleri için tam tanımı beyan etmesi için. Model tanımları bir model deposunda barındırılmadığında uygulanmalıdır. | Hayır | Evet |
| Dijital İkiz | vazo:azureiot:ModelDiscovery:DigitalTwin:1 | Çözüm geliştiricileriçin bir dijital ikiz için yetenek modeli kimliği ve arayüz kimlikleri almak için. Bu arabirim bir IoT Tak ve Çalıştır aygıtı tarafından beyan edilmemiş veya uygulanmaz. | Hayır | Hayır |

- Azure IoT SDK tarafından uygulanan - Azure IoT SDK'nın arabirimlerde beyan edilen yetenekleri uygulayıp uygulamadığı. Azure IoT SDK kullanan IoT Tak ve Çalıştır aygıtlarının bu arabirimi uygulaması gerekmez.
- Yetenek modelinde beyan edilmelidir - 'evet' ise, bu `"implements":` arabirim bu IoT Tak ve Çalıştır aygıtı için aygıt yetenek modeli bölümünde bildirilmelidir.

## <a name="retrieve-interface-definitions-from-the-public-repository"></a>Ortak depodan arabirim tanımlarını alma

### <a name="cli"></a>CLI

Ortak model deposundan ortak arabirimleri almak için Azure CLI için Azure IoT uzantısını kullanabilirsiniz.

```azurecli
az iot pnp interface show --interface {InterfaceID}
```

```azurecli
az iot pnp capability-model show --model {ModelID}
```

### <a name="vs-code"></a>VS Code

1. Komut paletini açmak için **Ctrl+Shift+P'yi** kullanın.

1. **Tak ve Çalıştır'ı** girin ve ardından **IoT Tak ve Çalıştır: Model Deposu** komutunu açın' ı seçin. **Genel depo seçin.** Genel model deposu VS Code'da açılır.

1. Ortak model deposuna, arama alanına arabirim adını girin.

1. Arabirimin yerel bir kopyasını oluşturmak için, arama sonuçlarında onu seçin ve sonra **İndir'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Artık ortak arabirimler hakkında bilgi edindiğinize göre, bazı ek kaynaklar şunlardır:

- [Dijital İkiz Tanımlı Dil (DTDL)](https://aka.ms/DTDL)
- [C cihaz API’si](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
- [IoT REST API](https://docs.microsoft.com/rest/api/iothub/device)
