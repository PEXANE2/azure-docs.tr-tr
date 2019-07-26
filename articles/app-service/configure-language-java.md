---
title: Windows Java uygulamalarını Yapılandırma-Azure App Service | Microsoft Docs
description: Java uygulamalarını, Azure App Service varsayılan Windows örneklerinde çalışacak şekilde nasıl yapılandıracağınızı öğrenin.
keywords: Azure App Service, Web uygulaması, Windows, Oss, Java
services: app-service
author: jasonfreeberg
manager: jeconnock
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 04/12/2019
ms.author: jafreebe
ms.reviewer: cephalin
ms.custom: seodec18
ms.openlocfilehash: c1ea306d8a6b5c1876ac6a9288820e1592dbfda6
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68498521"
---
# <a name="configure-a-windows-java-app-for-azure-app-service"></a>Azure App Service için bir Windows Java uygulaması yapılandırma

Azure App Service, Java geliştiricilerinin tam olarak yönetilen bir Windows tabanlı hizmette Tomcat veya Java Standard Edition (SE) paketlenmiş Web uygulamalarını hızlıca oluşturmasına, dağıtmasına ve ölçeklendirmesine olanak tanır. Maven eklentilerine sahip uygulamaları, komut satırından veya IntelliJ, tutulma veya Visual Studio Code gibi düzenleyicilerde dağıtın.

Bu kılavuz, App Service ' de kullanarak Java geliştiricileri için temel kavramlar ve yönergeler sağlar. Azure App Service hiç kullanmadıysanız, önce [Java hızlı](app-service-web-get-started-java.md) başlangıcı ' nı okumanız gerekir. Java geliştirmeye özgü olmayan App Service kullanımı hakkında genel sorular, [App Service WINDOWS SSS](faq-configuration-and-management.md)' de yanıtlanmıştır.

> [!NOTE]
> Aradığınızı bulamadınız mı? Java uygulamanızı dağıtma ve güvenliğini sağlama hakkında bilgi için lütfen [WINDOWS OSS SSS](faq-configuration-and-management.md) veya [Java Linux yapılandırma kılavuzu](containers/configure-language-java.md) ' na bakın.

## <a name="configuring-tomcat"></a>Tomcat yapılandırma

Tomcat 'in `server.xml` veya diğer yapılandırma dosyalarını düzenlemek için önce portalda Tomcat ana sürümünüzü bir yere göz atın.

1. `env` Komutunu çalıştırarak sürümünüz için Tomcat giriş dizinini bulun. İle `AZURE_TOMCAT`başlayan ve ana sürümünüzle eşleşen ortam değişkenini arayın. Örneğin, `AZURE_TOMCAT85_HOME` Tomcat 8,5 için Tomcat dizinine işaret eder.
1. Sürümünüz için Tomcat giriş dizinini tanımladıktan sonra yapılandırma dizinini öğesine `D:\home`kopyalayın. Örneğin, `AZURE_TOMCAT85_HOME` bir `D:\Program Files (x86)\apache-tomcat-8.5.37`değeri olsaydı, kopyalanmış dizinin `D:\home\apache-tomcat-8.5.37`yeni yolu olur.

Son olarak, App Service yeniden başlatın. Dağıtımlarınız daha önce olduğu `D:\home\site\wwwroot\webapps` gibi ' e gitmelidir.

## <a name="java-runtime-statement-of-support"></a>Java Runtime desteği

### <a name="jdk-versions-and-maintenance"></a>JDK sürümleri ve bakım

Azure 'un desteklenen Java Geliştirme Seti (JDK), [Azul sistemleri](https://www.azul.com/)aracılığıyla bir [Zulu](https://www.azul.com/downloads/azure-only/zulu/) dili olarak sunulmaktadır.

Ana sürüm güncelleştirmeleri, Windows için Azure App Service yeni çalışma zamanı seçenekleri aracılığıyla sağlanacaktır. Müşteriler, App Service dağıtımını yapılandırarak ve ana güncelleştirmenin ihtiyaçlarını karşıladığından sorumlu olduğundan, bu yeni Java sürümlerine güncelleştirir.

Desteklenen JDKs, her yıl Ocak, Nisan, Temmuz ve Ekim ayında otomatik olarak üç ayda bir düzeltme eki uygulanır.

### <a name="security-updates"></a>Güvenlik güncelleştirmeleri

Önemli güvenlik açıklarına yönelik düzeltme ekleri ve düzeltmeler Azul sistemlerinden kullanılabilir hale geldiğinde serbest bırakılır. "Ana" güvenlik açığı, [NIST ortak güvenlik açığı Puanlama sistemi, sürüm 2](https://nvd.nist.gov/cvss.cfm)üzerinde 9,0 veya üzeri bir taban puanı tarafından tanımlanır.

### <a name="deprecation-and-retirement"></a>Kullanımdan kaldırma ve kullanımdan kaldırma

Desteklenen bir Java çalışma zamanı devre dışı bırakırsa, etkilenen çalışma zamanını kullanan Azure geliştiricileri, çalışma zamanı kullanımdan kalkmadan önce en az altı ayda bir kullanımdan kaldırma olarak verilmeyecektir.

### <a name="local-development"></a>Yerel geliştirme

Geliştiriciler, Azul ['nin indirme sitesinden](https://www.azul.com/downloads/azure-only/zulu/)yerel geliştirme Için Azul Zulu kurumsal JDK üretim sürümünü indirebilir.

### <a name="development-support"></a>Geliştirme desteği

Azure [tarafından desteklenen Azul Zulu JDK](https://www.azul.com/downloads/azure-only/zulu/) için ürün desteği, Azure için geliştirme sırasında veya [nitelikli bir Azure destek planı](https://azure.microsoft.com/support/plans/)ile [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) Microsoft aracılığıyla edinilebilir.

### <a name="runtime-support"></a>Çalışma zamanı desteği

Geliştiriciler, [tam destek planına](https://azure.microsoft.com/support/plans/)sahip olmaları durumunda Azul Zulu JDKs ile ilgili [bir sorunu](/azure/azure-supportability/how-to-create-azure-support-request) , Azure desteği aracılığıyla açabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu konuda, Windows üzerinde Azure App Service için desteklenen Java çalışma zamanı açıklaması sağlanmaktadır.

- Web uygulamalarını barındırma hakkında daha fazla bilgi için Azure App Service [App Service genel bakış](overview.md)bölümüne bakın.
- Azure 'da Java hakkında daha fazla bilgi için bkz. [Java Için Azure Geliştirme Merkezi](https://docs.microsoft.com/java/azure/?view=azure-java-stable).
