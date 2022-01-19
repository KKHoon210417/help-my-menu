# 헤얿 마이 메뉴

<br>

## 🔗 라이브
<a href="https://helpmymenu.site">https://helpmymenu.site</a>

<br>

## 📢 소개
여러분의 냉장고에 있는 재료들로 만들 수 있는 요리를 추천 받고, 훌륭한 레시피를 친구들과 공유해보세요.

![실행화면](https://user-images.githubusercontent.com/43202607/135443176-6dfae082-289a-4b4c-9ba8-b75cd0147e4c.gif)

<br>

## 🗓 개발기간
**[1차]** 2021년 09월 23일 ~ 2021년 09월 30일 (8일)

<br>

## 🧙 멤버구성
### Back-End

[:octocat:](https://github.com/hellonayeon) 권나연

[:octocat:](https://github.com/KKHoon210417) 김광훈 

[:octocat:](https://github.com/HWON0720) 안휘원 

[:octocat:](https://github.com/profoundsea25) 양해준

<br>

## 🛠 프로젝트 사용기술 &nbsp;

### Back-End

- Python3
- Flask
- pymongo
- requests
- MongoDB
- AWS S3
- AWS EB
- Gitactions

### Front-End

- HTML
- Javascript
- CSS
- jQuery

### Data

`농림축산식품 공공데이터`

&nbsp;&nbsp;&nbsp; |__ `레시피 기본정보`: [🔗Link](https://data.mafra.go.kr/opendata/data/indexOpenDataDetail.do?data_id=20150827000000000464&filter_ty=)

&nbsp;&nbsp;&nbsp; |__ `레시피 재료정보`: [🔗Link](https://data.mafra.go.kr/opendata/data/indexOpenDataDetail.do?data_id=20150827000000000465&filter_ty=)

&nbsp;&nbsp;&nbsp; |__ `레시피 과정정보`: [🔗Link](https://data.mafra.go.kr/opendata/data/indexOpenDataDetail.do?data_id=20150827000000000466&filter_ty=)

<br>

## 💡 나의 사용기술 &nbsp;

- jQuery UI의 Autocomplete를 사용해 자동완성검색기능을 구현했습니다.
- Git을 이용해 프로젝트 Branch와 Issue를 관리했습니다.
- Boto3를 이용해 AWS S3 버킷에 이미지를 저장을 구현했습니다.
- Flask를 사용해 Restful하게 API를 구현했습니다.

<br>

## 💡 핵심 트러블 슈팅 &nbsp;

### jQuery UI 자동완성기능 데이터소스 처리 문제

- 기존의 사용자가 재료를 입력하기 위해서는 약 200가지의 모든 재료데이터를 Dropdown으로 가져와 직접 선택해야 하는 불편함이 있었습니다.
- 이를 해결하기 위해서 jQuery UI에서 제공하는 Autocomplete를 사용해서 자동완성기능을 만들었고, 자동완성기능에 사용될 데이터 소스를 어떤식으로 가져와야할지에 대해 고민을 했습니다.<br>
이에 대해 두가지의 방법을 생각했습니다.<br>

1. 페이지 로딩 시 데이터를 전부 보내서 클라이언트에 보관하는 방식
    - 페이지 로딩 시 한 번만 API를 호출하면 된다.
    - 데이터소스의 양이 적다면 클라이언트 메모리에 무리가 가지 않는다.

2. 자동완성검색 타이핑 시 이벤트 감지를 이용해 재료데이터 API를 호출하는 방식
    - 데이터소스가 많을 때, 필요한 데이터소스만 가져올 수 있다.
    - 이벤트 감지시마다 API가 호출해야할 필요가 있다.

- 두 가지 방법에서 다음과 같은 특징을 찾아내었고, 추후 데이터의 양이 점차 늘어날 것을 생각했을 때 2번안이 옳지만 재료는 종류가 한정적이라는 특징이 있어 프로젝트가 성장해도 데이터의 양이 기하 급수적으로 늘어나지 않기 때문에, 1번안을 선택하여 적용하였습니다.

<details markdown="1">
<summary>개선된 코드</summary>

-frontend

```javascript
//첫 화면 재료 선택 데이터 가져오기
function ingredientListing() {
    $.ajax({
        type: "GET",
        url: "/ingredient-and-recipe",
        data: {},
        success: function (response) {
            let ingreList = response['recipe_ingredient']

            for (let i = 0; i < ingreList.length; i++) {
                let ingredient = ingreList[i]
                let tempHtml = `<option value="main">${ingredient}</option>`
                $('#ingredient-select-list').append(tempHtml)
            }
        }
    });
}
```

- backend

```python
# 첫 화면 재료 항목 불러오기
@application.route('/ingredient-and-recipe', methods=['GET'])
def ingredient_listing():
    # 중복 제거
    irdnt = list(db.recipe_ingredient.distinct("IRDNT_NM"))
    recipe = list(db.recipe_basic.distinct("RECIPE_NM_KO"))
    return jsonify({'recipe_ingredient': irdnt, 'recipe_name_kor': recipe})
```

</details>

<br>

## 👾 그 외 트러블 슈팅 &nbsp;
 
<details markdown="2">
<summary>프로필 이미지 파일 저장 문제</summary>
  
- 프로젝트 폴더 내에 '프로필 이미지'를 저장하여 프로젝트 재 배포할 때 마다 프로필 이미지 초기화 되는 문제가 발생하였습니다.
- 이를 해결하기 위해 boto3를 이용해서 AWS S3의 버킷에 이미지 파일이 저장되도록 수정하였습니다.<br>
  
[issue87](https://github.com/hellonayeon/help-my-menu/issues/87)
  
</details>

<details markdown="3">
<summary>코드 컨벤션 재 정립 문제</summary>
  
- 각자 작성한 코드를 합치는 과정에서 컨벤션 정립 미흡하여 코드 가독성 저하 문제 발생하였습니다.
- 문제 해결을 위해 팀 회의를 통해 컨벤션 재 정립 후 코드 리팩토링 실시하였습니다.
  
[issue42](https://github.com/hellonayeon/help-my-menu/issues/42)

</details>

<details markdown="4">
<summary>User DB PK 중복 문제</summary>
  
- 회원 인증 기능을 도입하면서 사용자에 따라 처리해야할 기능이 많아졌습니다.
- 효과적으로 데이터를 관리할 수 있는 방법에 대해 팀원들과 회의를 통해 다음과 같이 User DB PK를 사용하도록 결정하였습니다.
- 로그인에 필요한 ID는 Email을 선정, 내부적으로 사용자를 식별하는 PK는 MongoDB에서 제공하는 Objectid로 선정하였습니다.
 
[issue63](https://github.com/hellonayeon/help-my-menu/issues/63)

</details>

<details markdown="5">
<summary>레시피 추천 기능 성능 개선</summary>
  
- 재료의 종류, 난이도, 조리 시간, 장르 선택에 따라 레시피 추천이 하나도 안뜨는 문제가 발생하였습니다.
- 사용자 입장에서 매번 모든 조건을 입력하고도 레시피 추천을 받지 못하는 것은 불편함으로서 작용할 것이라고 판단하였습니다.
- 이 문제를 해결하고자 난이도, 조리 시간, 장르 3가지의 조건은 필터로 사용자가 옵셔널하게 입력할 수 있도록 개선하였습니다.
- 그 결과 재료 선택 후 레시피 추천받을 경우 추천 레시피가 뜨지 않는 문제를 해결할 수 있었습니다.
 
[issue46](https://github.com/hellonayeon/help-my-menu/issues/46)

</details>
