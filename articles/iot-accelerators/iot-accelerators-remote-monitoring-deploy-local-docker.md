---
title: Uzaktan İzleme çözümünü yerel olarak dağıtın - Docker - Azure | Microsoft Dokümanlar
description: Bu nasıl yapılılır kılavuzu, test ve geliştirme için Docker'ı kullanarak uzaktan izleme çözüm hızlandırıcısını yerel makinenize nasıl dağıtabileceğinizi gösterir.
author: avneet723
manager: hegate
ms.author: avneets
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: 32b47d9d6d45ff471961f55f8159dbe85eae2dce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73888829"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---docker"></a>Uzaktan İzleme çözüm hızlandırıcısını yerel olarak dağıtın - Docker

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

Bu makalede, uzaktan izleme çözüm hızlandırıcısını test ve geliştirme için yerel makinenize nasıl dağıtabileceğinizgösterilmektedir. Mikro servisleri yerel Docker konteynerlerine nasıl dağıtabileceğinizi öğrenirsiniz. Yerel bir mikro hizmetler dağıtımı aşağıdaki bulut hizmetlerini kullanır: Buluttaki IoT Hub, Cosmos DB, Azure Akış Analizi ve Azure Zaman Serisi Öngörüleri hizmetleri.

Uzaktan İzleme çözüm hızlandırıcısını yerel makinenizde bir IDE'de çalıştırmak istiyorsanız, [Uzaktan İzleme çözüm hızlandırıcısını yerel olarak dağıtın - Visual Studio](iot-accelerators-remote-monitoring-deploy-local.md)' a bakın.

## <a name="prerequisites"></a>Ön koşullar

Uzaktan İzleme çözüm hızlandırıcısı tarafından kullanılan Azure hizmetlerini dağıtmak için etkin bir Azure aboneliğine ihtiyacınız var.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Makine kurulumu

Yerel dağıtımı tamamlamak için, yerel geliştirme makinenize aşağıdaki araçların yüklenmesi gerekir:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Visual Studio](https://visualstudio.microsoft.com/) - Eğer mikro hizmetlerde değişiklik yapmayı planlıyorsanız.
* [Node.js v8](https://nodejs.org/) - Bu yazılım, komut dosyalarının Azure kaynakları oluşturmak için kullandığı PCS CLI için bir ön koşuldur. Node.js v10 kullanmayın.

> [!NOTE]
> Bu araçlar Windows, Linux ve iOS gibi birçok platformda kullanılabilir.

[!INCLUDE [iot-accelerators-local-setup](../../includes/iot-accelerators-local-setup.md)]

## <a name="run-the-microservices-in-docker"></a>Docker'da mikro hizmetleri çalıştırın

**Start.cmd** komut dosyası tarafından ayarlanan ortam değişkenlerine erişebilmek için yeni bir komut istemi açın. Windows'da, ortam değişkenlerinin aşağıdaki komutu çalıştırarak ayarlı olduğunu doğrulayabilirsiniz:

```cmd
set PCS
```

Komut, **start.cmd** komut dosyası tarafından ayarlanan tüm ortam değişkenlerini gösterir.

Docker'ın yerel makinenizde çalıştığını unutmayın.
> [!NOTE]
> Docker, Windows'ta çalışıyorsa [Linux kapsayıcıları](https://docs.docker.com/docker-for-windows/) çalıştırıyor olmalıdır.

Yerel Docker kapsayıcılarında çalışan mikro hizmetlerin Azure bulut hizmetlerine erişmeleri gerekir. Docker ortamınızın internet bağlantısını, bir konteynerin içinden bir internet adresi pinglemek için aşağıdaki komutu kullanarak test edebilirsiniz:

```cmd/sh
docker run --rm -ti library/alpine ping google.com
```

Çözüm hızlandırıcısını çalıştırmak için, komut satırı ortamınızdaki **hizmetler\\komut dosyası\\yerel** klasörüne gidin ve aşağıdaki komutu çalıştırın:

```cmd/sh
docker-compose up
```

> [!NOTE] 
> Çalıştırmadan `docker-compose up`önce Docker ile [yerel bir sürücüyü paylaştığınıza](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/issues/115) emin olun.

Bu komutu ilk çalıştırdığınızda Docker, konteynerleri yerel olarak oluşturmak için Docker hub'ından mikro hizmet görüntülerini indirir. Takip pistlerinde Docker konteynerleri hemen çalıştırZ.

> [!TIP]
> Microsoft sık sık yeni işlevlerle yeni Docker görüntüleri yayınlar. En son olanları çekmeden önce yerel Docker kaplarınızı ve ilgili görüntüleri temizlemek için aşağıdaki komut kümesini kullanabilirsiniz:

```cmd/sh
docker list
docker rm <list_of_containers>
docker rmi <list_of_images>
```

Kapsayıcıdan günlükleri görüntülemek için ayrı bir kabuk kullanabilirsiniz. Önce `docker ps` komutu kullanarak konteyner kimliğini bulun. Ardından, `docker logs {container-id} --tail 1000` belirtilen kapsayıcıiçin son 1000 girişi görüntülemek için kullanın.

### <a name="start-the-stream-analytics-job"></a>Akış Analizi işini başlatın

Akış Analizi işini başlatmak için aşağıdaki adımları izleyin:

1. [Azure portalına](https://portal.azure.com) gidin.
1. Çözümünüz için oluşturulan **Kaynak grubuna** gidin. Kaynak grubunun **adı, start.cmd** komut dosyasını çalıştırdığınızda çözümünüz için seçtiğiniz addır.
1. Kaynaklar **listesindeKi Akış Analizi işini** tıklatın.
1. Akış Analizi iş **genel bakış** sayfasında **Başlat** düğmesini tıklatın. Ardından işe başlamak için **Başlat'ı** tıklatın.

### <a name="connect-to-the-dashboard"></a>Panoya bağlanma

Uzaktan İzleme çözüm panosuna erişmek `http://localhost:8080` için tarayıcınızda gidin. Artık Web UI'sini ve yerel mikro hizmetleri kullanabilirsiniz.

## <a name="clean-up"></a>Temizleme

Gereksiz ücretleri önlemek için, testinizi tamamladığınızda bulut hizmetlerini Azure aboneliğinizden kaldırın. Hizmetleri kaldırmak için Azure [portalına](https://ms.portal.azure.com) gidin ve **start.cmd** komut dosyasının oluşturduğu kaynak grubunu silin.

Docker `docker-compose down --rmi all` görüntülerini kaldırmak ve yerel makinenizde yer açmak için komutu kullanın. Ayrıca, Kaynak Kodu GitHub'dan klonladığınızda oluşturulan Uzaktan İzleme deposunun yerel kopyasını da silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uzaktan İzleme çözümünü dağıttığınız için bir sonraki adım [çözüm panosunun yeteneklerini keşfetmektir.](quickstart-remote-monitoring-deploy.md)
