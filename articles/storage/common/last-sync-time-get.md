---
title: Depolama hesabı için Son Eşitleme Zamanı özelliğini denetleme
titleSuffix: Azure Storage
description: Coğrafi olarak çoğaltılmış bir depolama hesabı için **Son Eşitleme Zamanı** özelliğini nasıl denetleriz öğrenin. **Son Eşitleme Zamanı** özelliği, birincil bölgeden gelen tüm yazmaların ikincil bölgeye başarıyla yazıldığı son zamanı gösterir.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 01/16/2019
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 3a406ce6db060b9ff5be7bcadecb6c7ff7e65a1f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77165494"
---
# <a name="check-the-last-sync-time-property-for-a-storage-account"></a>Depolama hesabı için Son Eşitleme Zamanı özelliğini denetleme

Bir depolama hesabı yapılandırdığınızda, verilerinizin birincil bölgeden yüzlerce mil uzaktaki ikincil bir bölgeye kopyalanmasını belirtebilirsiniz. Coğrafi çoğaltma, birincil bölgede doğal afet gibi önemli bir kesinti durumunda verileriniz için dayanıklılık sağlar. İkincil bölgeye okuma erişimini ayrıca etkinleştirirseniz, birincil bölge kullanılamaz hale gelirse verileriniz okuma işlemleri için kullanılabilir durumda kalır. Birincil bölge yanıt vermiyorsa, uygulamanızı ikincil bölgeden okumaya sorunsuz bir şekilde geçmek için tasarlayabilirsiniz.

Coğrafi yedekli depolama (GRS) ve coğrafi bölge yedekli depolama (GZRS) (önizleme) verilerinizi eşzamanlı olarak ikincil bir bölgeye kopyalar. İkincil bölgeye okuma erişimi için, okuma-erişim coğrafi yedekli depolama (RA-GRS) veya okuma-erişim coğrafi bölge yedekli depolama (RA-GZRS) etkinleştirin. Azure Depolama tarafından sunulan fazlalık için çeşitli seçenekler hakkında daha fazla bilgi için Azure [Depolama artıklığına](storage-redundancy.md)bakın.

Bu makalede, birincil ve ikincil bölgeler arasındaki tutarsızlığı değerlendirebilmeniz için depolama hesabınız için **Son Eşitleme Süresi** özelliğini nasıl denetleyebilirsiniz.

## <a name="about-the-last-sync-time-property"></a>Son Eşitleme Zamanı özelliği hakkında

Coğrafi çoğaltma asynchronous olduğundan, birincil bölgeye yazılan verilerin bir kesinti oluştuğu anda ikincil bölgeye henüz yazılmamış olması mümkündür. **Son Eşitleme Zamanı** özelliği, birincil bölgeden gelen verilerin ikincil bölgeye en son ne zaman başarıyla yazıldığını gösterir. Son eşitleme zamanından önce birincil bölgeye yapılan tüm yazılar ikincil konumdan okunabilir. Son eşitleme süresi özelliğinden sonra birincil bölgeye yapılan yazmalar henüz okunabilir veya kullanılamayabilir.

**Son Eşitleme Zamanı** özelliği BIR GMT tarih/saat değeridir.

## <a name="get-the-last-sync-time-property"></a>Son Eşitleme Zamanı özelliğini alma

**Son Eşitleme Zamanı** özelliğinin değerini almak için PowerShell veya Azure CLI'yi kullanabilirsiniz.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell ile depolama hesabı için son eşitleme süresini elde etmek için, coğrafi çoğaltma istatistikleri almayı destekleyen bir Azure Depolama önizleme modülü yükleyin. Örneğin:

```powershell
Install-Module Az.Storage –Repository PSGallery -RequiredVersion 1.1.1-preview –AllowPrerelease –AllowClobber –Force
```

Ardından depolama hesabının **GeoReplicationStats.LastSyncTime** özelliğini kontrol edin. Yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
$lastSyncTime = $(Get-AzStorageAccount -ResourceGroupName <resource-group> `
    -Name <storage-account> `
    -IncludeGeoReplicationStats).GeoReplicationStats.LastSyncTime
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLI ile depolama hesabıiçin son eşitleme süresini almak için depolama hesabının **geoReplicationStats.lastSyncTime** özelliğini kontrol edin. `--expand` **geoReplicationStats**altında iç içe özellikleri için değerleri döndürmek için parametreyi kullanın. Yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```azurecli-interactive
$lastSyncTime=$(az storage account show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --expand geoReplicationStats \
    --query geoReplicationStats.lastSyncTime \
    --output tsv)
```

---

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Depolama artıklığı](storage-redundancy.md)
- [Depolama hesabı için artıklık seçeneğini değiştirme](redundancy-migration.md)
- [Okuma erişimi coğrafi yedekli depolama yı kullanarak yüksek kullanılabilir uygulamalar tasarlama](storage-designing-ha-apps-with-ragrs.md)