---
title: Gelişmiş Uygulama Yükseltme Konuları
description: Bu makalede, Bir Hizmet Kumaşı uygulamasını yükseltmeyle ilgili bazı gelişmiş konular ele al.)
ms.topic: conceptual
ms.date: 03/11/2020
ms.openlocfilehash: a12d2ec55bda95c1c61d4a73c76f4a777f4237f2
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414495"
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a>Hizmet Kumaş uygulama yükseltme: Gelişmiş konular

## <a name="add-or-remove-service-types-during-an-application-upgrade"></a>Uygulama yükseltmesi sırasında hizmet türleri ekleme veya kaldırma

Yayımlanmış bir uygulamaya yükseltmenin bir parçası olarak yeni bir hizmet türü eklenirse, dağıtılan uygulamaya yeni hizmet türü eklenir. Böyle bir yükseltme, uygulamanın zaten bir parçası olan hizmet örneklerinin hiçbirini etkilemez, ancak eklenen hizmet türünün etkin olması için yeni hizmet türünün oluşturulması gerekir (bkz. [Yeni HizmetFabricService).](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps)

Benzer şekilde, hizmet türleri yükseltmenin bir parçası olarak bir uygulamadan kaldırılabilir. Ancak, kaldırılacak hizmet türünün tüm hizmet örnekleri yükseltmeye devam etmeden önce kaldırılmalıdır [(bkz.](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricservice?view=azureservicefabricps)

## <a name="avoid-connection-drops-during-stateless-service-planned-downtime"></a>Planlanan hizmet kapalı kalma süresi sırasında bağlantı düşüşlerini önleyebilirsiniz

Uygulama/küme yükseltmesi veya düğüm devre dışı bırakma gibi planlı durum dışı örnek düşüş süreleri için, açıkolan uç nokta nın, örnek kapatıldıktan sonra kaldırılması nedeniyle bağlantılar bırakılabilir ve bu da zorunlu bağlantı kapatmalarına neden olur.

Bunu önlemek için, kümedeki diğer hizmetlerden istekler alırken ve Ters Proxy kullanırken veya uç noktaları güncelleştirmek için bildirim modeliyle çözümleme modelini kullanarak, hizmet yapılandırmasına bir *örnek yakın gecikme süresi* ekleyerek *RequestDrain* (önizleme) özelliğini yapılandırın. Bu, örneği kapatmadan önce gecikme başlamadan *önce,* durum dolmadan örnek tarafından reklamı yapılan bitiş noktasının kaldırılmasını sağlar. Bu gecikme, varolan isteklerin örnek gerçekten çökmeden önce incelikle tüketilmesini sağlar. İstemciler, bitiş noktasını yeniden çözebilmeleri ve kapanan örne yeni istekler göndermekten kaçınabilmeleri için, gecikmeyi başlatma sırasında bir geri arama işlevi tarafından bitiş noktası değişikliği bildirilir.

### <a name="service-configuration"></a>Hizmet yapılandırması

Hizmet tarafında gecikme yapılandırmak için çeşitli yollar vardır.

 * **Yeni bir hizmet oluştururken,** bir `-InstanceCloseDelayDuration`belirtin:

    ```powershell
    New-ServiceFabricService -Stateless [-ServiceName] <Uri> -InstanceCloseDelayDuration <TimeSpan>`
    ```

 * **Uygulama bildirimindeki varsayılanlar bölümündeki hizmeti tanımlarken,** `InstanceCloseDelayDurationSeconds` özelliği atayın:

    ```xml
          <StatelessService ServiceTypeName="Web1Type" InstanceCount="[Web1_InstanceCount]" InstanceCloseDelayDurationSeconds="15">
              <SingletonPartition />
          </StatelessService>
    ```

 * **Varolan bir hizmeti güncellerken,** aşağıdakileri `-InstanceCloseDelayDuration`belirtin:

    ```powershell
    Update-ServiceFabricService [-Stateless] [-ServiceName] <Uri> [-InstanceCloseDelayDuration <TimeSpan>]`
    ```

### <a name="client-configuration"></a>İstemci yapılandırması

Bir bitiş noktası değiştiğinde bildirim almak için, istemcilerin bir geri arama kaydettirmeleri gerekir: [ServiceNotificationFilterDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicenotificationfilterdescription).
Değişiklik bildirimi, uç noktaların değiştiğini, istemcinin uç noktaları yeniden çözmesi ve yakında aşağı ineceği için artık reklamı olmayan uç noktaları kullanmaması gerektiğinin bir göstergesidir.

### <a name="optional-upgrade-overrides"></a>İsteğe bağlı yükseltme geçersiz kılar

Hizmet başına varsayılan gecikme sürelerini ayarlamaya ek olarak, uygulama/küme yükseltmesi`InstanceCloseDelayDurationSec`sırasında aynı ( ) seçeneğini kullanarak gecikmeyi de geçersiz kılabilirsiniz:

```powershell
Start-ServiceFabricApplicationUpgrade [-ApplicationName] <Uri> [-ApplicationTypeVersion] <String> [-InstanceCloseDelayDurationSec <UInt32>]

Start-ServiceFabricClusterUpgrade [-CodePackageVersion] <String> [-ClusterManifestVersion] <String> [-InstanceCloseDelayDurationSec <UInt32>]
```

Gecikme süresi yalnızca çağrılan yükseltme örneği için geçerlidir ve tek tek hizmet gecikme yapılandırmalarını değiştirmez. Örneğin, önceden yapılandırılmış yükseltme gecikmelerini `0` atlamak için gecikme belirtmek için bunu kullanabilirsiniz.

> [!NOTE]
> Azure Yük bakiyesi talebinden gelen istekler için istekleri boşaltma ayarı yerine getirilmiş değildir. Arama hizmeti şikayete dayalı çözüm kullanıyorsa ayar onurlandırılır.
>
>

> [!NOTE]
> Bu özellik, küme kodu sürümü 7.1.XXX veya üzerinde olduğunda, yukarıda belirtildiği gibi Update-ServiceFabricService cmdlet kullanılarak mevcut hizmetlerde yapılandırılabilir.
>
>

## <a name="manual-upgrade-mode"></a>Manuel yükseltme modu

> [!NOTE]
> *İzlenen* yükseltme modu tüm Service Fabric yükseltmeleri için önerilir.
> *UnmonitoredManual* yükseltme modu yalnızca başarısız veya askıya alınan yükseltmeler için düşünülmelidir. 
>
>

*Denetlenen* modda, Hizmet Kumaşı, yükseltme ilerledikçe uygulamanın sağlıklı olduğundan emin olmak için sistem durumu ilkeleri uygular. Sistem durumu ilkeleri ihlal edilirse, yükseltme askıya alınır veya belirtilen *FailureAction'a*bağlı olarak otomatik olarak geri alınır.

*UnmonitoredManual* modunda, uygulama yöneticisi yükseltme nin ilerlemesi üzerinde tam denetime sahiptir. Bu mod, özel sistem durumu değerlendirme ilkeleri uygularken veya sistem durumu izlemeyi tamamen atlamak için konvansiyonel olmayan yükseltmeler gerçekleştirirken (örn. uygulama zaten veri kaybı ndadır) yararlıdır. Bu modda çalışan bir yükseltme, her UD'yi tamamladıktan sonra kendisini askıya alır ve [Özgeçmiş-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/resume-servicefabricapplicationupgrade?view=azureservicefabricps)kullanılarak açıkça devam edilmelidir. Yükseltme askıya alınDığında ve kullanıcı tarafından devam ettirilmeye hazır olduğunda, yükseltme durumu *RollforwardPending'u* gösterir [(bkz. UpgradeState).](https://docs.microsoft.com/dotnet/api/system.fabric.applicationupgradestate?view=azure-dotnet)

Son olarak, Kullanıcı girişi gerekmediğinden ve uygulama durumu ilkeleri değerlendirilmediğinden, *UnmonitoredAuto* modu hizmet geliştirme veya sınama sırasında hızlı yükseltme yinelemeleri gerçekleştirmek için yararlıdır.

## <a name="upgrade-with-a-diff-package"></a>Diff paketiyle yükseltme

Tam bir uygulama paketi sağlanması yerine, yükseltmeler, tam uygulama bildirimi ve tam hizmet bildirimlerinin yanı sıra yalnızca güncelleştirilmiş kod/config/veri paketlerini içeren diff paketleri sağlayarak da gerçekleştirilebilir. Tam uygulama paketleri yalnızca bir uygulamanın kümeye ilk yüklenmesi için gereklidir. Sonraki yükseltmeler tam uygulama paketlerinden veya diff paketlerinden olabilir.  

Uygulama paketinde bulunamayan bir diff paketinin uygulama bildiriminde veya hizmet bildirimlerinde yer alan herhangi bir başvuru otomatik olarak geçerli sürümle değiştirilir.

Diff paketi kullanmak için senaryolar şunlardır:

* Birden fazla hizmet bildirimi dosyasına ve/veya birkaç kod paketine, config paketlerine veya veri paketlerine atıfta bulunulan büyük bir uygulama paketine sahipseniz.
* Yapı düzenini doğrudan uygulama oluşturma işleminizden oluşturan bir dağıtım sisteminiz olduğunda. Bu durumda, kod değişmemiş olsa bile, yeni oluşturulan derlemeler farklı bir denetim etüdü alır. Tam bir uygulama paketi kullanmak, tüm kod paketlerindeki sürümü güncelleştirmenizi gerektirir. Diff paketini kullanarak, yalnızca değiştirilen dosyaları ve sürümün değiştiği manifesto dosyalarını sağlarsınız.

Visual Studio kullanılarak bir uygulama yükseltildiğinde, bir diff paketi otomatik olarak yayımlanır. Diff paketini el ile oluşturmak için uygulama bildirimi ve hizmet bildirimleri güncelleştirilmelidir, ancak yalnızca değiştirilen paketler son uygulama paketine eklenmelidir.

Örneğin, aşağıdaki uygulamayla başlayalım (anlama kolaylığı için sağlanan sürüm numaraları):

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Diff paketini kullanarak yalnızca hizmet1 kod paketini güncelleştirmek istediğinizi varsayalım. Güncelleştirilmiş uygulamanızda aşağıdaki sürüm değişiklikleri vardır:

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Bu durumda, kod paketi güncelleştirmesini yansıtacak şekilde uygulama bildirimini 2.0.0'a ve service1 hizmet bildirimini güncelleştirebilirsiniz. Uygulama paketinizin klasörü aşağıdaki yapıya sahip olacaktır:

```text
app1/
  service1/
    code/
```

Başka bir deyişle, normal olarak tam bir uygulama paketi oluşturun, ardından sürümün değişmediği kod/config/veri paketi klasörlerini kaldırın.

## <a name="upgrade-application-parameters-independently-of-version"></a>Uygulama parametrelerini sürümden bağımsız olarak yükseltme

Bazen, bildirim sürümünü değiştirmeden bir Hizmet Kumaşı uygulamasının parametrelerini değiştirmek istenir. Bu, **Start-ServiceFabricApplicationUpgrade** Azure Service Fabric PowerShell cmdlet ile **-ApplicationParameter** bayrağı kullanılarak rahatlıkla yapılabilir. Aşağıdaki özelliklere sahip bir Hizmet Kumaşı uygulamasını varsayalım:

```PowerShell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/Application1

ApplicationName        : fabric:/Application1
ApplicationTypeName    : Application1Type
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Ok
ApplicationParameters  : { "ImportantParameter" = "1"; "NewParameter" = "testBefore" }
```

Şimdi, **Başlat-ServiceFabricApplicationUpgrade** cmdlet kullanarak uygulamayı yükseltin. Bu örnek, izlenen bir yükseltmeyi gösterir, ancak izlenmeyen bir yükseltme de kullanılabilir. Bu cmdlet tarafından kabul edilen bayrakların tam açıklamasını görmek için [Azure Service Fabric PowerShell modül başvurusuna](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps#parameters) bakın

```PowerShell
PS C:\> $appParams = @{ "ImportantParameter" = "2"; "NewParameter" = "testAfter"}

PS C:\> Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/Application1 -ApplicationTypeVers
ion 1.0.0 -ApplicationParameter $appParams -Monitored

```

Yükseltmeden sonra, uygulamanın güncelleştirilmiş parametrelere ve aynı sürüme sahip olduğunu onaylayın:

```PowerShell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/Application1

ApplicationName        : fabric:/Application1
ApplicationTypeName    : Application1Type
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Ok
ApplicationParameters  : { "ImportantParameter" = "2"; "NewParameter" = "testAfter" }
```

## <a name="roll-back-application-upgrades"></a>Uygulama yükseltmelerini geri alma

Yükseltmeler üç moddan birinde *(İzlenen, İzlenmeyen* *Otomatik*Veya *İzlenmeyen El Kitabı)* ileri ye doğru yuvarlanabilirken, yalnızca *UnmonitoredAuto veya UnmonitoredManual* modunda geri alınabilir. *UnmonitoredManual* *UnmonitoredAuto* modunda geri rolling *YükseltmeReplicaSetCheckTimeout* varsayılan değeri farklı olması dışında ileri haddeleme aynı şekilde çalışır - [Uygulama Yükseltme Parametreleri](service-fabric-application-upgrade-parameters.md)bakın . *UnmonitoredManual* modunda geri yuvarlama ileri haddeleme aynı şekilde çalışır - geri alma her UD tamamladıktan sonra kendini askıya alacak ve açıkça geri alma ile devam etmek [için Resume-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/resume-servicefabricapplicationupgrade?view=azureservicefabricps) kullanarak devam edilmelidir.

Geri alma, *Geri* *Alma* *Eylemi* ile Izlenen moddaki bir yükseltmenin sistem durumu ilkeleri ihlal edildiğinde [(Bkz. Uygulama Yükseltme Parametreleri)](service-fabric-application-upgrade-parameters.md)veya [açıkça Start-ServiceFabricApplicationRollback](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationrollback?view=azureservicefabricps)kullanılarak otomatik olarak tetiklenebilir.

Geri alma sırasında, *UpgradeReplicaSetCheckTimeout* değeri ve modu hala [Güncelleme-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricapplicationupgrade?view=azureservicefabricps)kullanarak herhangi bir zamanda değiştirilebilir.

## <a name="next-steps"></a>Sonraki adımlar
[Visual Studio kullanarak Uygulama yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltme ile size yol.

[Powershell kullanarak Uygulamayükseltme PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) kullanarak bir uygulama yükseltme ile size yol.

[Yükseltme](service-fabric-application-upgrade-parameters.md)Parametreleri'ni kullanarak uygulamanızın nasıl yükselttikini kontrol edin.

[Veri Serileştirme'nin](service-fabric-application-upgrade-data-serialization.md)nasıl kullanılacağını öğrenerek uygulama yükseltmelerinizi uyumlu hale getirin.

[Sorun Giderme Uygulama Yükseltmeleri'ndeki](service-fabric-application-upgrade-troubleshooting.md)adımlara atıfta bulunarak uygulama yükseltmelerinde sık karşılaşılan sorunları giderin.
