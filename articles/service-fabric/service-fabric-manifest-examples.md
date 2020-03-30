---
title: Azure Hizmet Kumaşı uygulama bildirim örnekleri
description: Hizmet Kumaşı uygulaması için uygulama ve hizmet bildirimi ayarlarını nasıl yapılandırıştırıyarız öğrenin.
author: peterpogorski
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: pepogors
ms.openlocfilehash: 6f44d2ddaac11545a3a3dc52fd20e02263c6ad67
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451635"
---
# <a name="service-fabric-application-and-service-manifest-examples"></a>Hizmet Kumaş uygulama ve hizmet manifestosu örnekleri
Bu bölümde uygulama ve hizmet bildirimlerinin örnekleri yer almaktadır. Bu örnekler önemli senaryoları göstermek için değil, kullanılabilir farklı ayarları ve bunların nasıl kullanılacağını göstermek içindir. 

Aşağıda gösterilen özelliklerin bir dizini ve bunların bir parçası oldukları örnek manifesto(lar) yer almaktadır.

|Özellik|Bildirim|
|---|---|
|[Kaynak idaresi](service-fabric-resource-governance.md)|[Güvenilir Hizmetler Başvuru bildirimi](service-fabric-manifest-example-reliable-services-app.md#application-manifest), [Konteyner Başvuru bildirimi](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Bir hizmeti yerel yönetici hesabı olarak çalıştırma](service-fabric-application-runas-security.md)|[Güvenilir Hizmetler Uygulama bildirimi](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Tüm hizmet kodu paketlerine varsayılan ilke uygulama](service-fabric-application-runas-security.md#apply-a-default-policy-to-all-service-code-packages)|[Güvenilir Hizmetler Uygulama bildirimi](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Kullanıcı ve grup ilkeleri oluşturma](service-fabric-application-runas-security.md)|[Güvenilir Hizmetler Uygulama bildirimi](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|hizmet örnekleri arasında bir veri paketi paylaşma|[Güvenilir Hizmetler Uygulama bildirimi](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Hizmet uç noktalarını geçersiz kılma](service-fabric-service-manifest-resources.md#overriding-endpoints-in-servicemanifestxml)|[Güvenilir Hizmetler Uygulama bildirimi](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Hizmet başlatmada komut dosyası çalıştırma](service-fabric-run-script-at-service-startup.md)|[OylamaWeb hizmet bildirimi](service-fabric-manifest-example-reliable-services-app.md#votingweb-service-manifest)|
|[HTTPS bitiş noktası tanımlama](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#define-an-https-endpoint-in-the-service-manifest)|[OylamaWeb hizmet bildirimi](service-fabric-manifest-example-reliable-services-app.md#votingweb-service-manifest)|
|[Config paketini bildir](service-fabric-application-and-service-manifests.md)|[VotingData hizmet bildirimi](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|[Veri paketini bildirme](service-fabric-application-and-service-manifests.md)|[VotingData hizmet bildirimi](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|[Ortam değişkenlerini geçersiz kılma](service-fabric-get-started-containers.md#configure-and-set-environment-variables)|[Konteyner Uygulama bildirimi](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Kapsayıcı bağlantı noktası-barındırış eşleme yapılandırma](service-fabric-get-started-containers.md#configure-container-port-to-host-port-mapping-and-container-to-container-discovery)| [Konteyner Uygulama bildirimi](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Kapsayıcı kayıt defteri kimlik doğrulamasını yapılandırma](service-fabric-get-started-containers.md#configure-container-repository-authentication)|[Konteyner Uygulama bildirimi](service-fabric-manifest-example-container-app.md#application-manifest)|
|[İzolasyon modunu ayarlama](service-fabric-get-started-containers.md#configure-isolation-mode)|[Konteyner Uygulama bildirimi](service-fabric-manifest-example-container-app.md#application-manifest)|
|[İşletim sistemi yapısına özgü kapsayıcı görüntülerini belirtin](service-fabric-get-started-containers.md#specify-os-build-specific-container-images)|[Konteyner Uygulama bildirimi](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Ortam değişkenlerini belirleme](service-fabric-get-started-containers.md#configure-and-set-environment-variables)|[Konteyner FrontEndService hizmet bildirimi](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest), [Konteyner BackEndService hizmet bildirimi](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)|
|[Bitiş noktasını yapılandırma](service-fabric-get-started-containers.md#configure-communication)|[Konteyner FrontEndService hizmet bildirimi](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest), [Konteyner BackEndService hizmet bildirimi](service-fabric-manifest-example-container-app.md#backendservice-service-manifest), [VotingData hizmet bildirimi](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|komutları kapsayıcıya geçirin|[Konteyner FrontEndService hizmet bildirimi](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)|
|[Sertifikayı kapsayıcıya alma](service-fabric-securing-containers.md)|[Konteyner FrontEndService hizmet bildirimi](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)|
|[Birim sürücüsünü yapılandırma](service-fabric-containers-volume-logging-drivers.md)|[Konteyner BackEndService hizmet bildirimi](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)|

