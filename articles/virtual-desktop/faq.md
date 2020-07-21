---
title: Windows sanal masaüstü hakkında SSS-Azure
description: Windows sanal masaüstü için sık sorulan sorular ve en iyi uygulamalar.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: b7ab9e63bfe92967eca22b60dceec0de882768a6
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86532456"
---
# <a name="windows-virtual-desktop-faq"></a>Windows sanal masaüstü hakkında SSS

Bu makalede sık sorulan sorular yanıtlanmaktadır ve Windows sanal masaüstü için en iyi yöntemler açıklanmaktadır.

## <a name="what-are-the-minimum-admin-permissions-i-need-to-manage-objects"></a>Nesneleri yönetmem gereken en düşük yönetici izinleri nelerdir?

Konak havuzları ve diğer nesneler oluşturmak istiyorsanız, üzerinde çalıştığınız abonelikte veya kaynak grubunda katkıda bulunan rolünün atanması gerekir.

Uygulama gruplarını kullanıcılara veya Kullanıcı gruplarına yayımlamak için, bir uygulama grubunda Kullanıcı erişimi Yöneticisi rolüne atanmalısınız.

Bir yöneticinin kullanıcılara ileti gönderme, kullanıcıları imzalama vb. gibi yalnızca kullanıcı oturumlarını yönetmesine izin vermek için özel roller oluşturabilirsiniz. Örneğin: 

```powershell
"actions": [
"Microsoft.Resources/deployments/operations/read",
"Microsoft.Resources/tags/read",
"Microsoft.Authorization/roleAssignments/read",
"Microsoft.DesktopVirtualization/hostpools/sessionhosts/usersessions/read",
"Microsoft.DesktopVirtualization/hostpools/sessionhosts/read",
"Microsoft.DesktopVirtualization/hostpools/sessionhosts/write",
"Microsoft.DesktopVirtualization/hostpools/sessionhosts/usersessions/write",
"Microsoft.DesktopVirtualization/hostpools/sessionhosts/usersessions/delete"
],
"notActions": [],
"dataActions": [],
"notDataActions": []
}
```

## <a name="does-windows-virtual-desktop-support-split-azure-active-directory-models"></a>Windows sanal masaüstü Azure Active Directory modellerini bölmeyi destekliyor mu?

Bir Kullanıcı bir uygulama grubuna atandığında, hizmet basit bir Azure rol tabanlı erişim denetimi (RBAC) rol ataması yapar. Sonuç olarak, kullanıcının Azure Active Directory (AD) ve uygulama grubunun Azure AD aynı konumda olmalıdır. Konak havuzları, uygulama grupları ve çalışma alanları gibi tüm hizmet nesneleri aynı zamanda kullanıcıyla aynı Azure AD içinde olmalıdır.

Aynı sanal ağdaki (VNET) Kullanıcı Azure AD ile Active Directory eşitlemedikçe, farklı bir Azure AD 'de sanal makineler (VM 'Ler) oluşturabilirsiniz.

Azure Dıthouse, Windows sanal masaüstü ortamının yönetimini tam olarak desteklemez. Açık oturum kullanımı Şu anda Azure AD Kiracı Kullanıcı yönetimini desteklemediğinden, hafif kullanım müşterileri, müşterilerin kullanıcıları yönetmek için kullandığı Azure AD 'de oturum açması gerekir.

## <a name="what-are-location-restrictions"></a>Konum kısıtlamaları nelerdir?

Tüm hizmet kaynaklarının kendileriyle ilişkili bir konumu vardır. Konak havuzunun konumu, konak havuzu için hizmet meta verilerinin hangi Coğrafya 'da depolandığını belirler. Bir uygulama grubu, bir konak havuzu olmadan bulunamaz. Bir RemoteApp uygulama grubuna uygulamalar eklerseniz, Başlat menüsü uygulamalarını tespit etmek için bir oturum ana bilgisayarına da ihtiyacınız olacaktır. Herhangi bir uygulama grubu eylemi için konak havuzunda ilgili bir veri erişimine de ihtiyacınız olacaktır. Verilerin birden çok konum arasında aktarılmadığından emin olmak için, uygulama grubunun konumu konak havuzunun ile aynı olmalıdır.

Çalışma alanları da uygulama gruplarıyla aynı konumda olmalıdır. Çalışma alanı her güncelleştirildiğinde ilgili uygulama grubu onunla birlikte güncelleştirilir. Uygulama grupları gibi, hizmet tüm çalışma alanlarının aynı konumda oluşturulan uygulama gruplarıyla ilişkilendirilmesini gerektirir.

## <a name="how-do-you-expand-an-objects-properties-in-powershell"></a>PowerShell 'de bir nesnenin özelliklerini nasıl genişletirsiniz?

Bir PowerShell cmdlet 'ini çalıştırdığınızda yalnızca kaynak adını ve konumunu görürsünüz.

Örneğin:

```powershell
Get-AzWvdHostPool -Name 0224hp -ResourceGroupName 0224rg

Location Name   Type
-------- ----   ----
westus   0224hp Microsoft.DesktopVirtualization/hostpools
```

Bir kaynağın tüm özelliklerini görmek için `format-list` cmdlet 'inin sonuna veya sonuna ekleyin `fl` .

Örneğin:

```powershell
Get-AzWvdHostPool -Name 0224hp -ResourceGroupName 0224rg |fl
```

Belirli özellikleri görmek için, veya sonrasında belirli özellik adlarını ekleyin `format-list` `fl` .

Örneğin:

```powershell
Get-AzWvdHostPool -Name demohp -ResourceGroupName 0414rg |fl CustomRdpProperty

CustomRdpProperty : audiocapturemode:i:0;audiomode:i:0;drivestoredirect:s:;redirectclipboard:i:1;redirectcomports:i:0;redirectprinters:i:1;redirectsmartcards:i:1;screen modeid:i:2;
```

## <a name="does-windows-virtual-desktop-support-guest-users"></a>Windows sanal masaüstü, Konuk kullanıcıları destekliyor mu?

Windows sanal masaüstü, Azure AD Konuk Kullanıcı hesaplarını desteklemez. Örneğin, bir grup Konuk kullanıcının kendi şirketindeki Kullanıcı başına Microsoft 365 E3, Windows E3 veya WIN VDA lisanslarına sahip olduğunu, ancak farklı bir şirketin Azure AD 'de Konuk kullanıcılar olduğunu varsayalım. Diğer şirket, Azure AD 'de konuk kullanıcıların kullanıcı nesnelerini ve yerel hesaplar gibi Active Directory yönetir.

Üçüncü bir tarafın avantajı için kendi lisanslarınızı kullanamazsınız. Ayrıca, Windows sanal masaüstü Şu anda Microsoft hesabı 'nı (MSA) desteklememektedir.

## <a name="why-dont-i-see-the-client-ip-address-in-the-wvdconnections-table"></a>WVDConnections tablosunda istemci IP adresini neden görmüyorum?

Şu anda web istemcisinin IP adreslerini toplamanın güvenilir bir yolu yoktur, bu nedenle tabloya bu değeri dahil etmemiz gerekir.

## <a name="how-does-windows-virtual-desktop-handle-backups"></a>Windows sanal masaüstü yedeklemeleri nasıl işler?

Azure 'da yedeklemenin yönetilmesi için birden çok seçenek vardır. Azure Backup, Site Recovery ve anlık görüntülerini kullanabilirsiniz.

## <a name="does-windows-virtual-desktop-support-third-party-collaboration-apps"></a>Windows sanal masaüstü üçüncü taraf işbirliği uygulamalarını destekliyor mu?

Windows sanal masaüstü Şu anda takımlar için iyileştirildi. Microsoft şu anda yakınlaştırma gibi üçüncü taraf işbirliği uygulamalarını desteklemez. Üçüncü taraf kuruluşlar müşterilerine uyumluluk yönergeleri vermekten sorumludur. Windows sanal masaüstü Ayrıca Skype Kurumsal 'ı desteklemez.

## <a name="can-i-change-from-pooled-to-personal-host-pools"></a>Havuza alınmış olarak kişisel ana bilgisayar havuzlarından değiştirilebilir miyim?

Bir konak havuzu oluşturduktan sonra, türünü değiştiremezsiniz. Ancak, bir konak havuzuna kayıt yaptığınız tüm VM 'Leri farklı bir konak havuzu türüne taşıyabilirsiniz.

## <a name="whats-the-largest-profile-size-fslogix-can-handle"></a>FSLogix 'in işleyebileceği en büyük profil boyutu nedir?

FSLogix 'teki sınırlamalar veya kotalar, Kullanıcı profili VHD (X) dosyalarını depolamak için kullanılan depolama dokusuna bağımlıdır.

Aşağıdaki tabloda, bir FSLogix profilinin her kullanıcıyı desteklemesi için gereken kaynakların nasıl yapılacağı hakkında bir örnek verilmiştir. Gereksinimler, her profildeki Kullanıcı, uygulama ve etkinliğe bağlı olarak farklılık gösterebilir. 

| Kaynak | Gereksinim |
|---|---|
| Sabit durum ıOPS 'si | 10 |
| Oturum açma/kapatma ıOPS | 5 |

Bu tablodaki örnek tek bir kullanıcıdır, ancak ortamınızdaki Toplam Kullanıcı sayısına ilişkin gereksinimleri tahmin etmek için kullanılabilir. Örneğin, 100 Kullanıcı için 1.000 ıOPS ve oturum açma ve oturum kapatma sırasında 5.000 ıOPS 'nin etrafında ihtiyacınız vardır.

## <a name="is-there-a-scale-limit-for-host-pools-created-in-the-azure-portal"></a>Azure portal oluşturulan konak havuzları için ölçek sınırı var mı?

Bu faktörler, konak havuzları için ölçek sınırını etkileyebilir:

- Azure şablonu 800 nesneleriyle sınırlıdır. Daha fazla bilgi edinmek için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md#template-limits). Her VM altı nesne de oluşturur; bu sayede, şablonu her çalıştırdığınızda 132 VM 'nin etrafında oluşturabileceğiniz anlamına gelir.

- Her bölge için ve abonelik başına kaç çekirdekli çekirdek oluşturabileceğiniz konusunda kısıtlamalar vardır. Örneğin, bir Kurumsal Anlaşma aboneliğiniz varsa, 350 çekirdek oluşturabilirsiniz. Şablonu her çalıştırdığınızda oluşturabileceğiniz VM 'Lerin sayısını belirleyebilmek için, VM başına varsayılan çekirdek sayısı veya kendi çekirdek sınırınıza göre 350 ' ı bölmeniz gerekir. [Sanal makine limitleriyle](../azure-resource-manager/management/azure-subscription-service-limits.md#virtual-machines-limits---azure-resource-manager)daha fazla bilgi edinin-Azure Resource Manager.

- VM ön eki adı ve VM sayısı 15 karakterden daha azdır. Daha fazla bilgi için bkz. [Azure kaynakları Için adlandırma kuralları ve kısıtlamaları](../azure-resource-manager/management/resource-name-rules.md#microsoftcompute).