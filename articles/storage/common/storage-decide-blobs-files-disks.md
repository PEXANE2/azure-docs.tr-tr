---
title: Azure Blob'ları, Azure Dosyaları'nı veya Azure Disklerini ne zaman kullanacağınıza karar verme
description: Hangi teknolojiyi kullanacağınıza karar vermenize yardımcı olmak için Azure'da verileri depolamanın ve erişmenin farklı yolları hakkında bilgi edinin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 4b1a42e25a6d8c7b4a3c24dffcb858ffe63dd10b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71671040"
---
# <a name="deciding-when-to-use-azure-blobs-azure-files-or-azure-disks"></a>Azure Blob'ları, Azure Dosyaları'nı veya Azure Disklerini ne zaman kullanacağınıza karar verme

Microsoft Azure, verilerinizi bulutta depolamak ve erişmek için Azure Depolama'da çeşitli özellikler sağlar. Bu makale, Azure Dosyaları, Blob'lar ve Diskler'i kapsar ve bu özellikler arasında seçim yapmanızı sağlamak üzere tasarlanmıştır.

## <a name="scenarios"></a>Senaryolar

Aşağıdaki tablo Dosyaları, Blob'ları ve Diskleri karşılaştırır ve her biri için uygun örnek senaryoları gösterir.

| Özellik | Açıklama | Kullanılması gereken durumlar |
|--------------|-------------|-------------|
| **Azure Dosyalar** | SMB arabirimi, istemci kitaplıkları ve depolanan dosyalara her yerden erişim sağlayan bir [REST arabirimi](/rest/api/storageservices/file-service-rest-api) sağlar. | Azure'da çalışan diğer uygulamalar arasında veri paylaşmak için yerel dosya sistemi API'lerini zaten kullanan bir uygulamayı buluta "kaldırmave kaydırmak" istiyorsunuz.<br/><br/>Birçok sanal makineden erişilmesi gereken geliştirme ve hata ayıklama araçlarını depolamak istiyorsunuz. |
| **Azure Blobları** | İstemci kitaplıkları ve yapılandırılmamış verilerin blok lekelerinde büyük bir ölçekte depolanıp erişilmesine olanak tanıyan bir [REST arabirimi](/rest/api/storageservices/blob-service-rest-api) sağlar.<br/><br/>Ayrıca kurumsal büyük veri analizi çözümleri için [Azure Veri Gölü Depolama Gen2'yi](../blobs/data-lake-storage-introduction.md) de destekler. | Uygulamanızın akış ve rasgele erişim senaryolarını desteklemesini istiyorsunuz.<br/><br/>Uygulama verilerine her yerden erişebilmek istiyorsunuz.<br/><br/>Azure'da kurumsal veri gölü oluşturmak ve büyük veri analitiği gerçekleştirmek istiyorsunuz. |
| **Azure Diskleri** | İstemci kitaplıkları ve verilerin kalıcı olarak depolanıp ekli bir sanal sabit diskten erişilmesine olanak tanıyan bir [REST arabirimi](/rest/api/compute/manageddisks/disks/disks-rest-api) sağlar. | Kalıcı disklere veri okumak ve yazmak için yerel dosya sistemi API'lerini kullanan uygulamaları kaldırmak ve değiştirmek istiyorsunuz.<br/><br/>Diskin bağlı olduğu sanal makinenin dışından erişilmesi gerekmeyen verileri depolamak istiyorsunuz. |


## <a name="next-steps"></a>Sonraki adımlar

Verilerinizin nasıl depolanıp erişilenlerle ilgili kararlar alırken, ilgili maliyetleri de göz önünde bulundurmalısınız. Daha fazla bilgi için Azure [Depolama Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/storage/)bakın.
  
Bazı Kobİ özellikleri bulut için geçerli değildir. Daha fazla bilgi için bkz. Azure [Dosyası hizmeti tarafından desteklenmeyen Özellikler.](/rest/api/storageservices/features-not-supported-by-the-azure-file-service)
 
Azure Blobs hakkında daha fazla bilgi için, Azure [Blob depolama alanı nedir makalemize bakın.](../blobs/storage-blobs-overview.md)

Disk Depolama hakkında daha fazla bilgi için [yönetilen disklere Giriş'e](../../virtual-machines/windows/managed-disks-overview.md)bakın.

Azure Dosyaları hakkında daha fazla bilgi için, [Azure Dosyaları dağıtımı için planlama](../files/storage-files-planning.md)başlıklı makalemize bakın.