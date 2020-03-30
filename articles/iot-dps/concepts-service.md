---
title: Azure IoT Hub Aygıt Sağlama Hizmetinde hizmet kavramları | Microsoft Dokümanlar
description: Aygıt Sağlama Hizmeti (DPS) ve IoT Hub ile aygıtlara özgü hizmet sağlama kavramlarını açıklar
author: nberdy
ms.author: nberdy
ms.date: 09/18/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: f42502ac4db12a060af5906243d3f8e7584c5df3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79285220"
---
# <a name="iot-hub-device-provisioning-service-concepts"></a>IoT Hub Cihaz Sağlama Hizmeti kavramları

IoT Hub Aygıt Sağlama Hizmeti, ioT Hub için sıfır dokunmatik aygıt sağlama sağlamasını belirli bir IoT hub'ına yapılandırmak için kullandığınız bir yardımcı hizmettir. Aygıt Sağlama Hizmeti ile milyonlarca cihazı güvenli ve ölçeklenebilir bir şekilde [otomatik olarak sağlayabilirsiniz.](concepts-auto-provisioning.md)

Aygıt sağlama iki bölümden biridir. İlk bölüm, aygıtı *kaydederek* aygıt ve IoT çözümü arasındaki ilk bağlantıyı kurmaktır. İkinci bölüm, çözümün özel gereksinimlerine göre aygıta uygun *yapılandırmayı* uygulamaktır. Her iki adım tamamlandıktan sonra, aygıt tam *olarak sağlanmıştır.* Cihaz Sağlama Hizmeti, bu adımların ikisini de otomatikleştirerek cihaz için sorunsuz bir sağlama deneyimi sağlar.

Bu makalede, *hizmetin*yönetimi için en geçerli olan sağlama kavramlarına genel bir bakış sunulmaktadır. Bu makale, bir aygıtı dağıtıma hazır hale getirmek için [bulut kurulum adımında](about-iot-dps.md#cloud-setup-step) yer alan kişilikler için en çok alakalıdır.

## <a name="service-operations-endpoint"></a>Servis işlemleri bitiş noktası

Hizmet işlemleri bitiş noktası, hizmet ayarlarını yönetmek ve kayıt listesini korumak için son noktadır. Bu bitiş noktası yalnızca hizmet yöneticisi tarafından kullanılır; cihazlar tarafından kullanılmaz.

## <a name="device-provisioning-endpoint"></a>Aygıt sağlama bitiş noktası

Aygıt sağlama bitiş noktası, tüm aygıtların otomatik sağlama için kullandığı tek uç noktadır. URL, tedarik zinciri senaryolarında yeni bağlantı bilgilerine sahip aygıtları yeniden flashlama gereksinimini ortadan kaldırmak için tüm sağlama hizmeti örnekleri için aynıdır. Kimlik kapsamı kiracı yalıtımı sağlar.

## <a name="linked-iot-hubs"></a>Bağlantılı IoT hub'ları

Aygıt Sağlama Hizmeti yalnızca aygıtları yalnızca ona bağlı olan IoT hub'larına sağlayabilir. Bir IoT hub'ını Aygıt Sağlama hizmetinin bir örneğine bağlamak, hizmete IoT hub'ının aygıt kayıt defterine okuma/yazma izinleri verir; bağlantı yla, Aygıt Sağlama hizmeti bir aygıt kimliği kaydedebilir ve aygıt ikizinde ilk yapılandırmayı ayarlayabilir. Bağlantılı IoT hub'ları herhangi bir Azure bölgesinde olabilir. Diğer abonelikteki hub'ları sağlama hizmetinize bağlayabilirsiniz.

## <a name="allocation-policy"></a>Tahsisat ilkesi

Aygıt Sağlama Hizmeti'nin aygıtları bir IoT hub'ına nasıl atadığını belirleyen hizmet düzeyi ayarı. Desteklenen üç ayırma ilkesi vardır:

* **Eşit ağırlıklı dağıtım**: bağlantılı IoT hub'larının aygıtlara eşit olarak sağlanması olasıdır. Varsayılan ayar. Yalnızca bir IoT hub'a aygıtları sağlıyorsanız bu ayarı değiştirmeyebilirsiniz.

* **En düşük gecikme gecikmesi**: aygıtlar, aygıta en düşük gecikme gecikmesi olan bir IoT hub'ına verilir. Birden çok bağlantılı IoT hub'ı aynı en düşük gecikmeyi sağlayacaksa, sağlama hizmeti aygıtları bu hub'larda

* **Kayıt listesi üzerinden statik yapılandırma**: kayıt listesinde istenen IoT hub belirtimi hizmet düzeyi ayırma ilkesi üzerinde öncelik alır.

## <a name="enrollment"></a>Kayıt

Kayıt, otomatik sağlama yoluyla kaydolabilecek aygıtların veya aygıt gruplarının kaydıdır. Kayıt kaydı, aygıt veya aygıt grubu hakkında aşağıdakiler de dahil olmak üzere bilgiler içerir:
- cihaz tarafından kullanılan [attestation mekanizması](concepts-security.md#attestation-mechanism)
- isteğe bağlı ilk istenilen yapılandırma
- istenilen IoT hub
- istenilen cihaz kimliği

Aygıt Sağlama Hizmeti tarafından desteklenen iki tür kayıt vardır:

### <a name="enrollment-group"></a>Kayıt grubu

Kayıt grubu, belirli bir attestation mekanizmasını paylaşan bir aygıt grubudur. Kayıt grupları hem X.509'u hem de simetrik desteği destekler. X.509 kayıt grubundaki tüm aygıtlar, aynı kök veya ara Sertifika Yetkilisi (CA) tarafından imzalanmış X.509 sertifikaları sunar. Simetrik anahtar kayıt grubundaki her aygıt, grup simetrik anahtarından türetilen SAS belirteçlerini sunar. Kayıt grubu adı ve sertifika adı alfasayısal, küçük ve tire içerebilir.

> [!TIP]
> İstenilen ilk yapılandırmayı paylaşan çok sayıda aygıt veya aynı kiracıya giden aygıtlar için bir kayıt grubu kullanmanızı öneririz.

### <a name="individual-enrollment"></a>Bireysel kayıt

Tek bir kayıt, kaydolabilecek tek bir aygıtın girişidir. Tek tek kayıtlar, attestation mekanizmaları olarak X.509 yaprak sertifikaları veya SAS belirteçleri (fiziksel veya sanal TPM'den) kullanabilir. Tek bir kayıttaki kayıt kimliği alfasayısal, küçük ve tire içerebilir. Bireysel kayıtlar için istenen IoT hub cihazı kimliği belirtilmiş olabilir.

> [!TIP]
> Benzersiz ilk yapılandırmalar gerektiren aygıtlar için veya yalnızca TPM attestation aracılığıyla SAS belirteçlerini kullanarak kimlik doğrulaması yapılabilen aygıtlar için tek tek kayıtları kullanmanızı öneririz.

## <a name="registration"></a>Kayıt

Kayıt, Aygıt Sağlama Hizmeti aracılığıyla bir IoT Hub'ına başarıyla kaydolan/sağlayan bir aygıtın kaydıdır. Kayıt kayıtları otomatik olarak oluşturulur; bunlar silinebilir, ancak güncelleştirilemez.

## <a name="operations"></a>İşlemler

İşlemler, Aygıt Sağlama Hizmetinin faturalandırma birimidir. Bir işlem, hizmete yönelik bir talimatın başarıyla tamamlanmasıdır. İşlemler arasında cihaz kayıtları ve yeniden kayıtlar; işlemler, kayıt listesi girişleri ekleme ve kayıt listesi girişlerini güncelleştirme gibi hizmet tarafındaki değişiklikleri de içerir.
