---
title: REST ile içerik anahtarları oluşturma | Microsoft Dokümanlar
description: Bu makalede, varlıklara güvenli erişim sağlayan içerik anahtarlarının nasıl oluşturulacak gösteriş yaptığı gösterilmiştir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 95e9322b-168e-4a9d-8d5d-d7c946103745
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: d256f417fb3bacbf3f363fc2a9f8701a1bb49d71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76773636"
---
# <a name="create-content-keys-with-rest"></a>REST ile içerik anahtarları oluşturma
> [!div class="op_single_selector"]
> * [Geri kalanı](media-services-rest-create-contentkey.md)
> * [.NET](media-services-dotnet-create-contentkey.md)
> 
> 

Medya Hizmetleri, şifreli varlıkları teslim etmenizi sağlar. **ContentKey,** Varlık'ınıza **Asset**güvenli erişim sağlar. 

Yeni bir varlık oluşturduğunuzda (örneğin, [dosya yüklemeden](media-services-rest-upload-files.md)önce), aşağıdaki şifreleme seçeneklerini belirtebilirsiniz: **StorageEncrypted**, **CommonEncryptionProtected**veya **EnvelopeEncryptionProtected**. 

Müşterilerinize varlık teslim ettiğinizde, varlıkların aşağıdaki iki şifrelemeden biriyle dinamik olarak şifrelenecek şekilde [yapılandırabilirsiniz:](media-services-rest-configure-asset-delivery-policy.md) **DynamicEnvelopeEncryption** veya **DynamicCommonEncryption**.

Şifrelenmiş varlıkların **ContentKey**s ile ilişkilendirilmesi gerekir. Bu makalede, içerik anahtarının nasıl oluşturulacak olduğu açıklanmaktadır.

Aşağıda, şifrelenmek istediğiniz varlıklarla ilişkilendirdiğiniz içerik anahtarları oluşturmak için genel adımlar veremeniz gerekir. 

1. Rasgele 16 baytA anahtarı (ortak ve zarf şifreleme için) veya 32 baytlık AES anahtarı (depolama şifrelemesi için) oluşturun. 
   
    Bu, kıymetinizin içerik anahtarıdır, bu da söz le ilişkili tüm dosyaların şifre çözme sırasında aynı içerik anahtarını kullanması gerektiği anlamına gelir. 
2. İçerik anahtarınızı şifrelemek için kullanılması gereken doğru X.509 Sertifikasını almak için [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) ve [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) yöntemlerini arayın.
3. İçerik anahtarınızı X.509 Sertifikası'nın ortak anahtarıyla şifreleyin. 
   
   Medya Hizmetleri .NET SDK şifreleme yaparken OAEP ile RSA kullanır.  [EncryptSymmetricKeyData işlevinde](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs)bir örnek görebilirsiniz.
4. Anahtar tanımlayıcısı ve içerik anahtarı kullanılarak hesaplanan bir checksum değeri (PlayReady AES anahtar kontrol algoritmasını temel alınarak) oluşturun. Daha fazla bilgi için playready başlık nesnesi belgesinin "PlayReady AES Anahtar Checksum Algoritması" [bölümüne](https://www.microsoft.com/playready/documents/)bakın.
   
   Aşağıdaki .NET örneği, anahtar tanımlayıcının GUID kısmını ve net içerik anahtarını kullanarak çekumhesaplar.
   
        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
         {
 
             byte[] array = null;
             using (AesCryptoServiceProvider aesCryptoServiceProvider = new AesCryptoServiceProvider())
             {
                 aesCryptoServiceProvider.Mode = CipherMode.ECB;
                 aesCryptoServiceProvider.Key = contentKey;
                 aesCryptoServiceProvider.Padding = PaddingMode.None;
                 ICryptoTransform cryptoTransform = aesCryptoServiceProvider.CreateEncryptor();
                 array = new byte[16];
                 cryptoTransform.TransformBlock(keyId.ToByteArray(), 0, 16, array, 0);
             }
             byte[] array2 = new byte[8];
             Array.Copy(array, array2, 8);
             return Convert.ToBase64String(array2);
         }
5. Önceki adımlarda aldığınız **EncryptedContentKey** (base64 kodlanmış dizeye dönüştürülür), **ProtectionKeyId,** **ProtectionKeyType,** **ContentKeyType**ve **Checksum** değerleri ile İçerik anahtarını oluşturun.
6. **ContentKey** varlığını $links işlemi yle **Varlık** varlığınızla ilişkilendirin.

Bu makalede, bir AES anahtarı oluşturmak için nasıl, anahtarı şifrelemek ve checksum hesaplamak için nasıl gösterilmez. 

> [!NOTE]
> 
> Medya Hizmetleri'ndeki varlıklara erişirken, HTTP isteklerinizde belirli üstbilgi alanları ve değerleri belirlemeniz gerekir. Daha fazla bilgi için Medya [Hizmetleri REST API Geliştirme kurulumu'na](media-services-rest-how-to-use.md)bakın.

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'sine nasıl bağlanabileceğiniz hakkında bilgi için Azure [AD kimlik doğrulaması yla Azure Medya Hizmetleri API'sine eriş'e](media-services-use-aad-auth-to-access-ams-api.md)bakın. 

## <a name="retrieve-the-protectionkeyid"></a>ProtectionKeyId'i alın
Aşağıdaki örnek, içerik anahtarınızı şifrelerken kullanmanız gereken sertifika için bir sertifika parmak izi olan ProtectionKeyId'in nasıl alındığını gösterir. Makinenizde zaten uygun sertifikaya sahip olduğundan emin olmak için bu adımı yapın.

İstek:

    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    Host: media.windows.net


Yanıt:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

## <a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>ProtectionKeyId için ProtectionKey'i alın
Aşağıdaki örnek, önceki adımda aldığınız ProtectionKeyId'i kullanarak X.509 sertifikasını nasıl alabileceğinizi gösterir.

İstek:

    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net



Yanıt:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

## <a name="create-the-contentkey"></a>ContentKey'i Oluşturun
X.509 sertifikasını aldıktan ve içerik anahtarınızı şifrelemek için ortak anahtarını kullandıktan sonra, bir **ContentKey** tüzel kişiliği oluşturun ve özellik değerlerini buna göre ayarlayın.

İçerik anahtarını oluştururken ayarlamanız gereken değerlerden biri de türdür. Aşağıdaki değerlerden birini seçin:

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }


Aşağıdaki örnek, depolama şifrelemesi için **ContentKeyType** kümesi ("1") ve koruma anahtarı kimliğinin X.509 sertifikası parmak izi olduğunu belirtmek için **ProtectionKeyType** kümesi "0" olan bir **ContentKey'in** nasıl oluşturultuğu gösterilmektedir.  

İstek

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }


Yanıt:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="associate-the-contentkey-with-an-asset"></a>ContentKey'i Bir Varlıkla Ilişkilendir
ContentKey'i oluşturduktan sonra, aşağıdaki örnekte gösterildiği gibi, $links işlemini kullanarak Kıymetinizle ilişkilendirin:

İstek:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    Host: media.windows.net


    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Yanıt:

    HTTP/1.1 204 No Content 


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

