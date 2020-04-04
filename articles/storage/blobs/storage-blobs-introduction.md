---
title: Blob (nesne) depolamasına giriş - Azure Depolama
description: Azure Blob depolama, metin veya ikili veri gibi çok miktarda yapılandırılmamış nesne verilerini depolar. Azure Blob depolama, yüksek oranda ölçeklenebilir ve kullanılabilir. İstemciler, REST kullanarak veya Azure Depolama istemci kitaplıkları aracılığıyla programlama yoluyla Azure CLI’den veya PowerShell’den Blob depolamadaki veri nesnelerine erişebilir.
services: storage
author: tamram
ms.service: storage
ms.topic: overview
ms.date: 03/18/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: f7d1967c8a9585fbf5131da986595761030f0e5f
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631989"
---
# <a name="introduction-to-azure-blob-storage"></a>Azure Blob depolamaya giriş

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

## <a name="blob-storage-resources"></a>Blob depolama kaynakları

Blob depolama üç tür kaynak sunar:

- **Depolama hesabı**
- Depolama hesabındaki bir **kapsayıcı**
- Bir kapta bir **damla**

Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Depolama hesabı, kapsayıcılar ve lekeler arasındaki ilişkiyi gösteren diyagram](./media/storage-blobs-introduction/blob1.png)

### <a name="storage-accounts"></a>Depolama hesapları

Depolama hesabı, verileriniz için Azure'da benzersiz bir ad alanı sağlar. Azure Depolama'da depoladığınız her nesnenin benzersiz hesap adınızı içeren bir adresi vardır. Hesap adı ve Azure Depolama blob uç noktası birleşimi, depolama hesabınızdaki nesnelerin temel adresini oluşturur.

Örneğin, depolama *hesabınıza mystorageaccount*adı verebiliyorsa, Blob depolama için varsayılan bitiş noktası şudur:

```
http://mystorageaccount.blob.core.windows.net
```

Depolama hesabı oluşturmak için [bkz.](../common/storage-account-create.md) Depolama hesapları hakkında daha fazla bilgi edinmek için [Azure depolama hesabına genel bakış'a](../common/storage-account-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)bakın.

### <a name="containers"></a>Kapsayıcılar

Kapsayıcı, dosya sistemindeki dizine benzer bir dizi blob düzenler. Depolama hesabında sınırsız sayıda kapsayıcı olabilir ve her kapsayıcı sınırsız sayıda blob depolayabilir.

> [!NOTE]
> Kapsayıcı adındaki harfler küçük harf olmalıdır. Kapsayıcıları adlandırma hakkında daha fazla bilgi için, [Bkz. Adlandırma ve Başvuru Kapsayıcıları, Blobs ve Metadata.](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata)

### <a name="blobs"></a>Bloblar

Azure Depolama üç tür blob'u destekler:

- **Blobs'un** metin ve ikili verileri yaklaşık 4,7 TB'a kadar depolamasını engelleyin. Blok blobları, ayrı ayrı yönetilebilen veri bloklarından oluşur.
- **Ek lekelerblok** lekeleri gibi bloklardan oluşur, ancak ek işlemler için optimize edilmiştir. Ekleme blobları sanal makine verilerini günlüğe alma gibi senaryolar için idealdir.
- **Sayfa lekeleri** rasgele erişim dosyalarını 8 TB boyutunda depolar. Sayfa blobs sanal sabit disk (VHD) dosyaları depolamak ve Azure sanal makineler için diskler olarak hizmet vermektedir. Sayfa lekeleri hakkında daha fazla bilgi için Azure [sayfa lekelerine genel bakış](storage-blob-pageblob-overview.md)

Farklı blob türleri hakkında daha fazla bilgi için [Bkz.](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

## <a name="move-data-to-blob-storage"></a>Verileri Blob depolamaya taşıma

Varolan verileri Blob depolamasına geçirmek için çeşitli çözümler vardır:

- **AzCopy,** Windows ve Linux için blob depolamaya, kapsayıcılara veya depolama hesaplarından verileri kopyalayan kullanımı kolay bir komut satırı aracıdır. AzCopy hakkında daha fazla bilgi için, [AzCopy v10 (Önizleme) ile veri aktarımı](../common/storage-use-azcopy-v10.md)bakın.
- **Azure Depolama Veri Hareketi kitaplığı,** verileri Azure Depolama hizmetleri arasında taşımak için bir .NET kitaplığıdır. AzCopy yardımcı programı Veri Hareketi kitaplığı ile oluşturulmuşdur. Daha fazla bilgi için Veri Hareketi kitaplığı için [başvuru belgelerine](/dotnet/api/microsoft.azure.storage.datamovement) bakın.
- **Azure Veri Fabrikası,** hesap anahtarını, paylaşılan erişim imzasını, hizmet sorumlusunu veya Azure kaynakları için yönetilen kimlikleri kullanarak Blob depolamaalanına veri kopyalamayı destekler. Daha fazla bilgi için Azure [Veri Fabrikası'nı kullanarak Azure Blob depolama alanına veya Azure Blob depolamasından veri kopyalama'ya](../../data-factory/connector-azure-blob-storage.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)bakın.
- **Blobfuse,** Azure Blob depolama alanı için sanal bir dosya sistemi sürücüsüdür. Linux dosya sistemi üzerinden Depolama hesabınızdaki mevcut blok blob verilerine erişmek için blobfuse kullanabilirsiniz. Daha fazla bilgi için [Blob depolamanın blobfuse içeren bir dosya sistemi olarak nasıl monte edilebildiğini](storage-how-to-mount-container-linux.md)görün.
- **Azure Veri Kutusu** hizmeti, büyük veri kümeleri veya ağ kısıtlamaları kablo üzerinden veri yüklemeyi gerçekçi hale getirince şirket içi verileri Blob depolamaalanına aktarmak için kullanılabilir. Veri boyutunuza bağlı olarak, [Microsoft'tan Azure Veri Kutusu Diski](../../databox/data-box-disk-overview.md), [Azure Veri Kutusu](../../databox/data-box-overview.md)veya Azure Veri [Kutusu Ağır](../../databox/data-box-heavy-overview.md) aygıtları isteyebilirsiniz. Daha sonra verilerinizi bu aygıtlara kopyalayabilir ve Blob depolama alanına yüklenmesi için Microsoft'a geri gönderebilirsiniz.
- **Azure İçe Alma/Dışa Aktarma hizmeti,** sağladığınız sabit diskleri kullanarak depolama hesabınıza büyük miktarda veri almanın veya dışa aktarmanın bir yolunu sağlar. Daha fazla bilgi için [bkz.](../common/storage-import-export-service.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Depolama hesabı oluşturma](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
- [Blob depolama için ölçeklenebilirlik ve performans hedefleri](scalability-targets.md)
