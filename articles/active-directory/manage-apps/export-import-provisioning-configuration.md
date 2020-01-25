---
title: Sağlama yapılandırmanızı dışarı aktarma veya Graph API ile içeri aktarma | Microsoft Docs
description: Graph API kullanarak sağlama yapılandırmasını dışarı ve içeri aktarmayı öğrenin.
services: active-directory
author: cmmdesai
documentationcenter: na
manager: daveba
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/09/2019
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: 609031bfad23a14a954a09a447e363e89a9d29d5
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76711753"
---
# <a name="export-or-import-your-provisioning-configuration-by-using-graph-api"></a>Sağlama yapılandırmanızı Graph API kullanarak dışarı aktarın veya içeri aktarın

Kullanıcı hazırlama öznitelik eşlemelerinizi ve şemanızı bir JSON dosyasına aktarmak ve yeniden Azure AD 'ye aktarmak için Microsoft Graph API ve Graf Gezginini kullanabilirsiniz. Sağlama yapılandırmanızın bir yedeğini oluşturmak için burada yakalanan adımları da kullanabilirsiniz. 

## <a name="step-1-retrieve-your-provisioning-app-service-principal-id-object-id"></a>1\. Adım: sağlama App Service asıl KIMLIĞINI alma (nesne KIMLIĞI)

1. [Azure Portal](https://portal.azure.com)başlatın ve sağlama uygulamanızın Özellikler bölümüne gidin. Örneğin, *Workday 'NIZI ad Kullanıcı sağlama uygulama* eşlemesinde dışarı aktarmak istiyorsanız, uygulamanın Özellikler bölümüne gidin. 
1. Sağlama uygulamanızın Özellikler bölümünde, *nesne kimliği* ALANıYLA ilişkili GUID değerini kopyalayın. Bu değer, uygulamanızın **Serviceprincipalıd** olarak da adlandırılır ve Graph Explorer işlemlerinde kullanılacaktır.

   ![App Service ana iş günü KIMLIĞI](media/export-import-provisioning-mappings/wd_export_01.png)

## <a name="step-2-sign-into-microsoft-graph-explorer"></a>2\. Adım: Microsoft Graph Explorer 'da oturum açın

1. [Microsoft Graph Gezginini](https://developer.microsoft.com/graph/graph-explorer) Başlat
1. "Microsoft 'a oturum aç" düğmesine tıklayın ve Azure AD Genel yönetici veya uygulama Yöneticisi kimlik bilgilerini kullanarak oturum açın.

    ![Graph oturum açma](media/export-import-provisioning-mappings/wd_export_02.png)

1. Başarılı oturum açma işlemi tamamlandıktan sonra, sol bölmedeki Kullanıcı hesabı ayrıntılarını görürsünüz.

## <a name="step-3-retrieve-the-provisioning-job-id-of-the-provisioning-app"></a>3\. Adım: sağlama uygulamasının sağlama Işi KIMLIĞINI alma

Microsoft Graph Gezgini ' nde, [Adım 1](#step-1-retrieve-your-provisioning-app-service-principal-id-object-id)' den ayıklanan **serviceprincipalıd** Ile [serviceprincipalıd] yerine aşağıdaki Get sorgusunu çalıştırın.

```http
   GET https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs
```

Aşağıda gösterildiği gibi bir yanıt alacaksınız. Yanıtta bulunan "ID özniteliği" öğesini kopyalayın. Bu değer **Provisioningjobıd** ' dir ve temel alınan şema meta verilerini almak için kullanılır.

   [![sağlama Işi KIMLIĞI](media/export-import-provisioning-mappings/wd_export_03.png)](media/export-import-provisioning-mappings/wd_export_03.png#lightbox)

## <a name="step-4-download-the-provisioning-schema"></a>4\. Adım: sağlama şemasını Indirme

Microsoft Graph Gezgini 'nde, aşağıdaki GET sorgusunu çalıştırarak [Serviceprincipalıd] ve [Provisioningjobıd] değerini, önceki adımlarda alınan Serviceprincipalıd ve Provisioningjobıd ile değiştirin.

```http
   GET https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs/[ProvisioningJobId]/schema
```

JSON nesnesini yanıttan kopyalayın ve şemanın bir yedeğini oluşturmak için dosyayı bir dosyaya kaydedin.

## <a name="step-5-import-the-provisioning-schema"></a>5\. Adım: sağlama şemasını Içeri aktarma

> [!CAUTION]
> Bu adımı yalnızca, Azure portal kullanarak değiştirilemeyen yapılandırma şemasını değiştirmeniz gerekiyorsa veya daha önce yedeklenen bir dosyanın geçerli ve çalışma şemasına sahip olan yapılandırmayı geri yüklemeniz gerekiyorsa gerçekleştirin.

Microsoft Graph Gezgini 'nde, aşağıdaki PUT sorgusunu, [Serviceprincipalıd] ve [Provisioningjobıd] değerini, önceki adımlarda alınan Serviceprincipalıd ve Provisioningjobıd ile değiştirerek yapılandırın.

```http
    PUT https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs/[ProvisioningJobId]/schema
```

"Istek gövdesi" sekmesinde, JSON Şema dosyasının içeriğini kopyalayın.

   [Istek gövdesini ![](media/export-import-provisioning-mappings/wd_export_04.png)](media/export-import-provisioning-mappings/wd_export_04.png#lightbox)

"Istek üstbilgileri" sekmesinde, Içerik türü üst bilgi özniteliğini "Application/JSON" değeriyle ekleyin

   [Istek üst bilgilerini ![](media/export-import-provisioning-mappings/wd_export_05.png)](media/export-import-provisioning-mappings/wd_export_05.png#lightbox)

Yeni şemayı içeri aktarmak için "Sorguyu Çalıştır" düğmesine tıklayın.
