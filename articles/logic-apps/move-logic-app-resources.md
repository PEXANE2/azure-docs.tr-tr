---
title: Mantık uygulamalarını abonelikler, kaynak grupları veya bölgeler arasında geçirme
description: Mantık uygulamalarını veya tümleştirme hesaplarını diğer Azure aboneliklerine, kaynak gruplarına veya konumlara (bölgelere) geçirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 07/31/2019
ms.openlocfilehash: f5944accb185f1311c811cf65a8ea8348fd569db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77605616"
---
# <a name="move-logic-app-resources-to-other-azure-subscriptions-resource-groups-or-regions"></a>Mantık uygulama kaynaklarını diğer Azure aboneliklerine, kaynak gruplarına veya bölgelere taşıma

Mantık uygulamanızı veya ilgili kaynaklarınızı başka bir Azure aboneliğine, kaynak grubuna veya bölgeye geçirmek için Azure portalı, Azure PowerShell, Azure CLI ve REST API gibi bu görevleri tamamlamak için çeşitli yollarınız vardır. Kaynakları taşımadan önce şu hususları gözden geçirin: 

* Azure kaynak grupları veya abonelikler arasında yalnızca [belirli mantık uygulaması kaynak türlerini](../azure-resource-manager/management/move-support-resources.md#microsoftlogic) taşıyabilirsiniz.

* Azure aboneliğinizde ve her Azure bölgesinde sahip olabileceğiniz mantık uygulaması kaynaklarının sayısının [sınırlarını](../logic-apps/logic-apps-limits-and-config.md) kontrol edin. Bu sınırlar, bölge abonelikler veya kaynak grupları arasında aynı kaldığında belirli kaynak türlerini taşıyıp taşıyamayacağınızı etkiler. Örneğin, her Azure aboneliğinde her Azure bölgesi için yalnızca bir Serbest katman tümleştirme hesabınız olabilir.

* Mantık uygulamalarını abonelikler, kaynak grupları veya bölgeler arasında geçiş yaptıktan sonra, Açık Kimlik Doğrulaması (OAuth) gerektiren bağlantıları yeniden oluşturmanız veya yeniden yetkilendirmeniz gerekir.

* Kaynakları her taşıdığınızda, Azure yeni kaynak tbm'leri oluşturur. Bu nedenle, bunun yerine yeni t.c.'leri kullandığınızdan ve taşınan kaynaklarla ilişkili komut dosyalarını veya araçları güncelleştirdiğinizden emin olun.

## <a name="prerequisites"></a>Ön koşullar

* Taşımak istediğiniz mantık uygulaması veya tümleştirme hesabını oluşturmak için kullanılan aynı Azure aboneliği

* Kaynak sahibi, istediğiniz kaynakları taşıma ve ayarlama izni verir. [Rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/built-in-roles.md#owner)hakkında daha fazla bilgi edinin.

<a name="move-subscription"></a>

## <a name="move-resources-between-subscriptions"></a>Kaynakları abonelikler arasında taşıma

Mantık uygulaması veya tümleştirme hesabı gibi bir kaynağı başka bir Azure aboneliğine taşımak için Azure portalını, Azure PowerShell'i, Azure CLI'yi veya REST API'yi kullanabilirsiniz. Bu adımlar, kaynağın bölgesi aynı kaldığında kullanabileceğiniz Azure portalını kapsar. Diğer adımlar ve genel hazırlık için [bkz.](../azure-resource-manager/management/move-resource-group-and-subscription.md)

1. Azure [portalında,](https://portal.azure.com)taşımak istediğiniz mantık uygulaması kaynağını bulun ve seçin.

1. Kaynağın Genel **Bakış** sayfasında, **Abonelik'in**yanında, **değiştir** bağlantısını seçin.

1. Kaynakları **Taşı** sayfasında, mantık uygulaması kaynağını ve taşımak istediğiniz ilgili kaynakları seçin.

1. **Abonelik** listesinden hedef aboneliği seçin.

1. Kaynak **grup** listesinden hedef kaynak grubunu seçin. Veya farklı bir kaynak grubu oluşturmak için **yeni bir grup oluştur'u**seçin.

1. Taşınan kaynaklarla ilişkili tüm komut dosyalarının veya araçlarının, siz bunları yeni kaynak ekleriyle güncelleştirene kadar çalışmayacağını anlamak için onay kutusunu seçin ve **ardından Tamam'ı**seçin.

<a name="move-resource-group"></a>

## <a name="move-resources-between-resource-groups"></a>Kaynakları kaynak grupları arasında taşıma

Mantık uygulaması veya tümleştirme hesabı gibi bir kaynağı başka bir Azure kaynak grubuna taşımak için Azure portalını, Azure PowerShell'i, Azure CLI'yi veya REST API'yi kullanabilirsiniz. Bu adımlar, kaynağın bölgesi aynı kaldığında kullanabileceğiniz Azure portalını kapsar. Diğer adımlar ve genel hazırlık için [bkz.](../azure-resource-manager/management/move-resource-group-and-subscription.md)

Kaynakları gruplar arasında gerçekten taşımadan önce, kaynağınızı başka bir gruba başarıyla taşıyıp taşıyamayacağınızı test edebilirsiniz. Daha fazla bilgi için [bkz.](../azure-resource-manager/management/move-resource-group-and-subscription.md#validate-move)

1. Azure [portalında,](https://portal.azure.com)taşımak istediğiniz mantık uygulaması kaynağını bulun ve seçin.

1. Kaynağın Genel **Bakış** sayfasında, **Kaynak grubunun**yanında, **değiştir** bağlantısını seçin.

1. Kaynakları **Taşı** sayfasında, mantık uygulaması kaynağını ve taşımak istediğiniz ilgili kaynakları seçin.

1. Kaynak **grup** listesinden hedef kaynak grubunu seçin. Veya farklı bir kaynak grubu oluşturmak için **yeni bir grup oluştur'u**seçin.

1. Taşınan kaynaklarla ilişkili tüm komut dosyalarının veya araçlarının, siz bunları yeni kaynak ekleriyle güncelleştirene kadar çalışmayacağını anlamak için onay kutusunu seçin ve **ardından Tamam'ı**seçin.

<a name="move-location"></a>

## <a name="move-resources-between-regions"></a>Kaynakları bölgeler arasında taşıma

Bir mantık uygulamasını farklı bir bölgeye taşımak istediğinizde, seçenekleriniz mantık uygulamanızı oluşturma şeklinize bağlıdır. Seçtiğiniz seçenebağlı olarak, mantık uygulamanızdaki bağlantıları yeniden oluşturmanız veya yeniden yetkilendirmeniz gerekir.

* Azure portalında, yeni bölgedeki mantık uygulamasını yeniden oluşturun ve iş akışı ayarlarını yeniden yapılandırın. Zamandan tasarruf etmek için, temel iş akışı tanımını ve bağlantıları kaynak uygulamadan hedef uygulamaya kopyalayabilirsiniz. Mantık Uygulaması nın arkasındaki "kodu" Logic App Designer araç çubuğunda görüntülemek için **Kod görünümü'nü**seçin.

* Visual Studio ve Visual Studio için Azure Logic Apps Araçlarını [kullanarak, mantıksal uygulamanızı](../logic-apps/manage-logic-apps-with-visual-studio.md) Azure portalından [Azure Kaynak Yöneticisi şablonu](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)olarak açıp indirebilirsiniz. Bu şablon çoğunlukla dağıtıma hazırdır ve iş akışının kendisi ve bağlantıları da dahil olmak üzere mantık uygulamanızın kaynak tanımlarını içerir. Şablon ayrıca dağıtımsırasında kullanılacak değerler için parametreleri de bildirir. Bu şekilde, mantık uygulamasını gereksinimlerinize göre nerede ve nasıl dağıtacağınızı daha kolay değiştirebilirsiniz. Dağıtım için konumu ve diğer gerekli bilgileri belirtmek için ayrı bir parametre dosyası kullanabilirsiniz.

* Azure DevOps'teki Azure Ardışık Hatları gibi sürekli tümleştirme (CI) ve sürekli teslim (CD) araçlarını kullanarak mantık uygulamanızı oluşturduysanız ve dağıttıysanız, bu araçları kullanarak uygulamanızı başka bir bölgeye dağıtabilirsiniz.

Mantık uygulamaları için dağıtım şablonları hakkında daha fazla bilgi için şu konulara bakın:

* [Genel Bakış: Azure Kaynak Yöneticisi şablonlarını kullanarak Azure Mantık Uygulamaları için dağıtımı otomatikleştirin](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)
* [Mantıksal uygulamanızı Azure portalından Visual Studio'da bulun, açın ve indirin](../logic-apps/manage-logic-apps-with-visual-studio.md)
* [Azure Mantık Uygulamaları için Azure Kaynak Yöneticisi şablonları oluşturma](../logic-apps/logic-apps-create-azure-resource-manager-templates.md)
* [Azure Mantık Uygulamaları için Azure Kaynak Yöneticisi şablonlarını dağıtma](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md)

### <a name="related-resources"></a>İlgili kaynaklar

Azure'daki şirket içi veri ağ geçidi kaynakları gibi bazı Azure kaynakları, bu kaynakları kullanan mantıksal uygulamalardan farklı bir bölgede bulunabilir. Ancak, bağlantılı tümleştirme hesapları gibi diğer Azure kaynaklarının mantık uygulamalarınızla aynı bölgede bulunması gerekir. Senaryonuza bağlı olarak, mantık uygulamalarınızın uygulamalarınızın aynı bölgede bulunmasını beklediği kaynaklara erişebilmesini unutmayın.

Örneğin, bir mantık uygulamasını bir tümleştirme hesabına bağlamak için her iki kaynağın da aynı bölgede bulunması gerekir. Olağanüstü durum kurtarma gibi senaryolarda, genellikle aynı yapılandırmaya ve yapıya sahip tümleştirme hesapları istersiniz. Diğer senaryolarda, farklı yapılandırmaları ve yapıları ile tümleştirme hesapları gerekebilir.

Azure Logic Apps'taki özel bağlayıcılar, aynı Azure aboneliğine ve aynı Azure Etkin Dizin kiracısına sahip olan bağlayıcıların yazarları ve kullanıcıları tarafından görülebilir. Bu bağlayıcılar, mantık uygulamalarının dağıtıldığı aynı bölgede kullanılabilir. Daha fazla bilgi için bkz. [Kuruluşunuzda özel bağlayıcıları paylaşma](https://docs.microsoft.com/connectors/custom-connectors/share).

Visual Studio'dan aldığınız şablon, yalnızca mantık uygulamanız ve bağlantıları için kaynak tanımlarını içerir. Bu nedenle, mantık uygulamanız bir tümleştirme hesabı ve ortaklar, anlaşmalar ve şemalar gibi diğer kaynakları kullanıyorsa, Azure portalını kullanarak bu tümleştirme hesabının şablonunu dışa aktarmanız gerekir. Bu şablon, hem tümleştirme hesabı hem de yapılar için kaynak tanımlarını içerir. Ancak, şablon tam olarak parametreli değil. Bu nedenle, dağıtım için kullanmak istediğiniz değerleri el ile parametrelemeniz gerekir.

### <a name="export-templates-for-integration-accounts"></a>Tümleştirme hesapları için dışa aktarma şablonları

1. Azure [portalında,](https://portal.azure.com)entegrasyon hesabınızı bulun ve açın.

1. Entegrasyon hesabınızın menüsünde Ayarlar **altında** **Dışa Aktar şablonunu**seçin.

1. Araç çubuğunda **İndir'i**seçin ve şablonu kaydedin.

1. Dağıtım için gerekli değerleri parametrelendirmek için şablonu açın ve edin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure kaynaklarını yeni kaynak gruplarına veya aboneliklere taşıma](../azure-resource-manager/management/move-resource-group-and-subscription.md)
