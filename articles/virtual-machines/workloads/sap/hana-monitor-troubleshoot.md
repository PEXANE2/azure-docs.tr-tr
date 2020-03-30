---
title: Azure'da SAP HANA'da HANA tarafından izleme ve sorun giderme (Büyük Örnekler) | Microsoft Dokümanlar
description: Bir Azure'da (Büyük Örnekler) SAP HANA'da HANA tarafından izleme ve sorun giderme.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 047ea4d07f2b497ac8c7deb90c056d63976094f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617068"
---
# <a name="monitoring-and-troubleshooting-from-hana-side"></a>HANA tarafından izleme ve sorun giderme

Azure'da (Büyük Örnekler) SAP HANA ile ilgili sorunları etkili bir şekilde çözümlemek için, bir sorunun temel nedenini daraltmak yararlıdır. SAP size yardımcı olmak için çok sayıda belge yayımlamıştır.

SAP HANA performansıyla ilgili geçerli SSS'leri aşağıdaki SAP Notlarında bulunabilir:

- [SAP Note #2222200 – SSS: SAP HANA Ağı](https://launchpad.support.sap.com/#/notes/2222200)
- [SAP Not #2100040 – SSS: SAP HANA CPU](https://launchpad.support.sap.com/#/notes/0002100040)
- [SAP Not #199997 – SSS: SAP HANA Bellek](https://launchpad.support.sap.com/#/notes/2177064)
- [SAP Not #200000 – SSS: SAP HANA Performans Optimizasyonu](https://launchpad.support.sap.com/#/notes/2000000)
- [SAP Not #199930 – SSS: SAP HANA I/O Analizi](https://launchpad.support.sap.com/#/notes/1999930)
- [SAP Note #2177064 – SSS: SAP HANA Hizmeti Yeniden Başlat Ve Çöküyor](https://launchpad.support.sap.com/#/notes/2177064)

## <a name="sap-hana-alerts"></a>SAP HANA Uyarıları

İlk adım olarak, geçerli SAP HANA uyarı günlüklerini kontrol edin. SAP HANA Studio'da **Yönetim Konsolu'na gidin: Uyarılar: Göster: tüm uyarılar.** Bu sekme, belirlenen minimum ve maksimum eşiklerin dışına çıkan belirli değerler (ücretsiz fiziksel bellek, CPU kullanımı, vb.) için tüm SAP HANA uyarılarını gösterir. Varsayılan olarak, denetimler her 15 dakikada bir otomatik olarak yenilenir.

![SAP HANA Studio'da Yönetim Konsolu'na gidin: Uyarılar: Göster: tüm uyarılar](./media/troubleshooting-monitoring/image1-show-alerts.png)

## <a name="cpu"></a>CPU

Yanlış eşik ayarı nedeniyle tetiklenen bir uyarı için, bir çözünürlük varsayılan değere veya daha makul bir eşik değerine sıfırlanır.

![Varsayılan değere veya daha makul bir eşik değerine sıfırlama](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

Aşağıdaki uyarılar CPU kaynak sorunlarını gösterebilir:

- Ana Bilgisayar CPU Kullanımı (Uyarı 5)
- En son savepoint işlemi (Alert 28)
- Savepoint süresi (Uyarı 54)

SAP HANA veritabanınızda yüksek CPU tüketimini aşağıdakilerden birinden fark edebilirsiniz:

- Alert 5 (Host CPU kullanımı) geçerli veya geçmiş CPU kullanımı için yükseltilir
- Genel bakış ekranında görüntülenen CPU kullanımı

![Genel bakış ekranında görüntülenen CPU kullanımı](./media/troubleshooting-monitoring/image3-cpu-usage.png)

Yük grafiği yüksek CPU tüketimini veya geçmişte yüksek tüketimi gösterebilir:

![Yük grafiği yüksek CPU tüketimini veya geçmişte yüksek tüketimi gösterebilir](./media/troubleshooting-monitoring/image4-load-graph.png)

Yüksek CPU kullanımı nedeniyle tetiklenen bir uyarı, belirli işlemlerin yürütülmesi, veri yüklemesi, yanıt vermeyen işler, uzun süren SQL deyimleri ve kötü sorgu performansı (örneğin, BW ile HANA küpleri).

Ayrıntılı sorun giderme adımları için [SAP HANA Sorun Giderme: CPU Ile İlgili Nedenler ve Çözümler](https://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) sitesine bakın.

## <a name="operating-system"></a>İşletim Sistemi

Linux'ta SAP HANA için en önemli kontrollerden biri Şeffaf Büyük Sayfaların devre dışı bırakıldığından emin olmaktır, sap [hana sunucularında SAP Note #2131662 – Şeffaf Büyük Sayfalar (THP)](https://launchpad.support.sap.com/#/notes/2131662)bölümüne bakın.

- Saydam Büyük Sayfaların aşağıdaki Linux komutu yla etkin olup olmadığını kontrol edebilirsiniz: **cat /sys/kernel/mm/transparent\_hugepage/enabled**
- _Her zaman_ aşağıdaki gibi parantez içinde kapalı ise, Saydam Büyük Sayfalar etkin olduğu anlamına gelir: [her zaman] madvise asla; _hiçbir zaman_ aşağıdaki gibi parantez içinde kapalı değilse, Saydam Büyük Sayfalar devre dışı bırakıldığı anlamına gelir: her zaman [asla]

Aşağıdaki Linux komutu hiçbir şey döndürmemelidir: **rpm -qa | grep ulimit.** _Ulimit_ yüklü görünüyorsa, hemen kaldırın.

## <a name="memory"></a>Bellek

SAP HANA veritabanı tarafından ayrılan bellek miktarının beklenenden yüksek olduğunu gözlemleebilirsiniz. Aşağıdaki uyarılar yüksek bellek kullanımı yla ilgili sorunları gösterir:

- Ana bilgisayar fiziksel bellek kullanımı (Uyarı 1)
- Ad sunucusunun bellek kullanımı (Alert 12)
- Sütun Deposu tablolarının toplam bellek kullanımı (Uyarı 40)
- Hizmetlerin bellek kullanımı (Alert 43)
- Sütun Deposu tablolarının ana depolama alanının bellek kullanımı (Uyarı 45)
- Çalışma zamanı döküm dosyaları (Alert 46)

Ayrıntılı sorun giderme adımları için [SAP HANA Sorun Giderme: Bellek Sorunları](https://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) sitesine bakın.

## <a name="network"></a>Ağ

SAP Note #2081065 ' a bakın [– SORUN Giderme SAP HANA Ağı](https://launchpad.support.sap.com/#/notes/2081065) ve bu SAP Notunda ağ sorun giderme adımlarını gerçekleştirin.

1. Sunucu ve istemci arasındaki gidiş-dönüş süresini çözümleme.
  A. SQL komut dosyası [_HANA\_\_Ağ İstemlerini_](https://launchpad.support.sap.com/#/notes/1969700)çalıştırın._._
  
2. İnternod iletişimini analiz edin.
  A. SQL komut dosyası [_HANA\_Ağ\_Hizmetleri_](https://launchpad.support.sap.com/#/notes/1969700)çalıştırın._._

3. Linux komutu **ifconfig** çalıştırın (çıktı herhangi bir paket kaybı meydana geliyorsa gösterir).
4. Linux **komutu tcpdump**çalıştırın.

Ayrıca, gerçek uygulama ağı performansını ölçmek için açık kaynak [IPERF](https://iperf.fr/) aracını (veya benzersini) kullanın.

Ayrıntılı sorun giderme adımları için [SAP HANA Sorun Giderme: Ağ Performansı ve Bağlantı Sorunları](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) sitesine bakın.

## <a name="storage"></a>Depolama

Son kullanıcı açısından bakıldığında, bir uygulama (veya bir bütün olarak sistem) yavaş çalışır, yanıt vermiyor veya G/Ç performansıyla ilgili sorunlar varsa yanıt vermeyi bile durdurabilir. SAP HANA Studio'daki **Birimler** sekmesinde, ekli birimleri ve her hizmet tarafından hangi birimlerin kullanıldığını görebilirsiniz.

![SAP HANA Studio'daki Birimler sekmesinde, ekli birimleri ve her hizmet tarafından hangi birimlerin kullanıldığını görebilirsiniz](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

Ekranın alt kısmında ekli birimler, dosyalar ve G/Ç istatistikleri gibi birimlerin ayrıntılarını görebilirsiniz.

![Ekranın alt kısmında ekli birimler, dosyalar ve G/Ç istatistikleri gibi birimlerin ayrıntılarını görebilirsiniz](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

Ayrıntılı sorun giderme adımları için [SAP HANA Sorun Giderme: I/Ç İlgili Kök Nedenleri ve Çözümleri](https://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) ve SAP [HANA Sorun Giderme: Diskle İlgili Kök Nedenleri ve Çözümleri](https://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) sitesine bakın.

## <a name="diagnostic-tools"></a>Tanı Araçları

HANA\_Configuration\_Minichecks aracılığıyla SAP HANA Sağlık Kontrolü yapın. Bu araç, SAP HANA Studio'da uyarı olarak zaten yükseltilmiş olması gereken potansiyel olarak kritik teknik sorunları döndürür.

SAP [HANA için SAP Note #1969700 - SQL ekstre koleksiyonuna](https://launchpad.support.sap.com/#/notes/1969700) bakın ve bu nota eklenen SQL Statements.zip dosyasını indirin. Bu .zip dosyasını yerel sabit diskte saklayın.

SAP HANA Studio'da, **Sistem Bilgileri** sekmesinde, **Ad** sütununa sağ tıklayın ve **SQL Deyimlerini Aktar'ı**seçin.

![SAP HANA Studio'da, Sistem Bilgileri sekmesinde, Ad sütununa sağ tıklayın ve SQL Deyimlerini Aktar'ı seçin](./media/troubleshooting-monitoring/image7-import-statements-a.png)

Yerel olarak depolanan SQL Statements.zip dosyasını seçin ve ilgili SQL deyimlerinin bulunduğu bir klasör içe aktarılır. Bu noktada, birçok farklı tanılama denetimleri bu SQL deyimleri ile çalıştırılabilir.

Örneğin, SAP HANA System Replication bant genişliği gereksinimlerini test etmek için Çoğaltma altındaki **Bant Genişliği** deyimini sağ **tıklatın: Bant Genişliği** ve SQL Konsolunda **Aç'ı** seçin.

Tam SQL deyimi, giriş parametrelerinin (değişiklik bölümü) değiştirilmesine ve sonra yürütülmesine izin vererek açılır.

![Tam SQL deyimi, giriş parametrelerinin (modifikasyon bölümü) değiştirilmesine ve sonra yürütülmesine izin veren açılır](./media/troubleshooting-monitoring/image8-import-statements-b.png)

Başka bir örnek çoğaltma altındaki ifadelere sağ **tıklayarak: Genel Bakış**. Bağlam **Execute** menüsünden Yürüt''yü seçin:

![Başka bir örnek, Çoğaltma altındaki ifadelere sağ tıklamadır: Genel Bakış. Bağlam menüsünden Yürüt''yü seçin](./media/troubleshooting-monitoring/image9-import-statements-c.png)

Bu, sorun giderme ile yardımcı olan bilgilerle sonuçlanır:

![Bu, sorun giderme ile yardımcı olacak bilgilerle sonuçlanır](./media/troubleshooting-monitoring/image10-import-statements-d.png)

\_Aynısını HANA\_Configuration Minichecks için de yapın ve _C_ (Kritik) sütunundaki _X_ işaretlerini kontrol edin.

Örnek çıktıları:

Genel SAP HANA kontrolleri için **\_HANA Configuration\_MiniChecks\_Rev102.01+1.**

![Genel\_SAP\_HANA çekleri için HANA Configuration MiniChecks\_Rev102.01+1](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

**SAP\_HANA\_** hizmetlerinin şu anda yürüttüğü ne genel bir bakış için HANA Hizmetlerine Genel Bakış.

![SAP\_HANA hizmetlerinin şu anda yürüttüğü ne genel bir bakış için HANA Hizmetlerine\_Genel Bakış](./media/troubleshooting-monitoring/image12-services-overview.png)

**SAP\_HANA\_** hizmet bilgileri için HANA Hizmet İstatistikleri (CPU, bellek, vb.).

![SAP\_HANA hizmet bilgileri için HANA Hizmet\_İstatistikleri](./media/troubleshooting-monitoring/image13-services-statistics.png)

SAP HANA örneği hakkında genel bilgi için **HANA\_Yapılandırmaya\_Genel Bakış\_Rev110+**

![SAP\_HANA örneği hakkında genel bilgi için HANA Yapılandırmaya\_Genel Bakış\_Rev110+](./media/troubleshooting-monitoring/image14-configuration-overview.png)

SAP HANA parametrelerini kontrol etmek için **HANA\_\_Konfigürasyon Parametreleri\_Rev70+**

![SAP\_HANA parametrelerini kontrol etmek için HANA Konfigürasyon\_Parametreleri\_Rev70+](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

**Sonraki adımlar**

- [STONITH kullanarak SUSE'da ayarlanmış yüksek kullanılabilirliği ifade edin.](ha-setup-with-stonith.md)