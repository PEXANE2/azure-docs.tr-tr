---
title: P2S'yi yapılandır - Yönetilen örnek
description: Şirket içi istemci bilgisayardan yerinde bağlantı kullanarak SQL Server Management Studio'u kullanarak Azure SQL Veritabanı Yönetilen Örneği'ne bağlanın.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab, bonova, jovanpop
ms.date: 03/13/2019
ms.openlocfilehash: 30b2ba92174996ea2bae34e7553a3258d8ebee27
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268892"
---
# <a name="quickstart-configure-a-point-to-site-connection-to-an-azure-sql-database-managed-instance-from-on-premises"></a>Hızlı başlatma: Şirket içinde Azure SQL Veritabanı Yönetilen Örneği'ne yerinde bağlantı oluşturma

Bu hızlı başlangıç, yerinde istemci bilgisayardan bir noktadan siteye bağlantı üzerinden [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) 'yı (SSMS) kullanarak Azure SQL Veritabanı Yönetilen Örneği'ne nasıl bağlanılabildiğini gösterir. Noktadan siteye bağlantılar hakkında bilgi için, [Bkz.](../vpn-gateway/point-to-site-about.md)

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç:

- Oluşturulan [kaynakları,](sql-database-managed-instance-get-started.md) Başlangıç noktası olarak Yönetilen Örnek Oluştur'u kullanır.
- PowerShell 5.1 ve AZ PowerShell 1.4.0 veya daha sonra şirket içi istemci bilgisayarınızda gerektirir. Gerekirse, [Azure PowerShell modüllerini yüklemek](https://docs.microsoft.com/powershell/azure/install-az-ps#install-the-azure-powershell-module)için verilen talimatlara bakın.
- Şirket içi istemci bilgisayarınızda [SQL Server Management Studio'nun](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) en yeni sürümünü gerektirir.

## <a name="attach-a-vpn-gateway-to-your-managed-instance-virtual-network"></a>Yönetilen Örnek sanal ağınıza VPN ağ geçidi ekleme

1. PowerShell'i şirket içi istemci bilgisayarınızda açın.

2. Bu PowerShell komut dosyasını kopyalayın. Bu komut dosyası, [Yönetilen Örnek Oluşturma](sql-database-managed-instance-get-started.md) hızlı başlatınca oluşturduğunuz Yönetilen Örnek sanal ağına bir VPN Ağ Geçidi bağlar. Bu komut dosyası Azure PowerShell Az Modülü'ni kullanır ve Windows veya Linux tabanlı ana bilgisayarlar için aşağıdakileri yapar:

   - İstemci makinesinde sertifika lar oluşturur ve yükler
   - Gelecekteki VPN Ağ Geçidi alt ağ ip aralığını hesaplar
   - GatewaySubnet oluşturur
   - VPN Ağ Geçidi'ni VPN alt ağına iliştiren Azure Kaynak Yöneticisi şablonuna dağıtır

     ```powershell
     $scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/attach-vpn-gateway'

     $parameters = @{
       subscriptionId = '<subscriptionId>'
       resourceGroupName = '<resourceGroupName>'
       virtualNetworkName = '<virtualNetworkName>'
       certificateNamePrefix  = '<certificateNamePrefix>'
       }

     Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/attachVPNGateway.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters, $scriptUrlBase
     ```

3. Komut dosyasını PowerShell pencerenize yapıştırın ve gerekli parametreleri sağlayın. Yönetilen [Örnek Oluştur](sql-database-managed-instance-get-started.md) `<virtualNetworkName>` için kullandıklarınız olanların , ve 'nin `<resourceGroup>`değerleri ile eşleşmesi gerekir. `<subscriptionId>` Değer için `<certificateNamePrefix>` seçtiğiniz bir dize olabilir.

4. PowerShell komut dosyasını çalıştırın.

> [!IMPORTANT]
> PowerShell komut dosyası tamamlanana kadar devam etmeyin.

## <a name="create-a-vpn-connection-to-your-managed-instance"></a>Yönetilen Örneğinize VPN bağlantısı oluşturun

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Sanal ağ ağ ağ geçidini oluşturduğunuz kaynak grubunu açın ve ardından sanal ağ ağ geçidi kaynağını açın.
3. **Noktaya Sayfa yapılandırmayı** seçin ve ardından **VPN istemcisini indir'i**seçin.

    ![VPN istemcisini indirin](./media/sql-database-managed-instance-configure-p2s/download-vpn-client.png)  
4. Şirket içi istemci bilgisayarınızda, zip dosyasından dosyaları ayıklayın ve ardından çıkarılan dosyalarla klasörü açın.
5. **' WindowsAmd64** klasörünü açın ve **VpnClientSetupAmd64.exe** dosyasını açın.
6. Windows tarafından **korunan bir BILGISAYAR iletisi** alırsanız, **daha fazla bilgi** ve ardından Yine de **Çalıştır'ı**tıklatın.

    ![VPN istemcisini yükleme](./media/sql-database-managed-instance-configure-p2s/vpn-client-defender.png)\
7. Kullanıcı Hesabı Denetimi iletişim kutusunda, devam etmek için **Evet**'e tıklayın.
8. Sanal ağınıza başvuran iletişim kutusunda, sanal ağınız için VPN İstemcisini yüklemek için **Evet'i** seçin.

## <a name="connect-to-the-vpn-connection"></a>VPN bağlantısına bağlanın

1. Şirket içi istemci bilgisayarınızda **Ağ & Internet'te** **VPN'e** gidin ve bu VNet'e bağlantı kurmak için Yönetilen Örnek sanal ağınızı seçin. Aşağıdaki resimde, VNet **MyNewVNet**adlandırılır.

    ![VPN bağlantısı](./media/sql-database-managed-instance-configure-p2s/vpn-connection.png)  
2. **Bağlan**’ı seçin.
3. İletişim kutusunda **Bağlan'ı**seçin.

    ![VPN bağlantısı](./media/sql-database-managed-instance-configure-p2s/vpn-connection2.png)  
4. Bağlantı Yöneticisi'nin rota tablonuzu güncelleştirmek için yüksek ayrıcalığa ihtiyacı olduğu istendiğinde **Devam et'i**seçin.
5. Devam etmek için Kullanıcı Hesabı Denetimi iletişim kutusunda **Evet'i** seçin.

   Yönetilen Örnek VNet'inize vpn bağlantısı kurdunuz.

    ![VPN bağlantısı](./media/sql-database-managed-instance-configure-p2s/vpn-connection-succeeded.png)  

## <a name="use-ssms-to-connect-to-the-managed-instance"></a>Yönetilen Örneğe bağlanmak için SSMS'i kullanma

1. Şirket içi istemci bilgisayarında SQL Server Management Studio'yu (SSMS) açın.
2. **Sunucuya Bağlan** iletişim kutusuna, Yönetilen Örneğinizin tam nitelikli **ana bilgisayar adını** Sunucu **ad** kutusuna girin.
3. **SQL Server Kimlik Doğrulaması'nı**seçin, kullanıcı adınızı ve parolanızı girin ve sonra **Bağlan'ı**seçin.

    ![ssms bağlanma](./media/sql-database-managed-instance-configure-vm/ssms-connect.png)  

Bağlandıktan sonra, sistem ve kullanıcı veritabanlarınızı Veritabanları düğümünde görüntüleyebilirsiniz. Güvenlik, Sunucu Nesneleri, Çoğaltma, Yönetim, SQL Server Agent ve XEvent Profiler düğümlerinde çeşitli nesneleri de görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Azure sanal makinesinden nasıl bağlanılmayı gösteren hızlı [bir](sql-database-managed-instance-configure-p2s.md)başlangıç için bkz.
- Uygulamaların bağlantı seçeneklerine genel bir bakış için bkz: [Uygulamalarınızı Yönetilen Örneğe bağlama](sql-database-managed-instance-connect-app.md).
- Varolan bir SQL Server veritabanını şirket içinde yönetilen bir örneğe geri yüklemek için, [geçiş için Azure Veritabanı Geçiş Hizmeti'ni (DMS)](../dms/tutorial-sql-server-to-managed-instance.md) veya veritabanı yedekleme dosyasından geri yüklemek için [T-SQL RESTORE komutunu](sql-database-managed-instance-get-started-restore.md) kullanabilirsiniz.
