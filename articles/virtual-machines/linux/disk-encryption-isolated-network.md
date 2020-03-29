---
title: Yalıtılmış bir ağda Azure Disk Şifreleme
description: Bu makalede, Linux VM'leri için Microsoft Azure Disk Şifrelemesi için sorun giderme ipuçları verilmektedir.
author: msmbaldwin
ms.service: virtual-machines-linux
ms.subservice: security
ms.topic: article
ms.author: mbaldwin
ms.date: 02/27/2020
ms.custom: seodec18
ms.openlocfilehash: aa0dc204a017e2d40eb3952a9ede0755127f8de2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78970671"
---
# <a name="azure-disk-encryption-on-an-isolated-network"></a>Yalıtılmış bir ağda Azure Disk Şifreleme

Bağlantı bir güvenlik duvarı, proxy gereksinimi veya ağ güvenlik grubu (NSG) ayarlarıyla kısıtlandığında, uzantının gerekli görevleri gerçekleştirme yeteneği kesintiye uğrayabilir. Bu bozulma, "VM'de uzantı durumu yok" gibi durum iletilerine neden olabilir.

## <a name="package-management"></a>Paket yönetimi

Azure Disk Şifrelemesi, genellikle zaten mevcut değilse ADE etkinleştirme sinin bir parçası olarak yüklenen bir dizi bileşene bağlıdır. Bir güvenlik duvarının arkasında veya Internet'ten başka bir şekilde izole edildiğinde, bu paketler önceden yüklenmiş veya yerel olarak kullanılabilir olmalıdır.

Burada her dağıtım için gerekli paketler vardır. Desteklenen dağıtımve hacim türlerinin tam listesi için [desteklenen VM'lere ve işletim sistemlerine](disk-encryption-overview.md#supported-vms-and-operating-systems)bakın.

- **Ubuntu 14.04, 16.04, 18.04**: lsscsi, psmisc, at, cryptsetup-bin, python-parted, python-altı, procps
- **CentOS 7.2 - 7.7**: lsscsi, psmisc, lvm2, uuid, at, yama, cryptsetup, cryptsetup-reencrypt, pyparted, procps-ng, util-linux
- **CentOS 6.8**: lsscsi, psmisc, lvm2, uuid, at, cryptsetup-reencrypt, pyparted, python-altı
- **RedHat 7.2 - 7.7**: lsscsi, psmisc, lvm2, uuid, at, yama, cryptsetup, cryptsetup-reencrypt, procps-ng, util-linux
- **RedHat 6.8**: lsscsi, psmisc, lvm2, uuid, at, yama, cryptsetup-reencrypt
- **openSUSE 42.3, SLES 12-SP4, 12-SP3**: lsscsi, cryptsetup

Red Hat'te, bir proxy gerektiğinde, abonelik yöneticisi nin ve yum'un düzgün ayarlandıkettiğinden emin olmalısınız. Daha fazla bilgi [için, abonelik yöneticisi ve yum sorunlarını nasıl giderin.](https://access.redhat.com/solutions/189533)  

Paketler el ile yüklendiğinde, yeni sürümler yayımlandıkça da el ile yükseltilmelidir.

## <a name="network-security-groups"></a>Ağ güvenlik grupları
Uygulanan tüm ağ güvenliği grubu ayarları, bitiş noktasının disk şifrelemesi için belgelenmiş ağ yapılandırma ön koşullarıyla karşılayabilmesine izin vermelidir.  Bkz. [Azure Disk Şifrelemesi: Ağ gereksinimleri](disk-encryption-overview.md#networking-requirements)

## <a name="azure-disk-encryption-with-azure-ad-previous-version"></a>Azure AD ile Azure Disk Şifreleme (önceki sürüm)

Azure [AD (önceki sürüm) ile Azure Disk Şifrelemesi](disk-encryption-overview-aad.md)kullanıyorsanız, [Azure Etkin Dizin Kitaplığı'nın](../../active-directory/azuread-dev/active-directory-authentication-libraries.md) tüm dağıtımlar için el ile yüklenmesi gerekir [(yukarıda listelendirildikçe](#package-management)dağıtıma uygun paketlere ek olarak).

Azure AD kimlik [bilgileriyle](disk-encryption-linux-aad.md)şifreleme etkinleştirilirken, hedef VM hem Azure Etkin Dizin uç noktalarına hem de Key Vault uç noktalarına bağlantı sağlar. Geçerli Azure Etkin Dizin kimlik doğrulama bitiş [noktaları, Office 365 URL'lerinin](https://docs.microsoft.com/office365/enterprise/urls-and-ip-address-ranges) 56 ve 59. Anahtar Vault yönergeleri, [güvenlik duvarının arkasındaki Azure Anahtar Kasası'na](../../key-vault/key-vault-access-behind-firewall.md)nasıl erişilene ilişkin belgelerde verilmiştir.

### <a name="azure-instance-metadata-service"></a>Azure Örneği Meta veri Hizmeti 

Sanal makine, yalnızca VM içinden erişilebilen, iyi bilinen, routable olmayan bir`169.254.169.254`IP adresi () kullanan [Azure Örneği Meta veri hizmeti](instance-metadata-service.md) bitiş noktasına erişebilmeli.  Bu adrese yerel HTTP trafiğini değiştiren proxy yapılandırmaları (örneğin, X-Forwarded-For üstbilgisi eklemek) desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

- Azure disk [şifreleme sorun giderme](disk-encryption-troubleshooting.md) için daha fazla adım alabiliyorum
- [Azure veri şifrelemesi istirahatte](../../security/fundamentals/encryption-atrest.md)
