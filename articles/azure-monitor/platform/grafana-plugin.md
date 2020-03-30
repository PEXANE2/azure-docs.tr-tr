---
title: Azure hizmetlerini ve uygulamalarını Grafana'yı kullanarak izleme
description: Azure Monitor ve Application Insights verilerini Grafana'da görüntüleyebilmeniz için yönlendirin.
ms.subservice: ''
ms.topic: conceptual
ms.date: 11/06/2017
ms.openlocfilehash: 142e3e19c13710963d239a75bc237b63713c29cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77672217"
---
# <a name="monitor-your-azure-services-in-grafana"></a>Azure hizmetlerinizi Grafana'da izleyin
Artık [Azure Monitörü veri kaynağı eklentisini](https://grafana.com/plugins/grafana-azure-monitor-datasource)kullanarak Azure hizmetlerini ve uygulamalarını [Grafana'dan](https://grafana.com/) izleyebilirsiniz. Eklenti, çeşitli günlükler ve ölçümler de dahil olmak üzere Azure Monitor tarafından toplanan uygulama performansı verilerini toplar. Daha sonra bu verileri Grafana panonuzda görüntüleyebilirsiniz.

Bir Grafana sunucusu kurmak ve Azure Monitor'dan ölçümler ve günlükler için panolar oluşturmak için aşağıdaki adımları kullanın.

## <a name="set-up-a-grafana-server"></a>Grafana sunucusu ayarlama

### <a name="set-up-grafana-locally"></a>Grafana'yı yerel olarak ayarlama
Yerel bir Grafana sunucusu kurmak için [Grafana'yı yerel ortamınızda indirin ve kurun.](https://grafana.com/grafana/download) Eklentinin Azure Monitör tümleştirmesini kullanmak için Grafana sürüm 5.3 veya üzeri sürümünü yükleyin.

### <a name="set-up-grafana-on-azure-through-the-azure-marketplace"></a>Azure Marketi üzerinden Azure'da Grafana'yı ayarlama
1. Azure Marketi'ne gidin ve Grafana Labs tarafından Grafana'yı seçin.

2. İsimleri ve ayrıntıları doldurun. Yeni bir kaynak grubu oluşturun. VM kullanıcı adı, VM parolası ve Grafana sunucu yöneticisi parolası için seçtiğiniz değerleri takip edin.  

3. VM boyutunu ve depolama hesabını seçin.

4. Ağ yapılandırma ayarlarını yapılandırın.

5. Özeti görüntüleyin ve kullanım koşullarını kabul ettikten sonra **Oluştur'u** seçin.

6. Dağıtım tamamlandıktan sonra **Kaynak Grubuna Git'i**seçin. Yeni oluşturulan kaynakların listesini görürsünüz.

    ![Grafana kaynak grubu nesneleri](media/grafana-plugin/grafana1.png)

    Ağ güvenlik grubunu (bu durumda*grafana-nsg)* seçerseniz, 3000 bağlantı noktasının Grafana sunucusuna erişmek için kullanıldığını görebilirsiniz.

7. Grafana sunucunuzun genel IP adresini alın - kaynaklar listesine geri dön ve **Genel IP adresini**seçin.

## <a name="sign-in-to-grafana"></a>Grafana'da oturum açın

1. Sunucunuzun IP adresini kullanarak, http:// *\<IP adresinden\>* Giriş sayfasını açın :3000 veya tarayıcınızda * \<DNSName>\:3000'i* açın. 3000 varsayılan bağlantı noktası olsa da, kurulum sırasında farklı bir bağlantı noktası seçmiş olabileceğinizi unutmayın. Oluşturduğunuz Grafana sunucusu için bir giriş sayfası görmeniz gerekir.

    ![Grafana giriş ekranı](./media/grafana-plugin/grafana-login-screen.png)

2. Daha önce oluşturduğunuz kullanıcı adı *yöneticisi* ve Grafana sunucu yöneticisi parolası ile oturum açın. Yerel bir kurulum kullanıyorsanız, varsayılan parola *yönetici*olur ve ilk girişinizde değiştirmeniz istenir.

## <a name="configure-data-source-plugin"></a>Veri kaynağı eklentisini yapılandırma

Başarılı bir şekilde oturum açtıktan sonra Azure Monitor veri kaynağı eklentisinin zaten dahil olduğunu görmeniz gerekir.

![Grafana Azure Monitör eklentisi içerir](./media/grafana-plugin/grafana-includes-azure-monitor-plugin-dark.png)

1. Azure Monitor veri kaynağını eklemek ve yapılandırmak için **veri kaynağı** ekle'yi seçin.

2. Veri kaynağı için bir ad seçin ve açılan dosyadan tür olarak **Azure Monitor'u** seçin.

3. Bir hizmet ilkesi oluşturun - Grafana, Azure Monitor API'lerine bağlanmak ve veri toplamak için bir Azure Active Directory hizmet ilkesi kullanır. Azure kaynaklarına erişimi yönetmek için varolan bir hizmet ilkesi oluşturmanız veya kullanmanız gerekir.
    * Bir hizmet ilkesi oluşturmak için [bu yönergeleri](../../azure-resource-manager/resource-group-create-service-principal-portal.md) izleyin. Kiracı kimliğinizi (Dizin Kimliğiniz), istemci kimliğinizi (Uygulama Kimliği) ve istemci sırrınızı (Uygulama anahtar değeri) kopyalayın ve kaydedin.
    * Bkz. Okuyucu rolünü izlemek istediğiniz abonelik, kaynak grubu veya kaynakta Azure Etkin Dizin uygulamasına Atamak için [uygulama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) atayın. 
    Log Analytics API, Reader rolünün izinlerini içeren ve buna ekleyen [Log Analytics Reader rolünü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#log-analytics-reader)gerektirir.

4. Kullanmak istediğiniz API'lere bağlantı ayrıntılarını sağlayın. Hepsine veya bazılarına bağlanabilirsiniz. 
    * Azure Monitor'da hem ölçümlere hem de günlüklere bağlanırsanız, Azure **Monitor API**ile aynı ayrıntıları seçerek aynı kimlik bilgilerini yeniden kullanabilirsiniz.
    * Eklentiyi yapılandırırken, eklentinin hangi Azure Bulutu'nu izlemesini istediğinizi belirtebilirsiniz (Genel, Azure ABD Hükümeti, Azure Almanya veya Azure Çin).
    * Application Insights kullanıyorsanız, Uygulama Öngörüleri tabanlı ölçümleri toplamak için Uygulama Öngörüleri API'nizi ve uygulama kimliğinizi de ekleyebilirsiniz. Daha fazla bilgi için [API anahtarınızı ve Uygulama Kimliğinizi alma'ya](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)bakın.

        > [!NOTE]
        > Bazı veri kaynağı alanları, ilişkili Azure ayarlarından farklı olarak adlandırılır:
        > * Kiracı Kimliği Azure Dizin Kimliğidir
        > * İstemci Kimliği, Azure Etkin Dizin Uygulama Kimliğidir
        > * İstemci Secret, Azure Etkin Dizin Uygulaması anahtar değeridir

5. Application Insights kullanıyorsanız, Uygulama Öngörüleri tabanlı ölçümleri toplamak için Uygulama Öngörüleri API'nizi ve uygulama kimliğinizi de ekleyebilirsiniz. Daha fazla bilgi için [API anahtarınızı ve Uygulama Kimliğinizi alma'ya](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)bakın.

6. **Kaydet'i**seçin ve Grafana her API için kimlik bilgilerini sınayacak. Aşağıdakine benzer bir ileti görmeniz gerekir.  
    ![Grafana veri kaynağı config onaylı](./media/grafana-plugin/grafana-data-source-config-approved-dark.png)

## <a name="build-a-grafana-dashboard"></a>Grafana panosu oluşturma

1. Grafana Giriş sayfasına gidin ve **Yeni Pano'yu**seçin.

2. Yeni panoda **Grafik'i**seçin. Diğer grafik seçeneklerini deneyebilirsiniz, ancak bu makalede Grafik örnek olarak *kullanAbilirsiniz.*

3. Panonuzda boş bir grafik belirir. Bu grafik grafiğinde çizmek istediğiniz verilerin ayrıntılarını girmek için panel başlığına tıklayın ve **Edit'i** seçin.
    ![Grafana yeni grafik](./media/grafana-plugin/grafana-new-graph-dark.png)

4. Yapılandırmayaptığınız Azure Monitör veri kaynağını seçin.
   * Azure Monitör ölçümleri toplama - hizmet açılır açılır ayında **Azure Monitor'u** seçin. Bu grafikte izlenecek kaynakları ve ölçütü seçebileceğiniz seçicilerin listesi gösterilmektedir. Bir VM'den ölçümler toplamak için **Microsoft.Compute/VirtualMachines**adını kullanın. VM'leri ve ölçümleri seçtikten sonra, verilerini panoda görüntülemeye başlayabilirsiniz.
     ![Azure Monitör için Grafana grafik config](./media/grafana-plugin/grafana-graph-config-for-azure-monitor-dark.png)
   * Azure Monitor günlük verilerini toplama - hizmet açılır düşüşünde **Azure Log Analytics'i** seçin. Sorgulamak istediğiniz çalışma alanını seçin ve sorgu metnini ayarlayın. Burada zaten sahip olduğunuz herhangi bir günlük sorgusukopyalayabilir veya yeni bir sorgu oluşturabilirsiniz. Sorgunuzu yazdıkça, IntelliSense açılır ve otomatik tamamlama seçenekleri önerir. Visualization türünü, **Zaman serisi** **Tablosunu**seçin ve sorguyu çalıştırın.
    
     > [!NOTE]
     >
     > Eklentiile sağlanan varsayılan sorgu iki makro kullanır: "$__timeFilter() ve $__interval. 
     > Bu makrolar, grafana'nın grafiğin bir kısmını yakınlaştırdığınızda zaman aralığını ve zaman dilimini dinamik olarak hesaplamasına olanak sağlar. Bu makroları kaldırabilir ve *TimeGenerated > önce (1h)* gibi standart bir zaman filtresi kullanabilirsiniz, ancak bu, grafiğin yakınlaştırma özelliğini desteklemeyeceğini anlamına gelir.
    
     ![Azure Günlük Analizi için Grafana grafik config](./media/grafana-plugin/grafana-graph-config-for-azure-log-analytics-dark.png)

5. Aşağıdaki iki grafik ile basit bir pano. Soldaki iki VM'nin CPU yüzdesini gösterir. Sağdaki grafikte, Bir Azure Depolama hesabındaki hareketler Hareket API türüne göre ayrılmıştır.
    ![Grafana İki Grafik Örneği](media/grafana-plugin/grafana6.png)


## <a name="optional-monitor-your-custom-metrics-in-the-same-grafana-server"></a>İsteğe bağlı: Özel ölçümlerinizi aynı Grafana sunucusunda izleyin

Ayrıca, hem özel hem de aracı tabanlı ölçümleri aynı Grafana örneğini toplamak ve çizmek için Telegraf ve InfluxDB'yi yükleyebilirsiniz. Bu ölçümleri bir panoda bir araya getirmek için kullanabileceğiniz birçok veri kaynağı eklentisi vardır.

Bu kurulumu Prometheus sunucunuzdaki ölçümleri içerecek şekilde yeniden kullanabilirsiniz. Grafana'nın eklenti galerisindeki Prometheus veri kaynağı eklentisini kullanın.

Burada Telegraf, InfludB, Prometheus ve Docker nasıl kullanılacağı hakkında iyi referans makaleler
 - [Ubuntu 16.04'te TICK Yığını ile Sistem Ölçümleri Nasıl İzlenir?](https://www.digitalocean.com/community/tutorials/how-to-monitor-system-metrics-with-the-tick-stack-on-ubuntu-16-04)

 - [Docker ana bilgisayarları, konteynerler ve konteynerhizmetleri için bir izleme çözümü](https://stefanprodan.com/2016/a-monitoring-solution-for-docker-hosts-containers-and-containerized-services/)

Azure Monitörü ve Uygulama Öngörüleri ölçümlerine sahip tam bir Grafana panosunun görüntüsü aşağıda veda edilmiştir.
![Grafana Örnek Ölçümleri](media/grafana-plugin/grafana8.png)

## <a name="advanced-grafana-features"></a>Gelişmiş Grafana özellikleri

### <a name="variables"></a>Değişkenler
Bazı sorgu değerleri UI açılır geçitleri aracılığıyla seçilebilir ve sorguda güncelleştirilebilir. Aşağıdaki sorguyu örnek olarak düşünün:
```
Usage 
| where $__timeFilter(TimeGenerated) 
| summarize total_KBytes=sum(Quantity)*1024 by bin(TimeGenerated, $__interval) 
| sort by TimeGenerated
```

Kullanılabilir tüm **Çözüm** değerlerini listeleyecek bir değişkeni yapılandırabilir ve ardından sorgunuzu kullanmak üzere güncelleştirebilirsiniz.
Yeni bir değişken oluşturmak için sağ üst alandaki pano ayarları düğmesini tıklatın, **Değişkenler'i**seçin ve ardından **Yeni**.
Değişken sayfasında, değerler listesini almak için çalışacak veri kaynağını ve sorguyu tanımlayın.
![Grafana değişkeni yapılandırmak](./media/grafana-plugin/grafana-configure-variable-dark.png)

Oluşturulduktan sonra, seçili değer(ler)i kullanacak şekilde sorguyu ayarlayın ve grafikleriniz buna göre yanıt verir:
```
Usage 
| where $__timeFilter(TimeGenerated) and Solution in ($Solutions)
| summarize total_KBytes=sum(Quantity)*1024 by bin(TimeGenerated, $__interval) 
| sort by TimeGenerated
```
    
![Grafana kullanım değişkenleri](./media/grafana-plugin/grafana-use-variables-dark.png)

### <a name="create-dashboard-playlists"></a>Pano çalma listeleri oluşturma

Grafana'nın birçok yararlı özelliğinden biri de pano çalma listesidir. Birden çok pano oluşturabilir ve bunları her pano için gösteriş aralığını yapılandıran bir çalma listesine ekleyebilirsiniz. Panoların geçişini görmek için **Oynat'ı** seçin. Grubunuz için bir durum panosu sağlamak için bunları büyük bir duvar monitöründe görüntülemek isteyebilirsiniz.

![Grafana Çalma Listesi Örneği](./media/grafana-plugin/grafana7.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure'da bir Grafana ortamı nı yüklediyseniz, kullanıp kullanmadığınıza bakılmaksızın SANAL'lar çalışırken ücretlendirilirsiniz. Ek ücret ödemesini önlemek için, bu makalede oluşturulan kaynak grubunu temizleyin.

1. Azure portalındaki sol menüden Kaynak **gruplarını** tıklatın ve ardından **Grafana'yı**tıklatın.
2. Kaynak grubu sayfanızda **Sil'i,** metin **kutusuna Grafana** yazın'ı ve ardından **Sil'i**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Monitör Ölçümlerine Genel Bakış](data-platform.md)

