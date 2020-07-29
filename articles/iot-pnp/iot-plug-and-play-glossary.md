---
title: Terimler sözlüğü-IoT Tak ve Kullan önizlemesi | Microsoft Docs
description: Kavramlar-IoT Tak ve Kullan önizlemesiyle ilgili yaygın koşulların bir sözlüğü.
author: dominicbetts
ms.author: dobett
ms.date: 07/22/2020
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 7d88ac62d9117df9d24c6d865e684a0972c87dae
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87337322"
---
# <a name="glossary-of-terms-for-iot-plug-and-play-preview"></a>IoT Tak ve Kullan önizlemesi için terimler sözlüğü

IoT Tak ve Kullan makalelerinde kullanılan yaygın terimlerin tanımları.

## <a name="azure-iot-explorer-tool"></a>Azure IoT gezgin aracı

Azure IoT Explorer, [ıot Tak ve kullan cihazlarınızla](#iot-plug-and-play-device)etkileşim kurmak ve test etmek için kullanabileceğiniz bir grafik aracıdır. Aracı yerel makinenize yükledikten sonra, şunları yapmak için kullanabilirsiniz:

- [IoT Hub 'ınıza](#azure-iot-hub)bağlı cihazları görüntüleyin.
- IoT Tak ve Kullan cihazına bağlanın.
- Cihaz [bileşenlerini](#component)görüntüleyin.
- Cihazın gönderdiği [Telemetriyi](#telemetry) görüntüleyin.
- Cihaz [özellikleriyle](#properties)çalışın.
- Çağrı cihazı [komutları](#commands).

## <a name="azure-iot-hub"></a>Azure IoT Hub

IoT Hub, bulutta barındırılan ve IoT uygulamanız ve yönettiği cihazlar arasındaki iki yönlü iletişim için merkezi ileti hub’ı görevi gören, yönetilen bir hizmettir. [Iot Tak ve kullan cihazları](#iot-plug-and-play-device) , IoT Hub 'ına bağlanabilir. IoT çözümü, şunları sağlamak için bir IoT Hub 'ı kullanır:

- Bulut tabanlı bir çözüme telemetri göndermek için cihazlar.
- Bağlı cihazları yönetmek için bulut tabanlı bir çözüm.

## <a name="azure-iot-device-sdk"></a>Azure IoT cihaz SDK 'Sı

IoT Tak ve Kullan cihaz istemci uygulamaları oluşturmak için kullanabileceğiniz birden çok dil için cihaz SDK 'Ları vardır.

## <a name="commands"></a>Komutlar

Bir [arabirimde](#interface) tanımlanan komutlar, [dijital ikizi](#digital-twin)yürütülebilecek yöntemleri temsil eder. Örneğin, bir cihazı yeniden başlatmak için bir komut.

## <a name="component"></a>Bileşen

Bileşenler, model [arabirimini](#interface) diğer arabirimlerin derlemesi olarak oluşturmanıza imkan tanır. Bir [cihaz modeli](#device-model) , birden çok arabirimi bileşen olarak birleştirebilir. Örneğin, bir model bir switch bileşeni ve termostat bileşeni içerebilir. Bir modeldeki birden çok bileşen aynı arabirim türünü de kullanabilir. Örneğin, bir model iki termostat bileşeni içerebilir.

## <a name="connection-string"></a>Bağlantı dizesi

Bir bağlantı dizesi, bir uç noktaya bağlanmak için gereken bilgileri kapsüller. Bağlantı dizesi genellikle uç nokta ve güvenlik bilgilerinin adresini içerir, ancak bağlantı dizesi biçimleri hizmetler arasında farklılık gösterir. IoT Hub hizmetiyle ilişkili iki tür bağlantı dizesi vardır:

- Cihaz bağlantı dizeleri IoT Hub 'ındaki cihaza yönelik uç noktalara bağlanmak için [ıot Tak ve kullan cihazları](#iot-plug-and-play-device) etkinleştirir. Bir cihazdaki istemci kodu, IoT Hub ile güvenli bir bağlantı kurmak için bağlantı dizesini kullanır.
- IoT Hub bağlantı dizeleri, bir IoT Hub 'ında hizmete yönelik uç noktalara güvenli bir şekilde bağlanmak için arka uç çözümlerini ve araçlarını etkinleştirir. Bu çözümler ve araçlar, IoT Hub 'ını ve ona bağlı olan cihazları yönetir.

## <a name="device-model"></a>Cihaz modeli

Bir cihaz modeli, [ıot Tak ve kullan cihazını](#iot-plug-and-play-device) açıklar ve cihazı oluşturan [bileşenleri](#component) tanımlar. Basit bir cihaz modelinde ayrı bileşenler yoktur ve tek bir kök düzeyinde arabirim için bir tanım içerir. Daha karmaşık bir cihaz modeli birden çok bileşen içerir. Bir cihaz modeli genellikle fiziksel bir cihaza, ürüne veya SKU 'ya karşılık gelir. Bir cihaz modeli tanımlamak için [dijital TWINS tanım dili sürüm 2](#digital-twins-definition-language) ' i kullanırsınız.

## <a name="device-builder"></a>Cihaz Oluşturucu

Bir cihaz Oluşturucu, [ıot Tak ve kullan cihazında](#iot-plug-and-play-device)çalıştırılacak kodu uygularken bir [cihaz modeli](#device-model) ve [arabirimleri](#interface) kullanır. Cihaz oluşturucular genellikle cihaz istemcisini uygulamak için [Azure IoT cihaz SDK](#azure-iot-device-sdk) 'larından birini kullanır, ancak bu gerekli değildir.

## <a name="device-modeling"></a>Cihaz modellemesi

Bir [cihaz Oluşturucu](#device-builder) , [IoT Tak ve kullan cihazının](#iot-plug-and-play-device)yeteneklerini modellemek Için [dijital TWINS tanım dilini](#digital-twins-definition-language) kullanır. Bir [çözüm Oluşturucusu](#solution-builder) , bir IoT çözümünü modelden yapılandırabilir.

## <a name="digital-twin"></a>Dijital ikizi

Dijital ikizi, [ıot Tak ve kullan cihazının](#iot-plug-and-play-device)bir modelidir. Dijital bir ikizi, [dijital TWINS tanım dili](#digital-twins-definition-language)kullanılarak modellenir. Çalışma zamanında dijital TWINS ile etkileşim kurmak için [Azure IoT cihaz SDK](#azure-iot-device-sdk) 'larını kullanabilirsiniz. Örneğin, bir cihazdaki dijital ikizi bir özellik değeri ayarlayabilirsiniz ve SDK bu değişikliği buluttaki IoT çözümünüz ile iletişim kurar.

## <a name="digital-twin-change-events"></a>Dijital ikizi değişiklik olayları

[Iot Tak ve Kullan cihazı](#iot-plug-and-play-device) bir [IoT Hub 'ına](#azure-iot-hub)bağlandığında, hub, dijital ikizi değişikliklerinin bildirimlerini göndermek için yönlendirme özelliğini kullanabilir. Örneğin, bir cihazdaki [özellik](#properties) değeri her değiştiğinde IoT Hub, bir olay hub 'ı gibi bir uç noktaya bildirim gönderebilir.

## <a name="digital-twins-definition-language"></a>Dijital TWINS tanım dili

[Iot Tak ve kullan cihazları](#iot-plug-and-play-device)için modelleri ve arabirimleri tanımlamaya yönelik bir dil. Dijital ikizi özelliklerini anlatmak için [dijital TWINS tanım dili sürüm 2](https://github.com/Azure/opendigitaltwins-dtdl) ' nı kullanın ve IoT Platformu ile IoT çözümlerini varlığın semantiğinin bir [üstünden](#digital-twin) yararlanmak için etkinleştirin.

## <a name="digital-twin-route"></a>Dijital ikizi rotası

Bir [IoT Hub 'ında](#azure-iot-hub) , bir olay hub 'ı gibi [dijital ikizi değişiklik olayları](#digital-twin-change-events) ve uç nokta sunmak için bir yol.

## <a name="interface"></a>Arabirim

Bir arabirim, [ıot Tak ve kullan cihazından](#iot-plug-and-play-device) veya [dijital ikizi](#digital-twin)tarafından uygulanan ilgili özellikleri açıklar. Arabirimleri farklı [cihaz modelleriyle](#device-model)yeniden kullanabilirsiniz. Bir arabirim bir cihaz modelinde kullanıldığında, cihazın bir [bileşenini](#component) tanımlar.

## <a name="iot-hub-query-language"></a>IoT Hub sorgu dili

IoT Hub sorgu dili birden çok amaçla kullanılır. Örneğin, IoT Hub 'ınıza kayıtlı cihazları aramak veya [dijital ikizi yönlendirme](#digital-twin-route) davranışını daraltmak için dilini kullanabilirsiniz.

## <a name="iot-plug-and-play-device"></a>IoT Tak ve Kullan cihazı

IoT Tak ve Kullan cihazı genellikle veri toplayan veya diğer cihazları denetleyen ve bir [cihaz modeli](#device-model)uygulayan yazılım veya bellenim çalıştıran bir küçük ölçekli, tek başına bilgi işlem aygıtıdır.  Örneğin, bir IoT Tak ve Kullan cihazı, bir çevre izleme cihazı veya bir akıllı tarım sulama sistemi denetleyicisi olabilir. Komut, denetim ve IoT Tak ve Kullan cihazlarındaki verileri almak için bulutta barındırılan bir IoT çözümü yazabilirsiniz.

## <a name="iot-plug-and-play-conventions"></a>IoT Tak ve Kullan kuralları

IoT Tak ve Kullan [cihazların](#iot-plug-and-play-device) bir çözüm ile veri alışverişi yaparken bir dizi [kural](concepts-convention.md) izlemesi beklenir.

## <a name="model-discovery"></a>Model keşfi

[Iot Tak ve Kullan cihazı](#iot-plug-and-play-device) bir IoT çözümüne bağlanırsa, çözüm cihaz [modelini](#device-model)bularak cihazın yeteneklerini bulabilir. Bu çözüm, bir [model deposunda](#model-repository) veya başka bir konumda cihaz modeli bulabilir.

## <a name="model-id"></a>Model Kimliği

IoT Tak ve Kullan cihazı bir IoT Hub bağlandığı zaman, uyguladığı [Dtdl](#digital-twins-definition-language) MODELININ **model kimliğini** gönderir. Bu, çözümün cihaz modelini bulmasını sağlar.

## <a name="model-repository"></a>Model deposu

[Model deposu](concepts-model-repository.md) [cihaz modellerini](#device-model) ve [arabirimlerini](#interface)depolar.

## <a name="model-repository-rest-api"></a>Model deposu REST API

Model deposunu yönetmek ve bunlarla etkileşim kurmak için bir API. Örneğin, API 'yi, [cihaz modellerini](#device-model)eklemek ve aramak için kullanabilirsiniz.

## <a name="properties"></a>Özellikler

Özellikler, bir Digital ikizi 'ın bazı durumlarını temsil eden bir [arabirimde](#interface) tanımlanan veri alanlarıdır. Özellikleri salt okunurdur veya yazılabilir olarak bildirebilirsiniz. Seri numarası gibi salt yazılır özellikler, [ıot Tak ve kullan cihazının](#iot-plug-and-play-device) kendisi üzerinde çalışan kodla ayarlanır.  Bir alarm eşiği gibi yazılabilir özellikler genellikle bulut tabanlı IoT çözümünden ayarlanır.

## <a name="shared-access-signature"></a>Paylaşılan erişim imzası

Paylaşılan erişim imzaları, SHA-256 güvenli karmaları veya URI 'Leri temel alan bir kimlik doğrulama mekanizmasıdır. Paylaşılan erişim imzası kimlik doğrulaması iki bileşene sahiptir: paylaşılan erişim ilkesi ve paylaşılan erişim imzası (genellikle belirteç olarak adlandırılır). [Iot Tak ve Kullan cihazı](#iot-plug-and-play-device) , [IoT Hub](#azure-iot-hub)ile kimlik doğrulamak için paylaşılan erişim imzasını kullanır.

## <a name="solution-builder"></a>Çözüm Oluşturucu

Çözüm Oluşturucu, çözüm arka ucu oluşturur. Çözüm Oluşturucu genellikle [IoT Hub](#azure-iot-hub) ve [model depoları](#model-repository)gibi Azure kaynaklarıyla birlikte kullanılabilir.

## <a name="telemetry"></a>Telemetri

Bir [arabirimde](#interface) tanımlanan telemetri alanları ölçümleri temsil eder. Bu ölçümler genellikle [ıot Tak ve kullan cihazının](#iot-plug-and-play-device) veri akışı olarak gönderdiği algılayıcı ayarları gibi değerlerdir.
