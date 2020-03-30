---
title: Yerel Azure Depolama geliştirme için Azurite emülatörü kullanma
description: Azurite açık kaynak emülatörü (önizleme), Azure depolama uygulamalarınızı test etmek için ücretsiz bir yerel ortam sağlar.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/31/2019
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.openlocfilehash: 5e1fce0852a4e820d7ee0af626ce3fddf6773750
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76029933"
---
# <a name="use-the-azurite-emulator-for-local-azure-storage-development-and-testing-preview"></a>Yerel Azure Depolama geliştirme ve test (önizleme) için Azurite emülatörü kullanın

Azurite sürüm 3.2 açık kaynak emülatörü (önizleme), Azure blob ve sıra depolama uygulamalarınızı test etmek için ücretsiz bir yerel ortam sağlar. Uygulamanızın yerel olarak çalışma şeklinden memnun olduğunuzda, bulutta bir Azure Depolama hesabı kullanmaya geçin. Emülatör, Windows, Linux ve MacOS'ta çapraz platform desteği sağlar. Azurite v3, Azure Blob hizmeti tarafından uygulanan API'leri destekler.

Azurite gelecekteki depolama emülatörü platformudur. Azurite, [Azure Depolama Emülatörü'nün](storage-use-emulator.md)yerini adabına kaldı. Azurite, Azure Depolama API'lerinin en son sürümlerini desteklemek için güncelleştirilmeye devam edecektir.

Yerel sisteminize Azurite yüklemenin ve çalıştırmanın birkaç farklı yolu vardır:

  1. [Azurite Visual Studio Code uzantısını yükleyin ve çalıştırın](#install-and-run-the-azurite-visual-studio-code-extension)
  1. [NPM kullanarak Azurite'yi yükleyin ve çalıştırın](#install-and-run-azurite-by-using-npm)
  1. [Azurite Docker görüntüsünü yükleyin ve çalıştırın](#install-and-run-the-azurite-docker-image)
  1. [Azurite'yi GitHub deposundan klonla, inşa et ve çalıştır](#clone-build-and-run-azurite-from-the-github-repository)

## <a name="install-and-run-the-azurite-visual-studio-code-extension"></a>Azurite Visual Studio Code uzantısını yükleyin ve çalıştırın

Visual Studio Code içinde, **EXTENSIONS** bölmesini seçin ve **EXTENSIONS:MARKETPLACE'de** *Azurite'yi* arayın.

![Görsel Stüdyo Kodu uzantıları pazar](media/storage-use-azurite/azurite-vs-code-extension.png)

Alternatif olarak, tarayıcınızdaki [VS Kodu uzantısı pazarına](https://marketplace.visualstudio.com/items?itemName=Azurite.azurite) gidin. Visual Studio Code'u açmak için **Yükle** düğmesini seçin ve doğrudan Azurite uzantı sayfasına gidin.

VS Kodu durum çubuğundaki **[Azurite Blob Hizmeti]** veya **[Azurite Kuyruk Hizmeti]** düğmesine tıklayarak veya VS Kodu komut paletinde aşağıdaki komutları vererek Azurite'yi hızlı bir şekilde başlatabilir veya kapatabilirsiniz. Komut paletini açmak için VS Kodu'nda **F1** tuşuna basın.

Uzantı aşağıdaki Visual Studio Code komutlarını destekler:

   * **Azurite: Başlat** - Tüm Azurite hizmetlerini başlatın
   * **Azurite: Close** - Tüm Azurite hizmetlerini kapatın
   * **Azürat: Temiz** - Tüm Azürat hizmetlerinin kalıcılık verilerini sıfırla
   * **Azurite: Blob Servisini Başlat** - Blob servisine başla
   * **Azürat: Blob Servisini Kapat** - Blob servisini kapat
   * **Azürat: Temiz Blob Servisi** - Temiz blob servisi
   * **Azurite: Kuyruk Hizmetini Başlat** - Kuyruk hizmetini başlat
   * **Azurite: Sıra Servisini Kapat** - Sıra servisini kapat
   * **Azurite: Temiz Kuyruk Hizmeti** - Temiz kuyruk hizmeti

Visual Studio Code içinde Azurite yapılandırmak için uzantıları bölmesini seçin. **Azurite**için **Yönet** (dişli) simgesini seçin. **Uzantı Ayarlarını Yapılandır'ı**seçin.

![Azurite yapılandırma uzantısı ayarları](media/storage-use-azurite/azurite-configure-extension-settings.png)

Aşağıdaki ayarlar desteklenir:

   * **Azurite: Blob Host** - Blob hizmeti dinleme bitiş noktası. Varsayılan ayar 127.0.0.1'dir.
   * **Azurite: Blob Port** - Blob servis dinleme portu. Varsayılan bağlantı noktası 10000'dir.
   * **Azurite: Hata ayıklama** - Hata ayıklama günlüğünü Azurite kanalına çıktı. Varsayılan değer **false** şeklindedir.
   * **Azurite: Konum** - çalışma alanı konum yolu. Varsayılan, Visual Studio Code çalışma klasörüdür.
   * **Azurite: Queue Host** - Sıra hizmeti dinleme bitiş noktası. Varsayılan ayar 127.0.0.1'dir.
   * **Azurite: Kuyruk Portu** - Kuyruk hizmeti dinleme portu. Varsayılan bağlantı noktası 10001'dir.
   * **Azurite: Sessiz** - Sessiz modu erişim günlüğünü devre dışı kılabilir. Varsayılan değer **false** şeklindedir.

## <a name="install-and-run-azurite-by-using-npm"></a>NPM kullanarak Azurite'yi yükleyin ve çalıştırın

Bu yükleme yöntemi, [Node.js sürüm 8.0 veya daha sonra](https://nodejs.org) yüklü olması gerekir. **npm,** her Node.js kurulumuile birlikte verilen paket yönetim aracıdır. Node.js yükledikten sonra, Azurite yüklemek için aşağıdaki **npm** komutunu çalıştırın.

```console
npm install -g azurite
```

Azurite'yi yükledikten sonra [bkz.](#run-azurite-from-a-command-line)

## <a name="install-and-run-the-azurite-docker-image"></a>Azurite Docker görüntüsünü yükleyin ve çalıştırın

Aşağıdaki komutu kullanarak [en son Azurite görüntüsünü](https://hub.docker.com/_/microsoft-azure-storage-azurite) çekmek için [DockerHub'ı](https://hub.docker.com/) kullanın:

```console
docker pull mcr.microsoft.com/azure-storage/azurite
```

**Azurite Docker görüntü çalıştırın:**

Aşağıdaki komut Azurite Docker görüntüsünü çalıştırın. Parametre, `-p 10000:10000` ana makinenin bağlantı noktası 10000'deki istekleri Docker örneğine yönlendirir.

```console
docker run -p 10000:10000 -p 10001:10001 mcr.microsoft.com/azure-storage/azurite
```

**Çalışma alanı konumunu belirtin:**

Aşağıdaki örnekte, `-v c:/azurite:/data` parametre *c:/azurite'yi Azurit* kalıcı veri konumu olarak belirtir. Dizin, *c:/azurite*, Docker komutunu çalıştırmadan önce oluşturulmalıdır.

```console
docker run -p 10000:10000 -p 10001:10001 -v c:/azurite:/data mcr.microsoft.com/azure-storage/azurite
```

**Sadece blob hizmeti çalıştırın**

```console
docker run -p 10000:10000 mcr.microsoft.com/azure-storage/azurite
    azurite-blob --blobHost 0.0.0.0 --blobPort 10000
```

**Tüm Azurit parametrelerini ayarlayın:**

Bu örnek, tüm komut satırı parametrelerinin nasıl ayarlanır gösteriş olduğunu gösterir. Aşağıdaki parametrelerin tümü tek bir komut satırına yerleştirilmelidir.

```console
docker run -p 8888:8888
           -p 9999:9999
           -v c:/azurite:/workspace mcr.microsoft.com/azure-storage/azurite azurite
           -l /workspace
           -d /workspace/debug.log
           --blobPort 8888
           --blobHost 0.0.0.0
           --queuePort 9999
           --queueHost 0.0.0.0
```

Başlangıç sırasında Azurite'yi yapılandırma hakkında daha fazla bilgi için [Komut satırı seçeneklerine](#command-line-options) bakın.

## <a name="clone-build-and-run-azurite-from-the-github-repository"></a>Azurite'yi GitHub deposundan klonla, inşa et ve çalıştır

Bu yükleme [yöntemi, Git](https://git-scm.com/) yüklü olması gerekir. Aşağıdaki konsol komutunu kullanarak Azurite projesi için [GitHub deposunu](https://github.com/azure/azurite) klonla.

```console
git clone https://github.com/Azure/Azurite.git
```

Kaynak kodu klonladıktan sonra, Azurite oluşturmak ve yüklemek için klonlanmış repo kökünden aşağıdaki komutları yürütmek.

```console
npm install
npm run build
npm install -g
```

Azurite'yi yükledikten ve inşa ettikten sonra [bkz.](#run-azurite-from-a-command-line)

## <a name="run-azurite-from-a-command-line"></a>Azurite'yi komut satırından çalıştırın

> [!NOTE]
> Yalnızca Visual Studio Code uzantısını yüklediyseniz, azurite komut satırından çalıştırılamaz. Bunun yerine, VS Kodu komut paletini kullanın. Daha fazla bilgi için [Azurite Visual Studio Code uzantısını yükleyin ve çalıştırın.](#install-and-run-the-azurite-visual-studio-code-extension)

Komut satırı ile hemen başlamak **için, c:\azurite**adlı bir dizin oluşturmak, sonra aşağıdaki komutu vererek Azurite başlatmak:

```console
azurite --silent --location c:\azurite --debug c:\azurite\debug.log
```

Bu komut Azurite belirli bir dizinde tüm verileri depolamak için söyler, **c:\azurite**. **--konum** seçeneği atlanırsa, geçerli çalışma dizinini kullanır.

## <a name="command-line-options"></a>Komut satırı seçenekleri

Bu bölümde, Azurit başlatırken kullanılabilir komut satırı anahtarları ayrıntılı olarak açıklanın. Tüm komut satırı anahtarları isteğe bağlıdır.

```console
C:\Azurite> azurite [--blobHost <IP address>] [--blobPort <port address>] 
    [-d | --debug <log file path>] [-l | --location <workspace path>]
    [--queueHost <IP address>] [--queuePort <port address>]
    [-s | --silent] [-h | --help]
```

**-d** için bir kısayol **-debug**, **-l** anahtarı için bir kısayol **-konum**, **-s** için bir kısayol **-sessiz**, ve **-h** için bir kısayol olduğunu **--yardım**.

### <a name="blob-listening-host"></a>Blob dinleme sunucusu

**İsteğe bağlı** Varsayılan olarak, Azurite yerel sunucu olarak 127.0.0.1 dinleyecek. Adresi gereksinimlerinize ayarlamak için **--blobHost** anahtarını kullanın.

Yalnızca yerel makinedeki istekleri kabul edin:

```console
azurite --blobHost 127.0.0.1
```

Uzaktan isteklere izin ver:

```console
azurite --blobHost 0.0.0.0
```

> [!CAUTION]
> Uzaktan isteklere izin vermek, sisteminizi dış saldırılara karşı savunmasız hale getirebilir.

### <a name="blob-listening-port-configuration"></a>Blob dinleme portu yapılandırması

**İsteğe bağlı** Varsayılan olarak, Azurite port 10000 blob hizmeti için dinleyecek. Gereksinim duyduğunuz dinleme bağlantı noktasını belirtmek için **--blobPort** anahtarını kullanın.

> [!NOTE]
> Özelleştirilmiş bir bağlantı noktası nı kullandıktan sonra, Azure Depolama araçlarınızda veya SDK'larınızda bağlantı dizesini veya ilgili yapılandırmayı güncelleştirmeniz gerekir.

Blob servis dinleme bağlantı noktasını özelleştirin:

```console
azurite --blobPort 8888
```

Sistemin otomatik olarak kullanılabilir bir bağlantı noktasını seçmesine izin verin:

```console
azurite --blobPort 0
```

Kullanılan bağlantı noktası Azurite başlatma sırasında görüntülenir.

### <a name="queue-listening-host"></a>Sıra dinleme ana bilgisayarı

**İsteğe bağlı** Varsayılan olarak, Azurite yerel sunucu olarak 127.0.0.1 dinleyecek. Adresi gereksinimlerinize ayarlamak için **--queueHost** anahtarını kullanın.

Yalnızca yerel makinedeki istekleri kabul edin:

```console
azurite --queueHost 127.0.0.1
```

Uzaktan isteklere izin ver:

```console
azurite --queueHost 0.0.0.0
```

> [!CAUTION]
> Uzaktan isteklere izin vermek, sisteminizi dış saldırılara karşı savunmasız hale getirebilir.

### <a name="queue-listening-port-configuration"></a>Sıra dinleme bağlantı noktası yapılandırması

**İsteğe bağlı** Varsayılan olarak, Azurite 10001 portundaki Kuyruk hizmetini dinler. Gereksinim duyduğunuz dinleme bağlantı noktasını belirtmek için **--queuePort** anahtarını kullanın.

> [!NOTE]
> Özelleştirilmiş bir bağlantı noktası nı kullandıktan sonra, Azure Depolama araçlarınızda veya SDK'larınızda bağlantı dizesini veya ilgili yapılandırmayı güncelleştirmeniz gerekir.

Sıra hizmeti dinleme bağlantı noktasını özelleştirin:

```console
azurite --queuePort 8888
```

Sistemin otomatik olarak kullanılabilir bir bağlantı noktasını seçmesine izin verin:

```console
azurite --queuePort 0
```

Kullanılan bağlantı noktası Azurite başlatma sırasında görüntülenir.

### <a name="workspace-path"></a>Çalışma alanı yolu

**İsteğe bağlı** Azurite yürütme sırasında verileri yerel diske depolar. Çalışma alanı konumu olarak bir yol belirtmek için **--konum** anahtarını kullanın. Varsayılan olarak, geçerli işlem çalışma dizini kullanılacaktır.

```console
azurite --location c:\azurite
```

```console
azurite -l c:\azurite
```

### <a name="access-log"></a>Erişim günlüğü

**İsteğe bağlı** Varsayılan olarak, erişim günlüğü konsol penceresinde görüntülenir. **Sessiz** anahtarı kullanarak erişim günlüğünün ekranını devre dışı kılmış olur.

```console
azurite --silent
```

```console
azurite -s
```

### <a name="debug-log"></a>Hata ayıklama günlüğü

**İsteğe bağlı** Hata ayıklama günlüğü, her istek ve özel durum yığını izleme ayrıntılı bilgi içerir. **--hata ayıklama** anahtarına geçerli bir yerel dosya yolu sağlayarak hata ayıklama günlüğünü etkinleştirin.

```console
azurite --debug path/debug.log
```

```console
azurite -d path/debug.log
```

### <a name="loose-mode"></a>Gevşek mod

**İsteğe bağlı** Varsayılan olarak, Azurite desteklenmeyen istek üstbilgilerini ve parametreleri engellemek için sıkı mod uygular. --gevşek anahtarı kullanarak sıkı modu devre dışı **bırakın.**

```console
azurite --loose
```

Büyük 'L' kısayol anahtarına dikkat edin:

```console
azurite -L
```

## <a name="authorization-for-tools-and-sdks"></a>Araçlar ve SDK'lar için yetkilendirme

Herhangi bir kimlik doğrulama stratejisini kullanarak Azure Depolama SDK'larından veya [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)gibi araçlardan Azurite'ye bağlanın. Kimlik doğrulaması gereklidir. Azurite, Paylaşılan Anahtar ve paylaşılan erişim imzaları (SAS) ile yetkilendirmeyi destekler. Azurite ayrıca kamu konteynerlerine anonim erişimi de destekler.

### <a name="well-known-storage-account-and-key"></a>Tanınmış depolama hesabı ve anahtar

Aşağıdaki hesap adını ve anahtarı azurite ile kullanabilirsiniz. Bu, eski Azure depolama emülatörü tarafından kullanılan aynı iyi bilinen hesap ve anahtardır.

* Hesap adı:`devstoreaccount1`
* Hesap anahtarı:`Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==`

> [!NOTE]
> SharedKey kimlik doğrulamasına ek olarak, Azurite hesap ve hizmet SAS kimlik doğrulamasını destekler. Bir kapsayıcı genel erişime izin verecek şekilde ayarlandığında anonim erişim de kullanılabilir.

### <a name="connection-string"></a>Bağlantı dizesi

Uygulamanızdan Azurite'ye bağlanmanın en kolay yolu, uygulamanızın yapılandırma dosyasında *UseDevelopmentStorage=true*kısayoluna başvuran bir bağlantı dizesini yapılandırmaktır. Aşağıda, *app.config* dosyasındaki bağlantı dizesinin bir örneği verilmiştir:

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

Daha fazla bilgi için [bkz.](storage-configure-connection-string.md)

### <a name="custom-storage-accounts-and-keys"></a>Özel depolama hesapları ve anahtarları

Azurite, ortam değişkenini `AZURITE_ACCOUNTS` aşağıdaki biçimde ayarlayarak özel `account1:key1[:key2];account2:key1[:key2];...`depolama hesabı adlarını ve anahtarlarını destekler: .

Örneğin, tek bir anahtarı olan özel bir depolama hesabı kullanın:

```cmd
set AZURITE_ACCOUNTS="account1:key1"
```

Veya her biri 2 anahtariçeren birden fazla depolama hesabı kullanın:

```cmd
set AZURITE_ACCOUNTS="account1:key1:key2;account2:key1:key2"
```

Azurite, varsayılan olarak her dakika ortam değişkeninden özel hesap adlarını ve anahtarlarını yeniler. Bu özellik sayesinde, hesap anahtarını dinamik olarak döndürebilir veya Azurite'yi yeniden başlatmadan yeni depolama hesapları ekleyebilirsiniz.

> [!NOTE]
> Özel `devstoreaccount1` depolama hesapları ayarladığınızda varsayılan depolama hesabı devre dışı bırakılır.

> [!NOTE]
> Özel hesap adlarını ve anahtarlarını kullanırken bağlantı dizesini buna göre güncelleştirin.

> [!NOTE]
> Windows'da `export` kullanılan Linux ortamında ortam değişkenlerini `set` ayarlamak için anahtar kelimeyi kullanın.

### <a name="storage-explorer"></a>Depolama Gezgini

Azure Depolama Gezgini'nde, **Hesap Ekle** simgesine tıklayarak Azurite'ye bağlanın, ardından yerel **bir emülatöre ekle'yi** seçin ve **Bağlan'ı**tıklatın.

## <a name="differences-between-azurite-and-azure-storage"></a>Azurit ve Azure Depolama arasındaki farklar

Bulutta yerel bir Azurite örneği ile Azure Depolama hesabı arasında işlevsel farklar vardır.

### <a name="endpoint-and-connection-url"></a>Bitiş noktası ve bağlantı URL'si

Azurite için hizmet bitiş noktaları, Bir Azure Depolama hesabının bitiş noktalarından farklıdır. Yerel bilgisayar etki alanı adı çözümlemesi yapmaz, Azurite uç noktalarının yerel adresler olmasını gerektirir.

Bir Azure Depolama hesabındabir kaynağa hitap ettiğinizde, hesap adı URI ana bilgisayar adının bir parçasıdır. Ele alınmakta olan kaynak URI yolunun bir parçasıdır:

`<http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>`

Aşağıdaki URI, Azure Depolama hesabındaki bir blob için geçerli bir adrestir:

`https://myaccount.blob.core.windows.net/mycontainer/myblob.txt`

Yerel bilgisayar etki alanı adı çözümlemesi yapmadığından, hesap adı ana bilgisayar adı yerine URI yolunun bir parçasıdır. Azurite'deki bir kaynak için aşağıdaki URI biçimini kullanın:

`http://<local-machine-address>:<port>/<account-name>/<resource-path>`

Aşağıdaki adres Azurite bir blob erişmek için kullanılabilir:

`http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt`

### <a name="scaling-and-performance"></a>Ölçekleme ve performans

Azürit ölçeklenebilir bir depolama hizmeti değildir ve çok sayıda eşzamanlı istemciyi desteklemez. Performans garantisi yok. Azürit geliştirme ve test amaçlıdır.

### <a name="error-handling"></a>Hata işleme

Azurite, Azure Depolama hata işleme mantığıyla uyumludur, ancak farklılıklar vardır. Örneğin, hata durum kodları hizalanırken hata iletileri farklı olabilir.

### <a name="ra-grs"></a>RA-GRS

Azürit, okuma-erişim coğrafi-yedekli çoğaltmayı (RA-GRS) destekler. Depolama kaynakları için, hesap adına **ikincil ekleyerek** ikincil konuma erişin. Örneğin, Aşağıdaki adres, Azurite'deki salt okunur ikincil bilgileri kullanarak bir blob'a erişmek için kullanılabilir:

`http://127.0.0.1:10000/devstoreaccount1-secondary/mycontainer/myblob.txt`

## <a name="azurite-is-open-source"></a>Azürit açık kaynak kodludur

Katkıları ve Azurite için öneriler bekliyoruz. Yaklaşan özellikler ve hata düzeltmeleri için izlediğimiz kilometre taşları ve iş öğeleri için Azurite [GitHub proje](https://github.com/Azure/Azurite/projects) sayfasına veya [GitHub sorunlarına](https://github.com/Azure/Azurite/issues) gidin. Ayrıntılı çalışma öğeleri de GitHub'da izlenir.

## <a name="next-steps"></a>Sonraki adımlar

* Azurite tarafından yerini alan eski Azure depolama emülatörü geliştirme ve test belgeleri [için Azure depolama emülatörü'ni kullanın.](storage-use-emulator.md)
* [Azure Depolama bağlantı dizelerini yapılandırmak,](storage-configure-connection-string.md) geçerli bir Azure STorage bağlantı dizesini nasıl birleştirin;
