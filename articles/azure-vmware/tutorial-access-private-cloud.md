---
title: Öğretici-özel bulutunuzun nasıl erişebileceğini öğrenin
description: Azure VMware çözümüne özel buluta erişmeyi öğrenin
ms.topic: tutorial
ms.date: 08/21/2020
ms.openlocfilehash: 73226c6aa567dc5fbe18251bed4812637664a02c
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88750541"
---
# <a name="tutorial-learn-how-to-access-an-azure-vmware-solution-private-cloud"></a>Öğretici: Azure VMware çözümüne özel buluta erişme hakkında bilgi edinin

Önizleme süresince Azure VMware çözümü, şirket içi vCenter 'unuzda özel bulutunuzu yönetmenize izin vermez. Bir bağlantı kutusu aracılığıyla ek kurulum ve yerel bir vCenter örneğine bağlantı gerçekleştirmeniz gerekir. 

Bu öğreticide, önceki öğretici öğreticisinde oluşturduğunuz kaynak grubundaki bir geçiş kutusu için bir Windows sanal makinesi oluşturacaksınız [: Azure 'Da VMware özel bulutunuz için ağ Iletişimini yapılandırın](tutorial-configure-networking.md) ve vCenter oturumu açın. Bu, oluşturduğunuz sanal ağ üzerindeki bir VM 'dir ve vCenter ve NSX Manager erişimi sağlar. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * VCenter 'a bağlanmak için kullanılacak bir Windows sanal makinesi oluşturun
> * Sanal makinenizden vCenter 'da oturum açın

## <a name="create-a-new-windows-virtual-machine"></a>Yeni bir Windows sanal makinesi oluşturma

1. Kaynak grubunda **+ Ekle** ' yi seçin ve **Microsoft Windows 10**' u seçin ve ardından **Oluştur**' a tıklayın.

   :::image type="content" source="./media/tutorial-access-private-cloud/ss8-azure-w10vm-create.png" alt-text="Sıçrama kutusu için yeni bir Windows 10 VM ekleyin." border="true":::

1. Alanlara gerekli bilgileri girin ve ardından **gözden geçir + oluştur**' u seçin. Alanlarla ilgili daha fazla bilgi için aşağıdaki tabloya bakın.

   | Alan | Değer |
   | --- | --- |
   | **Abonelik** | Bu değer, kaynak grubunun ait olduğu abonelikle zaten doldurulmuştur. |
   | **Kaynak grubu** | Bu değer, geçerli kaynak grubu için zaten doldurulmuştur. Bu, önceki bir öğreticide oluşturduğunuz kaynak grubu olmalıdır. |
   | **Sanal makine adı** | VM için benzersiz bir ad girin. |
   | **Bölge** | VM 'nin coğrafi konumunu seçin. |
   | **Kullanılabilirlik seçenekleri** | Varsayılan değeri seçili bırakın. |
   | **Görüntü** | VM görüntüsünü seçin. |
   | **Boyut** | Varsayılan boyut değerini bırakın. |
   | **Kimlik doğrulaması türü**  | **Parola**seçin. |
   | **Kullanıcı adı** | VM 'de oturum açmak için Kullanıcı adını girin. |
   | **Parola** | VM 'de oturum açmak için parolayı girin. |
   | **Parolayı onayla** | VM 'de oturum açmak için parolayı girin. |
   | **Genel gelen bağlantı noktaları** | **Hiçbiri** seçeneğini belirtin. Hiçbiri ' ni seçerseniz, sanal makine erişimini denetlemek için [JIT erişimini](../security-center/security-center-just-in-time.md#jit-configure) yalnızca ona erişmek istediğinizde kullanabilirsiniz.  |

1. Uygun bilgileri girdikten sonra, **gözden geçir + oluştur**' a tıklayın. 
1. Doğrulama başarılı olduktan sonra, sanal makine oluşturma işlemini başlatmak için **Oluştur** ' u seçin.

   :::image type="content" source="./media/tutorial-access-private-cloud/ss11-review-create-wjb01.png" alt-text="Sıçrama kutusu için yeni bir Windows 10 VM oluşturun." border="true":::

## <a name="connect-to-the-local-vcenter-of-your-private-cloud"></a>Özel bulutunuzun yerel vCenter 'a bağlanma

1. Sıçrama kutusundan, VMware vCenter SSO ile vSphere Istemcisinde oturum açın. bir bulut Yöneticisi Kullanıcı adı kullanarak vSphere Istemcisinde oturum açın; güvenlik riskini kabul edin ve olası bir güvenlik riski hakkında uyarı gördüğünüzde devam edin; VMware vCenter 'da çoklu oturum açma kimlik bilgileriyle oturum açın ve Kullanıcı arabiriminin başarıyla görüntülendiğini doğrulayın.

1. Azure portal özel bulutunuzu seçip **genel bakış** görünümünde, **kimlik > varsayılan**' ı seçin. Özel bulut vCenter ve NSX-T Yöneticisi için URL 'Ler ve oturum açma kimlik bilgileri görüntülenir.

   :::image type="content" source="./media/tutorial-access-private-cloud/ss4-display-identity.png" alt-text="Özel bulut vCenter ve NSX Yöneticisi URL 'Leri ve kimlik bilgilerini görüntüleyin." border="true":::

1. Önceki adımda oluşturduğunuz sanal makineye gidin ve sanal makineye bağlanın. Sanal makineye bağlanma hakkında ayrıntılı adımlar için bkz. [sanal makineye bağlanma](../virtual-machines/windows/connect-logon.md#connect-to-the-virtual-machine)

1. Windows VM 'de bir tarayıcı açın ve iki sekmede vCenter ve NSX-T Yöneticisi URL 'Lerine gidin. VCenter sekmesinde, `cloudadmin@vmcp.local` önceki adımdan Kullanıcı kimlik bilgilerini girin.

   :::image type="content" source="./media/tutorial-access-private-cloud/ss5-vcenter-login.png" alt-text="Özel bulut vCenter ' da oturum açın." border="true":::

   :::image type="content" source="./media/tutorial-access-private-cloud/ss6-vsphere-client-home.png" alt-text="vCenter portalı." border="true":::

1. Tarayıcının ikinci sekmesinde NSX-T Yöneticisi ' nde oturum açın.

   :::image type="content" source="./media/tutorial-access-private-cloud/ss10-nsx-manager-home.png" alt-text="Tarayıcının ikinci sekmesinde NSX-T Yöneticisi ' nde oturum açın." border="true":::

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * VCenter 'a bağlanmak için kullanılacak bir Windows sanal makinesi oluşturun
> * Sanal makinenizden vCenter 'da oturum açın

Azure VMware çözümünüz özel bulutunuzu nasıl ölçeklendirebileceğinizi öğrenmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure VMware çözümünün özel bulutunu ölçeklendirme](tutorial-scale-private-cloud.md)
