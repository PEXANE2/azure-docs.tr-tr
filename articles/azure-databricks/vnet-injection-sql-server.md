---
title: Azure Databricks ile sql server Linux Docker kapsayıcısorgulama
description: Bu makalede, Sanal ağınıza Azure Veri Tuğlaları nasıl dağıtılanıncaya kadar vnet enjeksiyonu olarak da bilinir.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 11/07/2019
ms.openlocfilehash: 460079248e6cbd939c36b84f94cac41dce4dda2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73747671"
---
# <a name="tutorial-query-a-sql-server-linux-docker-container-in-a-virtual-network-from-an-azure-databricks-notebook"></a>Öğretici: Azure Databricks dizüstü bilgisayarından sanal ağda sql server Linux Docker kapsayıcısı sorgulayın

Bu öğretici, Azure Databrick'leri sanal ağdaki bir SQL Server Linux Docker kapsayıcısıyla nasıl entegre edebilirsiniz öğretir. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Databricks çalışma alanını sanal ağa dağıtma
> * Ortak bir ağa Linux sanal makinesi yükleme
> * Docker'ı yükleme
> * Microsoft SQL Server'ı Linux docker konteynerine yükleyin
> * Databricks not defterinden JDBC kullanarak SQL Server'ı sorgula

## <a name="prerequisites"></a>Ön koşullar

* Sanal [ağda Databricks çalışma alanı](quickstart-create-databricks-workspace-vnet-injection.md)oluşturun.

* [Windows için Ubuntu'ya](https://www.microsoft.com/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab)yükleyin.

* [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)’yu indirin.

## <a name="create-a-linux-virtual-machine"></a>Linux sanal makinesi oluşturma

1. Azure portalında **Sanal Makineler**simgesini seçin. Daha sonra **+ Ekle'yi**seçin.

    ![Yeni Azure sanal makine ekleme](./media/vnet-injection-sql-server/add-virtual-machine.png)

2. Temel **bilgiler** sekmesinde, Ubuntu Server 18.04 LTS'yi seçin ve VM boyutunu B2'lerle değiştirin. Yönetici kullanıcı adı ve parola seçin.

    ![Yeni sanal makine yapılandırmasının temelsek sekmesi](./media/vnet-injection-sql-server/create-virtual-machine-basics.png)

3. **Ağ** sekmesine gidin. Sanal ağı ve Azure Veri Tuğlaları kümenizi içeren ortak alt ağı seçin. **Gözden Geçir + oluştur'u**seçin, ardından sanal makineyi dağıtmak için **Oluşturun'** i oluşturun.

    ![Yeni sanal makine yapılandırmasının ağ sekmesi](./media/vnet-injection-sql-server/create-virtual-machine-networking.png)

4. Dağıtım tamamlandığında, sanal makineye gidin. **Genel Bakış'ta**Genel IP adresine ve Sanal ağa/alt ağına dikkat edin. Genel **IP Adresini** Seçin

    ![Sanal makineye genel bakış](./media/vnet-injection-sql-server/virtual-machine-overview.png)

5. **Atamayı** **Statik** olarak değiştirin ve **bir DNS ad etiketi**girin. **Kaydet'i**seçin ve sanal makineyi yeniden başlatın.

    ![Genel IP Adresi yapılandırması](./media/vnet-injection-sql-server/virtual-machine-staticip.png)

6. **Ayarlar'ın**altındaki **Ağ** sekmesini seçin. Azure Veri Tuğlaları dağıtımı sırasında oluşturulan ağ güvenlik grubunun sanal makineyle ilişkili olduğuna dikkat edin. **Gelen bağlantı noktası kuralını ekle'yi**seçin.

7. SSH için 22 bağlantı noktasını açmak için bir kural ekleyin. Aşağıdaki ayarları kullanın:
    
    |Ayar|Önerilen değer|Açıklama|
    |-------|---------------|-----------|
    |Kaynak|IP Adresleri|IP Adresleri, belirli bir kaynak IP Adresinden gelen trafiğin bu kural tarafından izin verilip verilmeyeceğini veya reddedildiğini belirtir.|
    |Kaynak IP adresleri|<halka açık ip\>|Herkese açık IP adresinizi girin. [bing.com](https://www.bing.com/) ziyaret ederek ve **"BENIM IP"** arayarak genel IP adresinizi bulabilirsiniz.|
    |Kaynak bağlantı noktası aralıkları|*|Herhangi bir bağlantı noktasından trafiğe izin verin.|
    |Hedef|IP Adresleri|IP Adresleri, belirli bir kaynak IP Adresi için giden trafiğin bu kural tarafından izin verilip reddedilemeyeceğini belirtir.|
    |Hedef IP adresleri|<vm kamu ip\>|Sanal makinenizin genel IP adresini girin. Bunu sanal makinenizin **Genel Bakış** sayfasında bulabilirsiniz.|
    |Hedef bağlantı noktası aralıkları|22|SSH için 22.|
    |Öncelik|290|Kuralı öncelik verin.|
    |Adı|ssh-databricks-öğretici-vm|Kurala bir isim ver.|


    ![Bağlantı noktası 22 için gelen güvenlik kuralı ekleme](./media/vnet-injection-sql-server/open-port.png)

8. Aşağıdaki ayarları ile SQL için port 1433 açmak için bir kural ekleyin:

    |Ayar|Önerilen değer|Açıklama|
    |-------|---------------|-----------|
    |Kaynak|Herhangi biri|Kaynak, belirli bir kaynak IP Adresinden gelen trafiğin bu kural tarafından izin verilip reddedildiğini belirtir.|
    |Kaynak bağlantı noktası aralıkları|*|Herhangi bir bağlantı noktasından trafiğe izin verin.|
    |Hedef|IP Adresleri|IP Adresleri, belirli bir kaynak IP Adresi için giden trafiğin bu kural tarafından izin verilip reddedilemeyeceğini belirtir.|
    |Hedef IP adresleri|<vm kamu ip\>|Sanal makinenizin genel IP adresini girin. Bunu sanal makinenizin **Genel Bakış** sayfasında bulabilirsiniz.|
    |Hedef bağlantı noktası aralıkları|1433|SQL Server için 22 portu açın.|
    |Öncelik|300|Kuralı öncelik verin.|
    |Adı|sql-databricks-tutorial-vm|Kurala bir isim ver.|

    ![Bağlantı noktası 1433 için gelen güvenlik kuralı nı ekleme](./media/vnet-injection-sql-server/open-port2.png)

## <a name="run-sql-server-in-a-docker-container"></a>SQL Server'ı Docker kapsayıcısında çalıştırma

1. [Windows için Ubuntu'yu](https://www.microsoft.com/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab)veya sanal makineye SSH'yi girmenizi sağlayacak başka bir aracı açın. Azure portalındaki sanal makinenize gidin ve bağlanmak için gereken SSH komutunu almak için **Bağlan'ı** seçin.

    ![Sanal makineye bağlanma](./media/vnet-injection-sql-server/vm-ssh-connect.png)

2. Ubuntu terminalinize komutu girin ve sanal makineyi yapılandırdığınızda oluşturduğunuz yönetici parolasını girin.

    ![Ubuntu terminalI SSH oturum açma](./media/vnet-injection-sql-server/vm-login-terminal.png)

3. Docker'ı sanal makineye yüklemek için aşağıdaki komutu kullanın.

    ```bash
    sudo apt-get install docker.io
    ```

    Docker'ın kurulumunu aşağıdaki komutla doğrulayın:

    ```bash
    sudo docker --version
    ```

4. Görüntüyü yükleyin.

    ```bash
    sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
    ```

    Görüntüleri kontrol et.

    ```bash
    sudo docker images
    ```

5. Kapsayıcıyı görüntüden çalıştırın.

    ```bash
    sudo docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Password1234' -p 1433:1433 --name sql1  -d mcr.microsoft.com/mssql/server:2017-latest
    ```

    Kapsayıcının çalıştığını doğrulayın.

    ```bash
    sudo docker ps -a
    ```

## <a name="create-a-sql-database"></a>SQL veritabanı oluşturma

1. SQL Server Management Studio'yu açın ve sunucu adını ve SQL Authentication'ı kullanarak sunucuya bağlanın. Oturum açma kullanıcı adı **SA'dır** ve parola Docker komutunda ayarlanan paroladır. Örnek komuttaki parola `Password1234`.

    ![SQL Server Management Studio'u kullanarak SQL Server'a bağlanın](./media/vnet-injection-sql-server/ssms-login.png)

2. Başarıyla bağlandıktan sonra, **Yeni Sorgu'yu** seçin ve veritabanı, tablo oluşturmak ve tabloya bazı kayıtlar eklemek için aşağıdaki kod parçacıklarını girin.

    ```SQL
    CREATE DATABASE MYDB;
    GO
    USE MYDB;
    CREATE TABLE states(Name VARCHAR(20), Capitol VARCHAR(20));
    INSERT INTO states VALUES ('Delaware','Dover');
    INSERT INTO states VALUES ('South Carolina','Columbia');
    INSERT INTO states VALUES ('Texas','Austin');
    SELECT * FROM states
    GO
    ```

    ![SQL Server veritabanı oluşturmak için sorgu](./media/vnet-injection-sql-server/create-database.png)

## <a name="query-sql-server-from-azure-databricks"></a>Azure Databricks'ten SQL Server Sorgula

1. Azure Databricks çalışma alanınıza gidin ve ön koşulların bir parçası olarak bir küme oluşturduğunuzdan doğrulayın. Ardından, **Not Defteri Oluştur'u**seçin. Not defterine bir ad verin, dil olarak *Python'u* seçin ve oluşturduğunuz kümeyi seçin.

    ![Yeni Databricks dizüstü bilgisayar ayarları](./media/vnet-injection-sql-server/create-notebook.png)

2. SQL Server sanal makinesinin iç IP Adresini pinglemek için aşağıdaki komutu kullanın. Bu ping başarılı olmalıdır. Değilse, kapsayıcının çalıştığını doğrulayın ve ağ güvenlik grubu (NSG) yapılandırmasını gözden geçirin.

    ```python
    %sh
    ping 10.179.64.4
    ```

    İncelemek için nslookup komutunu da kullanabilirsiniz.

    ```python
    %sh
    nslookup databricks-tutorial-vm.westus2.cloudapp.azure.com
    ```

3. SQL Server'ı başarıyla pingledikten sonra veritabanını ve tabloları sorgulayabilirsiniz. Aşağıdaki python kodunu çalıştırın:

    ```python
    jdbcHostname = "10.179.64.4"
    jdbcDatabase = "MYDB"
    userName = 'SA'
    password = 'Password1234'
    jdbcPort = 1433
    jdbcUrl = "jdbc:sqlserver://{0}:{1};database={2};user={3};password={4}".format(jdbcHostname, jdbcPort, jdbcDatabase, userName, password)

    df = spark.read.jdbc(url=jdbcUrl, table='states')
    display(df)
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde kaynak grubunu, Azure Veri Tuğlaları çalışma alanını ve ilgili tüm kaynakları silin. İşi silerken gereksiz faturalandırmaönler. Gelecekte Azure Veri Tuğlaları çalışma alanını kullanmayı planlıyorsanız, kümeyi durdurabilir ve daha sonra yeniden başlatabilirsiniz. Bu Azure Veri Tuğlaları çalışma alanını kullanmaya devam etmeyecekseniz, aşağıdaki adımları kullanarak bu eğitimde oluşturduğunuz tüm kaynakları silin:

1. Azure portalındaki sol menüden Kaynak **gruplarını** tıklatın ve ardından oluşturduğunuz kaynak grubunun adını tıklatın.

2. Kaynak grubu sayfanızda **Sil,** metin kutusuna silmek için kaynağın adını yazın ve ardından yeniden **Sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Databricks'i kullanarak verileri nasıl ayıklayacağımı, dönüştürecek ve yükleyeceklerini öğrenmek için bir sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Öğretici: Azure Databricks'i kullanarak verileri ayıklayın, dönüştürün ve yükleyin](databricks-extract-load-sql-data-warehouse.md)
