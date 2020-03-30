---
title: Azure Monitör'de Aracı Durumu çözümü | Microsoft Dokümanlar
description: Bu makale, doğrudan Log Analytics veya System Center Operations Manager'a rapor eden aracılarınızın durumunu izlemek için bu çözümü nasıl kullanacağınızı anlamanıza yardımcı olmak amacıyla tasarlanmıştır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/06/2020
ms.openlocfilehash: 7093e20473b799a3f05ddf30803721636732241e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663272"
---
#  <a name="agent-health-solution-in-azure-monitor"></a>Azure Monitör'de Aracı Durumu çözümü
Azure'daki Aracı Durumu çözümü, Azure Monitor'daki Log Analytics çalışma alanına veya Azure Monitor'a bağlı bir Sistem Merkezi Operasyon Yöneticisi yönetim grubuna doğrudan rapor veren tüm aracıların yanıt vermeyen ve operasyonel veri gönderme.  Ayrıca, kaç aracının dağıtıldığını, bunların coğrafi olarak nerelere dağıtıldığını da izleyebilir ve Azure’da, diğer bulut ortamlarında ya da şirket içinde dağıtılmış aracıların dağılımından her zaman haberdar olmaya yönelik diğer sorguları gerçekleştirebilirsiniz.    

## <a name="prerequisites"></a>Ön koşullar
Bu çözümü dağıtmadan önce, Şu anda Log Analytics çalışma alanına rapor bildiren veya çalışma alanınızın tümleşik bir [Operasyon Yöneticisi yönetim grubuna](../../azure-monitor/platform/om-agents.md) raporlama [yaptığınızWindows aracılarını](../../log-analytics/log-analytics-windows-agent.md) desteklediğinizi doğrulayın.

## <a name="solution-components"></a>Çözüm bileşenleri
Bu çözüm, çalışma alanınıza eklenen aşağıdaki kaynaklardan ve doğrudan bağlanılan aracılardan veya Operations Manager bağlantılı yönetim grubundan oluşur.

### <a name="management-packs"></a>Yönetim paketleri
Sistem Merkezi Operasyon Yöneticisi yönetim grubunuz bir Log Analytics çalışma alanına bağlıysa, Aşağıdaki yönetim paketleri Operations Manager'a yüklenir.  Bu çözüm eklendikten sonra bu yönetim paketleri doğrudan bağlı Windows bilgisayarlarına da yüklenir. Bu yönetim paketlerinde yapılandırılacak veya yönetilecek hiçbir şey yoktur.

* Microsoft System Center Advisor HealthAssessment Direct Channel Intelligence Pack  (Microsoft.IntelligencePacks.HealthAssessmentDirect)
* Microsoft System Center Advisor HealthAssessment Server Channel Intelligence Pack (Microsoft.IntelligencePacks.HealthAssessmentViaServer).  

Çözüm yönetim paketlerini güncelleştirme hakkında daha fazla bilgi için bkz. [Operations Manager'ı Log Analytics’e Bağlama](../../azure-monitor/platform/om-agents.md).

## <a name="configuration"></a>Yapılandırma
[Ekle çözümlerinde](solutions.md)açıklanan işlemi kullanarak Günlük Analizi çalışma alanınıza Aracı Durumu çözümlerini ekleyin. Başka bir yapılandırma işlemi gerekmez.


## <a name="data-collection"></a>Veri toplama
### <a name="supported-agents"></a>Desteklenen aracılar
Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır.

| Bağlı Kaynak | Destekleniyor | Açıklama |
| --- | --- | --- |
| Windows aracıları | Evet | Sinyal olayları doğrudan Windows aracılarından toplanır.|
| System Center Operations Manager yönetim grubu | Evet | Kalp atışı olayları, her 60 saniyede bir yönetim grubuna rapor eden aracılardan toplanır ve ardından Azure Monitor'a iletilir. Operations Manager aracılarından Azure Monitor'a doğrudan bağlantı gerekmez. Sinyal olay verileri yönetim grubundan Log Analytics çalışma alanına iletilir.|

## <a name="using-the-solution"></a>Çözümü kullanma
Çözümü Log Analytics çalışma alanınıza eklediğinizde, **Aracı Durumu** döşemesi panonuza eklenir. Bu kutucuk, son 24 saat içindeki toplam aracı sayısını ve yanıt vermeyen aracı sayısını gösterir.<br><br> ![Panodaki Aracı Durumu Çözüm kutucuğu](./media/solution-agenthealth/agenthealth-solution-tile-homepage.png)

**Aracı Durumu** kutucuğuna tıklayarak **Aracı Durumu** panosunu açın.  Pano aşağıdaki tabloda gösterilen sütunları içerir. Her sütunda, ilgili sütunun belirtilen zaman aralığına ilişkin ölçütlerle eşleşen ilk on olay sayılarına göre listelenir. Her sütunun sağ alt tarafındaki **Tümünü görüntüle**’yi seçerek ya da sütun başlığına tıklayarak listenin tamamını sağlayan bir günlük araması çalıştırabilirsiniz.

| Sütun | Açıklama |
|--------|-------------|
| Zaman içinde aracı sayısı | Hem Linux hem de Windows aracıları için yedi günlük bir dönem boyunca aracı sayınızın eğilimi.|
| Yanıt vermeyen aracı sayısı | Son 24 saat içinde bir sinyal göndermemiş tüm aracıların listesi.|
| İşletim Sistemi Türüne Göre Dağılım | Ortamınızda kaçar tane Windows ve Linux aracısı olduğuna ilişkin bir bölüm.|
| Aracı Sürümüne Göre Dağılım | Ortamınızda yüklü olan farklı aracı sürümlerine ve her birinin sayısına ilişkin bir bölüm.|
| Aracı Kategorisine Göre Dağılım | Sinyal olayları gönderen farklı kategorilerdeki araçlara ilişkin bir bölüm: Doğrudan aracılar, OpsMgr aracıları veya OpsMgr Management Server.|
| Yönetim Grubuna Göre Dağılım | Ortamınızdaki farklı Operasyon Yöneticisi Yönetimi gruplarının bir bölümü.|
| Aracıların coğrafi konumu | Aracıların bulunduğu farklı ülkelerin/bölgelerin bölünmesi ve her ülkede/bölgede yüklenen aracı sayısının toplam sayısı.|
| Yüklü Ağ Geçidi Sayısı | Log Analytics ağ geçidiyüklü sunucu sayısı ve bu sunucuların listesi.|

![Aracı Durumu Çözüm panosu örneği](./media/solution-agenthealth/agenthealth-solution-dashboard.png)  

## <a name="azure-monitor-log-records"></a>Azure Monitör günlük kayıtları
Çözüm, Log Analytics çalışma alanında tek bir kayıt türü oluşturur.  

### <a name="heartbeat-records"></a>Sinyal kayıtları
**Sinyal** türünde bir kayıt oluşturulur.  Bu kayıtlar aşağıdaki tabloda gösterilen özelliklere sahiptir.  

| Özellik | Açıklama |
| --- | --- |
| `Type` | *Sinyal*|
| `Category` | Değer *Doğrudan Aracı*, *SCOM Aracısı* veya *SCOM Yönetim Sunucusu*’dur.|
| `Computer` | Bilgisayar adı.|
| `OSType` | Windows veya Linux işletim sistemi.|
| `OSMajorVersion` | İşletim sistemi ana sürümü.|
| `OSMinorVersion` | İşletim sistemi alt sürümü.|
| `Version` | Günlük Analytics Agent veya Operations Manager Agent sürümü.|
| `SCAgentChannel` | Değer *Doğrudan* ve/veya *SCManagementServer*’dır.|
| `IsGatewayInstalled` | Log Analytics ağ geçidi yüklüyse, değer *doğrudur,* aksi takdirde değer *yanlıştır.*|
| `ComputerIP` | Bilgisayarın genel IP adresi. Azure VM'lerde bu, varsa genel IP'yi gösterir. Özel IP'ler kullanan VM'ler için bu, Azure SNAT adresini (özel IP adresi değil) görüntüler. |
| `RemoteIPCountry` | Bilgisayarın dağıtıldığı coğrafi konum.|
| `ManagementGroupName` | Operations Manager yönetim grubunun adı.|
| `SourceComputerId` | Bilgisayarın benzersiz kimliği.|
| `RemoteIPLongitude` | Bilgisayarın coğrafi konumunun boylamı.|
| `RemoteIPLatitude` | Bilgisayarın coğrafi konumunun enlemi.|

Bir Operations Manager yönetim sunucusuna rapor veren her aracı iki sinyal gönderir ve SCAgentChannel özelliğinin değeri, aboneliğinizde etkinleştirdiğiniz veri kaynaklarına ve izleme çözümlerine bağlı olarak hem **Direct** hem de **SCManagementServer'ı** içerir. Hatırlarsanız, çözümlerden elde edilen veriler doğrudan bir Operations Manager yönetim sunucusundan Azure Monitor'a gönderilir veya aracıda toplanan veri hacmi nedeniyle doğrudan aracıdan Azure Monitor'a gönderilir. **SCManagementServer** değerine sahip sinyal olayları için ComputerIP değeri, verileri aslında karşıya yükleyen yönetim sunucusunun IP adresidir.  SCAgentChannel’ın **Doğrudan** olarak ayarlandığı sinyaller için bu adres, aracının genel IP adresidir.  

## <a name="sample-log-searches"></a>Örnek günlük aramaları
Aşağıdaki tabloda, bu çözüm tarafından toplanan kayıtlara ilişkin örnek günlük aramaları sunulmaktadır.

| Sorgu | Açıklama |
|:---|:---|
| Heartbeat &#124; distinct Computer |Toplam aracı sayısı |
| Heartbeat &#124; summarize LastCall = max(TimeGenerated) by Computer &#124; where LastCall < ago(24h) |Son 24 saat içinde yanıt vermeyen aracı sayısı |
| Heartbeat &#124; summarize LastCall = max(TimeGenerated) by Computer &#124; where LastCall < ago(15m) |Son 15 dakika içinde yanıt vermeyen aracı sayısı |
| Heartbeat &#124; where TimeGenerated > ago(24h) and Computer in ((Heartbeat &#124; where TimeGenerated > ago(24h) &#124; distinct Computer)) &#124; summarize LastCall = max(TimeGenerated) by Computer |Çevrimiçi olan bilgisayarlar (son 24 saat) |
| Heartbeat &#124; where TimeGenerated > ago(24h) and Computer !in ((Heartbeat &#124; where TimeGenerated > ago(30m) &#124; distinct Computer)) &#124; summarize LastCall = max(TimeGenerated) by Computer |Son 30 dakika içinde Toplam Çevrimdışı Aracı Sayısı (son 24 saat için) |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by OSType |İşletim sistemi türüne göre zaman içinde aracı sayısı eğilimini görün|
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by OSType |İşletim Sistemi Türüne Göre Dağılım |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by Version |Aracı Sürümüne Göre Dağılım |
| Heartbeat &#124; summarize AggregatedValue = count() by Category |Aracı Kategorisine Göre Dağılım |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by ManagementGroupName | Yönetim Grubuna Göre Dağılım |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by RemoteIPCountry |Aracıların coğrafi konumu |
| Heartbeat &#124; where iff(isnotnull(toint(IsGatewayInstalled)), IsGatewayInstalled == true, IsGatewayInstalled == "true") == true &#124; distinct Computer |Yüklenen Günlük Analitiği Ağ Geçitlerinin Sayısı |




## <a name="next-steps"></a>Sonraki adımlar

* Günlük sorgularından uyarı oluşturma yla ilgili ayrıntılar için [Azure Monitor'daki Uyarılar](../platform/alerts-overview.md) hakkında bilgi edinin. 
