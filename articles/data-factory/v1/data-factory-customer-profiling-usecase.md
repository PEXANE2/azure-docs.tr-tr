---
title: Kullanım Örneği - Müşteri Profilleme
description: Azure Veri Fabrikası'nın, oyun müşterilerinin profiline veri odaklı bir iş akışı (ardışık kaynak) oluşturmak için nasıl kullanıldığını öğrenin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: c570f988dea894b8106405f4e427edb386a3e74a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75969284"
---
# <a name="use-case---customer-profiling"></a>Kullanım Örneği - Müşteri Profilleme
Azure Veri Fabrikası, çözüm hızlandırıcıların Cortana İstihbarat Paketi'ni uygulamak için kullanılan birçok hizmetten biridir.  Cortana Intelligence hakkında daha fazla bilgi için [Cortana Intelligence Suite](https://www.microsoft.com/cortanaanalytics)adresini ziyaret edin. Bu belgede, Azure Veri Fabrikası'nın yaygın analiz sorunlarını nasıl çözebileceğini anlamaya başlamanıza yardımcı olacak basit bir kullanım örneğini açıklıyoruz.

## <a name="scenario"></a>Senaryo
Contoso birden fazla platform için oyun yaratan bir oyun şirketidir: oyun konsolları, el cihazları ve kişisel bilgisayarlar (bilgisayarlar). Oyuncular bu oyunları oynarken, kullanıcının kullanım kalıplarını, oyun stilini ve tercihlerini izleyen büyük hacimli günlük verileri üretilir.  Demografik, bölgesel ve ürün verileriyle birleştirildiğinde Contoso, oyuncuların deneyimlerini nasıl geliştireceklerine ve yükseltmeler ve oyun içi satın alımlar için onları nasıl hedefleyecekleri konusunda onlara rehberlik etmek için analiz ler gerçekleştirebilir. 

Contoso'nun amacı, oyuncularının oyun geçmişine dayalı up-sell/cross-sell fırsatlarını belirlemek ve iş büyümesini sağlamak ve müşterilere daha iyi bir deneyim sunmak için ilgi çekici özellikler eklemektir. Bu kullanım durumu için, bir oyun şirketini bir işletmeye örnek olarak kullanırız. Şirket, oyunlarını oyuncuların davranışlarına göre optimize etmek istiyor. Bu ilkeler, müşterilerinin mal ve hizmetleri etrafında etkileşime girmek ve müşterilerinin deneyimini geliştirmek isteyen her işletme için geçerlidir.

Bu çözümde Contoso, yakın zamanda başlattığı bir pazarlama kampanyasının etkinliğini değerlendirmek istemese de. Ham oyun günlükleriyle başlar, bunları coğrafi konum verileriyle işleyip zenginleştiririz, reklam referans verileriyle birleştirileriz ve son olarak kampanyanın etkisini analiz etmek için azure SQL Veritabanı'na kopyalarız.

## <a name="deploy-solution"></a>Çözüm Dağıt
Bu basit kullanım örneğine erişmek ve denemek için tek ihtiyacınız olan bir [Azure aboneliği,](https://azure.microsoft.com/pricing/free-trial/)bir [Azure Blob depolama hesabı](../../storage/common/storage-account-create.md)ve bir Azure SQL [Veritabanıdır.](../../sql-database/sql-database-get-started.md) Örnek **ardışık hatlar** döşemesinden müşteri profil oluşturma ardışık hattını veri fabrikanızın ana sayfasında dağıtMış sınız.

1. Bir veri fabrikası oluşturun veya varolan bir veri fabrikasını açın. Bkz. Bir veri fabrikası oluşturmak için adımlar için [Veri Fabrikası'nı kullanarak Blob Depolama'dan SQL Veritabanına verileri kopyalayın.](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
2. Veri fabrikası için **DATA FACTORY** bıçak, Örnek **boru hatları** döşemesi tıklatın.

    ![Örnek boru hatları döşemesi](./media/data-factory-samples/SamplePipelinesTile.png)
3. Örnek **ardışık hatlar** bıçak, dağıtmak istediğiniz **Müşteri profil oluşturma** tıklatın.

    ![Örnek boru hatları bıçak](./media/data-factory-samples/SampleTile.png)
4. Örnek için yapılandırma ayarlarını belirtin. Örneğin, Azure depolama hesap adınız ve anahtarınız, Azure SQL sunucu adınız, veritabanınız, Kullanıcı Kimliğiniz ve parolanız.

    ![Örnek bıçak](./media/data-factory-samples/SampleBlade.png)
5. Yapılandırma ayarlarını belirtmeyi bitirdikten sonra, örnek ardışık hatlar ve ardışık hatlar tarafından kullanılan bağlantılı hizmetleri/tabloları oluşturmak/dağıtmak için **Oluştur'u** tıklatın.
6. **Örnek ardışık hatlar** bıçak üzerinde daha önce tıkladığınız örnek döşemede dağıtım durumunu görürsünüz.

    ![Dağıtım durumu](./media/data-factory-samples/DeploymentStatus.png)
7. **Dağıtım'ın** örnek için döşemede başarılı iletisini gördüğünüzde, **Örnek ardışık hatlar bıçaklarını** kapatın.  
8. **DATA FACTORY** bıçak üzerinde, bağlantılı hizmetlerin, veri kümelerinin ve boru hatlarının veri fabrikanıza eklenmediğini görürsünüz.  

    ![Data Factory dikey penceresi](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="solution-overview"></a>Çözüme Genel Bakış
Bu basit kullanım örneği, verileri yutmak, hazırlamak, dönüştürmek, analiz etmek ve yayımlamak için Azure Veri Fabrikası'nı nasıl kullanabileceğinize bir örnek olarak kullanılabilir.

![Uçtan uca iş akışı](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

Bu Şekil, veri ardışık lıklarının dağıtıldıktan sonra Azure portalında nasıl göründüğünü gösterilmektedir.

1. **PartitionGameLogsPipeline** blob depolama dan ham oyun olayları okur ve yıl, ay ve gün dayalı bölümler oluşturur.
2. **EnrichGameLogsPipeline,** coğrafi kod referans verileriyle bölümlenmiş oyun etkinliklerine katılır ve IP adreslerini ilgili coğrafi konumlara eşleyerek verileri zenginleştirir.
3. **AnalyzeMarketingCampaignPipeline** ardışık alanı zenginleştirilmiş verileri kullanır ve pazarlama kampanyası etkinliğini içeren son çıktıyı oluşturmak için reklam verileriyle işler.

Bu örnekte, Veri Fabrikası, giriş verilerini kopyalayan, verileri dönüştüren ve işleyen ve son verileri bir Azure SQL Veritabanına aktaran etkinlikleri düzenlemek için kullanılır.  Ayrıca veri ardışık lıklar ağını görselleştirebilir, bunları yönetebilir ve durumlarını Kullanıcı UI'den izleyebilirsiniz.

## <a name="benefits"></a>Avantajlar
Oyun şirketi, kullanıcı profili analitiğini optimize ederek ve iş hedefleriyle uyumlu hale getirmekle, kullanım modellerini hızla toplayabilir ve pazarlama kampanyalarının etkinliğini analiz edebiliyor.

