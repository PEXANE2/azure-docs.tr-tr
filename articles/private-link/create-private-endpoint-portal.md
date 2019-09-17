---
title: Azure 'da özel uç noktaları yönetme
description: Azure portal kullanarak özel uç nokta oluşturmayı öğrenin
services: virtual-network
author: KumudD
ms.service: virtual-network
ms.topic: quickstart
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: c0ade6b0728717cfb8524eea92b63b49a979ec1f
ms.sourcegitcommit: 71db032bd5680c9287a7867b923bf6471ba8f6be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71018960"
---
# <a name="create-a-private-endpoint-using-azure-portal"></a>Azure portal kullanarak özel uç nokta oluşturma

Özel uç nokta, Azure 'da özel bağlantı için temel yapı taşdır. Sanal makineler (VM) gibi Azure kaynaklarının özel bağlantı kaynaklarıyla özel olarak iletişim kurmasına olanak sağlar. Bu hızlı başlangıçta, Azure PowerShell kullanarak Azure özel uç noktası olan bir SQL veritabanı sunucusu olan bir Azure sanal ağında bir VM oluşturmayı öğreneceksiniz. Ardından, VM 'den SQL veritabanı sunucusuna güvenle erişebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.

## <a name="create-a-vm"></a>VM oluşturma
Bu bölümde, özel bağlantı kaynağına erişmek için kullanılan VM 'yi barındırmak için sanal ağ ve alt ağ oluşturacaksınız (Bu örnekteki Azure 'da bir SQL Server).

### <a name="create-the-virtual-network"></a>Sanal ağı oluşturma


Bu bölümde, özel bağlantı kaynağına erişmek için kullanılan VM 'yi barındırmak için bir sanal ağ ve alt ağ oluşturacaksınız.

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


### <a name="create-virtual-machine"></a>Sanal Makine Oluştur

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
    |||


1. **İncele ve oluştur**’u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz.

1. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin.

## <a name="create-a-sql-database-server"></a>SQL veritabanı sunucusu oluşturma
Bu bölümde, Azure 'da bir SQL veritabanı sunucusu oluşturacaksınız. 

1. Ekranın sol üst tarafında Azure Portal **kaynak** > **veritabanları** > oluştur**SQL veritabanı**' nı seçin.

1. **SQL veritabanı oluşturma-temel**bilgiler bölümünde, bu bilgileri girin veya seçin:

    | Ayar | Value |
    | ------- | ----- |
    | **Veritabanı ayrıntıları** | |
    | Subscription | Aboneliğinizi seçin. |
    | Resource group | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz.|
    | **ÖRNEK AYRINTILARI** |  |
    | Veritabanı adı  | *MyDatabase*yazın. Bu ad alındıysanız, benzersiz bir ad oluşturun. |
    |||
5.  **Sunucu**' da, **Yeni oluştur**' u seçin. 
6.  **Yeni sunucu**' da bu bilgileri girin veya seçin:

    | Ayar | Value |
    | ------- | ----- |
    |Sunucu adı  | *Sunucum*girin. Bu ad alındıysanız, benzersiz bir ad oluşturun.|
    | Sunucu yöneticisi oturum açma bilgileri| Tercih etmek için bir yönetici adı girin. |
    | istemcisiyle yönetilen bir cihaz için) | Seçtiğiniz bir parolayı girin. Parola en az 8 karakter uzunluğunda olmalı ve tanımlanan gereksinimleri karşılamalıdır. |
    | Location | SQL Server bulunmasını istediğiniz bir Azure bölgesi seçin. |
    
7.  **Tamam ' ı**seçin. 
8.  **Gözden geçir + oluştur**' u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz. 
9. Doğrulama başarılı iletisini gördüğünüzde **Oluştur**' u seçin. 
10. Doğrulama başarılı iletisini gördüğünüzde oluştur ' u seçin. 

## <a name="create-a-private-endpoint"></a>Özel uç nokta oluşturma

Bu bölümde, bir SQL Server oluşturacaksınız ve kendisine özel bir uç nokta ekleyeceksiniz. 

1. Azure Portal ekranın sol üst tarafında, **kaynak** > oluştur**ağ** > **özel bağlantı merkezi (Önizleme)** öğesini seçin.
2.  **Özel bağlantı merkezi 'Ne genel bakış**' da, **bir hizmete özel bağlantı oluşturma**seçeneğinde, **Başlat**' ı seçin.
1. **Özel uç nokta oluştur (Önizleme)-temel**bilgiler için, bu bilgileri girin veya seçin:

    | Ayar | Value |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Subscription | Aboneliğinizi seçin. |
    | Resource group | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz.|
    | **ÖRNEK AYRINTILARI** |  |
    | Name | * Myprivateendpoint*girin. Bu ad alındıysanız, benzersiz bir ad oluşturun. |
    |Bölge|**WestCentralUS**öğesini seçin.|
    |||
5. İleri **' yi seçin: Kaynak**.
6.  **Özel uç nokta oluştur-kaynak**bölümünde bu bilgileri girin veya seçin:

    | Ayar | Value |
    | ------- | ----- |
    |Bağlantı yöntemi  | Dizinimde bir Azure kaynağına bağlan ' ı seçin.|
    | Subscription| Aboneliğinizi seçin. |
    | Kaynak türü |  **Microsoft. SQL/Servers**' ı seçin. |
    | Resource |*Sunucum* seçin|
    |Hedef alt kaynak |*SqlServer* seçin|
    |||
7. İleri **' yi seçin: Yapılandırma**.
8. **Özel uç nokta (Önizleme) oluştur-yapılandırma**' da bu bilgileri girin veya seçin:

    | Ayar | Value |
    | ------- | ----- |
    |**IŞLEMLERI**| |
    | Sanal ağ| *MyVirtualNetwork*öğesini seçin. |
    | Subnet |  *Mysubnet*öğesini seçin. |
    |**ÖZEL DNS TÜMLEŞTIRMESI**||
    |Özel DNS bölgesiyle tümleştirin | **Evet**' i seçin. |
    |Özel DNS bölgesi |Seçin *(yeni) Privatelink. Database. Windows. net* |
    |||

1.  **Gözden geçir + oluştur**' u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz. 
2.  **Doğrulama başarılı** iletisini gördüğünüzde **Oluştur**' u seçin. 
 
## <a name="connect-to-a-vm-using-remote-desktop-rdp"></a>Uzak Masaüstü (RDP) kullanarak bir VM 'ye bağlanma


**Myvm*oluşturduktan sonra, internet 'ten aşağıdaki gibi bağlanın: 

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

## <a name="access-the-sql-database-server-privately-from-the-vm"></a>SQL veritabanı sunucusuna VM 'den özel olarak erişme

1.  *Myvm*uzak masaüstünde PowerShell ' i açın.
2. Girin `myserver.database.windows.net`. 
' Şuna benzer bir ileti alacaksınız:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myserver.privatelink.database.windows.net
    Address:  10.0.0.5
    Aliases:   myserver.database.windows.net
3. Install [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017).

4. In **Connect to server**, enter or select this information:

    | Setting | Value |
    | ------- | ----- |
    | Server type| Select **Database Engine**.|
    | Server name| Select *myserver.database.windows.net* |
    | User name | Enter a password provided during the SQL server creation. |
    |Password |Enter a password provided during the SQL server creation. |
    |Remember password|Select **Yes**.|
    |||
1. Select **Connect**.
2. Browse databases from left menu.
3. (Optionally) Create or query information from mydatabase.
4. Close the remote desktop connection to *myVm*. 

## Clean up resources 
When you're done using the private endpoint, SQL server, and the VM, delete the resource group and all of the resources it contains: 
1. Enter *myResourceGroup* in the **Search** box at the top of the portal and select *myResourceGroup* from the search results. 
2. Select **Delete resource group**. 
3. Enter myResourceGroup for **TYPE THE RESOURCE GROUP NAME** and select **Delete**.

## Next steps

In this quickstart, you created a VM on a virtual network, a SQL database server, and a private endpoint for private access. You connected to one VM from the internet and securely communicated to the SQL database server using Private Link. To learn more about private endpoints, see [What is Azure private endpoint?](private-endpoint-overview.md).

