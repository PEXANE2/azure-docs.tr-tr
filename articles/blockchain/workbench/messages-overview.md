---
title: Azure Blockchain Workbench ile tümleştirmek için iletileri kullanma
description: Azure Blockchain Çalışma Tezgahı Önizlemesini diğer sistemlerle tümleştirmek için iletileri kullanmaya genel bakış.
ms.date: 09/05/2019
ms.topic: article
ms.reviewer: brendal
ms.openlocfilehash: 14bd0f84bc9490d95d3dbe0b9f122882f0d2059d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74324503"
---
# <a name="azure-blockchain-workbench-messaging-integration"></a>Azure Blockchain Workbench mesajlaşma entegrasyonu

Azure Blockchain Workbench, REST API sağlamanın yanı sıra mesajlaşma tabanlı tümleştirme de sağlar. Workbench, Azure Olay Ağıtı üzerinden genel muhasebe merkezli etkinlikler yayınlayarak akış aşağı tüketicilerin bu olaylara dayalı olarak veri yutmalarını veya harekete geçmelerini sağlar. Azure Blockchain Workbench, güvenilir ileti gerektiren istemciler için bir Azure Hizmet Veri Yolu bitiş noktasına da ileti ler sunar.

## <a name="input-apis"></a>Giriş API'leri

Kullanıcıları oluşturmak, sözleşmeler oluşturmak ve sözleşmeleri güncelleştirmek için dış sistemlerden hareketler başlatmak istiyorsanız, genel muhasebede işlem gerçekleştirmek için ileti giriş API'lerini kullanabilirsiniz. Giriş API'lerini gösteren bir örnek için [ileti tümleştirme örneklerine](https://aka.ms/blockchain-workbench-integration-sample) bakın.

Aşağıdaki ler şu anda kullanılabilir giriş API'leridir.

### <a name="create-user"></a>Kullanıcı oluştur

Yeni bir kullanıcı oluşturur.

İstek aşağıdaki alanları gerektirir:

| **Adı**             | **Açıklama**                                      |
|----------------------|------------------------------------------------------|
| Requestıd            | Müşteri sağlanan GUID                                |
| firstName            | Kullanıcının adı                              |
| lastName             | Kullanıcının soyadı                               |
| Emailaddress         | Kullanıcının e-posta adresi                           |
| externalId           | Kullanıcının Azure AD nesne kimliği                      |
| Connectionıd         | Blockchain bağlantısı için benzersiz tanımlayıcı |
| mesajSchemaVersion | Mesajlaşma şeması sürümü                            |
| messageName          | **CreateUserRequest**                               |

Örnek:

``` json
{
    "requestId": "e2264523-6147-41fc-bbbb-edba8e44562d",
    "firstName": "Ali",
    "lastName": "Alio",
    "emailAddress": "aa@contoso.com",
    "externalId": "6a9b7f65-ffff-442f-b3b8-58a35abd1bcd",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateUserRequest"
}
```

Blockchain Workbench aşağıdaki alanları içeren bir yanıt verir:

| **Adı**              | **Açıklama**                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------|
| Requestıd             | Müşteri sağlanan GUID |
| userId                | Oluşturulan kullanıcının kimliği |
| userChainIdentifier   | Blockchain ağında oluşturulan kullanıcının adresi. Ethereum'da adres kullanıcının **zincirleme** adresidir. |
| Connectionıd          | Blockchain bağlantısı için benzersiz tanımlayıcı|
| mesajSchemaVersion  | Mesajlaşma şeması sürümü |
| messageName           | **CreateUserUpdate** |
| durum                | Kullanıcı oluşturma isteğinin durumu.  Başarılı ise, değer **Başarıdır.** Başarısızlık, değer **Failure**olduğunu .     |
| ekBilgi | Duruma göre sağlanan ek bilgiler |

Blockchain Workbench'ten başarılı kullanıcı yanıtı **örneği:**

``` json
{ 
    "requestId": "e2264523-6147-41fc-bb59-edba8e44562d", 
    "userId": 15, 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateUserUpdate", 
    "status": "Success", 
    "additionalInformation": { } 
} 
```

İstek başarısız olduysa, hatayla ilgili ayrıntılar ek bilgilere dahil edilir.

``` json
{
    "requestId": "e2264523-6147-41fc-bb59-edba8e44562d", 
    "userId": 15, 
    "userChainIdentifier": null, 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateUserUpdate", 
    "status": "Failure", 
    "additionalInformation": { 
        "errorCode": 4000, 
        "errorMessage": "User cannot be provisioned on connection." 
    }
}
```

### <a name="create-contract"></a>Sözleşme oluşturma

Yeni bir sözleşme oluşturur.

İstek aşağıdaki alanları gerektirir:

| **Adı**             | **Açıklama**                                                                                                           |
|----------------------|---------------------------------------------------------------------------------------------------------------------------|
| Requestıd            | Müşteri sağlanan GUID |
| userChainIdentifier  | Blockchain ağında oluşturulan kullanıcının adresi. Ethereum'da bu adres kullanıcının **zincirli adresteki** adresidir. |
| applicationName      | Uygulamanın adı |
| version              | Uygulamanın sürümü. Uygulamanın birden çok sürümü etkinse gereklidir. Aksi takdirde, sürüm isteğe bağlıdır. Uygulama sürümü hakkında daha fazla bilgi için [Azure Blockchain Workbench uygulama sürümüne](version-app.md)bakın. |
| iş akışıAdı         | İş akışının adı |
| parametreler           | Sözleşme oluşturma için parametreler girişi |
| Connectionıd         | Blockchain bağlantısı için benzersiz tanımlayıcı |
| mesajSchemaVersion | Mesajlaşma şeması sürümü |
| messageName          | **Sözleşme İsteği Oluşturma** |

Örnek:

``` json
{ 
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211", 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "applicationName": "AssetTransfer",
    "version": "1.0",
    "workflowName": "AssetTransfer", 
    "parameters": [ 
        { 
            "name": "description", 
            "value": "a 1969 dodge charger" 
        }, 
        { 
            "name": "price", 
            "value": "12345" 
        } 
    ], 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateContractRequest" 
}
```

Blockchain Workbench aşağıdaki alanları içeren bir yanıt verir:

| **Adı**                 | **Açıklama**                                                                   |
|--------------------------|-----------------------------------------------------------------------------------|
| Requestıd                | Müşteri sağlanan GUID                                                             |
| contractId               | Azure Blockchain Workbench içindeki sözleşme için benzersiz tanımlayıcı |
| sözleşmeliLedgerIdentifier | Genel muhasebedeki sözleşmenin adresi                                            |
| Connectionıd             | Blockchain bağlantısı için benzersiz tanımlayıcı                               |
| mesajSchemaVersion     | Mesajlaşma şeması sürümü                                                         |
| messageName              | **Sözleşme Güncelleştirmesi Oluşturma**                                                      |
| durum                   | Sözleşme oluşturma isteğinin durumu.  Olası değerler: **Gönderilen**, **Taahhüt edilen**, **Hata .**  |
| ekBilgi    | Duruma göre sağlanan ek bilgiler                              |

Blockchain Workbench'ten gönderilen **sözleşme** yanıtı örneği:

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Submitted",
    "additionalInformation": { }
}
```

Blockchain Workbench'ten taahhüt edilen sözleşme yanıtı **örneği:**

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Committed",
    "additionalInformation": { }
}
```

İstek başarısız olduysa, hatayla ilgili ayrıntılar ek bilgilere dahil edilir.

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": null,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Failure",
    "additionalInformation": {
        "errorCode": 4000,
        "errorMessage": "Contract cannot be provisioned on connection."
    }
}
```

### <a name="create-contract-action"></a>Sözleşme eylemi oluşturma

Yeni bir sözleşme eylemi oluşturur.

İstek aşağıdaki alanları gerektirir:

| **Adı**                 | **Açıklama**                                                                                                           |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------|
| Requestıd                | Müşteri sağlanan GUID |
| userChainIdentifier      | Blockchain ağında oluşturulan kullanıcının adresi. Ethereum'da bu adres kullanıcının **zincirli adresteki** adresidir. |
| sözleşmeliLedgerIdentifier | Genel muhasebedeki sözleşmenin adresi |
| version                  | Uygulamanın sürümü. Uygulamanın birden çok sürümü etkinse gereklidir. Aksi takdirde, sürüm isteğe bağlıdır. Uygulama sürümü hakkında daha fazla bilgi için [Azure Blockchain Workbench uygulama sürümüne](version-app.md)bakın. |
| iş akışıFunctionName     | İş akışı işlevinin adı |
| parametreler               | Sözleşme oluşturma için parametreler girişi |
| Connectionıd             | Blockchain bağlantısı için benzersiz tanımlayıcı |
| mesajSchemaVersion     | Mesajlaşma şeması sürümü |
| messageName              | **SözleşmeEylem İsteği Oluşturma** |

Örnek:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398",
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "version": "1.0",
    "workflowFunctionName": "modify",
    "parameters": [
        {
            "name": "description",
            "value": "a 1969 dodge charger"
        },
        {
            "name": "price",
            "value": "12345"
        }
    ],
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionRequest"
}
```

Blockchain Workbench aşağıdaki alanları içeren bir yanıt verir:

| **Adı**              | **Açıklama**                                                                   |
|-----------------------|-----------------------------------------------------------------------------------|
| Requestıd             | Müşteri sağlanan GUID|
| contractId            | Azure Blockchain Workbench içindeki sözleşme için benzersiz tanımlayıcı |
| Connectionıd          | Blockchain bağlantısı için benzersiz tanımlayıcı |
| mesajSchemaVersion  | Mesajlaşma şeması sürümü |
| messageName           | **SözleşmeEylemGüncelleştirmesi Oluşturma** |
| durum                | Sözleşme eylem isteğinin durumu. Olası değerler: **Gönderilen**, **Taahhüt edilen**, **Hata .**                         |
| ekBilgi | Duruma göre sağlanan ek bilgiler |

Blockchain Workbench'ten gönderilen bir sözleşme eylem yanıtı **örneği:**

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Submitted",
    "additionalInformation": { }
}
```

Blockchain Workbench'ten taahhüt edilen bir sözleşme eylem yanıtı **örneği:**

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Committed",
    "additionalInformation": { }
}
```

İstek başarısız olduysa, hatayla ilgili ayrıntılar ek bilgilere dahil edilir.

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Failure",
    "additionalInformation": {
        "errorCode": 4000,
        "errorMessage": "Contract action cannot be provisioned on connection."
    }
}
```

### <a name="input-api-error-codes-and-messages"></a>Giriş API hata kodları ve iletileri

**Hata kodu 4000: Hatalı istek hatası**
- Geçersiz bağlantıKimliği
- CreateUserRequest deserialization başarısız oldu
- CreateContractRequest deserialization başarısız oldu
- CreateContractActionRequest deserialization başarısız oldu
- Uygulama {uygulama adı ile tanımlanan} yok
- Uygulama {uygulama adı ile tanımlanır} iş akışı yok
- UserChainIdentifier yok
- Genel muhasebe tanımlayıcısı tarafından tanımlanan sözleşme} yok
- Sözleşme {genel muhasebe tanımlayıcısı tarafından tanımlanan} işlev {iş akışı işlev adı} yok
- UserChainIdentifier yok

**Hata kodu 4090: Çakışma hatası**
- Kullanıcı zaten var
- Sözleşme zaten var
- Sözleşme eylemi zaten var

**Hata kodu 5000: Dahili sunucu hatası**
- Özel durum iletileri

## <a name="event-notifications"></a>Olay bildirimleri

Olay bildirimleri, blockchain Workbench'te ve bağlı olduğu blockchain ağında meydana gelen olayları kullanıcılara ve aşağı akış sistemlerine bildirmek için kullanılabilir. Olay bildirimleri doğrudan kod olarak tüketilebilir veya akış aşağı sistemlerine veri akışını tetiklemek için Logic Apps ve Flow gibi araçlarla kullanılabilir.

Alınabilecek çeşitli iletilerin ayrıntıları için [Bildirim iletisi başvurusuna](#notification-message-reference) bakın.

### <a name="consuming-event-grid-events-with-azure-functions"></a>Azure İşlevleriyle Olay Ağı etkinliklerini tüketme

Bir kullanıcı Blockchain Workbench'te meydana gelen olaylarhakkında bilgilendirilmek için Olay Kılavuzu'nu kullanmak isterse, Azure İşlevlerini kullanarak Olay Kılavuzu'ndaki olayları tüketebilirsiniz.

1. Azure portalında bir **Azure İşlevi Uygulaması** oluşturun.
2. Yeni bir işlev oluşturun.
3. Olay Izgarası için şablonu bulun. İletiyi okumak için temel şablon kodu gösterilir. Kodu gerektiği gibi değiştirin.
4. İşlevi kaydedin. 
5. Blockchain Workbench'in kaynak grubundan Olay Izgarasını seçin.

### <a name="consuming-event-grid-events-with-logic-apps"></a>Mantık Uygulamaları ile Olay Izgara etkinliklerini tüketme

1. Azure portalında yeni bir **Azure Mantık Uygulaması** oluşturun.
2. Portalda Azure Mantık Uygulaması'nı açarken bir tetikleyici seçmeniz istenir. **Azure Olay Idamı'nı seçin -- Kaynak olayı oluştuğunda.**
3. İş akışı tasarımcısı görüntülendiğinde, oturum açmanız istenir.
4. Abonelik'i seçin. Kaynak olarak **Microsoft.EventGrid.Topics**. Azure Blockchain Workbench kaynak grubundan kaynağın adından **Kaynak Adı'nı** seçin.
5. Blockchain Workbench'in kaynak grubundan Olay Izgarasını seçin.

## <a name="using-service-bus-topics-for-notifications"></a>Bildirimler için Servis Veri Hizmeti Konularını Kullanma

Servis Veri Yolu Konuları, Blockchain Workbench'te meydana gelen olaylar hakkında kullanıcıları bilgilendirmek için kullanılabilir. 

1. Workbench'in kaynak grubundaki Servis Veri Servisi'ne göz atın.
2. **Konular'ı**seçin.
3. **Çıkış-konu**seçin.
4. Bu konuya yeni bir abonelik oluşturun. Bunun için bir anahtar edinin.
5. Bu abonelikten etkinliklere abone olan bir program oluşturun.

### <a name="consuming-service-bus-messages-with-logic-apps"></a>Mantık Uygulamaları ile Servis Veri Günü İletilerini Tüketme

1. Azure portalında yeni bir **Azure Mantık Uygulaması** oluşturun.
2. Portalda Azure Mantık Uygulaması'nı açarken bir tetikleyici seçmeniz istenir. Arama kutusuna **Servis Veri Servisi** yazın ve Servis Veri Servisi ile olmasını istediğiniz etkileşim türüne uygun tetikleyiciyi seçin. Örneğin, **Servis Veri Servisi -- Bir konu aboneliğinde (otomatik tamamlama) bir ileti alındığı zaman.**
3. İş akışı tasarımcısı görüntülendiğinde, Servis Veri Kurumu'nun bağlantı bilgilerini belirtin.
4. Aboneliğinizi seçin ve **çalışma tezgahı-harici**konusunu belirtin.
5. Bu tetikleyiciden gelen iletiyi kullanan uygulamanız için mantığı geliştirin.

## <a name="notification-message-reference"></a>Bildirim iletisi başvurusu

**İleti Adına**bağlı olarak, bildirim iletilerinde aşağıdaki ileti türlerinden biri vardır.

### <a name="block-message"></a>İletiyi engelleme

Tek tek bloklar hakkında bilgi içerir. *BlockMessage,* blok düzeyi bilgilerinin bulunduğu bir bölüm ve işlem bilgilerinin bulunduğu bir bölüm içerir.

| Adı | Açıklama |
|------|-------------|
| Engelle | [Blok bilgileri](#block-information) içerir |
| işlemler | Blok için bir tahsilat [hareketi bilgileri](#transaction-information) içerir |
| Connectionıd | Bağlantı için benzersiz tanımlayıcı |
| mesajSchemaVersion | Mesajlaşma şeması sürümü |
| messageName | **Engelleme Mesajı** |
| ekBilgi | Sağlanan ek bilgiler |

#### <a name="block-information"></a>Bilgileri engelleme

| Adı              | Açıklama |
|-------------------|-------------|
| blockId           | Azure Blockchain Workbench içindeki blok için benzersiz tanımlayıcı |
| blockNumber       | Genel muhasebedeki bir blok için benzersiz tanımlayıcı |
| blockHash         | Bloğun karma |
| öncekiBlockHash | Önceki bloğun karma |
| blockTimestamp    | Bloğun zaman damgası |

#### <a name="transaction-information"></a>İşlem bilgileri

| Adı               | Açıklama |
|--------------------|-------------|
| Transactionıd      | Azure Blockchain Workbench içindeki işlem için benzersiz tanımlayıcı |
| işlemHash    | Genel muhasebedeki işlemin karma |
| Kaynak               | İşlem kaynağı için genel muhasebede benzersiz tanımlayıcı |
| -                 | Hareket hedefi için genel muhasebede benzersiz tanımlayıcı |
| sağlamaDurumu | Hareket için sağlama işleminin geçerli durumunu tanımlar. Olası değerler şunlardır: </br>0 - İşlem veritabanındaki API tarafından oluşturulmuştur</br>1 - İşlem genel muhasebeye gönderildi</br>2 - İşlem deftere başarıyla taahhüt edilmiştir</br>3 veya 4 - İşlem genel muhasebeye taahhüt edilememi</br>5 - İşlem deftere başarıyla bağlandı |

Blockchain Çalışma Tezgahı'ndan *BlockMessage* örneği:

``` json
{
    "block": {
        "blockId": 123,
        "blockNumber": 1738312,
        "blockHash": "0x03a39411e25e25b47d0ec6433b73b488554a4a5f6b1a253e0ac8a200d13fffff",
        "previousBlockHash": null,
        "blockTimestamp": "2018-10-09T23:35:58Z",
    },
    "transactions": [
        {
            "transactionId": 234,
            "transactionHash": "0xa4d9c95b581f299e41b8cc193dd742ef5a1d3a4ddf97bd11b80d123fec27ffff",
            "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
            "to": null,
            "provisioningStatus": 1
        },
        {
            "transactionId": 235,
            "transactionHash": "0x5c1fddea83bf19d719e52a935ec8620437a0a6bdaa00ecb7c3d852cf92e1ffff",
            "from": "0xadd97e1e595916e29ea94fda894941574000ffff",
            "to": "0x9a8DDaCa9B7488683A4d62d0817E965E8f24ffff",
            "provisioningStatus": 2
        }
    ],
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "BlockMessage",
    "additionalInformation": {}
}
```

### <a name="contract-message"></a>Sözleşme iletisi

Sözleşme hakkında bilgi içerir. İleti, sözleşme özelliklerinin bulunduğu bir bölüm ve işlem bilgilerini içeren bir bölüm içerir. Belirli bir bloğun sözleşmesini değiştiren tüm hareketler hareket bölümüne dahil edilir.

| Adı | Açıklama |
|------|-------------|
| blockId | Azure Blockchain Workbench içindeki blok için benzersiz tanımlayıcı |
| blockHash | Bloğun karma |
| moddeğiştirmeİşlemler | Sözleşmeyi [değiştiren hareketler](#modifying-transaction-information) |
| contractId | Azure Blockchain Workbench içindeki sözleşme için benzersiz tanımlayıcı |
| sözleşmeliLedgerIdentifier | Genel muhasebedeki sözleşme için benzersiz tanımlayıcı |
| sözleşmeÖzellikler | [Sözleşmenin özellikleri](#contract-properties) |
| isNewContract | Bu sözleşmenin yeni oluşturulup oluşturulmadığını gösterir. Olası değerler şunlardır: doğru: bu sözleşme oluşturulan yeni bir sözleşme oldu. false: Bu sözleşme bir sözleşme güncelleştirmesidir. |
| Connectionıd | Bağlantı için benzersiz tanımlayıcı |
| mesajSchemaVersion | Mesajlaşma şeması sürümü |
| messageName | **Contractmessage** |
| ekBilgi | Sağlanan ek bilgiler |

#### <a name="modifying-transaction-information"></a>Hareket bilgilerini değiştirme

| Adı               | Açıklama |
|--------------------|-------------|
| Transactionıd | Azure Blockchain Workbench içindeki işlem için benzersiz tanımlayıcı |
| işlemHash | Genel muhasebedeki işlemin karma |
| Kaynak | İşlem kaynağı için genel muhasebede benzersiz tanımlayıcı |
| - | Hareket hedefi için genel muhasebede benzersiz tanımlayıcı |

#### <a name="contract-properties"></a>Sözleşme özellikleri

| Adı               | Açıklama |
|--------------------|-------------|
| iş akışıPropertyId | Azure Blockchain Workbench içindeki iş akışı özelliği için benzersiz tanımlayıcı |
| ad | İş akışı özelliğinin adı |
| value | İş akışı özelliğinin değeri |

Blockchain Workbench'ten *Bir Sözleşme Mesajı* örneği:

``` json
{
    "blockId": 123,
    "blockhash": "0x03a39411e25e25b47d0ec6433b73b488554a4a5f6b1a253e0ac8a200d13fffff",
    "modifyingTransactions": [
        {
            "transactionId": 234,
            "transactionHash": "0x5c1fddea83bf19d719e52a935ec8620437a0a6bdaa00ecb7c3d852cf92e1ffff",
            "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
            "to": "0xf8559473b3c7197d59212b401f5a9f07ffff"
        },
        {
            "transactionId": 235,
            "transactionHash": "0xa4d9c95b581f299e41b8cc193dd742ef5a1d3a4ddf97bd11b80d123fec27ffff",
            "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
            "to": "0xf8559473b3c7197d59212b401f5a9f07b429ffff"
        }
    ],
    "contractId": 111,
    "contractLedgerIdentifier": "0xf8559473b3c7197d59212b401f5a9f07b429ffff",
    "contractProperties": [
        {
            "workflowPropertyId": 1,
            "name": "State",
            "value": "0"
        },
        {
            "workflowPropertyId": 2,
            "name": "Description",
            "value": "1969 Dodge Charger"
        },
        {
            "workflowPropertyId": 3,
            "name": "AskingPrice",
            "value": "30000"
        },
        {
            "workflowPropertyId": 4,
            "name": "OfferPrice",
            "value": "0"
        },
        {
            "workflowPropertyId": 5,
            "name": "InstanceAppraiser",
            "value": "0x0000000000000000000000000000000000000000"
        },
        {
            "workflowPropertyId": 6,
            "name": "InstanceBuyer",
            "value": "0x0000000000000000000000000000000000000000"
        },
        {
            "workflowPropertyId": 7,
            "name": "InstanceInspector",
            "value": "0x0000000000000000000000000000000000000000"
        },
        {
            "workflowPropertyId": 8,
            "name": "InstanceOwner",
            "value": "0x9a8DDaCa9B7488683A4d62d0817E965E8f24ffff"
        },
        {
            "workflowPropertyId": 9,
            "name": "ClosingDayOptions",
            "value": "[21,48,69]"
        }
    ],
    "isNewContract": false,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "ContractMessage",
    "additionalInformation": {}
}
```

### <a name="event-message-contract-function-invocation"></a>Olay iletisi: Sözleşme işlevi çağırma

İşlev adı, parametreler girişi ve işlevin arayan gibi bir sözleşme işlevi çağrıldığı zaman bilgileri içerir.

| Adı | Açıklama |
|------|-------------|
| eventName                   | **ContractFunctionInvocation** |
| Ara -yan                      | [Çağıran bilgileri](#caller-information) |
| contractId                  | Azure Blockchain Workbench içindeki sözleşme için benzersiz tanımlayıcı |
| sözleşmeliLedgerIdentifier    | Genel muhasebedeki sözleşme için benzersiz tanımlayıcı |
| functionName                | Fonksiyonun adı |
| parametreler                  | [Parametre bilgileri](#parameter-information) |
| Işlem                 | İşlem bilgileri |
| inTransactionSequenceNumber | Bloktaki hareketin sıra numarası |
| Connectionıd                | Bağlantı için benzersiz tanımlayıcı |
| mesajSchemaVersion        | Mesajlaşma şeması sürümü |
| messageName                 | **Olay Mesajı** |
| ekBilgi       | Sağlanan ek bilgiler |

#### <a name="caller-information"></a>Çağıran bilgileri

| Adı | Açıklama |
|------|-------------|
| type | Kullanıcı veya sözleşme gibi arayanın türü |
| id | Azure Blockchain Workbench içindeki arayan için benzersiz tanımlayıcı |
| muhasebeIdentifier | Genel muhasebede arayan için benzersiz tanımlayıcı |

#### <a name="parameter-information"></a>Parametre bilgileri

| Adı | Açıklama |
|------|-------------|
| ad | Parametre adı |
| value | Parametre değeri |

#### <a name="event-message-transaction-information"></a>Olay iletisi hareket bilgileri

| Adı               | Açıklama |
|--------------------|-------------|
| Transactionıd      | Azure Blockchain Workbench içindeki işlem için benzersiz tanımlayıcı |
| işlemHash    | Genel muhasebedeki işlemin karma |
| Kaynak               | İşlem kaynağı için genel muhasebede benzersiz tanımlayıcı |
| -                 | Hareket hedefi için genel muhasebede benzersiz tanımlayıcı |

Blockchain Workbench'ten *EventMessage ContractFunctionInvocation* örneği:

``` json
{
    "eventName": "ContractFunctionInvocation",
    "caller": {
        "type": "User",
        "id": 21,
        "ledgerIdentifier": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60ffff"
    },
    "contractId": 34,
    "contractLedgerIdentifier": "0xf8559473b3c7197d59212b401f5a9f07b429ffff",
    "functionName": "Modify",
    "parameters": [
        {
            "name": "description",
            "value": "a new description"
        },
        {
            "name": "price",
            "value": "4567"
        }
    ],
    "transaction": {
        "transactionId": 234,
        "transactionHash": "0x5c1fddea83bf19d719e52a935ec8620437a0a6bdaa00ecb7c3d852cf92e1ffff",
        "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
        "to": "0xf8559473b3c7197d59212b401f5a9f07b429ffff"
    },
    "inTransactionSequenceNumber": 1,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "EventMessage",
    "additionalInformation": { }
}
```

### <a name="event-message-application-ingestion"></a>Olay mesajı: Uygulama

Bir uygulama Workbench'e yüklendiğinde, yüklenen uygulamanın adı ve sürümü gibi bilgileri içerir.

| Adı | Açıklama |
|------|-------------|
| eventName | **UygulamaYutma** |
| applicationId | Azure Blockchain Workbench içindeki uygulama için benzersiz tanımlayıcı |
| applicationName | Uygulama adı |
| uygulamaDisplayName | Uygulama görüntü adı |
| uygulamaSürüm | Uygulama sürümü |
| uygulamaDefinitionLocation | Uygulama yapılandırma dosyasının bulunduğu URL |
| sözleşmeKodları | Uygulama için [sözleşme kodlarının](#contract-code-information) toplanması |
| uygulamaRoller | Uygulama için [uygulama rollerinin](#application-role-information) toplanması |
| uygulamaİş akışları | Uygulama için [uygulama iş akışlarının](#application-workflow-information) toplanması |
| Connectionıd | Bağlantı için benzersiz tanımlayıcı |
| mesajSchemaVersion | Mesajlaşma şeması sürümü |
| messageName | **Olay Mesajı** |
| ekBilgi | Burada sağlanan ek bilgiler, uygulama iş akışı durumlarını ve geçiş bilgilerini içerir. |

#### <a name="contract-code-information"></a>Sözleşme kodu bilgileri

| Adı | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içindeki sözleşme kodu dosyası için benzersiz tanımlayıcı |
| genel muhasebe | Azure Blockchain Workbench içindeki genel muhasebe için benzersiz tanımlayıcı |
| location | Sözleşme kodu dosyasının bulunduğu URL |

#### <a name="application-role-information"></a>Uygulama rol bilgileri

| Adı | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içindeki uygulama rolü için benzersiz tanımlayıcı |
| ad | Uygulama rolünün adı |

#### <a name="application-workflow-information"></a>Uygulama iş akışı bilgileri

| Adı | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içindeki uygulama iş akışı için benzersiz tanımlayıcı |
| ad | Uygulama iş akışı adı |
| displayName | Uygulama iş akışı ekran adı |
|  işlevleri | Uygulama [iş akışı için işlevlerin](#workflow-function-information) toplanması|
| Devletleri | Uygulama [iş akışı için durumların](#workflow-state-information) toplanması |
| properties | Uygulama [iş akışı özellikleri bilgileri](#workflow-property-information) |

##### <a name="workflow-function-information"></a>İş akışı işlevi bilgileri

| Adı | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içindeki uygulama iş akışı işlevi için benzersiz tanımlayıcı |
| ad | İşlev adı |
| parametreler | Fonksiyon için parametreler |

##### <a name="workflow-state-information"></a>İş akışı durumu bilgileri

| Adı | Açıklama |
|------|-------------|
| ad | Devlet adı |
| displayName | Durum görüntü adı |
|  stili | Durum stili (başarı veya başarısızlık) |

##### <a name="workflow-property-information"></a>İş akışı özelliği bilgileri

| Adı | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içindeki uygulama iş akışı özelliği için benzersiz tanımlayıcı |
| ad | Özellik adı |
| type | Özellik türü |

Blockchain Workbench'ten *EventMessage Uygulaması* Örneği:

``` json
{
    "eventName": "ApplicationIngestion",
    "applicationId": 31,
    "applicationName": "AssetTransfer",
    "applicationDisplayName": "Asset Transfer",
    "applicationVersion": "1.0",
    "applicationDefinitionLocation": "http://url",
    "contractCodes": [
        {
            "id": 23,
            "ledgerId": 1,
            "location": "http://url"
        }
    ],
    "applicationRoles": [
            {
                "id": 134,
                "name": "Buyer"
            },
            {
                "id": 135,
                "name": "Seller"
            }
       ],
    "applicationWorkflows": [
        {
            "id": 89,
            "name": "AssetTransfer",
            "displayName": "Asset Transfer",
            "functions": [
                {
                    "id": 912,
                    "name": "",
                    "parameters": [
                        {
                            "name": "description",
                            "type": {
                                "name": "string"
                             }
                        },
                        {
                            "name": "price",
                            "type": {
                                "name": "int"
                            }
                        }
                    ]
                },
                {
                    "id": 913,
                    "name": "modify",
                    "parameters": [
                        {
                            "name": "description",
                            "type": {
                                "name": "string"
                             }
                        },
                        {
                            "name": "price",
                            "type": {
                                "name": "int"
                            }
                        }
                    ]
                }
            ],
            "states": [ 
                 {
                      "name": "Created",
                      "displayName": "Created",
                      "style" : "Success"
                 },
                 {
                      "name": "Terminated",
                      "displayName": "Terminated",
                      "style" : "Failure"
                 }
            ],
            "properties": [
                {
                    "id": 879,
                    "name": "Description",
                    "type": {
                        "name": "string"
                     }
                },
                {
                    "id": 880,
                    "name": "Price",
                    "type": {
                        "name": "int"
                     }
                }
            ]
        }
    ],
    "connectionId": [ ],
    "messageSchemaVersion": "1.0.0",
    "messageName": "EventMessage",
    "additionalInformation":
        {
            "states" :
            [
                {
                    "Name": "BuyerAccepted",
                    "Transitions": [
                        {
                            "DisplayName": "Accept",
                            "AllowedRoles": [ ],
                            "AllowedInstanceRoles": [ "InstanceOwner" ],
                            "Function": "Accept",
                            "NextStates": [ "SellerAccepted" ]
                        }
                    ]
                }
            ]
        }
}
```

### <a name="event-message-role-assignment"></a>Olay iletisi: Rol ataması

Bir kullanıcıya Workbench'te rol atamasını kimin gerçekleştirdiği, rolün adı ve ilgili uygulama gibi bir rol atandığında bilgi içerir.

| Adı | Açıklama |
|------|-------------|
| eventName | **RoleAssignment** |
| applicationId | Azure Blockchain Workbench içindeki uygulama için benzersiz tanımlayıcı |
| applicationName | Uygulama adı |
| uygulamaDisplayName | Uygulama görüntü adı |
| uygulamaSürüm | Uygulama sürümü |
| uygulamaRol        | [Uygulama rolü](#roleassignment-application-role) hakkında bilgi |
| devren               | [Atanın hakkında](#roleassignment-assigner) bilgi |
| devrilen               | [Devralan](#roleassignment-assignee) hakkında bilgi |
| Connectionıd           | Bağlantı için benzersiz tanımlayıcı |
| mesajSchemaVersion   | Mesajlaşma şeması sürümü |
| messageName            | **Olay Mesajı** |
| ekBilgi  | Sağlanan ek bilgiler |

#### <a name="roleassignment-application-role"></a>RoleAssignment uygulama rolü

| Adı | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içindeki uygulama rolü için benzersiz tanımlayıcı |
| ad | Uygulama rolünün adı |

#### <a name="roleassignment-assigner"></a>RoleAssignment atleyici

| Adı | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içindeki kullanıcının benzersiz tanımlayıcısı |
| type | Devralan türü |
| chainIdentifier | Genel muhasebede kullanıcının benzersiz tanımlayıcısı |

#### <a name="roleassignment-assignee"></a>RoleAssignment atanan

| Adı | Açıklama |
|------|-------------|
| id | Azure Blockchain Workbench içindeki kullanıcının benzersiz tanımlayıcısı |
| type | Devrenin türü |
| chainIdentifier | Genel muhasebede kullanıcının benzersiz tanımlayıcısı |

Blockchain Workbench'ten *EventMessage RoleAssignment* örneği:

``` json
{
    "eventName": "RoleAssignment",
    "applicationId": 31,
    "applicationName": "AssetTransfer",
    "applicationDisplayName": "Asset Transfer",
    "applicationVersion": "1.0",
    "applicationRole": {
        "id": 134,
        "name": "Buyer"
    },
    "assigner": {
        "id": 1,
        "type": null,
        "chainIdentifier": "0xeFFC7766d38aC862d79706c3C5CEEf089564ffff"
    },
    "assignee": {
        "id": 3,
        "type": null,
        "chainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f24ffff"
    },
    "connectionId": [ ],
    "messageSchemaVersion": "1.0.0",
    "messageName": "EventMessage",
    "additionalInformation": { }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Akıllı anlaşma tümleştirme desenleri](integration-patterns.md)
