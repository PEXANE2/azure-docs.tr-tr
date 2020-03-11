---
title: Yalıtılmış bir ağda Azure disk şifrelemesi
description: Bu makalede, Linux VM 'Leri için Microsoft Azure disk şifrelemesi için sorun giderme ipuçları sunulmaktadır.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 02/27/2020
ms.custom: seodec18
ms.openlocfilehash: aa0dc204a017e2d40eb3952a9ede0755127f8de2
ms.sourcegitcommit: 5f39f60c4ae33b20156529a765b8f8c04f181143
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "78970671"
---
# <a name="azure-disk-encryption-on-an-isolated-network"></a>Yalıtılmış bir ağda Azure disk şifrelemesi

Bağlantı bir güvenlik duvarı, proxy gereksinim veya ağ güvenlik grubu (NSG) ayarları tarafından kısıtlanmış, görevleri gerçekleştirmek için uzantının özelliği kesilmiş olabilir. Durum iletileri "Uzantı durumu sanal makinede kullanılabilir değil." gibi bu kesintiye neden olabilir

## <a name="package-management"></a>Paket yönetimi

Azure disk şifrelemesi, genellikle zaten mevcut değilse, daha önce ADE etkinleştirme 'nin bir parçası olarak yüklenen bir dizi bileşene bağlıdır. Bir güvenlik duvarının arkasında veya Internet 'ten yalıtılmış olduğunda, bu paketlerin önceden yüklenmiş veya yerel olarak kullanılabilir olması gerekir.

Her dağıtım için gereken paketler aşağıda verilmiştir. Desteklenen destekler ve birim türlerinin tam listesi için bkz. [desteklenen VM 'ler ve işletim sistemleri](disk-encryption-overview.md#supported-vms-and-operating-systems).

- **Ubuntu 14,04, 16,04, 18,04**: lsscsı, psmisc, at, cryptsetup-bin, Python-partalı, Python-altı, PROCPS
- **CentOS 7,2-7,7**: lsscsı, psmisc, lvm2, UUID, at, Patch, cryptsetup, cryptsetup-reencrypt, pyparted, PROCPS-ng, Util-Linux
- **CentOS 6,8**: lsscsı, psmisc, lvm2, UUID, at, cryptsetup-reencrypt, pyparted, Python-altı
- **RedHat 7,2-7,7**: lsscsı, psmisc, lvm2, UUID, at, Patch, cryptsetup, cryptsetup-reencrypt, PROCPS-ng, Util-Linux
- **RedHat 6,8**: lsscsı, psmisc, lvm2, UUID, at, Patch, cryptsetup-reencrypt
- **openSUSE 42,3, SLES 12-SP4, 12-SP3**: lsscsı, cryptsetup

Bir proxy gerektiğinde Red Hat üzerinde size Abonelik Yöneticisi ve yum düzgün ayarlandığından emin olmanız gerekir. Daha fazla bilgi için bkz. [abonelik-yönetici ve sorun giderme sorunları nasıl giderilir](https://access.redhat.com/solutions/189533).  

Paketler el ile yüklendiğinde, yeni sürümler yayımlandıklarında ayrıca el ile yükseltilmelidir.

## <a name="network-security-groups"></a>Ağ güvenlik grupları
Uygulanan tüm ağ güvenlik grubu ayarları, uç noktanın disk şifrelemesi için belgelenen ağ yapılandırması önkoşullarını karşıladığından hala izin vermelidir.  Bkz [. Azure disk şifrelemesi: ağ gereksinimleri](disk-encryption-overview.md#networking-requirements)

## <a name="azure-disk-encryption-with-azure-ad-previous-version"></a>Azure AD ile Azure disk şifrelemesi (önceki sürüm)

Azure [ad (önceki sürüm) Ile Azure disk şifrelemesi](disk-encryption-overview-aad.md)kullanıyorsanız, tüm kaldırmalar Için [Azure Active Directory kitaplığının](../../active-directory/azuread-dev/active-directory-authentication-libraries.md) el ile yüklenmesi gerekir ( [yukarıda listelenen](#package-management)bir dağıtım için uygun paketlere ek olarak).

Şifreleme, [Azure AD kimlik bilgileriyle](disk-encryption-linux-aad.md)etkinleştirildiğinde, hedef VM hem Azure Active Directory uç noktalarına hem de Key Vault uç noktalarına bağlantı kurulmasına izin vermelidir. Geçerli Azure Active Directory kimlik doğrulama uç noktaları, [Office 365 URL 'leri ve IP adresi aralıkları](https://docs.microsoft.com/office365/enterprise/urls-and-ip-address-ranges) belgelerinin 56 ve 59 bölümlerinde saklanır. Key Vault yönergeler, [bir güvenlik duvarının arkasındaki Azure Key Vault erişme](../../key-vault/key-vault-access-behind-firewall.md)hakkındaki belgelerde sunulmaktadır.

### <a name="azure-instance-metadata-service"></a>Azure Instance Metadata Service 

Sanal makine, yalnızca VM içinden erişilebilen iyi bilinen yönlendirilemeyen IP adresini (`169.254.169.254`) kullanan [Azure örnek meta veri hizmeti](instance-metadata-service.md) uç noktasına erişebilmelidir.  Yerel HTTP trafiğini bu adrese dönüştüren ara sunucu (örneğin, X-Iletilmiş-for üstbilgisi ekleme) desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure disk şifrelemesi sorunlarını giderme](disk-encryption-troubleshooting.md) için daha fazla adım görüntüleyin
- [Bekleyen Azure veri şifrelemesi](../../security/fundamentals/encryption-atrest.md)
