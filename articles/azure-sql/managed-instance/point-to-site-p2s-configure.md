---
title: SSMS kullanarak noktadan siteye bağlantı yapılandırma
titleSuffix: Azure SQL Managed Instance
description: Şirket içi istemci bilgisayarından Noktadan siteye bağlantı kullanarak SQL Server Management Studio (SSMS) kullanarak Azure SQL yönetilen örneğine bağlanın.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab, bonova, jovanpop
ms.date: 03/13/2019
ms.openlocfilehash: 4e56454142cdbc91f621ca20532e689d5c6e6458
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84047996"
---
# <a name="quickstart-configure-a-point-to-site-connection-to-azure-sql-managed-instance-from-on-premises"></a>Hızlı başlangıç: şirket içi Azure SQL yönetilen örneği ile noktadan siteye bağlantı yapılandırma
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu hızlı başlangıçta, bir şirket içi istemci bilgisayarından Noktadan siteye bağlantı üzerinden [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) kullanarak Azure SQL yönetilen örneği 'ne nasıl bağlanacağı gösterilmektedir. Noktadan siteye bağlantılar hakkında bilgi için bkz. [noktadan sıteye VPN hakkında](../../vpn-gateway/point-to-site-about.md)

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç:

- , Başlangıç noktası olarak [BIR SQL yönetilen örneği oluşturma](instance-create-quickstart.md) bölümünde oluşturulan kaynakları kullanır.
- Şirket içi istemci bilgisayarınızda PowerShell 5,1 ve AZ PowerShell 1.4.0 veya üstünü gerektirir. Gerekirse, [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps#install-the-azure-powershell-module)yönergelerine bakın.
- Şirket içi istemci bilgisayarınızda en yeni [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) sürümünü gerektirir.

## <a name="attach-a-vpn-gateway-to-virtual-network"></a>Sanal ağa bir VPN ağ geçidi iliştirme

1. Şirket içi istemci bilgisayarınızda PowerShell 'i açın.

2. Bu PowerShell betiğini kopyalayın. Bu betik, [SQL yönetilen örnek oluşturma](instance-create-quickstart.md) hızlı başlangıç bölümünde oluşturduğunuz SQL yönetilen örnek sanal ağına bir VPN Gateway ekler. Bu betik, Azure PowerShell az modülünü kullanır ve Windows ya da Linux tabanlı konaklar için aşağıdakileri yapar:

   - İstemci makinesinde sertifika oluşturur ve yükler
   - Gelecek VPN Gateway alt ağ IP aralığını hesaplar
   - GatewaySubnet 'i oluşturur
   - , VPN Gateway VPN alt ağına bağlayan Azure Resource Manager şablonunu dağıtır

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

3. Betiği PowerShell pencerenize yapıştırın ve gerekli parametreleri sağlayın. `<subscriptionId>`, Ve değerlerinin, `<resourceGroup>` `<virtualNetworkName>` [SQL yönetilen örnek oluşturma](instance-create-quickstart.md) hızlı başlangıcı için kullandığınız adlarla eşleşmesi gerekir. Değeri, `<certificateNamePrefix>` tercih ettiğiniz bir dize olabilir.

4. PowerShell betiğini yürütün.

> [!IMPORTANT]
> PowerShell betiği tamamlanana kadar devam etmez.

## <a name="create-a-vpn-connection"></a>VPN bağlantısı oluşturma

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. Sanal ağ geçidini oluşturduğunuz kaynak grubunu açın ve sonra sanal ağ geçidi kaynağını açın.
3. **Noktadan siteye yapılandırma** ' yı seçin ve ardından **VPN istemcisini indir**' i seçin.

    ![VPN istemcisini indir](./media/point-to-site-p2s-configure/download-vpn-client.png)  
4. Şirket içi istemci bilgisayarınızda, ZIP dosyasındaki dosyaları ayıklayın ve ardından ayıklanan dosyaların bulunduğu klasörü açın.
5. '**WindowsAmd64** klasörünü açın ve **VpnClientSetupAmd64. exe** dosyasını açın.
6. **PC 'niz korumalı bir Windows** iletisi alırsanız, **daha fazla bilgi** ' ye ve **yine de Çalıştır**' a tıklayın.

    ![VPN istemcisi 'ni yükler](./media/point-to-site-p2s-configure/vpn-client-defender.png)\
7. Kullanıcı Hesabı Denetimi iletişim kutusunda, devam etmek için **Evet**'e tıklayın.
8. Sanal ağınıza başvuran iletişim kutusunda, sanal ağınızın VPN Istemcisini yüklemek için **Evet** ' i seçin.

## <a name="connect-to-the-vpn-connection"></a>VPN bağlantısına Bağlan

1. Şirket içi istemci bilgisayarınızda **ağ & Internet** 'Te bulunan **VPN** 'ye gidin ve bu VNET Ile bağlantı kurmak için SQL yönetilen örnek Sanal ağınızı seçin. Aşağıdaki görüntüde VNet, **Mynewvnet**olarak adlandırılmıştır.

    ![VPN bağlantısı](./media/point-to-site-p2s-configure/vpn-connection.png)  
2. **Bağlan**'ı seçin.
3. İletişim kutusunda **Bağlan**' ı seçin.

    ![VPN bağlantısı](./media/point-to-site-p2s-configure/vpn-connection2.png)  
4. Bağlantı Yöneticisi 'nin yol tablonuzu güncelleştirmek için yükseltilmiş ayrıcalığa ihtiyacı olması istendiğinde, **devam**' ı seçin.
5. Devam etmek için Kullanıcı hesabı denetimi iletişim kutusunda **Evet** ' i seçin.

   SQL yönetilen örnek VNet 'iniz için bir VPN bağlantısı oluşturdunuz.

    ![VPN bağlantısı](./media/point-to-site-p2s-configure/vpn-connection-succeeded.png)  

## <a name="connect-with-ssms"></a>SSMS ile bağlanma

1. Şirket içi istemci bilgisayarında SQL Server Management Studio (SSMS) öğesini açın.
2. **Sunucuya Bağlan** iletişim kutusunda, **sunucu adı** kutusuna SQL yönetilen örneğinizin tam **ana bilgisayar adını** girin.
3. **SQL Server kimlik doğrulaması**' nı seçin, Kullanıcı adınızı ve parolanızı girin ve ardından **Bağlan**' ı seçin.

    ![ssms bağlanma](./media/point-to-site-p2s-configure/ssms-connect.png)  

Bağlandıktan sonra, sistem ve Kullanıcı veritabanlarınızı veritabanları düğümünde görüntüleyebilirsiniz. Ayrıca güvenlik, sunucu nesneleri, çoğaltma, yönetim, SQL Server Agent ve XEvent Profiler düğümlerinde çeşitli nesneleri görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Azure sanal makinesinden bağlanmayı gösteren bir hızlı başlangıç için bkz. [Noktadan siteye bağlantı yapılandırma](point-to-site-p2s-configure.md).
- Uygulamalar için bağlantı seçeneklerine genel bakış için bkz. [UYGULAMALARıNıZı SQL yönetilen örneğine bağlama](connect-application-instance.md).
- Mevcut bir SQL Server veritabanını Şirket içinden bir SQL yönetilen örneğine geri yüklemek için, [geçiş Için Azure veritabanı geçiş hizmeti 'ni (DMS)](../../dms/tutorial-sql-server-to-managed-instance.md) veya bir veritabanı yedekleme dosyasından geri yüklemek için [T-SQL restore komutunu](restore-sample-database-quickstart.md) kullanabilirsiniz.
