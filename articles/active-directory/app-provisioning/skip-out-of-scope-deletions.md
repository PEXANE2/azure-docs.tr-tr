---
title: Kapsam dışı kullanıcıların silinmesini atlayın | Microsoft Dokümanlar
description: Kapsam dışı sağlama nın varsayılan davranışını nasıl geçersiz kısılamayı öğrenin.
services: active-directory
author: cmmdesai
documentationcenter: na
manager: daveba
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.subservice: app-provisioning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/10/2019
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: a1668c022a0f067a381ba09b397c7d38c99ad074
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77522458"
---
# <a name="skip-deletion-of-user-accounts-that-go-out-of-scope"></a>Kapsam dışına çıkan kullanıcı hesaplarının silinmesini atlama

Varsayılan olarak, Azure AD sağlama motoru yumuşak siler veya kapsam dışına çıkan kullanıcıları devre dışı kılabilir. Ancak, Ad Kullanıcı Gelen Sağlama Için İş Günü gibi belirli senaryolar için bu davranışı beklenen olmayabilir ve bu varsayılan davranışı geçersiz kılmak isteyebilirsiniz.  

Bu kılavuzda, kapsam dışına çıkan hesapların işlenmesini denetleyen ***SkipOutOfScopeDeletions*** bayrağını ayarlamak için Microsoft Graph API ve Microsoft Graph API gezgininin nasıl kullanılacağı açıklanmaktadır. 
* ***SkipOutOfScopeDeletions*** 0 (yanlış) olarak ayarlanırsa, kapsam dışına çıkan hesaplar hedefte devre dışı bırakılır
* ***SkipOutOfScopeDeletions*** 1 (true) olarak ayarlanırsa, kapsam dışına çıkan hesaplar hedefte devre dışı bırakılmaz Bu bayrak *Madde Uygulama* düzeyinde ayarlanır ve Grafik API kullanılarak yapılandırılabilir. 

Bu *yapılandırma, İş Günü'nden Active Directory kullanıcı sağlama uygulamasına* yaygın olarak kullanıldığından, aşağıdaki adımlar İş Günü uygulamasının ekran görüntülerini içerir. Ancak bu, (ServiceNow, Salesforce, Dropbox, vb.) gibi **diğer tüm uygulamalarda** da kullanılabilir.

## <a name="step-1-retrieve-your-provisioning-app-service-principal-id-object-id"></a>Adım 1: Uygulama Hizmeti Asıl Kimliğinizi (Object ID) alın

1. Azure [portalını](https://portal.azure.com)başlatın ve sağlama uygulamanızın Özellikler bölümüne gidin. Örneğin, *İş gününüzü AD Kullanıcı Sağlama uygulamasına* aktarmak istiyorsanız, söz konusu uygulamanın Özellikler bölümüne gidin. 
1. Sağlama uygulamanızın Özellikler bölümünde, *Nesne Kimliği* alanıyla ilişkili GUID değerini kopyalayın. Bu değer, Uygulamanızın **ServicePrincipalId'i** olarak da adlandırılır ve Grafik Explorer işlemlerinde kullanılır.

   ![İş Günü Uygulama Hizmeti Ana Kimliği](./media/skip-out-of-scope-deletions/wd_export_01.png)

## <a name="step-2-sign-into-microsoft-graph-explorer"></a>Adım 2: Microsoft Graph Explorer'da oturum açma

1. [Microsoft Graph Explorer'ı](https://developer.microsoft.com/graph/graph-explorer) başlatın
1. Azure AD Global Admin veya App Admin kimlik bilgilerini kullanarak "Microsoft ile Oturum Açma" düğmesine tıklayın ve oturum açın.

    ![Grafik Oturum Açma](./media/skip-out-of-scope-deletions/wd_export_02.png)

1. Başarılı oturum açma dan sonra, kullanıcı hesap ayrıntılarını sol bölmede görürsünüz.

## <a name="step-3-get-existing-app-credentials-and-connectivity-details"></a>Adım 3: Varolan uygulama kimlik bilgilerini ve bağlantı ayrıntılarını alın

Microsoft Graph Explorer'da, [servicePrincipalId] yerine adım [1'den](#step-1-retrieve-your-provisioning-app-service-principal-id-object-id)çıkarılan **ServicePrincipalId** ile aşağıdaki GET sorgusunu çalıştırın.

```http
   GET https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/secrets
```

   ![İş sorgusu alma](./media/skip-out-of-scope-deletions/skip-03.png)

Yanıt'ı bir metin dosyasına kopyalayın. Aşağıda gösterilen JSON metnine benzer ve dağıtımınıza özgü sarı renkte vurgulanır. Vurgulanan satırları sonuna yeşil olarak ekleyin ve mavi yle vurgulanan İş Günü bağlantı parolasını güncelleştirin. 

   ![İş yanıtı al](./media/skip-out-of-scope-deletions/skip-04.png)

Burada haritalama eklemek için JSON bloğudur. 

```json
        {
            "key": "SkipOutOfScopeDeletions",
            "value": "True"
        }
```

## <a name="step-4-update-the-secrets-endpoint-with-the-skipoutofscopedeletions-flag"></a>Adım 4: SkipOutOfScopeDeletions bayrağı ile sırları bitiş noktasını güncelleştirin

Grafik Gezgini'nde, ***SkipOutOfScopeDeletions*** bayrağıyla sırları niçin güncelleştirmek için aşağıdaki komutu çalıştırın. 

Aşağıdaki URL'de [servicePrincipalId] [adım 1'den](#step-1-retrieve-your-provisioning-app-service-principal-id-object-id)çıkarılan **ServicePrincipalId** ile değiştirin. 

```http
   PUT https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/secrets
```
Güncelleştirilmiş metni Adım 3'ten "İstek Gövdesi"ne kopyalayın ve üstbilgi "İçerik Türü"nü "İstek Başlıkları"nda "uygulama/json" olarak ayarlayın. 

   ![PUT isteği](./media/skip-out-of-scope-deletions/skip-05.png)

"Sorguyı Çalıştır"a tıklayın. 

Çıktıyı "Başarı – Durum Kodu 204" olarak almalısınız. 

   ![PUT yanıtı](./media/skip-out-of-scope-deletions/skip-06.png)

## <a name="step-5-verify-that-out-of-scope-users-dont-get-disabled"></a>Adım 5: Kapsam dışında kullanıcıların devre dışı bırakılmadığını doğrulayın

Belirli bir kullanıcıyı atlamak için kapsam kurallarınızı güncelleştirerek bu bayrak sonuçlarını beklenen davranışta sınayabilirsiniz. Aşağıdaki örnekte, yeni bir kapsam kuralı ekleyerek 21173 kimliğine sahip (daha önce kapsamda olan) çalışanı hariç tutarak: 

   ![Kapsam örneği](./media/skip-out-of-scope-deletions/skip-07.png)

Bir sonraki sağlama döngüsünde, Azure AD sağlama hizmeti, kullanıcı 21173'ün kapsam dışına çıktığını ve SkipOutOfScopeDeletions özelliği etkinse, bu kullanıcıiçin eşitleme kuralı aşağıda gösterildiği gibi bir ileti görüntüler: 

   ![Kapsam örneği](./media/skip-out-of-scope-deletions/skip-08.png)


