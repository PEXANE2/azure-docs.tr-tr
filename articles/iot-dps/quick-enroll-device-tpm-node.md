---
title: Node. js kullanarak TPM cihazını Azure cihaz sağlama hizmeti 'ne kaydetme
description: Hızlı başlangıç-Node. js hizmeti SDK 'sını kullanarak TPM cihazını Azure IoT Hub cihaz sağlama hizmeti 'ne kaydetme. Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır.
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.devlang: nodejs
ms.custom: mvc
ms.openlocfilehash: 890ad28d99bfc53fa8a3fb40caf0469b31aeee61
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422946"
---
# <a name="quickstart-enroll-tpm-device-to-iot-hub-device-provisioning-service-using-nodejs-service-sdk"></a>Hızlı başlangıç: node. js hizmeti SDK 'sını kullanarak cihaz sağlama hizmeti 'ne IoT Hub TPM cihazı kaydetme

[!INCLUDE [iot-dps-selector-quick-enroll-device-tpm](../../includes/iot-dps-selector-quick-enroll-device-tpm.md)]


Bu adımlar, [Node.js Hizmeti SDK'sını](https://github.com/Azure/azure-iot-sdk-node) ve örnek Node.js uygulamasını kullanarak bir TPM cihazı için Azure IoT Hub Cihaz Sağlama Hizmeti'nde programlı bireysel kayıt oluşturmayı gösterir. İsteğe bağlı olarak bu bireysel kayıt girişini kullanarak sağlama hizmetine sanal bir TPM cihazını da kaydedebilirsiniz. Bu adımlar hem Windows hem de Linux makineler için geçerli olsa da bu makalede Windows dağıtım makinesi kullanılmaktadır.

## <a name="prerequisites"></a>Önkoşullar

- Devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalıyla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun. 
-  Makinenizde [Node.js v4.0 veya üzeri](https://nodejs.org) bir sürümün yüklü olduğundan emin olun.
- Bu hızlı başlangıç sonunda sanal bir cihaz kaydetmek istiyorsanız, cihaz için onay anahtarı aldığınız adıma kadar [sanal cihaz oluşturma ve sağlama](quick-create-simulated-device.md) bölümündeki adımları izleyin. Onay anahtarını aklınızda, daha sonra bu hızlı başlangıçta kullanacaksınız. **Azure portalını kullanarak bireysel kayıt oluşturma adımlarını izlemeyin.**
 
## <a name="create-the-individual-enrollment-sample"></a>Bireysel kayıt örneğini oluşturma 

 
1. Çalışma klasöründeki bir komut penceresinden şu komutu çalıştırın:
  
    ```cmd\sh
    npm install azure-iot-provisioning-service
    ```  

2. Bir metin düzenleyicisi kullanarak çalışma klasörünüzde **create_individual_enrollment.js** adlı bir dosya oluşturun. Dosyaya aşağıdaki kodu ekleyip kaydedin:

    ```
    'use strict';

    var provisioningServiceClient = require('azure-iot-provisioning-service').ProvisioningServiceClient;

    var serviceClient = provisioningServiceClient.fromConnectionString(process.argv[2]);
    var endorsementKey = process.argv[3];

    var enrollment = {
      registrationId: 'first',
      attestation: {
        type: 'tpm',
        tpm: {
          endorsementKey: endorsementKey
        }
      }
    };

    serviceClient.createOrUpdateIndividualEnrollment(enrollment, function(err, enrollmentResponse) {
      if (err) {
        console.log('error creating the individual enrollment: ' + err);
      } else {
        console.log("enrollment record returned: " + JSON.stringify(enrollmentResponse, null, 2));
      }
    });
    ```

## <a name="run-the-individual-enrollment-sample"></a>Bireysel kayıt örneğini çalıştırma
  
1. Örneği çalıştırmak için sağlama hizmetinizin bağlantı dizesine ihtiyacınız vardır. 
    1. Azure portal oturum açın, sol taraftaki menüden **tüm kaynaklar** düğmesini seçin ve cihaz sağlama hizmetinizi açın. 
    2. **Paylaşılan erişim ilkeleri**' ni seçin ve ardından özelliklerini açmak için kullanmak istediğiniz erişim ilkesini seçin. **Erişim İlkesi** penceresinde birincil anahtar bağlantı dizesini kopyalayın ve not edin. 

       ![Portaldan sağlama hizmeti bağlantı dizesini alma](./media/quick-enroll-device-tpm-node/get-service-connection-string.png) 


2. Ayrıca cihazınızın onay anahtarını da almanız gerekir. [Sanal cihaz oluşturma ve sağlama](quick-create-simulated-device.md) hızlı başlangıcını izleyerek sanal bir TPM cihazı oluşturduysanız bu cihaz için oluşturulan anahtarı kullanın. Bunu yapmadıysanız örnek bireysel kaydı oluşturmak için SDK ile birlikte verilen aşağıdaki onay anahtarını kullanabilirsiniz:

    ```
    AToAAQALAAMAsgAgg3GXZ0SEs/gakMyNRqXXJP1S124GUgtk8qHaGzMUaaoABgCAAEMAEAgAAAAAAAEAxsj2gUScTk1UjuioeTlfGYZrrimExB+bScH75adUMRIi2UOMxG1kw4y+9RW/IVoMl4e620VxZad0ARX2gUqVjYO7KPVt3dyKhZS3dkcvfBisBhP1XH9B33VqHG9SHnbnQXdBUaCgKAfxome8UmBKfe+naTsE5fkvjb/do3/dD6l4sGBwFCnKRdln4XpM03zLpoHFao8zOwt8l/uP3qUIxmCYv9A7m69Ms+5/pCkTu/rK4mRDsfhZ0QLfbzVI6zQFOKF/rwsfBtFeWlWtcuJMKlXdD8TXWElTzgh7JS4qhFzreL0c1mI0GCj+Aws0usZh7dLIVPnlgZcBhgy1SSDQMQ==
    ```

3. TPM cihazınız için bireysel kayıt oluşturmak üzere aşağıdaki komutu çalıştırın (komut bağımsız değişkenlerinin tırnak işaretlerini kaldırmayın):
 
     ```cmd\sh
     node create_individual_enrollment.js "<the connection string for your provisioning service>" "<endorsement key>"
     ```
 
3. Oluşturma başarılı olursa komut penceresinde yeni bireysel kaydın özellikleri görüntülenir.

    ![Komut çıkışındaki kayıt özellikleri](./media/quick-enroll-device-tpm-node/output.png) 

4. Bireysel kaydın oluşturulduğunu doğrulayın. Azure portalının Cihaz Sağlama Hizmeti özet dikey penceresinde, **Kayıtları yönetme**'yi seçin. **Bireysel** kayıtlar sekmesini seçin ve giriş için onay anahtarını ve diğer özellikleri doğrulamak üzere yeni kayıt girişini (*ilk*) seçin.

    ![Portaldaki kayıt özellikleri](./media/quick-enroll-device-tpm-node/verify-enrollment-portal.png) 
 
Bir TPM cihazı için bireysel kayıt oluşturdunuz, sanal cihaz kaydetmek istiyorsanız [Sanal cihaz oluşturma ve sağlama](quick-create-simulated-device.md) bölümündeki adımlardan devam edebilirsiniz. Bu hızlı başlangıçta Azure portal kullanarak bireysel kayıt oluşturma adımlarını attığınızdan emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Node. js hizmet örneklerini keşfetmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Makinenizdeki Node.js örnek çıktı penceresini kapatın.
1. Sanal TPM cihazı oluşturduysanız, TPM simülatörü penceresini kapatın.
2. Azure portal cihaz sağlama hizmetine gidin, kayıtları **Yönet**' i seçin ve sonra **bireysel** kayıtlar sekmesini seçin. bu hızlı başlangıç Ile oluşturduğunuz kayıt girişinin *kayıt kimliği* ' nin yanındaki onay kutusunu işaretleyin ve bölmenin en üstündeki **Sil** düğmesine basın. 
 
## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, bir TPM aygıtı için program aracılığıyla tek bir kayıt girişi oluşturdunuz ve isteğe bağlı olarak, makinenizde bir TPM sanal cihazı oluşturdunuz ve Azure IoT Hub cihaz sağlama hizmeti 'ni kullanarak IoT Hub 'ınıza sağladınız. Cihaz sağlama hakkında ayrıntılı bilgi edinmek için Azure portalında Cihaz Sağlama Hizmeti ayarları öğreticisine geçin. 
 
> [!div class="nextstepaction"]
> [Azure IoT Hub Cihazı Sağlama Hizmeti öğreticileri](./tutorial-set-up-cloud.md)