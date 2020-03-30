---
title: Azure sayfa lekelerine genel bakış | Microsoft Dokümanlar
description: Azure sayfa lekelerine ve örnek komut dosyalarına sahip kullanım örnekleri de dahil olmak üzere avantajlarına genel bakış.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 05/13/2019
ms.author: tamram
ms.reviewer: wielriac
ms.subservice: blobs
ms.openlocfilehash: 060e1d01e5f078bad9852ae35d0af9142192a7b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68985626"
---
# <a name="overview-of-azure-page-blobs"></a>Azure sayfa lekelerine genel bakış

Azure Depolama üç tür blob depolama sunar: Blobs,Append Blobs ve sayfa lekeleri. Blok lekeleri bloklardan oluşur ve metin veya ikili dosyaları depolamak ve büyük dosyaları verimli bir şekilde yüklemek için idealdir. Ek blobs da bloklardan oluşur, ancak bunları yazma işlemleri için optimize edilmiştir, bu da onları günlük senaryoları için ideal hale getirir. Sayfa lekeleri toplam boyutu 8 TB'a kadar 512 baytlı sayfadan oluşur ve sık rasgele okuma/yazma işlemleri için tasarlanmıştır. Sayfa lekeleri Azure IaaS Disklerinin temelidir. Bu makalede, sayfa lekeleri özellikleri ve yararları açıklayan odaklanır.

Sayfa lekeleri, rasgele bayt aralıklarını okuma/yazma olanağı sağlayan 512 baytlık sayfalardan oluşan bir koleksiyon. Bu nedenle, sayfa lekeleri Sanal Makineler ve Veritabanları için işletim sistemi ve veri diskleri gibi dizin tabanlı ve seyrek veri yapılarını depolamak için idealdir. Örneğin, Azure SQL DB, veritabanları için temel kalıcı depolama alanı olarak sayfa lekeleri kullanır. Ayrıca, sayfa lekeleri genellikle Aralık Tabanlı güncelleştirmeleri olan dosyalar için de kullanılır.  

Azure sayfa lekelerinin temel özellikleri, REST arabirimi, temel depolamanın dayanıklılığı ve Azure'a sorunsuz geçiş yetenekleridir. Bu özellikler sonraki bölümde daha ayrıntılı olarak ele alınmıştır. Buna ek olarak, Azure sayfa lekeleri şu anda iki tür depolama alanında desteklenir: Premium Depolama ve Standart Depolama. Premium Depolama, tutarlı yüksek performans ve düşük gecikme liyatür gerektiren iş yükleri için özel olarak tasarlanmıştır ve yüksek performanslı depolama senaryoları için üstün sayfa blobları idealdir. Standart depolama hesapları, gecikmeye duyarsız iş yüklerini çalıştırmak için daha uygun maliyetlidir.

## <a name="sample-use-cases"></a>Örnek kullanım örnekleri

Azure IaaS Diskleri ile başlayan sayfa lekeleri için birkaç kullanım örneğini tartışalım. Azure sayfa lekeleri, Azure IaaS için sanal diskplatformunun belkemiğidir. Hem Azure İşletim Sistemi hem de veri diskleri, verilerin Azure Depolama platformunda kalıcı olarak kaldığı ve daha sonra maksimum performans için sanal makinelere teslim edildiği sanal diskler olarak uygulanır. Azure Diskleri Hyper-V [VHD biçiminde](https://technet.microsoft.com/library/dd979539.aspx) kalıcıdır ve Azure Depolama'da [sayfa blob'u](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs) olarak depolanır. Sayfa blob'ları, Azure IaaS VM'ler için sanal diskler kullanmanın yanı sıra, SQL verilerini depolamak için sayfa lekeleri kullanan ve veritabanı için hızlı rasgele okuma yazma işlemlerini etkinleştiren Azure SQL DB hizmeti gibi PaaS ve DBaaS senaryolarını da etkinleştirir. Başka bir örnek, ortak video düzenleme uygulamaları için paylaşılan medya erişimi için bir PaaS hizmetiniz varsa, sayfa blob'ları medyadaki rasgele konumlara hızlı erişim sağlar. Ayrıca, aynı ortamın birden çok kullanıcı tarafından hızlı ve verimli bir şekilde düzenlenmesini ve birleştirilmesine olanak tanır. 

Azure Site Kurtarma, Azure Yedekleme gibi birinci taraf Microsoft hizmetlerinin yanı sıra birçok üçüncü taraf geliştirici, page blob'un REST arabirimini kullanarak sektör lideri yenilikler icra etti. Azure'da uygulanan benzersiz senaryolardan bazıları şunlardır: 

* Uygulama yönelimli artımlı anlık görüntü yönetimi: Uygulamalar, pahalı miktarda veri çoğaltma işlemi yapmadan uygulama denetim noktalarını kaydetmek için sayfa leke anlık görüntülerinden ve REST API'lerinden yararlanabilir. Azure Depolama, tüm blob'un kopyalanmasını gerektirmeyen sayfa lekeleri için yerel anlık görüntüleri destekler. Bu genel anlık görüntü API'leri, anlık görüntüler arasındaki deltalara erişilmesini ve kopyalanmasını da sağlar.
* Uygulama ve verilerin şirket içi buluta canlı geçişi: Şirket içi verileri kopyalayın ve şirket içi VM çalışmadevam ederken doğrudan Bir Azure sayfasına yazmak için REST API'lerini kullanın. Hedef yakalandıktan sonra, bu verileri kullanarak Azure VM'ye hızlı bir şekilde geçebilirsiniz. Bu şekilde, VM'yi kullanmaya devam ederken veri geçişi arka planda oluştuğundan ve başarısız olmak için gereken kapalı kalma süresi kısa (dakika dakika) olacağı ndan, VM'lerinizi ve sanal disklerinizi şirket içinde en az kapalı kalma süresiyle şirket içinde buluta geçirebilirsiniz.
* Eşzamanlılık denetimi desteği ile birden çok okuyucu ve tek yazar gibi senaryolar sağlayan [SAS tabanlı](../common/storage-sas-overview.md) paylaşılan erişim.

## <a name="page-blob-features"></a>Sayfa blobu özellikleri

### <a name="rest-api"></a>REST API

[Sayfa lekeleri kullanarak geliştirmeye](storage-dotnet-how-to-use-blobs.md)başlamak için aşağıdaki belgeye bakın. Örnek olarak, .NET için Depolama İstemci Kitaplığı'nı kullanarak sayfa lekelerine nasıl erişilebildiğinize bakın. 

Aşağıdaki diyagramda hesap, kapsayıcılar ve sayfa lekeleri arasındaki genel ilişkileri açıklanır.

![Hesap, kapsayıcılar ve sayfa lekeleri arasındaki ilişkileri gösteren ekran görüntüsü](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure1.png)

#### <a name="creating-an-empty-page-blob-of-a-specified-size"></a>Belirli bir boyutta boş bir sayfa blob oluşturma

Bir sayfa blob oluşturmak için, ilk olarak aşağıdaki örnekte gösterildiği gibi Depolama **CredentialsAccountAndKey** nesnesi ile birlikte depolama hesabınız (şekil 1'deki*pbaccount)* için blob depolama erişmek için temel URI ile bir **CloudBlobClient** nesnesi oluştururuz. Örnek, **cloudblobcontainer** nesnesine bir başvuru oluşturmayı ve zaten yoksa kapsayıcıyı *(testvhds)* oluşturmayı gösterir. Daha sonra **CloudBlobContainer** nesnesini kullanarak, erişmek için sayfa blob adını (os4.vhd) belirterek **cloudPageBlob** nesnesine bir başvuru oluşturun. Sayfa blob oluşturmak için, [CloudPageBlob.Create](/dotnet/api/microsoft.azure.storage.blob.cloudpageblob.create)çağrı , blob oluşturmak için maksimum boyutta geçen. *BlobSize* 512 bayt bir kat olmalıdır.

```csharp
using Microsoft.Azure;
using Microsoft.Azure.Storage;
using Microsoft.Azure.Storage.Blob;

long OneGigabyteAsBytes = 1024 * 1024 * 1024;
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("testvhds");

// Create the container if it doesn't already exist.
container.CreateIfNotExists();

CloudPageBlob pageBlob = container.GetPageBlobReference("os4.vhd");
pageBlob.Create(16 * OneGigabyteAsBytes);
```

#### <a name="resizing-a-page-blob"></a>Sayfa lekelerini yeniden boyutlandırma

Oluşturmadan sonra bir sayfa lekesini yeniden boyutlandırmak için [Yeniden Boyutlandırma](/dotnet/api/microsoft.azure.storage.blob.cloudpageblob.resize) yöntemini kullanın. İstenen boyut 512 baytın bir katı olmalıdır.

```csharp
pageBlob.Resize(32 * OneGigabyteAsBytes);
```

#### <a name="writing-pages-to-a-page-blob"></a>Sayfa yıkıntın

Sayfa yazmak için [CloudPageBlob.WritePages](/dotnet/api/microsoft.azure.storage.blob.cloudpageblob.beginwritepages) yöntemini kullanın.  Bu, 4MB'ye kadar sıralı bir sayfa kümesi yazmanızı sağlar. Yazılan ofset 512 bayt lık bir sınırda başlamalı (başlangıç Offset % 512 == 0) ve 512 sınırında - 1 ile bitmelidir.  Aşağıdaki kod örneği, bir blob için **WritePages'in** nasıl arandığını gösterir:

```csharp
pageBlob.WritePages(dataStream, startingOffset); 
```

Sıralı bir sayfa kümesi için yazma isteği blob hizmetinde başarılı olur ve dayanıklılık ve esneklik için çoğaltılır, yazma taahhüt etti ve başarı istemciye geri döndürülür.  

Aşağıdaki diyagram2 ayrı yazma işlemi gösterir:

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure2.png)

1.  Uzunluk 1024 bayt0 ofset 0 başlayan bir Yazma işlemi 
2.  Uzunluk 1024 4096 ofset başlayan bir Yazma işlemi 

#### <a name="reading-pages-from-a-page-blob"></a>Sayfa lekesinden sayfaları okuma

Sayfaları okumak için, sayfa blob bayt bir dizi okumak için [CloudPageBlob.DownloadRangeToByteArray](/dotnet/api/microsoft.azure.storage.blob.icloudblob.downloadrangetobytearray) yöntemini kullanın. Bu, blob herhangi bir ofset başlayarak tam blob veya bayt aralığı indirmenize olanak sağlar. Okurken, ofset 512'nin katlarından başlamak zorunda değildir. Bir NUL sayfasından bayt okurken, hizmet sıfır bayt döndürür.

```csharp
byte[] buffer = new byte[rangeSize];
pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, pageBlobOffset, rangeSize); 
```

Aşağıdaki şekilde, 256 ofset ve 4352 aralık boyutuna sahip bir Okuma işlemi gösterilmektedir. Döndürülen veriler turuncu renkle vurgulanır. Sıfırlar NUL sayfaları için döndürülür.

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure3.png)

Seyrek nüfuslu bir blob'unuz varsa, sıfır bayt ın çıkış ücretini ödememek ve indirme gecikme süresini azaltmak için geçerli sayfa bölgelerini indirmek isteyebilirsiniz.  Hangi sayfaların verilerle yedeklediğini belirlemek için [CloudPageBlob.GetPageRanges'i](/dotnet/api/microsoft.azure.storage.blob.cloudpageblob.getpageranges)kullanın. Daha sonra döndürülen aralıkları sıralayabilir ve her aralıktaki verileri indirebilirsiniz. 

```csharp
IEnumerable<PageRange> pageRanges = pageBlob.GetPageRanges();

foreach (PageRange range in pageRanges)
{
    // Calculate the range size
    int rangeSize = (int)(range.EndOffset + 1 - range.StartOffset);

    byte[] buffer = new byte[rangeSize];

    // Read from the correct starting offset in the page blob and
    // place the data in the bufferOffset of the buffer byte array
    pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, range.StartOffset, rangeSize); 

    // Then use the buffer for the page range just read
}
```

#### <a name="leasing-a-page-blob"></a>Bir sayfa blob kiralama

Lease Blob işlemi, yazma ve silme işlemleri için bir blob'da kilit kurar ve yönetir. Bu işlem, bir sayfa blob bir anda blob yazabilirsiniz sağlamak için birden çok istemciden erişilen senaryolarda yararlıdır. Örneğin Azure Diskleri, diskin yalnızca tek bir VM tarafından yönetilmesini sağlamak için bu kiralama mekanizmasından yararlanır. Kilit süresi 15 ila 60 saniye olabilir veya sonsuz olabilir. Daha fazla bilgi için [buradaki](/rest/api/storageservices/lease-blob) belgelere bakın.

Zengin REST API'lerine ek olarak, sayfa lekeleri paylaşılan erişim, dayanıklılık ve gelişmiş güvenlik de sağlar. Bu avantajları sonraki paragraflarda daha ayrıntılı olarak ele alacağız. 

### <a name="concurrent-access"></a>Eşzamanlı erişim

Sayfa blobs REST API ve kiralama mekanizması uygulamaları birden çok istemciden sayfa blob erişmenizi sağlar. Örneğin, depolama nesnelerini birden çok kullanıcıyla paylaşan dağıtılmış bir bulut hizmeti oluşturmanız gerektiğini varsayalım. Birkaç kullanıcıya büyük bir resim koleksiyonu sunan bir web uygulaması olabilir. Bunu uygulamak için bir seçenek ekli diskler ile bir VM kullanmaktır. Bunun dezavantajları arasında( i) bir diskin yalnızca tek bir VM'ye bağlanabileceği kısıtlama, ölçeklenebilirliği, esnekliği ve artan riskleri sınırlandırmaktadır. VM'de veya VM'de çalışan hizmette bir sorun varsa, kira süresi dolana veya bozulana kadar görüntüye erişilemez; ve (ii) IaaS VM'ye sahip olmanın ek maliyeti. 

Alternatif bir seçenek, sayfa lekelerini doğrudan Azure Depolama REST API'leri üzerinden kullanmaktır. Bu seçenek, pahalı IaaS VM'lere olan gereksinimi ortadan kaldırır, birden çok istemciden doğrudan erişim esnekliği sunar, disk ekleme/ayırma gereksinimini ortadan kaldırarak klasik dağıtım modelini basitleştirir ve VM'deki sorun riskini ortadan kaldırır. Ayrıca, rasgele okuma/yazma işlemleri için bir disk le aynı performans düzeyini sağlar

### <a name="durability-and-high-availability"></a>Dayanıklılık ve yüksek kullanılabilirlik

Hem Standart hem de birinci sınıf depolama, dayanıklılık ve yüksek kullanılabilirlik sağlamak için sayfa blob verilerinin her zaman çoğaltıldığı dayanıklı depolama alanıdır. Azure Depolama Artıklığı hakkında daha fazla bilgi için bu [belgelere](../common/storage-redundancy.md)bakın. Azure, IaaS diskleri ve sayfa lekeleri için sürekli olarak kurumsal kalitede dayanıklılık sunarken, sektör lideri yüzde sıfır [Yıllık Hata Oranı](https://en.wikipedia.org/wiki/Annualized_failure_rate)ile .

### <a name="seamless-migration-to-azure"></a>Azure'a sorunsuz geçiş

Azure, kendi özelleştirilmiş yedekleme çözümlerini uygulamak isteyen müşteriler ve geliştiriciler için yalnızca deltaları tutan artımlı anlık görüntüler de sunar. Bu özellik, yedekleme maliyetini büyük ölçüde düşüren ilk tam kopyanın maliyetini önler. Diferansiyel verileri verimli bir şekilde okuma ve kopyalama olanağının yanı sıra, geliştiricilerden daha fazla yenilik sağlayan ve Azure'da sınıfının en iyisi yedekleme ve olağanüstü durum kurtarma (DR) deneyimine yol açan bir başka güçlü özelliktir. Sayfa Aralıkları API'sini ve DR için artımlı verileri kolayca kopyalamak için kullanabileceğiniz [Artımlı Kopya Blob](/rest/api/storageservices/incremental-copy-blob) API'sini kullanarak [Azure'daki](/rest/api/storageservices/snapshot-blob) Sanal [Am'larınız](/rest/api/storageservices/get-page-ranges) için Kendi yedekleme veya DR çözümünüzü Ayarlayabilirsiniz. 

Ayrıca, birçok işletmenin şirket içi veri merkezlerinde çalışan kritik iş yükleri vardır. İş yükünü buluta geçirmek için en önemli endişelerden biri, verilerin kopyalanması için gereken kesinti miktarı ve geçişten sonra öngörülemeyen sorunlar riski olacaktır. Çoğu durumda, kapalı kalma süresi buluta geçiş için bir showstopper olabilir. Azure, REST API sayfasını kullanarak, bulut geçişini kritik iş yüklerinde en az kesintiye yol ederek bu sorunu giderir. 

Anlık görüntü nün nasıl alınıp anlık görüntüden bir sayfa blob'una nasıl geri yüklenilenlerle ilgili örnekler için, lütfen [artımlı anlık görüntü](../../virtual-machines/windows/incremental-snapshots.md) makalesini kullanarak bir yedekleme işlemi ne rendeleyin.
