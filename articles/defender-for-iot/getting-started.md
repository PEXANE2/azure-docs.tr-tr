---
title: Kullanmaya başlama
description: IoT dağıtımı için Defender için temel iş akışını anlama ile çalışmaya başlayın.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/26/2020
ms.author: shhazam
ms.openlocfilehash: ed6c88826b41df0bdfef8cbbcb2569b3cea8f868
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97832395"
---
# <a name="getting-started-with-defender-for-iot"></a>IoT için Defender 'ı kullanmaya başlama

Bu makalede IoT için Azure Defender 'ı kurmak için uygulayabileceğiniz adımlara genel bakış sunulmaktadır. İşlem şunları gerektirir:

- IoT Portal için Azure Defender 'da aboneliğinizi ve Sensörlerinizi kaydedin.
- Algılayıcısı ve şirket içi yönetim konsolu yazılımını yükler.
- Algılayıcı ve yönetim konsolunun ilk etkinleştirmesini gerçekleştirin.

## <a name="permission-requirements"></a>İzin gereksinimleri

Bazı kurulum adımları için belirli kullanıcı izinleri gerekir.

Algılayıcı ve yönetim konsolunu etkinleştirmek, SSL/TLS sertifikalarını yüklemek ve yeni parolalar oluşturmak için Yönetici Kullanıcı izinleri gerekir.

Aşağıdaki tabloda IoT Portal araçları için Azure Defender 'a yönelik kullanıcı erişim izinleri açıklanmaktadır:

| İzin | Güvenlik okuyucusu | Güvenlik yöneticisi | Abonelik Katılımcısı | Abonelik sahibi |
|--|--|--|--|--|
| IoT ekranları ve verileri için tüm Defender 'ı görüntüleme | ✓ | ✓ | ✓ | ✓ |
| Bir algılayıcı ekleme  |  |  ✓ | ✓ | ✓ |
| Fiyatlandırmayı Güncelleştir  |  |  ✓ | ✓ | ✓ |
| Parolayı kurtar  | ✓  |  ✓ | ✓ | ✓ |

## <a name="1-identify-the-solution-infrastructure"></a>1. çözüm altyapısını tanımla

**Ağ Kurulum gereksinimlerinizi açıklığa kavuşturun**

Ağ mimarinizi, izlenen bant genişliğini ve diğer ağ ayrıntılarını araştırın. Daha fazla bilgi için bkz. [IoT Ağ kurulumu Için Azure Defender](how-to-set-up-your-network.md).

**Ağ yükünü işlemek için hangi sensörler ve Yönetim Konsolu gereçlerinin gerektiğini açıklığa kavuşturun**

IoT için Azure Defender hem fiziksel hem de sanal dağıtımları destekler. Fiziksel dağıtımlar için çeşitli Sertifikalı gereçler satın alabilirsiniz. Daha fazla bilgi için bkz. [gerekli gereçlerini tanımla](how-to-identify-required-appliances.md).

İzlenecek yaklaşık cihaz sayısını hesaplamanıza önerilir. Daha sonra, Azure aboneliğinizi portala kaydettiğinizde, bu numarayı girmeniz istenir. Numaralar, 1.000 saniyelik aralıklarla eklenebilir. İzlenen cihazların sayıları, *kaydedilmiş cihazlar* olarak adlandırılır.

## <a name="2-register-with-azure-defender-for-iot"></a>2. IoT için Azure Defender 'a kaydolun

Kayıt şunları içerir:

- IoT için Azure aboneliklerinizi Defender 'a ekleme.
- Kaydedilmiş cihazları tanımlama.
- Şirket içi yönetim konsolu için bir etkinleştirme dosyası indiriliyor.

Kaydolmak için:

1. IoT portalı için Azure Defender ' a gidin.
1. **Yerleşik abonelik**' ı seçin.
1. **Fiyatlandırma** sayfasında bir abonelik seçin ya da yeni bir tane oluşturun ve kaydedilmiş cihazların sayısını ekleyin.
1. **Şirket içi yönetim konsolunu indirin** sekmesini seçin ve indirilen etkinleştirme dosyasını kaydedin. Bu dosya, tanımladığınız toplam kaydedilmiş cihazları içerir. Bu dosya, ilk oturum açma işleminden sonra yönetim konsoluna yüklenir.

## <a name="3-install-and-set-up-the-on-premises-management-console"></a>3. Şirket içi yönetim konsolunu yükleyip kurun

Şirket içi yönetim konsolu gerecinizi aldıktan sonra:

- IoT portalı için Azure Defender 'dan ISO paketini indirin.
- Yazılımı yükler.
- İlk yönetim konsolu kurulumunu etkinleştirin ve gerçekleştirin.

Yüklemek ve ayarlamak için:

1. IoT portalı için **Defender 'dan Başlarken** ' i seçin.
1. **Şirket içi yönetim konsolu** sekmesini seçin.
1. Bir sürüm seçin ve **İndir**' i seçin.
1. Şirket içi yönetim konsolu yazılımını yükler. Daha fazla bilgi için bkz. [IoT yüklemesi Için Defender](how-to-install-software.md).
1. Yönetim konsolunu etkinleştirin ve ayarlayın. Daha fazla bilgi için bkz. Şirket [içi yönetim konsolunuzu etkinleştirme ve ayarlama](how-to-activate-and-set-up-your-on-premises-management-console.md).

## <a name="4-onboard-a-sensor"></a>4. bir algılayıcı ekleme

IoT için Azure Defender ile kaydederek bir algılayıcı ekleme ve bir algılayıcı etkinleştirme dosyası indirme:

1. Bir algılayıcı adı tanımlayın ve bunu bir abonelikle ilişkilendirin.
1. Bir algılayıcı yönetim modu seçin:

   - **Buluta bağlı sensörler**: algılayıcıyı algılayan bilgiler algılayıcı konsolunda görüntülenir. Ayrıca, uyarı bilgileri bir IoT Hub 'ı aracılığıyla dağıtılır ve Azure Sentinel gibi diğer Azure hizmetleriyle de paylaşılabilir.

   - **Yerel olarak yönetilen sensörler**: algılayıcıyı algılayan bilgiler algılayıcı konsolunda görüntülenir. Bir AIR-gapped ağında çalışıyorsanız ve birden çok yerel olarak yönetilen algılayıcı tarafından algılanan tüm bilgilerin birleştirilmiş bir görünümünü istiyorsanız, şirket içi yönetim konsoluyla çalışın. 

1. Bir algılayıcı etkinleştirme dosyası indirin.

Daha fazla bilgi için bkz. [IoT Portal Için Defender 'daki algılayıcı ekleme ve yönetme](how-to-manage-sensors-on-the-cloud.md).

## <a name="5-install-and-set-up-the-sensor"></a>5. algılayıcıyı kurun ve ayarlayın

IoT portalı için Azure Defender 'dan ISO paketini indirin, yazılımı yükleyin ve algılayıcıyı ayarlayın.

1. IoT portalı için **Defender 'dan Başlarken** ' i seçin.
1. **Algılayıcıyı ayarla**' yı seçin.
1. Bir sürüm seçin ve **İndir**' i seçin.
1. Algılayıcı yazılımını yükler. Daha fazla bilgi için bkz. [IoT yüklemesi Için Defender](how-to-install-software.md).
1. Algılayıcıyı etkinleştirin ve ayarlayın. Daha fazla bilgi için bkz. [oturum açma ve algılayıcı etkinleştirme](how-to-activate-and-set-up-your-sensor.md).

## <a name="6-connect-sensors-to-an-on-premises-management-console"></a>6. algılayıcıları şirket içi yönetim konsoluna bağlama

Aşağıdakileri sağlamak için algılayıcılar 'ı yönetim konsoluna bağlayın:

- Sensörler, şirket içi yönetim konsoluna uyarı ve cihaz envanter bilgilerini gönderir.

- Şirket içi yönetim konsolu, algılayıcı yedeklemeleri gerçekleştirebilir, sensör algılayan uyarıları yönetebilir, algılayıcı bağlantılarını araştırabilir ve bağlı sensörlerdeki diğer etkinlikleri gerçekleştirebilirler.

Aynı ağları tek bir bölgede izlemek için birden çok algılayıcı gruplandırmalarını öneririz. Bunun yapılması, birden fazla sensör tarafından toplanan bilgileri içine alınır.

Daha fazla bilgi için bkz. [Şirket içi yönetim konsoluna sensörlerden bağlanma](how-to-activate-and-set-up-your-on-premises-management-console.md#connect-sensors-to-the-on-premises-management-console).

## <a name="7-populate-azure-sentinel-with-alert-information-optional"></a>7. Azure Sentinel 'i uyarı bilgileriyle doldur (isteğe bağlı)

Azure Sentinel 'i yapılandırarak Azure Sentinel 'e uyarı bilgileri gönderin. Bkz. [IoT Için Defender 'daki verilerinizi Azure Sentinel 'e bağlama](how-to-configure-with-sentinel.md).

## <a name="see-also"></a>Ayrıca bkz.

- [IoT için Azure Defender 'a hoş geldiniz](overview.md)

- [IoT mimarisi için Azure Defender](architecture.md)
