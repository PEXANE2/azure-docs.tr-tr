---
title: Windows Sanal Masaüstü FSLogix profil kapsayıcıları dosyaları - Azure
description: Bu makalede, Windows Sanal Masaüstü ve Azure dosyaları içindeki FSLogix profil kapsayıcıları açıklanmaktadır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 1dc5d54fa24217c91e14a8f37e092888b2bb6474
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127885"
---
# <a name="fslogix-profile-containers-and-azure-files"></a>FSLogix profil kapsayıcıları ve Azure dosyaları

Windows Sanal Masaüstü hizmeti, FSLogix profil kaplarını kullanıcı profili çözümü olarak önerir. FSLogix, Windows Sanal Masaüstü gibi uzak bilgi işlem ortamlarında profillerde gezinmek üzere tasarlanmıştır. Tüm kullanıcı profilini tek bir kapta saklar. Oturum açma sırasında, bu kapsayıcı yerel olarak desteklenen Sanal Sabit Disk (VHD) ve Hyper-V Virtual Hard disk (VHDX) kullanılarak bilgisayar ortamına dinamik olarak bağlanır. Kullanıcı profili hemen kullanılabilir ve sistemde tam olarak yerel bir kullanıcı profili gibi görünür. Bu makalede, Windows Sanal Masaüstü'nde Azure Dosyaları ile kullanılan FSLogix profil kapsayıcılarının nasıl çalıştığı açıklanmaktadır.

>[!NOTE]
>Azure'daki farklı FSLogix Profil Kapsayıcısı depolama seçenekleri yle ilgili karşılaştırma malzemesi arıyorsanız, [FSLogix profil kapsayıcıları için Depolama seçeneklerine](store-fslogix-profile.md)bakın.

## <a name="user-profiles"></a>Kullanıcı profilleri

Kullanıcı profili, masaüstü ayarları, kalıcı ağ bağlantıları ve uygulama ayarları gibi yapılandırma bilgileri de dahil olmak üzere bir kişi hakkında veri öğeleri içerir. Varsayılan olarak, Windows işletim sistemiyle sıkı bir şekilde tümleşik yerel bir kullanıcı profili oluşturur.

Uzak kullanıcı profili, kullanıcı verileri ve işletim sistemi arasında bir bölüm sağlar. Kullanıcı verilerini etkilemeden işletim sisteminin değiştirilmesini veya değiştirilmesine olanak tanır. Uzak Masaüstü Oturum Ana Bilgisayar (RDSH) ve Sanal Masaüstü Altyapılarında (VDI) işletim sistemi aşağıdaki nedenlerle değiştirilebilir:

- İşletim sisteminin yükseltilmesi
- Varolan bir Sanal Makinenin (VM) değiştirilmesi
- Bir havuz (kalıcı olmayan) RDSH veya VDI ortamının parçası olan bir kullanıcı

Microsoft ürünleri, bu teknolojiler de dahil olmak üzere uzak kullanıcı profilleri için çeşitli teknolojilerle çalışır:
- Dolaşım kullanıcı profilleri (RUP)
- Kullanıcı profil diskleri (UPD)
- Kurumsal durum dolaşımı (ESR)

UPD ve RUP Uzak Masaüstü Oturum Ana Bilgisayar (RDSH) ve Sanal Sabit Disk (VHD) ortamlarında kullanıcı profilleri için en yaygın olarak kullanılan teknolojilerdir.

### <a name="challenges-with-previous-user-profile-technologies"></a>Önceki kullanıcı profili teknolojileri ile ilgili zorluklar

Kullanıcı profilleri için mevcut ve eski Microsoft çözümleri çeşitli zorluklarla birlikte geldi. RDSH veya VDI ortamıyla birlikte gelen tüm kullanıcı profili gereksinimlerini önceki çözüm de kullanmaz. Örneğin, UPD büyük OST dosyalarını işleyemez ve RUP modern ayarları devam etmez.

#### <a name="functionality"></a>İşlev

Aşağıdaki tablo, önceki kullanıcı profili teknolojilerinin avantajlarını ve sınırlamalarını gösterir.

| Teknoloji | Modern ayarlar | Win32 ayarları | İşletim sistemi ayarları | Kullanıcı verileri | Sunucu SKU'da desteklendi | Azure'da arka uç depolama | Şirket içinde arka uç depolama | Sürüm desteği | Sonraki oturum açma süresi |Notlar|
| ---------- | :-------------: | :------------: | :---------: | --------: | :---------------------: | :-----------------------: | :--------------------------: | :-------------: | :---------------------: |-----|
| **Kullanıcı Profil Diskleri (UPD)** | Evet | Evet | Evet | Evet | Evet | Hayır | Evet | Win 7+ | Evet | |
| **Dolaşım Kullanıcı Profili (RUP), bakım modu** | Hayır | Evet | Evet | Evet | Evet| Hayır | Evet | Win 7+ | Hayır | |
| **Kurumsal Devlet Dolaşımı (ESR)** | Evet | Hayır | Evet | Hayır | Notlara bakın | Evet | Hayır | 10 kazan | Hayır | Sunucu SKU'daki işlevler ancak destekleyici kullanıcı arabirimi yok |
| **Kullanıcı Deneyimi Sanallaştırma (UE-V)** | Evet | Evet | Evet | Hayır | Evet | Hayır | Evet | Win 7+ | Hayır |  |
| **OneDrive bulut dosyaları** | Hayır | Hayır | Hayır | Evet | Notlara bakın | Notlara bakın  | Notlara Bakın | 10 RS3 kazanın | Hayır | Sunucu SKU üzerinde test edilmemiş. Azure'da arka uç depolama, eşitleme istemcisi'ne bağlıdır. Prem'de arka uç depolamanın bir eşitleme istemcisi gerekir. |

#### <a name="performance"></a>Performans

UPD, performans gereksinimlerini karşılamak için [Depolama Alanları Doğrudan (S2D)](/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment/) gerektirir. UPD, Sunucu İleti Bloğu (SMB) protokolünü kullanır. Profili, kullanıcının günlüğe kaydedildiği VM'ye kopyalar. S2D ile UPD, Windows Sanal Masaüstü için önerdiğimiz çözümdür.  

#### <a name="cost"></a>Maliyet

S2D kümeleri gerekli performansı elde ederken, maliyet kurumsal müşteriler için pahalıdır, ancak özellikle küçük ve orta ölçekli işletmeler (Kobİ) müşterileri için pahalıdır. Bu çözüm için işletmeler, diskleri pay için kullanan VM'lerin maliyetiyle birlikte depolama diskleri için ödeme de bulunur.

#### <a name="administrative-overhead"></a>Yönetim yükü

S2D kümeleri, yamalı, güncelleştirilen ve güvenli bir durumda tutulan bir işletim sistemi gerektirir. Bu süreçler ve S2D olağanüstü durum kurtarma kurma karmaşıklığı S2D'yi yalnızca özel bt personeli olan işletmeler için uygulanabilir hale getirmektedir.

## <a name="fslogix-profile-containers"></a>FSLogix profil kapları

19 Kasım 2018'de [Microsoft FSLogix'i satın aldı.](https://blogs.microsoft.com/blog/2018/11/19/microsoft-acquires-fslogix-to-enhance-the-office-365-virtualization-experience/) FSLogix birçok profil konteyner zorlukları giderme. Bunların anahtarı şunlardır:

- **Performans:** [FSLogix profil kapsayıcıları](/fslogix/configure-profile-container-tutorial/) yüksek performansve geçmiş olarak önbelleğe alınmış exchange modunu engelleyen performans sorunlarını giderir.
- **OneDrive:** FSLogix profil kapsayıcıları olmadan, OneDrive for Business kalıcı olmayan RDSH veya VDI ortamlarında desteklenmez. [OneDrive for Business ve FSLogix en iyi uygulamaları](/fslogix/overview/) nasıl etkileşimde olduklarını açıklar. Daha fazla bilgi için bkz. [Sanal masaüstü bilgisayarlarda eşitleme istemcisini kullanın.](/deployoffice/rds-onedrive-business-vdi/)
- **Ek klasörler:** FSLogix, kullanıcı profillerini ek klasörler içerecek şekilde genişletme olanağı sağlar.

Satın alma dan bu yana, Microsoft FSLogix profil kapsayıcıları ile UPD gibi mevcut kullanıcı profili çözümleri değiştirmeye başladı.

## <a name="azure-files-integration-with-azure-active-directory-domain-service"></a>Azure Active Directory Etki Alanı Hizmeti ile Azure Dosyaları entegrasyonu

FSLogix profil konteynerlerinin performansı ve özellikleri buluttan yararlanır. 7 Ağustos 2019'da Microsoft Azure [Dosyaları, Azure Etkin Dizin Etki Alanı Hizmeti (AD DS) ile Azure Dosyaları kimlik doğrulamasının](../storage/files/storage-files-active-directory-overview.md)genel kullanılabilirliğini duyurdu. Azure AD DS Kimlik Doğrulamalı Azure Dosyaları, hem maliyet hem de yönetim ek yüküne değinerek, Windows Sanal Masaüstü hizmetindeki kullanıcı profilleri için birinci sınıf bir çözümdür.

## <a name="best-practices-for-windows-virtual-desktop"></a>Windows Sanal Masaüstü için en iyi uygulamalar

Windows Sanal Masaüstü, müşteriler tarafından kullanılan VM'lerin boyutu, türü ve sayısı üzerinde tam denetim sağlar. Daha fazla bilgi için [Windows Sanal Masaüstü nedir?](overview.md)

Windows Sanal Masaüstü ortamınızın aşağıdakileri takip etmesini sağlamak için:

- Azure Files depolama hesabı, oturum ana bilgisayar VM'ler ile aynı bölgede olmalıdır.
- Azure Dosyaları [izinleri, Gereksinimler - Profil](/fslogix/fslogix-storage-config-ht)Kapsayıcıları'nda açıklanan izinlerle eşleşmelidir.
- Her ana bilgisayar havuzu, aynı ana görüntüye dayalı olarak aynı tip ve boyutta VM'den oluşturulmalıdır.
- Her ana bilgisayar havuzu VM, yönetime, ölçekleme ve güncelleştirmeye yardımcı olmak için aynı kaynak grubunda olmalıdır.
- En iyi performans için depolama çözümü ve FSLogix profil kapsayıcısı aynı veri merkezi konumunda olmalıdır.
- Ana görüntüyü içeren depolama hesabı, VM'lerin sağlandığı aynı bölgede ve abonelikte olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Windows Sanal Masaüstü ortamı nı ayarlamak için aşağıdaki kılavuzları kullanın.

- Masaüstü sanallaştırma çözümünüzü oluşturmaya başlamak için windows [sanal masaüstünde kiracı oluşturma](tenant-setup-azure-active-directory.md)'ya bakın.
- Windows Sanal Masaüstü kiracınızda ana bilgisayar havuzu oluşturmak için [bkz.](create-host-pools-azure-marketplace.md)
- Bulutta tam olarak yönetilen dosya paylaşımlarını ayarlamak için Azure [Dosyalarını paylaş'ı ayarlama'ya](/azure/storage/files/storage-files-active-directory-enable/)bakın.
- FSLogix profil kapsayıcılarını yapılandırmak [için](create-host-pools-user-profile.md)bkz.
- Kullanıcıları ana bilgisayar havuzuna atamak [için Windows Sanal Masaüstü için uygulama gruplarını yönet'e](manage-app-groups.md)bakın.
- Windows Sanal Masaüstü kaynaklarınıza bir web tarayıcısından erişmek [için](connect-web.md)bkz.
