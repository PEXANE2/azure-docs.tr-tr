---
title: Akıllı sözleşmeler oluşturma, oluşturma & dağıtma eğitimi - Azure Blockchain Hizmeti
description: Visual Studio Code'daki Ethereum uzantısı için Azure Blockchain Geliştirme Kiti'ni, Azure Blockchain Hizmeti'nde akıllı bir sözleşme oluşturmak, oluşturmak ve dağıtmak için nasıl kullanacağınıza ilişkin öğretici.
ms.date: 12/06/2019
ms.topic: tutorial
ms.reviewer: chrisseg
ms.openlocfilehash: 5b901ab904425a22d2fe9643ffa75a4e978efa88
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74972879"
---
# <a name="tutorial-create-buildanddeploysmartcontracts-on-azure-blockchain-service"></a>Öğretici: Azure Blockchain Hizmetinde akıllı sözleşmeler oluşturun, oluşturun ve dağıtın

Bu eğitimde, Azure Blockchain Hizmeti'nde akıllı bir sözleşme oluşturmak, oluşturmak ve dağıtmak için Visual Studio Code'daki Ethereum uzantısı için Azure Blockchain Geliştirme Kiti'ni kullanın. Ayrıca, bir işlem aracılığıyla akıllı bir sözleşme işlevini yürütmek için geliştirme kitini de kullanırsınız.

Ethereum için Azure Blockchain Geliştirme Kiti'ni şu şekilde kullanıyorsunuz:

> [!div class="checklist"]
> * Akıllı bir sözleşme oluşturma
> * Akıllı bir sözleşme dağıtma
> * Bir işlem aracılığıyla akıllı sözleşme işlevini yürütme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Tam [Quickstart: Azure Blockchain Hizmeti konsorsiyum ağına bağlanmak için Visual Studio Kodunu kullanın](connect-vscode.md)
* [Visual Studio Code](https://code.visualstudio.com/Download)
* [Ethereum uzantısı için Azure Blockchain Geliştirme Kiti](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain)
* [Düğüm.js 10.15.x veya üzeri](https://nodejs.org/download)
* [Git 2.10.x veya üzeri](https://git-scm.com)
* [Piton 2.7.15](https://www.python.org/downloads/release/python-2715/) Yolunuza python.exe ekleyin. Python sürüm 2.7.15, Azure Blockchain Geliştirme Kiti için gereklidir.
* [Trüf Mantarı 5.0.0](https://www.trufflesuite.com/docs/truffle/getting-started/installation)
* [Ganache CLI 6.0.0](https://github.com/trufflesuite/ganache-cli)

Windows'da düğüm-gyp modülü için yüklü bir C++ derleyicisi gereklidir. MSBuild araçlarını kullanabilirsiniz:

* Visual Studio 2017 yüklüyse, MSBuild araçlarını komutla kullanacak şekilde npm yapılandırın`npm config set msvs_version 2017 -g`
* Visual Studio 2019 yüklüyse, MS build araçları yolunu npm için ayarlayın. Örneğin, `npm config set msbuild_path "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin\MSBuild.exe"`
* Aksi takdirde, yönetici komut uyruşu olarak yükseltilmiş bir `npm install --global windows-build-tools` *Çalıştır'da* tek başına VS Build araçlarını yükleyin.

Düğüm-gyp hakkında daha fazla bilgi için [GitHub'daki düğüm-gyp deposuna](https://github.com/node-gyp)bakın.

## <a name="create-a-smart-contract"></a>Akıllı bir sözleşme oluşturma

Ethereum için Azure Blockchain Geliştirme Kiti, sözleşmeleri iskeleye bağlamaya, oluşturmaya ve dağıtmaya yardımcı olmak için proje şablonları ve Trüf araçları kullanır. Başlamadan önce [Quickstart: Azure Blockchain Service konsorsiyum ağına bağlanmak için Visual Studio Kodunu kullanın.](connect-vscode.md) Hızlı başlatma, Ethereum için Azure Blockchain Geliştirme Kiti'nin yüklenmesi ve yapılandırması boyunca size yol sunar.

1. VS Code komut paletinden **Azure Blockchain: New Solidity Project'i**seçin.
1. **Temel proje oluştur'u**seçin.
1. Adlandırılmış `HelloBlockchain` yeni bir klasör oluşturun ve **yeni proje yolunu seçin.**

Azure Blockchain Geliştirme Kiti sizin için yeni bir Solidity projesi oluşturur ve başlatır. Temel proje, örnek bir **HelloBlockchain** akıllı sözleşmesini ve Azure Blockchain Hizmeti'ndeki konsorsiyum üyenize oluşturmak ve dağıtmak için gerekli tüm dosyaları içerir. Projenin oluşturulması birkaç dakika sürebilir. Azure Blockchain çıktısını seçerek VS Code'un terminal panelindeki ilerlemeyi izleyebilirsiniz.

Proje yapısı aşağıdaki örnek gibi görünür:

   ![Sağlamlık projesi](./media/send-transaction/solidity-project.png)

## <a name="build-a-smart-contract"></a>Akıllı bir sözleşme oluşturun

Akıllı sözleşmeler projenin **sözleşme** dizinde yer alır. Akıllı sözleşmeleri blockchain'e dağıtmadan önce derlersiniz. Projenizdeki tüm akıllı sözleşmeleri derlemek için **Sözleşmeler Oluştur** komutunu kullanın.

1. VS Code explorer kenar çubuğunda, projenizdeki **sözleşmeler** klasörünü genişletin.
1. **HelloBlockchain.sol'a** sağ tıklayın ve menüden **Sözleşmeler Oluştur'u** seçin.

    ![Sözleşmeler Oluştur menüsünü seçin ](./media/send-transaction/build-contracts.png)

Azure Blockchain Geliştirme Kiti, akıllı sözleşmeleri derlemek için Truffle'ı kullanır.

![Trüf mantarı derleyici çıkışı](./media/send-transaction/compile-output.png)

## <a name="deploy-a-smart-contract"></a>Akıllı bir sözleşme dağıtma

Truffle, sözleşmelerinizi bir Ethereum ağına dağıtmak için geçiş komut dosyalarını kullanır. Geçişler, projenin **geçişler** dizininde bulunan JavaScript dosyalarıdır.

1. Akıllı sözleşmenizi dağıtmak için **HelloBlockchain.sol'a** sağ tıklayın ve menüden **Sözleşmeleri Dağıt'ı** seçin.
1. Komut paletinde Azure Blockchain konsorsiyum ağınızı seçin. Projeyi oluşturduğunuzda konsorsiyum blockchain ağı projenin Truffle yapılandırma dosyasına eklendi.
1. **Mnemonic oluştur'u**seçin. Bir dosya adı seçin ve mnemonik dosyayı proje klasörüne kaydedin. Örneğin, `myblockchainmember.env`. Mnemonic dosyası blockchain üyeniz için bir Ethereum özel anahtarı oluşturmak için kullanılır.

Azure Blockchain Geliştirme Kiti, sözleşmeleri blockchain'e dağıtmak için geçiş komut dosyasını çalıştırmak için Truffle'ı kullanır.

![Başarıyla dağıtılan sözleşme](./media/send-transaction/deploy-contract.png)

## <a name="call-a-contract-function"></a>Sözleşme işlevini çağırma

**HelloBlockchain** sözleşmesinin **SendRequest** işlevi **RequestMessage** durum değişkenini değiştirir. Blockchain ağının durumunu değiştirmek bir işlem üzerinden yapılır. Bir işlem aracılığıyla **SendRequest** işlevini aramak için Azure Blockchain Geliştirme Kiti akıllı sözleşme etkileşim sayfasını kullanabilirsiniz.

1. Akıllı sözleşmenizle etkileşimkurmak için **HelloBlockchain.sol'a** sağ tıklayın ve menüden **Akıllı Sözleşme Etkileşim Sayfasını Göster'i** seçin.

    ![Menüden Akıllı Sözleşme Etkileşim sayfasını göster'i seçin](./media/send-transaction/contract-interaction.png)

1. Etkileşim sayfası, dağıtılmış bir sözleşme sürümü seçmenize, arama işlevlerini seçmenize, geçerli durumu görüntülemenize ve meta verileri görüntülemenize olanak tanır.

    ![Örnek Akıllı Sözleşme Etkileşim Sayfası](./media/send-transaction/interaction-page.png)

1. Akıllı sözleşme işlevini çağırmak için sözleşme eylemini seçin ve bağımsız değişkenlerinizi geçirin. **SendRequest** sözleşme eylemini seçin ve **Hello, Blockchain'e girin!** **requestMessage** parametresi için. Bir işlem aracılığıyla **SendRequest** işlevini çağırmak için Yürüt'''ün'u seçin. **Execute**

    ![SendRequest eylemini yürütme](./media/send-transaction/sendrequest-action.png)

Hareket işlendikten sonra, etkileşim bölümü durum değişikliklerini yansıtır.

![Sözleşme durumu değişiklikleri](./media/send-transaction/contract-state.png)

SendRequest işlevi **İstek İletisi** ve **Durum** alanlarını ayarlar. **RequestMessage** için geçerli durum **Merhaba, Blockchain**geçti bağımsız değişkendir. **Durum** alan değeri **İstek**kalır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, blockchain üye ön koşul `myResourceGroup` *quickstart'ında* oluşturduğunuz kaynak grubunu silerek kaynakları silebilirsiniz.

Kaynak grubunu silmek için:

1. Azure portalında, sol gezinti bölmesindeki **Kaynak grubuna** gidin ve silmek istediğiniz kaynak grubunu seçin.
1. **Kaynak grubunu sil**'i seçin. Kaynak grup adını girerek silme işlemini doğrulayın ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Blockchain Geliştirme Kiti'ni kullanarak örnek bir Katılık projesi oluşturdunuz. Azure Blockchain Hizmeti'nde barındırılan blockchain konsorsiyumağında bir işlem yoluyla işlev olarak adlandırılan akıllı bir sözleşme oluşturup dağıttınız.

> [!div class="nextstepaction"]
> [Azure Blockchain Hizmetini kullanarak blockchain uygulamaları geliştirme](develop.md)
