---
title: Düşük-orta ağ bant genişliğine sahip küçük veri kümeleri için Azure veri aktarımı seçenekleri | Microsoft Docs
description: Ortamınızda orta düzeyde ağ bant genişliğiniz olduğunda ve küçük veri kümelerini aktarmayı planlıyorsanız, veri aktarımı için bir Azure çözümü seçme hakkında bilgi edinin.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: alkohli
ms.openlocfilehash: 4f21e7f64338b7d50ca401081bf73ca0c1a1c88f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85504329"
---
# <a name="data-transfer-for-small-datasets-with-low-to-moderate-network-bandwidth"></a>Bant genişliği düşük veya orta olduğunda küçük veri kümeleri için veri aktarımı
 
Bu makalede, ortamınızda orta düzeyde ağ bant genişliğiniz olduğunda ve küçük veri kümelerini aktarmayı planladığınızda veri aktarımı çözümlerine genel bir bakış sağlanır. Makalede ayrıca önerilen veri aktarımı seçenekleri ve bu senaryonun ilgili anahtar özelliği matrisi de açıklanmaktadır.

Tüm kullanılabilir veri aktarımı seçeneklerine genel bir bakış için bkz. [Azure veri aktarımı çözümü seçme](storage-choose-data-transfer-solution.md).

## <a name="scenario-description"></a>Senaryo açıklaması

Küçük veri kümeleri, GBs 'nin bazı bir TBs sırasındaki veri boyutlarına başvurur. Düşük-orta ağ bant genişliği 45 Mbps (veri merkezinde T3 bağlantısı) ile 1 Gbps arasında bir bağlantı gerektirir.

- Yalnızca birkaç dosyayı aktarıyorsanız ve veri aktarımını otomatikleştirmeniz gerekmiyorsa, araçları bir grafik arabirimiyle değerlendirin.
- Sistem yönetimi konusunda deneyimli değilseniz komut satırı veya programlı/komut dosyası araçları ' nı düşünün.

## <a name="recommended-options"></a>Önerilen Seçenekler

Bu senaryoda önerilen seçenekler şunlardır:

- Azure Depolama Gezgini ve Azure portal Azure depolama gibi **grafik arabirim araçları** . Bu, verilerinizi görüntülemenin ve birkaç dosyayı hızlı bir şekilde aktarmanın kolay bir yolunu sağlar.

    - **Azure Depolama Gezgini** -bu platformlar arası araç, Azure depolama hesaplarınızın içeriğini yönetmenizi sağlar. Blob, dosya, kuyruk, tablo ve Azure Cosmos DB varlıklarını karşıya yüklemenize, indirmelerine ve yönetmenize olanak sağlar. Blob 'ları ve klasörleri yönetmek için blob depolamayla birlikte kullanın, ayrıca yerel dosya sisteminiz ile BLOB depolama alanı arasında veya depolama hesapları arasında Blobları karşıya yükleyin ve indirin.
    - **Azure Portal** -Azure Portal Azure depolama, dosyaları keşfetmeye ve yeni dosyaları tek seferde karşıya yüklemeye yönelik Web tabanlı bir arabirim sağlar. Bu, dosyalarınızı hızlı bir şekilde araştırmak için herhangi bir araç veya sorun komutu yüklemek istemiyorsanız ya da yalnızca yeni bir tane karşıya yüklemek istemiyorsanız iyi bir seçenektir.

- AzCopy/PowerShell/Azure CLı ve Azure depolama REST API 'Leri gibi **komut dosyası/programlama araçları** .

    - **AzCopy** -bu komut satırı aracını, En Iyi performansla Azure Blob 'Larına, dosyalarına ve tablo depolamasına verileri kolayca kopyalamak için kullanın. AzCopy eşzamanlılık ve paralellik destekler ve kesintiye uğradığında kopyalama işlemlerini sürdürülemez.
    - **Azure PowerShell** -sistem yönetimi sayesinde kullanıcılara rahat bir şekilde veri aktarmak Için Azure PowerShell Azure Storage modülünü kullanın.
    - **Azure CLI** -bu platformlar arası aracı kullanarak Azure hizmetlerini yönetin ve verileri Azure depolama 'ya yükleyin.
    - **Azure depolama REST API 'leri/SDK 'lar** – bir uygulama oluştururken, uygulamayı Azure Storage REST API 'Leri/SDK 'larda geliştirebilir ve birden çok dilde sunulan Azure istemci kitaplıklarını kullanabilirsiniz.


## <a name="comparison-of-key-capabilities"></a>Anahtar özellikleri karşılaştırması

Aşağıdaki tabloda, önemli özelliklerde farklılıklar özetlenmektedir.

| Özellik | Azure Depolama Gezgini | Azure portal | AzCopy<br>Azure PowerShell<br>Azure CLI | Azure depolama REST API 'Leri veya SDK 'Ları |
|---------|------------------------|--------------|-----------------------------------------|---------------------------------|
| Kullanılabilirlik | İndirme ve yükleme <br>Tek başına aracı | Azure portal web tabanlı araştırma araçları | Komut satırı aracı |.NET, Java, Python, JavaScript, C++, Go, Ruby ve PHP 'de programlanabilir arabirimler |
| Grafik arabirim | Yes | Evet | Hayır | Hayır |
| Desteklenen platformlar | Windows, Mac, Linux | Web tabanlı |Windows, Mac, Linux |Tüm platformlar |
| İzin verilen BLOB depolama işlemleri<br>Bloblar ve klasörler için | Karşıya Yükle<br>İndir<br>Yönetme | Karşıya Yükle<br>İndir<br>Yönetme |Karşıya Yükle<br>İndir<br>Yönetme | Evet, özelleştirilebilir |
| Data Lake Gen1 Storage izin verildi<br>dosyalar ve klasörler için işlemler | Karşıya Yükle<br>İndir<br>Yönetme | Hayır |Karşıya Yükle<br>İndir<br>Yönetme                   | Hayır |
| İzin verilen dosya depolama işlemleri<br>dosyalar ve dizinler için | Karşıya Yükle<br>İndir<br>Yönetme | Karşıya Yükle<br>İndir<br>Yönetme   |Karşıya Yükle<br>İndir<br>Yönetme | Evet, özelleştirilebilir |
| İzin verilen tablo depolama işlemleri<br>tablolar için |Yönetme | Hayır |AzCopy v7 'ta tablo desteği |Evet, özelleştirilebilir|
| İzin verilen kuyruk depolaması | Yönetme | Hayır  |Hayır | Evet, özelleştirilebilir|


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Depolama Gezgini ile veri aktarmayı](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/move-data-to-azure-blob-using-azure-storage-explorer)öğrenin.
- [AzCopy ile verileri aktarma](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)

