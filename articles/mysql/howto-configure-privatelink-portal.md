---
title: MySQL için Azure veritabanı (Önizleme) portalı kurulum yöntemi için özel bağlantı
description: Azure portal 'ten MySQL için Azure veritabanı için özel bağlantıyı yapılandırmayı öğrenin
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: fa8bc56376704b96f5ddee09db7b09e28f10a936
ms.sourcegitcommit: d9ec6e731e7508d02850c9e05d98d26c4b6f13e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2020
ms.locfileid: "76281249"
---
# <a name="create-and-manage-private-link-for-azure-database-for-mysql-preview-using-portal"></a>Portal kullanarak MySQL için Azure veritabanı (Önizleme) için özel bağlantı oluşturma ve yönetme

Özel uç nokta, Azure 'da özel bağlantı için temel yapı taşdır. Sanal makineler (VM) gibi Azure kaynaklarının özel bağlantı kaynaklarıyla özel olarak iletişim kurmasına olanak sağlar. Bu makalede, Azure sanal ağında bir sanal makıne oluşturmak için Azure portal kullanmayı ve Azure özel uç noktası ile MySQL için Azure veritabanı sunucusunu nasıl kullanacağınızı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> Bu özellik, MySQL için Azure veritabanı 'nın Genel Amaçlı ve bellek için Iyileştirilmiş fiyatlandırma katmanlarını desteklediği tüm Azure bölgelerinde kullanılabilir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın
[Azure Portal](https://portal.azure.com)’ında oturum açın.

## <a name="create-an-azure-vm"></a>Azure VM oluşturma

Bu bölümde, özel bağlantı kaynağına (Azure 'da bir MySQL sunucusu) erişmek için kullanılan VM 'yi barındırmak için sanal ağ ve alt ağ oluşturacaksınız.

### <a name="create-the-virtual-network"></a>Sanal ağı oluşturma
Bu bölümde, özel bağlantı kaynağına erişmek için kullanılan VM 'yi barındırmak için bir sanal ağ ve alt ağ oluşturacaksınız.

1. Ekranın sol üst kısmında, **kaynak oluştur** > **ağ** > **sanal ağ**' ı seçin.
2. **Sanal ağ oluştur**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad | *MyVirtualNetwork*girin. |
    | Adres alanı | *10.1.0.0/16*girin. |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | **Yeni oluştur**' u seçin, *myresourcegroup*yazın ve ardından **Tamam**' ı seçin. |
    | Konum | **Batı Avrupa**'yı seçin.|
    | Alt ağ adı | *Mysubnet*yazın. |
    | Alt Ağ - Adres aralığı | *10.1.0.0/24*girin. |
    |||
3. Rest 'i varsayılan olarak bırakın ve **Oluştur**' u seçin.

### <a name="create-virtual-machine"></a>Sanal Makine oluşturun

1. Azure portal ekranın sol üst kısmında, **sanal makine** > **Işlem** > **kaynak oluştur** ' u seçin.

2. **Sanal makine oluşturma-temel bilgiler**bölümünde, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE AYRıNTıLARı** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz.  |
    | **ÖRNEK AYRıNTıLARı** |  |
    | Sanal makine adı | *Myvm*' i girin. |
    | Bölge | **Batı Avrupa**'yı seçin. |
    | Kullanılabilirlik seçenekleri | Varsayılan **altyapı yedekliliği gerekli değildir**. |
    | Resim | **Windows Server 2019 Datacenter**öğesini seçin. |
    | Boyut | Varsayılan **Standart DS1 v2**' i bırakın. |
    | **YÖNETICI HESABı** |  |
    | Kullanıcı adı | Seçmekten bir Kullanıcı adı girin. |
    | Parola | Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    | Parolayı Onayla | Parolayı yeniden girin. |
    | **GELEN BAĞLANTı NOKTASı KURALLARı** |  |
    | Genel gelen bağlantı noktası | Varsayılanı **yok**olarak bırakın. |
    | **TASARRUF EDIN** |  |
    | Zaten bir Windows lisansınız var mı? | Varsayılan **Hayır**olarak bırakın. |
    |||

1. **İleri ' yi seçin: diskler**.

1. **Sanal makine oluşturma-diskler**' de, varsayılan değerleri bırakın ve **İleri ' yi seçin: ağ**.

1. **Sanal makine oluşturma-ağ oluşturma**bölümünde şu bilgileri seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Sanal ağ | Varsayılan **MyVirtualNetwork**bırakın.  |
    | Adres alanı | Varsayılan **10.1.0.0/24**' i bırakın.|
    | Alt ağ | Varsayılan **Mysubnet (10.1.0.0/24)** olarak bırakın.|
    | Genel IP | Varsayılan **(yeni) myVm-ip**' i bırakın. |
    | Genel gelen bağlantı noktası | **Seçili bağlantı noktalarına Izin ver**' i seçin. |
    | Gelen bağlantı noktalarını seçin | **Http** ve **RDP**' yi seçin.|
    |||


1. **İncele ve oluştur**’u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz.

1. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin.

## <a name="create-an-azure-database-for-mysql"></a>MySQL için Azure Veritabanı oluşturma

Bu bölümde, Azure 'da MySQL için Azure veritabanı sunucusu oluşturacaksınız. 

1. Azure portal ekranın sol üst tarafında, **kaynak oluştur** > **veritabanları** > **MySQL için Azure veritabanı**' nı seçin.

1. **MySQL Için Azure veritabanı** 'nda şu bilgileri sağlayın:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz.|
    | **Sunucu Ayrıntıları** |  |
    |Sunucu adı  | *Sunucum*girin. Bu ad alındıysanız, benzersiz bir ad oluşturun.|
    | Yönetici Kullanıcı adı| Tercih etmek için bir yönetici adı girin. |
    | Parola | Seçtiğiniz bir parolayı girin. Parola en az 8 karakter uzunluğunda olmalı ve tanımlanan gereksinimleri karşılamalıdır. |
    | Konum | MySQL sunucunuzun bulunmasını istediğiniz bir Azure bölgesi seçin. |
    |Sürüm  | Gerekli olan MySQL sunucusunun veritabanı sürümünü seçin.|
    | İşlem + depolama| İş yüküne göre sunucu için gereken fiyatlandırma katmanını seçin. |
    |||
 
7. **Tamam**’ı seçin. 
8. **İncele ve oluştur**’u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz. 
9. Doğrulama başarılı iletisini gördüğünüzde **Oluştur**' u seçin. 
10. Doğrulama başarılı iletisini gördüğünüzde oluştur ' u seçin. 

## <a name="create-a-private-endpoint"></a>Özel uç nokta oluşturma

Bu bölümde bir MySQL sunucusu oluşturacak ve kendisine özel bir uç nokta ekleyeceksiniz. 

1. Ekranın sol üst kısmında Azure portal **kaynak oluştur** > **ağ** > **özel bağlantı merkezi (Önizleme)** seçeneğini belirleyin.

2. **Özel bağlantı merkezi 'Ne genel bakış**' da, **bir hizmete özel bağlantı oluşturma**seçeneğinde, **Başlat**' ı seçin.

    ![Özel bağlantıya genel bakış](media/concepts-data-access-and-security-private-link/privatelink-overview.png)

1. **Özel uç nokta oluştur (Önizleme)-temel**bilgiler için, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz.|
    | **Örnek ayrıntıları** |  |
    | Ad | *Myprivateendpoint*girin. Bu ad alındıysanız, benzersiz bir ad oluşturun. |
    |Bölge|**Batı Avrupa**'yı seçin.|
    |||

5. **Sonraki: kaynak**' ı seçin.
6. **Özel uç nokta oluştur-kaynak**bölümünde bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |Bağlantı yöntemi  | Dizinimde bir Azure kaynağına bağlan ' ı seçin.|
    | Abonelik| Aboneliğinizi seçin. |
    | Kaynak türü | **Microsoft. Dbformyısql/sunucuları**' nı seçin. |
    | Kaynak |*Sunucum* seçin|
    |Hedef alt kaynak |*MySQLServer* seçin|
    |||
7. Ileri 'yi seçin **: yapılandırma**.
8. **Özel uç nokta (Önizleme) oluştur-yapılandırma**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |**IŞLEMLERI**| |
    | Sanal ağ| *MyVirtualNetwork*öğesini seçin. |
    | Alt ağ |  *Mysubnet*öğesini seçin. |
    |**ÖZEL DNS TÜMLEŞTIRMESI**||
    |Özel DNS bölgesiyle tümleştirin |**Evet**’i seçin. |
    |Özel DNS bölgesi |Seç *(yeni) Privatelink. MySQL. Database. Azure. com* |
    |||

1. **İncele ve oluştur**’u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz. 
2. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin. 

    ![Özel bağlantı oluşturuldu](media/concepts-data-access-and-security-private-link/show-mysql-private-link.png)

## <a name="connect-to-a-vm-using-remote-desktop-rdp"></a>Uzak Masaüstü (RDP) kullanarak bir VM 'ye bağlanma


**Myvm**'yi oluşturduktan sonra Internet 'ten şu şekilde bağlanın: 

1. Portalın arama çubuğunda *Myvm*' i girin.

1. **Bağlan** düğmesini seçin. **Bağlan** düğmesini seçtikten sonra **sanal makineye bağlan** açılır.

1. Seçin **RDP dosyasını indir**. Azure bir Uzak Masaüstü Protokolü ( *. rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

1. *İndirilen. rdp* dosyasını açın.

    1. İstendiğinde **Bağlan**’ı seçin.

    1. VM oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

        > [!NOTE]
        > VM oluştururken girdiğiniz kimlik bilgilerini belirtmek için **farklı bir hesap kullanmak** > **daha fazla seçenek** belirlemeniz gerekebilir.

1. **Tamam**’ı seçin.

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bir sertifika uyarısı alırsanız **Evet** ' i veya **devam et**' i seçin.

1. VM masaüstü seçildikten sonra, bunu yerel masaüstünüze geri dönmek için simge durumuna küçültün.

## <a name="access-the-mysql-server-privately-from-the-vm"></a>MySQL sunucusuna VM 'den özel olarak erişme

1.  *Myvm*uzak masaüstünde PowerShell ' i açın.

2.  `nslookup  myServer.privatelink.mysql.database.azure.com`girin. 

    Şuna benzer bir ileti alacaksınız:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myServer.privatelink.mysql.database.azure.com
    Address:  10.1.3.4

3. Test the private link connection for the MySQL server using any available client. In the example below I have used [MySQL Workbench](https://dev.mysql.com/doc/workbench/en/wb-installing-windows.html) to do the operation.

4. In **New connection**, enter or select this information:

    | Setting | Value |
    | ------- | ----- |
    | Server type| Select **MySQL**.|
    | Server name| Select *myServer.privatelink.mysql.database.azure.com* |
    | User name | Enter username as username@servername which is provided during the MySQL server creation. |
    |Password |Enter a password provided during the MySQL server creation. |
    |SSL|Select **Required**.|
    ||

5. Select Connect.

6. Browse databases from left menu.

7. (Optionally) Create or query information from the MySQL server.

8. Close the remote desktop connection to myVm.

## Clean up resources
When you're done using the private endpoint, MySQL server, and the VM, delete the resource group and all of the resources it contains:

1. Enter *myResourceGroup* in the **Search** box at the top of the portal and select *myResourceGroup* from the search results.
2. Select **Delete resource group**.
3. Enter myResourceGroup for **TYPE THE RESOURCE GROUP NAME** and select **Delete**.

## Next steps

In this how-to, you created a VM on a virtual network, an Azure Database for MySQL, and a private endpoint for private access. You connected to one VM from the internet and securely communicated to the MySQL server using Private Link. To learn more about private endpoints, see [What is Azure private endpoint](https://docs.microsoft.com/azure/private-link/private-endpoint-overview).
