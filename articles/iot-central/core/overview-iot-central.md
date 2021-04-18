---
title: Azure IoT Central nedir? | Microsoft Docs
description: IoT Central, güvenli bir şekilde barındırılan bir IoT uygulama platformudur, İşletmeniz büyüdükçe sizinle ölçeklenmektedir ve mevcut iş uygulamalarınızla tümleştirilir. Bu makalede Azure IoT Central’ın özelliklerine genel bir bakış sunulmaktadır.
author: vishwam
ms.author: vishwams
ms.date: 04/16/2021
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc, contperf-fy21q2
ms.openlocfilehash: 1ff8243f9f4d070592ae6dc99e7c33472a6a90de
ms.sourcegitcommit: 950e98d5b3e9984b884673e59e0d2c9aaeabb5bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2021
ms.locfileid: "107600451"
---
# <a name="what-is-azure-iot-central"></a>Azure IoT Central nedir?

IoT Central, güvenli bir şekilde barındırılan bir IoT uygulama platformudur, İşletmeniz büyüdükçe sizinle ölçeklenmektedir ve mevcut iş uygulamalarınızla tümleştirilir. IoT Central ile oluşturmayı seçtiğinizde, yalnızca karmaşık ve sürekli gelişen IoT altyapısını korumak ve güncelleştirmek yerine, işletmenizin IoT verileriyle dönüştürülmesi için zaman, para ve enerji tasarrufu sağlama fırsatı sunulmaktadır.

IoT Central cihazları Hızlı bir şekilde bağlamanıza, cihaz koşullarını izlemenize, kurallar oluşturmanıza ve milyonlarca cihazı ve verilerini yaşam döngülerinde yönetmenize olanak sağlar. Ayrıca, IoT Intelligence 'ı iş kolu uygulamalarına genişleterek cihaz öngörülerine göre hareket etmenizi sağlar.

## <a name="create-your-iot-central-application"></a>IoT Central uygulamanızı oluşturma

Hızlı bir şekilde yeni bir IoT Central uygulaması oluşturabilir ve bu uygulamayı benzersiz gereksinimlerinize göre özelleştirebilirsiniz. Bir genel _uygulama şablonuyla_ veya [Perakende](../retail/overview-iot-central-retail.md), [enerji](../energy/overview-iot-central-energy.md), [devlet](../government/overview-iot-central-government.md)veya [sağlık](../healthcare/overview-iot-central-healthcare.md)için sektör odaklı uygulama şablonlarından biriyle başlayabilirsiniz.

İlk uygulamanızı nasıl oluşturacağınız hakkında daha fazla bilgi için bkz. [Yeni uygulama](quick-deploy-iot-central.md) hızlı başlangıcı oluşturma.

## <a name="connect-your-devices"></a>Cihazlarınızı bağlama
Uygulamanızı oluşturduktan sonra ilk adım cihazlarınızın bağlanacağı bir adımdır. Cihazları IoT Central uygulamanıza bağlamak için bir giriş için bkz. [cihaz geliştirmeye genel bakış](./overview-iot-central-developer.md) .

### <a name="device-templates"></a>Cihaz şablonları

IoT Central cihazlar bir _cihaz şablonuyla_ ilişkilendirilir. Bir cihaz şablonu bir Blueprint gibidir: cihazlarınızın özelliklerini ve davranışlarını tanımlar, örneğin:

- Telemetriler, örneğin sıcaklık veya nem gibi sensörlerden ölçümleri temsil eder.
- Bir cihazın dayanıklı durumunu temsil eden özellikler. Örnek olarak bir cihaz için bir Coolant pompa veya hedef sıcaklık durumu verilebilir. Özellikleri salt okunurdur veya yazılabilir olarak bildirebilirsiniz. Yalnızca cihazlar salt okunurdur bir özelliğin değerini güncelleştirebilir. Bir işleç bir cihaza göndermek için yazılabilir bir özelliğin değerini ayarlayabilir.
- Komutlar, bir cihazda tetiklenebilecek işlemler, örneğin, bir cihazı uzaktan yeniden başlatmak için bir komut.
- IoT Central uygulamasında depolanacak cihaz meta verileri olan bulut özellikleri (örneğin, müşteri adresi veya son bakım tarihi).

Daha fazla bilgi edinmek için [cihaz şablonu oluşturma](howto-set-up-template.md) makalesine bakın.

### <a name="customize-the-ui"></a>Kullanıcı arabirimini özelleştirme

Uygulamanın gündelik kullanımı için sorumlu olan operatörler için IoT Central uygulamasını özelleştirebilirsiniz, örneğin:

- Operatörlerin içgörüleri keşfetmesine ve sorunları daha hızlı çözümlemesine yardımcı olacak özel panoları yapılandırma.


## <a name="manage-your-devices"></a>Cihazlarınızı yönetme


Ölçekte çalışacak şekilde tasarlanan tüm IoT çözümleri ile cihaz yönetimine yönelik yapılandırılmış bir yaklaşım önemlidir. Cihazlarınızı buluta bağlamak yeterli değildir, cihazlarınızı bağlı ve sağlıklı tutmanız gerekir.

Şunları yapmak için IoT Central uygulamanızı kullanarak [cihazları yönetebilirsiniz](howto-manage-devices.md) :

- Cihazları izleme.
- Cihazlarla ilgili sorunları giderme ve düzeltme.
- Cihazlarda toplu güncelleştirmeler gerçekleştirin.

### <a name="dashboards"></a>Panolar

Yerleşik [panolar](./howto-set-up-template.md#generate-default-views) cihaz sistem durumunu ve telemetrisini izlemek için özelleştirilebilir bir kullanıcı arabirimi sağlar. [Uygulama şablonunda](howto-use-app-templates.md) önceden oluşturulmuş bir panoyla başlayın veya işleçlerinizin ihtiyaçlarına göre özel kendi panolarınızı oluşturun. Panoları uygulamanızdaki tüm kullanıcılarla paylaşabilir veya özel tutabilirsiniz.

### <a name="rules-and-actions"></a>Kurallar ve eylemler

Dikkat edilmesi gereken cihazları tanımlamak için cihaz durumu ve telemetri temelinde [özel kurallar](tutorial-create-telemetry-rules.md) oluşturun. Doğru kişilere bildirimde bulunan eylemleri yapılandırın ve düzeltici ölçülerin zamanında alındığından emin olun.

### <a name="jobs"></a>İşler

[İşler](howto-run-a-job.md) , özellikleri ayarlayarak veya komutları çağırarak cihazlara tek veya toplu güncelleştirmeler uygulamanıza olanak tanır.

### <a name="analytics"></a>Analiz
[Analiz](howto-create-analytics.md) , geçmiş eğilimleri çözümlemek ve cihazlarınızdan çeşitli Telemetriler ilişkilendirmek için zengin yetenekler sunar.

## <a name="integrate-with-other-services"></a>Diğer hizmetlerle tümleştirme

Bir uygulama platformu olarak, IoT Central IoT verilerinizi, işlem yapılabilir sonuçları sağlayan iş öngörülerine dönüştürmenize olanak tanır. [Kurallar](./tutorial-create-telemetry-rules.md), [veri dışa aktarma](./howto-export-data.md)ve [genel REST API](/learn/modules/manage-iot-central-apps-with-rest-api/) , IoT Central iş kolu uygulamalarıyla nasıl tümleştirebileceğinizi gösteren örneklerdir:

![IoT verilerinizi nasıl dönüştürebileceğinizi IoT Central](media/overview-iot-central/transform.png)

Cihazlarınızdan Telemetriyi işlemek ve sonuçları depolamak için özel analiz işlem hatları oluşturarak makine verimlilik eğilimlerini belirleme veya bir fabrika katında gelecekteki enerji kullanımını tahmin etme gibi iş öngörüleri oluşturabilirsiniz. Verilerinizi tercih ettiğiniz araçlarla analiz edebilir, depolayabileceğiniz ve görselleştirebileceğiniz diğer hizmetlere dışarı aktarmak için IoT Central uygulamanızdaki veri dışarı aktarmaları yapılandırın.

### <a name="build-custom-iot-solutions-and-integrations-with-the-rest-apis"></a>REST API 'Leriyle özel IoT çözümleri ve tümleştirmeler oluşturun

Şu gibi IoT çözümleri oluşturun:

- Cihazları uzaktan ayarlayıp denetleyebileceği mobil yardımcı uygulamalar.
- Mevcut iş kolu uygulamalarının IoT cihazlarınızla ve verilerinizle etkileşime geçmesini sağlayan özel tümleştirmeler.
- Cihaz modellemesi, ekleme, yönetim ve veri erişimi için cihaz yönetimi uygulamaları.

## <a name="administer-your-application"></a>Uygulamanızı yönetme

IoT Central uygulamalar Microsoft tarafından tamamen barındırılır ve uygulamalarınızın yönetilmesi için yönetim yükünü azaltır. Yöneticiler [, Kullanıcı rolleri ve izinleriyle](howto-administer.md)uygulamanıza erişimi yönetir.

## <a name="pricing"></a>Fiyatlandırma

7 günlük ücretsiz deneme sürümünü kullanarak bir IoT Central uygulaması oluşturabilir veya standart bir fiyatlandırma planı kullanabilirsiniz.

- *Ücretsiz* planı kullanarak oluşturduğunuz uygulamalar yedi gün boyunca ücretsizdir ve en fazla beş cihaz destekler. Bunları, süresi dolmadan önce istediğiniz zaman standart bir fiyatlandırma planı kullanacak şekilde dönüştürebilirsiniz.
- *Standart* planı kullanarak oluşturduğunuz uygulamalar, cihaz başına temelinde faturalandırılır. İlk iki cihazın serbest bırakılmasıyla birlikte **Standart 0**, **Standart 1** veya **Standart 2** fiyatlandırma planı seçebilirsiniz. [IoT Central fiyatlandırması](https://aka.ms/iotcentral-pricing)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Artık IoT Central bir genel bakışın olduğuna göre, aşağıda önerilen bazı adımlar verilmiştir:

- [Azure IoT Central uygulaması oluşturmaya](quick-deploy-iot-central.md) bağlama.
- [Azure IoT Central Kullanıcı Arabirimi](overview-iot-central-tour.md)’ni tanıma.
- Bir cihaz geliştiricisiyseniz ve bazı kodları incelemek istiyorsanız, [bir istemci uygulamasını oluşturun ve Azure IoT Central uygulamanıza bağlayın](./tutorial-connect-device.md).
