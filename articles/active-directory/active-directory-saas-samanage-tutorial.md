---
title: '자습서: Samanage와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 Samanage 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f0db4fb0-7eec-48c2-9c7a-beab1ab49bc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: e96cbbadb3d36cdc022052acb5a9a42da35db263
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34352670"
---
# <a name="tutorial-azure-active-directory-integration-with-samanage"></a>자습서: Samanage와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Samanage를 통합하는 방법에 대해 알아봅니다.

Samanage를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

- Samanage에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 해당 Azure AD 계정으로 Samanage(Single Sign-on)에 자동으로 로그인하도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 응용 프로그램 액세스 및 Single Sign-On이란 무엇인가요?](manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

Samanage와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- Samanage Single Sign-On이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [여기](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 얻을 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다. 이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 Samanage 추가
2. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-samanage-from-the-gallery"></a>갤러리에서 Samanage 추가
Samanage의 Azure AD 통합을 구성하려면 갤러리의 Samanage를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 Samanage를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Active Directory][1]

2. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 응용 프로그램**으로 이동합니다.

    ![응용 프로그램][2]
    
3. 새 응용 프로그램을 추가하려면 대화 상자 맨 위 있는 **새 응용 프로그램** 단추를 클릭합니다.

    ![응용 프로그램][3]

4. 검색 상자에 **Samanage**를 입력합니다.

    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_search.png)

5. 결과 패널에서 **Samanage**를 선택하고 **추가** 단추를 클릭하여 응용 프로그램을 추가합니다.

    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD Single Sign-on 구성 및 테스트
이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 사용하여 Samanage에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 Samanage 사용자가 누구인지 알고 있어야 합니다. 즉, Azure AD 사용자와 Samanage의 관련 사용자 간에 연결이 형성되어야 합니다.

Samanage에서 Azure AD의 **사용자 이름** 값을 **Username** 값으로 할당하여 연결 관계를 설정합니다.

Samanage에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Azure AD 테스트 사용자 만들기](#creating-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On 테스트하는 데 사용합니다.
3. **[Samanage 테스트 사용자 만들기](#creating-a-samanage-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 Samanage에 만듭니다.
4. **[Azure AD 테스트 사용자 할당](#assigning-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Single Sign-On 테스트](#testing-single-sign-on)** - 구성이 작동하는지 확인합니다.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 Samanage 응용 프로그램에서 Single Sign-On을 구성합니다.

**Samanage에서 Azure AD Single Sign-on을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **Samanage** 응용 프로그램 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Configure Single Sign-On][4]

2. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Configure Single Sign-On](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_samlbase.png)

3. **Samanage 도메인 및 URL** 섹션에서 다음 단계를 수행합니다.

    ![Configure Single Sign-On](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_url.png)

    a. **로그온 URL** 텍스트 상자에서 다음 패턴으로 URL을 입력합니다. `https://<Company Name>.samanage.com/saml_login/<Company Name>`

    나. **식별자** 텍스트 상자에서 `https://<Company Name>.samanage.com` 패턴을 사용하여 URL을 입력합니다.

    > [!NOTE] 
    > 이러한 값은 실제 값이 아닙니다. 자습서 뒷부분에 설명된 실제 로그온 URL 및 식별자로 값을 업데이트합니다. 자세한 내용은 [Samanage 클라이언트 지원 팀](https://www.samanage.com/support)에 문의하세요.    
 
4. **SAML 서명 인증서** 섹션에서 **인증서(Base64)** 를 클릭한 후 컴퓨터에 인증서 파일을 저장합니다.

    ![Configure Single Sign-On](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_certificate.png) 

5. **저장** 단추를 클릭합니다.

    ![Configure Single Sign-On](./media/active-directory-saas-samanage-tutorial/tutorial_general_400.png)

6. **Samanage 구성** 섹션에서 **Samanage 구성**을 클릭하여 **로그온 구성** 창을 엽니다. **빠른 참조** 섹션에서 **로그아웃 URL 및 SAML 엔터티 ID**를 복사합니다.

    ![Configure Single Sign-On](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_configure.png) 

7. 다른 웹 브라우저 창에서 Samanage 회사 사이트에 관리자로 로그인합니다.

8. **대시보드**를 클릭하고 왼쪽 창에서 **설정**을 선택합니다.
   
    ![대시보드](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "대시보드")

9. **Single Sign-On**을 클릭합니다.
   
    ![Single Sign-On](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "Single Sign-On")

10. **SAML을 사용하여 로그인** 섹션으로 이동하고 다음 단계를 수행합니다.
   
    ![SAML을 사용하여 로그인](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "SAML을 사용하여 로그인")
 
    a. **SAML로 Single Sign-On 사용**을 클릭합니다.  
 
    나. Azure Portal에서 복사한 **SAML 엔터티 ID** 값을 **Identity Provider URL**(ID 공급자 URL) 텍스트 상자에 붙여넣습니다.    
 
    다. **로그인 URL**이 Azure Portal **Samanage 도메인 및 URL**의 **로그온 URL**과 일치하는지 확인합니다.
 
    d. Azure Portal에서 복사한 **로그아웃 URL** 값을 **로그아웃 URL** 텍스트 상자에 입력합니다.
 
    e. **SAML Issuer**(SAML 발급자) 텍스트 상자에 ID 공급자에서 설정한 앱 ID URI를 입력합니다.
 
    f. Azure Portal에서 다운로드한 Base-64로 인코딩된 인증서를 메모장에서 열고, 내용을 클립보드에 복사한 다음 **Paste your Identity Provider x.509 Certificate below**(아래 ID 공급자 x.509 인증서 붙여넣기) 텍스트 상자에 붙여넣습니다.
 
    g. **Samanage에 없는 경우 사용자 만들기**를 클릭합니다.
 
    h. **업데이트**를 클릭합니다.

> [!TIP]
> 이제 앱을 설정하는 동안 [Azure Portal](https://portal.azure.com) 내에서 이러한 지침의 간결한 버전을 읽을 수 있습니다.  **Active Directory > 엔터프라이즈 응용 프로그램** 섹션에서 이 앱을 추가한 후에는 **Single Sign-On** 탭을 클릭하고 맨 아래에 있는 **구성** 섹션을 통해 포함된 설명서에 액세스하면 됩니다. 포함된 설명서 기능에 대한 자세한 내용은 [Azure AD 포함된 설명서]( https://go.microsoft.com/fwlink/?linkid=845985)에서 확인할 수 있습니다.
 
### <a name="creating-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기
이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

![Azure AD 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. **Azure Portal**의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-samanage-tutorial/create_aaduser_01.png) 

2. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.
    
    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-samanage-tutorial/create_aaduser_02.png) 

3. **사용자** 대화 상자를 열려면 대화 상자 위쪽에서 **추가**를 클릭합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-samanage-tutorial/create_aaduser_03.png) 

4. **사용자** 대화 상자 페이지에서 다음 단계를 수행합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-samanage-tutorial/create_aaduser_04.png) 

    a. **이름** 텍스트 상자에 **BrittaSimon**을 입력합니다.

    나. **사용자 이름** 텍스트 상자에 BrittaSimon의 **전자 메일 주소**를 입력합니다.

    다. **암호 표시**를 선택하고 **암호** 값을 적어둡니다.

    d. **만들기**를 클릭합니다.
 
### <a name="creating-a-samanage-test-user"></a>Samanage 테스트 사용자 만들기

Azure AD 사용자가 Samanage에 로그인할 수 있도록 하려면 Samanage로 프로비전되어야 합니다.  
Samanage의 경우 프로비전은 수동 작업입니다.

**사용자 계정을 프로비전하려면 다음 단계를 수행합니다.**

1. Samanage 회사 사이트에 관리자 권한으로 로그인합니다.

2. **대시보드**를 클릭하고 왼쪽 창에서 **설정**을 선택합니다.
   
    ![설치](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "설치")

3. **사용자** 탭을 클릭합니다.
   
    ![사용자](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "사용자")

4. **새 사용자**를 클릭합니다.
   
    ![새 사용자](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "새 사용자")

5. 프로비전할 Azure Active Directory 계정의 **이름**과 **메일 주소**를 입력하고 **사용자 만들기**를 클릭합니다.
   
    ![사용자 만들기](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "사용자 만들기")
   
   >[!NOTE]
   >Azure Active Directory 계정 보유자는 활성화되기 전에 전자 메일을 받고 링크를 따라 계정을 확인합니다. Samanage에서 제공하는 다른 Samanage 사용자 계정 만들기 도구 또는 API를 사용하여 Azure Active Directory 사용자 계정을 프로비전할 수 있습니다.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 Samanage에 대한 액세스 권한을 부여합니다.

![사용자 할당][200] 

**Britta Simon을 Samanage에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 응용 프로그램 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 응용 프로그램**으로 이동한 후 **모든 응용 프로그램**을 클릭합니다.

    ![사용자 할당][201] 

2. 응용 프로그램 목록에서 **Samanage**를 선택합니다.

    ![Configure Single Sign-On](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_app.png) 

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    ![사용자 할당][202] 

4. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 할당][203]

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

6. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="testing-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 Samanage 타일을 클릭하면 Samanage 응용 프로그램에 자동으로 로그온됩니다.
액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](active-directory-saas-access-panel-introduction.md)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](active-directory-saas-tutorial-list.md)
* [Azure Active Directory로 응용 프로그램 액세스 및 Single Sign-On을 구현하는 방법](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_203.png

