---
title: Hostadlarını Görüntüleme ve Değiştirme | Microsoft Dokümanlar
description: Ad çözümlemesi için Azure sanal makineleri, web ve çalışan rolleri için ana bilgisayar adlarını görüntüleme ve değiştirme
services: virtual-network
documentationcenter: na
author: genlin
manager: dcscontentpm
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: cce248e2906f4a36737388e8cc7124b1bb19fbae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71058669"
---
# <a name="viewing-and-modifying-hostnames"></a>Ana bilgisayar adlarını görüntüleme ve değiştirme
Rol örneklerinizin ana bilgisayar adına başvurulmasını sağlamak için, her rol için hizmet yapılandırma dosyasındaki ana bilgisayar adının değerini ayarlamanız gerekir. Bunu, **Rol** öğesinin **vmName** özniteliğine istenen ana bilgisayar adını ekleyerek yaparsınız. **vmName** özniteliğinin değeri, her rol örneğinin ana adı için bir temel olarak kullanılır. Örneğin, **vmName** *webrole* ise ve bu rolün üç örneği varsa, örneklerin ana bilgisayar adları *webrole0,* *webrole1*ve *webrole2*olacaktır. Sanal makinenin ana bilgisayar adı sanal makine adına bağlı olarak doldurulduğundan, yapılandırma dosyasındaki sanal makineler için bir ana bilgisayar adı belirtmeniz gerekmez. Microsoft Azure hizmetini yapılandırma hakkında daha fazla bilgi için [Azure Hizmet Yapılandırma Şeması (.cscfg Dosyası)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Ev sahibi adlarını görüntüleme
Aşağıdaki araçlardan herhangi birini kullanarak bulut hizmetinde sanal makinelerin ana bilgisayar adlarını ve rol örneklerini görüntüleyebilirsiniz.

### <a name="service-configuration-file"></a>Hizmet yapılandırma dosyası
Dağıtılmış bir hizmetin hizmet yapılandırma dosyasını Azure portalındaki hizmetin **Yapılandırma** bıçağından indirebilirsiniz. Daha sonra ana bilgisayar adını görmek için **Rol adı** öğesinin **vmName** özniteliğini alabilirsiniz. Bu ana bilgisayar adının her rol örneğinin ana bilgisayarı adı için bir temel olarak kullanıldığını unutmayın. Örneğin, **vmName** *webrole* ise ve bu rolün üç örneği varsa, örneklerin ana bilgisayar adları *webrole0,* *webrole1*ve *webrole2*olacaktır.

### <a name="remote-desktop"></a>Uzak Masaüstü
Uzak Masaüstü (Windows), Windows PowerShell remoting (Windows) veya Sanal makinelerinize veya rol örneklerinize SSH (Linux ve Windows) bağlantılarını etkinleştirdikten sonra, ana bilgisayar adını etkin bir Uzak Masaüstü bağlantısından çeşitli şekillerde görüntüleyebilirsiniz:

* Komut istemiveya SSH terminalinde hostname yazın.
* Komut istemine ipconfig /all yazın (yalnızca Windows).
* Sistem ayarlarında bilgisayar adını görüntüleyin (yalnızca Windows).

### <a name="azure-service-management-rest-api"></a>Azure Hizmet Yönetimi REST API
Bir REST istemcisinden şu yönergeleri izleyin:

1. Azure portalına bağlanmak için bir istemci sertifikanız olduğundan emin olun. İstemci sertifikası almak için, [Nasıl Yapılır: Yayımlama Ayarları ve Abonelik Bilgileri'nde](https://msdn.microsoft.com/library/dn385850.aspx)sunulan adımları izleyin. 
2. 2013-11-01 değerine sahip x-ms sürümü adlı bir üstbilgi girişi ayarlayın.
3. Aşağıdaki biçimde bir istek gönderin:\/\<https: /management.core.windows.net/\>subscrition-id\</services/hostedservices/ service-name\>?embed-detail=true
4. Her **RoleInstance** öğesi için **HostName** öğesini arayın.

> [!WARNING]
> Ayrıca, **InternalDnsSSonefix** öğesini kontrol ederek veya uzak masaüstü oturumundaki (Windows) komut isteminden ipconfig /all çalıştırarak veya bir SSH terminalinden (Linux) cat /etc/resolv.conf çalıştırarak REST çağrı yanıtından bulut hizmetiniz için dahili etki alanı ekini görüntüleyebilirsiniz.
> 
> 

## <a name="modifying-a-hostname"></a>Ana bilgisayar adını değiştirme
Değiştirilen bir hizmet yapılandırma dosyası yükleyerek veya bilgisayarı Uzak Masaüstü oturumundan yeniden adlandırarak herhangi bir sanal makine veya rol örneği için ana bilgisayar adını değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Ad Çözümlemesi (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Azure Hizmet Yapılandırma Şeması (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure Sanal Ağ Yapılandırma Şeması](https://go.microsoft.com/fwlink/?LinkId=248093)

[Ağ yapılandırma dosyalarını kullanarak DNS ayarlarını belirtin](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

