---
title: Azure Kaynak Yöneticisi şablonu kullanarak Azure IoT Hub Aygıt Sağlama'yı ayarlama
description: Azure quickstart - Bir şablon kullanarak Azure IoT Hub Aygıt Sağlama Hizmeti'ni (DPS) ayarlama
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: 482401b75cadf44e2cef03cced8dd216d0980524
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74969591"
---
# <a name="quickstart-set-up-the-iot-hub-device-provisioning-service-with-an-azure-resource-manager-template"></a>Hızlı başlatma: Azure Kaynak Yöneticisi şablonuyla IoT Hub Aygıt Sağlama Hizmetini ayarlama

Cihazlarınızın sağlanması için gerekli Azure bulut kaynaklarını programlı olarak ayarlamak için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)’ı kullanabilirsiniz. Bu adımlar, bir IoT hub'ı ve yeni bir IoT Hub Aygıt Sağlama Hizmetinin nasıl oluşturulabileceğinizi ve azure kaynak yöneticisi şablonu kullanarak iki hizmeti birbirine nasıl bağlayacaklarını gösterir. Bu hızlı başlatma, bir kaynak grubu oluşturmak ve şablonu dağıtmak için gereken programlı adımları gerçekleştirmek için [Azure CLI'yi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli) kullanır, ancak bu adımları gerçekleştirmek ve şablonunuzu dağıtmak için [Azure portalı](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal), [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy), .NET, Ruby veya diğer programlama dillerini kolayca kullanabilirsiniz. 


## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.
- Bu hızlı başlatma, Azure CLI'yi yerel olarak çalıştırmanızı gerektirir. Azure CLI 2.0 veya sonraki bir sürümünü yüklemiş olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).


## <a name="sign-in-to-azure-and-create-a-resource-group"></a>Azure’da oturum açma ve kaynak grubu oluşturma

Azure hesabınızda oturum açın ve aboneliğinizi seçin.

1. Komut isteminde [oturum açma komutunu][lnk-login-command] çalıştırın:
    
    ```azurecli
    az login
    ```

    Kodu kullanarak kimlik doğrulaması gerçekleştirmek için yönergeleri uygulayın ve bir web tarayıcısı üzerinden Azure hesabınızda oturum açın.

2. Birden fazla Azure aboneliğiniz varsa Azure’da oturum açtığınızda, kimlik bilgilerinizle ilişkili tüm Azure hesaplarınıza erişim izni elde edersiniz. Kullanabileceğiniz [Azure hesaplarını listelemek için aşağıdaki komutu][lnk-az-account-command] kullanın:
    
    ```azurecli
    az account list 
    ```

    IoT hub’ınızı oluşturmak için komutları çalıştırmak amacıyla kullanmak istediğiniz aboneliği seçmek üzere aşağıdaki komutu kullanın. Önceki komutun çıkışında yer alan abonelik adını veya kimliği kullanabilirsiniz:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

3. IoT hub’ları ve sağlama hizmetleri gibi Azure bulut kaynaklarını bir kaynak grubunda oluşturursunuz. Mevcut bir kaynak grubunu kullanın veya [kaynak grubu oluşturmak için aşağıdaki komutu][lnk-az-resource-command] çalıştırabilirsiniz:
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > Önceki örnekte kaynak grubu Batı ABD konumunda oluşturulur. `az account list-locations -o table` komutunu çalıştırarak kullanılabilir konumların listesini görüntüleyebilirsiniz.
    >
    >

## <a name="create-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu oluşturma

Kaynak grubunuzda sağlama hizmeti ve bağlı bir IoT hub’ı oluşturmak için bir JSON şablonunu kullanın. Mevcut bir sağlama hizmetinde veya IoT hub’ında değişiklik yapmak için bir Azure Resource Manager şablonu da kullanabilirsiniz.

1. Aşağıdaki çatı içeriğiyle **template.json** adlı bir Azure Resource Manager şablonu oluşturmak için bir metin düzenleyici kullanın. 

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {},
       "variables": {},
       "resources": []
   }
   ```

2. **parameters** bölümünü aşağıdaki içerikle değiştirin. Parametreler bölümü, değerleri başka bir dosyadan geçirilebilen parametreleri tanımlar. Bu bölümde, oluşturulacak IoT hub'ının adı ve sağlama hizmeti tanımlanır. Ayrıca, hem IoT hub'ı hem de sağlama hizmetinin konumunu tanımlar. Değerler, IoT hub'larını ve sağlama hizmetlerini destekleyen Azure bölgeleriyle sınırlandırılacaktır. Cihaz Sağlama Hizmeti için desteklenen konumların listesi için aşağıdaki `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table` komutunu çalıştırabilir veya [Azure Durum](https://azure.microsoft.com/status/) sayfasına gidip "Cihaz Sağlama Hizmeti" araması yapabilirsiniz.

   ```json
    "parameters": {
        "iotHubName": {
            "type": "string"
        },
        "provisioningServiceName": {
            "type": "string"
        },
        "hubLocation": {
            "type": "string",
            "allowedValues": [
                "eastus",
                "westus",
                "westeurope",
                "northeurope",
                "southeastasia",
                "eastasia"
            ]
        }
    },

   ```

3. **variables** bölümünü aşağıdaki içerikle değiştirin. Bu bölüm daha sonra sağlama hizmetinin ve IoT hub’ının bağlanması için gereken IoT hub bağlantı dizesinin oluşturulması için kullanılan değerleri belirtir. 
 
   ```json
    "variables": {        
        "iotHubResourceId": "[resourceId('Microsoft.Devices/Iothubs', parameters('iotHubName'))]",
        "iotHubKeyName": "iothubowner",
        "iotHubKeyResource": "[resourceId('Microsoft.Devices/Iothubs/Iothubkeys', parameters('iotHubName'), variables('iotHubKeyName'))]"
    },

   ```

4. Bir IoT hub’ı oluşturmak için **kaynaklar** koleksiyonuna aşağıdaki satırları ekleyin. JSON, bir IoT hub'ı oluşturmak için gereken minimum özellikleri belirtir. **Ad** ve **konum** değerleri başka bir dosyadan parametre olarak geçirilir. Şablondaki bir IoT hub'ı için belirtebileceğiniz özellikler hakkında daha fazla bilgi edinmek için [Microsoft.Devices/IotHubs şablon başvurusuna](https://docs.microsoft.com/azure/templates/microsoft.devices/iothubs)bakın.

   ```json
        {
            "apiVersion": "2017-07-01",
            "type": "Microsoft.Devices/IotHubs",
            "name": "[parameters('iotHubName')]",
            "location": "[parameters('hubLocation')]",
            "sku": {
                "name": "S1",
                "capacity": 1
            },
            "tags": {
            },
            "properties": {
            }            
        },

   ``` 

5. Sağlama hizmetini oluşturmak için **kaynaklar** koleksiyonundaki IoT hub belirtiminden sonra aşağıdaki satırları ekleyin. Sağlama hizmetinin **adı** ve **konumu** parametre olarak geçirilecektir. **iotHubs** koleksiyonu, ioT hub'larını sağlama hizmetine bağlamak için belirtir. Her bağlı IoT hub’ı için en azından **connectionString** ve **location** özelliklerini belirtmeniz gerekir. Ayrıca her IoT hub’ında **allocationWeight** ve **applyAllocationPolicy** gibi özelliklerin yanı sıra sağlama hizmetinin kendisi üzerinde **allocationPolicy** ve **authorizationPolicies** gibi özellikler de ayarlayabilirsiniz. Daha fazla bilgi edinmek için bkz. [Microsoft.Devices/provisioningServices şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.devices/provisioningservices).

   **dependsOn** özelliği, Kaynak Yöneticisi’nin sağlama hizmetini oluşturmadan önce IoT hub’ını oluşturmasını sağlamak için kullanılır. Şablon, önce hub’ın ve hub’a ait anahtarların oluşturulması için IoT hub’ının bağlantı dizesinin sağlama hizmetiyle bağlantısını belirtmesini gerektirir. Şablon, parametreli değişkenlerden bağlantı dizesini oluşturmak için **concat** ve **listKeys** gibi işlevleri kullanır. Daha fazla bilgi edinmek için bkz. [Azure Resource Manager şablon işlevleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions).

   ```json
        {
            "type": "Microsoft.Devices/provisioningServices",
            "sku": {
                "name": "S1",
                "capacity": 1
            },
            "name": "[parameters('provisioningServiceName')]",
            "apiVersion": "2017-11-15",
            "location": "[parameters('hubLocation')]",
            "tags": {},
            "properties": {
                "iotHubs": [
                    {
                        "connectionString": "[concat('HostName=', reference(variables('iotHubResourceId')).hostName, ';SharedAccessKeyName=', variables('iotHubKeyName'), ';SharedAccessKey=', listkeys(variables('iotHubKeyResource'), '2017-07-01').primaryKey)]",
                        "location": "[parameters('hubLocation')]",
                        "name": "[concat(parameters('iotHubName'),'.azure-devices.net')]"
                    }
                ]
            },
            "dependsOn": ["[parameters('iotHubName')]"]
        }

   ```

6. Şablon dosyasını kaydedin. Tamamlanan şablon aşağıdaki gibi görünür:

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
           "iotHubName": {
               "type": "string"
           },
           "provisioningServiceName": {
               "type": "string"
           },
           "hubLocation": {
               "type": "string",
               "allowedValues": [
                   "eastus",
                   "westus",
                   "westeurope",
                   "northeurope",
                   "southeastasia",
                   "eastasia"
               ]
           }
       },
       "variables": {        
           "iotHubResourceId": "[resourceId('Microsoft.Devices/Iothubs', parameters('iotHubName'))]",
           "iotHubKeyName": "iothubowner",
           "iotHubKeyResource": "[resourceId('Microsoft.Devices/Iothubs/Iothubkeys', parameters('iotHubName'), variables('iotHubKeyName'))]"
       },
       "resources": [
           {
               "apiVersion": "2017-07-01",
               "type": "Microsoft.Devices/IotHubs",
               "name": "[parameters('iotHubName')]",
               "location": "[parameters('hubLocation')]",
               "sku": {
                   "name": "S1",
                   "capacity": 1
               },
               "tags": {
               },
               "properties": {
               }            
           },
           {
               "type": "Microsoft.Devices/provisioningServices",
               "sku": {
                   "name": "S1",
                   "capacity": 1
               },
               "name": "[parameters('provisioningServiceName')]",
               "apiVersion": "2017-11-15",
               "location": "[parameters('hubLocation')]",
               "tags": {},
               "properties": {
                   "iotHubs": [
                       {
                           "connectionString": "[concat('HostName=', reference(variables('iotHubResourceId')).hostName, ';SharedAccessKeyName=', variables('iotHubKeyName'), ';SharedAccessKey=', listkeys(variables('iotHubKeyResource'), '2017-07-01').primaryKey)]",
                           "location": "[parameters('hubLocation')]",
                           "name": "[concat(parameters('iotHubName'),'.azure-devices.net')]"
                       }
                   ]
               },
               "dependsOn": ["[parameters('iotHubName')]"]
           }
       ]
   }
   ```

## <a name="create-a-resource-manager-parameter-file"></a>Bir Kaynak Yöneticisi parametre dosyası oluşturma

Son adımda tanımladığınız şablon, ioT hub'ının adını, sağlama hizmetinin adını ve bunları oluşturmak için konumu (Azure bölgesi) belirtmek için parametreleri kullanır. Bu parametreleri ayrı bir dosyadan şablona geçirirsiniz. Bunu yapmanız birden fazla dağıtım için aynı şablonu yeniden kullanabilmenizi sağlar. Parametre dosyasını oluşturmak için şu adımları uygulayın:

1. Aşağıdaki çatı içeriğiyle **parameters.json** adlı bir Azure Resource Manager parametre dosyası oluşturmak için bir metin düzenleyici kullanın: 

   ```json
   {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {}
       }
   }
   ```

2. Parametre bölümüne **iotHubName** değerini ekleyin.  IoT hub adı Azure'da genel olarak benzersiz olmalıdır, bu nedenle örnek ada benzersiz bir önek veya sonek eklemek veya tamamen yeni bir ad seçmek isteyebilirsiniz. Adınızın bir IoT hub'ı için uygun adlandırma kurallarını izlediğinden emin olun: 3-50 karakter uzunluğunda olmalıdır ve yalnızca büyük veya küçük harfleralfaerik karakterler veya tireler ('-') içerebilir. 

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
    }
   
   ```

3. Parametre bölümüne **provisioningServiceName** değerini ekleyin. Ayrıca, sağlama hizmetiniz için benzersiz bir ad seçmeniz gerekir. Bir IoT Hub Aygıt Sağlama Hizmeti için uygun adlandırma kurallarını izlediğinden emin olun: 3-64 karakter uzunluğunda olmalıdır ve yalnızca büyük veya küçük harfleralfaerik karakterler veya tireler ('-') içerebilir.

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
        "provisioningServiceName": {
            "value": "my-sample-provisioning-service"
        },
    }

   ```

4. Parametre bölümüne **hubLocation** değerini ekleyin. Bu değer, hem IoT hub’ının hem de sağlama hizmetinin konumunu belirtir. Değerin, şablon dosyasındaki parametre tanımında yer alan **allowedValues** koleksiyonunda belirtilmiş konumlardan birine karşılık gelmesi gerekir. Bu koleksiyon, değerleri hem IoT hub’larını hem de sağlama hizmetlerini destekleyen Azure konumları ile kısıtlar. Aygıt Sağlama Hizmeti için desteklenen konumların listesi için komutu `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table`çalıştırabilir veya Azure [Durumu](https://azure.microsoft.com/status/) sayfasına gidip "Aygıt Sağlama Hizmeti"nde arama yapabilirsiniz.

   ```json
    "parameters": {
        "iotHubName": {
            "value": "my-sample-iot-hub"
        },
        "provisioningServiceName": {
            "value": "my-sample-provisioning-service"
        },
        "hubLocation": {
            "value": "westus"
        }
    }

   ```

5. Dosyayı kaydedin. 


> [!IMPORTANT]
> Hem IoT hub’ı hem de sağlama hizmeti, DNS uç noktaları şeklinde genel olarak bulunabilir hale gelir. Bu nedenle, bunları adlandırırken hassas bilgiler belirtmekten kaçının.
>

## <a name="deploy-the-template"></a>Şablonu dağıtma

Şablonlarınızı dağıtmak ve dağıtımı doğrulamak için aşağıdaki Azure CLI komutlarını kullanın.

1. Şablonunuzu dağıtmak için, şablon ve parametre dosyalarını içeren klasöre gidin ve dağıtımı başlatmak için aşağıdaki [komutu çalıştırın:](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)
    
    ```azurecli
     az group deployment create -g {your resource group name} --template-file template.json --parameters @parameters.json
    ```

   Bu işlemin tamamlanması birkaç dakika sürebilir. Bir kez bittikten sonra, çıkış "Başarılı" gösteren **provisioningState** özelliği arayın. 

   ![Sağlama çıkışı](./media/quick-setup-auto-provision-rm/output.png) 


2. Dağıtımınızı doğrulamak üzere [kaynakları listelemek](https://docs.microsoft.com/cli/azure/resource?view=azure-cli-latest#az-resource-list) ve çıkışta yeni sağlama hizmetinin yanı sıra IoT hub’ını kontrol etmek için aşağıdaki komutu çalıştırın:

    ```azurecli
     az resource list -g {your resource group name}
    ```


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar, bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıçlar veya öğreticilerle devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, Azure CLI'yi IoT hub'ı veya sağlama hizmeti gibi [tek bir kaynağı silmek][lnk-az-resource-command]veya bir kaynak grubunu ve tüm kaynaklarını silmek için kullanabilirsiniz.

Sağlama hizmetini silmek için aşağıdaki komutu çalıştırın:

```azurecli
az iot hub delete --name {your provisioning service name} --resource-group {your resource group name}
```
Bir IoT hub’ını silmek için aşağıdaki komutu çalıştırın:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

Bir kaynak grubuyla birlikte bu kaynak grubunun tüm kaynaklarını silmek için aşağıdaki komutu çalıştırın:

```azurecli
az group delete --name {your resource group name}
```

Azure portalı, PowerShell veya REST API'lerinin yanı sıra Azure Kaynak Yöneticisi veya IoT Hub Aygıt Sağlama Hizmeti için yayınlanan desteklenen platform SDK'larını kullanarak kaynak gruplarını ve tek tek kaynakları da silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir IoT hub'ı ve aygıt sağlama hizmeti örneğini dağıttınız ve iki kaynağı birbirine bağladınız. Bu kurulumu niçin simüle edilmiş bir aygıt sağlamak için kullanacağınızı öğrenmek için, benzetilen bir aygıt oluşturmak için hızlı başlatmaya devam edin.

> [!div class="nextstepaction"]
> [Simüle edilmiş bir aygıt oluşturmak için hızlı başlatma](./quick-create-simulated-device.md)


<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-devguide]: iot-hub-devguide.md
[lnk-portal]: iot-hub-create-through-portal.md 
