---
title: Kaynak dosyaları oluşturma ve kullanma - Azure Toplu İşlemi
description: Çeşitli giriş kaynaklarından Toplu iş kaynağı dosyalarının nasıl oluşturulup oluşturulmayı öğrenin. Bu makalede, nasıl oluşturmak ve bir VM bunları yerleştirmek için birkaç yaygın yöntemleri kapsar.
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: article
ms.date: 03/18/2020
ms.author: labrenne
ms.openlocfilehash: 0fe859ac30e7b8050d1f4688d7cf106a465e7566
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79531150"
---
# <a name="creating-and-using-resource-files"></a>Kaynak dosyaları oluşturma ve kullanma

Azure Toplu İşlem görevi genellikle işlemek için bir tür veri gerektirir. Kaynak dosyaları, bu verileri bir görev aracılığıyla Toplu sanal makinenize (VM) sağlamanın yoludur. Tüm görev türleri kaynak dosyalarını destekler: görevler, başlangıç görevleri, iş hazırlama görevleri, iş bırakma görevleri, vb. Bu makalede, kaynak dosyaları oluşturmak ve bir VM bunları yerleştirmek için nasıl birkaç yaygın yöntemleri kapsar.  

Kaynak dosyaları Toplu Olarak VM'ye veri koyar, ancak veri türü ve nasıl kullanıldığı esnektir. Ancak, bazı yaygın kullanım örnekleri vardır:

1. Başlangıç görevinde kaynak dosyalarını kullanarak her VM'de ortak dosyaları sağlama
1. Görevler tarafından işlenecek giriş verilerini sağlama

Genel dosyalar, örneğin, görevlerinizin çalıştırdığı uygulamaları yüklemek için kullanılan bir başlangıç görevindeki dosyalar olabilir. Giriş verileri ham görüntü veya video verileri veya Toplu İşlem tarafından işlenecek herhangi bir bilgi olabilir.

## <a name="types-of-resource-files"></a>Kaynak dosya türleri

Kaynak dosyaları oluşturmak için birkaç farklı seçenek vardır. Kaynak dosyaları için oluşturma işlemi, özgün verilerin depolandığı yere bağlı olarak değişir.

Kaynak dosyası oluşturma seçenekleri:

- [Depolama kapsayıcısı URL'si](#storage-container-url): Azure'daki herhangi bir depolama kapsayıcısından kaynak dosyası oluşturur
- [Depolama kapsayıcı adı](#storage-container-name): Toplu İşleme'ye bağlı bir Azure depolama hesabındaki bir kapsayıcının adından kaynak dosyası oluşturur
- [Web bitiş noktası](#web-endpoint): Geçerli herhangi bir HTTP URL'sinden kaynak dosyası oluşturur

### <a name="storage-container-url"></a>Depolama kapsayıcısı URL'si

Depolama kapsayıcısı URL'sini kullanmak, doğru izinlerle Azure'daki herhangi bir depolama kapsayıcısındaki dosyalara erişebilirsiniz anlamına gelir. 

Bu C# örneğinde, dosyalar zaten blob depolama olarak bir Azure depolama kapsayıcısına yüklendi. Kaynak dosyası oluşturmak için gereken verilere erişmek için öncelikle depolama kapsayıcısına erişmemiz gerekir.

Depolama kapsayıcısına erişmek için doğru izinlerle paylaşılan bir erişim imzası (SAS) URI oluşturun. SAS için son kullanma süresini ve izinlerini ayarlayın. Bu durumda, hiçbir başlangıç saati belirtilir, bu nedenle SAS hemen geçerli olur ve oluşturulduktan iki saat sonra sona erer.

```csharp
SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
{
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
    Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List
};
```

> [!NOTE]
> Kapsayıcı erişimi için hem `Read` de `List` izinlere sahip olmanız gerekirken, blob erişiminde yalnızca izin almanız gerekir. `Read`

İzinler yapılandırıldıktan sonra SAS belirteci oluşturun ve depolama kapsayıcısına erişmek için SAS URL'sini biçimlendirin. Depolama kapsayıcısı için biçimlendirilmiş SAS URL'sini [`FromStorageContainerUrl`](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.resourcefile.fromstoragecontainerurl?view=azure-dotnet)kullanarak, '' ile bir kaynak dosyası oluşturun.

```csharp
CloudBlobContainer container = blobClient.GetContainerReference(containerName);

string sasToken = container.GetSharedAccessSignature(sasConstraints);
string containerSasUrl = String.Format("{0}{1}", container.Uri, sasToken);

ResourceFile inputFile = ResourceFile.FromStorageContainerUrl(containerSasUrl);
```

SAS URL oluşturmanın alternatifi, Azure Blob depolama alanında bir kapsayıcıya ve lekelerine anonim, herkese açık okuma erişimi sağlamaktır. Bunu yaparak, hesap anahtarınızı paylaşmadan ve SAS gerektirmeden bu kaynaklara salt okunur erişim izni verebilirsiniz. Genel okuma erişimi genellikle belirli lekelerin her zaman anonim okuma erişimi için kullanılabilir olmasını istediğiniz senaryolar için kullanılır. Bu senaryo çözümünüze uygunsa, blob verilerinize erişimi yönetme hakkında daha fazla bilgi edinmek [için Blobs makalesine Anonim erişimine](../storage/blobs/storage-manage-access-to-resources.md) bakın.

### <a name="storage-container-name"></a>Depolama konteyner adı

Bir SAS URL'si yapılandırmak ve oluşturmak yerine, blob verilerinize erişmek için Azure depolama kapsayıcınızın adını kullanabilirsiniz. Kullandığınız depolama kapsayıcısı Toplu Iş hesabınıza bağlı Azure depolama hesabında olmalıdır. Bu depolama hesabı otomatik depolama hesabı olarak bilinir. Otomatik depolama kapsayıcısını kullanmak, bir depolama kapsayıcısına erişmek için yapılandırmayı ve SAS URL'sini atlamanızı sağlar.

Bu örnekte, kaynak dosyası oluşturma için kullanılacak verilerin Toplu İş hesabınıza bağlı bir Azure Depolama hesabında zaten olduğunu varsayıyoruz. Otomatik depolama hesabınız yoksa, bir hesabı oluşturma ve bağlama hakkında ayrıntılar için [Toplu İş Hesabı Oluştur'daki](batch-account-create-portal.md) adımlara bakın.

Bağlantılı bir depolama hesabı kullanarak, bir Depolama kapsayıcısı için bir SAS URL oluşturmanız ve yapılandırmanız gerekmez. Bunun yerine, bağlı depolama hesabınızdaki depolama kapsayıcısının adını sağlayın.

```csharp
ResourceFile inputFile = ResourceFile.FromAutoStorageContainer(containerName);
```

### <a name="web-endpoint"></a>Web bitiş noktası

Azure Depolama'ya yüklenmeyen veriler kaynak dosyaları oluşturmak için kullanılabilir. Giriş verilerinizi içeren geçerli bir HTTP URL'si belirtebilirsiniz. URL Toplu İş API'sine sağlanır ve ardından veriler bir kaynak dosyası oluşturmak için kullanılır.

Aşağıdaki C# örneğinde, giriş verileri hayali bir GitHub bitiş noktasında barındırılır. API, dosyayı geçerli web bitiş noktasından alır ve göreviniz tarafından tüketilecek bir kaynak dosyası oluşturur. Bu senaryo için kimlik bilgileri gerekmez.

```csharp
ResourceFile inputFile = ResourceFile.FromUrl("https://github.com/foo/file.txt", filePath);
```

## <a name="tips-and-suggestions"></a>İpuçları ve öneriler

Her Azure Toplu İş görevi dosyaları farklı kullanır, bu nedenle Toplu İşlem'in görevlerdeki dosyaları yönetmek için seçenekleri vardır. Aşağıdaki senaryoların kapsamlı olması amaçlanmıyor, bunun yerine birkaç yaygın durumu kapsar ve öneriler sağlayın.

### <a name="many-resource-files"></a>Birçok kaynak dosyası

Toplu Iş tüm aynı, ortak dosyaları kullanan birkaç görev içerebilir. Sık karşılaşılan görev dosyaları birçok görev arasında paylaşılırsa, kaynak dosyalarını kullanmak yerine dosyaları içerecek bir uygulama paketi kullanmak daha iyi bir seçenek olabilir. Uygulama paketleri indirme hızı için optimizasyon sağlar. Ayrıca, uygulama paketlerindeki veriler görevler arasında önbelleğe alınr, bu nedenle görev dosyalarınız sık sık değişmezse, uygulama paketleri çözümünüz için uygun olabilir. Uygulama paketlerinde, Azure Depolama'daki dosyalara erişmek için birden fazla kaynak dosyasını el ile yönetmeniz veya SAS URL'leri oluşturmanız gerekmez. Toplu işlem düğümlerini hesaplamak için uygulama paketlerini depolamak ve dağıtmak için Azure Depolama ile arka planda çalışır.

Her görevin bu göreve özgü birçok dosyası varsa, benzersiz dosyaları kullanan görevlerin genellikle güncelleştirilmesi veya değiştirilmesi gerektiğinden ve uygulama paketleri içeriğiyle yapmak o kadar kolay olmadığından, kaynak dosyaları en iyi seçenektir. Kaynak dosyaları, tek tek dosyaları güncelleştirmek, eklemek veya düzenlemek için ek esneklik sağlar.

### <a name="number-of-resource-files-per-task"></a>Görev başına kaynak dosyası sayısı

Bir görevde belirtilen birkaç yüz kaynak dosyası varsa, Toplu İşlem görevi çok büyük olarak reddedebilir. Görevin kendisindeki kaynak dosya sayısını en aza indirerek görevlerinizi küçük tutmak en iyisidir.

Görevinizin gerektirdiği dosya sayısını en aza indirmenin bir yolu yoksa, kaynak dosyalarının depolama kapsayıcısına başvuran tek bir kaynak dosyası oluşturarak görevi en iyi duruma getirebilirsiniz. Bunu yapmak için kaynak dosyalarınızı bir Azure Depolama kapsayıcısına koyun ve kaynak dosyaları için farklı "kapsayıcı" [yöntemlerini](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.resourcefile?view=azure-dotnet#methods) kullanın. Görevleriniz için indirilecek dosya koleksiyonlarını belirtmek için blob öneki seçeneklerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- Kaynak dosyalarına alternatif olarak [uygulama paketleri](batch-application-packages.md) hakkında bilgi edinin.
- Kaynak dosyaları için kapsayıcıları kullanma hakkında daha fazla bilgi için [Kapsayıcı iş yüklerine](batch-docker-container-workloads.md)bakın.
- Görevlerinizdeki çıktı verilerini nasıl toplayıp kaydedebilirsiniz öğrenmek için, [iş ve görev çıktısını devam edin'](batch-task-output.md)e bakın.
- Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.
