---
title: Storage FSLogix profil kapsayıcısı Windows sanal masaüstü-Azure
description: Azure depolama 'da Windows sanal masaüstü FSLogix profilinizi depolama seçenekleri.
author: Heidilohr
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 0b1a5e36232e74caa34037efbbb0da0c39051998
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89568702"
---
# <a name="storage-options-for-fslogix-profile-containers-in-windows-virtual-desktop"></a>Windows sanal masaüstündeki FSLogix profil kapsayıcıları için depolama seçenekleri

Azure, FSLogix profil kapsayıcınızı depolamak için kullanabileceğiniz birden çok depolama çözümleri sunar. Bu makalede, Azure 'un Windows sanal masaüstü FSLogix Kullanıcı profili kapsayıcıları için sunduğu depolama çözümleri karşılaştırılır. FSLogix profil kapsayıcılarını, müşterilerimizin çoğu için Azure dosyalarında depolamanızı öneririz.

Windows sanal masaüstü, önerilen Kullanıcı profili çözümü olarak FSLogix profil kapsayıcıları sunar. FSLogix, Windows Sanal Masaüstü gibi uzak bilgi işlem ortamlarında dolaşım profillerini etkinleştirme amacıyla tasarlanmıştır. Bu kapsayıcı, oturum açma sırasında, yerel olarak desteklenen bir sanal sabit disk (VHD) ve Hyper-V sanal sabit diski (VHDX) kullanılarak bilgi işlem ortamına dinamik olarak eklenir. Kullanıcı profili hemen kullanılabilir olur ve sistemde tam olarak yerel bir kullanıcı profili gibi görünür.

Aşağıdaki tablolar, Windows sanal masaüstü FSLogix profil kapsayıcısı Kullanıcı profilleri için Azure depolama tarafından sunulan depolama çözümlerini karşılaştırın.

## <a name="azure-platform-details"></a>Azure platformu ayrıntıları

|Özellikler|Azure Dosyaları|Azure NetApp Files|Doğrudan Depolama Alanları|
|--------|-----------|------------------|---------------------|
|Kullanım örneği|Genel amaçlı|Şirket içi NetApp 'ten ultra performans veya geçiş|Platformlar arası|
|Platform hizmeti|Evet, Azure-Native çözümü|Evet, Azure-Native çözümü|Hayır, otomatik olarak yönetilen|
|Bölgesel kullanılabilirlik|Tüm bölgeler|[Bölge seçin](https://azure.microsoft.com/global-infrastructure/services/?products=netapp&regions=all)|Tüm bölgeler|
|Yedeklilik|Yerel olarak yedekli/bölge-yedekli/coğrafi olarak yedekli|Yerel olarak yedekli|Yerel olarak yedekli/bölge-yedekli/coğrafi olarak yedekli|
|Katmanlar ve performans|Standart<br>Premium<br>Paylaşma başına 5 Gbps ile, paylaşma başına en fazla 100.000 IOPS, yaklaşık 3 ms gecikme|Standart<br>Premium<br>Ultra<br>Yaklaşık 1 ms gecikme süresi içinde birim başına 4,5 GB/sn 'ye kadar 320 KB (16K) ıOPS|Standart HDD: disk başına en çok 500 ıOPS<br>Standart SSD: disk başına en fazla 4k ıOPS<br>Premium SSD: disk başına en fazla 20.000 IOPS sınırı<br>Depolama Alanları Doğrudan için Premium diskler öneriyoruz|
|Kapasite|paylaşma başına 100 TiB|Her birim için 100 TiB, abonelik başına en fazla 12,5 PiB|Disk başına en fazla 32 TiB|
|Gerekli altyapı|En az paylaşma boyutu 1 GiB|Minimum kapasite havuzu 4 TiB, minimum birim boyutu 100 GiB|Azure IaaS (+ Cloud tanık) üzerinde iki VM veya disk 'siz ve maliyeti olan en az üç VM|
|Protokoller|SMB 2.1/3. ve REST|NFSv3, NFSv 4.1 (Önizleme), SMB 3. x/2. x|NFSv3, NFSv 4.1, SMB 3,1|

## <a name="azure-management-details"></a>Azure yönetim ayrıntıları

|Özellikler|Azure Dosyaları|Azure NetApp Files|Doğrudan Depolama Alanları|
|--------|-----------|------------------|---------------------|
|Access|Bulut, şirket içi ve karma (Azure dosya eşitleme)|Bulut, şirket içi (ExpressRoute aracılığıyla)|Bulut, şirket içi|
|Backup|Azure Backup anlık görüntü tümleştirmesi|Azure NetApp Files anlık görüntüler|Azure Backup anlık görüntü tümleştirmesi|
|Güvenlik ve uyumluluk|[Tüm Azure desteklenen sertifikaları](https://www.microsoft.com/trustcenter/compliance/complianceofferings)|ISO tamamlandı|[Tüm Azure desteklenen sertifikaları](https://www.microsoft.com/trustcenter/compliance/complianceofferings)|
|Azure Active Directory tümleştirmesi|[Yerel Active Directory ve Azure Active Directory Domain Services](https://docs.microsoft.com/azure/storage/files/storage-files-active-directory-overview)|[Azure Active Directory Domain Services ve yerel Active Directory](../azure-netapp-files/azure-netapp-files-faqs.md#does-azure-netapp-files-support-azure-active-directory)|Yalnızca yerel Active Directory veya Azure Active Directory Domain Services desteği|

Depolama yönteminizi seçtikten sonra, fiyatlandırma planlarımız hakkında bilgi edinmek için [Windows sanal masaüstü fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-desktop/) ' na göz atın.

## <a name="next-steps"></a>Sonraki adımlar

FSLogix profil kapsayıcıları, Kullanıcı profili diskleri ve diğer Kullanıcı profili teknolojileri hakkında daha fazla bilgi edinmek için [Fslogix profil kapsayıcıları ve Azure dosyaları](fslogix-containers-azure-files.md)içindeki tabloya bakın.

Kendi FSLogix profil Kapsayıcılarınızı oluşturmaya hazırsanız, şu öğreticilerden birini kullanmaya başlayın:

- [Windows sanal masaüstündeki Azure dosyalarında FSLogix profil kapsayıcılarıyla çalışmaya başlama](create-file-share.md)
- [Azure NetApp dosyalarını kullanarak bir konak havuzu için FSLogix profil kapsayıcısı oluşturma](create-fslogix-profile-container.md)
- [Azure 'DA UPD depolama için iki düğümlü depolama alanları doğrudan genişleme dosya sunucusu dağıtma](/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment/) ' daki yönergeler, bir kullanıcı profili diski yerine FSLogix profil kapsayıcısını kullandığınızda da geçerlidir.

Ayrıca, [Windows sanal masaüstü 'nde kiracı oluşturma bölümünde](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md), tek başına başlayıp kendi Windows sanal masaüstü çözümünüzü ayarlayabilirsiniz.
