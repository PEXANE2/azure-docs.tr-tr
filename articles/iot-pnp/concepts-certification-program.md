---
title: Azure Sertifikalı cihaz programı | Microsoft Docs
description: Azure Sertifikalı cihaz programını anlayın.
author: koichih
ms.author: koichih
ms.date: 09/25/2020
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 1fcfc7a9e632e5db1fb809dba7a938c8641c9ddc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92048109"
---
# <a name="what-is-the-azure-certified-device-program"></a>Azure Sertifikalı cihaz programı nedir?

Azure Sertifikalı cihaz programı, müşteri çözümlerinin Azure hizmetlerinizle sorunsuz bir şekilde çalışmasını sağlar. Program, endüstri bilgilerini ve en iyi uygulamaları bir cihaz ve çözüm oluşturucuları topluluğuyla paylaşmak için araçlar, hizmetler ve bir market kullanır.

Bu program şu şekilde tasarlanmıştır:

- **Müşterilere güven verme:** Müşteriler, Azure hizmetleri ile kullanılmak üzere Microsoft tarafından sertifikalı cihazları güvenle satın alabilir.

- **Müşterilerinin çözümüne yönelik doğru cihazları bulmasına yardımcı olun:** Cihaz oluşturucular, sertifika sürecinin bir parçası olarak cihazlarının benzersiz yeteneklerini yayımlayabilir. Müşteriler, ihtiyaçlarına uygun ürünleri kolayca bulabilir.

- **Sertifikalı cihazları yükselt:** Cihaz oluşturucular, daha fazla görünürlük, müşterilerle iletişim kurma ve Azure Sertifikalı cihaz markasının kullanımıyla ilgili bir görünüm alır.

Bu makalede nasıl yapılacağı açıklanır:

- Şirketinizi Azure Sertifikalı cihaz programına ekleyin.
- Cihazınız için geçerli olan sertifikayı belirleme.
- Teste başlamanızı sağlamak için program gereksinimlerini ve diğer kaynakları bulun.
- Cihazınızı doğrulamak için Azure Sertifikalı cihaz portalını kullanın.

## <a name="onboarding"></a>Ekleme

[Azure Sertifikalı cihaz portalında](https://aka.ms/acdp)cihazlarınızı onaylamak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Şirketinizin iş veya okul kiracısı kullanarak Azure Active Directory bir hesabı olduğundan emin olun.
2. Hesabınızı kullanarak [Microsoft iş ortağı ağı (MPN)](https://partner.microsoft.com/) öğesine katın.
3. MPN 'ye katıldıktan sonra [Azure Sertifikalı cihaz portalında](https://aka.ms/acdp) oturum açın.
4. Şirket profili sayfasında [Program sözleşmesini](https://aka.ms/acdagreement) gözden geçirin ve imzalayın

### <a name="company-profile"></a>Şirket profili

Azure Sertifikalı cihaz portalında şirketinizin profilini yönetmek için **Şirket profili**' ni seçin. Şirket profili, şirket URL 'sini, e-posta adresini ve logoyu içerir. Herhangi bir sertifika işlemine devam etmeden önce bu sayfadaki program anlaşmasını kabul edin.

Azure Sertifikalı cihaz kataloğunda cihaz açıklaması sayfası, şirket profili bilgilerini kullanır.

## <a name="choose-the-certification"></a>Sertifikayı seçin

Her biri farklı bir müşteri değeri sunmaya odaklanan üç farklı sertifika vardır. Oluşturmakta olduğunuz cihazın türüne ve hedef hedef kitlenize bağlı olarak, en uygun sertifika veya sertifikaları seçebilirsiniz:

### <a name="azure-certified-device"></a>Azure Sertifikalı cihaz

_Azure Sertifikalı cihaz sertifikası_ , bir cihazın Azure IoT Hub bağlanıp bağlanamadığını doğrular ve cihaz sağlama HIZMETI (DPS) aracılığıyla güvenli bir şekilde temin edebilir. Bu sertifika, daha gelişmiş sertifikalar için gerekli bir taban çizgisi olan bir cihazın işlevselliğini ve birlikte çalışabilirliğini yansıtır.

- Daha fazla bilgi edinmek için bkz. [sertifika gereksinimleri](https://aka.ms/acdrequirements).
- Cihazınızı Azure IoT Hub 'e bağlamak için DPS kullanma hakkında daha fazla bilgi edinmek için bkz. [cihazlara genel bakış](../iot-dps/about-iot-dps.md).

### <a name="iot-plug-and-play"></a>IoT Tak Çalıştır

Azure Sertifikalı cihaz sertifikasında artımlı bir sertifika olan _ıot Tak ve kullan sertifikası_, özel cihaz kodu olmadan cihaz oluşturma sürecini basitleştirir. IoT Tak ve Kullan, donanım iş ortaklarının Azure IoT Central ve üçüncü taraf çözümlerine göre bulut çözümleriyle kolayca tümleştirilen cihazlar oluşturmasına olanak sağlar.

- Daha fazla bilgi edinmek için bkz. [sertifika gereksinimleri](https://aka.ms/acdiotpnprequirements).
- IoT Tak ve Kullan test için bir cihaz hazırlama hakkında daha fazla bilgi edinmek için bkz. [ıot Tak ve kullan cihazlarını onaylama](howto-certify-device.md).

### <a name="edge-managed"></a>Sınır yönetiliyor

Azure Sertifikalı cihaz sertifikalarında artımlı bir sertifika olan _Edge tarafından yönetilen sertifika_, Windows, Linux veya RTOS çalıştıran Azure IoT cihazları için cihaz yönetim standartlarına odaklanır. Şu anda, bu program sertifikası modül dağıtımı ve yönetimi için Edge çalışma zamanı uyumluluğuna odaklanır.

> [!TIP]
> Bu program daha önce _IoT Edge sertifika programı_ olarak biliniyordu.

- Daha fazla bilgi edinmek için bkz. [sertifika gereksinimleri](https://aka.ms/acdedgemanagedrequirements).
- IoT Edge hakkında daha fazla bilgi edinmek için bkz. [IoT Edge genel bakış](../iot-edge/about-iot-edge.md).
- Desteklenen işletim sistemleri ve kapsayıcılar hakkında daha fazla bilgi için bkz. [IoT Edge desteklenen sistemler](../iot-edge/support.md).

## <a name="use-the-azure-certified-device-portal"></a>Azure Sertifikalı cihaz portalını kullanma

Bu bölümde, [Azure Sertifikalı cihaz portalının](https://certify.azure.com)nasıl kullanılacağı özetlenmektedir. Daha fazla bilgi edinmek için bkz. [Portal ile çalışmaya başlama kılavuzu](https://aka.ms/acdhelp).

Azure Sertifikalı cihaz programında bir cihazı onaylamak için aşağıdaki dört adımı izleyin:

1. Cihaz ayrıntılarını sağlama
2. Cihazı test etme
3. Gözden geçirmeyi gönderme ve tamamlanma
4. Azure Sertifikalı cihaz kataloğuna yayımlama (isteğe bağlı)

### <a name="provide-device-details"></a>Cihaz ayrıntılarını sağlama

Onaylamak istediğiniz her cihaz için, cihaz donanımı, kurulum yönergeleri ve pazarlama malzemesiyle ilgili ayrıntıları kaydetmek üzere sertifika portalındaki formları kullanın:

- **Cihaz bilgileri:** Cihaz hakkındaki adı, açıklaması, donanım ayrıntıları ve işletim sistemi gibi bilgileri toplar.
- **Kullanmaya başlama kılavuzu**: bir müşterinin ürününüzü hızlı bir şekilde kullanmak için KULLANABILECEĞI bir PDF belgesi. [Örnek şablonlar](https://aka.ms/GSTemplate) kullanılabilir.
- **Pazarlama ayrıntıları:** Cihazınız için bir resim ve dağıtıcı bilgileri gibi müşterilere yönelik pazarlama bilgilerini sağlayın.
- **Ek sektör sertifikaları:** Cihazın sahip olduğu diğer sertifikalar hakkında bilgi sağlamanıza olanak sağlayan isteğe bağlı bir bölüm.

### <a name="test-the-device"></a>Cihazı test etme

Bu aşama, cihazlarınız ile etkileşime girer ve cihaz IoT Hub bağlanmak için DPS kullandığında bir dizi test çalıştırır. Tamamlandığında, bir günlük dosyaları kümesini cihaz test sonuçlarınıza göre görüntüleyebilirsiniz.

Sertifika Portalı, test için kullanılan IoT Hub örneğine nasıl bağlanacağınız hakkında yönergeler içerir. Her türlü [desteklenen kanıtlama yöntemlerinden](../iot-dps/concepts-service.md#attestation-mechanism)herhangi bırını kullanarak DPS bağlantısı kurabilirsiniz.

Azure Sertifikalı cihaz ekibi, cihazın el ile doğrulanması için cihaz Oluşturucu ile iletişim kurabilme.

### <a name="submit-and-publish"></a>Gönderme ve yayımlama

Son gerekli aşama, projeyi gözden geçirilmek üzere göndermaktır. Bu adım, bir Azure Sertifikalı cihaz ekibi üyesine cihaz ve pazarlama ayrıntıları ve Başlarken Kılavuzu dahil olmak üzere projeyi daha ayrıntılı bir şekilde gözden geçirmesine bildirir. Bir ekip üyesi, daha önce sorularla birlikte sunulan şirket e-posta adresinde sizinle veya onay öncesinde istekleri düzenlerken sizinle iletişim sağlayabilir.

Cihazınızın sertifika kapsamında daha el ile doğrulama gerektiriyorsa, şu anda bir bildirim alırsınız.

Bir cihaz sertifikalandırılması durumunda ürün Özeti sayfasındaki **kataloğa Yayımla** özelliğini kullanarak ürün ayrıntılarınızı Azure Sertifikalı cihaz kataloğunda yayımlamayı tercih edebilirsiniz.

## <a name="if-you-have-questions"></a>Sorularınız varsa

[iotcert@microsoft.com](mailto:iotcert@microsoft.com?subject=Azure%20Certified%20Device%20question)Sertifika programları, sertifika portalı veya Azure Sertifikalı cihaz Kataloğu hakkında sorularınız varsa ekibine başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Azure Sertifikalı cihaz programına bir genel bakışa sahip olduğunuza göre, [ıot Tak ve kullan cihazlarını nasıl sertifikalacağınızı](howto-certify-device.md)öğrenmek için önerilen bir sonraki adım aşağıda sunulmuştur.