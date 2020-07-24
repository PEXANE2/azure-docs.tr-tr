---
title: "Öğretici: Azure VMware çözümünde (AVS) VM 'Leri dağıtmak için bir içerik kitaplığı oluşturma"
description: Bu Azure VMware çözümü (AVS) öğreticisinde, bir AVS özel bulutu 'nda VM dağıtmak için bir içerik kitaplığı oluşturun.
ms.topic: tutorial
ms.date: 07/16/2020
ms.openlocfilehash: d5f0f26293af9eb146cfb4b2f3adf47fe8cce94f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079392"
---
# <a name="tutorial-create-a-content-library-to-deploy-vms-in-azure-vmware-solution-avs"></a>Öğretici: Azure VMware çözümünde (AVS) VM 'Leri dağıtmak için bir içerik kitaplığı oluşturma

İçerik kitaplığı, içeriği kitaplık öğeleri biçiminde depolar ve yönetir. Tek bir kitaplık öğesi, sanal makineleri (VM 'Ler) dağıtmak için kullandığınız bir veya daha fazla dosyadan oluşur. 
 
Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * İçerik kitaplığı oluşturma
> * İçerik kitaplığına bir ISO görüntüsü yükleme
> * İçerik kitaplığındaki ISO kullanarak VM dağıtma

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için NSX-T mantıksal anahtar segmenti ve yönetilen bir DHCP hizmeti gereklidir.  Ayrıntılar için bkz. [Azure VMware çözümünde DHCP 'yi yönetme (AVS) önizleme](manage-dhcp.md) .

## <a name="create-a-content-library"></a>İçerik kitaplığı oluşturma

1. Şirket içi vSphere Istemcisinde **menü > Içerik kitaplıkları**' nı seçin.

   ![Menü-> Içerik kitaplıklarını seçin](./media/content-library/vsphere-menu-content-libraries.png)

1. Yeni bir içerik kitaplığı oluşturmak için **Ekle** düğmesine tıklayın.

   ![Yeni bir içerik kitaplığı oluşturmak için Ekle düğmesine tıklayın.](./media/content-library/create-new-content-library.png)

1. Bir ad belirtin ve vCenter sunucusunun IP adresini doğrulayın ve **İleri ' yi**seçin.

   ![Seçtiğiniz bir ad ve notları belirtin ve ardından Ileri ' yi seçin.](./media/content-library/new-content-library-step1.png)

1. **Yerel içerik kitaplığını** seçin ve Ileri ' **yi**seçin.

   ![Bu örnekte, bir yerel içerik kitaplığı oluşturacağız, Ileri ' yi seçeceğiz.](./media/content-library/new-content-library-step2.png)

1. İçerik kitaplığınızı depolayacak veri deposunu seçin ve ardından **İleri**' yi seçin.

   ![İçerik kitaplığınızı barındırmak istediğiniz veri deposunu seçin, ileri ' yi seçin.](./media/content-library/new-content-library-step3.png)

1. İçerik kitaplığı ayarlarını gözden geçirip doğrulayın ve ardından **son**' u seçin.

   ![Ayarlarınızı doğrulayın, son ' u seçin.](./media/content-library/new-content-library-step4.png)

## <a name="upload-an-iso-image-to-the-content-library"></a>İçerik kitaplığına bir ISO görüntüsü yükleme

Artık içerik kitaplığı oluşturulduğuna göre, bir VM 'yi özel bir bulut kümesine dağıtmak için bir ISO görüntüsü ekleyebilirsiniz. 

1. VSphere Istemcisinde **menü > Içerik kitaplıkları**' nı seçin.

1. Yeni ISO için kullanmak istediğiniz içerik kitaplığına sağ tıklayın ve **öğeyi Içeri aktar**' ı seçin.

1. Aşağıdakilerden birini yaparak kaynak için bir kitaplık öğesi içeri aktarın ve ardından **Içeri aktar**' ı seçin:
   1. URL ' yi seçin ve ISO indirmek için bir URL sağlayın.

   1. Yerel sisteminizden karşıya yüklenecek **yerel dosya** ' yı seçin.

   > [!TIP]
   > İsteğe bağlı olarak, hedef için bir özel öğe adı ve notları tanımlayabilirsiniz.

1. Bu kitaplığı açın ve ISO 'nizin başarıyla karşıya yüklendiğini doğrulamak için **diğer türler** sekmesini seçin.


## <a name="deploy-a-vm-to-a-private-cloud-cluster"></a>Bir VM 'yi özel bir bulut kümesine dağıtma

1. VSphere Istemcisinde **menü > konaklar ve kümeler**' ı seçin.

1. Sol bölmede, ağacı genişletin ve bir küme seçin.

1. **Yeni sanal makine > eylemler**' i seçin.

1. Sihirbaza giderek istediğiniz ayarları değiştirin.

1. **Yenı CD/DVD sürücüsü > Istemci cihazı > Içerik KITAPLıĞı ISO dosyası**' nı seçin.

1. Önceki bölümde karşıya yüklenen ISO ' yı seçin ve ardından **Tamam**' ı seçin.

1. ISO 'nın anında bağlanmasını sağlamak için **Bağlan** onay kutusunu seçin.

1. Yeni ağ ' ı seçin **> açılan menü > araştır seçeneğini belirleyin**.

1. **Mantıksal anahtarı (segmenti)** seçin ve **Tamam**' ı seçin.

1. Diğer donanım ayarlarını değiştirin ve Ileri ' **yi**seçin.

1. Ayarları doğrulayın ve **son**' u seçin.


## <a name="next-steps"></a>Sonraki adımlar

VM iş yüklerini özel buluta geçirmek için karma bulut uzantısı (HCX) kullanmayı planlıyorsanız, [Azure IÇIN HCX VMware çözümünü](hybrid-cloud-extension-installation.md) kullanın.

<!-- LINKS - external-->

<!-- LINKS - internal -->