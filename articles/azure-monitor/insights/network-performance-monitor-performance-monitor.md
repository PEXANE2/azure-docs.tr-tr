---
title: Ağ Performans Monitöründe Performans Monitörü
description: Ağ Performans İzleyicisi'ndeki Performans İzleyicisi özelliği, ağınızdaki çeşitli noktalardaki ağ bağlantılarını izlemenize yardımcı olur. Bulut dağıtımlarını ve şirket içi konumları, birden çok veri merkezini ve şube ofisini ve görev açısından kritik çok katmanlı uygulamaları veya mikro hizmetleri izleyebilirsiniz.
ms.subservice: logs
ms.topic: conceptual
author: abshamsft
ms.author: absha
ms.date: 02/20/2018
ms.openlocfilehash: 126cca9d3606b378e59e4f4e1c5b52d985d19d94
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80055683"
---
# <a name="network-performance-monitor-solution-performance-monitoring"></a>Ağ Performans Monitörü çözümü: Performans izleme

Ağ Performans İzleyicisi'ndeki [Performans İzleyicisi](network-performance-monitor.md) özelliği, ağınızdaki çeşitli noktalardaki ağ bağlantılarını izlemenize yardımcı olur. Bulut dağıtımlarını ve şirket içi konumları, birden çok veri merkezini ve şube ofisini ve görev açısından kritik çok katmanlı uygulamaları veya mikro hizmetleri izleyebilirsiniz. Performans Monitörü ile, kullanıcılarınız şikayet etmeden önce ağ sorunlarını algılayabilirsiniz. Önemli avantajları şunlardır: 

- Çeşitli alt ağlarda kaybı ve gecikmeyi izleyin ve uyarıları ayarlayın.
- Ağdaki tüm yolları (gereksiz yollar dahil) izleyin.
- Çoğaltması zor olan geçici ve zaman içinde nokta ağ sorunlarını giderin.
- Bozulmuş performansa karşı sorumlu olan ağdaki belirli kesimi belirleyin.
- SNMP'ye gerek kalmadan ağın durumunu izleyin.


![Ağ Performansı İzleyicisi](media/network-performance-monitor-performance-monitor/npm-performance-monitor.png)

## <a name="configuration"></a>Yapılandırma
Ağ Performans Monitörü için yapılandırmayı açmak [için, Ağ Performans Monitörü çözümünü](network-performance-monitor.md)açın ve **Yapılandır'ı**seçin.

![Ağ Performansı İzleyicisi’ni Yapılandırma](media/network-performance-monitor-performance-monitor/npm-configure-button.png)

### <a name="create-new-networks"></a>Yeni ağlar oluşturma

Ağ Performans İzleyicisi'ndeki ağ, alt ağlar için mantıksal bir kapsayıcıdır. Ağ altyapınızın izlenmesini ihtiyaçlarınıza göre düzenlemenize yardımcı olur. Dostane bir ada sahip bir ağ oluşturabilir ve iş mantığınıza göre bu ağa alt ağlar ekleyebilirsiniz. Örneğin, Londra adlı bir ağ oluşturabilir ve Londra veri merkezinizdeki tüm alt ağları ekleyebilirsiniz. Ya da *ContosoFrontEnd* adında bir ağ oluşturabilir ve bu ağa uygulamanızın ön ucuna hizmet veren Contoso adlı tüm alt ağları ekleyebilirsiniz. Çözüm, ortamınızda keşfedilen tüm alt ağları içeren varsayılan bir ağ oluşturur. 

Bir ağ oluşturduğunuzda, ağa bir alt ağ eklersiniz. Daha sonra bu alt ağ varsayılan ağdan kaldırılır. Bir ağı silerseniz, tüm alt ağları varsayılan ağa otomatik olarak döndürülür. Varsayılan ağ, kullanıcı tanımlı ağda yer alan tüm alt ağlar için bir kapsayıcı görevi görür. Varsayılan ağı düzenleyebilir veya silemezsiniz. Her zaman sistemde kalır. İhtiyacınız olduğu kadar özel ağ oluşturabilirsiniz. Çoğu durumda, kuruluşunuzdaki alt ağlar birden fazla ağda düzenlenir. Alt ağlarınızı iş mantığınız için gruplandırmak için bir veya daha fazla ağ oluşturun.

Yeni bir ağ oluşturmak için:


1. **Ağlar** sekmesini seçin.
1.  **Ağ Ekle'yi**seçin ve ardından ağ adını ve açıklamasını girin. 
2. Bir veya daha fazla alt ağ seçin ve sonra **Ekle'yi**seçin. 
3. Yapılandırmayı kaydetmek için **Kaydet'i** seçin. 


### <a name="create-monitoring-rules"></a>İzleme kuralları oluşturma 

Performans İzleyicisi, iki alt ağ veya iki ağ arasındaki ağ bağlantılarının performans eşiği ihlal edildiğinde sistem durumu olayları oluşturur. Sistem bu eşikleri otomatik olarak öğrenebilir. Ayrıca özel eşikler de sağlayabilirsiniz. Sistem otomatik olarak bir varsayılan kural oluşturur ve herhangi bir ağ veya alt ağ bağlantısı çifti arasında herhangi bir kayıp veya gecikme sistemi öğrenilen eşiği ihlal ettiğinde bir sistem olayı oluşturur. Bu işlem, açıkça herhangi bir izleme kuralı oluşturmadan çözümün ağ altyapınızı izlemesine yardımcı olur. Varsayılan kural etkinse, tüm düğümler izleme için etkinleştirdiğiniz tüm diğer düğümlere sentetik hareketler gönderir. Varsayılan kural küçük ağlar için yararlıdır. Bir mikro hizmet çalıştıran az sayıda sunucuya sahip olduğunuz ve tüm sunucuların birbirine bağlantı verdiğinden emin olmak istediğiniz bir senaryo örneğidir.

>[!NOTE]
> Varsayılan kuralı devre dışı bırakıp, özellikle izleme için çok sayıda düğüm kullandığınız büyük ağlarda özel izleme kuralları oluşturmanızı öneririz. Özel izleme kuralları, çözümün oluşturduğu trafiği azaltabilir ve ağınızın denetimini düzenlemenize yardımcı olabilir.

İş mantığınıza göre izleme kuralları oluşturun. İki ofis sitesi merkezinin ağ bağlantısının performansını izlemek istiyorsanız, bir örnek. Ofis sitesindeki tüm alt ağları 1 ağ O1'de gruplandırın. Daha sonra ofis sitesi2'deki tüm alt ağları ağ O2'de gruplandırın. Son olarak, h ağındaki merkezdeki tüm alt ağları gruplandırın. 

Özel izleme kuralları oluşturmak için:

1. **Monitör** sekmesinde **Kural Ekle'yi** seçin ve kural adını ve açıklamasını girin.
2. Listelerden izlemek için ağ veya alt ağ bağlantısı çiftini seçin. 
3. Ağ açılır listesinden istediğiniz alt ağları içeren ağı seçin. Ardından, ilgili alt ağ açılır listesinden alt ağları seçin. Bir ağ bağlantısındaki tüm alt ağları izlemek istiyorsanız, **Tüm alt ağları**seçin. Benzer şekilde, istediğiniz diğer alt ağları seçin. Belirli alt ağ bağlantılarının izlemesini yaptığınız seçimlerden hariç tutmak için **Özel Durum Ekle'yi**seçin. 
4. Sentetik işlemleri yürütmek için ICMP ve TCP protokolleri arasında seçim yapın. 
5. Seçtiğiniz öğeler için sistem durumu olayları oluşturmak istemiyorsanız, **bu kuralın kapsadığı bağlantılarda Sistem Durumu İzlemeyi Etkinleştir'i**temizleyin. 
6. İzleme koşullarını seçin. Sistem durumu-olay oluşturma için özel eşikler ayarlamak için eşik değerlerini girin. Koşulun değeri seçili ağ veya alt ağ çifti için seçili eşiğini aştığında, bir sistem durumu olayı oluşturulur. 
7. Yapılandırmayı kaydetmek için **Kaydet'i** seçin. 

Bir izleme kuralını kurtardıktan sonra, **Uyarı Oluştur'u**seçerek bu kuralı Uyarı Yönetimi ile tümleştirebilirsiniz. Arama sorgusuyla birlikte otomatik olarak bir uyarı kuralı oluşturulur. Gerekli diğer parametreler otomatik olarak doldurulur. Bir uyarı kuralı nı kullanarak, Ağ Performans İzleyicisi'ndeki varolan uyarılara ek olarak e-posta tabanlı uyarılar alabilirsiniz. Uyarılar ayrıca runbook'larla düzeltici eylemleri tetikleyebilir veya webhooks kullanarak varolan hizmet yönetimi çözümleriyle tümleştirebilirsiniz. Uyarı ayarlarını yönetmek için **Uyarıyı Yönet'i** seçin. 

Artık daha fazla Performans İzleyicisi kuralı oluşturabilir veya bu özelliği kullanmak için çözüm panosuna geçebilirsiniz.

### <a name="choose-the-protocol"></a>Protokolü seçin

Ağ Performans İzleyicisi, paket kaybı ve bağlantı gecikmesi gibi ağ performansı ölçümlerini hesaplamak için sentetik işlemler kullanır. Bu kavramı daha iyi anlamak için, ağ bağlantısının bir ucuna bağlı bir Ağ Performans İzleme aracısı düşünün. Bu Ağ Performans İzleyicisi aracısı, sonda paketlerini ağın başka bir ucuna bağlı ikinci bir Ağ Performans İzleme aracısına gönderir. İkinci aracı yanıt paketleriyle yanıt verir. Bu işlem birkaç kez yinelenir. Yanıt sayısını ve her yanıtı almak için geçen süreyi ölçerek, ilk Ağ Performans İzleme aracısı bağlantı gecikmesi ve paket düşüşlerini değerlendirir. 

Bu paketlerin biçimi, boyutu ve sırası, izleme kuralları oluştururken seçtiğiniz protokol tarafından belirlenir. Paketlerin protokolüne bağlı olarak, yönlendiriciler ve anahtarlar gibi ara ağ aygıtları bu paketleri farklı şekilde işleyebilir. Sonuç olarak, protokol seçiminiz sonuçların doğruluğunu etkiler. Protokol seçiminiz, Ağ Performans İzleyicisi çözümünü dağıttıktan sonra herhangi bir el ile adım atmanız gerekip gerekmediğini de belirler. 

Ağ Performans İzleyicisi, sentetik işlemleri yürütmek için ICMP ve TCP protokolleri arasında seçim sunar. Sentetik bir işlem kuralı oluşturduğunuzda ICMP'yi seçerseniz, Ağ Performans İzleyicisi aracıları ağ gecikmesi ve paket kaybını hesaplamak için ICMP ECHO iletilerini kullanır. ICMP ECHO, geleneksel ping yardımcı programı tarafından gönderilen iletiyi kullanır. TCP'yi protokol olarak kullandığınızda, Ağ Performans İzleyicisi aracıları TCP SYN paketlerini ağ üzerinden gönderir. Bu adımı Bir TCP el sıkışma tamamlanması takip eder ve bağlantı RST paketleri kullanılarak kaldırılır. 

Bir protokol seçmeden önce aşağıdaki bilgileri göz önünde bulundurun: 

* **Birden çok ağ rotasının keşfi.** TCP birden çok rota yı keşfederken daha doğrudur ve her alt ağda daha az aracıya ihtiyaç duyar. Örneğin, TCP kullanan bir veya iki aracı alt ağlar arasındaki tüm gereksiz yolları keşfedebilir. Benzer sonuçlar elde etmek için ICMP kullanan birkaç aracıya ihtiyacınız vardır. ICMP kullanarak, iki alt ağ arasında bir dizi rotanız varsa, kaynak veya hedef alt ağda 5N'den fazla aracıya ihtiyacınız vardır.

* **Sonuçların doğruluğu.** Yönlendiriciler ve anahtarlar, TCP paketlerine kıyasla ICMP ECHO paketlerine daha düşük öncelik atama eğilimindedir. Bazı durumlarda, ağ aygıtları yoğun olarak yüklendiğinde, TCP tarafından elde edilen veriler uygulamaların yaşadığı kayıp ve gecikmeyi daha yakından yansıtır. Bunun nedeni, uygulama trafiğinin çoğunun TCP üzerinde akmasıdır. Bu gibi durumlarda ICMP, TCP'ye göre daha az doğru sonuçlar sağlar. 

* **Güvenlik duvarı yapılandırması.** TCP protokolü, TCP paketlerinin bir hedef bağlantı noktasına gönderilmesini gerektirir. Ağ Performans İzleyicisi aracıları tarafından kullanılan varsayılan bağlantı noktası 8084'dür. Aracıları yapılandırdığınızda bağlantı noktasını değiştirebilirsiniz. Ağ güvenlik duvarlarınızın veya ağ güvenlik grubunuzun (NSG) kurallarının (Azure'da) bağlantı noktasındaki trafiğe izin verdiğinden emin olun. Aracıların yüklü olduğu bilgisayarlardaki yerel güvenlik duvarının bu bağlantı noktasındaki trafiğe izin verecek şekilde yapılandırıldığından da emin olmanız gerekir. Windows çalıştıran bilgisayarlarınızda güvenlik duvarı kurallarını yapılandırmak için PowerShell komut dosyalarını kullanabilirsiniz, ancak ağ güvenlik duvarınızı el ile yapılandırmanız gerekir. Buna karşılık, ICMP bir bağlantı noktası kullanarak çalışmaz. Çoğu kurumsal senaryoda, ping yardımcı programı gibi ağ tanılama araçlarını kullanmanıza izin vermek için güvenlik duvarları aracılığıyla ICMP trafiğine izin verilir. Bir makineyi diğerinden pingleyebilirseniz, güvenlik duvarlarını el ile yapılandırmak zorunda kalmadan ICMP protokolünü kullanabilirsiniz.

>[!NOTE] 
> Bazı güvenlik duvarları ICMP'yi engelleyebilir ve bu da güvenlik bilgilerinizde ve olay yönetim sisteminizde çok sayıda olaya neden olan yeniden aktarıma neden olabilir. Seçtiğiniz protokolün bir ağ güvenlik duvarı veya NSG tarafından engellenmediğinden emin olun. Aksi takdirde, Ağ Performans İzleyicisi ağ kesimini izleyebilir. İzleme için TCP kullanmanızı öneririz. ICMP'yi TCP'yi kullanamadığınız senaryolarda kullanın( örneğin: 
>
> - Windows istemcilerinde TCP ham soketlerine izin verilmediği için Windows istemci tabanlı düğümler kullanırsınız.
> - Ağ güvenlik duvarınız veya NSG TCP'yi engeller.
> - Protokolü nasıl değiştirip değiştirip değiştirebildiğini bilmiyorsun.

Dağıtım sırasında ICMP kullanmayı seçtiyseniz, varsayılan izleme kuralını düzenleyerek istediğiniz zaman TCP'ye geçebilirsiniz.

1. Ağ **Performans** > **Monitörü** > **Yapılandırma Monitörü'ne** > **Monitor**gidin. Ardından **Varsayılan kuralını**seçin. 
2. **Protokol** bölümüne gidin ve kullanmak istediğiniz protokolü seçin. 
3. Ayarı uygulamak için **Kaydet'i** seçin. 

Varsayılan kural belirli bir protokol kullansa bile, farklı bir protokolle yeni kurallar oluşturabilirsiniz. Hatta bazı kuralların ICMP, diğerlerinin ise TCP kullandığı bir kural karışımı oluşturabilirsiniz. 

## <a name="walkthrough"></a>Kılavuz 

Şimdi bir sağlık olayı için kök nedeni içine basit bir soruşturma bak.

Çözüm panosunda, bir sistem durumu olayı bir ağ bağlantısının sağlıksız olduğunu gösterir. Sorunu araştırmak **için, izlenen Ağ bağlantılarını** seçin.

Detaya kadar olan sayfa, **DMZ2-DMZ1** ağ bağlantısının sağlıksız olduğunu gösterir. Bu ağ bağlantısı için **alt ağ bağlantılarını görüntüle'yi** seçin. 


Detaylandırma **sayfası, DMZ2-DMZ1** ağ bağlantısındaki tüm alt ağ bağlantılarını gösterir. Her iki alt ağ bağlantısı için gecikme sınırını geçti ve bu da ağ bağlantısını sağlıksız hale getiriyor. Ayrıca, her iki alt ağ bağlantısının gecikme eğilimlerini de görebilirsiniz. Gerekli zaman aralığına odaklanmak için grafikteki zaman seçim denetimini kullanın. Gecikme süresinin zirveye ulaştığı saati görebilirsiniz. Sorunu araştırmak için bu süre için daha sonra günlükleri arayın. Daha fazla detaya inmek için **düğüm bağlantılarını** görüntüle'yi seçin. 
 
 ![Alt Ağ Bağlantıları sayfası](media/network-performance-monitor-performance-monitor/subnetwork-links.png) 

Önceki sayfaya benzer şekilde, belirli alt ağ bağlantısının ayrıntısayfası, kurucu düğüm bağlantılarını listeler. Önceki adımda yaptığınız gibi burada da benzer eylemler gerçekleştirebilirsiniz. İki düğüm arasındaki topolojiyi görüntülemek için **topolojiyi** görüntüle'yi seçin. 
 
 ![Düğüm Bağlantıları sayfası](media/network-performance-monitor-performance-monitor/node-links.png) 

Seçilen iki düğüm arasındaki tüm yollar topoloji haritasında çizilir. Topoloji haritasında iki düğüm arasındaki yolların hop-by-hop topolojisini görselleştirebilirsiniz. İki düğüm arasında kaç yol olduğunu ve veri paketlerinin hangi yolları izlediğihakkında net bir resim sunar. Ağ performans darboğazları kırmızı renkte gösterilir. Hatalı bir ağ bağlantısını veya hatalı bir ağ aygıtını bulmak için topoloji haritasındaki kırmızı öğelere bakın. 

 ![Topoloji haritası ile Topoloji Panosu](media/network-performance-monitor-performance-monitor/topology-dashboard.png) 

**Eylem** bölmesindeki her yoldaki kayıp, gecikme sonu ve atlama sayısını gözden geçirebilirsiniz. Sağlıksız yolların ayrıntılarını görüntülemek için kaydırma çubuğunu kullanın. Yalnızca seçili yolların topolojisinin çizilen sağlıksız atlamalı yolları seçmek için filtreleri kullanın. Topoloji haritasını yakınlaştırmak veya uzaklaştırmak için fare tekerleğinizi kullanın. 

Aşağıdaki resimde, sorunun alanlarının ağ belirli bölümüne kök nedeni kırmızı yollarda ve atlamalarda görünür. FQDN ve IP adresini içeren düğümün özelliklerini göstermek için topoloji haritasında bir düğüm seçin. Atlama seçilmesi, atlamanın IP adresini gösterir. 
 
![Düğüm özellikleri seçili topoloji haritası](media/network-performance-monitor-performance-monitor/topology-dashboard-root-cause.png) 

## <a name="next-steps"></a>Sonraki adımlar
Ayrıntılı ağ performansı veri kayıtlarını görüntülemek için [günlükleri arayın.](../../azure-monitor/log-query/log-query-overview.md)
