---
title: Enroll TPM device to Azure Device Provisioning Service using Node.js
description: Quickstart - Enroll TPM device to Azure IoT Hub Device Provisioning Service using Node.js service SDK. Bu hızlı başlangıçta bireysel kayıtlar kullanılmaktadır.
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
# <a name="quickstart-enroll-tpm-device-to-iot-hub-device-provisioning-service-using-nodejs-service-sdk"></a>Quickstart: Enroll TPM device to IoT Hub Device Provisioning Service using Node.js service SDK

[!INCLUDE [iot-dps-selector-quick-enroll-device-tpm](../../includes/iot-dps-selector-quick-enroll-device-tpm.md)]


Bu adımlar, [Node.js Hizmeti SDK'sını](https://github.com/Azure/azure-iot-sdk-node) ve örnek Node.js uygulamasını kullanarak bir TPM cihazı için Azure IoT Hub Cihaz Sağlama Hizmeti'nde programlı bireysel kayıt oluşturmayı gösterir. İsteğe bağlı olarak bu bireysel kayıt girişini kullanarak sağlama hizmetine sanal bir TPM cihazını da kaydedebilirsiniz. Bu adımlar hem Windows hem de Linux makineler için geçerli olsa da bu makalede Windows dağıtım makinesi kullanılmaktadır.

## <a name="prerequisites"></a>Önkoşullar

- Devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalıyla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun. 
-  Makinenizde [Node.js v4.0 veya üzeri](https://nodejs.org) bir sürümün yüklü olduğundan emin olun.
- If you want to enroll a simulated device at the end of this quickstart, follow the steps in [Create and provision a simulated device](quick-create-simulated-device.md) up until the step where you get an endorsement key for the device. Note down the endorsement key, you will use it later in this quickstart. **Azure portalını kullanarak bireysel kayıt oluşturma adımlarını izlemeyin.**
 
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
    1. Sign in to the Azure portal, select the **All resources** button on the left-hand menu and open your Device Provisioning service. 
    2. Select **Shared access policies**, then select the access policy you want to use to open its properties. **Erişim İlkesi** penceresinde birincil anahtar bağlantı dizesini kopyalayın ve not edin. 

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

4. Bireysel kaydın oluşturulduğunu doğrulayın. Azure portalının Cihaz Sağlama Hizmeti özet dikey penceresinde, **Kayıtları yönetme**'yi seçin. Select the **Individual Enrollments** tab and select the new enrollment entry (*first*) to verify the endorsement key and other properties for the entry.

    ![Portaldaki kayıt özellikleri](./media/quick-enroll-device-tpm-node/verify-enrollment-portal.png) 
 
Bir TPM cihazı için bireysel kayıt oluşturdunuz, sanal cihaz kaydetmek istiyorsanız [Sanal cihaz oluşturma ve sağlama](quick-create-simulated-device.md) bölümündeki adımlardan devam edebilirsiniz. Be sure to skip the steps to create an individual enrollment using the Azure portal in that quickstart.

## <a name="clean-up-resources"></a>Kaynakları temizleme
If you plan to explore the Node.js service samples, do not clean up the resources created in this quickstart. If you do not plan to continue, use the following steps to delete all resources created by this quickstart.

1. Makinenizdeki Node.js örnek çıktı penceresini kapatın.
1. Sanal TPM cihazı oluşturduysanız, TPM simülatörü penceresini kapatın.
2. Navigate to your Device Provisioning service in the Azure portal, select **Manage enrollments**, and then select the **Individual Enrollments** tab. Select the check box next to the *Registration ID* for the enrollment entry you created using this quickstart, and press the **Delete** button at the top of the pane. 
 
## <a name="next-steps"></a>Sonraki adımlar
In this quickstart, you’ve programmatically created an individual enrollment entry for a TPM device, and, optionally, created a TPM simulated device on your machine and provisioned it to your IoT hub using the Azure IoT Hub Device Provisioning Service. Cihaz sağlama hakkında ayrıntılı bilgi edinmek için Azure portalında Cihaz Sağlama Hizmeti ayarları öğreticisine geçin. 
 
> [!div class="nextstepaction"]
> [Azure IoT Hub Cihazı Sağlama Hizmeti öğreticileri](./tutorial-set-up-cloud.md)