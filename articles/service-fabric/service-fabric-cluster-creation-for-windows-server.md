---
title: Bağımsız bir Azure Hizmet Kumaşı kümesi oluşturma
description: İster şirket içinde ister herhangi bir bulutta olsun, Windows Server çalıştıran herhangi bir makinede (fiziksel veya sanal) bir Azure Hizmet Kumaşı kümesi oluşturun.
author: dkkapur
ms.topic: conceptual
ms.date: 2/21/2019
ms.author: dekapur
ms.openlocfilehash: 461d6021a201ca1fa5722bb44c427baca2a7728e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258830"
---
# <a name="create-a-standalone-cluster-running-on-windows-server"></a>Windows Server üzerinde çalıştırılan bir tek başına küme oluşturma
Windows Server çalıştıran tüm sanal makinelerde veya bilgisayarlarda Service Fabric kümeleri oluşturmak için Azure Hizmet Kumaşı'nı kullanabilirsiniz. Bu, Service Fabric uygulamalarını şirket içinde veya herhangi bir bulut sağlayıcısıyla bağlantılı windows server bilgisayarları kümesi içeren herhangi bir ortamda dağıtabileceğiniz ve çalıştırabileceğiniz anlamına gelir. Service Fabric, bağımsız Windows Server paketi adı verilen Service Fabric kümeleri oluşturmak için bir kurulum paketi sağlar. Azure'daki Geleneksel Hizmet Kumaş kümeleri yönetilen bir hizmet olarak kullanılabilirken, bağımsız Hizmet Kumaş kümeleri self servistir. Farklar hakkında daha fazla şey için Azure [ve bağımsız Hizmet Kumaş kümelerini karşılaştırma ya](./service-fabric-deploy-anywhere.md)da karşılaştırma 'ya bakın.

Bu makalede, bir Hizmet Kumaş bağımsız küme oluşturmak için adımlar boyunca size yol.

> [!NOTE]
> Bu bağımsız Windows Server paketi ticari olarak hiçbir ücret ödemeden kullanılabilir ve üretim dağıtımları için kullanılabilir. Bu paket, "Önizleme"de bulunan yeni Hizmet Kumaşı özellikleri içerebilir. Aşağı kaydırın "[Bu pakette yer alan önizleme özellikleri."](#previewfeatures_anchor) önizleme özellikleri listesi için bölüm. [EULA'nın bir kopyasını](https://go.microsoft.com/fwlink/?LinkID=733084) şimdi indirebilirsiniz.
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-the-service-fabric-for-windows-server-package"></a>Windows Server paketi için Hizmet Dokusu için destek alın
* [Azure Hizmet Kumaşı forumunda](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?)Windows Server için Hizmet Kumaşı bağımsız paketi hakkında topluluğa sorun.
* [Hizmet Kumaşı için Profesyonel Destek için](https://support.microsoft.com/oas/default.aspx?prid=16146)bir bilet açın.  Microsoft'tan Profesyonel Destek hakkında daha fazla bilgi [edinin.](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0)
* Ayrıca, [Microsoft Premier Desteği'nin](https://support.microsoft.com/en-us/premier)bir parçası olarak bu paket için destek de alabilirsiniz.
* Daha fazla bilgi için lütfen [Azure Hizmet Kumaşı destek seçeneklerine](https://docs.microsoft.com/azure/service-fabric/service-fabric-support)bakın.
* Destek amacıyla günlükleri toplamak için [Service Fabric Standalone Log toplayıcısını](service-fabric-cluster-standalone-package-contents.md)çalıştırın.

<a id="downloadpackage"></a>

## <a name="download-the-service-fabric-for-windows-server-package"></a>Windows Server paketi için Service Fabric indirme
Kümeoluşturmak için, burada bulunan Windows Server paketi (Windows Server 2012 R2 ve daha yeni) için Hizmet Dokusunu kullanın: <br>
[Download Link - Servis Kumaş Bağımsız Paket - Windows Server](https://go.microsoft.com/fwlink/?LinkId=730690)

[Paketin](service-fabric-cluster-standalone-package-contents.md)içeriği hakkında ayrıntılı bilgi bulabilirsiniz.

Hizmet Kumaşı çalışma zamanı paketi küme oluşturma sırasında otomatik olarak indirilir. Internet'e bağlı olmayan bir makineden dağıtım alıyorsanız, lütfen çalışma zamanı paketini banttan indirin: <br>
[Download Link - Servis Kumaş Çalışma Zamanı - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)

Bağımsız Küme Yapılandırma örneklerini şu anda bulun: <br>
[Bağımsız Küme Yapılandırma Örnekleri](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

<a id="createcluster"></a>

## <a name="create-the-cluster"></a>Kümeyi oluşturma
Kurulum paketiyle birlikte birkaç örnek küme yapılandırma dosyası yüklenir. Tek bir bilgisayarda çalışan korumasız ve üç düğümlü bir küme olan *ClusterConfig.Unsecure.DevCluster.json*, en basit küme yapılandırmasıdır.  Diğer yapılandırma dosyalarında X.509 sertifikalarıyla ya da Windows güvenliğiyle korunan tek veya çok makineli kümeler açıklanır.  Bu öğreticide varsayılan yapılandırma ayarlarından herhangi birini değiştirmeniz gerekmez, ancak yapılandırma dosyasını inceleyip ayarları tanıyın.  **Düğümler** bölümünde, kümedeki üç düğüm açıklanmaktadır: ad, IP adresi, [düğüm türü, hata etki alanı ve yükseltme etki alanı](service-fabric-cluster-manifest.md#nodes-on-the-cluster).  **Özellikler** bölümü kümenin [güvenlik, güvenilirlik düzeyi, tanılama koleksiyonu ve düğüm türlerini](service-fabric-cluster-manifest.md#cluster-properties) tanımlar.

Bu makalede oluşturulan küme güvenli değil.  Herkes anonim olarak bağlanıp yönetim işlemleri gerçekleştirebileceğinden, üretim kümeleri her zaman X.509 sertifikaları veya Windows güvenliği kullanılarak güvenli hale getirilmelidir.  Güvenlik yalnızca küme oluşturma sırasında yapılandırılır ve küme oluşturulduktan sonra güvenliği etkinleştirmek mümkün değildir. Config dosyasını güncelleştirme [sertifika güvenliğini](service-fabric-windows-cluster-x509-security.md) veya [Windows güvenliğini](service-fabric-windows-cluster-windows-security.md)etkinleştirin. Service Fabric küme güvenliği hakkında daha fazla bilgi edinmek için [Küme güvenliğini sağlama](service-fabric-cluster-security.md) makalesini okuyun.

### <a name="step-1-create-the-cluster"></a>Adım 1: Küme oluşturma

#### <a name="scenario-a-create-an-unsecured-local-development-cluster"></a>Senaryo A: Güvenli olmayan bir yerel geliştirme kümesi oluşturma
Service Fabric, [Örnekler'de](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)yer alan *ClusterConfig.Unsecure.DevCluster.json* dosyasını kullanarak tek makineli geliştirme kümesine dağıtılabilir.

Tek başına paketi makinenize açın, örnek config dosyasını yerel makineye kopyalayın ve ardından *createServiceFabricCluster.ps1* komut dosyasını tek başına paket klasöründen bir yönetici PowerShell oturumu aracılığıyla çalıştırın.

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

Plan'daki Çevre Kurulumu bölümüne bakın ve küme dağıtımınızı sorun giderme ayrıntılarına [hazırlayın.](service-fabric-cluster-standalone-deployment-preparation.md)

Geliştirme senaryolarını çalıştırmayı bitirdiyseniz, "[Kümeyi kaldır](#removecluster_anchor)" bölümündeki adımlara atıfta bulunarak Hizmet Kumaşı kümesini makineden kaldırabilirsiniz. 

#### <a name="scenario-b-create-a-multi-machine-cluster"></a>Senaryo B: Çok makineli küme oluşturma
Plan'da ayrıntılı olarak açıklanan planlama ve hazırlık adımlarını geçtikten [ve küme dağıtımınızı hazırladıktan](service-fabric-cluster-standalone-deployment-preparation.md)sonra, küme yapılandırma dosyanızı kullanarak üretim kümenizi oluşturmaya hazırsınız.

Kümeyi dağıtan ve yapılandıran küme yöneticisinin bilgisayarda yönetici ayrıcalıklarına sahip olması gerekir. Service Fabric’i bir etki alanı denetleyicisine yükleyemezsiniz.

1. Tek başına paketteki *TestConfiguration.ps1* betiği, bir kümenin belirli bir ortamda dağıtılıp dağıtılamayacağını doğrulamak için bir en iyi yöntem çözümleyicisi olarak kullanılır. [Dağıtım hazırlığı](service-fabric-cluster-standalone-deployment-preparation.md) belgesinde, ön koşullar ve ortam gereksinimleri listelenir. Geliştirme kümesini oluşturabileceğinizi doğrulamak için betiği çalıştırın:  

    ```powershell
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

    Aşağıdakine benzer bir çıktı görmeniz gerekir. Alt takiben "Geçti" alanı "True" olarak döndürülürse, akıl sağlığı denetimleri geçmiştir ve küme giriş yapılandırmasına göre dağıtılabilir gibi görünür.

    ```powershell
    Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
    Running Best Practices Analyzer...
    Best Practices Analyzer completed successfully.
    
    LocalAdminPrivilege        : True
    IsJsonValid                : True
    IsCabValid                 : True
    RequiredPortsOpen          : True
    RemoteRegistryAvailable    : True
    FirewallAvailable          : True
    RpcCheckPassed             : True
    NoConflictingInstallations : True
    FabricInstallable          : True
    Passed                     : True
    ```

2. Küme oluşturma: Service Fabric kümesini yapılandırmadaki her makineye dağıtmak için *CreateServiceFabricCluster.ps1* komut dosyasını çalıştırın. 
    ```powershell
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -AcceptEULA
    ```

> [!NOTE]
> Dağıtım izlemeleri, üzerinde CreateServiceFabricCluster.ps1 PowerShell betiğini çalıştırabileceğiniz VM/makineye yazılır. Bunlar, betiğin çalıştırıldığı dizine göre DeploymentTraces alt klasöründe bulunabilir. Service Fabric’in bir makineye doğru şekilde dağıtılıp dağıtılmadığını görmek için, FabricSettings bölümündeki küme yapılandırma dosyasında (varsayılan olarak c:\ProgramData\SF) ayrıntılı olarak açıklandığı gibi FabricDataRoot dizinindeki yüklü dosyaları bulun. Ayrıca, FabricHost.exe ve Fabric.exe işlemlerinin Görev Yöneticisi’nde çalıştığı görülebilir.
> 
> 

#### <a name="scenario-c-create-an-offline-internet-disconnected-cluster"></a>Senaryo C: Çevrimdışı (internet bağlantısı kesilmiş) bir küme oluşturma
Service Fabric çalışma zamanı paketi küme oluşturmada otomatik olarak indirilir. Bir kümeyi internete bağlı olmayan makinelere dağıtırken, Service Fabric çalışma zamanı paketini ayrı olarak indirmeniz ve küme oluşturmada bu küme oluşturma da bu pakete giden yolu sağlamanız gerekir.
Çalışma süresi paketi, Internet'e bağlı başka bir makineden, [Download Link - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)adresinden ayrı olarak indirilebilir. Çalışma zamanı paketini çevrimdışı kümeyi dağıttığınız yere kopyalayın ve `CreateServiceFabricCluster.ps1` bu `-FabricRuntimePackagePath` örnekte gösterildiği gibi parametre yle birlikte çalıştırarak kümeyi oluşturun: 

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -FabricRuntimePackagePath .\MicrosoftAzureServiceFabric.cab
```

*.\ClusterConfig.json* ve *.\MicrosoftAzureServiceFabric.cab* küme yapılandırmasına giden yollar ve çalışma zamanı .cab dosyası dır.

### <a name="step-2-connect-to-the-cluster"></a>Adım 2: Kümeye bağlan
Kümenin çalıştığını ve kullanılabilir olduğunu doğrulamak için kümeye bağlanın. ServiceFabric PowerShell modülü çalışma zamanıyla birlikte yüklenir.  Kümedüğümün birinden veya Service Fabric çalışma süresine sahip uzak bir bilgisayardan kümeye bağlanabilirsiniz.  [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet’i, kümeyle bir bağlantı kurar.

Güvenli olmayan bir kümeye bağlanmak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>
```

Örnek:
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000
```

Bir kümeye bağlanmayla ilgili diğer örnekler için bkz. [Güvenli bir kümeye bağlanma](service-fabric-connect-to-secure-cluster.md). Kümeye bağlandıktan sonra, kümedeki düğümlerin listesini ve her bir düğümün durum bilgilerini görüntülemek için [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet’ini kullanın. Her düğüm için **HealthState** değerinin *OK* olması gerekir.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- -------- --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     vm2      localhost       NodeType2 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm1      localhost       NodeType1 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm0      localhost       NodeType0 5.6.220.9494 0                     Up 00:02:43   00:00:00              OK
```

### <a name="step-3-visualize-the-cluster-using-service-fabric-explorer"></a>Adım 3: Service Fabric explorer'ı kullanarak kümeyi görselleştirin
[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), kümenizi görselleştirmek ve uygulamaları yönetmek için iyi bir araçtır.  Service Fabric Explorer, 'de gezinerek [http://localhost:19080/Explorer](http://localhost:19080/Explorer)bir tarayıcı kullanarak erişebildiğiniz kümede çalışan bir hizmettir.

Küme panosu, kümenize uygulama ve düğüm durumunun özetini de içeren bir genel bakış sağlar. Düğüm görünümü, kümenin fiziksel düzenini gösterir. Belirli bir düğümde, hangi uygulamalara kod dağıtıldığını denetleyebilirsiniz.

![Service Fabric Explorer][service-fabric-explorer]

## <a name="add-and-remove-nodes"></a>Düğüm ekleme ve kaldırma
İş gereksinimleriniz değiştikçe tek başına Service Fabric kümenize düğüm ekleyebilir veya kaldırabilirsiniz. Ayrıntılı adımlar için bkz. [Service Fabric tek başına kümesine düğüm ekleme veya kaldırma](service-fabric-cluster-windows-server-add-remove-nodes.md).

<a id="removecluster" name="removecluster_anchor"></a>
## <a name="remove-a-cluster"></a>Bir kümeyi kaldırma
Bir kümeyi kaldırmak için paket klasöründen *RemoveServiceFabricCluster.ps1* PowerShell betiğini çalıştırın ve yolu JSON yapılandırma dosyasına geçirin. İsteğe bağlı olarak silme işleminin günlüğü için bir konum belirtebilirsiniz.

Bu komut dosyası, küme yapılandırma dosyasında düğüm olarak listelenen tüm makinelere yönetici erişimi olan herhangi bir makinede çalıştırılabilir. Bu komut dosyasının çalıştırılan makine kümenin bir parçası olmak zorunda değildir.

```powershell
# Removes Service Fabric from each machine in the configuration
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -Force
```

```powershell
# Removes Service Fabric from the current machine
.\CleanFabric.ps1
```

<a id="telemetry"></a>

## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a>Toplanan Telemetri verileri ve nasıl devre dışı bırakmak
Varsayılan olarak, ürün ürünü geliştirmek için Service Fabric kullanımında telemetri toplar. Kurulumun bir parçası olarak çalışan En İyi Uygulama [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1)Çözümleyicisi' ne bağlantı için denetimler. Erişilemezse, telemetriyi devre dışı bırakmadığınız sürece kurulum başarısız olur.

1. Telemetri boru hattı aşağıdaki verileri [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) her gün bir kez yüklemeye çalışır. Bu en iyi çaba yükleme ve küme işlevselliği üzerinde hiçbir etkisi yoktur. Telemetri yalnızca başarısız yönetici birincil çalıştıran düğümgönderilir. Başka düğüm ler telemetri göndermez.
2. Telemetri aşağıdakilerden oluşur:

* Hizmet sayısı
* Hizmet Türleri Sayısı
* Başvuru Sayısı
* Uygulama SayısıYükseltmeleri
* FailoverUnits Sayısı
* InBuildFailoverUnits Sayısı
* SağlıksızFailoverUnits Sayısı
* Çoğaltma Sayısı
* InBuildReplicas sayısı
* StandByReplicas sayısı
* ÇevrimdışıÇoğaltma Sayısı
* CommonQueueLength
* QueryQueueLength
* FailoverUnitQueueLength
* CommitQueueLength
* Düğüm sayısı
* IsContextComplete: True/False
* ClusterId: Bu, her küme için rasgele oluşturulan bir GUID
* ServiceFabricVersion
* Telemetrinin yüklendiği sanal makine veya makinenin IP adresi

Telemetriyi devre dışı atmak için, küme config'inizdeki *özelliklere* aşağıdakileri ekleyin: *enableTelemetry: false*.

<a id="previewfeatures" name="previewfeatures_anchor"></a>

## <a name="preview-features-included-in-this-package"></a>Bu pakette yer alan önizleme özellikleri
Yok.


> [!NOTE]
> Windows Server [için bağımsız kümenin yeni GA sürümünden başlayarak (sürüm 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/)kümenizi el ile veya otomatik olarak gelecekteki sürümlere yükseltebilirsiniz. Ayrıntılar için bağımsız bir Service Fabric küme sürümü belgesini [yükseltmeye](service-fabric-cluster-upgrade-windows-server.md) bakın.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [PowerShell kullanarak uygulamaları dağıtma ve kaldırma](service-fabric-deploy-remove-applications.md)
* [Bağımsız Windows kümesi için yapılandırma ayarları](service-fabric-cluster-manifest.md)
* [Bağımsız Hizmet Kumaş ı kümesine düğüm ekleme veya kaldırma](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Bağımsız Service Fabric küme sürümünü yükseltme](service-fabric-cluster-upgrade-windows-server.md)
* [Windows çalıştıran Azure VM'leri ile bağımsız bir Hizmet Kumaşı kümesi oluşturma](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [Windows güvenliğini kullanarak Windows'da bağımsız bir küme emniyeti](service-fabric-windows-cluster-windows-security.md)
* [X509 sertifikalarını kullanarak Windows'da bağımsız bir küme emniyeti](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
[service-fabric-explorer]: ./media/service-fabric-cluster-creation-for-windows-server/sfx.png
