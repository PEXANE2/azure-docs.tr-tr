---
title: Azure Marketi için VHD 'inizden bir VM dağıtma
description: Azure tarafından dağıtılan bir VHD 'den bir sanal makinenin nasıl kaydedileceği açıklanmaktadır.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: dsindona
ms.openlocfilehash: b02fda545ac135735186885d7db597885bf6cc21
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82147960"
---
# <a name="deploy-a-vm-from-your-vhds"></a>VHD 'inizden bir VM dağıtma

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure sanal makine tekliflerinizin Iş Ortağı Merkezi 'nin hareketli yönetimine başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure sanal makine teknik varlıklarınızı oluşturma](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) bölümündeki yönergeleri izleyin.

Bu bölümde, bir sanal makinenin (VM) Azure tarafından dağıtılan bir sanal sabit diskten (VHD) nasıl dağıtılacağı açıklanmaktadır.  Gerekli araçları ve Kullanıcı VM görüntüsü oluşturmak için bunları nasıl kullanacağınızı ve PowerShell betiklerini kullanarak Azure 'a dağıtmayı listeler.

Sanal sabit disklerinizi (VHD) karşıya yükledikten sonra, genelleştirilmiş işletim sistemi VHD 'SI ve sıfır veya daha fazla veri diski VHD 'leri Azure depolama hesabınıza yükledikten sonra, bunları bir Kullanıcı VM görüntüsü olarak kaydedebilirsiniz. Daha sonra bu görüntüyü test edebilirsiniz. İşletim sistemi VHD 'niz Genelleştirilmiş olduğundan, VHD URL 'sini sağlayarak VM 'yi doğrudan dağıtamazsınız.

VM görüntüleri hakkında daha fazla bilgi edinmek için aşağıdaki blog gönderilerine bakın:

- [VM görüntüsü](https://azure.microsoft.com/blog/vm-image-blog-post/)
- [VM görüntüsü PowerShell ' nasıl yapılır '](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="prerequisite-install-the-necessary-tools"></a>Önkoşul: gerekli araçları yükler

Daha önce yapmadıysanız, aşağıdaki yönergeleri kullanarak Azure PowerShell ve Azure CLı 'yi yükleyebilirsiniz:

- [Azure PowerShell yüklensin](https://docs.microsoft.com/powershell/azure/install-Az-ps)
- [Azure CLı 'yı yükler](https://docs.microsoft.com/cli/azure/install-azure-cli)


## <a name="deployment-steps"></a>Dağıtım adımları

Bir Kullanıcı VM görüntüsü oluşturmak ve dağıtmak için aşağıdaki adımları kullanacaksınız:

1. Görüntüyü yakalamayı ve genelleştirmeyi gerektiren Kullanıcı VM görüntüsünü oluşturun. 
2. Sertifikaları oluşturun ve yeni bir Azure Key Vault depolayın. VM ile güvenli bir WinRM bağlantısı kurmak için bir sertifika gerekir.  Bir Azure Resource Manager şablonu ve bir Azure PowerShell betiği sağlanır. 
3. Sağlanan şablon ve betiği kullanarak VM 'yi bir Kullanıcı VM görüntüsünden dağıtın.

VM 'niz dağıtıldıktan sonra [VM görüntünüzü onaylamaya](./cpp-certify-vm.md)hazırsınızdır.

1. **Yeni** ' ye tıklayın ve **şablon dağıtımı**için arama yapın, ardından **düzenleyicide kendi şablonunuzu oluşturun**' i seçin.  <br/>
   ![Azure portal 'de VHD dağıtım şablonu oluşturma](./media/publishvm_021.png)

1. Bu [JSON şablonunu](./cpp-deploy-json-template.md) kopyalayıp düzenleyiciye yapıştırın ve **Kaydet**' e tıklayın. <br/>
   ![VHD Dağıtım şablonunu Azure portal Kaydet](./media/publishvm_022.png)

1. Görüntülenmiş **özel dağıtım** özelliği sayfaları için parametre değerlerini belirtin.

   <table> <tr> <td valign="top"> <img src="./media/publishvm_023.png" alt="Custom deployment property page 1"> </td> <td valign="top"> <img src="./media/publishvm_024.png" alt="Custom deployment property page 2"> </td> </tr> </table> <br/> 

   |  **Parametre**              |   **Açıklama**                                                            |
   |  -------------              |   ---------------                                                            |
   | Kullanıcı depolama hesabı adı   | Genelleştirilmiş VHD 'nin bulunduğu depolama hesabı adı                    |
   | Kullanıcı depolama kapsayıcısı adı | Genelleştirilmiş VHD 'nin bulunduğu kapsayıcı adı                          |
   | Genel IP için DNS adı      | Genel IP DNS adı. DNS adı, VM 'nin, teklif dağıtıldıktan sonra bunu Azure portalında tanımlayacaksınız.  |
   | Yönetici Kullanıcı adı             | Yeni VM için yönetici hesabının Kullanıcı adı                                  |
   | Yönetici Parolası              | Yeni VM için yönetici hesabının parolası                                  |
   | İşletim Sistemi Türü                     | VM işletim sistemi: `Windows` \|`Linux`                                    |
   | Abonelik Kimliği             | Seçili aboneliğin tanımlayıcısı                                      |
   | Konum                    | Dağıtımın coğrafi konumu                                        |
   | VM Boyutu                     | [Azure VM boyutu](https://docs.microsoft.com/azure/virtual-machines/windows/sizes), örneğin`Standard_A2` |
   | Genel IP adresi adı      | Genel IP adresinizin adı                                               |
   | VM Adı                     | Yeni VM 'nin adı                                                           |
   | Sanal ağ adı        | VM tarafından kullanılan sanal ağın adı                                   |
   | NIC adı                    | Sanal ağı çalıştıran ağ arabirimi kartının adı               |
   | VHD URL 'SI                     | İşletim sistemi diski VHD URL 'SI                                                     |
   |  |  |
            
1. Bu değerleri girdikten sonra **satın al**' a tıklayın. 

Azure dağıtıma başlayacak: belirtilen depolama hesabı yolunda belirtilen yönetilmeyen VHD 'ye sahip yeni bir VM oluşturur.  Portalın sol tarafındaki **sanal makinelere** tıklayarak Azure Portal ilerleme durumunu izleyebilirsiniz.  VM oluşturulduğunda, durumu `Starting` olarak `Running`değişir. 


### <a name="deploy-a-vm-from-powershell"></a>PowerShell 'den VM dağıtma

Yeni oluşturulan Genelleştirilmiş VM görüntüsünden büyük bir VM dağıtmak için aşağıdaki cmdlet 'leri kullanın.

``` powershell
    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot
```


## <a name="next-steps"></a>Sonraki adımlar

Ardından, çözümünüz için [bir Kullanıcı VM görüntüsü oluşturacaksınız](cpp-create-user-image.md) .

