---
title: Windows Server için Azure Hizmet Kumaş Tek Başına Paketi
description: Windows Server için Azure Hizmet Kumaş Standalone paketinin açıklaması ve içeriği.
author: maburlik
ms.topic: conceptual
ms.date: 8/10/2017
ms.author: maburlik
ms.openlocfilehash: 1bb73fa69717f067139067f127a0d50af4878d6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451840"
---
# <a name="contents-of-service-fabric-standalone-package-for-windows-server"></a>Windows Server için Hizmet Kumaş Bağımsız paketinin içeriği
[İndirilen](https://go.microsoft.com/fwlink/?LinkId=730690) Service Fabric Standalone paketinde aşağıdaki dosyaları bulabilirsiniz:

| **Dosya adı** | **Kısa açıklama** |
| --- | --- |
| CreateServiceFabricCluster.ps1 |ClusterConfig.json'daki ayarları kullanarak kümeyi oluşturan bir PowerShell komut dosyası. |
| RemoveServiceFabricCluster.ps1 |ClusterConfig.json'daki ayarları kullanarak bir kümeyi kaldıran bir PowerShell komut dosyası. |
| AddNode.ps1 |Geçerli makinede varolan bir kümeye düğüm eklemek için bir PowerShell komut dosyası. |
| RemoveNode.ps1 |Geçerli makineden varolan bir kümeden bir düğümü kaldırmak için bir PowerShell komut dosyası. |
| CleanFabric.ps1 |Geçerli makineden bağımsız servis kumaşı kurulumunu temizlemek için bir PowerShell komut dosyası. Önceki MSI yüklemeleri kendi ilişkili kaldırıcıları kullanılarak kaldırılmalıdır. |
| TestConfiguration.ps1 |Cluster.json'da belirtildiği gibi altyapıyı analiz etmek için bir PowerShell komut dosyası. |
| DownloadServiceFabricRuntimePackage.ps1 |Dağıtım makinesinin internete bağlı olmadığı senaryolar için en son çalışma zamanı paketini bantdışına indirmek için kullanılan bir PowerShell komut dosyası. |
| DağıtımComponentsAutoextractor.exe |Bağımsız paket komut dosyaları tarafından kullanılan Dağıtım Bileşenlerini içeren kendi kendini ayıklayan arşiv. |
| EULA_ENU.txt |Microsoft Azure Service Fabric bağımsız Windows Server paketinin kullanımına ilişkin lisans koşulları. [EULA'nın bir kopyasını](https://go.microsoft.com/fwlink/?LinkID=733084) şimdi indirebilirsiniz. |
| Benioku.txt |Sürüm notlarına ve temel yükleme yönergelerine bağlantı. Bu belgedeki yönergelerin bir alt kümesidir. |
| ÜçüncüPartyNotice.rtf |Pakette bulunan üçüncü taraf yazılımbildirimi. |
| Araçlar\Microsoft.Azure.ServiceFabric.WindowsServer.SupportPackage.zip |StandaloneLogCollector.exe destek amacıyla Microsoft'a izleme günlükleri toplamak ve yüklemek için isteğe bağlı olarak çalıştırılır. |
| Araçlar\ServiceFabricUpdateService.zip |Internet erişimi olmayan kümeler için otomatik kod yükseltmesini etkinleştirmek için kullanılan bir araç. Daha fazla bilgi burada [bulabilirsiniz](service-fabric-cluster-upgrade-windows-server.md)|

**Şablon** 

| **Dosya adı** | **Kısa açıklama** |
| --- | --- |
| ClusterConfig.Unsecure.DevCluster.json |Kümedeki her düğüm için bilgiler de dahil olmak üzere, güvenli olmayan, üç düğümlü, tek makineli (veya sanal makine) geliştirme kümesinin ayarlarını içeren küme yapılandırma örnek dosyası. |
| ClusterConfig.Unsecure.MultiMachine.json |Kümedeki her makinenin bilgilerini içeren, güvenli olmayan, çok makineli (veya sanal makine) kümenin ayarlarını içeren küme yapılandırma örnek dosyası. |
| ClusterConfig.Windows.DevCluster.json |Kümedeki her düğüm için bilgiler de dahil olmak üzere güvenli, üç düğümlü, tek makineli (veya sanal makine) geliştirme kümesinin tüm ayarlarını içeren küme yapılandırma örnek dosyası. Küme, Windows kimlikleri kullanılarak güvenli hale [alınmıştır.](https://msdn.microsoft.com/library/ff649396.aspx) |
| ClusterConfig.Windows.MultiMachine.json |Güvenli kümedeki her makinenin bilgileri de dahil olmak üzere, Windows güvenliğini kullanan güvenli, çok makineli (veya sanal makine) kümenin tüm ayarlarını içeren küme yapılandırma örnek dosyası. Küme, Windows kimlikleri kullanılarak güvenli hale [alınmıştır.](https://msdn.microsoft.com/library/ff649396.aspx) |
| ClusterConfig.x509.DevCluster.json |Kümedeki her düğüm için bilgiler de dahil olmak üzere güvenli, üç düğümlü, tek makineli (veya sanal makine) geliştirme kümesinin tüm ayarlarını içeren küme yapılandırma örnek dosyası. Küme x509 sertifikaları kullanılarak güvenlidir. |
| ClusterConfig.x509.MultiMachine.json |Güvenli kümedeki her düğüm için bilgiler de dahil olmak üzere, güvenli, çok makineli (veya sanal makine) kümeiçin tüm ayarları içeren bir küme yapılandırma örnek dosyası. Küme x509 sertifikaları kullanılarak güvenlidir. |
| ClusterConfig.gMSA.Windows.MultiMachine.json |Güvenli kümedeki her düğüm için bilgiler de dahil olmak üzere, güvenli, çok makineli (veya sanal makine) kümeiçin tüm ayarları içeren bir küme yapılandırma örnek dosyası. Küme, [Grup Yönetilen Hizmet Hesapları](https://technet.microsoft.com/library/jj128431(v=ws.11).aspx)kullanılarak güvenli hale alınmıştır. |

## <a name="cluster-configuration-samples"></a>Küme Yapılandırma Örnekleri
Küme yapılandırma şablonlarının en son sürümleri GitHub sayfasında bulunabilir: [Bağımsız Küme Yapılandırma Örnekleri.](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

## <a name="independent-runtime-package"></a>Bağımsız Çalışma Zamanı Paketi
En son çalışma zamanı [paketi, Indirme Linki - Service Fabric Runtime - Windows Server'dan](https://go.microsoft.com/fwlink/?linkid=839354)küme dağıtımı sırasında otomatik olarak indirilir.

## <a name="related"></a>İlgili
* [Bağımsız bir Azure Hizmet Kumaşı kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md)
* [Hizmet Kumaş küme güvenlik senaryoları](service-fabric-windows-cluster-windows-security.md)
