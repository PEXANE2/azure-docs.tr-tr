---
title: Azure IoT Central nedir? | Microsoft Docs
description: Azure IoT Central, IoT çözümlerinin oluşturulmasını kolaylaştıran ve IoT yönetim işlemlerinin ve geliştirmenin yükünü ve maliyetini azaltmaya yardımcı olan bir IoT uygulama platformudur. Bu makalede Azure IoT Central’ın özelliklerine genel bir bakış sunulmaktadır.
author: dominicbetts
ms.author: dobett
ms.date: 12/10/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: timlt
ms.openlocfilehash: ffaab22efb2f6969f03720abba4a7afc6387021a
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758214"
---
# <a name="what-is-azure-iot-central"></a>Azure IoT Central nedir?

IoT Central, kurumsal kalitede IoT çözümleri geliştirme, yönetme ve sürdürme yükünü ve maliyetini azaltan bir IoT uygulama platformudur. IoT Central ile birlikte inşa etmeyi seçmek, karmaşık ve sürekli gelişen Bir IoT altyapısını korumak ve güncellemek yerine, işinizi IoT verileriyle dönüştürmeye odaklanma fırsatı verir.

Web Kullanıcı Aracı, yaşam döngüleri boyunca cihaz koşullarını izlemenize, kurallar oluşturmanıza ve milyonlarca aygıtı ve verilerini yönetmenize olanak tanır. Ayrıca, IoT zekasını iş yeri uygulamalarına genişleterek cihaz öngörüleri üzerinde hareket etmenizi sağlar.

Bu makalede, IoT Central için ana hatları:

- Bir projeyle ilişkili tipik kişilikler.
- Uygulamanızı oluşturma.
- Cihazlarınızı uygulamanıza bağlama
- Uygulamanızı yönetme.
- IoT Central'da Azure IoT Edge özellikleri.
- Azure IoT Edge çalışma zamanı ile çalışan cihazlarınızı uygulamanız için nasıl bağlayabilirsiniz?

## <a name="known-issues"></a>Bilinen sorunlar

- Sürekli veri dışa aktarma Avro biçimini (uyumsuzluk) desteklemez.
- GeoJSON şu anda desteklenmiyor.
- Harita döşemesi şu anda desteklenmiyor.
- İşler karmaşık türleri desteklemez.
- Dizi şeması türleri desteklenmez.
- Yalnızca C cihazı SDK ve Düğüm.js cihazı ve servis SDK'ları desteklenir.
- IoT Central şu anda Amerika Birleşik Devletleri, Avrupa, Asya Pasifik, Avustralya, Birleşik Krallık ve Japonya'da kullanılabilir.
- Birleşik Krallık ve Japonya konumlarında **Özel uygulama (eski)** uygulama şablonu kullanamazsınız.
- Aygıt özelliği modelleri, aynı dosyada sıralı olarak tanımlanan tüm arabirimlere sahip olmalıdır.
- [IoT Tak ve Çalıştır](../../iot-pnp/overview-iot-plug-and-play.md) desteği önizlemededir ve yalnızca belirli bölgelerde desteklenir.

## <a name="personas"></a>Kişilikler

IoT Merkezi dokümantasyonu, Bir IoT Merkezi uygulamasıyla etkileşimde bulunan dört kişiyi ifade eder:

- Bir _çözüm oluşturucu,_ uygulamaya bağlanan aygıt türlerini tanımlamaktan ve uygulamayı operatör için özelleştirmekten sorumludur.
- _Operatör_, uygulamaya bağlı cihazları yönetir.
- _Yönetici,_ uygulama içindeki [kullanıcı rollerini ve izinleri](howto-administer.md) yönetme gibi yönetim görevlerinden sorumludur.
- _Aygıt_ geliştiricisi, uygulamanıza bağlı bir aygıt veya IoT Edge modülünde çalışan kodu oluşturur.

## <a name="create-your-iot-central-application"></a>IoT Merkezi uygulamanızı oluşturun

Çözüm oluşturucu olarak, kuruluşunuz için özel, bulut barındırılan bir IoT çözümü oluşturmak için IoT Central'ı kullanırsınız. Özel bir IoT çözümü genellikle aşağıdakilerden oluşur:

- Cihazlarınızdan telemetri alan ve bu cihazları yönetmenizi sağlayan bulut tabanlı bir uygulama.
- Bulut tabanlı uygulamanıza bağlı özel kod çalıştıran birden fazla cihaz.

Yeni bir IoT Merkezi uygulamasını hızla dağıtabilir ve ardından tarayıcınızdaki özel gereksinimlerinize göre özelleştirebilirsiniz. Genel bir uygulama _şablonu_ yla veya [Perakende,](../retail/overview-iot-central-retail-pnp.md) [Enerji,](../energy/overview-iot-central-energy.md) [Devlet](../government/overview-iot-central-government.md)veya [Sağlık](../healthcare/overview-iot-central-healthcare.md)hizmetleri için endüstri odaklı uygulama şablonlarından biriyle başlayabilirsiniz.

Çözüm oluşturucu olarak, uygulamanıza bağlanan aygıtlar için bir _aygıt şablonu_ oluşturmak için web tabanlı araçları kullanırsınız. Aygıt şablonu, aşağıdakiler gibi bir aygıt türünün özelliklerini ve davranışını tanımlayan plandır:

- Telemetri gönderir.
- Bir operatörün değiştirebileceği iş özellikleri.
- Bir cihaz tarafından ayarlanan ve uygulamada salt okunur özellikte olan cihaz özellikleri.
- Aygıtın davranışını belirleyen bir işleç kümesi olan özellikler.

Bu aygıt şablonu şunları içerir:

- Bir aygıtın göndermesi ve bildirdiği özellikler gibi uygulaması gereken yetenekleri açıklayan bir _aygıt yeteneği modeli._
- Aygıtta depolanan bulut özellikleri.
- IoT Merkezi uygulamanızın bir parçası olan özelleştirmeler, panolar ve formlar.

### <a name="pricing"></a>Fiyatlandırma

7 günlük ücretsiz deneme sürümünü kullanarak IoT Central uygulaması oluşturabilir veya standart bir fiyatlandırma planı kullanabilirsiniz.

- *Ücretsiz* planı kullanarak oluşturduğunuz uygulamalar yedi gün boyunca ücretsizdir ve beş cihaza kadar destek lenir. Bunları, süresi dolmadan önce istediğiniz zaman standart bir fiyatlandırma planı kullanmaya dönüştürebilirsiniz.
- *Standart* bir plan kullanarak oluşturduğunuz uygulamalar cihaz bazında faturalandırılır, ilk iki cihaz ücretsiz olan **Standart 1** veya Standart **2** fiyatlandırma planını seçebilirsiniz. [Azure IoT Merkezi fiyatlandırma sayfasındaki](https://azure.microsoft.com/pricing/details/iot-central/)ücretsiz ve standart fiyatlandırma planları hakkında daha fazla bilgi edinin.

### <a name="create-device-templates"></a>Aygıt şablonları oluşturma

[IoT Tak ve Çalıştır (önizleme), IoT](../../iot-pnp/overview-iot-plug-and-play.md) Central'ın siz gömülü aygıt kodu yazmadan cihazları tümleştirmesini sağlar. IoT Tak ve Çalıştır'ın (önizleme) merkezinde, aygıt özelliklerini açıklayan bir aygıt yeteneği modeli şeması yer alan bir model şeması yer alan bir modeldir. Bir IoT Merkezi uygulamasında, aygıt şablonları bu IoT Tak ve Çalıştır (önizleme) aygıt özelliği modellerini kullanır.

Çözüm oluşturucu olarak, aygıt şablonları oluşturmak için çeşitli seçenekleriniz vardır:

- [IoT aygıt kataloğundan](https://aka.ms/iotdevcat) bir aygıt yetenek modeli aktarın ve ardından IoT Merkezi uygulama gereksinimleriniz için bulut özellikleri, özelleştirmeler ve panolar ekleyin.
- Aygıt şablonunu IoT Central'da tasarlayın ve aygıt kodunda aygıt yetenek modelini uygulayın.
- Visual Studio kodunu kullanarak bir aygıt yeteneği modeli oluşturun ve modeli bir depoda yayımlayın. Cihaz kodunuzu modelden uygulayın ve aygıtınızı IoT Merkezi uygulamanıza bağlayın. IoT Central depodan aygıt yetenek modelini bulur ve sizin için basit bir aygıt şablonu oluşturur.
- Visual Studio kodunu kullanarak bir aygıt yeteneği modeli oluşturun. Cihaz kodunuzu modelden uygulayın. Aygıt yetenek modelini IoT Merkezi uygulamanıza el ile aktarın ve ardından IoT Merkezi uygulama gereksinimleriniz için bulut özellikleri, özelleştirmeler ve panolar ekleyin.

Çözüm oluşturucu olarak, aygıt şablonlarınızı doğrulamak için test aygıtları için kod oluşturmak için IoT Central'ı kullanabilirsiniz.

### <a name="customize-the-ui"></a>Kullanıcı arabirimini özelleştirme

Çözüm oluşturucu olarak, uygulamanın günlük kullanımından sorumlu operatörler için IoT Merkezi uygulama kullanıcı arabirimi'ni de özelleştirebilirsiniz. Bir çözüm oluşturucu yapabilir özelleştirmeler şunlardır:

- Bir cihaz şablonundaki özellik ve ayarların düzenini tanımlama.
- Operatörlerin içgörüleri keşfetmesine ve sorunları daha hızlı çözümlemesine yardımcı olacak özel panoları yapılandırma.
- Bağlı cihazlarınızdan zaman serisi verilerini keşfetmek için özel analizler yapılandırma.

## <a name="pricing"></a>Fiyatlandırma

7 günlük ücretsiz deneme sürümünü kullanarak IoT Central uygulaması oluşturabilir veya standart bir fiyatlandırma planı kullanabilirsiniz.

- *Ücretsiz* planı kullanarak oluşturduğunuz uygulamalar yedi gün boyunca ücretsizdir ve beş cihaza kadar destek lenir. Bunları, süresi dolmadan önce istediğiniz zaman standart bir fiyatlandırma planı kullanmaya dönüştürebilirsiniz.
- *Standart* planı kullanarak oluşturduğunuz uygulamalar cihaz bazında faturalandırılır, ilk iki cihaz ücretsiz olan **Standart 1** veya Standart **2** fiyatlandırma planını seçebilirsiniz. [IoT Merkezi fiyatlandırması](https://aka.ms/iotcentral-pricing)hakkında daha fazla bilgi edinin.

## <a name="connect-your-devices"></a>Cihazlarınızı bağlama

Azure IoT Central, tüm cihaz kaydını ve bağlantısını yönetmek için [Azure IoT Hub Aygıt Sağlama hizmetini (DPS)](../../iot-dps/about-iot-dps.md) kullanır.

DPS'nin kullanılması şunları sağlar:

- IoT Central, ölçekte biniş ve bağlantı aygıtlarını desteklemek için.
- Aygıt kimlik bilgilerini oluşturmak ve aygıtları IoT Merkezi Web Aracı aracılığıyla kaydetmeden çevrimdışı olarak yapılandırmak için.
- Paylaşılan erişim imzalarını kullanarak bağlanacak aygıtlar.
- Endüstri standardı X.509 sertifikalarını kullanarak bağlanacak cihazlar.
- IoT Central'daki cihazları kaydetmek için kendi cihaz lı lı lıları kullanmanız gerekir. Kendi aygıt ınızın kullanılması, varolan arka ofis sistemleriyle tümleştirmeyi kolaylaştırır.
- Aygıtları IoT Central'a bağlamanın tek ve tutarlı bir yolu.

Daha fazla bilgi için azure [IoT Central'a bağlanın.](./concepts-get-connected.md)

### <a name="azure-iot-edge-devices"></a>Azure IoT Edge cihazları

[Azure IoT SDK'ları](https://github.com/Azure/azure-iot-sdks)kullanılarak oluşturulan aygıtların yanı sıra, [Azure IoT Edge aygıtlarını](../../iot-edge/about-iot-edge.md) bir IoT Merkezi uygulamasına da bağlayabilirsiniz. Azure IoT Edge, bulut zekası ve özel mantığı doğrudan IoT Central tarafından yönetilen IoT aygıtlarında çalıştırmanızı sağlar. IoT Edge çalışma süresi şunları yapmanızı sağlar:

- Aygıttaki iş yüklerini yükleyin ve güncelleyin.
- Aygıtta Azure IoT Edge güvenlik standartlarını koruyun.
- IoT Edge modüllerinin her zaman çalışır durumda olduğundan emin olun.
- Uzaktan izleme için modül durumunu buluta bildirin.
- Akış aşağı yaprak aygıtları ile IoT Edge aygıtı arasındaki iletişimi, bir IoT Edge aygıtındaki modüller arasındaki ve bir IoT Edge aygıtı ile bulut arasındaki iletişimi yönetin.

Daha fazla bilgi için [Azure IoT Edge aygıtları ve IoT Central'a](concepts-architecture.md#azure-iot-edge-devices)bakın.

## <a name="stay-connected"></a>Bağlı kalma

IoT Central uygulamaları tamamen Microsoft tarafından barındırılır ve bu da uygulamalarınızı yönetme yönetim ek yüküne neden olur.

Operatör olarak, IoT Central çözümündeki cihazları yönetmek için IoT Central uygulamasını kullanırsınız. İşleçler şu gibi görevleri yapar:

- Uygulamaya bağlı cihazları izleme.
- Cihazlarla ilgili sorunları giderme ve düzeltme.
- Yeni cihazlar hazırlama.

Çözüm oluşturucu olarak, bağlı aygıtlardan veri akışı üzerinden çalışan özel kurallar ve eylemler tanımlayabilirsiniz. Operatör ise uygulama içindeki görevleri denetlemek ve otomatik hale getirmek için cihaz düzeyinde bu kuralları etkinleştirebilir ya da devre dışı bırakabilir.

Yöneticiler, kullanıcı rolleri [ve izinlerle](howto-administer.md)uygulamanıza erişimi yönetir.

Ölçekte çalışmak üzere tasarlanmış herhangi bir IoT çözümü yle, cihaz yönetimine yapılandırılmış bir yaklaşım önemlidir. Yalnızca cihazlarınızı buluta bağlamak yeterli değildir, cihazlarınızı bağlı ve sağlıklı tutmanız gerekir. Bir operatör, uygulama ömrü boyunca cihazlarınızı yönetmek için aşağıdaki IoT Merkezi özelliklerini kullanabilir:

### <a name="dashboards"></a>Panolar 

Dahili [panolar,](./howto-set-up-template.md#generate-default-views) aygıt durumunu ve telemetriyi izlemek için özelleştirilebilir bir kullanıcı aracı sağlar. [Uygulama şablonundaki](howto-use-app-templates.md) önceden oluşturulmuş bir panoyla başlayın veya operatörlerinizin ihtiyaçlarına göre uyarlanmış kendi panolarınızı oluşturun. Panoları uygulamanızdaki tüm kullanıcılarla paylaşabilir veya gizli tutabilirsiniz.

### <a name="rules-and-actions"></a>Kurallar ve eylemler 

Dikkat edilmesi gereken aygıtları tanımlamak için aygıt durumuna ve telemetriye dayalı [özel kurallar](tutorial-create-telemetry-rules.md) oluşturun. Eylemleri doğru kişileri bilgilendirmek ve düzeltici önlemlerin zamanında alınmasını sağlamak için yapılandırın.

### <a name="jobs"></a>İşler 

[İşler,](howto-run-a-job.md) özellikleri ayarlayarak veya komutları çağırarak aygıtlara tek veya toplu güncelleştirmeler uygulamanıza izin verir. 

### <a name="user-roles-and-permissions"></a>Kullanıcı rolleri ve izinleri

[Roller ve izinler,](howto-manage-users-roles.md) bir yöneticinin her kullanıcının deneyimini uyarlamasına izin verir. Yönetici, roller oluşturmak ve izinler atamak için web UI'sini kullanır.

## <a name="transform-your-iot-data"></a>IoT verilerinizi dönüştürün

Bir uygulama platformu olarak IoT Central, IoT verilerinizi işlem edilebilir sonuçlar elde eden iş öngörülerine dönüştürmenize olanak tanır. [Kurallar,](./tutorial-create-telemetry-rules.md) [veri dışa aktarma](./howto-export-data.md)ve [genel REST API'si,](https://docs.microsoft.com/learn/modules/manage-iot-central-apps-with-rest-api/) IoT Central'ı iş yeri uygulamalarıyla nasıl entegre edebileceğinize örnektir:

![IoT Central, IoT verilerinizi nasıl dönüştürebilir?](media/overview-iot-central/transform.png)

### <a name="monitor-device-health-and-operations-using-rules"></a>Kuralları kullanarak cihaz durumunu ve işlemlerini izleme

Aygıtlarınız bağlandığında ve veri gönderdiğinde, kurallar sorun yaşayan veya hata iletileri gönderen cihazları belirleyebilir, böylece bunları en az kapalı kalma süresiyle düzeltebilirsiniz. Aygıtlarınızdan telemetriyi izlemek ve bir metrik bir eşiği geçtiğinde veya bir aygıt belirli bir ileti gönderdiğinde operatörü uyarmak için IoT Merkezi uygulamanızda kurallar oluşturun. Kurallarınız için e-posta eylemleri ve webhooks doğru kişileri ve doğru downstream sistemleri bildirir.

### <a name="run-custom-analytics-and-processing-on-your-exported-data"></a>İhraç edilen verilerinizde özel analizler ve işleme ler çalıştırın

Cihazlarınızdan telemetriyi işlemek ve sonuçları depolamak için özel analiz boru hatları oluşturarak, makine verimliliği eğilimlerini belirleme veya fabrika zemininde gelecekteki enerji kullanımını tahmin etme gibi iş öngörüleri oluşturabilirsiniz. IoT Central uygulamanızdaki veri dışa aktarımlarını, telemetriyi, aygıt özelliği değişikliklerini ve aygıt şablonu değişikliklerini, verileri tercih ettiğiniz araçlarla analiz edebileceğiniz, depolaabileceğiniz ve görselleştirebileceğiniz diğer hizmetlere aktarmak için yapılandırın.

### <a name="build-custom-iot-solutions-and-integrations-with-the-rest-apis"></a>REST API'leri ile özel IoT çözümleri ve entegrasyonlar oluşturun

Şu gibi IoT çözümleri oluşturun:

- Cihazları uzaktan kurabilen ve kontrol edebilen mobil yardımcı uygulamalar.
- Varolan iş hattı uygulamalarının IoT aygıtların ve verilerinizle etkileşimkurmasını sağlayan özel tümleştirmeler.
- Cihaz modelleme, onboarding, yönetim ve veri erişimi için aygıt yönetimi uygulamaları.

## <a name="quotas"></a>Kotalar

Her Azure aboneliği, IoT çözümünüzün kapsamını etkileyebilecek varsayılan kotalara sahiptir. Şu anda, IoT Central bir abonelikte dağıtabileceğiniz uygulama sayısını 10 ile sınırlar. Bu sınırı artırmanız gerekiyorsa, [Microsoft desteğine](https://azure.microsoft.com/support/options/)başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi IoT Central genel bir bakış var, burada bazı önerilen sonraki adımlar şunlardır:

- [IoT çözümleri oluşturmak için](../../iot-fundamentals/iot-services-and-technologies.md)kullanılabilir Azure teknolojilerini ve hizmetlerini anlayın.
- [Azure IoT Central Kullanıcı Arabirimi](overview-iot-central-tour.md)’ni tanıma.
- [Azure IoT Central uygulaması oluşturmaya](quick-deploy-iot-central.md) bağlama.
- [IoT Tak ve Çalıştır (önizleme)](../../iot-pnp/overview-iot-plug-and-play.md)hakkında daha fazla bilgi edinin.
- [Azure IoT Edge aygıtını nasıl bağlayabilirsiniz](./tutorial-add-edge-as-leaf-device.md)öğrenin.
- [Azure IoT teknolojileri ve hizmetleri](../../iot-fundamentals/iot-services-and-technologies.md)hakkında daha fazla bilgi edinin.

Bir aygıt geliştiricisiyseniz ve bazı kodlara dalmak istiyorsanız, önerilen bir sonraki adım [bir istemci uygulaması oluşturmak ve Azure IoT Merkezi uygulamanıza bağlamaktır.](./tutorial-connect-device-nodejs.md)
