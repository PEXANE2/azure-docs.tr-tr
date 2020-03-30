---
title: Terimler Sözlüğü - IoT Tak ve Çalıştır Önizleme | Microsoft Dokümanlar
description: Kavramlar - IoT Tak ve Çalıştır Önizleme ile ilgili ortak terimler sözlüğü.
author: ChrisGMsft
ms.author: chrisgre
ms.date: 12/23/2019
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: ef5ce9cc1cda7f1ff6b1985771e20cb20123e264
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025648"
---
# <a name="glossary-of-terms-for-iot-plug-and-play-preview"></a>IoT Tak ve Çalıştır Önizleme terimleri sözlüğü

IoT Tak ve Çalıştır makalelerinde kullanılan ortak terimlerin tanımları.

## <a name="azure-certified-for-iot-portal"></a>IoT portalı için Azure Sertifikalı

[IoT portalı için Azure Sertifikalı](https://aka.ms/ACFI) web sitesini şu şekilde kullanabilirsiniz:

- [IoT Tak ve Çalıştır cihazınız](#iot-plug-and-play-device)için sertifika [işlemini](#device-certification) tamamlayın.
- [Aygıt özelliği modellerini](#device-capability-model)bulun.
- Aygıt yetenek [modelini ortak model deposuna](#public-model-repository)yayımlayın.

## <a name="azure-cli"></a>Azure CLI

Azure CLI, Azure kaynaklarını yönetmek için bir çapraz platform, komut satırı aracıdır. Azure CLI için Azure IoT uzantısı, [IoT Tak ve Çalıştır aygıtlarıyla](#iot-plug-and-play-device)etkileşim kurmak ve test etmek için bir komut satırı aracıdır. Uzantıyı şu adreste kullanabilirsiniz:

- Bir IoT Tak ve Çalıştır aygıtına bağlanın.
- Aygıtın gönderdiği [telemetriyi](#telemetry) görüntüleyin.
- Aygıt [özellikleriyle](#properties)çalışın.
- Çağrı aygıtı [komutları.](#commands)
- [Model depolarını,](#model-repository) [arabirimleri](#interface)ve [aygıt yetenek modellerini](#device-capability-model)yönetin.

## <a name="azure-iot-central"></a>Azure IoT Central

Azure IoT Central, [IoT Tak ve Çalıştır cihazlarınızı](#iot-plug-and-play-device)bağlamayı, izlemeyi ve yönetmeyi kolaylaştıran, hizmet olarak yazılım olarak tamamen yönetilen bir çözümdür. Aygıtlarınızı izlemek ve yönetmek için bir IoT Merkezi uygulamasını otomatik olarak yapılandırmak için [aygıt özelliği modellerini](#device-capability-model) kullanabilirsiniz.

## <a name="azure-iot-certification-service"></a>Azure IoT sertifika hizmeti

Azure IoT sertifika hizmeti, [IoT için](#azure-certified-for-iot-portal)Azure Sertifikalı portalı üzerinden sertifikaiçin bir [IoT Tak ve Çalıştır aygıtı](#iot-plug-and-play-device) gönderdiğinizde bir dizi sertifika testi çalıştırMaktadır. [Sertifikalı IoT aygıt kataloğuna](#certified-for-iot-device-catalog)bir aygıt eklemeden önce, aygıtın sertifikalı olması gerekir.

## <a name="azure-iot-tools-extension"></a>Azure IoT Araçları uzantısı

Azure IoT Araçları, Visual Studio [kodunda](#visual-studio-code) IoT Hub ile etkileşimkurmanıza ve IoT aygıtları geliştirmenize yardımcı olan bir uzantı koleksiyonudur. IoT Tak ve Çalıştır aygıtı geliştirmeiçin şunları yardımcı olur:

- Yazar [cihaz yeteneği modelleri](#device-capability-model) ve [arayüzleri](#interface).
- Model [depoları](#model-repository)için yayımlayın.
- Aygıt uygulamasını uygulamak için iskelet kodu oluşturun.

## <a name="azure-iot-explorer-tool"></a>Azure IoT explorer aracı

Azure IoT gezgini, [IoT Tak ve Çalıştır aygıtlarınızla](#iot-plug-and-play-device)etkileşimde kalmak ve test etmek için kullanabileceğiniz bir grafik aracıdır. Aracı yerel makinenize yükledikten sonra şunları yapabilirsiniz:

- IoT hub'ınıza bağlı aygıtları [görüntüleyin.](#azure-iot-hub)
- Bir IoT Tak ve Çalıştır aygıtına bağlanın.
- Aygıtın gönderdiği [telemetriyi](#telemetry) görüntüleyin.
- Aygıt [özellikleriyle](#properties)çalışın.
- Çağrı aygıtı [komutları.](#commands)

## <a name="azure-iot-hub"></a>Azure IoT Hub’ı

IoT Hub, bulutta barındırılan ve IoT uygulamanız ve yönettiği cihazlar arasındaki iki yönlü iletişim için merkezi ileti hub’ı görevi gören, yönetilen bir hizmettir. [IoT Tak ve Çalıştır aygıtları](#iot-plug-and-play-device) bir IoT hub'ına bağlanabilir. Bir IoT çözümü, şunları etkinleştirmek için bir IoT hub'ı kullanır:

- Telemetriyi bulut tabanlı bir çözüme gönderecek cihazlar.
- Bağlı aygıtları yönetmek için bulut tabanlı bir çözüm.

## <a name="azure-iot-device-sdk"></a>Azure IoT cihazı SDK

IoT Tak ve Çalıştır aygıtı istemci uygulamalarını oluşturmak için kullanabileceğiniz birden çok dil için aygıt SDK'ları vardır. [Aygıt sertifikası](#device-certification) için gereken gereksinimlerden biri, aygıt istemci kodunun Azure IoT aygıt SDK'larından birini kullanmasıdır.

## <a name="certified-for-iot-device-catalog"></a>IoT cihaz kataloğu için onaylı

[Sertifikalı IoT aygıt kataloğu,](https://catalog.azureiotsolutions.com/) [aygıt sertifika](#device-certification) testlerini geçmiş [Olan IoT Tak ve Çalıştır aygıtlarını](#iot-plug-and-play-device) listeler. Katalogdaki IoT Tak ve Çalıştır aygıtları için [aygıt özelliği modelleri](#device-capability-model) ve genel model deposunda yayınlandı.

## <a name="commands"></a>Komutlar

[Arabirimde](#interface) tanımlanan komutlar, [dijital ikizüzerinde](#digital-twin)yürütülebilecek yöntemleri temsil eder. Örneğin, aygıtı yeniden başlatma komutu.

## <a name="common-interface"></a>Ortak arayüz

Tüm [IoT Tak ve Çalıştır aygıtlarının](#iot-plug-and-play-device) bazı ortak [arayüzleri](#interface)uygulaması beklenmektedir. Örneğin, aygıt bilgi arabirimi aygıt la ilgili donanım ve işletim sistemi bilgilerini tanımlar. [Aygıt sertifikası,](#device-certification) cihazınızın birkaç ortak arabirim uygulamasını gerektirir. Ortak model deposundan ortak arabirim tanımlarını alabilirsiniz.

## <a name="company-model-repository"></a>Şirket model deposu

Bir [kuruluş, aygıt özelliği modelleri](#device-capability-model) ve [arabirimleri](#interface)için özel mağaza olarak şirket [modeli deposunu](#model-repository) kullanabilir.

## <a name="connection-string"></a>Bağlantı dizesi

Bağlantı dizesi, bitiş noktasına bağlanmak için gereken bilgileri kapsüller. Bağlantı dizesi genellikle bitiş noktası nın adresini ve güvenlik bilgilerini içerir, ancak bağlantı dize biçimleri hizmetler arasında farklılık gösterir. IoT Hub hizmetiyle ilişkili iki tür bağlantı dizesi vardır:

- Aygıt bağlantı [dizeleri, IoT Tak ve Çalıştır aygıtlarının ioT](#iot-plug-and-play-device) hub'ındaki aygıta bakan uç noktalara bağlanmasını sağlar. Aygıttaki istemci kodu, bir IoT hub'ı ile güvenli bir bağlantı kurmak için bağlantı dizesini kullanır.
- IoT Hub bağlantı dizeleri, bir IoT hub'ında hizmete bakan uç noktalara güvenli bir şekilde bağlanmak için arka uç çözümlerinin ve araçları sağlar. Bu çözümler ve araçlar IoT hub'ını ve ona bağlı aygıtları yönetir.
- Şirket modeli depo bağlantı dizeleri arka uç çözümleri ve araçları güvenli bir [şirket modeli deposuna](#company-model-repository)bağlanmak için etkinleştirin. Bu çözümler ve araçlar depodaki [aygıt yetenek modellerini](#device-capability-model) ve [arabirimlerini](#interface) tüketir veya yönetir.

## <a name="device-capability-model"></a>Cihaz yetenek modeli

Aygıt özelliği modeli, bir [IoT Tak ve Çalıştır aygıtını](#iot-plug-and-play-device) açıklar ve aygıt tarafından uygulanan [arabirim](#interface) kümesini tanımlar. Aygıt yeteneği modeli genellikle fiziksel bir aygıta, ürüne veya SKU'ya karşılık gelir. Bir aygıt yeteneği modelini tanımlamak için [Dijital İkiz Tanımlı Dili](#digital-twin-definition-language) kullanırsınız.

## <a name="device-certification"></a>Cihaz sertifikası

Cihaz sertifikası, [ioT aygıtının Sertifikalı IoT aygıt kataloğuna](#certified-for-iot-device-catalog) ve [cihaz yetenek modeline](#device-capability-model) ve [genel model deposuna](#public-model-repository)eklenen [arayüzlere](#interface) eklenabilmesi için bir [IoT Tak ve Çalıştır cihazını](#iot-plug-and-play-device) onaylama işlemidir.

## <a name="device-developer"></a>Cihaz geliştirici

Aygıt geliştiricisi, [ioT Tak ve Çalıştır aygıtında](#iot-plug-and-play-device)çalışacak kod uygulamak için bir [aygıt yeteneği modeli,](#device-capability-model) [arabirimleri](#interface)ve [Bir Azure IoT aygıtı SDK](#azure-iot-device-sdk) kullanır.

## <a name="device-modeling"></a>Cihaz modelleme

Bir [aygıt geliştiricisi,](#device-developer) Bir [IoT Tak ve Çalıştır aygıtının](#iot-plug-and-play-device)özelliklerini modellemek için [Dijital İkiz Tanım Lı Dil'i](#digital-twin-definition-language) kullanır. Model bir model deposu kullanılarak paylaşılabilir. Aygıt geliştiricisi modelden iskelet aygıt kodu oluşturabilir. Bir [çözüm geliştiricisi](#solution-developer) modelden bir IoT çözümüsekleyebilir.

## <a name="device-provisioning-service"></a>Cihaz Sağlama Hizmeti

[Azure IoT Central,](#azure-iot-central) tüm cihaz kaydını ve bağlantısını yönetmek için Aygıt Sağlama Hizmetini kullanır. Daha fazla bilgi için [Azure IoT Central'daki Aygıt bağlantısına](../iot-central/core/concepts-get-connected.md)bakın. Cihaz kaydını ve IoT Hub tabanlı IoT çözümünüze bağlantıyı yönetmek için Aygıt Sağlama Hizmetini de kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure IoT Hub Aygıt Sağlama Hizmeti ile Sağlama aygıtları.](../iot-dps/about-iot-dps.md)

## <a name="device-registration"></a>Cihaz kaydı

Bir [IoT Tak ve Çalıştır aygıtının bir IoT](#iot-plug-and-play-device) çözümüne bağlanabilmesi için önce çözüme kaydedilmesi gerekir. [Azure IoT Central,](#azure-iot-central) aygıt kaydını yönetmek için [Aygıt Sağlama Hizmetini](#device-provisioning-service) kullanır. Özel bir IoT çözümünde, aygıtları Azure portalında veya programlı bir şekilde IoT hub'ınızla kaydedebilirsiniz.

## <a name="device-first"></a>Önce cihaz

[Azure IoT Central,](#azure-iot-central) aygıta ilk kayıt ve bağlantı senaryosunı destekler. Bu senaryoda, bir [IoT Tak ve Çalıştır aygıtı](#iot-plug-and-play-device) önceden kaydolmadan bir IoT Merkezi uygulamasına bağlanabilir. Kayıt, bir aygıt uygulamaya ilk bağlandığında otomatik olarak gerçekleşir.

## <a name="digital-twin"></a>Dijital ikiz

Dijital ikiz, [IoT Tak ve Çalıştır cihazının](#iot-plug-and-play-device)bir modelidir. Dijital [ikiz, Dijital İkiz Tanımlı Dil](#digital-twin-definition-language)kullanılarak modellenmiştir. Çalışma zamanında dijital ikizler ile etkileşim kurmak için [Azure IoT aygıt SDK'larını](#azure-iot-device-sdk) kullanabilirsiniz. Örneğin, bir aygıttaki dijital ikizde özellik değeri ayarlayabilirsiniz ve SDK bu değişikliği buluttaki IoT çözümünüze iletir.

## <a name="digital-twin-change-events"></a>Dijital ikiz değişim etkinlikleri

Bir [IoT Tak ve Çalıştır aygıtı](#iot-plug-and-play-device) bir [IoT hub'ına](#azure-iot-hub)bağlandığında, hub yönlendirme yeteneğini kullanarak dijital ikiz değişiklikleriyle ilgili bildirimler gönderebilir. Örneğin, [bir](#properties) aygıtta özellik değeri değiştiğinde, IoT Hub bir bildirimi Hizmet Veri Servisi kuyruğu gibi bir bitiş noktasına gönderebilir.

## <a name="digital-twin-definition-language"></a>Dijital İkiz Tanımlı Dil

[IoT Tak](#iot-plug-and-play-device)ve Çalıştır aygıtları için modelleri ve arayüzleri açıklayan bir dil. [Dijital ikizin](#digital-twin) yeteneklerini tanımlamak ve IoT platformu ve IoT çözümlerinin varlığın anlambiliminden yararlanmasını sağlamak için [Dijital İkiz Tanımlı Dili](https://aka.ms/DTDL) kullanın.

## <a name="digital-twin-route"></a>Dijital ikiz rota

Bir [IoT hub'ında,](#azure-iot-hub) Servis Veri Yolu kuyruğu gibi [dijital ikiz değiştirme olaylarını](#digital-twin-change-events) ve bitiş noktasına teslim etmek için ayarlanmış bir rota.

## <a name="interface"></a>Arabirim

Arabirim, bir [IoT Tak ve Çalıştır aygıtı](#iot-plug-and-play-device) veya [dijital ikiz](#digital-twin)tarafından uygulanan ilgili yetenekleri açıklar. Farklı [aygıt özelliği modelleri](#device-capability-model)arasında arabirimleri yeniden kullanabilirsiniz.

## <a name="iot-hub-query-language"></a>IoT Hub sorgu dili

IoT Hub sorgu dili birden çok amaç için kullanılır. Örneğin, dili IoT hub'ınıza [kayıtlı aygıtları](#device-registration) aramak veya [dijital ikiz yönlendirme](#digital-twin-route) davranışını hassaslaştırmak için kullanabilirsiniz.

## <a name="iot-plug-and-play-device"></a>IoT Tak ve Çalıştır cihazı

IoT Tak ve Çalıştır aygıtı genellikle veri toplayan veya diğer aygıtları kontrol eden ve bir [aygıt yetenek modeli](#device-capability-model)uygulayan yazılım veya yazılım çalıştıran küçük ölçekli, bağımsız bir bilgi işlem aygıtıdır.  Örneğin, bir IoT Tak ve Çalıştır cihazı bir çevre izleme cihazı veya akıllı tarım sulama sistemi için bir denetleyici olabilir. Komut, denetim ve IoT Tak ve Çalıştır aygıtlarından veri almak için bulut barındırılan bir IoT çözümü yazabilirsiniz. [Azure Sertifikalı IoT aygıt kataloğu,](#certified-for-iot-device-catalog) kullanılabilir IoT Tak ve Çalıştır aygıtlarını listeler. Katalogdaki her IoT Tak ve Çalıştır aygıtı doğrulandı ve bir [aygıt yeteneği modeli](#device-capability-model)var.

## <a name="microsoft-partner-center"></a>Microsoft İş Ortağı Merkezi

[Microsoft İş Ortağı Merkezi,](https://docs.microsoft.com/partner-center/) kuruluşunuzun Microsoft ile uçlardan uca ilişkisini yönettiği yerdir. [IoT Tak ve Çalıştır cihazınızı](#iot-plug-and-play-device) [Azure Sertifikalı IoT portalında](#azure-certified-for-iot-portal)onaylamadan önce bir Microsoft İş Ortağı Merkezi hesabına ihtiyacınız vardır.

## <a name="model-discovery"></a>Model keşfi

Bir [IoT Tak ve Çalıştır aygıtı](#iot-plug-and-play-device) bir IoT çözümüne bağlandığında, çözüm aygıt [yetenek modelini](#device-capability-model)bularak cihazın özelliklerini keşfedebilir. Bir aygıt, yetenek modelini çözüme gönderebilir veya çözüm bir [model deposunda](#model-repository)aygıt yetenek modeli bulabilir.

## <a name="model-repository"></a>Model deposu

Bir model [deposu, aygıt özelliği modellerini](#device-capability-model) ve [arabirimlerini](#interface)depolar. Tek bir [halka açık model deposu](#public-model-repository)var. Kuruluşlar kendi kuruluş modeli depolarını oluşturabilir.

## <a name="model-repository-rest-api"></a>Model deposu REST API

Model depolarını yönetmek ve bunlarla etkileşimde olmak için bir API. Örneğin, [aygıt yeteneği modelleri](#device-capability-model) eklemek ve yetenek modellerini aramak için API'yi kullanabilirsiniz.

## <a name="properties"></a>Özellikler

Özellikler, dijital ikizin bazı durumunu temsil eden bir [arabirimde](#interface) tanımlanan veri alanlarıdır. Özellikleri salt okunur veya yazılabilir olarak bildirebilirsiniz. Seri numarası gibi salt okunur özellikler, [IoT Tak ve Çalıştır aygıtında](#iot-plug-and-play-device) çalışan kodla ayarlanır.  Alarm eşiği gibi yazılabilir özellikler genellikle bulut tabanlı IoT çözümünden ayarlanır.

## <a name="public-model-repository"></a>Genel model deposu

[Sertifikalı aygıtlar](#device-certification)için [aygıt özelliği modellerini](#device-capability-model) ve [arabirimlerini](#interface) depolayan tek bir ortak model deposu vardır. Ortak model deposu da [ortak arabirim](#common-interface) tanımları de depolar.

## <a name="registration-id"></a>Kayıt Kimliği

Kayıt kimliği, [Aygıt Sağlama Hizmeti'ndeki](#device-provisioning-service)bir aygıtı benzersiz olarak tanımlar. Bu kimlik, [IoT](#azure-iot-hub)hub'ındaki bir aygıt için benzersiz bir tanımlayıcı olan aygıt kimliğiyle aynı değildir.

## <a name="scope-id"></a>Kapsam Kimliği

Kapsam Kimliği kapsamı, aygıt [sağlama hizmeti](#device-provisioning-service) örneğini benzersiz olarak tanımlar.

## <a name="shared-access-signature"></a>Paylaşılan erişim imzası

Paylaşılan erişim imzaları SHA-256 güvenli iş birliğini veya URI'leri temel alan bir kimlik doğrulama mekanizmasıdır. Paylaşılan erişim imzası kimlik doğrulaması iki bileşenden oluşur: paylaşılan erişim ilkesi ve paylaşılan erişim imzası (genellikle belirteç olarak adlandırılır). Bir [IoT Tak ve Çalıştır aygıtı,](#iot-plug-and-play-device) bir [IoT hub'ı](#azure-iot-hub)ile kimlik doğrulaması yapmak için paylaşılan bir erişim imzası kullanır.

## <a name="solution-developer"></a>Çözüm geliştiricisi

Bir çözüm geliştiricisi çözümü arka uçta oluşturur. Bir çözüm geliştiricisi genellikle [IoT Hub](#azure-iot-hub) ve [model depoları](#model-repository)gibi Azure kaynaklarıyla çalışır veya [IoT Central](#azure-iot-central)ile çalışır.

## <a name="telemetry"></a>Telemetri

[Arabirimde](#interface) tanımlanan telemetri alanları ölçümleri temsil eder. Bu ölçümler genellikle [IoT Tak ve Çalıştır aygıtı](#iot-plug-and-play-device) tarafından veri akışı olarak gönderilen sensör okumaları gibi değerlerdir.

## <a name="visual-studio-code"></a>Visual Studio kodu

Visual Studio kodu, birden fazla platform için kullanılabilen modern bir kod düzenleyicisidir. [Azure IoT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) paketindeki ler gibi uzantılar, çok çeşitli geliştirme senaryolarını destekleyecek şekilde düzenleyiciyi özelleştirmenize olanak tanır.
