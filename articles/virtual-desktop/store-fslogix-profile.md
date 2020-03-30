---
title: Depolama FSLogix profil konteynerI Windows Sanal Masaüstü - Azure
description: Windows Sanal Masaüstü FSLogix profilinizi Azure Depolama'da depolama seçenekleri.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 71ba24784dee7771acbe19bf0261c7dc02478b24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127524"
---
# <a name="storage-options-for-fslogix-profile-containers-in-windows-virtual-desktop"></a>Windows Virtual Desktop'da FSLogix profil kapları için depolama seçenekleri

Azure, FSLogix profil kapsayıcınızı depolamak için kullanabileceğiniz birden çok depolama çözümü sunar. Bu makalede, Azure'un Windows Sanal Masaüstü FSLogix kullanıcı profili kapsayıcıları için sunduğu depolama çözümleri karşılaştırılır.

Windows Sanal Masaüstü, önerilen kullanıcı profili çözümü olarak FSLogix profil kaplarını sunar. FSLogix, Windows Sanal Masaüstü gibi uzak bilgi işlem ortamlarında profillerde gezinmek üzere tasarlanmıştır. Oturum açma sırasında, bu kapsayıcı yerel olarak desteklenen Sanal Sabit Disk (VHD) ve Hyper-V Sanal Sabit Disk (VHDX) kullanılarak bilgisayar ortamına dinamik olarak bağlanır. Kullanıcı profili hemen kullanılabilir ve sistemde tam olarak yerel bir kullanıcı profili gibi görünür.

Aşağıdaki tablolar, Azure Depolama'nın Windows Virtual Desktop FSLogix profil kapsayıcısı kullanıcı profilleri için sunduğu depolama çözümlerini karşılaştırın.

## <a name="azure-platform-details"></a>Azure platform ayrıntıları

|Özellikler|Azure Dosyaları|Azure NetApp Files|Doğrudan Depolama Alanları|
|--------|-----------|------------------|---------------------|
|Kullanım örneği|Genel amaçlı|NetApp'ten ultra performans veya geçiş|Platformlar arası|
|Platform hizmeti|Evet, Azure'a özgü çözüm|Evet, Azure'a özgü çözüm|Hayır, kendi kendini idare etti.|
|Bölgesel kullanılabilirlik|Tüm bölgeler|[Bölgeleri seçin](https://azure.microsoft.com/global-infrastructure/services/?products=netapp&regions=all)|Tüm bölgeler|
|Yedeklilik|Yerel olarak yedekli/bölge-yedekli/coğrafi-yedekli|Yerel olarak gereksiz|Yerel olarak yedekli/bölge-yedekli/coğrafi-yedekli|
|Kademeler ve performans|Standart<br>Premium<br>Yaklaşık 3 ms gecikme de hisse başına 5 GBps ile hisse başına maksimum 100k IOPS'ye kadar|Standart<br>Premium<br>Ultra<br>Yaklaşık 1 ms gecikme de hacim başına 4,5 GBps ile 320k (16K) IOPS'ye kadar|Standart HDD: disk başına 500 IOPS limitine kadar<br>Standart SSD: disk başına 4k IOPS limitlerine kadar<br>Premium SSD: disk başına 20k IOPS limitlerine kadar<br>Doğrudan Depolama Alanları için Premium diskler öneririz|
|Kapasite|Hisse başına 100 TiB|Birim başına 100 TiB, abonelik başına 12,5 PiB'ye kadar|Disk başına maksimum 32 TiB|
|Gerekli altyapı|Minimum hisse boyutu 1 GiB|Minimum kapasite havuzu 4 TiB, min hacim boyutu 100 GiB|Azure IaaS'da iki VM (+ Bulut Tanığı) veya diskler için maliyet olmadan en az üç VM|
|Protokoller|SMB 2.1/3. ve REST|NFSv3, NFSv4.1 (önizleme), Kobİ 3.x/2.x|NFSv3, NFSv4.1, Kobİ 3.1|

## <a name="azure-management-details"></a>Azure yönetim ayrıntıları

|Özellikler|Azure Dosyaları|Azure NetApp Files|Doğrudan Depolama Alanları|
|--------|-----------|------------------|---------------------|
|Erişim|Bulut, şirket içi ve karma (Azure dosya eşitleme)|Bulut, şirket içi (ExpressRoute üzerinden)|Bulut, şirket içi|
|Backup|Azure yedekleme anlık tümleştirmesi|Azure NetApp Dosyaları anlık görüntüleri|Azure yedekleme anlık tümleştirmesi|
|Güvenlik ve uyumluluk|[Tüm Azure destekli sertifikalar](https://www.microsoft.com/trustcenter/compliance/complianceofferings)|ISO tamamlandı|[Tüm Azure destekli sertifikalar](https://www.microsoft.com/trustcenter/compliance/complianceofferings)|
|Azure Active Directory tümleştirmesi|[Yerel Etkin Dizin ve Azure Etkin Dizin Etki Alanı Hizmetleri](https://docs.microsoft.com/azure/storage/files/storage-files-active-directory-overview)|[Azure Active Directory Etki Alanı Hizmetleri ve Yerel Etkin Dizin](../azure-netapp-files/azure-netapp-files-faqs.md#does-azure-netapp-files-support-azure-active-directory)|Yalnızca Yerel Etkin Dizin veya Azure Etkin Dizin Etki Alanı Hizmetleri desteği|

Depolama yönteminizi seçtikten sonra, fiyatlandırma planlarımız hakkında bilgi için [Windows Sanal Masaüstü fiyatlandırmasına](https://azure.microsoft.com/pricing/details/virtual-desktop/) göz atın.

## <a name="next-steps"></a>Sonraki adımlar

FSLogix profil kapsayıcıları, kullanıcı profil diskleri ve diğer kullanıcı profili teknolojileri hakkında daha fazla bilgi edinmek için [FSLogix profil kaplarında ve Azure dosyalarındaki](fslogix-containers-azure-files.md)tabloya bakın.

Kendi FSLogix profil kaplarınızı oluşturmaya hazırsanız, şu eğitimlerden biriyle başlayın:

- [Windows Sanal Masaüstünde Azure Dosyalarındaki FSLogix profil kapsayıcılarıyla başlarken](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Getting-started-with-FSLogix-profile-containers-on-Azure-Files/ba-p/746477)
- [Azure NetApp dosyalarını kullanarak ana bilgisayar havuzu için FSLogix profil kapsayıcısı oluşturma](create-fslogix-profile-container.md)
- [Azure'da UPD depolama için iki düğümlü Depolama Alanlarını Dağıt'taki](/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment/) yönergeler Kullanıcı profil diski yerine FSLogix profil kapsayıcısı kullandığınızda da geçerlidir

Ayrıca en başından itibaren başlayabilir ve [Windows Sanal Masaüstü'nde kiracı oluşturun'da](tenant-setup-azure-active-directory.md)kendi Windows Sanal Masaüstü çözümünüzü kurabilirsiniz.
