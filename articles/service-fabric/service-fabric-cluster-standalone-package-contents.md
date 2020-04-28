---
title: Windows Server için Azure Service Fabric tek başına paketi
description: Windows Server için Azure Service Fabric tek başına paketinin açıklaması ve içeriği.
author: maburlik
ms.topic: conceptual
ms.date: 8/10/2017
ms.author: maburlik
ms.openlocfilehash: 1bb73fa69717f067139067f127a0d50af4878d6c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75451840"
---
# <a name="contents-of-service-fabric-standalone-package-for-windows-server"></a>Windows Server için tek başına Service Fabric paketi içeriği
[İndirilen](https://go.microsoft.com/fwlink/?LinkId=730690) Service Fabric tek başına paketinde aşağıdaki dosyaları bulacaksınız:

| **Dosya adı** | **Kısa açıklama** |
| --- | --- |
| CreateServiceFabricCluster. ps1 |Kümeconfig. JSON içindeki ayarları kullanarak kümeyi oluşturan bir PowerShell betiği. |
| RemoveServiceFabricCluster. ps1 |ClusterConfig. JSON içindeki ayarları kullanarak bir kümeyi kaldıran bir PowerShell betiği. |
| AddNode. ps1 |Geçerli makinedeki mevcut dağıtılan bir kümeye düğüm eklemek için bir PowerShell betiği. |
| RemoveNode. ps1 |Geçerli makineden mevcut bir dağıtılan kümeden düğüm kaldırmak için bir PowerShell betiği. |
| CleanFabric. ps1 |Geçerli makinenin tek başına Service Fabric yüklemesini temizlemek için bir PowerShell betiği. Önceki MSI yüklemeleri, kendi ilişkili yükleyicileri kullanılarak kaldırılmalıdır. |
| TestConfiguration. ps1 |Altyapıyı Cluster. json dosyasında belirtilen şekilde çözümlemek için bir PowerShell betiği. |
| DownloadServiceFabricRuntimePackage. ps1 |Dağıtım makinesinin internet 'e bağlı olmadığı senaryolar için en son çalışma zamanı paketini bant dışı indirmek üzere kullanılan bir PowerShell betiği. |
| DeploymentComponentsAutoextractor. exe |Bağımsız paket betikleri tarafından kullanılan dağıtım bileşenlerini içeren kendiliğinden ayıklanan arşiv. |
| EULA_ENU. txt |Tek başına Windows Server paketi Microsoft Azure Service Fabric kullanımı için lisans koşulları. [EULA 'nın bir kopyasını şimdi indirebilirsiniz](https://go.microsoft.com/fwlink/?LinkID=733084) . |
| Readme. txt |Sürüm notlarına ve temel yükleme yönergelerine bir bağlantı. Bu belgedeki yönergelerin bir alt kümesidir. |
| Üçüncü taraf bildirimi. rtf |Pakette olan üçüncü taraf yazılımlara dikkat edin. |
| Tools\Microsoft.Azure.ServiceFabric.WindowsServer.SupportPackage.zip |Destek amacıyla izleme günlüklerini toplamak ve Microsoft 'a yüklemek için isteğe bağlı olarak çalıştırılan StandaloneLogCollector. exe. |
| Tools\servicefabricupdateservice.exe |İnternet erişimi olmayan kümeler için otomatik kod yükseltmesini etkinleştirmek üzere kullanılan bir araç. Daha fazla ayrıntı [burada](service-fabric-cluster-upgrade-windows-server.md) bulunabilir|

**Şablonlar** 

| **Dosya adı** | **Kısa açıklama** |
| --- | --- |
| ClusterConfig. güvensiz. DevCluster. JSON |Kümedeki her bir düğümle ilgili bilgiler de dahil olmak üzere, güvenli olmayan, üç düğümlü, tek makineli (veya sanal makine) geliştirme kümesinin ayarlarını içeren bir küme yapılandırma örnek dosyası. |
| ClusterConfig. güvensiz. MultiMachine. JSON |Kümedeki her makinenin bilgileri de dahil olmak üzere, güvenli olmayan, çok makineli (veya sanal makine) kümesine yönelik ayarları içeren bir küme yapılandırma örnek dosyası. |
| ClusterConfig. Windows. DevCluster. JSON |Kümedeki her bir düğümle ilgili bilgiler de dahil olmak üzere, güvenli, üç düğümlü, tek makineli (veya sanal makine) geliştirme kümesinin tüm ayarlarını içeren bir küme yapılandırma örnek dosyası. Küme, [Windows kimlikleri](https://msdn.microsoft.com/library/ff649396.aspx)kullanılarak güvenli hale getirilir. |
| ClusterConfig. Windows. MultiMachine. JSON |Güvenli kümede bulunan her makineyle ilgili bilgiler de dahil olmak üzere, Windows güvenliği kullanan güvenli, çok makineli (veya sanal makine) kümenin tüm ayarlarını içeren bir küme yapılandırma örneği dosyası. Küme, [Windows kimlikleri](https://msdn.microsoft.com/library/ff649396.aspx)kullanılarak güvenli hale getirilir. |
| ClusterConfig. X509. DevCluster. JSON |Kümedeki her bir düğümle ilgili bilgiler de dahil olmak üzere, güvenli, üç düğümlü, tek makineli (veya sanal makine) geliştirme kümesinin tüm ayarlarını içeren bir küme yapılandırma örnek dosyası. Küme, x509 sertifikaları kullanılarak güvenli hale getirilir. |
| ClusterConfig. X509. MultiMachine. JSON |Güvenli kümedeki her bir düğümün bilgileri de dahil olmak üzere, güvenli, çok makineli (veya sanal makine) kümesine yönelik tüm ayarları içeren bir küme yapılandırma örnek dosyası. Küme, x509 sertifikaları kullanılarak güvenli hale getirilir. |
| ClusterConfig. gMSA. Windows. MultiMachine. JSON |Güvenli kümedeki her bir düğümün bilgileri de dahil olmak üzere, güvenli, çok makineli (veya sanal makine) kümesine yönelik tüm ayarları içeren bir küme yapılandırma örnek dosyası. Küme, [Grup yönetilen hizmet hesapları](https://technet.microsoft.com/library/jj128431(v=ws.11).aspx)kullanılarak güvenli hale getirilir. |

## <a name="cluster-configuration-samples"></a>Küme yapılandırma örnekleri
Küme yapılandırma şablonlarının en son sürümleri GitHub sayfasında bulunabilir: [tek başına küme yapılandırma örnekleri](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

## <a name="independent-runtime-package"></a>Bağımsız çalışma zamanı paketi
En son çalışma zamanı paketi, [Indirme bağlantısı-Service Fabric Runtime-Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)'dan küme dağıtımı sırasında otomatik olarak indirilir.

## <a name="related"></a>İlgili
* [Tek başına Azure Service Fabric kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md)
* [Service Fabric kümesi güvenlik senaryoları](service-fabric-windows-cluster-windows-security.md)
