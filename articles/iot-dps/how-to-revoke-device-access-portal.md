---
title: Azure IoT Hub Aygıt Sağlama Hizmeti'nden Disenroll aygıtı
description: Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) aracılığıyla sağlanmasını önlemek için bir aygıtı niçin disenroll'a alırnız?
author: wesmc7777
ms.author: wesmc
ms.date: 04/05/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: af883da67f4e1bc819514e88ff480526e16124db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74974930"
---
# <a name="how-to-disenroll-a-device-from-azure-iot-hub-device-provisioning-service"></a>Bir aygıtı Azure IoT Hub Aygıt Sağlama Hizmeti'nden nasıl çıkarabilirsiniz?

Cihaz kimlik bilgilerinin doğru yönetimi, IoT çözümleri gibi yüksek profilli sistemler için çok önemlidir. Bu tür sistemler için en iyi uygulama, kimlik bilgileri, paylaşılan erişim imzaları (SAS) belirteci veya X.509 sertifikası tehlikeye atıldığında aygıtlar için erişimi nasıl iptal edebileceklerine ilişkin net bir plana sahip olmaktır. 

Aygıt Sağlama Hizmetine kaydolmak, aygıtın [otomatik olarak sağlanmasını](concepts-auto-provisioning.md)sağlar. Sağlanan aygıt, IoT Hub'a kaydedilmiş olup, ilk [aygıt ikiz](~/articles/iot-hub/iot-hub-devguide-device-twins.md) durumunu almasına ve telemetri verilerini raporlamaya başlamasına olanak tanır. Bu makalede, bir aygıtı sağlama hizmeti örneğinden nasıl çıkarılatır, gelecekte yeniden sağlanmasını önler.

> [!NOTE] 
> Erişimi iptal ettiğiniz aygıtların yeniden deneme ilkesine dikkat edin. Örneğin, sonsuz yeniden deneme ilkesi olan bir aygıt sürekli olarak sağlama hizmetine kaydolmayı deneyebilir. Bu durum hizmet kaynaklarını tüketir ve performansı etkiler.

## <a name="blacklist-devices-by-using-an-individual-enrollment-entry"></a>Tek bir kayıt girişi kullanarak aygıtları kara listeye a

Tek tek kayıtlar tek bir aygıtiçin geçerlidir ve x.509 sertifikaları veya SAS belirteçleri (gerçek veya sanal TPM olarak) attestation mekanizması olarak kullanabilirsiniz. (SAS belirteçlerini attestation mekanizması olarak kullanan aygıtlar yalnızca tek bir kayıt yoluyla sağlanabilir.) Tek bir kaydı olan bir aygıtı kara listeye almak için, aygıtın kayıt girişini devre dışı layabilir veya silebilirsiniz. 

Kayıt girişini devre dışı bırakarak aygıtı geçici olarak kara listeye almak için: 

1. Azure portalında oturum açın ve sol menüden **Tüm kaynakları** seçin.
2. Kaynaklar listesinde, cihazınızı kara listeye almak istediğiniz sağlama hizmetini seçin.
3. Sağlama hizmetinizde, **Kayıtları Yönet'i**seçin ve ardından **Bireysel Kayıtlar** sekmesini seçin.
4. Kara listeye almak istediğiniz aygıtın kayıt girişini seçin. 

    ![Bireysel kaydınızı seçin](./media/how-to-revoke-device-access-portal/select-individual-enrollment.png)

5. Kayıt sayfanızda, en alta gidin ve Giriş **Anahtarını Etkinleştir** için **Devre Dışı'yı** seçin ve sonra **Kaydet'i**seçin.  

   ![Portaldaki bireysel kayıt girişini devre dışı](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)

Kayıt girişini silerek aygıtı kalıcı olarak kara listeye almak için:

1. Azure portalında oturum açın ve sol menüden **Tüm kaynakları** seçin.
2. Kaynaklar listesinde, cihazınızı kara listeye almak istediğiniz sağlama hizmetini seçin.
3. Sağlama hizmetinizde, **Kayıtları Yönet'i**seçin ve ardından **Bireysel Kayıtlar** sekmesini seçin.
4. Kara listeye almak istediğiniz aygıtın kayıt girişinin yanındaki onay kutusunu seçin. 
5. Pencerenin üst kısmında **Sil'i** ve ardından kaydı kaldırmak istediğinizi onaylamak için **Evet'i** seçin. 

   ![Portaldaki tek tek kayıt girişini silme](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)


Yordamı tamamladıktan sonra, girişinizin tek tek kayıtlar listesinden kaldırıldığını görmeniz gerekir.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-by-using-an-enrollment-group"></a>Bir kayıt grubu kullanarak bir X.509 ara veya kök CA sertifikasını kara listeye ala

X.509 sertifikaları genellikle bir güven sertifikası zincirinde düzenlenir. Zincirdeki herhangi bir aşamada ki sertifika tehlikeye girerse, güven bozulur. Sertifika, Aygıt Sağlama Hizmeti'nin bu sertifikayı içeren herhangi bir zincirde aygıtları akış aşağı sağlamasını önlemek için kara listeye alınmalıdır. X.509 sertifikaları ve sağlama hizmetinde nasıl kullanıldıkları hakkında daha fazla bilgi edinmek için [X.509 sertifikalarına](./concepts-security.md#x509-certificates)bakın. 

Kayıt grubu, aynı ara veya kök CA tarafından imzalanmış X.509 sertifikalarının ortak bir attestation mekanizmasını paylaşan aygıtlar için bir giriştir. Kayıt grubu girişi, ara veya kök CA ile ilişkili X.509 sertifikası ile yapılandırılır. Giriş, sertifika zincirinde bu sertifikaya sahip aygıtlar tarafından paylaşılan ikiz durum ve IoT hub bağlantısı gibi yapılandırma değerleriyle de yapılandırılır. Sertifikayı kara listeye almak için, sertifikayı devre dışı veya silebilirsiniz.

Kayıt grubunu devre dışı bırakarak sertifikayı geçici olarak kara listeye almak için: 

1. Azure portalında oturum açın ve sol menüden **Tüm kaynakları** seçin.
2. Kaynaklar listesinde, imzalama sertifikasını kara listeye almak istediğiniz sağlama hizmetini seçin.
3. Sağlama hizmetinizde, **kayıtları Yönet'i**seçin ve ardından **Kayıt Grupları** sekmesini seçin.
4. Kara listeye almak istediğiniz sertifikayı kullanarak kayıt grubunu seçin.
5. Giriş anahtarını etkinleştir'de **Devre dışı** **ver'i** seçin ve sonra **Kaydet'i**seçin.  

   ![Portaldaki kayıt grubu girişini devre dışı](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
Sertifikayı kayıt grubunu silerek kalıcı olarak kara listeye almak için:

1. Azure portalında oturum açın ve sol menüden **Tüm kaynakları** seçin.
2. Kaynaklar listesinde, cihazınızı kara listeye almak istediğiniz sağlama hizmetini seçin.
3. Sağlama hizmetinizde, **kayıtları Yönet'i**seçin ve ardından **Kayıt Grupları** sekmesini seçin.
4. Kara listeye almak istediğiniz sertifika için kayıt grubunun yanındaki onay kutusunu seçin. 
5. Pencerenin üst kısmında **Sil'i** ve ardından kayıt grubunu kaldırmak istediğinizi onaylamak için **Evet'i** seçin. 

   ![Portaldaki kayıt grubu girişini silme](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

Yordamı tamamladıktan sonra, girişinizin kayıt grupları listesinden kaldırıldığını görmeniz gerekir.  

> [!NOTE]
> Bir sertifika için bir kayıt grubunu silerseniz, sertifika zincirinde sertifika bulunan aygıtlar, kök sertifika için etkin bir kayıt grubu veya sertifikasında daha yüksek bir ara sertifika varsa kaydolmaya devam edebilir zincir vardır.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Kayıt grubundaki belirli aygıtları kara listeye ada

X.509 attestation mekanizmasını uygulayan aygıtlar, cihazın sertifika zincirini ve kimlik doğrulaması için özel anahtarı kullanır. Bir aygıt Aygıt Sağlama Hizmeti'ne bağlandığında ve kimlik doğruladığında, hizmet önce aygıtın kimlik bilgileriyle eşleşen tek bir kayıt arar. Hizmet daha sonra, aygıtın sağlanıp sağlanamayacağını belirlemek için kayıt gruplarını arar. Hizmet aygıt için engelli bir tek tek kayıt bulursa, aygıtın bağlanmasını engeller. Hizmet, aygıtın sertifika zincirinde ara veya kök CA için etkin leştirilmiş bir kayıt grubu olsa bile bağlantıyı engeller. 

Kayıt grubunda tek bir aygıtı kara listeye almak için aşağıdaki adımları izleyin:

1. Azure portalında oturum açın ve sol menüden **Tüm kaynakları** seçin.
2. Kaynaklar listesinden, kara listeye almak istediğiniz aygıt için kayıt grubunu içeren sağlama hizmetini seçin.
3. Sağlama hizmetinizde, **Kayıtları Yönet'i**seçin ve ardından **Bireysel Kayıtlar** sekmesini seçin.
4. En üstteki **tek tek kayıt ekle** düğmesini seçin. 
5. Kayıt **Ekle** sayfasında, aygıt için attestation **Mekanizması** olarak **X.509'u** seçin.

    Aygıt sertifikasını yükleyin ve kara listeye alınacak aygıtın aygıt kimliğini girin. Sertifika için aygıta yüklenen imzalı son varlık sertifikasını kullanın. Aygıt, kimlik doğrulaması için imzalı son varlık sertifikasını kullanır.

    ![Kara listeye alınan aygıt için aygıt özelliklerini ayarlama](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group-1.png)

6. **Kayıt Ekle** sayfasının altına gidin ve **Giriş Anahtarını Etkinleştir'de** Devre **dışı** Ver'i seçin ve ardından **Kaydet'i**seçin. 

    [![Portalda, grup kaydından aygıtı devre dışı bırakma için engelli bireysel kayıt girişini kullanma](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group.png)](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group.png#lightbox)

Kaydınızı başarıyla oluşturduğunuzda, engelli aygıt kaydınızın **Bireysel Kayıtlar** sekmesinde listelenmiş olduğunu görmeniz gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

Kayıt tanınma da daha büyük deprovisioning sürecinin bir parçasıdır. Bir aygıtın hükmünü ortadan köme, hem sağlama hizmetinden kaydın silinmesini hem de IoT hub'ından kaydın silinmesini içerir. Tam işlem hakkında bilgi edinmek [için, daha önce otomatik olarak sağlanan aygıtların nasıl ayrışdırılabildiğini](how-to-unprovision-devices.md) görün 

