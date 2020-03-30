---
title: Portalda SQL Server Windows Sanal Makinesi oluşturma | Microsoft Docs
description: Bu öğreticide Azure portalında Windows SQL Server 2017 sanal makinesi oluşturma adımları gösterilmiştir.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 07/11/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 801a6fc0602882d1af49c06bafcfd51942e6da2e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75965649"
---
# <a name="quickstart-create-a-sql-server-2017-windows-virtual-machine-in-the-azure-portal"></a>Hızlı başlangıç: Azure portalında SQL Server 2017 Windows sanal makinesi oluşturma

> [!div class="op_single_selector"]
> * [Windows](quickstart-sql-vm-create-portal.md)
> * [Linux](../../linux/sql/provision-sql-server-linux-virtual-machine.md)

Bu hızlı başlangıç, Azure Portal’da SQL Server sanal makinesi oluşturma adımlarında yol gösterir.


  > [!TIP]
  > - Bu hızlı başlangıç, hızlı bir şekilde bir SQL VM sağlama ve VM’ye bağlanma yolu sağlar. Diğer SQL VM sağlama seçimleri hakkında daha fazla bilgi için bkz. [Azure portalında Windows SQL Server VM'leri için sağlama kılavuzu](virtual-machines-windows-portal-sql-server-provision.md).
  > - SQL Server sanal makineleri hakkında sorularınız olursa [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md) bölümüne bakın.

## <a name="get-an-azure-subscription"></a><a id="subscription"></a>Azure aboneliği kazanın

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="select-a-sql-server-vm-image"></a><a id="select"></a> SQL Server VM görüntüsü seçme

1. Hesabınızı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure portalının sol menüsünde **Azure SQL'i** seçin. **Azure SQL** listede yoksa, **Tüm hizmetler'i**seçin ve ardından arama kutusuna *Azure SQL* yazın.
1. SELECT SQL dağıtım **seçeneği** sayfasını açmak için **+Ekle'yi** seçin. **SQL sanal makineler** döşemesindeki ayrıntıları **göster'i** seçerek ek bilgileri görüntüleyebilirsiniz.
1. Açılır pencereden **Ücretsiz SQL Server Lisansı: SQL Server 2017 Developer on Windows Server 2016** görüntüsünü seçin.

   ![Yeni arama penceresi](./media/quickstart-sql-vm-create-portal/select-sql-2017-vm-image.png)

1. **Oluştur'u**seçin.

   ![Yeni arama penceresi](./media/quickstart-sql-vm-create-portal/create-sql-2017-vm-image.png)

## <a name="provide-basic-details"></a><a id="configure"></a> Temel ayrıntıları sağlama

Temel **Bilgiler** sekmesinde aşağıdaki bilgileri sağlayın:

1. Proje **Ayrıntıları** bölümünde, Azure aboneliğinizi seçin ve ardından yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** seçin. Ad için _SQLVM-RG_ yazın.

   ![Abonelik](media/quickstart-sql-vm-create-portal/basics-project-details.png)

1. **Örnek altında ayrıntılar**:
    1. **Sanal makine adı**için _SQLVM_ yazın. 
    1. **Bölgeniz**için bir konum seçin. 
    1. Bu hızlı başlangıç amacıyla, **Kullanılabilirlik seçeneklerini** _altyapı artıklığı gerektirmeyecek_şekilde ayarlayın. Kullanılabilirlik seçenekleri hakkında daha fazla bilgi edinmek için [Kullanılabilirlik'e](../../windows/availability.md)bakın. 
    1. **Resim** _listesinde, Ücretsiz SQL Server Lisansı: SQL Server 2017 Developer on Windows Server 2016'yı_seçin. 
    1. Sanal makinenin **boyutu** için **boyutu değiştirmeyi** seçin ve **A2 Basic** teklifini seçin. Beklenmeyen ücretleri önlemek için kaynaklarınızı temizledikten sonra bu kaynaklarla işiniz bittiğinden emin olun. 

   ![Örnek ayrıntıları](media/quickstart-sql-vm-create-portal/basics-instance-details.png)

1. **Administrator hesabı**altında, _azureuser_ ve parola gibi bir kullanıcı adı sağlayın. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../../windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.

   ![Yönetici hesabı](media/quickstart-sql-vm-create-portal/basics-administrator-account.png)

1. **Gelen bağlantı noktası kuralları altında,** **seçili bağlantı noktalarına izin ver'i** seçin ve ardından açılır bağlantı noktasından **RDP'yi (3389)** seçin. 

   ![Gelen bağlantı noktası kuralları](media/quickstart-sql-vm-create-portal/basics-inbound-port-rules.png)

## <a name="sql-server-settings"></a>SQL Server ayarları

SQL **Server ayarları** sekmesinde aşağıdaki seçenekleri yapılandırın:

1. **Security & Networking**altında, SQL **Bağlantısı** için Ortak `1401` _(Internet)_ seçeneğini belirleyin ve ortak senaryoda iyi bilinen bir bağlantı noktası numarasını kullanmaktan kaçınmak için bağlantı noktasını değiştirin. 
1. **SQL Kimlik Doğrulaması**altında **Etkinleştir'i**seçin. SQL Oturum Açma, sanal makine için yapılandırdığınız kullanıcı adı ve parolanın aynısına ayarlanır. [**Azure Anahtar Kasası tümleştirmesi**](virtual-machines-windows-ps-sql-keyvault.md)için varsayılan ayarı kullanın. Temel SQL Server VM görüntüsü için **depolama yapılandırması** kullanılamıyor ancak [depolama yapılandırmasındaki](virtual-machines-windows-sql-server-storage-configuration.md#new-vms)diğer görüntüler için kullanılabilir seçenekler hakkında daha fazla bilgi bulabilirsiniz.  

   ![SQL sunucu güvenlik ayarları](media/quickstart-sql-vm-create-portal/sql-server-settings.png)


1. Gerekirse diğer ayarları değiştirin ve ardından **Gözden Geçir + oluştur'u**seçin. 

   ![İnceleme + oluşturma](media/quickstart-sql-vm-create-portal/review-create.png)


## <a name="create-the-sql-server-vm"></a>SQL Server VM’sini oluşturma

Gözden **Geçir + oluştur** sekmesinde, özeti gözden geçirin ve bu VM için belirtilen SQL Server, kaynak grubu ve kaynakları oluşturmak için **Oluştur'u** seçin.

Azure portalından dağıtımı izleyebilirsiniz. Ekranın üst kısmındaki **Bildirimler** düğmesi dağıtımın temel durumunu gösterir. Dağıtım birkaç dakika sürebilir. 

## <a name="connect-to-sql-server"></a>SQL Server’a bağlanma

1. Portalda, sanal makinenizin özelliklerinin **Genel Bakış** bölümünde SQL Server VM'nizin Genel **IP adresini** bulun.

1. Internet'e bağlı farklı bir bilgisayarda [SQL Server Management Studio'yu (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms)açın.


1. **Sunucuya Bağlan** veya **Veritabanı Altyapısına Bağlan** iletişim kutusunda **Sunucu adı** değerini düzenleyin. Sanal makinenizin genel IP adresini girin. Sonra bir virgül koyun ve yeni sanal makineyi yapılandırırken belirttiğimiz özel bağlantı noktasını (**1401**) ekleyin. Örneğin, `11.22.33.444,1401`.

1. **Kimlik Doğrulaması** kutusunda **SQL Server Kimlik Doğrulaması**’nı seçin.

1. **Oturum Aç** kutusuna geçerli bir SQL oturum açma adı yazın.

1. **Parola** kutusuna oturum açma parolasını yazın.

1. **Bağlan**’ı seçin.

    ![ssms bağlanma](./media/quickstart-sql-vm-create-portal/ssms-connect.png)

## <a name="log-in-to-the-vm-remotely"></a><a id="remotedesktop"></a> VM’de uzaktan oturum açma

Uzak Masaüstü kullanarak SQL Server sanal makinesine bağlanmak için aşağıdaki adımları kullanın:

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

SQL Server sanal makineye bağlandıktan sonra, SQL Server Management Studio'yu başlatabilir ve yerel yönetici kimlik bilgilerinizi kullanarak Windows Kimlik Doğrulamasına bağlanabilirsiniz. SQL Server Kimlik Doğrulamasını etkinleştirdiyseniz, sağlama işlemi sırasında yapılandırdığınız SQL oturum açma adı ve parolasını kullanarak da SQL Kimlik Doğrulamasına bağlanabilirsiniz.

Makineye erişim, gereksinimlerinize göre makineyi ve SQL Server ayarlarını doğrudan değiştirmenize olanak tanır. Örneğin, güvenlik duvarı ayarlarını yapılandırabilir veya SQL Server yapılandırma ayarlarını değiştirebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

SQL VM’nizin sürekli çalıştırılması gerekmiyorsa, kullanımda olmadığında durdurarak gereksiz ödeme yapmaktan kaçının. Ayrıca, portalda ilişkili kaynak grubunu silerek, sanal makineyle ilişkilendirilmiş tüm kaynakları kalıcı olarak silebilirsiniz. Bu işlem sanal makineyi de kalıcı olarak sildiğinden, bu komutu dikkatli kullanın. Daha fazla bilgi için bkz. [Azure kaynaklarınızı portal üzerinden yönetme](../../../azure-resource-manager/management/manage-resource-groups-portal.md).


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure portalında bir SQL Server 2017 sanal makinesi oluşturdunuz. Verilerinizi yeni SQL Server’a geçirme hakkında daha fazla bilgi edinmek için, aşağıdaki makaleye bakın.

> [!div class="nextstepaction"]
> [Veritabanını SQL VM'ye geçirme](virtual-machines-windows-migrate-sql.md)
