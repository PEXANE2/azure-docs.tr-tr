---
title: Azure Service Fabric ortam değişkenleri
description: Azure Service Fabric 'de ortam değişkenleri hakkında bilgi edinin. Değişkenlerin tam listesi ve kullanımları içeren bir başvuru içerir.
author: mikkelhegn
ms.topic: reference
ms.date: 12/07/2017
ms.author: mikhegn
ms.openlocfilehash: b13522b1d9f2acd2aa3f7923c1b623fab696056d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75645693"
---
# <a name="service-fabric-environment-variables"></a>Service Fabric ortam değişkenleri

Service Fabric her bir hizmet örneği için yerleşik ortam değişkenleri ayarlanmış. Ortam değişkenlerinin tam listesi aşağıda verilmiştir:

| Ortam değişkeni                         | Açıklama                                                            | Örnek                                                              |
|----------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------------|
| Fabric_ApplicationName                       | Uygulamanın yapı URI 'si adı                                 | Yapı:/MyApplication                                                |
| Fabric_CodePackageName                       | İşlemin ait olduğu kod paketinin adı              | Kod                                                                 |
| Fabric_Endpoint \_ ıporfqdn \_ *serviceendpointname*     | Uç noktanın IP adresi veya FQDN 'SI                                 | 10.0.0.1                                                     |
| Yapı \_ uç noktası \_ *serviceendpointname*              | Uç nokta için bağlantı noktası numarası                                  | 8234                                                                 |
| Fabric_Folder_App_Log                        | Günlük klasörü                                                             | C: \\ \\ veri \\ \\ _App \\ \\ _Node_0 \\ \\ MyApplicationType_App12 \\ \\ günlüğe kaydet      |
| Fabric_Folder_App_Temp                       | Geçici klasör                                                            | C: \\ \\ veri \\ \\ _App \\ \\ _Node_0 \\ \\ MyApplicationType_App12 \\ \\ geçici     |
| Fabric_Folder_App_Work                       | Çalışma klasörü                                                            | C: \\ \\ veri \\ \\ _App \\ \\ _Node_0 \\ \\ MyApplicationType_App12 \\ \\ iş     |
| Fabric_Folder_Application                    | Uygulamalar giriş klasörü                                           | C: \\ \\ veri \\ \\ _App \\ \\ _Node_0 \\ \\ MyApplicationType_App12             |
| Fabric_IsContainerHost                       | İşlemin bir kapsayıcı olup olmadığını belirten bir bool                   | yanlış                                                                |
| Fabric_NodeId                                | İşlemi çalıştıran düğümün düğüm KIMLIĞI                            | bf865279ba277deb864a976fbf4c200e                                     |
| Fabric_NodeIPOrFQDN                          | Küme bildirim dosyasında belirtildiği gibi, düğümün IP veya FQDN 'SI. | localhost veya 10.0.0.1                                                |
| Fabric_NodeName                              | İşlemi çalıştıran düğümün düğüm adı                          | _Node_0                                                              |
| Fabric_ServiceName                           | Hizmet ExclusiveProcess modunda barındırılıyorsa hizmetin yapı URI 'si adı. Bu değişken değeri yalnızca hizmeti ServicePackageActivationMode ExclusiveProcess ile oluşturursanız kullanılabilir.  | Fabric:/MyApplication/hizmetim                                               |
| Fabric_ServicePackageActivationId            | Servicepackageactivationıd                                         | BIR GUıD                                                               |
| Fabric_ServicePackageName                    | İşlemin bir parçası olduğu hizmet paketinin adı                     | Web1Pkg                                                              |

Service Fabric çalışma zamanı tarafından kullanılan iç ortam değişkenleri:

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
