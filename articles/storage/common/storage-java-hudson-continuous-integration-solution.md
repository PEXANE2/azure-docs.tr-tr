---
title: Blob depolama ile Hudson nasıl kullanılır | Microsoft Dokümanlar
description: Yapı yapıları için bir depo olarak Azure Blob depolama alanı ile Hudson'ın nasıl kullanılacağını açıklar.
services: storage
author: seguler
ms.service: storage
ms.devlang: Java
ms.topic: article
ms.date: 08/13/2019
ms.author: tarcher
ms.subservice: common
ms.openlocfilehash: a89439f49dd53f09d5cd40be0bf2e4981e9235d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77201394"
---
# <a name="using-azure-storage-with-a-hudson-continuous-integration-solution"></a>Hudson Sürekli Tümleştirme çözümüyle Azure Depolama'yı kullanma
## <a name="overview"></a>Genel Bakış
Aşağıdaki bilgiler, Blob depolamaalanının Hudson Sürekli Tümleştirme (CI) çözümü tarafından oluşturulan yapı yapılarının deposu olarak veya bir yapı işleminde kullanılacak indirilebilir dosyaların kaynağı olarak nasıl kullanılacağını gösterir. Bunu yararlı bulacağınız senaryolardan biri, çevik bir geliştirme ortamında (Java veya diğer dilleri kullanarak) kodlama yaparken, yapıların sürekli tümleştirmeye dayalı olarak çalıştığı ve yapı yapılarınız için bir depoya ihtiyaç duyduğunuz senaryolardan biri, örneğin, bunları diğer kuruluş üyeleriyle, müşterilerinizle paylaşın veya bir arşiv idame ettirin.  Başka bir senaryo, yapı işinizin kendisi diğer dosyaları (örneğin, yapı girişinin bir parçası olarak karşıdan yüklenir) gerektirdiğinde.

Bu eğitimde, Microsoft tarafından sunulan Hudson CI için Azure Depolama eklentisini kullanacaksınız.

## <a name="introduction-to-hudson"></a>Hudson'a Giriş
Hudson, geliştiricilerin kod değişikliklerini kolayca entegre etmelerine ve otomatik ve sık üretilen yapılara sahip olmalarını sağlayarak bir yazılım projesinin sürekli entegrasyonunu sağlayarak geliştiricilerin üretkenliğini artırır. Yapılar sürümlenir ve yapı yapıları çeşitli depolara yüklenebilir. Bu makalede, yapı yapılarının deposu olarak Azure Blob depolama alanının nasıl kullanılacağı gösterilecek. Ayrıca, Azure Blob depolamadan bağımlılıkların nasıl indirilemeyeceğini de gösterir.

Hudson hakkında daha fazla bilgi [Meet Hudson](https://wiki.eclipse.org/Hudson-ci/Meet_Hudson)bulunabilir.

## <a name="benefits-of-using-the-blob-service"></a>Blob hizmetini kullanmanın faydaları
Çevik geliştirme yapı yapı yapılarınızı barındırmak için Blob hizmetini kullanmanın yararları şunlardır:

* Yapı yapılarınızın ve/veya indirilebilir bağımlılıklarınızın yüksek kullanılabilirliği.
* Hudson CI çözümünüz yapı yapılarınızı yüklediğinde performans.
* Müşterileriniz ve iş ortaklarınız yapı yapılarınızı indirdiğinde performans.
* Anonim erişim, sona erme tabanlı paylaşılan erişim imza erişimi, özel erişim vb. arasında bir seçimle kullanıcı erişim ilkeleri üzerinde denetim.

## <a name="prerequisites"></a>Ön koşullar
Hudson CI çözümünüzle Blob hizmetini kullanmak için aşağıdakilere ihtiyacınız olacaktır:

* Hudson Sürekli Entegrasyon çözümü.
  
    Şu anda hudson CI çözümünüz yoksa, aşağıdaki tekniği kullanarak Hudson CI çözümlerini çalıştırabilirsiniz:
  
  1. Java özellikli bir [makinede Hudson WAR dosyasını indirin.](https://www.eclipse.org/hudson/download.php)
  2. Hudson WAR içeren klasöre açılan bir komut istemi, Hudson WAR çalıştırın. Örneğin, sürüm 3.1.2'yi indirdiyseniz:
     
      `java -jar hudson-3.1.2.war`

  3. Tarayıcınızda, açık. `http://localhost:8080/` Bu Hudson panosunu açacak.
  4. Hudson ilk kullanımı üzerine, ilk `http://localhost:8080/`kurulum tamamlamak .
  5. İlk kurulumu tamamladıktan sonra Hudson WAR'ın çalışan örneğini iptal edin, Hudson WAR'ı yeniden başlatın ve Azure Depolama eklentisini yüklemek ve yapılandırmak için kullanacağınız Hudson panosunu `http://localhost:8080/`yeniden açın.
     
      Tipik bir Hudson CI çözüm bir hizmet olarak çalıştırmak için ayarlanmış olsa da, komut satırında Hudson savaşı çalışan bu öğretici için yeterli olacaktır.
* Bir Azure hesabı. Bir Azure hesabına <https://www.azure.com>kaydolabilirsiniz.
* Bir Azure depolama hesabı. Zaten bir depolama hesabınız yoksa, [Depolama Hesabı Oluştur'daki](../common/storage-account-create.md)adımları kullanarak bir depo oluşturabilirsiniz.
* Hudson CI çözüm ile aşinalık önerilir ama gerekli değildir, aşağıdaki içerik Hudson CI yapı eserler için bir depo olarak Blob hizmeti kullanırken gerekli adımları göstermek için temel bir örnek kullanacağız.

## <a name="how-to-use-the-blob-service-with-hudson-ci"></a>Hudson CI ile Blob hizmeti nasıl kullanılır?
Blob hizmetini Hudson ile kullanmak için Azure Depolama eklentisini yüklemeniz, eklentiyi depolama hesabınızı kullanacak şekilde yapılandırmanız ve ardından yapı yapılarınızı depolama hesabınıza yükleyen bir yapı sonrası eylem oluşturmanız gerekir. Bu adımlar aşağıdaki bölümlerde açıklanmıştır.

## <a name="how-to-install-the-azure-storage-plugin"></a>Azure Depolama eklentisi nasıl yüklenir?
1. Hudson panosu içinde, **Hudson'ı Yönet'i**tıklatın.
2. **Hudson'ı Yönet** sayfasında **Eklentileri Yönet'i**tıklatın.
3. **Kullanılabilir** sekmesini tıklatın.
4. **Diğerleri'ni**tıklatın.
5. **Artefakt Yükleyiciler** bölümünde **Microsoft Azure Depolama eklentisini**seçin.
6. **Yükle'yi**tıklatın.
7. Yükleme tamamlandıktan sonra Hudson'ı yeniden başlatın.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Azure Depolama eklentisini depolama hesabınızı kullanmak üzere yapılandırma
1. Hudson panosu içinde, **Hudson'ı Yönet'i**tıklatın.
2. **Hudson'ı Yönet** sayfasında, **Sistemi Yapılandır'ı**tıklatın.
3. Microsoft **Azure Depolama Hesabı Yapılandırması** bölümünde:
   
    a. [Azure portalından](https://portal.azure.com)edinebileceğiniz depolama hesabı adınızı girin.
   
    b. [Azure portalından](https://portal.azure.com)da elde edilen depolama hesabı anahtarınızı girin.
   
    c. Genel Azure bulutu kullanıyorsanız **Blob Service Endpoint URL** için varsayılan değeri kullanın. Farklı bir Azure bulutu kullanıyorsanız, depolama hesabınız için [Azure portalında](https://portal.azure.com) belirtildiği gibi bitiş noktasını kullanın.
   
    d. Depolama hesabınızı doğrulamak için **depolama kimlik bilgilerini doğrula'yı** tıklatın.
   
    e. [İsteğe bağlı] Hudson CI'niziçin kullanılabilir hale getirin istediğiniz ek depolama alanı hesaplarınız varsa, **daha fazla depolama hesabı ekle'yi**tıklatın.
   
    f. Ayarlarınızı kaydetmek için **Kaydet'i** tıklatın.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Yapı yapılarınızı depolama hesabınıza yükleyen bir yapı sonrası eylem oluşturma
Talimat amacıyla, önce birkaç dosya oluşturacak bir iş oluşturmamız ve ardından dosyaları depolama hesabınıza yüklemek için yapı sonrası eylemi eklememiz gerekir.

1. Hudson panosu içinde, **Yeni İş'i**tıklatın.
2. İşi **MyJob**olarak adlandırın, **serbest stil yazılım işi oluşturun'u**tıklatın ve ardından **Tamam'ı**tıklatın.
3. İş yapılandırmasının **Yapı** bölümünde, **yapı adımı ekle'yi** tıklatın ve **Windows toplu iş komutunu yürüt'ü**seçin.
4. **Komut'ta,** aşağıdaki komutları kullanın:

    ```   
        md text
        cd text
        echo Hello Azure Storage from Hudson > hello.txt
        date /t > date.txt
        time /t >> date.txt
    ```

5. İş yapılandırmasının **Yapı Sonrası Eylemler** bölümünde, **yapıyı Microsoft Azure Blob depolama alanına yükle'yi**tıklatın.
6. **Depolama Hesabı Adı**için kullanılacak depolama hesabını seçin.
7. **Kapsayıcı Adı**için, kapsayıcı adını belirtin. (Yapı yapıları yüklendiğinde kapsayıcı zaten yoksa oluşturulur.) Ortam değişkenlerini kullanabilirsiniz, bu nedenle bu örnek için kapsayıcı adı olarak **${JOB_NAME}** girin.
   
    **İpucu**
   
    **Windows toplu komutu için** bir komut dosyası girdiğiniz **Komut** bölümünün altında Hudson tarafından tanınan ortam değişkenlerine bir bağlantı yer almaktadır. Ortam değişken adlarını ve açıklamalarını öğrenmek için bu bağlantıyı tıklatın. **BUILD_URL** ortam değişkeni gibi özel karakterler içeren ortam değişkenlerine kapsayıcı adı veya ortak sanal yol olarak izin verilmez.
8. Bu örnekte **varsayılan olarak yeni kapsayıcıyı herkese açık hale getir'i** tıklatın. (Özel bir kapsayıcı kullanmak istiyorsanız, erişime izin vermek için paylaşılan bir erişim imzası oluşturmanız gerekir. Bu makalenin kapsamı dışındadır. [Paylaşılan Erişim İmzalarını (SAS) Kullanarak](storage-sas-overview.md)paylaşılan erişim imzaları hakkında daha fazla bilgi edinebilirsiniz.)
9. [İsteğe bağlı] Yapı yapıları yüklenmeden önce kapsayıcının içeriğinden temizlenmesini istiyorsanız **yüklemeden önce kapsayıcıyı** temizle'yi tıklatın (kapsayıcının içeriğini temizlemek istemiyorsanız işaretsiz bırakın).
10. **Yüklenmesi Gereken Eserler Listesi**için **metin girin/*.txt**.
11. **Yüklenen yapılar için ortak sanal yol için** **${BUILD\_ID}/${BUILD\_NUMBER}** girin.
12. Ayarlarınızı kaydetmek için **Kaydet'i** tıklatın.
13. Hudson panosunda, **MyJob'u**çalıştırmak için **Şimdi Oluştur'u** tıklatın. Durum için konsol çıktısını inceleyin. Yapı oluşturma eylemi yapı yapılarını yüklemeye başladığında Azure Depolama'nın durum iletileri konsol çıktısına eklenir.
14. İşin başarıyla tamamlanmasından sonra, genel blob açarak yapı eserler inceleyebilirsiniz.
    
    a. [Azure portalında](https://portal.azure.com)oturum açın.
    
    b. **Depolama'yı**tıklatın.
    
    c. Hudson için kullandığınız depolama hesabı adını tıklatın.
    
    d. **Kapsayıcılar**'a tıklayın.
    
    e. Hudson işini oluşturduğunuzda atadığınız iş adının küçük versiyonu olan **myjob**adlı kapsayıcıyı tıklatın. Kapsayıcı adları ve blob adları Azure Depolama'da küçük (ve büyük/küçük harf duyarlıdır). **Myjob** adlı konteyner için blobs listesi içinde **hello.txt ve date.txt** görmelisiniz . **date.txt** Bu öğelerden birinin URL'sini kopyalayın ve tarayıcınızda açın. Yapı oluşturma olarak yüklenen metin dosyasını görürsünüz.

Yapı larını Azure Blob depolama alanına yükleyen yalnızca bir yapı sonrası eylem iş başına oluşturulabilir. Yapıları Azure Blob depolama alanına yüklemek için yapılan tek bir yapı sonrası eylem, ayırıcı olarak yarı iki nokta üst üste yüklemek için Yapı **Listesi'ndeki dosyalara** farklı dosyalar (joker karakterler dahil) ve dosyalara giden yolları belirtebilir. Örneğin, Hudson yapınız çalışma alanınızın **yapı** klasöründe JAR dosyaları ve TXT dosyaları oluşturuyorsa ve her ikisini de Azure Blob depolama alanına yüklemek istiyorsanız, değer yüklemek için **Yapı Listesi** için aşağıdakileri kullanın: **build/\*.jar;build/\*.txt**. Blob adı içinde kullanılacak bir yol belirtmek için çift nokta noktalı sözdizimini de kullanabilirsiniz. Örneğin, JAR blob yolunda **ikililer** ve blob yolunda **bildirimler** kullanarak yüklenen almak için TXT dosyaları kullanarak yüklemek istiyorsanız, değer yüklemek için **Eserler Listesi** için aşağıdaki kullanın: **build/\*\*.jar::binaries;build/ .txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Azure Blob depolama dan indiren bir yapı adımı oluşturma
Aşağıdaki adımlar, Azure Blob depolama dan öğeleri indirmek için bir yapı adımı nasıl yapılandırılabildiğini gösterir. Bu, yapınıza (örneğin, Azure Blob depolama alanında sakladığınız JAR'lar) öğeleri eklemek istiyorsanız yararlı olacaktır.

1. İş yapılandırmasının **Yapı** bölümünde yapı **adımını ekle'yi** tıklatın ve Azure **Blob depolama alanından İndir'i**seçin.
2. **Depolama hesabı adı için**kullanılacak depolama hesabını seçin.
3. **Kapsayıcı adı için,** indirmek istediğiniz lekelere sahip kapsayıcının adını belirtin. Ortam değişkenlerini kullanabilirsiniz.
4. **Blob adı için,** blob adını belirtin. Ortam değişkenlerini kullanabilirsiniz. Ayrıca, blob adının ilk harfini(ler) belirttikten sonra joker karakter olarak yıldız işareti kullanabilirsiniz. Örneğin, **proje\\*** adları **project**ile başlayan tüm lekeleri belirtir.
5. [İsteğe bağlı] **İndirme yolu**için, Azure Blob depolamasundan dosya indirmek istediğiniz Hudson makinesindeki yolu belirtin. Ortam değişkenleri de kullanılabilir. **(İndirme yolu**için bir değer sağlamazsanız, Azure Blob depolama alanındaki dosyalar işin çalışma alanına indirilir.)

Azure Blob depolama sundan indirmek istediğiniz ek öğeler varsa, ek yapı adımları oluşturabilirsiniz.

Bir yapıyı çalıştırdıktan sonra, beklediğiniz lekelerin başarıyla indirilip indirilmediğini görmek için yapı geçmişi konsolçıktısını denetleyebilir veya indirme konumunuza bakabilirsiniz.

## <a name="components-used-by-the-blob-service"></a>Blob hizmeti tarafından kullanılan bileşenler
Aşağıda Blob hizmet bileşenlerine genel bir bakış sağlar.

* **Depolama hesabı**: Azure Depolama'ya tüm erişim bir depolama hesabı üzerinden yapılır. Bu, lekelere erişmek için ad alanının en yüksek düzeyidir. Bir hesap, toplam boyutu 100 TB'nin altında olduğu sürece sınırsız sayıda kapsayıcı içerebilir.
* **Konteyner**: Konteyner, bir dizi blob'un gruplandırması sağlar. Tüm bloblar bir kapsayıcıda olmalıdır. Bir hesapta sınırsız sayıda kapsayıcı olabilir. Kapsayıcıda sınırsız sayıda blob depolanabilir.
* **Blob**: Her tür ve boyutta bir dosya. Azure Depolama'da depolanabilecek iki tür blob vardır: blok ve sayfa lekeleri. Çoğu dosya blok lekeleri vardır. Tek bir blok blob boyutu 200 GB'a kadar olabilir. Bu öğretici blok lekeleri kullanır. Başka bir blob türü olan sayfa lekeleri, boyutu 1 TB'a kadar olabilir ve bir dosyadaki bayt aralıkları sık sık değiştirildiğinde daha verimlidir. Lekeler hakkında daha fazla bilgi için [Bkz.](https://msdn.microsoft.com/library/azure/ee691964.aspx)
* **URL biçimi**: Blobs aşağıdaki URL biçimi kullanılarak adreslenebilir:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (Yukarıdaki biçim genel Azure bulutu için geçerlidir. Farklı bir Azure bulutu kullanıyorsanız, URL bitiş noktanızı belirlemek için [Azure portalındaki](https://portal.azure.com) bitiş noktasını kullanın.)
  
    Yukarıdaki biçimde, `storageaccount` depolama hesabınızın adını `container_name` temsil eder, kapsayıcınızın `blob_name` adını temsil eder ve sırasıyla blob'unuzun adını temsil eder. Kapsayıcı adı içinde, bir ileri eğik çizgi ile **/** ayrılmış birden çok yol olabilir. Bu öğreticideki örnek kapsayıcı adı **MyJob**idi ve **\_${BUILD\_ID}/${BUILD NUMBER}** ortak sanal yol için kullanıldı ve bu da blob'un aşağıdaki formun URL'si olmasına neden oldu:
  
    `http://example.blob.core.windows.net/myjob/2014-05-01_11-56-22/1/hello.txt`

## <a name="next-steps"></a>Sonraki adımlar
* [Hudson ile tanışın](https://wiki.eclipse.org/Hudson-ci/Meet_Hudson)
* [Java için Azure Depolama SDK'sı](https://github.com/azure/azure-storage-java)
* [Azure Depolama İstemcisi SDK Başvurusu](https://javadoc.io/doc/com.microsoft.azure/azure-core/0.8.0/index.html)
* [Azure Storage Hizmetleri REST API’si](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Depolama Ekibi Blogu](https://blogs.msdn.com/b/windowsazurestorage/)

Daha fazla bilgi için bkz. [Java geliştiricileri için Azure](/java/azure).
