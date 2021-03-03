---
title: Öğretici-özel buluta erişin
description: Azure VMware çözümüne özel buluta erişmeyi öğrenin
ms.topic: tutorial
ms.date: 02/22/2021
ms.openlocfilehash: 81296223ab941633a4b0f1316e359a0eb2ff73ce
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101738489"
---
# <a name="tutorial-access-an-azure-vmware-solution-private-cloud"></a>Öğretici: Azure VMware çözümüne özel buluta erişme

Azure VMware çözümü, şirket içi vCenter 'unuzda özel bulutunuzu yönetmenize izin vermez. Bir bağlantı kutusu aracılığıyla ek kurulum ve yerel bir vCenter örneğine bağlantı yapmanız gerekir. 

Bu öğreticide, [önceki öğreticide](tutorial-configure-networking.md) oluşturduğunuz kaynak grubunda bir sıçrama kutusu oluşturacak ve vCenter 'da oturum açacaksınız. Atma kutusu, oluşturduğunuz aynı sanal ağ üzerinde bir Windows sanal makinedir (VM).  VCenter ve NSX Manager erişimi sağlar. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * VCenter 'a bağlanmak için kullanılacak bir Windows sanal makinesi oluşturun
> * Sanal makinenizden vCenter 'da oturum açın

## <a name="create-a-new-windows-virtual-machine"></a>Yeni bir Windows sanal makinesi oluşturma

[!INCLUDE [create-avs-jump-box-steps](includes/create-jump-box-steps.md)]

## <a name="connect-to-the-local-vcenter-of-your-private-cloud"></a>Özel bulutunuzun yerel vCenter 'a bağlanma

1. Geçiş kutusundan, bir bulut Yöneticisi Kullanıcı adı ve Kullanıcı arabiriminin başarıyla gösterdiği bir ifade kullanarak VMware vCenter SSO ile vSphere Istemcisinde oturum açın.

1. Azure Portal, özel bulutunuzu seçin ve ardından   >  **kimliği** yönetin. 

   Özel bulut vCenter ve NSX-T Manager için URL 'Ler ve Kullanıcı kimlik bilgileri görüntülenir.

   >[!TIP]
   >Yeni vCenter ve NSX-T parolaları oluşturmak için **Yeni bir parola oluştur** ' u seçin.

   :::image type="content" source="media/tutorial-access-private-cloud/ss4-display-identity.png" alt-text="Özel bulut vCenter ve NSX Yöneticisi URL 'Leri ve kimlik bilgilerini görüntüleyin." border="true":::

1. Önceki adımda oluşturduğunuz VM 'ye gidin ve sanal makineye bağlanın. 

   VM 'ye bağlanmada yardıma ihtiyacınız varsa, Ayrıntılar için bkz. [sanal makineye bağlanma](../virtual-machines/windows/connect-logon.md#connect-to-the-virtual-machine) .

1. Windows VM 'de bir tarayıcı açın ve iki sekmede vCenter ve NSX-T Yöneticisi URL 'Lerine gidin. 

1. VCenter sekmesinde, `cloudadmin@vmcp.local` önceki adımdan Kullanıcı kimlik bilgilerini girin.

   :::image type="content" source="media/tutorial-access-private-cloud/ss5-vcenter-login.png" alt-text="Özel bulut vCenter ' da oturum açın." border="true":::

   :::image type="content" source="media/tutorial-access-private-cloud/ss6-vsphere-client-home.png" alt-text="vCenter portalı." border="true":::

1. Tarayıcının ikinci sekmesinde NSX-T Yöneticisi ' nde oturum açın.

   :::image type="content" source="media/tutorial-access-private-cloud/ss10-nsx-manager-home.png" alt-text="Tarayıcının ikinci sekmesinde NSX-T Yöneticisi ' nde oturum açın." border="true":::



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * VCenter 'a bağlanmak için kullanılacak bir Windows sanal makinesi oluşturun
> * Sanal makinenizden vCenter 'da oturum açın

Özel bulut kümeleriniz için yerel yönetim ayarlamak üzere bir sanal ağ oluşturmayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Sanal ağ oluşturma](tutorial-configure-networking.md)


