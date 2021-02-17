---
title: Azure NetApp Files için SMB veya çift protokol birimlerinde sorun giderme | Microsoft Docs
description: Azure NetApp Files için SMB veya çift protokol sorunlarını gidermenize yardımcı olabilecek hata iletilerini ve çözümlerini açıklar.
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
ms.date: 02/02/2021
ms.author: b-juche
ms.openlocfilehash: dbc9f466437a575866c33219ff11af2d85d9a58b
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100571089"
---
# <a name="troubleshoot-smb-or-dual-protocol-volumes"></a>SMB veya çift protokol birimlerinde sorun giderme

Bu makalede, çift protokol birimleri oluştururken veya yönetirken oluşabilecek hata koşullarının çözümleri açıklanmaktadır.

## <a name="errors-for-dual-protocol-volumes"></a>Çift protokol birimlerine yönelik hatalar

|     Hata koşulları    |     Çözümler    |
|-|-|
| TLS üzerinden LDAP etkinleştirilmiştir ve çift protokol birimi oluşturma hatayla başarısız olur `This Active Directory has no Server root CA Certificate` .    |     Bir çift protokol birimi oluştururken bu hata oluşursa, kök CA sertifikasının NetApp hesabınıza yüklendiğinden emin olun.    |
| Çift protokol birimi oluşturma hatayla başarısız oluyor `Failed to validate LDAP configuration, try again after correcting LDAP configuration` .    |  AD ana makinesinin işaretçi (PTR) kaydı DNS sunucusunda eksik olabilir. DNS sunucusunda bir geriye doğru arama bölgesi oluşturmanız ve ardından bu geriye doğru arama bölgesine AD ana makinesinin PTR kaydını eklemeniz gerekir. <br> Örneğin, AD makinenin IP adresinin olduğunu `10.X.X.X` , ad makinenin ana bilgisayar adının (komutunu kullanarak bulunduğu şekilde `hostname` ) olduğunu `AD1` ve etki alanı adının olduğunu varsayalım `contoso.com` .  Geriye doğru arama bölgesine eklenen PTR kaydı olmalıdır `10.X.X.X`  ->  `contoso.com` .   |
| Çift protokol birimi oluşturma hatayla başarısız oluyor `Failed to create the Active Directory machine account \\\"TESTAD-C8DD\\\". Reason: Kerberos Error: Pre-authentication information was invalid Details: Error: Machine account creation procedure failed\\n [ 434] Loaded the preliminary configuration.\\n [ 537] Successfully connected to ip 10.X.X.X, port 88 using TCP\\n**[ 950] FAILURE` . |     Bu hata, Active Directory NetApp hesabına katıldığında AD parolasının yanlış olduğunu gösterir. AD bağlantısını doğru parolayla güncelleştirip yeniden deneyin. |
| Çift protokol birimi oluşturma hatayla başarısız oluyor `Could not query DNS server. Verify that the network configuration is correct and that DNS servers are available` . |   Bu hata, DNS 'ye ulaşılamıyor olduğunu gösterir. Nedeni, DNS IP yanlış veya bir ağ sorunu olabilir. AD bağlantısı ' nda girilen DNS IP 'sini kontrol edin ve IP 'nin doğru olduğundan emin olun. <br> Ayrıca, AD ve birimin aynı bölgede ve aynı VNet 'de olduğundan emin olun. Bunlar farklı VNET 'lerde yer alıyorsa, iki sanal ağ arasında VNet eşlemesi kurulduğundan emin olun.|
| Çift protokol birimi bağlanırken izin reddedildi hatası. | Çift protokol birimi hem NFS hem de SMB protokollerini destekler.  UNIX sisteminde takılı birime erişmeye çalıştığınızda, sistem kullandığınız UNIX kullanıcısını bir Windows kullanıcısına eşlemeye çalışır. Hiçbir eşleme bulunmazsa, "Izin verilmedi" hatası oluşur. <br> Bu durum, erişim için ' root ' kullanıcısını kullandığınızda da geçerlidir. <br> "Izin reddedildi" sorununu önlemek için, `pcuser` bağlama noktasına erişmeden önce Windows Active Directory 'nin dahil olduğundan emin olun. `pcuser`"İzin reddedildi" sorunuyla karşılaşduktan sonra eklerseniz, erişimi yeniden denemeden önce önbellek girişinin Temizleme için 24 saat bekleyin. |

## <a name="common-errors-for-smb-and-dual-protocol-volumes"></a>SMB ve çift protokol birimleri için sık karşılaşılan hatalar

|     Hata koşulları    |     Çözümler    |
|-|-|
| SMB veya çift protokol birimi oluşturma işlemi şu hatayla başarısız olur: <br> `{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/DeployOperations for usage details.","details":[{"code":"InternalServerError", "message":"Error when creating - Could not query DNS server. Verify that the network configuration is correct and that DNS servers are available."}]}` | Bu hata, DNS 'ye ulaşılamadığını gösterir. <br> Aşağıdaki çözümleri göz önünde bulundurun: <ul><li>EKLEME ve birimin aynı bölgede dağıtılıp dağıtılmadığını denetleyin.</li> <li>EKLEME ve birimin aynı VNet 'i kullanıp kullanmından emin olun. Farklı VNET 'ler kullanıyorsa, sanal ağların birbirleriyle eşlendiklerinden emin olun. [Azure NetApp Files ağ planlama yönergelerine](azure-netapp-files-network-topologies.md)bakın. </li> <li>DNS sunucusunda ağ güvenlik grupları (NSG 'ler) uygulanmış olabilir.  Bu nedenle trafiğin akışa izin vermez. Bu durumda, çeşitli bağlantı noktalarına bağlanmak için NSG 'leri DNS veya AD 'de açın. Bağlantı noktası gereksinimleri için bkz. [Active Directory bağlantıları gereksinimleri](create-active-directory-connections.md#requirements-for-active-directory-connections). </li></ul> <br>Azure EKLEMELERI için aynı çözümler geçerlidir. Azure EKLEMELERI aynı bölgede dağıtılmalıdır. VNet, birim tarafından kullanılan VNet ile aynı bölgede veya eşlenmiş olmalıdır. | 
| SMB veya çift protokol birimi oluşturma işlemi şu hatayla başarısız olur: <br> `{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/DeployOperations for usage details.","details":[{"code":"InternalServerError", "message":"Error when creating - Failed to create the Active Directory machine account \"SMBTESTAD-C1C8\". Reason: Kerberos Error: Invalid credentials were given Details: Error: Machine account creation procedure failed\n [ 563] Loaded the preliminary configuration.\n**[ 670] FAILURE: Could not authenticate as 'test@contoso.com':\n** Unknown user (KRB5KDC_ERR_C_PRINCIPAL_UNKNOWN)\n. "}]}`  |  <ul><li>Girilen kullanıcı adının doğru olduğundan emin olun. </li> <li>Kullanıcının makine hesabı oluşturma ayrıcalığına sahip yönetici grubunun bir parçası olduğundan emin olun. </li> <li> Azure EKLEMELERI kullanıyorsanız, kullanıcının Azure AD grubunun bir parçası olduğundan emin olun `Azure AD DC Administrators` . </li></ul> | 
| SMB veya çift protokol birimi oluşturma işlemi şu hatayla başarısız olur: <br> `{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/DeployOperations for usage details.","details":[{"code":"InternalServerError", "message":"Error when creating - Failed to create the Active Directory machine account \"SMBTESTAD-A452\". Reason: Kerberos Error: Pre-authentication information was invalid Details: Error: Machine account creation procedure failed\n [ 567] Loaded the preliminary configuration.\n [ 671] Successfully connected to ip 10.X.X.X, port 88 using TCP\n**[ 1099] FAILURE: Could not authenticate as\n** 'user@contoso.com': CIFS server account password does\n** not match password stored in Active Directory\n** (KRB5KDC_ERR_PREAUTH_FAILED)\n. "}]}` | AD bağlantısına katılmak için girilen parolanın doğru olduğundan emin olun. |
| SMB veya çift protokol birimi oluşturma işlemi şu hatayla başarısız olur: `{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/DeployOperations for usage details.","details":[{"code":"InternalServerError","message":"Error when creating - Failed to create the Active Directory machine account \"SMBTESTAD-D9A2\". Reason: SecD Error: ou not found Details: Error: Machine account creation procedure failed\n [ 561] Loaded the preliminary configuration.\n [ 665] Successfully connected to ip 10.X.X.X, port 88 using TCP\n [ 1039] Successfully connected to ip 10.x.x.x, port 389 using TCP\n**[ 1147] FAILURE: Specifed OU 'OU=AADDC Com' does not exist in\n** contoso.com\n. "}]}` | AD bağlantısına katılmak için belirtilen OU yolunun doğru olduğundan emin olun. Azure EKLEMELERI kullanıyorsanız, kuruluş birimi yolunun olduğundan emin olun `OU=AADDC Computers` . |

## <a name="next-steps"></a>Sonraki adımlar  

* [SMB birimi oluşturma](azure-netapp-files-create-volumes-smb.md)
* [Çift protokollü birim oluşturma](create-volumes-dual-protocol.md)
* [Azure NetApp Files için NFS istemcisini yapılandırma](configure-nfs-clients.md)
