---
title: Sertifikalarla REKLAM Raporlama API'si için Öğretici | Microsoft Dokümanlar
description: Bu öğretici, kullanıcı müdahalesi olmadan dizinlerden veri almak için sertifika kimlik bilgileriyle birlikte Azure AD Raporlama API'sinin nasıl kullanılacağını açıklar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d723af5d994006c4ae4f90905ede73fa87326bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74014261"
---
# <a name="tutorial-get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Öğretici: Sertifikaları olan Azure Etkin Dizin raporlama API'sini kullanarak veri alın

[Azure Active Directory (Azure AD) raporlama API'leri](concept-reporting-api.md), bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz. Azure AD Raporlama API'sine kullanıcı müdahalesi olmadan erişmek istiyorsanız, erişiminizi sertifikaları kullanmak üzere yapılandırmanız gerekir.

Bu eğitimde, raporlama için MS Graph API'sine erişmek için bir test sertifikasını nasıl kullanacağınızı öğrenirsiniz. Test sertifikalarını üretim ortamında kullanmanızı önermiyoruz. 

## <a name="prerequisites"></a>Ön koşullar

1. Oturum açma verilerine erişmek için, premium (P1/P2) lisansına sahip bir Azure Active Directory kiracınız olduğundan emin olun. Azure Active Directory baskınızı yükseltmek için [Azure Active Directory Premium ile başlarken](../fundamentals/active-directory-get-started-premium.md) bakın. Yükseltmeden önce herhangi bir etkinlik veriniz yoksa, bir premium lisansa yükselttikten sonra verilerin raporlarda gösterilmesinin birkaç gün süreceğini unutmayın. 

2. **Genel yönetici,** **güvenlik yöneticisi,** **güvenlik okuyucu** veya kiracı için **rapor okuyucu** rolü nde bir kullanıcı hesabı oluşturun veya geçiş yapmak. 

3. Azure [Etkin Dizin raporlama API'sine erişmek için ön koşulları](howto-configure-prerequisites-for-reporting-api.md)tamamlayın. 

4. [Azure AD PowerShell V2'yi](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/docs-conceptual/azureadps-2.0/install-adv2.md)indirin ve kurun.

5. [MSCloudIdUtils](https://www.powershellgallery.com/packages/MSCloudIdUtils/)yükleyin. Bu modül aşağıdaki çeşitli yardımcı program cmdlet'lerini sağlar:
    - Kimlik doğrulaması için gerekli ADAL kitaplıkları
    - ADAL kullanarak kullanıcı, uygulama anahtarları ve sertifikalardan erişim belirteçleri
    - Disk belleğine alınmış Graph API işleme sonuçları

6. **Install-MSCloudIdUtilsModule**modül çalıştırın, aksi takdirde **Alma-Modül** Powershell komutunu kullanarak almak modül çalıştırmak ilk kez ise. Oturumunuz bu ekrana benzer ![olmalıdır: Windows Powershell](./media/tutorial-access-api-with-certificates/module-install.png)
  
7. Test sertifikası oluşturmak için **Yeni İmzalı Sertifika** Powershell komut unu kullanın.

   ```
   $cert = New-SelfSignedCertificate -Subject "CN=MSGraph_ReportingAPI" -CertStoreLocation "Cert:\CurrentUser\My" -KeyExportPolicy Exportable -KeySpec Signature -KeyLength 2048 -KeyAlgorithm RSA -HashAlgorithm SHA256
   ```

8. Sertifika dosyasına aktarmak için **Dışa Aktarma Sertifikası** komutunu kullanın.

   ```
   Export-Certificate -Cert $cert -FilePath "C:\Reporting\MSGraph_ReportingAPI.cer"

   ```

## <a name="get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Sertifikalarla Azure Active Directory raporlama API’sini kullanarak veri alma

1. [Azure portalına](https://portal.azure.com)gidin, **Azure Etkin Dizini'ni**seçin, ardından **Uygulama kayıtlarını** seçin ve uygulamanızı listeden seçin. 

2. **Ayarlar** > **Tuşlarını** seçin ve Ortak Anahtarı **Yükle'yi**seçin.

3. Önceki adımdan sertifika dosyasını seçin ve **Kaydet'i**seçin. 

4. Başvuru Kimliğini ve başvurunuza yeni kaydolduğunuz sertifikanın parmak izini not edin. Portaldaki uygulama sayfanızdan parmak izini bulmak için **Ayarlar'a** gidin ve **Tuşlar'ı**tıklatın. Parmak izi **Ortak Anahtarlar** listesinin altında olacaktır.

5. Satır İçi bildirim düzenleyicisinde uygulama bildirimini açın ve aşağıdaki şema'yı kullanarak *keyCredentials* özelliğini yeni sertifika bilgilerinizle değiştirin. 

   ```
   "keyCredentials": [
        {
            "customKeyIdentifier": "$base64Thumbprint", //base64 encoding of the certificate hash
            "keyId": "$keyid", //GUID to identify the key in the manifest
            "type": "AsymmetricX509Cert",
            "usage": "Verify",
            "value":  "$base64Value" //base64 encoding of the certificate raw data
        }
    ]
   ```

6. Manifestoyu kaydedin. 
  
7. Şimdi, bu sertifikayı kullanarak MS Graph API için bir erişim belirteci alabilirsiniz. UYGULAMA Kimliği'ni ve önceki adımdan elde ettiğiniz parmak izini kullanarak MSCloudIdUtils PowerShell modülünden **Get-MSCloudIdMSMSGraphAccessTokenFromCert** cmdlet'i kullanın. 

   ![Azure portalında](./media/tutorial-access-api-with-certificates/getaccesstoken.png)

8. Grafik API'sini sorgulamak için Powershell komut dosyanızdaki erişim belirtecisini kullanın. İşaretleri ve dizinDenetimleri bitiş noktasını saymak için MSCloudIDUtils'in **Invoke-MSCloudIdMSGraphQuery** cmdlet'ini kullanın. Bu cmdlet çok sayfalı sonuçları işler ve bu sonuçları PowerShell ardışık boru hattına gönderir.

9. Denetim günlüklerini almak için dizinDenetimleri bitiş noktasını sorgula. 
   ![Azure portalda](./media/tutorial-access-api-with-certificates/query-directoryAudits.png)

10. Oturum açma günlüklerini almak için oturum bitiş noktasını sorgulayın.
    ![Azure portalda](./media/tutorial-access-api-with-certificates/query-signins.png)

11. Artık bu verileri bir CSV'ye aktarmayı ve bir SIEM sistemine kaydetmeyi seçebilirsiniz. Ayrıca, betiğinizi zamanlanmış bir göreve kaydırarak, uygulama anahtarlarını kaynak kodda depolamak zorunda kalmadan kiracınızdan Azure AD verilerini düzenli olarak alabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Raporlama API'leriyle ilgili ilk izlenim elde edin](concept-reporting-api.md)
* [Denetim API başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Oturum açma faaliyet raporu API başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
