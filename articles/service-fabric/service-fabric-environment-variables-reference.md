---
title: Azure Hizmet Kumaşı ortam değişkenleri
description: Azure Hizmet Kumaşı'ndaki ortam değişkenleri hakkında bilgi edinin. Değişkenlerin ve bunların kullanımlarının tam listesinin referansını içerir.
author: mikkelhegn
ms.topic: reference
ms.date: 12/07/2017
ms.author: mikhegn
ms.openlocfilehash: b13522b1d9f2acd2aa3f7923c1b623fab696056d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645693"
---
# <a name="service-fabric-environment-variables"></a>Servis Kumaş ı ortam değişkenleri

Service Fabric, her hizmet örneği için ayarlanmış yerleşik ortam değişkenlerine sahiptir. Ortam değişkenlerinin tam listesi aşağıdadır:

| Çevre Değişkeni                         | Açıklama                                                            | Örnek                                                              |
|----------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------------|
| Fabric_ApplicationName                       | Uygulamanın kumaş uri adı                                 | kumaş:/MyApplication                                                |
| Fabric_CodePackageName                       | İşlemin ait olduğu kod paketinin adı              | Kod                                                                 |
| Fabric_Endpoint\_IPOrFQDN\_*ServiceEndpointName*     | Bitiş noktasının ip adresi veya FQDN                                 | 10.0.0.1                                                     |
| Kumaş\_Endpoint\_*ServiceEndpointName*              | Bitiş noktası için bağlantı noktası numarası                                  | 8234                                                                 |
| Fabric_Folder_App_Log                        | Günlük klasörü                                                             | C:\\\\\\\\Veri\\\\\\\\_App\\\\_Node_0 MyApplicationType_App12 günlüğü      |
| Fabric_Folder_App_Temp                       | Geçici klasör                                                            | C:\\\\\\\\_Node_0\\\\\\MyApplicationType_App12\\MyApplicationType_App12 MyApplicationType_App12 _App veri\\\\     |
| Fabric_Folder_App_Work                       | Çalışma klasörü                                                            | C:\\\\\\\\Veri\\\\\\\\_App\\\\_Node_0 MyApplicationType_App12 çalışması     |
| Fabric_Folder_Application                    | Uygulamalar ev klasörü                                           | C:\\\\\\\\Veri\\\\\\\\_App _Node_0 MyApplicationType_App12             |
| Fabric_IsContainerHost                       | İşlemin bir kapsayıcı olup olmadığını belirten bir bool                   | yanlış                                                                |
| Fabric_NodeId                                | İşlemi çalıştıran düğümün düğüm kimliği                            | bf865279ba27deb864a976fbf4c200e                                     |
| Fabric_NodeIPOrFQDN                          | Küme bildirimi dosyasında belirtildiği gibi düğümün IP veya FQDN'si. | localhost veya 10.0.0.1                                                |
| Fabric_NodeName                              | İşlemi çalıştıran düğümün düğüm adı                          | _Node_0                                                              |
| Fabric_ServiceName                           | Hizmet ExclusiveProcess modunda barındırılırsa, hizmetin kumaş uri adı. Bu değişken değer yalnızca ServicePackageActivationMode ExclusiveProcess ile hizmet oluşturursanız kullanılabilir.  | kumaş:/MyApplication/MyService                                               |
| Fabric_ServicePackageActivationId            | The ServicePackageActivationId                                         | Bİr REHBER                                                               |
| Fabric_ServicePackageName                    | Hizmet paketinin adı işlemin bir parçasıdır                     | Web1Pkg                                                              |

Servis KumaşÇalışma Zamanı Tarafından Kullanılan İç Ortam Değişkenleri:

- Fabric_ApplicationHostId
- Fabric_ApplicationHostType
- Fabric_ApplicationId
- Fabric_CodePackageInstanceId
- Fabric_CodePackageInstanceSeqNum
- Fabric_RuntimeConnectionAddress
- Fabric_ServicePackageActivationGuid
- Fabric_ServicePackageInstanceId
- Fabric_ServicePackageInstanceSeqNum
- Fabric_ServicePackageVersionInstance
- FabricActivatorAddress
- FabricPackageFileName
- HostedServiceName
