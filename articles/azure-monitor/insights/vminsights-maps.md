---
title: VM'ler için Azure Monitor ile uygulama bağımlılıklarını görüntüleme
description: Harita, Azure Monitor'un VM'ler için bir özelliğidir. Windows ve Linux sistemlerindeki uygulama bileşenlerini otomatik olarak keşfeder ve hizmetler arasındaki iletişimi eşler. Bu makalede, çeşitli senaryolarda Harita özelliğinin nasıl kullanılacağı hakkında ayrıntılı bilgi verilmektedir.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/20/2020
ms.openlocfilehash: acb96984a49e4ad8535f87a41da11b3b63ae207b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80283863"
---
# <a name="use-the-map-feature-of-azure-monitor-for-vms-to-understand-application-components"></a>Uygulama bileşenlerini anlamak için SANAL'lar için Azure Monitörünün Harita özelliğini kullanma
Sanal Ayarlar için Azure Monitörü'nde, Azure'da veya çevrenizde çalışan Windows ve Linux sanal makinelerinde (VM) keşfedilen uygulama bileşenlerini görüntüleyebilirsiniz. VM'leri iki şekilde gözlemleyebilirsiniz. Bir haritayı doğrudan bir VM'den görüntüleyin veya Bileşenleri VM grupları arasında görmek için Azure Monitor'dan bir harita görüntüleyin. Bu makale, bu iki görüntüleme yöntemini ve Harita özelliğinin nasıl kullanılacağını anlamanıza yardımcı olacaktır. 

VM'ler için Azure Monitörünü yapılandırma hakkında bilgi için [vm'ler için Azure Monitörünü Etkinleştir'e](vminsights-enable-overview.md)bakın.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="introduction-to-the-map-experience"></a>Harita deneyimine giriş
Harita deneyimine dalmadan önce, bilgiyi nasıl sunduğunu ve görselleştirdiğini anlamalısınız. Harita özelliğini doğrudan bir VM'den veya Azure Monitör'den seçin, Harita özelliği tutarlı bir deneyim sunar. Tek fark, Azure Monitor'dan bir haritanın birden katmanlı bir uygulamanın veya kümenin tüm üyelerini göstermesidir.

Harita özelliği, aşağıdakileri yapan çalışan işlemleri keşfederek VM bağımlılıklarını görselleştirir: 

- Sunucular arasında etkin ağ bağlantıları.
- Gelen ve giden bağlantı gecikmesi.
- Belirli bir zaman aralığında TCP'ye bağlı herhangi bir mimarideki bağlantı noktaları.  
 
İşlem ayrıntılarını ve yalnızca VM ile iletişim kurabilen işlemleri göstermek için bir VM'yi genişletin. İstemci grubu VM'ye bağlanan ön uç istemcilerinin sayısını gösterir. Sunucu bağlantı noktası grupları, VM'nin bağlandığı arka uç sunucuların sayısını gösterir. Bu bağlantı noktası üzerinden bağlanan sunucuların ayrıntılı listesini görmek için bir sunucu bağlantı noktası grubunu genişletin.  

VM'yi seçtiğinizde, sağdaki **Özellikler** bölmesi VM'nin özelliklerini gösterir. Özellikler, işletim sistemi tarafından bildirilen sistem bilgilerini, Azure VM özelliklerini ve keşfedilen bağlantıları özetleyen halka grafiğiiçerir. 

![Özellikler bölmesi](./media/vminsights-maps/properties-pane-01.png)

Bölmenin sağ tarafında, VM'nin Azure Monitor'a gönderdiği verilerin listesini göstermek için **Günlük Olayları'nı** seçin. Bu veriler sorgu için kullanılabilir.  **Günlükler** sayfasını açmak için, bu kayıt türünün sonuçlarını gördüğünüz herhangi bir kayıt türünü seçin. VM'ye karşı filtre uygulanmış önceden yapılandırılmış bir sorgu da görürsünüz.  

![Günlük Olayları bölmesi](./media/vminsights-maps/properties-pane-logs-01.png)

**Günlükler** sayfasını kapatın ve **Özellikler** bölmesine geri dönün. Burada, VM sistem durumu ölçütleri uyarılarını görüntülemek için **Uyarılar'ı** seçin. Harita özelliği, seçili zaman aralığında seçilen sunucuiçin uyarıları göstermek için Azure Uyarıları ile tümleşir. Sunucu geçerli uyarılar için bir simge görüntüler ve **Makine Uyarıları** bölmesi uyarıları listeler. 

![Uyarılar bölmesi](./media/vminsights-maps/properties-pane-alerts-01.png)

Harita özelliğinin ilgili uyarıları görüntülemesini sağlamak için, belirli bir bilgisayar için geçerli olan bir uyarı kuralı oluşturun:

- Uyarıları bilgisayara göre gruplandırmak için bir yan tümce ekleyin (örneğin, **Bilgisayar aralığına göre 1 dakika).**
- Uyarıyı bir metrme dayandırın.

Azure Uyarıları ve uyarı kuralları oluşturma hakkında daha fazla bilgi için [Azure Monitor'da Birleşik uyarılar'a](../../azure-monitor/platform/alerts-overview.md)bakın.

Sağ üst köşede, **Gösterge** seçeneği haritadaki sembolleri ve rolleri açıklar. Haritanıza daha yakından bakmak ve hareket ettirmek için sağ alt köşedeki yakınlaştırma denetimlerini kullanın. Yakınlaştırma düzeyini ayarlayabilir ve haritayı sayfanın boyutuna sığdırabilirsiniz.  

## <a name="connection-metrics"></a>Bağlantı ölçümleri
**Bağlantılar** bölmesi, VM'den Seçilen bağlantıiçin TCP bağlantı noktası üzerinden standart ölçümler görüntüler. Ölçümler yanıt süresini, dakika başına istekleri, trafik verime ve bağlantıları içerir.  

![Bağlantılar bölmesindeki ağ bağlantısı grafikleri](./media/vminsights-maps/map-group-network-conn-pane-01.png)  

### <a name="failed-connections"></a>Başarısız bağlantılar
Harita, işlemler ve bilgisayarlar için başarısız bağlantıları gösterir. Kesikli kırmızı çizgi, istemci sisteminin bir işleme veya bağlantı noktasına ulaşamadıgIni gösterir. Bağımlılık aracısını kullanan sistemler için aracı, başarısız bağlantı denemelerini raporlar. Harita özelliği, bağlantı kuramayan TCP soketlerini gözlemleyerek bir işlemi izler. Bu hata, bir güvenlik duvarı, istemci veya sunucuda bir yanlış yapılandırma veya kullanılamayan bir uzak hizmetten kaynaklanabilir.

![Haritada başarısız bir bağlantı](./media/vminsights-maps/map-group-failed-connection-01.png)

Başarısız bağlantıları anlamak, sorun gidermenize, geçişi doğrulamanıza, güvenliği çözümlemenize ve hizmetin genel mimarisini anlamanıza yardımcı olabilir. Başarısız bağlantılar bazen zararsızdır, ancak genellikle bir soruna işaret ederler. Bağlantılar, örneğin, bir başarısız ortam aniden erişimediğinde veya iki uygulama katmanı bulut geçişinden sonra birbirleriyle iletişim kuramadığında başarısız olabilir.

### <a name="client-groups"></a>İstemci grupları
Haritada, istemci grupları eşlenen makineye bağlanan istemci makineleri temsil eder. Tek bir istemci grubu, tek bir işlem veya makine için istemcileri temsil eder.

![Haritada bir istemci grubu](./media/vminsights-maps/map-group-client-groups-01.png)

İstemci grubundaki sistemlerin izlenen istemcilerini ve IP adreslerini görmek için grubu seçin. Grubun içeriği aşağıda görünür.  

![Bir istemci grubunun haritadaki IP adresleri listesi](./media/vminsights-maps/map-group-client-group-iplist-01.png)

Grup izlenen ve izlenmeyen istemciler içeriyorsa, istemcilere filtre uygulayacak şekilde grubun halka grafiğinin uygun bölümünü seçebilirsiniz.

### <a name="server-port-groups"></a>Sunucu bağlantı noktası grupları
Sunucu bağlantı noktası grupları, eşlenen makineden gelen bağlantıları olan sunucularda bağlantı noktalarını temsil eder. Grup sunucu bağlantı noktasını ve bu bağlantı noktasına bağlantısı olan sunucu sayısının sayısını içerir. Tek tek sunucuları ve bağlantıları görmek için grubu seçin. 

![Haritada bir sunucu bağlantı noktası grubu](./media/vminsights-maps/map-group-server-port-groups-01.png)  

Grup izlenen ve izlenmeyen sunucular içeriyorsa, sunuculara filtre uygulayacak uygun grubu halka grafiği bölümünü seçebilirsiniz.

## <a name="view-a-map-from-a-vm"></a>Bir VM'den harita görüntüleme 

VM'ler için Azure Monitör'e doğrudan bir VM'den erişmek için:

1. Azure portalında **Sanal Makineler'i**seçin. 
2. Listeden bir VM seçin. **İzleme** **bölümünde, İstatistikler'i**seçin.  
3. **Harita** sekmesini seçin.

Harita, belirli bir zaman aralığında etkin ağ bağlantıları olan çalışan işlem gruplarını ve süreçlerini keşfederek VM'nin bağımlılıklarını görselleştirir.  

Varsayılan olarak, harita son 30 dakikayı gösterir. Geçmişte bağımlılıkların nasıl göründüğünü görmek istiyorsanız, bir saate kadar olan geçmiş zaman aralıklarını sorgulayabilirsiniz. Sorguyu çalıştırmak için sol üst köşedeki **TimeRange** seçicisini kullanın. Örneğin, bir olay sırasında veya değişiklikten önce durumu görmek için bir sorgu çalıştırabilirsiniz.  

![Doğrudan VM haritasına genel bakış](./media/vminsights-maps/map-direct-vm-01.png)

## <a name="view-a-map-from-a-virtual-machine-scale-set"></a>Sanal makine ölçeği kümesinden harita görüntüleme

Sanal makine ölçeği kümesinden doğrudan Sanal Sanal Ayarlar için Azure Monitor'a erişmek için:

1. Azure portalında **Sanal makine ölçek kümelerini**seçin.
2. Listeden bir VM seçin. Ardından **İzleme** **bölümünde, Insights'ı**seçin.  
3. **Harita** sekmesini seçin.

Eşleşme, grup düğümü olarak belirlenen çlümdeki bütün çi Genişletilmiş düğüm, ölçek kümesindeki örnekleri listeler. Bu örnekler arasında bir seferde 10 arasında gezinebilirsiniz. 

Belirli bir örnek için harita yüklemek için, ilk olarak haritada bu örneği seçin. Ardından sağdaki **elips düğmesini** (...) seçin ve **Load Server Map'i**seçin. Görünen haritada, belirli bir zaman aralığında etkin ağ bağlantıları olan işlem gruplarını ve işlemleri görürsünüz. 

Varsayılan olarak, harita son 30 dakikayı gösterir. Geçmişte bağımlılıkların nasıl göründüğünü görmek istiyorsanız, bir saate kadar olan geçmiş zaman aralıklarını sorgulayabilirsiniz. Sorguyu çalıştırmak için **TimeRange** seçiciyi kullanın. Örneğin, bir olay sırasında veya değişiklikten önce durumu görmek için bir sorgu çalıştırabilirsiniz.

![Doğrudan VM haritasına genel bakış](./media/vminsights-maps/map-direct-vmss-01.png)

>[!NOTE]
>Ayrıca, sanal makine ölçek kümeniz için **Örnekler** görünümünden belirli bir örnek için bir haritaya da erişebilirsiniz. **Ayarlar** **bölümünde, Örnekler** > **Öngörüleri**bölümüne gidin.

## <a name="view-a-map-from-azure-monitor"></a>Azure Monitör'den harita görüntüleme

Azure Monitor'da Harita özelliği, VM'lerinizin ve bunların bağımlılıklarının genel görünümünü sağlar. Azure Monitor'da Harita özelliğine erişmek için:

1. Azure portalında **Monitör'ü**seçin. 
2. **Öngörüler** bölümünde Sanal **Makineler'i**seçin.
3. **Harita** sekmesini seçin.

   ![Azure Monitor birden fazla VM'nin genel bakış haritası](./media/vminsights-maps/map-multivm-azure-monitor-01.png)

Sayfanın üst kısmındaki **Çalışma Alanı** seçicisini kullanarak bir çalışma alanı seçin. Birden fazla Log Analytics çalışma alanınız varsa, çözümle birlikte etkinleştirilen ve VM'lerin rapor verdiği çalışma alanını seçin. 

**Grup** seçici, seçili çalışma alanıyla ilgili abonelikleri, kaynak gruplarını, [bilgisayar gruplarını](../../azure-monitor/platform/computer-groups.md)ve sanal makine ölçeği kümelerini döndürür. Seçiminiz yalnızca Harita özelliği için geçerlidir ve Performans veya Sağlık'a taşımaz.

Varsayılan olarak, harita son 30 dakikayı gösterir. Geçmişte bağımlılıkların nasıl göründüğünü görmek istiyorsanız, bir saate kadar olan geçmiş zaman aralıklarını sorgulayabilirsiniz. Sorguyu çalıştırmak için **TimeRange** seçiciyi kullanın. Örneğin, bir olay sırasında veya değişiklikten önce durumu görmek için bir sorgu çalıştırabilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

Performans sorunlarını belirlemek, performansı kontrol etmek ve VM'lerinizin genel kullanımını anlamak [için, VM'ler için Azure Monitörü için görünüm performans durumuna](vminsights-performance.md)bakın. 
