---
title: Striim kullanarak verileri Azure Cosmos DB SQL API hesabına geçirin
description: Verileri Oracle veritabanından Azure Cosmos DB SQL API hesabına geçirmek için Striim'i nasıl kullanacağınızı öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: sngun
ms.reviewer: sngun
ms.openlocfilehash: 36ba9e2d3385184f32876a6d067b58f7c21a90bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71003282"
---
# <a name="migrate-data-to-azure-cosmos-db-sql-api-account-using-striim"></a>Striim kullanarak verileri Azure Cosmos DB SQL API hesabına geçirin
 
Azure pazarındaki Striim görüntüsü, veri ambarlarından veritabanlarından Azure'a sürekli gerçek zamanlı veri hareketi sunar. Verileri hareket ettirirken, satır içi denormalleştirme, veri dönüşümü, gerçek zamanlı analiz ve veri raporlama senaryoları etkinleştirme gerçekleştirebilirsiniz. Kurumsal verileri sürekli olarak Azure Cosmos DB SQL API'ye taşımak için Striim ile başlamak kolaydır. Azure, Striim'i dağıtmayı ve verileri Azure Cosmos DB'ye taşımayı kolaylaştıran bir pazar teklifi sunar. 

Bu makalede, verileri **Oracle veritabanından** **Azure Cosmos DB SQL API hesabına**geçirmek için Striim'in nasıl kullanılacağı gösterilmektedir.

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
   |Striim dağıtım türü |Tek Başına | Striim **Bağımsız** veya **Küme** dağıtım türlerinde çalıştırılabilir. Bağımsız mod, Striim sunucusunu tek bir sanal makineye dağıtır ve veri hacminize bağlı olarak VM'lerin boyutunu seçebilirsiniz. Küme modu, Striim sunucusunu seçili boyuta sahip iki veya daha fazla VM'ye dağıtır. 2'den fazla düğüme sahip küme ortamları otomatik yüksek kullanılabilirlik ve başarısızlık sunar.</br></br> Bu eğitimde, Bağımsız seçeneğini seçebilirsiniz. Varsayılan "Standard_F4s" boyutu VM kullanın.  | 
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

## <a name="configure-the-target-database"></a>Hedef veritabanını yapılandırma

Bu bölümde, veri hareketinin hedefi olarak Azure Cosmos DB SQL API hesabını yapılandırırsınız.

1. Azure portalını kullanarak bir [Azure Cosmos DB SQL API hesabı](create-cosmosdb-resources-portal.md) oluşturun.

1. Azure Cosmos hesabınızdaki **Veri Gezgini** bölmesine gidin. Yeni bir kapsayıcı oluşturmak için **Yeni Kapsayıcı'yı** seçin. *Ürünleri* ve *siparişleri* Oracle veritabanından Azure Cosmos DB'ye geçirdiğinizi varsayalım. **Siparişler**adlı bir kapsayıcı ile **StriimDemo** adlı yeni bir veritabanı oluşturun. Kapsayıcıyı **1000 RUs** ile sağlama (bu örnekte 1000 RUs kullanılır, ancak iş yükünüz için tahmin edilen iş bilgili iş bilgisini kullanmanız gerekir) ve **/ORDER_ID** bölüm anahtarı olarak. Bu değerler kaynak verilerinize bağlı olarak değişir. 

   ![SQL API hesabı oluşturma](./media/cosmosdb-sql-api-migrate-data-striim/create-sql-api-account.png)

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

1. Striim uygulamaları oluşturmak için birkaç farklı yolu vardır. Varolan bir şablonla başlamak için **Şablonla Başlat'ı** seçin.

   ![Uygulamayı şablonla başlatın](./media/cosmosdb-sql-api-migrate-data-striim/start-with-template.png)

1. Arama **şablonları** alanında "Cosmos" yazın ve **Hedef: Azure Cosmos DB'yi** seçin ve ardından **Azure Cosmos DB'ye Oracle CDC'yi**seçin.

   ![Cosmos DB için Oracle CDC'yi seçin](./media/cosmosdb-sql-api-migrate-data-striim/oracle-cdc-cosmosdb.png)

1. Bir sonraki sayfada, başvurunuzu adlandırın. **OraToCosmosDB** gibi bir ad sağlayabilir ve sonra **Kaydet'i**seçebilirsiniz.

1. Ardından, kaynak Oracle örneğinizin kaynak yapılandırmasını girin. **Kaynak Adı**için bir değer girin. Kaynak adı Striim uygulaması için sadece bir adlandırma kuralı, **src_onPremOracle**gibi bir şey kullanabilirsiniz. Kaynak **parametreleri URL**, **Kullanıcı Adı**, **Şifre**, Oracle verileri okumak için okuyucu olarak **LogMiner** seçin geri kalanı için değerleri girin. Devam etmek için **İleri**’yi seçin.

   ![Kaynak parametrelerini yapılandırma](./media/cosmosdb-sql-api-migrate-data-striim/configure-source-parameters.png)

1. Striim ortamınızı kontrol edecek ve kaynak Oracle örneğinize bağlanabilmesini, doğru ayrıcalıklara sahip olduğundan ve CDC'nin düzgün şekilde yapılandırıldığından emin olacaktır. Tüm değerler doğrulandıktan sonra **İleri'yi**seçin.

   ![Kaynak parametrelerini doğrulama](./media/cosmosdb-sql-api-migrate-data-striim/validate-source-parameters.png)

1. Geçirmek istediğiniz Oracle veritabanından tabloları seçin. Örneğin, Siparişler tablosunu seçelim ve **İleri'yi**seçelim. 

   ![Kaynak tabloları seçin](./media/cosmosdb-sql-api-migrate-data-striim/select-source-tables.png)

1. Kaynak tabloyu seçtikten sonra, eşleme ve filtreleme gibi daha karmaşık işlemler yapabilirsiniz. Bu durumda, azure cosmos DB'de kaynak tablonuzun bir kopyasını oluşturursunuz. Bu nedenle, hedefi yapılandırmak için **İleri'yi** seçin

1. Şimdi, hedefi yapılandıralım:

   * **Hedef Adı** - Hedef için dostça bir ad sağlayın. 
   * **Giriş Gönderen** - Açılan listeden, kaynak Oracle yapılandırmasında oluşturduğunuz giriş akışını seçin. 
   * **Koleksiyonlar**- Hedef Azure Cosmos DB yapılandırma özelliklerini girin. Koleksiyonlar sözdizimi **SourceSchema.SourceTable, TargetDatabase.TargetContainer**olduğunu. Bu örnekte, değer "SİsteM" olacaktır. SİPARİş, StriimDemo.Orders". 
   * **AccessKey** - Azure Cosmos hesabınızın Birincil Anahtarı.
   * **ServiceEndpoint** – Azure Cosmos hesabınızın URI'si, Azure portalının **Keys** bölümünde bulunabilir. 

   **Kaydet** ve **İleri'yi**seçin.

   ![Hedef parametreleri yapılandırma](./media/cosmosdb-sql-api-migrate-data-striim/configure-target-parameters.png)


1. Ardından, akış uygulamalarınızı oluşturmak için kutu konektörlerini sürükleyip çıkarabileceğiniz akış tasarımcısına ulaşacaksınız. Bu noktada akışta herhangi bir değişiklik yapmazsınız. bu nedenle Uygulamayı **Dağıt** düğmesini seçerek uygulamayı dağıtın.
 
   ![Uygulamayı dağıtma](./media/cosmosdb-sql-api-migrate-data-striim/deploy-app.png)

1. Dağıtım penceresinde, uygulamanızın belirli bölümlerini dağıtım topolojinizin belirli bölümlerinde çalıştırmak isteyip istemediğinizbelirtebilirsiniz. Azure üzerinden basit bir dağıtım topolojisinde çalışmaya devam ettiğimizden, varsayılan seçeneği kullanırız.

   ![Varsayılan seçeneği kullanma](./media/cosmosdb-sql-api-migrate-data-striim/deploy-using-default-option.png)

1. Dağıttıktan sonra, akış üzerinden akan verileri görmek için akışı önizleyebilirsiniz. **Dalga** simgesini ve yanındaki göz küresini seçin. Üst menü çubuğunda **Dağıtılan** düğmesini seçin ve **Uygulamayı Başlat'ı**seçin.

   ![Uygulamayı başlatın](./media/cosmosdb-sql-api-migrate-data-striim/start-app.png)

1. Bir **CDC (Change Data Capture)** okuyucu kullanarak, Striim veritabanında yalnızca yeni değişiklikler alır. Kaynak tablolarınızda veri akışı varsa, bunu görürsünüz. Ancak, bu bir demo tablo olduğundan, kaynak herhangi bir uygulamaya bağlı değildir. Örnek bir veri üreteci kullanıyorsanız, Oracle veritabanınıza bir olaylar zinciri ekleyebilirsiniz.

1. Striim platformundan akan verileri göreceksiniz. Striim, tablonuzla ilişkili tüm meta verileri de alır ve bu da verileri izlemek ve verilerin doğru hedefe indiğinden emin olmak için yararlıdır.

   ![CDC boru hattını yapılandırma](./media/cosmosdb-sql-api-migrate-data-striim/configure-cdc-pipeline.png)

1. Son olarak, Azure'da oturum açalım ve Azure Cosmos hesabınıza gidelim. Veri Gezgini'ni yenileyin ve verilerin geldiğini görebilirsiniz.  

   ![Azure'da geçirilen verileri doğrulama](./media/cosmosdb-sql-api-migrate-data-striim/portal-validate-results.png)

Azure'daki Striim çözümlerini kullanarak, Oracle, Cassandra, MongoDB ve diğer çeşitli kaynaklardan verileri sürekli olarak Azure Cosmos DB'ye aktarabilirsiniz. Daha fazla bilgi için Lütfen [Striim web sitesini](https://www.striim.com/)ziyaret edin, [Striim ücretsiz 30 günlük deneme indirmek](https://go2.striim.com/download-free-trial), ve Striim ile geçiş yolu kurarken herhangi bir sorun için, bir destek isteği [dosyası.](https://go2.striim.com/request-support-striim)

## <a name="next-steps"></a>Sonraki adımlar

* Verileri Azure Cosmos DB SQL API'ye aktarıyorsanız, [Striim kullanarak verileri Cassandra API hesabına nasıl geçirebilirsiniz](cosmosdb-cassandra-api-migrate-data-striim.md)

* [Azure Cosmos DB ölçümleri ile verilerinizi izleyin ve hata ayıklama](use-metrics.md)
