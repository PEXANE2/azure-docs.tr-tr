---
title: Azure Blockchain Hizmetine bağlanmak için Visual Studio Kodunu kullanma
description: Visual Studio Code'daki Ethereum uzantısı için Azure Blockchain Geliştirme Kiti'ni kullanarak Azure Blockchain Hizmeti konsorsiyum ağına bağlanın
ms.date: 04/22/2020
ms.topic: quickstart
ms.reviewer: caleteet
ms.openlocfilehash: 8b502966317c5d07e89de4ae70ff72b899e963e6
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82084847"
---
# <a name="quickstart-use-visual-studio-code-to-connect-to-an-azure-blockchain-service-consortium-network"></a>Quickstart: Azure Blockchain Hizmeti konsorsiyum ağına bağlanmak için Visual Studio Kodunu kullanın

Bu hızlı başlangıçta, Azure Blockchain Hizmeti'ndeki bir konsorsiyuma bağlanmak için Ethereum Visual Studio Code (VS Code) uzantısı için Azure Blockchain Geliştirme Kiti'ni yükler ve kullanırsınız. Azure Blockchain Geliştirme Kiti, Ethereum blockchain defterlerinde akıllı sözleşmeler oluşturma, bağlama, oluşturma ve dağıtma şeklinizi basitleştirir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Tam [Hızlı Başlatma: Azure portalını veya Quickstart'ı kullanarak blockchain üyesi oluşturun:](create-member.md) [Azure CLI'yi kullanarak bir Azure Blockchain Hizmeti blockchain üyesi oluşturun](create-member-cli.md)
* [Visual Studio Code](https://code.visualstudio.com/Download)
* [Ethereum uzantısı için Azure Blockchain Geliştirme Kiti](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain)
* [Düğüm.js 10.15.x veya üzeri](https://nodejs.org)
* [Git 2.10.x veya üzeri](https://git-scm.com)
* [Piton 2.7.15](https://www.python.org/downloads/release/python-2715/) Yolunuza python.exe ekleyin. Azure Blockchain Geliştirme Kiti için Python sürümünün 2.7.15 sürümüne sahip olmak sizin yolunuzda olması gerekir.
* [Trüf Mantarı 5.0.0](https://www.trufflesuite.com/docs/truffle/getting-started/installation)
* [Ganache CLI 6.0.0](https://github.com/trufflesuite/ganache-cli)

Windows'da düğüm-gyp modülü için yüklü bir C++ derleyicisi gereklidir. MSBuild araçlarını kullanabilirsiniz:

* Visual Studio 2017 yüklüyse, MSBuild araçlarını komutla kullanacak şekilde npm yapılandırın`npm config set msvs_version 2017 -g`
* Visual Studio 2019 yüklüyse, MS build araçları yolunu npm için ayarlayın. Örneğin, `npm config set msbuild_path "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin\MSBuild.exe"`
* Aksi takdirde, yönetici komut uyruşu olarak yükseltilmiş bir `npm install --global windows-build-tools` *Çalıştır'da* tek başına VS Build araçlarını yükleyin.

Düğüm-gyp hakkında daha fazla bilgi için [GitHub'daki düğüm-gyp deposuna](https://github.com/nodejs/node-gyp)bakın.

### <a name="verify-azure-blockchain-development-kit-environment"></a>Azure Blockchain Geliştirme Kiti ortamını doğrulayın

Azure Blockchain Geliştirme Kiti, geliştirme ortamı ön koşullarınızın karşılandığını doğrular. Geliştirme ortamınızı doğrulamak için:

VS Code komut paletinden **Azure Blockchain: Hoş Geldiniz Sayfasını Göster'i**seçin.

Azure Blockchain Geliştirme Kiti, tamamlanması yaklaşık bir dakika süren bir doğrulama komut dosyası çalıştırıyor. **Terminal > Yeni Terminal'i**seçerek çıktıyı görüntüleyebilirsiniz. Terminal menü çubuğunda, açılır menüde **Çıktı** sekmesini ve **Azure Blockchain'i** seçin. Başarılı doğrulama aşağıdaki görüntügibi görünür:

![Geçerli geliştirme ortamı](./media/connect-vscode/valid-environment.png)

 Gerekli bir aracı nız yoksa, **Azure Blockchain Geliştirme Kiti - Önizleme** adlı yeni bir sekme indirme bağlantılarıolan gerekli araçları listeler.

![Dev kiti gerekli uygulamalar](./media/connect-vscode/required-apps.png)

Hızlı başlatmaile devam etmeden önce eksik ön koşulları yükleyin.

## <a name="connect-to-consortium-member"></a>Konsorsiyum üyesine bağlanın

Azure Blockchain Geliştirme Kiti VS Kodu uzantısını kullanarak konsorsiyum üyelerine bağlanabilirsiniz. Bir konsorsiyuma bağlandıktan sonra, bir Azure Blockchain Service konsorsiyumu üyesine akıllı sözleşmeler derleyebilir, oluşturabilir ve dağıtabilirsiniz.

Bir Azure Blockchain Hizmeti konsorsiyumüyesine erişiminiz yoksa, ön koşulu tamamlayan [Quickstart: Azure portalını](create-member.md) veya Quickstart'ı kullanarak bir blockchain üyesi [oluşturun: Azure CLI'yi kullanarak bir Azure Blockchain Hizmeti blockchain üyesi oluşturun.](create-member-cli.md)

1. VS Code gezgini bölmesinde **Azure Blockchain** uzantısını genişletin.
1. **Ağa Bağlan'ı**seçin.

   ![Ağa bağlanma](./media/connect-vscode/connect-consortium.png)

    Azure kimlik doğrulaması için istenirse, bir tarayıcı kullanarak kimlik doğrulaması için istemleri izleyin.
1. Komut paleti açılır açılır ayında **Azure Blockchain Hizmeti'ni** seçin.
1. Azure Blockchain Hizmeti konsorsiyumu üyenizle ilişkili abonelik ve kaynak grubunu seçin.
1. Listeden konsorsiyumunuzu seçin.

Konsorsiyum ve blockchain üyeleri VS Code explorer yan çubuğunda listelenir.

![Konsorsiyum explorer'da görüntülendi](./media/connect-vscode/consortium-node.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Blockchain Hizmeti'ndeki bir konsorsiyuma bağlanmak için Ethereum VS Code uzantısı için Azure Blockchain Geliştirme Kiti'ni kullandınız. Bir işlem aracılığıyla akıllı bir sözleşme işlevi oluşturmak, oluşturmak, dağıtmak ve yürütmek için Ethereum için Azure Blockchain Geliştirme Kiti'ni kullanmak için bir sonraki öğreticiyi deneyin.

> [!div class="nextstepaction"]
> [Azure Blockchain Hizmeti'nde akıllı sözleşmeler oluşturma, oluşturma ve dağıtma](send-transaction.md)