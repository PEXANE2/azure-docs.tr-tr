---
title: Kullanılabilir Azure dosyaları protokolleri
description: Azure dosya paylaşma oluşturmadan önce kullanılabilir protokoller hakkında bilgi edinin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 09/15/2020
ms.author: rogarana
ms.subservice: files
ms.custom: references_regions
ms.openlocfilehash: 90d51d7eb5425530ebad0fc9ac66ba2c95776076
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90564977"
---
# <a name="azure-file-share-protocols"></a>Azure dosya paylaşma protokolleri

Azure dosyaları, Azure dosya paylaşımlarınızı bağlamaya ve bağlamaya yönelik iki protokol sunar. [Sunucu Ileti bloğu (SMB) protokolü](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) ve [ağ dosya sistemi (NFS) protokolü](https://en.wikipedia.org/wiki/Network_File_System) (Önizleme). Azure dosyaları şu anda çoklu protokol erişimini desteklemez, bu nedenle bir paylaşımın yalnızca bir NFS paylaşımında veya bir SMB paylaşımında olabilir. Bu nedenle, Azure dosya paylaşımları oluşturmadan önce gereksinimlerinize en uygun protokolü belirlemeyi öneririz.

## <a name="differences-at-a-glance"></a>Bir bakışta farklılıklar

|Öne çıkan özelliği  |NFS (Önizleme)  |SMB  |
|---------|---------|---------|
|Erişim protokolleri     |NFS 4,1         |SMB 2,1, SMB 3,0         |
|Desteklenen işletim sistemi     |Linux çekirdek sürümü 4.3 +         |Windows 2008 R2 +, Linux çekirdek sürümü 4.11 +         |
|Kullanılabilir katmanlar     |Premium depolama         |Premium Depolama, standart depolama, sık erişimli ve seyrek erişimli         |
|Çoğaltma     |LRS         |LRS, ZRS, GRS         |
|Kimlik Doğrulaması     |Yalnızca ana bilgisayar tabanlı kimlik doğrulaması        |Kimlik tabanlı kimlik doğrulaması, Kullanıcı tabanlı kimlik doğrulaması         |
|İzinler     |UNIX stili izinler         |NTFS stili izinleri         |
|Dosya sistemi semantiği     |POSIX uyumlu         |POSIX uyumlu değil         |
|Büyük/küçük harf duyarlılığı     |Büyük/Küçük harfe duyarlı         |Büyük/küçük harfe duyarlı değil         |
|Sabit bağlantı desteği     |Desteklenir         |Desteklenmez         |
|Sembolik bağlantılar desteği     |Desteklenir         |Desteklenmez         |
|Açık dosyaları silme veya değiştirme     |Desteklenir         |Desteklenmez         |
|Kilitleme     |Bayt aralığı danışmanlık ağ kilidi Yöneticisi         |Desteklenir         |
|Genel IP güvenli listesi | Desteklenmez | Desteklenir|
|Protokol birlikte çalışma| Desteklenmez | FileREST|

## <a name="nfs-shares-preview"></a>NFS paylaşımları (Önizleme)

Azure dosya paylaşımlarını NFS 4,1 ile bağlama Şu anda önizleme aşamasındadır. Linux ile daha sıkı bir tümleştirme sunar. Bu, Unix ve diğer * Nix tabanlı işletim sistemlerinin çeşitleri arasında standart olan tam bir POSIX uyumlu tekliftir. Bu kurumsal sınıf dosya depolama hizmeti, depolama gereksinimlerinizi karşılayacak şekilde ölçeklenir ve binlerce işlem örneği tarafından aynı anda erişilebilir.

### <a name="limitations"></a>Sınırlamalar

[!INCLUDE [files-nfs-limitations](../../../includes/files-nfs-limitations.md)]

#### <a name="regional-availability"></a>Bölgesel kullanılabilirlik

[!INCLUDE [files-nfs-regional-availability](../../../includes/files-nfs-regional-availability.md)]

### <a name="best-suited"></a>En uygun

Azure dosyaları ile NFS için idealdir:

- POSIX ile uyumlu dosya paylaşımları, büyük/küçük harf duyarlılığı veya UNIX stil izinleri (UID/GID) gerektiren iş yükleri.
- SMB erişimi gerektirmeyen Linux merkezli iş yükleri.

### <a name="security"></a>Güvenlik

Tüm Azure dosyaları verileri bekleyen bir şekilde şifrelenir. Azure, aktarım sırasında şifreleme için [MACsec](https://en.wikipedia.org/wiki/IEEE_802.1AE)kullanarak Azure veri merkezleri arasında geçiş halindeki tüm veriler için bir şifreleme katmanı sunar. Bu arada, veriler Azure veri merkezleri arasında aktarıldığında şifreleme bulunur. SMB protokolünü kullanan Azure dosyalarının aksine, NFS protokolünü kullanan dosya paylaşımları Kullanıcı tabanlı kimlik doğrulaması sunmaz. NFS paylaşımları için kimlik doğrulaması, yapılandırılan ağ güvenliği kurallarına dayanır. Bu nedenle, NFS paylaşımınızda yalnızca güvenli bağlantıların kurulduğundan emin olmak için hizmet uç noktalarını veya özel uç noktaları kullanmanız gerekir. Şirket içinden paylaşımlara daha sonra erişmek istiyorsanız, özel bir uç noktaya ek olarak bir VPN veya ExpressRoute oluşturmanız gerekir. Aşağıdaki kaynaklardan kaynaklanmayan istekler reddedilir:

- [Özel bir uç nokta](storage-files-networking-overview.md#private-endpoints)
- [Azure VPN Gateway](../../vpn-gateway/vpn-gateway-about-vpngateways.md)
    - [Noktadan siteye (P2S) VPN](../../vpn-gateway/point-to-site-about.md)
    - [Siteden siteye](https://docs.microsoft.com/azure/vpn-gateway/design#s2smulti)
- [ExpressRoute](../../expressroute/expressroute-introduction.md)
- [Kısıtlanmış bir genel uç nokta](storage-files-networking-overview.md#storage-account-firewall-settings)

Kullanılabilir ağ seçenekleri hakkında daha fazla bilgi için bkz. [Azure dosyaları ağ iletişimi konuları](storage-files-networking-overview.md).

## <a name="smb-shares"></a>SMB paylaşımları

SMB ile bağlanmış Azure dosya paylaşımları, genel kullanıma sunulduğundan bu yana daha fazla Azure dosya özelliği sunar ve Azure dosyaları Özellik kısıtlamalarına sahip değildir.

### <a name="features"></a>Özellikler

- Azure dosya eşitleme
- Kimlik tabanlı kimlik doğrulaması
- Azure Backup desteği
- Anlık Görüntüler
- Geçici silme
- Geçiş sırasında şifreleme ve bekleyen şifreleme

### <a name="best-suited"></a>En uygun

Azure dosyaları ile SMB, için idealdir:

- Üretim ortamları
- [Özelliklerde](#features) listelenen özelliklerden herhangi birini gerektiren müşteriler

## <a name="next-steps"></a>Sonraki adımlar

- [NFS dosya paylaşma oluşturma](storage-files-how-to-create-nfs-shares.md)
- [SMB dosya paylaşma oluşturma](storage-how-to-create-file-share.md)