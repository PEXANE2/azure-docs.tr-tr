---
title: Sağlama yapılandırmanızı dışa aktarın ve olağanüstü durum kurtarma için bilinen iyi bir duruma geri dön.| Microsoft Dokümanlar
description: Sağlama yapılandırmanızı nasıl dışa aktarıp olağanüstü durum kurtarma için bilinen iyi bir duruma geri döndürmeyi öğrenin.
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
ms.date: 03/19/2020
ms.author: chmutali
ms.collection: M365-identity-device-management
ms.openlocfilehash: a92a40a5fe3067cf96d3c742102c9ca66078cd5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80051307"
---
# <a name="export-your-provisioning-configuration-and-roll-back-to-a-known-good-state"></a>Sağlama yapılandırmanızı dışa aktarın ve bilinen iyi bir duruma geri dön

## <a name="export-and-import-your-provisioning-configuration-from-the-azure-portal"></a>Sağlama yapılandırmanızı Azure portalından dışa aktarma ve alma

### <a name="how-can-i-export-my-provisioning-configuration"></a>Sağlama yapılandırmamı nasıl dışa aktarabilirim?
Yapılandırmanızı dışa aktarmak için:
1. Sol daki gezinti panelinde [bulunan Azure portalında](https://portal.azure.com/) **Azure Etkin Dizin'i**seçin.
2. Azure **Etkin Dizin** bölmesinde **Kurumsal uygulamaları** seçin ve uygulamanızı seçin.
3. Sol gezinti bölmesinde, **hükmü**seçin. Sağlama yapılandırma sayfasından, **öznitelik eşlemeleri**tıklayın, sonra **gelişmiş seçenekleri göstermek**ve nihayet **şema gözden geçirin.** Bu seni şema editörüne götürecek. 
5. Şemanızı indirmek için sayfanın üst kısmındaki komut çubuğuna indir'e tıklayın.

### <a name="disaster-recovery---roll-back-to-a-known-good-state"></a>Olağanüstü durum kurtarma - bilinen iyi bir duruma geri dön
Yapılandırmanızı dışa aktarmak ve kaydetmek yapılandırmanızın önceki bir sürümüne geri dönmenizi sağlar. Sağlama yapılandırmanızı dışa aktarmanızı ve öznitelik eşlemelerinizde veya kapsam filtrelerinizde bir değişiklik yaptığınızda daha sonra kullanmak üzere kaydetmenizi öneririz. Yapmanız gereken tek şey yukarıdaki adımlarda indirdiğiniz JSON dosyasını açmak, JSON dosyasının tüm içeriğini kopyalamak, şema düzenleyicisinde JSON yükünün tüm içeriğini değiştirmek ve sonra kaydetmektir. Etkin bir sağlama döngüsü varsa, tamamlanır ve bir sonraki döngü güncelleştirilmiş şema kullanır. Bir sonraki döngü, her kullanıcıyı ve grubu yeni yapılandırmaya göre yeniden değerlendiren bir başlangıç döngüsü de olacaktır. Önceki yapılandırmaya geri dönerken aşağıdakileri göz önünde bulundurun:
* Kullanıcılar kapsamda olmaları gerekip gerekmeyeceğini belirlemek için yeniden değerlendirilir. Kapsam filtreleri değiştiyse, kullanıcı artık kapsamda değilse devre dışı bırakılır. Bu çoğu durumda istenen davranış olsa da, bunu önlemek isteyebilirsiniz ve kapsam silme işlevini [natle](https://docs.microsoft.com/azure/active-directory/app-provisioning/skip-out-of-scope-deletions) kullanabilirsiniz zamanlar vardır. 
* Sağlama yapılandırmanızı değiştirmek hizmeti yeniden başlatır ve [bir başlangıç döngüsünü](https://docs.microsoft.com/azure/active-directory/app-provisioning/how-provisioning-works#provisioning-cycles-initial-and-incremental)tetikler.


## <a name="export-and-import-your-provisioning-configuration-by-using-the-microsoft-graph-api"></a>Microsoft Graph API'yi kullanarak sağlama yapılandırmanızı dışa aktarma ve alma
Microsoft Graph API'nizi ve Microsoft Graph Explorer'ı kullanarak Kullanıcı Sağlama öznitelik eşlemenizi ve şemasını bir JSON dosyasına dışa aktarabilir ve Azure AD'ye geri aktarabilirsiniz. Ayrıca, sağlama yapılandırmanızın bir yedeklemesini oluşturmak için burada yakalanan adımları da kullanabilirsiniz. 

### <a name="step-1-retrieve-your-provisioning-app-service-principal-id-object-id"></a>Adım 1: Uygulama Hizmeti Asıl Kimliğinizi (Object ID) alın

1. Azure [portalını](https://portal.azure.com)başlatın ve sağlama uygulamanızın Özellikler bölümüne gidin. Örneğin, *İş gününüzü AD Kullanıcı Sağlama uygulaması* eşlenesine aktarmak istiyorsanız, söz konusu uygulamanın Özellikler bölümüne gidin. 
1. Sağlama uygulamanızın Özellikler bölümünde, *Nesne Kimliği* alanıyla ilişkili GUID değerini kopyalayın. Bu değer, Uygulamanızın **ServicePrincipalId'i** olarak da adlandırılır ve Microsoft Graph Explorer işlemlerinde kullanılır.

   ![İş Günü Uygulama Hizmeti Ana Kimliği](./media/export-import-provisioning-configuration/wd_export_01.png)

### <a name="step-2-sign-into-microsoft-graph-explorer"></a>Adım 2: Microsoft Graph Explorer'da oturum açma

1. [Microsoft Graph Explorer'ı](https://developer.microsoft.com/graph/graph-explorer) başlatın
1. Azure AD Global Admin veya App Admin kimlik bilgilerini kullanarak "Microsoft ile Oturum Açma" düğmesine tıklayın ve oturum açın.

    ![Microsoft Graph Oturum Açma](./media/export-import-provisioning-configuration/wd_export_02.png)

1. Başarılı oturum açma dan sonra, kullanıcı hesap ayrıntılarını sol bölmede görürsünüz.

### <a name="step-3-retrieve-the-provisioning-job-id-of-the-provisioning-app"></a>Adım 3: Sağlama Uygulamasının Sağlama İş Kimliğini Alma

Microsoft Graph Explorer'da, [servicePrincipalId] yerine adım [1'den](#step-1-retrieve-your-provisioning-app-service-principal-id-object-id)çıkarılan **ServicePrincipalId** ile aşağıdaki GET sorgusunu çalıştırın.

```http
   GET https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs
```

Aşağıda gösterildiği gibi bir yanıt alırsınız. Yanıtta bulunan "kimlik özniteliği"ni kopyalayın. Bu değer **ProvisioningJobId'dir** ve temel şema meta verilerini almak için kullanılır.

   [![İş Kimliği Sağlama](./media/export-import-provisioning-configuration/wd_export_03.png)](./media/export-import-provisioning-configuration/wd_export_03.png#lightbox)

### <a name="step-4-download-the-provisioning-schema"></a>Adım 4: Provizyon Şema İndir

Microsoft Graph Explorer'da, [servicePrincipalId] ve [ProvisioningJobId] ile önceki adımlarda alınan ServicePrincipalId ve ProvisioningJobId'i değiştirerek aşağıdaki GET sorgusunu çalıştırın.

```http
   GET https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs/[ProvisioningJobId]/schema
```

Yanıttan JSON nesnesini kopyalayın ve şema yedeklemesi oluşturmak için bir dosyaya kaydedin.

### <a name="step-5-import-the-provisioning-schema"></a>Adım 5: Provizyon Şema'sını İthalat

> [!CAUTION]
> Bu adımı yalnızca Azure portalı kullanılarak değiştirilemeyen yapılandırma için şemayı değiştirmeniz gerekiyorsa veya yapılandırmayı önceden yedeklenmiş bir dosyadan geçerli ve çalışan şema ile geri yüklemeniz gerekiyorsa gerçekleştirin.

Microsoft Graph Explorer'da, [servicePrincipalId] ve [ProvisioningJobId] ile önceki adımlarda alınan ServicePrincipalId ve ProvisioningJobId'i değiştirerek aşağıdaki PUT sorgusunu yapılandırın.

```http
    PUT https://graph.microsoft.com/beta/servicePrincipals/[servicePrincipalId]/synchronization/jobs/[ProvisioningJobId]/schema
```

"İstek Gövdesi" sekmesinde, JSON şema dosyasının içeriğini kopyalayın.

   [![İstek Gövdesi](./media/export-import-provisioning-configuration/wd_export_04.png)](./media/export-import-provisioning-configuration/wd_export_04.png#lightbox)

"İstek Üstbilgi" sekmesinde, "uygulama/json" değeriyle İçerik Türü üstbilgi özniteliğini ekleyin

   [![İstek Başlıkları](./media/export-import-provisioning-configuration/wd_export_05.png)](./media/export-import-provisioning-configuration/wd_export_05.png#lightbox)

Yeni şemayı almak için "Sorguyu Çalıştır" düğmesine tıklayın.
