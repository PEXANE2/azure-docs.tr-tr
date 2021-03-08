---
title: Yaygın olarak kullanılan Azure Sentinel çalışma kitapları | Microsoft Docs
description: Popüler, yerleşik Azure Sentinel kaynaklarını kullanmak için en yaygın olarak kullanılan çalışma kitapları hakkında bilgi edinin.
services: sentinel
cloud: na
documentationcenter: na
author: batamig
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 03/07/2021
ms.author: bagol
ms.openlocfilehash: a37501498a9222025860702a7f29dccc9abfc8f7
ms.sourcegitcommit: f6193c2c6ce3b4db379c3f474fdbb40c6585553b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102450073"
---
# <a name="commonly-used-azure-sentinel-workbooks"></a>Yaygın olarak kullanılan Azure Sentinel çalışma kitapları

Aşağıdaki tabloda, en yaygın kullanılan yerleşik Azure Sentinel çalışma kitapları listelenmektedir.

Sol taraftaki **tehdit yönetimi** çalışma kitapları altında Azure Sentinel 'deki çalışma kitaplarına erişin  >   ve sonra kullanmak istediğiniz çalışma kitabını arayın. Daha fazla bilgi için bkz. [öğretici: verilerinizi görselleştirme ve izleme](tutorial-monitor-your-data.md).

|Çalışma kitabı adı  |Açıklama  |
|---------|---------|
|**Analiz verimliliği**     |  Daha iyi bir SOC performansı elde etmenize yardımcı olması için analiz kurallarınızın göre etkinliğine hakkında öngörüler sağlar. <br><br>Daha fazla bilgi için bkz. [Data-Driven SOCs araç seti](https://techcommunity.microsoft.com/t5/azure-sentinel/the-toolkit-for-data-driven-socs/ba-p/2143152).|
|**Azure etkinliği**     |     Tüm kullanıcı işlemlerini ve olaylarını çözümleyip ilişkilendirerek kuruluşunuzun Azure etkinliğine kapsamlı öngörüler sağlar. <br><br>Daha fazla bilgi için bkz. [Azure etkinlik günlükleri Ile denetleme](audit-sentinel-data.md#auditing-with-azure-activity-logs).    |
|**Azure AD denetim günlükleri**     |  Azure AD senaryolarıyla ilgili Öngörüler sağlamak için Azure Active Directory Denetim günlüklerini kullanır. <br><br>Daha fazla bilgi için bkz.  [hızlı başlangıç: Azure Sentinel ile çalışmaya başlama](quickstart-get-visibility.md).     |
|**Azure AD denetimi, etkinlik ve oturum açma günlükleri**     |   Bir çalışma kitabıyla Azure Active Directory Denetim, etkinlik ve oturum açma verileriyle ilgili öngörüler sağlar. Bu çalışma kitabı hem güvenlik hem de Azure yöneticileri tarafından kullanılabilir.      |
|**Azure AD Oturum açma günlükleri**     | Azure AD oturum açma günlüklerini kullanarak Azure AD senaryolarıyla ilgili öngörüler sağlar.        |
|**Siber güvenlik vade Model sertifikası (CMMC)**     |   Microsoft Güvenlik teklifleri, Office 365, takımlar, Intune, Windows sanal masaüstü vb. dahil olmak üzere Microsoft portföyündeki CMMC denetimlerine hizalanmış günlük sorgularını görüntülemek için bir mekanizma sağlar. <br><br>Daha fazla bilgi için bkz. [genel önizlemede Sisecurity vade modeli sertifikası (CMMC) çalışma kitabı](https://techcommunity.microsoft.com/t5/azure-sentinel/what-s-new-cybersecurity-maturity-model-certification-cmmc/ba-p/2111184).|
|**Veri toplama sistem durumu izleme**     |   Çalışma alanınızın veri alma durumu hakkında öngörüler sağlar. Çalışma alanları veri toplama sistem durumunu belirlemenize yardımcı olmak için izleyicileri görüntüleyin ve anormallikleri tespit edin.  <br><br>Daha fazla bilgi için, [Bu Azure Sentinel çalışma kitabıyla veri bağlayıcılarınızın sistem durumunu izleme](monitor-data-connector-health.md)bölümüne bakın.    |
|**Olay Çözümleyicisi**     |  Güvenlik, uygulama, sistem, kurulum, dizin hizmeti, DNS vb. gibi tüm olay ayrıntıları ve öznitelikleri dahil olmak üzere Windows olay günlüğü analizini araştırmanıza, denetlemeye ve hızlandırmanıza olanak sağlar.       |
|**Exchange Online**     |Tüm Exchange işlemlerini ve Kullanıcı etkinliklerini izleyerek ve çözümleyerek Microsoft Exchange Online ile ilgili öngörüler sağlar.         |
|**Kimlik ve Erişim**     |   Denetim ve oturum açma günlüklerini içeren güvenlik günlükleri aracılığıyla Microsoft ürün kullanımında kimlik ve erişim işlemlerine yönelik öngörüler sağlar.     |
|**Olaya genel bakış**     |   Genel bilgiler, varlık verileri, önceliklendirme süresi, risk azaltma süresi ve açıklamalar dahil olmak üzere bir olayla ilgili ayrıntılı bilgiler sunarak önceliklendirme ve araştırmaya yardımcı olmak için tasarlanmıştır. <br><br>Daha fazla bilgi için bkz. [Data-Driven SOCs araç seti](https://techcommunity.microsoft.com/t5/azure-sentinel/the-toolkit-for-data-driven-socs/ba-p/2143152).      |
|**Araştırma öngörüleri**     | Analistleri olay, yer işareti ve varlık verileri hakkında öngörüler sağlar. Yaygın sorgular ve ayrıntılı görselleştirmeler analistlerin şüpheli etkinlikleri araştırmaya yardımcı olabilir.       |
|**Microsoft Cloud App Security bulma günlükleri**     |   Kuruluşunuzda kullanılan bulut uygulamaları ve belirli kullanıcılar ve uygulamalar için kullanım eğilimleri ve detaya gitme verileri hakkında ayrıntılı bilgiler sağlar.  <br><br>Daha fazla bilgi için bkz. [Microsoft Cloud App Security verileri bağlama](connect-cloud-app-security.md).|
|**MITRE ATT&CK çalışma kitabı**     |   Azure Sentinel için MITRE ATT&CK kapsamına ilişkin ayrıntıları sağlar.      |
|**Office 365**     |  Tüm işlemleri ve etkinlikleri izleyerek ve çözümleyerek Office 365 ile ilgili öngörüler sağlar. SharePoint, OneDrive ve Exchange verilerinin detayına gidin.       |
|**Güvenlik uyarıları**     |  Azure Sentinel ortamınızda uyarılar için bir güvenlik uyarıları panosu sağlar. <br><br>Daha fazla bilgi için bkz. [Microsoft Güvenlik uyarılarından olayları otomatik olarak oluşturma](create-incidents-from-alerts.md).      |
|**Güvenlik Işlemleri verimliliği**     |  Güvenlik işlemleri Merkezi (SOC) yöneticilerine, takımlarının performansına ilişkin genel verimlilik ve ölçümler görüntüleme amaçlıdır. <br><br>Daha fazla bilgi için bkz. [olay ölçümleriyle SOC daha Iyi yönetme](manage-soc-with-incident-metrics.md).  |
|**Tehdit Bilgisi**     | Tehditler ve önem derecesi, zaman içinde tehdit etkinliği ve Office 365 ve güvenlik duvarları dahil diğer veri kaynaklarıyla bağıntı dahil tehdit göstergelerine yönelik öngörüler sağlar.  <br><br>Daha fazla bilgi için bkz. [tehdit zekasını Azure Sentinel 'e aktarma](import-threat-intelligence.md).      |
|**Çalışma alanı denetimi**     |  Çalışma alanınızdaki sorgu çalıştırmalarını anlamanıza olanak sağlayan bir çalışma alanı denetleme raporu sağlar.   <br><br>Daha fazla bilgi için bkz. [Azure Sentinel sorgularını ve etkinliklerini denetleme](audit-sentinel-data.md).  |
|     |         |

