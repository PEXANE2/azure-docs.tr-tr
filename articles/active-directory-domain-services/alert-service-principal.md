---
title: 'Azure Active Directory Domain Services: Hizmet sorumlusu sorunlarını giderme | Microsoft Docs'
description: Azure AD Domain Services için hizmet sorumlusu yapılandırması sorunlarını giderme
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: ''
editor: ''
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: iainfou
ms.openlocfilehash: 9e5fa8c84f5e7ca58117666846b603a118826150
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234136"
---
# <a name="troubleshoot-invalid-service-principal-configurations-for-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services için geçersiz hizmet sorumlusu yapılandırmalarının sorunlarını giderme

Bu makale, aşağıdaki uyarı iletisine neden olan hizmet sorumlusu ile ilgili yapılandırma hatalarını gidermenize ve çözmenize yardımcı olur:

## <a name="alert-aadds102-service-principal-not-found"></a>Uyarı AADDS102: Hizmet sorumlusu bulunamadı

**Uyarı iletisi:** *Azure AD Domain Services çalışması için gereken bir hizmet sorumlusu Azure AD dizininden silindi. Bu yapılandırma, Microsoft 'un yönetilen etki alanınızı izleme, yönetme, düzeltme eki uygulama ve eşitlemeye yönelik yeteneğini etkiler.*

[Hizmet sorumluları](../active-directory/develop/app-objects-and-service-principals.md) , Microsoft 'un yönetilen etki alanınızı yönetmek, güncelleştirmek ve sürdürmek için kullandığı uygulamalardır. Bunlar silinirse, Microsoft 'un etki alanınızı hizmetine hizmet verme yeteneğini keser.


## <a name="check-for-missing-service-principals"></a>Eksik hizmet sorumlularını denetle
Hangi hizmet sorumlularının yeniden oluşturulması gerektiğini öğrenmek için aşağıdaki adımları kullanın:

1. Azure portal [Kurumsal uygulamalar-tüm uygulamalar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) sayfasına gidin.
2. **Göster** açılan menüsünde, **tüm uygulamalar** ' ı seçin ve **Uygula**' ya tıklayın.
3. Aşağıdaki tabloyu kullanarak, KIMLIĞI arama kutusuna yapıştırarak ve ENTER ' a basarak her uygulama KIMLIĞI için arama yapın. Arama sonuçları boşsa, "Çözüm" sütunundaki adımları izleyerek hizmet sorumlusunu yeniden oluşturmanız gerekir.

| Uygulama Kimliği | Çözüm |
| :--- | :--- |
| 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [PowerShell ile eksik hizmet sorumlusunu yeniden oluşturma](#recreate-a-missing-service-principal-with-powershell) |
| 443155a6-77f3-45e3-882b-22b3a8d431fb | [Microsoft. AAD ad alanına yeniden kaydolun](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| abba844e-bc0e-44b0-947a-dc74e5d09022  | [Microsoft. AAD ad alanına yeniden kaydolun](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |
| d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Microsoft. AAD ad alanına yeniden kaydolun](#re-register-to-the-microsoft-aad-namespace-using-the-azure-portal) |

## <a name="recreate-a-missing-service-principal-with-powershell"></a>PowerShell ile eksik hizmet sorumlusunu yeniden oluşturma
Azure AD dizininizde kimliğe ```2565bd9d-da50-47d4-8b85-4c97f669dc36``` sahip bir hizmet sorumlusu eksikse bu adımları izleyin.

**Çözüm:** Bu adımları tamamlayabilmeniz için Azure AD PowerShell gereklidir. Azure AD PowerShell 'i yükleme hakkında daha fazla bilgi için [Bu makaleye](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.)bakın.

Bu sorunu gidermek için bir PowerShell penceresine aşağıdaki komutları yazın:
1. Azure AD PowerShell modülünü yükleyip içeri aktarın.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

2. Aşağıdaki PowerShell komutunu yürüterek, Azure AD Domain Services için gereken hizmet sorumlusunun dizininizde eksik olup olmadığını denetleyin:

    ```powershell
    Get-AzureAdServicePrincipal -filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
    ```

3. Aşağıdaki PowerShell komutunu yazarak hizmet sorumlusu oluşturun:

    ```powershell
    New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
    ```

4. Eksik hizmet sorumlusunu oluşturduktan sonra iki saat bekleyin ve yönetilen etki alanı sistem durumunu denetleyin.


## <a name="re-register-to-the-microsoft-aad-namespace-using-the-azure-portal"></a>Azure portal kullanarak Microsoft AAD ad alanına yeniden kaydolun
Azure AD dizininizde kimliğe ```443155a6-77f3-45e3-882b-22b3a8d431fb``` veya ```abba844e-bc0e-44b0-947a-dc74e5d09022``` veya veya ```d87dcbc6-a371-462e-88e3-28ad15ec4e64``` olmayan bir hizmet sorumlusu yoksa, bu adımları izleyin.

**Çözüm:** Dizininizdeki etki alanı hizmetlerini geri yüklemek için aşağıdaki adımları kullanın:

1. Azure portal [abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) sayfasına gidin.
2. Yönetilen etki alanınız ile ilişkili tablodan aboneliği seçin
3. Sol taraftaki gezintiyi kullanarak **kaynak sağlayıcıları** ' nı seçin.
4. Tabloda "Microsoft. AAD" araması yapın ve **yeniden kaydet** ' e tıklayın.
5. Uyarının çözümlendiğinden emin olmak için, yönetilen etki alanınız için sistem durumu sayfasını iki saat içinde görüntüleyin.


## <a name="alert-aadds105-password-synchronization-application-is-out-of-date"></a>Uyarı AADDS105: Parola eşitleme uygulaması güncel değil

**Uyarı iletisi:** "D87dcbc6-a371-462e-88e3-28ad15ec4e64" uygulama KIMLIĞINE sahip hizmet sorumlusu silindi ve sonra yeniden oluşturuldu. Yeniden oluşturma, yönetilen etki alanınızı hizmet etmek için gereken Azure AD Domain Services kaynaklardaki tutarsız izinlerin arkasında kalır. Yönetilen etki alanındaki parolaların eşitlenmesi etkilenebilir.


**Çözüm:** Bu adımları tamamlayabilmeniz için Azure AD PowerShell gereklidir. Azure AD PowerShell 'i yükleme hakkında daha fazla bilgi için [Bu makaleye](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0.)bakın.

Bu sorunu gidermek için bir PowerShell penceresine aşağıdaki komutları yazın:
1. Azure AD PowerShell modülünü yükleyip içeri aktarın.

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```
2. Aşağıdaki PowerShell komutlarını kullanarak eski uygulamayı ve nesneyi Sil

    ```powershell
    $app = Get-AzureADApplication -Filter "IdentifierUris eq 'https://sync.aaddc.activedirectory.windowsazure.com'"
    Remove-AzureADApplication -ObjectId $app.ObjectId
    $spObject = Get-AzureADServicePrincipal -Filter "DisplayName eq 'Azure AD Domain Services Sync'"
    Remove-AzureADServicePrincipal -ObjectId $app.ObjectId
    ```
3. Her ikisini de sildikten sonra, sistem kendisini düzeltir ve parola eşitleme için gereken uygulamaları yeniden oluşturur. Uyarının düzeltildiğinden emin olmak için iki saati bekleyip etki alanı sistem durumunu kontrol edin.


## <a name="contact-us"></a>Bize Ulaşın
[Geri bildirim paylaşmak veya destek için](contact-us.md)Azure Active Directory Domain Services ürün ekibine başvurun.
