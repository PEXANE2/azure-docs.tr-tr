---
title: Sanal dosya sistemini havuza monte edin - Azure Toplu İş | Microsoft Dokümanlar
description: Toplu Iş havuzuna sanal dosya sistemini nasıl monte ediniz öğrenin.
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/13/2019
ms.author: labrenne
ms.openlocfilehash: bdf0b3bfc955d8a2e2ce1b363c8699ca719b957c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919014"
---
# <a name="mount-a-virtual-file-system-on-a-batch-pool"></a>Toplu iş havuzuna sanal dosya sistemi takma

Azure Toplu İşlem artık Toplu Toplu havuzlarınızda Windows veya Linux işlem düğümlerinde bulut depolama veya harici bir dosya sistemi montajını destekler. Bir işlem düğümü bir havuza katıldığında, sanal dosya sistemi monte edilir ve bu düğümüzerinde yerel bir sürücü olarak kabul edilir. Azure Dosyaları, Azure Blob depolama, Ağ Dosya Sistemi (NFS) gibi [Avere vFXT önbelleği](../avere-vfxt/avere-vfxt-overview.md)veya Ortak Internet Dosya Sistemi (CIFS) gibi dosya sistemleri monte edebilirsiniz.

Bu makalede, [.NET için Toplu Yönetim Kitaplığı'nı](https://docs.microsoft.com/dotnet/api/overview/azure/batch?view=azure-dotnet)kullanarak bir bilgi işlem düğümleri havuzuna sanal dosya sistemi nasıl monte edileceksiniz.

> [!NOTE]
> Sanal dosya sistemi montajı, 2019-08-19 tarihinde veya sonrasında oluşturulan Toplu Havuzlarda desteklenir. 2019-08-19'dan önce oluşturulan toplu havuzlar bu özelliği desteklemez.
> 
> Bir işlem düğümüne dosya sistemleri montaj için API'ler [Toplu .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch?view=azure-dotnet) kitaplığı bir parçasıdır.

## <a name="benefits-of-mounting-on-a-pool"></a>Havuza monte nin faydaları

Görevlerin büyük bir veri kümesinden kendi verilerini almasına izin vermek yerine, dosya sistemini havuza monte etmek, görevlerin gerekli verilere erişmesi için daha kolay ve verimli hale getirir.

Film oluşturma gibi ortak bir veri kümesine erişim gerektiren birden çok görev içeren bir senaryo düşünün. Her görev, sahne dosyalarından aynı anda bir veya daha fazla kare işler. Sahne dosyalarını içeren bir sürücü takarak, bilgi düğümlerinin paylaşılan verilere erişmesi daha kolaydır. Ayrıca, temel dosya sistemi, verilere aynı anda erişen bilgi işlem düğümlerinin sayısının gerektirdiği performans ve ölçek (iş ve IOPS) temel alınarak bağımsız olarak seçilebilir ve ölçeklenebilir. Örneğin, bellek içi dağıtılmış bir [Avere vFXT](../avere-vfxt/avere-vfxt-overview.md) önbelleği, şirket içinde bulunan kaynak verilere erişerek binlerce eşzamanlı render düğümüyle büyük sinema ölçeğindeki işlemi desteklemek için kullanılabilir. Alternatif olarak, bulut tabanlı Blob depolamasında zaten bulunan veriler [için, blobfuse](../storage/blobs/storage-how-to-mount-container-linux.md) bu verileri yerel bir dosya sistemi olarak monte etmek için kullanılabilir. Blobfuse yalnızca Linux düğümlerinde kullanılabilir, ancak [Azure Files](https://azure.microsoft.com/blog/a-new-era-for-azure-files-bigger-faster-better/) benzer bir iş akışı sağlar ve hem Windows hem de Linux'ta kullanılabilir.

## <a name="mount-a-virtual-file-system-on-a-pool"></a>Sanal dosya sistemini havuza monte edin  

Sanal bir dosya sistemini havuza monte etmek, dosya sistemini havuzdaki her işlem düğümü için kullanılabilir hale getirir. Dosya sistemi, bir bilgi işlem düğümü bir havuza katıldığında veya düğüm yeniden başlatıldığında veya yeniden görüntülendiğinde yapılandırılır.

Bir dosya sistemini havuza monte `MountConfiguration` etmek için bir nesne oluşturun. Sanal dosya sisteminize uyan nesneyi `AzureFileShareConfiguration` `NfsMountConfiguration`seçin: `CifsMountConfiguration` `AzureBlobFileSystemConfiguration`, , veya .

Tüm montaj yapılandırma nesneleri aşağıdaki temel parametrelere ihtiyaç duyar. Bazı montaj yapılandırmalarında, kod örneklerinde daha ayrıntılı olarak açıklanan kullanılan dosya sistemine özgü parametreler vardır.

- **Hesap adı veya kaynak**: Sanal bir dosya paylaşımı yapmak için depolama hesabının veya kaynağının adını almanız gerekir.
- **Bağıl montaj yolu veya Kaynak**: Dosya sisteminin işlem düğümüne monte `fsmounts` edilen konumu, düğüm üzerinden `AZ_BATCH_NODE_MOUNTS_DIR`erişilebilen standart dizine göre. Tam konumu düğüm üzerinde kullanılan işletim sistemine bağlı olarak değişir. Örneğin, bir Ubuntu düğümündeki fiziksel konum eşlenir `mnt\batch\tasks\fsmounts`ve bir CentOS düğümünde `mnt\resources\batch\tasks\fsmounts`eşlenir.
- **Montaj seçenekleri veya blobfuse seçenekleri**: Bu seçenekler, bir dosya sistemi montajı için belirli parametreleri açıklar.

`MountConfiguration` Nesne oluşturulduktan sonra, havuzu oluştururken `MountConfigurationList` nesneyi özelliğe atayın. Dosya sistemi, düğüm bir havuza katıldığında veya düğüm yeniden başlatıldığında veya yeniden görüntülendiğinde monte edilir.

Dosya sistemi monte edildiğinde, takılı `AZ_BATCH_NODE_MOUNTS_DIR` dosya sistemlerinin yanı sıra sorun giderme ve hata ayıklama için yararlı olan günlük dosyalarının konumunu gösteren bir ortam değişkeni oluşturulur. Günlük dosyaları, [montaj hatalarını tanıla](#diagnose-mount-errors) bölümünde daha ayrıntılı olarak açıklanmıştır.  

> [!IMPORTANT]
> Bir havuzda monte edilmiş dosya sistemlerinin maksimum sayısı 10'dur. Ayrıntılar ve diğer sınırlar için [Toplu hizmet kotalarına ve sınırlarına](batch-quota-limit.md#other-limits) bakın.

## <a name="examples"></a>Örnekler

Aşağıdaki kod örnekleri, çeşitli dosya paylaşımlarının bir işlem düğümü havuzuna montajını gösterir.

### <a name="azure-files-share"></a>Azure Dosyaları paylaşımı

Azure Files, standart Azure bulut dosya sistemi teklifidir. Montaj yapılandırma kodu örneğindeki parametrelerden herhangi birini nasıl alacağınız hakkında daha fazla bilgi edinmek için [bkz.](../storage/files/storage-how-to-use-files-windows.md)

```csharp
new PoolAddParameter
{
    Id = poolId,
    MountConfiguration = new[]
    {
        new MountConfiguration
        {
            AzureFileShareConfiguration = new AzureFileShareConfiguration
            {
                AccountName = "AccountName",
                AzureFileUrl = "AzureFileShareUrl",
                AccountKey = "StorageAccountKey",
                RelativeMountPath = "RelativeMountPath",
                MountOptions = "-o vers=3.0,dir_mode=0777,file_mode=0777,sec=ntlmssp"
            },
        }
    }
}
```

### <a name="azure-blob-file-system"></a>Azure Blob dosya sistemi

Başka bir seçenek [blobfuse](../storage/blobs/storage-how-to-mount-container-linux.md)üzerinden Azure Blob depolama kullanmaktır. Blob dosya sistemi takmak için depolama hesabınız için bir `AccountKey` veya `SasKey` ihtiyaç fazlası gerektirir. Bu anahtarları alma hakkında bilgi için bkz: [Depolama hesabı erişim anahtarlarını yönet](../storage/common/storage-account-keys-manage.md)veya paylaşılan erişim [imzalarını (SAS) kullanma.](../storage/common/storage-dotnet-shared-access-signature-part-1.md) Blobfuse kullanma hakkında daha fazla bilgi için, blobfuse [Sorun Giderme SSS](https://github.com/Azure/azure-storage-fuse/wiki/3.-Troubleshoot-FAQ)bakın. Blobfuse monte dizin varsayılan erişim elde etmek için, bir **Yönetici**olarak görev çalıştırın. Blobfuse, dizini kullanıcı alanına bağlar ve havuz oluşturmada kök olarak monte edilir. Linux'ta tüm **Yönetici** görevleri kökvardır. FUSE modülü için tüm seçenekler [FUSE başvuru sayfasında](https://manpages.ubuntu.com/manpages/xenial/man8/mount.fuse.8.html)açıklanmıştır.

Sorun giderme kılavuzuna ek olarak, blobfuse deposundaki GitHub sorunları, geçerli blobfuse sorunlarını ve çözümlerini denetlemenin yararlı bir yoludur. Daha fazla bilgi [için, blobfuse sorunları](https://github.com/Azure/azure-storage-fuse/issues)bakın.

```csharp
new PoolAddParameter
{
    Id = poolId,
    MountConfiguration = new[]
    {
        new MountConfiguration
        {
            AzureBlobFileSystemConfiguration = new AzureBlobFileSystemConfiguration
            {
                AccountName = "StorageAccountName",
                ContainerName = "containerName",
                AccountKey = "StorageAccountKey",
                SasKey = "",
                RelativeMountPath = "RelativeMountPath",
                BlobfuseOptions = "-o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120 "
            },
        }
    }
}
```

### <a name="network-file-system"></a>Ağ Dosya Sistemi

Ağ Dosya Sistemleri (NFS), geleneksel dosya sistemlerine Azure Toplu Düğümleri tarafından kolayca erişilebilmesine olanak tanıyan havuz düğümlerine de monte edilebilir. Bu, bulutta dağıtılan tek bir NFS sunucusu veya sanal ağ üzerinden erişilen şirket içi Bir NFS sunucusu olabilir. Alternatif olarak, şirket içi depolamaya kesintisiz bağlantı sağlayan, verileri isteğe bağlı olarak önbelleğine okumayı sağlayan ve bulut tabanlı bilgi işlem düğümlerine yüksek performans ve ölçek sağlayan [Avere vFXT](../avere-vfxt/avere-vfxt-overview.md) dağıtılmış bellek içi önbellek çözümünden yararlanın.

```csharp
new PoolAddParameter
{
    Id = poolId,
    MountConfiguration = new[]
    {
        new MountConfiguration
        {
            NfsMountConfiguration = new NFSMountConfiguration
            {
                Source = "source",
                RelativeMountPath = "RelativeMountPath",
                MountOptions = "options ver=1.0"
            },
        }
    }
}
```

### <a name="common-internet-file-system"></a>Ortak İnternet Dosya Sistemi

Ortak Internet Dosya Sistemleri (CIFS), geleneksel dosya sistemlerine Azure Toplu Düğümler tarafından kolayca erişilebilmesine olanak tanıyan havuz düğümlerine de monte edilebilir. CIFS, ağ sunucusu dosyaları ve hizmetleri istemek için açık ve çapraz platform mekanizması sağlayan bir dosya paylaşım protokolüdür. CIFS, Microsoft'un Internet ve intranet dosya paylaşımı için geliştirdiği Sunucu İleti Bloğu (SMB) protokolünün gelişmiş sürümüne dayanır ve Windows düğümlerine harici dosya sistemleri monte etmek için kullanılır. Kobİ hakkında daha fazla bilgi edinmek için [Dosya Sunucusu ve SMB'ye](https://docs.microsoft.com/windows-server/storage/file-server/file-server-smb-overview)bakın.

```csharp
new PoolAddParameter
{
    Id = poolId,
    MountConfiguration = new[]
    {
        new MountConfiguration
        {
            CifsMountConfiguration = new CIFSMountConfiguration
            {
                Username = "StorageAccountName",
                RelativeMountPath = "cifsmountpoint",
                Source = "source",
                Password = "StorageAccountKey",
                MountOptions = "-o vers=3.0,dir_mode=0777,file_mode=0777,serverino"
            },
        }
    }
}
```

## <a name="diagnose-mount-errors"></a>Montaj hatalarını tanılama

Bir montaj yapılandırması başarısız olursa, havuzdaki işlem düğümü başarısız olur ve düğüm durumu kullanılamaz hale gelir. Montaj yapılandırma hatasını tanılamak [`ComputeNodeError`](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror) için, hatayla ilgili ayrıntılar için özelliği denetleyin.

Hata ayıklama için günlük dosyalarını almak için, `*.log` dosyaları yüklemek için [OutputFiles'ı](batch-task-output-files.md) kullanın. `AZ_BATCH_NODE_MOUNTS_DIR` Dosyalar, `*.log` konumdaki dosya sistemi montajı hakkında bilgi içerir. Montaj günlüğü dosyaları biçimine sahiptir: `<type>-<mountDirOrDrive>.log` her montaj için. Örneğin, adlı `cifs` `test` bir montaj dizininde bir montaj, adlı `cifs-test.log`bir montaj günlüğü dosyası na sahip olacaktır: .

## <a name="supported-skus"></a>Desteklenen SK'ler

| Yayımcı | Sunduğu | SKU | Azure Dosyaları Paylaş | Blobfuse | NFS yuvası | CIFS montaj |
|---|---|---|---|---|---|---|
| toplu iş | render-centos73 | Işleme | :heavy_check_mark: <br>Not: CentOS 7.7 ile uyumlu</br>| :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Canonical | UbuntuServer | 16.04-LTS, 18.04-LTS | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Credativ | Debian | 8| :heavy_check_mark: | :x: | :heavy_check_mark: | :heavy_check_mark: |
| Credativ | Debian | 9 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-reklamlar | linux-veri-bilim-vm | linuxdsvm | :heavy_check_mark: <br>Not: CentOS 7.4 ile uyumludur. </br> | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-azure-toplu iş | centos-konteyner | 7.6 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-azure-toplu iş | centos-konteyner-rdma | 7.4 | :heavy_check_mark: <br>Not: A_8 veya 9 depolama alanını destekler</br> | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-azure-toplu iş | ubuntu-server-konteyner | 16.04-LTS | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-dsvm | linux-data-science-vm-ubuntu | linuxdsvmubuntu | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| OpenLogic | CentOS | 7.6 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| OpenLogic | CentOS-HPC | 7.4, 7.3, 7.1 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Oracle | Oracle-Linux | 7.6 | :x: | :x: | :x: | :x: |
| Windows | WindowsServer | 2012, 2016, 2019 | :heavy_check_mark: | :x: | :x: | :x: |

## <a name="next-steps"></a>Sonraki adımlar

- [Windows](../storage/files/storage-how-to-use-files-windows.md) veya [Linux](../storage/files/storage-how-to-use-files-linux.md)ile Azure Dosyaları paylaşımını ekleme hakkında daha fazla bilgi edinin.
- [Blobfuse](https://github.com/Azure/azure-storage-fuse) sanal dosya sistemlerini kullanma ve montaj hakkında bilgi edinin.
- NFS ve uygulamaları hakkında bilgi edinmek için [Ağ Dosya Sistemi'ne genel bakışa](https://docs.microsoft.com/windows-server/storage/nfs/nfs-overview) bakın.
- CIFS hakkında daha fazla bilgi edinmek için [Microsoft SMB protokolü ve CIFS protokolüne genel bakışa](https://docs.microsoft.com/windows/desktop/fileio/microsoft-smb-protocol-and-cifs-protocol-overview) bakın.
