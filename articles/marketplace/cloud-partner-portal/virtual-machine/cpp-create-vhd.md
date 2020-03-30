---
title: Azure Marketi için Azure uyumlu bir VHD oluşturun
description: Azure Marketi'nde sanal makine teklifi için nasıl VHD oluşturulacak larını açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: dsindona
ms.openlocfilehash: 2014a775edd4e24f5d302d863d0b69d83009b8a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80277999"
---
# <a name="create-an-azure-compatible-vhd"></a>Azure uyumlu bir VHD oluşturma

Bu makalede, Azure Marketi'ndeki sanal makine (VM) teklifi için sanal bir sabit disk (VHD) oluşturmak için gereken adımlar ayrıntılı olarak anlatılmalıdır.  Ayrıca, Uzak Masaüstü Protokolü 'nü (RDP) kullanma, VM için bir boyut seçme, en son Windows güncelleştirmelerini yükleme ve VHD görüntüsünü genelleme gibi çeşitli yönleriyle ilgili en iyi uygulamaları da içerir.  Aşağıdaki bölümler de daha çok windows tabanlı VHD'lere odaklanılabı; Linux tabanlı VHD'ler oluşturma hakkında daha fazla bilgi için [Azure tarafından onaylanan dağıtımlar hakkında Linux'a](../../../virtual-machines/linux/endorsed-distros.md)bakın. 

> [!WARNING]
> Önceden yapılandırılmış, onaylanmış bir işletim sistemi içeren bir VM oluşturmak için Azure'u kullanmak için bu konuyla ilgili kılavuzu izlemeniz önerilir.  Bu, çözümünüzle uyumlu değilse, onaylanmış bir işletim sistemi kullanarak şirket içi bir VM oluşturmak ve yapılandırmak mümkündür.  Daha sonra, Azure'a yüklemek için [Windows VHD veya VHDX'te](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image)açıklandığı gibi yapılandırAbilir ve yüklemeye hazırlayabilirsiniz.


## <a name="select-an-approved-base"></a>Onaylı bir taban seçin
VM görüntünüz için vhd işletim sistemi, Windows Server veya SQL Server içeren Azure onaylı bir temel görüntüye dayanmalıdır.
Başlamak için, Microsoft Azure portalında bulunan aşağıdaki resimlerden birinden bir VM oluşturun:

-   Windows Server ([2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016), [2012 R2 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2012 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview), [2008 R2 SP1](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview))
-   [SQL Server 2014](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) (Kurumsal, Standart, Web)
-   [SQL Server 2012 SP2](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) (Kurumsal, Standart, Web)

> [!TIP]
> Geçerli Azure portalını veya PowerShell'i kullanıyorsanız, 8 Eylül 2014 tarihinde ve daha sonra yayınlanan Windows Server görüntüleri onaylanır.

Azure alternatif olarak bir dizi onaylı Linux dağıtımı sunar.  Geçerli bir liste için Azure [tarafından onaylanan dağıtımlar hakkında Linux'a](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)bakın.


## <a name="create-vm-in-the-azure-portal"></a>Azure portalında VM oluşturma 

Microsoft [Azure portalında,](https://ms.portal.azure.com/)aşağıdaki adımları kullanarak temel görüntüyü oluşturun.

1. VM teklifinizi yayınlamak istediğiniz Azure aboneliği için Microsoft hesabıyla portalda oturum açın.
2. Yeni bir kaynak grubu oluşturun ve **Kaynak grup adınızı,** **Aboneliğinizi**ve **Kaynak grup konumunuzu**sağlayın.  Daha fazla kılavuz için kaynak [gruplarını yönet'e](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal)bakın.
3. Sanal makineler ayrıntıları sayfasını görüntülemek için sol menü çubuğundaki **Sanal makinelere** tıklayın. 
4. Bu yeni sayfada, **Bilgi İşlem** bıçağını görüntülemek için **+Ekle'ye** tıklayın.  İlk ekranda VM türünü görmüyorsanız, örneğin temel VM adınızı arayabilirsiniz:

    ![Yeni VM'nin işlem bıçağı](./media/publishvm_014.png)

5. Uygun sanal görüntüyü seçtikten sonra aşağıdaki değerleri sağlayın:
   * **Basics** bıçağında, 1-15 alfasayısal karakter arasında sanal makine için bir **Ad** girin. (Bu örnek `DemoVm009`kullanır .)
   * VM'de yerel bir hesap oluşturmak için kullanılan bir **Kullanıcı adı** ve güçlü bir **Parola**girin.  (Burada `adminUser` kullanılır.)  Parola 8-123 karakter uzunluğunda olmalı ve aşağıdaki dört karmaşıklık gereksiniminden üçünü karşılamalıdır: bir küçük harf karakteri, bir büyük harf karakteri, bir sayı ve bir özel karakter. Daha fazla bilgi için [kullanıcı adı ve parola gereksinimlerine](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-faq#what-are-the-username-requirements-when-creating-a-vm)bakın.
   * Oluşturduğunuz Kaynak grubunu seçin `DemoResourceGroup`(burada).
   * Azure Veri Merkezi **Konumu** `West US`'nü seçin (burada).
   * Bu değerleri kaydetmek için **Tamam'ı** tıklatın. 

6. Aşağıdaki önerileri kullanarak dağıtmak için VM boyutunu seçin:
   * VHD'yi şirket içinde geliştirmeyi planlıyorsanız, boyutu önemli değildir. Küçük VM'lerden birini kullanmayı düşünün.
   * Görüntüyü Azure'da geliştirmeyi planlıyorsanız, seçili görüntü için önerilen VM boyutlarından birini kullanmayı düşünün.
   * Fiyatlandırma bilgileri için portalda görüntülenen **Önerilen fiyatlandırma katmanları** seçicisine bakın. Yayımcı tarafından sağlanan önerilen üç boyutu görüntüler. (Burada, yayımcı Microsoft'dur.)

   ![Yeni VM boyutu bıçak](./media/publishvm_015.png)

7. **Ayarlar** bıçağında, **Yönetilen Diski Kullan** seçeneğini **No**olarak ayarlayın.  Bu, yeni VHD'yi el ile yönetmenize olanak tanır. (Ayarlar **Settings** bıçağı, depolama ve ağ seçeneklerini değiştirmenizi de sağlar, örneğin **Disk türünde** **Premium (SSD)** seçeneğini seçer.)  Devam etmek için **Tamam'ı** tıklatın.

    ![Yeni VM'nin ayarları](./media/publishvm_016.png)

8. Seçimlerinizi gözden geçirmek için **Özet**’e tıklayın. **Doğrulama başarılı** iletisini gördüğünüzde **Tamam**’a tıklayın.

    ![Yeni VM'nin özeti](./media/publishvm_017.png)

Azure, belirttiğiniz sanal makinenin sağlanmasına başlar.  Soldaki **Sanal Makineler** sekmesine tıklayarak ilerlemesini izleyebilirsiniz.  Oluşturulduktan sonra durum **Çalışan**olarak değişecektir.  Bu noktada, [sanal makineye bağlanabilirsiniz.](./cpp-connect-vm.md)


## <a name="next-steps"></a>Sonraki adımlar

Yeni Azure tabanlı VHD'nizi oluştururken zorlukla karşılaştıysanız, [VHD oluşturma sırasında sık karşılaşılan sorunlara](./cpp-common-vhd-creation-issues.md)bakın.  Aksi takdirde, azure'da oluşturduğunuz [VM'lere daha](./cpp-connect-vm.md) sonra bağlanmanız gerekir. 
