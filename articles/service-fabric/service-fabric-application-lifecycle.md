---
title: Hizmet Kumaşında Uygulama Yaşam Döngüsü
description: Service Fabric uygulamalarının geliştirilmesini, dağıtılmasını, test edilmesini, yükseltilmesinin, sürdürülmesini ve kaldırılmasını açıklar.
ms.topic: conceptual
ms.date: 1/19/2018
ms.openlocfilehash: beeb1f1512cf94582dd561fa768f2e8e6649d686
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75378013"
---
# <a name="service-fabric-application-lifecycle"></a>Service Fabric uygulaması yaşam döngüsü
Diğer platformlarda olduğu gibi, Azure Hizmet Kumaşı'ndaki bir uygulama genellikle aşağıdaki aşamalardan geçer: tasarım, geliştirme, test, dağıtım, yükseltme, bakım ve kaldırma. Service Fabric, geliştirmeden dağıtıma, günlük yönetime ve bakıma ve nihai kullanımdan kaldırmaya kadar bulut uygulamalarının tam uygulama yaşam döngüsü için birinci sınıf destek sağlar. Hizmet modeli, uygulama yaşam döngüsüne bağımsız olarak birkaç farklı rolün katılmasını sağlar. Bu makalede, API'ler ve bunların Hizmet Kumaşı uygulama yaşam döngüsünün aşamalarında farklı roller tarafından nasıl kullanıldığına genel bir bakış sunulmaktadır.

[!INCLUDE [links to azure cli and service fabric cli](../../includes/service-fabric-sfctl.md)]

## <a name="service-model-roles"></a>Hizmet modeli rolleri
Hizmet modeli rolleri şunlardır:

* **Hizmet geliştiricisi**: Aynı türde veya farklı türde birden fazla uygulamada yeniden tasarlanmış ve kullanılabilen modüler ve genel hizmetler geliştirir. Örneğin, bir kuyruk hizmeti bir bilet uygulaması (yardım masası) veya bir e-ticaret uygulaması (alışveriş sepeti) oluşturmak için kullanılabilir.
* **Uygulama geliştiricisi**: Belirli belirli gereksinimleri veya senaryoları karşılamak için bir hizmet koleksiyonunu tümleştirerek uygulamalar oluşturur. Örneğin, bir e-ticaret web sitesi açık artırma çözümü oluşturmak için "JSON State-End Service", "Auction Stateful Service" ve "Queue Stateful Service"i entegre edebilir.
* **Uygulama yöneticisi**: Uygulama yapılandırması (yapılandırma şablon parametrelerinin doldurulması), dağıtım (kullanılabilir kaynaklara eşleme) ve hizmet kalitesi ile ilgili kararlar verir. Örneğin, bir uygulama yöneticisi uygulamanın dil yerel sini (ABD için İngilizce veya Japonya için Japonca) karar verir. Farklı bir dağıtılan uygulama farklı ayarlara sahip olabilir.
* **Operatör**: Uygulamaları uygulama yöneticisi tarafından belirtilen uygulama yapılandırması ve gereksinimlerine göre dağıtır. Örneğin, bir işleç uygulamayı karşılar ve dağıtır ve Azure'da çalışmasını sağlar. Operatörler uygulama durumu ve performans bilgilerini izler ve gerektiğinde fiziksel altyapıyı korur.

## <a name="develop"></a>Geliştirme
1. Bir *hizmet geliştiricisi,* Güvenilir [Aktörler](service-fabric-reliable-actors-introduction.md) veya [Güvenilir Hizmetler](service-fabric-reliable-services-introduction.md) programlama modelini kullanarak farklı hizmet türleri geliştirir.
2. Bir *hizmet geliştiricisi,* bir veya daha fazla kod, yapılandırma ve veri paketlerinden oluşan bir hizmet bildirimi dosyasında geliştirilen hizmet türlerini açıklar.
3. Bir *uygulama geliştiricisi* daha sonra farklı hizmet türlerini kullanarak bir uygulama oluşturur.
4. Bir *uygulama geliştiricisi,* kurucu hizmetlerin hizmet bildirimlerine başvurarak ve kurucu hizmetlerin farklı yapılandırma ve dağıtım ayarlarını uygun şekilde geçersiz kılarak ve parametrelendirerek uygulama türünü bir uygulama bildiriminde açıklar.

Bkz. [Güvenilir Aktörler le başlayın](service-fabric-reliable-actors-get-started.md) ve örnekler için Güvenilir [Hizmetlerle başlayın.](service-fabric-reliable-services-quick-start.md)

## <a name="deploy"></a>Dağıtma
1. Uygulama *yöneticisi,* uygulama bildiriminde **ApplicationType** öğesinin uygun parametrelerini belirterek uygulama türünü Hizmet Dokusu kümesine dağıtılacak belirli bir uygulamaya uyarlar.
2. Bir *operatör* [ **CopyApplicationPackage** yöntemini](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) veya [ **Copy-ServiceFabricApplicationPackage** cmdlet](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps)kullanarak uygulama paketini küme görüntü deposuna yükler. Uygulama paketi, uygulama bildirimini ve hizmet paketlerinin toplanmasını içerir. Service Fabric, azure blob deposu veya Service Fabric sistem hizmeti olabilecek uygulama paketindeki uygulamaları görüntü deposunda depolanır.
3. *Operatör* daha sonra [ **ProvisionApplicationAsync** yöntemi,](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) [ **Register-ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/register-servicefabricapplicationtype)veya [ **Provision a Application** REST işlemi](https://docs.microsoft.com/rest/api/servicefabric/provision-an-application)kullanarak yüklenen uygulama paketinden hedef kümedeki uygulama türünü karşılar.
4. Uygulamayı sağladıktan sonra, bir *operatör* [ **CreateApplicationAsync** yöntemini](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient)kullanarak *uygulama yöneticisi* tarafından sağlanan parametrelerle uygulamayı başlatır, Yeni [ **ServisKumaşUygulama** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricapplication)veya Create [ **Application** REST işlemi.](https://docs.microsoft.com/rest/api/servicefabric/create-an-application)
5. Uygulama dağıtıldıktan sonra, bir *operatör* kullanılabilir hizmet türlerine dayalı olarak uygulama için yeni hizmet örnekleri oluşturmak için [ **CreateServiceAsync** yöntemini](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient), [ **Yeni ServisFabricService** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice)veya Create [ **Service** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/create-a-service) kullanır.
6. Uygulama artık Service Fabric kümesinde çalışıyor.

Bkz. Örnekler için [bir uygulama dağıt.](service-fabric-deploy-remove-applications.md)

## <a name="test"></a>Test
1. Yerel geliştirme kümesine veya bir test kümesine dağıtıldıktan sonra, bir *hizmet geliştiricisi* [**FailoverTestScenarioParameters**](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.failovertestscenarioparameters) ve [**FailoverTestScenario**](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.failovertestscenario) sınıflarını veya [ **Invoke-ServiceFabricFailoverTestScenario** cmdlet'i](/powershell/module/servicefabric/invoke-servicefabricfailovertestscenario?view=azureservicefabricps)kullanarak yerleşik başarısız test senaryosunu çalıştırın. Başarısız test senaryosu, hala kullanılabilir ve çalıştığından emin olmak için önemli geçişler ve başarısızlar aracılığıyla belirli bir hizmeti çalıştırın.
2. *Hizmet geliştiricisi* daha sonra [**ChaosTestScenarioParameters**](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.chaostestscenarioparameters) ve [**ChaosTestScenario**](https://docs.microsoft.com/dotnet/api/system.fabric.testability.scenario.chaostestscenario) sınıfları veya [ **Invoke-ServiceFabricChaosTestScenario** cmdlet](/powershell/module/servicefabric/invoke-servicefabricchaostestscenario?view=azureservicefabricps)kullanarak yerleşik kaos test senaryosu çalışır. Kaos testi senaryosu rasgele kümeye birden çok düğüm, kod paketi ve yineleme hataları neden olur.
3. *Hizmet geliştiricisi,* birincil yinelemeleri kümenin etrafında hareket ettiren test senaryoları yazarak [hizmete iletişimi sınar.](service-fabric-testability-scenarios-service-communication.md)

Daha fazla bilgi için [Hata Analizi Hizmetine Giriş'e](service-fabric-testability-overview.md) bakın.

## <a name="upgrade"></a>Yükseltme
1. Bir *hizmet geliştiricisi,* anlık uygulamanın kurucu hizmetlerini güncelleştirir ve/veya hataları giderir ve hizmet bildiriminin yeni bir sürümünü sağlar.
2. Bir *uygulama geliştiricisi,* tutarlı hizmetlerin yapılandırma ve dağıtım ayarlarını geçersiz kılar ve parametreye getirir ve uygulama bildiriminin yeni bir sürümünü sağlar. Uygulama geliştiricisi daha sonra hizmetin yeni sürümlerini uygulamaya dahil eder ve güncelleştirilmiş bir uygulama paketinde uygulama türünün yeni bir sürümünü sağlar.
3. Bir *uygulama yöneticisi,* uygun parametreleri güncelleştirerek uygulama türünün yeni sürümünü hedef uygulamaya dahil eder.
4. Bir *operatör,* güncellenen uygulama paketini [ **CopyApplicationPackage** yöntemini](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) veya [ **Copy-ServiceFabricApplicationPackage** cmdlet'i](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps)kullanarak küme görüntüsü deposuna yükler. Uygulama paketi, uygulama bildirimini ve hizmet paketlerinin toplanmasını içerir.
5. Bir *operatör* [ **ProvisionApplicationAsync** yöntemi](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient)ni kullanarak hedef kümedeki uygulamanın yeni sürümünü, [ **Register-ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/register-servicefabricapplicationtype)veya Provision a [ **Application** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/provision-an-application)kullanarak sağlar.
6. Bir *operatör* [ **YükseltmeApplicationAsync** yöntemi,](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) [ **Start-ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade)veya [ **Yükseltme bir Uygulama** REST işlemi](https://docs.microsoft.com/rest/api/servicefabric/upgrade-an-application)kullanarak yeni sürüme hedef uygulama yükseltir.
7. Bir *operatör* [ **GetApplicationUpgradeProgressAsync** yöntemi,](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) [ **Get-ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricapplicationupgrade)veya [ **Uygulama Yükseltme İlerleme** REST işlemi alın](https://docs.microsoft.com/rest/api/servicefabric/get-the-progress-of-an-application-upgrade1)kullanarak yükseltme ilerlemesini denetler.
8. Gerekirse, *operatör* değiştirir ve [ **UpdateApplicationUpgradeAsync** yöntemi,](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) [ **Güncelleme-ServiceFabricApplicationUpgrade** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricapplicationupgrade)veya [ **Güncelleme Uygulama Yükseltme** REST işlemi](https://docs.microsoft.com/rest/api/servicefabric/update-an-application-upgrade)kullanarak geçerli uygulama yükseltme parametrelerini yeniden uygular.
9. Gerekirse, *operatör* [ **RollbackApplicationUpgradeAsync** yöntemi,](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) [ **Start-ServiceFabricApplicationRollback** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationrollback)veya [ **Rollback Application Yükseltme** REST işlemi](https://docs.microsoft.com/rest/api/servicefabric/rollback-an-application-upgrade)kullanarak geçerli uygulama yükseltme geri rulo .
10. Service Fabric, herhangi bir kurucu hizmetinin kullanılabilirliğini kaybetmeden kümede çalışan hedef uygulamayı yükseltir.

Örnekler için Uygulama yükseltme öğreticisi'ne bakın. [Application upgrade tutorial](service-fabric-application-upgrade-tutorial.md)

## <a name="maintain"></a>Bakım
1. İşletim sistemi yükseltmeleri ve yamaları için, Cluster'da çalışan tüm uygulamaların kullanılabilirliğini garanti etmek için Hizmet Yapısı, Azure altyapısıyla arabirimini aralar.
2. Service Fabric platformundaki yükseltmeler ve yamalar için Service Fabric, kümeüzerinde çalışan uygulamaların herhangi birinin kullanılabilirliğini kaybetmeden kendisini yükseltir.
3. Bir *uygulama yöneticisi,* geçmiş kapasite kullanım verilerini ve öngörülen gelecekteki talebi çözümledikten sonra düğümlerin kümeden eklenmesini veya kaldırılmasını onaylar.
4. *İşleç,* *uygulama yöneticisi*tarafından belirtilen düğümleri ekler ve kaldırır.
5. Kümeye yeni düğümler eklendiğinde veya varolan düğümler kümeden çıkarıldığında, Service Fabric en iyi performansı elde etmek için kümedeki tüm düğümler arasında çalışan uygulamaları otomatik olarak yükler.

## <a name="remove"></a>Kaldır
1. *İşleç,* [ **DeleteServiceAsync** yöntemini,](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) [ **Remove-ServiceFabricService** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricservice)veya [ **Delete Service** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/delete-a-service)kullanarak uygulamanın tamamını kaldırmadan kümedeki çalışan bir hizmetin belirli bir örneğini silebilir.  
2. Bir *operatör* ayrıca [ **deleteApplicationAsync** yöntemini,](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) [ **Remove-ServiceFabricApplication** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricapplication)veya [ **Delete Application** REST işlemini](https://docs.microsoft.com/rest/api/servicefabric/delete-an-application)kullanarak bir uygulama örneğini ve tüm hizmetlerini silebilir.
3. Uygulama ve hizmetler durdurulduktan sonra, *operatör* [ **UnprovisionApplicationAsync** yöntemini](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient)kullanarak uygulama türünü unprovision olabilir , [ **Unregister-ServiceFabricApplicationType** cmdlet](https://docs.microsoft.com/powershell/module/servicefabric/unregister-servicefabricapplicationtype), veya [ **Unprovision bir Uygulama** REST işlemi](https://docs.microsoft.com/rest/api/servicefabric/unprovision-an-application). Uygulama türünü sağlamamak, uygulama paketini ImageStore'dan kaldırmaz. Uygulama paketini el ile kaldırmanız gerekir.
4. Bir *operatör* Uygulama paketini [ **RemoveApplicationPackage** yöntemini](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.applicationmanagementclient) veya [ **Remove-ServiceFabricApplicationPackage** cmdlet'i](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps)kullanarak ImageStore'dan kaldırır.

Bkz. Örnekler için [bir uygulama dağıt.](service-fabric-deploy-remove-applications.md)

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric uygulamaları ve hizmetleri geliştirme, test etme ve yönetme hakkında daha fazla bilgi için bkz:

* [Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Reliable Services](service-fabric-reliable-services-introduction.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)
* [Uygulama yükseltme](service-fabric-application-upgrade.md)
* [Test edilebilirlik genel bakış](service-fabric-testability-overview.md)
