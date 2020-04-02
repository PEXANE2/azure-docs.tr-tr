---
title: Bağlantı istemcisi VM - yönetilen örnek
description: Azure sanal makinesinden SQL Server Management Studio'u kullanarak Bir Azure SQL Veritabanı Yönetilen Örneği'ne bağlanın.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, srbozovi, bonova
ms.date: 02/18/2019
ms.openlocfilehash: 8b5dce0b43fac7cfd0e974f26451338ca1541f8f
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80528413"
---
# <a name="quickstart-configure-azure-vm-to-connect-to-an-azure-sql-database-managed-instance"></a>Hızlı başlatma: Azure SQL Veritabanı Yönetilen Örneğine bağlanmak için Azure VM'yi yapılandırın

Bu hızlı başlangıç, SQL Server Management Studio (SSMS) kullanarak Bir Azure SQL Veritabanı Yönetilen Örneği'ne bağlanmak için bir Azure sanal makinesini nasıl yapılandırabileceğinizi gösterir. Yerinde istemci bilgisayardan noktaya bağlantı kullanarak nasıl bağlanılabildiğini gösteren hızlı [bir](sql-database-managed-instance-configure-p2s.md) başlangıç için bkz.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç, [Yönetilen Örnek Oluştur'da](sql-database-managed-instance-get-started.md) oluşturulan kaynakları başlangıç noktası olarak kullanır.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="create-a-new-subnet-in-the-managed-instance-vnet"></a>Yönetilen Örnek VNet'te yeni bir alt ağ oluşturma

Aşağıdaki adımlar, Yönetilen Örnek VNet'te yeni bir alt ağ oluşturur, böylece Bir Azure sanal makinesi Yönetilen Örnek'e bağlanabilir. Yönetilen Örnek alt ağı Yönetilen Örnekler'e adanmıştır. Bu alt ağda Azure sanal makineleri gibi başka kaynaklar oluşturamazsınız.

1. [Yönetilen Örnek Oluştur](sql-database-managed-instance-get-started.md) hızlı başlangıcında oluşturduğunuz Yönetilen Örnek için kaynak grubunu açın. Yönetilen Örneğiniz için sanal ağı seçin.

   ![Yönetilen Örnek kaynakları](./media/sql-database-managed-instance-configure-vm/resources.png)

2. **Alt ağlar'ı** seçin ve ardından yeni bir alt ağ oluşturmak için **+ Subnet'i** seçin.

   ![Yönetilen Örnek alt ağları](./media/sql-database-managed-instance-configure-vm/subnets.png)

3. Bu tablodaki bilgileri kullanarak formu doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ---------------- | ----------------- | ----------- |
   | **Adı** | Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming).|
   | **Adres aralığı (CIDR bloğu)** | Geçerli bir aralık | Varsayılan değer bu hızlı başlatma için iyidir.|
   | **Ağ güvenlik grubu** | None | Varsayılan değer bu hızlı başlatma için iyidir.|
   | **Rota tablosu** | None | Varsayılan değer bu hızlı başlatma için iyidir.|
   | **Hizmet uç noktaları** | 0 seçili | Varsayılan değer bu hızlı başlatma için iyidir.|
   | **Alt ağ temsilcisi** | None | Varsayılan değer bu hızlı başlatma için iyidir.|

   ![İstemci VM için Yeni Yönetilen Örnek alt ağı](./media/sql-database-managed-instance-configure-vm/new-subnet.png)

4. Yönetilen Örnek VNet'te bu ek alt ağı oluşturmak için **Tamam'ı** seçin.

## <a name="create-a-virtual-machine-in-the-new-subnet-in-the-vnet"></a>Sanal ağdaki yeni alt ağ içinde sanal makine oluşturma

Aşağıdaki adımlar, Yönetilen Örnek'e bağlanmak için yeni alt ağda nasıl sanal bir makine oluşturabileceğinizi gösterir.

## <a name="prepare-the-azure-virtual-machine"></a>Azure sanal makinesini hazırlayın

SQL Yönetilen Örnek özel Sanal Ağınıza yerleştirildiğinden, SQL Server Management Studio veya Azure Veri Stüdyosu gibi yüklü bir SQL istemci aracıyla bir Azure VM oluşturmanız gerekir. Bu araç Yönetilen Örnek'e bağlanmanızı ve sorguları yürütmenizi sağlar. Bu hızlı başlangıçta SQL Server Management Studio kullanılmıştır.

Gerekli tüm araçlarla bir istemci sanal makine oluşturmanın en kolay yolu Azure Kaynak Yöneticisi şablonlarını kullanmaktır.

1. Azure portalında başka bir tarayıcı sekmesinde oturum açmış olduğunuzdan emin olun. Ardından, istemci sanal makine oluşturmak ve SQL Server Management Studio'yu yüklemek için aşağıdaki düğmeyi seçin:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjovanpop-msft%2Fazure-quickstart-templates%2Fsql-win-vm-w-tools%2F201-vm-win-vnet-sql-tools%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

2. Aşağıdaki tablodaki bilgileri kullanarak formu doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ---------------- | ----------------- | ----------- |
   | **Abonelik** | Geçerli bir abonelik | Yeni kaynaklar oluşturma iznine sahip olduğunuz bir abonelik olmalıdır. |
   | **Kaynak Grubu** |[Yönetilen Örnek Oluştur'da](sql-database-managed-instance-get-started.md) belirttiğiniz kaynak grubu hızlı başlat.|Bu kaynak grubu, VNet'in var olduğu grup olmalıdır.|
   | **Konum** | Kaynak grubunun konumu | Bu değer, seçilen kaynak grubuna göre doldurulur. |
   | **Sanal makine adı**  | Geçerli bir ad | Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming).|
   |**Yönetici Kullanıcı Adı**|Geçerli kullanıcı adı|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming). Ayrılmış bir sunucu düzeyi rolü olduğu için "serveradmin" kullanmayın.<br>[VM'ye](#connect-to-virtual-machine)her bağlandığınızda bu kullanıcı adını kullanırsınız.|
   |**Parola**|Geçerli bir parola|Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.<br>[VM'ye](#connect-to-virtual-machine)her bağlandığınızda bu parolayı kullanırsınız.|
   | **Sanal Makine Boyutu** | Herhangi bir geçerli boyut | Bu **Standard_B2s** şablonundaki varsayılan değer, bu hızlı başlatma için yeterlidir. |
   | **Konum**|[resourceGroup().konum].| Bu değeri değiştirme. |
   | **Sanal Ağ Adı**|Yönetilen Örnek'i oluşturduğunuz sanal ağ.|
   | **Alt ağ adı**|Önceki yordamda oluşturduğunuz alt netin adı| Yönetilen Örnek'i oluşturduğunuz alt ağı seçmeyin.|
   | **eserler Yer** | [deployment().properties.templateLink.uri] | Bu değeri değiştirme. |
   | **eserler Konum Sas belirteci** | boş bırakın | Bu değeri değiştirme. |

   ![İstemci sanal makinesi oluşturma](./media/sql-database-managed-instance-configure-vm/create-client-sql-vm.png)

   [Yönetilen Örneğinizi oluştururken](sql-database-managed-instance-get-started.md)önerilen VNet adını ve varsayılan alt ağı kullandıysanız, son iki parametreyi değiştirmeniz gerekmez. Aksi takdirde, ağ ortamını ayarlarken bu değerleri girdiğiniz değerlerle değiştirmeniz gerekir.

3. Onay kutusunun **üzerinde belirtilen hüküm ve koşulları kabul** ediyorum seçin.
4. Azure VM'yi abunuza dağıtmak için **Satın Al'ı** seçin.
5. Dağıtım durumunu görüntülemek için **Bildirimler** simgesini seçin.

> [!IMPORTANT]
> Sanal makine oluşturulduktan yaklaşık 15 dakika sonrasına kadar devam etmeyin ve oluşturma sonrası komut dosyalarının SQL Server Management Studio'yu yüklemesi için zaman verin.

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Aşağıdaki adımlarda, uzak masaüstü bağlantısı kullanarak yeni oluşturduğunuz sanal makineye bağlanma işlemi gösterilmektedir.

1. Dağıtım tamamlandıktan sonra sanal makine kaynağına gidin.

    ![VM](./media/sql-database-managed-instance-configure-vm/vm.png)  

2. **Bağlan**’ı seçin.

   Sanal makinenin ortak IP adresi ve bağlantı noktası numarasıyla birlikte Uzak Masaüstü Protokolü dosyası (.rdp dosyası) formu görüntülenir.

   ![RDP formu](./media/sql-database-managed-instance-configure-vm/rdp.png)  

3. **RDP Dosyasını İndir'i**seçin.

   > [!NOTE]
   > VM'inize bağlanmak için SSH'yi de kullanabilirsiniz.

4. **Connect'i sanal makine formuna** kapatın.
5. VM'nize bağlanmak için indirilen RDP dosyasını açın.
6. İstendiğinde **Bağlan'ı**seçin. Mac bilgisayarlarda, Mac App Store’dan bu [Uzak Masaüstü İstemcisi](https://apps.apple.com/app/microsoft-remote-desktop-10/id1295203466?mt=12) gibi bir RDP istemcisi indirmeniz gerekir.

7. Sanal makine oluştururken belirttiğiniz kullanıcı adı ve parolayı girin ve **ardından Tamam'ı**seçin.

8. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. **Bağlantıya** devam etmek için Evet veya **Devam et'i** seçin.

Server Manager panosundaki sanal makinenize bağlısınız.

## <a name="use-ssms-to-connect-to-the-managed-instance"></a>Yönetilen Örneğe bağlanmak için SSMS'i kullanma

1. Sanal makinede SQL Server Management Studio'yu (SSMS) açın.

   SSMS ilk kez başlatıldığından yapılandırmasını tamamlaması gerektiğinden, açılması birkaç dakika sürer.
2. **Sunucuya Bağlan** iletişim kutusuna, Yönetilen Örneğinizin tam nitelikli **ana bilgisayar adını** Sunucu **ad** kutusuna girin. **SQL Server Kimlik Doğrulaması'nı**seçin, kullanıcı adınızı ve parolanızı girin ve sonra **Bağlan'ı**seçin.

    ![ssms bağlanma](./media/sql-database-managed-instance-configure-vm/ssms-connect.png)  

Bağlandıktan sonra Veritabanları düğümündeki sistem ve kullanıcı veritabanlarınızı ve Güvenlik, Sunucu Nesneleri, Çoğaltma, Yönetim, SQL Server Agent ile XEvent Profil Oluşturucu düğümlerindeki çeşitli nesneleri görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Bir şirket içi istemci bilgisayardan noktadan siteye bağlantı kullanarak nasıl bağlanılmayı gösteren hızlı [bir](sql-database-managed-instance-configure-p2s.md)başlangıç için bkz.
- Uygulamaların bağlantı seçeneklerine genel bir bakış için bkz: [Uygulamalarınızı Yönetilen Örneğe bağlama](sql-database-managed-instance-connect-app.md).
- Varolan bir SQL Server veritabanını şirket içinde yönetilen bir örneğe geri yüklemek için, [geçiş için Azure Veritabanı Geçiş Hizmeti'ni (DMS)](../dms/tutorial-sql-server-to-managed-instance.md) veya veritabanı yedekleme dosyasından geri yüklemek için [T-SQL RESTORE komutunu](sql-database-managed-instance-get-started-restore.md) kullanabilirsiniz.
