---
title: Azure Blockchain Workbench'teki uygulamaları kullanma
description: Azure Blockchain Workbench Preview'da uygulama sözleşmelerinin nasıl kullanılacağı hakkında öğretici.
ms.date: 10/14/2019
ms.topic: tutorial
ms.reviewer: brendal
ms.openlocfilehash: 5761bf1294691c2d50e0e389fe69ec286df4a06c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74324053"
---
# <a name="tutorial-using-applications-in-azure-blockchain-workbench"></a>Öğretici: Azure Blockchain Workbench'teki uygulamaları kullanma

Blockchain Workbench'i sözleşmeler oluşturmak ve eylemlerde bulunmak için kullanabilirsiniz. Durum ve işlem geçmişi gibi sözleşme ayrıntılarını da görüntüleyebilirsiniz.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Yeni bir sözleşme oluşturma
> * Sözleşme yle ilgili işlem

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Blockchain Çalışma Tezgahı dağıtımı. Daha fazla bilgi için, dağıtımla ilgili ayrıntılar için [Azure Blockchain Workbench dağıtımına](deploy.md) bakın
* Blockchain Workbench'te dağıtılan blockchain uygulaması. Bkz. [Azure Blockchain Workbench'te blockchain uygulaması oluşturma](create-app.md)

Tarayıcınızda [Blockchain Çalışma Tezgahı'nı açın.](deploy.md#blockchain-workbench-web-url)

![Blockchain Workbench](./media/use/workbench.png)

Blockchain Çalışma Tezgahı'nın bir üyesi olarak oturum açmanız gerekiyor. Listelenen herhangi bir uygulama yoksa, Blockchain Workbench üyesi siniz, ancak herhangi bir uygulamanın üyesi değil. Blockchain Workbench yöneticisi uygulamalara üye atayabilir.

## <a name="create-new-contract"></a>Yeni sözleşme oluşturma

Yeni bir sözleşme oluşturmak için, sözleşme **başlatıcısı**olarak belirtilen bir üye olmanız gerekir. Sözleşmenin uygulama rollerini ve başlatıcılarını tanımlayan bilgiler için [yapılandırmaya genel bakıştaki iş akışlarına](configuration.md#workflows)bakın. Uygulama rollerine üye atama hakkında bilgi için, [uygulamaya üye ekleme'ye](manage-users.md#add-member-to-application)bakın.

1. Blockchain Çalışma Tezgahı uygulama bölümünde, oluşturmak istediğiniz sözleşmeyi içeren uygulama döşemesini seçin. Etkin sözleşmelerin listesi görüntülenir.

2. Yeni bir sözleşme oluşturmak için **Yeni sözleşme'yi**seçin.

    ![Yeni sözleşme düğmesi](./media/use/contract-list.png)

3. **Yeni sözleşme** bölmesi görüntülenir. İlk parametre değerlerini belirtin. **Oluştur'u**seçin.

    ![Yeni sözleşme bölmesi](./media/use/new-contract.png)

    Yeni oluşturulan sözleşme, diğer etkin sözleşmeler ile listede görüntülenir.

    ![Etkin sözleşmeler listesi](./media/use/active-contracts.png)

## <a name="take-action-on-contract"></a>Sözleşme yle ilgili işlem

Sözleşmenin içinde olduğu duruma bağlı olarak, üyeler sözleşmenin bir sonraki durumuna geçiş için işlem yapabilir. Eylemler, [bir durum](configuration.md#states)içindeki [geçişler](configuration.md#transitions) olarak tanımlanır. Geçiş için izin verilen bir uygulama veya örnek rolüne ait üyeler harekete geçebilir. 

1. Blockchain Çalışma Tezgahı uygulama bölümünde, eylemi yapmak için sözleşmeyi içeren uygulama döşemesini seçin.
2. Listedeki sözleşmeyi seçin. Sözleşmeyle ilgili ayrıntılar farklı bölümlerde görüntülenir. 

    ![Sözleşme ayrıntıları](./media/use/contract-details.png)

    | Section  | Açıklama  |
    |---------|---------|
    | Durum | Sözleşme aşamalarındaki mevcut ilerlemeyi listeler |
    | Ayrıntılar | Sözleşmenin geçerli değerleri |
    | Eylem | Son eylem le ilgili ayrıntılar |
    | Etkinlik | Sözleşmenin işlem geçmişi |
    
3. **Eylem** **bölümünde, Eylem'i seç'** i seçin.

4. Sözleşmenin geçerli durumuyla ilgili ayrıntılar bir bölmede görüntülenir. Açılır yolda yapmak istediğiniz eylemi seçin. 

    ![Eylem seçin](./media/use/choose-action.png)

5. Eylemi başlatmak için **eyleme** geç'i seçin.
6. Eylem için parametreler gerekiyorsa, eylem için değerleri belirtin.

    ![Harekete geçin](./media/use/take-action.png)

7. Eylemi yürütmek için **eylem** eve alın'ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench uygulama sürümü](version-app.md)
