---
title: "教學課程：Azure Active Directory 與 Thirdlight 整合 | Microsoft Docs"
description: "了解如何使用 Thirdlight 搭配 Azure Active Directory 來啟用單一登入、自動佈建和更多功能！"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 168aae9a-54ee-4c2b-ab12-650a2c62b901
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 3/07/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 0ed8c8a24fd5125690c8fadee4918c25498b6693
ms.lasthandoff: 03/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>教學課程：Azure Active Directory 與 Thirdlight 整合
本教學課程的目的是要示範 Azure 與 Thirdlight 的整合。  

本教學課程中說明的案例假設您已經具有下列項目：

* 有效的 Azure 訂用帳戶
* 已啟用 Thirdlight 單一登入 (SSO) 的訂用帳戶

完成本教學課程之後，您指派給 Thirdlight 的 Azure AD 使用者就能夠在您的 Thirdlight 公司網站上使用 SSO (服務提供者起始的登入)，或使用[存取面板的簡介](active-directory-saas-access-panel-introduction.md)來單一登入應用程式。

本教學課程中說明的案例由下列建置組塊組成：

1. 啟用 Thirdlight 的應用程式整合
2. 設定單一登入 (SSO)
3. 設定使用者佈建
4. 指派使用者

![案例](./media/active-directory-saas-thirdlight-tutorial/IC805836.png "案例")

## <a name="enable-the-application-integration-for-thirdlight"></a>啟用 Thirdlight 的應用程式整合
本節的目的是要說明如何啟用 Thirdlight 的應用程式整合。

**若要啟用 Thirdlight 的應用程式整合，請執行下列步驟：**

1. 在 Azure 傳統入口網站中，按一下左方瀏覽窗格的 [Active Directory] 。
   
    ![Active Directory](./media/active-directory-saas-thirdlight-tutorial/IC700993.png "Active Directory")

2. 從 [目錄]  清單中，選取要啟用目錄整合的目錄。

3. 若要開啟應用程式檢視，請在目錄檢視中，按一下頂端功能表中的 [應用程式]  。
   
    ![應用程式](./media/active-directory-saas-thirdlight-tutorial/IC700994.png "應用程式")

4. 按一下頁面底部的 [新增]  。
   
    ![新增應用程式](./media/active-directory-saas-thirdlight-tutorial/IC749321.png "新增應用程式")

5. 在 [欲執行動作] 對話方塊上，按一下 [從資源庫中新增應用程式]。
   
    ![從資源庫新增應用程式](./media/active-directory-saas-thirdlight-tutorial/IC749322.png "從資源庫新增應用程式")

6. 在**搜尋方塊**中，輸入 **Thirdlight**。
   
    ![應用程式資源庫](./media/active-directory-saas-thirdlight-tutorial/IC805837.png "應用程式資源庫")

7. 在結果窗格中，選取 [Thirdlight]，然後按一下 [完成] 以加入應用程式。
   
    ![ThirdLight](./media/active-directory-saas-thirdlight-tutorial/IC805838.png "ThirdLight")

## <a name="configure-single-sign-on"></a>設定單一登入
本節的目的是要說明如何依據 SAML 通訊協定來使用同盟，讓使用者能夠用自己在 Azure AD 中的帳戶驗證至 Thirdlight。  

設定 Thirdlight 的 SSO 需要您從憑證擷取指紋值。

如果您不熟悉這個程序，請參閱 [如何抓取憑證的指紋值](http://youtu.be/YKQF266SAxI)。

**若要設定單一登入，請執行下列步驟：**

1. 在 Azure 傳統入口網站的 [Thirdlight] 應用程式整合頁面上，按一下 [設定單一登入] 以開啟 [設定單一登入] 對話方塊。
   
    ![設定單一登入](./media/active-directory-saas-thirdlight-tutorial/IC805839.png "設定單一登入")

2. 在 [您希望使用者如何登入 Thirdlight] 頁面上，選取 [Microsoft Azure AD 單一登入]，然後按 [下一步]。
   
    ![設定單一登入](./media/active-directory-saas-thirdlight-tutorial/IC805840.png "設定單一登入")

3. 在 [設定應用程式 URL] 頁面的 [Thirdlight 登入 URL] 文字方塊中，輸入使用者用來登入 Thirdlight 應用程式的 URL (例如："*http://azuresso2.thirdlight.com/*")，然後按 [下一步]。
   
    ![設定應用程式 URL](./media/active-directory-saas-thirdlight-tutorial/IC805841.png "設定應用程式 URL")

4. 在 [設定在 Thirdlight 單一登入] 頁面上，若要下載您的中繼資料，請按一下 [下載中繼資料]，然後將中繼資料檔儲存在本機電腦上。
   
    ![設定單一登入](./media/active-directory-saas-thirdlight-tutorial/IC805842.png "設定單一登入")

5. 在不同的網頁瀏覽器視窗中，以系統管理員身分登入您的 Thirdlight 公司網站。

6. 移至 [組態] \> [系統管理]，然後按一下 [SAML2]。
   
    ![系統管理](./media/active-directory-saas-thirdlight-tutorial/IC805843.png "系統管理")

7. 在 [SAML2 組態] 區段中，執行下列步驟：
   
    ![SAML 單一登入](./media/active-directory-saas-thirdlight-tutorial/IC805844.png "SAML 單一登入")   
 1. 選取 [啟用 SAML2 單一登入] 。 
 2. 在 [IdP 中繼資料的來源] 選取 [從 XML 載入 IdP 中繼資料]。 
 3. 開啟下載的中繼資料檔案，然後複製內容並貼到 [IdP 中繼資料 XML]  文字方塊中。 
 4. 按一下 [儲存 SAML2 設定] 。

8. 在 Azure 傳統入口網站上，選取單一登入設定確認，然後按一下 [完成] 來關閉 [設定單一登入] 對話方塊。
   
    ![設定單一登入](./media/active-directory-saas-thirdlight-tutorial/IC805845.png "設定單一登入")

## <a name="configure-user-provisioning"></a>設定使用者佈建
若要讓 Azure AD 使用者可以登入 Thirdlight，則必須將他們佈建到 Thirdlight。  

* Thirdlight 需以手動的方式佈建。

**若要設定使用者佈建，請執行下列步驟：**

1. 以系統管理員身分登入您的 **Thirdlight** 公司網站。

2. 移至 [使用者]  索引標籤。

3. 選取 [使用者和群組] 。

4. 按一下 [新增使用者]  按鈕。

5. 輸入使用者名稱、名稱或描述、電子郵件，選擇您想要佈建之有效 AAD 帳戶新成員的 [預設] 或 [群組]  。

6. 按一下 [建立] 。

>[!NOTE]
>您可以使用任何其他的 Thirdlight 使用者帳戶建立工具或 Thirdlight 提供的 API 來佈建 AAD 使用者帳戶。 
> 

## <a name="assign-users"></a>指派使用者
若要測試您的組態，則需指派您所允許使用您應用程式的 Azure AD 使用者，藉此授予其存取組態的權限。

**若要指派使用者給 Thirdlight，請執行下列步驟：**

1. 在 Azure 傳統入口網站中建立測試帳戶。

2. 在 [Thirdlight]**** 應用程式整合頁面上，按一下 [指派使用者]。
   
    ![指派使用者](./media/active-directory-saas-thirdlight-tutorial/IC805846.png "指派使用者")

3. 選取測試使用者，按一下 [指派]，然後按一下 [是] 以確認指派。
   
    ![是](./media/active-directory-saas-thirdlight-tutorial/IC767830.png "是")

如果要測試您的單一登入設定，請開啟存取面板。 如需 [存取面板] 的詳細資訊，請參閱 [存取面板簡介](active-directory-saas-access-panel-introduction.md)。


