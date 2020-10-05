---
title: Azure Resource Manager kullanarak Service Fabric yönetilen küme dağıtma (Önizleme)
description: Azure Resource Manager şablonuyla Service Fabric yönetilen bir küme oluşturmayı öğrenin
ms.topic: quickstart
ms.date: 09/28/2020
ms.custom: references_regions
ms.openlocfilehash: 079e8e3e39bd306332abb43fa2ae1f8cc04e305b
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91410516"
---
# <a name="quickstart-deploy-a-service-fabric-managed-cluster-preview-with-an-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Resource Manager şablonuyla Service Fabric yönetilen bir küme (Önizleme) dağıtma

Service Fabric yönetilen kümeler, dağıtım ve küme yönetimi deneyiminizi kolaylaştıran Azure Service Fabric kümesi kaynak modelinin bir gelişmidir. Service Fabric yönetilen kümeler, bir Service Fabric kümesi oluşturan temel kaynakların tümünü dağıtmak yerine tek bir Service Fabric küme kaynağı dağıtmanızı sağlayan tamamen kapsüllenmiş bir kaynaktır. Bu makalede, Azure 'da bir Azure Resource Manager şablonu (ARM şablonu) kullanarak test için Service Fabric yönetilen bir kümenin nasıl dağıtılacağı açıklanır.

Bu öğreticide dağıtılan üç düğümlü temel SKU kümesi yalnızca, eğitim amaçlarıyla kullanılmak üzere tasarlanmıştır (üretim iş yükleri yerine). Daha fazla bilgi için bkz.  [yönetilen küme SKU 'ları Service Fabric](overview-managed-cluster.md#service-fabric-managed-cluster-skus).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıca başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure örnekleri-Service Fabric küme şablonlarından](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/SF-Managed-Basic-SKU-1-NT).

## <a name="create-a-client-certificate"></a>İstemci sertifikası oluşturma

Service Fabric yönetilen kümeler, erişim denetimi için bir anahtar olarak bir istemci sertifikası kullanır. Kümenize erişim denetimi için kullanmak istediğiniz bir istemci sertifikanız zaten varsa, bu adımı atlayabilirsiniz.

Yeni bir istemci sertifikası oluşturmanız gerekiyorsa, [Azure Key Vault bir sertifikayı ayarlama ve alma](../key-vault/certificates/quick-create-portal.md)bölümündeki adımları uygulayın.

Bir sonraki adımda şablonu dağıtmak için gerekli olacağı için sertifika parmak izini göz önünde atın.

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın.

      [![Azure’a dağıtın](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fservice-fabric-cluster-templates%2Fmaster%2FSF-Managed-Basic-SKU-1-NT%2Fazuredeploy.json)

2. Aşağıdaki değerleri seçin veya girin

    Bu hızlı başlangıç için aşağıdaki şablon parametreleri için kendi değerlerinizi sağlayın:

    * **Abonelik**: bir Azure aboneliği seçin.
    * **Kaynak grubu**: **Yeni oluştur**' u seçin. Kaynak grubu için *Myresourcegroup*gibi benzersiz bir ad girin ve ardından **Tamam**' ı seçin.
    * **Konum**: **eastus2**gibi bir konum seçin. Service Fabric yönetilen kümeler önizlemesi için desteklenen bölgeler,,,, `centraluseuap` `eastus2euap` ve içerir `eastasia` `northeurope` `westcentralus` `eastus2` .
    * **Küme adı**: kümeniz için, *hayal fcluster*gibi benzersiz bir ad girin.
    * **Yönetici Kullanıcı adı**: kümedeki temeldeki VM 'lerde RDP için kullanılacak bir ad girin.
    * **Yönetici parolası**: yöneticinin, kümedeki temel alınan VM 'lerde RDP için kullanılacak bir parola girin.
    * **Istemci sertifikası parmak izi**: kümenize erişmek için kullanmak istediğiniz istemci sertifikasının parmak izini sağlayın. Bir sertifikanız yoksa, otomatik olarak imzalanan sertifika oluşturmak için [set ve bir sertifika al](../key-vault/certificates/quick-create-portal.md) ' ı izleyin.
    * **Düğüm türü adı**: düğüm türü için *NT1*gibi benzersiz bir ad girin.
    * **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**: kabul etmek için bu kutuyu işaretleyin. 

3. **Satın al**'ı seçin.

4. Yönetilen Service Fabric kümenizin dağıtılması birkaç dakika sürer. Sonraki adımlara geçmeden önce dağıtımın başarıyla tamamlanmasını bekleyin.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

### <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Dağıtım tamamlandıktan sonra, çıkışta Service Fabric Explorer değerini bulun ve Service Fabric Explorer kümenizi görüntülemek için adresi bir Web tarayıcısında açın. Bir sertifika istendiğinde, şablonda istemci parmak izinin sağlandığı sertifikayı kullanın.

> [!NOTE]
> Dağıtım çıktısını Azure portalında kaynak grubu dağıtımları sekmesinde bulabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, Service Fabric yönetilen kümeniz için kaynak grubunu silin. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki *arama* kutusuna kaynak grubunuzun adını girin. Bu hızlı başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yönetilen bir Service Fabric kümesi dağıttınız. Bir kümenin ölçeklendirilmesi hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Service Fabric yönetilen bir kümeyi genişletme](tutorial-managed-cluster-scale.md)
