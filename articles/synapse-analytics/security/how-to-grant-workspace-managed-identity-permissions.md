---
title: Azure Synapse çalışma alanında yönetilen kimlik için izin verme
description: Azure Synapse çalışma alanında yönetilen kimlik izinlerinin nasıl yapılandırılabildiğini açıklayan bir makale.
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: how-to
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: 9f519022fffe98c565c3b2d30f6578b9ebb70c57
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81428023"
---
# <a name="grant-permissions-to-workspace-managed-identity-preview"></a>Çalışma alanı yönetilen kimliğine izin verme (önizleme)

Bu makalede, Azure sinaps çalışma alanında yönetilen kimliğe nasıl izin verilebilirsiniz. İzinler, buna karşılık, Azure portalı üzerinden çalışma alanındaki SQL havuzlarına ve ADLS gen2 depolama hesabındaki SQL havuzlarına erişime izin verir.

>[!NOTE]
>Bu çalışma alanı yönetilen kimlik, bu belgenin geri kalanı aracılığıyla yönetilen kimlik olarak anılacaktır.

## <a name="grant-the-managed-identity--permissions-to-the-sql-pool"></a>Yönetilen kimlik izinlerini SQL havuzuna verme

Yönetilen kimlik, çalışma alanındaki SQL havuzlarına izin verir. Verilen izinlerle, SQL havuzuyla ilgili etkinlikleri gerçekleştiren ardışık hatlar düzenleyebilir. Azure portalını kullanarak bir Azure Synapse çalışma alanı oluşturduğunuzda, SQL havuzlarında yönetilen kimlik DENETIMI izinlerini verebilirsiniz.

Azure Synapse çalışma alanınızı oluştururken **Güvenlik + ağ** oluşturma yı seçin. Ardından, **SQL havuzlarında çalışma alanının yönetilen kimliğine Hibe DENETIMI'ni**seçin.

![SQL havuzlarında CONTROL izni](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-16.png)

## <a name="grant-the-managed-identity-permissions-to-adls-gen2-storage-account"></a>Yönetilen kimlik izinlerini ADLS gen2 depolama hesabına verme

Azure Synapse çalışma alanı oluşturmak için bir ADLS gen2 depolama hesabı gereklidir. Azure Synapse çalışma alanında Spark havuzlarını başarıyla başlatmak için Azure Synapse yönetilen kimliğin bu depolama *hesabındaki Depolama Blob Veri Katılımcısı* rolüne ihtiyacı vardır. Azure Synapse'deki boru hattı düzenlemesi de bu rolden yararlanır.

### <a name="grant-permissions-to-managed-identity-during-workspace-creation"></a>Çalışma alanı oluşturma sırasında yönetilen kimliğe izin verme

Azure Synapse, Azure portalını kullanarak Azure Synapse çalışma alanını oluşturduktan sonra, Depolama Blob Veri Katılımcısı rolünü yönetilen kimliğe vermeye çalışır. TEMELLER sekmesinde ADLS gen2 depolama hesabı ayrıntılarını **sağlarsınız.**

![Çalışma alanı oluşturma akışında temeller sekmesi](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-1.png)

**Hesap adı** ve **Dosya sistem adı**ADLS gen2 depolama hesabı ve dosya sistemi seçin.

![ADLS gen2 depolama hesabı ayrıntılarını sağlama](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-2.png)

Çalışma alanı oluşturucusu aynı zamanda ADLS gen2 depolama hesabının **sahibiyse,** Azure Synapse, *Depolama Blob Veri Katılımcısı* rolünü yönetilen kimliğe atar. Girdiğiniz depolama hesabı ayrıntılarının altında aşağıdaki iletiyi görürsünüz.

![Başarılı Depolama Blob Veri Katılımcısı ataması](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-3.png)

Çalışma alanı oluşturucusu ADLS gen2 depolama hesabının sahibi değilse, Azure Synapse, *Depolama Blob Veri Oluşturucurolünü* yönetilen kimliğe atamaz. Depolama hesabı ayrıntılarının altında görünen ileti, çalışma alanı oluşturucuya, *Depolama Blob Veri Oluşturucusu* rolünü yönetilen kimliğe vermek için yeterli izine sahip olmadıklarını belirtir.

![Başarısız Depolama Blob Veri Katılımcısı ataması](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-4.png)

İletinin belirttiği gibi, *Depolama Blob Veri Oluşturkanı* yönetilen kimliğe atanmadığı sürece Spark havuzları oluşturamazsınız.

### <a name="grant-permissions-to-managed-identity-after-workspace-creation"></a>Çalışma alanı oluşturulduktan sonra yönetilen kimliğe izin verme

Çalışma alanı oluşturma sırasında, *Depolama Blob Veri katkıda bulunanı* yönetilen kimliğe atadıysanız, ADLS gen2 depolama hesabının **sahibi** bu rolü el ile kimliğe atar. Aşağıdaki adımlar el ile atama gerçekleştirmenize yardımcı olacaktır.

#### <a name="step-1-navigate-to-the-adls-gen2-storage-account-in-azure-portal"></a>Adım 1: Azure portalındaki ADLS gen2 depolama hesabına gidin

Azure portalında ADLS gen2 depolama hesabını açın ve sol gezintiden **Genel Bakış'ı** seçin. Yalnızca kapsayıcı veya dosya sistemi düzeyinde *Depolama Blob Veri Katılımcısı* rolünü atamanız gerekir. **Kapsayıcılar'ı**seçin.  
![ADLS gen2 depolama hesabına genel bakış](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-5.png)

#### <a name="step-2-select-the-container"></a>Adım 2: Kapsayıcıyı seçin

Yönetilen kimlik, çalışma alanı oluşturulduğunda sağlanan kapsayıcıya (dosya sistemi) veri erişimine sahip olmalıdır. Bu kapsayıcıyı veya dosya sistemini Azure portalında bulabilirsiniz. Azure portalında Azure Synapse çalışma alanını açın ve sol gezintiden **Genel Bakış** sekmesini seçin.
![ADLS gen2 depolama hesabı konteyner](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-7.png)


*Depolama Blob Veri Katılımcısı* rolünü yönetilen kimliğe vermek için aynı kapsayıcıyı veya dosya sistemini seçin.
![ADLS gen2 depolama hesabı konteyner seçimi](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-6.png)

#### <a name="step-3-navigate-to-access-control"></a>Adım 3: Access denetimine gidin

**Erişim Denetimi 'ni (IAM)** seçin.

![Erişim denetimi(IAM)](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-8.png)

#### <a name="step-4-add-a-new-role-assignment"></a>Adım 4: Yeni bir rol ataması ekleme

**+ Ekle** öğesini seçin.

![Yeni rol ataması ekleme](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-9.png)

#### <a name="step-5-select-the-rbac-role"></a>Adım 5: RBAC rolünü seçin

Depolama **Blob Veri Katılımcısı** rolünü seçin.

![RBAC rolünü seçin](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-10.png)

#### <a name="step-6-select-the-azure-ad-security-principal"></a>Adım 6: Azure REKLAM güvenlik ilkesini seçin

Açılan yere atama erişiminden **Azure AD kullanıcısını, grubunu veya hizmet** **ilkesini** seçin.

![AAD güvenlik ilkesini seçin](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-11.png)

#### <a name="step-7-search-for-the-managed-identity"></a>Adım 7: Yönetilen kimliği arama

Yönetilen kimliğin adı aynı zamanda çalışma alanı adıdır. **Select'e**Azure Synapse çalışma alanı adını girerek yönetilen kimliğinizi arayın. Yönetilen kimliğin listelenmiş olduğunu görmeniz gerekir.

![Yönetilen kimliği bulma](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-12.png)

#### <a name="step-8-select-the-managed-identity"></a>Adım 8: Yönetilen kimliği seçin

**Seçili üyelere**yönetilen kimliği seçin. Rol atamasını eklemek için **Kaydet'i** seçin.

![Yönetilen kimliği seçin](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-13.png)

#### <a name="step-9-verify-that-the-storage-blob-data-contributor-role-is-assigned-to-the-managed-identity"></a>Adım 9: Depolama Blob Veri Katılımcısı rolünün yönetilen kimliğe atandığını doğrulayın

**Access Denetimi'ni(IAM)** seçin ve ardından **Rol atamaları'nı**seçin.

![Rol atamayı doğrulama](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-14.png)

Yönetilen kimliğinizi **Depolama Blob Veri Katılımcısı** bölümünün altında, kendisine atanan *Depolama Blob Veri Katılımcısı* rolüyle birlikte görmeniz gerekir. 
![ADLS gen2 depolama hesabı konteyner seçimi](./media/how-to-grant-workspace-managed-identity-permissions/configure-workspace-managed-identity-15.png)

## <a name="next-steps"></a>Sonraki adımlar

[Workspace yönetilen kimlik](./synapse-workspace-managed-identity.md) hakkında daha fazla bilgi edinin
