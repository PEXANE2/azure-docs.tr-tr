---
title: Azure veri bilimi sanal makinelerini kullanma
description: Azure Notebooks önizlemesi için kullanılabilir işlem gücünü genişletmek üzere bir Azure Veri Bilimi Sanal Makinesi (DSVM) ile nasıl bağlanacağınızı öğrenin.
author: getroyer
manager: andneil
ms.author: getroyer
ms.topic: how-to
ms.date: 06/13/2019
ms.openlocfilehash: b4da63b7b2a6da4316215b85a09ca7420745251c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78898411"
---
# <a name="use-azure-data-science-virtual-machines"></a>Azure veri bilimi sanal makinelerini kullanma

Varsayılan olarak, projeler **ücretsiz işlem** katmanında çalışır, bu da kötüye kullanımı engellemek IÇIN 4 GB bellek ve 1 GB veri ile sınırlıdır. Bu sınırlamaları, bir Azure aboneliğinde sağladığınız farklı bir sanal makineyi kullanarak atlayabilirsiniz. Bu amaçla en iyi seçenek, **Linux için veri bilimi sanal makinesi (Ubuntu)** görüntüsünü kullanan bir Azure veri bilimi sanal makinesi (dsvm) ' dir. Bu tür bir DSVM, Azure Notebooks için ihtiyaç duyduğunuz her şey ile önceden yapılandırılmış olarak gelir ve Azure Notebooks **açılan listesinde** otomatik olarak görünür.

> [!Note]
> Azure Notebooks yalnızca Linux Ubuntu görüntüsünde oluşturulan DSVMs üzerinde desteklenir. Windows 2012, Windows 2016 veya Linux CentOS görüntülerinde Not defterleri desteklenmez.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="create-a-dsvm-instance"></a>DSVM örneği oluşturma

Yeni bir DSVM örneği oluşturmak için [Ubuntu veri bilimi VM'si oluşturma](/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)yönergelerini izleyin. Fiyatlandırma ayrıntıları dahil daha fazla bilgi için bkz. [veri bilimi sanal makineleri](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="connect-to-the-dsvm"></a>DSVM 'ye bağlanma

DSVM 'yi oluşturduktan sonra, Azure Notebooks projesi panosunda **Çalıştır** açılan listesini seçin ve uygun dsvm örneğini seçin. Aşağıdaki koşullar doğruysa, açılan listede DSVM örnekleri gösterilmektedir:

- Şirket hesabı gibi Azure Active Directory (AAD) kullanan bir hesapla Azure Notebooks oturum açtınız.
- Hesabınız bir Azure aboneliğine bağlı.
- Bu abonelikte, Linux (Ubuntu) görüntüsü için Veri Bilimi Sanal Makinesi kullanan en az okuyucu erişimi olan bir veya daha fazla sanal makineniz vardır.)

![Proje panosundaki açılan listede örnekleri Veri Bilimi Sanal Makinesi](media/project-compute-tier-dsvm.png)

Bir DSVM örneği seçtiğinizde Azure Notebooks, VM 'yi oluştururken kullanılan belirli makine kimlik bilgilerini isteyip istemeyebilir.

> [!Important]
> Kullanıcı adının jupi Terhub ile kullanılması için küçük harfli olması gerekir.

Koşulların herhangi birine uyulmazsa DSVM 'ye bağlanabilirsiniz. Aşağı açılan listede, bir ad (listede göstermek için), sanal makinenin IP adresi ve bağlantı noktası (genellikle 8000, JupyterHub 'ın dinlediği varsayılan bağlantı noktası) ve VM kimlik bilgilerini isteyen **doğrudan işlem** seçeneğini belirleyin:

![Doğrudan Işlem seçeneği için sunucu bilgilerini toplamayı isteme](media/project-compute-tier-direct.png)

Bu değerleri Azure portal DSVM sayfasından elde edersiniz.

## <a name="accessing-azure-notebooks-files-from-the-dsvm"></a>DSVM 'deki Azure Notebooks dosyalarına erişme

Dosya sistemi erişimi, DSVM sürümleri 19.06.15 veya üzeri için desteklenir. Sürümü denetlemek için öncelikle SSH aracılığıyla DSVM 'nize bağlanın, ardından şu komutu çalıştırın: `curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2018-10-01"` (burada GÖSTERILEN tam IP adresini kullanmanız gerekir). Sürüm numarası, "sürüm" için çıktıda gösterilir.

Dosya yollarının, **ücretsiz işlem** katmanıyla birlikte kullanımını korumak için, BIR anda dsvm 'de yalnızca bir proje açabilirsiniz. Yeni bir proje açmak için önce açık projeyi kapatmanız gerekir.

Bir proje bir VM üzerinde çalıştırıldığında, dosyalar jupi sunucusunun kök dizinine (jupde gösterilen dizin), varsayılan Azure Notebooks dosyalarını değiştirerek bağlanır. Not defteri Kullanıcı arabirimindeki **kapatma** düğmesini kullanarak VM 'yi kapattığınızda, Azure Notebooks varsayılan dosyaları geri yükler.

![Azure Notebooks 'da düğme kapatılıyor](media/shutdown.png)

## <a name="create-new-dsvm-users"></a>Yeni DSVM kullanıcıları oluştur

Birden çok kullanıcı bir DSVM 'yi paylaşıyorsa, her bir not defteri kullanıcısı için bir DSVM kullanıcısı oluşturup kullanarak birbirini engellemeyi önleyebilirsiniz:

1. [Azure Portal](https://portal.azure.com)sanal makinenize gidin.
1. Sol kenar boşluğunda **destek + sorun giderme** ' nin altında, **Parolayı Sıfırla**' yı seçin.
1. Yeni bir **Kullanıcı adı**girin. Kullanıcı adının jupi Terhub ile kullanılması için küçük harfli olması gerekir. Bir parola girin. Ardından **Güncelleştir**' i seçin. (Mevcut kullanıcı adları etkilenmez.)
1. Ek kullanıcılar için önceki adımı yineleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure veri bilimi sanal makinelerine giriş](/azure/machine-learning/data-science-virtual-machine/overview)hakkında daha fazla bilgi edinin.
