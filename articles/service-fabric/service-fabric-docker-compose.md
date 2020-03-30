---
title: Azure Hizmet Kumaş Docker Oluşturma Dağıtım Önizleme
description: Azure Hizmet Kumaşı, Service Fabric kullanarak varolan kapsayıcıları düzenlemeyi kolaylaştırmak için Docker Compose biçimini kabul eder. Bu destek şu anda önizlemededir.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: f84dd0ecb7a4002182c8455bfd86354d794a6f7c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282464"
---
# <a name="docker-compose-deployment-support-in-azure-service-fabric-preview"></a>Docker Azure Hizmet Dokusunda dağıtım desteği oluşturun (Önizleme)

Docker, çoklu kapsayıcı uygulamalarını tanımlamak için [docker-compose.yml](https://docs.docker.com/compose) dosyasını kullanır. Docker'ı tanıyan müşterilerin Azure Hizmet Kumaşı'ndaki mevcut konteyner uygulamalarını düzenlemesini kolaylaştırmak için, Docker Compose dağıtımı için önizleme desteğini platforma dahil ettik. Service `docker-compose.yml` Fabric, sürüm 3 ve sonraki dosyaları kabul edebilir. 

Bu destek önizlemede olduğundan, yalnızca Bir Alt Oluşturma yönergeleri desteklenir.

Bu önizlemeyi kullanmak için, ilgili SDK ile birlikte Azure portalı üzerinden Hizmet Kumaşı çalışma süresinin 5,7 veya üzerinde sürümüyle kümenizi oluşturun. 

> [!NOTE]
> Bu özellik önizlemededir ve üretimde desteklenmez.
> Aşağıdaki örnekler çalışma zamanı sürüm 6.0 ve SDK sürüm 2.8'e dayanmaktadır.

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a>Hizmet Kumaşında Docker Oluştur dosyayı dağıtma

Aşağıdaki komutlar, diğer Service Fabric `fabric:/TestContainerApp`uygulamaları gibi izleyip yönetebileceğiniz bir Hizmet Kumaşı uygulaması (adlandırılmış) oluşturur. Sistem durumu sorguları için belirtilen uygulama adını kullanabilirsiniz.
Service Fabric, "DeploymentName"i Oluştur dağıtımının tanımlayıcısı olarak tanır.

### <a name="use-powershell"></a>PowerShell kullanma

PowerShell'de aşağıdaki komutu çalıştırarak docker-compose.yml dosyasından bir Hizmet Kumaşı Oluştur dağıtımı oluşturun:

```powershell
New-ServiceFabricComposeDeployment -DeploymentName TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

`RegistryUserName`ve `RegistryPassword` konteyner kayıt defteri kullanıcı adı ve şifrebakın. Dağıtımı tamamladıktan sonra, aşağıdaki komutu kullanarak durumunu denetleyebilirsiniz:

```powershell
Get-ServiceFabricComposeDeploymentStatus -DeploymentName TestContainerApp
```

PowerShell üzerinden Dağıtım Oluştur'u silmek için aşağıdaki komutu kullanın:

```powershell
Remove-ServiceFabricComposeDeployment  -DeploymentName TestContainerApp
```

PowerShell üzerinden dağıtım yükseltmesi oluşturmayı başlatmak için aşağıdaki komutu kullanın:

```powershell
Start-ServiceFabricComposeDeploymentUpgrade -DeploymentName TestContainerApp -Compose docker-compose-v2.yml -Monitored -FailureAction Rollback
```

PowerShell üzerinden Toplama dağıtım yükseltmesini geri almak için aşağıdaki komutu kullanın:

```powershell
Start-ServiceFabricComposeDeploymentRollback -DeploymentName TestContainerApp
```

Yükseltme kabul edildikten sonra, yükseltme ilerlemesi aşağıdaki komut kullanılarak izlenebilir:

```powershell
Get-ServiceFabricComposeDeploymentUpgrade -DeploymentName TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a>Azure Hizmet Kumaşı CLI (sfctl) kullanın

Alternatif olarak, aşağıdaki Service Fabric CLI komutunu kullanabilirsiniz:

```shell
sfctl compose create --deployment-name TestContainerApp --file-path docker-compose.yml [ [ --user --encrypted-pass ] | [ --user --has-pass ] ] [ --timeout ]
```

Dağıtımı oluşturduktan sonra, aşağıdaki komutu kullanarak durumunu denetleyebilirsiniz:

```shell
sfctl compose status --deployment-name TestContainerApp [ --timeout ]
```

Oluştur dağıtımını silmek için aşağıdaki komutu kullanın:

```shell
sfctl compose remove  --deployment-name TestContainerApp [ --timeout ]
```

Bir Dağıtım Oluştur yükseltmesini başlatmak için aşağıdaki komutu kullanın:

```shell
sfctl compose upgrade --deployment-name TestContainerApp --file-path docker-compose-v2.yml [ [ --user --encrypted-pass ] | [ --user --has-pass ] ] [--upgrade-mode Monitored] [--failure-action Rollback] [ --timeout ]
```

Toplama yükseltmesini geri almak için aşağıdaki komutu kullanın:

```shell
sfctl compose upgrade-rollback --deployment-name TestContainerApp [ --timeout ]
```

Yükseltme kabul edildikten sonra, yükseltme ilerlemesi aşağıdaki komut kullanılarak izlenebilir:

```shell
sfctl compose upgrade-status --deployment-name TestContainerApp
```

## <a name="supported-compose-directives"></a>Desteklenen Oluşturma yönergeleri

Bu önizleme, aşağıdaki ilkel ler de dahil olmak üzere Sürüm 3'ten yapılandırma seçeneklerinin bir alt kümesini destekler:

* > Çoğaltmaları Dağıtma> Hizmetler
* Hizmetler > > Yerleştirme > Kısıtlamaları Dağıtma
* > Kaynakları > Sınırları Dağıtma> Hizmetler
    * -cpu-hisse
    * -bellek
    * -bellek değiştirme
* Hizmetler > Komutanlıkları
* Hizmetler > Çevre
* Hizmetler > Limanlar
* Hizmetler > İmaj
* Yalıtım > Hizmetler (yalnızca Windows için)
* Hizmetler > Logging > Sürücüsü
* Hizmetler > Günlük > Sürücü > Seçenekleri
* Hacim & Dağıtma > Hacmi

[Service Fabric kaynak yönetiminde](service-fabric-resource-governance.md)açıklandığı gibi kaynak sınırlarını zorlamak için kümeyi ayarlayın. Diğer tüm Docker Compose yönergeleri bu önizleme için desteklenmez.

### <a name="ports-section"></a>Bağlantı noktaları bölümü

Service Fabric servis dinleyicisi tarafından kullanılacak Bağlantı Noktaları bölümünde http veya https protokolünü belirtin. Bu, ters proxy istekleri iletmek için izin vermek için adlandırma hizmeti ile bitiş noktası protokolü doğru yayımlanır sağlayacaktır:
* Güvenli olmayan Hizmet Kumaş Oluşturma hizmetlerine yönlendirmek için **/http**' i belirtin. Örneğin, - **"80:80/http"**.
* Güvenli Hizmet Kumaş Oluşturma hizmetlerine yönlendirmek için **/https**' yi belirtin. Örneğin, - **"443:443/https"**.

> [!NOTE]
> /http ve/https Ports bölümü sözdizimi, doğru Service Fabric dinleyici URL'sini kaydetmek için Service Fabric'e özgüdür.  Docker dosya sözdizimini oluşturan programlı olarak doğrulanırsa, doğrulama hatasına neden olabilir.

## <a name="servicednsname-computation"></a>ServiceDnsName hesaplama

Bir Oluştur dosyasında belirttiğiniz hizmet adı tam nitelikli bir etki alanı adıysa (diğer bir deyişle, bir nokta [.]içeriyorsa), Service Fabric tarafından kayıtlı DNS adı `<ServiceName>` (nokta dahil). Değilse, uygulama adındaki her yol kesimi hizmet DNS adında bir etki alanı etiketi olur ve ilk yol kesimi üst düzey etki alanı etiketi olur.

Örneğin, belirtilen uygulama `fabric:/SampleApp/MyComposeApp`adı, `<ServiceName>.MyComposeApp.SampleApp` kayıtlı DNS adı olacaktır.

## <a name="compose-deployment-instance-definition-versus-service-fabric-app-model-type-definition"></a>Dağıtım oluşturma (örnek tanımı) ve Service Fabric uygulama modeli (tür tanımı)

Docker-compose.yml dosyası, özellikleri ve yapılandırmaları da dahil olmak üzere dağıtılabilir bir kapsayıcı kümesini açıklar.
Örneğin, dosya ortam değişkenleri ve bağlantı noktaları içerebilir. Docker-compose.yml dosyasında yerleşim kısıtlamaları, kaynak sınırları ve DNS adları gibi dağıtım parametrelerini de belirtebilirsiniz.

[Service Fabric uygulama modeli,](service-fabric-application-model.md) aynı türde birçok uygulama örneğinin olabileceği hizmet türlerini ve uygulama türlerini kullanır. Örneğin, müşteri başına bir uygulama örneğiniz olabilir. Bu tür tabanlı model, çalışma süresine kayıtlı aynı uygulama türünün birden çok sürümüne destek sağlar.

Örneğin, A müşterisi AppTypeA'nın 1.0 türüyle anında bir uygulama yapabilir ve B müşterisi aynı tür ve sürümle anında başka bir uygulama kaplayabilir. Uygulama bildirimlerinde uygulama türlerini tanımlarsınız ve uygulamayı oluştururken uygulama adını ve dağıtım parametrelerini belirtirsiniz.

Bu model esneklik sunsa da, türlerin manifesto dosyasından örtülü olduğu daha basit, örnek tabanlı bir dağıtım modelini desteklemeyi de planlıyoruz. Bu modelde, her uygulama kendi bağımsız bildirimi alır. Örnek tabanlı dağıtım biçimi olan docker-compose.yml için destek ekleyerek bu çabayı önizliyoruz.

## <a name="next-steps"></a>Sonraki adımlar

* Service Fabric [uygulama modelini](service-fabric-application-model.md) okuyun
* [Service Fabric CLI kullanmaya başlama](service-fabric-cli.md)
