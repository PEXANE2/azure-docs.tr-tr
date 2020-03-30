---
title: Azure NetApp Dosyaları için NFSv4.1 varsayılan etki alanını yapılandırma | Microsoft Dokümanlar
description: NFSv4.1'i Azure NetApp Dosyaları ile kullanmak için NFS istemcisinin nasıl yapılandırılabildiğini açıklar.
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2019
ms.author: b-juche
ms.openlocfilehash: 77178a23206eadae941794c92b8dd99fe2ca1e05
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73906292"
---
# <a name="configure-nfsv41-default-domain-for-azure-netapp-files"></a>Azure NetApp Files için NFSv 4.1 varsayılan etki alanını yapılandırma

NFSv4 kimlik doğrulama etki alanı kavramını sunar. Azure NetApp Files şu anda hizmetten NFS istemcisine yalnızca root kullanıcı eşleme sini destekler. Azure NetApp Dosyaları ile NFSv4.1 işlevini kullanmak için NFS istemcisini güncellemeniz gerekir.

## <a name="default-behavior-of-usergroup-mapping"></a>Kullanıcı/grup eşlemesinin varsayılan davranışı

NFSv4 `nobody` etki alanı `localdomain`olarak ayarlandığı için kök eşleme kullanıcı için varsayılan. Bir Azure NetApp Files NFSv4.1 birimini kök olarak monte ettiğinizde, dosya izinlerini aşağıdaki gibi görürsünüz:  

![NFSv4.1 için kullanıcı/grup eşlemenin varsayılan davranışı](../media/azure-netapp-files/azure-netapp-files-nfsv41-default-behavior-user-group-mapping.png)

Yukarıdaki örnekte de görüldüğü `file1` gibi, `root`kullanıcı için `nobody` olmalıdır , ancak varsayılan olarak eşler.  Bu makalede, kullanıcıyı `file1` nasıl `root`.'a ayarlayabileceğinizi gösterir  

## <a name="steps"></a>Adımlar 

1. NFS `/etc/idmapd.conf` istemcisindeki dosyayı edin.   
    Satırı `#Domain` açıklamayı kaldırın (diğer `#` bir şekilde satırı kaldırın) `localdomain` `defaultv4iddomain.com`ve değeri ' le değiştirin. 

    İlk yapılandırma: 
    
    ![NFSv4.1 için ilk yapılandırma](../media/azure-netapp-files/azure-netapp-files-nfsv41-initial-config.png)

    Güncelleştirilmiş yapılandırma:
    
    ![NFSv4.1 için güncelleştirilmiş yapılandırma](../media/azure-netapp-files/azure-netapp-files-nfsv41-updated-config.png)

2. Şu anda monte edilmiş herhangi bir NFS cildini sökme.
3. Dosyayı `/etc/idmapd.conf` güncelleştirin.
4. Hizmeti ana `rpcbind` bilgisayarda yeniden`service rpcbind restart`başlatın ( veya yalnızca ana bilgisayarı yeniden başlatın.
5. NFS birimlerini gerektiği gibi monte edin.   

    [Bkz. Windows veya Linux sanal makineleri için bir ses düzeyini mount veya sökme.](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md) 

Aşağıdaki örnekte, ortaya çıkan kullanıcı/grup değişikliği gösterilmektedir: 

![NFSv4.1 için ortaya çıkan yapılandırma](../media/azure-netapp-files/azure-netapp-files-nfsv41-resulting-config.png)

Örnekte de görüldüğü gibi, kullanıcı/grup `nobody` `root`artık .'dan .'a değiştirildi.

## <a name="behavior-of-other-non-root-users-and-groups"></a>Diğer (kökolmayan) kullanıcı ve grupların davranışı

Azure NetApp Files, NFSv4.1 birimlerinde dosya veya klasörlerle ilişkili izinleri olan yerel kullanıcıları (ana bilgisayarda yerel olarak oluşturulan kullanıcılar) destekler. Ancak, hizmet şu anda birden çok düğüm arasında kullanıcıların/grupların eşleneme desteklemiyor. Bu nedenle, bir ana bilgisayarda oluşturulan kullanıcılar varsayılan olarak başka bir ana bilgisayarda oluşturulan kullanıcılarla eşleşmez. 

Aşağıdaki örnekte, `Host1` varolan üç test`testuser01` `testuser02`kullanıcı `testuser03`hesabı vardır ( , , ): 

![NFSv4.1 için ortaya çıkan yapılandırma](../media/azure-netapp-files/azure-netapp-files-nfsv41-host1-users.png)

On `Host2`, test kullanıcı hesapları oluşturulmadı unutmayın, ancak aynı birim her iki ana bilgisayara monte edilir:

![NFSv4.1 için ortaya çıkan yapılandırma](../media/azure-netapp-files/azure-netapp-files-nfsv41-host2-users.png)

## <a name="next-step"></a>Sonraki adım 

[Windows veya Linux sanal makineleri için birimi bağlama veya ayırma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)

