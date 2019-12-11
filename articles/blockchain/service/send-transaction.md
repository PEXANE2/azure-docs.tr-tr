---
title: Akıllı sözleşmeleri oluşturma, derleme, & dağıtma öğreticisi-Azure blok zinciri hizmeti
description: Azure blok zinciri hizmetinde akıllı bir sözleşme oluşturmak, derlemek ve dağıtmak için Visual Studio Code 'de Ethereum uzantısı için Azure blok zinciri geliştirme seti 'ni kullanma hakkında öğretici.
ms.date: 12/06/2019
ms.topic: tutorial
ms.reviewer: chrisseg
ms.openlocfilehash: 5b901ab904425a22d2fe9643ffa75a4e978efa88
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74972879"
---
# <a name="tutorial-create-buildanddeploysmartcontracts-on-azure-blockchain-service"></a>Öğretici: Azure blok zinciri hizmetinde akıllı sözleşmeler oluşturma, derleme ve dağıtma

Bu öğreticide, Azure blok zinciri hizmetinde akıllı bir sözleşme oluşturmak, derlemek ve dağıtmak için Visual Studio Code 'de Ethereum uzantısı için Azure blok zinciri geliştirme setini kullanın. Ayrıca, bir işlem aracılığıyla akıllı sözleşme işlevini yürütmek için geliştirme setini da kullanırsınız.

Ethereum için Azure blok zinciri geliştirme setini şu şekilde kullanabilirsiniz:

> [!div class="checklist"]
> * Akıllı sözleşme oluşturma
> * Akıllı sözleşme dağıtma
> * Bir işlem aracılığıyla akıllı sözleşme işlevini yürütme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Hızlı başlangıç: Azure blok zinciri hizmeti Consortium ağına bağlanmak için Visual Studio Code kullanma](connect-vscode.md)
* [Visual Studio Code](https://code.visualstudio.com/Download)
* [Ethereum uzantısı için Azure blok zinciri geliştirme seti](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain)
* [Node. js 10.15. x veya üzeri](https://nodejs.org/download)
* [Git 2.10. x veya üzeri](https://git-scm.com)
* [Python 2.7.15](https://www.python.org/downloads/release/python-2715/) Yolunuza Python. exe ' yi ekleyin. Azure blok zinciri geliştirme seti için yolunuzda Python sürüm 2.7.15 gereklidir.
* [Truffle 5.0.0](https://www.trufflesuite.com/docs/truffle/getting-started/installation)
* [Ganache CLı 6.0.0](https://github.com/trufflesuite/ganache-cli)

Windows 'da, Node- C++ JP modülü için yüklü bir derleyici gerekir. MSBuild araçlarını kullanabilirsiniz:

* Visual Studio 2017 yüklüyse, NPM 'yi komutuyla MSBuild araçlarını kullanacak şekilde yapılandırın `npm config set msvs_version 2017 -g`
* Visual Studio 2019 yüklüyse, NPM için MS derleme araçları yolunu ayarlayın. Örneğin, `npm config set msbuild_path "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin\MSBuild.exe"`
* Aksi takdirde, yükseltilmiş bir *yönetici olarak çalıştır* komut kabuğu `npm install --global windows-build-tools` kullanarak tek başına vs Build araçlarını yükleyemezsiniz.

Node-cayp hakkında daha fazla bilgi için [GitHub 'daki node-cayp deposuna](https://github.com/node-gyp)bakın.

## <a name="create-a-smart-contract"></a>Akıllı sözleşme oluşturma

Ethereum için Azure blok zinciri geliştirme seti, sözleşmeleri dolandırmaya, oluşturmaya ve dağıtmanıza yardımcı olmak için proje şablonları ve truffle araçlarını kullanır. Başlamadan önce önkoşul hızlı başlangıcını doldurun [: Azure blok zinciri hizmeti Consortium ağına bağlanmak için Visual Studio Code kullanın](connect-vscode.md). Hızlı başlangıç, Ethereum için Azure blok zinciri geliştirme setini yükleme ve yapılandırma sürecinde size rehberlik eder.

1. VS Code komut paletinden **Azure blok Zinciri: New Solidity Project**' i seçin.
1. **Temel proje oluştur**seçeneğini belirleyin.
1. `HelloBlockchain` adlı yeni bir klasör oluşturun ve **Yeni proje yolu**' nu seçin.

Azure blok zinciri geliştirme seti sizin için yeni bir Solidity projesi oluşturur ve başlatır. Temel proje, örnek bir **Helloblockzincirine** akıllı sözleşme ve Azure blok zinciri hizmeti 'nde Consortium üyelemenize dağıtmak için gereken tüm dosyaları içerir. Projenin oluşturulması birkaç dakika sürebilir. Azure blok zinciri için çıktıyı seçerek VS Code 'ın Terminal panelinde ilerleme durumunu izleyebilirsiniz.

Proje yapısı aşağıdaki örneğe benzer şekilde görünür:

   ![Solidity projesi](./media/send-transaction/solidity-project.png)

## <a name="build-a-smart-contract"></a>Akıllı sözleşme oluşturma

Akıllı sözleşmeler, projenin **sözleşmeler** dizininde bulunur. Akıllı sözleşmeleri bir blok zincirine dağıtmadan önce derleyebilirsiniz. Projenizdeki tüm akıllı sözleşmeleri derlemek için, **Yapı sözleşmeleri** komutunu kullanın.

1. VS Code Gezgini kenar çubuğunda, projenizdeki **sözleşmeler** klasörünü genişletin.
1. **Helloblockzincirine. Nuevo** öğesine sağ tıklayın ve menüden **sözleşmeleri oluştur** ' u seçin.

    ![Yapı sözleşmeleri menüsünü seçin ](./media/send-transaction/build-contracts.png)

Azure blok zinciri geliştirme seti, akıllı sözleşmeleri derlemek için Truffle kullanır.

![Truffle derleyici çıkışı](./media/send-transaction/compile-output.png)

## <a name="deploy-a-smart-contract"></a>Akıllı sözleşme dağıtma

Truffle, sözleşmelerinizi bir Ethereum ağına dağıtmak için geçiş betikleri kullanır. Geçişler, projenin **geçiş** dizininde bulunan JavaScript dosyalarıdır.

1. Akıllı sözleşmenizi dağıtmak için, **Helloblockzincirine. Nuevo** öğesine sağ tıklayın ve menüden **sözleşmeleri dağıt** ' ı seçin.
1. Komut paletinde Azure blok zinciri Konsorsiyumu ağınızı seçin. Projeyi oluştururken, konsorsiyum blok zinciri ağı projenin Truffle yapılandırma dosyasına eklendi.
1. **Anımsatıcı üret**' i seçin. Bir dosya adı seçin ve anımsatıcı dosyasını proje klasörüne kaydedin. Örneğin, `myblockchainmember.env`. Anımsatıcı dosyası, blok zinciri üyesiyseniz bir Ethereum özel anahtarı oluşturmak için kullanılır.

Azure blok zinciri geliştirme seti, sözleşmeleri blok zincirine dağıtmak üzere geçiş betiğini yürütmek için Truffle kullanır.

![Sözleşme başarıyla dağıtıldı](./media/send-transaction/deploy-contract.png)

## <a name="call-a-contract-function"></a>Sözleşme işlevini çağırma

**Helloblockzincirleri** sözleşmesinin **SendRequest** işlevi **RequestMessage** durum değişkenini değiştirir. Bir blok zinciri ağının durumunun değiştirilmesi bir işlem aracılığıyla yapılır. Bir işlem aracılığıyla **SendRequest** işlevini çağırmak Için Azure blok zinciri geliştirme seti Akıllı sözleşme etkileşimi sayfasını kullanabilirsiniz.

1. Akıllı sözleşmeniz ile etkileşime geçmek için, **Helloblockzincirine. Nuevo** öğesine sağ tıklayın ve menüden **akıllı sözleşme etkileşimi sayfasını göster** ' i seçin.

    ![Menüden akıllı sözleşme etkileşimi sayfasını göster ' i seçin](./media/send-transaction/contract-interaction.png)

1. Etkileşim sayfası, dağıtılan bir sözleşme sürümü seçmenize, işlevleri çağıralmanıza, geçerli durumu görüntülemenize ve meta verileri görüntülemenize olanak sağlar.

    ![Örnek akıllı sözleşme etkileşimi sayfası](./media/send-transaction/interaction-page.png)

1. Akıllı sözleşme işlevini çağırmak için, sözleşme eylemini seçin ve bağımsız değişkenlerinizi geçirin. **SendRequest** sözleşme eylemini seçin ve **Merhaba, blok zinciri! girin!** **RequestMessage** parametresi için. Bir işlem aracılığıyla **SendRequest** işlevini çağırmak için **Yürüt** ' ü seçin.

    ![SendRequest eylemini Yürüt](./media/send-transaction/sendrequest-action.png)

İşlem işlendikten sonra, etkileşim bölümü durum değişikliklerini yansıtır.

![Sözleşme durumu değişiklikleri](./media/send-transaction/contract-state.png)

SendRequest işlevi **RequestMessage** ve **State** alanlarını ayarlar. **RequestMessage** için geçerli durum, **Hello, blockzincirine**geçirilen bağımsız değişkendir. **Durum** alanı değeri **istek**olarak kalmaya devam eder.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, *blok zinciri üye* önkoşul hızlı başlangıcı ' nda oluşturduğunuz `myResourceGroup` kaynak grubunu silerek kaynakları silebilirsiniz.

Kaynak grubunu silmek için:

1. Azure portal sol gezinti bölmesindeki **kaynak grubu** ' na gidin ve silmek istediğiniz kaynak grubunu seçin.
1. **Kaynak grubunu sil**'i seçin. Kaynak grubu adını girip **Sil**' i seçerek silmeyi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure blok zinciri geliştirme seti 'ni kullanarak örnek bir Solidity projesi oluşturdunuz. Azure blok zinciri hizmeti 'nde barındırılan bir blok zinciri konsorsiyum ağı üzerinde bir işlem aracılığıyla işlev olarak adlandırılan akıllı bir sözleşmeyi oluşturup dağıttığınız.

> [!div class="nextstepaction"]
> [Azure blok zinciri hizmetini kullanarak blok zinciri uygulamaları geliştirme](develop.md)
