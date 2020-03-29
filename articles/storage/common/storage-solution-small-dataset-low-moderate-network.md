---
title: Düşük ve orta ağ bant genişliğine sahip küçük veri kümeleri için Azure veri aktarım seçenekleri| Microsoft Dokümanlar
description: Ortamınızda düşük ve orta ağ bant genişliğine sahipseniz ve küçük veri kümeleri aktarmayı planlıyorsanız, veri aktarımı için bir Azure çözümünüzü nasıl seçeceğinizi öğrenin.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 12/05/2018
ms.author: alkohli
ms.openlocfilehash: 3e6f4f3eb312f0d4d96a008c0944a9608d0bf4a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60397286"
---
# <a name="data-transfer-for-small-datasets-with-low-to-moderate-network-bandwidth"></a>Bant genişliği düşük veya orta olduğunda küçük veri kümeleri için veri aktarımı
 
Bu makalede, ortamınızda düşük ve orta ağ bant genişliği varsa ve küçük veri kümeleri aktarmayı planlıyorsanız veri aktarım çözümlerine genel bir bakış sağlar. Makalede ayrıca önerilen veri aktarım seçenekleri ve bu senaryo için ilgili anahtar yetenek matrisi açıklanmaktadır.

Kullanılabilir tüm veri aktarım seçeneklerine genel bir bakış anlamak için [Azure veri aktarım çözümünün seçin'e](storage-choose-data-transfer-solution.md)gidin.

## <a name="scenario-description"></a>Senaryo açıklaması

Küçük veri kümeleri, gb sırasına göre veri boyutlarını birkaç TB'ye yönlendirir. Düşük ve orta ağ bant genişliği, 45 Mbps (veri merkezinde T3 bağlantısı) ile 1 Gbps anlamına gelir.

- Yalnızca bir avuç dosya aktarıyorsanız ve veri aktarımını otomatikleştirmeniz gerekmiyorsa, grafik arabirimine sahip araçları göz önünde bulundurun.
- Sistem yönetimi konusunda rahatsanız, komut satırı veya programlı/komut dosyası araçlarını düşünün.

## <a name="recommended-options"></a>Önerilen seçenekler

Bu senaryoda önerilen seçenekler şunlardır:

- Azure portalında Azure Depolama Gezgini ve Azure Depolama gibi **grafik arabirim araçları.** Bunlar, verilerinizi görüntülemek ve birkaç dosyayı hızlı bir şekilde aktarmak için kolay bir yol sağlar.

    - **Azure Depolama Gezgini** - Bu çapraz platform aracı, Azure depolama hesaplarınızın içeriğini yönetmenize olanak tanır. Blob'ları, dosyaları, kuyrukları, tabloları ve Azure Cosmos DB varlıklarını yüklemenize, indirmenize ve yönetmenize olanak tanır. Blob'ları ve klasörleri yönetmek için Blob depolama ile kullanın, ayrıca yerel dosya sisteminiz ile Blob depolama nız arasında veya depolama hesapları arasında blob'ları yükleyin ve indirin.
    - **Azure portalı** - Azure portalındaki Azure Depolama, dosyaları keşfetmek ve yeni dosyaları teker teker yüklemek için web tabanlı bir arayüz sağlar. Dosyalarınızı hızlı bir şekilde keşfetmek veya yalnızca bir avuç yeniaraç yüklemek için herhangi bir araç veya sorun komutları yüklemek istemiyorsanız, bu iyi bir seçenektir.

- AzCopy/PowerShell/Azure CLI ve Azure Depolama REST API'leri gibi **komut dosyası oluşturma/programlı araçlar.**

    - **AzCopy** - Azure Blobs, Files ve Table depolama alanına en iyi performansla verileri kolayca kopyalamak için bu komut satırı aracını kullanın. AzCopy eşzamanlılık ve paralellik ve kesildiğinde kopyalama işlemlerine devam etme yeteneğini destekler.
    - **Azure PowerShell** - Sistem yönetiminden memnun olan kullanıcılar için veri aktarmak için Azure PowerShell'deki Azure Depolama modüllerini kullanın.
    - **Azure CLI** - Azure hizmetlerini yönetmek ve Verileri Azure Depolama'ya yüklemek için bu çapraz platform aracını kullanın.
    - **Azure Depolama REST API'leri/SDK'ları** – Bir uygulama geliştirirken, uygulamayı Azure Depolama REST API'lerine/SDK'larına karşı geliştirebilir ve birden çok dilde sunulan Azure istemci kitaplıklarını kullanabilirsiniz.


## <a name="comparison-of-key-capabilities"></a>Anahtar özelliklerin karşılaştırılması

Aşağıdaki tabloda anahtar yetenekleri farklılıkları özetlenebilir.

| Özellik | Azure Storage Gezgini | Azure portalında | AzCopy<br>Azure PowerShell<br>Azure CLI | Azure Depolama REST API'leri veya SDK'ları |
|---------|------------------------|--------------|-----------------------------------------|---------------------------------|
| Kullanılabilirlik | İndirme ve yükleme <br>Bağımsız araç | Azure portalında Web tabanlı arama araçları | Komut satırı aracı |.NET, Java, Python, JavaScript, C++, Go, Ruby ve PHP'de programlanabilir arayüzler |
| Grafik arabirimi | Evet | Evet | Hayır | Hayır |
| Desteklenen platformlar | Windows, Mac, Linux | Web tabanlı |Windows, Mac, Linux |Tüm platformlar |
| İzin verilen Blob depolama işlemleri<br>blobs ve klasörler için | Karşıya Yükle<br>İndirme<br>Yönetme | Karşıya Yükle<br>İndirme<br>Yönetme |Karşıya Yükle<br>İndirme<br>Yönetme | Evet, özelleştirilebilir |
| İzin Verilen Veri Gölü Gen1 depolama<br>dosya ve klasörler için işlemler | Karşıya Yükle<br>İndirme<br>Yönetme | Hayır |Karşıya Yükle<br>İndirme<br>Yönetme                   | Hayır |
| İzin verilen Dosya depolama işlemleri<br>dosyalar ve dizinler için | Karşıya Yükle<br>İndirme<br>Yönetme | Karşıya Yükle<br>İndirme<br>Yönetme   |Karşıya Yükle<br>İndirme<br>Yönetme | Evet, özelleştirilebilir |
| İzin verilen Tablo depolama işlemleri<br>tablolar için |Yönetme | Hayır |AzCopy v7'de tablo desteği |Evet, özelleştirilebilir|
| İzin verilen Sıra depolama | Yönetme | Hayır  |Hayır | Evet, özelleştirilebilir|


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Depolama Gezgini ile nasıl veri](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/move-data-to-azure-blob-using-azure-storage-explorer)aktarılamayı öğrenin.
- [AzCopy ile verileri aktarma](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)

