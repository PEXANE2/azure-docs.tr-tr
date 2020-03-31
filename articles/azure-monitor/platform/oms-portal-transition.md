---
title: Azure'a taşınan OMS portalı | Microsoft Dokümanlar
description: OMS portalı, azure portalına taşınan tüm işlevlerle gün batımına bağlanıyor. Bu makalede, bu geçiş ayrıntıları sağlar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/22/2019
ms.openlocfilehash: 680cd9b44cc447f9bdea38cb9d04fc661fba9c79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77659263"
---
# <a name="oms-portal-moving-to-azure"></a>Azure'a taşınan OMS portalı

> [!NOTE]
> Bu makale, aksi belirtilmedikçe hem Azure genel bulutu hem de devlet bulutu için geçerlidir.

**Azure genel bulutu için OMS portalı resmi olarak kullanımdan kaldırıldı. Azure ABD Devlet bulutu için OMS portalı 15 Mayıs 2019'da resmen kullanımdan kaldırıldı.** Azure portalına geçmekten heyecan duyuyoruz ve geçişin kolay olmasını bekliyoruz. Ama değişikliklerin zor olduğunu ve yıkıcı olabileceğini anlıyoruz. Bu makalenin geri kalanı, anahtar senaryolar ve bu geçiş için yol haritası üzerinden gider.

Azure portalı tüm Azure hizmetlerinin merkezidir ve kaynakları sabitleme panelleri, kaynak bulmak için akıllı arama ve kaynak yönetimi için etiketleme gibi özelliklerle zengin bir yönetim deneyimi sunar. İzleme ve yönetim iş akışını birleştirmek ve kolaylaştırmak için Azure portalına OMS portal özelliklerini eklemeye başladık. OMS portalının tüm özellikleri artık Azure portalının bir parçasıdır. Aslında, Traffic Analytics gibi bazı yeni özellikler yalnızca Azure portalında kullanılabilir. Azure portalı ve daha fazlası ile OMS portalında yaptığınız her şeyi gerçekleştirebileceksiniz. Henüz yapmadıysanız, Azure portalını bugün kullanmaya başlamalısınız!

## <a name="what-is-changing"></a>Ne değişiyor? 
Aşağıdaki değişiklikler OMS portalının amortismanı ile duyurulmaktadır. Bu değişikliklerin her biri aşağıdaki bölümlerde daha ayrıntılı olarak açıklanmıştır.

- Yalnızca Azure portalında yeni [çalışma alanları](#new-workspaces) oluşturabilirsiniz.
- Yeni uyarı yönetimi [deneyimi, Uyarı Yönetimi çözümleminin yerini alır.](#changes-to-alerts)
- [Kullanıcı erişim yönetimi](#user-access-and-role-migration) artık Azure rolü tabanlı erişim denetimi kullanılarak Azure portalında yapılır.
- Aynı işlevsellik çalışma alanı arası sorgular aracılığıyla etkinleştirildiğinden, [Uygulama Öngörüleri Bağlayıcısı artık gerekli değildir.](#application-insights-connector-and-solution)
- [OMS Mobil Uygulaması](#oms-mobile-app) küçümsüyor. 
- [NSG çözümü,](#azure-network-security-group-analytics) Traffic Analytics çözümü aracılığıyla kullanılabilen gelişmiş işlevsellikle değiştiriliyor.
- System Center Operations Manager'dan Log Analytics'e yeni bağlantılar [için güncelleştirilmiş yönetim paketleri](#system-center-operations-manager)gerekir.
- [Bkz. OMS](../../automation/automation-update-management.md) [Update Dağıtımlarınızı Azure'a geçirin,](../../automation/migrate-oms-update-deployments.md) Güncelleştirme Yönetimi'ndeki değişikliklerle ilgili ayrıntılar için bkz.


## <a name="what-should-i-do-now"></a>Şimdi ne yapmalıyım?
Çoğu özellik geçiş gerçekleştirmeden çalışmaya devam edecek olsa da, aşağıdaki görevleri gerçekleştirmeniz gerekir:

- [Kullanıcı izinlerinizi](#user-access-and-role-migration) Azure portalına geçirmeniz gerekir.
- Bkz. Güncelleştirme Yönetimi çözümüne geçiş hakkında ayrıntılı bilgi için [OMS Update Dağıtımlarınızı Azure'a geçirin.](../../automation/migrate-oms-update-deployments.md)

Azure portalına nasıl geçiş yapılacağını öğrenmek [için Log Analytics kullanıcıları için OMS portalından Azure portalına geçiş](oms-portal-faq.md) için sık sorulan sorulara bakın. 

## <a name="user-access-and-role-migration"></a>Kullanıcı erişimi ve rol geçişi
Azure portalı erişim yönetimi, OMS Portalı'ndaki erişim yönetiminden daha zengin ve daha güçlüdür. Bkz. Log Analytics'te erişim yönetimi ayrıntıları için [Azure Monitör Günlükleri çalışma alanınızı tasarlama.](design-logs-deployment.md)

> [!NOTE]
> Bu makalenin önceki sürümlerinde, izinlerin otomatik olarak OMS portalından Azure portalına dönüştürüleceği belirtilmiştir. Bu otomatik dönüştürme artık planlanmaz ve dönüşümü kendiniz gerçekleştirmeniz gerekir.

Azure portalında zaten uygun erişime sahip olabilirsiniz, bu durumda herhangi bir değişiklik yapmanız gerekmez. Uygun erişime sahip olmayabileceğiniz birkaç durum vardır ve bu durumda yöneticinizizin size izin ler ataması gerekir.

- OMS portalında ReadOnly Kullanıcı izinleri vardır, ancak Azure portalında izin yoktur. 
- OMS portalında Katılımcı izinleri vardır, ancak Azure portalında yalnızca Reader erişimi vardır.
 
Bu durumların her ikisinde de yöneticinizin aşağıdaki tablodan size uygun rolü el ile ataması gerekir. Bu rolü kaynak grubuna veya abonelik düzeyinde atamanızı öneririz.  Bu iki durum için de kısa süre içinde daha açıklayıcı rehberlik sağlanacaktır.

| OMS portalı izni | Azure Rolü |
|:---|:---|
| ReadOnly | Log Analytics Okuyucusu |
| Katılımcı | Log Analytics Katkıda Bulunan |
| Yönetici | Sahip | 
 

## <a name="new-workspaces"></a>Yeni çalışma alanları
Artık OMS portalını kullanarak yeni çalışma alanları oluşturamazsınız. Azure portalında yeni bir çalışma alanı oluşturmak için [Azure portalında Günlük Analizi oluştur çalışma alanı oluşturma](../learn/quick-create-workspace.md) kılavuzunu izleyin.

## <a name="changes-to-alerts"></a>Uyarılarda yapılan değişiklikler

### <a name="alert-extension"></a>Uyarı uzantısı  

Uyarılar Azure [portalına genişletildi](alerts-extend.md) Mevcut uyarılar OMS portalında listelenmeye devam eder, ancak bunları yalnızca Azure portalında yönetebilirsiniz. Günlük Analizi Uyarısı REST API veya Log Analytics Alert Resource Template'i kullanarak uyarılara programlı olarak erişiyorsanız, API aramalarınızdaki eylemler, Azure Kaynak Yöneticisi şablonları ve PowerShell komutları yerine eylem grupları kullanmanız gerekir.

### <a name="alert-management-solution"></a>Uyarı yönetimi çözümü
Önceki duyurudan bir değişiklik olarak, [Uyarı yönetimi çözümü](alert-management-solution.md) Azure portalında kullanılabilir ve tam olarak desteklenmeye devam edecektir. Çözümü Azure Marketi'nden yüklemeye devam edebilirsiniz.

Uyarı yönetimi çözümü kullanılabilir durumda olmaya devam ederken, Azure'daki tüm uyarıları görselleştirmek ve yönetmek için [Azure Monitor'un birleşik uyarı arabirimini](alerts-overview.md) kullanmanızı öneririz. Bu yeni deneyim, Log Analytics'ten gelen günlük uyarıları da dahil olmak üzere Azure'daki birden çok kaynaktan gelen uyarıları doğal olarak toplar. Azure Monitor'un birleşik uyarı arabirimini kullanıyorsanız, Uyarı yönetimi çözümü yalnızca Sistem Merkezi İşlem Yöneticisi'nden Azure'a uyarıların entegrasyonunu etkinleştirmek için gereklidir. Azure Monitor'un birleşik uyarı arabiriminde, uyarılarınızın dağıtımlarını görebilir, akıllı gruplar aracılığıyla ilgili uyarıların otomatik gruplandırması ndan yararlanabilir ve zengin filtreler uygularken uyarıları birden çok abonelikte görüntüleyebilirsiniz. Uyarı yönetiminde gelecekteki gelişmeler öncelikle bu yeni deneyimden elde edilecektir. 

Çözüm çalışma alanı için yüklendiğinde, Uyarı yönetimi çözümü tarafından toplanan veriler (bir uyarı türüne sahip kayıtlar) Log Analytics'te yer almaya devam eder. 

## <a name="oms-mobile-app"></a>OMS Mobil Uygulaması
OMS mobil uygulaması, OMS portalı ile birlikte gün batımına bağlanacaktır. BT altyapınız, panolarınız ve kayıtlı sorgularınız hakkındaki bilgilere erişmek için OMS mobil uygulaması yerine, Azure portalına doğrudan mobil cihazınızdaki tarayıcınızdan erişebilirsiniz. Uyarı almak için [Azure Eylem Gruplarını](action-groups.md) SMS veya sesli arama şeklinde bildirim alacak şekilde yapılandırmanız gerekir

## <a name="application-insights-connector-and-solution"></a>Uygulama Öngörüleri Konektörü ve çözümü
[Application Insights Connector,](app-insights-connector.md) Uygulama Öngörüleri verilerini Log Analytics çalışma alanına eklemenin bir yolunu sağlar. Bu veri çoğaltma altyapı ve uygulama verileri arasında görünürlük sağlamak için gerekli oldu. Application Insights Mart 2019'da veri saklama desteğini genişletti ve birden çok Azure Monitor [Application Insights kaynağını görüntüleyebilmenizin](../log-query/unify-app-resource-data.md)yanı sıra [kaynaklar arası sorgular](../log-query/cross-workspace-query.md) gerçekleştirebilme özelliği sayesinde, Application Insights kaynaklarınızdaki verileri çoğaltmaya ve Log Analytics'e göndermeye gerek yoktur. Ayrıca, Bağlayıcı uygulama özelliklerinin bir alt kümesini Log Analytics'e gönderirken, çapraz kaynak sorguları size gelişmiş esneklik sağlar.  

Bu nedenle, Application Insights Connector 30 Mart 2019'da OMS portalı amortismanı ile birlikte Azure Marketi'nden küçümsüldü ve kaldırıldı. Mevcut bağlantılar 30 Haziran 2019'a kadar çalışmaya devam edecektir. OMS portalı amortismanı ile, varolan bağlantıları yapılandırmanın ve portaldan kaldırmanın bir yolu yoktur. Bu, Ocak 2019'da kullanıma sunulacak OLAN REST API'si kullanılarak desteklenecek ve [Azure güncelleştirmelerinde](https://azure.microsoft.com/updates/)bir bildirim yayınlanacaktır. 

## <a name="azure-network-security-group-analytics"></a>Azure Ağ Güvenliği Grubu Analizi
[Azure Ağ Güvenliği Grubu Analytics çözümü,](../insights/azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-azure-monitor) bulut ağlarında kullanıcı ve uygulama etkinliklerinde görünürlük sağlayan yakın zamanda başlatılan Traffic [Analytics](https://azure.microsoft.com/blog/traffic-analytics-in-preview/) ile değiştirilecektir. Trafik Analitiği, kuruluşunuzun ağ etkinliğini denetlemenize, uygulamaları ve verileri korumanıza, iş yükü performansını optimize eleştirmenize ve uyumlu kalmanıza yardımcı olur. 

Bu çözüm NSG Akış günlüklerini analiz eder ve aşağıdakilere ilişkin öngörüler sağlar.

- Azure ve Internet, genel bulut bölgeleri, VNET'ler ve alt ağlar arasında ağlarınız üzerinden trafik akışı.
- Algılayıcılara veya özel akış toplama cihazlarına gerek kalmadan ağınızdaki uygulamalar ve protokoller.
- En çok konuşanlar, geveze uygulamalar, buluttaki VM konuşmaları, trafik noktaları.
- VNET'ler arasındaki trafiğin kaynakları ve varış noktaları, kritik iş hizmetleri ve uygulamalar arasındaki ilişkiler.
- Kötü amaçlı trafik, Internet'e açık bağlantı noktaları, Internet erişimine çalışan uygulamalar veya VM'ler gibi güvenlik.
- Kapasite kullanımı, aşırı sağlama veya yetersiz kullanım sorunlarını ortadan kaldırmanıza yardımcı olur.

NSG günlüklerini Log Analytics'e göndermek için Tanılama Ayarları'na güvenmeye devam edebilirsiniz, böylece mevcut kayıtlı aramalarınız, uyarılarınız, panolarınız çalışmaya devam eder. Çözümü zaten yüklemiş olan müşteriler, bir sonraki duyuruya kadar bu çözümü kullanmaya devam edebilirler. 5 Eylül'den itibaren, Network Security Group Analytics çözümü pazardan kaldırılacak ve topluluk aracılığıyla [Azure QuickStart Şablonu](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights)olarak kullanıma sunulacaktır.

## <a name="system-center-operations-manager"></a>System Center Operations Manager
Operasyon Yöneticisi [yönetim grubunuzu Log Analytics'e bağladıysanız,](om-agents.md)hiçbir değişiklik olmadan çalışmaya devam eder. Ancak yeni bağlantılar için, [Operations Management Suite'i yapılandırmak için Microsoft System Center Operations Manager Management Pack'teki](https://blogs.technet.microsoft.com/momteam/2018/07/25/microsoft-system-center-operations-manager-management-pack-to-configure-operations-management-suite/)kılavuzu izlemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- OMS portalından Azure portalına geçiş konusunda rehberlik yapmak [için Log Analytics kullanıcıları için OMS portalından Azure portalına geçiş için sık sorulan sorulara](oms-portal-faq.md) bakın.
