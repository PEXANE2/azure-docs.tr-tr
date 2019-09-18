---
title: Azure özel uç noktası kullanarak bir depolama hesabına özel olarak bağlanma
description: Özel bir uç nokta kullanarak Azure 'daki bir depolama hesabına özel olarak nasıl bağlanacağınızı öğrenin.
services: virtual-network
author: KumudD
ms.service: virtual-network
ms.topic: article
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: 93ca3c8c4b7a4da0387d1302b301cf677b689281
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71074965"
---
# <a name="connect-privately-to-a-storage-account-using-an-azure-private-endpoint"></a>Azure özel uç noktası kullanarak bir depolama hesabına özel olarak bağlanma
Azure özel uç noktası, Azure 'da özel bağlantı için temel yapı taşdır. Sanal makineler (VM) gibi Azure kaynaklarının özel bağlantı kaynaklarıyla özel olarak iletişim kurmasına olanak sağlar.

Bu hızlı başlangıçta, bir Azure sanal ağında, Azure portal kullanarak özel uç nokta olan bir depolama hesabı olan bir VM oluşturmayı öğreneceksiniz. Daha sonra, depolama hesabına VM 'den güvenli bir şekilde erişebilirsiniz.


## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.

## <a name="create-a-vm"></a>VM oluşturma
Bu bölümde, özel bağlantı kaynağına (Bu örnekteki bir depolama hesabı) erişmek için kullanılan VM 'yi barındırmak için sanal ağ ve alt ağ oluşturacaksınız.

### <a name="create-the-virtual-network"></a>Sanal ağı oluşturma

Bu bölümde, özel bağlantı kaynağına erişmek için kullanılan VM 'yi barındırmak için sanal ağ ve alt ağ oluşturacaksınız.

1. Ekranın sol üst kısmında **kaynak** > oluştur**ağ** > **sanal ağ**' ı seçin.
1. **Sanal ağ oluştur**' da bu bilgileri girin veya seçin:

    | Ayar | Value |
    | ------- | ----- |
    | Name | *MyVirtualNetwork*girin. |
    | Adres alanı | *10.1.0.0/16*girin. |
    | Subscription | Aboneliğinizi seçin.|
    | Resource group | **Yeni oluştur**' u seçin, *myresourcegroup*yazın ve ardından **Tamam**' ı seçin. |
    | Location | **WestCentralUS**öğesini seçin.|
    | Alt ağ adı | *Mysubnet*yazın. |
    | Alt Ağ - Adres aralığı | *10.1.0.0/24*girin. |
    |||
1. Rest 'i varsayılan olarak bırakın ve **Oluştur**' u seçin.


### <a name="create-virtual-machine"></a>Sanal makine oluşturma

1. Azure Portal ekranın sol üst tarafında **kaynak** > oluştur**işlem** > **sanal makinesi**' ni seçin.

1. **Sanal makine oluşturma-temel bilgiler**bölümünde, bu bilgileri girin veya seçin:

    | Ayar | Value |
    | ------- | ----- |
    | **PROJE AYRINTILARI** | |
    | Subscription | Aboneliğinizi seçin. |
    | Resource group | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz.  |
    | **ÖRNEK AYRINTILARI** |  |
    | Sanal makine adı | *Myvm*' i girin. |
    | Bölge | **WestCentralUS**öğesini seçin. |
    | Kullanılabilirlik seçenekleri | Varsayılan **altyapı yedekliliği gerekli değildir**. |
    | Image | **Windows Server 2019 Datacenter**öğesini seçin. |
    | Size | Varsayılan **Standart DS1 v2**' i bırakın. |
    | **YÖNETICI HESABI** |  |
    | Kullanıcı Adı | Seçmekten bir Kullanıcı adı girin. |
    | istemcisiyle yönetilen bir cihaz için) | Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    | Parolayı Onayla | Parolayı yeniden girin. |
    | **GELEN BAĞLANTI NOKTASI KURALLARI** |  |
    | Ortak gelen bağlantı noktaları | Varsayılanı **yok**olarak bırakın. |
    | **TASARRUF EDIN** |  |
    | Zaten bir Windows lisansınız var mı? | Varsayılan **Hayır**olarak bırakın. |
    |||

1. İleri **' yi seçin: Diskler**.

1. **Sanal makine oluşturma-diskler**' de, varsayılan değerleri bırakın ve İleri **' yi seçin: Ağ**iletişimi.

1. **Sanal makine oluşturma-ağ oluşturma**bölümünde şu bilgileri seçin:

    | Ayar | Value |
    | ------- | ----- |
    | Sanal ağ | Varsayılan **MyVirtualNetwork**bırakın.  |
    | Adres alanı | Varsayılan **10.1.0.0/24**' i bırakın.|
    | Subnet | Varsayılan **Mysubnet (10.1.0.0/24)** olarak bırakın.|
    | Genel IP | Varsayılan **(yeni) myVm-ip**' i bırakın. |
    | Ortak gelen bağlantı noktaları | **Seçili bağlantı noktalarına Izin ver**' i seçin. |
    | Gelen bağlantı noktalarını seçin | **Http** ve **RDP**' yi seçin.|
    ||

1. **İncele ve oluştur**’u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz.

1. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin.

## <a name="create-your-private-endpoint"></a>Özel uç noktanızı oluşturma
Bu bölümde, için özel bir uç nokta kullanarak özel bir depolama hesabı oluşturacaksınız. 

1. Azure Portal ekranın sol üst tarafında, **kaynak** > **depolama** > **depolama hesabı**oluştur ' u seçin.

1. **Depolama hesabı oluşturma-temel**bilgiler bölümünde, bu bilgileri girin veya seçin:

    | Ayar | Value |
    | ------- | ----- |
    | **PROJE AYRINTILARI** | |
    | Subscription | Aboneliğinizi seçin. |
    | Resource group | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz.|
    | **ÖRNEK AYRINTILARI** |  |
    | Depolama hesabı adı  | *Mystorageaccount*değerini girin. Bu ad alındıysanız, benzersiz bir ad oluşturun. |
    | Bölge | **WestCentralUS**öğesini seçin. |
    | Performans| Varsayılan **Standart**bırakın. |
    | Hesap türü | Varsayılan **depolama alanını (genel amaçlı v2)** bırakın. |
    | Çoğaltma | **Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS)** seçeneğini belirleyin. |
    |||
  
3. İleri **' yi seçin: Ağ**iletişimi.
4.  **Depolama hesabı oluşturma-ağ**, bağlantı yöntemi ' nde, **Özel uç nokta**' ı seçin.
5.  **Depolama hesabı oluştur-ağ**' da, **Özel uç nokta Ekle**' yi seçin. 
6.  **Özel uç nokta oluştur**' da bu bilgileri girin veya seçin:

    | Ayar | Value |
    | ------- | ----- |
    | **PROJE AYRINTILARI** | |
    | Subscription | Aboneliğinizi seçin. |
    | Resource group | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz.|
    |Location|**WestCentralUS**öğesini seçin.|
    |Name| *Myprivateendpoint*girin.  |
    |Depolama alt kaynağı|Varsayılan **blobu**bırakın. |
    | **IŞLEMLERI** |  |
    | Sanal ağ  | *Myresourcegroup*kaynak grubundan *MyVirtualNetwork* öğesini seçin. |
    | Subnet |  *Mysubnet*öğesini seçin. |
    | **ÖZEL DNS TÜMLEŞTIRMESI**|  |
    | Özel DNS bölgesiyle tümleştirin  | Varsayılan **Evet**' i bırakın. |
    | Özel DNS bölgesi  | **Varsayılan (yeni) Privatelink.blob.Core.Windows.net**bırakın. |
    |||
7.  **Tamam ' ı**seçin. 
8.  **Gözden geçir + oluştur**' u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz. 
9.  **Doğrulama başarılı** iletisini gördüğünüzde **Oluştur**' u seçin. 
10. Yeni oluşturduğunuz depolama hesabı kaynağına gidin.
11. Sol içerik menüsünden **erişim tuşları** ' nı seçin.
12. KEY1 için bağlantı dizesinde **Kopyala** ' yı seçin.
 
## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Aşağıdaki gibi, internet *'ten gelen VM VM* 'sine bağlanın:

1. Portalın arama çubuğunda *Myvm*' i girin.

1. **Bağlan** düğmesini seçin. **Bağlan** düğmesini seçtikten sonra **sanal makineye bağlan** açılır.

1. Seçin **RDP dosyasını indir**. Azure bir Uzak Masaüstü Protokolü ( *. rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. İndirilen. rdp * dosyasını açın.

    1. İstendiğinde **Bağlan**’ı seçin.

    1. VM oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

        > [!NOTE]
        > VM oluştururken girdiğiniz kimlik bilgilerini belirtmek için**farklı bir hesap kullan**' **ı seçmeniz** > gerekebilir.

1. **Tamam**’ı seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız **Evet** ' i veya **devam et**' i seçin.

1. VM masaüstü seçildikten sonra, bunu yerel masaüstünüze geri dönmek için simge durumuna küçültün.  

## <a name="access-storage-account-privately-from-the-vm"></a>Depolama hesabına özel olarak VM 'den erişin

Bu bölümde, Özel uç nokta kullanarak, depolama hesabına özel olarak bağlanacaksınız.

> [!IMPORTANT]
> Depolama için DNS yapılandırması, belirli bir hesabın FQDN 'sini içermesi için konaklar dosyasında el ile bir değişikliğe ihtiyaç duyuyor. lütfen Windows üzerinde yönetici izinlerini kullanarak şu dosyayı değiştirin: c:\Windows\System32\Drivers\etc\hosts veya Linux/etc/hosts Önceki adımdan hesabın DNS bilgilerini şu biçimde ekleyin [özel IP adresi] myaccount.blob.core.windows.net

1.  *Myvm*uzak masaüstünde PowerShell ' i açın.
2.  `nslookup mystorageaccount.blob.core.windows.net` Şuna benzer bir ileti alacaksınız:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mystorageaccount123123.privatelink.blob.core.windows.net
    Address:  10.0.0.5
    Aliases:  mystorageaccount.blob.core.windows.net
3. Install [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=windows).
4. Select **Storage accounts** with the right-click.
5. Select **Connect to an azure storage**.
6. Select **Use a connection string**.
7. Select **Next**.
8. Enter the connection string by pasting the information previously copied.
9. Select **Next**.
10. Select **Connect**.
11. Browse the Blob containers from mystorageaccount 
12. (Optionally) Create folders and/or upload files to *mystorageaccount*. 
13. Close the remote desktop connection to *myVM*. 

Additional options to access the storage account:
- Microsoft Azure Storage Explorer is a standalone free app from Microsoft that enables you to work visually with Azure storage data on Windows, macOS, and Linux. You can install the application to browse privately the storage account content. 
 
- The AzCopy utility is another option for high-performance scriptable data transfer for Azure storage. Use AzCopy to transfer data to and from Blob, File, and Table storage. 


## Clean up resources 
When you're done using the Private Endpoint, storage account and the VM, delete the resource group and all of the resources it contains: 
1. Enter *myResourceGroup* in the **Search** box at the top of the portal and select *myResourceGroup* from the search results. 
2. Select **Delete resource group**. 
3. Enter *myResourceGroup* for **TYPE THE RESOURCE GROUP NAME** and select **Delete**. 

## Next steps
In this Quickstart, you created a VM on a virtual network and storage account and a Private Endpoint. You connected to one VM from the internet and securely communicated to the storage account using Private Link. To learn more about Private Endpoint, see [What is Azure Private Endpoint?](private-endpoint-overview.md).
