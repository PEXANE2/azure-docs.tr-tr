---
title: Azure IoT Central ile X. 509.440 sertifikalarını al
description: IoT Central uygulamanızla X. 509.440 sertifikalarını alma
author: v-krghan
ms.author: v-krghan
ms.date: 07/31/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 76e2b9542d20b5788a2875dec89d447ce38276a0
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88122107"
---
# <a name="how-to-roll-x509-device-certificates-in-iot-central-application"></a>IoT Central uygulamasında X. 509.440 cihaz sertifikalarını alma

IoT çözümünüzün yaşam döngüsü boyunca sertifikaları almanız gerekir. Sıralı sertifikaların temel nedenlerinden ikisi bir güvenlik ihlali ve sertifika süre sonları olacaktır. 

Çalışan sertifikalar, bir ihlal durumunda sisteminizin güvenliğini sağlamaya yardımcı olmak için en iyi güvenlik uygulamasıdır. [Ihlal metodolojinin](https://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf)bir parçası olarak Microsoft, önleyici ölçülerle birlikte reaktif güvenlik işlemlerine sahip olma gereksinimini ortadan kaldırır. Cihaz sertifikalarınızın, bu güvenlik işlemlerinin bir parçası olarak eklenmesi gerekir. Sertifikalarınızı alacağınız sıklık, çözümünüzün güvenlik ihtiyaçlarına bağlı olarak değişir. Son derece hassas verileri içeren çözümleri olan müşteriler, sertifikaları her iki yılda bir kez sunarken, her gün sertifika alabilir.


## <a name="obtain-new-x509-certificates"></a>Yeni X. 509.952 sertifikaları edinin

OpenSSL gibi bir araç kullanarak kendi X. 509.952 sertifikalarınızı oluşturabilirsiniz. Bu yaklaşım, X. 509.440 sertifikalarını test etmek için idealdir, ancak güvenlik konusunda birkaç güvence sağlar. Bu yaklaşımı yalnızca kendi CA sağlayıcınız olarak davranmaya hazırlanmadığınız sürece test için kullanın.


## <a name="enrollment-groups-and-security-breaches"></a>Kayıt grupları ve güvenlik ihlaller

Bir güvenlik ihlaline yanıt olarak bir grup kaydını güncelleştirmek için, geçerli sertifikayı hemen güncelleştiren aşağıdaki yaklaşımı kullanmanız gerekir:

1. Sol bölmedeki **Yönetim** ' e gidin ve **cihaz bağlantısı**' na tıklayın.

2. **Kayıt grupları**' na tıklayın ve listede Grup adına tıklayın.

    ![Cihaz bağlantısı](./media/how-to-roll-x509-certificates/device-connection.png)


3. Sertifika güncelleştirmesi için **birincil Yönet** ' e tıklayın veya **ikincili yönetin**.

    ![Sertifikaları yönetme](./media/how-to-roll-x509-certificates/certificates.png)


4. Kayıt grubuna kök X. 509.440 sertifikası ekleyin ve doğrulayın.

   Her ikisi de tehlikeye girerse, birincil ve ikincil sertifikalar için bu adımları uygulayın.



## <a name="enrollment-groups-and-certificate-expiration"></a>Kayıt grupları ve sertifika süre sonu

Sertifika süre sonlarını işlemek için bir sertifika alıyorsanız, geçerli sertifikayı hemen güncelleştirmek için aşağıdaki yaklaşımı kullanın:

1. Sol bölmedeki **Yönetim** ' e gidin ve **cihaz bağlantısı**' na tıklayın. 

2. **Kayıt grupları**' na tıklayın ve listede Grup adına tıklayın.

    ![Cihaz bağlantısı](./media/how-to-roll-x509-certificates/device-connection.png)


3. Sertifika güncelleştirmesi için, **birincil Yönet**' e tıklayın.

    ![Cihaz bağlantısı](./media/how-to-roll-x509-certificates/manage-certs.png)

4. Kayıt grubuna kök X. 509.440 sertifikası ekleyin ve doğrulayın.

5. Daha sonra ikincil sertifikanın süresi dolduğunda, geri dönüp ikincil sertifikayı güncelleştirin.



## <a name="individual-enrollments-and-security-breaches"></a>Bireysel kayıtlar ve güvenlik ihlalleri

Bir güvenlik ihlaline yanıt olarak sertifika alıyorsanız, geçerli sertifikayı hemen güncelleştirmek için aşağıdaki yaklaşımı kullanın:


1. **Cihazlar**' a tıklayın ve cihazı seçin. 

2. **Bağlan**' a tıklayın ve yöntemi **bireysel kayıt** olarak bağlan ' ı seçin

3. Bir mekanizma olarak **Sertifikalar (X. 509.440)** seçeneğini belirleyin.

    ![Bireysel kayıtları yönetme](./media/how-to-roll-x509-certificates/certificate-update.png)

4. Sertifika güncelleştirme için, kayıt girişi için karşıya yüklenecek yeni sertifikayı seçmek üzere klasör simgesine tıklayın. **Kaydet**’e tıklayın.

    Her ikisi de tehlikeye girerse, birincil ve ikincil sertifikalar için bu adımları izleyin



## <a name="individual-enrollments-and-certificate-expiration"></a>Bireysel kayıtlar ve sertifika süre sonu

Sertifika süre sonlarını işlemek için bir sertifika alıyorsanız, sağlama girişiminde bulunan cihazların kapalı kalma süresini azaltmak için ikincil sertifika yapılandırmasını aşağıdaki şekilde kullanmanız gerekir.

Daha sonra ikincil sertifika süre sonu yaklaştığında ve alınması gerektiğinde, birincil yapılandırmayı kullanarak döndürebilirsiniz. Bu şekilde birincil ve ikincil Sertifikalar arasında döndürme, sağlamaya çalışan cihazların kapalı kalma süresini azaltır.

1. **Cihazlar**' a tıklayın ve cihazı seçin.

2. **Bağlan**' a tıklayın ve yöntemi **bireysel kayıt** olarak bağlan ' ı seçin

3. Bir mekanizma olarak **Sertifikalar (X. 509.440)** seçeneğini belirleyin.

    ![Bireysel kayıtları yönetme](./media/how-to-roll-x509-certificates/certificate-update.png)

4. İkincil sertifika güncelleştirmesi için, kayıt girişi için yüklenecek yeni sertifikayı seçmek üzere klasör simgesine tıklayın. **Kaydet**’e tıklayın.


5. Daha sonra birincil sertifikanın süresi dolduğunda, geri dönüp ilgili birincil sertifikayı güncelleştirin.


## <a name="next-steps"></a>Sonraki adımlar

Artık Azure IoT Central uygulamanızda X. 509.440 sertifikalarının nasıl alınacağını öğrendiğinize göre [azure IoT Central 'ye](concepts-get-connected.md)bağlanabilirsiniz.


