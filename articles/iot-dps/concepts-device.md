---
title: Azure cihaz sağlama 'da cihaz kavramları | Microsoft Docs
description: Cihaz sağlama hizmeti ve IoT Hub cihazları için özel cihaz sağlama kavramlarını açıklar
author: nberdy
ms.author: nberdy
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 8ea1be02dee0e0ef00010e8ac7a4dfb75eadbe96
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72173393"
---
# <a name="iot-hub-device-provisioning-service-device-concepts"></a>Cihaz sağlama hizmeti cihaz kavramlarını IoT Hub

IoT Hub cihaz sağlama hizmeti, belirli bir IoT Hub 'ına sıfır Touch cihaz sağlamayı yapılandırmak için kullandığınız IoT Hub yardımcı hizmettir. Cihaz sağlama hizmeti ile milyonlarca cihazı güvenli ve ölçeklenebilir bir şekilde sağlayabilirsiniz.

Bu makale, cihaz sağlama ile ilgili *cihaz* kavramlarına genel bir bakış sunar. Bu makale, bir cihazı dağıtıma hazırlamak için [Üretim adımında](about-iot-dps.md#manufacturing-step) yer alan personbuna uygundur.

## <a name="attestation-mechanism"></a>Kanıtlama mekanizması

Kanıtlama mekanizması, bir cihazın kimliğini onaylamak için kullanılan yöntemdir. Kanıtlama mekanizması, sağlama hizmetine belirli bir cihazla hangi kanıtlama yöntemini kullanacağınızı belirten kayıt listesi ile de ilgilidir.

> [!NOTE]
> IoT Hub, söz konusu hizmette benzer bir kavram için "kimlik doğrulama şeması" nı kullanır.

Cihaz sağlama hizmeti aşağıdaki kanıtlama biçimlerini destekler:
* Standart X. 509.440 sertifika kimlik doğrulama akışına göre **X. 509.440 sertifikaları** .
* Anahtar bir paylaşılan erişim Imzası (SAS) belirteci sunmak için anahtarlar için TPM standardı kullanarak, bir kerelik anahtar sınamasını temel alan **Güvenilir Platform Modülü (TPM)** . Bu, cihazda fiziksel TPM gerektirmez, ancak hizmet [TPM belirtimi](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/)başına onay anahtarını kullanarak test bulmayı bekler.
* Karma **anahtar** , karma bir imza ve katıştırılmış süre sonu içeren paylaşılan erişim IMZASı (SAS) [güvenlik belirteçlerini](../iot-hub/iot-hub-devguide-security.md#security-tokens)temel alır. Daha fazla bilgi için bkz. [simetrik anahtar kanıtlama](concepts-symmetric-key-attestation.md).

## <a name="hardware-security-module"></a>Donanım güvenlik modülü

Donanım güvenlik modülü veya HSM, cihaz gizli dizileri için güvenli, donanım tabanlı depolamada kullanılır ve gizli depolama alanının en güvenli biçimidir. Hem X. 509.440 sertifikaları hem de SAS belirteçleri HSM 'de depolanabilir. HSM 'ler, sağlama hizmeti 'nin desteklediği kanıtlama mekanizmalarıyla birlikte kullanılabilir.

> [!TIP]
> Cihazlarınızda gizli dizileri güvenli bir şekilde depolamak için cihazlarla bir HSM kullanmanız önemle önerilir.

Cihaz gizli dizileri de yazılımda (bellek) depolanabilir, ancak bir HSM 'den daha az güvenli bir depolama biçimidir.

## <a name="registration-id"></a>Kayıt KIMLIĞI

Kayıt KIMLIĞI, cihaz sağlama hizmetinde bir cihazı benzersiz şekilde tanımlamak için kullanılır. Kayıt KIMLIĞI, sağlama hizmeti [kimlik kapsamında](#id-scope)benzersiz olmalıdır. Her cihazın bir kayıt KIMLIĞI olmalıdır. Kayıt KIMLIĞI alfasayısal, küçük harf ve kısa çizgi içerebilir.

* TPM söz konusu olduğunda kayıt KIMLIĞI TPM tarafından sağlanır.
* X. 509.440 tabanlı kanıtlama söz konusu olduğunda, kayıt KIMLIĞI sertifikanın konu adı olarak sağlanır.

## <a name="device-id"></a>Cihaz Kimliği

Cihaz KIMLIĞI, IoT Hub göründüğü şekliyle KIMLIĞIDIR. İstenen cihaz KIMLIĞI kayıt girişinde ayarlanabilir, ancak ayarlanması gerekli değildir. Kayıt listesinde istenen cihaz KIMLIĞI belirtilmemişse, cihaz kaydı sırasında cihaz KIMLIĞI olarak kayıt KIMLIĞI kullanılır. [IoT Hub cihaz kimlikleri](../iot-hub/iot-hub-devguide-identity-registry.md)hakkında daha fazla bilgi edinin.

## <a name="id-scope"></a>KIMLIK kapsamı

KIMLIK kapsamı, Kullanıcı tarafından oluşturulduğunda bir cihaz sağlama hizmetine atanır ve cihazın kaydedileceği belirli sağlama hizmetini benzersiz şekilde tanımlamak için kullanılır. KIMLIK kapsamı, hizmet tarafından oluşturulur ve, benzersizliği garanti eden sabittir.

> [!NOTE]
> Benzersizlik, uzun süre çalışan dağıtım işlemleri ve merme ve Alım senaryoları için önemlidir.
