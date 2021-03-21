---
title: IoT Hub içeri aktarmaya yönelik cihaz güncelleştirmesini anlama | Microsoft Docs
description: IoT Hub için yeni bir güncelleştirmeyi cihaz güncelleştirmesine aktarmaya yönelik temel kavramlar.
author: andrewbrownmsft
ms.author: andbrown
ms.date: 2/10/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: 4cd5e0c016b98a3dc9336237a5c1b14e6b0f5789
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102040592"
---
# <a name="importing-updates-into-device-update-for-iot-hub"></a>IoT Hub için güncelleştirmeleri cihaz güncelleştirmesine aktarma
IoT Hub cihaz güncelleştirmesinden cihazlara bir güncelleştirme dağıtmak için, önce bu güncelleştirmeyi cihaz güncelleştirme hizmetine _aktarmanız_ gerekir. Güncelleştirmelerin içeri aktarılmasının ne zaman geldiğini anlamak için bazı önemli kavramlara genel bakış sunulmaktadır.

## <a name="import-manifest"></a>İçeri aktarma bildirimi

İçeri aktarma bildirimi, içeri aktardığınız güncelleştirmeyle ilgili önemli bilgileri tanımlayan bir JSON dosyasıdır. İçeri aktarma işleminin bir parçası olarak hem içeri aktarma bildirimi ve ilişkili güncelleştirme dosyası ya da dosyalarınızı (bellenim güncelleştirme paketi gibi) gönderebilirsiniz. İçeri aktarma bildiriminde tanımlanan meta veriler, güncelleştirmeyi almak için kullanılır. Meta verilerden bazıları dağıtım zamanında da kullanılır. Örneğin, bir güncelleştirmenin doğru şekilde yüklenip yüklenmediğini doğrulamak için.

İçeri aktarma bildirimi, IoT Hub kavramları için önemli cihaz güncelleştirmesini temsil eden çeşitli öğeler içerir. Bu kavramlar aşağıda özetlenmiştir.

### <a name="update-identity-update-id"></a>Güncelleştirme kimliği (güncelleştirme KIMLIĞI)

Güncelleştirme kimliği, bir güncelleştirmenin benzersiz tanımlayıcısını temsil eder. İçeri aktarılmakta olan bir güncelleştirmeyle ilgili önemli özellikleri tanımlar. Güncelleştirme kimliği üç bölümden oluşur:
* Sağlayıcı: Bu, güncelleştirmeyi oluşturan veya doğrudan sorumlu olan varlıktır. Genellikle şirket adı olacaktır.
* Ad: bir güncelleştirme sınıfının tanımlayıcısı. Sınıf, seçtiğiniz herhangi bir şey olabilir. Genellikle bir cihaz veya model adı olacaktır.
* Sürüm: Bu güncelleştirme, aynı sağlayıcıya ve ada sahip olan başkalarından ayırt edici bir sürüm numarasıdır. Bu sürüm, IoT Hub hizmeti için cihaz güncelleştirmesi tarafından kullanılır ve cihazdaki bireysel yazılım bileşeninin bir sürümüyle eşleşmeyebilir veya eşleşmeyebilir. 

### <a name="compatibility"></a>Uyumluluk

Güncelleştirme dağıtımlarını basitleştirmek için, IoT Hub için cihaz güncelleştirmesi, içeri aktarma bildiriminde tanımlanan ve karşılık gelen cihaz özellikleriyle tanımlanan uyumluluk özelliklerini karşılaştırır. Yalnızca eşleşen özellikleri olan güncelleştirmeler döndürülecek ve dağıtım için kullanılabilir olacaktır.

### <a name="installedcriteria"></a>Instalınstalbu ölçüt

Instalyüklenmekte olan ölçütler bir güncelleştirmenin başarıyla yüklenip yüklenmediğini öğrenmek için bir cihazdaki Güncelleştirme Aracısı tarafından kullanılır.


## <a name="next-steps"></a>Sonraki adımlar

Hazır olduğunuzda Içeri aktarma işlemi adım adım yönergeler için size yol gösterecek [Içeri aktarma How-To kılavuzunu](./import-update.md)deneyin.


