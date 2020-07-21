---
title: Öğretici-özel bulutunuzun nasıl erişebileceğini öğrenin
description: Azure VMware çözümüne (AVS) özel buluta erişme hakkında bilgi edinin
ms.topic: tutorial
ms.date: 05/04/2020
ms.openlocfilehash: 32dc5c173789996d2559eda1153b1509e10e5984
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86497973"
---
# <a name="tutorial-learn-how-to-access-an-azure-vmware-solution-avs-private-cloud"></a>Öğretici: Azure VMware çözümüne (AVS) özel buluta erişme hakkında bilgi edinin

Önizleme süresince, AVS şirket içi vCenter 'unuzda özel bulutunuzu yönetmenize izin vermez. Bir bağlantı kutusu aracılığıyla ek kurulum ve yerel bir vCenter örneğine bağlantı gerçekleştirmeniz gerekir. 

Bu öğreticide, önceki öğretici öğreticisinde oluşturduğunuz kaynak grubundaki bir geçiş kutusu için bir Windows sanal makinesi oluşturacaksınız [: Azure 'Da VMware özel bulutunuz için ağ Iletişimini yapılandırın](tutorial-configure-networking.md) ve vCenter oturumu açın. Bu, oluşturduğunuz sanal ağ üzerindeki bir VM 'dir ve vCenter ve NSX Manager erişimi sağlar. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * VCenter 'a bağlanmak için kullanılacak bir Windows sanal makinesi oluşturun
> * Sanal makinenizden vCenter 'da oturum açın

## <a name="create-a-new-windows-virtual-machine"></a>Yeni bir Windows sanal makinesi oluşturma

Kaynak grubunda **+ Ekle** ' yi seçin ve **Microsoft Windows 10**' u seçin ve ardından **Oluştur**' a tıklayın.

:::image type="content" source="./media/tutorial-access-private-cloud/ss8-azure-w10vm-create.png" alt-text="Sıçrama kutusu için yeni bir Windows 10 VM ekleme" border="true":::

Alanlara gerekli bilgileri girin ve ardından **gözden geçir + oluştur**' u seçin. Alanlarla ilgili daha fazla bilgi için aşağıdaki tabloya bakın.

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

Uygun bilgileri girdikten sonra, **gözden geçir + oluştur**' a tıklayın. Doğrulama başarılı olduktan sonra, sanal makine oluşturma işlemini başlatmak için **Oluştur** ' u seçin.

:::image type="content" source="./media/tutorial-access-private-cloud/ss11-review-create-wjb01.png" alt-text="Sıçrama kutusu için yeni bir Windows 10 VM oluşturma" border="true":::

## <a name="connect-to-the-local-vcenter-of-your-private-cloud"></a>Özel bulutunuzun yerel vCenter 'a bağlanma

Sıçrama kutusundan, VMware vCenter SSO ile vSphere Istemcisinde oturum açın. bir bulut Yöneticisi Kullanıcı adı kullanarak vSphere Istemcisinde oturum açın; güvenlik riskini kabul edin ve olası bir güvenlik riski hakkında uyarı gördüğünüzde devam edin; VMware vCenter 'da çoklu oturum açma kimlik bilgileriyle oturum açın ve Kullanıcı arabiriminin başarıyla görüntülendiğini doğrulayın.

Azure portal özel bulutunuzu seçip **genel bakış** görünümünde, **kimlik > varsayılan**' ı seçin. Özel bulut vCenter ve NSX-T Yöneticisi için URL 'Ler ve oturum açma kimlik bilgileri görüntülenir.

:::image type="content" source="./media/tutorial-access-private-cloud/ss4-display-identity.png" alt-text="Özel bulut vCenter ve NSX Yöneticisi URL 'Lerini ve kimlik bilgilerini görüntüleme" border="true":::

Önceki adımda oluşturduğunuz sanal makineye gidin ve sanal makineye bağlanın. Sanal makineye bağlanma hakkında ayrıntılı adımlar için bkz. [sanal makineye bağlanma](../virtual-machines/windows/connect-logon.md#connect-to-the-virtual-machine)

Windows VM 'de bir tarayıcı açın ve iki sekmede vCenter ve NSX-T Yöneticisi URL 'Lerine gidin. VCenter sekmesinde, `cloudadmin@vmcp.local` önceki adımdan Kullanıcı kimlik bilgilerini girin.

:::image type="content" source="./media/tutorial-access-private-cloud/ss5-vcenter-login.png" alt-text="Özel bulut vCenter 'da oturum açın" border="true":::

:::image type="content" source="./media/tutorial-access-private-cloud/ss6-vsphere-client-home.png" alt-text="vCenter portalı" border="true":::

Tarayıcının ikinci sekmesinde NSX-T Yöneticisi ' nde oturum açın.

:::image type="content" source="./media/tutorial-access-private-cloud/ss10-nsx-manager-home.png" alt-text="Yerel özel bulut NSX Yöneticisi ana sayfası" border="true":::

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * VCenter 'a bağlanmak için kullanılacak bir Windows sanal makinesi oluşturun
> * Sanal makinenizden vCenter 'da oturum açın

AVS özel bulutunuzu nasıl ölçeklendirebileceğinizi öğrenmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [AVS özel bulutunu ölçeklendirme](tutorial-scale-private-cloud.md)
