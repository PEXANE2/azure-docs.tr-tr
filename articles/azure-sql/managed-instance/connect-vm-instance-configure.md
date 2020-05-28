---
title: Azure VM bağlantısını yapılandırma
titleSuffix: Azure SQL Managed Instance
description: Azure sanal makinesinden SQL Server Management Studio kullanarak Azure SQL yönetilen örneğine bağlanın.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, srbozovi, bonova
ms.date: 02/18/2019
ms.openlocfilehash: b94993a67fadad2c31c8787deab49e62dd457512
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84044377"
---
# <a name="quickstart-configure-azure-vm-to-connect-to-an-azure-sql-managed-instance"></a>Hızlı başlangıç: Azure SQL yönetilen örneğine bağlanmak için Azure VM 'yi yapılandırma
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu hızlı başlangıçta, Azure sanal makinesini SQL Server Management Studio (SSMS) kullanarak Azure SQL yönetilen örneğine bağlanmak üzere nasıl yapılandırabileceğiniz gösterilmektedir. 


Bunun yerine bir noktadan siteye bağlantı kullanarak şirket içi istemci bilgisayarından bağlanmayı gösteren hızlı başlangıç için bkz. [Noktadan siteye bağlantı yapılandırma](point-to-site-p2s-configure.md)

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıçta, başlangıç noktası olarak [BIR SQL yönetilen örneği oluşturma](instance-create-quickstart.md) bölümünde oluşturulan kaynaklar kullanılmaktadır.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/) oturum açın.

## <a name="create-a-new-subnet-vnet"></a>Yeni bir alt ağ VNet oluştur

Aşağıdaki adımlar SQL yönetilen örnek VNet 'inde bir Azure sanal makinesinin SQL yönetilen örneğine bağlanabilmesi için yeni bir alt ağ oluşturur. SQL yönetilen örnek alt ağı, SQL yönetilen örneklerine ayrılmıştır. Bu alt ağda Azure sanal makineleri gibi başka herhangi bir kaynak oluşturamazsınız.

1. [SQL yönetilen örneği oluşturma](instance-create-quickstart.md) hızlı başlangıç bölümünde oluşturduğunuz SQL yönetilen örneği için kaynak grubunu açın. SQL yönetilen örneğiniz için sanal ağı seçin.

   ![SQL yönetilen örnek kaynakları](./media/connect-vm-instance-configure/resources.png)

2. **Alt ağları** seçin ve ardından **+ alt ağ** ' ı seçerek yeni bir alt ağ oluşturun.

   ![SQL yönetilen örnek alt ağları](./media/connect-vm-instance-configure/subnets.png)

3. Bu tablodaki bilgileri kullanarak formu doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ---------------- | ----------------- | ----------- |
   | **Adı** | Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming).|
   | **Adres aralığı (CIDR bloğu)** | Geçerli bir Aralık | Bu hızlı başlangıç için varsayılan değer iyidir.|
   | **Ağ güvenlik grubu** | Yok | Bu hızlı başlangıç için varsayılan değer iyidir.|
   | **Yol tablosu** | Yok | Bu hızlı başlangıç için varsayılan değer iyidir.|
   | **Hizmet uç noktaları** | 0 seçili | Bu hızlı başlangıç için varsayılan değer iyidir.|
   | **Alt ağ temsilcisi** | Yok | Bu hızlı başlangıç için varsayılan değer iyidir.|

   ![İstemci VM için yeni SQL yönetilen örnek alt ağı](./media/connect-vm-instance-configure/new-subnet.png)

4. SQL yönetilen örnek VNet 'te bu ek alt ağı oluşturmak için **Tamam ' ı** seçin.

## <a name="create-vm-in-new-subnet"></a>Yeni alt ağda VM oluştur 

Aşağıdaki adımlarda, yeni alt ağda SQL yönetilen örneğine bağlanmak için nasıl sanal makine oluşturacağınız gösterilmektedir.

## <a name="prepare-the-azure-virtual-machine"></a>Azure sanal makinesini hazırlama

SQL yönetilen örneği özel sanal ağınıza yerleştirildiğinden, SQL Server Management Studio veya Azure Data Studio gibi yüklü bir SQL istemci aracı ile Azure VM oluşturmanız gerekir. Bu araç SQL yönetilen örneğine bağlanmanızı ve sorguları yürütmeyi sağlar. Bu hızlı başlangıçta SQL Server Management Studio kullanılmıştır.

Tüm gerekli araçlarla istemci sanal makinesi oluşturmanın en kolay yolu Azure Resource Manager şablonlarını kullanmaktır.

1. Azure portal başka bir tarayıcı sekmesinde oturum açtığınızdan emin olun. Ardından, bir istemci sanal makinesi oluşturmak ve SQL Server Management Studio yüklemek için aşağıdaki düğmeyi seçin:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjovanpop-msft%2Fazure-quickstart-templates%2Fsql-win-vm-w-tools%2F201-vm-win-vnet-sql-tools%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

2. Aşağıdaki tabloda bulunan bilgileri kullanarak formu doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ---------------- | ----------------- | ----------- |
   | **Abonelik** | Geçerli bir abonelik | Yeni kaynaklar oluşturmak için izninizin olduğu bir abonelik olmalıdır. |
   | **Kaynak grubu** |[SQL yönetilen örneği oluşturma](instance-create-quickstart.md) hızlı başlangıç bölümünde belirttiğiniz kaynak grubu.|Bu kaynak grubu, VNet 'in bulunduğu bir kaynak olmalıdır.|
   | **Konum** | Kaynak grubunun konumu | Bu değer, seçili kaynak grubuna göre doldurulur. |
   | **Sanal makine adı**  | Geçerli bir ad | Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming).|
   |**Yönetici Kullanıcı adı**|Geçerli bir Kullanıcı adı|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming). Ayrılmış sunucu düzeyi rol olan gibi "serveradmin" kullanmayın.<br>Bu Kullanıcı adını [, sanal makineye bağlandığınız](#connect-to-virtual-machine)her seferinde kullanırsınız.|
   |**Parola**|Geçerli bir parola|Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.<br>Bu parolayı [, sanal makineye bağlandığınız](#connect-to-virtual-machine)her seferinde kullanırsınız.|
   | **Sanal makine boyutu** | Geçerli boyut | Bu **Standard_B2s** şablonundaki varsayılan değer bu hızlı başlangıç için yeterlidir. |
   | **Konum**|[resourceGroup (). Location].| Bu değeri değiştirmeyin. |
   | **Sanal ağ adı**|SQL yönetilen örneğini oluşturduğunuz sanal ağ.|
   | **Alt ağ adı**|Önceki yordamda oluşturduğunuz alt ağın adı| SQL yönetilen örneğini oluşturduğunuz alt ağı seçmeyin.|
   | **yapıt konumu** | [Deployment (). Properties. templateLink. Uri] | Bu değeri değiştirmeyin. |
   | **yapıt konumu SAS belirteci** | boş bırakın | Bu değeri değiştirmeyin. |

   ![İstemci sanal makinesi oluşturma](./media/connect-vm-instance-configure/create-client-sql-vm.png)

   Önerilen VNet adını ve [SQL yönetilen örneğinizi oluştururken](instance-create-quickstart.md)varsayılan alt ağı kullandıysanız, son iki parametreyi değiştirmeniz gerekmez. Aksi takdirde, bu değerleri ağ ortamını ayarlarken girdiğiniz değerlerle değiştirmeniz gerekir.

3. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum** onay kutusunu seçin.
4. Azure VM 'yi ağınızda dağıtmak için **satın al** ' ı seçin.
5. Dağıtım durumunu görüntülemek için **Bildirimler** simgesini seçin.

> [!IMPORTANT]
> Oluşturma sonrası betiklerin SQL Server Management Studio yüklemesi için zaman kazandırmak üzere sanal makine oluşturulduktan yaklaşık 15 dakika sonrasına devam etmez.

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Aşağıdaki adımlarda, uzak masaüstü bağlantısı kullanarak yeni oluşturduğunuz sanal makineye bağlanma işlemi gösterilmektedir.

1. Dağıtım tamamlandıktan sonra sanal makine kaynağına gidin.

    ![VM](./media/connect-vm-instance-configure/vm.png)  

2. **Bağlan**'ı seçin.

   Bir Uzak Masaüstü Protokolü dosya (. rdp dosyası) formu, sanal makinenin genel IP adresi ve bağlantı noktası numarasıyla birlikte görüntülenir.

   ![RDP formu](./media/connect-vm-instance-configure/rdp.png)  

3. **RDP dosyasını indir**' i seçin.

   > [!NOTE]
   > Sanal makinenize bağlanmak için SSH de kullanabilirsiniz.

4. **Sanal makine bağlantısı** formunu kapatın.
5. VM'nize bağlanmak için indirilen RDP dosyasını açın.
6. İstendiğinde, **Bağlan**' ı seçin. Mac bilgisayarlarda, Mac App Store’dan bu [Uzak Masaüstü İstemcisi](https://apps.apple.com/app/microsoft-remote-desktop-10/id1295203466?mt=12) gibi bir RDP istemcisi indirmeniz gerekir.

7. Sanal makineyi oluştururken belirttiğiniz kullanıcı adını ve parolayı girip **Tamam**' ı seçin.

8. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** ' i veya **devam et** ' i seçin.

Sunucu Yöneticisi panosunda sanal makinenize bağlanırsınız.

## <a name="connect-to-sql-managed-instance"></a>SQL Yönetilen Örneğine bağlanma 

1. Sanal makinede SQL Server Management Studio (SSMS) öğesini açın.

   Bu, her SSMS ilk kez başlatıldığından bu yana yapılandırmayı tamamlaması gerektiğinden, açılması birkaç dakika sürer.
2. **Sunucuya Bağlan** iletişim kutusunda, **sunucu adı** kutusuna SQL yönetilen örneğinizin tam **ana bilgisayar adını** girin. **SQL Server kimlik doğrulaması**' nı seçin, Kullanıcı adınızı ve parolanızı girin ve ardından **Bağlan**' ı seçin.

    ![ssms bağlanma](./media/connect-vm-instance-configure/ssms-connect.png)  

Bağlandıktan sonra Veritabanları düğümündeki sistem ve kullanıcı veritabanlarınızı ve Güvenlik, Sunucu Nesneleri, Çoğaltma, Yönetim, SQL Server Agent ile XEvent Profil Oluşturucu düğümlerindeki çeşitli nesneleri görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Noktadan siteye bağlantı kullanarak şirket içi istemci bilgisayarından bağlanmayı gösteren hızlı başlangıç için bkz. [Noktadan siteye bağlantı yapılandırma](point-to-site-p2s-configure.md).
- Uygulamalar için bağlantı seçeneklerine genel bakış için bkz. [UYGULAMALARıNıZı SQL yönetilen örneğine bağlama](connect-application-instance.md).
- Mevcut bir SQL Server veritabanını Şirket içinden bir SQL yönetilen örneğine geri yüklemek için, [geçiş Için Azure veritabanı geçiş hizmeti 'ni (DMS)](../../dms/tutorial-sql-server-to-managed-instance.md) veya bir veritabanı yedekleme dosyasından geri yüklemek için [T-SQL restore komutunu](restore-sample-database-quickstart.md) kullanabilirsiniz.
