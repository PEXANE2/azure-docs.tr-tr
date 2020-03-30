---
title: CLI - Azure | Microsoft Dokümanlar
description: Bu nasıl yapılsın kılavuzu, CLI'yi kullanarak Uzaktan İzleme çözüm hızlandırıcısını nasıl sağabileceğinizi gösterir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.openlocfilehash: 501ca51a9542229a14e98a56679837950a82891e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80258303"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-using-the-cli"></a>CLI'yi kullanarak Uzaktan İzleme çözüm hızlandırıcısını dağıtın

Bu nasıl yapılılır kılavuzu, Uzaktan İzleme çözüm hızlandırıcısını nasıl dağıtabileceğinizi gösterir. CLI'yi kullanarak çözümü dağıtın. Ayrıca, bu seçenek hakkında bilgi edinmek için azureiotsolutions.com'da web tabanlı Kullanıcı Arabirimi'ni kullanarak çözümü dağıtabilirsiniz: [Uzaktan İzleme çözüm hızlandırıcısını hızla](quickstart-remote-monitoring-deploy.md) başlatın.

## <a name="prerequisites"></a>Ön koşullar

Uzaktan İzleme çözüm hızlandırıcısını dağıtmak için etkin bir Azure aboneliğine ihtiyacınız var.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

CLI'yi çalıştırmak için yerel makinenize [Node.js](https://nodejs.org/) yüklü olması gerekir.

## <a name="install-the-cli"></a>CLI’yı yükleme

CLI'yi yüklemek için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>CLI'de oturum açın

Çözüm hızlandırıcısını dağıtmadan önce CLI'yi kullanarak Azure aboneliğinizde oturum açmanız gerekir:

```cmd/sh
pcs login
```

Oturum açma işlemini tamamlamak için ekrandaki yönergeleri izleyin.

## <a name="deployment-options"></a>Dağıtım seçenekleri

Çözüm hızlandırıcısını dağıttığınızda, dağıtım işlemini yapılandıran birkaç seçenek vardır:

| Seçenek | Değerler | Açıklama |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | _Temel_ bir dağıtım test ve gösteriler için tasarlanmıştır, tek bir sanal makineye tüm mikro hizmetleri dağıtır. _Standart_ bir dağıtım üretim için tasarlanmıştır, çeşitli sanal makinelere mikro hizmetleri dağıtır. _Yerel_ dağıtım, yerel makinenizdeki mikro hizmetleri çalıştırmak için bir Docker kapsayıcısı yapılandırır ve depolama ve Cosmos DB gibi Azure bulut hizmetlerini kullanır. |
| Çalışma Zamanı | `dotnet`, `java` | Mikro hizmetlerin dil uygulamasını seçer. |

Yerel dağıtım seçeneğini nasıl kullanacağınızı öğrenmek için uzaktan [izleme çözümünü yerel olarak çalıştırma'ya](iot-accelerators-remote-monitoring-deploy-local.md)bakın.

## <a name="basic-and-standard-deployments"></a>Temel ve standart dağıtımlar

Bu bölümde, temel ve standart dağıtım arasındaki temel farklar özetlenmiştir.

### <a name="basic"></a>Temel

[Azureiotsolutions.com'dan](https://www.azureiotsolutions.com/Accelerators) veya CLI'yi kullanarak temel bir dağıtım yapabilirsiniz.

Temel dağıtım, çözümü sergilemeye yöneliktir. Maliyetleri azaltmak için, tüm mikro hizmetler tek bir sanal makinede dağıtılır. Bu dağıtım üretime hazır bir mimari kullanmaz.

Temel dağıtım, Azure aboneliğinizde aşağıdaki hizmetleri oluşturur:

| Sayı | Kaynak                       | Tür         | Için Kullanılır |
|-------|--------------------------------|--------------|----------|
| 1     | [Linux Sanal Makinesi](https://azure.microsoft.com/services/virtual-machines/) | Standart D1 V2  | Mikro hizmetlere ev sahipliği yapma |
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                  | S1 – Standart katman | Cihaz yönetimi ve iletişim |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)              | Standart        | Yapılandırma verilerini, kuralları, uyarıları ve diğer soğuk hava depolarını depolama |  
| 1     | [Azure Depolama Hesabı](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)  | Standart        | VM ve akış denetim noktaları için depolama |
| 1     | [Web Uygulaması](https://azure.microsoft.com/services/app-service/web/)        |                 | Ön uç web uygulamasını barındırma |
| 1     | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)        |                 | Kullanıcı kimliklerini ve güvenliğini yönetme |
| 1     | [Azure Haritalar](https://azure.microsoft.com/services/azure-maps/)        | Standart                | Varlık konumlarını görüntüleme |
| 1     | [Azure Akış Analizi](https://azure.microsoft.com/services/stream-analytics/)        |   3 adet              | Gerçek zamanlı analizleri etkinleştirme |
| 1     | [Azure Aygıt Sağlama Hizmeti](https://docs.microsoft.com/azure/iot-dps/)        |       S1          | Cihazları ölçekte sağlama |
| 1     | [Azure Zaman Serisi Öngörüleri](https://azure.microsoft.com/services/time-series-insights/)        |   S1 – 1 adet              | İleti verileri için depolama ve derin dalış telemetri analizi sağlar |

### <a name="standard"></a>Standart

Standart bir dağıtımı yalnızca CLI'yi kullanarak yapabilirsiniz.

Standart dağıtım, bir geliştiricinin özelleştirebileceği ve genişletebileceği üretime hazır bir dağıtımdır. Ölçek ve genişletilebilirlik için oluşturulmuş üretime hazır bir mimari özelleştirmeye hazır olduğunuzda standart dağıtım seçeneğini kullanın. Uygulama mikro hizmetleri Docker kapsayıcıları olarak oluşturulur ve Azure Kubernetes Hizmeti kullanılarak dağıtılır. Kubernetes orkestratör, mikro hizmetleri dağır, ölçeklendirin ve yönetir.

Standart bir dağıtım, Azure aboneliğinizde aşağıdaki hizmetleri oluşturur:

| Sayı | Kaynak                                     | SKU / Boyut      | Için Kullanılır |
|-------|----------------------------------------------|-----------------|----------|
| 1     | [Azure Kubernetes Servisi](https://azure.microsoft.com/services/kubernetes-service)| Tam olarak yönetilen bir Kubernetes konteyner düzenleme hizmetini kullanın, varsayılan olarak 3 aracı|
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                     | S2 – Standart katman | Cihaz yönetimi, komuta ve kontrol |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)                 | Standart        | Yapılandırma verilerini ve kurallar, uyarılar ve iletiler gibi aygıt telemetrilerini depolama |
| 5     | [Azure Depolama Hesapları](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)    | Standart        | VM depolama için 4 ve akış denetim noktaları için 1 |
| 1     | [App Service](https://azure.microsoft.com/services/app-service/web/)             | S1 Standart     | TLS üzerinden uygulama ağ geçidi |
| 1     | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)        |                 | Kullanıcı kimliklerini ve güvenliğini yönetme |
| 1     | [Azure Haritalar](https://azure.microsoft.com/services/azure-maps/)        | Standart                | Varlık konumlarını görüntüleme |
| 1     | [Azure Akış Analizi](https://azure.microsoft.com/services/stream-analytics/)        |   3 adet              | Gerçek zamanlı analizleri etkinleştirme |
| 1     | [Azure Aygıt Sağlama Hizmeti](https://docs.microsoft.com/azure/iot-dps/)        |       S1          | Cihazları ölçekte sağlama |
| 1     | [Azure Zaman Serisi Öngörüleri](https://azure.microsoft.com/services/time-series-insights/)        |   S1 – 1 adet              | İleti verileri için depolama ve derin dalış telemetri analizi sağlar |

> [!NOTE]
> Bu hizmetler için fiyatlandırma bilgilerini [https://azure.microsoft.com/pricing](https://azure.microsoft.com/pricing). Aboneliğinizin kullanım ve fatura ayrıntılarını [Azure Portalı'nda](https://portal.azure.com/)bulabilirsiniz.

## <a name="deploy-the-solution-accelerator"></a>Çözüm hızlandırıcısını dağıtma

Dağıtım örnekleri:

### <a name="example-deploy-net-version"></a>Örnek: deploy .NET sürümü

Aşağıdaki örnek, Uzaktan İzleme çözüm hızlandırıcısının temel ,.NET sürümünün nasıl dağıtılangerektiğini gösterir:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Örnek: Java sürümünü dağıtma

Aşağıdaki örnek, Uzaktan İzleme çözüm hızlandırıcısının standart, Java sürümünün nasıl dağıtılangerektiğini gösterir:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>pcs komut seçenekleri

Bir çözüm `pcs` dağıtmak için komutu çalıştırdığınızda, sizden aşağıdakileri istenir:

- Çözümün için bir isim. Bu ad benzersiz olmalıdır.
- Kullanılacak Azure aboneliği.
- Bir yer.
- Mikro hizmetleri barındıran sanal makinelerin kimlik bilgileri. Sorun giderme için sanal makinelere erişmek için bu kimlik bilgilerini kullanabilirsiniz.

`pcs` Komut bittiğinde, yeni çözüm hızlandırıcınızın URL'sini görüntüler. Komut `pcs` ayrıca, oluşturduğu `{deployment-name}-output.json` IoT Hub'ının adı gibi bilgiler içeren bir dosya da oluşturur.

Komut satırı parametreleri hakkında daha fazla bilgi için çalıştırın:

```cmd/sh
pcs -h
```

CLI hakkında daha fazla bilgi için [CLI'yi nasıl kullanacağına](https://github.com/Azure/pcs-cli/blob/master/README.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapIlir kılavuzunda, nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Çözüm hızlandırıcısını yapılandırma
> * Çözüm hızlandırıcısını dağıtma
> * Çözüm hızlandırıcıda oturum açın

Uzaktan İzleme çözümünü dağıttığınız için bir sonraki adım [çözüm panosunun yeteneklerini keşfetmektir.](./quickstart-remote-monitoring-deploy.md)

<!-- Next how-to guides in the sequence -->
