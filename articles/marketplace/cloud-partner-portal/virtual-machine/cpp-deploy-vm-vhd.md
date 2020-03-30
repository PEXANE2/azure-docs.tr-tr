---
title: Azure Marketi için VHD'lerinizden bir VM dağıtma
description: Azure tarafından dağıtılan bir VHD'den vm'nin nasıl kaydedildiğini açıklar.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: dsindona
ms.openlocfilehash: 5263d24c411ef8de4187c2fd750013374d779f04
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80277948"
---
# <a name="deploy-a-vm-from-your-vhds"></a>VHD'lerinizden bir VM dağıtma

Bu bölümde, Azure tarafından dağıtılan bir sanal sabit diskten (VHD) sanal makine (VM) nasıl dağıtılanın açıklanır.  Gerekli araçları listeler ve bunları bir kullanıcı VM görüntüsü oluşturmak için nasıl kullanılacağını listeler ve ardından PowerShell komut dosyalarını kullanarak Azure'a dağıtır.

Genelleştirilmiş işletim sistemi VHD ve sıfır veya daha fazla veri diski VHD'lerinizi Azure depolama hesabınıza sanal sabit disklerinizi (VHD'ler) yükledikten sonra, bunları kullanıcı VM görüntüsü olarak kaydedebilirsiniz. O zaman bu görüntüyü test edebilirsin. İşletim sisteminiz VHD genelleştirilmiş olduğundan, VHD URL'sini sağlayarak VM'yi doğrudan dağıtamazsınız.

VM görüntüleri hakkında daha fazla bilgi edinmek için aşağıdaki blog gönderilerine bakın:

- [VM Görüntü](https://azure.microsoft.com/blog/vm-image-blog-post/)
- [VM Image PowerShell 'Nasıl'](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="prerequisite-install-the-necessary-tools"></a>Ön koşul: gerekli araçları yüklemek

Bunu daha önce yapmadıysanız, aşağıdaki yönergeleri kullanarak Azure PowerShell ve Azure CLI'yi yükleyin:

- [Azure PowerShell'i yükleme](https://docs.microsoft.com/powershell/azure/install-Az-ps)
- [Azure CLI'yi yükle](https://docs.microsoft.com/cli/azure/install-azure-cli)


## <a name="deployment-steps"></a>Dağıtım adımları

Bir kullanıcı VM görüntüsü oluşturmak ve dağıtmak için aşağıdaki adımları kullanırsınız:

1. Görüntüyü yakalamayı ve genelletmeyi gerektiren kullanıcı VM görüntüsünü oluşturun. 
2. Sertifikalar oluşturun ve bunları yeni bir Azure Anahtar Kasasında saklayın. VM'ye güvenli bir WinRM bağlantısı kurmak için bir sertifika gereklidir.  Azure Kaynak Yöneticisi şablonu ve Azure PowerShell komut dosyası sağlanır. 
3. VM'yi, sağlanan şablonu ve komut dosyasını kullanarak kullanıcı VM görüntüsünden dağıtın.

VM'niz dağıtıldıktan [sonra, VM görüntünüzü onaylamaya](./cpp-certify-vm.md)hazırsınız.

1. **Yeni'yi** tıklatın ve **Şablon Dağıtımı'nı**arayın, ardından **Düzenleyici'de kendi şablonunuzu oluştur'u**seçin.  <br/>
   ![Azure portalında VHD dağıtım şablonu oluşturma](./media/publishvm_021.png)

1. Bu [JSON şablonunu](./cpp-deploy-json-template.md) kopyalayıp düzenleyiciye yapıştırın ve **Kaydet'i**tıklatın. <br/>
   ![Azure portalında VHD dağıtım şablonu kaydetme](./media/publishvm_022.png)

1. Görüntülenen **Özel dağıtım** özelliği sayfaları için parametre değerlerini sağlayın.

   <table> <tr> <td valign="top"> <img src="./media/publishvm_023.png" alt="Custom deployment property page 1"> </td> <td valign="top"> <img src="./media/publishvm_024.png" alt="Custom deployment property page 2"> </td> </tr> </table> <br/> 

   |  **Parametre**              |   **Açıklama**                                                            |
   |  -------------              |   ---------------                                                            |
   | Kullanıcı Depolama Hesap Adı   | Genelleştirilmiş VHD'nin bulunduğu depolama hesabı adı                    |
   | Kullanıcı Depolama Konteyner Adı | Genelleştirilmiş VHD'nin bulunduğu konteyner adı                          |
   | Genel IP için DNS Adı      | Genel IP DNS adı. DNS Adı VM'ye ait, teklif dağıtıldıktan sonra bunu Azure Portalı'nda tanımlarsınız.  |
   | Yönetici Kullanıcı Adı             | Yeni VM için yönetici hesabının kullanıcı adı                                  |
   | Yönetici Parolası              | Yeni VM için yönetici hesabının parolası                                  |
   | İşletim Sistemi Türü                     | VM işletim sistemi: `Windows` \|`Linux`                                    |
   | Abonelik Kimliği             | Seçili aboneliğin tanımlayıcısı                                      |
   | Konum                    | Dağıtımın coğrafi konumu                                        |
   | VM Boyutu                     | Örneğin [Azure VM boyutu](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)`Standard_A2` |
   | Genel IP Adresi Adı      | Herkese açık IP adresinizin adı                                               |
   | VM Adı                     | Yeni VM'nin adı                                                           |
   | Sanal Ağ Adı        | VM tarafından kullanılan sanal ağın adı                                   |
   | NIC Adı                    | Sanal ağı çalıştıran ağ arabirim kartının adı               |
   | VHD URL                     | Komple Işletim Sistemi Disk VHD URL                                                     |
   |  |  |
            
1. Bu değerleri tedarik ettikten sonra **Satın Al'ı**tıklatın. 

Azure dağıtıma başlayacaktır: belirtilen depolama hesabı yolunda, belirtilen yönetilmeyen VHD ile yeni bir VM oluşturur.  Portalın sol tarafındaki **Sanal Makineler'e** tıklayarak Azure portalındaki ilerlemeyi izleyebilirsiniz.  VM oluşturulduğunda, durum ' dan `Starting` ' `Running`a dönüşecek 


### <a name="deploy-a-vm-from-powershell"></a>PowerShell'den VM dağıtma

Az önce oluşturulan genelleştirilmiş VM görüntüden büyük bir VM dağıtmak için aşağıdaki cmdlets'i kullanın.

``` powershell
    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot
```


## <a name="next-steps"></a>Sonraki adımlar

Ardından, çözümünüz için [bir kullanıcı VM görüntüsü oluşturacaksınız.](cpp-create-user-image.md)

