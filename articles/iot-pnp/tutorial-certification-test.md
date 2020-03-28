---
title: IoT Tak ve Çalıştır Önizleme aygıtınızı onaylayın | Microsoft Dokümanlar
description: Bu öğreticide, ürün bilgilerinizi Azure Sertifikalı IoT aygıt kataloğuna nasıl ekleyeceğiniz, cihazınızı Azure IoT sertifika hizmetine nasıl bağladığınızı ve ardından IoT Tak ve Çalıştır sertifika testlerini nasıl çalıştırılacınız açıklanmaktadır.
manager: philmea
ms.service: iot-pnp
services: iot-pnp
ms.topic: tutorial
ms.author: koichih
author: konichi3
ms.date: 12/27/2019
ms.openlocfilehash: dc2c33659f3f3a3df0f11fcc6ab36a9fc993da43
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75550171"
---
# <a name="tutorial-certify-your-iot-plug-and-play-preview-device"></a>Öğretici: IoT Tak ve Çalıştır Önizleme aygıtınızı sertifikası

[IoT aygıt için Azure Sertifikalı aygıt kataloğunda](https://aka.ms/iotdevcat)bir IoT Tak ve Çalıştır Önizleme aygıtı yayınlamak için bir dizi sertifika testini geçmesi gerekir. Cihazınızı sertifikaya göndermek [için IoT için Azure Sertifikalı](https://aka.ms/ACFI) portalını kullanın. [Azure IoT sertifika hizmeti](https://aka.ms/azure-iot-aics) sertifika testlerini çalıştırZ.

Bu öğreticide şunları öğreniyorsunuz:

> [!div class="checklist"]
> * IoT Tak ve Çalıştır sertifika gereksinimleri nelerdir.
> * Ürün adınızı ve bilgilerinizi portala ekleme.
> * IoT Tak ve Çalıştır arayüzlerini nasıl bağlayıp keşfedebilirim.
> * IoT Tak ve Çalıştır arayüzlerini gözden geçirme ve sertifika testlerini çalıştırma.
> * Sertifikalı IoT Tak ve Çalıştır cihazını katalogda nasıl yayınlarım?

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Microsoft İş Ortağı Merkezi hesabı.
* Microsoft İş Ortağı Ağı Kimliği.

Daha fazla bilgi için, [IoT için Azure Sertifikalı portalına nasıl biningerektiğini](howto-onboard-portal.md) öğrenin.

## <a name="certification-requirements"></a>Sertifikasyon gereksinimleri

IoT Tak ve Çalıştır cihazınızı onaylamak için cihazınızın aşağıdaki gereksinimleri karşılaması gerekir:

* IoT Tak ve Çalıştır aygıt kodunuz cihazınıza yüklenmiş olmalıdır.
* IoT Tak ve Çalıştır aygıt kodunuz Azure IoT SDK ile oluşturulmuştür.
* Cihaz kodunuz [Azure IoT Hub Aygıt Sağlama Hizmetini](../iot-dps/about-iot-dps.md)desteklemelidir.
* Aygıt kodunuz [Aygıt Bilgi Arabirimini](concepts-common-interfaces.md)uygulamalıdır.
* Yetenek modeli ve aygıt kodu IoT Central ile çalışır.

Katalogda bulunan tüm aygıtlar önceden sertifikalı IoT Tak ve Çalıştır aygıtları olarak kabul edilir. SDK ve Dijital İkiz Tanımlı Dil gibi IoT Tak ve Çalıştır yazılım bileşenlerinin nihai ürününün kalitesini ve uyumluluğunu garanti etmez.

Önceden onaylanmış Tüm IoT Tak ve Çalıştır cihazları, Microsoft tarafından sağlanan sertifika gereksinimleri ve yazılım bileşenlerinin son sürümüne bağlı olarak IoT Tak ve Çalıştır'ın genel kullanılabilirliğine bağlı olarak yeniden düzenlemelidir.

## <a name="add-product-name"></a>Ürün adı ekleme

**Ürün,** müşterinin satın alabileceği bir ürün modeli için uygun bir addır. Ürün eklemek için:

1. [IoT için Azure Sertifikalı](https://aka.ms/ACFI) portalında oturum açın.
1. Sol menüden portaldaki **Ürünler** sayfasına gidin ve **+ Yeni'yi**seçin.
1. Güler yüzlü ürün adınızı girin ve **Oluştur'u**seçin. Buraya girilen ad, aygıt kataloğunda görüntülenen addan farklıdır.

## <a name="add-product-information"></a>Ürün bilgileri ekleme

Ürünü portalda başarıyla oluşturduktan sonra ürün **Ürünler** sayfasında görüntülenir. Ürün bilgilerini eklemek için:

1. Ürün sütunundaki **Ürün** sayfasında bulunan oluşturulan ürün bağlantısını seçin. Devlet taslak durumda olmalıdır.
1. **Ürün bilgileri** başlığının yanındaki **Edit** bağlantısını seçin. Ürün bilgileri sayfasına ürününizle ilgili bilgileri girin ve gerekli tüm alanları tamamladığınızdan emin olun.
1. **Kaydet'i**seçin. **Kaydet** düğmesi yalnızca gerekli tüm alanları tamamladığınızda görüntülenir. Alanlar eksikse, düğme **kaydet ve daha sonra bitir**diyor.
1. Girdiğiniz içeriği inceleyin. Aygıtı aygıt kataloğunda yayımlamak için gerekli tüm alanları tamamlayın. **Ürün** bilgileri başlığının yanındaki **düzenleme** bağlantısını tıklayarak ürün ayrıntı sayfasındaki ürün bilgilerinde düzenleme yapmak için her zaman geri dönebilirsiniz.

## <a name="connect-and-discover-interfaces"></a>Arayüzleri bağlayın ve keşfedin

Sertifika testlerini çalıştırmak için cihazınızı portalda bulunan [Azure IoT sertifika hizmetine](https://aka.ms/azure-iot-aics) (AICS) bağlayın.

Bu adımlar, sertifika sınamalarını çalıştırmak için bir adımdır ve ürün aygıt kodunuzu değiştirmeniz gerekmez. Başlamak için, AICS'ye bağlanmak için aşağıdaki adımları izleyin:

1. Ortak Merkezi hesabınızı kullanarak portalda oturum açın.
1. Sertifika akışını başlatmak için **Bağlan + testi'ni** tıklatın.
1. [Azure IoT Hub Aygıt Sağlama Hizmeti'ni](../iot-dps/about-iot-dps.md)kullanarak cihazınızı AICS'ye sağlamak için [kimlik doğrulama yöntemini](../iot-dps/concepts-security.md#attestation-mechanism) seçin.
   * [X.509 sertifikası](../iot-hub/iot-hub-security-x509-get-started.md#prerequisites)kullanıyorsanız, oluşturduğunuz X.509 sertifikasını yükleyin. X.509 sertifikalarının nasıl kullanılacağını gösteren örnek kodu gözden geçirmek isteyebilirsiniz: [C](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/samples/iothub_ll_client_x509_sample/iothub_ll_client_x509_sample.c), [C#](../iot-hub/iot-hub-security-x509-get-started.md).
   * [Simetrik](../iot-dps/concepts-symmetric-key-attestation.md)bir anahtar kullanıyorsanız, simetrik anahtarı aygıt kodunuza kopyalayın ve yapıştırın.
   * TPM kimlik doğrulama yöntemi şu anda desteklenmez.
1. Aşağıdaki oluşturulan kodları aygıt kodunuza kopyalayıp yapıştırın.
   * [Kayıt Kimliği](../iot-dps/use-hsm-with-sdk.md)
   * [DPS Kimliği](../iot-dps/tutorial-set-up-device.md#create-the-device-registration-software)
   * [DPS bitiş noktası](../iot-dps/tutorial-set-up-device.md#create-the-device-registration-software)
1. Aygıt kodunuz oluşturulup aygıta dağıtıldığında, IoT Tak ve Çalıştır arabirimlerini keşfetmek için **Bağlan'ı** seçin.
1. AICS bağlantısı başarılı olduğunda, IoT Tak ve Çalıştır arabirimlerini gözden geçirmek için **İleri'yi** seçin.

## <a name="run-tests-and-publish-the-device"></a>Testleri çalıştırın ve aygıtı yayımlayın

İnceleme sayfasında, keşfedilen IoT Tak ve Çalıştır arayüzlerini inceleyebilirsiniz. Arabirim ekranında uygulanan ilkel leri doğru şekilde denetlemek için bu sayfayı kullanın. Arabirimin konumunu da kontrol edebilirsiniz.

1. Gerekli alanlar için yük girişlerinin girilen olduğundan emin olun. Bu alanlar, belirtilen arabirim için ilkel komutiçin yük bilgilerini içerir.
1. Gerekli tüm bilgileri girdikten sonra **İleri'yi**seçin.
1. Uygulanan IoT Tak ve Çalıştır arabirimleri için testleri çalıştırmak için **Testleri Çalıştır'ı**seçin.
1. Tüm testler otomatik olarak çalışır. Herhangi bir test başarısız olursa, AICS'den gelen hata iletilerini ve Azure IoT Hub'ına gönderilen ham telemetriyi görüntülemek için **günlükleri** görüntüle'yi seçin.
1. Sertifika testlerini tamamlamak için **Finish'i**seçin.
1. Sertifikalı IoT Tak ve Çalıştır cihazını katalogda yayınlayın. Sertifikalı aygıtı kataloğa eklemek için, araç çubuğundaki **kataloğa Ekle'** yi seçin. Ekle **kataloğu** soluksa, ürün bilgilerinin eksik olduğu veya testlerin başarısız olduğu anlamına gelir. 
1. Yayınlanan cihazınızı cihaz kataloğunda görüntülemek için "ONAYLI VE KATALOGDA" bağlantısını seçin.

## <a name="next-steps"></a>Sonraki adımlar

IoT Tak ve Çalıştır cihazını sertifikalendirmeyi öğrendiğiniz için önerilen bir sonraki adım, yetenek modellerini yönetme hakkında daha fazla bilgi edinmektir:

> [!div class="nextstepaction"]
> [Modelleri yönetme](./howto-manage-models.md)
