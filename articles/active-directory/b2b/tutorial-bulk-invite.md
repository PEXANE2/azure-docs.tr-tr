---
title: B2B işbirliği kullanıcılarının toplu olarak davet için çalışması - Azure AD
description: Bu öğreticide, harici Azure AD B2B işbirliği kullanıcılarına toplu davet göndermek için PowerShell ve CSV dosyasının nasıl kullanılacağını öğreneceksiniz.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: tutorial
ms.date: 04/13/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0ef9172ca5d0961bb6de1949a61199ce1d6c1bff
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81603391"
---
# <a name="tutorial-bulk-invite-azure-ad-b2b-collaboration-users"></a>Öğretici: Azure AD B2B işbirliği kullanıcılarını toplu davet etme

Şirket dışındaki ortaklarla çalışmak için Azure Active Directory (Azure AD) B2B işbirliğini kullanıyorsanız, kuruluşunuza aynı anda birden çok konuk kullanıcı davet edebilirsiniz. Bu eğitimde, dış kullanıcılara toplu davetiye göndermek için Azure portalını nasıl kullanacağınızı öğrenirsiniz. Özellikle aşağıdakileri yapın:

> [!div class="checklist"]
> * Kullanıcı bilgileri ve davet tercihleri ile virgülden ayrılmış bir değer (.csv) dosyası hazırlamak için **Toplu davet kullanıcılarını** kullanma
> * .csv dosyasını Azure AD'ye yükleme
> * Kullanıcıların dizine eklendiğini doğrulama

Azure Etkin Dizin'iniz yoksa, başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Davetleri gönderebileceğiniz iki veya daha fazla test e-posta hesabı olması gerekir. Hesaplar, kuruluşunuzun dışından olmalıdır. Gmail.com veya outlook.com adresleri gibi sosyal hesapları içeren herhangi bir hesap türü kullanabilirsiniz.

## <a name="invite-guest-users-in-bulk"></a>Konuk kullanıcıları toplu olarak davet edin

1. Kuruluşta Kullanıcı yöneticisi olan bir hesapla Azure portalında oturum açın.
2. Gezinti bölmesinde Azure **Etkin Dizin'i**seçin.
3. **Yönet**altında, **Kullanıcılar** > **Toplu davet**seçin.
4. Toplu **davet kullanıcıları** sayfasında, davet özelliklerine sahip geçerli bir .csv dosyası almak için **İndir'i** seçin.

    ![Toplu davet indirme düğmesi](media/tutorial-bulk-invite/bulk-invite-button.png)

5. .csv dosyasını açın ve her konuk kullanıcı için bir satır ekleyin. Gerekli değerler şunlardır:

   * **Davet edilecek e-posta adresi** - davet alacak kullanıcı

   * **Yeniden yönlendirme url'si** - davet edilen kullanıcının daveti kabul ettikten sonra iletildiği URL

    ![Konuk kullanıcıların girdiği bir CSV dosyası örneği](media/tutorial-bulk-invite/bulk-invite-csv.png)

   > [!NOTE]
   > **Özelleştirilmiş davet iletisinde** virgül kullanmayın, çünkü iletinin başarıyla ayrıştırılmasını engeller.

6. Dosyayı kaydedin.
7. Toplu **davet kullanıcıları** sayfasında, **csv dosyanızı yükleyin**altında, dosyaya göz atın. Dosyayı seçtiğinizde,.csv dosyasının doğrulanması başlar. 
8. Dosya içeriği doğrulandığında, **Dosyanın başarıyla yüklendiğini**görürsünüz. Hatalar varsa, işi göndermeden önce bunları düzeltmeniz gerekir.
9. Dosyanız doğrulamadan geçtiğinde, davetleri ekleyen Azure toplu işlemini başlatmak için **Gönder'i** seçin. 
10. İş durumunu görüntülemek **için, her işlemin durumunu görüntülemek için burayı tıklatın'ı**seçin. Veya **Etkinlik** bölümünde **Toplu işlem sonuçlarını** seçebilirsiniz. Toplu işlemdeki her satır öğesi yle ilgili ayrıntılar için **# Success**, **# Failure**veya Toplam **İstek** sütunları altındaki değerleri seçin. Hatalar oluştuysa, hata nedenleri listelenir.

    ![Toplu işlem sonuçları örneği](media/tutorial-bulk-invite/bulk-operation-results.png)

11. İş tamamlandığında, toplu işlemin başarılı olduğuna dair bir bildirim görürsünüz.

## <a name="verify-guest-users-in-the-directory"></a>Dizindeki konuk kullanıcıları doğrulama

Eklediğiniz konuk kullanıcıların Azure portalında veya PowerShell'i kullanarak dizinde bulunabiliyor mu olup olmadığını denetleyin.

### <a name="view-guest-users-in-the-azure-portal"></a>Azure portalında konuk kullanıcıları görüntüleme

1. Kuruluşta Kullanıcı yöneticisi olan bir hesapla Azure portalında oturum açın.
2. Gezinti bölmesinde Azure **Etkin Dizin'i**seçin.
3. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
4. **Göster'in**altında yalnızca **Konuk kullanıcıları** seçin ve eklediğiniz kullanıcıların listeli olduğunu doğrulayın.

### <a name="view-guest-users-with-powershell"></a>PowerShell ile konuk kullanıcıları görüntüleyin

Şu komutu çalıştırın:

```powershell
 Get-AzureADUser -Filter "UserType eq 'Guest'"
```

Listelediğiniz kullanıcıları, *e-posta adresi*#EXT#\@*etki alanında*bir kullanıcı ana adı (UPN) ile görmeniz gerekir. Örneğin, *lstokes_fabrikam.com#EXT#\@contoso.onmicrosoft.com*, davetiyeleri gönderdiğiniz kuruluş contoso.onmicrosoft.com yerdir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, konuk kullanıcının yanındaki onay kutusunu seçip **Sil'i**seçerek Kullanıcılar sayfasındaki Azure portalındaki dizindeki test kullanıcı hesaplarını silebilirsiniz. 

Veya bir kullanıcı hesabını silmek için aşağıdaki PowerShell komutunu çalıştırabilirsiniz:

```powershell
 Remove-AzureADUser -ObjectId "<UPN>"
```

Örneğin, `Remove-AzureADUser -ObjectId "lstokes_fabrikam.com#EXT#@contoso.onmicrosoft.com"`

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kuruluşunuz dışındaki konuk kullanıcılara toplu davetler göndereceksiniz. Daha sonra, davet kullanımı işleminin nasıl çalıştığını öğreneceksiniz.

> [!div class="nextstepaction"]
> [Azure AD B2B işbirliği daveti kullanım işlemi hakkında bilgi edinin](redemption-experience.md)
