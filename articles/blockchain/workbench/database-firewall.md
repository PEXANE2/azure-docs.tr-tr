---
title: Configure Azure Blockchain Workbench database firewall
description: Learn how to configure the Azure Blockchain Workbench Preview database firewall to allow external clients and applications to connect.
ms.date: 09/09/2019
ms.topic: article
ms.reviewer: mmercuri
ms.openlocfilehash: e6739b7ead9299f020465d3c50ed01826334ca76
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74325995"
---
# <a name="configure-the-azure-blockchain-workbench-database-firewall"></a>Azure Blockchain Workbench veritabanı güvenlik duvarını yapılandırma

Bu makalede Azure portalını kullanarak güvenlik duvarı kuralı yapılandırma gösterilir. Güvenlik duvarı kuralları, dış istemcilerin veya uygulamaların Azure Blockchain Workbench veritabanınıza bağlanmasına imkan tanır.

## <a name="connect-to-the-blockchain-workbench-database"></a>Blockchain Workbench veritabanına bağlanma

Bir kural yapılandırmak istediğiniz veritabanına bağlanmak için:

1. Sign in to the Azure portal with an account that has **Owner**     permissions for the Azure Blockchain Workbench resources.
2. Sol gezinti bölmesinden **Kaynak Grupları**'nı seçin.
3. Blockchain Workbench dağıtımınıza yönelik kaynak grubunun adını seçin.
4. Kaynakları sıralamak için **Tür**’ü, sonra da **SQL sunucunuzu** seçin.
5. Aşağıdaki ekran görüntüsünde yer alan kaynak listesi örneğinde iki veritabanı görülüyor: *master* ve *lsgn-sdk*. Güvenlik duvarı kuralını *lsgn-sdk* üzerinde yapılandırırsınız.

![Blockchain Workbench kaynaklarını listeleme](./media/database-firewall/list-database-resources.png)

## <a name="create-a-database-firewall-rule"></a>Veritabanı güvenlik duvarı kuralı oluşturma

Güvenlik duvarı kuralı oluşturmak için:

1. "lsgn-sdk" veritabanının bağlantısını seçin.
2. Menü çubuğundan **Sunucu güvenlik duvarı ayarla**’yı seçin.

   ![Sunucu güvenlik duvarı ayarla](./media/database-firewall/configure-server-firewall.png)

3. Kuruluşunuza yönelik bir kural oluşturmak için:

   * Bir **KURAL ADI** girin
   * Adres aralığının **BAŞLANGIÇ IP'Sİ** için bir IP adresi girin
   * Adres aralığının **BİTİŞ IP'Sİ** için bir IP adresi girin

   ![Güvenlik duvarı kuralı oluşturma](./media/database-firewall/create-firewall-rule.png)

    > [!NOTE]
    > Yalnızca bilgisayarınızın IP adresini eklemek istiyorsanız **+ İstemci IP'si Ekle**’yi seçin.
        
1. Güvenlik duvarı yapılandırmanızı kaydetmek için **Kaydet**’i seçin.
2. Bir uygulamadan veya araçtan bağlanarak veritabanı için yapılandırdığınız IP adresi aralığını test edin. Örneğin, SQL Server Management Studio.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench’te veritabanı görünümleri](database-views.md)