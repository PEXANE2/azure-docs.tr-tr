---
title: Azure IoT Hub aygıt kimliklerinin alma/dışa aktarılması | Microsoft Dokümanlar
description: Aygıt kimliklerini içe aktarmak ve dışa aktarmak için kimlik kayıt defterine karşı toplu işlemler yürütmek için Azure IoT hizmeti SDK nasıl kullanılır? Alma işlemleri, aygıt kimliklerini toplu olarak oluşturmanıza, güncelleştirmenize ve silmenize olanak tanır.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: robinsh
ms.openlocfilehash: 2a0394e6e7c17e0a4954bbdddb1d5b2811959746
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371588"
---
# <a name="import-and-export-iot-hub-device-identities-in-bulk"></a>IoT Hub cihaz kimliklerinizi toplu olarak içeri ve dışarı aktarma

Her IoT hub'ı, hizmette aygıt başına kaynakları oluşturmak için kullanabileceğiniz bir kimlik kaydına sahiptir. Kimlik kayıt defteri, aygıta bakan uç noktalara erişimi denetlemenize de olanak tanır. Bu makalede, aygıt kimliklerinin bir kimlik kayıt defterine toplu olarak nasıl ve bir kimlik kayıt defterinden nasıl içe aktarılacagı ve dışa aktarılabildiğini açıklanmaktadır. C#'da çalışan bir örneği görmek ve hub'ı farklı bir bölgeye klonlarken bu özelliği nasıl kullanabileceğinizi öğrenmek [için, bir IoT Hub'ı nasıl klonladığınıza](iot-hub-how-to-clone.md)bakın.

> [!NOTE]
> IoT Hub yakın zamanda sınırlı sayıda bölgeye sanal ağ desteği ekledi. Bu özellik, alma ve dışa aktarma işlemlerini güvence altına almak ve kimlik doğrulama için anahtarları geçme gereksinimini ortadan kaldırır.  Başlangıçta, sanal ağ desteği sadece bu bölgelerde kullanılabilir: *WestUS2*, *EastUS*, ve *SouthCentralUS*. Sanal ağ desteği ve uygulama api çağrıları hakkında daha fazla bilgi edinmek [için sanal ağlar için IoT Hub Desteği'ne](virtual-network-support.md)bakın.

Alma ve dışa aktarma işlemleri, bir IoT hub'ına karşı toplu hizmet işlemleri yürütmenize olanak tanıyan *İşler* bağlamında gerçekleşir.

**RegistryManager** sınıfı, **İş** çerçevesini kullanan **ExportDevicesAsync** ve **ImportDevicesAsync** yöntemlerini içerir. Bu yöntemler, bir IoT hub kimlik kayıt defterinin tamamını dışa aktarmanızı, içe aktarmanızı ve eşitlemenizi sağlar.

Bu konu, bir IoT merkezinin kimlik kaydına ve aygıtların toplu içe aktarımlarını ve dışa aktarımlarını gerçekleştirmek için **RegistryManager** sınıf ını ve **İş** sistemini kullanır. İnsan müdahalesine gerek kalmadan bir veya daha fazla IoT hub'ına sıfır dokunuş, tam zamanında sağlama sağlamak için Azure IoT Hub Aygıt Sağlama Hizmetini de kullanabilirsiniz. Daha fazla bilgi edinmek [için, hizmet sağlama belgelerine](/azure/iot-dps)bakın.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

## <a name="what-are-jobs"></a>İş nedir?

Kimlik kayıt işlemleri, çalışma olduğunda **İş** sistemini kullanır:

* Standart çalışma zamanı işlemleriyle karşılaştırıldığında potansiyel olarak uzun bir yürütme süresine sahiptir.

* Kullanıcıya büyük miktarda veri verir.

İşlemin sonucunu bekleyen veya engelleyen tek bir API çağrısı yerine, işlem eşzamanlı olarak bu IoT hub'ı için bir **İş** oluşturur. İşlem daha sonra hemen bir **JobProperties** nesnesi döndürür.

Aşağıdaki C# kodu snippet nasıl bir dışa aktarma işi oluşturmak için gösterir:

```csharp
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await 
  registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> C# kodunuzdaki **RegistryManager** sınıfını kullanmak için projenize **Microsoft.Azure.Devices** NuGet paketini ekleyin. **RegistryManager** sınıfı **Microsoft.Azure.Devices** ad alanındadır.

Döndürülen **JobProperties** meta verilerini kullanarak **İş** durumunu sorgulamak için **RegistryManager** sınıfını kullanabilirsiniz. **RegistryManager** sınıfının bir örneğini oluşturmak için **CreateFromConnectionString** yöntemini kullanın.

```csharp
RegistryManager registryManager =
  RegistryManager.CreateFromConnectionString("{your IoT Hub connection string}");
```

Azure portalında IoT hub'ınız için bağlantı dizesini bulmak için:

- IoT Hub'ınıza gidin.

- **Paylaşılan erişim ilkelerini**seçin.

- İhtiyacınız olan izinleri dikkate alarak bir ilke seçin.

- Ekranın sağ tarafındaki panelden bağlantı ipini kopyalayın.

Aşağıdaki C# kodu snippet, işin yürütmeyi tamamlayıp tamamlamadığını görmek için her beş saniyede bir nasıl yoklanır:

```csharp
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

> [!NOTE]
> Depolama hesabınızda IoT Hub'ın bağlantısını kısıtlayan güvenlik duvarı yapılandırmaları varsa, [Microsoft'un güvendiği birinci taraf özel durumu](./virtual-network-support.md#egress-connectivity-to-storage-account-endpoints-for-routing) kullanmayı düşünün (yönetilen hizmet kimliğine sahip IoT hub'ları için belirli bölgelerde kullanılabilir).


## <a name="device-importexport-job-limits"></a>Cihaz alma/dışa aktarma iş sınırları

Tüm IoT Hub katmanları için aynı anda yalnızca 1 etkin aygıt alma veya dışa aktarma işine izin verilir. IoT Hub'ın iş işlemleri için de sınırları vardır. Daha fazla bilgi için [Bkz. Başvuru - IoT Hub kotaları ve azaltma.](iot-hub-devguide-quotas-throttling.md)

## <a name="export-devices"></a>Aygıtları dışa aktarma

IoT hub kimlik kayıt defterinin tamamını paylaşılan erişim imzası (SAS) kullanarak Azure Depolama blob kapsayıcısına aktarmak için **ExportDevicesAsync** yöntemini kullanın. Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz: [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni](../storage/common/storage-sas-overview.md)ver.

Bu yöntem, kontrol ettiğiniz bir blob kapsayıcısında aygıt bilgilerinizin güvenilir yedeklemelerini oluşturmanıza olanak tanır.

**ExportDevicesAsync** yöntemi iki parametre gerektirir:

* Bir blob konteyner URI içeren bir *dize.* Bu URI, kapsayıcıya yazma izni veren bir SAS belirteci içermelidir. İş, seri leştirilmiş dışa aktarma aygıtı verilerini depolamak için bu kapsayıcıda bir blok blob oluşturur. SAS belirteci şu izinleri içermelidir:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read 
     | SharedAccessBlobPermissions.Delete
   ```

* Kimlik doğrulama anahtarlarını dışlamak isteyip istemediğinizi belirten bir *boolean,* dışa aktarma verilerinizden. **Yanlışsa,** kimlik doğrulama anahtarları dışa aktarma çıktısı dahil edilir. Aksi takdirde, anahtarlar **null**olarak dışa aktarılır.

Aşağıdaki C# kodu snippet, dışa aktarma verilerinde aygıt kimlik doğrulama anahtarlarını içeren bir dışa aktarma işinin nasıl başlatılabildiğini ve ardından tamamlanmak üzere nasıl yoklanır:

```csharp
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = 
  await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

İş adı **devices.txt**ile bir blok blob olarak sağlanan blob konteyner çıktısını depolar. Çıktı verileri, satır başına bir aygıtla JSON seri leştirilmiş aygıt verilerinden oluşur.

Aşağıdaki örnek, çıktı verilerini gösterir:

```json
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Bir aygıtın ikiz verileri varsa, çift veri aygıt verileriyle birlikte dışa aktarılır. Aşağıdaki örnekte bu biçimi gösterilmektedir. Sonuna kadar "twinETag" satırından tüm veriler ikiz veridir.

```json
{
   "id":"export-6d84f075-0",
   "eTag":"MQ==",
   "status":"enabled",
   "statusReason":"firstUpdate",
   "authentication":null,
   "twinETag":"AAAAAAAAAAI=",
   "tags":{
      "Location":"LivingRoom"
   },
   "properties":{
      "desired":{
         "Thermostat":{
            "Temperature":75.1,
            "Unit":"F"
         },
         "$metadata":{
            "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
            "$lastUpdatedVersion":2,
            "Thermostat":{
               "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
               "$lastUpdatedVersion":2,
               "Temperature":{
                  "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
                  "$lastUpdatedVersion":2
               },
               "Unit":{
                  "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
                  "$lastUpdatedVersion":2
               }
            }
         },
         "$version":2
      },
      "reported":{
         "$metadata":{
            "$lastUpdated":"2017-03-09T18:30:51.1309437Z"
         },
         "$version":1
      }
   }
}
```

Bu verilere kod halinde erişmeniz gerekiyorsa, **ExportImportDevice** sınıfını kullanarak bu verileri kolayca deserialize edebilirsiniz. Aşağıdaki C# kodu snippet, daha önce bir blok blob'una dışa aktarılan aygıt bilgilerinin nasıl okunduğunu gösterir:

```csharp
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), null, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

## <a name="import-devices"></a>Aygıtları içe aktarma

**RegistryManager** sınıfındaki **ImportDevicesAsync** yöntemi, bir IoT hub kimlik kayıt defterinde toplu alma ve eşitleme işlemleri gerçekleştirmenizi sağlar. **ExportDevicesAsync** yöntemi **gibi, ImportDevicesAsync** yöntemi **İş** çerçevesini kullanır.

**ImportDevicesAsync** yöntemini kullanarak dikkatli olmaya özen gösterir, çünkü kimlik defterinizde yeni aygıtlar sağlamanın yanı sıra varolan aygıtları güncelleyebilir ve silebilir.

> [!WARNING]
> Bir alma işlemi geri alınamaz. Kimlik kaydınızda toplu değişiklikler yapmadan önce **ExportDevicesAsync** yöntemini kullanarak her zaman varolan verilerinizi başka bir blob kapsayıcısına yedekleyin.

**ImportDevicesAsync** yöntemi iki parametre alır:

* İş *string* için *giriş* olarak kullanmak üzere bir Azure [Depolama](../storage/index.yml) blob kapsayıcısının URI'sini içeren dize. Bu URI, kapsayıcıya okuma erişimi sağlayan bir SAS belirteci içermelidir. Bu kapsayıcı, kimlik kayıt defterinize içe aktarılabilmek için serileştirilmiş aygıt verilerini içeren **devices.txt** adını içeren bir blob içermelidir. Alma verileri, **ExportImportDevice** işinin **devices.txt** blob oluştururken kullandığı aynı JSON biçimindeaygıt bilgilerini içermelidir. SAS belirteci şu izinleri içermelidir:

   ```csharp
   SharedAccessBlobPermissions.Read
   ```

* İş *string* *çıktısı* olarak kullanmak üzere bir [Azure Depolama](https://azure.microsoft.com/documentation/services/storage/) blob kapsayıcısının URI'sini içeren dize. İş, tamamlanan alma İşi'nden gelen hata bilgilerini depolamak **Job**için bu kapsayıcıda bir blok blob oluşturur. SAS belirteci şu izinleri içermelidir:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read 
     | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> İki parametre aynı blob kapsayıcısını işaret edebilir. Ayrı parametreler, çıktı kapsayıcısı ek izinler gerektirdiğinden verileriniz üzerinde daha fazla denetim sağlar.

Aşağıdaki C# kodu snippet bir alma işi başlatmak için nasıl gösterir:

```csharp
JobProperties importJob = 
   await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

Bu yöntem, aygıt ikizi için veri almak için de kullanılabilir. Veri girişinin biçimi **ExportDevicesAsync** bölümünde gösterilen biçimle aynıdır. Bu şekilde, dışa aktarılan verileri yeniden içe aktarabilirsiniz. **$metadata** isteğe bağlıdır.

## <a name="import-behavior"></a>Alma davranışı

Kimlik defterinizde aşağıdaki toplu işlemleri gerçekleştirmek için **ImportDevicesAsync** yöntemini kullanabilirsiniz:

* Yeni cihazların toplu kaydı
* Varolan cihazların toplu silme
* Toplu durum değişiklikleri (cihazları etkinleştirme veya devre dışı)
* Yeni aygıt kimlik doğrulama anahtarlarının toplu olarak atanması
* Aygıt kimlik doğrulama anahtarlarının toplu otomatik yenilenmesi
* İkiz verilerin toplu güncelleştirmesi

Tek bir **ImportDevicesAsync** araması içinde önceki işlemlerin herhangi bir birleşimini gerçekleştirebilirsiniz. Örneğin, yeni aygıtları kaydedebilir ve varolan aygıtları aynı anda silebilir veya güncelleyebilirsiniz. **ExportDevicesAsync** yöntemiyle birlikte kullanıldığında, tüm aygıtlarınızı bir IoT hub'ından diğerine tamamen geçirebilirsiniz.

Alma dosyası ikiz meta veri içeriyorsa, bu meta veri varolan ikiz meta verilerin üzerine yazar. Alma dosyası ikiz meta verileri içermiyorsa, geçerli saat kullanılarak yalnızca `lastUpdateTime` meta veriler güncelleştirilir.

Aygıt başına alma işlemini denetlemek için her aygıt için içe aktarma serileştirme verilerinde isteğe bağlı **içe aktarmaModu** özelliğini kullanın. **ImportMode** özelliği aşağıdaki seçeneklere sahiptir:

| importMode | Açıklama |
| --- | --- |
| **createOrUpdate** |Belirtilen **kimlikle**bir aygıt yoksa, yeni kaydedilir. <br/>Aygıt zaten varsa, varolan bilgiler **ETag** değerine bakılmaksızın sağlanan giriş verileriyle birlikte üzerine yazılır. <br> Kullanıcı isteğe bağlı olarak aygıt verileriyle birlikte ikiz verileri belirtebilir. İkizin eetiketi, belirtilirse, aygıtın etag'ından bağımsız olarak işlenir. Varolan ikizin eetiketiyle bir uyumsuzluk varsa, günlük dosyasına bir hata yazılır. |
| **oluşturmaya** |Belirtilen **kimlikle**bir aygıt yoksa, yeni kaydedilir. <br/>Aygıt zaten varsa, günlük dosyasına bir hata yazılır. <br> Kullanıcı isteğe bağlı olarak aygıt verileriyle birlikte ikiz verileri belirtebilir. İkizin eetiketi, belirtilirse, aygıtın etag'ından bağımsız olarak işlenir. Varolan ikizin eetiketiyle bir uyumsuzluk varsa, günlük dosyasına bir hata yazılır. |
| **update** |Bir aygıt zaten belirtilen **kimlikle**varsa, varolan bilgiler **ETag** değerine bakılmaksızın sağlanan giriş verileriyle birlikte üzerine yazılır. <br/>Aygıt yoksa, günlük dosyasına bir hata yazılır. |
| **güncellemeIfMatchETag** |Bir aygıt zaten belirtilen **kimlikle**varsa, varolan bilgiler yalnızca bir **ETag** eşleşmesi varsa sağlanan giriş verileriyle birlikte üzerine yazılır. <br/>Aygıt yoksa, günlük dosyasına bir hata yazılır. <br/>**ETag** uyuşmazlığı varsa, günlük dosyasına bir hata yazılır. |
| **createOrUpdateIfMatchETag** |Belirtilen **kimlikle**bir aygıt yoksa, yeni kaydedilir. <br/>Aygıt zaten varsa, varolan bilgiler yalnızca bir **ETag** eşleşmesi varsa sağlanan giriş verileriyle birlikte üzerine yazılır. <br/>**ETag** uyuşmazlığı varsa, günlük dosyasına bir hata yazılır. <br> Kullanıcı isteğe bağlı olarak aygıt verileriyle birlikte ikiz verileri belirtebilir. İkizin eetiketi, belirtilirse, aygıtın etag'ından bağımsız olarak işlenir. Varolan ikizin eetiketiyle bir uyumsuzluk varsa, günlük dosyasına bir hata yazılır. |
| **Silmek** |Bir aygıt belirtilen **kimlikle**zaten varsa, **ETag** değerine bakılmaksızın silinir. <br/>Aygıt yoksa, günlük dosyasına bir hata yazılır. |
| **deleteIfMatchETag** |Bir aygıt zaten belirtilen **kimlikle**varsa, yalnızca bir **ETag** eşleşmesi varsa silinir. Aygıt yoksa, günlük dosyasına bir hata yazılır. <br/>ETag uyuşmazlığı varsa, günlük dosyasına bir hata yazılır. |

> [!NOTE]
> Serileştirme verileri bir aygıt için **importMode** bayrağını açıkça tanımlamazsa, alma işlemi sırasında **OrUpdate oluşturmayı** varsayılan olarak belirler.

## <a name="import-devices-example--bulk-device-provisioning"></a>Aygıtları içe aktarma örneği – toplu cihaz sağlama

Aşağıdaki C# kodu örneği, aşağıdakilerden hangisinin birden çok aygıt kimliği oluşturacağı açıklanır:

* Kimlik doğrulama anahtarlarını ekleyin.
* Bu aygıt bilgilerini bir blok blob'una yazın.
* Aygıtları kimlik kaydına aktarın.

```csharp
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
  // Create a new ExportImportDevice
  // CryptoKeyGenerator is in the Microsoft.Azure.Devices.Common namespace
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to the list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write the list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the blob to add new devices
// Log information related to the job is written to the same container
// This normally takes 1 minute per 100 devices
JobProperties importJob =
   await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>Aygıtları alma örneği – toplu silme

Aşağıdaki kod örneği, önceki kod örneğini kullanarak eklediğiniz aygıtları nasıl sildiğinizi gösterir:

```csharp
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="get-the-container-sas-uri"></a>Konteyner SAS URI alın

Aşağıdaki kod örneği, bir blob kapsayıcısı için okuma, yazma ve silme izinleriyle bir [SAS URI'nin](../storage/common/storage-dotnet-shared-access-signature-part-1.md) nasıl oluşturacağınızı gösterir:

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir IoT hub'ında kimlik kaydına karşı toplu işlemleri nasıl gerçekleştireceklerini öğrendiniz. Aygıtların bir hub'dan diğerine nasıl taşınır gibi bu işlemlerin çoğu, [IoT Hub'ı Nasıl Klonlanır'ın IoT hub'ına kayıtlı Yönetim aygıtlarında](iot-hub-how-to-clone.md#managing-the-devices-registered-to-the-iot-hub)kullanılır. 

Klonlama makalesinde, bu sayfadaki IoT C# örneklerinde bulunan ve proje ImportExportDevicesSample olan [Azure IoT Örnekleri](https://azure.microsoft.com/resources/samples/azure-iot-samples-csharp/)ile ilişkili bir çalışma örneği bulunur. Örneği indirebilir ve deneyebilirsiniz; [Bir IoT Hub](iot-hub-how-to-clone.md) makalesinde nasıl klonlanır yönergeler vardır.

Azure IoT Hub'ını yönetme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere göz atın:

* [IoT Hub ölçümleri](iot-hub-metrics.md)
* [IoT Hub günlükleri](iot-hub-monitor-resource-health.md)

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [IoT Hub geliştirici kılavuzu](iot-hub-devguide.md)
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/tutorial-simulate-device-linux.md)

Sıfır dokunmayı, tam zamanında sağlamayı etkinleştirmek için IoT Hub Aygıt Sağlama Hizmetini kullanmak için bkz: 

* [Azure IoT Hub Cihazı Sağlama Hizmeti](/azure/iot-dps)
