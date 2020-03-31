---
title: Azure Monitör'ü kullanarak günlükleri ArcSight ile tümleştirin | Microsoft Dokümanlar
description: Azure Monitörünü kullanarak Azure Active Directory günlüklerini ArcSight ile nasıl entegre edebilirsiniz öğrenin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: b37bef0d-982e-4e28-86b2-6c61ca524ae1
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/19/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 05002c1b11ef31b61fb4036f09dc8edcdafca767
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75608389"
---
# <a name="integrate-azure-active-directory-logs-with-arcsight-using-azure-monitor"></a>Azure Monitörünü kullanarak Azure Active Directory günlüklerini ArcSight ile tümleştirin

[Micro Focus ArcSight,](https://software.microfocus.com/products/siem-security-information-event-management/overview) platformunuzdaki güvenlik tehditlerini algılamanıza ve yanıt lamanıza yardımcı olan bir güvenlik bilgileri ve olay yönetimi (SIEM) çözümüdür. Artık Azure AD için ArcSight konektörü kullanarak Azure Monitor'u kullanarak Azure Active Directory (Azure AD) günlüklerini ArcSight'a yönlendirebilirsiniz. Bu özellik, ArcSight'ı kullanarak kiracınızı güvenlik tenebize etmek için izlemenize olanak tanır.  

Bu makalede, Azure Monitor'u kullanarak Azure AD günlüklerini ArcSight'a nasıl yönlendirdiğinizi öğreneceksiniz. 

## <a name="prerequisites"></a>Ön koşullar

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:
* Azure AD etkinlik günlükleri içeren bir Azure etkinlik merkezi. [Etkinlik günlüklerinizi bir etkinlik hub'ına nasıl aktartığın](quickstart-azure-monitor-stream-logs-to-event-hub.md)öğren. 
* ArcSight Syslog NG Daemon SmartConnector (SmartConnector) veya ArcSight Yük Dengeleyici'nin yapılandırılmış bir örneği. Olaylar ArcSight Yük Dengeleyicisi'ne gönderilirse, sonuç olarak Yük Dengeleyicisi tarafından SmartConnector'a gönderilir.

[Azure Monitörü Etkinlik Hub'ı için ArcSight SmartConnector için yapılandırma kılavuzunu](https://community.microfocus.com/t5/ArcSight-Connectors/SmartConnector-for-Microsoft-Azure-Monitor-Event-Hub/ta-p/1671292)indirin ve açın. Bu kılavuz, Azure Monitör için ArcSight SmartConnector'u yüklemek ve yapılandırmak için gereken adımları içerir. 

## <a name="integrate-azure-ad-logs-with-arcsight"></a>Azure REKLAM günlüklerini ArcSight ile tümleştirme

1. İlk olarak, yapılandırma kılavuzunun **Önkoşullar** bölümündeki adımları tamamlayın. Bu bölümde aşağıdaki adımlar yer almaktadır:
    * Bağlayıcıyı dağıtacak ve yapılandıracak **sahip** rolü olan bir kullanıcı olduğundan emin olmak için kullanıcı izinlerini Azure'da ayarlayın.
    * Syslog NG Daemon SmartConnector ile sunucuda bağlantı noktalarını açın, böylece Azure'dan erişebilsin. 
    * Dağıtım bir Windows PowerShell komut dosyası çalıştırır, bu nedenle PowerShell'in konektörü dağıtmak istediğiniz makinede komut dosyalarını çalıştırmasını etkinleştirmeniz gerekir.

2. Konektörü dağıtmak için yapılandırma **kılavuzunun Bağlayıcıyı Dağıtma** bölümündeki adımları izleyin. Bu bölüm, bağlayıcıyı nasıl indirip ayıkladığınız, uygulama özelliklerini yapılandırmanız ve çıkarılan klasörden dağıtım komut dosyasını çalıştırmanız için size yol açar. 

3. Bağlayıcının ayarlanıp düzgün çalıştığından emin olmak için **Azure'da Dağıtımı Doğrulama** adımlarını kullanın. Aşağıdakileri doğrulayın:
    * Gerekli Azure işlevleri Azure aboneliğinizde oluşturulur.
    * Azure AD günlükleri doğru hedefe akışlanır. 
    * Dağıtımınızdaki uygulama ayarları Azure İşlev Uygulamaları'ndaki Uygulama Ayarları'nda devam eder. 
    * Azure'da, CeF formatında eşlenen dosyaları içeren ArcSight bağlayıcısı ve depolama hesapları için bir Azure REKLAM uygulamasıyla, ArcSight için yeni bir kaynak grubu oluşturulur.

4. Son olarak, yapılandırma kılavuzunun **Dağıtım Sonrası Yapılandırmaları'ndaki** dağıtım sonrası adımları tamamlayın. Bu bölümde, işlev uygulamalarının bir zaman aşım döneminden sonra boşta durmasını önlemek, olay merkezinden tanılama günlüklerinin akışını yapılandırmak ve SysLog NG Daemon'u güncelleştirmek için bir Uygulama Hizmet Planı'ndaysanız ek yapılandırmanasıl gerçekleştirileceğiniz açıklanmaktadır SmartConnector keystore sertifikası yeni oluşturulan depolama hesabı ile ilişkilendirmek için.

5. Yapılandırma kılavuzu, Azure'daki bağlayıcı özelliklerini nasıl özelleştireceklerini ve bağlayıcıyı nasıl yükseltip kaldırabilirsiniz imal etmeyi de açıklar. Ayrıca, etkinlik yükü tek bir Syslog NG Daemon SmartConnector'un kaldırabileceğinden daha büyükse, [Azure Tüketim planına](https://azure.microsoft.com/pricing/details/functions) yükseltme ve ArcSight Yük Dengeleyicisini yapılandırma dahil olmak üzere performans iyileştirmeleri ile ilgili bir bölüm de vardır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure MonitörÜ Etkinlik Hub'ı için ArcSight SmartConnector için yapılandırma kılavuzu](https://community.microfocus.com/t5/ArcSight-Connectors/SmartConnector-for-Microsoft-Azure-Monitor-Event-Hub/ta-p/1671292)
