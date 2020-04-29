---
title: CLı-Azure kullanarak uzaktan Izleme çözümünü dağıtma | Microsoft Docs
description: Bu nasıl yapılır Kılavuzu, CLı kullanarak uzaktan Izleme çözüm hızlandırıcısının nasıl sağlanacağını gösterir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: conceptual
ms.openlocfilehash: 501ca51a9542229a14e98a56679837950a82891e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80258303"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-using-the-cli"></a>CLı kullanarak uzaktan Izleme çözüm Hızlandırıcısını dağıtma

Bu nasıl yapılır kılavuzunda, uzaktan Izleme çözümü hızlandırıcının nasıl dağıtılacağı gösterilmektedir. Çözümü CLı kullanarak dağıtırsınız. Çözümü, azureiotsolutions.com adresindeki Web tabanlı kullanıcı arabirimini kullanarak da dağıtabilirsiniz. Bu seçenek hakkında bilgi edinmek için [Uzaktan İzleme Çözüm Hızlandırıcısı](quickstart-remote-monitoring-deploy.md) hızlı başlangıcını Dağıtma bölümüne bakın.

## <a name="prerequisites"></a>Ön koşullar

Uzaktan Izleme çözüm Hızlandırıcısını dağıtmak için etkin bir Azure aboneliğine ihtiyacınız vardır.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

CLı 'yı çalıştırmak için [Node. js](https://nodejs.org/) ' nin yerel makinenizde yüklü olması gerekir.

## <a name="install-the-cli"></a>CLI’yı yükleme

CLı 'yı yüklemek için komut satırı ortamınızda aşağıdaki komutu çalıştırın:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>CLı 'da oturum açma

Çözüm hızlandırıcıyı dağıtabilmeniz için önce CLı kullanarak Azure aboneliğinizde oturum açmanız gerekir:

```cmd/sh
pcs login
```

Oturum açma işlemini gerçekleştirmek için ekrandaki yönergeleri izleyin.

## <a name="deployment-options"></a>Dağıtım seçenekleri

Çözüm hızlandırıcıyı dağıtırken, dağıtım işlemini yapılandıran çeşitli seçenekler vardır:

| Seçenek | Değerler | Açıklama |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | _Temel_ bir dağıtım test ve gösteriler için tasarlanan tüm mikro hizmetleri tek bir sanal makineye dağıtır. _Standart_ bir dağıtım üretime yöneliktir, mikro hizmetleri birkaç sanal makineye dağıtır. _Yerel_ bir dağıtım, bir Docker kapsayıcısını yerel makinenizde mikro hizmetleri çalıştıracak şekilde yapılandırır ve depolama ve Cosmos DB gibi Azure bulut hizmetlerini kullanır. |
| Çalışma Zamanı | `dotnet`, `java` | Mikro hizmetlerin dil uygulamasını seçer. |

Yerel dağıtım seçeneğini nasıl kullanacağınızı öğrenmek için bkz. [Uzaktan izleme çözümünü yerel olarak çalıştırma](iot-accelerators-remote-monitoring-deploy-local.md).

## <a name="basic-and-standard-deployments"></a>Temel ve standart dağıtımlar

Bu bölüm, temel ve standart dağıtım arasındaki önemli farklılıkları özetler.

### <a name="basic"></a>Temel

[Azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) adresınden veya CLI kullanarak temel bir dağıtım yapabilirsiniz.

Temel dağıtım, çözüm için büyük/küçük harfe yönelik olarak tasarlanmıştır. Maliyetleri azaltmak için tüm mikro hizmetler tek bir sanal makinede dağıtılır. Bu dağıtım, üretime yönelik kullanıma yönelik bir mimari kullanmaz.

Temel dağıtım, Azure aboneliğinizde aşağıdaki hizmetleri oluşturur:

| Sayı | Kaynak                       | Tür         | Kullanıldığı yer |
|-------|--------------------------------|--------------|----------|
| 1     | [Linux Sanal Makinesi](https://azure.microsoft.com/services/virtual-machines/) | Standart D1 v2  | Mikro hizmetleri barındırma |
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                  | S1 – Standart katman | Cihaz yönetimi ve iletişim |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)              | Standart        | Yapılandırma verilerini, kuralları, uyarıları ve diğer soğuk depolamayı depolama |  
| 1     | [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)  | Standart        | VM ve akış denetim noktaları için depolama |
| 1     | [Web uygulaması](https://azure.microsoft.com/services/app-service/web/)        |                 | Ön uç Web uygulaması barındırma |
| 1     | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)        |                 | Kullanıcı kimliklerini ve güvenliğini yönetme |
| 1     | [Azure Haritalar](https://azure.microsoft.com/services/azure-maps/)        | Standart                | Varlık konumlarını görüntüleme |
| 1     | [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)        |   3 birim              | Gerçek zamanlı analizi etkinleştirme |
| 1     | [Azure cihaz sağlama hizmeti](https://docs.microsoft.com/azure/iot-dps/)        |       S1          | Cihazları ölçekli olarak sağlama |
| 1     | [Azure Zaman Serisi Görüşleri](https://azure.microsoft.com/services/time-series-insights/)        |   S1 – 1 birim              | İleti verileri için depolama ve derin telemetri analizini mümkün |

### <a name="standard"></a>Standart

Standart bir dağıtımı yalnızca CLı kullanarak yapabilirsiniz.

Standart dağıtım, bir geliştiricinin özelleştirebileceği ve genişletebileceği üretime hazırlamış bir dağıtımdır. Ölçek ve genişletilebilirlik için tasarlanan, üretime hazır bir mimariyi özelleştirmeye hazır olduğunuzda standart dağıtım seçeneğini kullanın. Uygulama mikro hizmetleri Docker Kapsayıcıları olarak oluşturulmuştur ve Azure Kubernetes hizmeti kullanılarak dağıtılır. Kubernetes Orchestrator, mikro hizmetleri dağıtır, ölçeklendirir ve yönetir.

Standart dağıtım, Azure aboneliğinizde aşağıdaki hizmetleri oluşturur:

| Sayı | Kaynak                                     | SKU/boyut      | Kullanıldığı yer |
|-------|----------------------------------------------|-----------------|----------|
| 1     | [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service)| Tam olarak yönetilen bir Kubernetes kapsayıcı düzenleme hizmeti kullanın, varsayılan olarak 3 aracılardır|
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                     | S2 – Standart katman | Cihaz yönetimi, komut ve denetim |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)                 | Standart        | Yapılandırma verilerini depolama ve kurallar, uyarılar ve iletiler gibi cihaz telemetrisi |
| 5     | [Azure depolama hesapları](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)    | Standart        | VM depolaması için 4 ve akış denetim noktaları için 1 |
| 1     | [App Service](https://azure.microsoft.com/services/app-service/web/)             | S1 Standart     | TLS üzerinden uygulama ağ geçidi |
| 1     | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)        |                 | Kullanıcı kimliklerini ve güvenliğini yönetme |
| 1     | [Azure Haritalar](https://azure.microsoft.com/services/azure-maps/)        | Standart                | Varlık konumlarını görüntüleme |
| 1     | [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)        |   3 birim              | Gerçek zamanlı analizi etkinleştirme |
| 1     | [Azure cihaz sağlama hizmeti](https://docs.microsoft.com/azure/iot-dps/)        |       S1          | Cihazları ölçekli olarak sağlama |
| 1     | [Azure Zaman Serisi Görüşleri](https://azure.microsoft.com/services/time-series-insights/)        |   S1 – 1 birim              | İleti verileri için depolama ve derin telemetri analizini mümkün |

> [!NOTE]
> Bu hizmetlerin fiyatlandırma bilgilerini adresinde [https://azure.microsoft.com/pricing](https://azure.microsoft.com/pricing)bulabilirsiniz. [Azure portalında](https://portal.azure.com/)aboneliğiniz için kullanım ve faturalandırma ayrıntılarını bulabilirsiniz.

## <a name="deploy-the-solution-accelerator"></a>Çözüm hızlandırıcısını dağıtma

Dağıtım örnekleri:

### <a name="example-deploy-net-version"></a>Örnek: .NET sürümü dağıt

Aşağıdaki örnek, uzaktan Izleme çözümü hızlandırıcının temel, .NET sürümünün nasıl dağıtılacağını göstermektedir:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Örnek: Java sürümünü dağıtma

Aşağıdaki örnek, uzaktan Izleme çözümü hızlandırıcının standart, Java sürümünün nasıl dağıtılacağını göstermektedir:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>PC komut seçenekleri

Bir çözümü dağıtmak için `pcs` komutunu çalıştırdığınızda şunları yapmanız istenir:

- Çözümünüz için bir ad. Bu ad benzersiz olmalıdır.
- Kullanılacak Azure aboneliği.
- Bir konum.
- Mikro hizmetleri barındıran sanal makinelerin kimlik bilgileri. Sorun giderme amacıyla sanal makinelere erişmek için bu kimlik bilgilerini kullanabilirsiniz.

`pcs` Komut tamamlandığında, yeni çözüm HıZLANDıRıCıNıN URL 'sini görüntüler. `pcs` Komut Ayrıca, oluşturduğu IoT Hub adı `{deployment-name}-output.json` gibi bilgileri içeren bir dosya oluşturur.

Komut satırı parametreleri hakkında daha fazla bilgi için şunu çalıştırın:

```cmd/sh
pcs -h
```

CLı hakkında daha fazla bilgi için bkz. [CLI 'yi kullanma](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır kılavuzunda şunları öğrenirsiniz:

> [!div class="checklist"]
> * Çözüm hızlandırıcısını yapılandırma
> * Çözüm hızlandırıcısını dağıtma
> * Çözüm hızlandırıcısında oturum açın

Uzaktan Izleme çözümünü dağıttığınıza göre, bir sonraki adım [çözüm panosunun yeteneklerini araştırmakta](./quickstart-remote-monitoring-deploy.md).

<!-- Next how-to guides in the sequence -->
