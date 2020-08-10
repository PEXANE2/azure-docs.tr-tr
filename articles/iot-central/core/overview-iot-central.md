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
ms.openlocfilehash: 1af549feb551eca02bb1ef8df6481eceb959f6d1
ms.sourcegitcommit: 1a0dfa54116aa036af86bd95dcf322307cfb3f83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88042726"
---
# <a name="what-is-azure-iot-central"></a>Azure IoT Central nedir?

IoT Central, kurumsal düzeyde IoT çözümlerinin geliştirilmesine, yönetilmesine ve bakımının yapılmasına yönelik maliyet ve yükü azaltan bir IoT uygulama platformudur. IoT Central ile oluşturmayı seçtiğinizde, yalnızca karmaşık ve sürekli gelişen IoT altyapısını korumak ve güncelleştirmek yerine, işletmenizin IoT verileriyle dönüştürülmesi için zaman, para ve enerji tasarrufu sağlama fırsatı sunulmaktadır.

Web Kullanıcı arabirimi, cihaz koşullarını izlemenizi, kurallar oluşturmanızı ve milyonlarca cihazı ve bunların yaşam döngüsü boyunca verilerini yönetmenizi sağlar. Ayrıca, IoT Intelligence 'ı iş kolu uygulamalarına genişleterek cihaz öngörülerine göre hareket etmenizi sağlar.

Bu makalede IoT Central için özetlenmektedir:

- Bir projeyle ilişkili tipik kişilikler.
- Uygulamanızı oluşturma.
- Cihazlarınızı uygulamanıza bağlama
- Uygulamanızı yönetme.
- IoT Central Azure IoT Edge özellikleri.
- Azure IoT Edge çalışma zamanı ile desteklenen cihazlarınızı uygulamanıza bağlama.

## <a name="personas"></a>Kişilikler

IoT Central belgeler, IoT Central bir uygulamayla etkileşime geçen dört kişilik anlamına gelir:

- _Çözüm Oluşturucu_ , uygulamaya bağlanan ve uygulamayı operatör için özelleştiren [cihazların türlerini tanımlamaktan](howto-set-up-template.md) sorumludur.
- Bir _işleç_ , uygulamaya bağlı [aygıtları yönetir](howto-manage-devices.md) .
- _Yönetici_ , uygulama içindeki [Kullanıcı rollerini ve izinleri](howto-administer.md) yönetme gibi yönetim görevlerinden sorumludur.
- Bir _cihaz geliştiricisi_ , uygulamanıza bağlı bir cihazda veya [IoT Edge modülde](concepts-iot-edge.md) [çalışan kodu oluşturur](concepts-telemetry-properties-commands.md) .

## <a name="create-your-iot-central-application"></a>IoT Central uygulamanızı oluşturma

Bir çözüm Oluşturucusu olarak, kuruluşunuz için özel, bulutta barındırılan bir IoT çözümü oluşturmak üzere IoT Central kullanırsınız. Özel bir IoT çözümü genellikle aşağıdakilerden oluşur:

- Cihazlarınızdan telemetri alan ve bu cihazları yönetmenizi sağlayan bulut tabanlı bir uygulama.
- Bulut tabanlı uygulamanıza bağlı özel kod çalıştıran birden fazla cihaz.

Yeni bir IoT Central uygulamasını hızlıca dağıtabilir ve sonra tarayıcınızda özel gereksinimlerinize göre özelleştirebilirsiniz. Bir genel _uygulama şablonuyla_ veya [Perakende](../retail/overview-iot-central-retail.md), [enerji](../energy/overview-iot-central-energy.md), [devlet](../government/overview-iot-central-government.md)veya [sağlık](../healthcare/overview-iot-central-healthcare.md)için sektör odaklı uygulama şablonlarından biriyle başlayabilirsiniz.

Bir çözüm Oluşturucusu olarak, uygulamanıza bağlanan cihazlar için bir _cihaz şablonu_ oluşturmak üzere Web tabanlı araçları kullanın. Cihaz şablonu, şöyle bir cihaz türünün özelliklerini ve davranışını tanımlayan şema ' dir:

- Gönderdiği telemetri.
- Bir operatörün değiştirebileceği iş özellikleri.
- Bir cihaz tarafından ayarlanan ve uygulamada salt okunur özellikte olan cihaz özellikleri.
- Bir işleç kümelerinin, cihazın davranışını belirleyen özellikler.

Bu [cihaz şablonu](howto-set-up-template.md) şunları içerir:

- Bir cihazın gönderdiği telemetri ve rapor aldığı özellikler gibi uygulaması gereken özellikleri açıklayan bir _cihaz yetenek modeli_ .
- Cihazda depolanmayan bulut özellikleri.
- IoT Central uygulamanızın parçası olan özelleştirmeler, panolar ve formlar.

### <a name="create-device-templates"></a>Cihaz şablonları oluşturma

Bir çözüm Oluşturucusu olarak, cihaz şablonları oluşturmak için çeşitli seçenekleriniz vardır:

- IoT Central cihaz şablonunu tasarlayın ve cihaz kodunuzda cihaz yetenek modelini uygulayın.
- Visual Studio Code 'u kullanarak bir cihaz yetenek modeli oluşturun ve modeli bir depoya yayımlayın. Modelden cihaz kodunuzu uygulayın ve cihazınızı IoT Central uygulamanıza bağlayın. IoT Central, depodan cihaz yetenek modelini bulur ve sizin için basit bir cihaz şablonu oluşturur.
- Visual Studio Code 'u kullanarak bir cihaz yetenek modeli oluşturun. Modelden cihaz kodunuzu uygulayın. Cihaz yetenek modelini IoT Central uygulamanıza el ile aktarın ve ardından IoT Central uygulamanızın ihtiyaç duyacağı tüm bulut özelliklerini, özelleştirmeleri ve panoları ekleyin.

Bir çözüm Oluşturucusu olarak, cihaz şablonlarınızı doğrulamak üzere test cihazları için kod oluşturmak üzere IoT Central kullanabilirsiniz.

Bir cihaz geliştiricisiyseniz, bu cihaz şablonlarını kullanan cihazları uygulamaya giriş için [IoT Central cihaz geliştirmeye genel bakış](./overview-iot-central-developer.md) bölümüne bakın.

### <a name="customize-the-ui"></a>Kullanıcı arabirimini özelleştirme

Bir çözüm Oluşturucusu olarak, uygulamanın günlük kullanımıyla sorumlu operatörler için IoT Central uygulama kullanıcı arabirimini de özelleştirebilirsiniz. Bir çözüm oluşturucusunun yapaiçereme özelleştirmeleri şunlardır:

- Bir cihaz şablonundaki özellik ve ayarların düzenini tanımlama.
- Operatörlerin içgörüleri keşfetmesine ve sorunları daha hızlı çözümlemesine yardımcı olacak özel panoları yapılandırma.
- Bağlı cihazlarınızdan zaman serisi verilerini keşfetmek için özel analizler yapılandırma.

## <a name="manage-your-devices"></a>Cihazlarınızı yönetme

Bir operatör olarak, IoT Central çözümünüzdeki [cihazları yönetmek](howto-manage-devices.md) için IoT Central uygulamasını kullanırsınız. İşleçler gibi görevler şunlardır:

- Uygulamaya bağlı cihazları izleme.
- Cihazlarla ilgili sorunları giderme ve düzeltme.
- Yeni cihazlar hazırlama.

Bir çözüm Oluşturucusu olarak, bağlı cihazlardan veri akışı üzerinde çalışan [özel kurallar ve eylemler tanımlayabilirsiniz](howto-configure-rules.md) . Operatör ise uygulama içindeki görevleri denetlemek ve otomatik hale getirmek için cihaz düzeyinde bu kuralları etkinleştirebilir ya da devre dışı bırakabilir.

Ölçekte çalışacak şekilde tasarlanan tüm IoT çözümleri ile cihaz yönetimine yönelik yapılandırılmış bir yaklaşım önemlidir. Cihazlarınızı buluta bağlamak yeterli değildir, cihazlarınızı bağlı ve sağlıklı tutmanız gerekir. Bir operatör, uygulama yaşam döngüsü boyunca cihazlarınızı yönetmek için aşağıdaki IoT Central yeteneklerini kullanabilir:

### <a name="dashboards"></a>Panolar

Yerleşik [panolar](./howto-set-up-template.md#generate-default-views) cihaz sistem durumunu ve telemetrisini izlemek için özelleştirilebilir bir kullanıcı arabirimi sağlar. [Uygulama şablonunda](howto-use-app-templates.md) önceden oluşturulmuş bir panoyla başlayın veya işleçlerinizin ihtiyaçlarına göre özel kendi panolarınızı oluşturun. Panoları uygulamanızdaki tüm kullanıcılarla paylaşabilir veya özel tutabilirsiniz.

### <a name="rules-and-actions"></a>Kurallar ve eylemler

Dikkat edilmesi gereken cihazları tanımlamak için cihaz durumu ve telemetri temelinde [özel kurallar](tutorial-create-telemetry-rules.md) oluşturun. Doğru kişilere bildirimde bulunan eylemleri yapılandırın ve düzeltici ölçülerin zamanında alındığından emin olun.

### <a name="jobs"></a>İşler

[İşler](howto-run-a-job.md) , özellikleri ayarlayarak veya komutları çağırarak cihazlara tek veya toplu güncelleştirmeler uygulamanıza olanak tanır.

## <a name="integrate-with-other-services"></a>Diğer hizmetlerle tümleştirme

Bir uygulama platformu olarak, IoT Central IoT verilerinizi, işlem yapılabilir sonuçları sağlayan iş öngörülerine dönüştürmenize olanak tanır. [Kurallar](./tutorial-create-telemetry-rules.md), [veri dışa aktarma](./howto-export-data.md)ve [genel REST API](https://docs.microsoft.com/learn/modules/manage-iot-central-apps-with-rest-api/) , IoT Central iş kolu uygulamalarıyla nasıl tümleştirebileceğinizi gösteren örneklerdir:

![IoT verilerinizi nasıl dönüştürebileceğinizi IoT Central](media/overview-iot-central/transform.png)

Cihazlarınızdan Telemetriyi işlemek ve sonuçları depolamak için özel analiz işlem hatları oluşturarak makine verimlilik eğilimlerini belirleme veya bir fabrika katında gelecekteki enerji kullanımını tahmin etme gibi iş öngörüleri oluşturabilirsiniz. Tercih ettiğiniz araçlarla verileri analiz edebilirsiniz, depolayabileceğiniz ve görselleştirebileceğiniz diğer hizmetlere telemetri, cihaz özelliği değişikliklerini ve cihaz şablonu değişikliklerini dışarı aktarmak için IoT Central uygulamanızdaki veri dışarı aktarmaları yapılandırın.

### <a name="build-custom-iot-solutions-and-integrations-with-the-rest-apis"></a>REST API 'Leriyle özel IoT çözümleri ve tümleştirmeler oluşturun

Şu gibi IoT çözümleri oluşturun:

- Cihazları uzaktan ayarlayıp denetleyebileceği mobil yardımcı uygulamalar.
- Mevcut iş kolu uygulamalarının IoT cihazlarınızla ve verilerinizle etkileşime geçmesini sağlayan özel tümleştirmeler.
- Cihaz modellemesi, ekleme, yönetim ve veri erişimi için cihaz yönetimi uygulamaları.

## <a name="administer-your-application"></a>Uygulamanızı yönetme

IoT Central uygulamalar Microsoft tarafından tamamen barındırılır ve uygulamalarınızın yönetilmesi için yönetim yükünü azaltır. Yöneticiler [, Kullanıcı rolleri ve izinleriyle](howto-administer.md)uygulamanıza erişimi yönetir.

## <a name="pricing"></a>Fiyatlandırma

7 günlük ücretsiz deneme sürümünü kullanarak IoT Central uygulaması oluşturabilir veya standart bir fiyatlandırma planı kullanabilirsiniz.

- *Ücretsiz* planı kullanarak oluşturduğunuz uygulamalar yedi gün boyunca ücretsizdir ve en fazla beş cihaz destekler. Bunları, süresi dolmadan önce istediğiniz zaman standart bir fiyatlandırma planı kullanacak şekilde dönüştürebilirsiniz.
- *Standart* planı kullanarak oluşturduğunuz uygulamalar cihaz başına alınarak faturalandırılır, ilk iki cihazla ücretsiz olarak **Standart 1** veya **Standart 2** fiyatlandırma planı seçebilirsiniz. [IoT Central fiyatlandırması](https://aka.ms/iotcentral-pricing)hakkında daha fazla bilgi edinin.

## <a name="quotas"></a>Kotalar

Her Azure aboneliğinin, IoT çözümünüzün kapsamını etkileyebilecek varsayılan kotaları vardır. Şu anda IoT Central bir abonelikte dağıtabileceğiniz uygulama sayısını 10 ' a kısıtlar. Bu sınırı artırmanız gerekiyorsa, [Microsoft desteği](https://azure.microsoft.com/support/options/)'ne başvurun.

## <a name="known-issues"></a>Bilinen sorunlar

- Sürekli veri dışa aktarma avro biçimini (uyumsuzluk) desteklemez.
- GeoJSON Şu anda desteklenmiyor.
- Harita kutucuğu Şu anda desteklenmiyor.
- Dizi şeması türleri desteklenmez.
- Yalnızca C cihaz SDK 'Sı ve Node.js cihaz ve hizmet SDK 'Ları desteklenir.
- IoT Central Şu anda Birleşik Devletler, Avrupa, Asya Pasifik, Avustralya, Birleşik Krallık ve Japonya konumlarında sunulmaktadır.
- Birleşik Krallık ve Japonya konumlarında **özel uygulama (eski)** uygulama şablonunu kullanamazsınız.
- Cihaz yetenek modelleri aynı dosyada satır içi tanımlanmış tüm arabirimlere sahip olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Artık IoT Central bir genel bakışın olduğuna göre, aşağıda önerilen bazı adımlar verilmiştir:

- [IoT çözümleri oluşturmak için kullanılabilir Azure teknolojilerini ve hizmetlerini](../../iot-fundamentals/iot-services-and-technologies.md)anlayın.
- [Azure IoT Central Kullanıcı Arabirimi](overview-iot-central-tour.md)’ni tanıma.
- [Azure IoT Central uygulaması oluşturmaya](quick-deploy-iot-central.md) bağlama.
- [Azure IoT Edge cihazı bağlamayı](./tutorial-add-edge-as-leaf-device.md)öğrenin.
- [Azure IoT teknolojileri ve Hizmetleri](../../iot-fundamentals/iot-services-and-technologies.md)hakkında daha fazla bilgi edinin.

Bir cihaz geliştiricisiyseniz ve bazı kodlara gitmek istiyorsanız, önerilen sonraki adım, [bir istemci uygulamasını oluşturmak ve Azure IoT Central uygulamanıza bağlamak](./tutorial-connect-device-nodejs.md)için kullanılır.
