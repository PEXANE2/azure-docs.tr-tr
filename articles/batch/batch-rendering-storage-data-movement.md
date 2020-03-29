---
title: Oluşturma için depolama ve veri hareketi - Azure Toplu İş
description: Varlık ve çıktı dosyası iş yüklerini işlemek için çeşitli depolama ve veri hareketi seçenekleri hakkında bilgi edinin.
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 1076fc152ddf2c3a2d4f2346262ca90215d68ddf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75390394"
---
# <a name="storage-and-data-movement-options-for-rendering-asset-and-output-files"></a>Varlık ve çıktı dosyalarını işlemek için depolama ve veri hareketi seçenekleri

Havuz VM'lerinde görüntü uygulamaları için sahne ve varlık dosyalarını kullanılabilir hale getirmek için birden çok seçenek vardır:

* [Azure blob depolama:](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)
  * Sahne ve varlık dosyaları yerel bir dosya sisteminden blob depolamayüklenir. Uygulama bir görev tarafından çalıştırıldığında, gerekli dosyalar blob depolamadan VM'ye kopyalanır, böylece işleme uygulaması tarafından erişilebilir. Çıktı dosyaları vm diske render uygulaması tarafından yazılır ve daha sonra blob depolama kopyalanır.  Gerekirse, çıktı dosyaları blob depolamadan yerel bir dosya sistemine indirilebilir.
  * Azure blob depolama küçük projeler için basit ve uygun maliyetli bir seçenektir.  Tüm varlık dosyaları her havuz VM gerekli olduğundan, o zaman bir kez sayısı ve varlık dosyalarının boyutu artar zaman dosya aktarımları mümkün olduğunca verimli olduğundan emin olmak için alınması gereken bakım gerekir.  
* [Blobfuse](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux)kullanarak bir dosya sistemi olarak Azure depolama:
  * Linux VM'leri için, blobfuse sanal dosya sistemi sürücüsü kullanıldığında bir depolama hesabı açığa çıkabilir ve dosya sistemi olarak kullanılabilir.
  * Bu seçenek, dosya sistemi için vm'ler gerekmeden, ayrıca VM'lerde blobfuse önbelleğe alınması birden çok iş ve görev için aynı dosyaların tekrarlanan karşıdan yüklerini önleyebildiğiniz için, çok uygun maliyetli olması avantajına sahiptir.  Dosyalar yalnızca blobs ve standart API'ler ve azcopy gibi araçlar, şirket içi dosya sistemi ve Azure depolama arasında dosya kopyalamak için kullanılabilir gibi veri hareketi de basittir.
* Dosya sistemi veya dosya paylaşımı:
  * VM işletim sistemine ve performans/ölçek gereksinimlerine bağlı olarak, seçenekler arasında [Azure Dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction), NFS için ekli disklere sahip bir VM kullanma, GlusterFS gibi dağıtılmış bir dosya sistemi için ekli disklerle birden çok VM kullanma veya üçüncü taraf teklifi kullanma yer almaktadır.
  * [Avere Systems](https://www.averesystems.com/) artık Microsoft'un bir parçasıdır ve yakın gelecekte büyük ölçekli, yüksek performanslı görüntüleme için ideal çözümlere sahip olacaktır.  Avere çözümü, blob depolama yla veya şirket içi NAS aygıtlarıyla birlikte çalışan Azure tabanlı bir NFS veya Kobİ önbelleği oluşturulmasını sağlar.
  * Bir dosya sistemi ile, dosyalar doğrudan dosya sistemine okunabilir veya yazılabilir veya dosya sistemi ile havuz VM'leri arasında kopyalanabilir.
  * Paylaşılan dosya sistemi, projeler ve işler arasında paylaşılan çok sayıda varlığın kullanılmasına olanak sağlar ve görevleri yalnızca gerekene erişir.

## <a name="using-azure-blob-storage"></a>Azure blob depolama yı kullanma

Bir blob depolama hesabı veya genel amaçlı v2 depolama hesabı kullanılmalıdır.  Bu iki depolama hesabı [türleri, bu blog gönderisinde](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/)ayrıntılı olarak belirtildiği gibi, genel amaçlı v1 depolama hesabına kıyasla önemli ölçüde daha yüksek limitlerle yapılandırılabilir.  Yapılandırıldığında, daha yüksek sınırlar, özellikle depolama hesabına erişen çok sayıda havuz VM'si olduğunda çok daha iyi performans ve ölçeklenebilirlik sağlar.

### <a name="copying-files-between-client-and-blob-storage"></a>İstemci ve blob depolama arasında dosya kopyalama

Dosyaları Azure depolamaya kopyalamak için depolama blob API, [Azure Depolama Veri Hareketi Kitaplığı,](https://github.com/Azure/azure-storage-net-data-movement) [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) veya [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux)için azcopy komut satırı aracı, Azure [Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)ve Azure Toplu [Gezgini](https://azure.github.io/BatchExplorer/)dahil olmak üzere çeşitli mekanizmalar kullanılabilir.

Örneğin, azcopy kullanılarak, bir klasördeki tüm varlıklar aşağıdaki gibi aktarılabilir:


`azcopy /source:. /dest:https://account.blob.core.windows.net/rendering/project /destsas:"?st=2018-03-30T16%3A26%3A00Z&se=2020-03-31T16%3A26%3A00Z&sp=rwdl&sv=2017-04-17&sr=c&sig=sig" /Y`

Yalnızca değiştirilmiş dosyaları kopyalamak için /XO parametresi kullanılabilir:

`azcopy /source:. /dest:https://account.blob.core.windows.net/rendering/project /destsas:"?st=2018-03-30T16%3A26%3A00Z&se=2020-03-31T16%3A26%3A00Z&sp=rwdl&sv=2017-04-17&sr=c&sig=sig" /XO /Y`

### <a name="copying-input-asset-files-from-blob-storage-to-batch-pool-vms"></a>Giriş varlık dosyalarını blob depolamadan Toplu havuz VM'lerine kopyalama

İş varlıklarının boyutuna göre belirlenen en iyi yaklaşımla dosyaları kopyalamak için birkaç farklı yaklaşım vardır.
En basit yaklaşım, her iş için havuz VM'lerine tüm varlık dosyalarını kopyalamaktır:

* Bir işe özgü dosyalar varsa, ancak bir işin tüm görevleri için gerekli yse, tüm dosyaları kopyalamak için bir [iş hazırlama görevi](https://docs.microsoft.com/rest/api/batchservice/job/add#jobpreparationtask) belirtilebilir.  İlk iş görevi VM'de yürütüldüğünde ancak sonraki iş görevleri için yeniden çalıştırıldığında iş hazırlama görevi bir kez çalıştırılır.
* İş tamamlandıktan sonra iş başına dosyaları kaldırmak için bir [iş serbest bırakma görevi](https://docs.microsoft.com/rest/api/batchservice/job/add#jobreleasetask) belirtilmelidir; bu, VM diskinin tüm iş varlık dosyaları tarafından doldurulmasını önler.
* Her iş için varlıklarda yalnızca artımlı değişiklikler le aynı varlıkları kullanan birden çok iş olduğunda, yalnızca bir alt küme güncelleştirmiş olsa bile tüm varlık dosyaları yine de kopyalanır.  Çok sayıda büyük varlık dosyası olduğunda bu verimsiz olacaktır.

Varlık dosyaları işler arasında yeniden kullanıldığında, işler arasında yalnızca artımlı değişiklikler le, daha verimli ama biraz daha ilgili bir yaklaşım, varlıkları VM'deki paylaşılan klasörde depolamak ve değiştirilen dosyaları eşitlemektir.

* İş hazırlama görevi, /XO parametresi ile AZ_BATCH_NODE_SHARED_DIR ortam değişkeni tarafından belirtilen VM paylaşılan klasörüne azcopy kullanarak kopyayı gerçekleştirir.  Bu yalnızca değiştirilen dosyaları her VM'ye kopyalar.
* Onlar havuz VMs geçici sürücü sığacak sağlamak için düşünce tüm varlıkların boyutuna verilmelidir.

Azure Toplu İşlem, bir depolama hesabı ve Toplu Toplu Havuz VM'leri arasındaki dosyaları kopyalamak için yerleşik desteğine sahiptir.  Görev [kaynağı dosyaları](https://docs.microsoft.com/rest/api/batchservice/job/add#resourcefile) dosyaları depolamadan havuz VM'lerine kopyalar ve iş hazırlama görevi için belirtilebilir.  Ne yazık ki, yüzlerce dosya olduğunda bir sınıra ve görevlere basmak mümkündür.  Çok sayıda varlık olduğunda, joker karakterleri kullanabilen ve sınırı olmayan iş hazırlama görevinde azcopy komut satırının kullanılması önerilir.

### <a name="copying-output-files-to-blob-storage-from-batch-pool-vms"></a>Çıktı dosyalarını Toplu havuz VM'lerden blob depolamasına kopyalama

[Çıktı dosyaları](https://docs.microsoft.com/rest/api/batchservice/task/add#outputfile) bir havuz VM depolama kopyalama dosyaları kullanılabilir.  Görev tamamlandıktan sonra vm'den belirli bir depolama hesabına bir veya daha fazla dosya kopyalanabilir.  İşlenen çıktı kopyalanmalıdır, ancak günlük dosyalarını depolamak da istenebilir.

## <a name="using-a-blobfuse-virtual-file-system-for-linux-vm-pools"></a>Linux VM havuzları için blobfuse sanal dosya sistemi kullanma

Blobfuse, Azure Blob Depolama için sanal bir dosya sistemi sürücüsüdür ve Linux dosya sistemi aracılığıyla bir Depolama hesabında blob olarak depolanan dosyalara erişmenizi sağlar.

Havuz düğümleri başlatıldığında dosya sistemini monte edebilir veya montaj bir iş hazırlama görevinin bir parçası olarak gerçekleşebilir – yalnızca bir işteki ilk görev düğüm üzerinde çalıştığında çalıştırılan bir görev.  Blobfuse, hem ramdisk hem de VMs yerel SSD'den dosyaların önbelleğe alınması için kullanılabilir ve bu da düğümdeki birden çok görevin aynı dosyalardan bazılarına erişirse performansı önemli ölçüde artırır.

[Örnek şablonlar,](https://github.com/Azure/BatchExplorer-data/tree/master/ncj/vray/render-linux-with-blobfuse-mount) blobfuse dosya sistemi kullanarak bağımsız V-Ray renderçalıştırmak için kullanılabilir ve diğer uygulamalar için şablonlar için temel olarak kullanılabilir.

### <a name="accessing-files"></a>Dosyalara erişim

İş görevleri, monte edilmiş dosya sistemini kullanarak giriş dosyaları ve çıktı dosyaları için yolları belirtir.

### <a name="copying-input-asset-files-from-blob-storage-to-batch-pool-vms"></a>Giriş varlık dosyalarını blob depolamadan Toplu havuz VM'lerine kopyalama

Dosyalar Azure Depolama'da yalnızca blobs olduğundan, daha sonra standart blob API'leri, araçları ve Kullanıcı Araları, şirket içi dosya sistemi ile blob depolama arasında dosyaları kopyalamak için kullanılabilir; örneğin, azcopy, Depolama Explorer, Toplu Explorer, vb.

## <a name="using-azure-files-with-windows-vms"></a>Windows VM'lerle Azure Dosyalarını Kullanma

[Azure Files,](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) BULUT'ta Kobİ protokolü aracılığıyla erişilebilen tam yönetilen dosya paylaşımları sunar.  Azure Dosyaları Azure blob depolama dayanmaktadır; [maliyet-etkindir](https://azure.microsoft.com/pricing/details/storage/files/) ve veri çoğaltma ile başka bir bölgeye yapılandırılabilir, bu nedenle küresel olarak gereksizdir.  [Tahmin](https://docs.microsoft.com/azure/storage/files/storage-files-scale-targets#azure-files-scale-targets) havuzu boyutu ve varlık dosyalarının sayısı göz önüne alındığında Azure Dosyalarının kullanıp kullanılmaması gerektiğini belirlemek için ölçek hedefleri gözden geçirilmelidir.

Azure Dosyası paylaşımının nasıl monte edilebildiğini kapsayan bir [blog gönderisi](https://blogs.msdn.microsoft.com/windowsazurestorage/2014/05/26/persisting-connections-to-microsoft-azure-files/) ve [belgeler](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) vardır.

### <a name="mounting-an-azure-files-share"></a>Azure Dosyaları paylaşımını ekleme

Toplu İşlem'de kullanmak için, görevler arasındaki bağlantıyı sürdürmek mümkün olmadığı için, bir görev her çalıştırılında bir montaj işleminin gerçekleştirilmesi gerekir.  Bunu yapmanın en kolay yolu, havuz yapılandırmasındaki başlangıç görevini kullanarak kimlik bilgilerini sürdürmek için cmdkey kullanmak ve ardından her görevden önce payı monte etmektir.

Bir havuz şablonunda cmdkey örneği kullanımı (JSON dosyasında kullanılmak üzere kaçtı) – cmdkey çağrısını net kullanım çağrısından ayırırken, başlangıç görevi için kullanıcı bağlamının görevleri çalıştırmak için kullanılanla aynı olması gerektiğini unutmayın:

```
"startTask": {
  "commandLine": "cmdkey /add:storageaccountname.file.core.windows.net
    /user:AZURE\\markscuscusbatch /pass:storage_account_key",
  "userIdentity":{
    "autoUser": {
      "elevationLevel": "nonadmin",
      "scope": "pool"
    }
}
```

Örnek iş görev komut satırı:
```
"commandLine":"net use S:
  \\\\storageaccountname.file.core.windows.net\\rendering &
3dsmaxcmdio.exe -v:5 -rfw:0 -10 -end:10
  -bitmapPath:\"s:\\3dsMax\\Dragon\\Assets\"
  -outputName:\"s:\\3dsMax\\Dragon\\RenderOutput\\dragon.jpg\"
  -w:1280 -h:720
  \"s:\\3dsMax\\Dragon\\Assets\\Dragon_Character_Rig.max\""
```

### <a name="accessing-files"></a>Dosyalara erişim

İş görevleri, eşlenen bir sürücü veya UNC yolunu kullanarak monte edilmiş dosya sistemini kullanarak giriş dosyaları ve çıktı dosyaları için yollar belirtir.

### <a name="copying-input-asset-files-from-blob-storage-to-batch-pool-vms"></a>Giriş varlık dosyalarını blob depolamadan Toplu havuz VM'lerine kopyalama

Azure Dosyaları, Azure Depolama desteğine sahip tüm ana API'ler ve araçlar tarafından desteklenir; örneğin azcopy, Azure CLI, Storage Explorer, Azure PowerShell, Toplu Explorer, vb.

[Azure Dosya Eşitlemi,](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) dosyaları şirket içi dosya sistemi yle Azure Dosyası paylaşımı arasında otomatik olarak eşitlemek için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Depolama seçenekleri hakkında daha fazla bilgi için ayrıntılı belgelere bakın:

* [Azure blob depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)
* [Blobfuse](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux)
* [Azure Dosyalar](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)
