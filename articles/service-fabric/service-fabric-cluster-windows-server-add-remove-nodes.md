---
title: Bağımsız Hizmet Kumaş ı kümesine düğüm ekleme veya kaldırma
description: Windows Server çalıştıran fiziksel veya sanal bir makinede, şirket içinde veya herhangi bir bulutta olabilecek bir Azure Hizmet Kumaş ı kümesine düğüm ekleme veya kaldırma yı öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: dekapur
ms.openlocfilehash: f9bee35ee8e82070b4cf601139b471562ba5e10b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934217"
---
# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Windows Server’da çalışan tek başına Service Fabric kümesine düğüm ekleme veya kaldırma
[Windows Server makinelerde bağımsız Hizmet Kumaşı kümenizi oluşturduktan](service-fabric-cluster-creation-for-windows-server.md)sonra (işletmeniz) gereksinimleriniz değişebilir ve kümenize düğüm eklemeniz veya kaldırmanız gerekir. Bu makalede, bunu başarmak için ayrıntılı adımlar sağlar. Yerel geliştirme kümelerinde düğüm ekleme/kaldırma işlevinin desteklenmediğini lütfen unutmayın.

## <a name="add-nodes-to-your-cluster"></a>Kümenize düğüm ekleme

1. Plan'da özetlenen adımları izleyerek kümenize eklemek istediğiniz VM/makineyi hazırlayın [ve Hizmet Kumaşı küme dağıtımınızı hazırlayın.](service-fabric-cluster-standalone-deployment-preparation.md)

2. Bu VM/makineyi hangi hata etki alanına ekleyeceğiniz hata etki alanını belirleyin ve yükseltin.

   Kümeyi güvenli hale getirmek için sertifikalar kullanıyorsanız, kümeye katılmak için düğüm ekibe hazırlık için sertifikaların yerel sertifika depolarına yüklenmesi beklenir. Analog, diğer güvenlik biçimleri kullanılarak uygulanabilir.

3. Kümeye eklemek istediğiniz VM/makineye uzak masaüstü (RDP).

4. Windows [Server için Hizmet Dokusu için bağımsız paketi](https://go.microsoft.com/fwlink/?LinkId=730690) VM/makineye kopyalayın veya indirin ve paketin zip'ini kaldırın.

5. PowerShell'i yüksek ayrıcalıklarla çalıştırın ve fermuarsız paketin konumuna gidin.

6. *AddNode.ps1* komut dosyasını eklenecek yeni düğümü açıklayan parametrelerle çalıştırın. Aşağıdaki örnek, Ud1 ve fd:/dc1/r0 içine Tip NodeType0 ve IP adresi 182.17.34.52 ile VM5 adı verilen yeni bir düğüm ekler. `ExistingClusterConnectionEndPoint`kümedeki *herhangi* bir düğümün IP adresi olabilecek varolan kümedeki bir düğüm için bağlantı bitiş noktasıdır. 

   Güvenli olmayan (prototip oluşturma):

   ```
   .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
   ```

   Güvenli (sertifika tabanlı):

   ```  
   $CertThumbprint= "***********************"
    
   .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -X509Credential -ServerCertThumbprint $CertThumbprint  -AcceptEULA

   ```

   Komut dosyası çalışma bittiğinde, [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet çalıştırarak yeni düğüm eklenip eklenmediğini kontrol edebilirsiniz.

7. Kümedeki farklı düğümler arasında tutarlılık sağlamak için bir yapılandırma yükseltmesi başlatmanız gerekir. En son yapılandırma dosyasını almak için [Get-ServiceFabricClusterConfiguration'ı](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) çalıştırın ve yeni eklenen düğümü "Düğümler" bölümüne ekleyin. Ayrıca, aynı yapılandırmaya sahip bir kümeyi yeniden dağıtmanız gerektiğinde her zaman en son küme yapılandırmasına sahip olmanız önerilir.

   ```
    {
        "nodeName": "vm5",
        "iPAddress": "182.17.34.52",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD1"
    }
   ```

8. Yükseltmeye başlamak için [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) çalıştırın.

   ```
   Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
   ```

   Service Fabric Explorer'da yükseltmenin ilerlemesini izleyebilirsiniz. Alternatif olarak, [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)çalıştırabilirsiniz.

### <a name="add-nodes-to-clusters-configured-with-windows-security-using-gmsa"></a>gMSA kullanarak Windows Security ile yapılandırılan kümelere düğüm ekleme
Grup Yönetilen Hizmet Hesabı (gMSA) ilehttps://technet.microsoft.com/library/hh831782.aspx)yapılandırılan kümeler için yapılandırma yükseltmesi kullanılarak yeni bir düğüm eklenebilir:
1. En son yapılandırma dosyasını almak ve "Düğümler" bölümüne eklemek istediğiniz yeni düğüm hakkında ayrıntılar eklemek için varolan düğümlerden herhangi birinde [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) çalıştırın. Yeni düğümün aynı grup yönetilen hesabının bir parçası olduğundan emin olun. Bu hesap tüm makinelerde yönetici olmalıdır.

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. Yükseltmeye başlamak için [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) çalıştırın.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
    ```
    Service Fabric Explorer'da yükseltmenin ilerlemesini izleyebilirsiniz. Alternatif olarak, [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps) çalıştırabilirsiniz

### <a name="add-node-types-to-your-cluster"></a>Kümenize düğüm türleri ekleme
Yeni bir düğüm türü eklemek için yapılandırmanızı "Özellikler" altındaki "NodeTypes" bölümüne yeni düğüm türünü eklemek için değiştirin ve [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps)kullanarak yapılandırma yükseltmesi başlatın. Yükseltme tamamlandıktan sonra, bu düğüm türüyle kümenize yeni düğümler ekleyebilirsiniz.

## <a name="remove-nodes-from-your-cluster"></a>Kümenizdeki düğümleri kaldırma
Bir düğüm, aşağıdaki şekilde yapılandırma yükseltmesi kullanılarak kümeden kaldırılabilir:

1. En son yapılandırma dosyasını almak ve düğümü "Düğümler" bölümünden *kaldırmak* için [Get-ServiceFabricClusterConfiguration'ı](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) çalıştırın.
"FabricSettings" bölümünün içindeki "Kurulum" bölümüne "DüğümlerToBeRemoved" parametresini ekleyin. "Değer" kaldırılması gereken düğüm düğüm adlarının virgülle ayrılmış bir listesi olmalıdır.

    ```
         "fabricSettings": [
            {
            "name": "Setup",
            "parameters": [
                {
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
                },
                {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
                },
                {
                "name": "NodesToBeRemoved",
                "value": "vm0, vm1"
                }
            ]
            }
        ]
    ```
2. Yükseltmeye başlamak için [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) çalıştırın.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

    ```
    Service Fabric Explorer'da yükseltmenin ilerlemesini izleyebilirsiniz. Alternatif olarak, [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)çalıştırabilirsiniz.

> [!NOTE]
> Düğümlerin kaldırılması birden çok yükseltme başlatabilir. Bazı düğümler etiketle `IsSeedNode=”true”` işaretlenir ve küme bildirimini kullanarak `Get-ServiceFabricClusterManifest`sorgulayarak tanımlanabilir. Tohum düğümleri bu tür senaryolarda etrafında taşınması gerekecektir bu tür düğümlerin kaldırılması diğerlerinden daha uzun sürebilir. Küme en az 3 birincil düğüm türü düğümleri korumak gerekir.
> 
> 

### <a name="remove-node-types-from-your-cluster"></a>Kümenizden düğüm türlerini kaldırma
Düğüm türünü çıkarmadan önce, düğüm türüne başvuran düğüm olup olmadığını lütfen iki kez kontrol edin. Karşılık gelen düğüm türünü çıkarmadan önce bu düğümleri kaldırın. Karşılık gelen tüm düğümler kaldırıldıktan sonra, Küme yapılandırmasından NodeType'ı kaldırabilir ve [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps)kullanarak yapılandırma yükseltmesi başlatabilirsiniz.


### <a name="replace-primary-nodes-of-your-cluster"></a>Kümenizin birincil düğümlerini değiştirin
Birincil düğümlerin değiştirilmesi, kaldırılıp toplu olarak eklemek yerine birdüğüm diğerine yapılmalıdır.


## <a name="next-steps"></a>Sonraki adımlar
* [Bağımsız Windows kümesi için yapılandırma ayarları](service-fabric-cluster-manifest.md)
* [X509 sertifikalarını kullanarak Windows'da bağımsız bir küme emniyeti](service-fabric-windows-cluster-x509-security.md)
* [Windows çalıştıran Azure VM'leri ile bağımsız bir Hizmet Kumaşı kümesi oluşturma](service-fabric-cluster-creation-with-windows-azure-vms.md)

