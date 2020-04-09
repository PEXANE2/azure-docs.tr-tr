---
title: Azure Blockchain Hizmeti hareket düğümlerini yapılandırma
description: Azure Blockchain Hizmeti işlem düğümleri nasıl yapılandırılır?
ms.date: 11/20/2019
ms.topic: article
ms.reviewer: janders
ms.openlocfilehash: 8fa18496d0c1aa59beb55569e731967d5ebea427
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80876938"
---
# <a name="configure-azure-blockchain-service-transaction-nodes"></a>Azure Blockchain Hizmeti hareket düğümlerini yapılandırma

İşlem düğümleri, blockchain hareketlerini ortak bir bitiş noktası üzerinden Azure Blockchain Hizmetine göndermek için kullanılır. Varsayılan hareket düğümü, blockchain'de kayıtlı Ethereum hesabının özel anahtarını içerir ve bu nedenle silinemez.

Varsayılan hareket node ayrıntılarını görüntülemek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure Blockchain Service üyenize gidin. **Hareket düğümlerini**seçin.

    ![Varsayılan hareket düğümünü seçin](./media/configure-transaction-nodes/nodes.png)

    Genel bakış ayrıntıları genel uç nokta adreslerini ve ortak anahtarı içerir.

## <a name="create-transaction-node"></a>İşlem düğümü oluşturma

Blockchain üyenize toplam 10 işlem düğümü için en fazla dokuz ek işlem düğümü ekleyebilirsiniz. Hareket düğümleri ekleyerek ölçeklenebilirliği artırabilir veya yük dağıtabilirsiniz. Örneğin, farklı istemci uygulamaları için bir işlem düğümü bitiş noktası olabilir.

İşlem düğümü eklemek için:

1. Azure portalında Azure Blockchain Service üyenize gidin ve **Ekle > İşlem düğümlerini**seçin.
1. Yeni hareket düğümü nün ayarlarını tamamlayın.

    ![İşlem düğümü ekleme](./media/configure-transaction-nodes/add-node.png)

    | Ayar | Açıklama |
    |---------|-------------|
    | Adı | Hareket düğümü adı. Ad, hareket düğümü bitiş noktası için DNS adresini oluşturmak için kullanılır. Örneğin, `newnode-myblockchainmember.blockchain.azure.com`. Düğüm adı oluşturulduktan sonra değiştirilemez. |
    | Parola | Güçlü bir parola ayarlayın. Temel kimlik doğrulaması ile işlem düğümü bitiş noktasına erişmek için parolayı kullanın.

1. **Oluştur'u**seçin.

    Yeni bir hareket düğümü sağlama işlemi yaklaşık 10 dakika sürer. Ek hareket düğümleri maliyete tabidir. Maliyetler hakkında daha fazla [Azure pricing](https://aka.ms/ABSPricing)bilgi için Azure fiyatlandırması'na bakın.

## <a name="endpoints"></a>Uç Noktalar

Hareket düğümlerinin benzersiz bir DNS adı ve ortak uç noktaları vardır.

Bir hareket düğümün bitiş noktası ayrıntılarını görüntülemek için:

1. Azure portalında, Azure Blockchain Hizmeti üye işlem düğümlerinizden birine gidin ve **Genel Bakış'ı**seçin.

    ![Uç Noktalar](./media/configure-transaction-nodes/endpoints.png)

İşlem düğümü uç noktaları güvenlidir ve kimlik doğrulaması gerektirir. Azure AD kimlik doğrulaması, HTTPS temel kimlik doğrulaması ve TLS üzerinden HTTPS veya Websocket üzerinden bir erişim anahtarı nı kullanarak bir işlem bitiş noktasına bağlanabilirsiniz.

### <a name="azure-active-directory-access-control"></a>Azure Active Directory erişim denetimi

Azure Blockchain Hizmet hareket düğümü uç noktaları Azure Etkin Dizin (Azure AD) kimlik doğrulamasını destekler. Azure AD kullanıcısına, grubuna ve hizmet temel indeki lere bitiş noktanıza erişebilirsiniz.

Azure AD erişim denetimini bitiş noktanıza vermek için:

1. Azure portalında Azure Blockchain Service üyenize gidin ve Rol Atama ekle > ekle > **> Access denetimini (IAM)** seçin.
1. Kullanıcı, grup veya hizmet ilkesi (uygulama rolleri) için yeni bir rol ataması oluşturun.

    ![IAM rolünü ekle](./media/configure-transaction-nodes/add-role.png)

    | Ayar | Eylem |
    |---------|-------------|
    | Rol | Sahibi , **Katkıda Bulunan**veya **Okuyucu'yı**seçin. **Owner**
    | Şunlara erişim ata: | **Azure AD kullanıcı, grup veya hizmet sorumlusu'nu**seçin.
    | Şunu seçin: | Eklemek istediğiniz kullanıcıyı, grubu veya hizmet ilkesini arayın.

1. Rol atamasını eklemek için **Kaydet'i** seçin.

Azure AD erişim denetimi hakkında daha fazla bilgi için [Bkz. RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönet](../../role-based-access-control/role-assignments-portal.md)

Azure AD kimlik doğrulamasını kullanarak nasıl bağlanılabağlanabilirsiniz hakkında ayrıntılı bilgi için, [AAD kimlik doğrulamasını kullanarak düğümünüze bağlan'a](configure-aad.md)bakın.

### <a name="basic-authentication"></a>Temel kimlik doğrulama

HTTPS temel kimlik doğrulaması için, kullanıcı adı ve parola kimlik bilgileri isteğin HTTPS üstbilgisinde bitiş noktasına geçirilir.

Azure portalında bir işlem düğümünün temel kimlik doğrulama bitiş noktası ayrıntılarını görüntüleyebilirsiniz. Azure Blockchain Hizmeti üye işlem düğümlerinizden birine gidin ve ayarlarda **Temel Kimlik Doğrulama'yı** seçin.

![Temel kimlik doğrulama](./media/configure-transaction-nodes/basic.png)

Kullanıcı adı düğümünüzün adıdır ve değiştirilemez.

URL'yi kullanmak \<için, düğüm sağlandığında parola kümesiyle değiştirin.\> **Parolayı Sıfırla**seçeneğini seçerek parolayı güncelleştirebilirsiniz.

### <a name="access-keys"></a>Erişim tuşları

Erişim anahtarı kimlik doğrulaması için erişim anahtarı bitiş noktası URL'sinde yer alan anahtardır. Hareket düğümü sağlandığında, iki erişim anahtarı oluşturulur. Her iki erişim anahtarı da kimlik doğrulama için kullanılabilir. İki tuş tuşları değiştirmenizi ve döndürmenizi sağlar.

Bir işlem düğümünün erişim anahtarı ayrıntılarını görüntüleyebilir ve erişim anahtarlarını içeren bitiş noktası adreslerini kopyalayabilirsiniz. Azure Blockchain Hizmeti üye işlem düğümlerinizden birine gidin ve ayarlarda **Erişim Tuşları'nı** seçin.

### <a name="firewall-rules"></a>Güvenlik duvarı kuralları

Güvenlik duvarı kuralları, işlem düğümünüzde kimlik doğrulamagirişiminde bulunabilecek IP adreslerini sınırlamanızı sağlar.  İşlem düğümünüz için güvenlik duvarı kuralları yapılandırılmamışsa, herhangi bir taraf tarafından erişilemez.  

Bir işlem düğümünün güvenlik duvarı kurallarını görüntülemek için Azure Blockchain Hizmeti üye işlem düğümlerinizden birine gidin ve ayarlardaki **Güvenlik Duvarı kurallarını** seçin.

Güvenlik Duvarı kuralları ızgarasında bir kural adı, başlangıç IP adresi ve bitiş IP adresi girerek güvenlik duvarı **kuralları** ekleyebilirsiniz.

![Güvenlik duvarı kuralları](./media/configure-transaction-nodes/firewall-rules.png)

Etkinleştirmek için:

* **Tek IP adresi:** Başlangıç ve bitiş IP adresleri için aynı IP adresini yapılandırın.
* **IP adresi aralığı:** Başlangıç ve bitiş IP adresi aralığını yapılandırın. Örneğin, 10.221.34.0'dan başlayan ve 10.221.34.255'te biten bir aralık, tüm 10.221.34.xxx alt ağının tamamını sağlar.
* **Tüm IP adreslerine izin verin:** Başlangıç IP adresini 0.0.0.0 ve bitiş IP adresini 255.255.255.255 olarak yapılandırın.

## <a name="connection-strings"></a>Bağlantı dizeleri

İşlem düğümünüz için bağlantı dize sözdizimi temel kimlik doğrulaması veya erişim anahtarlarını kullanmak için sağlanır. HTTPS ve WebSockets üzerinden erişim anahtarları da dahil olmak üzere bağlantı dizeleri sağlanır.

İşlem düğümübağlantı dizelerini görüntüleyebilir ve bitiş noktası adreslerini kopyalayabilirsiniz. Azure Blockchain Service üye işlem düğümlerinizden birine gidin ve ayarlarda **Bağlantı dizelerini** seçin.

![Bağlantı dizeleri](./media/configure-transaction-nodes/connection-strings.png)

## <a name="sample-code"></a>Örnek kod

Web3, Nethereum, Web3js ve Trüf mantarı üzerinden işlem düğümünüze hızlı bir şekilde bağlanmayı etkinleştirmek için örnek kod sağlanır.

İşlem düğümüörnek bağlantı kodunu görüntüleyebilir ve popüler geliştirici araçlarıyla kullanmak üzere kopyalayabilirsiniz. Azure Blockchain Hizmeti üye işlem düğümlerinizden birine gidin ve ayarlarda **Örnek Kodu'ni** seçin.

Kullanmak istediğiniz kod örneğini görüntülemek için Web3, Nethereum, Truffle veya Web3j sekmesini seçin.

![Örnek kod](./media/configure-transaction-nodes/sample-code.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak işlem düğümlerini yapılandırma](manage-cli.md)
