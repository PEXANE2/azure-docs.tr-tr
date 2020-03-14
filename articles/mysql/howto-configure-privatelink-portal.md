---
title: Özel bağlantı-Azure portal-MySQL için Azure veritabanı
description: Azure portal 'ten MySQL için Azure veritabanı için özel bağlantıyı yapılandırmayı öğrenin
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 01/09/2020
ms.openlocfilehash: 4a4824a9f8340b12bca7e18562d723eb24e58b71
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79371928"
---
# <a name="create-and-manage-private-link-for-azure-database-for-mysql-using-portal"></a>Portal kullanarak MySQL için Azure veritabanı için özel bağlantı oluşturma ve yönetme

Özel uç nokta, Azure 'da özel bağlantı için temel yapı taşdır. Sanal makineler (VM) gibi Azure kaynaklarının özel bağlantı kaynaklarıyla özel olarak iletişim kurmasına olanak sağlar. Bu makalede, Azure sanal ağında bir sanal makıne oluşturmak için Azure portal kullanmayı ve Azure özel uç noktası ile MySQL için Azure veritabanı sunucusunu nasıl kullanacağınızı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> Bu özellik, MySQL için Azure veritabanı 'nın Genel Amaçlı ve bellek için Iyileştirilmiş fiyatlandırma katmanlarını desteklediği tüm Azure bölgelerinde kullanılabilir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="create-an-azure-vm"></a>Azure VM oluşturma

Bu bölümde, özel bağlantı kaynağına (Azure 'da bir MySQL sunucusu) erişmek için kullanılan VM 'yi barındırmak için sanal ağ ve alt ağ oluşturacaksınız.

### <a name="create-the-virtual-network"></a>Sanal ağı oluşturma
Bu bölümde, özel bağlantı kaynağına erişmek için kullanılan VM 'yi barındırmak için bir sanal ağ ve alt ağ oluşturacaksınız.

1. Ekranın sol üst kısmında, **kaynak oluştur** > **ağ** > **sanal ağ**' ı seçin.
2. **Sanal ağ oluştur**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *MyVirtualNetwork*girin. |
    | Adres alanı | *10.1.0.0/16*girin. |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | **Yeni oluştur**' u seçin, *myresourcegroup*yazın ve ardından **Tamam**' ı seçin. |
    | Konum | **Batı Avrupa**'yı seçin.|
    | Alt ağ adı | *Mysubnet*yazın. |
    | Alt Ağ - Adres aralığı | *10.1.0.0/24*girin. |
    |||
3. Rest 'i varsayılan olarak bırakın ve **Oluştur**' u seçin.

### <a name="create-virtual-machine"></a>Sanal Makine Oluştur

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
    | Görüntü | **Windows Server 2019 Datacenter**öğesini seçin. |
    | Boyut | Varsayılan **Standart DS1 v2**' i bırakın. |
    | **YÖNETICI HESABı** |  |
    | Kullanıcı adı | Seçmekten bir Kullanıcı adı girin. |
    | Parola | Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    | Parolayı Onayla | Parolayı yeniden girin. |
    | **GELEN BAĞLANTı NOKTASı KURALLARı** |  |
    | Ortak gelen bağlantı noktaları | Varsayılanı **yok**olarak bırakın. |
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
    | Ortak gelen bağlantı noktaları | **Seçili bağlantı noktalarına Izin ver**' i seçin. |
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

1. Ekranın sol üst kısmında Azure portal **kaynak oluştur** > **ağ** > **özel bağlantı**' yı seçin.

2. **Özel bağlantı merkezi 'Ne genel bakış**' da, **bir hizmete özel bağlantı oluşturma**seçeneğinde, **Başlat**' ı seçin.

    ![Özel bağlantıya genel bakış](media/concepts-data-access-and-security-private-link/privatelink-overview.png)

1. **Özel uç nokta oluşturma-temel**bilgiler için, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **Proje ayrıntıları** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz.|
    | **Örnek ayrıntıları** |  |
    | Adı | *Myprivateendpoint*girin. Bu ad alındıysanız, benzersiz bir ad oluşturun. |
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
8. **Özel uç nokta oluşturma-yapılandırma**' da bu bilgileri girin veya seçin:

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

    > [!NOTE] 
    > Müşteri DNS ayarındaki FQDN, yapılandırılan özel IP 'ye çözümlenmez. [Burada](../dns/dns-operations-recordsets-portal.md)gösterildiği gibi, yapılandırılmış FQDN IÇIN bir DNS bölgesi oluşturmanız gerekir.

## <a name="connect-to-a-vm-using-remote-desktop-rdp"></a>Uzak Masaüstü (RDP) kullanarak bir VM 'ye bağlanma


**Myvm**'yi oluşturduktan sonra Internet 'ten şu şekilde bağlanın: 

1. Portalın arama çubuğunda *Myvm*' i girin.

1. **Bağlan** düğmesini seçin. **Bağlan** düğmesini seçtikten sonra **sanal makineye bağlan** açılır.

1. **RDP dosyasını indir**' i seçin. Azure bir Uzak Masaüstü Protokolü ( *. rdp*) dosyası oluşturur ve bilgisayarınıza indirir.

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
    ```

3. Kullanılabilir herhangi bir istemciyi kullanarak MySQL sunucusu için özel bağlantı bağlantısını test edin. Aşağıdaki örnekte, işlemi yapmak için [MySQL çalışma ekranı](https://dev.mysql.com/doc/workbench/en/wb-installing-windows.html) kullandım.

4. **Yeni bağlantı**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Sunucu türü| **MySQL**öğesini seçin.|
    | Sunucu adı| *Myserver.Privatelink.MySQL.Database.Azure.com* seçin |
    | Kullanıcı adı | MySQL sunucusu oluşturma sırasında belirtilen username@servername Kullanıcı adını girin. |
    |Parola |MySQL sunucusu oluşturma sırasında bir parola girin. |
    |SSL|**Gerekli**' yi seçin.|
    ||

5. Bağlan ' ı seçin.

6. Sol menüden veritabanlarına gözatamazsınız.

7. I MySQL sunucusundan bilgi oluşturun veya sorgulayın.

8. MyVm ile uzak masaüstü bağlantısını kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Özel uç nokta, MySQL Server ve VM 'yi kullanarak işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Portalın üst kısmındaki **arama** kutusuna *myresourcegroup* girin ve arama sonuçlarından *myresourcegroup* öğesini seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **Kaynak grubu adını yazın** ve **Sil**' i seçmek için myresourcegroup girin.

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır, bir sanal ağ üzerinde bir VM, MySQL için Azure veritabanı ve özel erişim için özel bir uç nokta oluşturdunuz. İnternet 'ten bir VM 'ye bağlanırsınız ve özel bağlantı kullanarak MySQL sunucusuna güvenli bir şekilde iletilecaksınız. Özel uç noktalar hakkında daha fazla bilgi için bkz. [Azure özel uç noktası nedir?](https://docs.microsoft.com/azure/private-link/private-endpoint-overview).
