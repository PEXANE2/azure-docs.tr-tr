---
title: Aygıtlarda blok blobları depolama - Azure IoT Edge | Microsoft Dokümanlar
description: Katmanlama ve zaman içinde yaşam daki özellikleri anlayın, desteklenen blob depolama işlemlerini görün ve blob depolama hesabınıza bağlanın.
author: kgremban
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 12/13/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: bea00f429f31f2be62ee6a9c00f88873c595d94c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76509827"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge"></a>IoT Edge'de Azure Blob Depolama ile verileri kenarda depolama

IoT Edge'deki Azure Blob Depolama, kenarda bir [blok blob](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs) ve ek [blob](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-append-blobs) depolama çözümü sağlar. IoT Edge aygıtınızdaki bir blob depolama modülü, ioT Edge aygıtınızda yerel olarak depolanması dışında Azure blob hizmeti gibi çalışır. Lekelerinize, alışık olduğunuz aynı Azure depolama SDK yöntemlerini veya blob API çağrılarını kullanarak erişebilirsiniz. Bu makalede, IoT Edge aygıtınızda blob hizmeti çalıştıran IoT Edge kapsayıcıdaki Azure Blob Depolamaile ilgili kavramlar açıklanmaktadır.

Bu modül senaryolarda yararlıdır:

* işlenebilir veya buluta aktarılana kadar verilerin yerel olarak depolanması gerekir. Bu veriler videolar, görüntüler, finans verileri, hastane verileri veya diğer yapılandırılmamış veriler olabilir.
* aygıtlar sınırlı bağlantı ya da bağlantı ya da
* verilere düşük gecikme erişimi elde etmek için verileri yerel olarak verimli bir şekilde işlemek istediğinizde, örneğin acil durumlara mümkün olan en kısa sürede yanıt verebilirsiniz.
* bant genişliği maliyetlerini azaltmak ve terabaytlarca veriyi buluta aktarmaktan kaçınmak istediğinizde. Verileri yerel olarak işleyebilir ve yalnızca işlenen verileri buluta gönderebilirsiniz.

Hızlı giriş için videoyu izleyin
> [!VIDEO https://www.youtube.com/embed/xbwgMNGB_3Y]

Bu modül **deviceToCloudUpload** ve **deviceAutoDelete** özellikleri ile birlikte gelir.

**deviceToCloudUpload** yapılandırılabilir bir işlevseldir. Bu işlev, yerel blob depolama alanınızdaki verileri aralıklı internet bağlantısı desteğiyle azure'a otomatik olarak yükler. Şunları yapmanızı sağlar:

* Cihazı Açma/KAPAMAToCloudUpload özelliğini açın.
* En Yeni İlk veya En Eski İlk gibi verilerin Azure'a kopyalandığı sırayı seçin.
* Verilerinizin yüklenmesini istediğiniz Azure Depolama hesabını belirtin.
* Azure'a yüklemek istediğiniz kapsayıcıları belirtin. Bu modül, hem kaynak hem de hedef kapsayıcı adlarını belirtmenizi sağlar.
* Bulut depolamaya yükleme tamamlandıktan sonra lekeleri hemen silme özelliğini seçin
* Tam blob yükleme `Put Blob` (işlemi kullanarak) ve `Put Block`blok `Put Block List` `Append Block` düzeyi yükleme (kullanarak , ve işlemleri) yapın.

Bu modül, blob bloklardan oluştuğunda blok seviyesi yüklemekullanır. Sık karşılaşılan senaryolardan bazıları şunlardır:

* Uygulamanız daha önce yüklenen blok blob bazı blokları günceller veya bir ek blob yeni bloklar ekler, bu modül sadece güncelleştirilmiş bloklar yükler ve tüm blob.
* Modül blob yükleniyor ve internet bağlantısı kayboluyor, bağlantı geri geldiğinde sadece kalan blokları değil, tüm blob yükler.

Beklenmeyen bir işlem sonlandırma (güç kesintisi gibi) bir blob yükleme sırasında gerçekleşirse, modül yeniden çevrimiçi olduğunda yükleme için gereken tüm bloklar yeniden yüklenir.

**deviceAutoDelete** yapılandırılabilir bir işlevseldir. Bu işlev, belirtilen süre (dakika cinsinden ölçülen) süresi dolduğunda lekelerinizi yerel depolama alanından otomatik olarak siler. Şunları yapmanızı sağlar:

* DeviceAutoDelete özelliğini AÇıN/KAPATıN.
* Lekelerin otomatik olarak silineceği dakika (deleteAfterMinutes) süresini belirtin.
* DeleteAfterMinutes değeri sona erdiğinde, yükleme sırasında blob'u koruma özelliğini seçin.

## <a name="prerequisites"></a>Ön koşullar

Bir Azure IoT Edge cihazı:

* [Linux](quickstart-linux.md) veya [Windows aygıtları](quickstart.md)için hızlı başlatma adımlarını izleyerek geliştirme makinenizi veya sanal makinenizi IoT Edge aygıtı olarak kullanabilirsiniz.

* Desteklenen işletim sistemleri ve mimarilerinin listesi için [Azure IoT Edge desteklenen sistemlere](support.md#operating-systems) bakın. IoT Edge modülündeki Azure Blob Depolama aşağıdaki mimarileri destekler:
  * Windows AMD64
  * Linux AMD64
  * Linux ARM32
  * Linux ARM64 (önizleme)

Bulut kaynakları:

Azure'da standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).

## <a name="devicetocloudupload-and-deviceautodelete-properties"></a>deviceToCloudUpload ve deviceAutoDelete özellikleri

**DeviceToCloudUploadProperties** ve **deviceAutoDeleteProperties'i**ayarlamak için modülün istenilen özelliklerini kullanın. İstenen özellikler dağıtım sırasında ayarlanabilir veya daha sonra modül ikizi yeniden dağıtmaya gerek kalmadan düzenleyerek değiştirilebilir. "Modül İkiz"ini `reported configuration` kontrol `configurationValidation` etmenizi ve değerlerin doğru şekilde yayDığından emin olmamızı öneririz.

### <a name="devicetoclouduploadproperties"></a>deviceToCloudUploadProperties

Bu ayarın `deviceToCloudUploadProperties`adı. IoT Edge simülatörü kullanıyorsanız, açıklama bölümünde bulabileceğiniz bu özellikler için ilgili ortam değişkenlerine değerleri ayarlayın.

| Özellik | Olası Değerler | Açıklama |
| ----- | ----- | ---- |
| uploadOn | doğru, yanlış | `false` Varsayılan olarak ayarlayın. Özelliği açmak istiyorsanız, bu alanı 'da `true`ayarla <br><br> Ortam değişkeni:`deviceToCloudUploadProperties__uploadOn={false,true}` |
| uploadOrder | En Yeni, En Eskiİlk | Verilerin Azure'a kopyalandığı sırayı seçmenize olanak tanır. `OldestFirst` Varsayılan olarak ayarlayın. Sipariş Blob'un son değiştirilen zamanına göre belirlenir. <br><br> Ortam değişkeni:`deviceToCloudUploadProperties__uploadOrder={NewestFirst,OldestFirst}` |
| cloudStorageConnectionString |  | `"DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>"`verilerinizin yüklenmesini istediğiniz depolama hesabını belirtmenizi sağlayan bir bağlantı dizesidir. `Azure Storage Account Name`Belirtin `Azure Storage Account Key` `End point suffix`, . Verilerin yüklendiği azure'un uygun EndpointSuffix'ini ekleyin, Global Azure, Azure ve Microsoft Azure Yığını için değişir. <br><br> Azure Depolama SAS bağlantı dizesini burada belirtmeyi seçebilirsiniz. Ancak süresi dolduğunda bu özelliği güncelleştirmeniz gerekir. <br><br> Ortam değişkeni:`deviceToCloudUploadProperties__cloudStorageConnectionString=<connection string>` |
| depolamaContainersForUpload | `"<source container name1>": {"target": "<target container name>"}`,<br><br> `"<source container name1>": {"target": "%h-%d-%m-%c"}`, <br><br> `"<source container name1>": {"target": "%d-%c"}` | Azure'a yüklemek istediğiniz kapsayıcı adlarını belirtmenizi sağlar. Bu modül, hem kaynak hem de hedef kapsayıcı adlarını belirtmenizi sağlar. Hedef kapsayıcı adını belirtmezseniz, kapsayıcı adını otomatik olarak `<IoTHubName>-<IotEdgeDeviceID>-<ModuleName>-<SourceContainerName>`. Hedef kapsayıcı adı için şablon dizeleri oluşturabilir, olası değerler sütununa göz atabilirsiniz. <br>* %h -> IoT Hub Adı (3-50 karakter). <br>* %d -> IoT Edge Aygıt Kimliği (1 ila 129 karakter). <br>* %m -> Modül Adı (1 - 64 karakter). <br>* %c -> Kaynak Kapsayıcı Adı (3 ila 63 karakter). <br><br>Kapsayıcı adının maksimum boyutu 63 karakterdir, kapsayıcının boyutu 63 karakteri aşarsa otomatik olarak hedef kapsayıcı adını atanırsa her bölümü (IoTHubName, IotEdgeDeviceID, ModuleName, SourceContainerName) 15'e kırpacaktır Karakter. <br><br> Ortam değişkeni:`deviceToCloudUploadProperties__storageContainersForUpload__<sourceName>__target=<targetName>` |
| silmeAfterUpload | doğru, yanlış | `false` Varsayılan olarak ayarlayın. Bulut `true`depolama alanına yükleme tamamlandığında verileri otomatik olarak siler. <br><br> **DİkKAT**: Apandisi blobs kullanıyorsanız, bu ayar başarılı bir yüklemeden sonra yerel depolama dan append blobs siler ve bu lekeler için gelecekteki Append Block işlemleri başarısız olur. Bu ayarı dikkatli kullanın, uygulamanız seyrek ek işlemleri yapıyorsa veya sürekli ek işlemleri desteklemiyorsa bunu etkinleştirme<br><br> Ortam değişkeni: `deviceToCloudUploadProperties__deleteAfterUpload={false,true}`. |

### <a name="deviceautodeleteproperties"></a>deviceAutoDeleteProperties

Bu ayarın `deviceAutoDeleteProperties`adı. IoT Edge simülatörü kullanıyorsanız, açıklama bölümünde bulabileceğiniz bu özellikler için ilgili ortam değişkenlerine değerleri ayarlayın.

| Özellik | Olası Değerler | Açıklama |
| ----- | ----- | ---- |
| deleteOn | doğru, yanlış | `false` Varsayılan olarak ayarlayın. Özelliği açmak istiyorsanız, bu alanı 'da `true`ayarla <br><br> Ortam değişkeni:`deviceAutoDeleteProperties__deleteOn={false,true}` |
| deleteAfterMinutes | `<minutes>` | Saati dakika içinde belirtin. Bu değer sona erdiğinde modül lekelerinizi yerel depolama dan otomatik olarak siler. <br><br> Ortam değişkeni:`deviceAutoDeleteProperties__ deleteAfterMinutes=<minutes>` |
| retainUploading sırasında | doğru, yanlış | Varsayılan olarak ayarlanır `true`ve deleteAfterMinutes süresi dolduğunda bulut depolama ya yüklenirken blob korur. Bunu ayarlayabilirsiniz `false` ve siler siler siler siler siler siler bitmez verileri siler. Not: Bu özelliğin çalışması için uploadOn'un doğru olarak ayarlanmalıdır.  <br><br> **DİkKAT**: Ek blobs kullanıyorsanız, bu ayar değeri sona erdiğinde yerel depolama dan ek lekeleri siler ve bu lekeler için gelecekteki Append Block işlemleri başarısız olur. Son kullanma değerinin, uygulamanız tarafından gerçekleştirilen ek işlemlerin beklenen sıklığı için yeterince büyük olduğundan emin olmak isteyebilirsiniz.<br><br> Ortam değişkeni:`deviceAutoDeleteProperties__retainWhileUploading={false,true}`|

## <a name="using-smb-share-as-your-local-storage"></a>Kobİ payını yerel depolama alanınız olarak kullanma

Bu modülün Windows kapsayıcısına Windows ana bilgisayarda dağıttığınızda, yerel depolama yolunuz olarak SMB payı sağlayabilirsiniz.

SMB paylaşımı ve IoT aygıtının karşılıklı güvenilen etki alanlarında olduğundan emin olun.

Windows çalıştıran IoT aygıtında Kobİ paylaşımını yerel olarak eşlemek için PowerShell komutunu çalıştırabilirsiniz. `New-SmbGlobalMapping`

Yapılandırma adımları aşağıda verilmiştir:

```PowerShell
$creds = Get-Credential
New-SmbGlobalMapping -RemotePath <remote SMB path> -Credential $creds -LocalPath <Any available drive letter>
```

Örnek:

```powershell
$creds = Get-Credential
New-SmbGlobalMapping -RemotePath \\contosofileserver\share1 -Credential $creds -LocalPath G:
```

Bu komut, uzak SMB sunucusuyla kimlik doğrulama yapmak için kimlik bilgilerini kullanır. Ardından, uzak paylaşım yolunu G: drive letter (diğer kullanılabilir sürücü harfi olabilir) ile eşleyin. IoT aygıtı artık veri hacmini G: sürücü üzerinde bir yola eşledi.

IoT aygıtındaki kullanıcının uzak Kobİ paylaşımına okuma/yazma yapabilmesini unutmayın.

Dağıtımınız için değeri `<storage mount>` **G:/ContainerData:C:/BlobRoot**olabilir.

## <a name="granting-directory-access-to-container-user-on-linux"></a>Linux'ta konteyner kullanıcısına dizin erişimi verilmesi

Linux kapsayıcıları için oluşturma seçeneklerinizde depolama için [birim montaj](https://docs.docker.com/storage/volumes/) kullandıysanız, o zaman herhangi bir ek adım yapmanız gerekmez, ancak [bağlama montaj](https://docs.docker.com/storage/bind-mounts/) kullandıysanız, bu adımların hizmeti doğru çalıştırmak için gereklidir.

Kullanıcıların işlerini gerçekleştirmek için gereken minimum izinlerle erişim haklarını sınırlandırmak için en az ayrıcalık ilkesini izleyen bu modül, bir kullanıcı (ad: absie, ID: 11000) ve bir kullanıcı grubu (ad: absie, ID: 11000) içerir. Kapsayıcı **kök** olarak başlatılırsa (varsayılan kullanıcı **köktür),** hizmetimiz düşük ayrıcalıklı **absie** kullanıcısı olarak başlatılacaktır.

Bu davranış, ana bilgisayar yolundaki izinlerin yapılandırmasını, hizmetin düzgün çalışması için çok önemli olan bağlamalar yapar, aksi takdirde hizmet erişim reddedilen hatalarla çlenir. Dizin bağlamada kullanılan yolun kapsayıcı kullanıcısı tarafından erişilebilir olması gerekir (örnek: absie 11000). Aşağıdaki komutları ana bilgisayarda çalıştırarak kapsayıcı kullanıcısının dizine erişme sini sağlayabilirsiniz:

```terminal
sudo chown -R 11000:11000 <blob-dir>
sudo chmod -R 700 <blob-dir>
```

Örnek:

```terminal
sudo chown -R 11000:11000 /srv/containerdata
sudo chmod -R 700 /srv/containerdata
```

Hizmeti **absie**dışında bir kullanıcı olarak çalıştırmanız gerekiyorsa, dağıtım bildiriminizde "Kullanıcı" özelliği altında createOptions'ta özel kullanıcı kimliğinizi belirtebilirsiniz. Bu durumda varsayılan veya kök grup `0`kimliği kullanmanız gerekir.

```json
"createOptions": {
  "User": "<custom user ID>:0"
}
```

Şimdi, kapsayıcı kullanıcının dizine erişimini tanıyın

```terminal
sudo chown -R <user ID>:<group ID> <blob-dir>
sudo chmod -R 700 <blob-dir>
```

## <a name="configure-log-files"></a>Günlük dosyalarını yapılandırma

Modülünüz için günlük dosyalarını yapılandırma hakkında daha fazla bilgi için bu [üretim en iyi uygulamalarına](https://docs.microsoft.com/azure/iot-edge/production-checklist#set-up-logs-and-diagnostics)bakın.

## <a name="connect-to-your-blob-storage-module"></a>Blob depolama modülünüze bağlanın

IoT Edge aygıtınızdaki blob depolama alanına erişmek için modülünüz için yapılandırdığınız hesap adı ve hesap anahtarını kullanabilirsiniz.

IoT Edge aygıtınızı, yaptığınız depolama istekleri için blob bitiş noktası olarak belirtin. IoT Edge aygıt bilgilerini ve yapılandırdığınız hesap adını kullanarak [açık bir depolama bitiş noktası için bağlantı dizesi oluşturabilirsiniz.](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint)

* IoT Edge modülündeki Azure Blob Depolama'nın çalıştığı cihazda dağıtılan modüller için blob bitiş `http://<module name>:11002/<account name>`noktası: .
* Farklı bir aygıtta çalışan modüller veya uygulamalar için, ağınız için doğru bitiş noktasını seçmeniz gerekir. Ağ kurulumunuza bağlı olarak, harici modülünüzden veya uygulamanızdan gelen veri trafiğinin IoT Edge modülünde Azure Blob Depolamasını çalıştıran aygıta ulaşabileceği bir uç nokta biçimi seçin. Bu senaryo için blob bitiş noktası biridir:
  * `http://<device IP >:11002/<account name>`
  * `http://<IoT Edge device hostname>:11002/<account name>`
  * `http://<fully qualified domain name>:11002/<account name>`

## <a name="azure-blob-storage-quickstart-samples"></a>Azure Blob Depolama hızlı başlatma örnekleri

Azure Blob Depolama belgeleri, çeşitli dillerde hızlı başlatma örnek kodu içerir. Yerel blob depolama modülüne bağlanmak için blob bitiş noktasını değiştirerek Azure Blob Depolamasını IoT Edge'de test etmek için bu örnekleri çalıştırabilirsiniz.

Aşağıdaki hızlı başlatma örnekleri, IoT Edge tarafından da desteklenen dilleri kullanır, böylece bunları blob depolama modülünün yanında IoT Edge modülleri olarak dağıtabilirsiniz:

* [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Python](../storage/blobs/storage-quickstart-blobs-python.md)
  * Python SDK'nın V2.1'den önceki sürümlerinde, modülün blob oluşturma süresini döndürmediği bilinen bir sorun vardır. Bu sorun nedeniyle, liste lekeleri gibi bazı yöntemler çalışmıyor. Geçici çözüm olarak, blob istemcisindeki API sürümünü açıkça '2017-04-17' olarak ayarlayın. Örnek:`block_blob_service._X_MS_VERSION = '2017-04-17'`
  * [Ek Blob Örneği](https://github.com/Azure/azure-storage-python/blob/master/samples/blob/append_blob_usage.py)
* [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs-legacy.md)
* [JS/HTML](../storage/blobs/storage-quickstart-blobs-javascript-client-libraries-legacy.md)
* [Ruby](../storage/blobs/storage-quickstart-blobs-ruby.md)
* [Git](../storage/blobs/storage-quickstart-blobs-go.md)
* [PHP](../storage/blobs/storage-quickstart-blobs-php.md)

## <a name="connect-to-your-local-storage-with-azure-storage-explorer"></a>Azure Depolama Gezgini ile yerel depolama alanınıza bağlanın

Yerel depolama hesabınıza bağlanmak için [Azure Depolama Gezgini'ni](https://azure.microsoft.com/features/storage-explorer/) kullanabilirsiniz.

1. Azure Depolama Gezgini’ni indirme ve yükleme

1. Bağlantı dizesini kullanarak Azure Depolama'ya bağlanma

1. Bağlantı dizesi sağlayın:`DefaultEndpointsProtocol=http;BlobEndpoint=http://<host device name>:11002/<your local account name>;AccountName=<your local account name>;AccountKey=<your local account key>;`

1. Bağlanmak için adımları gözden geçirin.

1. Yerel depolama hesabınızda kapsayıcı oluşturma

1. Dosyaları Blok blobs veya Append Blobs olarak yüklemeye başlayın.
   > [!NOTE]
   > Bu modül Sayfa blobs desteklemez.

1. Azure depolama hesaplarınızı Depolama Gezgini'nde de bağlamayı seçebilirsiniz. Bu yapılandırma, hem yerel depolama hesabınız hem de Azure depolama hesabınız için tek bir görünüm sağlar

## <a name="supported-storage-operations"></a>Desteklenen depolama işlemleri

IoT Edge'deki Blob depolama modülleri Azure Depolama SDK'larını kullanır ve blok blob uç noktaları için Azure Depolama API'sinin 2017-04-17 sürümüyle tutarlıdır.

Tüm Azure Blob Depolama işlemleri IoT Edge'deki Azure Blob Depolama tarafından desteklenmedığından, bu bölümde her birinin durumu listelenir.

### <a name="account"></a>Hesap

Desteklenen:

* Kapsayıcıları listeleme

Desteklenme -yen:

* Blob servis özelliklerini alın ve ayarlayın
* Ön kontrol blob isteği
* Blob servis istatistiklerini alın
* Hesap bilgilerini alma

### <a name="containers"></a>Kapsayıcılar

Desteklenen:

* Kapsayıcı oluşturma ve silme
* Kapsayıcı özelliklerini ve meta verileri alın
* Blobları listeleme
* Konteyner ACL'yi alın ve ayarlayın
* Kapsayıcı meta verilerini ayarlama

Desteklenme -yen:

* Kira konteyneri

### <a name="blobs"></a>Bloblar

Desteklenen:

* Blob'u koyun, alın ve silin
* Blob özelliklerini alın ve ayarlayın
* Blob meta verilerini alın ve ayarlayın

Desteklenme -yen:

* Kira blob
* Anlık görüntü blob
* Kopyalama ve iptal kopya blob
* Undelete blob
* Blob katmanını ayarlayın

### <a name="block-blobs"></a>Blok blobları

Desteklenen:

* Blok koyma
* Blok listesini koy ve al

Desteklenme -yen:

* URL'den blok koyma

### <a name="append-blobs"></a>Ekleme blobları

Desteklenen:

* Ek blok

Desteklenme -yen:

* URL'den ek engelleme

## <a name="event-grid-on-iot-edge-integration"></a>IoT Edge Entegrasyonunda Olay Izgara

> [!CAUTION]
> IoT Edge'deki Event Grid ile tümleştirme önizlemede

IoT Edge modülündeki bu Azure Blob Depolama, artık IoT Edge'deki Event Grid ile tümleştirme sağlar. Bu tümleştirme hakkında ayrıntılı bilgi [için, modülleri dağıtmak, olayları yayımlamak ve olay teslimini doğrulamak için öğreticiye](../event-grid/edge/react-blob-storage-events-locally.md)bakın.

## <a name="release-notes"></a>Sürüm Notları

İşte bu modül için [docker hub sürüm notları](https://hub.docker.com/_/microsoft-azure-blob-storage)

## <a name="feedback"></a>Geri Bildirim

Geri bildiriminiz bu modülü ve özelliklerini kullanışlı ve kullanımı kolay hale getirmek için bizim için önemlidir. Lütfen görüşlerinizi paylaşın ve nasıl geliştirebileceğimizi bize bildirin.

Bize şu andan ulaşabilirsiniz:absiotfeedback@microsoft.com

## <a name="next-steps"></a>Sonraki adımlar

[Azure Blob Depolamasını IoT Edge'de](how-to-deploy-blob.md) nasıl dağıtılayacağım öğrenin

[IoT Edge blogundaki Azure Blob Depolama'daki](https://aka.ms/abs-iot-blogpost) en son güncellemeler ve duyurularla güncel kalın
