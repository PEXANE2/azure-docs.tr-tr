---
title: Blockchain uygulaması oluşturma - Azure Blockchain Workbench
description: Azure Blockchain Workbench Preview için blockchain uygulaması nın nasıl oluşturulabildiğini öğrenin.
ms.date: 10/14/2019
ms.topic: tutorial
ms.reviewer: brendal
ms.openlocfilehash: 33a9e9c10c07d0808626353a7edfd505e0f60bc9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74324817"
---
# <a name="tutorial-create-a-blockchain-application-for-azure-blockchain-workbench"></a>Öğretici: Azure Blockchain Workbench için blockchain uygulaması oluşturun

Yapılandırma ve akıllı sözleşme kodu yla tanımlanan çok taraflı iş akışlarını temsil eden blockchain uygulamalarını oluşturmak için Azure Blockchain Workbench'i kullanabilirsiniz.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Blockchain uygulamasını yapılandırma
> * Akıllı sözleşme kodu dosyası oluşturma
> * Blockchain Workbench'e blockchain uygulaması ekleme
> * Blockchain uygulamasına üye ekleme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Blockchain Çalışma Tezgahı dağıtımı. Daha fazla bilgi için, dağıtımla ilgili ayrıntılar için [Azure Blockchain Workbench dağıtımına](deploy.md) bakın.
* Blockchain Workbench ile ilişkili kiracıdaki Azure Etkin Dizin kullanıcıları. Daha fazla bilgi için Azure [Blockchain Workbench'teki Azure AD kullanıcılarını ekleyin'e](manage-users.md#add-azure-ad-users)bakın.
* Blockchain Workbench yönetici hesabı. Daha fazla bilgi için Azure [Blockchain Workbench'teki Blockchain Workbench yöneticilerini](manage-users.md#manage-blockchain-workbench-administrators)ekleyin' e bakın.

## <a name="hello-blockchain"></a>Merhaba Blockchain!

İstekçinin bir istek gönderdiği ve yanıtlayanın isteğe yanıt gönderdiği temel bir uygulama oluşturalım.
Örneğin, bir istek "Merhaba, nasılsın?", ve yanıt olabilir, "Ben harikayım!". Hem istek hem de yanıt altta yatan blockchain'e kaydedilir.

Uygulama dosyalarını oluşturmak için adımları izleyin veya [örneği GitHub'dan indirebilirsiniz.](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/application-and-smart-contract-samples/hello-blockchain)

## <a name="configuration-file"></a>Yapılandırma dosyası

Yapılandırma meta verileri blockchain uygulamasının üst düzey iş akışlarını ve etkileşim modelini tanımlar. Yapılandırma meta verileri blockchain uygulamasının iş akışı aşamalarını ve etkileşim modelini temsil eder.

1. Sık kullanılan düzenleyicinizde, '' adlı `HelloBlockchain.json`bir dosya oluşturun.
2. Blockchain uygulamasının yapılandırmasını tanımlamak için aşağıdaki JSON'u ekleyin.

    ``` json
    {
      "ApplicationName": "HelloBlockchain",
      "DisplayName": "Hello, Blockchain!",
      "Description": "A simple application to send request and get response",
      "ApplicationRoles": [
        {
          "Name": "Requestor",
          "Description": "A person sending a request."
        },
        {
          "Name": "Responder",
          "Description": "A person responding to a request"
        }
      ],
      "Workflows": [
        {
          "Name": "HelloBlockchain",
          "DisplayName": "Request Response",
          "Description": "A simple workflow to send a request and receive a response.",
          "Initiators": [ "Requestor" ],
          "StartState": "Request",
          "Properties": [
            {
              "Name": "State",
              "DisplayName": "State",
              "Description": "Holds the state of the contract.",
              "Type": {
                "Name": "state"
              }
            },
            {
              "Name": "Requestor",
              "DisplayName": "Requestor",
              "Description": "A person sending a request.",
              "Type": {
                "Name": "Requestor"
              }
            },
            {
              "Name": "Responder",
              "DisplayName": "Responder",
              "Description": "A person sending a response.",
              "Type": {
                "Name": "Responder"
              }
            },
            {
              "Name": "RequestMessage",
              "DisplayName": "Request Message",
              "Description": "A request message.",
              "Type": {
                "Name": "string"
              }
            },
            {
              "Name": "ResponseMessage",
              "DisplayName": "Response Message",
              "Description": "A response message.",
              "Type": {
                "Name": "string"
              }
            }
          ],
          "Constructor": {
            "Parameters": [
              {
                "Name": "message",
                "Description": "...",
                "DisplayName": "Request Message",
                "Type": {
                  "Name": "string"
                }
              }
            ]
          },
          "Functions": [
            {
              "Name": "SendRequest",
              "DisplayName": "Request",
              "Description": "...",
              "Parameters": [
                {
                  "Name": "requestMessage",
                  "Description": "...",
                  "DisplayName": "Request Message",
                  "Type": {
                    "Name": "string"
                  }
                }
              ]
            },
            {
              "Name": "SendResponse",
              "DisplayName": "Response",
              "Description": "...",
              "Parameters": [
                {
                  "Name": "responseMessage",
                  "Description": "...",
                  "DisplayName": "Response Message",
                  "Type": {
                    "Name": "string"
                  }
                }
              ]
            }
          ],
          "States": [
            {
              "Name": "Request",
              "DisplayName": "Request",
              "Description": "...",
              "PercentComplete": 50,
              "Value": 0,
              "Style": "Success",
              "Transitions": [
                {
                  "AllowedRoles": ["Responder"],
                  "AllowedInstanceRoles": [],
                  "Description": "...",
                  "Function": "SendResponse",
                  "NextStates": [ "Respond" ],
                  "DisplayName": "Send Response"
                }
              ]
            },
            {
              "Name": "Respond",
              "DisplayName": "Respond",
              "Description": "...",
              "PercentComplete": 90,
              "Value": 1,
              "Style": "Success",
              "Transitions": [
                {
                  "AllowedRoles": [],
                  "AllowedInstanceRoles": ["Requestor"],
                  "Description": "...",
                  "Function": "SendRequest",
                  "NextStates": [ "Request" ],
                  "DisplayName": "Send Request"
                }
              ]
            }
          ]
        }
      ]
    }
    ```

3. `HelloBlockchain.json` dosyasını kaydedin.

Yapılandırma dosyasının birkaç bölümü vardır. Her bölüm le ilgili ayrıntılar aşağıdaki gibidir:

### <a name="application-metadata"></a>Uygulama meta verileri

Yapılandırma dosyasının başında uygulama adı ve açıklaması da dahil olmak üzere uygulama hakkında bilgi içerir.

### <a name="application-roles"></a>Uygulama rolleri

Uygulama rolleri bölümü, blockchain uygulamasında rol yapabilecek veya katılabilecek kullanıcı rollerini tanımlar. İşlevsellik temeline dayalı farklı roller kümesi tanımlarsınız. İstek-yanıt senaryosunda, istek oluşturan bir varlık olarak istekverenin işlevselliği ile yanıt üreten bir kuruluş olarak yanıtlayıcı arasında bir ayrım vardır.

### <a name="workflows"></a>İş akışları

İş akışları, sözleşmenin bir veya daha fazla aşamasını ve eylemlerini tanımlar. İstek-yanıt senaryosunda, iş akışının ilk aşaması (durum) bir istekçi (rol) bir istek (işlev) göndermek için bir eylem (geçiş) alır. Sonraki aşama (durum) yanıtlayıcı (rol) bir yanıt (işlev) göndermek için bir eylem (geçiş) alır. Bir uygulamanın iş akışı özellikleri, işlevleri ve gerekli durumları bir sözleşmenin akışını açıklamak içerebilir.

Yapılandırma dosyalarının içeriği hakkında daha fazla bilgi için [Azure Blockchain İş Akışı yapılandırma başvurusuna](configuration.md)bakın.

## <a name="smart-contract-code-file"></a>Akıllı sözleşme kodu dosyası

Akıllı sözleşmeler blockchain uygulamasının iş mantığını temsil ediyor. Şu anda Blockchain Workbench, blockchain defteri için Ethereum'u desteklemektedir. Ethereum, akıllı sözleşmeler için kendi kendini zorlayan iş mantığı yazmak için programlama dili olarak [Solidity](https://solidity.readthedocs.io) kullanır.

Solidity'deki akıllı sözleşmeler nesne yönelimli dillerdeki sınıflara benzer. Her sözleşme, akıllı sözleşmenin aşamalarını ve eylemlerini uygulamak için durum ve işlevler içerir.

Sık kullanılan düzenleyicinizde , `HelloBlockchain.sol`'.

### <a name="version-pragma"></a>Sürüm pragma

En iyi uygulama olarak, hedeflediğiniz Solidity sürümünü belirtin. Sürümü belirtmek, gelecekteki Solidity sürümleriyle uyumsuzlukları önlemeye yardımcı olur.

Akıllı sözleşme kodu dosyasının `HelloBlockchain.sol` üst kısmında aşağıdaki sürümü pragma ekleyin.

``` solidity
pragma solidity >=0.4.25 <0.6.0;
```

### <a name="configuration-and-smart-contract-code-relationship"></a>Yapılandırma ve akıllı sözleşme kodu ilişkisi

Blockchain Workbench, blockchain uygulaması oluşturmak için yapılandırma dosyasını ve akıllı sözleşme kodu dosyasını kullanır. Yapılandırmada tanımlanan ile akıllı sözleşmedeki kod arasında bir ilişki vardır. Uygulamayı oluşturmak için sözleşme ayrıntıları, işlevleri, parametreleri ve türleri eşleştirmek için gereklidir. Blockchain Workbench, uygulama oluşturmadan önce dosyaları doğrular.

### <a name="contract"></a>Sözleşme

**Sözleşme** üstbilgisini akıllı `HelloBlockchain.sol` sözleşme kodu dosyanıza ekleyin.

``` solidity
contract HelloBlockchain {
```

### <a name="state-variables"></a>Durum değişkenleri

Durum değişkenleri her sözleşme örneği için durum değerlerini depolar. Sözleşmenizdeki durum değişkenleri, yapılandırma dosyasında tanımlanan iş akışı özellikleriyle eşleşmelidir.

Akıllı sözleşme kodu dosyanızda `HelloBlockchain.sol` sözleşmenize durum değişkenlerini ekleyin.

``` solidity
    //Set of States
    enum StateType { Request, Respond}

    //List of properties
    StateType public  State;
    address public  Requestor;
    address public  Responder;

    string public RequestMessage;
    string public ResponseMessage;
```

### <a name="constructor"></a>Oluşturucu

Oluşturucu, iş akışının yeni bir akıllı sözleşme örneği için giriş parametrelerini tanımlar. Konstrüktör için gerekli parametreler yapılandırma dosyasında yapıcı parametreler olarak tanımlanır. Parametrelerin sayısı, sırası ve türü her iki dosyada da eşleşmelidir.

Oluşturucu işlevinde, sözleşmeyi oluşturmadan önce gerçekleştirmek istediğiniz herhangi bir iş mantığını yazın. Örneğin, başlangıç değerleri yle durum değişkenlerini başlatın.

Akıllı sözleşme kodu dosyanızda `HelloBlockchain.sol` sözleşmenize kurucu işlevini ekleyin.

``` solidity
    // constructor function
    constructor(string memory message) public
    {
        Requestor = msg.sender;
        RequestMessage = message;
        State = StateType.Request;
    }
```

### <a name="functions"></a>İşlevler

İşlevler, bir sözleşme deki iş mantığının yürütülebilir birimleridir. İşlev için gerekli parametreler yapılandırma dosyasında işlev parametreleri olarak tanımlanır. Parametrelerin sayısı, sırası ve türü her iki dosyada da eşleşmelidir. İşlevler, yapılandırma dosyasındaki Blockchain Çalışma Tezgahı iş akışındaki geçişlerle ilişkilidir. Geçiş, sözleşme tarafından belirlenen bir uygulamanın iş akışının bir sonraki aşamasına geçmek için gerçekleştirilen bir eylemdir.

İşleviçinde gerçekleştirmek istediğiniz herhangi bir iş mantığını yazın. Örneğin, bir durum değişkeninin değerini değiştirme.

1. Akıllı sözleşme kodu dosyanızda `HelloBlockchain.sol` sözleşmenize aşağıdaki işlevleri ekleyin.

    ``` solidity
        // call this function to send a request
        function SendRequest(string memory requestMessage) public
        {
            if (Requestor != msg.sender)
            {
                revert();
            }
    
            RequestMessage = requestMessage;
            State = StateType.Request;
        }
    
        // call this function to send a response
        function SendResponse(string memory responseMessage) public
        {
            Responder = msg.sender;
    
            ResponseMessage = responseMessage;
            State = StateType.Respond;
        }
    }
    ```

2. Akıllı `HelloBlockchain.sol` sözleşme kodu dosyanızı kaydedin.

## <a name="add-blockchain-application-to-blockchain-workbench"></a>Blockchain Workbench'e blockchain uygulaması ekleme

Blockchain Workbench'e blockchain uygulaması eklemek için, uygulamayı tanımlamak için yapılandırmayı ve akıllı sözleşme dosyalarını yüklersiniz.

1. Bir web tarayıcısında Blockchain Workbench web adresine gidin. Örneğin, `https://{workbench URL}.azurewebsites.net/` Blockchain Workbench'i dağıttığınızda web uygulaması oluşturulur. Blockchain Workbench web adresinizi nasıl bulacağınız hakkında bilgi için [Blockchain Workbench Web URL'sine](deploy.md#blockchain-workbench-web-url) bakın
2. [Blockchain Workbench yöneticisi](manage-users.md#manage-blockchain-workbench-administrators)olarak oturum açın.
3. **Yeni Uygulamaları** > **Seçin.** **Yeni uygulama** bölmesi görüntülenir.
4. Oluşturduğunuz **HelloBlockchain.json** yapılandırma dosyasını bulmak için **sözleşme yapılandırmasına** > **Göz At** yükle'yi seçin. Yapılandırma dosyası otomatik olarak doğrulanır. Doğrulama hatalarını görüntülemek için **Göster** bağlantısını seçin. Uygulamayı dağıtmadan önce doğrulama hatalarını düzeltin.
5. **HelloBlockchain.sol** akıllı sözleşme kodu **dosyasını**bulmak için sözleşme kodunu > Yükle'yi**seçin.** Kod dosyası otomatik olarak doğrulanır. Doğrulama hatalarını görüntülemek için **Göster** bağlantısını seçin. Uygulamayı dağıtmadan önce doğrulama hatalarını düzeltin.
6. Yapılandırmayı ve akıllı sözleşme dosyalarını temel alan blockchain uygulamasını oluşturmak için **Dağıt'ı** seçin.

Blockchain uygulamasının dağıtımı birkaç dakika sürer. Dağıtım tamamlandığında, yeni uygulama **Uygulamalar'da**görüntülenir. 

> [!NOTE]
> [Ayrıca Azure Blockchain Workbench REST API'yi](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)kullanarak blockchain uygulamaları oluşturabilirsiniz.

## <a name="add-blockchain-application-members"></a>Blockchain uygulama üyeleri ekleme

Sözleşmeleri başlatmak ve eylemde bulunmak için uygulama üyelerini uygulamanıza ekleyin. Uygulama üyeleri eklemek için [Blockchain Workbench yöneticisi](manage-users.md#manage-blockchain-workbench-administrators)olmanız gerekir.

1. **Uygulamalar** > **Merhaba, Blockchain seçin!**.
2. Uygulamayla ilişkili üye sayısı sayfanın sağ üst köşesinde görüntülenir. Yeni bir uygulama için üye sayısı sıfır olacaktır.
3. Sayfanın sağ üst köşesindeki **üyeler** bağlantısını seçin. Uygulama için geçerli bir üye listesi görüntülenir.
4. Üyelik listesinde üye **ekle'yi**seçin.
5. Eklemek istediğiniz üyenin adını seçin veya girin. Yalnızca Blockchain Workbench kiracısında bulunan Azure AD kullanıcıları listelenir. Kullanıcı bulunamazsa, [Azure AD kullanıcıları eklemeniz](manage-users.md#add-azure-ad-users)gerekir.
6. Üyenin **Rolü'nü** seçin. İlk üye için rol olarak **İstekçi'yi** seçin.
7. İlişkili role sahip üyeyi uygulamaya eklemek için **Ekle'yi** seçin.
8. **Yanıtlayan** rolüyle uygulamaya başka bir üye ekleyin.

Blockchain Workbench'teki kullanıcıları yönetme hakkında daha fazla bilgi için [Azure Blockchain Workbench'teki kullanıcıları yönetme](manage-users.md) ye bakın

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılabilir makalesinde, temel bir istek ve yanıt uygulaması oluşturdunuz. Uygulamayı nasıl kullanacağınızı öğrenmek için, sonraki nasıl yapılacağını makaleye devam edin.

> [!div class="nextstepaction"]
> [Blockchain uygulamasını kullanma](use.md)
