---
title: Azure IoT Hub Aygıt Sağlama Hizmeti ile sağlanan sağlama silme aygıtları
description: Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) ile sağlanan aygıtları sağlama
author: wesmc7777
ms.author: wesmc
ms.date: 05/11/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 8a3677ba285f5b02407ca3d176979bf6c016ef9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74974845"
---
# <a name="how-to-deprovision-devices-that-were-previously-auto-provisioned"></a>Daha önce otomatik olarak sağlanan aygıtların nasıl devre dışı sayılmalı? 

Aygıt Sağlama Hizmeti aracılığıyla daha önce otomatik olarak sağlanan aygıtları iptal etmeyi gerekli bulabilirsiniz. Örneğin, bir aygıt farklı bir IoT hub'ına satılabilir veya taşınabilir veya kaybolabilir, çalınabilir veya başka bir şekilde tehlikeye atılabilir. 

Genel olarak, bir aygıtın sağlanması iki adım gerektirir:

1. Gelecekteki otomatik sağlamayı önlemek için cihazı sağlama hizmetinizden uzaklaştırın. Erişimi geçici veya kalıcı olarak iptal etmek isteyip istemediğiniz bağlı olarak, bir kayıt girişini devre dışı veya silmek isteyebilirsiniz. X.509 attestation kullanan aygıtlar için, varolan kayıt gruplarınızın hiyerarşisindeki bir girişi devre dışı/silmek isteyebilirsiniz.  
 
   - Bir aygıtı nasıl disenroll'a çevireceğimiz hakkında bilgi almak için Azure [IoT Hub Aygıt Sağlama Hizmeti'nden bir aygıtı nasıl disenroll'a çevirebilirsiniz](how-to-revoke-device-access-portal.md)konusuna bakın.
   - Sağlama hizmeti SDK'larından birini kullanarak bir aygıtı programlı olarak nasıl disenroll'a getirebilirsiniz öğrenmek için, [bkz.](how-to-manage-enrollments-sdks.md)

2. Gelecekteki iletişim ve veri aktarımını önlemek için aygıtı IoT Hub'ınızdan silin. Yine, aygıtın kimlik kayıt defterine girişini, sağlandığı IoT Hub'ınkimlik defterine geçici olarak devre dışı layabilir veya kalıcı olarak silebilirsiniz. Devre dışı alma hakkında daha fazla bilgi edinmek için [aygıtları devre dışı](/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices) etme hakkında bkz. [Azure portalında](https://portal.azure.com)IoT Hub kaynağınız için "Aygıt Yönetimi / IoT Aygıtları" adlı kullanıcıya bakın.

Bir aygıtın hükmünü devre denarındırmak için attığınız tam adımlar, cihaz mekanizmasına ve sağlama hizmetinizle ilgili kayıt girişine bağlıdır. Aşağıdaki bölümler, kayıt ve attestation türüne bağlı olarak sürecin genel görünümünü sağlar.

## <a name="individual-enrollments"></a>Bireysel kayıtlar
Yaprak sertifikası yla TPM attestation veya X.509 attestation kullanan aygıtlar, tek bir kayıt girişi aracılığıyla sağlanır. 

Tek bir kaydı olan bir aygıtı niçin devre denecek: 

1. Cihazı sağlama hizmetinizden uzaklaştırın:

   - TPM attestation kullanan aygıtlar için, aygıtın sağlama hizmetine erişimini kalıcı olarak iptal etmek için tek tek kayıt girişini silin veya erişimini geçici olarak iptal etmek için girişi devre dışı bırakın. 
   - X.509 attestation kullanan aygıtlar için girişi silebilir veya devre dışı kullanabilirsiniz. Ancak, X.509 kullanan bir aygıt için tek tek bir kaydı silerseniz ve söz le ilgili bir imza sertifikası için etkin bir kayıt grubu varsa, aygıt yeniden kaydolabilir. Bu tür aygıtlar için kayıt girişini devre dışı kılabilir. Bunu yapmak, imza sertifikalarından biri için etkin bir kayıt grubunun bulunup bulunmadığına bakılmaksızın aygıtın yeniden kaydolmasını engeller.

2. IoT hub'ının kimlik kayıt defterindeki aygıtı devre dışı bırakın veya silin. 


## <a name="enrollment-groups"></a>Kayıt grupları
X.509 attestation ile aygıtlar bir kayıt grubu aracılığıyla da sağlanabilir. Kayıt grupları, ara veya kök CA sertifikası olan bir imza sertifikasıyla yapılandırılır ve sertifika zincirinde bu sertifikaya sahip aygıtlar için sağlama hizmetine erişimi denetler. Kayıt grupları ve sağlama hizmetiyle X.509 sertifikaları hakkında daha fazla bilgi edinmek için [X.509 sertifikalarına](concepts-security.md#x509-certificates)bakın. 

Bir kayıt grubu aracılığıyla sağlanan aygıtların listesini görmek için, kayıt grubunun ayrıntılarını görüntüleyebilirsiniz. Bu, her aygıtın hangi IoT hub'ına sağlandığını anlamanın kolay bir yoludur. Cihaz listesini görüntülemek için: 

1. Azure portalına giriş yapın ve sol menüdeki **Tüm kaynaklar'ı** tıklatın.
2. Kaynaklar listesinde sağlama hizmetinizi tıklatın.
3. Sağlama hizmetinizde, **kayıtları Yönet'i**tıklatın ve ardından **Kayıt Grupları** sekmesini seçin.
4. Açmak için kayıt grubunu tıklatın.

   ![Portalda kayıt grubu girişini görüntüleme](./media/how-to-unprovision-devices/view-enrollment-group.png)

Kayıt gruplarında göz önünde bulundurulması gereken iki senaryo vardır:

- Bir kayıt grubu aracılığıyla sağlanan tüm aygıtların hükmünü iptal etmek için:
  1. Kayıt grubunu imzalama sertifikasını kara listeye almak için devre dışı kalınır. 
  2. Her aygıtı ilgili IoT merkezinin kimlik kayıt defterinden devre dışı kakmak veya silmek için söz konusu kayıt grubu için sağlanan aygıtlar listesini kullanın. 
  3. Tüm aygıtları ilgili IoT hub'larından devre dışı bıraktıktan veya sildikten sonra, isteğe bağlı olarak kayıt grubunu silebilirsiniz. Ancak, kayıt grubunu silerseniz ve aygıtlardan birinin veya daha fazlasının sertifika zincirinde daha yüksek bir oturum sertifikası için etkin bir kayıt grubu varsa, bu aygıtların yeniden kaydolabileceğini unutmayın. 

- Kayıt grubundan tek bir aygıtı ayırmak için:
  1. Yaprak (aygıt) sertifikası için engelli bir bireysel kayıt oluşturun. Bu, bu aygıt için sağlama hizmetine erişimi iptal ederken, zincirlerinde kayıt grubunun imzalama sertifikasına sahip diğer aygıtlara erişime izin vermeye devam eder. Aygıt için engelli bireysel kaydını silmeyin. Bunu yapmak, aygıtın kayıt grubu üzerinden yeniden kaydolmasını sağlar. 
  2. Aygıtın bulunduğu IoT merkezini bulmak ve bu hub'ın kimlik kayıt defterinden devre dışı kalmak veya silmek için bu kayıt grubu için sağlanan aygıtlar listesini kullanın. 
  
  










