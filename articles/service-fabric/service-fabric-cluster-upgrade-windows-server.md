---
title: Bağımsız kümenin sürümünü yükseltme
description: Bağımsız hizmet kumaş ı kümesini çalıştıran Azure Hizmet Kumaşı kodunu yükseltin.
author: dkkapur
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 489a90180454e2b4a9dad34730fbd3c4f235a2ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77598111"
---
# <a name="upgrade-the-service-fabric-version-that-runs-on-your-cluster"></a>Kümenizde çalışan Hizmet Kumaşı sürümünü yükseltme 

Herhangi bir modern sistem için, yükseltme yeteneği ürününüzün uzun vadeli başarısının anahtarıdır. Azure Hizmet Kumaşı kümesi, sahip olduğunuz bir kaynaktır. Bu makalede, bağımsız kümenizde çalışan Hizmet Kumaşı sürümünün nasıl yükseltilen anlatılmaktadır.

> [!NOTE]
> Kümenizin her zaman desteklenen bir Hizmet Kumaşı sürümünü çalıştırdığından emin olun. Microsoft, Service Fabric'in yeni bir sürümünüduyurduğunda, önceki sürüm, duyuru tarihinden itibaren en az 60 gün sonra desteğin sona erdirilme üzere işaretlenir. Yeni sürümler [Service Fabric takım blogunda](https://blogs.msdn.microsoft.com/azureservicefabric/)duyurulur. Yeni sürüm bu noktada seçmek için kullanılabilir.
>
>

Kümenizi yalnızca her Servis Kumaş düğümünün ayrı bir fiziksel veya sanal makinede tahsis edildiği üretim tarzı bir düğüm yapılandırması kullanıyorsanız, yeni sürüme yükseltebilirsiniz. Birden fazla Hizmet Kumaş düğümünün tek bir fiziksel veya sanal makinede olduğu bir geliştirme kümeniz varsa, kümeyi yeni sürümle yeniden oluşturmanız gerekir.

İki farklı iş akışı kümenizi en son sürüme veya desteklenen Service Fabric sürümüne yükseltebilir. İş akışından biri, en son sürümü otomatik olarak indirmek için bağlantıya sahip kümeler içindir. Diğer iş akışı, en son Service Fabric sürümünü indirmek için bağlantısı olmayan kümeler içindir.

## <a name="enable-auto-upgrade-of-the-service-fabric-version-of-your-cluster"></a>Kümenizin Servis Kumaşı sürümünün otomatik yükseltmesini etkinleştirme
Microsoft yeni bir sürüm yayımladığında Hizmet Dokusu güncelleştirmelerini karşıdan yükecek kümenizi ayarlamak için küme yapılandırmasını `fabricClusterAutoupgradeEnabled` *doğru*ayarlayın. Hizmet Kumaşı'nın kümenizin açık olmasını istediğiniz desteklenen bir sürümünü `fabricClusterAutoupgradeEnabled` el ile seçmek için küme yapılandırmasını *false'a*ayarlayın.

## <a name="upgrade-clusters-that-have-connectivity-to-download-the-latest-code-and-configuration"></a>En son kodu ve yapılandırmayı indirmek için bağlantıya sahip yükseltme kümeleri
Küme [düğümlerinizmicrosoft Download Center'a](https://download.microsoft.com)internet bağlantısı varsa, kümenizi desteklenen bir sürüme yükseltmek için bu adımları kullanın.

[Microsoft İndirme Merkezi'ne](https://download.microsoft.com)bağlantısı olan kümeler için Microsoft, yeni Service Fabric sürümlerinin kullanılabilirliğini düzenli olarak denetler.

Yeni bir Service Fabric sürümü kullanılabilir olduğunda, paket kümeye yerel olarak indirilir ve yükseltme için verilir. Ayrıca, müşteriyi bu yeni sürüm hakkında bilgilendirmek için, sistem aşağıdakilere benzer açık bir küme durumu uyarısı gösterir:

"Geçerli küme sürümü [sürüm #] desteği [tarih] sona erer."

Küme en son sürümü çalıştırdıktan sonra uyarı gider.

Küme durumu uyarısını gördüğünüzde, kümeyi yükseltin:

1. Kümedeki düğümler olarak listelenen tüm makinelere yönetici erişimi olan herhangi bir makineden kümeye bağlanın. Bu komut dosyasının çalıştırılan makine kümenin bir parçası olmak zorunda değildir.

    ```powershell
    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3"
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Yükseltebileceğiniz Service Fabric sürümlerinin listesini alın.

    ```powershell
    ###### Get the list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    Buna benzer bir çıktı almalısınız:

    ![Servis Kumaş ı sürümlerini alın][getfabversions]
3. [Başlat-ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterupgrade) Windows PowerShell komutunu kullanarak kullanılabilir bir sürüme küme yükseltmesi başlatın.

    ```powershell
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    ```
   Yükseltmenin ilerlemesini izlemek için Service Fabric Explorer'ı kullanabilir veya aşağıdaki PowerShell komutunu çalıştırabilirsiniz:

    ```powershell
    Get-ServiceFabricClusterUpgrade
    ```

    Küme sistem durumu ilkeleri karşılanmazsa, yükseltme geri alınır. Start-ServiceFabricClusterUpgrade komutu için özel sistem durumu ilkeleri belirtmek [için, Start-ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterupgrade)için belgelere bakın.

    Geri almayla sonuçlanan sorunları giderdikten sonra, daha önce açıklandığı gibi aynı adımları izleyerek yükseltmeyi yeniden başlatın.

## <a name="upgrade-clusters-that-have-no-connectivity-to-download-the-latest-code-and-configuration"></a>En son kodu ve yapılandırmayı indirmek için *bağlantısı olmayan* yükseltme kümeleri
Küme düğümlerinizmicrosoft [Download Center'a](https://download.microsoft.com)internet bağlantısı yoksa kümenizi desteklenen bir sürüme yükseltmek için bu adımları kullanın.

> [!NOTE]
> Internet'e bağlı olmayan bir küme çalıştırıyorsanız, yeni sürümler hakkında bilgi edinmek için [Service Fabric takım blogunu](https://blogs.msdn.microsoft.com/azureservicefabric/) izlemeniz gerekir. Sistem, yeni sürümler hakkında sizi uyarmak için bir küme durumu uyarısı göstermez.  
>
>

### <a name="auto-provisioning-vs-manual-provisioning"></a>Otomatik sağlama ve manuel sağlama
En son kod sürümü için otomatik indirme ve kayıt işlemlerini etkinleştirmek için Service Fabric Update Service'i ayarlayın. Talimatlar için, tek [başına pakette](service-fabric-cluster-standalone-package-contents.md) *Araçlar\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt* bakın.

Manuel işlem için bu yönergeleri izleyin.

Yapılandırma yükseltmesini başlatmadan önce aşağıdaki özelliği *yanlış* olarak ayarlamak için küme yapılandırmanızı değiştirin:

```json
"fabricClusterAutoupgradeEnabled": false,
```

Kullanım ayrıntıları için [Start-ServiceFabricClusterConfigurationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade) PowerShell komutuna bakın. Yapılandırma yükseltmesini başlatmadan önce JSON'unuzda 'clusterConfigurationVersion'u güncelleştirdiğinizden emin olun.

```powershell
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
```

### <a name="cluster-upgrade-workflow"></a>Küme yükseltme iş akışı

1. Kümedeki düğümlerden birinden [Get-ServiceFabricClusterYükseltme'i](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterupgrade) çalıştırın ve *TargetCodeVersion'u*not edin.

2. Geçerli sürümle tüm yükseltme uyumlu sürümleri listelemek ve ilgili paketi ilgili indirme bağlantılarından indirmek için internete bağlı bir makineden aşağıdakileri çalıştırın:

    ```powershell
    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. Kümedeki düğümler olarak listelenen tüm makinelere yönetici erişimi olan herhangi bir makineden kümeye bağlanın. Bu komut dosyasının çalıştırılan makine kümenin bir parçası olmak zorunda değildir.

    ```powershell
    ###### Get the list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"
    ```
4. İndirilen paketi küme görüntü deposuna kopyalayın.

5. Kopyalanan paketi kaydedin.

    ```powershell
    ###### Get the list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab
    ```
6. Kullanılabilir bir sürüme küme yükseltmesi başlatın.

    ```powershell
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    ```
    Service Fabric Explorer'da yükseltmenin ilerlemesini izleyebilir veya aşağıdaki PowerShell komutunu çalıştırabilirsiniz:

    ```powershell
    Get-ServiceFabricClusterUpgrade
    ```

    Küme sistem durumu ilkeleri karşılanmazsa, yükseltme geri alınır. Start-ServiceFabricClusterUpgrade komutu için özel sistem durumu ilkeleri belirtmek [için, Start-ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterupgrade)için belgelere bakın.

    Geri almayla sonuçlanan sorunları giderdikten sonra, daha önce açıklandığı gibi aynı adımları izleyerek yükseltmeyi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar
* [Bağımsız bir kümenin yapılandırmasını yükseltme](service-fabric-cluster-config-upgrade-windows-server.md)
* Bazı [Service Fabric küme ayarlarını](service-fabric-cluster-fabric-settings.md)özelleştirin.
* [Kümenizi girip çıkar.](service-fabric-cluster-scale-up-down.md)

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
