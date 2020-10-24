---
title: Azure Cosmos DB öykünücüsü ile yerel olarak yükleyip geliştirin
description: Windows, Linux, macOS ve Windows Docker ortamlarında Azure Cosmos DB öykünücüsünü yüklemeyi ve kullanmayı öğrenin. Öykünücüyü kullanarak, Azure aboneliği oluşturmadan uygulamanızı ücretsiz olarak geliştirebilir ve test edebilirsiniz.
ms.service: cosmos-db
ms.topic: how-to
author: markjbrown
ms.author: mjbrown
ms.date: 09/22/2020
ms.custom: devx-track-csharp, contperfq1
ms.openlocfilehash: 9455ac3520192274e80f2d9e0fdfd1c8f8a238a3
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92482630"
---
# <a name="install-and-use-the-azure-cosmos-emulator-for-local-development-and-testing"></a>Yerel geliştirme ve test için Azure Cosmos öykünücüsü 'nü yükleyip kullanın

Azure Cosmos öykünücüsü, Azure Cosmos DB hizmetini geliştirme amacıyla taklit eden bir yerel ortam sağlar. Azure Cosmos öykünücüsünü kullanarak, Azure aboneliği oluşturmadan veya herhangi bir ücret ödemeden uygulamanızı yerel olarak geliştirebilir ve test edebilirsiniz. Uygulamanızın Azure Cosmos öykünücüsünde nasıl çalıştığı konusunda memnun kaldığınızda, bulutta bir Azure Cosmos hesabı kullanarak geçiş yapabilirsiniz. Bu makalede, Emulator 'un Windows, Linux, macOS ve Windows Docker ortamlarında nasıl yükleneceği ve kullanılacağı açıklanmaktadır.

## <a name="download-the-emulator"></a>Öykünücüyü indirin

Başlamak için, yerel bilgisayarınıza Azure Cosmos öykünücüsü ' nin en son sürümünü indirin ve yükleyin. [Öykünücü sürüm notları](local-emulator-release-notes.md) makalesinde, her yayında yapılan tüm kullanılabilir sürümler ve özellik güncelleştirmeleri listelenir.

:::image type="icon" source="media/local-emulator/download-icon.png" border="false":::**[Azure Cosmos öykünücüsünü indirin](https://aka.ms/cosmosdb-emulator)**

[SQL](local-emulator.md#sql-api), [Cassandra](local-emulator.md#cassandra-api), [MongoDB](local-emulator.md#azure-cosmos-dbs-api-for-mongodb), [Gremlin](local-emulator.md#gremlin-api)ve [tablo](local-emulator.md#table-api) API hesaplarıyla Azure Cosmos öykünücüsü 'nü kullanarak uygulamalar geliştirebilirsiniz. Şu anda öykünücüsünde bulunan Veri Gezgini yalnızca SQL verilerini görüntülemeyi tamamen destekler; MongoDB, Gremlin/Graph ve Cassandra istemci uygulamaları kullanılarak oluşturulan veriler şu anda görüntülenemez. Daha fazla bilgi edinmek için bkz. farklı API 'lerden [öykünücü uç noktasına bağlanma](#connect-with-emulator-apis) .

## <a name="how-does-the-emulator-work"></a>Öykünücü nasıl çalışır?

Azure Cosmos öykünücüsü Azure Cosmos DB hizmeti için yüksek uygunlukta bir öykünme sağlar. Veri oluşturma, veri sorgulama, kapsayıcıları hazırlama ve ölçekleme ve saklı yordamları ve Tetikleyicileri yürütme dahil olmak üzere Azure Cosmos DB olarak eşdeğer işlevleri destekler. Azure Cosmos öykünücüsünü kullanarak uygulamalar geliştirebilir ve test edebilir ve Azure Cosmos DB bağlantı uç noktasını güncelleştirerek bunları küresel ölçekte Azure 'a dağıtabilirsiniz.

Azure Cosmos DB hizmetinin öykünmesi aslına sadık olsa da, öykünücünün uygulaması hizmetten farklıdır. Örneğin öykünücü, kalıcılık için yerel dosya sistemi gibi standart işletim sistemi bileşenlerini ve bağlantı için HTTPS protokolü yığınını kullanır. Genel çoğaltma gibi Azure altyapısına, okuma/yazma işlemleri için tek basamaklı milisaniyelik gecikme süresine ve bu öykünücü kullandığınızda, ayarlanabilir tutarlılık düzeylerine bağlı olan işlevler geçerli değildir.

[Azure Cosmos DB veri geçiş aracını](https://github.com/azure/azure-documentdb-datamigrationtool)kullanarak Azure Cosmos öykünücüsü ile Azure Cosmos DB hizmeti arasında veri geçirebilirsiniz.

## <a name="differences-between-the-emulator-and-the-cloud-service"></a>Öykünücü ve bulut hizmeti arasındaki farklar

Azure Cosmos öykünücüsü yerel geliştirici iş istasyonunda çalışan bir Öykünülmüş ortam sağladığından, öykünücü ve buluttaki bir Azure Cosmos hesabı arasındaki işlevlerde bazı farklılıklar vardır:

* Şu anda öykünücüsünde **Veri Gezgini** BÖLMESI yalnızca SQL API istemcilerini desteklemektedir. MongoDB, Table, Graph ve Cassandra API 'Leri gibi Azure Cosmos DB API 'Ler için **Veri Gezgini** görünümü ve işlemleri tam olarak desteklenmez.

* Öykünücü yalnızca tek bir sabit hesabı ve iyi bilinen bir birincil anahtarı destekler. Azure Cosmos öykünücüsü 'nü kullanırken anahtarı yeniden oluşturamazsınız, ancak [komut satırı](emulator-command-line-parameters.md) seçeneğini kullanarak varsayılan anahtarı değiştirebilirsiniz.

* Öykünücü ile yalnızca [sağlanan aktarım hızı](set-throughput.md) modunda bir Azure Cosmos hesabı oluşturabilirsiniz; Şu anda [sunucusuz](serverless.md) modunu desteklemiyor.

* Öykünücü ölçeklenebilir bir hizmet değildir ve çok sayıda kapsayıcıyı desteklemez. Azure Cosmos öykünücüsü 'nü kullanırken, varsayılan olarak, 400 RU/sn (yalnızca Azure Cosmos DB SDK 'lar kullanılarak desteklenir) veya 5 sınırsız kapsayıcı için en fazla 25 sabit boyut kapsayıcısı oluşturabilirsiniz. Bu değeri değiştirme hakkında daha fazla bilgi için bkz. [PartitionCount değeri ayarlama](emulator-command-line-parameters.md#set-partitioncount) makalesi.

* Öykünücü, bulut hizmeti gibi farklı [Azure Cosmos DB tutarlılığı düzeyi](consistency-levels.md) sunmaz.

* Öykünücü [çok bölgeli çoğaltma](distribute-data-globally.md)sunmaz.

* Azure Cosmos öykünücünüzün kopyasının Azure Cosmos DB hizmetindeki en son değişiklikler ile her zaman güncel olmayabilir, uygulamanızın aktarım hızı (ru) ihtiyaçlarını doğru şekilde tahmin etmek için her zaman [Azure Cosmos DB kapasite planlayıcıya](estimate-ru-with-capacity-planner.md) başvurmalısınız.

* Öykünücü 254 karakterlik maksimum KIMLIK özelliği boyutunu destekler.

## <a name="install-the-emulator"></a>Öykünücüyü yükler

Öykünücüyü yüklemeden önce, aşağıdaki donanım ve yazılım gereksinimlerine sahip olduğunuzdan emin olun:

* Yazılım gereksinimleri:
  * Şu anda Windows Server 2012 R2, Windows Server 2016, 2019 veya Windows 8, 10 ana bilgisayar işletim sistemi destekleniyor. Active Directory etkinleştirilmiş konak işletim sistemi şu anda desteklenmiyor.
  * 64 bit işletim sistemi

* En düşük donanım gereksinimleri:
  * 2-GB RAM
  * 10 GB kullanılabilir sabit disk alanı

* Azure Cosmos öykünücüsü 'nü yüklemek, yapılandırmak ve çalıştırmak için bilgisayarda yönetici ayrıcalıklarına sahip olmanız gerekir. Öykünücü, bir sertifika ekleyecek ve ayrıca hizmetlerini çalıştırmak için güvenlik duvarı kurallarını ayarlamış olur. Bu nedenle, öykünücüsünün bu tür işlemleri yürütebilmesi için yönetici hakları gereklidir.

Başlamak için, yerel bilgisayarınıza [Azure Cosmos öykünücüsü](https://aka.ms/cosmosdb-emulator) ' nin en son sürümünü indirin ve yükleyin. Öykünücüyü yüklerken herhangi bir sorunla karşılaşırsanız hata ayıklama için [öykünücü sorun giderme](troubleshoot-local-emulator.md) makalesine bakın.

Sistem gereksinimlerinize bağlı olarak, bu makalenin sonraki bölümlerinde açıklandığı gibi öykünücü 'yı [Windows](#run-on-windows), [Docker for Windows](#run-on-windows-docker), [Linux veya MacOS](#run-on-linux-macos) üzerinde çalıştırabilirsiniz.

## <a name="check-for-emulator-updates"></a>Öykünücü güncelleştirmelerini denetle

Her öykünücü sürümü bir özellik güncelleştirmeleri veya hata düzeltmeleri kümesiyle gelir. Kullanılabilir sürümleri görmek için [öykünücü sürüm notları](local-emulator-release-notes.md) makalesini okuyun.

Yükleme sonrasında, varsayılan ayarları kullandıysanız öykünücüye karşılık gelen veriler%LOCALAPPDATA%\CosmosDBEmulator konumunda kaydedilir. İsteğe bağlı veri yolu ayarlarını kullanarak farklı bir konum yapılandırabilirsiniz; Bu, `/DataPath=PREFERRED_LOCATION` [komut satırı parametresi](emulator-command-line-parameters.md)olarak olur. Azure Cosmos öykünücüsünün bir sürümünde oluşturulan verilerin farklı bir sürüm kullanılırken erişilebilir olmaması garanti edilmez. Verilerinizi uzun vadede kalıcı hale getirmeniz gerekiyorsa, bu verileri Azure Cosmos öykünücüsü yerine bir Azure Cosmos hesabında depolamanız önerilir.

## <a name="use-the-emulator-on-windows"></a><a id="run-on-windows"></a>Windows üzerinde öykünücü kullanma

Azure Cosmos öykünücüsü, `C:\Program Files\Azure Cosmos DB Emulator` Varsayılan olarak konuma yüklenir. Windows üzerinde Azure Cosmos öykünücüsü 'nü başlatmak için **Başlat** düğmesini seçin veya Windows tuşuna basın. **Azure Cosmos öykünücüsü**yazmaya başlayın ve uygulama listesinden öykünücü ' ı seçin.

:::image type="content" source="./media/local-emulator/database-local-emulator-start.png" alt-text="Başlat düğmesini seçin veya Windows tuşuna basın, Azure Cosmos öykünücüsü yazmaya başlayın ve uygulama listesinden öykünücü ' ı seçin":::

Öykünücü başladığında Windows görev çubuğu bildirim alanında bir simge görürsünüz. Bu URL URL 'sindeki tarayıcınızda Azure Cosmos Veri Gezgini 'ni otomatik olarak açar `https://localhost:8081/_explorer/index.html` .

:::image type="content" source="./media/local-emulator/database-local-emulator-taskbar.png" alt-text="Başlat düğmesini seçin veya Windows tuşuna basın, Azure Cosmos öykünücüsü yazmaya başlayın ve uygulama listesinden öykünücü ' ı seçin":::

Ayrıca, öykünücüsü komut satırı veya PowerShell komutlarıyla başlatabilir ve durdurabilirsiniz. Daha fazla bilgi için bkz. [komut satırı aracı başvuru](emulator-command-line-parameters.md) makalesi.

Azure Cosmos öykünücüsü, bağlantı noktası 8081 ' ü dinleyen yerel makinede ("localhost") varsayılan olarak çalışır. Adres, `https://localhost:8081/_explorer/index.html` olarak görüntülenir. Gezgin 'i kapatır ve daha sonra yeniden açmak isterseniz, URL 'YI tarayıcınızda açabilir veya aşağıda gösterildiği gibi Windows tepsi simgesindeki Azure Cosmos öykünücüsünde başlatabilirsiniz.

:::image type="content" source="./media/local-emulator/database-local-emulator-data-explorer-launcher.png" alt-text="Başlat düğmesini seçin veya Windows tuşuna basın, Azure Cosmos öykünücüsü yazmaya başlayın ve uygulama listesinden öykünücü ' ı seçin":::

## <a name="use-the-emulator-on-docker-for-windows"></a><a id="run-on-windows-docker"></a>Docker for Windows üzerinde öykünücü kullanın

Azure Cosmos öykünücüsü 'nü Windows Docker kapsayıcısında çalıştırabilirsiniz. Docker pull komutu için [Docker Hub 'ına](https://hub.docker.com/r/microsoft/azure-cosmosdb-emulator/) [GitHub](https://github.com/Azure/azure-cosmos-db-emulator-docker) `Dockerfile` ve daha fazla bilgi için GitHub ' a bakın. Şu anda öykünücü Oracle Linux için Docker üzerinde çalışmıyor. Docker for Windows üzerinde öykünücü çalıştırmak için aşağıdaki yönergeleri kullanın:

1. [Docker for Windows](https://www.docker.com/docker-windows) yükledikten sonra, araç çubuğundaki Docker simgesine sağ tıklayıp **Windows kapsayıcılarına geç**' i seçerek Windows kapsayıcılarına geçiş yapın.

1. Daha sonra sık kullandığınız kabuktan aşağıdaki komutu çalıştırarak Docker Hub'dan Öykünücü görüntüsünü çekin.

   ```bash
   docker pull mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator
   ```

1. Görüntüyü başlatmak için komut satırına veya PowerShell ortamına bağlı olarak aşağıdaki komutları çalıştırın:

   # <a name="command-line"></a>[Komut satırı](#tab/cli)

   ```bash

   md %LOCALAPPDATA%\CosmosDBEmulator\bind-mount

   docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=%LOCALAPPDATA%\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator
   ```
   Windows tabanlı Docker görüntüleri, her Windows ana bilgisayar işletim sistemi ile genel olarak uyumlu olmayabilir. Örneğin, varsayılan Azure Cosmos öykünücü görüntüsü yalnızca Windows 10 ve Windows Server 2016 ile uyumludur. Windows Server 2019 ile uyumlu bir görüntüye ihtiyacınız varsa, bunun yerine aşağıdaki komutu çalıştırın:

   ```bash
   docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=%hostDirectory%,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 mcr.microsoft.com/cosmosdb/winsrv2019/azure-cosmos-emulator:latest
   ```

   # <a name="powershell"></a>[PowerShell](#tab/powershell)

   ```powershell

   md $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount 2>null

   docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=$env:LOCALAPPDATA\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator

   ```

   Yanıt şuna benzer:

   ```bash
   Starting emulator
   Emulator Endpoint: https://172.20.229.193:8081/
   Primary Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
   Exporting SSL Certificate
   You can import the SSL certificate from an administrator command prompt on the host by running:
   cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
   powershell .\importcert.ps1
   --------------------------------------------------------------------------------------------------
   Starting interactive shell
   ```
   ---

   > [!NOTE]
   > `docker run`Komutu yürütürken bir bağlantı noktası çakışması hatası görürseniz (belirtilen bağlantı noktası zaten kullanımda ise), bağlantı noktası numaralarını değiştirerek özel bir bağlantı noktası geçirin. Örneğin, "-p 8081:8081" parametresini "-p 443:8081" olarak değiştirebilirsiniz

1. Şimdi, yanıttaki öykünücü uç noktasını ve birincil anahtarı kullanın ve TLS/SSL sertifikasını ana bilgisayarınıza aktarın. TLS/SSL sertifikasını içeri aktarmak için yönetici komut isteminden aşağıdaki adımları çalıştırın:

   # <a name="command-line"></a>[Komut satırı](#tab/cli)

   ```bash
   cd  %LOCALAPPDATA%\CosmosDBEmulator\bind-mount
   powershell .\importcert.ps1
   ```

   # <a name="powershell"></a>[PowerShell](#tab/powershell)

   ```powershell
   cd $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount
   .\importcert.ps1
   ```
   ---

1. Öykünücü başlatıldıktan sonra etkileşimli kabuğu kapatırsanız, öykünücü kapsayıcısını kapatır. Veri Gezgini 'ni yeniden açmak için tarayıcınızda aşağıdaki URL 'ye gidin. Yukarıda gösterilen yanıt iletisinde öykünücü uç noktası sağlanır.

   `https://<emulator endpoint provided in response>/_explorer/index.html`

Linux Docker kapsayıcısında çalışan bir .NET istemci uygulamanız varsa ve bir konak makinesinde Azure Cosmos öykünücüsü çalıştırıyorsanız, sertifikayı Linux Docker kapsayıcısına aktarmak için sonraki bölümde yer alan yönergeleri kullanın.

### <a name="regenerate-the-emulator-certificates-when-running-on-a-docker-container"></a>Bir Docker kapsayıcısında çalışırken öykünücü sertifikalarını yeniden üret

Öykünücü bir Docker kapsayıcısında çalıştırılırken, öykünücü ile ilişkili sertifikalar, ilgili kapsayıcıyı her durdurup yeniden başlattığınızda yeniden oluşturulur. Bu nedenle, her bir kapsayıcı başladıktan sonra sertifikaları yeniden içeri aktarmanız gerekir. Bu sınırlamaya geçici bir çözüm olarak, Docker kapsayıcısını belirli bir IP adresine ve kapsayıcı görüntüsüne bağlamak için bir Docker Compose dosyası kullanabilirsiniz.

Örneğin, Docker Compose dosyası içinde aşağıdaki yapılandırmayı kullanabilirsiniz, bunu gereksiniminize göre biçimlendirmeniz gerekir: 

```yml
version: '2.4' # Do not upgrade to 3.x yet, unless you plan to use swarm/docker stack: https://github.com/docker/compose/issues/4513

networks:
  default:
    external: false
    ipam:
      driver: default
      config:
        - subnet: "172.16.238.0/24"

services:

  # First create a directory that will hold the emulator traces and certificate to be imported
  # set hostDirectory=C:\emulator\bind-mount
  # mkdir %hostDirectory%

  cosmosdb:
    container_name: "azurecosmosemulator"
    hostname: "azurecosmosemulator"
    image: 'mcr.microsoft.com/cosmosdb/windows/azure-cosmos-emulator'
    platform: windows
    tty: true
    mem_limit: 3GB
    ports:
        - '8081:8081'
        - '8900:8900'
        - '8901:8901'
        - '8902:8902'
        - '10250:10250'
        - '10251:10251'
        - '10252:10252'
        - '10253:10253'
        - '10254:10254'
        - '10255:10255'
        - '10256:10256'
        - '10350:10350'
    networks:
      default:
        ipv4_address: 172.16.238.246
    volumes:
        - '${hostDirectory}:C:\CosmosDB.Emulator\bind-mount'
```

## <a name="use-the-emulator-on-linux-or-macos"></a><a id="run-on-linux-macos"></a>Linux veya macOS üzerinde öykünücü kullanma

Şu anda Azure Cosmos öykünücüsü yalnızca Windows 'da çalıştırılabilir. Linux veya macOS kullanıyorsanız, öykünücüyü Parallels veya VirtualBox gibi bir Hiper yöneticide barındırılan bir Windows sanal makinesinde çalıştırabilirsiniz.

> [!NOTE]
> Bir Hiper yöneticide barındırılan Windows sanal makinesini her yeniden başlattığınızda, sanal makinenin IP adresi değiştiği için sertifikayı yeniden içeri aktarmanız gerekir. Sanal makineyi IP adresini koruyacak şekilde yapılandırdıysanız sertifikayı içeri aktarma işlemi gerekli değildir.

Linux veya macOS ortamlarında öykünücüyü kullanmak için aşağıdaki adımları kullanın:

1. Windows sanal makinesinden aşağıdaki komutu çalıştırın ve IPv4 adresini bir yere göz önünde oluşturun:

   ```bash
   ipconfig.exe
   ```

1. Uygulamanızın içinde, yerine tarafından döndürülen IPv4 adresini kullanmak için uç nokta URL 'sini değiştirin `ipconfig.exe` `localhost` .

1. Windows VM 'de, aşağıdaki seçenekleri kullanarak komut satırından Azure Cosmos öykünücüsünü başlatın. Komut satırı tarafından desteklenen parametrelerle ilgili ayrıntılar için bkz. [öykünücü komut satırı aracı başvurusu](emulator-command-line-parameters.md):

   ```bash
   Microsoft.Azure.Cosmos.Emulator.exe /AllowNetworkAccess /Key=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
   ```

1. Son olarak, Linux veya Mac ortamında ve öykünücü üzerinde çalışan uygulama arasındaki sertifika güven sürecini çözmeniz gerekir. Sertifikayı çözümlemek için aşağıdaki iki seçenekten birini kullanabilirsiniz:

   1. [ÖYKÜNÜCÜ TLS/SSL sertifikasını Linux veya Mac ortamına aktarın](#import-certificate) veya
   2. [Uygulamada TLS/SSL doğrulamasını devre dışı bırakma](#disable-ssl-validation)

### <a name="option-1-import-the-emulator-tlsssl-certificate"></a><a id="import-certificate"></a>Seçenek 1: öykünücü TLS/SSL sertifikasını Içeri aktarma

Aşağıdaki bölümlerde, öykünücü TLS/SSL sertifikasının Linux ve macOS ortamlarına nasıl aktarılacağı gösterilmektedir.

#### <a name="linux-environment"></a>Linux ortamı

Linux üzerinde çalışıyorsanız, doğrulama yapmak için OpenSSL üzerinde .NET geçişleri:

1. [SERTIFIKAYı PFX biçiminde dışarı aktarın](local-emulator-export-ssl-certificates.md#export-emulator-certificate). PFX seçeneği, özel anahtarı dışarı aktarmaya seçerken kullanılabilir.

1. Bu PFX dosyasını Linux ortamınıza kopyalayın.

1. PFX dosyasını CRT dosyasına Dönüştür

   ```bash
   openssl pkcs12 -in YourPFX.pfx -clcerts -nokeys -out YourCTR.crt
   ```

1. CRT dosyasını Linux dağılısdaki özel sertifikaları içeren klasöre kopyalayın. Yaygın olarak, DEMIN dağıtımları üzerinde bulunur `/usr/local/share/ca-certificates/` .

   ```bash
   cp YourCTR.crt /usr/local/share/ca-certificates/
   ```

1. Klasörü güncelleştirecek TLS/SSL sertifikalarını güncelleştirin `/etc/ssl/certs/` .

   ```bash
   update-ca-certificates
   ```

#### <a name="macos-environment"></a>macOS ortamı

Mac üzerinde çalışıyorsanız aşağıdaki adımları kullanın:

1. [SERTIFIKAYı PFX biçiminde dışarı aktarın](local-emulator-export-ssl-certificates.md#export-emulator-certificate). PFX seçeneği, özel anahtarı dışarı aktarmaya seçerken kullanılabilir.

1. Bu PFX dosyasını Mac ortamınıza kopyalayın.

1. *Anahtarlık erişimi* uygulamasını açın ve pfx dosyasını içeri aktarın.

1. Sertifika listesini açın ve adı ile ayırt edin `localhost` .

1. Söz konusu öğe için bağlam menüsünü açın, *öğeyi al* ' ı seçin ve *güven*altında,  >  *Bu sertifikayı kullanırken* *her zaman güven*' i seçin. 

   :::image type="content" source="./media/local-emulator/mac-trust-certificate.png" alt-text="Başlat düğmesini seçin veya Windows tuşuna basın, Azure Cosmos öykünücüsü yazmaya başlayın ve uygulama listesinden öykünücü ' ı seçin":::
  
### <a name="option-2-disable-the-ssl-validation-in-the-application"></a><a id="disable-ssl-validation"></a>Seçenek 2: uygulamada SSL doğrulamasını devre dışı bırakma

SSL doğrulamasının devre dışı bırakılması yalnızca geliştirme amacıyla önerilir ve bir üretim ortamında çalıştırıldığında yapılmamalıdır. Aşağıdaki örneklerde, .NET ve Node.js uygulamaları için SSL doğrulamasının nasıl devre dışı bırakılacağı gösterilmektedir.

# <a name="net-standard-21"></a>[.NET Standard 2.1 +](#tab/ssl-netstd21)

.NET Standard 2,1 veya üzeri sürümlerle uyumlu bir çerçevede çalışan tüm uygulamalar için şunları kullanabilirsiniz `CosmosClientOptions.HttpClientFactory` :

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/HttpClientFactory/Program.cs?name=DisableSSLNETStandard21)]

# <a name="net-standard-20"></a>[.NET Standard 2,0](#tab/ssl-netstd20)

.NET Standard 2,0 ile uyumlu bir çerçevede çalışan tüm uygulamalar için şunları kullanabilirsiniz `CosmosClientOptions.HttpClientFactory` :

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/HttpClientFactory/Program.cs?name=DisableSSLNETStandard20)]

# <a name="nodejs"></a>[Node.js](#tab/ssl-nodejs)

Node.js uygulamalar için `package.json` dosyanızı, uygulamayı başlatırken ayarlanacak şekilde değiştirebilirsiniz `NODE_TLS_REJECT_UNAUTHORIZED` :

```json
"start": NODE_TLS_REJECT_UNAUTHORIZED=0 node app.js
```
--- 

## <a name="enable-access-to-emulator-on-a-local-network"></a>Yerel ağ üzerinde öykünücü erişimini etkinleştir

Tek bir ağ kullanan birden fazla makineniz varsa ve öykünücüyü bir makinede ayarlarsanız ve başka bir makineden erişmek istiyorsanız. Böyle bir durumda, yerel bir ağ üzerinde öykünücü erişimini etkinleştirmeniz gerekir.

Yerel bir ağ üzerinde öykünücüyü çalıştırabilirsiniz. Ağ erişimini etkinleştirmek için, `/AllowNetworkAccess` ya da belirtmenizi gerektiren [komut satırında](emulator-command-line-parameters.md)seçeneğini belirtin `/Key=key_string` `/KeyFile=file_name` . `/GenKeyFile=file_name`Rastgele bir anahtarla bir dosya oluşturmak için ' i kullanabilirsiniz. Daha sonra, veya öğesine geçirebilirsiniz `/KeyFile=file_name` `/Key=contents_of_file` .

Ağ erişimini ilk kez etkinleştirmek için, Kullanıcı öykünücüyü kapatıp öykünücü veri dizinini *%LocalAppData%\cosmosdbemulator*olarak siler.

## <a name="authenticate-connections-when-using-emulator"></a><a id="authenticate-requests"></a>Öykünücü kullanırken bağlantı kimlik doğrulaması

Bulutta Azure Cosmos DB olduğu gibi, Azure Cosmos öykünücüsünde karşı yaptığınız her isteğin kimliği doğrulanmalıdır. Azure Cosmos öykünücüsü yalnızca TLS aracılığıyla güvenli iletişimi destekler. Azure Cosmos öykünücüsü, birincil anahtar kimlik doğrulaması için tek bir sabit hesabı ve iyi bilinen bir kimlik doğrulama anahtarını destekler. Bu hesap ve anahtar, Azure Cosmos öykünücüsü ile kullanım için izin verilen tek kimlik bilgileridir. Bunlar:

```bash
Account name: localhost:<port>
Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
```

> [!NOTE]
> Azure Cosmos öykünücüsü tarafından desteklenen birincil anahtar, yalnızca öykünücü ile kullanılmaya yöneliktir. Üretim Azure Cosmos DB hesabınızı ve anahtarınızı Azure Cosmos öykünücüsü ile kullanamazsınız.

> [!NOTE]
> Emulator 'u/Key seçeneğiyle başlattıysanız varsayılan anahtar yerine oluşturulan anahtarı kullanın `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==` . /Key seçeneği hakkında daha fazla bilgi için bkz [. komut satırı araç başvurusu.](emulator-command-line-parameters.md)

## <a name="connect-to-different-apis-with-the-emulator"></a><a id="connect-with-emulator-apis"></a>Öykünücü ile farklı API 'lere bağlanma

### <a name="sql-api"></a>SQL API

Azure Cosmos öykünücüsünü masaüstünüzde çalışır olduktan sonra, öykünücüle etkileşim kurmak için desteklenen [Azure Cosmos DB SDK](sql-api-sdk-dotnet-standard.md) veya [Azure Cosmos DB REST API](/rest/api/cosmos-db/) kullanabilirsiniz. Azure Cosmos öykünücüsü Ayrıca, Mongo DB API 'SI için SQL API veya Azure Cosmos DB kapsayıcıları oluşturmanıza olanak sağlayan yerleşik bir veri Gezgini içerir. Veri Gezgini 'ni kullanarak, herhangi bir kod yazmadan öğeleri görüntüleyebilir ve düzenleyebilirsiniz.

```csharp
// Connect to the Azure Cosmos emulator running locally
CosmosClient client = new CosmosClient(
   "https://localhost:8081", 
    "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

```

### <a name="azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API'si

Azure Cosmos öykünücüsünü masaüstünüzde çalışır olduktan sonra, öykünücüle etkileşimde bulunmak için [Azure Cosmos DB MongoDB IÇIN API](mongodb-introduction.md) 'sini kullanabilirsiniz. "/EnableMongoDbEndpoint" ile yönetici olarak [komut isteminden](emulator-command-line-parameters.md) öykünücü başlatın. Daha sonra MongoDB API hesabına bağlanmak için aşağıdaki bağlantı dizesini kullanın:

```bash
mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true
```

### <a name="table-api"></a>Tablo API’si

Azure Cosmos öykünücüsünü masaüstünüzde çalışan bir kez etkinleştirdikten sonra, öykünücüle etkileşim kurmak için [Azure Cosmos DB tablo API'si SDK 'sını](./tutorial-develop-table-dotnet.md) kullanabilirsiniz. "/EnableTableEndpoint" ile yönetici olarak [komut isteminden](emulator-command-line-parameters.md) öykünücü başlatın. Daha sonra tablo API hesabına bağlanmak için aşağıdaki kodu çalıştırın:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Table;
using CloudTable = Microsoft.WindowsAzure.Storage.Table.CloudTable;
using CloudTableClient = Microsoft.WindowsAzure.Storage.Table.CloudTableClient;

string connectionString = "DefaultEndpointsProtocol=http;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=http://localhost:8902/;";

CloudStorageAccount account = CloudStorageAccount.Parse(connectionString);
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("testtable");
table.CreateIfNotExists();
table.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
```

### <a name="cassandra-api"></a>Cassandra API’si

"/EnableCassandraEndpoint" ile yönetici [komut isteminden](emulator-command-line-parameters.md) öykünücü başlatın. Alternatif olarak, ortam değişkenini de ayarlayabilirsiniz `AZURE_COSMOS_EMULATOR_CASSANDRA_ENDPOINT=true` .

1. [Python 2,7 'yi yükler](https://www.python.org/downloads/release/python-2716/)

1. [Cassandra CLı/CSQLSH 'i yükler](https://cassandra.apache.org/download/)

1. Normal bir komut istemi penceresinde aşağıdaki komutları çalıştırın:

   ```bash
   set Path=c:\Python27;%Path%
   cd /d C:\sdk\apache-cassandra-3.11.3\bin
   set SSL_VERSION=TLSv1_2
   set SSL_VALIDATE=false
   cqlsh localhost 10350 -u localhost -p C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw== --ssl
   ```

1. CSQLSH kabuğu 'nda Cassandra uç noktasına bağlanmak için aşağıdaki komutları çalıştırın:

   ```bash
   CREATE KEYSPACE MyKeySpace WITH replication = {'class':'MyClass', 'replication_factor': 1};
   DESCRIBE keyspaces;
   USE mykeyspace;
   CREATE table table1(my_id int PRIMARY KEY, my_name text, my_desc text);
   INSERT into table1 (my_id, my_name, my_desc) values( 1, 'name1', 'description 1');
   SELECT * from table1;
   EXIT
   ```

### <a name="gremlin-api"></a>Gremlin API

"/EnableGremlinEndpoint" ile yönetici [komut isteminden](emulator-command-line-parameters.md)öykünücü başlatın. Alternatif olarak, ortam değişkenini de belirleyebilirsiniz `AZURE_COSMOS_EMULATOR_GREMLIN_ENDPOINT=true`

1. [Apache-tinkerpop-Gremlin-Console-3.3.4](https://archive.apache.org/dist/tinkerpop/3.3.4)' i yükler.

1. Öykünücü Veri Gezgini ' nden bir veritabanı "DB1" ve "coll1" koleksiyonu oluşturun. bölüm anahtarı için "/Name" öğesini seçin

1. Normal bir komut istemi penceresinde aşağıdaki komutları çalıştırın:

   ```bash
   cd /d C:\sdk\apache-tinkerpop-gremlin-console-3.3.4-bin\apache-tinkerpop-gremlin-console-3.3.4
  
   copy /y conf\remote.yaml conf\remote-localcompute.yaml
   notepad.exe conf\remote-localcompute.yaml
     hosts: [localhost]
     port: 8901
     username: /dbs/db1/colls/coll1
     password: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
     connectionPool: {
     enableSsl: false}
     serializer: { className: org.apache.tinkerpop.gremlin.driver.ser.GraphSONMessageSerializerV1d0,
     config: { serializeResultToString: true  }}

   bin\gremlin.bat
   ```

1. Gremlin kabuğu 'nda Gremlin uç noktasına bağlanmak için aşağıdaki komutları çalıştırın:

   ```bash
   :remote connect tinkerpop.server conf/remote-localcompute.yaml
   :remote console
   :> g.V()
   :> g.addV('person1').property(id, '1').property('name', 'somename1')
   :> g.addV('person2').property(id, '2').property('name', 'somename2')
   :> g.V()
   ```

## <a name="uninstall-the-local-emulator"></a><a id="uninstall"></a>Yerel öykünücüyü kaldır

Öykünücüyü kaldırmak için aşağıdaki adımları kullanın:

1. Sistem tepsisindeki **Azure Cosmos öykünücü** simgesine sağ tıklayıp **Çıkış**' ı seçerek yerel öykünücüsünün tüm açık örneklerinden çıkın. Tüm örneklerin çıkması bir dakika sürebilir.

1. Windows Arama kutusuna **uygulamalar & Özellikler** yazın ve **uygulamalar & Özellikler (sistem ayarları)** sonucu ' nı seçin.

1. Uygulamalar listesinde **Azure Cosmos DB öykünücüsü**' ne gidin, seçin, **Kaldır**' ı tıklatın ve ardından yeniden **Kaldır** ' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, yerel öykünücüyü ücretsiz yerel geliştirme için nasıl kullanacağınızı öğrendiniz. Şimdi bir sonraki makaleye devam edebilirsiniz:

* [Java, Python ve Node.js uygulamalarıyla kullanılmak üzere Azure Cosmos öykünücü sertifikalarını dışarı aktarma](local-emulator-export-ssl-certificates.md)
* [Öykünücüyü denetlemek için komut satırı parametrelerini ve PowerShell komutlarını kullanma](emulator-command-line-parameters.md)
* [Öykünücü ile ilgili sorunları ayıklama](troubleshoot-local-emulator.md)