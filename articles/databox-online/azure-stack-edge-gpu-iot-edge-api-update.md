---
title: Azure Stack Edge Ocak 2021 güncelleştirme etkisi | Microsoft Docs
description: Ocak 2021 güncelleştirmesi yüklendikten sonra Azure Stack Edge cihazlarında IoT Edge rol yönetiminin etkilerini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 10/26/2020
ms.author: alkohli
ms.openlocfilehash: a86c4c9c32f78cda1b5183a8651783fdcbb20e85
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92754755"
---
# <a name="iot-edge-role-management-changes-for-your-azure-stack-edge"></a>Azure Stack Kenarlarınızın rol yönetimi değişikliklerini IoT Edge

Azure Stack Edge cihazınız için rol yönetimi IoT Edge için en son API sürümünü, SDK 'yı ve Azure PowerShell Ocak 2021 ' deki yayın nedeniyle kullanın. Bu makalede, Azure Stack Kenarünüzde IoT Edge rol yönetimi için API, SDK ve PowerShell cmdlet 'lerinin en son sürümünü kullandığınızda gereken değişiklikleri ayrıntılı olarak açıklanmaktadır.

Ocak 2021 güncelleştirmesi yalnızca Azure Stack Edge Pro-GPU, Azure Stack Edge Pro R ve Azure Stack Edge Mini R cihazlarında kullanılabilir ve bu makaledeki bilgiler yalnızca bu cihazlar için geçerlidir. 

## <a name="iot-edge-role-management-changes"></a>Rol yönetimi değişikliklerini IoT Edge

İsteğe bağlı Ocak 2021 cihaz yazılım sürümünü Azure Stack Edge cihazınıza yükledikten sonra, IoT Edge rol yönetimi için API, SDK ve PowerShell cmdlet 'lerinin en son sürümünü kullanmanız gerekir.

- Sürüm 2019-08-01 ile rol yönetimi API kullanıyorsanız, 2021 Ocak 'ta API sürümüne yükseltme yapın. 
- SDK sürümü 1.0.0 aracılığıyla rol yönetimi kullanıyorsanız, Ocak 2021 ' de sürüm sürümüne yükseltin.
- ,, Veya gibi Azure PowerShell cmdlet 'leri (Önizleme) ile rol yönetimi kullanıyorsanız `Get-AzStackEdgeRole` , `New-AzStackEdgeRole` `Set-AzStackEdgeRole` `Remove-AzStackEdgeRole` Şubat 2021 ' de yayımlanacak yeni cmdlet 'leri beklemeniz gerekir.

Yukarıdaki değişiklikler yalnızca Ocak 2021 güncelleştirme uygularsanız gereklidir. Mevcut cihaz yazılımı sürümünde kalırsa IoT Edge rol yönetimi üzerinde hiçbir etkisi olmaz. Ancak, yeni özellikler ve güvenlik geliştirmeleri için cihazınızı güncelleştirmenizi öneririz. 


## <a name="api-usage"></a>API kullanımı

API aracılığıyla IoT Edge rol yönetimi gerçekleştirmiş olmanız durumunda daha sonra yayımlanacak yeni API sürümünü (2020-12-01) kullanmanız gerekir. Geçerli rol API 'sini kullanıyorsanız ve yaklaşan cihaz yazılımı sürümünü yüklediyseniz, IoT eklentisi API 'sini YERLEŞTIR ' in ardından PUT, GET, Kubernetes rolüne geçmeniz gerekir.


### <a name="for-put-method"></a>PUT yöntemi için

#### <a name="current-http-request"></a>Geçerli http isteği 

- API çağrıları bu URI 'de yapılır: [https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01](https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01)

- İstek gövdesi şöyle görünür:

    ```json
    {
        "kind": "IOT",
        "properties": {
            "hostPlatform": "Linux",
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotDevice;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "348586569999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotEdge;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "1245475856069999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            },
            "shareMappings": [],
            "roleStatus": "Enabled"
        }
    }
    ```

#### <a name="upcoming-http-request"></a>Yaklaşan http isteği 

- Kubernetes rolü için API çağrıları şu URI 'de yapılır: [https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1?api-version=2020-12-01](https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1?api-version=2020-12-01)

    İstek gövdesi şöyle görünür:

    ```json
    {
        "kind": "Kubernetes",
        "properties": {
            "hostPlatform": "Linux",
            "kubernetesClusterInfo": {
                "version": "v1.17.3"
            },
            "kubernetesRoleResources": {
                "storage": {
                    "endpoints": []
                },
                "compute": {
                    "vmProfile": "DS1_v2"
                }
            }
        }
    }
    ```

- IoT Edge eklentisi için API çağrıları aşağıdaki URI 'de yapılır: [https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01](https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01)


    İstek gövdesi şöyle görünür:

    ```json
    {
        "kind": "IoT",
        "properties": {
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotDevice;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "348586569999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotEdge;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "1245475856069999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            }
        }
    }
    ```


### <a name="for-get-method"></a>GET yöntemi için

#### <a name="current-http-response"></a>Geçerli http yanıtı

- API çağrıları şu URI 'de yapılır: [https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01](https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01)


- Yanıt gövdesi şöyle görünür:

    ```json
        "kind": "IOT",
        "properties": {
            "hostPlatform": "Linux",
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "shareMappings": [],
            "roleStatus": "Enabled"
        },
        "id": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1",
        "name": "IoTRole1",
        "type": "dataBoxEdgeDevices/roles"
    }    
    ```

#### <a name="upcoming-http-response"></a>Yaklaşan http yanıtı

- API çağrıları şu URI 'de yapılır: [https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01](https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01)
- Yanıt gövdesi şöyle görünür: 

    ```json
    {
        "kind": "IoT",
        "properties": {
            "provisioningState": "Creating",
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "version": "0.1.0-beta10"
        },
        "id": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/res1/roles/kubernetesRole/addons/iotName",
        "name": " iotName",
        "type": "Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/addon",
    }
    ```

### <a name="for-delete-method"></a>DELETE yöntemi için

### <a name="current"></a>Geçerli

API çağrıları şu URI 'de yapılır: [https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01](https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01)

### <a name="upcoming"></a>İlerideki

API çağrıları şu URI 'de yapılır: [https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01](https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01)


## <a name="sdk-usage"></a>SDK kullanımı

SDK kullanıyorsanız ve Ocak 2021 tarihli cihaz güncelleştirmesini yüklediyseniz, aşağıdaki tabloda gösterildiği gibi IoT Edge rolünü ayarlama yönteminizi değiştirmeniz gerekir. Ardından, aşağıdaki örnekte gösterildiği gibi yeni SDK 'ya gitmek için yaklaşan NuGet paketini indirip yüklemelisiniz.

### <a name="current-sdk-sample"></a>Geçerli SDK örneği

```csharp
var iotRoleStatus = "Enabled";
var iotHostPlatform = "Linux";
var id = $@"/subscriptions/546ec571-2d7f-426f-9cd8-0d695fa7edba/resourceGroups/resourceGroup/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/deviceName/roles/iotrole"; 
var name = "iotrole";
var type = "Microsoft.DataBoxEdge/dataBoxEdgeDevices/role";
var iotRoleName = "iotrole";
var ioTDeviceDetails = new IoTDeviceInfo(...);
var ioTEdgeDeviceDetails = new IoTDeviceInfo(...);
var ioTEdgeAgentInfo = new IoTEdgeAgentInfo(...);
var shareMappings = new List<MountPointMap>(...);

var role = new IoTRole(roleStatus, 
    hostPlatform, 
    shareMappings, 
    ioTDeviceDetails, 
    ioTEdgeDeviceDetails, 
    ioTEdgeAgentInfo, 
    id, 
    name, 
    type);

DataBoxEdgeManagementClient.Roles.CreateOrUpdate(deviceName, iotRoleName, role, resourceGroup);
```


### <a name="new-sdk-sample"></a>Yeni SDK örneği

```csharp
var k8sRoleStatus = "Enabled";
var k8sHostPlatform = "Linux";
var k8sId = $@"/subscriptions/546ec571-2d7f-426f-9cd8-0d695fa7edba/resourceGroups/resourceGroup/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/deviceName/roles/KubernetesRole"; 
var k8sRoleName = "KubernetesRole";
var k8sClusterVersion = "v1.17.3"; //Final values will be updated here around January 2021
var k8sVmProfile = "DS1_v2"; //Final values will be updated here around January 2021
var type = "Microsoft.DataBoxEdge/dataBoxEdgeDevices/role";
var k8sRole = new KubernetesRole(
    roleStatus,
    hostPlatform,
    shareMappings,
    k8sClusterVersion,
    k8sVmProfile,
    k8sId,
    k8sRoleName,
    type
);
DataBoxEdgeManagementClient.Roles.CreateOrUpdate(deviceName, k8sRoleName, k8sRole, resourceGroup); //Final usage will be updated here around January 2021

var ioTId = $@"/subscriptions/546ec571-2d7f-426f-9cd8-0d695fa7edba/resourceGroups/resourceGroup/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/deviceName/roles/KubernetesRole/addons/iotaddon";
var ioTAddonName = "iotaddon";
var ioTAddonType = "Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/addons";
var addon = new IoTAddon(
    ioTDeviceDetails, 
    ioTEdgeDeviceDetails, 
    ioTEdgeAgentInfo, 
    ioTId, 
    ioTAddonName, 
    ioTAddonType);
DataBoxEdgeManagementClient.AddOns.CreateOrUpdate(deviceName, k8sRoleName, addonName, addon, resourceGroup); //Final usage will be updated here around January 2021
```

## <a name="cmdlet-usage"></a>Cmdlet kullanımı

,, `Get-AzStackEdgeRole` `New-AzStackEdgeRole` `Set-AzStackEdgeRole` Veya `Remove-AzStackEdgeRole` cmdlet 'lerini kullanıyorsanız, Şubat 2021 ' de planlanmış yeni sürümü beklemeniz gerekir.

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

#### <a name="i-am-using-azure-stack-edge-pro--fpga-does-the-january-2021-update-affect-the-fpga-model"></a>Azure Stack Edge Pro – FPGA kullanıyorum. Ocak 2021 güncelleştirmesi FPGA modelini etkiler mi?

Hayır. Ocak 2021 güncelleştirmesi yalnızca Azure Stack Edge – GPU, Azure Stack Edge Pro R ve Azure Stack Edge Mini R cihazları için geçerlidir. Azure Stack Edge Pro – FPGA bu güncelleştirmeden etkilenmez ve IoT Edge rol yönetimine herhangi bir değişiklik gerektirmez.

#### <a name="after-i-update-my-azure-stack-edge-pro---gpu-to-the-new-device-software-in-january-2021-are-any-of-the-existing-services-affected"></a>Azure Stack Edge Pro-GPU 'umu yeni cihaz yazılımına Ocak 2021 ' de güncelleştirdikten sonra, var olan hizmetlerden herhangi biri etkileniyor mu?

Hayır. Yapılandırılmış hizmetler 2021 Ocak cihaz güncelleştirmesi yüklendikten sonra etkilenmeyecektir. <!--check w/ Anoob, what existing services you are talking about in this question-->

#### <a name="what-are-the-high-level-changes-to-iot-edge-management-api-sdk-or-cmdlet"></a>IoT Edge yönetim API 'SI, SDK veya CmdLet 'e ilişkin üst düzey değişiklikler nelerdir?

IoT Edge, Kubernetes rolü altındaki bir eklenti. Bunun anlamı, ilk olarak Kubernetes 'in yapılandırıldığından emin olmanız ve ardından IoT Edge yapılandırmasını gerçekleştirdiğinizden emin olmanızı sağlar.


## <a name="next-steps"></a>Sonraki adımlar

- [Güncelleştirmelerin nasıl uygulanacağını](azure-stack-edge-gpu-install-update.md)öğrenin.

