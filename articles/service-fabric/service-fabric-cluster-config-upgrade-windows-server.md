---
title: Tek başına kümenin yapılandırmasını yükseltme
description: Tek başına Service Fabric kümesi çalıştıran yapılandırmayı nasıl yükselteceğinizi öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 8e7e01dac29cb9ba91c83270dac4e46c73b2089e
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75610140"
---
# <a name="upgrade-the-configuration-of-a-standalone-cluster"></a>Tek başına kümenin yapılandırmasını yükseltme 

Modern bir sistem için yükseltme özelliği, ürününüzün uzun süreli başarısına yönelik bir anahtardır. Azure Service Fabric kümesi, sahip olduğunuz bir kaynaktır. Bu makalede, tek başına Service Fabric kümenizin yapılandırma ayarlarının nasıl yükseltileceği açıklanır.

## <a name="customize-cluster-settings-in-the-clusterconfigjson-file"></a>ClusterConfig. JSON dosyasındaki küme ayarlarını özelleştirme
Tek başına kümeler *clusterConfig. JSON* dosyası aracılığıyla yapılandırılır. Farklı ayarlar hakkında daha fazla bilgi edinmek için bkz. [tek başına Windows kümesi Için yapılandırma ayarları](service-fabric-cluster-manifest.md).

*ClusterConfig. JSON*içindeki [küme özellikleri](./service-fabric-cluster-manifest.md#cluster-properties) bölümünde `fabricSettings` bölümünde ayarları ekleyebilir, güncelleştirebilir veya kaldırabilirsiniz. 

Örneğin, aşağıdaki JSON, `fabricSettings`altındaki *Tanılama* bölümüne *Maxdiskquocontainer MB* yeni bir ayar ekler:

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

ClusterConfig. JSON dosyanızdaki ayarları değiştirdikten sonra, [küme yapılandırmasını test](#test-the-cluster-configuration) edin ve ardından ayarları kümenize uygulamak için [küme yapılandırmasını yükseltin](#upgrade-the-cluster-configuration) . 

## <a name="test-the-cluster-configuration"></a>Küme yapılandırmasını test etme
Yapılandırma yükseltmesini başlatmadan önce, tek başına pakette aşağıdaki PowerShell betiğini çalıştırarak yeni küme yapılandırma JSON 'nizi test edebilirsiniz:

```powershell
TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>
```

Veya şu betiği kullanın:

```powershell
TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>
```

Uç noktalar, küme adı, düğüm IP vb. gibi bazı konfigürasyonlar yükseltilemez. Yeni küme yapılandırması JSON, eskileri göre test edilir ve bir sorun varsa PowerShell penceresinde hata oluşturur.

## <a name="upgrade-the-cluster-configuration"></a>Küme yapılandırmasını yükseltme
Küme yapılandırma yükseltmesini yükseltmek için [Start-ServiceFabricClusterConfigurationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade)komutunu çalıştırın. Yapılandırma yükseltme etki alanı yükseltme etki alanına göre işlenir.

```powershell
Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
```

## <a name="upgrade-cluster-certificate-configuration"></a>Küme sertifikası yapılandırmasını yükselt
Küme düğümleri arasında kimlik doğrulaması için bir küme sertifikası kullanılır. Hata, küme düğümleri arasındaki iletişimi engellediğinden, sertifika geçişi çok dikkatli bir şekilde gerçekleştirilmelidir.

Dört seçenek desteklenir:  

* Tek sertifika yükseltme: yükseltme yolu (birincil) -> sertifika B (birincil) sertifikasıdır sertifika C (birincil) -> ->....

* Double sertifika yükseltme: (birincil) -> sertifika sertifika (birincil) ve B (ikincil) -> sertifika B (birincil) yükseltme yolu olan sertifika B (birincil) -> ve C (ikincil) sertifika C (birincil) -> ->....

* Sertifika türü yükseltme: Parmak Izi tabanlı sertifika yapılandırma < > CommonName tabanlı sertifika yapılandırması. Örneğin, sertifika parmak Izi A (birincil) ve Parmak Izi B (Ikincil)-> sertifika CommonName C.

* Sertifika verenin parmak izi yükseltmesi: sertifika CN = A, ıssuerparmak Izi = IT1 (birincil)-> sertifikası CN = A, ıssuerparmak izi = IT1, IT2 (birincil)-> Certificate CN = A, ıssuerparmak Izi = IT2 (birincil).


## <a name="next-steps"></a>Sonraki adımlar
* Bazı [Service Fabric kümesi ayarlarını](service-fabric-cluster-fabric-settings.md)özelleştirmeyi öğrenin.
* [Kümenizin ölçeğini ve ölçeğini nasıl ölçeklendireceğinizi](service-fabric-cluster-scale-up-down.md)öğrenin.
* [Uygulama yükseltmeleri](service-fabric-application-upgrade.md)hakkında bilgi edinin.

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
