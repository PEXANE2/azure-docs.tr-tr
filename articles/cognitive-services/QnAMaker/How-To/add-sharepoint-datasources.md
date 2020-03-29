---
title: SharePoint dosyaları - QnA Maker
description: Active Directory ile güvence altına alınabilecek sorular ve yanıtlarla bilgi tabanını zenginleştirmek için bilgi tabanınıza güvenli SharePoint veri kaynakları ekleyin.
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 6f05079e39c8afb001bd4ba09d68f435c18efad5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294871"
---
# <a name="add-a-secured-sharepoint-data-source-to-your-knowledge-base"></a>Bilgi tabanınıza güvenli bir SharePoint veri kaynağı ekleyin

Bilgi tabanını Active Directory ile güvence altına alabilecek sorular ve yanıtlarla zenginleştirmek için güvenli bulut tabanlı SharePoint veri kaynaklarını bilgi tabanınıza ekleyin.

QnA Maker yöneticisi olarak bilgi tabanınıza güvenli bir SharePoint belgesi eklediğinizde, QnA Maker için Etkin Dizin izni istemeniz gerekir. Bu izin, SharePoint erişimi için QnA Maker'a Active Directory yöneticisinden verildikten sonra, yeniden verilmesi gerekmez. Bilgi tabanına eklenen her belge, aynı SharePoint kaynağındaysa yetkilendirmeye gerek duymaz.

QnA Maker bilgi bankası yöneticisi Active Directory yöneticisi değilse, bu işlemi tamamlamak için Active Directory yöneticisiyle iletişim kurmanız gerekir.

## <a name="prerequisites"></a>Ön koşullar

* Bulut tabanlı SharePoint - QnA Maker izinler için Microsoft Graph kullanır. SharePoint'iniz şirket içindeyse, Microsoft Graph izinleri belirleyemeyeceğinden SharePoint'ten çıkaramazsınız.
* URL formatı - QnA Maker yalnızca paylaşım için oluşturulan ve biçime uygun olan SharePoint url'lerini destekler`https://\*.sharepoint.com`

## <a name="add-supported-file-types-to-knowledge-base"></a>Desteklenen dosya türlerini bilgi tabanına ekleme

Bir SharePoint sitesinden tüm QnA Maker destekli [dosya türlerini](../Concepts/content-types.md) bilgi tabanınıza ekleyebilirsiniz. Dosya kaynağı güvenliyse [izin vermeniz](#permissions) gerekebilir.

1. SharePoint sitesinin bulunduğu kitaplıktan, dosyanın elips menüsünü seçin. `...`
1. Dosyanın URL'sini kopyalayın.

   ![Dosyanın elips menüsünü seçerek sharepoint dosya URL'sini alın ve ardından URL'yi kopyalayın.](../media/add-sharepoint-datasources/get-sharepoint-file-url.png)

1. QnA Maker portalında, **Ayarlar** sayfasında, [URL'yi](manage-knowledge-bases.md#edit-knowledge-base) bilgi tabanına ekleyin.

### <a name="images-with-sharepoint-files"></a>SharePoint dosyaları yla görüntüler

Dosyalarda resim varsa, bunlar ayıklanmaz. Dosya QnA çiftleri içine ayıklandıktan sonra görüntüyü QnA Maker portalından ekleyebilirsiniz.

Aşağıdaki işaretleme sözdizimi ile görüntü ekleyin:

```markdown
![Explanation or description of image](URL of public image)
```

Kare köşeli ayraçlarda `[]`metin, görüntüyü açıklar. Parantez içinde URL, `()`görüntüye doğrudan bağlantıdır.

QnA çiftini etkileşimli test panelinde, QnA Maker portalında test ettiğinizde, görüntü işaretleme metni yerine görüntülenir. Bu, görüntünün istemci uygulamanızdan genel olarak alınabileceğini doğrular.

## <a name="permissions"></a>İzinler

İzin verme, sharepoint sunucusundan güvenli bir dosya bilgi tabanına eklendiğinde gerçekleşir. SharePoint'in nasıl ayarlıştırılabileceğine ve dosyayı ekleyen kişinin izinlerine bağlı olarak, bu şunları gerektirebilir:

* ek adım yok - dosyayı ekleyen kişi gereken tüm izinlere sahiptir.
* hem [bilgi bankası yöneticisi](#knowledge-base-manager-add-sharepoint-data-source-in-qna-maker-portal) hem de Active [Directory yöneticisi](#active-directory-manager-grant-file-read-access-to-qna-maker)tarafından adımlarla.

Aşağıda listelenen adımlara bakın.

### <a name="knowledge-base-manager-add-sharepoint-data-source-in-qna-maker-portal"></a>Bilgi bankası yöneticisi: QnA Maker portalına SharePoint veri kaynağı ekleyin

**QnA Maker yöneticisi** bir bilgi tabanına güvenli bir SharePoint belgesi eklediğinde, bilgi bankası yöneticisi Active Directory yöneticisinin tamamlaması gereken bir izin isteği başlatır.

İstek, Active Directory hesabına kimlik doğrulamak için açılan pencereyle başlar.

![Kullanıcı Hesabını Doğrula](../media/add-sharepoint-datasources/authenticate-user-account.png)

QnA Maker yöneticisi hesabı seçtikten sonra, Azure Active Directory yöneticisi, QnA Maker uygulamasının (QnA Maker yöneticisi değil) SharePoint kaynağına erişmesine izin vermek için gereken bir bildirim alır. Azure Etkin Dizin yöneticisinin bunu her SharePoint kaynağı için yapması gerekir, ancak bu kaynaktaki her belgeyi yapmaz.

### <a name="active-directory-manager-grant-file-read-access-to-qna-maker"></a>Etkin dizin yöneticisi: QnA Maker'a dosya okuma izni

Active Directory yöneticisinin (QnA Maker yöneticisi değil) QnA Maker'a sharepoint kaynağına erişmek için bu [bağlantıyı](https://login.microsoftonline.com/common/oauth2/v2.0/authorize?response_type=id_token&scope=Files.Read%20Files.Read.All%20Sites.Read.All%20User.Read%20User.ReadBasic.All%20profile%20openid%20email&client_id=c2c11949-e9bb-4035-bda8-59542eb907a6&redirect_uri=https%3A%2F%2Fwww.qnamaker.ai%3A%2FCreate&state=68) seçerek QnA Maker Portal SharePoint kurumsal uygulamasına dosya okuma izinleri için yetki vermesi gerekir.

![Azure Active Directory yöneticisi interaktif olarak izin verir](../media/add-sharepoint-datasources/aad-manager-grants-permission-interactively.png)

<!--
The Active Directory manager must grant QnA Maker access either by application name, `QnAMakerPortalSharePoint`, or by application ID, `c2c11949-e9bb-4035-bda8-59542eb907a6`.
-->
<!--
### Grant access from the interactive pop-up window

The Active Directory manager will get a pop-up window requesting permissions to the `QnAMakerPortalSharePoint` app. The pop-up window includes the QnA Maker Manager email address that initiated the request, an `App Info` link to learn more about **QnAMakerPortalSharePoint**, and a list of permissions requested. Select **Accept** to provide those permissions.

![Azure Active Directory manager grants permission interactively](../media/add-sharepoint-datasources/aad-manager-grants-permission-interactively.png)
-->
<!--

### Grant access from the App Registrations list

1. The Active Directory manager signs in to the Azure portal and opens **[App registrations list](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ApplicationsListBlade)**.

1. Search for and select the **QnAMakerPortalSharePoint** app. Change the second filter box from **My apps** to **All apps**. The app information will open on the right side.

    ![Select QnA Maker app in App registrations list](../media/add-sharepoint-datasources/select-qna-maker-app-in-app-registrations.png)

1. Select **Settings**.

    [![Select Settings in the right-side blade](../media/add-sharepoint-datasources/select-settings-for-qna-maker-app-registration.png)](../media/add-sharepoint-datasources/select-settings-for-qna-maker-app-registration.png#lightbox)

1. Under **API access**, select **Required permissions**.

    ![Select 'Settings', then under 'API access', select 'Required permission'](../media/add-sharepoint-datasources/select-required-permissions-in-settings-blade.png)

1. Do not change any settings in the **Enable Access** window. Select **Grant Permission**.

    [![Under 'Grant Permission', select 'Yes'](../media/add-sharepoint-datasources/grant-app-required-permissions.png)](../media/add-sharepoint-datasources/grant-app-required-permissions.png#lightbox)

1. Select **YES** in the pop-up confirmation windows.

    ![Grant required permissions](../media/add-sharepoint-datasources/grant-required-permissions.png)
-->
### <a name="grant-access-from-the-azure-active-directory-admin-center"></a>Azure Active Directory yönetici merkezinden erişim izni verme

1. Active Directory yöneticisi Azure portalında imzalar ve **[Kurumsal uygulamaları](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps)** açar.

1. `QnAMakerPortalSharePoint` QnA Maker uygulamasını seçin.

    [![Kurumsal uygulamalar listesinde QnAMakerPortalSharePoint ara](../media/add-sharepoint-datasources/search-enterprise-apps-for-qna-maker.png)](../media/add-sharepoint-datasources/search-enterprise-apps-for-qna-maker.png#lightbox)

1. **Güvenlik**altında, **İzinler**gidin. **Organizasyon için Hibe yönetici onayı**seçin.

    [![Active Directory Admin için kimlik doğrulaması kullanıcıseçin](../media/add-sharepoint-datasources/grant-aad-permissions-to-enterprise-app.png)](../media/add-sharepoint-datasources/grant-aad-permissions-to-enterprise-app.png#lightbox)

1. Active Directory için izin verme izinleri olan bir Oturum Açma hesabı seçin.



<!--

## Add SharePoint data source with APIs

You need to get the SharePoint file's URI before adding it to QnA Maker.

## Get SharePoint File URI

Use the following steps to transform the SharePoint URL into a sharing token.

1. Encode the URL using [base64](https://en.wikipedia.org/wiki/Base64).

1. Convert the base64-encoded result to an unpadded base64url format with the following character changes.

    * Remove the equal character, `=` from the end of the value.
    * Replace `/` with `_`.
    * Replace `+` with `-`.
    * Append `u!` to be beginning of the string.

1. Sign in to Graph explorer and run the following query, where `sharedURL` is ...:

    ```
    https://graph.microsoft.com/v1.0/shares/<sharedURL>/driveitem
    ```

    Get the **@microsoft.graph.downloadUrl** and use this as `fileuri` in the QnA Maker APIs.

### Add or update a SharePoint File URI to your knowledge base

Use the **@microsoft.graph.downloadUrl** from the previous section as the `fileuri` in the QnA Maker API for [adding a knowledge base](https://go.microsoft.com/fwlink/?linkid=2092179) or [updating a knowledge base](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update). The following fields are mandatory: name, fileuri, filename, source.

```
{
    "name": "Knowledge base name",
    "files": [
        {
            "fileUri": "<@microsoft.graph.downloadURL>",
            "fileName": "filename.xlsx",
            "source": "<SharePoint link>"
        }
    ],
    "urls": [],
    "users": [],
    "hostUrl": "",
    "qnaList": []
}
```



## Remove QnA Maker app from SharePoint authorization

1. Use the steps in the previous section to find the Qna Maker app in the Active Directory admin center.
1. When you select the **QnAMakerPortalSharePoint**, select **Overview**.
1. Select **Delete** to remove permissions.

-->

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi tabanınızda işbirliği yapın](collaborate-knowledge-base.md)
