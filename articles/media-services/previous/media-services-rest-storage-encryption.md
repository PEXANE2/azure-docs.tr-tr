---
title: AMS REST API kullanarak içeriğinizi depolama şifrelemesiyle şifreleme
description: AMS REST API'lerini kullanarak içeriğinizi depolama şifrelemesiyle nasıl şifreleriz öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: a0a79f3d-76a1-4994-9202-59b91a2230e0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 2a5ef1837375cc395a871f9a9860fa8bde572a94
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76773595"
---
# <a name="encrypting-your-content-with-storage-encryption"></a>İçeriğinizi depolama şifrelemesi ile şifreleme 

> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).   > Media Services v2'ye yeni özellikler veya işlevler eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın
>   

AES-256 bit şifreleme kullanarak içeriğinizi yerel olarak şifrelemeniz ve ardından güvenli bir şekilde şifrelendiği Azure Depolama'ya yüklemeniz önerilir.

Bu makalede, AMS depolama şifrelemesine genel bir bakış sunar ve depolama şifrelenmiş içeriğinasıl yükleyeceğimiz gösterilmektedir:

* Bir içerik anahtarı oluşturun.
* Bir Varlık Oluşturun. Varlık Oluşturma Seçeneğini, Varlık Oluştururken StorageEncryption'a ayarlayın.
  
     Şifrelenmiş varlıklar içerik anahtarlarıyla ilişkilidir.
* İçerik anahtarını varlığa bağla.  
* AssetFile varlıklarındaki şifrelemeyle ilgili parametreleri ayarlayın.

## <a name="considerations"></a>Dikkat edilmesi gerekenler 

Depolama şifreli bir varlık teslim etmek istiyorsanız, varlığın teslim ilkesini yapılandırmanız gerekir. Varlığınız akışa geçmeden önce, akış sunucusu depolama şifrelemesini kaldırır ve belirtilen teslim ilkesini kullanarak içeriğinizi akışı sağlar. Daha fazla bilgi için [bkz.](media-services-rest-configure-asset-delivery-policy.md)

Medya Hizmetleri'ndeki varlıklara erişirken, HTTP isteklerinizde belirli üstbilgi alanları ve değerleri belirlemeniz gerekir. Daha fazla bilgi için Medya [Hizmetleri REST API Geliştirme kurulumu'na](media-services-rest-how-to-use.md)bakın. 

### <a name="storage-side-encryption"></a>Depolama tarafı şifreleme

|Şifreleme seçeneği|Açıklama|Media Services v2|Media Services v3|
|---|---|---|---|
|Medya Hizmetleri Depolama Şifrelemesi|AES-256 şifreleme, Medya Hizmetleri tarafından yönetilen anahtar|Desteklenen<sup>(1)</sup>|Desteklenmiyor<sup>(2)</sup>|
|[Veriler için Depolama Hizmeti Şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Azure Depolama tarafından sunulan sunucu tarafı şifrelemesi, Azure veya müşteri tarafından yönetilen anahtar|Destekleniyor|Destekleniyor|
|[Depolama İstemci Tarafı Şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Anahtar Vault'ta müşteri tarafından yönetilen anahtar olan Azure depolama tarafından sunulan istemci tarafı şifrelemesi|Desteklenmiyor|Desteklenmiyor|

<sup>1</sup> Medya Hizmetleri içeriğin açık/net bir şekilde işlenmesini desteklerken, bunu yapmak önerilmez.

<sup>2</sup> Medya Hizmetleri v3'te depolama şifrelemesi (AES-256 şifrelemesi), yalnızca Varlıklarınız Medya Hizmetleri v2 ile oluşturulduğunda geriye dönük uyumluluk için desteklenir. Yani v3 varolan depolama şifreli varlıklarla çalışır, ancak yenilerinin oluşturulmasına izin vermez.

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'sine nasıl bağlanabileceğiniz hakkında bilgi için Azure [AD kimlik doğrulaması yla Azure Medya Hizmetleri API'sine eriş'e](media-services-use-aad-auth-to-access-ams-api.md)bakın. 

## <a name="storage-encryption-overview"></a>Depolama şifrelemesi genel bakış
AMS depolama şifrelemesi tüm dosyaya **AES-TO** modu şifrelemesi uygular.  AES-TO modu, dolguya gerek kalmadan rasgele uzunluktaki verileri şifreleyebilen bir blok şifrelemesidir. AES algoritması ile bir sayaç bloğunu şifreleyerek ve ardından AES çıktısını şifrelemek veya şifresini çözmek için verilerle XOR-ing ile çalışır.  Kullanılan sayaç bloğu, InitializationVector değerinin 0-7 sayaç değerine kopyalanmasıyla oluşturulur ve sayaç değerinin 8-15'inin baytları sıfıra ayarlanır. 16 baytlık sayaç bloğundan 8 ila 15 bayt (yani en az önemli baytlar), sonraki her veri bloğu için bir sayı yla artıp işlenen ve ağ bayt sırasına göre tutulan basit bir 64 bit imzasız tamsayı olarak kullanılır. Bu tamsayı maksimum değere ulaşırsa (0xFFFFFFFFFFFFFFFFFFFFFFFFFFF) daha sonra, sayaç takidiğer 64 bitini (yani 0'dan 7'ye bayt) etkilemeden blok sayacını sıfırlayarak sıfırlar (bayt 8-15).   AES-TO modu şifrelemesinin güvenliğini sağlamak için, her içerik anahtarı için belirli bir Anahtar Tanımlayıcısı için InitializationVector değeri benzersiz olacaktır ve dosyalar 2^64 bloktan daha kısa uzunlukta olacaktır.  Bu benzersiz değer, sayaç değerinin belirli bir anahtarla hiçbir zaman yeniden kullanılmadığından emin olmak tır. TO modu hakkında daha fazla bilgi için [bu wiki sayfasına](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) bakın (wiki makalesi "InitializationVector" yerine "Nonce" terimini kullanır).

Temiz içeriğinizi AES-256 bit şifrelemekullanarak yerel olarak şifrelemek için **Depolama Şifreleme'yi** kullanın ve ardından güvenli bir şekilde şifrelendiği Azure Depolama'ya yükleyin. Depolama şifrelemesi ile korunan varlıklar otomatik olarak şifrelenmez ve kodlamadan önce şifrelenmiş bir dosya sistemine yerleştirilir ve isteğe bağlı olarak yeni bir çıktı varlığı olarak geri yüklemeden önce yeniden şifrelenir. Depolama şifrelemesi için birincil kullanım örneği, yüksek kaliteli giriş ortam dosyalarınızı diskte güçlü şifreleme yle güvence altına almak istediğinizde olur.

Depolama şifreli bir varlık sunmak için, Medya Hizmetleri'nin içeriğinizi nasıl teslim etmek istediğinizi bilmesi için varlığın teslim politikasını yapılandırmanız gerekir. Varlığınız akışa geçmeden önce, akış sunucusu depolama şifrelemesini kaldırır ve belirtilen teslim ilkesini (örneğin, AES, ortak şifreleme veya şifreleme olmadan) kullanarak içeriğinizi akışı sağlar.

## <a name="create-contentkeys-used-for-encryption"></a>Şifreleme için kullanılan İçerikTuşlarını Oluşturma
Şifrelenmiş varlıklar Depolama Şifreleme anahtarları ile ilişkilidir. Varlık dosyalarını oluşturmadan önce şifreleme için kullanılacak içerik anahtarını oluşturun. Bu bölümde içerik anahtarının nasıl oluşturulacak olduğu açıklanmaktadır.

Aşağıda, şifrelenmek istediğiniz varlıklarla ilişkilendirdiğiniz içerik anahtarları oluşturmak için genel adımlar veremeniz gerekir. 

1. Depolama şifrelemesi için rasgele 32 baytlık bir AES anahtarı oluşturun. 
   
    32 baytlık AES Anahtarı, kıymetinizin içerik anahtarıdır, bu da söz le ilişkili tüm dosyaların şifre çözme sırasında aynı içerik anahtarını kullanması gerektiği anlamına gelir. 
2. İçerik anahtarınızı şifrelemek için kullanılması gereken doğru X.509 Sertifikasını almak için [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) ve [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) yöntemlerini arayın.
3. İçerik anahtarınızı X.509 Sertifikası'nın ortak anahtarıyla şifreleyin. 
   
   Medya Hizmetleri .NET SDK şifreleme yaparken OAEP ile RSA kullanır.  [EncryptSymmetricKeyData işlevinde](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs)bir .NET örneği görebilirsiniz.
4. Anahtar tanımlayıcısı ve içerik anahtarı kullanılarak hesaplanan bir çekum değeri oluşturun. Aşağıdaki .NET örneği, anahtar tanımlayıcının GUID kısmını ve net içerik anahtarını kullanarak çekumhesaplar.

    ```csharp
            public static string CalculateChecksum(byte[] contentKey, Guid keyId)
            {
                const int ChecksumLength = 8;
                const int KeyIdLength = 16;

                byte[] encryptedKeyId = null;

                // Checksum is computed by AES-ECB encrypting the KID
                // with the content key.
                using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
                {
                    rijndael.Mode = CipherMode.ECB;
                    rijndael.Key = contentKey;
                    rijndael.Padding = PaddingMode.None;

                    ICryptoTransform encryptor = rijndael.CreateEncryptor();
                    encryptedKeyId = new byte[KeyIdLength];
                    encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
                }

                byte[] retVal = new byte[ChecksumLength];
                Array.Copy(encryptedKeyId, retVal, ChecksumLength);

                return Convert.ToBase64String(retVal);
            }
    ```

5. Önceki adımlarda aldığınız **EncryptedContentKey** (base64 kodlanmış dizeye dönüştürülür), **ProtectionKeyId,** **ProtectionKeyType,** **ContentKeyType**ve **Checksum** değerleri ile İçerik anahtarını oluşturun.

    Depolama şifrelemesi için, aşağıdaki özelliklerin istek gövdesine dahil edilmesi gerekir.

    Gövde özelliği isteğinde bulunun    | Açıklama
    ---|---
    Kimlik | ContentKey ID aşağıdaki biçimde oluşturulur, "nb:kid:UUID:\<Yenİ GUID>".
    İçerikKeyType | İçerik anahtar türü, anahtarı tanımlayan bir karşıcıdır. Depolama şifreleme biçimi için değer 1'dir.
    ŞifreliİçerikAnahtar | 256 bit (32 bayt) değerinde yeni bir içerik anahtar değeri oluşturuyoruz. Anahtar, GetProtectionKeyId ve GetProtectionKey Yöntemleri için http GET isteği gerçekleştirerek Microsoft Azure Medya Hizmetleri'nden aldığımız depolama şifrelemex.509 sertifikası kullanılarak şifrelenir. Örnek olarak, aşağıdaki .NET koduna bakın: [Burada](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs)tanımlanan **EncryptSymmetricKeyData** yöntemi.
    KorumaKeyid | Bu, içerik anahtarımızı şifrelemek için kullanılan depolama şifreleme X.509 sertifikasının koruma anahtarı kimliğidir.
    ProtectionKeyType | Bu, içerik anahtarını şifrelemek için kullanılan koruma anahtarının şifreleme türüdür. Bu değer, örneğimiz için StorageEncryption(1) değeridir.
    Sağlama |MD5, içerik anahtarı için hesaplanan checksum'u hesapladı. İçerik numarası içerik anahtarı ile şifrelenerek hesaplanır. Örnek kod, checksum'un nasıl hesaplanacağını gösterir.


### <a name="retrieve-the-protectionkeyid"></a>ProtectionKeyId'i alın
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

### <a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>ProtectionKeyId için ProtectionKey'i alın
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

### <a name="create-the-content-key"></a>İçerik anahtarını oluşturma
X.509 sertifikasını aldıktan ve içerik anahtarınızı şifrelemek için ortak anahtarını kullandıktan sonra, bir **ContentKey** tüzel kişiliği oluşturun ve özellik değerlerini buna göre ayarlayın.

İçerik anahtarını oluştururken ayarlamanız gereken değerlerden biri de türdür. Depolama şifrelemesi kullanırken, değer '1' olarak ayarlanmalıdır. 

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

## <a name="create-an-asset"></a>Varlık oluşturma
Aşağıdaki örnekte, bir varlığın nasıl oluşturulabildiğini gösterilmektedir.

**HTTP İsteği**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.19
    Host: media.windows.net

    {"Name":"BigBuckBunny" "Options":1}

**HTTP Yanıt**

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

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

## <a name="create-an-assetfile"></a>Varlık Dosyası Oluşturma
[AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) varlığı, bir blob kapsayıcısında depolanan bir video veya ses dosyasını temsil eder. Kıymet dosyası her zaman bir varlıkla ilişkilidir ve bir varlık bir veya birden çok varlık dosyası içerebilir. Bir varlık dosyası nesnesi bir blob kapsayıcısındaki dijital bir dosyayla ilişkilendirilmezse, Medya Hizmetleri Encoder görevi başarısız olur.

**AssetFile** örneği ve gerçek ortam dosyası iki farklı nesnedir. AssetFile örneği medya dosyası yla ilgili meta verileri içerirken, ortam dosyası gerçek medya içeriğini içerir.

Dijital medya dosyanızı bir blob kapsayıcısına yükledikten sonra, AssetFile'ı medya dosyanızla ilgili bilgilerle (bu makalede gösterilmez) güncelleştirmek için **BIRLEŞTIRME** HTTP isteğini kullanırsınız. 

**HTTP İsteği**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.19
    Host: media.windows.net
    Content-Length: 164

    {  
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }

**HTTP Yanıt**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
