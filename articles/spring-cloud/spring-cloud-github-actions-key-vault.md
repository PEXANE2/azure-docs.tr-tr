---
title: GitHub Eylemlerinde Anahtar Kasası ile Azure Bahar Bulutu'nun kimliğini doğrula
description: GitHub Eylemleri ile Azure Bahar Bulutu için CI/CD iş akışı ile anahtar kasası nasıl kullanılır?
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/20/2019
ms.openlocfilehash: 78cd5945e394219be0551bbe97afef07f18b61f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945478"
---
# <a name="authenticate-azure-spring-cloud-with-key-vault-in-github-actions"></a>GitHub Eylemlerinde Anahtar Kasası ile Azure Bahar Bulutu'nun kimliğini doğrula
Anahtar kasası anahtarları saklamak için güvenli bir yerdir. Kurumsal kullanıcıların, DENETLEDIKLERI kapsamda CI/CD ortamları için kimlik bilgilerini depolamaları gerekir. Anahtar kasasında kimlik bilgileri almak için anahtar kaynak kapsamı ile sınırlı olmalıdır.  Azure kapsamının tamamına değil, yalnızca önemli kasa kapsamına erişebilir. Bu sadece güçlü bir kutuyu açabilen bir anahtar gibi, bir binadaki tüm kapıları açabilen bir ana anahtar değil. CicD iş akışında yararlı olan başka bir anahtarla anahtar elde etmenin bir yoludur. 

## <a name="generate-credential"></a>Kimlik Bilgisi Oluştur
Anahtar kasasına erişmek için bir anahtar oluşturmak için, yerel makinenizde aşağıdaki komutu uygulayın:
```
az ad sp create-for-rbac --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP>/providers/Microsoft.KeyVault/vaults/<KEY_VAULT> --sdk-auth
```
`--scopes` Parametre tarafından belirtilen kapsam, kaynağa anahtar erişimini sınırlar.  Sadece güçlü kutuya erişebilir.

Sonuçlarla:
```
{
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
}
```
Ardından sonuçları [GitHub deponuzu ayarlayın ve Azure ile kimlik doğrulaması'nda](./spring-cloud-howto-github-actions.md#set-up-github-repository-and-authenticate)açıklandığı şekilde GitHub **sırlarına** kaydedin.

## <a name="add-access-policies-for-the-credential"></a>Kimlik Bilgileri için Erişim İlkeleri Ekleme
Yukarıda oluşturduğunuz kimlik bilgileri, depoladığı içerikle değil, yalnızca Key Vault hakkında genel bilgi alabilir.  Anahtar Kasası'nda saklanan sırları almak için kimlik bilgisi için erişim ilkeleri belirlemeniz gerekir.

Azure portalındaki **Anahtar Kasa** panosuna gidin, **Access denetim** menüsünü tıklatın ve ardından Rol `This resource` **atamaları** sekmesini açın. **Tür** ve **kapsam**için **Uygulamalar** seçin.  Önceki adımda oluşturduğunuz kimlik belgesini görmeniz gerekir:

 ![Erişim ilkesini ayarlama](./media/github-actions/key-vault1.png)

Kimlik bilgisi adını kopyalama, örneğin. `azure-cli-2020-01-19-04-39-02` Access **ilkeleri** menüsünü açın, **+Ekle İlkesi** bağlantısını tıklatın.  Şablon `Secret Management` **Template**için seçin, ardından **Müdür'u**seçin. **Müdür**/**Seç** giriş kutusuna kimlik bilgisi adını yapıştır:

 ![Şunu seçin:](./media/github-actions/key-vault2.png)

 **Erişim ilkesi ekle** iletişim kutusunda **Ekle** düğmesini tıklatın ve sonra **Kaydet'i**tıklatın.

## <a name="generate-full-scope-azure-credential"></a>Tam kapsamlı Azure Kimlik Bilgileri oluşturma
Bu binadaki tüm kapıları açmak için ana anahtardır. Yordam önceki adıma benzer, ancak burada ana anahtarı oluşturmak için kapsamı değiştiriyoruz:

```
az ad sp create-for-rbac --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID> --sdk-auth
```

Yine, sonuçlar:
```
{
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
}
```
Tüm JSON dizesini kopyalayın.  Bo, **Key Vault** panosuna geri dön. **Sırlar** menüsünü açın ve **ardından Oluştur/İçe Aktar** düğmesini tıklatın. Gizli adı, örneğin. `AZURE-CRENDENTIALS-FOR-SPRING` JSON kimlik belgesi dizesini **Değer** giriş kutusuna yapıştırın. Değer giriş kutusunun çok satırlı metin alanı yerine tek satırlık bir metin alanı olduğunu fark edebilirsiniz.  Orada tam JSON dize yapıştırabilirsiniz.

 ![Tam kapsam kimlik bilgisi](./media/github-actions/key-vault3.png)

## <a name="combine-credentials-in-github-actions"></a>GitHub Eylemleri'nde kimlik bilgilerini birleştirme
CICD ardışık hattı yürütüldüğünde kullanılan kimlik bilgilerini ayarlayın:

```
on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}           # Strong box key you generated in the first step
    - uses: Azure/get-keyvault-secrets@v1.0
      with:
        keyvault: "zlhe-test"
        secrets: "AZURE-CREDENTIALS-FOR-SPRING"           # Master key to open all doors in the building
      id: keyvaultaction
    - uses: azure/login@v1
      with:
        creds: ${{ steps.keyvaultaction.outputs.AZURE-CREDENTIALS-FOR-SPRING }}
    - name: Azure CLI script
      uses: azure/CLI@v1
      with:
        azcliversion: 2.0.75
        inlineScript: |
          az extension add --name spring-cloud             # Spring CLI commands from here
          az spring-cloud list

```

## <a name="next-steps"></a>Sonraki adımlar
* [Bahar Bulut github eylemler](./spring-cloud-howto-github-actions.md)