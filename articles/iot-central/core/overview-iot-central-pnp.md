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
ms.openlocfilehash: dfb550fd0bc93a6b865495bf2c19f394fb0737ed
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2019
ms.locfileid: "72987874"
---
# <a name="what-is-azure-iot-central-preview-features"></a>Azure IoT Central (Önizleme özellikleri) nedir?

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

> [!WARNING]
> Azure IoT Central [ıot Tak ve kullan](../../iot-pnp/overview-iot-plug-and-play.md) özellikleri şu anda genel önizlemededir. Üretim iş yükleri için IoT Tak ve Kullan özellikli IoT Central uygulaması kullanmayın. Üretim ortamları için, geçerli, genel olarak kullanılabilir bir uygulama şablonundan oluşturulan bir IoT Merkezi uygulaması kullanın.

Azure IoT Central, kurumsal düzeyde IoT çözümlerinin geliştirilmesine, yönetilmesine ve bakımının yükünü ve maliyetini azaltan bir IoT uygulama platformudur. Azure IoT Central ile oluşturmayı seçtiğinizde, yalnızca karmaşık ve sürekli gelişen IoT altyapısını korumak ve güncelleştirmek yerine, işletmenizin IoT verileriyle dönüştürülmesi için zaman, para ve enerji tasarrufu sağlama fırsatı sunulmaktadır.

Web Kullanıcı arabirimi, cihaz koşullarını izlemenizi, kurallar oluşturmanızı ve milyonlarca cihazı ve bunların yaşam döngüsü boyunca verilerini yönetmenizi sağlar. Ayrıca, IoT Intelligence 'ı iş kolu uygulamalarına genişleterek cihaz öngörülerine göre hareket etmenizi sağlar.

Bu makalede, Azure IoT Central için özetlenmektedir:

- Bir projeyle ilişkili tipik kişilikler.
- Uygulamanızı oluşturma.
- Cihazlarınızı uygulamanıza bağlama
- Uygulamanızı yönetme.
- IoT Central IoT Edge özelliklerine genel bakış
- Azure IoT Edge çalışma zamanı ile desteklenen cihazlarınızı uygulamanıza bağlama.


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
- Yalnızca Kuzey Avrupa ve Orta ABD bölgeleri mevcuttur.
- Cihaz yetenek modelleri aynı dosyada satır içi tanımlanmış tüm arabirimlere sahip olmalıdır.

## <a name="personas"></a>Kişilikler

Azure IoT Central belgeleri, Azure IoT Central uygulamasıyla etkileşime geçen dört kişilik anlamına gelir:

- _Çözüm Oluşturucu_ , uygulamaya bağlanan ve uygulamayı operatör için özelleştiren cihazların türlerini tanımlamaktan sorumludur.
- _Operatör_, uygulamaya bağlı cihazları yönetir.
- _Yönetici_ , uygulama içindeki [Kullanıcı rollerini ve izinleri](howto-administer.md) yönetme gibi yönetim görevlerinden sorumludur.
- _Cihaz geliştiricisi_, uygulamanıza bağlı bir cihaz üzerinde çalışan kodu oluşturur.
- Bir _cihaz/modül geliştiricisi_ , uygulamanıza bağlı bir cihazda çalışan kodu/modülü oluşturur.

## <a name="create-your-azure-iot-central-application"></a>Azure IoT Central uygulamanızı oluşturma

Oluşturucu olarak Azure IoT Central’ı, kuruluşunuz için özel, bulutta barındırılan bir IoT çözümü oluşturmak için kullanırsınız. Özel bir IoT çözümü genellikle aşağıdakilerden oluşur:

- Cihazlarınızdan telemetri alan ve bu cihazları yönetmenizi sağlayan bulut tabanlı bir uygulama.
- Bulut tabanlı uygulamanıza bağlı özel kod çalıştıran birden fazla cihaz.

Yeni bir Azure IoT Central uygulamasını hızlıca dağıtabilir ve sonra tarayıcınızda özel gereksinimlerinize göre özelleştirebilirsiniz. Bir Oluşturucu olarak, uygulamanıza bağlanan cihazlar için bir _cihaz şablonu_ oluşturmak üzere Web tabanlı araçları kullanın. Cihaz şablonu, şöyle bir cihaz türünün özelliklerini ve davranışını tanımlayan şema ' dir:

- Gönderdiği telemetri.
- Bir operatörün değiştirebileceği iş özellikleri.
- Bir cihaz tarafından ayarlanan ve uygulamada salt okunur özellikte olan cihaz özellikleri.
- Bir işleç kümelerinin, cihazın davranışını belirleyen özellikler.

Bu cihaz şablonu şunları içerir:

- Bir cihazın gönderdiği telemetri ve rapor aldığı özellikler gibi uygulaması gereken özellikleri açıklayan bir _cihaz yetenek modeli_ .
- Cihazda depolanmayan bulut özellikleri.
- IoT Central uygulamanızın parçası olan özelleştirmeler, panolar ve formlar.

### <a name="create-device-templates"></a>Cihaz şablonları oluşturma

[Iot Tak ve kullan](../../iot-pnp/overview-iot-plug-and-play.md) , hiçbir katıştırılmış cihaz kodu yazmadan IoT Central cihazları tümleştirmenize olanak sağlar. IoT Tak ve Kullan 'nin çekirdeğinden, cihaz yeteneklerini tanımlayan bir cihaz yetenek modeli şemadır. IoT Central önizleme uygulamasında, cihaz şablonları bu IoT Tak ve Kullan cihaz yeteneği modellerini kullanır.

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


## <a name="what-is-azure-iot-central-with-azure-iot-edge-preview-features"></a>Azure IoT Edge ile Azure IoT Central nedir (Önizleme özellikleri)

IoT Central, Azure IoT Edge cihazlarını destekleyerek portföyünü genişletiyor. 

İşletmeler artık, Azure IoT Central tarafından yönetilen IoT cihazlarında doğrudan bulut zekasını çalıştırabilir. Bu yeni özellik, işletmelerin Azure IoT Edge çalışma zamanı çalıştıran Azure IoT Edge cihazları bağlanmasına ve yönetmesine, yazılım modüllerini dağıtmalarına, öngörülere yayımlamasına ve tüm bunları IoT Central içinden tamamen ölçeklendirmenize yardımcı olur. 

[Azure IoT Edge genel bakış](../../iot-edge/about-iot-edge.md)

### <a name="overview-of-iot-edge-capabilities-in-iot-central"></a>IoT Central IoT Edge özelliklerine genel bakış

Azure IoT Edge çalışma zamanı, IoT Edge cihazlarında özel mantığa ve bulut mantığına olanak tanır. IoT Edge cihaz çalışma zamanı tarafından destekleniyor ve yönetim ve iletişim işlemleri gerçekleştirir. 

Azure IoT Edge çalışma zamanı aşağıdaki işlevleri gerçekleştirir:

- Cihaza iş yüklerini yükleyip güncelleştirin.
- Cihazda Azure IoT Edge güvenlik standartlarının bakımını yapın.
- IoT Edge modüllerinin her zaman çalıştığından emin olun.
- Uzaktan izleme için modül durumunu buluta bildirin.
- Bir IoT Edge cihazdaki modüller arasında ve bir IoT Edge cihaz ile bulut arasında aşağı akış yaprak cihazları ve bir IoT Edge cihazı arasındaki iletişimi yönetin.

![Azure IoT Edge genel bakış ile IoT Central](./media/overview-iot-central-pnp-edge/iotedge.png)

Azure IoT Central aşağıdaki işlevleri gerçekleştirir: 

- Azure IoT Edge bir cihazın uygulaması gereken özellikleri açıklayan cihaz şablonu desteği Azure IoT Edge 
  1. bir cihaz için bir bildirimin yönetilmesine yardımcı olacak dağıtım bildirimi karşıya yükleme özelliği
  2. Azure IoT Edge cihazda çalışacak modüller
  3. her modülün gönderdiği telemetri
  4. Her modül raporunun ve 
  5. her modülün yanıt verdiği komut
  6. Azure IoT Edge ağ geçidi cihazı yetenek modeli ve aşağı akış cihazı yetenek modeli arasında ilişki oluşturma
  7. Azure IoT Edge cihazında depolanmayan bulut özellikleri
  8. IoT Central uygulamanızın parçası olan özelleştirmeler, panolar ve formlar

  [Azure IoT Edge cihaz şablonu oluşturma](./tutorial-define-edge-device-type-pnp.md)
   
- Azure IoT cihaz sağlama hizmeti 'ni kullanarak ölçekte Azure IoT Edge cihazları sağlama
- Kuralları tetikleme ve Azure IoT Edge cihazlarda işlem yapın
- Panolar ve çözümlemeler oluşturun 
- Azure IoT Edge cihazlarından Telemetriyi sürekli veri dışa aktarma

### <a name="azure-iot-edge-device-types-in-iot-central"></a>IoT Central cihaz türleri Azure IoT Edge

Azure IoT Central Azure IoT Edge cihaz türlerini aşağıdaki şekilde sınıflandırır:

- Cihazı yaprak cihaz olarak Azure IoT Edge. Azure IoT Edge cihazda aşağı akış cihazları olabilir, ancak aşağı akış cihazları sağlanmadı IoT Central
- Cihaz, aşağı akış cihazlarına sahip bir ağ geçidi cihazı olarak Azure IoT Edge. Ağ geçidi cihazı ve aşağı akış Cihazları IoT Central

![Azure IoT Edge genel bakış ile IoT Central](./media/overview-iot-central-pnp-edge/gatewayedge.png)

### <a name="azure-iot-edge-patterns-supported-in-iot-central"></a>IoT Central desteklenen Azure IoT Edge desenleri

- Yaprak cihaz olarak Azure IoT Edge ![yaprak cihaz olarak Azure IoT Edge Azure IoT Edge cihaz](./media/overview-iot-central-pnp-edge/edgeasleafdevice.png) IoT Central ve tüm aşağı akış cihazları ve telemetri Azure IoT Edge cihazdan geldiği şekilde temsil edilir. Azure IoT Edge cihazına bağlı herhangi bir bağlantı IoT Central içinde sağlanmayacak aşağı akış cihazları. 

- Azure IoT Edge ağ geçidi cihazı, aşağı akış cihaz kimliği ile Azure IoT Edge kimlik ![olan aşağı akış cihazlara bağlandı](./media/overview-iot-central-pnp-edge/edgewithdownstreamdeviceidentity.png) Azure IoT Edge cihaz IoT Central bağlı aşağı akış cihazları ile birlikte sağlanır Azure IoT Edge aygıtların. Çalışma zamanı desteği, ağ geçidi aracılığıyla aşağı akış cihazların sağlanması ileride planlanmaktadır. IoT Central, aşağı akış cihazlarını Ilk kez sağlamak ve kimlik bilgileri aşağı akış cihazında el ile yönetilir. Gelecekteki tüm kullanımlar için planlanmış aşağı akış cihazlarını cihazın ilk kez sağlaması. 

- Uç ağ geçidi ![tarafından sağlanan kimlik](./media/overview-iot-central-pnp-edge/edgewithoutdownstreamdeviceidentity.png) Azure IoT Edge ağ geçidi cihazı, oturum yönündeki Azure IoT Edge kimliği olmadan aşağı akış cihazlarıyla birlikte, Azure IoT Edge cihaz IoT Central Azure IoT Edge cihazına bağlı cihazlar. Aşağı akış cihazlarına kimlik sağlayan ve aşağı akış cihazlarının sağlanması için ağ geçidinin çalışma zamanı desteği geleceğe planlanmaktadır. Kendi kimlik çevirisi modülünüzü getirebilirsiniz ve IoT Central bu kalıbı destekleyecektir. 

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central’a genel bir bakış elde ettiğinize göre, aşağıdaki önerilen adımlara geçebilirsiniz:

- [Azure IoT Central ile Azure IoT çözüm hızlandırıcıları](overview-iot-options.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) arasındaki farklılıkları anlayın.
- [Azure IoT Central Kullanıcı Arabirimi](overview-iot-central-tour-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)’ni tanıma.
- [Azure IoT Central uygulaması oluşturmaya](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) bağlama.
- [Iot Tak ve kullan](../../iot-pnp/overview-iot-plug-and-play.md) hakkında daha fazla bilgi edinin
- [Azure IoT Edge cihaz şablonu oluşturmayı](./tutorial-define-edge-device-type-pnp.md) öğrenin
