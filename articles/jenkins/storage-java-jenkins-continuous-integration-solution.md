---
title: Jenkins sürekli tümleştirme çözümüyle Azure Depolama'yı kullanma
description: Bu öğretici, Jenkins sürekli tümleştirme çözümü tarafından oluşturulan yapı yapıları için bir depo olarak Azure blob hizmetinin nasıl kullanılacağını gösterir.
keywords: jenkins, masmavi, devops, depolama, cicd
ms.topic: article
ms.date: 08/13/2019
ms.openlocfilehash: df1d59c40fd09fb055db9d7622d86ff9c82991b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77624689"
---
# <a name="using-azure-storage-with-a-jenkins-continuous-integration-solution"></a>Jenkins sürekli tümleştirme çözümüyle Azure Depolama'yı kullanma

Bu makalede, Bir Jenkins sürekli tümleştirme (CI) çözümü tarafından oluşturulan yapı yapıları deposu olarak blob depolama nasıl kullanılacağını veya bir yapı işleminde kullanılacak indirilebilir dosyaların kaynağı olarak gösterin. Bu çözümü yararlı bulacağınız senaryolardan biri, çevik bir geliştirme ortamında (Java veya diğer dilleri kullanarak) kodlama yaparken, yapıların sürekli tümleştirmeye dayalı olarak çalıştığı ve yapı yapılarınız için bir depoya ihtiyaç duyduğunuz senaryodur, böylece örneğin, bunları diğer kuruluş üyeleriyle, müşterilerinizle paylaşabilir veya bir arşiv idame ettirebilirsiniz. Başka bir senaryo, yapı işinizin kendisi diğer dosyaları (örneğin, yapı girişinin bir parçası olarak karşıdan yüklenir) gerektirdiğinde.

Bu eğitimde, Microsoft tarafından sunulan Jenkins CI için Azure Depolama Eklentisi'ni kullanacaksınız.

## <a name="jenkins-overview"></a>Jenkins'e genel bakış

Jenkins, geliştiricilerin kod değişikliklerini kolayca entegre etmelerine ve otomatik ve sık üretilen yapılara sahip olmalarına izin vererek bir yazılım projesinin sürekli entegrasyonunu sağlayarak geliştiricilerin üretkenliğini artırır. Yapılar sürümlenir ve yapı yapıları çeşitli depolara yüklenebilir. Bu makalede, yapı yapılarının deposu olarak Azure blob depolama nasıl kullanılacağı nı gösterilmektedir. Ayrıca, Azure blob depolamadan bağımlılıkların nasıl indirilemeyeceğini de gösterir.

Jenkins hakkında daha fazla bilgiyi [Meet Jenkins'te](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)bulabilirsiniz.

## <a name="benefits-of-using-the-blob-service"></a>Blob hizmetini kullanmanın faydaları

Çevik geliştirme yapı yapı yapılarınızı barındırmak için Blob hizmetini kullanmanın yararları şunlardır:

* Yapı yapılarınızın ve/veya indirilebilir bağımlılıklarınızın yüksek kullanılabilirliği.
* Jenkins CI çözümünüz yapı yapılarınızı yüklediğinde performans.
* Müşterileriniz ve iş ortaklarınız yapı yapılarınızı indirdiğinde performans.
* Anonim erişim, sona erme tabanlı paylaşılan erişim imza erişimi, özel erişim vb. arasında bir seçimle kullanıcı erişim ilkeleri üzerinde denetim.

## <a name="prerequisites"></a>Ön koşullar

* Jenkins sürekli entegrasyon çözümü.
  
    Şu anda Jenkins CI çözümünüz yoksa, aşağıdaki tekniği kullanarak jenkins CI çözümlerini çalıştırabilirsiniz:
  
  1. Java özellikli bir makinede jenkins.war'dan <https://jenkins-ci.org>indirin.
  2. Jenkins.war içeren klasöre açılan komut isteminde çalıştır:
     
      `java -jar jenkins.war`

  3. Tarayıcınızda, Azure `http://localhost:8080/` Depolama eklentisini yüklemek ve yapılandırmak için kullanacağınız Jenkins panosunu açmak için açın.
     
      Tipik bir Jenkins CI çözüm bir hizmet olarak çalıştırmak için ayarlanmış olsa da, komut satırında Jenkins savaş çalışan bu öğretici için yeterli olacaktır.
* Bir Azure hesabı. Bir Azure hesabına <https://www.azure.com>kaydolabilirsiniz.
* Bir Azure depolama hesabı. Zaten bir depolama hesabınız yoksa, [Depolama Hesabı Oluştur'daki](../storage/common/storage-account-create.md)adımları kullanarak bir depo oluşturabilirsiniz.
* Jenkins CI çözümüne aşinalık önerilir, ancak gerekli değildir, çünkü aşağıdaki içerik, Blob hizmetini Jenkins CI yapı yapıları için bir depo olarak kullanırken gereken adımları göstermek için temel bir örnek kullanır.

## <a name="how-to-use-the-blob-service-with-jenkins-ci"></a>Jenkins CI ile Blob hizmeti nasıl kullanılır?
Blob hizmetini Jenkins ile kullanmak için Azure Depolama eklentisini yüklemeniz, eklentiyi depolama hesabınızı kullanacak şekilde yapılandırmanız ve ardından yapı yapılarınızı depolama hesabınıza yükleyen bir yapı sonrası eylem oluşturmanız gerekir. Bu adımlar aşağıdaki bölümlerde açıklanmıştır.

## <a name="how-to-install-the-azure-storage-plugin"></a>Azure Depolama eklentisi nasıl yüklenir?
1. Jenkins panosu **içinde, Jenkins'i Yönet'i**seçin.
2. **Jenkins'i Yönet** sayfasında **Eklentileri Yönet'i**seçin.
3. **Available** (Kullanılabilir) sekmesini seçin.
4. **Artefakt Yükleyiciler** bölümünde Microsoft Azure Depolama eklentisini kontrol **edin.**
5. Yeniden **başlatmadan Yükle'yi** seçin veya **şimdi indirin ve yeniden başlattıktan sonra yükleyin.**
6. Jenkins'i yeniden başlat.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Azure Depolama eklentisini depolama hesabınızı kullanmak üzere yapılandırma
1. Jenkins panosu **içinde, Jenkins'i Yönet'i**seçin.
2. **Jenkins'i Yönet** **sayfasında, Sistemi Yapılandır'ı**seçin.
3. Microsoft **Azure Depolama Hesabı Yapılandırması** bölümünde:
   1. [Azure portalından](https://portal.azure.com)edinebileceğiniz depolama hesabı adınızı girin.
   2. [Azure portalından](https://portal.azure.com)da elde edilen depolama hesabı anahtarınızı girin.
   3. Genel Azure bulutu kullanıyorsanız **Blob Service Endpoint URL** için varsayılan değeri kullanın. Farklı bir Azure bulutu kullanıyorsanız, depolama hesabınız için [Azure portalında](https://portal.azure.com) belirtildiği gibi bitiş noktasını kullanın. 
   4. Depolama hesabınızı doğrulamak için **depolama kimlik bilgilerini doğrula'yı** seçin. 
   5. [İsteğe bağlı] Jenkins CI'nizin kullanımına sunulmasını istediğiniz ek depolama hesabınız varsa, **daha fazla Depolama Hesabı Ekle'yi**seçin.
   6. Ayarlarınızı kaydetmek için **Kaydet'i** seçin.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Yapı yapılarınızı depolama hesabınıza yükleyen bir yapı sonrası eylem oluşturma
Öğretim amacıyla, önce birkaç dosya oluşturacak bir iş oluşturmanız ve ardından dosyaları depolama hesabınıza yüklemek için yapı sonrası eylem eklemeniz gerekir.

1. Jenkins panosu içinde **Yeni Öğe'yi**seçin.
2. **İşmyJob'u**adlandırın, **serbest stil yazılım projesi oluşturun'u**seçin ve ardından **Tamam'ı**seçin.
3. İş yapılandırmasının **Yapı** bölümünde yapı **adımını ekle'yi** seçin ve Windows toplu iş **komutunu yürüt'ü**seçin.
4. **Komut'ta,** aşağıdaki komutları kullanın:

    ```   
    md text
    cd text
    echo Hello Azure Storage from Jenkins > hello.txt
    date /t > date.txt
    time /t >> date.txt
    ```

5. İş yapılandırmasının **Yapı Sonrası Eylemler** bölümünde, yapı sonrası eylem **ekle'yi** seçin ve **Yapı Yıkın depolama alanına Yapı Yükle'yi**seçin.
6. **Depolama hesabı adı için**kullanılacak depolama hesabını seçin.
7. **Kapsayıcı adı için,** kapsayıcı adını belirtin. (Yapı yapıları yüklendiğinde kapsayıcı zaten yoksa oluşturulur.) Ortam değişkenlerini kullanabilirsiniz, bu nedenle `${JOB_NAME}` bu örnek için kapsayıcı adı olarak girin.
   
    **İpucu**
   
    **Windows toplu komutu için** bir komut dosyası girdiğiniz **Komut** bölümünün altında Jenkins tarafından tanınan ortam değişkenlerine bir bağlantı bulunmaktadır. Ortam değişken adlarını ve açıklamalarını öğrenmek için bu bağlantıyı seçin. **BUILD_URL** ortam değişkeni gibi özel karakterler içeren ortam değişkenlerine kapsayıcı adı veya ortak sanal yol olarak izin verilmez.
8. Bu örnekte **varsayılan olarak yeni kapsayıcıyı herkese açık hale getir'i** seçin. (Özel bir kapsayıcı kullanmak istiyorsanız, erişime izin vermek için bu makalenin kapsamı dışında olan paylaşılan bir erişim imzası oluşturmanız gerekir. [Paylaşılan Erişim İmzalarını (SAS) Kullanarak](../storage/common/storage-sas-overview.md)paylaşılan erişim imzaları hakkında daha fazla bilgi edinebilirsiniz.)
9. [İsteğe bağlı] Yapı yapıları yüklenmeden önce kapsayıcının içeriğinden temizlenmesini istiyorsanız **yüklemeden önce temizle kapsayıcıyı** seçin (kapsayıcının içeriğini temizlemek istemiyorsanız işaretsiz bırakın).
10. **Yüklenmesi Gereken Eserler Listesi** `text/*.txt`için , girin .
11. **Yüklenen eserler için Ortak sanal yol için**, bu `${BUILD\_ID}/${BUILD\_NUMBER}`öğretici amaçları için, girin .
12. Ayarlarınızı kaydetmek için **Kaydet'i** seçin.
13. Jenkins panosunda, **MyJob'u**çalıştırmak için **Şimdi Oluştur'u** seçin. Durum için konsol çıktısını inceleyin. Yapı sonrası eylem yapı yapılarını yüklemeye başladığında, Azure depolamasına yönelik durum iletileri konsol çıktısına eklenir.
14. İşin başarıyla tamamlanmasından sonra, genel blob açarak yapı eserler inceleyebilirsiniz.
    1. [Azure portalında](https://portal.azure.com)oturum açın.
    2. **Depolama**’yı seçin.
    3. Jenkins için kullandığınız depolama hesabı adını seçin.
    4. **Kapsayıcılar'ı**seçin.
    5. Jenkins işini oluşturduğunuzda atadığınız iş adının küçük versiyonu olan **myjob**adlı kapsayıcıyı seçin. Kapsayıcı adları ve blob adları Azure depolama alanında küçük (ve büyük/küçük harf duyarlıdır). **Myjob**adlı konteyner için blobs listesi içinde , **hello.txt ve date.txt** görmelisiniz. **date.txt** Bu öğelerden birinin URL'sini kopyalayın ve tarayıcınızda açın. Yapı oluşturma olarak yüklenen metin dosyasını görürsünüz.

Yapı larını Azure blob depolama alanına yükleyen yalnızca bir yapı sonrası eylem iş başına oluşturulabilir. Yapıları Azure blob depolamaalanına yüklemek için yapılan tek bir yapı sonrası eylem, ayırıcı olarak yarı iki nokta üst üste yüklemek için Yapı **Listesi'ndeki dosyalara** farklı dosyalar (joker karakterler dahil) ve dosyalara giden yolları belirtebilir. Örneğin, Jenkins yapınız çalışma alanınızın **yapı** klasöründe JAR dosyaları ve TXT dosyaları oluşturuyorsa ve her ikisini de Azure blob depolama alanına `build/\*.jar;build/\*.txt`yüklemek istiyorsanız, yükleme seçeneği için Yapı **Listesi** için aşağıdaki değeri kullanın: . Blob adı içinde kullanılacak bir yol belirtmek için çift nokta noktalı sözdizimini de kullanabilirsiniz. Örneğin, JAR'ların blob **yolundaki ikili dosyalar** la yüklenmesini ve TXT dosyalarının blob yolundaki **bildirimleri** kullanarak yüklenmesini istiyorsanız, yükleme seçeneği için `build/\*.jar::binaries;build/\*.txt::notices`Eserler **Listesi** için aşağıdaki değeri kullanın: .

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Azure blob depolamadan indirilen bir yapı adımı oluşturma
Aşağıdaki adımlar, Azure blob depolama alanından öğeleri indirmek için bir yapı adımı yapılandırmayı göstermektedir ve bu adım, yapınıza öğeleri eklemek istiyorsanız yararlıdır. Bu deseni kullanmaya örnek olarak, Azure blob depolama alanında devam etmek isteyebileceğiniz JAR'lar verilmiştir.

1. İş yapılandırmasının **Yapı** bölümünde **Yapı adımını ekle'yi** seçin ve Azure **Blob depolama alanından İndir'i**seçin.
2. **Depolama hesabı adı için**kullanılacak depolama hesabını seçin.
3. **Kapsayıcı adı için,** indirmek istediğiniz lekelere sahip kapsayıcının adını belirtin. Ortam değişkenlerini kullanabilirsiniz.
4. **Blob adı için,** blob adını belirtin. Ortam değişkenlerini kullanabilirsiniz. Ayrıca, blob adının ilk harfini(ler) belirttikten sonra joker karakter olarak yıldız işareti kullanabilirsiniz. Örneğin, **proje\\*** adları **project**ile başlayan tüm lekeleri belirtir.
5. [İsteğe bağlı] **İndirme yolu**için, Azure blob depolamadan dosya indirmek istediğiniz Jenkins makinesindeki yolu belirtin. Ortam değişkenleri de kullanılabilir. **(İndirme yolu**için bir değer sağlamazsanız, Azure blob depolama sundaki dosyalar işin çalışma alanına indirilir.)

Azure blob depolamadan indirmek istediğiniz ek öğeler varsa, ek oluşturma adımları oluşturabilirsiniz.

Bir yapıyı çalıştırdıktan sonra, beklediğiniz lekelerin başarıyla indirilip indirilmediğini görmek için yapı geçmişi konsolçıktısını denetleyebilir veya indirme konumunuza bakabilirsiniz.  

## <a name="components-used-by-the-blob-service"></a>Blob hizmeti tarafından kullanılan bileşenler
Bu bölümde Blob hizmet bileşenlerine genel bir bakış sağ.) yer almaktadır.

* **Depolama Hesabı**: Tüm Azure Depolama erişimi bir depolama hesabı üzerinden yapılır. Depolama hesabı, lekelere erişmek için ad alanının en üst düzey düzeyidir. Bir hesap, toplam boyutu 100 TB'nin altında olduğu sürece sınırsız sayıda kapsayıcı içerebilir.
* **Konteyner**: Konteyner, bir dizi blob'un gruplandırması sağlar. Tüm bloblar bir kapsayıcıda olmalıdır. Bir hesapta sınırsız sayıda kapsayıcı olabilir. Kapsayıcıda sınırsız sayıda blob depolanabilir.
* **Blob**: Her tür ve boyutta bir dosya. Azure Depolama'da depolanabilecek iki tür blob vardır: blok ve sayfa lekeleri. Çoğu dosya blok lekeleri vardır. Tek bir blok blob boyutu 200 GB'a kadar olabilir. Bu öğretici blok lekeleri kullanır. Başka bir blob türü olan sayfa lekeleri, boyutu 1 TB'a kadar olabilir ve bir dosyadaki bayt aralıkları sık sık değiştirildiğinde daha verimlidir. Lekeler hakkında daha fazla bilgi için [Bkz.](https://msdn.microsoft.com/library/azure/ee691964.aspx)
* **URL biçimi**: Blobs aşağıdaki URL biçimi kullanılarak adreslenebilir:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (Yukarıdaki biçim genel Azure bulutu için geçerlidir. Farklı bir Azure bulutu kullanıyorsanız, URL bitiş noktanızı belirlemek için [Azure portalındaki](https://portal.azure.com) bitiş noktasını kullanın.)
  
    Yukarıdaki biçimde, `storageaccount` depolama hesabınızın adını `container_name` temsil eder, kapsayıcınızın `blob_name` adını temsil eder ve sırasıyla blob'unuzun adını temsil eder. Kapsayıcı adı içinde, bir ileri eğik çizgi ile **/** ayrılmış birden çok yol olabilir. Bu öğretici için kullanılan örnek kapsayıcı adı **MyJob**oldu ve **${BUILD\_\_ID}/${BUILD NUMBER}** ortak sanal yol için kullanıldı ve blob aşağıdaki formun bir URL'si ile sonuçlandı:
  
    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="troubleshooting-the-jenkins-plugin"></a>Jenkins eklentisiyle ilgili sorunları giderme

Jenkins eklentileriyle ilgili hatalarla karşılaşırsanız [Jenkins JIRA](https://issues.jenkins-ci.org/) sayfasında söz konusu bileşenle ilgili sorun bildirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure'da Jenkins](/azure/Jenkins/)