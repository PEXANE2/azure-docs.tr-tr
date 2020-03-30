---
title: Azure IoT Hub Cihazı Sağlama Hizmeti’ne Genel Bakış | Microsoft Docs
description: Aygıt Sağlama Hizmeti (DPS) ve IoT Hub ile Azure'da aygıt sağlamayı açıklar
author: wesmc7777
ms.author: wesmc
ms.date: 04/04/2019
ms.topic: overview
ms.service: iot-dps
services: iot-dps
manager: eliotgra
ms.openlocfilehash: e564660b502a950021ba4e4754ff5c210dfd477f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241181"
---
# <a name="provisioning-devices-with-azure-iot-hub-device-provisioning-service"></a>Azure IoT Hub Cihazı Sağlama Hizmeti ile cihaz sağlama
Microsoft Azure, tüm IoT çözüm ihtiyaçlarınız için zengin bir tümleşik genel bulut hizmetleri kümesi sağlar. IoT Hub Aygıt Sağlama Hizmeti (DPS), IoT Hub için insan müdahalesi gerektirmeden doğru IoT hub'ına sıfır dokunuş, tam zamanında sağlama sağlayan bir yardımcı hizmettir. DPS, milyonlarca aygıtın güvenli ve ölçeklenebilir bir şekilde sağlanmasına olanak tanır.

## <a name="when-to-use-device-provisioning-service"></a>Cihaz Sağlama Hizmeti’ni kullanma zamanı
DPS'nin aygıtları bağlı hale getirmek ve IoT Hub'a yapılandırmak için mükemmel bir seçim olduğu birçok sağlama senaryosu vardır:

* IoT Hub bağlantı bilgilerini fabrikada sabit kodlamaya gerek kalmadan tek bir IoT çözümüne dokunma olmadan sağlama (ilk kurulum)
* Birden çok hub'da yük dengeleme cihazları
* Cihazları satış işlemi verilerine göre cihaz sahibinin IoT çözümüne bağlama (çoklu kiracı)
* Cihazları kullanım durumuna göre belirli bir IoT çözümüne bağlama (çözüm yalıtımı)
* Bir cihazı en düşük gecikme süresine sahip IoT hub’a bağlama (coğrafi parçalama)
* Cihazdaki bir değişikliğe göre yeniden sağlama
* Cihaz tarafından IoT Hub’a bağlanmak için kullanılan anahtarları değiştirme (bağlanmak için X.509 sertifikaları kullanılmadığında)

## <a name="behind-the-scenes"></a>Arka planda
Önceki bölümde listelenen tüm senaryolar aynı akış ile sıfır dokunmatik sağlama için DPS kullanılarak yapılabilir. Geleneksel olarak tedarik le ilgili el ile atılan adımların çoğu, IoT aygıtlarını dağıtma süresini azaltmak ve el ile hata riskini azaltmak için DPS ile otomatikleştirilmiştir. Aşağıdaki bölümde bir cihazın sağlanması için arka planda gerçekleştirilen işlemler açıklanmaktadır. İlk adım el ile, sonraki adımların tümü ise otomatik olarak gerçekleştirilmektedir.

![Temel sağlama akışı](./media/about-iot-dps/dps-provisioning-flow.png)

1. Cihaz üreticisi, cihaz kayıt bilgilerini Azure portalındaki kayıt listesine ekler.
2. Aygıt, fabrikada dps uç noktası kümesine temas eder. Aygıt, kimliğini kanıtlamak için tanımlayıcı bilgileri DPS'ye aktarıyor.
3. DPS, kayıt kimliği ve anahtarını nonce mücadelesi[(Güvenilir Platform Modülü)](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/)veya standart X.509 doğrulamasını (X.509) kullanarak kayıt listesi girişine karşı doğrulayarak cihazın kimliğini doğrular.
4. DPS aygıtı bir IoT hub'ına kaydeder ve aygıtın [istenen ikiz durumunu](../iot-hub/iot-hub-devguide-device-twins.md)doldurur.
5. IoT hub'ı aygıt kimliği bilgilerini DPS'ye döndürür.
6. DPS, IoT hub bağlantı bilgilerini aygıta döndürür. Cihaz artık doğrudan IoT hub’a veri göndermeye başlayabilir.
7. Cihaz IoT hub’a bağlanır.
8. Cihaz IoT hub’daki cihaz ikizinden istenen durumu alır.

## <a name="provisioning-process"></a>Sağlama işlemi
DPS'nin bağımsız olarak yapılabilecek bir rol aldığı bir aygıtın dağıtım işleminde iki farklı adım vardır:

* Fabrikada cihazın oluşturulduğu ve hazırlandığı **üretim adımı** ve
* Cihaz Sağlama Hizmeti’nin otomatik sağlama için yapılandırıldığı **bulut kurulumu adımı**.

Bu adımların ikisi de mevcut üretim ve dağıtım işlemlerine sorunsuz olarak eklenebilir. DPS, aygıta bağlantı bilgilerini almak için manuel çalışma içeren bazı dağıtım işlemlerini bile basitleştirir.

### <a name="manufacturing-step"></a>Üretim adımı
Bu adım, üretim hattında gerçekleştirilen işlemleri kapsar. Bu adımda bulunan roller silikon tasarımcısı, silikon üreticisi, tümleştirici ve/veya cihazın son üreticisidir. Bu adım, donanımı oluşturmayla ilgilidir.

DPS üretim sürecinde yeni bir adım getirmez; bunun yerine, ilk yazılımı ve (ideal olarak) hsm'yi aygıta yükleyen varolan adıma bağlar. Bu adımda cihaz kimliği oluşturma yerine cihaz, sağlama hizmeti bilgileriyle programlanarak açıldığında bağlantı bilgilerini/IoT çözümü atamasını almak üzere sağlama hizmetini araması sağlanır.

Ayrıca bu adımda üretici cihaz dağıtıcısına/operatöre kimlik anahtarı bilgilerini iletir. Bu bilgilerin sağlanması, tüm cihazların cihaz dağıtıcısı/operatörü tarafından sağlanan imzalama sertifikası ile oluşturulmuş olan bir X.509 sertifikasına sahip olduğunun onaylanması kadar basit veya her bir TPM cihazından TPM onay anahtarının genel bölümünün ayıklanması kadar karmaşık olabilir. Bu hizmetler günümüzde birçok silikon üreticisi tarafından sunulmaktadır.

### <a name="cloud-setup-step"></a>Bulut kurulumu adımı
Bu adımda bulut, uygun otomatik sağlama için yapılandırılır. Bulut kurulumu adımında genellikle iki tür kullanıcı vardır. Cihazların ilk kurulumunun nasıl olması gerektiğini bilen bir kullanıcı (cihaz operatörü) ve cihazların IoT hub’larına nasıl dağıtılması gerektiğini bilen bir kullanıcı (çözüm operatörü).

Sağlama süresinde genellikle çözüm operatörü tarafından gerçekleştirilen tek seferlik bir kurulum adımı vardır. Sağlama hizmeti yapılandırıldıktan sonra kullanım örneği değiştirilmediği sürece değiştirilmesi gerekmez.

Hizmet otomatik sağlama için yapılandırıldıktan sonra cihaz kaydına hazır duruma getirilmesi gerekir. Bu adım, cihazların istenen yapılandırmasını bilen ve IoT hub’ını arayan cihazın kimliğinin sağlama hizmeti tarafından uygun şekilde kanıtlayacağından emin olma konusunda yetkili olan cihaz operatörü tarafından gerçekleştirilir. Cihaz operatörü tanımlama anahtarını üreticiden alır ve kayıt listesine ekler. Yeni girişlerin eklenmesi veya var olan girişlerin cihazlar hakkındaki en son bilgilerle güncelleştirmesi nedeniyle kayıt listesinde daha sonradan güncelleştirme yapılabilir.

## <a name="registration-and-provisioning"></a>Kayıt ve sağlama
*Sağlama*, terimin kullanıldığı sektöre bağlı olarak farklı anlamlara gelebilir. IoT cihazlarını bulut çözümlerine sağlama bağlamında sağlama işlemi iki bölümden oluşan bir işlemdir:

1. İlk bölüm, cihazı kaydederek cihaz ile IoT çözümü arasındaki ilk bağlantıyı kurmaktır.
2. İkinci bölüm ise kaydedildiği çözümün gereksinimlerine bağlı olarak cihaz uygun yapılandırmanın uygulanmasıdır.

Bu adımların ikisi de tamamlandıktan sonra cihazın tam olarak sağlandığını söylemek mümkündür. Bazı bulut hizmetleri cihazları IoT çözümü uç noktasına kaydedip ilk yapılandırmayı gerçekleştirmeyerek sağlama işleminin yalnızca ilk bölümünü tamamlar. DPS, aygıt için sorunsuz bir sağlama deneyimi sağlamak için her iki adımı da otomatikleştirir.

## <a name="features-of-the-device-provisioning-service"></a>Cihaz Sağlama Hizmeti’nin özellikleri
DPS birçok özelliğe sahiptir, bu da onu aygıt sağlama için ideal hale getirir.

* **Güvenli kanıtlama**: Hem X.509 hem de TPM tabanlı kimlikler için destek.
* **Kayıt listesi**: Bir noktada kaydedilebilecek cihazların/cihaz gruplarının tam kaydını içerir. Kayıt listesi, kaydedilen cihazın istenen yapılandırması hakkında bilgi içerir ve istenen zamanda güncelleştirilebilir.
* DPS'nin senaryolarınızı desteklemek üzere aygıtları IoT hub'larına nasıl atadığını denetlemek için **birden çok ayırma ilkeleri:** En düşük gecikme, eşit ağırlıklı dağıtım (varsayılan) ve kayıt listesi üzerinden statik yapılandırma. Gecikme, [Trafik Yöneticisi](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods#performance)ile aynı yöntem kullanılarak belirlenir.
* **İzleme ve tanılama günlüğü**: Her şeyin düzgün çalıştığından emin olmanızı sağlar.
* **Çok hub'lı destek,** DPS'nin aygıtları birden fazla IoT hub'ına atamasına olanak tanır. DPS, birden çok Azure aboneliğinde hub'larla konuşabilir.
* **Bölgeler arası destek,** DPS'nin aygıtları diğer bölgelerdeki IoT hub'larına atamasına olanak tanır.
* **Geri kalan veriler için şifreleme** DPS'deki verilerin, mevcut en güçlü blok şifrelemelerinden biri olan 256 bit AES şifrelemesi kullanılarak şifrelenmesini ve şifrelerinin şifresini çözmesini sağlar ve FIPS 140-2 uyumludur.


[Cihaz kavramları](concepts-device.md), [hizmet kavramları](concepts-service.md) ve [güvenlik kavramları](concepts-security.md) bölümlerinde cihaz sağlamayla ilgili kavramlar ve özellikler hakkında daha fazla bilgi edinebilirsiniz.

## <a name="cross-platform-support"></a>Platformlar arası destek
Tüm Azure IoT hizmetleri gibi DPS de çeşitli işletim sistemleriyle çapraz platform da çalışır. Azure, cihazların bağlanmasını ve hizmetin yönetilmesini kolaylaştırmak amacıyla birçok [dilde](https://github.com/Azure/azure-iot-sdks) açık kaynak SDK’ları sunar. DPS, aygıtları bağlamak için aşağıdaki protokolleri destekler:

* HTTPS
* AMQP
* Web yuvaları üzerinden AMQP
* MQTT
* Web yuvaları üzerinden MQTT

DPS yalnızca hizmet işlemleri için HTTPS bağlantılarını destekler.

## <a name="regions"></a>Bölgeler
DPS birçok bölgede kullanılabilir. Tüm hizmetler için mevcut ve yeni duyurulan bölgelerin güncel listesi [Azure Bölgeleri](https://azure.microsoft.com/regions/) sayfasında yer almaktadır. Cihaz Sağlama Hizmeti’nin kullanılabilirliğini [Azure Durumu](https://azure.microsoft.com/status/) sayfasından kontrol edebilirsiniz.

> [!NOTE]
> DPS geneldir ve bir konuma bağlı değildir. Ancak, DPS profilinizle ilişkili meta verilerin bulunduğu bir bölge belirtmeniz gerekir.

## <a name="availability"></a>Kullanılabilirlik
DPS için %99,9 Hizmet Düzeyi Sözleşmesi vardır ve [SLA'yı okuyabilirsiniz.](https://azure.microsoft.com/support/legal/sla/iot-hub/) [Azure SLA](https://azure.microsoft.com/support/legal/sla/) şartları, Azure’un tamamının kullanılabilirlik garantisini açıklamaktadır.

## <a name="quotas"></a>Kotalar
Her Azure aboneliği varsayılan kota sınırları içerir ve bu sınırlar, IoT çözümünüzün kapsamını etkileyebilir. Abonelik başına geçerli sınır, 10 Cihaz Sağlama Hizmeti/abonelik şeklindedir.

[!INCLUDE [azure-iotdps-limits](../../includes/iot-dps-limits.md)]

Kota sınırları hakkındaki diğer ayrıntılar için:
* [Azure Aboneliği Hizmet Sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md)

## <a name="related-azure-components"></a>İlgili Azure bileşenleri
DPS, Azure IoT Hub ile aygıt sağlamayı otomatikleştirir. [IoT Hub](https://docs.microsoft.com/azure/iot-hub/) hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar
Artık Azure’da IoT cihazlarını sağlama hakkında genel bilgilere sahipsiniz. Bir sonraki adımda uçtan uca bir IoT senaryosunu tamamlamayı deneyeceksiniz.
> [!div class="nextstepaction"]
> [Azure portalı](quick-setup-auto-provision.md)
> Oluşturma ile IoT Hub Aygıt Sağlama Hizmeti'ni ayarlama[ve simüle edilmiş bir aygıt](quick-create-simulated-device.md)
> [sağlama için cihaz ayarlama](tutorial-set-up-device.md)
