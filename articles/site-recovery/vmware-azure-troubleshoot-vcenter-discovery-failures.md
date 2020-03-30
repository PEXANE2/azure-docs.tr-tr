---
title: Azure Site Kurtarma'da VMware vCenter bulma hatalarını giderme
description: Bu makalede, Azure Site Kurtarma'da VMware vCenter bulma hatalarının nasıl giderilen sorun giderilen açıklanmaktadır.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: mayg
ms.openlocfilehash: f00c7b12accde9df9a5708a2b8b378d70428318d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74091248"
---
# <a name="troubleshoot-vcenter-server-discovery-failures"></a>sorun giderme vCenter Server bulma hataları

Bu makale, VMware vCenter bulma hataları nedeniyle oluşan sorunları gidermek için yardımcı olur.

## <a name="non-numeric-values-in-the-maxsnapshots-property"></a>MaxSnapShots özelliğinde sayısal olmayan değerler

9.20'den önceki sürümlerde vCenter, VM'deki özellik `snapshot.maxSnapShots` için sayısal olmayan bir değer aldığında bağlantısını keser.

Bu sorun hata kimliği 95126 ile tanımlanır.

    ERROR :: Hit an exception while fetching the required informationfrom vCenter/vSphere.Exception details:
    System.FormatException: Input string was not in a correct format.
       at System.Number.StringToNumber(String str, NumberStyles options, NumberBuffer& number, NumberFormatInfo info, Boolean parseDecimal)
       at System.Number.ParseInt32(String s, NumberStyles style, NumberFormatInfo info)
       at VMware.VSphere.Management.InfraContracts.VirtualMachineInfo.get_MaxSnapshots()
    
Sorunu gidermek için:

- VM'yi tanımlayın ve değeri sayısal bir değere ayarlayın (vCenter'daki VM Edit ayarları).

Veya

- Yapılandırma sunucunuzu sürüm 9.20 veya sonraki sürümlere yükseltin.

## <a name="proxy-configuration-issues-for-vcenter-connectivity"></a>vCenter bağlantısı için proxy yapılandırma sorunları

vCenter Discovery, Sistem kullanıcısı tarafından yapılandırılan Sistem varsayılan proxy ayarlarını onurlandırıyor. DRA hizmeti, birleştirilmiş kurulum yükleyicisini veya OVA şablonunu kullanarak yapılandırma sunucusunun yüklenmesi sırasında kullanıcı tarafından sağlanan proxy ayarlarını onurlandırMaktadır. 

Genel olarak, proxy ortak ağlara iletişim kurmak için kullanılır; Azure ile iletişim kurmak gibi. Proxy yapılandırılmış ve vCenter yerel bir ortamda ise, DRA ile iletişim kurmak mümkün olmayacaktır.

Bu sorunla karşılaşıldığında aşağıdaki durumlar oluşur:

- vCenter server \<vCenter> hata nedeniyle erişilemez: Uzak sunucu bir hata döndü: (503) Sunucu Kullanılamıyor
- vCenter server \<vCenter> hata nedeniyle erişilemez: Uzak sunucu bir hata döndü: Uzak sunucuya bağlanamıyor.
- vCenter/ESXi sunucusuna bağlanamıyor.

Sorunu gidermek için:

[PsExec aracını](https://aka.ms/PsExec)indirin. 

Sistem kullanıcı bağlamına erişmek ve proxy adresinin yapılandırılıp yapılandırılmadığını belirlemek için PsExec aracını kullanın. Daha sonra aşağıdaki yordamları kullanarak bypass listesine vCenter ekleyebilirsiniz.

Discovery proxy yapılandırması için:

1. PsExec aracını kullanarak sistem kullanıcı bağlamında IE'yi açın.
    
    psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe"

2. vCenter IP adresini atlamak için Internet Explorer'daki proxy ayarlarını değiştirin.
3. Tmanssvc hizmetini yeniden başlatın.

DRA proxy yapılandırması için:

1. Bir komut istemi açın ve Microsoft Azure Site Kurtarma Sağlayıcısı klasörünü açın.
 
    **cd C:\Program Files\Microsoft Azure Site Kurtarma Sağlayıcısı**

3. Komut isteminden aşağıdaki komutu çalıştırın.
   
   **DRCONFIGURATOR. EXE /configure /AddBypassUrls [vCenter Server'ın IP Adresi/FQDN vCenter ekle zamanında sağlanmıştır]**

4. DRA sağlayıcı hizmetini yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

[VMware VM olağanüstü durum kurtarma yapılandırma sunucusunu yönetme](https://docs.microsoft.com/azure/site-recovery/vmware-azure-manage-configuration-server#refresh-configuration-server) 
