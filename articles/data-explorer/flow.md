---
title: Microsoft Azure Veri Gezgini Akış bağlayıcısı (Önizleme)
description: Otomatik olarak zamanlanmış veya tetiklenen görevlerin akışlarını oluşturmak için Microsoft Flow bağlayıcısını kullanma hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: dorcohen
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/15/2020
ms.openlocfilehash: 7745888dcaa1324d4a9d956e93d0504c8da8c026
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79501772"
---
# <a name="microsoft-flow-connector-preview"></a>Microsoft Flow bağlayıcısı (Önizleme)

Azure Veri Gezgini akış bağlayıcısı, Azure Veri Gezgini'nin, Planlanan veya tetiklenen bir görevin parçası olarak Kusto sorgularını ve komutlarını otomatik olarak çalıştırmak için [Microsoft Power Automate'in Akış özelliklerini](https://flow.microsoft.com/) kullanmasına olanak tanır.

Yaygın kullanım senaryoları şunlardır:

* Tablolar ve grafikler içeren günlük raporlar gönderme
* Bildirimleri sorgu sonuçlarına göre ayarlama
* Kümeler üzerindeki denetim komutlarını zamanlama
* Azure Veri Gezgini ve diğer veritabanları arasında veri verme ve alma 

Daha fazla bilgi için [Microsoft Flow bağlayıcısı kullanım örneklerine](flow-usage.md)bakın.

##  <a name="log-in"></a>Oturum açma 

1. [Microsoft Power Automate'de](https://flow.microsoft.com/)oturum açın.

1. İlk kez bağlandığınızda oturum açmanız istenir.

1. **Oturum aç**’ı seçip kimlik bilgilerinizi girin.

![Oturum açma iletişim kutusu](./media/flow/flow-signin.png)

## <a name="authentication"></a>Kimlik doğrulaması

Kullanıcı kimlik bilgileri veya AAD uygulamasıyla kimlik doğrulaması yapabilirsiniz.

> [!Note]
> Uygulamanızın bir [AAD uygulaması](https://docs.microsoft.com/azure/kusto/management/access-control/how-to-provision-aad-app) olduğundan ve kümenizdeki sorguları yürütmeyetkisine olduğundan emin olun.

1. Microsoft Flow konektörünün sağ üst kısmındaki üç ![noktayı seçin: Bağlantı ekleme](./media/flow/flow-addconnection.png)

1. **Yeni bağlantı ekle'yi** seçin ve ardından **Servis Sorumlusuyla Bağlan'ı**seçin.
![Oturum açma iletişim kutusu](./media/flow/flow-signin.png)

1. Gerekli bilgileri girin:
    * Bağlantı Adı: Yeni bağlantı için açıklayıcı ve anlamlı bir ad
    * İstemci Kimliği: Başvuru Kimliğiniz
    * İstemci Sırrı: Uygulama anahtarınız
    * Kiracı: Uygulamayı oluşturduğunuz AAD dizininin kimliği. Örneğin, Microsoft kiracı kimliği: 72f988bf-86f1-41af-91ab-2d7cd011db47

![Uygulama kimlik doğrulaması](./media/flow/flow-appauth.png)

Kimlik doğrulama tamamlandığında, akışınızın yeni eklenen bağlantıyı kullandığını görürsünüz.

![Tamamlanmış uygulama kimlik doğrulaması](./media/flow/flow-appauthcomplete.png)

Şu andan itibaren, bu akış bu uygulama kimlik bilgilerini kullanarak çalışacaktır.

## <a name="find-the-azure-kusto-connector"></a>Azure Kusto konektörünü bulun

Microsoft Flow bağlayıcısını kullanmak için önce bir tetikleyici eklemeniz gerekir. Tetikleyici, yinelenen bir zaman dönemine göre veya önceki bir akış eylemine yanıt olarak tanımlanabilir.

1. [Yeni bir akış oluşturun](https://flow.microsoft.com/manage/flows/new) veya Ana sayfadan **Akışlarım** eylemini seçin ve ardından **+ Yeni'yi**seçin.

    ![Yeni akış oluşturma](./media/flow/flow-newflow.png)

1. Zamanlanmış-kaynaktan boş ekleyin.

    ![Yeni zamanlanmış akış](./media/flow/flow-scheduled-from-blank.png)

1. Zamanlanmış akış sayfası oluştur'a gerekli bilgileri girin.
    ![Zamanlanmış bir akış oluşturma](./media/flow/flow-build-scheduled-flow.png)
1. **Oluştur'u**seçin.
1. **+ Yeni adım**’ı seçin.
1. Arama kutusuna "Kusto" girin.

    ![Akış eylemlerini seçme](./media/flow/flow-actions.png)

1. **Azure Veri Gezgini'ni**seçin.

## <a name="flow-actions"></a>Akış Eylemleri

Azure Veri Gezgini bağlayıcısını açtığınızda, akışınıza ekleyebileceğiniz üç olası eylem vardır.

Bu bölümde, her Microsoft Akış eylemi için özellikler ve parametreler açıklanmaktadır.

![Akış Azure Veri Gezgini eylemleri](./media/flow/flow-adx-actions.png)

### <a name="run-control-command-and-visualize-results"></a>Denetim komutunu çalıştırın ve sonuçları görselleştirin

Denetim komutunu çalıştırmak için Çalıştır denetim [control command](https://docs.microsoft.com/azure/kusto/management/index)komutunu kullanın ve sonuç eylemini görselleştirin.

1. Küme URL'sini belirtin. Örneğin, https://clusterName.eastus.kusto.windows.net
1. Veritabanının adını girin.
1. Denetim komutunu belirtin:
    * Akışta kullanılan uygulamalardan ve konektörlerden dinamik içerik seçin
    * Değerlere erişmek, dönüştürmek ve karşılaştırmak için ifade ekleme
1. Bu eylemin sonuçlarını tablo veya grafik olarak e-posta ile göndermek için, aşağıdakileri olabilecek grafik türünü belirtin:
    * HTML tablosu
    * Pasta grafiği
    * Bir zaman çizelgesi
    * Çubuk grafik

![Akış denetim komutunu çalıştır](./media/flow/flow-runcontrolcommand.png)

> [!IMPORTANT]
> Küme *Adı* alanına küme URL'sini girin.

### <a name="run-query-and-list-results"></a>Sorgu ve liste sonuçlarını çalıştırma

> [!Note]
> Sorgunuz bir noktayla başlarsa (yani [bir denetim komutu),](https://docs.microsoft.com/azure/kusto/management/index)Çalıştır denetim komutunu kullanın [ve sonuçları görselleştirin](#run-control-command-and-visualize-results)

Bu eylem, Kusto kümesine bir sorgu gönderir. Daha sonra eklenen eylemler, sorgu sonuçlarının her satırı üzerinde yinelenir.

Aşağıdaki örnek her dakika bir sorgutetikler ve sorgu sonuçlarına göre bir e-posta gönderir. Sorgu veritabanındaki satır sayısını denetler ve ardından yalnızca satır sayısı 0'dan büyükse e-posta gönderir. 

![Sorgu listesi sonuçlarını çalıştırma](./media/flow/flow-runquerylistresults-2.png)

> [!Note]
> Sütunda birden çok satır varsa, bağlayıcı sütundaki her satır için çalışır.

### <a name="run-query-and-visualize-results"></a>Sorguçalıştırma ve sonuçları görselleştirme
        
> [!Note]
> Sorgunuz bir noktayla başlarsa (yani [bir denetim komutu),](https://docs.microsoft.com/azure/kusto/management/index)Çalıştır denetim komutunu kullanın [ve sonuçları görselleştirin](#run-control-command-and-visualize-results)
        
Kusto sorgu sonucunu tablo veya grafik olarak görselleştirmek için Çalıştır sorgusunu kullanın ve sonuç eylemini görselleştirin. Örneğin, günlük ICM raporlarını e-posta ile almak için bu akışı kullanın. 
    
Bu örnekte, sorgusonuçları HTML tablosu olarak döndürülür.
            
![Sorguçalıştırma ve sonuçları görselleştirme](./media/flow/flow-runquery.png)

> [!IMPORTANT]
> Küme *Adı* alanına küme URL'sini girin.

## <a name="email-kusto-query-results"></a>Kusto sorgu sonuçlarını e-posta yla gönder

Herhangi bir e-posta adresine e-posta yoluyla rapor göndermek için herhangi bir akışa bir adım ekleyebilirsiniz. 

1. Akışınıza yeni bir adım eklemek için **+ Yeni Adım'ı** seçin.
1. Arama alanında Office 365'i girin ve **Office 365 Outlook'u**seçin.
1. **E-posta gönder**'i seçin.
1. E-posta raporunun gönderilmesini istediğiniz yere e-posta adresini girin.
1. E-postanın konusunu girin.
1. *Gövde* alanında, Dinamik içerik alanından **Gövde'yi**seçin.
1. **Gelişmiş seçenekleri göster**’i seçin.
1. Ekler *Adı -1* **alanında, Ek Adı'nı**seçin.
1. Ekler *İçeriği* alanında, **Ek İçeriği'ni**seçin.
1. Gerekirse, önem düzeyini ayarlayın.
1. **Kaydet'i**seçin.

![E-posta gönderin](./media/flow/flow-sendemail.png)

## <a name="check-if-your-flow-succeeded"></a>Akışınızın başarılı olup olmadığını kontrol edin

Akışınızın başarılı olup olmadığını kontrol etmek için, akışın çalışma geçmişine bakın:
1. [Microsoft Flow ana sayfasına](https://flow.microsoft.com/)gidin.
1. Ana menüden [Akışlarım'ı](https://flow.microsoft.com/manage/flows)seçin.
    ![Akışlarım sayfası](./media/flow/flow-myflows.png)
1. Araştırmak istediğiniz akış satırında, daha fazla komut simgesini seçin ve ardından **geçmişi çalıştırın.**

    ![Geçmiş menüsünü çalıştır](./media/flow//flow-runhistory.png)

    Tüm akış çalıştırmaları başlangıç zamanı, süresi ve durumuyla birlikte listelenir.
    ![Geçmiş sonuçları sayfasını çalıştırma](./media/flow/flow-runhistoryresults.png)

    Akışlarım sayfasında, [akışla](https://flow.microsoft.com/manage/flows) ilgili tüm ayrıntılar için, araştırmak istediğiniz akışı seçin.

    ![Geçmiş tam sonuçlar sayfasını çalıştırın](./media/flow/flow-fulldetails.png) 

Bir çalıştırmanın neden başarısız olduğunu görmek için çalıştırma başlangıç saatini seçin. Akış görünür ve başarısız olan akış adımı kırmızı bir ünlem işaretiyle gösterilir. Ayrıntıları görüntülemek için başarısız adımı genişletin. Sağ bölme, sorun gidermeniz için hata yla ilgili bilgiler içerir.

![Akış hatası](./media/flow/flow-error.png)

## <a name="timeout-exceptions"></a>Zaman arası özel durumları

Akışınız başarısız olabilir ve yedi dakikadan fazla çalışırsa bir "RequestTimeout" özel durumu döndürebilir.

[Microsoft Akış sınırlamaları](#limitations)hakkında daha fazla bilgi edinin.
    
Aynı sorgu, sürenin sınırlı olmadığı ve değiştirilebileceği Azure Veri Gezgini'nde başarılı bir şekilde çalışabilir.
            
"RequestTimeout" özel durumu aşağıdaki resimde gösterilmiştir:
    
![Akış isteği zaman dışarı özel durum hatası](./media/flow/flow-requesttimeout.png)
    
Bir zaman aşımı sorununu gidermek için, sorgunuzun daha hızlı çalışması için daha verimli olmasını sağlamaya çalışın veya parçalara ayırın. Her öbek sorgunun farklı bir bölümünde çalıştırılabilir.

Daha fazla bilgi için Sorgu [en iyi uygulamaları](https://docs.microsoft.com/azure/kusto/query/best-practices)hakkında bilgi edinin.

## <a name="limitations"></a>Sınırlamalar

* İstemciledöndürülen sonuçlar 500.000 kayıtla sınırlıdır. Bu kayıtların genel belleği 64 MB'ı ve yedi dakikalık yürütme süresini geçemez.
* Konektör [çatal](https://docs.microsoft.com/azure/kusto/query/forkoperator) ve [fason işleçlerini](https://docs.microsoft.com/azure/kusto/query/facetoperator) desteklemez.
* Akış en iyi Microsoft Edge ve Chrome'da çalışır.

## <a name="next-steps"></a>Sonraki adımlar

Planlanan veya tetiklenen bir görevin parçası olarak Kusto sorgularını ve komutlarını otomatik olarak çalıştırmanın başka bir yolu olan [Microsoft Azure Explorer Mantık Uygulaması bağlayıcısı](https://docs.microsoft.com/azure/kusto/tools/logicapps) hakkında bilgi edinin.
