---
title: Azure NetApp Files için çift protokol birimlerinde sorun giderme | Microsoft Docs
description: Azure NetApp Files için çift protokol sorunlarını gidermenize yardımcı olabilecek hata iletilerini ve çözümlerini açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/12/2021
ms.author: b-juche
ms.openlocfilehash: 0ae7e8f745a91e080d12a47271057ed90f9bc835
ms.sourcegitcommit: 431bf5709b433bb12ab1f2e591f1f61f6d87f66c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98134339"
---
# <a name="troubleshoot-dual-protocol-volumes"></a>Çift protokol birimlerinde sorunları giderme

Bu makalede, çift protokol birimleri oluştururken veya yönetirken oluşabilecek hata koşullarının çözümleri açıklanmaktadır.

## <a name="error-conditions-and-resolutions"></a>Hata koşulları ve çözümleri

|     Hata koşulları    |     Çözüm    |
|-|-|
| Çift protokol birimi oluşturma hatayla başarısız oluyor `This Active Directory has no Server root CA Certificate` .    |     Bir çift protokol birimi oluştururken bu hata oluşursa, kök CA sertifikasının NetApp hesabınıza yüklendiğinden emin olun.    |
| Çift protokol birimi oluşturma hatayla başarısız oluyor `Failed to validate LDAP configuration, try again after correcting LDAP configuration` .    |  AD ana makinesinin işaretçi (PTR) kaydı DNS sunucusunda eksik olabilir. DNS sunucusunda bir geriye doğru arama bölgesi oluşturmanız ve ardından bu geriye doğru arama bölgesine AD ana makinesinin PTR kaydını eklemeniz gerekir. <br> Örneğin, AD makinenin IP adresinin olduğunu `1.1.1.1` , ad makinenin ana bilgisayar adının (komutunu kullanarak bulunduğu şekilde `hostname` ) olduğunu `AD1` ve etki alanı adının olduğunu varsayalım `contoso.com` .  Geriye doğru arama bölgesine eklenen PTR kaydı olmalıdır `1.1.1.1`  ->  `contoso.com` .   |
| Çift protokol birimi oluşturma hatayla başarısız oluyor `Failed to create the Active Directory machine account \\\"TESTAD-C8DD\\\". Reason: Kerberos Error: Pre-authentication information was invalid Details: Error: Machine account creation procedure failed\\n [ 434] Loaded the preliminary configuration.\\n [ 537] Successfully connected to ip 1.1.1.1, port 88 using TCP\\n**[ 950] FAILURE` . |  Bu hata, Active Directory NetApp hesabına katıldığında AD parolasının yanlış olduğunu gösterir. AD bağlantısını doğru parolayla güncelleştirip yeniden deneyin. |
| Çift protokol birimi oluşturma hatayla başarısız oluyor `Could not query DNS server. Verify that the network configuration is correct and that DNS servers are available` . |   Bu hata, DNS 'ye ulaşılamıyor olduğunu gösterir. Nedeni, DNS IP yanlış veya bir ağ sorunu olabilir. AD bağlantısı ' nda girilen DNS IP 'sini kontrol edin ve IP 'nin doğru olduğundan emin olun. <br> Ayrıca, AD ve birimin aynı bölgede ve aynı VNet 'de olduğundan emin olun. Bunlar farklı VNET 'lerde yer alıyorsa, iki sanal ağ arasında VNet eşlemesi kurulduğundan emin olun.|
| Çift protokol birimi bağlanırken izin reddedildi hatası. | Çift protokol birimi hem NFS hem de SMB protokollerini destekler.  UNIX sisteminde takılı birime erişmeye çalıştığınızda, sistem kullandığınız UNIX kullanıcısını bir Windows kullanıcısına eşlemeye çalışır. Hiçbir eşleme bulunmazsa, "Izin verilmedi" hatası oluşur. <br> Bu durum, erişim için ' root ' kullanıcısını kullandığınızda da geçerlidir. <br> "Izin reddedildi" sorununu önlemek için, `pcuser` bağlama noktasına erişmeden önce Windows Active Directory 'nin dahil olduğundan emin olun. `pcuser`"İzin reddedildi" sorunuyla karşılaşduktan sonra eklerseniz, erişimi yeniden denemeden önce önbellek girişinin Temizleme için 24 saat bekleyin. |

## <a name="next-steps"></a>Sonraki adımlar  

* [Çift protokollü birim oluşturma](create-volumes-dual-protocol.md)
* [Azure NetApp Files için NFS istemcisini yapılandırma](configure-nfs-clients.md)
