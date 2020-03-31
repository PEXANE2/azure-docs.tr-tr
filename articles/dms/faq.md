---
title: SSS - Azure Veritabanı Geçiş Hizmeti
description: Veritabanı geçişlerini gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanma yla ilgili sık sorulan sorular.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: a664f12843585ac7524cf8d51aef156d15d32504
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77650990"
---
# <a name="faq-about-using-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti'ni kullanma hakkında SSS

Bu makalede, ilgili yanıtlarla birlikte Azure Veritabanı Geçiş Hizmeti'ni kullanma yla ilgili sık sorulan sorular listeleneb..

## <a name="overview"></a>Genel Bakış

**S. Azure Veritabanı Geçiş Hizmeti nedir?**
Azure Veritabanı Geçiş Hizmeti, birden çok veritabanı kaynağından Azure Veri platformlarına en az kapalı kalma süresiyle sorunsuz geçişler sağlamak üzere tasarlanmış tam olarak yönetilen bir hizmettir. Hizmet şu anda Genel Kullanılabilirlik'te dir ve devam eden geliştirme çabaları aşağıdakileri odaklanmıştır:

* Güvenilirlik ve performans.
* Kaynak hedef çiftleri yinelemeli eklenmesi.
* Sürtünmesiz göçlere sürekli yatırım.

**S. Azure Veritabanı Geçiş Hizmeti şu anda hangi kaynak/hedef çiftleri destekliyor?**
Hizmet şu anda çeşitli kaynak/hedef çiftleri veya geçiş senaryoları destekler. Kullanılabilir her geçiş senaryosunun durumunun tam bir listesi için, [Azure Veritabanı Geçiş Hizmeti tarafından desteklenen geçiş senaryolarının makale durumu'na](https://docs.microsoft.com/azure/dms/resource-scenario-status)bakın.

Diğer geçiş senaryoları önizlemededir ve DMS Preview sitesi üzerinden aday gösterilmesini gerektirir. Önizlemedeki senaryoların tam listesi ve bu tekliflerden birine katılmak için kaydolmak için [DMS Preview sitesine](https://aka.ms/dms-preview/)bakın.

**S. SQL Server'ın hangi sürümlerini destekler?**
SQL Server'dan geçiş yaparken, Azure Veritabanı Geçiş Hizmeti için desteklenen kaynaklar SQL Server 2005 ile SQL Server 2019'dır.

**S: Azure Veritabanı Geçiş Hizmeti kullanırken, çevrimdışı ve çevrimiçi geçiş arasındaki fark nedir?**
Çevrimdışı ve çevrimiçi geçişleri gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanabilirsiniz. *Çevrimdışı* geçişle, geçiş başladığında uygulama kapalı kalma süresi başlar. *Çevrimiçi* geçişte, kesinti süresi geçiş sonunda kesinti süresiyle sınırlıdır. Çalışmama süresinin kabul edilebilir olup olmadığını belirlemek için bir çevrimdışı geçişini test etmenizi öneririz.

> [!NOTE]
> Çevrimiçi geçiş gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanmak, Premium fiyatlandırma katmanını temel alan bir örnek oluşturmayı gerektirir. Daha fazla bilgi için Azure Veritabanı Geçiş Hizmeti [fiyatlandırma](https://azure.microsoft.com/pricing/details/database-migration/) sayfasına bakın.

**S. Azure Veritabanı Geçiş Hizmeti, Veritabanı Geçiş Yardımcısı (DMA) veya SQL Server Geçiş Yardımcısı (SSMA) gibi diğer Microsoft veritabanı geçiş araçlarıyla nasıl karşılaştırılır?**
Azure Veritabanı Geçiş Hizmeti, veritabanının Microsoft Azure'a ölçekte geçişi için tercih edilen yöntemdir. Azure Veritabanı Geçiş Hizmeti'nin diğer Microsoft veritabanı geçiş araçlarıyla karşılaştırılması ve çeşitli senaryolar için hizmeti kullanma yla ilgili öneriler hakkında daha fazla ayrıntı için, [Microsoft'un Veritabanı Geçiş Araçları ve Hizmetlerini Farklılatan](https://techcommunity.microsoft.com/t5/microsoft-data-migration/differentiating-microsoft-s-database-migration-tools-and/ba-p/368529)bloga bakın.

**S. Azure Veritabanı Geçiş Hizmeti, Azure Geçiş teklifiyle nasıl karşılaştırılır?**
Azure Geçir, şirket içi sanal makinelerin Azure IaaS'a geçişine yardımcı olur. Hizmet geçiş uygunluğunu ve performansa dayalı boyutlandırmayı değerlendirir ve şirket içi sanal makinelerinizi Azure'da çalıştırmak için maliyet tahminleri sağlar. Azure Geçişi, şirket içi VM tabanlı iş yüklerinin Azure IaaS VM'lerine kaldırma ve kaydırma geçişleri için yararlıdır. Ancak, Azure Veritabanı Geçiş Hizmeti'nin aksine, Azure Geçiş, Azure SQL Veritabanı veya Azure SQL Veritabanı Yönetilen Örneği gibi Azure PaaS ilişkisel veritabanı platformları için özel leştirilmiş bir veritabanı geçiş hizmeti hizmeti değildir.

## <a name="setup"></a>Kurulum

**S. Azure Veritabanı Geçiş Hizmeti'ni kullanmanın ön koşulları nelerdir?**
Veritabanı geçişlerini gerçekleştirirken Azure Veritabanı Geçiş Hizmeti'nin sorunsuz çalıştığından emin olmak için gereken birkaç ön koşul vardır. Bazı ön koşullar, hizmet tarafından desteklenen tüm senaryolar (kaynak hedef çiftleri) arasında geçerliyken, diğer ön koşullar belirli bir senaryoya özgüdür.

Desteklenen tüm geçiş senaryolarında yaygın olan Azure Veritabanı Geçiş Hizmeti ön koşulları aşağıdakileri içerir:

* [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanarak şirket içi kaynak sunucularınıza site bağlantısı sağlayan Azure Kaynak Yöneticisi dağıtım modelini kullanarak Azure Veritabanı Geçiş Hizmeti için bir Microsoft Azure Sanal Ağı oluşturun.
* Sanal ağ Ağ Güvenlik Grubu kurallarınızın aşağıdaki iletişim bağlantı noktalarını engellemediğinden emin olun 443, 53, 9354, 445, 12000. Sanal ağ NSG trafik filtreleme hakkında daha fazla ayrıntı için, [ağ güvenlik grupları ile](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)makale Filtre ağ trafiği bakın.
* Kaynak veritabanınızın (ler) önünde bir güvenlik duvarı cihazı kullanırken, Azure Veritabanı Geçiş Hizmeti'nin geçiş için kaynak veritabanına(ler) erişmesine izin vermek için güvenlik duvarı kuralları eklemeniz gerekebilir.

Azure Veritabanı Geçiş Hizmeti'ni kullanarak belirli geçiş senaryolarıyla rekabet etmek için gereken tüm ön koşulların listesi için, docs.microsoft.com'daki Azure Veritabanı Geçiş Hizmeti [belgelerinde](https://docs.microsoft.com/azure/dms/dms-overview) ilgili öğreticilere bakın.

**S. Geçiş için kaynak veritabanıma erişmek için kullanılan güvenlik duvarı kuralları için izin listesi oluşturabilmek için Azure Veritabanı Geçiş Hizmeti'nin IP adresini nasıl bulabilirim?**
Azure Veritabanı Geçiş Hizmeti'nin geçiş için kaynak veritabanınıza erişmesine izin veren güvenlik duvarı kuralları eklemeniz gerekebilir. Hizmetin IP adresi dinamiktir, ancak ExpressRoute kullanıyorsanız, bu adres şirket ağınız tarafından özel olarak atanır. Uygun IP adresini belirlemenin en kolay yolu, ilişkili Ağ Arabirimini bulmak için sağlanan Azure Veritabanı Geçiş Hizmeti kaynağınızla aynı kaynak grubuna bakmaktır. Genellikle Ağ Arabirimi kaynağının adı NIC öneki ile başlar ve ardından benzersiz bir karakter ve sayı dizisi, örneğin NIC-jj6tnztnmarpsskr82rbndyp. Bu ağ arabirimi kaynağını seçerek, kaynak genel bakışı Azure portalı sayfasında izin listesine eklenmesi gereken IP adresini görebilirsiniz.

SQL Server'ın izin verenler listesine dinlediği bağlantı noktası kaynağını da eklemeniz gerekebilir. Varsayılan olarak, port 1433, ancak kaynak SQL Server diğer bağlantı noktalarını da dinlemek için yapılandırılabilir. Bu durumda, bu bağlantı noktalarını da izin listesine eklemeniz gerekir. Dinamik Yönetim Görünümü sorgusunu kullanarak SQL Server'ın dinlediği bağlantı noktasını belirleyebilirsiniz:

```sql
    SELECT DISTINCT
        local_tcp_port
    FROM sys.dm_exec_connections
    WHERE local_tcp_port IS NOT NULL
```

SQL Server hata günlüğünü sorgulayarak SQL Server'ın dinlediği bağlantı noktasını da belirleyebilirsiniz:

```sql
    USE master
    GO
    xp_readerrorlog 0, 1, N'Server is listening on'
    GO
```

**S. Microsoft Azure Sanal Ağı'nı nasıl kuruyum?**
Sanal ağ kurma sürecinde size yol gösteren birden çok Microsoft öğreticisi olsa da, resmi belgeler [Azure Sanal Ağı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)makalesinde görünür.

## <a name="usage"></a>Kullanım

**S. Veritabanı geçişi gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti'ni kullanmak için gereken adımların özeti nedir?**
Tipik, basit bir veritabanı geçişi sırasında:

1. Hedef veritabanı(lar) oluşturun.
2. Kaynak veritabanınızı(lar) değerlendirin.
    * Homojen geçişler için, Varolan veritabanınızı(lar) [DMA](https://www.microsoft.com/download/details.aspx?id=53595)kullanarak değerlendirin.
    * Heterojen geçişler için (rekabet kaynaklarından), mevcut veritabanınızı [SSMA](https://aka.ms/get-ssma)ile değerlendirin. Veritabanı nesnelerini dönüştürmek ve şemayı hedef platformunuza geçirmek için SSMA'yı da kullanırsınız.
3. Azure Veritabanı Geçiş Hizmeti örneği oluşturun.
4. Kaynak veritabanı(lar), hedef veritabanı(lar) ve geçirilen tabloları belirten bir geçiş projesi oluşturun.
5. Tam yükü başlatın.
6. Sonraki doğrulamayı seçin.
7. Üretim ortamınızın yeni bulut tabanlı veritabanına manuel geçiş ini gerçekleştirin.

## <a name="troubleshooting-and-optimization"></a>Sorun giderme ve en iyi duruma getirilmesi

**S. DMS'de bir geçiş projesi ayarlıyorum ve kaynak veritabanıma bağlanmada zorluk yaşıyorum. Ne yapmalıyım?**
Geçiş üzerinde çalışırken kaynak veritabanı sisteminize bağlanmada sorun yaşıyorsanız, DMS örneğini ayarladığınız sanal ağda sanal bir makine oluşturun. Sanal makinede, SQL Server'a bağlantı sağlamak için UDL dosyası kullanmak veya MongoDB bağlantılarını test etmek için Robo 3T indirmek gibi bir bağlantı testi çalıştırabilmelisiniz. Bağlantı testi başarılı olursa, kaynak veritabanınıza bağlanma yla ilgili bir sorun olmamalıdır. Bağlantı testi başarılı olmazsa, ağ yöneticinize başvurun.

**S. Azure Veritabanı Geçiş Hizmetim neden kullanılamıyor veya durduruldu?**
Kullanıcı Azure Veritabanı Geçiş Hizmeti'ni (DMS) açıkça durdurursa veya hizmet 24 saatlik bir süre için etkin değilse, hizmet durdurulmuş veya otomatik duraklatılmış durumda olacaktır. Her durumda, hizmet kullanılamıyor ve durdurulmuş durumda olacaktır.  Etkin geçişleri devam ettirmek için hizmeti yeniden başlatın.

**S. Azure Veritabanı Geçiş Hizmeti'nin performansını optimize etmek için herhangi bir öneri var mı?**
Hizmeti kullanarak veritabanı geçişinizi hızlandırmak için birkaç şey yapabilirsiniz:

* Hizmet örneğini oluştururken çoklu CPU Genel Amaçlı Fiyatlandırma Katmanı'nı kullanarak hizmetin paralelleştirme ve daha hızlı veri aktarımı için birden fazla vCPUs'tan yararlanmasını bekleyin.
* Alt düzey SKU'lar kullanırken veri aktarım etkinliklerini etkileyebilecek Azure SQL Veritabanı azaltmayı en aza indirmek için veri geçişi işlemi sırasında Azure SQL Veritabanı hedef örneğini Geçici olarak Premium katman SKU olarak ölçeklendirin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Veritabanı Geçiş Hizmeti'ne ve bölgesel kullanılabilirlik egenel bakış için, [Azure Veritabanı Geçiş Hizmeti nedir makalesine](dms-overview.md)bakın.
