---
title: Azure Marketi için sanal makine teklifi için teknik varlıklar oluşturun
description: Azure Marketi'ndeki sanal makine teklifi için teknik varlıkların nasıl oluşturulabildiğini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: dsindona
ms.openlocfilehash: 57f56a341cfc3db6a5f0664503809e6ab6cf3d3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278033"
---
# <a name="create-technical-assets-for-a-virtual-machine-offer"></a>Sanal makine teklifi için teknik varlıklar oluşturma

Bu bölüm, Azure Marketi için sanal bir makine (VM) teklifi için teknik varlıkları oluşturma ve yapılandırma konusunda size yol göstermektedir.  VM iki bileşen içerir: çözüm sanal sabit disk (VHD) ve isteğe bağlı ilişkili veri diskleri.  

- Azure Marketi teklifinizle dağıtacağınız işletim sistemini ve çözümünüzü içeren *sanal sabit diskler (VHD'ler).* VHD'yi hazırlama işlemi, Linux tabanlı, Windows tabanlı veya özel tabanlı bir VM olmasına bağlı olarak değişir.
- *Veri diskleri,* sanal bir makine için özel, kalıcı depolamayı temsil eder. Kalıcı bilgileri depolamak için VHD `C:` çözümlerini (örneğin, sürücü) *kullanmayın.*

VM görüntüsü, bir işletim sistemi diski ve sıfır veya daha fazla veri diski içerir. Disk başına bir VHD gereklidir. Boş veri disklerinin bile oluşturulması için bir VHD gerekir.
VM işletim sistemi, VM boyutu, açılacak bağlantı noktaları ve en fazla 15 bağlı veri diski yapılandırmanız gerekir.

> [!TIP] 
> Hangi işletim sistemini kullandığınızdan bağımsız olarak, SKU'nun ihtiyaç duyduğu en az sayıda veri diski ekleyin. Müşteriler dağıtım sırasında görüntünün parçası olan diskleri kaldıramaz, ancak dağıtım sırasında veya sonrasında her zaman disk ekleyebilirler. 

> [!IMPORTANT]
> *Yeni bir resim sürümünde disk sayısını değiştirmeyin.* Görüntüdeki Veri disklerini yeniden yapılandırmanız gerekiyorsa, yeni bir SKU tanımlayın. Farklı disk sayılarıyla yeni bir resim sürümünü yayımlamak, otomatik ölçekleme durumlarında yeni resim sürümüne dayalı olarak yeni dağıtımı kırma, Azure Kaynak Yöneticisi şablonları ve diğer senaryolar aracılığıyla çözümlerin otomatik dağıtımları potansiyeline sahip olacaktır.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="fundamental-technical-knowledge"></a>Temel teknik bilgi

Bu varlıkların tasarlanması, oluşturulması ve test edilmesi zaman alır ve hem Azure platformu hem de teklifi oluşturmak için kullanılan teknolojiler hakkında teknik bilgi gerektirir. Çözüm etki alanınıza ek olarak, mühendislik ekibiniz aşağıdaki Microsoft teknolojileri hakkında bilgi sahibi olmalıdır: 
-   [Azure Hizmetlerinin](https://azure.microsoft.com/services/) temel anlayışı 
-   Azure uygulamaları nasıl [tasarlar ve tasarlar](https://azure.microsoft.com/solutions/architecture/)
-   [Azure Sanal Makineler,](https://azure.microsoft.com/services/virtual-machines/) [Azure Depolama](https://azure.microsoft.com/services/?filter=storage) ve [Azure Ağı](https://azure.microsoft.com/services/?filter=networking) çalışma bilgisi
-   [Azure Kaynak Yöneticisi'nin](https://azure.microsoft.com/features/resource-manager/) çalışma bilgisi
-   [JSON](https://www.json.org/) Çalışma Bilgisi


## <a name="suggested-tools"></a>Önerilen araçlar 

VHD'leri ve VM'leri yönetmenize yardımcı olmak için aşağıdaki komut dosyası ortamlarından birini veya her ikisini seçin:
-   [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
-   [Azure CLI](https://docs.microsoft.com/cli/azure)

Ayrıca, geliştirme ortamınıza aşağıdaki araçları eklemenizi öneririz: 

-   [Azure Storage Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
-   [Visual Studio Code](https://code.visualstudio.com/)
    *   Uzantı: [Azure Kaynak Yöneticisi Araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
    *   Uzantısı: [Güzelleştirmek](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
    *   Uzatma: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

[Ayrıca Azure Geliştirici Araçları](https://azure.microsoft.com/tools/) sayfasında ve Visual Studio kullanıyorsanız, Visual Studio [Marketplace'te](https://marketplace.visualstudio.com/)mevcut araçları incelemenizi öneririz.


## <a name="next-steps"></a>Sonraki adımlar

Bu bölümdeki sonraki makaleler, bu VM varlıklarını oluşturma ve kaydetme adımlarında size yol verebolur:

1. [Azure uyumlu bir sanal sabit disk oluşturma,](./cpp-create-vhd.md) Azure ile uyumlu bir Linux veya Windows tabanlı VHD'nin nasıl oluşturulabildiğini açıklar.  Boyutlandırma, yama ve yükleme için VM hazırlama gibi en iyi uygulamaları içerir.

2. [Sanal makineye bağlanmak,](./cpp-connect-vm.md) yeni oluşturduğunuz VM'nize uzaktan nasıl bağlanıp oturum açabilirsiniz açıklar.  Bu makalede, kullanım maliyetlerinden tasarruf etmek için VM'nin nasıl durdurululması da açıklanmaktadır.

3. [Sanal makineyi yapılandırmak,](./cpp-configure-vm.md) doğru VHD boyutunu nasıl seçeceğinizi, resminizi genelleştireceğinizi, son güncelleştirmeleri (düzeltme emültleri) nasıl uygulayacağınızı ve özel yapılandırmaları zamanlamayı açıklar.

4. [Sanal sabit diskten sanal bir makine dağıtma,](./cpp-deploy-vm-vhd.md) Azure'da dağıtılan bir VHD'den vm'nin nasıl kaydedildiğini açıklar.  Gerekli araçları listeler ve bunları bir kullanıcı VM görüntüsü oluşturmak için nasıl kullanılacağını listeler ve ardından [Microsoft Azure portalı](https://ms.portal.azure.com/) veya PowerShell komut dosyalarını kullanarak Azure'a dağıtır. 

5. [Sanal makine görüntüsünü onaylamak,](./cpp-certify-vm.md) Azure Marketi sertifikası için vm görüntüsünün nasıl test edilip gönderilebildiğini açıklar. Azure Sertifikalı araç *için Sertifika Test Aracı'nı* nereden alacağınızı ve VM resminizi onaylamak için bu aracı nasıl kullanacağınızı açıklar. 

6. [SAS URI'nin](./cpp-get-sas-uri.md) VM görüntünüz (ler) için paylaşılan erişim imzasını (SAS) URI'yi nasıl alacağınızı açıkla.
 
Destekleyici bir makale olarak, [Ortak paylaşılan erişim imzası URL sorunları,](./cpp-common-sas-url-issues.md) SAS URI'lerini ve buna karşılık gelen olası çözümleri kullanarak karşılaşabileceğiniz bazı yaygın sorunları listeler.

Tüm bu adımları tamamladıktan [sonra, VM teklifinizi](./cpp-publish-offer.md) Azure Marketi'nde yayınlamaya hazır olursunuz.
