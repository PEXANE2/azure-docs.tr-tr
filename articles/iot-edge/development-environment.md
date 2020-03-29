---
title: Azure IoT Edge geliştirme ortamı | Microsoft Dokümanlar
description: IoT Edge modülleri oluşturmanıza yardımcı olacak desteklenen sistemler ve birinci taraf geliştirme araçları hakkında bilgi edinin
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/04/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 96078be20e8048e481a994fefc169e48ab1d8459
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76511102"
---
# <a name="prepare-your-development-and-test-environment-for-iot-edge"></a>Geliştirme ve test ortamınızı IoT Edge için hazırlayın

Azure IoT Edge, varolan iş mantığınızı kenarda çalışan aygıtlara taşır. Uygulamalarınızı ve iş yüklerinizi [IoT Edge modülleri](iot-edge-modules.md)olarak çalışacak şekilde hazırlamak için bunları kapsayıcı olarak oluşturmanız gerekir. Bu makalede, başarıyla bir IoT Edge çözümü oluşturabilmeniz için geliştirme ortamınızı nasıl yapılandırabileceğiniz konusunda kılavuzluk sağlanmaktadır. Geliştirme ortamınızı kurduktan sonra [kendi IoT Edge modüllerinizi](module-development.md)nasıl geliştirebileceğinizi öğrenebilirsiniz.

Herhangi bir IoT Edge çözümünde, göz önünde bulundurulması gereken en az iki makine vardır. Bunlardan biri, IoT Edge modüllerini çalıştıran IoT Edge aygıtının (veya aygıtların) kendisidir. Diğeri, modüloluşturmak, test etmek ve dağıtmak için kullandığınız geliştirme makinesidir. Bu makalede, öncelikle geliştirme makinesi üzerinde duruluyor. Test amacıyla, iki makine aynı olabilir. IoT Edge'i geliştirme makinenizde çalıştırabilir ve modülleri ona dağıtabilirsiniz.

## <a name="operating-system"></a>İşletim sistemi

Azure IoT Edge, desteklenen belirli bir [işletim sistemi](support.md)kümesinde çalışır. IoT Edge için geliştirme için, bir konteyner motoru çalıştırabilirsiniz en işletim sistemleri kullanabilirsiniz. Konteyner motoru, modüllerinizi konteyner olarak oluşturmak ve bunları bir konteyner kayıt defterine itmek için geliştirme makinesinin bir gereğidir.

Geliştirme makineniz Azure IoT Edge'i çalıştıramıyorsa, yerel olarak test etmenize ve hata ayıklamanıza yardımcı olan [test araçları](#testing-tools) hakkında bilgi edinmek için bu makaleye devam edin.

Geliştirme makinenizin işletim sistemi, IoT Edge cihazınızın işletim sistemiyle eşleşmesine gerek yoktur. Ancak, konteyner işletim sistemi geliştirme makinesi ve IoT Edge aygıtı arasında tutarlı olmalıdır. Örneğin, bir Windows makinesinde modüller geliştirebilir ve bunları bir Linux aygıtına dağıtabilirsiniz. Windows makinesi Linux cihazı için modülleri oluşturmak için Linux kapsayıcıları çalıştırmak gerekir.

## <a name="container-engine"></a>Konteyner motoru

IoT Edge'in temel konsepti, işletmenizi ve bulut mantığınızı konteynerlere paketleyerek cihazlara uzaktan dağıtabilmektir. Konteyner oluşturmak için geliştirme makinenizde bir konteyner motoruna ihtiyacınız var.

Üretimde IoT Edge cihazları için desteklenen tek konteyner motoru Moby'dir. Ancak, Docker gibi Açık Konteyner Girişimi ile uyumlu herhangi bir konteyner motoru, IoT Edge modül görüntüleri oluşturma yeteneğine sahiptir.

## <a name="development-tools"></a>Geliştirme araçları

Hem Visual Studio hem de Visual Studio Code, IoT Edge çözümleri geliştirmeye yardımcı olacak eklenti uzantılarına sahiptir. Bu uzantılar, yeni IoT Edge senaryoları oluşturmaya ve dağıtmaya yardımcı olmak için dile özgü şablonlar sağlar. Visual Studio ve Visual Studio Code için Azure IoT Edge uzantıları, IoT Edge çözümlerinizi kodlamanıza, oluşturmanıza, dağıtmanıza ve hata ayıklamanıza yardımcı olur. Birden çok modül içeren bir IoT Edge çözümü oluşturabilirsiniz ve uzantılar her yeni modül ekiyle birlikte dağıtım bildirimi şablonuna otomatik olarak güncellenir. Uzantıları ile IoT cihazlarınızı Visual Studio veya Visual Studio Code içinden de yönetebilirsiniz. Modülleri bir aygıta dağıtın, durumu izleyin ve iletileri IoT Hub'a geldiklerinde görüntüleyin. Her iki uzantı da geliştirme makinenizde modüllerin yerel olarak çalıştırılmasını ve hata ayıklanmasını sağlamak için [IoT EdgeHub dev aracını](#iot-edgehub-dev-tool) kullanır.

Diğer düzenleyicilerle veya CLI'den geliştirmeyi tercih ederseniz, Azure IoT Edge geliştirme aracı komut satırından geliştirip test edilebilmeniz için komutlar sağlar. Yeni IoT Edge senaryoları oluşturabilir, modül görüntüleri oluşturabilir, modülleri simülatörde çalıştırabilir ve IoT Hub'a gönderilen iletileri izleyebilirsiniz.

### <a name="visual-studio-code-extension"></a>Visual Studio Code uzantısı

Visual Studio Code için Azure IoT Edge uzantısı, C, C#, Java, Node.js ve Python gibi programlama dillerinin yanı sıra C#'daki Azure işlevleri üzerine oluşturulmuş IoT Edge modül şablonları sağlar.

Daha fazla bilgi ve indirmek [için Visual Studio Code için Azure IoT Araçları'na](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)bakın.

IoT Edge uzantılarına ek olarak, geliştirme için ek uzantılar yüklemeyi yararlı bulabilirsiniz. Örneğin, resimlerinizi, kapsayıcılarınızı ve kayıt defterlerinizi yönetmek [için Görsel Stüdyo Kodu için Docker Desteği'ni](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) kullanabilirsiniz. Ayrıca, tüm büyük desteklenen dillerde, modüller geliştirirken yardımcı olabilecek Visual Studio Code uzantıları vardır.

#### <a name="prerequisites"></a>Ön koşullar

Bazı diller ve hizmetler için modül şablonları, Visual Studio Code ile geliştirme makinenizde proje klasörleri oluşturmak için gerekli olan ön koşullara sahiptir.

| Modül şablonu | Önkoşul |
| --------------- | ------------ |
| Azure İşlevleri | [.NET Çekirdek 2.1 SDK](https://www.microsoft.com/net/download) |
| C | [Git](https://git-scm.com/) |
| C# | [.NET Çekirdek 2.1 SDK](https://www.microsoft.com/net/download) |
| Java | <ul><li>[Java SE Geliştirme Seti 10](https://aka.ms/azure-jdks) <li> [JAVA_HOME ortamı değişkenini ayarlama](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) <li> [Maven](https://maven.apache.org/)</ul> |
| Node.js | <ul><li>[Node.js](https://nodejs.org/) <li> [Yaşar](https://www.npmjs.com/package/yo) <li> [Azure IoT Edge Düğüm.js modül jeneratörü](https://www.npmjs.com/package/generator-azure-iot-edge-module)</ul> |
| Python |<ul><li> [Python](https://www.python.org/downloads/) <li> [Pip](https://pip.pypa.io/en/stable/installing/#installation) <li> [Git](https://git-scm.com/) </ul> |

### <a name="visual-studio-20172019-extension"></a>Visual Studio 2017/2019 uzantısı

Visual Studio için Azure IoT Edge araçları, C# ve C üzerine oluşturulmuş bir IoT Edge modül şablonu sağlar.

Daha fazla bilgi ve indirmek [için Visual Studio 2017 için Azure IoT Edge Araçları](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) veya Visual Studio [2019 için Azure IoT Edge Araçları'na](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools)bakın.

### <a name="iot-edge-dev-tool"></a>IoT Edge geliştirme aracı

Azure IoT Edge geliştirme aracı, komut satırı yetenekleriyle IoT Edge geliştirmeyi kolaylaştırır. Bu araç, modülleri geliştirmek, hata ayıklamak ve test etmek için CLI komutları sağlar. IoT Edge geliştirme aracı, makinenizdeki bağımlılıkları el ile yüklemiş veya IoT Edge dev konteynerini kullanıyor olun, geliştirme sisteminizle birlikte çalışır.

Daha fazla bilgi ve başlamak için, [IoT Edge dev aracı wiki](https://github.com/Azure/iotedgedev/wiki)bakın.

## <a name="testing-tools"></a>Test araçları

IoT Edge aygıtlarını veya hata ayıklama modüllerini daha verimli bir şekilde simüle etmenize yardımcı olacak çeşitli test araçları vardır. Aşağıdaki tablo, araçlar arasında üst düzey bir karşılaştırma gösterir ve sonra tek tek bölümler her aracı daha özel olarak açıklar.

Üretim dağıtımları için yalnızca IoT Edge çalışma süresi desteklenir, ancak aşağıdaki araçlar geliştirme ve sınama amacıyla IoT Edge aygıtlarını simüle etmenize veya kolayca oluşturmanıza olanak tanır. Bu araçlar birbirini dışlayan araçlar değildir, ancak tam bir geliştirme deneyimi için birlikte çalışabilir.

| Araç | Olarak da bilinir | Desteklenen platformlar | En iyi kullanım alanı: |
| ---- | ------------- | ------------------- | --------- |
| IoT EdgeHub geliştirme aracı  | iotedgehubdev | Windows, Linux, MacOS | Modülleri ayıklamak için bir aygıtı simüle etme. |
| IoT Edge dev konteyner | microsoft/iotedgedev | Windows, Linux, MacOS | Bağımlılıkları yüklemeden geliştirme. |
| Bir kapta IoT Edge çalışma süresi | iotedgec | Windows, Linux, MacOS, ARM | Çalışma süresini desteklemeyebilecek bir aygıtta sınama. |
| IoT Edge cihaz konteyneri | toolboc/azure-iot-edge-device-container | Windows, Linux, MacOS, ARM | Birçok IoT Edge aygıtıyla bir senaryoyu ölçekte sınama. |

### <a name="iot-edgehub-dev-tool"></a>IoT EdgeHub geliştirme aracı

Azure IoT EdgeHub geliştirme aracı yerel bir geliştirme ve hata ayıklama deneyimi sağlar. Araç, IoT Edge modüllerini yerel olarak oluşturabilmeniz, geliştirebilmeniz, test edebilmeniz, çalıştırabilmeniz ve hata ayıklamanız için IoT Edge çalışma zamanı olmadan IoT Edge modüllerinin başlatılmasına yardımcı olur. Görüntüleri bir kapsayıcı kayıt defterine itmeniz ve bunları test etmek üzere bir aygıta dağıtmanız gerekmez.

IoT EdgeHub dev aracı Visual Studio ve Visual Studio Code uzantıları nın yanı sıra IoT Edge dev aracıyla birlikte çalışacak şekilde tasarlanmıştır. Bu iç döngü geliştirme yanı sıra dış döngü test destekler, bu yüzden devops araçları ile de entegre.

Daha fazla bilgi ve yüklemek için [Azure IoT EdgeHub geliştirme aracına](https://pypi.org/project/iotedgehubdev/)bakın.

### <a name="iot-edge-dev-container"></a>IoT Edge dev konteyner

Azure IoT Edge geliştirme kapsayıcısı, IoT Edge geliştirme si için ihtiyacınız olan tüm bağımlılıklara sahip bir Docker kapsayıcısır. Bu kapsayıcı, C#, Python, Node.js ve Java dahil olmak üzere hangi dilde geliştirmek isterseniz, işe başlamanızı kolaylaştırır. Yüklemeniz gereken tek şey, docker veya Moby gibi bir konteyner motoru, geliştirme makinesine konteyner çekmek için.

Daha fazla bilgi için [Azure IoT Edge dev kapsayıcıya](https://hub.docker.com/r/microsoft/iotedgedev/)bakın.

### <a name="iot-edge-runtime-in-a-container"></a>Bir kapta IoT Edge çalışma süresi

Bir kapsayıcıdaki IoT Edge çalışma süresi, aygıt bağlantı dizenizi ortam değişkeni olarak alan tam bir çalışma süresi sağlar. Bu kapsayıcı, IoT Edge modüllerini ve senaryolarını MacOS gibi çalışma zamanını yerel olarak desteklemeyebilecek bir sistemde test etmenizi sağlar. Dağıttığınız tüm modüller çalışma zamanı kapsayıcısının dışında başlatılır. Çalışma zamanının ve dağıtılan modüllerin aynı kapsayıcı içinde bulunmasını istiyorsanız, bunun yerine IoT Edge aygıt kapsayıcısını düşünün.

Daha fazla bilgi için azure [IoT Edge'i bir kapsayıcıda çalıştırma'ya](https://github.com/Azure/iotedgedev/tree/master/docker/runtime)bakın.

### <a name="iot-edge-device-container"></a>IoT Edge cihaz konteyneri

IoT Edge cihaz konteyneri komple bir IoT Edge cihazıdır, konteyner motoru olan herhangi bir makinede fırlatılacak şekilde hazırdır. Cihaz kapsayıcısı IoT Edge çalışma süresini ve konteyner motorunun kendisini içerir. Kapsayıcının her örneği tamamen işlevsel bir kendi kendini sağlayan IoT Edge cihazıdır. Aygıt kapsayıcısı, modüle giden bir ağ yolu olduğu sürece modüllerin uzaktan hata ayıklamasını destekler. Aygıt kapsayıcısı, ölçeksenaryolarını veya Azure Ardışık Hatlarını sınamak için çok sayıda IoT Edge aygıtını hızla oluşturmak için iyidir. Ayrıca dümen yoluyla kubernetes dağıtım destekler.

Daha fazla bilgi için [Azure IoT Edge aygıt kapsayıcısı'na](https://github.com/toolboc/azure-iot-edge-device-container)bakın.

## <a name="devops-tools"></a>DevOps araçları

Kapsamlı üretim senaryoları için ölçekte çözümler geliştirmeye hazır olduğunuzda, otomasyon, izleme ve kolaylaştırılmış yazılım mühendisliği süreçleri gibi modern DevOps ilkelerinden yararlanın. IoT Edge, Azure DevOps, Azure DevOps Projeleri ve Jenkins gibi DevOps araçlarını destekleyen uzantılara sahiptir. Varolan bir ardışık hattı özelleştirmek veya CircleCI veya TravisCI gibi farklı bir DevOps aracı kullanmak istiyorsanız, bunu IoT Edge geliştirme aracında bulunan CLI özellikleriyle yapabilirsiniz.

Daha fazla bilgi, kılavuz ve örnekler için aşağıdaki sayfalara bakın:

* [Azure IoT Edge'e sürekli tümleştirme ve sürekli dağıtım](how-to-ci-cd.md)
* [Azure DevOps Projeleri ile IoT Edge için BIR CI/CD ardışık](how-to-devops-project.md)
* [Azure IoT Edge Jenkins eklentisi](https://plugins.jenkins.io/azure-iot-edge)
* [IoT Kenar DevOps GitHub repo](https://github.com/toolboc/IoTEdge-DevOps)
