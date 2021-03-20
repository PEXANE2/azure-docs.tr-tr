---
title: Hızlı başlangıç-Azure IoT Hub Node.js kullanarak bir cihaz sağlamak için simetrik anahtar kullanın
description: Bu hızlı başlangıçta, bir IoT Hub 'ına simetrik anahtar cihazı sağlamak için cihaz sağlama hizmeti (DPS) ile birlikte Node.js için Azure IoT SDK 'sını kullanacaksınız
author: wesmc7777
ms.author: wesmc
ms.date: 10/21/2020
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: eliotgra
ms.custom: mvc
ms.openlocfilehash: 78005ba46952bcf05b19c7627feecb1ec30ac651
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92429378"
---
# <a name="quickstart-provision-a-symmetric-key-device-using-nodejs"></a>Hızlı başlangıç: Node.js kullanarak bir simetrik anahtar cihazı sağlama

Bu hızlı başlangıçta, Node.js kullanarak bir IoT Hub 'ına cihaz olarak Windows geliştirme makinesi sağlamayı öğreneceksiniz. Bu cihaz, bir IoT Hub 'ına atanmak üzere bir cihaz sağlama hizmeti (DPS) örneğiyle kimlik doğrulaması yapmak için bir simetrik anahtar ve tek bir kayıt kullanır. [Node.jsIçin Azure ıOT SDK 'sının](https://github.com/Azure/azure-iot-sdk-node.git) örnek kodu, cihazı sağlamak için kullanılacaktır. 

Bu makalede tek bir kayıt ile sağlama işlemi gösterilmektedir, ancak kayıt grupları da kullanabilirsiniz. Kayıt grupları kullanılırken bazı farklılıklar vardır. Örneğin, cihaz için benzersiz bir kayıt KIMLIĞIYLE türetilmiş bir cihaz anahtarı kullanmanız gerekir. [Simetrik anahtarlarla cihaz sağlama](how-to-legacy-device-symm-key.md) bir kayıt grubu örneği sağlar. Kayıt grupları hakkında daha fazla bilgi için bkz. [simetrik anahtar kanıtlama Için Grup](concepts-symmetric-key-attestation.md#group-enrollments)kayıtları.

Otomatik sağlama işlemini bilmiyorsanız, [sağlamaya](about-iot-dps.md#provisioning-process) genel bakış konusunu gözden geçirin. 

Ayrıca, bu hızlı başlangıçla devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun. Bu hızlı başlangıç, Cihaz Sağlama Hizmeti örneğinizi zaten oluşturmuş olmanızı gerektirir.

Bu makale Windows tabanlı bir iş istasyonuna yöneliktir. Ancak yordamları Linux üzerinde gerçekleştirebilirsiniz. Linux örneği için bkz. [çok kiracılı sağlama](how-to-provision-multitenant.md).


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Önkoşullar

- [Sağlama](about-iot-dps.md#provisioning-process) kavramları hakkında bilgi edinin.
- [Azure Portal Ile cihaz sağlama hizmeti IoT Hub ayarlama](./quick-setup-auto-provision.md)işlemi tamamlandı.
- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Node.js v 4.0 +](https://nodejs.org).
- [Git](https://git-scm.com/download/).


## <a name="create-a-device-enrollment"></a>Cihaz kaydı oluşturma

1. [Azure Portal](https://portal.azure.com)oturum açın, sol taraftaki menüden **tüm kaynaklar** düğmesini seçin ve CIHAZ sağlama hizmeti (DPS) örneğinizi açın.

2. Kayıtları **Yönet** sekmesini seçin ve ardından üst kısımdaki **tek kayıt Ekle** düğmesini seçin. 

3. **Kayıt Ekle** panelinde, aşağıdaki bilgileri girin ve **Kaydet** düğmesine basın.

   - **Mekanizma**: Kimlik onay *Mekanizması* olarak **Simetrik Anahtar** seçeneğini belirleyin.

   - **Anahtarları otomatik oluştur**: Bu kutuyu işaretleyin.

   - **Kayıt Kimliği**: Kaydı tanımlamak için bir kayıt kimliği girin. Yalnızca küçük alfasayısal karakterler ile kısa çizgi ('-') karakterlerini kullanın. Örneğin, **SYMM-anahtar-NodeJS-Device-01**.

   - **IoT Hub Cihaz Kimliği:** Bir cihaz tanımlayıcısı girin. Örneğin, **NodeJS-Device-01**.

     ![Portalda simetrik anahtar kanıtı için bireysel kayıt ekleme](./media/quick-create-device-symmetric-key-node/create-individual-enrollment-node.png)

4. Kaydınız kaydedildikten sonra, **birincil anahtar** ve **İkincil anahtar** oluşturulur ve kayıt girişine eklenir. Simetrik anahtar cihaz kaydınız, *bireysel* kayıtlar SEKMESINDEKI *kayıt kimliği* sütununun altında **SYMM-Key-NodeJS-Device-01** olarak görünür. 

5. Kaydı açın ve oluşturduğunuz **Birincil Anahtar** değerini kopyalayın. Bu anahtar değerini ve **kayıt kimliğini** daha sonra, cihaz sağlama örnek kodu ile kullanmak üzere ortam değişkenleri eklediğinizde kullanacaksınız.



## <a name="prepare-the-nodejs-environment"></a>Node.js ortamını hazırlama 

1. Git CMD veya git Bash komut satırı ortamını açın. Aşağıdaki komutu kullanarak Node.jsGitHub deposu [Için Azure ıOT SDK ](https://github.com/Azure/azure-iot-sdk-node.git) 'yı kopyalayın:

    ```cmd
    git clone https://github.com/Azure/azure-iot-sdk-node.git --recursive
    ```


<a id="firstbootsequence"></a>

## <a name="prepare-the-device-provisioning-code"></a>Cihaz sağlama kodunu hazırlama

Bu bölümde, simetrik anahtar cihazınızı sağlamak üzere cihaz sağlama örnek kodu için parametre olarak kullanılacak aşağıdaki dört ortam değişkenini ekleyeceksiniz. 

* `PROVISIONING_HOST`
* `PROVISIONING_IDSCOPE`
* `PROVISIONING_REGISTRATION_ID`
* `PROVISIONING_SYMMETRIC_KEY`

Sağlama kodu, cihazınızın kimliğini doğrulamak için bu değişkenlere göre DPS örneğiyle iletişim kuracaktır. Daha sonra cihaz, bireysel kayıt yapılandırmasına göre zaten DPS örneğine bağlanmış olan bir IoT Hub 'ına atanır. Sağlandıktan sonra örnek kod, IoT Hub 'ına bazı test telemetrisi gönderir.

1. [Azure Portal](https://portal.azure.com), cihaz sağlama hizmeti menünüzde **genel bakış** ' ı seçin ve _hizmet uç_ noktanızı ve _kimlik kapsamınızı_ kopyalayın. `PROVISIONING_HOST`Ve ortam değişkenleri için bu değerleri kullanacaksınız `PROVISIONING_IDSCOPE` .

    ![Hizmet bilgileri](./media/quick-create-device-symmetric-key-node/extract-dps-endpoints.png)

2. Node.js komutları yürütmek için bir komut istemi açın ve aşağıdaki *sağlama/cihaz/örnekler* dizinine gidin.

    ```cmd
    cd azure-iot-sdk-node/provisioning/device/samples
    ```

3. *Sağlama/cihaz/örnekler* klasöründe *register_symkey.js* açın ve kodu gözden geçirin. 

    Örnek kodun özel bir yük ayarladığına dikkat edin...

    ```nodejs
    provisioningClient.setProvisioningPayload({a: 'b'});
    ```

    Bu hızlı başlangıç için bu kod gerekli değildir. Bu kod, cihazınızı bir IoT Hub atamak için özel bir ayırma işlevi kullanmak istiyorsanız özel yük ayarlamaya yönelik bir örnektir. Daha fazla bilgi için bkz. [öğretici: özel ayırma Ilkeleri kullanma](tutorial-custom-allocation-policies.md).

    `provisioningClient.register()`Yöntemi, cihazınızın kaydını dener.

    Cihazınızı kaydetmek için örnek koda hiçbir değişiklik yapılması gerekmez.

4. Komut isteminiz için, önceki bölümde bireysel kayıttan kopyaladığınız sağlama Konağı, KIMLIK kapsamı, kayıt KIMLIĞI ve birincil simetrik anahtar için ortam değişkenlerini ekleyin.  

    Aşağıdaki komutlar komut sözdizimini göstermek için örnektir. Doğru değerlerinizi kullandığınızdan emin olun.

    ```console
    set PROVISIONING_HOST=test-dps-docs.azure-devices-provisioning.net
    ```

    ```console
    set PROVISIONING_IDSCOPE=0ne00000A0A
    ```

    ```console
    set PROVISIONING_REGISTRATION_ID=symm-key-nodejs-device-01
    ```

    ```console
    set PROVISIONING_SYMMETRIC_KEY=sbDDeEzRuEuGKag+kQKV+T1QGakRtHpsERLP0yPjwR93TrpEgEh/Y07CXstfha6dhIPWvdD1nRxK5T0KGKA+nQ==
    ```


4. Aşağıdaki komutları kullanarak örnek kodu derleyin ve çalıştırın.

    ```console
    npm install
    ```

    ```console
    node register_symkey.js
    ```

5. Beklenen çıkış, cihazın ayrı kayıt ayarlarına bağlı olarak atandığı bağlantılı IoT Hub 'ını gösteren aşağıdakine benzer olmalıdır. Bir "Merhaba Dünya" dizesi, bir sınama iletisi olarak hub 'a gönderilir:

    ```output
    D:\Docs\test\azure-iot-sdk-node\provisioning\device\samples>node register_symkey.js
    registration succeeded
    assigned hub=docs-test-iot-hub.azure-devices.net
    deviceId=nodejs-device-01
    payload=undefined
    Client connected
    send status: MessageEnqueued    
    ```
    
6. Azure portal, sağlama hizmetinize bağlı olan IoT Hub 'ına gidin ve **IoT cihazları** dikey penceresini açın. Simetrik anahtar cihazı hub 'a başarıyla sağlamaktan sonra cihaz KIMLIĞI, **etkin** olarak *durumuyla* gösterilir. Cihaz örnek kodunu çalıştırmadan önce dikey pencereyi zaten açtıysanız, en üstteki **Yenile** düğmesine basmanız gerekebilir. 

    ![Cihaz IOT hub'da kayıtlı](./media/quick-create-device-symmetric-key-node/hub-registration-node.png) 

> [!NOTE]
> Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için bkz. [IoT Hub cihaz TWINS 'ı anlama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md).
>


## <a name="clean-up-resources"></a>Kaynakları temizleme

Cihaz istemci örneğini üzerinde çalışmaya ve keşfetmeye devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından cihaz sağlama hizmetinizi seçin. Hizmetiniz için kayıtları **Yönet** ' i açın ve **bireysel** kayıtlar sekmesini seçin. Bu hızlı başlangıçta kaydettiğiniz cihazın *kayıt kimliği* ' nin yanındaki onay kutusunu işaretleyin ve bölmenin en üstündeki **Sil** düğmesine basın. 
1. Azure portal sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından IoT Hub 'ınızı seçin. , Hub 'ınız için **IoT cihazlarını** açın, bu hızlı başlangıçta kaydettiğiniz CIHAZıN *cihaz kimliği* ' nin yanındaki onay kutusunu işaretleyin ve ardından bölmenin en üstündeki **Sil** düğmesine basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, IoT Hub cihaz sağlama hizmetini kullanarak IoT Hub 'ınıza Windows tabanlı bir simetrik anahtar cihazı sağlamış olursunuz. Node.js kullanarak X. 509.440 sertifika cihazlarını sağlamayı öğrenmek için, X. 509.440 cihazları için aşağıdaki hızlı başlangıç ile devam edin. 

> [!div class="nextstepaction"]
> [Azure hızlı başlangıç-DPS ve Node.jskullanarak X. 509.440 cihazları sağlama ](quick-create-simulated-device-x509-node.md)
