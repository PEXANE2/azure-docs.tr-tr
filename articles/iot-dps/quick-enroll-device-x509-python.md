---
title: X.509 aygıtlarını Python kullanarak Azure Aygıt Sağlama Hizmetine kaydedin
description: Bu hızlı başlangıçta grup kayıtları kullanılmaktadır. Bu hızlı başlangıçta, X.509 aygıtlarını Python kullanarak Azure IoT Hub Aygıt Sağlama Hizmetine (DPS) kaydedebilirsiniz
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.devlang: python
ms.custom: mvc
ms.openlocfilehash: ed51fb7589247b1a52930931ed297d4292b07ea6
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77921139"
---
# <a name="quickstart-enroll-x509-devices-to-the-device-provisioning-service-using-python"></a>Hızlı başlangıç: Python kullanarak X.509 cihazlarını Cihaz Sağlama Hizmeti'ne kaydetme

[!INCLUDE [iot-dps-selector-quick-enroll-device-x509](../../includes/iot-dps-selector-quick-enroll-device-x509.md)]

Bu hızlı başlatmada, ara veya kök CA X.509 sertifikalarını kullanan bir kayıt grubu oluşturmak için Python'u kullanırsınız. Kayıt grubu, sertifika zincirlerinde ortak imzalama sertifikasını paylaşan cihazlar için sağlama hizmetine erişimi denetler. Kayıt grubu Python Sağlama Hizmeti SDK'sı ve örnek Python uygulaması kullanılarak oluşturulur.

## <a name="prerequisites"></a>Ön koşullar

- Azure [portalı ile IoT Hub Aygıt Sağlama Hizmetini Ayarlama'nın](./quick-setup-auto-provision.md)tamamlanması.
- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Python 2.x veya 3.x](https://www.python.org/downloads/). Platforma özel ortam değişkenlerinize Python ekleyin. Bu quickstart [Python Sağlama Hizmeti SDK'yı](https://github.com/Azure/azure-iot-sdk-python/tree/v1-deprecated/provisioning_service_client) aşağıda yükler.
- [Pip](https://pip.pypa.io/en/stable/installing/), Python dağıtımınıza dahil değilse.
- [Git.](https://git-scm.com/download/)

> [!IMPORTANT]
> Bu makale yalnızca amortismana uygulanan V1 Python SDK için geçerlidir. IoT Hub Cihaz Sağlama Hizmeti için aygıt ve servis istemcileri v2'de henüz kullanıma sunulmadı. Takım şu anda v2 özelliği parite getirmek için iş başında.

## <a name="prepare-test-certificates"></a>Test sertifikalarını hazırlama

Bu hızlı başlangıç için ara veya kök CA X.509 sertifikasının ortak bölümünü içeren bir .pem veya .cer dosyasına sahip olmanız gerekir. Bu sertifikanın sağlama hizmetinize yüklenmesi ve hizmet tarafından doğrulanması gerekir.

Azure IoT Hub ve Cihaz Sağlama Hizmeti ile X.509 sertifikası tabanlı Ortak Anahtar Altyapısı'nı (PKI) kullanma hakkında daha fazla bilgi için bkz. [X.509 CA sertifikası güvenliğine genel bakış](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview).

[Azure IoT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c) X.509 sertifika zinciri oluşturmanıza, bu zincirdeki bir kök veya ara sertifikayı yüklemenize ve sertifikayı doğrulamak için hizmette sahip olma onayı gerçekleştirmenize yardımcı olabilecek test araçları içerir. SDK aracıyla oluşturulan sertifikalar **yalnızca geliştirme testi** amacıyla kullanılacak şekilde tasarlanmıştır. Bu sertifikalar **üretim ortamında kullanılmamalıdır**. Sertifikalarda 30 gün sonra süresi dolacak değiştirilemeyen parolalar ("1234") bulunur. Üretim ortamında kullanıma uygun sertifikalar almayı öğrenmek için, Azure IoT Hub belgelerinde [X.509 CA sertifikası alma](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview#how-to-get-an-x509-ca-certificate) konusuna bakın.

Bu test araçlarını kullanarak sertifika üretmek için aşağıdaki adımları izleyin:

1. Azure IoT C SDK'nın [en son sürümü](https://github.com/Azure/azure-iot-sdk-c/releases/latest) için etiket adını bulun.

2. Komut istemi veya Git Bash kabuğu açın ve makinenizdeki çalışma klasörüne geçin. [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub deposunun en son sürümünde klonlamak için aşağıdaki komutları çalıştırın. Önceki adımda bulduğunuz etiketi `-b` parametre nin değeri olarak kullanın:

    ```cmd/sh
    git clone -b <release-tag> https://github.com/Azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c
    git submodule update --init
    ```

    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.

   Test araçları kopyaladığınız deponun *azure-iot-sdk-c/tools/CACertificates* dizininde bulunur.

3. [Örnekler ve öğreticiler için test amaçlı CA sertifikalarını yönetme](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) adımlarını izleyin. 

## <a name="modify-the-python-sample-code"></a>Python örnek kodunu değiştirme

Bu bölümde örnek koda X.509 cihazınızın sağlama ayrıntılarını nasıl ekleyeceğiniz gösterilir. 

1. Metin düzenleyicisi kullanarak yeni bir **EnrollmentGroup.py** dosyası oluşturun.

1. Aşağıdaki `import` deyimlerini ve değişkenlerini **EnrollmentGroup.py** dosyasının başlangıcına ekleyin. Ardından `dpsConnectionString` **Azure portalındaki** **Aygıt Sağlama Hizmetinizde** **Paylaşılan erişim ilkeleri** altında bulunan bağlantı dizenizi değiştirin. Sertifika yer tutucusunu, daha önce [Test sertifikalarını hazırlama](quick-enroll-device-x509-python.md#prepare-test-certificates) konusunda oluşturulan sertifika ile değiştirin. Son olarak, benzersiz bir `registrationid` oluşturun ve yalnızca küçük harf alfasayısal karakterler ve kısa çizgiler içerdiğinden emin olun.  
   
    ```python
    from provisioningserviceclient import ProvisioningServiceClient
    from provisioningserviceclient.models import EnrollmentGroup, AttestationMechanism

    CONNECTION_STRING = "{dpsConnectionString}"

    SIGNING_CERT = """-----BEGIN CERTIFICATE-----
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    -----END CERTIFICATE-----"""

    GROUP_ID = "{registrationid}"
    ```

1. Grup kaydı oluşturmasını uygulamak için aşağıdaki işlevi ve işlev çağrısını ekleyin:
   
    ```python
    def main():
        print ( "Initiating enrollment group creation..." )

        psc = ProvisioningServiceClient.create_from_connection_string(CONNECTION_STRING)
        att = AttestationMechanism.create_with_x509_signing_certs(SIGNING_CERT)
        eg = EnrollmentGroup.create(GROUP_ID, att)

        eg = psc.create_or_update(eg)
    
        print ( "Enrollment group created." )

    if __name__ == '__main__':
        main()
    ```

1. **EnrollmentGroup.py** dosyasını kaydedip kapatın.
 

## <a name="run-the-sample-group-enrollment"></a>Örnek grup kaydını çalıştırma

Azure IoT Cihaz Sağlama Hizmeti iki tür kaydı destekler:

- [Kayıt grupları](concepts-service.md#enrollment-group): Birden fazla ilgili cihazı kaydetmek için kullanılır.
- [Tek tek kayıtlar](concepts-service.md#individual-enrollment): Tek bir cihazı kaydetmek için kullanılır.

[Python Sağlama Hizmeti SDK](https://github.com/Azure/azure-iot-sdk-python/tree/v1-deprecated/provisioning_service_client)'sı ile Bireysel kayıt oluşturma özelliği yapım aşamasındadır. Daha fazla bilgi edinmek için bkz. [X.509 sertifikalarıyla sağlama hizmetine cihaz erişimini denetleme](./concepts-security.md#controlling-device-access-to-the-provisioning-service-with-x509-certificates).

1. Bir komut istemi açın ve [azure-iot-provisioning-device-client'ı](https://pypi.org/project/azure-iot-provisioning-device-client)yüklemek için aşağıdaki komutu çalıştırın.

    ```cmd/sh
    pip install azure-iothub-provisioningserviceclient    
    ```

2. Komut isteminde betiği çalıştırın.

    ```cmd/sh
    python EnrollmentGroup.py
    ```

3. Kaydın başarılı olup olmadığını görmek için çıktıyı gözlemleyin.

4. Azure portalında sağlama hizmetinize gidin. **Kayıtları yönetme**'ye tıklayın. X.509 cihaz grubunuzun daha önce oluşturulmuş `registrationid` adıyla **Kayıt Grupları** sekmesi altında göründüğüne dikkat edin. 

    ![Portalda X.509 kaydının başarılı olup olmadığını doğrulama](./media/quick-enroll-device-x509-python/1.png)  


## <a name="clean-up-resources"></a>Kaynakları temizleme
Java hizmeti örneğini keşfetmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlatma tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizdeki Java örnek çıktı penceresini kapatın.
1. Makinenizde _X509 Cert Generator_ penceresini kapatın.
1. Azure portalında Cihaz Sağlama hizmetinize gidin, **kayıtları Yönet'i**seçin ve ardından **Kayıt Grupları** sekmesini seçin. Bu hızlı başlangıç kullanarak kaydolduğunuz X.509 aygıtları için *GROUP NAME'nin* yanındaki onay kutusunu seçin ve bölmenin üst kısmındaki **Sil** düğmesine basın.    


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Simüle edilmiş bir X.509 aygıtı grubunu Aygıt Sağlama hizmetinize kaydettiniz. Cihaz sağlama hakkında ayrıntılı bilgi edinmek için Azure portalında Cihaz Sağlama Hizmeti ayarları öğreticisine geçin. 

> [!div class="nextstepaction"]
> [Azure IoT Hub Cihazı Sağlama Hizmeti öğreticileri](./tutorial-set-up-cloud.md)
