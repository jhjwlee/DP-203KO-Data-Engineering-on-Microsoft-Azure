# 모듈 6 - Azure Databricks에서 데이터 탐색 및 변환

이 모듈에서는 다양한 Apache Spark DataFrame 메서드를 사용하여 Azure Databricks에서 데이터를 탐색 및 변환하는 방법을 알아봅니다. 구체적으로는 표준 DataFrame 메서드를 수행하여 데이터를 탐색 및 변환하는 방법을 알아봅니다. 그리고 중복 데이터 제거, 날짜/시간 값 조작, 열 이름 바꾸기, 데이터 집계 등의 고급 작업 수행 방법도 배웁니다.

이 모듈을 완료하면 다음 작업을 수행할 수 있습니다.

- Azure Databricks에서 DataFrames를 사용하여 데이터 탐색 및 필터링
- 후속 쿼리를 더욱 빠르게 실행할 수 있도록 DataFrame 캐시
- 중복 데이터 제거
- 날짜/시간 값 조작
- DataFrame 열 제거 및 이름 바꾸기
- DataFrame에 저장된 데이터 집계

## 랩 세부 정보

- [모듈 6 - Azure Databricks에서 데이터 탐색 및 변환](#module-6---data-exploration-and-transformation-in-azure-databricks)
  - [랩 세부 정보](#lab-details)
  - [랩 1 - 데이터 프레임 사용](#lab-1---working-with-dataframes)
    - [실습 랩 시작 전 준비 사항](#before-the-hands-on-lab)
      - [작업 1 - Azure Databricks 작업 영역 만들기 및 구성](#task-1---create-and-configure-the-azure-databricks-workspace)
    - [연습 1: 랩 Notebook 완료](#exercise-1-complete-the-lab-notebook)
      - [작업 1: Databricks 보관 파일 복제](#task-1-clone-the-databricks-archive)
      - [작업 2: Describe a DataFrame Notebook 완료](#task-2-complete-the-describe-a-dataframe-notebook)
    - [연습 2: Working with DataFrames Notebook 완료](#exercise-2-complete-the-working-with-dataframes-notebook)
    - [연습 3: Display Function Notebook 완료](#exercise-3-complete-the-display-function-notebook)
    - [연습 4: Distinct Articles exercise Notebook 완료](#exercise-4-complete-the-distinct-articles-exercise-notebook)
  - [랩 2 - 데이터 프레임 고급 메서드 사용](#lab-2---working-with-dataframes-advanced-methods)
    - [연습 2: 랩 Notebook 완료](#exercise-2-complete-the-lab-notebook)
      - [작업 1: Databricks 보관 파일 복제](#task-1-clone-the-databricks-archive-1)
      - [작업 2: Date and Time Manipulation Notebook 완료](#task-2-complete-the-date-and-time-manipulation-notebook)
    - [연습 3: Use Aggregate Functions Notebook 완료](#exercise-3-complete-the-use-aggregate-functions-notebook)
    - [연습 4: De-Duping Data exercise Notebook 완료](#exercise-4-complete-the-de-duping-data-exercise-notebook)

## 랩 1 - 데이터 프레임 사용

데이터를 읽고 처리하도록 데이터 프레임을 정의하여 Azure Databricks의 데이터 처리를 수행합니다. 이 랩에서는 Azure Databricks 데이터 프레임을 사용하여 데이터를 읽는 방법을 알아봅니다. 이 랩의 연습은 Databricks Notebooks 내에서 완료해야 합니다. 시작하려면 Azure Databricks 작업 영역에 대한 액세스 권한이 있어야 합니다. 사용 가능한 작업 영역이 없으면 아래 지침을 따르세요. 사용 가능한 작업 영역이 있으면 `Databricks 보관 파일 복제` 단계부터 진행하면 됩니다.

### 실습 랩 시작 전 준비 사항

> **참고:** `실습 랩 시작 전 준비 사항` 단계는 호스트형 랩 환경이 **아닌 **자체 Azure 구독을 사용하는 경우에만 완료하세요. 호스트형 랩 환경을 사용하는 경우에는 연습 1부터 바로 진행하면 됩니다.

이 랩의 연습을 진행하기 전에 사용 가능한 클러스터가 있는 Azure Databricks 작업 영역에 액세스할 수 있는지 확인하세요. 작업 영역을 구성하려면 아래 작업을 수행합니다.

#### 작업 1 - Azure Databricks 작업 영역 만들기 및 구성

[랩 06 설정 지침](https://github.com/solliancenet/microsoft-data-engineering-ilt-deploy/blob/main/setup/06/lab-01-setup.md)에 따라 작업 영역을 만들고 구성합니다.

### 연습 1: 랩 Notebook 완료

#### 작업 1: Databricks 보관 파일 복제

1. 현재 Azure Databricks 작업 영역이 열려 있지 않은 경우 Azure Portal에서 배포된 Azure Databricks 작업 영역으로 이동하고 **작업 영역 시작**을 선택합니다.
1. 왼쪽 창에서 **작업 영역** > **사용자**를 선택하고, 사용자 이름(집 모양 아이콘이 있는 항목)을 선택합니다.
1. 표시되는 창에서 이름 옆에 있는 화살표를 선택한 다음, **가져오기**를 선택합니다.

    ![보관 파일을 가져오는 메뉴 옵션](media/import-archive.png)

1. **Notebook 가져오기** 대화 상자에서 URL을 선택하고, 다음 URL에 붙여넣습니다.

 ```
  https://github.com/solliancenet/microsoft-learning-paths-databricks-notebooks/blob/master/data-engineering/DBC/04-Working-With-Dataframes.dbc?raw=true
 ```

1. **가져오기**를 선택합니다.
1. 표시되는 **04-Working-With-Dataframes** 폴더를 선택합니다.

#### 작업 2: Describe a DataFrame Notebook 완료

**1.Describe-a-dataframe** Notebook을 엽니다. 지침에 따라 내부에서 셀을 실행하기 전에 클러스터를 Notebook에 연결했는지 확인합니다.

Notebook 내에서 다음을 수행합니다.

- `DataFrame` API 숙지
- 다음 클래스 살펴보기
  - `SparkSession`
  - `DataFrame`(`Dataset[Row]`)
- 다음 작업 살펴보기
  - `count()`

Notebook이 완료되면 이 화면으로 돌아와서 다음 단계를 계속 진행하세요.

### 연습 2: Working with DataFrames Notebook 완료

Azure Databricks 작업 영역에서 사용자 폴더에 가져온 **04-Working-With-Dataframes** 폴더를 엽니다.

**2.Use-common-dataframe-methods** Notebook을 엽니다. 지침에 따라 내부에서 셀을 실행하기 전에 클러스터를 Notebook에 연결했는지 확인합니다.

Notebook 내에서 다음을 수행합니다.

- `DataFrame` API 숙지
- 성능에 일반적인 DataFrame 메서드 사용
- Spark API 설명서 살펴보기

Notebook이 완료되면 이 화면으로 돌아와서 다음 단계를 계속 진행하세요.

### 연습 3: Display Function Notebook 완료

Azure Databricks 작업 영역에서 사용자 폴더에 가져온 **04-Working-With-Dataframes** 폴더를 엽니다.

**3.Display-function** Notebook을 엽니다. 지침에 따라 내부에서 셀을 실행하기 전에 클러스터를 Notebook에 연결했는지 확인합니다.

Notebook 내에서 다음을 수행합니다.

- 다음 변환 살펴보기
  - `limit(..)`
  - `select(..)`
  - `drop(..)`
  - `distinct()`
  - `dropDuplicates(..)`
- 다음 작업 살펴보기
  - `show(..)`
  - `display(..)`

Notebook이 완료되면 이 화면으로 돌아와서 다음 단계를 계속 진행하세요.

### 연습 4: Distinct Articles exercise Notebook 완료

Azure Databricks 작업 영역에서 사용자 폴더에 가져온 **04-Working-With-Dataframes** 폴더를 엽니다.

**4.Exercise: Distinct Articles** Notebook을 엽니다. 지침에 따라 내부에서 셀을 실행하기 전에 클러스터를 Notebook에 연결했는지 확인합니다.

이 연습에서는 Parquet 파일을 읽고, 필요한 변환을 적용하며, 총 레코드 수를 수행하고, 모든 데이터가 올바르게 로드되었는지 확인합니다. 데이터와 일치하는 스키마를 정의하고 스키마를 사용하도록 읽기 작업을 업데이트합니다.

> 참고: `Solutions` 하위 폴더 내에서 해당하는 Notebook을 찾을 수 있습니다. 여기에는 연습에 사용할 전체 셀이 포함되어 있습니다. 어려운 점이 있거나 단순히 솔루션만 보려는 경우 Notebook을 참조하세요.

Notebook이 완료되면 이 화면으로 돌아와서 다음 랩을 계속 진행하세요.

## 랩 2 - 데이터 프레임 고급 메서드 사용

이 랩에서는 이전 랩에서 살펴본 Azure Databricks 데이터 프레임 관련 개념을 추가로 살펴봅니다. 구체적으로는 데이터 엔지니어가 데이터 프레임을 사용하여 데이터 읽기, 쓰기, 변환을 수행할 때 사용할 수 있는 몇 가지 고급 메서드를 살펴봅니다.

### 연습 2: 랩 Notebook 완료

#### 작업 1: Databricks 보관 파일 복제

1. 현재 Azure Databricks 작업 영역이 열려 있지 않은 경우 Azure Portal에서 배포된 Azure Databricks 작업 영역으로 이동하고 **작업 영역 시작**을 선택합니다.
1. 왼쪽 창에서 **작업 영역** > **사용자**를 선택하고, 사용자 이름(집 모양 아이콘이 있는 항목)을 선택합니다.
1. 표시되는 창에서 이름 옆에 있는 화살표를 선택한 다음, **가져오기**를 선택합니다.

    ![보관 파일을 가져오는 메뉴 옵션](media/import-archive.png)

1. **Notebook 가져오기** 대화 상자에서 URL을 선택하고, 다음 URL에 붙여넣습니다.

 ```
  https://github.com/solliancenet/microsoft-learning-paths-databricks-notebooks/blob/master/data-engineering/DBC/07-Dataframe-Advanced-Methods.dbc?raw=true
 ```

1. **가져오기**를 선택합니다.
1. 표시되는 **07-Dataframe-Advanced-Methods** 폴더를 선택합니다.

#### 작업 2: Date and Time Manipulation Notebook 완료

**1.DateTime-Manipulation** Notebook을 엽니다. 지침에 따라 내부에서 셀을 실행하기 전에 클러스터를 Notebook에 연결했는지 확인합니다.

Notebook 내에서 다음을 수행합니다.

- `...sql.functions` 작업 자세히 살펴보기
  - 날짜/시간 함수

Notebook이 완료되면 이 화면으로 돌아와서 다음 단계를 계속 진행하세요.

### 연습 3: Use Aggregate Functions Notebook 완료

Azure Databricks 작업 영역의 사용자 폴더에서 가져온 **07-Dataframe-Advanced-Methods** 폴더를 엽니다.

**2.Use-Aggregate-Functions** Notebook을 엽니다. 지침에 따라 내부에서 셀을 실행하기 전에 클러스터를 Notebook에 연결했는지 확인합니다.

Notebook에서 다양한 집계 함수를 알아봅니다.

Notebook이 완료되면 이 화면으로 돌아와서 다음 단계를 계속 진행하세요.

### 연습 4: De-Duping Data exercise Notebook 완료

Azure Databricks 작업 영역의 사용자 폴더에서 가져온 **07-Dataframe-Advanced-Methods** 폴더를 엽니다.

**3.Exercise-Deduplication-of-Data** Notebook을 엽니다. 지침에 따라 내부에서 셀을 실행하기 전에 클러스터를 Notebook에 연결했는지 확인합니다.

이 연습의 목표는 나머지 열을 포함하여 DataFrames를 사용하는 방법에 대해 배운 몇 가지를 연습하는 것입니다. 작업을 수행하는 데 사용할 수 있는 빈 셀과 함께 지침이 Notebook에 제공됩니다. Notebook의 하단에는 작업이 정확한지 확인하는 데 도움이 되는 추가 셀이 있습니다.

> 참고: `Solutions` 하위 폴더 내에서 해당하는 Notebook을 찾을 수 있습니다. 여기에는 연습에 사용할 전체 셀이 포함되어 있습니다. 어려운 점이 있거나 단순히 솔루션만 보려는 경우 Notebook을 참조하세요.
