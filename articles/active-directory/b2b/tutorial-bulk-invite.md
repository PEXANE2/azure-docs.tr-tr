---
title: B2B işbirliği kullanıcılarına toplu davet etme öğreticisi-Azure AD
description: Bu öğreticide, harici Azure AD B2B işbirliği kullanıcılarına toplu davet göndermek için PowerShell ve CSV dosyasının nasıl kullanılacağını öğreneceksiniz.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: tutorial
ms.date: 2/11/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c43f1fe0d5850b53f0f72f05633e498a94d871c
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77149313"
---
# <a name="tutorial-bulk-invite-azure-ad-b2b-collaboration-users-preview"></a>Öğretici: Azure AD B2B işbirliği kullanıcılarını toplu davet etme (Önizleme)

|     |
| --- |
| Bu makalede, Azure Active Directory genel önizleme özelliği açıklanır. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

> [!NOTE]
> 12/22/2019 itibariyle, toplu davet kullanıcıları (Önizleme) özelliği geçici olarak devre dışı bırakılmıştır.
> Şu anda bu özelliğin Azure portal için yeniden etkinleştirileceği bilinen bir tarih yok. PowerShell kullanarak Konuk kullanıcıları toplu olarak davet etmek için bkz. [B2B kodu ve PowerShell örnekleri](code-samples.md).

Şirket dışındaki ortaklarla çalışmak için Azure Active Directory (Azure AD) B2B işbirliğini kullanıyorsanız, kuruluşunuza aynı anda birden çok konuk kullanıcı davet edebilirsiniz. Bu öğreticide, dış kullanıcılara toplu davetiye göndermek için Azure portal kullanmayı öğreneceksiniz. Özellikle aşağıdakileri yapın:

> [!div class="checklist"]
> * Kullanıcı bilgileri ve davetiye tercihleri ile bir virgülle ayrılmış değer (. csv) dosyası hazırlamak için **toplu davet kullanıcılarını (Önizleme)** kullanın
> * . Csv dosyasını Azure AD 'ye yükleyin
> * Kullanıcıların dizine eklendiğini doğrulama

Azure Active Directory yoksa, başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

Davetleri gönderebileceğiniz iki veya daha fazla test e-posta hesabı olması gerekir. Hesaplar, kuruluşunuzun dışından olmalıdır. Gmail.com veya outlook.com adresleri gibi sosyal hesapları içeren herhangi bir hesap türü kullanabilirsiniz.

## <a name="invite-guest-users-in-bulk"></a>Konuk kullanıcıları toplu olarak davet etme

1. Kuruluşta Kullanıcı Yöneticisi olan bir hesapla Azure portal oturum açın.
2. Gezinti bölmesinde **Azure Active Directory**' yi seçin.
3. **Yönet**altında, **toplu davet** > **Kullanıcılar** ' ı seçin.
4. **Toplu davet kullanıcıları (Önizleme)** sayfasında, davet özelliklerine sahip geçerli bir. csv dosyası almak için **İndir** ' i seçin.

    ![Toplu davet indirme düğmesi](media/tutorial-bulk-invite/bulk-invite-button.png)

5. . Csv dosyasını açın ve her Konuk Kullanıcı için bir satır ekleyin. Gerekli değerler şunlardır:

   * **Davet e-posta adresi** -davet alacak Kullanıcı

   * **Yeniden yönlendirme URL 'si** -daveti kabul ettikten sonra davet edilen kullanıcının iletildiği URL

    ![Konuk kullanıcılar için girilen bir CSV dosyası örneği](media/tutorial-bulk-invite/bulk-invite-csv.png)

   > [!NOTE]
   > İletinin başarıyla ayrıştırılmasını engelleyecek şekilde **özelleştirilmiş davet iletisinde** virgül kullanmayın.

6. Dosyayı kaydedin.
7. **Toplu davet kullanıcıları (Önizleme)** sayfasında, **CSV dosyanızı karşıya yükleyin**bölümünde dosyaya gidin. Dosyayı seçtiğinizde,. csv dosyasının doğrulanması başlar. 
8. Dosya içeriği doğrulandığında, **dosyanın başarıyla karşıya yüklendiğini**görürsünüz. Hatalar varsa, işi gönderebilmeniz için önce bunları çözmeniz gerekir.
9. Dosyanız doğrulamayı geçtiğinde, davetleri ekleyen Azure toplu işlemini başlatmak için **Gönder** ' i seçin. 
10. İş durumunu görüntülemek için, **her bir işlemin durumunu görüntülemek üzere buraya tıklayın ' ı**seçin. Ya da **etkinlik** bölümünde **toplu Işlem sonuçları (Önizleme)** seçeneğini belirleyebilirsiniz. Toplu işlemdeki her bir satır öğesi hakkında ayrıntılar için **# Success**, **# Failure**veya **Total Requests** sütunlarının altındaki değerleri seçin. Hatalar oluştuysa, başarısızlık nedenleri listelenecektir.

    ![Toplu işlem sonuçları örneği](media/tutorial-bulk-invite/bulk-operation-results.png)

11. İş tamamlandığında toplu işlemin başarılı olduğunu belirten bir bildirim görürsünüz.

## <a name="verify-guest-users-in-the-directory"></a>Dizindeki Konuk kullanıcıları doğrulama

Eklediğiniz konuk kullanıcıların Azure portal dizinde mı yoksa PowerShell kullanarak mı bulunduğunu denetleyin.

### <a name="view-guest-users-in-the-azure-portal"></a>Azure portal Konuk kullanıcıları görüntüleme

1. Kuruluşta Kullanıcı Yöneticisi olan bir hesapla Azure portal oturum açın.
2. Gezinti bölmesinde **Azure Active Directory**' yi seçin.
3. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
4. **Göster**altında **Yalnızca Konuk kullanıcılar** ' ı seçin ve eklediğiniz kullanıcıların listelendiğini doğrulayın.

### <a name="view-guest-users-with-powershell"></a>PowerShell ile Konuk kullanıcıları görüntüleme

Şu komutu çalıştırın:

```powershell
 Get-AzureADUser -Filter "UserType eq 'Guest'"
```

Bir Kullanıcı asıl adı (UPN) olarak, davet ettiğiniz kullanıcıları *emadresi*#EXT #\@*etki alanı*biçiminde görmeniz gerekir. Örneğin, *lstokes_fabrikam. com # ext #\@contoso.onmicrosoft.com*, burada contoso.onmicrosoft.com, davetleri gönderdiğiniz kuruluştur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, Konuk kullanıcının yanındaki onay kutusunu seçip **Sil**' i seçerek kullanıcılar sayfasındaki Azure Portal dizindeki test Kullanıcı hesaplarını silebilirsiniz. 

Ya da bir kullanıcı hesabını silmek için aşağıdaki PowerShell komutunu çalıştırabilirsiniz:

```powershell
 Remove-AzureADUser -ObjectId "<UPN>"
```

Örneğin, `Remove-AzureADUser -ObjectId "lstokes_fabrikam.com#EXT#@contoso.onmicrosoft.com"`

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kuruluşunuz dışındaki konuk kullanıcılara toplu davetler göndereceksiniz. Daha sonra, davet kullanımı işleminin nasıl çalıştığını öğreneceksiniz.

> [!div class="nextstepaction"]
> [Azure AD B2B işbirliği daveti kullanım işlemi hakkında bilgi edinin](redemption-experience.md)
