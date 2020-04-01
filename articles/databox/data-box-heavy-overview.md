---
title: Microsoft Azure Data Box Heavy'ye genel bakış | Verilerde Microsoft Docs
description: Muazzam miktarlarda verinin Azure’a aktarılmasını sağlayan bir hibrit çözüm olan Azure Data Box açıklanır
services: databox
documentationcenter: NA
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: overview
ms.date: 08/28/2019
ms.author: alkohli
ms.openlocfilehash: 2f1f01a8cfa25c222848e7cc5c86dc3532eba348
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437771"
---
# <a name="what-is-azure-data-box-heavy"></a>Azure Data Box Heavy nedir?

Azure Veri Kutusu Heavy, yüzlerce terabaytlık veriyi Azure'a hızlı, ucuz ve güvenilir bir şekilde göndermenize olanak tanır. Veriler, verilerinizle doldurduğunuz ve Microsoft'a geri gönderdiğiniz 1PB depolama kapasitesine sahip bir Veri Kutusu Ağır aygıtı göndererek Azure'a aktarılır. Cihaz, aktarım sırasında verilerinizi korumak ve güvence altına almak için sağlam bir kasaya sahiptir.

Once the device is received at your datacenter, set it up using the local web UI. Sunucularınızdaki verileri cihaza kopyalayın ve cihazı Azure'a geri gönderin. Azure veri merkezinde verileriniz Azure Depolama hesabınıza (lar) yüklenir. Azure portalında tüm uçuçişlem işlemini izleyebilirsiniz.


> [!IMPORTANT]
> - Bir aygıt istemek için [Azure portalına](https://portal.azure.com)kaydolun.


## <a name="use-cases"></a>Uygulama alanları

Data Box Heavy, ağ bağlantısının verileri Azure'a yüklemek için yetersiz olduğu yüzlerce terabayttaki veri boyutları için en uygun uyrmaktadır. Veri taşıma işlemi tek seferlik, düzenli veya başta toplu veri aktarımı ve ardından düzenli aktarımlardan oluşabilir. Burada Data Box Heavy'nin veri aktarımı için kullanılabileceği çeşitli senaryoları bulabilirsiniz.

 - **Tek seferlik geçiş**: Büyük miktardaki şirket içi verilerinin Azure'a taşınmasıdır.
     - Çevrimiçi medya kitaplığı oluşturmak için medya kitaplığını çevrimdışı teyplerden Azure'a taşıyın.
     - VM çiftliğinizi, SQL sunucunuzu ve uygulamalarınızı Azure'a geçirin.
     - HDInsight'ı kullanarak ayrıntılı analiz ve rapor için geçmiş verileri Azure'a taşıyın.

 - **İlk toplu aktarım**: Data Box Heavy (seed) ile toplu veri aktarımı yapıldıktan sonra ağ üzerinden artımlı aktarım işlemlerinin gerçekleştirilmesidir.
     - Örneğin, Data Box Heavy ve yedek çözümler iş ortağı, ilk büyük geçmiş yedeklemeyi Azure'a taşımak için kullanılır. İşlem tamamlandıktan sonra, artımlı veriler ağ üzerinden Azure depolamasına aktarılır.

 - **Düzenli yüklemeler**: Düzenli olarak oluşturulan büyük miktarlardaki verilerin Azure'a taşınması gerektiği durumlardır. Enerji keşfinde petrol kuyularında ve rüzgar enerjisini santrallerinde oluşturulan videolar örnek olarak verilebilir.

## <a name="benefits"></a>Avantajlar

Data Box Heavy, büyük miktarda veriyi ağınız üzerinde çok az ve hiç etkisi olmadan Azure'a taşımak için tasarlanmıştır. Çözümün şöyle avantajları vardır:

- **Hız** - Data Box Heavy yüksek performanslı 40 Gbps ağ arabirimleri kullanır.

- **Güvenlik** - Data Box Heavy, aygıt, veri ve hizmet için yerleşik güvenlik korumalarına sahiptir.
    - Cihaz, üzerinde oynanmaya karşı korumalı vidalar ve üzerinde oynandığını belli eden çıkartmalarla korunan, sağlamlaştırılmış bir cihaz kasasına sahiptir.
    - Cihazdaki veriler her zaman AES 256 bit ile şifrelenir.
    - Cihaz kilidi yalnızca Azure portalından alınan parolayla açılabilir.
    - Hizmet, Azure güvenlik özelliklerinin koruması altındadır.
    - Verileriniz Azure'a yüklendikten sonra, ulusal Standartlar ve Teknoloji Enstitüsü (NIST) 800-88r1 standartlarına uygun olarak cihazdaki diskler silinir.


## <a name="features-and-specifications"></a>Özellikler ve belirtimler

Data Box Heavy cihazı bu sürümde aşağıdaki özelliklere sahiptir.

| Belirtimler                                          | Açıklama              |
|---------------------------------------------------------|--------------------------|
| Ağırlık                                                  | Yaklaşık 500 lbs. <br>Taşıma için kilitleme tekerleklerinde cihaz|
| Boyutlar                                              | Genişlik: 26 inç Yükseklik: 28 inç Uzunluk: 48 inç |
| Raf alanı                                              | Rafa monte edilemez|
| Gerekli kablolar                                         | 4 topraklı 120 V / 10 A güç kablosu (NEMA 5-15) dahil <br> Cihaz 240 V'a kadar gücü destekler ve C-13 güç alıcılarına sahiptir <br> [Mellanox MCX314A-BCCT](https://store.mellanox.com/products/mellanox-mcx314a-bcct-connectx-3-pro-en-network-interface-card-40-56gbe-dual-port-qsfp-pcie3-0-x8-8gt-s-rohs-r6.html) ile uyumlu ağ kablolarını kullanın  |
| Üs                                                    | Cihaz düğümlerinde paylaşılan 4 dahili güç kaynağı ünitesi (PSU) <br> 1.200 watt tipik güç çekme|
| Depolama kapasitesi                                        | ~ 1-PB ham, 14 TB her 70 diskler <br> 770-TB kullanılabilir kapasite|
| Düğüm sayısı                                          | Cihaz başına 2 bağımsız düğüm (her biri 500 TB) |
| Düğüm başına ağ arabirimleri                             | Düğüm başına 4 ağ arabirimi <br><br> MGMT, DATA3 <ul><li> 2 X 1 GbE arabirimleri </li><li> MGMT yönetim ve ilk kurulum içindeğil, kullanıcı yapılandırılabilir </li><li> DATA3 varsayılan olarak kullanıcı tarafından yapılandırılabilir ve Dinamik Ana Bilgisayar Yapılandırma Protokolü (DHCP)</li></ul>DATA1, DATA2 veri arayüzleri <ul><li>2 X 40 GbE arabirimleri </li><li> DHCP (varsayılan) veya statik için kullanıcı yapılandırılabilir</li></ul>|


## <a name="components"></a>Bileşenler

Data Box Heavy aşağıdaki bileşenleri içerir:

* **Data Box Heavy cihazı**: Verileri güvenle depolayan, dış yüzeyi sağlamlaştırılmış bir fiziksel cihaz. Bu cihaz 770 TB kullanılabilir depolama kapasitesine sahiptir.
    
* **Data Box hizmeti**: Azure portalının, farklı coğrafi konumlardan erişebildiğiniz bir web arabiriminde Data Box Heavy cihazını yönetmenize olanak tanıyan bir uzantısı. Veri Kutusu Ağır cihazınızı yönetmek için Veri Kutusu hizmetini kullanın. Hizmetin görevleri, siparişleri oluşturma ve yönetmeyi, uyarıları görüntülemeyi ve yönetmeyi, paylaşımları yönetmeyi içerir.  

* **Yerel web kullanıcı arabirimi**: Yerel ağa bağlanabilmesi için cihazı yapılandırmak ve ardından Data Box hizmetine kaydetmek için kullanılan, web tabanlı kullanıcı arabirimi. Yerel web kullanıcı arabirimini cihazı kapatmak ve yeniden başlatmak, kopya günlüklerini görüntülemek ve hizmet isteğinde bulunmak üzere Microsoft Desteği'ne başvurmak için de kullanın.


## <a name="the-workflow"></a>İş akışı

Tipik iş akışı aşağıdaki adımlardan oluşur:

1. **Sipariş**: Azure portaldan bir sipariş oluşturup gönderim bilgilerini ve verileriniz için hedef Azure depolama hesabını belirtin. Cihaz varsa, Azure cihazı hazırlar, gönderir ve bir gönderi takip numarası iletir.

2. **Alma**: Cihaz teslim edildiğinde, belirtilen kabloları kullanarak cihazı ağa ve güç kaynağına kabloyla bağlayın. Cihazı açın ve bağlayın. Cihaz ağını yapılandırın ve verileri kopyalamak istediğiniz konak bilgisayarına paylaşımları takın.

3. **Veri kopyalama**: Verileri Data Box Heavy paylaşımlarına kopyalayın.

4. **İade etme**: Cihazı hazırlayın, kapatın ve Azure veri merkezine geri gönderin.

5. **Karşıya yükleme**: Veriler otomatik olarak cihazdan Azure'a kopyalanır. Cihazın diskleri Ulusal Standart ve Teknoloji Kurumu (NIST) yönergelerine uygun ve güvenli bir şekilde silinir.

Bu işlem boyunca, tüm durum değişiklikleri hakkında e-posta yoluyla bilgilendirilirsiniz.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Data Box Heavy, hizmetin dağıtıldığı bölgeye, aygıtın sevk edildiği ülkeye/bölgeye ve verileri aktardığınız hedef Azure depolama hesabına göre veri aktarımı yapabilir.

- **Hizmet kullanılabilirliği** - Bu sürümde, Data Box Heavy şu bölgelerde kullanılabilir:
    - Birleşik Devletler'deki tüm genel bulut bölgeleri: Orta Batı ABD, Batı ABD2, Batı ABD, Orta Güney ABD, Orta ABD, Orta Kuzey ABD, Doğu ABD ve Doğu ABD2.
    - Avrupa Birliği: Batı Avrupa ve Kuzey Avrupa.
    - Birleşik Krallık: Güney Birleşik Krallık ve Batı Birleşik Krallık.
    - Fransa: Orta Fransa ve Güney Fransa.

- **Hedef Depolama hesapları**: Verilerin depolandığı depolama hesapları, hizmetin kullanılabildiği tüm Azure bölgelerinde sağlanır.

Data Box Heavy için bölge kullanılabilirliği yle ilgili en güncel bilgiler [için, bölgeye göre Azure ürünlerine](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all)gidin.

## <a name="sign-up"></a>Kaydolma

Data Box Heavy'ye kaydolmak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın.](https://portal.azure.com)
2. Yeni bir kaynak oluşturmak için **+ kaynak** oluşturun' seçeneğini tıklatın. **Azure Data Box** için arama yapın. **Azure Data Box** hizmetini seçin.
3. **Oluştur'u**tıklatın.
4. Data Box Heavy için kullanmak istediğiniz aboneliği seçin. Data Box Heavy kaynağını dağıtmak istediğiniz bölgeyi seçin. **Data Box Heavy** seçeneğinde **Kaydol**'a tıklayın.
5. Veri ikamet ülke / bölge, zaman çerçevesi, hedef Azure hizmeti veri aktarım, ağ bant genişliği ve veri aktarım sıklığı ile ilgili soruları yanıtlayın. Gizlilik ve koşulları gözden geçirdikten sonra, Microsoft sizinle iletişim kurmak için e-posta adresinizi kullanabilir onay kutusunu seçin.

Kaydolduktan sonra, Bir Veri Kutusu Ağır sipariş edebilirsiniz.

    
