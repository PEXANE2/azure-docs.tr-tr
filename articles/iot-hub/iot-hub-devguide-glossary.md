---
title: Azure IoT Hub terimler sözlüğü | Microsoft Dokümanlar
description: Geliştirici kılavuzu - Azure IoT Hub makalelerinde kullanılan ortak terimlerden bazılarını açıklayan bir sözlük.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/15/2019
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 5c59ddf046e7ffe936b097878d5d049b43ea8aec
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81729018"
---
# <a name="glossary-of-iot-hub-terms"></a>IoT Hub terimleri sözlüğü

Bu makalede, IoT Hub makalelerinde kullanılan ortak terimlerden bazıları listelenilir.

## <a name="advanced-message-queueing-protocol"></a>Gelişmiş İleti Sıraya Protokolü

[Gelişmiş İleti Sıraya Protokolü (AMQP),](https://www.amqp.org/) [IoT Hub'ın](#iot-hub) aygıtlarla iletişim kurmak için desteklediği ileti protokollerinden biridir. IoT Hub'ın desteklediği ileti iletişim protokolleri hakkında daha fazla bilgi [için, IoT Hub ile ileti gönder ve al'](iot-hub-devguide-messaging.md)bakın.

## <a name="automatic-device-management"></a>Otomatik Cihaz Yönetimi

Azure IoT Hub'daki Otomatik Aygıt Yönetimi, büyük aygıt filolarını tüm yaşam döngüleri boyunca yönetmenin yinelenen ve karmaşık görevlerinin çoğunu otomatikleştirir. Otomatik Aygıt Yönetimi ile, özelliklerine göre bir aygıt kümesini hedefleyebilir, istenen yapılandırmayı tanımlayabilir ve ioT Hub'ın aygıtları kapsama girdiklerinde güncelleştirmesine izin verebilirsiniz.  Otomatik [aygıt yapılandırmaları](iot-hub-auto-device-config.md) ve [IoT Edge otomatik dağıtımlarından](../iot-edge/how-to-deploy-monitor.md)oluşur.

## <a name="automatic-device-configuration"></a>Otomatik aygıt yapılandırması

Çözüm arka uç sistem ölçümleri ve özel ölçümleri kullanarak aygıt [ikizleri](#device-twin) kümesine istenen özellikleri atamak ve durumu bildirmek için [otomatik aygıt yapılandırmaları](iot-hub-auto-device-config.md) kullanabilirsiniz. 

## <a name="azure-classic-cli"></a>Azure klasik CLI

[Azure klasik CLI,](../cli-install-nodejs.md) Microsoft Azure'da kaynak oluşturmak ve yönetmek için çapraz platform, açık kaynak kodlu, kabuk tabanlı bir komut aracıdır. CLI'nin bu sürümü yalnızca klasik dağıtımlar için kullanılmalıdır.

## <a name="azure-cli"></a>Azure CLI

[Azure CLI,](https://docs.microsoft.com/cli/azure/install-az-cli2) Microsoft Azure'da kaynak oluşturmak ve yönetmek için çapraz platform, açık kaynak, kabuk tabanlı, komut aracıdır.

## <a name="azure-iot-device-sdks"></a>Azure IoT aygıtı SDK'ları

Bir IoT hub'ı ile etkileşimedebilen [aygıt uygulamaları](#device-app) oluşturmanıza olanak tanıyan birden çok dil için kullanılabilir _aygıt SDK'ları_ vardır. IoT Hub öğreticileri, bu aygıt SDK'larını nasıl kullanacağınızı gösterir. Kaynak kodunu ve aygıt SDK'ları hakkında daha fazla bilgiyi bu GitHub [deposunda](https://github.com/Azure/azure-iot-sdks)bulabilirsiniz.

## <a name="azure-iot-explorer"></a>Azure IoT Gezgini

[Azure IoT Gezgini,](https://github.com/Azure/azure-iot-explorer) aygıtın gönderdiği telemetriyi görüntülemek, aygıt özellikleriyle çalışmak ve komutları aramak için kullanılır. Ayrıca aygıtlarınızla etkileşim kurmak ve bunları test etmek ve aygıtları takmak ve oynatmak için explorer'ı da kullanabilirsiniz.

## <a name="azure-iot-service-sdks"></a>Azure IoT hizmeti SDK'ları

Bir IoT hub'ı ile etkileşimedebilen [arka uç uygulamaları](#back-end-app) oluşturmanıza olanak tanıyan birden çok dil için hizmet _SDK'ları_ kullanılabilir. IoT Hub öğreticileri, bu hizmet SDK'larını nasıl kullanacağınızı gösterir. Kaynak kodunu ve hizmet SDK'ları hakkında daha fazla bilgiyi bu GitHub [deposunda](https://github.com/Azure/azure-iot-sdks)bulabilirsiniz.

## <a name="azure-iot-tools"></a>Azure IoT Araçları

[Azure IoT Araçları,](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) VS Kodu'ndaki Azure IoT Hub'ını ve aygıtları yönetmenize yardımcı olan çapraz platformlu, açık kaynaklı bir Visual Studio Kodu uzantısıdır. Azure IoT Araçları ile IoT geliştiricileri VS Code'da IoT projesini kolaylıkla geliştirebilir.

## <a name="azure-portal"></a>Azure portal

[Microsoft Azure portalı,](https://portal.azure.com) Azure kaynaklarınızı sağlayıp yönetebileceğiniz merkezi bir yerdir. İçeriğini _bıçaklar_kullanarak düzenler.

## <a name="azure-powershell"></a>Azure PowerShell

[Azure PowerShell,](/powershell/azure/overview) Windows PowerShell ile Azure'u yönetmek için kullanabileceğiniz bir cmdlet koleksiyonudur. Cmdlet'leri Azure platformu üzerinden sunulan çözümleri ve hizmetleri oluşturmak, sınamak, dağıtmak ve yönetmek için kullanabilirsiniz.

## <a name="azure-resource-manager"></a>Azure Resource Manager

[Azure Kaynak Yöneticisi,](../azure-resource-manager/management/overview.md) grup olarak çözümünüzdeki kaynaklarla çalışmanızı sağlar. Çözümünüz için kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleyebilir veya silebilirsiniz.

## <a name="azure-service-bus"></a>Azure Service Bus

[Service Bus,](../service-bus/index.md) şirket içi çözümleri bulutla bağlamanıza yardımcı olan kurumsal mesajlaşma ve geçişli iletişim ile bulut özellikli iletişim sağlar. Bazı IoT Hub öğreticileri Service Bus [kuyruklarını](../service-bus-messaging/service-bus-messaging-overview.md)kullanır.

## <a name="azure-storage"></a>Azure Depolama

[Azure Depolama](../storage/common/storage-introduction.md) bir bulut depolama çözümüdür. Yapılandırılmamış nesne verilerini depolamak için kullanabileceğiniz Blob Depolama hizmetini içerir. Bazı IoT Hub öğreticileri blob depolama kullanır.

## <a name="back-end-app"></a>Arka uç uygulaması

[IoT Hub](#iot-hub)bağlamında, bir arka uç uygulaması, bir IoT hub'ındaki hizmete bakan uç noktalardan birine bağlanan bir uygulamadır. Örneğin, bir arka uç uygulaması [aygıttan buluta](#device-to-cloud) iletileri alabilir veya [kimlik kaydını](#identity-registry)yönetebilir. Genellikle, bir arka uç uygulaması bulutta çalışır, ancak öğreticilerin çoğunda arka uç uygulamaları yerel geliştirme makinenizde çalışan konsol uygulamalarıdır.

## <a name="built-in-endpoints"></a>Yerleşik uç noktalar

Her IoT hub'ı, Olay Hub'ı uyumlu yerleşik bir [uç nokta](iot-hub-devguide-endpoints.md) içerir. Bu uç noktadan aygıttan buluta iletileri okumak için Olay Hub'larıyla çalışan tüm mekanizmaları kullanabilirsiniz.

## <a name="cloud-gateway"></a>Bulut ağ geçidi

Bulut ağ geçidi, doğrudan [IoT](#iot-hub)Hub'ına bağlanamayan aygıtlar için bağlantı sağlar. Bulut ağ geçidi, aygıtlarınıza yerel olarak çalışan bir [alan ağ geçidinin](#field-gateway) aksine bulutta barındırılır. Bulut ağ geçidi için tipik bir kullanım örneği, aygıtlarınız için protokol çevirisi uygulamaktır.

## <a name="cloud-to-device"></a>Bulut-aygıt

IoT hub'ından bağlı bir aygıta gönderilen iletileri ifade eder. Genellikle, bu iletiler aygıta eylem de etmesini söyleyen komutlardır. Daha fazla bilgi için [IoT Hub ile ileti gönder ve al.](iot-hub-devguide-messaging.md)

## <a name="configuration"></a>Yapılandırma

Otomatik aygıt [yapılandırması](iot-hub-auto-device-config.md)bağlamında, IoT Hub içindeki bir yapılandırma, bir aygıt kümesi için istenen yapılandırmayı tanımlar ve durumu ve ilerlemeyi bildirmek için bir dizi ölçüm sağlar.

## <a name="connection-string"></a>Bağlantı dizesi

Bir bitiş noktasına bağlanmak için gereken bilgileri kapsüllemek için uygulama kodunuzdaki bağlantı dizelerini kullanırsınız. Bağlantı dizesi genellikle bitiş noktası nın adresini ve güvenlik bilgilerini içerir, ancak bağlantı dize biçimleri hizmetler arasında farklılık gösterir. IoT Hub hizmetiyle ilişkili iki tür bağlantı dizesi vardır:

- *Aygıt bağlantı dizeleri,* aygıtların aygıta bakan uç noktalarına bir IoT hub'ında bağlanmasını sağlar.

- *IoT Hub bağlantı dizeleri, bir IoT* hub'ında hizmete bakan uç noktalara bağlanmak için arka uç uygulamaları etkinleştirin.

## <a name="custom-endpoints"></a>Özel uç noktalar

Yönlendirme [kuralı](#routing-rules)yla gönderilen iletileri teslim etmek için Bir IoT hub'ında özel [uç noktalar](iot-hub-devguide-endpoints.md) oluşturabilirsiniz. Özel uç noktaları doğrudan bir Olay hub'ına, Hizmet Veri Servisi kuyruğuna veya Hizmet Veri Servisi konusuna bağlanır.

## <a name="custom-gateway"></a>Özel ağ geçidi

Ağ geçidi, doğrudan [IoT](#iot-hub)Hub'ına bağlanamayan aygıtlar için bağlantı sağlar. İletileri, özel iletişim kuralı dönüşümlerini ve kenardaki diğer işlemleri işlemek için özel mantık uygulayan özel ağ geçitleri oluşturmak için Azure IoT Edge'i kullanabilirsiniz.

## <a name="data-point-message"></a>Veri noktası iletisi

Veri noktası iletisi, rüzgar hızı veya sıcaklık gibi [telemetri](#telemetry) verilerini içeren [aygıttan buluta](#device-to-cloud) iletidir.

## <a name="desired-configuration"></a>İstenilen yapılandırma

Bir [aygıt ikizi](iot-hub-devguide-device-twins.md)bağlamında, istenen yapılandırma aygıt la senkronize edilmesi gereken aygıt ikizindeki tüm özellikler ve meta veri kümesini ifade eder.

## <a name="desired-properties"></a>İstenilen özellikler

Bir [aygıt ikizi](iot-hub-devguide-device-twins.md)bağlamında, istenen özellikler, aygıt yapılandırmasını veya koşulunu eşitlemek için [bildirilen özelliklerle](#reported-properties) birlikte kullanılan aygıt ikizinin bir alt bölümüdür. İstenen özellikler yalnızca bir [arka uç uygulaması](#back-end-app) tarafından ayarlanabilir ve cihaz [uygulaması](#device-app)tarafından gözlemlenir.

## <a name="device-to-cloud"></a>Cihazdan buluta

Bağlı bir aygıttan [IoT](#iot-hub)Hub'ına gönderilen iletileri ifade eder. Bu iletiler [veri noktası](#data-point-message) veya [etkileşimli](#interactive-message) iletiler olabilir. Daha fazla bilgi için [IoT Hub ile ileti gönder ve al.](iot-hub-devguide-messaging.md)

## <a name="device"></a>Cihaz

IoT bağlamında, bir aygıt genellikle veri toplayabilir veya diğer aygıtları kontrol küçük ölçekli, bağımsız bir bilgi işlem aygıtıdır. Örneğin, bir cihaz bir çevre izleme cihazı veya seradaki sulama ve havalandırma sistemleri için bir denetleyici olabilir. [Aygıt kataloğu,](https://catalog.azureiotsolutions.com/) [IoT Hub](#iot-hub)ile çalışmak üzere sertifikalı donanım aygıtlarının bir listesini sağlar.

## <a name="device-app"></a>Cihaz uygulaması

Bir aygıt uygulaması [cihazınızda](#device) çalışır ve [IoT](#iot-hub)hub'ınızla iletişimi işler. Genellikle, bir aygıt uygulamasını uygularken [Azure IoT aygıtı SDK'larından](#azure-iot-device-sdks) birini kullanırsınız. IoT öğreticilerin çoğunda, kolaylık sağlamak için [simüle edilmiş](#simulated-device) bir aygıt kullanırsınız.

## <a name="device-condition"></a>Cihaz durumu

Bir [aygıt uygulaması](#device-app)tarafından bildirilen, şu anda kullanılmakta olan bağlantı yöntemi gibi aygıt durumu bilgilerini ifade eder. [Cihaz uygulamaları](#device-app) da yeteneklerini bildirebilir. Aygıt ikizlerini kullanarak koşul ve yetenek bilgilerini sorgulayabilirsiniz.

## <a name="device-data"></a>Cihaz verileri

Aygıt verileri, IoT Hub [kimlik kayıt defterinde](#identity-registry)depolanan aygıt başına verileri ifade eder. Bu verileri almak ve dışa aktarmak mümkündür.

## <a name="device-explorer"></a>Device explorer

Aygıt gezgini, aygıtın gönderdiği telemetriyi görüntülemek, aygıt özellikleriyle çalışmak ve arama komutları ile birlikte kullanılan [Azure IoT Gezgini](https://github.com/Azure/azure-iot-explorer)ile değiştirildi. Ayrıca aygıtlarınızla etkileşim kurmak ve bunları test etmek ve aygıtları takmak ve oynatmak için explorer'ı da kullanabilirsiniz.

## <a name="device-identity"></a>Cihaz kimliği

Aygıt kimliği, kimlik kaydında kayıtlı her aygıta atanan benzersiz [tanımlayıcıdır.](#identity-registry)

## <a name="device-management"></a>Cihaz yönetimi

Cihaz yönetimi, Planlama, sağlama, yapılandırma, izleme ve emekli etme dahil olmak üzere IoT çözümünüzdeki aygıtları yönetmeyle ilişkili tam yaşam döngüsünü kapsar.

## <a name="device-management-patterns"></a>Cihaz yönetimi modelleri

[IoT](#iot-hub) hub'ı, yeniden başlatma, fabrika sıfırlamalarını gerçekleştirme ve aygıtlarınızda firmware güncelleştirmeleri gerçekleştirme gibi yaygın aygıt yönetimi kalıplarını etkinleştiriyor.

## <a name="device-rest-api"></a>Cihaz REST API

Aygıt REST [API'sini](https://docs.microsoft.com/rest/api/iothub/device) bir aygıttan ioT hub'ına aygıttan buluta iletigöndermek ve bir IoT hub'ından [buluttan aygıta](#cloud-to-device) iletiler almak için kullanabilirsiniz. Genellikle, IoT Hub öğreticilerinde gösterildiği gibi üst düzey [aygıt SDK'larından](#azure-iot-device-sdks) birini kullanmanız gerekir.

## <a name="device-provisioning"></a>Cihaz sağlama

Aygıt sağlama, çözümünüzüzdeki mağazalara ilk [aygıt verilerini](#device-data) ekleme işlemidir. Hub'ınıza bağlanmak için yeni bir aygıtın etkinleştirmek için IoT Hub [kimlik kayıt defterine](#identity-registry)bir aygıt kimliği ve anahtarları eklemeniz gerekir. Sağlama işleminin bir parçası olarak, diğer çözüm mağazalarında aygıta özgü verileri başlatmanız gerekebilir.

## <a name="device-twin"></a>Cihaz çifti

[Aygıt ikizi,](iot-hub-devguide-device-twins.md) meta veriler, yapılandırmalar ve koşullar gibi aygıt durumu bilgilerini depolayan JSON belgesidir. [IoT Hub, IoT](#iot-hub) hub'ınızda sağlamanız gereken her aygıt için bir aygıt ikizini kalıcıyor. Aygıt ikizleri, aygıt ve çözüm arka uç arasında [aygıt koşullarını](#device-condition) ve yapılandırmaları senkronize etmenizi sağlar. Belirli aygıtları bulmak ve uzun süren işlemlerin durumunu sorgulamak için aygıt ikizlerini sorgulayabilirsiniz.

## <a name="direct-method"></a>Doğrudan yöntem

[Doğrudan yöntem,](iot-hub-devguide-direct-methods.md) IoT hub'ınıza bir API çağırarak aygıtta çalıştırılabilmek için bir yöntemi tetiklemenin bir yoludur.

## <a name="endpoint"></a>Uç Nokta

Bir IoT hub'ı, uygulamalarınızın IoT hub'ına bağlanmasını sağlayan birden çok [uç noktayı](iot-hub-devguide-endpoints.md) ortaya çıkarır. Aygıttan [buluta](#device-to-cloud) ileti gönderme ve [buluttan aygıta](#cloud-to-device) iletiler alma gibi işlemleri gerçekleştirmesini sağlayan aygıta bakan uç noktalar vardır. Arka [uç uygulamalarının](#back-end-app) [aygıt kimlik](#device-identity) yönetimi ve aygıt ikiz yönetimi gibi işlemleri gerçekleştirmesini sağlayan hizmete dönük yönetim uç noktaları vardır. Aygıttan buluta iletileri okumak için hizmete bakan [yerleşik uç noktalar](#built-in-endpoints) vardır. Yönlendirme [kuralı](#routing-rules)yla gönderilen aygıttan buluta iletileri almak için [özel uç noktalar](#custom-endpoints) oluşturabilirsiniz.

## <a name="event-hubs-service"></a>Olay Hub'ları hizmeti

[Olay Hub'ları,](../event-hubs/event-hubs-what-is-event-hubs.md) saniyede milyonlarca olay alabilen yüksek ölçeklenebilir bir veri girişi hizmetidir. Hizmet, bağlı cihazlarınız ve uygulamalarınız tarafından üretilen büyük miktardaver'i işlemenize ve analiz etmenizi sağlar. IoT Hub hizmetiyle karşılaştırma için [Azure IoT Hub'ı ve Azure Etkinlik Hub'larının Karşılaştırılması](iot-hub-compare-event-hubs.md)bölümüne bakın.

## <a name="event-hub-compatible-endpoint"></a>Olay Hub uyumlu bitiş noktası

IoT hub'ınıza gönderilen [aygıttan buluta](#device-to-cloud) iletileri okumak için hub'ınızdaki bir bitiş noktasına bağlanabilir ve bu iletileri okumak için Etkinlik Hub'ı uyumlu herhangi bir yöntemi kullanabilirsiniz. Olay Hub'ı uyumlu yöntemler arasında [Olay Hub'ları SDK'ları](../event-hubs/event-hubs-programming-guide.md) ve [Azure Akış Analizi'ni](../stream-analytics/stream-analytics-introduction.md)kullanmak yer almaktadır.

## <a name="field-gateway"></a>Alan ağ geçidi

Alan ağ geçidi, doğrudan [IoT](#iot-hub) Hub'ına bağlanamayan ve genellikle aygıtlarınızla yerel olarak dağıtılan aygıtlar için bağlantı sağlar. Daha fazla bilgi için Azure [IoT Hub nedir?](about-iot-hub.md)

## <a name="free-account"></a>Ücretsiz hesap

IoT Hub öğreticilerini tamamlamak ve IoT Hub hizmeti (ve diğer Azure hizmetleri) ile denemeler yapmak için ücretsiz bir [Azure hesabı](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.

## <a name="gateway"></a>Ağ geçidi

Ağ geçidi, doğrudan [IoT](#iot-hub)Hub'ına bağlanamayan aygıtlar için bağlantı sağlar. Ayrıca bakınız [Alan Ağ Geçidi](#field-gateway), Bulut Ağ [Geçidi](#cloud-gateway)ve Özel [Ağ Geçidi](#custom-gateway).

## <a name="identity-registry"></a>Kimlik kaydı

[Kimlik kaydı,](iot-hub-devguide-identity-registry.md) bir IoT hub'ına bağlanmasına izin verilen tek tek aygıtlar hakkında bilgi depolayan bir IoT hub'ının yerleşik bileşenidir.

## <a name="interactive-message"></a>Etkileşimli ileti

Etkileşimli ileti, çözümün arka ucunda anında bir eylemi tetikleyen [buluttan aygıta](#cloud-to-device) iletidir. Örneğin, bir aygıt crm sistemine otomatik olarak oturum açması gereken bir hata yla ilgili bir alarm gönderebilir.

[!INCLUDE [azure-iot-hub-edge-glossary-includes](../../includes/azure-iot-hub-edge-glossary-includes.md)]

## <a name="iot-hub"></a>IoT Hub

IoT Hub, milyonlarca cihaz ve bir çözüm arka uç arasında güvenilir ve güvenli çift yönlü iletişim sağlayan tam olarak yönetilen bir Azure hizmetidir. Daha fazla bilgi için Azure [IoT Hub nedir?](about-iot-hub.md) Azure [aboneliğinizi](#subscription)kullanarak, IoT ileti iş yüklerinizi işlemek için IoT hub'ları oluşturabilirsiniz.

## <a name="iot-hub-metrics"></a>IoT Hub ölçümleri

[IoT Hub ölçümleri,](iot-hub-metrics.md) [Azure aboneliğinizdeki](#subscription)IoT hub'larının durumu hakkında veri verir. IoT Hub ölçümleri, hizmetin genel durumunu ve ona bağlı aygıtları değerlendirmenize olanak tanır. IoT Hub ölçümleri, IoT hub'ınızda neler olup bittiğini görmenize ve Azure desteğine başvurmaya gerek kalmadan kök neden sorunlarını araştırmanıza yardımcı olabilir.

## <a name="iot-hub-query-language"></a>IoT Hub sorgu dili

[IoT Hub sorgu dili,](iot-hub-devguide-query-language.md) sizin ve aygıt ikizlerinizi [](#job) sorgulamanızı sağlayan SQL benzeri bir dildir.

## <a name="iot-hub-resource-rest-api"></a>IoT Hub Kaynak REST API

[IoT Hub Kaynak REST API'sini,](https://docs.microsoft.com/rest/api/iothub/iothubresource) [Azure aboneliğinizde](#subscription) hub oluşturma, güncelleştirme ve silme gibi işlemleri gerçekleştiren IoT hub'larını yönetmek için kullanabilirsiniz.

## <a name="iot-solution-accelerators"></a>IoT çözüm hızlandırıcıları

Azure IoT çözüm hızlandırıcıları, birden çok Azure hizmetlerini çözümlerde bir araya getirir. Bu çözümler, ortak IoT senaryolarının uçtan uca uygulamalarıyla hızlı bir şekilde başlamanızı sağlar. Daha fazla bilgi için Azure [IoT çözüm hızlandırıcıları nelerdir?](../iot-accelerators/about-iot-accelerators.md)

## <a name="the-iot-extension-for-azure-cli"></a>Azure CLI için IoT uzantısı 

[Azure CLI için IoT uzantısı](https://github.com/Azure/azure-iot-cli-extension) çapraz platform, komut satırı aracıdır. Araç, [aygıtlarınızı kimlik kayıt defterinde](#identity-registry)yönetmenize, aygıtlarınızdan ileti ve dosya göndermenize ve almanıza ve IoT hub işlemlerinizi izlemenize olanak tanır.

## <a name="job"></a>İş

Çözümünüz arka uç, IoT hub'ınıza kayıtlı bir aygıt kümesindeki etkinlikleri zamanlamak ve izlemek için [işleri](iot-hub-devguide-jobs.md) kullanabilir. Etkinlikler arasında [aygıtın](#desired-properties)istediği özelliklerin güncellenmesi, aygıtın ikiz [etiketlerinin](#tags)güncellenmesi ve [doğrudan yöntemlerin](#direct-method)yönlendirilen bir etkinlik olduğu yer almaktadır. [IoT Hub](#iot-hub) ayrıca [kimlik kaydına](#identity-registry) [almak ve dışa aktarmak](iot-hub-devguide-identity-registry.md#import-and-export-device-identities) için de kullanır.

## <a name="modules"></a>Modüller

Aygıt tarafında, IoT Hub aygıt SDK'ları, her birinin IoT Hub'a bağımsız bir bağlantı açtığı [modüller](iot-hub-devguide-module-twins.md) oluşturmanıza olanak tanır. Bu işlev, cihazınızdaki farklı bileşenler için ayrı ad alanları kullanmanıza olanak tanır.

Modül kimliği ve modül [ikizi, cihaz kimliği](#device-identity) ve [aygıt ikizi](#device-twin) ile aynı yetenekleri sağlar, ancak daha ince bir tanecikliolarak. Bu ince parçalılık, işletim sistemi tabanlı aygıtlar veya birden çok bileşeni yöneten firmware aygıtları gibi yetenekli aygıtların bu bileşenlerin her biri için yapılandırmayı ve koşulları yalıtmalarını sağlar.

## <a name="module-identity"></a>Modül kimliği

Modül kimliği, bir aygıta ait her modüle atanan benzersiz tanımlayıcıdır. Modül kimliği de [kimlik kaydına](#identity-registry)kaydedilir.

## <a name="module-twin"></a>Modül ikizi

Aygıt ikizine benzer şekilde, modül ikizi, meta veriler, yapılandırmalar ve koşullar gibi modül durumu bilgilerini depolayan JSON belgesidir. IoT Hub, IoT hub'ınızda bir aygıt kimliği altında sağladığınız her modül kimliği için bir modül ikizi üzerinde kalır. Modül ikizleri, modül ve çözüm arka uç arasında modül koşullarını ve yapılandırmaları senkronize etmenizi sağlar. Belirli modülleri bulmak ve uzun süren işlemlerin durumunu sorgulamak için modül ikizlerini sorgulayabilirsiniz.

## <a name="mqtt"></a>MQTT

[MQTT,](https://mqtt.org/) [IoT Hub'ın](#iot-hub) aygıtlarla iletişim kurmak için desteklediği mesajlaşma protokollerinden biridir. IoT Hub'ın desteklediği ileti iletişim protokolleri hakkında daha fazla bilgi [için, IoT Hub ile ileti gönder ve al'](iot-hub-devguide-messaging.md)bakın.

## <a name="operations-monitoring"></a>İşlemleri izleme

IoT Hub [işlemleri izleme,](iot-hub-operations-monitoring.md) IoT hub'ınızdaki operasyonların durumunu gerçek zamanlı olarak izlemenize olanak tanır. [IoT Hub,](#iot-hub) çeşitli işlem kategorileri boyunca olayları izler. Bir veya daha fazla kategoriden işlenmek üzere bir IoT Hub bitiş noktasına olay göndermeyi tercih edebilirsiniz. Verileri hatalar için izleyebilir veya veri desenlerine göre daha karmaşık bir işleme ayarlayabilirsiniz.

## <a name="physical-device"></a>Fiziksel cihaz

Fiziksel aygıt, Raspberry Pi gibi bir IoT hub'ına bağlanan gerçek bir aygıttır. Kolaylık sağlamak için, IoT Hub öğreticilerinin çoğu, örnekleri yerel makinenizde çalıştırmanızı sağlamak için [simüle edilmiş aygıtlar](#simulated-device) kullanır.

## <a name="primary-and-secondary-keys"></a>Birincil ve ikincil anahtarlar

Bir IoT hub'ında aygıta veya hizmete bakan bir bitiş noktasına bağlandığınızda, [bağlantı dizeniz](#connection-string) size erişim izni veren anahtarı içerir. [Kimlik kayıt defterine](#identity-registry) bir aygıt eklediğinizde veya hub'ınıza paylaşılan bir [erişim ilkesi](#shared-access-policy) eklediğinizde, hizmet birincil ve ikincil bir anahtar oluşturur. İki tuşa sahip olmak, IoT hub'ına erişimi kaybetmeden bir anahtarı güncellediğinizde bir tuştan diğerine yuvarlanmanızı sağlar.

## <a name="protocol-gateway"></a>Protokol ağ geçidi

Bir protokol ağ geçidi genellikle bulutta dağıtılır ve [IoT Hub'a](#iot-hub)bağlanan aygıtlar için protokol çeviri hizmetleri sağlar. Daha fazla bilgi için Azure [IoT Hub nedir?](about-iot-hub.md)

## <a name="quotas-and-throttling"></a>Kotalar ve azaltma

[IoT Hub](#iot-hub)kullanımınız için geçerli olan çeşitli [kotalar](iot-hub-devguide-quotas-throttling.md) vardır, kotaların çoğu IoT hub'ının katmanına bağlı olarak değişir. [IoT Hub,](#iot-hub) hizmetin çalışma zamanında kullanımınızda [da gaz](iot-hub-devguide-quotas-throttling.md) uygular.

## <a name="reported-configuration"></a>Bildirilen yapılandırma

Bir [aygıt ikizi](iot-hub-devguide-device-twins.md)bağlamında, bildirilen yapılandırma, çözüm arka uca bildirilmesi gereken aygıt ikizindeki özelliklerin ve meta verilerin tam kümesini ifade eder.

## <a name="reported-properties"></a>Bildirilen özellikler

Bir [aygıt ikizi](iot-hub-devguide-device-twins.md)bağlamında, bildirilen özellikler, aygıt yapılandırmasını veya koşulunu senkronize etmek için [istenen özelliklere](#desired-properties) sahip aygıt ikizinin bir alt bölümüdür. Bildirilen özellikler yalnızca aygıt [uygulaması](#device-app) tarafından ayarlanabilir ve bir [arka uç uygulaması](#back-end-app)tarafından okunabilir ve sorgulanabilir.

## <a name="resource-group"></a>Kaynak grubu

[Azure Kaynak Yöneticisi,](#azure-resource-manager) ilgili kaynakları bir araya getirmek için kaynak gruplarını kullanır. Gruptaki tüm kaynaklar üzerinde aynı anda işlem gerçekleştirmek için bir kaynak grubu kullanabilirsiniz.

## <a name="retry-policy"></a>Yeniden deneme ilkesi

Bir bulut hizmetine bağlandığınızda [geçici hataları](/azure/architecture/best-practices/transient-faults) işlemek için yeniden deneme ilkesi kullanırsınız.

## <a name="routing-rules"></a>Yönlendirme kuralları

IoT hub'ınızdaki [yönlendirme kurallarını,](iot-hub-devguide-messages-read-custom.md) aygıttan buluta iletileri yerleşik bir [uç noktaya](#built-in-endpoints) veya çözüm arka uçunuzda işlenmek üzere özel [uç noktalara](#custom-endpoints) yönlendirmek için yapılandırırsınız.

## <a name="sasl-plain"></a>SASL OVASI

SASL PLAIN, AMQP protokolünün güvenlik belirteçlerini aktarmak için kullandığı bir protokoldür.

## <a name="service-rest-api"></a>Hizmet REST API'si

Aygıtlarınızı yönetmek için çözüm arka ucundan [Service REST API'yi](https://docs.microsoft.com/rest/api/iothub/service/configuration) kullanabilirsiniz. API, [aygıt ikiz](#device-twin) özelliklerini almanızı ve güncelleştirmenizi, [doğrudan yöntemleri](#direct-method)çağırmanızı ve [işleri](#job)zamanlamanızı sağlar. Genellikle, IoT Hub öğreticilerinde gösterildiği gibi üst düzey [hizmet SDK'larından](#azure-iot-service-sdks) birini kullanmanız gerekir.

## <a name="shared-access-signature"></a>Paylaşılan erişim imzası

Paylaşılan Erişim İmzaları (SAS), SHA-256 güvenli iş birliğini veya URI'leri temel alan bir kimlik doğrulama mekanizmasıdır. SAS kimlik doğrulamanın iki bileşeni vardır: _Paylaşılan Erişim İlkesi_ ve _Paylaşılan Erişim İmzası_ (genellikle belirteç olarak adlandırılır). Bir aygıt, IoT hub'ı ile kimlik doğrulaması yapmak için SAS kullanır. [Arka uç uygulamaları,](#back-end-app) bir IoT hub'ındaki hizmete bakan uç noktalarıyla kimlik doğrulaması yapmak için SAS'ı da kullanır. Genellikle, bir uygulamanın Bir IoT hub'ına bağlantı kurmak için kullandığı [bağlantı dizesine](#connection-string) SAS belirteci eklersiniz.

## <a name="shared-access-policy"></a>Paylaşılan erişim ilkesi

Paylaşılan erişim ilkesi, bu ilkeyle ilişkili geçerli bir [birincil veya ikincil anahtarı](#primary-and-secondary-keys) olan herkese verilen izinleri tanımlar. [Portalda](#azure-portal)hub'ınız için paylaşılan erişim ilkelerini ve anahtarlarını yönetebilirsiniz.

## <a name="simulated-device"></a>Sanal cihaz

Kolaylık sağlamak için, IoT Hub öğreticilerinin çoğu, örnekleri yerel makinenizde çalıştırmanızı sağlamak için simüle edilmiş aygıtlar kullanır. Buna karşılık, fiziksel bir [aygıt,](#physical-device) Bir IoT hub'ına bağlanan Raspberry Pi gibi gerçek bir aygıttır.

## <a name="solution"></a>Çözüm
Bir _çözüm,_ bir veya daha fazla proje içeren bir Visual Studio çözümüne başvurabilir. Bir _çözüm,_ aygıtlar, [aygıt uygulamaları,](#device-app)IoT hub'ı, diğer Azure hizmetleri ve [arka uç uygulamaları](#back-end-app)gibi öğeleri içeren bir IoT çözümüne de başvurabilir.

## <a name="subscription"></a>Abonelik

Azure aboneliği, faturalandırmanın gerçekleştiği yerdir. Oluşturduğunuz her Azure kaynağı veya kullandığınız Azure hizmeti tek bir abonelikle ilişkilidir. Birçok kota da abonelik düzeyinde geçerlidir.

## <a name="system-properties"></a>Sistem özellikleri

Aygıt [ikizi](iot-hub-devguide-device-twins.md)bağlamında, sistem özellikleri salt okunur ve son etkinlik süresi ve bağlantı durumu gibi aygıt kullanımıyla ilgili bilgileri içerir.

## <a name="tags"></a>Etiketler

Bir [aygıt ikizi](iot-hub-devguide-device-twins.md)bağlamında, etiketler aygıt meta verileri depolanır ve çözüm arka uç tarafından bir JSON belge şeklinde alınır. Etiketler aygıttaki uygulamalar tarafından görülemez.

## <a name="telemetry"></a>Telemetri

Aygıtlar rüzgar hızı veya sıcaklık gibi telemetri verilerini toplar ve telemetriyi bir IoT hub'ına göndermek için veri noktası iletileri kullanır.

## <a name="token-service"></a>Belirteç hizmeti

Aygıtlarınız için bir kimlik doğrulama mekanizması uygulamak için bir belirteç hizmeti kullanabilirsiniz. *Aygıt kapsamı namına* belirteçler oluşturmak için **DeviceConnect** izinleriyle birlikte IoT Hub [paylaşılan erişim ilkesini](#shared-access-policy) kullanır. Bu belirteçler, aygıtın IoT hub'ınıza bağlanmasını sağlar. Aygıt, belirteç hizmetiyle kimlik doğrulaması yapmak için özel bir kimlik doğrulama mekanizması kullanır. Aygıt başarılı bir şekilde kimlik doğrulaması yaparsa, belirteç hizmeti, Aygıtın IoT hub'ınıza erişmek için kullanması için bir SAS belirteci verir.

## <a name="twin-queries"></a>İkiz sorgular

[Aygıt ve modül ikiz sorguları,](iot-hub-devguide-query-language.md) aygıtınızdan veya modül ikizlerinizden bilgi almak için SQL benzeri IoT Hub sorgu dilini kullanır. IoT hub'ınızda çalışma hakkında [](#job) bilgi almak için aynı IoT Hub sorgu dilini kullanabilirsiniz.

## <a name="twin-synchronization"></a>İkiz senkronizasyonu

İkiz senkronizasyon, aygıtlarınızı veya modüllerinizi yapılandırmak ve [bildirilen özellikleri](#reported-properties) ikizde depolamak için onlardan almak için aygıtınızda veya modül ikizlerinizde istenen [özellikleri](#desired-properties) kullanır.

## <a name="x509-client-certificate"></a>X.509 istemci sertifikası

Aygıt, [IoT Hub](#iot-hub)ile kimlik doğrulaması yapmak için X.509 sertifikası kullanabilir. X.509 sertifikası [kullanmak, SAS belirteci](#shared-access-signature)kullanmaya alternatiftir.
