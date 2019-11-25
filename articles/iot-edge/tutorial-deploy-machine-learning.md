---
title: 'Tutorial: Deploy Azure Machine Learning to a device - Azure IoT Edge'
description: In this tutorial, you create an Azure Machine Learning model, then deploy it as a module to an edge device
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/11/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 3f1bd4ce5b701652318679f3277bc7c9109fa529
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74457627"
---
# <a name="tutorial-deploy-azure-machine-learning-as-an-iot-edge-module-preview"></a>Öğretici: Azure Machine Learning'i bir IoT Edge modülü olarak dağıtma (önizleme)

Use Azure Notebooks to develop a machine learning module and deploy it to a Linux device running Azure IoT Edge. 

İş mantığınızı uygulayan kodu doğrudan IoT Edge cihazlarınıza dağıtmak için IoT Edge modüllerini kullanabilirsiniz. Bu öğreticide simülasyon makinesi sıcaklık verilerini temel alarak bir cihazın arızalanacağı zamanı tahmin eden bir Azure Machine Learning modülünü dağıtma adımları açıklanmaktadır. For more information about Azure Machine Learning on IoT Edge, see [Azure Machine Learning documentation](../machine-learning/service/how-to-deploy-to-iot.md).

Bu öğreticide oluşturduğunuz Azure Machine Learning modülü, cihazınızın ürettiği ortam verilerini okur ve iletileri normal veya anormal olarak etiketler.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir Azure Machine Learning modülü oluşturma
> * Azure kapsayıcı kayıt defterine bir modülü kapsayıcısı gönderme
> * IoT Edge cihazınıza bir Azure Machine Learning modülü dağıtma
> * Oluşturulan verileri görüntüleme

>[!NOTE]
>Azure IoT Edge üzerindeki Azure Machine Learning modülleri genel önizleme sürümündedir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

* You can use an Azure virtual machine as an IoT Edge device by following the steps in the quickstart for [Linux](quickstart-linux.md).
* The Azure Machine Learning module doesn't support Windows containers.
* The Azure Machine Learning module doesn't support ARM processors.

Bulut kaynakları:

* Azure'da ücretsiz veya standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).
* Azure Machine Learning çalışma alanı. Follow the instructions in [Use the Azure portal to get started with Azure Machine Learning](../machine-learning/service/quickstart-get-started.md) to create one and learn how to use it.
   * Make a note of the workspace name, resource group, and subscription ID. These values are all available on the workspace overview in the Azure portal. You'll use these values later in the tutorial to connect an Azure notebook to your workspace resources. 


## <a name="create-and-deploy-azure-machine-learning-module"></a>Create and deploy Azure Machine Learning module

In this section, you convert trained machine learning model files and into an Azure Machine Learning container. Docker görüntüsü için gerekli tüm bileşenler [Azure IoT Edge için AI Toolkit deposunda](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial) mevcuttur. Follow these steps to upload that repository into Microsoft Azure Notebooks to create the container and push it to Azure Container Registry.


1. Navigate to your Azure Notebooks projects. You can get there from your Azure Machine Learning workspace in the [Azure portal](https://portal.azure.com) or by signing in to [Microsoft Azure Notebooks](https://notebooks.azure.com/home/projects) with your Azure account.

2. Select **Upload GitHub Repo**.

3. Provide the following GitHub repository name: `Azure/ai-toolkit-iot-edge`. Uncheck the **Public** box if you want to keep your project private. Select **Import**. 

4. Once the import is finished, navigate into the new **ai-toolkit-iot-edge** project and open the **IoT Edge anomaly detection tutorial** folder. 

5. Verify that your project is running. If not, select **Run on Free Compute**.

   ![Run on free compute](./media/tutorial-deploy-machine-learning/run-on-free-compute.png)

6. Open the **aml_config/config.json** file.

7. Edit the config file to include the values for your Azure subscription ID, a resource group in your subscription, and your Azure Machine Learning workspace name. You can get all these values from the **Overview** section of your workspace in Azure. 

8. Save the config file.

9. Open the **00-anomaly-detection-tutorial.ipynb** file.

10. When prompted, select the **Python 3.6** kernel then select **Set Kernel**.

11. Edit the first cell in the notebook according to the instructions in the comments. Use the same resource group, subscription ID, and workspace name that you added to the config file.

12. Run the cells in the notebook by selecting them and selecting **Run** or pressing `Shift + Enter`.

    >[!TIP]
    >Some of the cells in the anomaly detection tutorial notebook are optional, because they create resources that some users may or may not have yet, like an IoT Hub. If you put your existing resource information in the first cell, you'll receive errors if you run the cells that create new resources because Azure won't create duplicate resources. This is fine, and you can ignore the errors or skip those optional sections entirely. 

By completing all the steps in the notebook, you trained an anomaly detection model, built it as a Docker container image, and pushed that image to Azure Container Registry. Then, you tested the model and finally deployed it to your IoT Edge device. 

## <a name="view-container-repository"></a>View container repository

Check that your container image was successfully created and stored in the Azure container registry associated with your machine learning environment. The notebook that you used in the previous section automatically provided the container image and the registry credentials to your IoT Edge device, but you should know where they're stored so that you can find the information yourself later. 

1. In the [Azure portal](https://portal.azure.com), navigate to your Machine Learning service workspace. 

2. The **Overview** section lists the workspace details as well its associated resources. Select the **Registry** value, which should be your workspace name followed by random numbers. 

3. In the container registry, select **Repositories**. You should see a repository called **tempanomalydetection** that was created by the notebook you ran in the earlier section. 

4. Select **tempanomalydetection**. You should see that the repository has one tag: **1**. 

   Now that you know the registry name, repository name, and tag, you know the full image path of the container. Image paths look like **\<registry_name\>.azurecr.io/tempanomalydetection:1**. You can use the image path to deploy this container to IoT Edge devices. 

5. In the container registry, select **Access keys**. You should see a number of access credentials, including **Login server** and the **Username**, and **Password** for an admin user.

   These credentials can be included in the deployment manifest to give your IoT Edge device access to pull container images from the registry. 

Now you know where the Machine Learning container image is stored. The next section walks through steps to view the container running as a module on your IoT Edge device. 

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Her bir IoT Edge modülü tarafından oluşturulan ve IoT hub'ınıza gönderilen iletileri görüntüleyebilirsiniz.

### <a name="view-data-on-your-iot-edge-device"></a>IoT Edge cihazınızdaki verileri görüntüleme

IoT Edge cihazınızda her bir modülden gönderilen iletileri görüntüleyebilirsiniz.

You may need to use `sudo` for elevated permissions to run `iotedge` commands. Signing out and signing back in to your device automatically updates your permissions.

1. IoT Edge cihazınızda tüm modülleri görüntüleyin.

   ```cmd/sh
   iotedge list
   ```

2. Belirli bir cihazdan gönderilen iletileri görüntüleyin. Önceki komutun çıktısında yer alan modül adını kullanın.

   ```cmd/sh
   iotedge logs <module_name> -f
   ```

### <a name="view-data-arriving-at-your-iot-hub"></a>IoT hub'ınıza ulaşan verileri görüntüleme

You can view the device-to-cloud messages that your IoT hub receives by using the [Azure IoT Hub Toolkit extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (formerly Azure IoT Toolkit extension).

Aşağıdaki adımlar, IoT hub'ınıza ulaşan cihazdan buluta iletileri izlemek için yapmanız gereken Visual Studio Code ayarlarını göstermektedir.

1. Visual Studio Code'da **IoT Hub Cihazları**'nı seçin.

2. Menüden **...** öğesini, sonra **IoT Hub Bağlantı Dizesini Ayarla**'yı seçin.

   ![Set IoT Hub Connection String](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Sayfanın üstünde açılan metin kutusuna IoT Hub'ınız için iothubowner bağlantı dizesini girin. IoT Edge cihazınız IoT Hub Cihazları listesinde görünmelidir.

4. Select **...** again then select **Start Monitoring Built-in Event Endpoint**.

5. tempSensor her beş saniyede bir gelen iletileri gözlemleyin. The message body contains a property called **anomaly**, which the machinelearningmodule provides with a true or false value. Model başarıyla çalıştıysa, **AzureMLResponse** özelliği "OK" değerini içerir.

   ![Azure Machine Learning response in message body](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki önerilen makaleye geçmeyi planlıyorsanız, oluşturduğunuz kaynaklarla yapılandırmaları tutabilir ve yeniden kullanabilirsiniz. Aynı IoT Edge cihazını test cihazı olarak kullanmaya devam edebilirsiniz.

Geçmeyecekseniz ücret kesilmesini önlemek için yerel yapılandırmalarınızı ve bu makalede oluşturulan Azure kaynaklarını silebilirsiniz.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Machine Learning ile çalışan bir IoT Edge modülü dağıttınız. Azure IoT Edge'in verileri iş içgörüsüne çevirmenize yardımcı olabilecek diğer yollar hakkında bilgi edinmek için diğer öğreticilere geçebilirsiniz.

> [!div class="nextstepaction"]
> [Özel Görüntü İşleme hizmetiyle görüntüleri sınıflandırma](tutorial-deploy-custom-vision.md)

