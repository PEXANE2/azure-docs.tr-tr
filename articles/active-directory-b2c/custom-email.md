---
title: Özel e-posta doğrulamaları
titleSuffix: Azure AD B2C
description: Azure AD B2C özellikli uygulamalarınızı kullanmak için kaydolurken müşterilerinize gönderilen doğrulama e-postasını nasıl özelleştireceğiz öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/05/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 6cc0508a63f26b955ac5e0ebf3ef58a184a35997
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671643"
---
# <a name="custom-email-verification-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel e-posta doğrulaması

Uygulamalarınızı kullanmak için kaydolan kullanıcılara özel e-posta göndermek için Azure Active Directory B2C'de (Azure AD B2C) özel e-posta kullanın. [DisplayControls](display-controls.md) (şu anda önizlemede) ve bir üçüncü taraf e-posta sağlayıcısı nı kullanarak, kendi e-posta şablonunuzu ve *Adres* ve Konuyu kullanabilir, yerelleştirme ve özel tek kullanımlık parola (OTP) ayarlarını destekleyebilirsiniz.

Özel e-posta doğrulama [sendgrid](https://sendgrid.com) veya [SparkPost,](https://sparkpost.com)özel bir REST API veya herhangi bir HTTP tabanlı e-posta sağlayıcısı (kendi dahil) gibi bir üçüncü taraf e-posta sağlayıcısı nın kullanımını gerektirir. Bu makalede, SendGrid kullanan bir çözüm kurma açıklanmaktadır.

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-sendgrid-account"></a>SendGrid hesabı oluşturma

Zaten hesabınız yoksa, bir SendGrid hesabı oluşturarak başlayın (Azure müşterileri her ay 25.000 ücretsiz e-postanın kilidini açabilir). Kurulum yönergeleri için, Azure ile [SendGrid kullanarak e-posta gönderme nin Bir](../sendgrid-dotnet-how-to-send-email.md) [SendGrid Hesabı Oluştur](../sendgrid-dotnet-how-to-send-email.md#create-a-sendgrid-account) bölümüne bakın.

[SendGrid API anahtarı oluşturduğunuz](../sendgrid-dotnet-how-to-send-email.md#to-find-your-sendgrid-api-key)bölümü tamamladığınızdan emin olun. Daha sonraki bir adımda kullanılmak üzere API anahtarını kaydedin.

## <a name="create-azure-ad-b2c-policy-key"></a>Azure AD B2C ilke anahtarı oluşturma

Ardından, sendgrid API anahtarını ilkelerinizin başvurması için Azure AD B2C ilke anahtarında saklayın.

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve Azure AD B2C diziniseçin.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
1. Genel Bakış sayfasında Kimlik **Deneyimi Çerçevesi'ni**seçin.
1. **İlke Anahtarları'nı** seçin ve sonra **Ekle'yi**seçin.
1. **Seçenekler**için `Manual`, seçin.
1. İlke anahtarı için bir **Ad** girin. Örneğin, `SendGridSecret`. Önek `B2C_1A_` anahtarınızın adına otomatik olarak eklenir.
1. **Gizli**olarak, daha önce kaydettiğiniz müşteri sırrını girin.
1. **Anahtar kullanımı**için `Signature`.
1. **Oluştur'u**seçin.

## <a name="create-sendgrid-template"></a>SendGrid şablonu oluşturma

Oluşturulan SendGrid hesabı ve Azure AD B2C ilke anahtarında depolanan SendGrid API anahtarıyla, SendGrid [dinamik işlem şablonu](https://sendgrid.com/docs/ui/sending-email/how-to-send-an-email-with-dynamic-transactional-templates/)oluşturun.

1. SendGrid [sitesinde, işlem şablonları](https://sendgrid.com/dynamic_templates) sayfasını açın ve **Şablon Oluştur'u**seçin.
1. Gibi `Verification email` benzersiz bir şablon adı girin ve sonra **Kaydet'i**seçin.
1. Yeni şablonunuzu düzenlemeye başlamak için **Sürüm Ekle'yi**seçin.
1. **Kod Düzenleyicisi'ni** seçin ve ardından **Devam edin.**
1. HTML düzenleyicisinde, HTML şablonunu takip ederek yapıştırın veya kendi şablonunuzu kullanın. Ve `{{otp}}` `{{email}}` parametreler dinamik olarak tek seferlik parola değeri ve kullanıcı e-posta adresi ile değiştirilecektir.

    ```HTML
    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

    <html xmlns="http://www.w3.org/1999/xhtml" dir="ltr" lang="en"><head id="Head1">
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8"><title>Contoso demo account email verification code</title><meta name="ROBOTS" content="NOINDEX, NOFOLLOW">
       <!-- Template B O365 -->
       <style>
           table td {border-collapse:collapse;margin:0;padding:0;}
       </style>
    </head>
    <body dir="ltr" lang="en">
       <table width="100%" cellpadding="0" cellspacing="0" border="0" dir="ltr" lang="en">
            <tr>
               <td valign="top" width="50%"></td>
               <td valign="top">
                  <!-- Email Header -->
                  <table width="640" cellpadding="0" cellspacing="0" border="0" dir="ltr" lang="en" style="border-left:1px solid #e3e3e3;border-right: 1px solid #e3e3e3;">
                   <tr style="background-color: #0072C6;">
                       <td width="1" style="background:#0072C6; border-top:1px solid #e3e3e3;"></td>
                       <td width="24" style="border-top:1px solid #e3e3e3;border-bottom:1px solid #e3e3e3;">&nbsp;</td>
                       <td width="310" valign="middle" style="border-top:1px solid #e3e3e3; border-bottom:1px solid #e3e3e3;padding:12px 0;">
                           <h1 style="line-height:20pt;font-family:Segoe UI Light; font-size:18pt; color:#ffffff; font-weight:normal;">
                            <span id="HeaderPlaceholder_UserVerificationEmailHeader"><font color="#FFFFFF">Verify your email address</font></span>
                           </h1>
                       </td>
                       <td width="24" style="border-top: 1px solid #e3e3e3;border-bottom: 1px solid #e3e3e3;">&nbsp;</td>
                   </tr>
                  </table>
                  <!-- Email Content -->
                  <table width="640" cellpadding="0" cellspacing="0" border="0" dir="ltr" lang="en">
                   <tr>
                       <td width="1" style="background:#e3e3e3;"></td>
                       <td width="24">&nbsp;</td>
                       <td id="PageBody" width="640" valign="top" colspan="2" style="border-bottom:1px solid #e3e3e3;padding:10px 0 20px;border-bottom-style:hidden;">
                           <table cellpadding="0" cellspacing="0" border="0">
                               <tr>
                                   <td width="630" style="font-size:10pt; line-height:13pt; color:#000;">
                                       <table cellpadding="0" cellspacing="0" border="0" width="100%" style="" dir="ltr" lang="en">
                                           <tr>
                                               <td>

       <div style="font-family:'Segoe UI', Tahoma, sans-serif; font-size:14px; color:#333;">
           <span id="BodyPlaceholder_UserVerificationEmailBodySentence1">Thanks for verifying your {{email}} account!</span>
       </div>
       <br>
       <div style="font-family:'Segoe UI', Tahoma, sans-serif; font-size:14px; color:#333; font-weight: bold">
           <span id="BodyPlaceholder_UserVerificationEmailBodySentence2">Your code is: {{otp}}</span>
       </div>
       <br>
       <br>

                                                   <div style="font-family:'Segoe UI', Tahoma, sans-serif; font-size:14px; color:#333;">
                                                   Sincerely,
                                                   </div>
                                                   <div style="font-family:'Segoe UI', Tahoma, sans-serif; font-size:14px; font-style:italic; color:#333;">
                                                       Contoso
                                                   </div>
                                               </td>
                                           </tr>
                                       </table>
                                   </td>
                               </tr>
                           </table>

                       </td>

                       <td width="1">&nbsp;</td>
                       <td width="1"></td>
                       <td width="1">&nbsp;</td>
                       <td width="1" valign="top"></td>
                       <td width="29">&nbsp;</td>
                       <td width="1" style="background:#e3e3e3;"></td>
                   </tr>
                   <tr>
                       <td width="1" style="background:#e3e3e3; border-bottom:1px solid #e3e3e3;"></td>
                       <td width="24" style="border-bottom:1px solid #e3e3e3;">&nbsp;</td>
                       <td id="PageFooterContainer" width="585" valign="top" colspan="6" style="border-bottom:1px solid #e3e3e3;padding:0px;">


                       </td>

                       <td width="29" style="border-bottom:1px solid #e3e3e3;">&nbsp;</td>
                       <td width="1" style="background:#e3e3e3; border-bottom:1px solid #e3e3e3;"></td>
                   </tr>
                  </table>

               </td>
               <td valign="top" width="50%"></td>
           </tr>
       </table>
    <img src="https://mucp.api.account.microsoft.com/m/v2/v?d=AIAACWEPFYXYIUTJIJVV4ST7XLBHVI5MLLYBKJAVXHBDTBHUM5VBSVVPTTVRWDFIXJ5JQTHYOH5TUYIPO4ZAFRFK52UAMIS3UNIPPI7ZJNDZPRXD5VEJBN4H6RO3SPTBS6AJEEAJOUYL4APQX5RJUJOWGPKUABY&amp;i=AIAACL23GD2PFRFEY5YVM2XQLM5YYWMHFDZOCDXUI2B4LM7ETZQO473CVF22PT6WPGR5IIE6TCS6VGEKO5OZIONJWCDMRKWQQVNP5VBYAINF3S7STKYOVDJ4JF2XEW4QQVNHMAPQNHFV3KMR3V3BA4I36B6BO7L4VQUHQOI64EOWPLMG5RB3SIMEDEHPILXTF73ZYD3JT6MYOLAZJG7PJJCAXCZCQOEFVH5VCW2KBQOKRYISWQLRWAT7IINZ3EFGQI2CY2EMK3FQOXM7UI3R7CZ6D73IKDI" width="1" height="1"></body>
    </html>
    ```

1. **Soldaki Ayarları** Genişlet in ve **E-posta Konusu**için girin. `{{subject}}`
1. **Şablonu Kaydet'i**seçin.
1. Arka oku seçerek **İşlem Şablonları** sayfasına geri dönün.
1. Daha sonraki bir adımda kullanılmak üzere oluşturduğunuz şablonun **kimliğini** kaydedin. Örneğin, `d-989077fbba9746e89f3f6411f596fb96`. [Talep dönüştürmesini eklerken](#add-the-claims-transformation)bu kimliği belirtirsiniz.

## <a name="add-azure-ad-b2c-claim-types"></a>Azure AD B2C talep türleri ekleme

İlkenizde, içindeki `<BuildingBlocks>`öğeye aşağıdaki `<ClaimsSchema>` talep türlerini ekleyin.

Bu talep türleri, tek seferlik parola (OTP) kodu kullanarak e-posta adresini oluşturmak ve doğrulamak için gereklidir.

```XML
<ClaimType Id="Otp">
  <DisplayName>Secondary One-time password</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="sendGridReqBody">
  <DisplayName>SendGrid request body</DisplayName>
  <DataType>string</DataType>
</ClaimType>
<ClaimType Id="VerificationCode">
  <DisplayName>Secondary Verification Code</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Enter your email verification code</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

## <a name="add-the-claims-transformation"></a>Talep dönüştürmesini ekleme

Daha sonra, SendGrid'e gönderilen isteğin gövdesi olacak bir JSON dize iddiası çıktısı için bir talep dönüşümü ne gerekir.

JSON nesnesinin yapısı, Giriş Parametreleri'nin nokta gösterimindeki disler ve Giriş Taleplerinin DönüşümHakkı Türleri tarafından tanımlanır. Nokta gösterimindeki sayılar dizileri ima eder. Değerler, InputClaims'in değerlerinden ve InputParametrelerinin "Değer" özelliklerinden gelir. JSON talepleri dönüşümleri hakkında daha fazla bilgi için, [JSON talepleri dönüşümleri](json-transformations.md)bakın.

Aşağıdaki talep dönüşüm öğesini `<ClaimsTransformations>` `<BuildingBlocks>`ekleyin. Talep dönüşümü XML için aşağıdaki güncelleştirmeleri yapın:

* SendGrid `template_id` [şablonu oluştur'da](#create-sendgrid-template)daha önce oluşturduğunuz SendGrid işlem şablonunun kimliğiyle GirişParametre değerini güncelleştirin.
* Adres `from.email` değerini güncelleştirin. Doğrulama e-postasının spam olarak işaretlanmasını önlemeye yardımcı olmak için geçerli bir e-posta adresi kullanın.
* `personalizations.0.dynamic_template_data.subject` Konu satırı giriş parametresinin değerini kuruluşunuz için uygun bir konu satırıyla güncelleştirin.

```XML
<ClaimsTransformation Id="GenerateSendGridRequestBody" TransformationMethod="GenerateJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="personalizations.0.to.0.email" />
    <InputClaim ClaimTypeReferenceId="otp" TransformationClaimType="personalizations.0.dynamic_template_data.otp" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="personalizations.0.dynamic_template_data.email" />
  </InputClaims>
  <InputParameters>
    <!-- Update the template_id value with the ID of your SendGrid template. -->
    <InputParameter Id="template_id" DataType="string" Value="d-989077fbba9746e89f3f6411f596fb96"/>
    <InputParameter Id="from.email" DataType="string" Value="my_email@mydomain.com"/>
    <!-- Update with a subject line appropriate for your organization. -->
    <InputParameter Id="personalizations.0.dynamic_template_data.subject" DataType="string" Value="Contoso account email verification code"/>
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="sendGridReqBody" TransformationClaimType="outputClaim"/>
  </OutputClaims>
</ClaimsTransformation>
```

## <a name="add-datauri-content-definition"></a>DataUri içerik tanımı ekle

İçindeki `<BuildingBlocks>`talep dönüşümlerinin altında, sürüm 2.0.0 veri URI'ye başvurmak için aşağıdaki [ContentDefinition'ı](contentdefinitions.md) ekleyin:

```XML
<ContentDefinitions>
 <ContentDefinition Id="api.localaccountsignup">
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:2.0.0</DataUri>
  </ContentDefinition>
</ContentDefinitions>
```

## <a name="create-a-displaycontrol"></a>DisplayControl Oluşturma

Doğrulama görüntüleme denetimi, kullanıcıya gönderilen bir doğrulama koduyla e-posta adresini doğrulamak için kullanılır.

Bu örnek ekran denetimi şu şekilde yapılandırılır:

1. `email` Adres talep türünü kullanıcıdan toplayın.
1. Kullanıcının `verificationCode` kullanıcıya gönderilen kodla birlikte talep türünü sağlamasını bekleyin.
1. Bu `email` ekran denetimine atıfta bulunulan kendini öne süren teknik profile geri döndürün.
1. `SendCode` Eylemi kullanarak, bir OTP kodu oluşturun ve kullanıcıya OTP kodu içeren bir e-posta gönderin.

![Doğrulama kodu e-posta eylemi gönderme](media/custom-email/display-control-verification-email-action-01.png)

İçerik tanımları altında, `<BuildingBlocks>`hala içinde , poliçenize doğrulama denetimi türünden aşağıdaki [DisplayControl'ü](display-controls.md) ekleyin. [VerificationControl](display-control-verification.md)

```XML
<DisplayControls>
  <DisplayControl Id="emailVerificationControl" UserInterfaceControlType="VerificationControl">
    <DisplayClaims>
      <DisplayClaim ClaimTypeReferenceId="email" Required="true" />
      <DisplayClaim ClaimTypeReferenceId="verificationCode" ControlClaimType="VerificationCode" Required="true" />
    </DisplayClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="email" />
    </OutputClaims>
    <Actions>
      <Action Id="SendCode">
        <ValidationClaimsExchange>
          <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="GenerateOtp" />
          <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="SendGrid" />
        </ValidationClaimsExchange>
      </Action>
      <Action Id="VerifyCode">
        <ValidationClaimsExchange>
          <ValidationClaimsExchangeTechnicalProfile TechnicalProfileReferenceId="VerifyOtp" />
        </ValidationClaimsExchange>
      </Action>
    </Actions>
  </DisplayControl>
</DisplayControls>
```

## <a name="add-otp-technical-profiles"></a>OTP teknik profilleri ekleme

Teknik `GenerateOtp` profil, e-posta adresi için bir kod oluşturur. Teknik `VerifyOtp` profil, e-posta adresiyle ilişkili kodu doğrular. Biçimyapılandırmasını ve tek seferlik parolanın sona ermesini değiştirebilirsiniz. OTP teknik profilleri hakkında daha fazla bilgi için tek [seferlik parola teknik profilini tanımla'ya](one-time-password-technical-profile.md)bakın.

`<ClaimsProviders>` Öğeye aşağıdaki teknik profilleri ekleyin.

```XML
<ClaimsProvider>
  <DisplayName>One time password technical profiles</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="GenerateOtp">
      <DisplayName>Generate one time password</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="Operation">GenerateCode</Item>
        <Item Key="CodeExpirationInSeconds">1200</Item>
        <Item Key="CodeLength">6</Item>
        <Item Key="CharacterSet">0-9</Item>
        <Item Key="ReuseSameCode">true</Item>
        <Item Key="MaxNumAttempts">5</Item>
      </Metadata>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="identifier" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="otp" PartnerClaimType="otpGenerated" />
      </OutputClaims>
    </TechnicalProfile>

    <TechnicalProfile Id="VerifyOtp">
      <DisplayName>Verify one time password</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.OneTimePasswordProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="Operation">VerifyCode</Item>
      </Metadata>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="identifier" />
        <InputClaim ClaimTypeReferenceId="verificationCode" PartnerClaimType="otpToVerify" />
      </InputClaims>
    </TechnicalProfile>
   </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="add-a-rest-api-technical-profile"></a>REST API teknik profili ekle

Bu REST API teknik profili e-posta içeriğini oluşturur (SendGrid biçimini kullanarak). ReSTful teknik profilleri hakkında daha fazla bilgi için [bkz.](restful-technical-profile.md)

OTP teknik profillerinde olduğu gibi, `<ClaimsProviders>` öğeye aşağıdaki teknik profilleri ekleyin.

```XML
<ClaimsProvider>
  <DisplayName>RestfulProvider</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SendGrid">
      <DisplayName>Use SendGrid's email API to send the code the the user</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://api.sendgrid.com/v3/mail/send</Item>
        <Item Key="AuthenticationType">Bearer</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <Item Key="ClaimUsedForRequestPayload">sendGridReqBody</Item>
      </Metadata>
      <CryptographicKeys>
        <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_SendGridSecret" />
      </CryptographicKeys>
      <InputClaimsTransformations>
        <InputClaimsTransformation ReferenceId="GenerateSendGridRequestBody" />
      </InputClaimsTransformations>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="sendGridReqBody" />
      </InputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="make-a-reference-to-the-displaycontrol"></a>DisplayControl'e başvuruyapma

Son adımda, oluşturduğunuz DisplayControl'e bir başvuru ekleyin. Azure AD `LocalAccountSignUpWithLogonEmail` B2C ilkesinin önceki bir sürümünü kullandıysanız, varolan kendi kendini öne süren teknik profilinizi aşağıdakilerle değiştirin. Bu teknik `DisplayClaims` profil, DisplayControl'e atıfta bulunarak kullanır.

Daha fazla bilgi için [bkz.](restful-technical-profile.md) [DisplayControl](display-controls.md)

```XML
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
      <DisplayName>Email signup</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="IpAddressClaimReferenceId">IpAddress</Item>
        <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
        <Item Key="language.button_continue">Create</Item>
        
        <!--OTP validation error messages-->
        <Item Key="UserMessageIfSessionDoesNotExist">You have exceed the maximum time allowed.</Item>
        <Item Key="UserMessageIfMaxRetryAttempted">You have exceed the number of retries allowed.</Item>
        <Item Key="UserMessageIfInvalidCode">You have entered the wrong code.</Item>
        <Item Key="UserMessageIfSessionConflict">Cannot verify the code, please try again later.</Item>
      </Metadata>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="email" />
      </InputClaims>
      <DisplayClaims>
        <DisplayClaim DisplayControlReferenceId="emailVerificationControl" />
        <DisplayClaim ClaimTypeReferenceId="displayName" Required="true" />
        <DisplayClaim ClaimTypeReferenceId="givenName" Required="true" />
        <DisplayClaim ClaimTypeReferenceId="surName" Required="true" />
        <DisplayClaim ClaimTypeReferenceId="newPassword" Required="true" />
        <DisplayClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
      </DisplayClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="email" Required="true" />
        <OutputClaim ClaimTypeReferenceId="objectId" />
        <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" />
        <OutputClaim ClaimTypeReferenceId="newUser" />
      </OutputClaims>
      <ValidationTechnicalProfiles>
        <ValidationTechnicalProfile ReferenceId="AAD-UserWriteUsingLogonEmail" />
      </ValidationTechnicalProfiles>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="optional-localize-your-email"></a>[İsteğe bağlı] E-postanızı yerelleştirin

E-postayı yerelleştirmek için SendGrid'e veya e-posta sağlayıcınıza yerelleştirilmiş dizeleri göndermeniz gerekir. Örneğin, e-posta konusunu, gövdesini, kod iletinizi veya e-postanın imzasını yerelleştirmek için. Bunu yapmak için, yerelleştirilmiş dizeleri talep türlerine kopyalamak için [GetLocalizedStringsTransformation](string-transformations.md) talepleri dönüşümlerini kullanabilirsiniz. JSON `GenerateSendGridRequestBody` yükünü oluşturan talep dönüşümünde, yerelleştirilmiş dizeleri içeren giriş talepleri kullanır.

1. İlkenizde aşağıdaki dize taleplerini tanımlayın: özne, ileti, codeIntro ve imza.
1. Yerelleştirilmiş dize değerlerini adım 1'den gelen taleplere değiştirmek için [GetLocalizedStringsTransformation](string-transformations.md) talepleri dönüştürmesini tanımlayın.
1. Giriş `GenerateSendGridRequestBody` taleplerini kullanmak için talep dönüşümünü aşağıdaki XML snippet ile değiştirin.
1. Azure AD B2C tarafından yerelleştirilecek tüm dizeleri yerine dinamik parametreleri kullanmak için SendGrind şablonunuzu güncelleştirin.

```XML
<ClaimsTransformation Id="GenerateSendGridRequestBody" TransformationMethod="GenerateJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="personalizations.0.to.0.email" />
    <InputClaim ClaimTypeReferenceId="subject" TransformationClaimType="personalizations.0.dynamic_template_data.subject" />
    <InputClaim ClaimTypeReferenceId="otp" TransformationClaimType="personalizations.0.dynamic_template_data.otp" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="personalizations.0.dynamic_template_data.email" />
    <InputClaim ClaimTypeReferenceId="message" TransformationClaimType="personalizations.0.dynamic_template_data.message" />
    <InputClaim ClaimTypeReferenceId="codeIntro" TransformationClaimType="personalizations.0.dynamic_template_data.codeIntro" />
    <InputClaim ClaimTypeReferenceId="signature" TransformationClaimType="personalizations.0.dynamic_template_data.signature" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="template_id" DataType="string" Value="d-1234567890" />
    <InputParameter Id="from.email" DataType="string" Value="my_email@mydomain.com" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="sendGridReqBody" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

## <a name="next-steps"></a>Sonraki adımlar

GitHub'da özel bir e-posta doğrulama ilkesinin bir örneğini bulabilirsiniz:

[Özel e-posta doğrulaması - DisplayControls](https://github.com/azure-ad-b2c/samples/tree/master/policies/custom-email-verifcation-displaycontrol)

Özel bir REST API veya http tabanlı SMTP e-posta sağlayıcısı kullanma hakkında bilgi [için](restful-technical-profile.md)bkz.
