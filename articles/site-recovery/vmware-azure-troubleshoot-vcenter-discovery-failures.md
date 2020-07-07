---
title: Azure Site Recovery 'de VMware vCenter bulma hatalarıyla ilgili sorunları giderme
description: Bu makalede Azure Site Recovery 'de VMware vCenter bulma hatalarının nasıl giderileceği açıklanır.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: mayg
ms.openlocfilehash: f00c7b12accde9df9a5708a2b8b378d70428318d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74091248"
---
# <a name="troubleshoot-vcenter-server-discovery-failures"></a>vCenter Server bulma hatalarıyla ilgili sorunları giderme

Bu makale, VMware vCenter bulma hataları nedeniyle oluşan sorunları gidermenize yardımcı olur.

## <a name="non-numeric-values-in-the-maxsnapshots-property"></a>MaxSnapShots özelliğindeki sayısal olmayan değerler

9,20 ' den önceki sürümlerde vCenter, bir VM 'deki Özellik özelliği için sayısal olmayan bir değer aldığında bağlantı kesilir `snapshot.maxSnapShots` .

Bu sorun 95126 hata KIMLIĞIYLE tanımlanır.

    ERROR :: Hit an exception while fetching the required informationfrom vCenter/vSphere.Exception details:
    System.FormatException: Input string was not in a correct format.
       at System.Number.StringToNumber(String str, NumberStyles options, NumberBuffer& number, NumberFormatInfo info, Boolean parseDecimal)
       at System.Number.ParseInt32(String s, NumberStyles style, NumberFormatInfo info)
       at VMware.VSphere.Management.InfraContracts.VirtualMachineInfo.get_MaxSnapshots()
    
Bu sorunu çözmek için:

- VM 'yi belirleyin ve değeri sayısal bir değere ayarlayın (vCenter 'da VM düzenleme ayarları).

Veya

- Yapılandırma sunucunuzu 9,20 veya sonraki bir sürüme yükseltin.

## <a name="proxy-configuration-issues-for-vcenter-connectivity"></a>VCenter bağlantısı için proxy yapılandırma sorunları

vCenter Discovery sistem kullanıcısı tarafından yapılandırılan sistem varsayılan proxy ayarlarını geliştirir. DRA hizmeti, Birleşik kurulum yükleyicisi veya OVA şablonu kullanılarak yapılandırma sunucusu yüklenirken Kullanıcı tarafından belirtilen proxy ayarlarını kabul eder. 

Genel olarak, ara sunucu ortak ağlarla iletişim kurmak için kullanılır; Azure ile iletişim kurma gibi. Ara sunucu yapılandırıldıysa ve vCenter yerel ortamındaysanız, DRA ile iletişim kuramaz.

Bu sorunla karşılaşıldığında aşağıdaki durumlar oluşur:

- VCenter sunucusuna \<vCenter> Şu hata nedeniyle erişilemiyor: uzak sunucu bir hata döndürdü: (503) sunucu kullanılamıyor
- VCenter sunucusuna \<vCenter> Şu hata nedeniyle erişilemiyor: uzak sunucu bir hata döndürdü: uzak sunucuya bağlanılamıyor.
- VCenter/ESXi sunucusuyla bağlantı kurulamıyor.

Bu sorunu çözmek için:

[PsExec aracını](https://aka.ms/PsExec)indirin. 

Sistem Kullanıcı bağlamına erişmek ve proxy adresinin yapılandırılıp yapılandırılmadığını öğrenmek için PsExec aracını kullanın. Ardından aşağıdaki yordamları kullanarak atlama listesine vCenter ekleyebilirsiniz.

Bulma proxy yapılandırması için:

1. PsExec aracını kullanarak IE 'yi sistem kullanıcı bağlamında açın.
    
    PsExec-s-i "%ProgramFiles%\Internet Explorer\iexplore.exe"

2. Internet Explorer 'daki proxy ayarlarını, vCenter IP adresini atlayacak şekilde değiştirin.
3. Tmanssvc hizmetini yeniden başlatın.

DRA proxy yapılandırması için:

1. Bir komut istemi açın ve Microsoft Azure Site Recovery sağlayıcı klasörünü açın.
 
    **CD C:\Program Files\Microsoft Azure Site Recovery sağlayıcısı**

3. Komut isteminden aşağıdaki komutu çalıştırın.
   
   **DRCONFIGURATOR.EXE/configure/AddBypassUrls [vCenter ekleme sırasında sağlanmış vCenter Server IP adresi/FQDN 'SI]**

4. DRA sağlayıcı hizmetini yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

[VMware VM olağanüstü durum kurtarma için yapılandırma sunucusunu yönetme](https://docs.microsoft.com/azure/site-recovery/vmware-azure-manage-configuration-server#refresh-configuration-server) 
