---
title: Öğretici-Azure Olay Hub 'ına günlük Azure Active Directory akışı | Microsoft Docs
description: Azure Active Directory günlüklerini bir olay hub 'ına göndermek için Azure Tanılama ayarlamayı öğrenin
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
editor: ''
ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 93dd7b13ca9e1a3f078994e76610c45447cfa41c
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72264081"
---
# <a name="tutorial-stream-azure-active-directory-logs-to-an-azure-event-hub"></a>Öğretici: Azure Olay Hub 'ına akış Azure Active Directory günlükleri

Bu öğreticide, Azure Olay Hub 'ına Azure Active Directory (Azure AD) günlüklerini akışa almak için Azure Izleyici tanılama ayarlarını ayarlamayı öğreneceksiniz. Günlüklerinizi splunk ve QRadar gibi üçüncü taraf güvenlik bilgileri ve olay yönetimi (SıEM) araçlarıyla bütünleştirmek için bu mekanizmayı kullanın.

## <a name="prerequisites"></a>Prerequisites 

Bu özelliği kullanmak için şunlar gerekir:

* Bir Azure aboneliği. Azure aboneliğiniz yoksa, [ücretsiz deneme için kaydolabilirsiniz](https://azure.microsoft.com/free/).
* Bir Azure AD kiracısı.
* Azure AD kiracısı için *genel yönetici* veya *Güvenlik Yöneticisi* olan bir kullanıcı.
* Azure aboneliğinizdeki bir Event Hubs ad alanı ve bir olay hub 'ı. [Bir olay hub 'ı oluşturmayı](https://docs.microsoft.com/azure/event-hubs/event-hubs-create)öğrenin.

## <a name="stream-logs-to-an-event-hub"></a>Günlükleri bir olay hub 'ına akış

1. [Azure Portal](https://portal.azure.com)oturum açın. 

2. **Azure Active Directory** > **izleme** > **Denetim günlüklerini**seçin. 

3. **Dışarı aktarma ayarları**' nı seçin.  
    
4. **Tanılama ayarları** bölmesinde, aşağıdakilerden birini yapın:
    * Var olan ayarları değiştirmek için **ayarı Düzenle**' yi seçin.
    * Yeni ayarlar eklemek için **Tanılama ayarı Ekle**' yi seçin.  
      En fazla üç ayar olabilir.

      ![Ayarları dışarı aktar](./media/quickstart-azure-monitor-stream-logs-to-event-hub/ExportSettings.png)

5. **Bir olay hub 'ına akış** onay kutusunu seçin ve **Olay Hub 'ı/Yapılandır**' ı seçin.

6. Günlükleri dolaştırmak istediğiniz Azure aboneliğini ve Event Hubs ad alanını seçin.  
    Abonelik ve Event Hubs ad alanı, günlük akışının içinden her ikisi de Azure AD kiracısı ile ilişkilendirilmelidir. Ayrıca, günlüklerin gönderilmesi gereken Event Hubs ad alanı içinde bir olay hub 'ı da belirtebilirsiniz. Herhangi bir olay hub 'ı belirtilmemişse, ad alanında **Öngörüler-logs-Audit**adlı varsayılan ad ile bir olay hub 'ı oluşturulur.

7. Olay Hub 'ı yapılandırmasından çıkmak için **Tamam ' ı** seçin.

8. Aşağıdakilerden birini veya her ikisini de yapın:
    * Denetim günlüklerini depolama hesabına göndermek için **auditlogs** onay kutusunu seçin. 
    * Depolama hesabına oturum açma günlükleri göndermek için **Signınlogs** onay kutusunu seçin.

9. Ayarları kaydetmek için **Kaydet** ' i seçin.

    ![Tanılama ayarları](./media/quickstart-azure-monitor-stream-logs-to-event-hub/DiagnosticSettings.png)

10. Yaklaşık 15 dakika sonra Olay Hub 'ınızdaki olayların görüntülendiğini doğrulayın. Bunu yapmak için, portaldan Olay Hub 'ına gidin ve **gelen ileti** sayısının sıfırdan büyük olduğunu doğrulayın. 

    ![Denetim günlükleri](./media/quickstart-azure-monitor-stream-logs-to-event-hub/InsightsLogsAudit.png)

## <a name="access-data-from-your-event-hub"></a>Olay Hub 'ınızdan verilere erişin

Olay Hub 'ında veriler görüntülendikten sonra, verileri iki şekilde erişebilir ve okuyabilirsiniz:

* **Desteklenen BIR SıEM aracı yapılandırın**. Olay Hub 'ından veri okumak için çoğu araç, Olay Hub 'ı bağlantı dizesi ve Azure aboneliğiniz için belirli izinler gerektirir. Azure Izleyici tümleştirmesiyle üçüncü taraf araçlar şunları içerir, ancak bunlarla sınırlı değildir:
    
    * **ARCTIS**: Azure AD günlüklerini splunk ile tümleştirme hakkında daha fazla bilgi için bkz. [Azure izleyici kullanarak arctımla Azure Active Directory günlüklerini tümleştirme](howto-integrate-activity-logs-with-arcsight.md).
    
    * **Splunk**: Azure AD günlüklerini splunk ile tümleştirme hakkında daha fazla bilgi için bkz. [Azure IZLEYICI kullanarak Azure AD günlüklerini splunk ile tümleştirme](tutorial-integrate-activity-logs-with-splunk.md).
    
    * **IBM QRadar**: DSM ve Azure Olay Hub 'ı Protokolü [IBM desteği](https://www.ibm.com/support)' nde indirilebilir. Azure ile tümleştirme hakkında daha fazla bilgi için [IBM QRadar güvenlik zekası platformu 7.3.0](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0) sitesine gidin.
    
    * **Sumo Logic**: bir olay hub 'ından veri tüketmek üzere Sumo mantığını ayarlamak için bkz. [Azure AD uygulamasını yüklemek ve panoları görüntülemek](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards). 

* **Özel araç ayarlama**. Geçerli SıEM 'niz Azure Izleyici tanılamasında henüz desteklenmiyorsa, Event Hubs API 'sini kullanarak özel araç ayarlayabilirsiniz. Daha fazla bilgi edinmek için bkz. [bir olay hub 'ından iletileri almaya](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph)başlama.


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Izleyici kullanarak Azure Active Directory günlüklerini Arcizle tümleştirin](howto-integrate-activity-logs-with-arcsight.md)
* [Azure Izleyici 'yi kullanarak Azure AD günlüklerini splunk ile tümleştirme](tutorial-integrate-activity-logs-with-splunk.md)
* [Azure Izleyici 'yi kullanarak Azure AD günlüklerini SumoLogic ile tümleştirme](howto-integrate-activity-logs-with-sumologic.md)
* [Azure Izleyici 'de denetim günlükleri şemasını yorumlama](reference-azure-monitor-audit-log-schema.md)
* [Azure Izleyici 'de oturum açma günlüğü şemasını yorumlama](reference-azure-monitor-sign-ins-log-schema.md)
