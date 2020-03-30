---
title: Blockchain uygulaması sürümü - Azure Blockchain Workbench
description: Azure Blockchain Workbench Preview'da uygulama sürümleri nasıl kullanılır?
ms.date: 11/20/2019
ms.topic: article
ms.reviewer: brendal
ms.openlocfilehash: 45219790cf0cd064e0fcd456e262b2f93aa03ac9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74323904"
---
# <a name="azure-blockchain-workbench-preview-application-versioning"></a>Azure Blockchain Çalışma Tezgahı Önizleme uygulama sürümü

Bir Azure Blockchain Workbench Preview uygulamasının birden çok sürümü oluşturabilir ve kullanabilirsiniz. Aynı uygulamanın birden çok sürümü yüklenirse, bir sürüm geçmişi kullanılabilir ve kullanıcılar hangi sürümü kullanmak istediklerini seçebilir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Blockchain Çalışma Tezgahı dağıtımı. Daha fazla bilgi için, dağıtımla ilgili ayrıntılar için [Azure Blockchain Workbench dağıtımına](deploy.md) bakın
* Blockchain Workbench'te dağıtılan blockchain uygulaması. Bkz. [Azure Blockchain Workbench'te blockchain uygulaması oluşturma](create-app.md)

## <a name="add-an-app-version"></a>Uygulama sürümü ekleme

Yeni bir sürüm eklemek için blockchain Workbench'e yeni yapılandırma ve akıllı sözleşme dosyalarını yükleyin.

1. Bir web tarayıcısında Blockchain Workbench web adresine gidin. Örneğin, `https://{workbench URL}.azurewebsites.net/` Blockchain Workbench web adresinizi nasıl bulacağınız hakkında bilgi için [Blockchain Workbench Web URL'sine](deploy.md#blockchain-workbench-web-url) bakın
2. [Blockchain Workbench yöneticisi](manage-users.md#manage-blockchain-workbench-administrators)olarak oturum açın.
3. Güncelleştirmek istediğiniz blockchain uygulamasını başka bir sürümle seçin.
4. **Sürüm Ekle'yi**seçin. **Sürüm Ekle** bölmesi görüntülenir.
5. Yüklemek için yeni sürüm sözleşme yapılandırmasını ve sözleşme kodu dosyalarını seçin. Yapılandırma dosyası otomatik olarak doğrulanır. Uygulamayı dağıtmadan önce tüm doğrulama hatalarını düzeltin.
6. Yeni blockchain uygulama sürümünü eklemek için **Sürüm Ekle'yi** seçin.

    ![Yeni bir sürüm ekleme](media/version-app/add-version.png)

Blockchain uygulamasının dağıtımı birkaç dakika sürebilir. Dağıtım tamamlandığında, uygulama sayfasını yenileyin. Uygulamayı seçmek ve Sürüm **geçmişi** düğmesini seçmek, uygulamanın sürüm geçmişini görüntüler.

> [!IMPORTANT]
> Uygulamanın önceki sürümleri devre dışı bırakılır. Geçmiş sürümleri tek tek yeniden etkinleştirebilirsiniz.
>
> Yeni sürümdeki uygulama rollerinde değişiklik yapıldıysa, üyeleri uygulama rollerine yeniden eklemeniz gerekebilir.

## <a name="using-app-versions"></a>Uygulama sürümlerini kullanma

Varsayılan olarak, uygulamanın en son etkin sürümü Blockchain Workbench kullanılır. Bir uygulamanın önceki sürümünü kullanmak istiyorsanız, önce uygulama sayfasından sürümü seçmeniz gerekir.

1. Blockchain Çalışma Tezgahı uygulama bölümünde, kullanmak istediğiniz sözleşmeyi içeren uygulama onay kutusunu seçin. Önceki sürümler etkinleştirilmişse, sürüm geçmişi düğmesi kullanılabilir.
2. Sürüm **geçmişi** düğmesini seçin.
3. Sürüm geçmişi bölmesinde, *Tarih değiştirilen* sütundaki bağlantıyı seçerek uygulamanın sürümünü seçin.

    ![Önceki sürümü seçme](media/version-app/use-version.png)

    Yeni sözleşmeler oluşturabilir veya önceki sürüm sözleşmelerinde işlem yapabilirsiniz. Uygulamanın sürümü uygulama adını takip ederek görüntülenir ve eski sürüm hakkında bir uyarı görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Blockchain Workbench sorun giderme](troubleshooting.md)
