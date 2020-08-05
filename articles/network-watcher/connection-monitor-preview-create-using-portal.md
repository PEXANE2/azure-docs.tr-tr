---
title: Bağlantı Izleyicisi önizlemesi oluşturma-Azure portal
titleSuffix: Azure Network Watcher
description: Azure portal kullanarak bağlantı Izleyicisi 'ni (Önizleme) oluşturmayı öğrenin.
services: network-watcher
documentationcenter: na
author: vinigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/30/2020
ms.author: vinigam
ms.openlocfilehash: 4e7067e537ce8fb6faf82198f7863957f79ffb22
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87567995"
---
# <a name="create-a-connection-monitor-preview-using-the-azure-portal"></a>Azure portal kullanarak bağlantı Izleyicisi (Önizleme) oluşturma

Azure portal kullanarak kaynaklarınız arasındaki iletişimi izlemek için bağlantı Izleyicisi 'ni (Önizleme) oluşturmayı öğrenin. Karma ve Azure bulut dağıtımlarını destekler.

## <a name="before-you-begin"></a>Başlamadan önce 

Bağlantı Izleyici (Önizleme) bölümünde oluşturduğunuz bağlantı izleyicilerinde, hem şirket içi makineleri hem de Azure VM 'lerini kaynak olarak ekleyebilirsiniz. Bu bağlantı izleyicileri, uç noktalara bağlantıyı da izleyebilir. Uç noktalar Azure veya başka bir URL veya IP üzerinde olabilir.

Bağlantı Izleyicisi (Önizleme) aşağıdaki varlıkları içerir:


* **Bağlantı İzleyicisi kaynağı** : bölgeye özgü bir Azure kaynağı. Aşağıdaki varlıkların hepsi bir bağlantı İzleyicisi kaynağının özellikleridir.
* **Uç nokta** : bağlantı denetimlerine katılan kaynak veya hedef. Uç noktalara örnek olarak Azure VM 'Leri, şirket içi aracılar, URL 'Ler ve IP 'Ler verilebilir.
* **Test yapılandırması** – bir test için protokole özgü bir yapılandırma. Seçtiğiniz protokole bağlı olarak, bağlantı noktası, eşikler, sınama sıklığı ve diğer parametreleri tanımlayabilirsiniz.
* **Test grubu** : kaynak uç noktaları, hedef uç noktaları ve test yapılandırmalarının bulunduğu grup. Bir bağlantı İzleyicisi birden fazla test grubu içerebilir.
* **Test** : kaynak uç noktası, hedef uç noktası ve test yapılandırmasının birleşimi. Test, izleme verilerinin kullanılabildiği en ayrıntılı düzeydir. İzleme verileri, başarısız olan denetim yüzdesini ve gidiş dönüş süresini (RTT) içerir.

    ![Test grupları ve testler arasındaki ilişkiyi tanımlayan bir bağlantı izleyicisini gösteren diyagram](./media/connection-monitor-2-preview/cm-tg-2.png)

## <a name="steps-to-create"></a>Oluşturma adımları

Azure portal kullanarak bir bağlantı Izleyicisi (Önizleme) oluşturmak için aşağıdaki adımları izleyin:

1. Azure portal giriş sayfasında **Ağ İzleyicisi**' ne gidin.
1. Sol tarafta, **izleme** bölümünde **Bağlantı İzleyicisi (Önizleme)** öğesini seçin.
1. Bağlantı Izleyicisinde (Önizleme) oluşturulan tüm bağlantı izleyicilerini görürsünüz. Klasik bağlantı Izleyici deneyiminde oluşturulan bağlantı izleyicilerini görmek için **Bağlantı İzleyicisi** sekmesine gidin.

    ![Bağlantı Izleyicisinde oluşturulan bağlantı izleyicilerini gösteren ekran görüntüsü (Önizleme)](./media/connection-monitor-2-preview/cm-resource-view.png)   
    
1. **Bağlantı İzleyicisi (Önizleme)** panosunda, sol üst köşedeki **Oluştur**' u seçin.
1. **Temel bilgiler** sekmesinde, bağlantı monitörünüz için bilgi girin:
   * **Bağlantı Izleyicisi adı** – bağlantı monitörünüzün adını ekleyin. Azure kaynakları için standart adlandırma kurallarını kullanın.
   * **Abonelik** : bağlantı monitörünüz için bir abonelik seçin.
   * **Region** : bağlantı monitörünüz için bir bölge seçin. Yalnızca bu bölgede oluşturulan kaynak VM 'Leri seçebilirsiniz.
   * **Çalışma alanı yapılandırması** -çalışma alanınız izleme verilerinizi tutar. Özel bir çalışma alanı veya varsayılan çalışma alanı kullanabilirsiniz. 
       * Varsayılan çalışma alanını kullanmak için onay kutusunu işaretleyin. 
       * Özel bir çalışma alanı seçmek için onay kutusunu temizleyin. Ardından özel çalışma alanınız için abonelik ve bölge seçin. 
1. Sekmenin alt kısmındaki **İleri: test grupları**' nı seçin.

   ![Bağlantı Izleyicisinde temel bilgiler sekmesini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/create-cm-basics.png)

1. **Test grupları** sekmesinde **+ test grubu**' nu seçin. Test gruplarınızı ayarlamak için, bkz. [bağlantı Izleyicisinde test grupları oluşturma](#create-test-groups-in-a-connection-monitor). 
1. Sekmenin en altında, Ileri ' yi seçin ve bağlantı monitörünüzü gözden geçirmek için **Oluştur** ' a tıklayın.

   ![Test grupları sekmesini ve test grubu ayrıntılarını eklediğiniz bölmeyi gösteren ekran görüntüsü](./media/connection-monitor-2-preview/create-tg.png)

1. **Gözden geçir + oluştur** sekmesinde, bağlantı izleyicisini oluşturmadan önce temel bilgileri ve test gruplarını gözden geçirin. Bağlantı izleyicisini düzenlemeniz gerekiyorsa:
   * Temel ayrıntıları düzenlemek için kalem simgesini seçin.
   * Bir test grubunu düzenlemek için seçin.

   > [!NOTE] 
   > **Gözden geçir + oluştur** sekmesi, Bağlantı İzleyicisi önizleme aşamasında aylık maliyeti gösterir. Şu anda **GEÇERLI maliyet** sütunu hiçbir ücret göstermez. Bağlantı Izleyicisi genel kullanıma sunulduğunda, bu sütunda aylık bir ücret gösterilir. 
   > 
   > Bağlantı Izleyicisi önizleme aşamasında bile, Log Analytics alma ücretleri uygulanır.

1. Bağlantı izleyicisini oluşturmaya hazırsanız, **gözden geçir + oluştur** sekmesinin altındaki **Oluştur**' u seçin.

   ![Inceleme + Oluştur sekmesini gösteren bağlantı Izleyicisinin ekran görüntüsü](./media/connection-monitor-2-preview/review-create-cm.png)

Bağlantı Izleyicisi (Önizleme) arka planda Bağlantı İzleyicisi kaynağını oluşturur.

## <a name="create-test-groups-in-a-connection-monitor"></a>Bağlantı izleyicisinde test grupları oluşturma

Bir bağlantı izleyicisinde bulunan her bir test grubu, ağ parametrelerini test eden kaynakları ve hedefleri içerir. Başarısız olan denetim yüzdesi ve test yapılandırmalarının RTT üzerinden test edilmiştir.

Azure portal, bir bağlantı izleyicisinde bir test grubu oluşturmak için aşağıdaki alanlar için değerler belirtirsiniz:

* **Test grubunu devre dışı bırak** – test grubunun belirttiği tüm kaynaklar ve hedefler için izlemeyi devre dışı bırakmak üzere bu alanı seçebilirsiniz. Bu seçim varsayılan olarak temizlenir.
* **Ad** – test grubunuzu adlandırın.
* **Kaynaklar** : aracılar bunlara yüklenmişse hem Azure VM 'lerini hem de şirket içi makineleri kaynak olarak belirtebilirsiniz. Kaynağınıza yönelik bir aracı yüklemek için bkz. [izleme aracılarını yüklemek](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#install-monitoring-agents).
   * Azure aracılarını seçmek için **Azure aracıları** sekmesini seçin. Burada yalnızca bağlantı izleyicisini oluştururken belirttiğiniz bölgeye bağlı olan VM 'Leri görürsünüz. Varsayılan olarak VM 'Ler, ait oldukları abonelikte gruplandırılır. Bu gruplar daraltılmıştır. 
   
       Abonelik düzeyinden hiyerarşideki diğer düzeylere ayrıntıya gidebilirsiniz:

      **Abonelik**  >  **Kaynak grupları**  >  **VNET 'ler**  >  **Alt ağlar**  >  **Aracılarla VM 'ler**

      Ayrıca, ağacı başka bir düzeyden başlatmak için **Group By** alanı değerini de değiştirebilirsiniz. Örneğin, sanal ağ ile gruplandırma yaparsanız, **VNETs**  >  aracılarla sanal**ağlar alt ağları**  >  **VM**'lerinde aracıların bulunduğu VM 'leri görürsünüz.

      ![Kaynak ekleme paneli ve Azure aracıları sekmesini gösteren bağlantı Izleyicisinin ekran görüntüsü](./media/connection-monitor-2-preview/add-azure-sources.png)

   * Şirket içi aracıları seçmek için, **Azure dışı aracılar** sekmesini seçin. Varsayılan olarak, aracılar bölgelere göre çalışma alanları halinde gruplandırılır. Bu çalışma alanlarının tümünde Ağ Performansı İzleyicisi çözümü yapılandırıldı. 
   
       Çalışma alanınıza Ağ Performansı İzleyicisi eklemeniz gerekiyorsa [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview)'nden alın. Ağ Performansı İzleyicisi ekleme hakkında daha fazla bilgi için bkz. [Azure izleyici 'de çözümleri izleme](https://docs.microsoft.com/azure/azure-monitor/insights/solutions). 
   
       **Bağlantı Izleyicisi oluştur** görünümünde, **temel bilgiler** sekmesinde varsayılan bölge seçilidir. Bölgeyi değiştirirseniz, yeni bölgedeki çalışma alanlarından aracılar seçebilirsiniz. Ayrıca **Group By** alanı değerini, alt ağlara göre gruplandırma olarak değiştirebilirsiniz.

      ![Kaynak ekleme paneli ve Azure dışı aracılar sekmesini gösteren bağlantı Izleyicisinin ekran görüntüsü](./media/connection-monitor-2-preview/add-non-azure-sources.png)


   * Seçtiğiniz Azure ve Azure olmayan aracıları gözden geçirmek için **İnceleme** sekmesine gidin.

      ![Kaynak Ekle panelini ve gözden geçirme sekmesini gösteren bağlantı Izleyicisinin ekran görüntüsü](./media/connection-monitor-2-preview/review-sources.png)

   * Kaynakları ayarlamayı tamamladığınızda, **Kaynak Ekle** panelinin alt kısmındaki **bitti**' yi seçin.

* **Hedefler** : Azure VM 'lerine veya herhangi bir uç noktaya (genel IP, URL veya FQDN) yönelik bağlantıyı, hedefler olarak belirterek izleyebilirsiniz. Tek bir test grubunda Azure VM 'leri, Office 365 URL 'Leri, Dynamics 365 URL 'Leri ve özel uç noktaları ekleyebilirsiniz.

    * Azure VM 'lerini hedef olarak seçmek için **Azure VM 'leri** sekmesini seçin. Varsayılan olarak, Azure VM 'Leri, **temel bilgiler** sekmesinde **Bağlantı İzleyicisi oluştur** görünümünde seçtiğiniz bölgede bulunan bir abonelik hiyerarşisinde gruplandırılır. Bölgeyi değiştirebilir ve yeni seçilen bölgeden Azure VM 'Leri seçebilirsiniz. Daha sonra, Azure Agents düzeyi gibi, abonelik düzeyinden hiyerarşideki diğer düzeylere gidebilirsiniz.

       ![Azure VM 'Leri sekmesini gösteren hedefler ekle bölmesinin ekran görüntüsü](./media/connection-monitor-2-preview/add-azure-dests1.png)

       ![Abonelik düzeyini gösteren hedefler ekle bölmesinin ekran görüntüsü](./media/connection-monitor-2-preview/add-azure-dests2.png)

    * Uç noktaları hedef olarak seçmek için **bitiş noktaları** sekmesini seçin. Uç noktalar listesi, Office 365 test URL 'Leri ve Dynamics 365 test URL 'Lerini ada göre gruplanmış olarak içerir. Bu uç noktalara ek olarak, aynı bağlantı izleyicisinde diğer test gruplarında oluşturulmuş bir uç nokta seçebilirsiniz. 
    
        Yeni bir uç nokta eklemek için sağ üst köşede **+ uç noktalar**' ı seçin. Ardından bir uç nokta adı ve URL, IP veya FQDN belirtin.

       ![Bağlantı Izleyicisinde uç noktaların hedef olarak nereye ekleneceğini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/add-endpoints.png)

    * Seçtiğiniz Azure sanal makinelerini ve uç noktalarını gözden geçirmek için **İnceleme** sekmesini seçin.
    * Hedefleri seçme işini bitirdiğinizde **bitti**' yi seçin.

* **Test yapılandırması** – test yapılandırmasını bir test grubunda ilişkilendirebilirsiniz. Azure portal, test grubu başına yalnızca bir test yapılandırmasına izin verir, ancak daha fazla eklemek için ARMClient kullanabilirsiniz.

    * **Ad** – test yapılandırmasını adlandırın.
    * **Protokol** : TCP, ıCMP veya http ' yi seçin. HTTP 'yi HTTPS olarak değiştirmek için protokol olarak **http** ' yi seçin ve bağlantı noktası olarak **443** ' yı seçin.
        * **Ağ test yapılandırması oluştur** – bu onay kutusu yalnızca **protokol** alanında **http** ' yi seçtiğinizde görünür. Yapılandırmanızda başka bir yerde belirttiğiniz kaynakları ve hedefleri kullanan başka bir test yapılandırması oluşturmak için bu kutuyu seçin. Yeni oluşturulan test yapılandırması olarak adlandırılmıştır `<the name of your test configuration>_networkTestConfig` .
        * İzleme hareketini **devre dışı bırak** – Bu alan, protokolü TCP veya ICMP olan test grupları için geçerlidir. Kaynakları, topolojiyi ve Atlama RTT 'yi bulmaktan durdurmak için bu kutuyu seçin.
    * **Hedef bağlantı noktası** : bu alanı, seçtiğiniz bir hedef bağlantı noktasıyla özelleştirebilirsiniz.
    * **Sınama sıklığı** – kaynakların, belirttiğiniz protokol ve bağlantı noktasındaki hedeflere ne sıklıkta ping uygulayabileceğini belirlemek için bu alanı kullanın. 30 saniye, 1 dakika, 5 dakika, 15 dakika veya 30 dakika seçebilirsiniz. Kaynaklar, seçtiğiniz değere göre hedeflere bağlantıyı test edecektir.  Örneğin, 30 saniye seçerseniz, kaynaklar en az 30 saniyelik bir dönemde hedefe bağlantıyı kontrol eder.
    * **Başarı eşiği** – aşağıdaki ağ parametrelerinde eşikler ayarlayabilirsiniz:
       * **Denetim başarısız** : kaynaklar, belirttiğiniz kriterleri kullanarak hedeflere bağlantı denetlediğinde başarısız olan denetim yüzdesini ayarlayın. TCP veya ıCMP protokolü için, başarısız denetimlerin yüzdesi, paket kaybı yüzdesine eşit hale getirilmiş olabilir. HTTP protokolü için, bu alan yanıt olmadan alınan HTTP isteklerinin yüzdesini temsil eder.
       * **Gidiş dönüş süresi** – bir test yapılandırması üzerinden hedefe bağlanmak için kaynakların ne kadar süreceğine ILIŞKIN olarak RTT 'yi milisaniye olarak ayarlayın.
    
       ![Bağlantı Izleyicisinde bir test yapılandırmasının nerede ayarlanacağını gösteren ekran görüntüsü](./media/connection-monitor-2-preview/add-test-config.png)


## <a name="scale-limits"></a>Ölçek sınırları

Bağlantı izleyicileri aşağıdaki ölçek sınırlarına sahiptir:

* Her bölge için abonelik başına en fazla bağlantı İzleyicisi: 100
* Bağlantı İzleyicisi başına en fazla test grubu: 20
* Bağlantı İzleyicisi başına en fazla kaynak ve hedef sayısı: 100
* Bağlantı İzleyicisi başına en fazla test yapılandırması: Azure portal ile 2

## <a name="next-steps"></a>Sonraki adımlar

* [İzleme verilerini çözümlemeyi ve uyarıları ayarlamayı](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#analyze-monitoring-data-and-set-alerts) öğrenin
* [Ağınızdaki sorunları tanılamayı](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#diagnose-issues-in-your-network) öğrenin
