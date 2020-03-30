---
title: Giriş IoT Tak ve Çalıştır Önizleme | Microsoft Dokümanlar
description: IoT Tak ve Çalıştır Önizleme si hakkında bilgi edinin. IoT Tak ve Çalıştır, IoT aygıtlarının yeteneklerini bildirmesini sağlayan açık bir modelleme diline dayanır. IoT aygıtları, Azure IoT Merkezi veya iş ortağı uygulamaları gibi bulut çözümlerine bağlandıklarında aygıt yetenek modeli adı verilen bu bildirimi sunar. Bulut çözümü daha sonra aygıtı otomatik olarak anlayabilir ve herhangi bir kod yazmadan onunla etkileşime başlayabilir.
author: ChrisGMsft
ms.author: chrisgre
ms.date: 12/23/2019
ms.topic: overview
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: 0399e1659fb7cc6a650c6b3c1d0189c8802d4904
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80064321"
---
# <a name="what-is-iot-plug-and-play-preview"></a>IoT Tak ve Çalıştır Önizleme nedir?

IoT Tak ve Çalıştır Önizleme, çözüm geliştiricilerinin cihazları gömülü kod yazmadan çözümleriyle tümleştirmelerine olanak tanır. IoT Tak ve Çalıştır'ın özünde cihaz özelliklerini açıklayan bir _aygıt yetenek modeli_ şeması yer alan bir model şeması yer alan bir model dir. Bu şema, aşağıdaki lerin tanımlarını içeren bir arabirim kümesi olarak yapılandırılan bir JSON belgesidir:

- Bir aygıtın veya diğer varlığın salt okuma ve okuma/yazma durumunu temsil eden _özellikler._ Örneğin, bir aygıt seri numarası salt okunur bir özellik olabilir ve termostattaki hedef sıcaklık okuma/yazma özelliği olabilir.
- _Telemetri,_ bir aygıt tarafından yayılan verilerdir, veri ister sensör okumaları düzenli bir akış, ara sıra bir hata veya bilgi iletisi olsun.
- Aygıtta yapılabilecek bir işlevi veya işlemi açıklayan _komutlar._ Örneğin, bir komut bir ağ geçidini yeniden başlatabilir veya uzak bir kamera kullanarak fotoğraf çekebilir.

İşbirliğini kolaylaştırmak ve geliştirmeyi hızlandırmak için aygıt özelliği modelleri arasında arabirimleri yeniden kullanabilirsiniz.

IoT Tak ve [Çalıştır'ın Azure Digital Twins](../digital-twins/about-digital-twins.md)ile sorunsuz çalışmasını sağlamak için, IoT Tak ve Çalıştır şeması [Dijital İkiz Tanımlı Dil (DTDL)](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL)kullanılarak tanımlanır. IoT Tak ve Çalıştır ve DTDL topluluğa açıktır ve Microsoft müşteriler, iş ortakları ve sektörle işbirliğini memnuniyetle karşılar. Her ikisi de JSON-LD ve RDF gibi açık W3C standartlarına dayanmaktadır ve bu standartlar, hizmetler ve takım lar arasında daha kolay benimsenmesini sağlar. Ayrıca, IoT Tak ve Çalıştır ve DTDL kullanmak için ekstra bir ücret yoktur. [Azure IoT Hub,](../iot-hub/about-iot-hub.md) [Azure IoT Merkezi](../iot-central/core/overview-iot-central.md)ve diğer Azure hizmetleri için standart ücretler aynı kalır.

IoT Hub veya IoT Central üzerine inşa edilen çözümler, IoT Tak ve Çalıştır'dan yararlanabilir.

Bu makalede ana hatları:

- IoT Tak ve Çalıştır kullanan bir projeyle ilişkili tipik roller.
- Uygulamanızda IoT Tak ve Çalıştır aygıtlarını nasıl kullanırsınız.
- Nasıl IoT Tak ve Çalıştır destekleyen bir IoT cihaz uygulaması geliştirmek için.
- Nasıl bir IoT Tak ve Çalıştır cihazı onaylamak ve [Sertifikalı IoT aygıt kataloğunda](https://catalog.azureiotsolutions.com/)yayımlamak için .

## <a name="user-roles"></a>Kullanıcı rolleri

IoT Tak ve Çalıştır iki tür geliştirici için yararlıdır:

- Bir _çözüm geliştiricisi,_ Azure IoT ve diğer Azure kaynaklarını kullanarak bir IoT çözümü geliştirmek ve tümleştirilemeyecek IoT aygıtlarını tanımlamaktan sorumludur.
- Aygıt _device developer_ geliştiricisi, çözümünüze bağlı bir aygıtta çalışan kodu oluşturur.

## <a name="use-iot-plug-and-play-devices"></a>IoT Tak ve Çalıştır cihazlarını kullanma

Bir çözüm geliştiricisi olarak, IoT Tak ve Çalıştır aygıtlarını kullanan bulut barındırılan bir IoT çözümü geliştirebilirsiniz. Aşağıdaki Azure hizmetlerinden birini kullanabilirsiniz:

- [IoT Central](../iot-central/core/overview-iot-central.md) - fiziksel ve dijital dünyaları birbirine bağlayan ürünler oluşturmayı kolaylaştıran, hizmet olarak tamamen yönetilen bir IoT yazılım çözümüdür.
- [IoT Hub - IoT](../iot-hub/about-iot-hub.md) uygulamanız ve cihazlarınız arasında güvenli, çift yönlü iletişim için bir ileti merkezi görevi gören yönetilen bir bulut hizmetidir.

IoT Tak ve Çalıştır cihazlarını Azure Sertifikalı IoT aygıt kataloğundan bulabilirsiniz. Katalogdaki her IoT Tak ve Çalıştır aygıtı doğrulandı ve bir aygıt yeteneği modeli var. Aygıtın işlevselliğini anlamak veya aygıtı Azure IoT Central'da simüle etmek için kullanmak için aygıt yetenek modelini görüntüleyin.

Bir IoT Tak ve Çalıştır aygıtıbağladığınızda, aygıt yetenek modelini, modelde yer alan arabirimleri ve bu arabirimlerde tanımlanan telemetri, özellikleri ve komutları görüntüleyebilirsiniz.

## <a name="develop-an-iot-device-application"></a>Bir IoT cihazı uygulaması geliştirme

Aygıt geliştiricisi olarak, IoT Tak ve Çalıştır'ı destekleyen bir IoT donanım ürünü geliştirebilirsiniz. İşlem iki önemli adım içerir:

1. Aygıt yetenek modelini ve arabirimlerini tanımlayın. [DTDL](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL)kullanarak cihazınızın yeteneklerini bildiren bir dizi JSON dosyası yazarsınız. Aygıt yeteneği modeli, fiziksel bir ürün gibi tam bir varlığı açıklar ve bu varlık tarafından uygulanan arabirim kümesini tanımlar. Arabirimler, bir aygıt tarafından desteklenen telemetriyi, özellikleri ve komutları benzersiz olarak tanımlayan paylaşılan sözleşmelerdir. Arabirimler farklı aygıt yetenek modelleri arasında yeniden kullanılabilir.

1. Aygıt yetenek modeli nde ve arabirimlerinde beyan edilen yetenekleri uygulayan aygıt yazılımını veya firmware'i yazar. Azure IoT SDK, aygıt özelliği modellerini uygulamak için API'ler içerir.

VS Kodu uzantısı [paketi için Azure IoT Araçları,](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) size yardımcı olacak birçok özellik sağlar. Örneğin, aygıt geliştiricisi olarak bir yetenek modelinden bir iskelet C projesi oluşturmak için bir uzantı kullanabilirsiniz. Ancak, aygıt yetenek modellerini yazarve uygulamak için herhangi bir IDE'yi kullanabilirsiniz.

## <a name="certify-an-iot-plug-and-play-device"></a>Bir IoT Tak ve Çalıştır cihazını sertifikaya bağlama

Aygıt geliştiricisi olarak, ioT donanım ürünlerini sertifikaiçin gönderebilirsiniz. Sertifikalı bir aygıtı Sertifikalı IoT aygıt kataloğunda yayınlayabilirsiniz. Sertifika süreci adımları şunlardır:

- Microsoft [İş Ortağı Ağı'na](https://partner.microsoft.com)katılın.
- Azure IoT için Sertifikalı portalına dahil.
- Yeni bir aygıt kaydı oluşturmak için bir IoT Tak ve Çalıştır aygıtı yetenek modeli ve pazarlama bilgileri gönderin.
- Aygıt için otomatik doğrulama testlerinden geçirin.
- Sertifikalı IoT cihaz kataloğuna yayınlayın.

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Genel önizleme sırasında, IoT Tak ve Çalıştır tüm bölgelerde kullanılabilir.

## <a name="message-quotas-in-iot-hub"></a>IoT Hub'daki ileti kotaları
Genel önizleme sırasında, IoT Tak ve Çalıştır aygıtları arabirim başına ayrı iletiler gönderir ve bu da [ileti kotanıza](../iot-hub/iot-hub-devguide-quotas-throttling.md)doğru sayılan ileti sayısını artırabilir.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi IoT Tak ve Çalıştır genel bir bakış var, önerilen bir sonraki adım quickstarts birini denemektir:

- [IoT Tak ve Çalıştır aygıtı oluşturmak için aygıt yeteneği modelini kullanma](./quickstart-create-pnp-device-windows.md)
- [IoT Hub’a cihaz bağlama](./quickstart-connect-pnp-device-c-windows.md)
- [Çözümünüzde bir cihaza bağlanma](./quickstart-connect-pnp-device-solution-node.md)
