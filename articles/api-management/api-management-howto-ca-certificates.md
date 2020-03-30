---
title: Özel BIR CA sertifikası ekleme - Azure API Yönetimi | Microsoft Dokümanlar
description: Azure API Yönetimi'nde özel bir CA sertifikasını nasıl ekleyeceğinizi öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/20/2018
ms.author: apimpm
ms.openlocfilehash: 21d5869f2bcdfb6383b6ef89869d8098135ea7ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70073599"
---
# <a name="how-to-add-a-custom-ca-certificate-in-azure-api-management"></a>Azure API Yönetimi'nde özel BIR CA sertifikası ekleme

Azure API Yönetimi, güvenilir kök ve ara sertifika depolarının içindeki makineye CA sertifikaları yüklemeye olanak tanır. Hizmetleriniz özel bir CA sertifikası gerektiriyorsa, bu işlevsellik kullanılmalıdır.

Makale, Azure portalında bir Azure API Yönetimi hizmeti örneğinin CA sertifikalarının nasıl yönetileceğinizi gösterir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="upload-a-ca-certificate"></a><a name="step1"> </a>CA sertifikası yükleme

![CA sertifikaları ekleme](media/api-management-howto-ca-certificates/00.png)

Yeni bir CA sertifikası yüklemek için aşağıdaki adımları izleyin. Henüz bir API Yönetimi hizmeti örneği oluşturmadıysanız, bir [API Yönetimi hizmeti örneği oluştur](get-started-create-service-instance.md)öğreticiye bakın.

1. Azure portalındaki Azure API Yönetimi hizmet örneğinize gidin.

2. Menüden **CA sertifikalarını** seçin.

3. **+ Ekle** düğmesine tıklayın.  

    ![CA sertifikaları ekleme](media/api-management-howto-ca-certificates/01.png)  

4. Sertifikaya göz atın ve sertifika deposuna karar verin. Yalnızca ortak anahtar gereklidir, bu nedenle parola gerekli değildir.

    ![CA sertifikaları ekleme](media/api-management-howto-ca-certificates/02.png)  

5. **Kaydet**'e tıklayın. Bu işlem birkaç dakika sürebilir.

    ![CA sertifikaları ekleme](media/api-management-howto-ca-certificates/03.png)  

> [!NOTE]
> Powershell komutunu kullanarak `New-AzApiManagementSystemCertificate` CA sertifikası yükleyebilirsiniz.

## <a name="delete-a-client-certificate"></a><a name="step1a"> </a>İstemci sertifikasını silme

Bir sertifikayı silmek için bağlam **menüsünü** ... tıklatın ve sertifikanın yanındaki **Sil'i** seçin.

![CA sertifikalarını silme](media/api-management-howto-ca-certificates/04.png)  

[Upload a CA certificate]: #step1
[Delete a CA certificate]: #step1a
