---
title: Striim kullanarak verileri Azure Cosmos DB Cassandra API hesabına geçirin
description: Verileri Oracle veritabanından Azure Cosmos DB Cassandra API hesabına geçirmek için Striim'i nasıl kullanacağınızı öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: sngun
ms.reviewer: sngun
ms.openlocfilehash: af99f369245d6006d0d4784e572020b820e406a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71266049"
---
# <a name="migrate-data-to-azure-cosmos-db-cassandra-api-account-using-striim"></a>Striim kullanarak verileri Azure Cosmos DB Cassandra API hesabına geçirin

Azure pazarındaki Striim görüntüsü, veri ambarlarından veritabanlarından Azure'a sürekli gerçek zamanlı veri hareketi sunar. Verileri hareket ettirirken, satır içi denormalleştirme, veri dönüşümü, gerçek zamanlı analiz ve veri raporlama senaryoları etkinleştirme gerçekleştirebilirsiniz. Kurumsal verileri sürekli olarak Azure Cosmos DB Cassandra API'ye taşımak için Striim ile başlamak kolaydır. Azure, Striim'i dağıtmayı ve verileri Azure Cosmos DB'ye taşımayı kolaylaştıran bir pazar teklifi sunar. 

Bu makalede, verileri **Oracle veritabanından** **Azure Cosmos DB Cassandra API hesabına**geçirmek için Striim'in nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* [Azure aboneliğiniz](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing) yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

* Bazı verilerin yer alan şirket içinde çalışan bir Oracle veritabanı.

## <a name="deploy-the-striim-marketplace-solution"></a>Striim pazar çözümü dağıtın

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Azure marketinde **kaynak oluştur'u** ve **Striim'i** arayın'ı seçin. İlk seçeneği seçin ve **oluşturun.**

   ![Striim pazar öğesi bul](./media/cosmosdb-sql-api-migrate-data-striim/striim-azure-marketplace.png)

1. Ardından, Striim örneğinin yapılandırma özelliklerini girin. Striim ortamı sanal bir makinede dağıtılır. Temel **ler** bölmesinden **VM kullanıcı adı**, **VM parolası** (bu parola VM'ye SSH için kullanılır) girin. **Aboneliğinizi,** **Kaynak Grubunuzu**ve Striim'i dağıtmak istediğiniz **konum ayrıntılarını** seçin. Tamamlandıktan sonra **Tamam'ı**seçin.

   ![Striim için temel ayarları yapılandırma](./media/cosmosdb-sql-api-migrate-data-striim/striim-configure-basic-settings.png)


1. **Striim Cluster ayarları** bölmesinde, Striim dağıtım türünü ve sanal makine boyutunu seçin.

   |Ayar | Değer | Açıklama |
   | ---| ---| ---|
   |Striim dağıtım türü |Tek Başına | Striim **Bağımsız** veya **Küme** dağıtım türlerinde çalıştırılabilir. Bağımsız mod, Striim sunucusunu tek bir sanal makineye dağıtır ve veri hacminize bağlı olarak VM'lerin boyutunu seçebilirsiniz. Küme modu, Striim sunucusunu seçili boyuta sahip iki veya daha fazla VM'ye dağıtır. 2'den fazla düğüme sahip küme ortamları otomatik yüksek kullanılabilirlik ve başarısızlık sunar.</br></br> Bu eğitimde, Bağımsız seçeneğini seçebilirsiniz. Varsayılan "Standard_F4s" boyutu VM kullanın. | 
   | Striim kümesinin adı|    <Striim_cluster_Name>|  Striim kümesinin adı.|
   | Striim küme parolası|   <Striim_cluster_password>|  Küme için parola.|

   Formu doldurduktan sonra devam etmek için **Tamam'ı** seçin.

1. **Striim erişim ayarları** bölmesinde, **Ortak IP adresini** (varsayılan değerleri seçin), **Striim için alan adı, Striim**UI'ye giriş yapmak için kullanmak istediğiniz **Yönetici parolasını** yapılandırın. Bir VNET ve Subnet yapılandırın (varsayılan değerleri seçin). Ayrıntıları doldurduktan sonra devam etmek için **Tamam'ı** seçin.

   ![Striim erişim ayarları](./media/cosmosdb-sql-api-migrate-data-striim/striim-access-settings.png)

1. Azure dağıtımı doğrulayacak ve her şeyin iyi göründüğünden emin olacak; doğrulamanın tamamlanması birkaç dakika sürer. Doğrulama tamamlandıktan sonra **Tamam'ı**seçin.
  
1. Son olarak, kullanım koşullarını gözden geçirin ve Striim örneğini oluşturmak için **Oluştur'u** seçin. 

## <a name="configure-the-source-database"></a>Kaynak veritabanını yapılandırma

Bu bölümde, Oracle veritabanını veri hareketi için kaynak olarak yapılandırırsınız.  Oracle'a bağlanmak için [Oracle JDBC sürücüsüne](https://www.oracle.com/technetwork/database/features/jdbc/jdbc-ucp-122-3110062.html) ihtiyacınız vardır. Kaynak Oracle veritabanınızdaki değişiklikleri okumak için [LogMiner](https://www.oracle.com/technetwork/database/features/availability/logmineroverview-088844.html) veya [XStream API'lerini](https://docs.oracle.com/cd/E11882_01/server.112/e16545/xstrm_intro.htm#XSTRM72647)kullanabilirsiniz. Oracle JDBC sürücüsü, Oracle veritabanından verileri okumak, yazmak veya kalıcı olarak striim'in Java sınıfında bulunmalıdır.

[Ojdbc8.jar](https://www.oracle.com/technetwork/database/features/jdbc/jdbc-ucp-122-3110062.html) sürücüsünü yerel makinenize indirin. Daha sonra Striim kümesine yüklersiniz.

## <a name="configure-target-database"></a>Hedef veritabanını yapılandırma

Bu bölümde, veri hareketinin hedefi olarak Azure Cosmos DB Cassandra API hesabını yapılandırırsınız.

1. Azure portalını kullanarak bir [Azure Cosmos DB Cassandra API hesabı](create-cassandra-dotnet.md#create-a-database-account) oluşturun.

1. Azure Cosmos hesabınızdaki **Veri Gezgini** bölmesine gidin. Yeni bir kapsayıcı oluşturmak için **Yeni Tablo'yu** seçin. *Ürünleri* ve *siparişleri* Oracle veritabanından Azure Cosmos DB'ye geçirdiğinizi varsayalım. Sipariş kapsayıcısıyla **StriimDemo** adında yeni bir Keyspace oluşturun. Konteyneri **1000 RUs**ile sağlama (bu örnekte 1000 RUs kullanılır, ancak iş yükünüz için tahmin edilen iş ortasını kullanmanız gerekir) ve **/ORDER_ID** birincil anahtar olarak. Bu değerler kaynak verilerinize bağlı olarak değişir. 

   ![Cassandra API hesabı oluşturma](./media/cosmosdb-cassandra-api-migrate-data-striim/create-cassandra-api-account.png)

## <a name="configure-oracle-to-azure-cosmos-db-data-flow"></a>Oracle'ı Azure Cosmos DB veri akışına yapılandırma

1. Şimdi Striim'e geri dönelim. Striim ile etkileşime girmeden önce, daha önce indirdiğiniz Oracle JDBC sürücüsünü yükleyin.

1. Azure portalında dağıttığınız Striim örneğine gidin. Üst menü çubuğundaki **Bağlan** düğmesini ve **SSH** sekmesinden Bağlan düğmesini seçin, **VM yerel hesap** alanını kullanarak Giriş'teki URL'yi kopyalayın.

   ![SSH URL'sini alın](./media/cosmosdb-sql-api-migrate-data-striim/get-ssh-url.png)

1. Yeni bir terminal penceresi açın ve Azure portalından kopyaladığınız SSH komutunu çalıştırın. Bu makalede, MacOS terminalkullanır, bir Windows makinede PuTTY veya farklı bir SSH istemcisi kullanarak benzer yönergeleri izleyebilirsiniz. İstendiğinde, devam etmek için **evet** yazın ve önceki adımda sanal makine için ayarladığınız **parolayı** girin.

   ![Striim VM'ye bağlanın](./media/cosmosdb-sql-api-migrate-data-striim/striim-vm-connect.png)

1. Şimdi, daha önce indirdiğiniz **ojdbc8.jar** dosyasını kopyalamak için yeni bir terminal sekmesi açın. Kavanoz dosyasını yerel makinenizden Azure'da çalışan Striim örneğinin tmp klasörüne kopyalamak için aşağıdaki SCP komutunu kullanın:

   ```bash
   cd <Directory_path_where_the_Jar_file_exists> 
   scp ojdbc8.jar striimdemo@striimdemo.westus.cloudapp.azure.com:/tmp
   ```

   ![Jar dosyasını konum makinesinden Striim'e kopyalayın](./media/cosmosdb-sql-api-migrate-data-striim/copy-jar-file.png)

1. Ardından, SSH'yi Striim örneğine ve Giriş'i sudo olarak yaptığınız pencereye geri gidin. **Ojdbc8.jar** dosyasını **/tmp** dizininden Striim örneğinizin **lib** dizinine aşağıdaki komutlarla taşıyın:

   ```bash
   sudo su
   cd /tmp
   mv ojdbc8.jar /opt/striim/lib
   chmod +x ojdbc8.jar
   ```

   ![Jar dosyasını lib klasörüne taşıma](./media/cosmosdb-sql-api-migrate-data-striim/move-jar-file.png)


1. Aynı terminal penceresinden, aşağıdaki komutları çalıştırarak Striim sunucusunu yeniden başlatın:

   ```bash
   Systemctl stop striim-node
   Systemctl stop striim-dbms
   Systemctl start striim-dbms
   Systemctl start striim-node
   ```
 
1. Striim'in başlaması bir dakika sürer. Durumu görmek istiyorsanız, aşağıdaki komutu çalıştırın: 

   ```bash
   tail -f /opt/striim/logs/striim-node.log
   ```

1. Şimdi Azure'a geri gidin ve Striim VM'nizin Genel IP adresini kopyalayın. 

   ![Striim VM IP adresini kopyala](./media/cosmosdb-sql-api-migrate-data-striim/copy-public-ip-address.png)

1. Striim'in Web UI'sine gitmek için tarayıcıda yeni bir sekme açın ve ardından genel IP'yi kopyalayın: 9080. Azure portalında belirttiğiniz yönetici parolasıyla birlikte **yönetici** kullanıcı adını kullanarak oturum açın.

   ![Striim'de oturum açın](./media/cosmosdb-sql-api-migrate-data-striim/striim-login-ui.png)

1. Şimdi Striim'in ana sayfasına geleceksin. Üç farklı bölmeleri vardır - **Panolar**, **Uygulamalar**, ve **SourcePreview**. Panolar bölmesi, verileri gerçek zamanlı olarak taşımanızı ve görselleştirmenizi sağlar. Uygulamalar bölmesi akış veri ardışık alanlarınızı veya veri akışlarınızı içerir. Sayfanın sağ tarafında, verilerinizi taşımadan önce önizebileceğiniz SourcePreview yer nizdedir.

1. **Uygulamalar** bölmesini seçin, şimdilik bu bölmeye odaklanacağız. Striim hakkında bilgi edinmek için kullanabileceğiniz çeşitli örnek uygulamalar vardır, ancak bu makalede kendi uygulamamızı oluşturabilirsiniz. Sağ üst köşedeki **Uygulama Ekle** düğmesini seçin.

   ![Striim uygulamasını ekle](./media/cosmosdb-sql-api-migrate-data-striim/add-striim-app.png)

1. Striim uygulamaları oluşturmak için birkaç farklı yolu vardır. Bu senaryo için **Scratch'den Başlat'ı** seçin.

   ![Uygulamayı sıfırdan başlatın](./media/cosmosdb-cassandra-api-migrate-data-striim/start-app-from-scratch.png)

1. Uygulamanız için, **oraToCosmosDB** gibi bir şey için dostça bir ad verin ve **Kaydet'i**seçin.

   ![Yeni uygulama oluşturma](./media/cosmosdb-cassandra-api-migrate-data-striim/create-new-application.png)

1. Akış uygulamalarınızı oluşturmak için kutu konektörlerini sürükleyip çıkarabileceğiniz Akış Tasarımcısı'na ulaşacaksınız. Arama çubuğuna **Oracle** yazın, **Oracle CDC** kaynağını uygulama tuvaline sürükleyin ve bırakın.  

   ![Oracle CDC kaynak](./media/cosmosdb-cassandra-api-migrate-data-striim/oracle-cdc-source.png)

1. Oracle örneğinizin kaynak yapılandırma özelliklerini girin. Kaynak adı Striim uygulaması için sadece bir adlandırma kuralı, **src_onPremOracle**gibi bir ad kullanabilirsiniz. Ayrıca Bağdaştırıcı türü, bağlantı URL'si, kullanıcı adı, parola, tablo adı gibi diğer ayrıntıları girin. Devam etmek için **Kaydet'i** seçin.

   ![Kaynak parametrelerini yapılandırma](./media/cosmosdb-cassandra-api-migrate-data-striim/configure-source-parameters.png)

1. Şimdi, hedef Azure Cosmos DB örneğini bağlamak için akışın dalga simgesini tıklatın. 

   ![Hedefe bağlanma](./media/cosmosdb-cassandra-api-migrate-data-striim/connect-to-target.png)

1. Hedefi yapılandırmadan önce, [Striim'in Java ortamına baltimore kök sertifikası](/azure/java/java-sdk-add-certificate-ca-store#to-add-a-root-certificate-to-the-cacerts-store)eklediğinizden emin olun.

1. Hedef Azure Cosmos DB örneğinizin yapılandırma özelliklerini girin ve devam etmek için **Kaydet'i** seçin. Dikkat edilmesi gereken temel parametreler şunlardır:

   * **Adaptör** - **DatabaseWriter**kullanın. Azure Cosmos DB Cassandra API'ye yazarken DatabaseWriter gereklidir. Cassandra sürücü 3.6.0 Striim ile birlikte. DatabaseWriter, Azure Cosmos kapsayıcınızda sağlanan RUS sayısını aşarsa, uygulama kilitlenir.

   * **Bağlantı URL'si** - Azure Cosmos DB JDBC bağlantı URL'nizi belirtin. URL formatta`jdbc:cassandra://<contactpoint>:10350/<databaseName>?SSL=true`

   * **Kullanıcı adı** - Azure Cosmos hesap adınızı belirtin.
   
   * **Parola** - Azure Cosmos hesabınızın birincil anahtarını belirtin.

   * **Tablolar** - Hedef tablolarbirincil anahtarlara sahip olmalıdır ve birincil anahtarlar güncelleştirilememelidir.

   ![Hedef özellikleri yapılandırma](./media/cosmosdb-cassandra-api-migrate-data-striim/configure-target-parameters1.png)

   ![Hedef özellikleri yapılandırma](./media/cosmosdb-cassandra-api-migrate-data-striim/configure-target-parameters2.png)

1. Şimdi, devam edeceğiz ve Striim uygulamasını çalıştıracağız. Üst menü çubuğunda **Created'ı**seçin ve ardından **Uygulamayı Dağıt'ı dağıtın.** Dağıtım penceresinde, uygulamanızın belirli bölümlerini dağıtım topolojinizin belirli bölümlerinde çalıştırmak isteyip istemediğinizbelirtebilirsiniz. Azure üzerinden basit bir dağıtım topolojisinde çalışmaya devam ettiğimizden, varsayılan seçeneği kullanırız.

   ![Uygulamayı dağıtma](./media/cosmosdb-cassandra-api-migrate-data-striim/deploy-the-app.png)


1. Şimdi, Striim'den akan verileri görmek için akışı önizleyeceğiz. Dalga simgesini tıklatın ve yanındaki göz simgesine tıklayın. Dağıttıktan sonra, akış üzerinden akan verileri görmek için akışı önizleyebilirsiniz. **Dalga** simgesini ve yanındaki **göz küresini** seçin. Üst menü çubuğunda **Dağıtılan** düğmesini seçin ve **Uygulamayı Başlat'ı**seçin.

   ![Uygulamayı başlatın](./media/cosmosdb-cassandra-api-migrate-data-striim/start-the-app.png)

1. Bir **CDC (Change Data Capture)** okuyucu kullanarak, Striim veritabanında yalnızca yeni değişiklikler alır. Kaynak tablolarınızda veri akışı varsa, bunu görürsünüz. Ancak, bu bir örnek tablo olduğundan, herhangi bir uygulamaya bağlı olmayan kaynak. Örnek bir veri üreteci kullanıyorsanız, Oracle veritabanınıza bir olaylar zinciri ekleyebilirsiniz.

1. Striim platformundan akan verileri göreceksiniz. Striim, tablonuzla ilişkili tüm meta verileri de alır ve bu da verileri izlemek ve verilerin doğru hedefe indiğinden emin olmak için yararlıdır.

   ![CDC boru hattını ayarlama](./media/cosmosdb-cassandra-api-migrate-data-striim/setup-cdc-pipeline.png)

1. Son olarak, Azure'da oturum açalım ve Azure Cosmos hesabınıza gidelim. Veri Gezgini'ni yenileyin ve verilerin geldiğini görebilirsiniz. 

Azure'daki Striim çözümlerini kullanarak, Oracle, Cassandra, MongoDB ve diğer çeşitli kaynaklardan verileri sürekli olarak Azure Cosmos DB'ye aktarabilirsiniz. Daha fazla bilgi için Lütfen [Striim web sitesini](https://www.striim.com/)ziyaret edin, [Striim ücretsiz 30 günlük deneme indirmek](https://go2.striim.com/download-free-trial), ve Striim ile geçiş yolu kurarken herhangi bir sorun için, bir destek isteği [dosyası.](https://go2.striim.com/request-support-striim)

## <a name="next-steps"></a>Sonraki adımlar

* Verileri Azure Cosmos DB SQL API'ye aktarıyorsanız, [Striim kullanarak verileri Cassandra API hesabına nasıl geçirebilirsiniz](cosmosdb-sql-api-migrate-data-striim.md)

* [Azure Cosmos DB ölçümleri ile verilerinizi izleyin ve hata ayıklama](use-metrics.md)
