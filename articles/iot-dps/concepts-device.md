---
title: Azure aygıt sağlamada aygıt kavramları | Microsoft Dokümanlar
description: Aygıt Sağlama Hizmeti (DPS) ve IoT Hub ile aygıtlara özgü aygıt sağlama kavramlarını açıklar
author: nberdy
ms.author: nberdy
ms.date: 11/06/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: f5f931622f793a1146c04403e8c5e1a5ef7a7d62
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79285168"
---
# <a name="iot-hub-device-provisioning-service-device-concepts"></a>IoT Hub Cihazı Sağlama Hizmeti cihaz kavramları

IoT Hub Aygıt Sağlama Hizmeti, ioT Hub için sıfır dokunmatik aygıt sağlama sağlamasını belirli bir IoT hub'ına yapılandırmak için kullandığınız bir yardımcı hizmettir. Cihaz Sağlama Hizmeti ile güvenli ve ölçeklenebilir bir şekilde milyonlarca cihaz sağlayabilirsiniz.

Bu makalede, aygıt sağlama ile ilgili *aygıt* kavramlarına genel bir bakış sağlar. Bu makale, bir aygıtı dağıtıma hazır hale getirmek için [üretim adımında](about-iot-dps.md#manufacturing-step) yer alan kişilikler için en çok alakalıdır.

## <a name="attestation-mechanism"></a>Attestation mekanizması

Attestation mekanizması, bir aygıtın kimliğini doğrulamak için kullanılan yöntemdir. Attestation mekanizması, belirli bir cihazla hangi attestation yönteminin kullanılacağını sağlayan kayıt listesiyle de ilgilidir.

> [!NOTE]
> IoT Hub, söz bu hizmetteki benzer bir kavram için "kimlik doğrulama şeması" kullanır.

Cihaz Sağlama Hizmeti aşağıdaki attestation formlarını destekler:
* **X.509 sertifikaları standart X.509** sertifika kimlik doğrulama akışını temel alınca.
* **Güvenilir Platform Modülü (TPM),** anahtarların imzalı paylaşılan erişim imzası (SAS) belirteci sunmak için TPM standardını kullanarak bir nonce meydan okumaya dayanır. Bu cihazda fiziksel bir TPM gerektirmez, ancak hizmet [TPM spec](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/)başına onay anahtarı nı kullanarak kanıtlamak için bekliyor.
* Ortak erişim imzası (SAS) [Güvenlik belirteçlerine](../iot-hub/iot-hub-devguide-security.md#security-tokens)dayalı **simetrik anahtar,** bir hashed imza ve gömülü bir son kullanma süresi içerir. Daha fazla bilgi için [Bkz. Simetrik anahtar attestation.](concepts-symmetric-key-attestation.md)

## <a name="hardware-security-module"></a>Donanım güvenlik modülü

Donanım güvenlik modülü veya HSM, aygıt sırlarının güvenli, donanım tabanlı depolanması için kullanılır ve gizli depolamanın en güvenli biçimidir. Hem X.509 sertifikaları hem de SAS belirteçleri HSM'de depolanabilir. HSM'ler, sağlama hizmetinin desteklediği her iki attestation mekanizmasıyla da kullanılabilir.

> [!TIP]
> Aygıtlarınızdaki sırları güvenli bir şekilde depolamak için cihazlarla birlikte bir HSM kullanmanızı şiddetle öneririz.

Aygıt sırları da yazılımda (bellekte) depolanabilir, ancak HSM'den daha az güvenli bir depolama biçimidir.

## <a name="registration-id"></a>Kayıt Kimliği

Kayıt kimliği, Aygıt Sağlama Hizmeti'ndeki bir aygıtı benzersiz olarak tanımlamak için kullanılır. Aygıt kimliği, sağlama hizmeti kimliği [kapsamında](#id-scope)benzersiz olmalıdır. Her aygıtın bir kayıt kimliği olmalıdır. Kayıt kimliği alfasayısal, büyük/küçük harf duyarsız ve kolon, nokta, alt çizgi ve tire gibi özel karakterler içerebilir.

* TPM durumunda, kayıt kimliği TPM kendisi tarafından sağlanır.
* X.509 tabanlı attestation durumunda, kayıt kimliği sertifikanın özne adı olarak verilir.

## <a name="device-id"></a>Cihaz Kimliği

Aygıt kimliği, IoT Hub'da göründüğü gibi kimliktir. İstenilen aygıt kimliği kayıt girişinde ayarlanabilir, ancak ayarlanmayı gerektirmez. İstenilen aygıt kimliğinin ayarlanması yalnızca tek tek kayıtlarda desteklenir. Kayıt listesinde istenilen aygıt kimliği belirtilmemişse, kayıt kimliği aygıtı kaydederken aygıt kimliği olarak kullanılır. [IoT Hub'da aygıt tbm'leri](../iot-hub/iot-hub-devguide-identity-registry.md)hakkında daha fazla bilgi edinin.

## <a name="id-scope"></a>Kimlik kapsamı

Kimlik kapsamı, kullanıcı tarafından oluşturulduğunda bir Aygıt Sağlama Hizmetine atanır ve aygıtın kaydedeceği özel sağlama hizmetini benzersiz olarak tanımlamak için kullanılır. Kimlik kapsamı hizmet tarafından oluşturulur ve benzersizliği garanti eden değişmezdir.

> [!NOTE]
> Benzersizlik, uzun süren dağıtım işlemleri ve birleşme ve satın alma senaryoları için önemlidir.
