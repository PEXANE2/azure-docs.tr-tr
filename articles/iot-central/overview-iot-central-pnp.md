---
title: Azure IoT Central nedir? | Microsoft Docs
description: Azure IoT Central, özel IoT çözümünüzü derlemek ve yönetmek için kullanabileceğiniz bir uçtan uca SaaS çözümüdür. Bu makalede Azure IoT Central’ın özelliklerine genel bir bakış sunulmaktadır.
author: dominicbetts
ms.author: dobett
ms.date: 07/06/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: 0675be988e7e9560560dd07338563c9fa22fcb4c
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878295"
---
# <a name="what-is-azure-iot-central-preview-features"></a>Azure IoT Central (Önizleme özellikleri) nedir?

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

> [!WARNING]
> Azure IoT Central [ıot Tak ve kullan](https://aka.ms/iot-pnp-docs) özellikleri şu anda genel önizlemededir. Üretim iş yükleri için IoT Tak ve Kullan özellikli IoT Central uygulaması kullanmayın. Üretim ortamları için, geçerli, genel olarak kullanılabilir bir uygulama şablonundan oluşturulan bir IoT Merkezi uygulaması kullanın.

Azure IoT Central, fiziksel ve dijital çalışma LDS 'yi bağlayan ürünler oluşturmayı kolaylaştıran, tam olarak yönetilen bir hizmet olarak yazılım çözümüdür. Aşağıdakileri yaparak bağlı ürününüzün vizyonunu hayata geçirebilirsiniz:

- Müşterileriniz için daha iyi ürün ve deneyimleri etkinleştirmek amacıyla bağlı cihazlardan yeni içgörüler edinme.
- Kuruluşunuz için yeni iş fırsatları oluşturma.

Azure IoT Central, tipik bir IoT projesiyle karşılaştırıldığında:

- Yönetim yükünü azaltır.
- İşlem maliyetlerini ve fazla kafaları azaltır.
- , İle çalışırken uygulamanızı özelleştirmeyi kolaylaştırır:
  - [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) ve [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/) gibi sektör lideri teknolojiler.
  - Uçtan uca şifreleme gibi kurumsal düzeyde güvenlik özellikleri.

Aşağıdaki video Azure IoT Central’a genel bir bakış sunar:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Microsoft-IoT-Central-intro-walkthrough/Player]

Bu makalede, Azure IoT Central için özetlenmektedir:

- Bir projeyle ilişkili tipik kişilikler.
- Uygulamanızı oluşturma.
- Cihazlarınızı uygulamanıza bağlama
- Uygulamanızı yönetme.

## <a name="known-issues"></a>Bilinen sorunlar

> [!Note]
> Bu bilinen sorunlar yalnızca IoT Central önizleme uygulaması için geçerlidir.

- Kurallar tüm eylemleri desteklemez (yalnızca e-posta).
- Karmaşık türler için-kurallar, analiz ve cihaz grupları desteklenmez.
- Sürekli veri dışa aktarma avro biçimini (uyumsuzluk) desteklemez.
- Sanal cihazlar tüm karmaşık türleri desteklemez.
- GeoJSON Şu anda desteklenmiyor.
- Harita kutucuğu Şu anda desteklenmiyor.
- İşler karmaşık türleri desteklemez.
- Dizi şeması türleri desteklenmez.
- Uygulama şablonu dışarı aktarma ve uygulama kopyalama desteklenmez.
- Yalnızca C cihaz SDK 'Sı ve Node. js cihaz ve hizmet SDK 'Ları desteklenir.
- Yalnızca belirli bölgelerde kullanılabilir.

## <a name="personas"></a>Kişilikler

Azure IoT Central belgeleri, Azure IoT Central uygulamasıyla etkileşime geçen dört kişilik anlamına gelir:

- _Oluşturucu_, uygulamaya bağlanan cihaz türlerini tanımlamaktan ve uygulamayı operatör için özelleştirmekten sorumludur.
- _Operatör_, uygulamaya bağlı cihazları yönetir.
- _Yönetici_, uygulamanın içindeki kullanıcı ve rolleri yönetme gibi yönetim görevlerinden sorumludur.
- _Cihaz geliştiricisi_, uygulamanıza bağlı bir cihaz üzerinde çalışan kodu oluşturur.

## <a name="create-your-azure-iot-central-application"></a>Azure IoT Central uygulamanızı oluşturma

Oluşturucu olarak Azure IoT Central’ı, kuruluşunuz için özel, bulutta barındırılan bir IoT çözümü oluşturmak için kullanırsınız. Özel bir IoT çözümü genellikle aşağıdakilerden oluşur:

- Cihazlarınızdan telemetri alan ve bu cihazları yönetmenizi sağlayan bulut tabanlı bir uygulama.
- Bulut tabanlı uygulamanıza bağlı özel kod çalıştıran birden fazla cihaz.

Yeni bir Azure IoT Central uygulamasını hızlıca dağıtabilir ve sonra tarayıcınızda özel gereksinimlerinize göre özelleştirebilirsiniz. Bir Oluşturucu olarak, uygulamanıza bağlanan cihazlar için bir _cihaz şablonu_ oluşturmak üzere Web tabanlı araçları kullanın. Cihaz şablonu, şöyle bir cihaz türünün özelliklerini ve davranışını tanımlayan şema ' dir:

- Gönderdiği telemetri.
- Bir operatörün değiştirebileceği iş özellikleri.
- Bir cihaz tarafından ayarlanan ve uygulamada salt okunur özellikte olan cihaz özellikleri.
- Cihazın davranışını tanımlayan bir operatör tarafından ayarlanan özellikler.

Bu cihaz şablonu şunları içerir:

- Bir cihazın gönderdiği telemetri ve rapor aldığı özellikler gibi uygulaması gereken özellikleri açıklayan bir _cihaz yetenek modeli_ .
- Cihazda depolanmayan bulut özellikleri.
- IoT Central uygulamanızın parçası olan özelleştirmeler, panolar ve formlar.

### <a name="create-device-templates"></a>Cihaz şablonları oluşturma

[Iot Tak ve kullan](https://aka.ms/iot-pnp-docs) , hiçbir katıştırılmış cihaz kodu yazmadan IoT Central cihazları tümleştirmenize olanak sağlar. IoT Tak ve Kullan 'nin çekirdeği, cihaz yeteneklerini açıklayan bir cihaz yetenek modeli şemadır. IoT Central önizleme uygulamasında, cihaz şablonları bu IoT Tak ve Kullan cihaz yeteneği modellerini kullanır.

Bir Oluşturucu olarak, cihaz şablonları oluşturmak için çeşitli seçenekleriniz vardır:

- IoT Central cihaz şablonunu tasarlayın ve cihaz kodunuzda cihaz yetenek modelini uygulayın.
- [IoT cihaz kataloğu Için Azure Sertifikalı](https://aka.ms/iotdevcat) bir cihaz yetenek modeli içeri aktarın ve ardından IoT Central uygulamanızın ihtiyaç duyacağı tüm bulut özelliklerini, özelleştirmeleri ve panoları ekleyin.
- Visual Studio Code 'u kullanarak bir cihaz yetenek modeli oluşturun. Modelden cihaz kodunuzu uygulayın ve cihazınızı IoT Central uygulamanıza bağlayın. IoT Central, cihaz yetenek modelini bir depodan bulur ve sizin için basit bir cihaz şablonu oluşturur.
- Visual Studio Code 'u kullanarak bir cihaz yetenek modeli oluşturun. Modelden cihaz kodunuzu uygulayın. Cihaz yetenek modelini IoT Central uygulamanıza el ile aktarın ve ardından IoT Central uygulamanızın ihtiyaç duyacağı tüm bulut özelliklerini, özelleştirmeleri ve panoları ekleyin.

Bir Oluşturucu olarak, cihaz şablonlarınızı doğrulamak üzere test cihazları için kod oluşturmak üzere IoT Central kullanabilirsiniz.

### <a name="customize-the-ui"></a>Kullanıcı arabirimini özelleştirme

Oluşturucu olarak, Azure IoT Central uygulamanızın kullanıcı arabirimini uygulamanızın günlük kullanımından sorumlu operatörler için de özelleştirebilirsiniz. Bir oluşturucu aşağıdaki özelleştirmeleri yapabilir:

- Bir cihaz şablonundaki özellik ve ayarların düzenini tanımlama.
- Operatörlerin içgörüleri keşfetmesine ve sorunları daha hızlı çözümlemesine yardımcı olacak özel panoları yapılandırma.
- Bağlı cihazlarınızdan zaman serisi verilerini keşfetmek için özel analizler yapılandırma.

## <a name="connect-your-devices"></a>Cihazlarınızı bağlama

Oluşturucunun uygulamaya bağlanabilen cihaz türlerini tanımlamasından sonra, cihaz geliştiricisi cihazlar üzerinde çalıştırılacak kodu oluşturur. Cihaz geliştiricisi olarak, cihaz kodunuzu oluşturmak için Microsoft'un açık kaynak [Azure IoT SDK’larını](https://github.com/Azure/azure-iot-sdks) kullanırsınız. Bu SDK’lar, Azure IoT Central uygulamanıza cihazlarınızı bağlama gereksinimlerinizi karşılamak üzere geniş dil, platform ve protokol desteği sunar. SDK 'lar aşağıdaki cihaz yeteneklerini uygulamanıza yardımcı olur:

- Güvenli bir bağlantı oluşturma.
- Telemetri gönderme.
- Durum bildirme.
- Yapılandırma güncelleştirmeleri alma.

Daha fazla bilgi için, [Azure IoT SDK’larını kullanmanın avantajları ve kullanmamanız durumunda kaçınılması gereken tuzaklar](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/) başlıklı blog gönderisine bakın.

## <a name="manage-your-application"></a>Uygulamanızı yönetme

Azure IoT Central uygulamaları tamamen Microsoft tarafından barındırılır, böylece uygulamalarınızı yönetmenin idari ek yükü azalır.

Bir operatör olarak, Azure IoT Central uygulamanızı kullanarak Azure IoT Central çözümünüzdeki cihazları yönetebilirsiniz. İşleçler gibi görevler şunlardır:

- Uygulamaya bağlı cihazları izleme.
- Cihazlarla ilgili sorunları giderme ve düzeltme.
- Yeni cihazlar hazırlama.

Bir Oluşturucu olarak, bağlı cihazlardan veri akışı üzerinde çalışan özel kurallar ve eylemler tanımlayabilirsiniz. Operatör ise uygulama içindeki görevleri denetlemek ve otomatik hale getirmek için cihaz düzeyinde bu kuralları etkinleştirebilir ya da devre dışı bırakabilir.

Yöneticiler [, Kullanıcı rolleri ve izinleriyle](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)uygulamanıza erişimi yönetir.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central’a genel bir bakış elde ettiğinize göre, aşağıdaki önerilen adımlara geçebilirsiniz:

- [Azure IoT Central ile Azure IoT çözüm hızlandırıcıları](overview-iot-options.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) arasındaki farklılıkları anlayın.
- [Azure IoT Central Kullanıcı Arabirimi](overview-iot-central-tour-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)’ni tanıma.
- [Azure IoT Central uygulaması oluşturmaya](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) bağlama.
- Aşağıdaki işlemlerin nasıl yapılacağını gösteren bir öğretici dizisini takip etme:
  - [Oluşturucu olarak, bir cihaz şablonu oluşturma](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
  - [Oluşturucu olarak, çözümünüzü otomatikleştirmek için kurallar ekleme](tutorial-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
  - [Operatör olarak, cihazlarınızı izleme](tutorial-monitor-devices-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
  - [Bir operatör olarak çözümünüze bir cihaz ekleyin](tutorial-add-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
- [Iot Tak ve kullan](https://aka.ms/iot-pnp-docs) hakkında daha fazla bilgi edinin
