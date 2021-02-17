---
title: Salesforce hizmeti bulut verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Salesforce günlüklerini Azure Sentinel 'e çekmek için Salesforce hizmeti bulutu veri bağlayıcısını nasıl kullanacağınızı öğrenin. Çalışma kitaplarında Salesforce verilerini görüntüleyin, uyarılar oluşturun ve araştırmayı geliştirebilirsiniz.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/17/2021
ms.author: yelevin
ms.openlocfilehash: 1efd91d92bac1bc1f39d82aaa0cc71daa0275f8e
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100570531"
---
# <a name="connect-your-salesforce-service-cloud-to-azure-sentinel"></a>Salesforce hizmeti bulutunuzu Azure Sentinel 'e bağlama

> [!IMPORTANT]
> Salesforce hizmeti bulutu Bağlayıcısı Şu anda **Önizleme** aşamasındadır. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek koşullar için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.

Bu makalede Salesforce hizmeti bulut çözümünüzün Azure Sentinel 'e nasıl bağlanacağı açıklanır. Salesforce Service Cloud Data Connector, Salesforce verilerinizi Azure Sentinel 'e kolayca bağlamanıza olanak tanır. böylece, çalışma kitaplarında görüntüleyebilmeniz için özel uyarılar oluşturmak üzere onu kullanabilir ve araştırmayı geliştirebilirsiniz. Salesforce hizmeti bulutu ve Azure Sentinel arasında tümleştirme, REST API kullanımını sağlar.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="prerequisites"></a>Önkoşullar

- Azure Sentinel çalışma alanında okuma ve yazma izninizin olması gerekir.

- Çalışma alanı için paylaşılan anahtarlar için okuma izninizin olması gerekir. [Çalışma alanı anahtarları hakkında daha fazla bilgi edinin](../azure-monitor/agents/log-analytics-agent.md#workspace-id-and-key).

- İşlev Uygulaması oluşturmak için Azure Işlevleri üzerinde okuma ve yazma izinlerine sahip olmanız gerekir. [Azure işlevleri hakkında daha fazla bilgi edinin](../azure-functions/index.yml).

- Şu Salesforce REST API kimlik bilgilerine sahip olmanız gerekir: **SALESFORCE API Kullanıcı adı**, **Salesforce API parolası**, **Salesforce güvenlik belirteci**, **Salesforce tüketici anahtarı**, **Salesforce tüketici parolası**. [Salesforce REST API hakkında daha fazla bilgi edinin](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/quickstart.htm).

## <a name="configure-and-connect-salesforce-service-cloud"></a>Salesforce hizmeti bulutunu yapılandırma ve bağlama

Salesforce hizmeti bulutu, günlükleri doğrudan Azure Sentinel 'e tümleştirebilir ve dışarı aktarabilir.

1. Azure Sentinel gezinti menüsünde **veri bağlayıcıları**' nı seçin.

1. **Veri bağlayıcıları** galerisinden **Salesforce hizmeti bulutu (Önizleme)** öğesini seçin ve ardından **bağlayıcı sayfasını açın**.

1. Bağlayıcı sayfasının **yapılandırma** bölümünde açıklanan adımları izleyin.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler, tablodaki **customlogs** bölümünün altında **Günlükler**' de görünür `SalesforceServiceCloud_CL` .

Bazı yararlı örnek sorgular için bağlayıcı sayfasındaki **sonraki adımlar** sekmesine bakın.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünmeye başlaması 20 dakikaya kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede Salesforce hizmeti bulutunu Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .