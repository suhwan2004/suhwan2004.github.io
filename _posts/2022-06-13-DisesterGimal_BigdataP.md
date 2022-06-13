---
published: true
title: "학교 - 4학년 1학기 파멸의 기말고사 - 빅데이터에서 살아남기"
categories:
  - PTU
tags:
  - 빅데이터프로그래밍(4학년-1학기)
---

![image](https://user-images.githubusercontent.com/60723373/173362641-d2f323d8-7d86-4dfb-8448-75315d641f21.png)

망하지 않기 위해 정리한 함수 모음...

### CSV 파일 받기

    pd.read_csv(파일경로)

### 특정 데이터를 Dataframe으로 변환

    df = pd.Dataframe(해당 자료)

### dataframe의 행 이름들을 설정

    df.columns = [~~~]

### 정보를 보기(컬럼 이름, null이 아닌 갯수, 타입)

    df.info()

### 해당 컬럼의 고유값들을 보여줌

    df[컬럼이름].unique()

### 컬럼 내에 특정 단어를 바꿈.

    df[컬럼이름].replace(타켓단어, 바꿀단어, inplace = True or False)
    	=> inplace = True 시 원본에 적용됨.

##### 또한, 다중으로 변경 가능. 아래는 {}로 값들을 묶고 1,2,3을 나라 이름으로 바꿔주는 모습임.

    ex) df['origin'].replace({1:'USA', 2:'EU', 3:'KOREA'}, inplace = True)

### df의 nan 값을 없애기

    df.dropna(subset = [적용컬럼1, 적용컬럼2...], axis = 0, inplace = True or False)
    => axis가 0일 시 가로를 보고 행 정보를 제거, 1일 시 세로로 보고 그 해당 열 정보를 제거

### 특정 컬럼에 들어있는 값의 타입을 바꿔줌.

    df[특정컬럼] = df[특정컬럼].astype('float')

##### 다중으로 바꾸기 가능

    df = df.astype({'a' : np.int, 'b' : np.int64})

### 특정 컬럼의 타입을 알려줌

    df[특정컬럼].dtypes

### Dataframe의 앞 5개 보여주기

    df.head()

### 끝에 5개는?

    df.tail()

### df의 특정컬럼을 이용해서 히스토그램생성

    np.histogram(df[특정컬럼], bins = 3)
     => bins는 구간의 갯수임. 아래는 3개니까 3개의 구간으로 나눠짐.

##### 이런 식으로 js의 구조분해 할당처럼 여러 변수에 각각의 값을 넣어주고 있음.

    count, bin_dividers = np.histogram(df[특정컬럼], bins = 3)

### 위의 히스토그램과 비슷한 느낌의 cut함수

    pd.cut(데이터, 구간의 갯수, 레이블명)

### df 내 특정 컬럼의 각 값별 몇개가 있는지를 보여줌

    df[특정컬럼].value_counts()

### 시간 형식의 object 자료형 컬럼의 자료형을 datetime 자료형으로 바꿔주기

    pd.to_datetime(df[특정컬럼])

### datatime 자료형(timestamp)를 Period로 변환

    changeVal = timestamp.to_period(freq = 'D')  => 년-월-일 단위로 변환
    changeVal = timestamp.to_period(freq = 'M')  => 년-월 단위로 변환
    changeVal = timestamp.to_period(freq = 'Y')  => 년 단위로 변환

### timestamp 배열 만들기

    pd.date_range(start = '년-월-일', end = None. periods = 6, freq = 'MS', tz = 'Asia/Seoul')
     =>'년-월-일' 부터 'None'까지 '서울시간' 기준 '6개'의 '월의 시작일'을 반환
     => M만 하면 월의 마지막 날이 된다고 한다. 또한 3M을 하면 3개월이 된다고 한다.
     => pd.date_range의 경우 freq는 정확히 범위라고 보면 됨. 또한, 아래와 같이 시분초까지 다 나오는 결과가 나옴.

    DatetimeIndex(['2019-01-01 00:00:00+09:00', '2019-02-01 00:00:00+09:00', .....])

### Period 배열 만들기

    pd.period_range(start = '년-월-일', end = None, periods = 3, freq = 'M')
     => 위와 많이 비슷함. 그러나, Period의 경우 나오는 값이 freq에 많이 영향을 받음.
      간격도 간격인데 값의 템플릿이 M이 될것.
    PeriodIndex(['2019-01', '2019-02', .... ]) => 요런식으로...

### 년-월-일로 되어 있을 때 년, 월, 일 따로따로 자르기

    df_stock['Year'] = df_stock['new_Date_p'].dt.year
    df_stock['Month'] = df_stock['new_Date_p'].dt.month
    df_stock['Day'] = df_stock['new_Date_p'].dt.day

### df.loc[] 내에 날짜로 범위를 두고 구할때는 '큰 날짜' : '작은 날짜'가 됨을 꼭 기억하자.

    df_ymd_range = df_stock.loc['2018-06-20':'2018-06-10']

### 시리즈 내 모든 값에 함수 매핑 (반환값은 series)

    Series.apply(함수)

### Dataframe은 안됨? 반환값은 Dataframe이여야 됨. => 된다!

    dataFrame.applymap(함수)

##### 여러개를 하고 싶다면?

    df[[특정컬럼, 특정컬럼1, ...]].applymap()

### 각 컬럼 별로 데이터가 몇 개인지 세고 싶다면?

    df.count()

##### 이걸 더 활용하면 loc된 것에도 사용가능.

    df.loc[df['episodes'] == 'Unknown'].count()
    #에피소드가 '모르는 상태'인 것의 갯수.

### 나는 특정 조건을 만족하는 가로 한 줄(행)은 제대로 뜨고, 아니면 아닌 행은 값이 다 NaN으로 되게 하고 싶음.

    df.where(조건)

### loc의 매개변수중 조건의 첫 째는 가로의 조건, 둘 째는 세로의 조건임. 아래의 문은 episodes가 Unknown인걸 np.nan으로 교체

    df.loc[df['episodes'] == 'Unknown', 'episodes'] = np.nan

### 가로, 세로 갯수

    df.shape

### group별 집계

    df.groupby(특정컬럼)

##### 여러개 하고 싶다면?

    df.grouby([특정 컬럼, 특정 컬럼1...])

##### 값별로 갯수를 보고 싶다면?

    df.groupby([]).count()

### 카운트, 평균, 최저, 최대... 등의 여러 정보를 보고 싶다면?

    df.describe()

### 피봇 테이블 생성

     => index는 행 위치, columns는 열 위치, 데이터는 values,
      데이터 집계 함수는 aggfunc, nan 처리는 fill_value

    df.pivot_table(index = 'type', columns = 'episodes',
     aggfunc = np.mean, fill_value = 0)


         episodes   episodes   episodes   episodes
    type
    type
    type
     => 이런 느낌임.

### apply, applymap과 비슷한 함수 매핑 (아래는 람다함수를 넣은 것임.)

    df[특정컬럼].map(lambda x : x.split(','))

### 2차원 배열로 되 있는걸 아예 한 줄로 바꿔버림.

    np.hstack(원하는 배열)

### 공백제거

    Series.strip()

### 정렬

    Series.sort()

### 아래는 concat인데 기능이 두 개 정도 확인 됨.

    1.들어간 파라미터가 배열 단 한 개일때는 dataframe으로 만들어줌.
    pd.concat(df_list)

    2. 배열들을 합쳐줌
    pd.concat([a,b], axis = 0 or 1, ignore_index = True or False)
     => axis가 0일때는 가로로 합쳐짐. 1일때는 세로로 합쳐짐 또한 값이 빈 것들은 NaN이 들어감.
     => ignore_index가 True면 기존 index 무시, False면 기존 인덱스 유지

### concat처럼 Dataframe을 병합해주는 기능이 있으나 공동 칼럼을 기준으로 join 해주는 Merge도 있다.

    pd.merge(left, right, on = '기준열', how = '조인방식')
     => how에는 left, right, inner, outer이 있다.
     => left는 왼쪽 df, right은 오른쪽 df이다.
     => on은 join에 사용할 기준열임.
     만약, 서로 각자 없는 기준열로 join하고 싶다면
      on을 left_on = 'left의 기준열',
      right_on ='right의 기준열'과 같은 방법으로 해주면 된다.

### 긴 소수를 소숫점 한자리로 만들고 싶다면?

    join_data.describe().round(1)
     => 소숫점 한자리로 만들어주는 모습이다.

### 우리가 원하는 형식으로 연,월을 받고싶다면?

    df[시계열 컬럼].dt.strftime("%Y-%m")
     => "%Y-%m" 방식으로 받기 위해 strftime을 쓴 것임.

### 선형 그래프를 그리고 싶다면?

    import matplotlib.pyplot as plt
    plt.plot(x값들, y값들, label = "~~")

### 그래프에 범례(지금 이 선이 label이 pc-a인 선이에요! 라는 표시)를 하고 싶다면?

    plt.legend()

### 문자열을 대문자로 만들기

    str.upper()

### dataframe과 같은 데이터에서 값으로 정렬한다면

    sort_values(by = [목표컬럼, ....], ascending = True or False)

### 각 컬럼 별 결측값 갯수 보기

    df.isnull().sum()

### K-Means, 선형회귀 모델 훈련 및 결과 띄워보기

    이게 K-Means
    from sklearn.cluster import KMeans
    from sklearn.preprocessing import StandardScaler
    sc = StandardScaler()
    customer_clustering_sc = sc.fit_transform(customer_clustering)
    	=> fit + transfrom => fit : 평균과 표준편차 계산, transform : 정규화.

    kmeans = KMeans(n_clusters=4, random_state=0)
    => n_clusters는 k이다. 한마디로 군집형성의 갯수, random_state는 난수를 고정해준다.

    clusters = kmeans.fit(customer_clustering_sc) => 모형 학습

    customer_clustering["cluster"] = clusters.labels_ => 예측(군집)
    print(customer_clustering["cluster"].unique())

    ------------
    이게 선형회귀
    from sklearn import linear_model
    import sklearn.model_selection

    model = linear_model.LinearRegression() => 모델생성

    X = predict_data[["count_0","count_1","count_2","count_3","count_4","count_5","period"]]
    y = predict_data["count_pred"]
    X_train, X_test, y_train, y_test = 	sklearn.model_selection.train_test_split(X,y)
    	=>X,y를 훈련값, 테스트값으로 쪼갬
    model.fit(X_train, y_train) => 훈련값 x,y로 훈련

    x1 = [3, 4, 4, 6, 8, 7, 8]
    x2 = [2, 2, 3, 3, 4, 6, 8]
    x_pred = [x1, x2]

    model.predict(x_pred) => 새 입력값 x1, x2을 방금 학습한 모델을 통해 예측값 받기
