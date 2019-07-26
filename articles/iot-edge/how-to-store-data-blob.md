---
title: Blok blobları cihazlarda - Azure IOT Edge Store | Microsoft Docs
description: Katmanlama ve yaşam süresi özelliklerini anlayın, bkz. desteklenen blob Storage işlemleri ve BLOB depolama hesabınıza bağlanma.
author: arduppal
manager: mchad
ms.author: arduppal
ms.reviewer: arduppal
ms.date: 06/19/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: c5a27a8016202f7f8c9e256eaf6b3077fbef295b
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414521"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>IOT Edge (Önizleme) Azure Blob Depolama ile uçta veri Store

IOT Edge üzerinde Azure Blob Depolama sağlayan bir [blok blobu](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs) uçta depolama çözümü. IoT Edge cihazınızdaki BLOB depolama modülü bir Azure Blok Blobu hizmeti gibi davranır, ancak blok Blobları IoT Edge cihazınızda yerel olarak depolanır. Aynı Azure depolama SDK'ın yöntemleri kullanarak bloblarınızın erişebilir veya zaten kullanılan blob API çağrılarını engelle. Bu makalede, IoT Edge cihazınızda bir blob hizmeti çalıştıran IoT Edge kapsayıcısında Azure Blob depolama ile ilgili kavramlar açıklanmaktadır.

Bu modül, verilerin işlenene veya buluta aktarılıncaya kadar yerel olarak depolanması gereken senaryolarda yararlıdır. Bu veriler, videolar, resimler, Finans verileri, barındırma verileri veya diğer yapılandırılmamış veriler olabilir.

> [!NOTE]
> IOT Edge üzerinde Azure Blob Depolama, içinde [genel Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Hızlı giriş için videoyu izleyin
> [!VIDEO https://www.youtube.com/embed/QhCYCvu3tiM]

Bu modül **Devicetocloudupload** ve **deviceoto Delete** özellikleriyle birlikte gelir.

**Devicetocloudupload** , yapılandırılabilir bir işlevdir. Bu işlev, zaman aralıklı internet bağlantısı desteğiyle, verileri yerel blob depolamadan Azure 'a otomatik olarak yükler. Şunları yapmanıza olanak sağlar:

- DeviceToCloudUpload özelliğini açın/kapatın.
- Verilerin NewestFirst veya OldestFirst gibi Azure 'a kopyalandığı sırayı seçin.
- Verilerinizin karşıya yüklenmesini istediğiniz Azure Depolama hesabını belirtin.
- Azure 'a yüklemek istediğiniz kapsayıcıları belirtin. Bu modül hem kaynak hem de hedef kapsayıcı adlarını belirtmenize olanak tanır.
- Bulut depolamaya yükleme tamamlandıktan sonra Blobları hemen silme özelliğini seçin
- Tam blob yüklemesi yapın (işlem `Put Blob` kullanarak) ve blok düzeyinde karşıya yükleme ( `Put Block` ve `Put Block List` işlemlerini kullanarak).

Bu modül Blobun blokları içeriyorsa blok düzeyinde karşıya yükleme kullanır. Yaygın senaryolardan bazıları şunlardır:

- Uygulamanız, daha önce karşıya yüklenen bir Blobun bazı blokları güncelleştirir, bu modül tüm blobu değil yalnızca güncelleştirilmiş blokları karşıya yükler.
- Modül, blob 'u karşıya yüklüyor ve internet bağlantısı geri geldiğinde, bağlantı tekrar tekrar olduğunda blob tamamını değil yalnızca kalan blokları karşıya yükler.

Blob karşıya yükleme sırasında beklenmeyen bir işlem sonlandırması (güç hatası gibi) olursa, modül yeniden çevrimiçi olduktan sonra karşıya yükleme işlemi sırasında oluşan tüm bloklar tekrar karşıya yüklenir.

**Deviceoto silme** , yapılandırılabilir bir işlevdir. Bu işlev, belirtilen süre (dakika cinsinden ölçülür) sona erdiğinde bloblarınızı yerel depolamadan otomatik olarak siler. Şunları yapmanıza olanak sağlar:

- Deviceoto Delete özelliğini açın/kapatın.
- Blobların otomatik olarak silineceği süreyi dakika cinsinden (Deleteafsonlandırutes) belirtin.
- Deleteafsonlandırutes değerinin süresi dolarsa blobu karşıya yüklerken bekletme özelliğini seçin.


## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

- [Linux](quickstart-linux.md) veya [Windows cihazları](quickstart.md)için Hızlı Başlangıç bölümündeki adımları izleyerek geliştirme makinenizi veya bir sanal makineyi IoT Edge bir cihaz olarak kullanabilirsiniz.

- IOT Edge modülü Azure Blob Depolama, aşağıdaki cihaz yapılandırmalarını destekler:

  | İşletim sistemi | AMD64 | ARM32v7 | ARM64 |
  | ---------------- | ----- | ----- | ---- |
  | Raspbian Uzat | Hayır | Evet | Hayır |  
  | Ubuntu Server 16.04 | Evet | Hayır | Evet |
  | Ubuntu Server 18.04 | Evet | Hayır | Evet |
  | Windows 10 IoT Enterprise, derleme 17763 | Evet | Hayır | Hayır |
  | Windows Server 2019, derleme 17763 | Evet | Hayır | Hayır |
  

Bulut kaynakları:

Azure'da standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).

## <a name="devicetocloudupload-and-deviceautodelete-properties"></a>deviceToCloudUpload ve Deviceoto Delete özellikleri

**Devicetoclouduploadproperties** ve **deviceoto deleteproperties**ayarlamak için modülün istenen özelliklerini kullanın. İstenen özellikler dağıtım sırasında ayarlanabilir veya daha sonra yeniden dağıtmak zorunda kalmadan modül ikizi düzenlenerek değiştirilebilir. Değerlerinin doğru şekilde yayıldığından emin olmak için, `reported configuration` için `configurationValidation` "Module ikizi" olup olmadığını kontrol etmenizi öneririz.

### <a name="devicetoclouduploadproperties"></a>deviceToCloudUploadProperties

Bu ayarın adı`deviceToCloudUploadProperties`

| Alan | Olası Değerler | Açıklama | Ortam değişkeni |
| ----- | ----- | ---- | ---- |
| uploadOn | true, false | Varsayılan olarak `false` olarak ayarlayın. Özelliği etkinleştirmek istiyorsanız, bu alanı olarak `true`ayarlayın. | `deviceToCloudUploadProperties__uploadOn={false,true}` |
| uploadOrder | NewestFirst, OldestFirst | Verilerin Azure 'a kopyalandığı sırayı seçmenizi sağlar. Varsayılan olarak `OldestFirst` olarak ayarlayın. Sıra, blob 'un son değiştirilme zamanına göre belirlenir | `deviceToCloudUploadProperties__uploadOrder={NewestFirst,OldestFirst}` |
| cloudStorageConnectionString |  | `"DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>"`, verilerinizin karşıya yüklenmesini istediğiniz Azure Depolama hesabını belirtmenizi sağlayan bir bağlantı dizesidir. , `Azure Storage Account Name` ,`Azure Storage Account Key` ,`End point suffix`Belirtin. Verilerin karşıya yükleneceği Azure 'un uygun EndpointSuffix öğesini ekleyin; küresel Azure, kamu Azure ve Microsoft Azure Stack değişir. | `deviceToCloudUploadProperties__cloudStorageConnectionString=<connection string>` |
| storageContainersForUpload | `"<source container name1>": {"target": "<target container name>"}`,<br><br> `"<source container name1>": {"target": "%h-%d-%m-%c"}`, <br><br> `"<source container name1>": {"target": "%d-%c"}` | Azure 'a yüklemek istediğiniz kapsayıcı adlarını belirtmenize olanak tanır. Bu modül hem kaynak hem de hedef kapsayıcı adlarını belirtmenize olanak tanır. Hedef kapsayıcı adını belirtmezseniz, kapsayıcı adını otomatik olarak olarak `<IoTHubName>-<IotEdgeDeviceID>-<ModuleName>-<SourceContainerName>`atayacaktır. Hedef kapsayıcı adı için şablon dizeleri oluşturabilir, olası değerler sütununa bakabilirsiniz. <br>*% h-> IoT Hub adı (3-50 karakter). <br>*% d-> IoT Edge cihaz KIMLIĞI (1-129 karakter). <br>*% d-> Modül adı (1-64 karakter). <br>*% c-> kaynak kapsayıcısı adı (3 ile 63 karakter arasında). <br><br>Kapsayıcı adının en büyük boyutu 63 karakterdir, çünkü kapsayıcının boyutu 63 karakteri aşarsa, her bir bölümü (IoTHubName, ıotedgedeviceıd, ModuleName, SourceContainerName) 15 olarak kırpacaktır karakterle. | `deviceToCloudUploadProperties__storageContainersForUpload__<sourceName>__target: <targetName>` |
| deleteAfterUpload | true, false | Varsayılan olarak `false` olarak ayarlayın. Olarak ayarlandığında `true`, bulut depolamaya yükleme tamamlandığında verileri otomatik olarak siler | `deviceToCloudUploadProperties__deleteAfterUpload={false,true}` |


### <a name="deviceautodeleteproperties"></a>Deviceoto Deleteproperties

Bu ayarın adı`deviceAutoDeleteProperties`

| Alan | Olası Değerler | Açıklama | Ortam değişkeni |
| ----- | ----- | ---- | ---- |
| deleteOn | true, false | Varsayılan olarak `false` olarak ayarlayın. Özelliği etkinleştirmek istiyorsanız, bu alanı olarak `true`ayarlayın. | `deviceAutoDeleteProperties__deleteOn={false,true}` |
| deleteAfterMinutes | `<minutes>` | Süreyi dakika olarak belirtin. Bu değerin süresi dolarsa modül yerel depolamadan Blobları otomatik olarak siler | `deviceAutoDeleteProperties__ deleteAfterMinutes=<minutes>` |
| retainWhileUploading | true, false | Varsayılan olarak `true`, olarak ayarlanır ve deleteafsonlandırutes süresi dolarsa blob 'u bulut depolamaya yüklerken korur. Bunu olarak `false` ayarlayabilirsiniz ve deleteafsonlandırutes süresi dolduktan hemen sonra verileri siler. Not: Bu özelliğin iş için uploadOn, true olarak ayarlanmalıdır| `deviceAutoDeleteProperties__retainWhileUploading={false,true}` |

## <a name="configure-log-files"></a>Günlük dosyalarını yapılandırma

Modülünüzün günlük dosyalarını yapılandırma hakkında daha fazla bilgi için, bu [üretim en iyi uygulamalarına](https://docs.microsoft.com/azure/iot-edge/production-checklist#set-up-logs-and-diagnostics)bakın.

## <a name="connect-to-your-blob-storage-module"></a>Blob depolama modülüne bağlayın

Hesap adı ve IOT Edge Cihazınızda blob depolamaya erişmek, bir modül için yapılandırılan hesap anahtarı kullanabilirsiniz.

IOT Edge Cihazınızı herhangi bir depolama alanı için blob uç nokta olarak belirtmek için yaptığınız istekleri. Yapabilecekleriniz [bir açık depolama uç noktası için bir bağlantı dizesi oluşturma](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) IOT Edge cihaz bilgileri ve yapılandırdığınız hesabı adını kullanarak.

- IoT Edge modülündeki Azure Blob depolama alanının çalıştığı aynı cihaza dağıtılan modüller için, blob uç noktası: `http://<module name>:11002/<account name>`.
- Farklı bir cihazda çalışan modüller veya uygulamalar için ağınız için doğru uç noktayı seçmeniz gerekir. Ağ kurulumunuza bağlı olarak, dış modülünüzün veya uygulamanızdan alınan veri trafiğinin IoT Edge modülünde Azure Blob depolamayı çalıştıran cihaza ulaşabilmesi için bir uç nokta biçimi seçin. Bu senaryonun blob uç noktası şunlardan biridir:
  - `http://<device IP >:11002/<account name>`
  - `http://<IoT Edge device hostname>:11002/<account name>`
  - `http://<fully qualified domain name>:11002/<account name>`

## <a name="azure-blob-storage-quickstart-samples"></a>Azure Blob depolama hızlı başlangıç örnekleri

Azure Blob depolama belgeleri, birkaç dilde hızlı başlangıç örnek kodunu içerir. Blob uç noktasını yerel BLOB depolama modülünüzün bağlanacağı şekilde değiştirerek, IoT Edge Azure Blob depolamayı test etmek için bu örnekleri çalıştırabilirsiniz.

Aşağıdaki hızlı başlangıç örnekleri, IoT Edge tarafından da desteklenen dilleri kullanır, bu nedenle bunları BLOB depolama modülünün yanı sıra IoT Edge modüller olarak dağıtabilirsiniz:

- [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
- [Java](../storage/blobs/storage-quickstart-blobs-java.md)
- [Python](../storage/blobs/storage-quickstart-blobs-python.md)
- [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md)

## <a name="connect-to-your-local-storage-with-azure-storage-explorer"></a>Azure Depolama Gezgini ile yerel depolamaya bağlanma

[Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) , yerel depolama hesabınıza bağlanmak için kullanabilirsiniz.

1. Azure Depolama Gezgini indir ve yükle

1. Bağlantı dizesi kullanarak Azure depolama 'ya bağlanma

1. Bağlantı dizesi sağla:`DefaultEndpointsProtocol=http;BlobEndpoint=http://<host device name>:11002/<your local account name>;AccountName=<your local account name>;AccountKey=<your local account key>;`

1. Bağlanmak için adımları izleyin.

1. Yerel depolama hesabınız içinde kapsayıcı oluşturma

1. Dosya yüklemeyi blok Bloblar olarak başlatın.
   > [!NOTE]
   > Bu modül, sayfa bloblarını desteklemez.

1. Azure depolama hesaplarınızı Depolama Gezgini, aynı şekilde bağlamayı seçebilirsiniz. Bu yapılandırma, hem yerel depolama hesabınız hem de Azure Storage hesabınız için tek bir görünüm sağlar

## <a name="supported-storage-operations"></a>Desteklenen depolama işlemleri

IoT Edge BLOB depolama modülleri Azure Storage SDK 'larını kullanır ve Blok Blobu uç noktaları için Azure Storage API 'sinin 2017-04-17 sürümüyle tutarlıdır. 

Tüm Azure Blob depolama işlemleri IoT Edge Azure Blob depolama tarafından desteklenmediğinden, bu bölümde her birinin durumu listelenir.

### <a name="account"></a>Hesap

Desteklenen:

- Kapsayıcıları listeleme

Desteklenmeyen:

- Alma ve blob hizmeti özelliklerini ayarla
- Denetim öncesi blob isteği
- BLOB hizmeti istatistikleri alın
- Hesap bilgilerini alma

### <a name="containers"></a>Kapsayıcılar

Desteklenen:

- Oluşturma ve kapsayıcı silme
- Kapsayıcı özellikleri ve meta verileri alma
- Blobları listeleme
- Alma ve kapsayıcı ACL ayarlama
- Kümesi kapsayıcı meta verileri

Desteklenmeyen:

- Kira kapsayıcı

### <a name="blobs"></a>Bloblar

Desteklenen:

- Yerleştirme, alma ve blob silme
- Alma ve blob özelliklerini ayarlama
- Alma ve blob meta verileri ayarlama

Desteklenmeyen:

- Kira blob'u
- Blob anlık görüntüsü
- Kopyalama ve kopyalama blob durdurma
- Blobu silme işlemi geri
- Blob katmanı Ayarla

### <a name="block-blobs"></a>Blok blobları

Desteklenen:

- Yerleştirme bloğu
- Koy ve engelleme listesine alın

Desteklenmeyen:

- URL'den blok yerleştirme

## <a name="release-notes"></a>Sürüm Notları

Bu modül için [Docker Hub 'daki sürüm notları](https://hub.docker.com/_/microsoft-azure-blob-storage) aşağıda verilmiştir

## <a name="feedback"></a>Geri Bildirim

Bu modülün ve özelliklerinin kullanışlı ve kullanımı kolay olması için geri bildiriminiz bizim için önemlidir. Lütfen geri bildiriminizi paylaşabilir ve nasıl iyileştirebileceğimizi bize bildirin.

Bize şu adresten ulaşabilirsiniz:absiotfeedback@microsoft.com

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge Azure Blob depolamayı dağıtma](how-to-deploy-blob.md) hakkında daha fazla bilgi edinin
