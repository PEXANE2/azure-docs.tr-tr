---
title: Bağımsız bir kümenin yapılandırmasını yükseltme
description: Bağımsız hizmet kumaş kümesini çalıştıran yapılandırmayı nasıl yükselteceklerini öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 8e7e01dac29cb9ba91c83270dac4e46c73b2089e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75610140"
---
# <a name="upgrade-the-configuration-of-a-standalone-cluster"></a>Bağımsız bir kümenin yapılandırmasını yükseltme 

Herhangi bir modern sistem için, yükseltme yeteneği ürününüzün uzun vadeli başarısının anahtarıdır. Azure Hizmet Kumaşı kümesi, sahip olduğunuz bir kaynaktır. Bu makalede, bağımsız Hizmet Kumaş kümenizin yapılandırma ayarlarını nasıl yükseltiler.

## <a name="customize-cluster-settings-in-the-clusterconfigjson-file"></a>ClusterConfig.json dosyasındaki küme ayarlarını özelleştirme
Bağımsız kümeler *ClusterConfig.json* dosyası aracılığıyla yapılandırılır. Farklı ayarlar hakkında daha fazla bilgi edinmek [için bağımsız bir Windows kümesi için Yapılandırma ayarlarına](service-fabric-cluster-manifest.md)bakın.

*ClusterConfig.json'daki*Cluster `fabricSettings` [özellikleri](./service-fabric-cluster-manifest.md#cluster-properties) bölümünün altındaki bölüme ayarlar ekleyebilir, güncelleyebilir veya kaldırabilirsiniz. 

Örneğin, aşağıdaki JSON altındaki `fabricSettings` *Tanılama* bölümüne yeni bir ayar *MaxDiskQuotaInMB* ekler:

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

ClusterConfig.json dosyanızdaki ayarları değiştirdikten sonra küme [yapılandırmasını test edin](#test-the-cluster-configuration) ve ayarları kümenize uygulamak için [küme yapılandırmasını yükseltin.](#upgrade-the-cluster-configuration) 

## <a name="test-the-cluster-configuration"></a>Küme yapılandırmasını test edin
Yapılandırma yükseltmesini başlatmadan önce, bağımsız pakette aşağıdaki PowerShell komut dosyasını çalıştırarak yeni küme yapılandırmajınızı Test edebilirsiniz:

```powershell
TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>
```

Veya bu komut dosyalarını kullanın:

```powershell
TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>
```

Uç noktalar, küme adı, düğüm IP, vb. gibi bazı yapılandırmalar yükseltilememektedir. Yeni küme yapılandırması JSON eskisi karşı sınanır ve bir sorun varsa PowerShell penceresinde hatalar atar.

## <a name="upgrade-the-cluster-configuration"></a>Küme yapılandırmasını yükseltme
Küme yapılandırma yükseltmesini yükseltmek için [Başlat-ServiceFabricClusterConfigurationUpgrade'i](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade)çalıştırın. Yapılandırma yükseltmesi yükseltme etki alanına göre yükseltilerek işlenir.

```powershell
Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
```

## <a name="upgrade-cluster-certificate-configuration"></a>Yükseltme küme sertifikası yapılandırması
Küme düğümleri arasında kimlik doğrulama için bir küme sertifikası kullanılır. Hata küme düğümleri arasındaki iletişimi engellediği için sertifika devri ekstra dikkatli bir şekilde yapılmalıdır.

Dört seçenek desteklenir:  

* Tek sertifika yükseltmesi: Yükseltme yolu A (Birincil) -> Sertifikası B (Birincil) -> Sertifikası C (Birincil) ->....

* Çift sertifika yükseltmesi: Yükseltme yolu A (Birincil) -> Sertifikası A (Birincil) ve B (İkincil) -> Sertifikası B (Birincil) -> Sertifikası B (Birincil) ve C (İkincil) -> Sertifikası C (Birincil) ->....

* Sertifika türü yükseltmesi: Thumbprint tabanlı sertifika yapılandırması < > CommonName tabanlı sertifika yapılandırması. Örneğin, Sertifika Thumbprint A (Birincil) ve Thumbprint B (İkincil) -> Sertifika CommonName C.

* Sertifika veren thumbprint yükseltme: Yükseltme yolu Sertifika CN=A,IssuerThumbprint=IT1 (Birincil) -> Sertifikası CN=A,IssuerThumbprint=IT1,IT2 (Birincil) -> Sertifikası CN=A,IssuerThumbprint=IT2 (Birincil).


## <a name="next-steps"></a>Sonraki adımlar
* Bazı Service Fabric [küme ayarlarını](service-fabric-cluster-fabric-settings.md)nasıl özelleştirin için çıktığını öğrenin.
* [Kümenizi nasıl ölçeklendirecek ve dışarı çıkar' ı](service-fabric-cluster-scale-up-down.md)öğrenin.
* Uygulama [yükseltmeleri](service-fabric-application-upgrade.md)hakkında bilgi edinin.

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
