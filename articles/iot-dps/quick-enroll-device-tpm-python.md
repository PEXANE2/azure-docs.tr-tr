---
title: Python kullanarak TPM cihazını Azure cihaz sağlama hizmeti 'ne kaydetme
description: Hızlı başlangıç-Python sağlama hizmeti SDK 'sını kullanarak TPM cihazını Azure IoT Hub cihaz sağlama hizmeti 'ne (DPS) kaydetme. Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.devlang: python
ms.custom: mvc, tracking-python
ms.openlocfilehash: 3acb8b2ac75753a36097cb94a9294860c3f14354
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84608987"
---
# <a name="quickstart-enroll-tpm-device-to-iot-hub-device-provisioning-service-using-python-provisioning-service-sdk"></a>Hızlı başlangıç: Python sağlama hizmeti SDK 'sını kullanarak cihaz sağlama hizmeti 'ne IoT Hub TPM cihazı kaydetme

[!INCLUDE [iot-dps-selector-quick-enroll-device-tpm](../../includes/iot-dps-selector-quick-enroll-device-tpm.md)]

Bu hızlı başlangıçta, örnek Python uygulamasının yardımıyla Python sağlama hizmeti SDK 'sını kullanarak Azure IoT Hub cihaz sağlama hizmeti 'nde bir TPM cihazı için tek bir kayıt oluşturursunuz.

## <a name="prerequisites"></a>Önkoşullar

- [IoT Hub cihaz sağlama hizmetini Azure Portal Ile ayarlama](./quick-setup-auto-provision.md)işlemi tamamlandı.
- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Python 2.x veya 3.x](https://www.python.org/downloads/). Bu hızlı başlangıçta [Python sağlama hizmeti SDK 'sı](https://github.com/Azure/azure-iot-sdk-python/tree/v1-deprecated/provisioning_service_client) yüklenir.
- Python dağıtımına dahil edilmediğinden [PIP](https://pip.pypa.io/en/stable/installing/).
- Onay anahtarı. [Sanal cihaz oluşturma ve sağlama](quick-create-simulated-device.md) bölümündeki adımları kullanın veya AŞAĞıDA açıklanan SDK ile sağlanan onay anahtarını kullanın.

> [!IMPORTANT]
> Bu makale yalnızca kullanım dışı v1 Python SDK 'Sı için geçerlidir. IoT Hub cihaz sağlama hizmeti için cihaz ve hizmet istemcileri henüz v2 sürümünde kullanılamaz. Takım şu anda v2 'yi Özellik eşliği 'na getirmek için çalışıyoruz.

<a id="prepareenvironment"></a>

## <a name="prepare-the-environment"></a>Ortamı hazırlama 

1. [Python 2.x veya 3.x](https://www.python.org/downloads/) sürümünü indirip yükleyin. Kurulumunuzun gereksinimine uygun olarak 32 bit veya 64 bit yüklemeyi kullanmaya dikkat edin. Yükleme sırasında istendiğinde, platforma özgü ortam değişkeninize Python'u eklediğinizden emin olun. 

1. [Python sağlama hizmeti SDK 'sı](https://github.com/Azure/azure-iot-sdk-python/tree/v1-deprecated/provisioning_service_client)için aşağıdaki seçeneklerden birini seçin:

    - **Azure IoT Python SDK'sını** oluşturup derleyin. Python paketlerini derlemek için [bu yönergeleri](https://github.com/Azure/azure-iot-sdk-python/blob/v1-deprecated/doc/python-devbox-setup.md) uygulayın. Windows işletim sistemi kullanıyorsanız, Python’dan yerel DLL’lerin kullanımına olanak tanımak için [Visual C++ yeniden dağıtılabilir paketini](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) ayrıca yükleyin.

    - [Python paket yönetim sistemi olan *pip*’i yükleyin](https://pip.pypa.io/en/stable/installing/) veya yükseltin ve şu komut aracılığıyla paketi yükleyin:

        ```cmd/sh
        pip install azure-iothub-provisioningserviceclient
        ```

1. Cihazınız için onay anahtarı gerekir. [Sanal cihaz oluşturma ve sağlama](quick-create-simulated-device.md) hızlı başlangıcını izleyerek sanal bir TPM cihazı oluşturduysanız bu cihaz için oluşturulan anahtarı kullanın. Aksi takdirde, SDK ile birlikte verilen aşağıdaki onay anahtarını kullanabilirsiniz:

    ```
    AToAAQALAAMAsgAgg3GXZ0SEs/gakMyNRqXXJP1S124GUgtk8qHaGzMUaaoABgCAAEMAEAgAAAAAAAEAtW6MOyCu/Nih47atIIoZtlYkhLeCTiSrtRN3q6hqgOllA979No4BOcDWF90OyzJvjQknMfXS/Dx/IJIBnORgCg1YX/j4EEtO7Ase29Xd63HjvG8M94+u2XINu79rkTxeueqW7gPeRZQPnl1xYmqawYcyzJS6GKWKdoIdS+UWu6bJr58V3xwvOQI4NibXKD7htvz07jLItWTFhsWnTdZbJ7PnmfCa2vbRH/9pZIow+CcAL9mNTNNN4FdzYwapNVO+6SY/W4XU0Q+dLMCKYarqVNH5GzAWDfKT8nKzg69yQejJM8oeUWag/8odWOfbszA+iFjw3wVNrA5n8grUieRkPQ==
    ```


## <a name="modify-the-python-sample-code"></a>Python örnek kodunu değiştirme

Bu bölümde örnek koda TPM cihazınızın sağlama ayrıntılarını nasıl ekleyeceğiniz gösterilir. 

1. Metin düzenleyicisi kullanarak yeni bir **TpmEnrollment.py** dosyası oluşturun.

1. Aşağıdaki `import` deyimlerini ve değişkenlerini **TpmEnrollment.py** dosyasının başlangıcına ekleyin. Ardından `dpsConnectionString` , **Azure Portal** **cihaz sağlama hizmetinizdeki** **paylaşılan erişim ilkeleri** altında bulunan bağlantı dizenizle değiştirin. `endorsementKey` öğesini, daha önce [Ortamı hazırlama](quick-enroll-device-tpm-python.md#prepareenvironment) konusunda belirtilen değerle değiştirin. Son olarak, benzersiz bir `registrationid` oluşturun ve yalnızca küçük harf alfasayısal karakterler ve kısa çizgiler içerdiğinden emin olun.  
   
    ```python
    from provisioningserviceclient import ProvisioningServiceClient
    from provisioningserviceclient.models import IndividualEnrollment, AttestationMechanism

    CONNECTION_STRING = "{dpsConnectionString}"

    ENDORSEMENT_KEY = "{endorsementKey}"

    REGISTRATION_ID = "{registrationid}"
    ```

1. Grup kaydı oluşturmasını uygulamak için aşağıdaki işlevi ve işlev çağrısını ekleyin:
   
    ```python
    def main():
        print ( "Starting individual enrollment..." )

        psc = ProvisioningServiceClient.create_from_connection_string(CONNECTION_STRING)

        att = AttestationMechanism.create_with_tpm(ENDORSEMENT_KEY)
        ie = IndividualEnrollment.create(REGISTRATION_ID, att)

        ie = psc.create_or_update(ie)
    
        print ( "Individual enrollment successful." )
    
    if __name__ == '__main__':
        main()
    ```

1. **TpmEnrollment.py** dosyasını kaydedip kapatın.
 

## <a name="run-the-sample-tpm-enrollment"></a>Örnek TPM kaydını çalıştırma

1. Bir komut istemi açın ve betiği çalıştırın.

    ```cmd/sh
    python TpmEnrollment.py
    ```

1. Kaydın başarılı olup olmadığını görmek için çıktıyı gözlemleyin.

1. Azure portalında sağlama hizmetinize gidin. **Kayıtları yönetme**'yi seçin. TPM cihazınızın, daha önce oluşturulmuş `registrationid` adıyla **Bireysel Kayıtlar** sekmesi altında göründüğüne dikkat edin. 

    ![Portalda TPM kaydının başarılı olup olmadığını doğrulama](./media/quick-enroll-device-tpm-python/1.png)  


## <a name="clean-up-resources"></a>Kaynakları temizleme
Java hizmet örneğini keşfetmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizdeki Python örnek çıkış penceresini kapatın.
1. Sanal TPM cihazı oluşturduysanız, TPM simülatörü penceresini kapatın.
1. Azure portal cihaz sağlama hizmetine gidin, kayıtları **Yönet**' i seçin ve sonra **bireysel** kayıtlar sekmesini seçin. bu hızlı başlangıç Ile oluşturduğunuz kayıt girişinin *kayıt kimliği* ' nin yanındaki onay kutusunu işaretleyin ve bölmenin en üstündeki **Sil** düğmesine basın.


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, bir TPM aygıtı için program aracılığıyla tek bir kayıt girişi oluşturdunuz ve isteğe bağlı olarak, makinenizde bir TPM sanal cihazı oluşturdunuz ve Azure IoT Hub cihaz sağlama hizmeti 'ni kullanarak IoT Hub 'ınıza sağladınız. Cihaz sağlama hakkında ayrıntılı bilgi edinmek için Azure portalında Cihaz Sağlama Hizmeti ayarları öğreticisine geçin.

> [!div class="nextstepaction"]
> [Azure IoT Hub Cihazı Sağlama Hizmeti öğreticileri](./tutorial-set-up-cloud.md)
